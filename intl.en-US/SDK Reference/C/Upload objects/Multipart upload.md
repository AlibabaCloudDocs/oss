# Multipart upload

By using the multipart upload feature provided by OSS, you can split a large object into multiple parts and upload them separately. After all parts are uploaded, call the CompleteMultipartUpload operation to combine these parts into a single object to implement resumable upload.

## Multipart upload process

To perform multipart upload, follow these steps:

1.  Initialize a multipart upload task.

    Call the oss\_init\_multipart\_upload method to create a globally unique upload ID in OSS.

2.  Start multipart upload.

    Call the oss\_upload\_part\_from\_file method to upload parts.

    **Note:**

    -   For parts that share an upload ID, partNumber identifies the relative position of a part in the entire object. If you have uploaded a part and used its partNumber again to upload another part, the latter part overwrites the former part.
    -   OSS places the MD5 value of part data in the ETag header and returns the MD5 value to the user.
    -   OSS calculates the MD5 value of uploaded data and compares it with the MD5 value calculated by the SDK. If the two values are different, the InvalidDigest error code is returned.
3.  Complete the multipart upload task.

    After you upload all parts, you can call the oss\_complete\_multipart\_upload method to combine the parts into a complete object.


## Complete sample code

The following code provides a complete example that describes the process of multipart upload:

```
#include "oss_api.h"
#include "aos_http_io.h"
#include <sys/stat.h>
const char *endpoint = "<yourEndpoint>";
const char *access_key_id = "<yourAccessKeyId>";
const char *access_key_secret = "<yourAccessKeySecret>";
const char *bucket_name = "<yourBucketName>";
const char *object_name = "<yourObjectName>";
const char *local_filename = "<yourLocalFilename>";
void init_options(oss_request_options_t *options)
{
    options->config = oss_config_create(options->pool);
    /* Use a char* string to initialize the aos_string_t data type. */
    aos_str_set(&options->config->endpoint, endpoint);
    aos_str_set(&options->config->access_key_id, access_key_id);
    aos_str_set(&options->config->access_key_secret, access_key_secret);
    /* Determine whether CNAME is used. A value of 0 indicates that CNAME is not used. */
    options->config->is_cname = 0;
    /* Configure network parameters such as timeout. */
    options->ctl = aos_http_controller_create(options->pool, 0);
}
int64_t get_file_size(const char *file_path)
{
    int64_t filesize = -1;
    struct stat statbuff;
    if(stat(file_path, &statbuff) < 0){
        return filesize;
    } else {
        filesize = statbuff.st_size;
    }
    return filesize;
}
int main(int argc, char *argv[])
{
    /* Call aos_http_io_initialize in main() to initialize global resources such as the network and memory. */
    if (aos_http_io_initialize(NULL, 0) ! = AOSE_OK) {
        exit(1);
    }
    /* Create a memory pool to manage memory resources. The implementation code is included in the APR library. aos_pool_t is equivalent to apr_pool_t. */
    aos_pool_t *pool;
    /* Create a new memory pool. The second parameter value is NULL. This value indicates that the pool does not inherit any other memory pool. */
    aos_pool_create(&pool, NULL);
    /* Create and initialize options. This parameter includes global configuration information such as endpoint, access_key_id, access_key_secret, is_cname, and curl. */
    oss_request_options_t *oss_client_options;
    /* Allocate memory resources in the memory pool to options. */
    oss_client_options = oss_request_options_create(pool);
    /* Call oss_client_options to initialize the client options. */
    init_options(oss_client_options);
    /* Initialize parameters. */
    aos_string_t bucket;
    aos_string_t object;
    oss_upload_file_t *upload_file = NULL;
    aos_string_t upload_id;   
    aos_table_t *headers = NULL;
    aos_table_t *complete_headers = NULL;
    aos_table_t *resp_headers = NULL;
    aos_status_t *resp_status = NULL; 
    aos_str_set(&bucket, bucket_name);
    aos_str_set(&object, object_name);
    aos_str_null(&upload_id);
    headers = aos_table_make(pool, 1);
    complete_headers = aos_table_make(pool, 1);
    int part_num = 1;
    /* Initialize a multipart upload task and obtain an upload ID. */
    resp_status = oss_init_multipart_upload(oss_client_options, &bucket, &object, &upload_id, headers, &resp_headers);
    /* Determine whether the multipart upload task is initialized. */
    if (aos_status_is_ok(resp_status)) {
        printf("Init multipart upload succeeded, upload_id:%.*s\n", 
               upload_id.len, upload_id.data);
    } else {
        printf("Init multipart upload failed, upload_id:%.*s\n", 
               upload_id.len, upload_id.data);
    }
    /* Upload the parts. */
    int64_t file_length = 0;
    int64_t pos = 0;
    aos_list_t complete_part_list;
       oss_complete_part_content_t* complete_content = NULL;
    char* part_num_str = NULL;
    char* etag = NULL;
    aos_list_init(&complete_part_list);
    file_length = get_file_size(local_filename);
    while(pos < file_length) {
        upload_file = oss_create_upload_file(pool);
        aos_str_set(&upload_file->filename, local_filename);
        upload_file->file_pos = pos;
        pos += 100 * 1024;
        upload_file->file_last = pos < file_length ? pos : file_length;
        resp_status = oss_upload_part_from_file(oss_client_options, &bucket, &object, &upload_id, part_num++, upload_file, &resp_headers);

        /* Save the part numbers and ETags. */
        complete_content = oss_create_complete_part_content(pool);
        part_num_str = apr_psprintf(pool, "%d", part_num-1);
        aos_str_set(&complete_content->part_number, part_num_str);
        etag = apr_pstrdup(pool,
        (char*)apr_table_get(resp_headers, "ETag"));
        aos_str_set(&complete_content->etag, etag);
        aos_list_add_tail(&complete_content->node, &complete_part_list);

        if (aos_status_is_ok(resp_status)) {
            printf("Multipart upload part from file succeeded\n");
        } else {
            printf("Multipart upload part from file failed\n");
        }
    }

    /* Complete the multipart upload task. */
    resp_status = oss_complete_multipart_upload(oss_client_options, &bucket, &object, &upload_id,
            &complete_part_list, complete_headers, &resp_headers);
    /* Determine whether the multipart upload is complete. */
    if (aos_status_is_ok(resp_status)) {
        printf("Complete multipart upload from file succeeded, upload_id:%.*s\n", 
               upload_id.len, upload_id.data);
    } else {
        printf("Complete multipart upload from file failed\n");
    }
    /* Release the memory pool. The memory allocated to various resources used for the request is released. */
    aos_pool_destroy(pool);
    /* Release the allocated global resources. */
    aos_http_io_deinitialize();
    return 0;
}
```

Obtain the uploaded parts. You must provide the ETag value of each part to call oss\_complete\_multipart\_upload. You can use one of the following methods to obtain the ETag value:

-   The ETag of a part is included in the returned result when the part is uploaded. You can save and use this ETag value.
-   You can call oss\_list\_upload\_part to obtain the ETag of an uploaded part. In this example, the first method is used.

## Cancel a multipart upload task

You can use the ossClient.abortMultipartUpload method to cancel a multipart upload task. If a multipart upload task is canceled, the upload ID can no longer be used to perform further operations. The parts uploaded with that upload ID are also deleted.

The following code provides an example on how to cancel a multipart upload task:

```
#include "oss_api.h"
#include "aos_http_io.h"
const char *endpoint = "<yourEndpoint>";
const char *access_key_id = "<yourAccessKeyId>";
const char *access_key_secret = "<yourAccessKeySecret>";
const char *bucket_name = "<yourBucketName>";
const char *object_name = "<yourObjectName>";
void init_options(oss_request_options_t *options)
{
    options->config = oss_config_create(options->pool);
    /* Use a char* string to initialize the aos_string_t data type. */
    aos_str_set(&options->config->endpoint, endpoint);
    aos_str_set(&options->config->access_key_id, access_key_id);
    aos_str_set(&options->config->access_key_secret, access_key_secret);
    /* Determine whether CNAME is used. A value of 0 indicates that CNAME is not used. */
    options->config->is_cname = 0;
    /* Configure network parameters such as timeout. */
    options->ctl = aos_http_controller_create(options->pool, 0);
}
int main(int argc, char *argv[])
{
    /* Call aos_http_io_initialize in main() to initialize global resources such as the network and memory. */
    if (aos_http_io_initialize(NULL, 0) ! = AOSE_OK) {
        exit(1);
    }
    /* Create a memory pool to manage memory resources. The implementation code is included in the APR library. aos_pool_t is equivalent to apr_pool_t. */
    aos_pool_t *pool;
    /* Create a new memory pool. The second parameter value is NULL. This value indicates that the pool does not inherit any other memory pool. */
    aos_pool_create(&pool, NULL);
    /* Create and initialize options. This parameter includes global configuration information such as endpoint, access_key_id, access_key_secret, is_cname, and curl. */
    oss_request_options_t *oss_client_options;
    /* Allocate memory resources in the memory pool to options. */
    oss_client_options = oss_request_options_create(pool);
    /* Call oss_client_options to initialize the client options. */
    init_options(oss_client_options);
    /* Initialize parameters. */
    aos_string_t bucket;
    aos_string_t object;
    aos_string_t upload_id;   
    aos_table_t *headers = NULL;
    aos_table_t *resp_headers = NULL;
    aos_status_t *resp_status = NULL; 
    aos_str_set(&bucket, bucket_name);
    aos_str_set(&object, object_name);
    aos_str_null(&upload_id);
    /* Initialize a multipart upload task and obtain an upload ID. */
    resp_status = oss_init_multipart_upload(oss_client_options, &bucket, &object, &upload_id, headers, &resp_headers);
    /* Determine whether the multipart upload task is initialized. */
    if (aos_status_is_ok(resp_status)) {
        printf("Init multipart upload succeeded, upload_id:%.*s\n", 
               upload_id.len, upload_id.data);
    } else {
        printf("Init multipart upload failed, upload_id:%.*s\n", 
               upload_id.len, upload_id.data);
    }
    /* Cancel the multipart upload task. */
    resp_status = oss_abort_multipart_upload(oss_client_options, &bucket, &object, &upload_id, &resp_headers);
    /* Determine whether the multipart upload task is canceled. */
    if (aos_status_is_ok(resp_status)) {
        printf("Abort multipart upload succeeded, upload_id::%.*s\n", 
               upload_id.len, upload_id.data);
    } else {
        printf("Abort multipart upload failed\n"); 
    }
    /* Release the memory pool. The memory allocated to various resources used for the request is released. */
    aos_pool_destroy(pool);
    /* Release the allocated global resources. */
    aos_http_io_deinitialize();
    return 0;
}
```

