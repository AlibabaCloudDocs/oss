# Append upload

You can use AppendObject to append content to appendable objects that are uploaded.

## Usage notes

When you call AppendObject to upload an object, take note of the following items:

-   If the object to append does not exist, an append object is created.
-   If the object to append is an existing append object and the specified position from which the append operation starts is not equal to the current object size, the PositionNotEqualToLength error is returned. If the object to append is a non-append object, the ObjectNotAppendable error is returned.
-   The CopyObject operation cannot be performed on append objects.

For more information about how to perform append upload, see [AppendObject](/intl.en-US/API Reference/Object operations/Basic operations/AppendObject.md).

## Upload data from the local memory by using append upload

The following code provides an example on how to upload data from the local memory to a specified bucket by using append upload:

```
#include "oss_api.h"
#include "aos_http_io.h"
/* The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint. */
const char *endpoint = "<yourEndpoint>";
/* Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. */
const char *access_key_id = "<yourAccessKeyId>";
const char *access_key_secret = "<yourAccessKeySecret>";
/* Configure the bucket name. */
const char *bucket_name = "<yourBucketName>";
/* Configure the object name. The object name indicates the complete path of the object excluding the bucket name. Example: example/folder/abc.jpg. */
const char *object_name = "<yourObjectName>";
const char *object_content = "More than just cloud.";
void init_options(oss_request_options_t *options)
{
    options->config = oss_config_create(options->pool);
    /* Use a char* string to initialize aos_string_t. */
    aos_str_set(&options->config->endpoint, endpoint);
    aos_str_set(&options->config->access_key_id, access_key_id);
    aos_str_set(&options->config->access_key_secret, access_key_secret);
    /* Specify whether to use CNAME. A value of 0 indicates that CNAME is not used. */
    options->config->is_cname = 0;
    /* Configure network parameters such as timeout periods. */
    options->ctl = aos_http_controller_create(options->pool, 0);
}
int main(int argc, char *argv[])
{
    /* Call the aos_http_io_initialize method in main() to initialize global resources such as networks and memory. */
    if (aos_http_io_initialize(NULL, 0) ! = AOSE_OK) {
        exit(1);
    }
    /* Create a memory pool to manage memory. aos_pool_t is equivalent to apr_pool_t. The code used to create a memory pool is included in the APR library. */
    aos_pool_t *pool;
    /* Create a memory pool. The second parameter is NULL. This value indicates that the pool does not inherit any other memory pool. */
    aos_pool_create(&pool, NULL);
    /* Create and initialize options. This parameter includes global configuration information such as endpoint, access_key_id, access_key_secret, is_cname, and curl. */
    oss_request_options_t *oss_client_options;
    /* Allocate the memory resources in the memory pool to the options. */
    oss_client_options = oss_request_options_create(pool);
    /* Initialize oss_client_options. */
    init_options(oss_client_options);
    /* Initialize the parameters. */
    aos_string_t bucket;
    aos_string_t object;
    aos_list_t buffer;
    int64_t position = 0;
    char *next_append_position = NULL;
    aos_buf_t *content = NULL;
    aos_table_t *headers1 = NULL;
    aos_table_t *headers2 = NULL;
    aos_table_t *resp_headers = NULL; 
    aos_status_t *resp_status = NULL; 
    aos_str_set(&bucket, bucket_name);
    aos_str_set(&object, object_name);
    headers1 = aos_table_make(pool, 0);
    /* Obtain the position from which the first append operation starts. */
    resp_status = oss_head_object(oss_client_options, &bucket, &object, headers1, &resp_headers);
    if (aos_status_is_ok(resp_status)) {
        next_append_position = (char*)(apr_table_get(resp_headers, "x-oss-next-append-position"));
        position = atoi(next_append_position);
    }
    /* Perform the append operation. */
    headers2 = aos_table_make(pool, 0);
    aos_list_init(&buffer);
    content = aos_buf_pack(pool, object_content, strlen(object_content));
    aos_list_add_tail(&content->node, &buffer);
    resp_status = oss_append_object_from_buffer(oss_client_options, &bucket, &object, position, &buffer, headers2, &resp_headers);
    if (aos_status_is_ok(resp_status)) {
        printf("append object from buffer succeeded\n");
    } else {
        printf("append object from buffer failed\n");
    }
    /* Release the memory pool, which releases memory resources allocated for the request. */
    aos_pool_destroy(pool);
    /* Release the allocated global resources. */
    aos_http_io_deinitialize();
    return 0;
}
```

For more information about the naming conventions for buckets, see [bucket](/intl.en-US/Developer Guide/Terms.md). For more information about the naming conventions for objects, see [object](/intl.en-US/Developer Guide/Terms.md).

## Upload a local file by using append upload

The following code provides an example on how to upload a local file to a specified bucket by using append upload:

```
#include "oss_api.h"
#include "aos_http_io.h"
/* The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint. */
const char *endpoint = "<yourEndpoint>";
/* Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. */
const char *access_key_id = "<yourAccessKeyId>";
const char *access_key_secret = "<yourAccessKeySecret>";
/* Configure the bucket name. */
const char *bucket_name = "<yourBucketName>";
/* Configure the object name. The object name indicates the complete path of the object excluding the bucket name. Example: example/folder/abc.jpg. */
const char *object_name = "<yourObjectName>";
/* Configure the name of the local file you want to upload. Specify the complete path of the local file. Example: /users/local/abc.jpg.
const char *local_filename = "<yourLocalFilename>";
void init_options(oss_request_options_t *options)
{
    options->config = oss_config_create(options->pool);
    /* Use a char* string to initialize aos_string_t. */
    aos_str_set(&options->config->endpoint, endpoint);
    aos_str_set(&options->config->access_key_id, access_key_id);
    aos_str_set(&options->config->access_key_secret, access_key_secret);
    /* Specify whether to use CNAME. A value of 0 indicates that CNAME is not used. */
    options->config->is_cname = 0;
    /* Configure network parameters such as timeout periods. */
    options->ctl = aos_http_controller_create(options->pool, 0);
}
int main(int argc, char *argv[])
{
    /* Call the aos_http_io_initialize method in main() to initialize global resources such as networks and memory. */
    if (aos_http_io_initialize(NULL, 0) ! = AOSE_OK) {
        exit(1);
    }
    /* Create a memory pool to manage memory. aos_pool_t is equivalent to apr_pool_t. The code used to create a memory pool is included in the APR library. */
    aos_pool_t *pool;
    /* Create a memory pool. The second parameter is NULL. This value indicates that the pool does not inherit any other memory pool. */
    aos_pool_create(&pool, NULL);
    /* Create and initialize options. This parameter includes global configuration information such as endpoint, access_key_id, access_key_secret, is_cname, and curl. */
    oss_request_options_t *oss_client_options;
    /* Allocate the memory resources in the memory pool to the options. */
    oss_client_options = oss_request_options_create(pool);
    /* Initialize oss_client_options. */
    init_options(oss_client_options);
    /* Initialize the parameters. */
    aos_string_t bucket;
    aos_string_t object;
    aos_string_t file;
    int64_t position = 0;
    char *next_append_position = NULL;
    aos_table_t *headers1 = NULL;
    aos_table_t *headers2 = NULL;
    aos_table_t *resp_headers = NULL; 
    aos_status_t *resp_status = NULL; 
    aos_str_set(&bucket, bucket_name);
    aos_str_set(&object, object_name);
    aos_str_set(&file, local_filename);
    /* Obtain the position from which the first append operation starts. */
    resp_status = oss_head_object(oss_client_options, &bucket, &object, headers1, &resp_headers);
    if(aos_status_is_ok(resp_status)) {
        next_append_position = (char*)(apr_table_get(resp_headers, "x-oss-next-append-position"));
        position = atoi(next_append_position);
    }
    /* Perform the append operation. */
    resp_status = oss_append_object_from_file(oss_client_options, &bucket, &object, position, &file, headers2, &resp_headers);
    /* Determine whether the append operation is successful. */
    if (aos_status_is_ok(resp_status)) {
        printf("append object from file succeeded\n");
    } else {
        printf("append object from file failed\n");
    }
    /* Release the memory pool, which releases memory resources allocated for the request. */
    aos_pool_destroy(pool);
    /* Release the allocated global resources. */
    aos_http_io_deinitialize();
    return 0;
}
```

