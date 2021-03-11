# Simple upload

This topic describes how to use simple upload.

For the complete simple upload code, visit [GitHub](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_put_object_sample.c).

## Upload data from memory

The following code provides an example on how to upload data from memory:

```
#include "oss_api.h"
#include "aos_http_io.h"
const char *endpoint = "<yourEndpoint>";
const char *access_key_id = "<yourAccessKeyId>";
const char *access_key_secret = "<yourAccessKeySecret>";
const char *bucket_name = "<yourBucketName>";
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
    if (aos_http_io_initialize(NULL, 0) != AOSE_OK) {
        exit(1);
    }
    /* Create a memory pool to manage memory. aos_pool_t is equivalent to apr_pool_t. The code used to create a memory pool is included in the APR library. */
    aos_pool_t *pool;
    /* Create a memory pool. The second parameter is NULL. This value indicates that the pool does not inherit other memory pool. */
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
    aos_buf_t *content = NULL;
    aos_table_t *headers = NULL;
    aos_table_t *resp_headers = NULL; 
    aos_status_t *resp_status = NULL; 
    aos_str_set(&bucket, bucket_name);
    aos_str_set(&object, object_name);
    aos_list_init(&buffer);
    content = aos_buf_pack(oss_client_options->pool, object_content, strlen(object_content));
    aos_list_add_tail(&content->node, &buffer);
    /* Upload the object. */
    resp_status = oss_put_object_from_buffer(oss_client_options, &bucket, &object, &buffer, headers, &resp_headers);
    /* Determine whether the object is uploaded. */
    if (aos_status_is_ok(resp_status)) {
        printf("put object from buffer succeeded\n");
    } else {
        printf("put object from buffer failed\n");      
    }
    /* Release the memory pool, which releases memory resources allocated for the request. */
    aos_pool_destroy(pool);
    /* Release the allocated global resources. */
    aos_http_io_deinitialize();
    return 0;
}
```

## Encrypt objects to upload from a file

The following code provides an example on how to upload a file to OSS:

```
#include "oss_api.h"
#include "aos_http_io.h"
const char *endpoint = "<yourEndpoint>";
const char *access_key_id = "<yourAccessKeyId>";
const char *access_key_secret = "<yourAccessKeySecret>";
const char *bucket_name = "<yourBucketName>";
const char *object_name = "<yourObjectName>";
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
    if (aos_http_io_initialize(NULL, 0) != AOSE_OK) {
        exit(1);
    }
    /* Create a memory pool to manage memory. aos_pool_t is equivalent to apr_pool_t. The code used to create a memory pool is included in the APR library. */
    aos_pool_t *pool;
    /* Create a memory pool. The second parameter is NULL. This value indicates that the pool does not inherit other memory pool. */
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
    aos_table_t *headers = NULL;
    aos_table_t *resp_headers = NULL; 
    aos_status_t *resp_status = NULL; 
    aos_str_set(&bucket, bucket_name);
    aos_str_set(&object, object_name);
    aos_str_set(&file, local_filename);
    /* Upload the object. */
    resp_status = oss_put_object_from_file(oss_client_options, &bucket, &object, &file, headers, &resp_headers);
    /* Determine whether the object is uploaded. */
    if (aos_status_is_ok(resp_status)) {
        printf("put object from file succeeded\n");
    } else {
        printf("put object from file failed\n");
    }
    /* Release the memory pool, which releases memory resources allocated for the request. */
    aos_pool_destroy(pool);
    /* Release the allocated global resources. */
    aos_http_io_deinitialize();
    return 0;
}
```

## FAQ: How can I upload objects whose names contain Chinese characters?

On Windows systems, you can use UTF-8 to encode the object names that contain letters and upload the object. The following code provides an example on how to upload objects whose names contain Chinese characters:

```
#include "oss_api.h"
#include "aos_http_io.h"
#include <wchar.h>
const char *endpoint = "<yourEndpoint>";
const char *access_key_id = "<yourAccessKeyId>";
const char *access_key_secret = "<yourAccessKeySecret>";
const char *bucket_name = "<yourBucketName>";
const char *object_name = "<yourObjectName>";
const char *local_filename_ch = "c:\\test.txt";
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

char* multibyte_to_utf8(const char * ch, char *str, int size)
{
　　if (!ch || !str || size <= 0)
　　    return NULL;

　　int chlen = strlen(ch);
　　int multi_byte_cnt = 0;
　　for (int i = 0; i < chlen - 1; i++) {
        if ((ch[i] & 0x80) && (ch[i + 1] & 0x80)) {
            i++;
            multi_byte_cnt++;
        }
　　}
　　if (multi_byte_cnt == 0)
        return ch;

　　int len = MultiByteToWideChar(CP_ACP, 0, ch, -1, NULL, 0);
　　if (len <= 0)
        return NULL;
　　wchar_t *wch = malloc(sizeof(wchar_t) * (len + 1));
　　if (!wch)
        return NULL;
　　wmemset(wch, 0, len + 1);
　　MultiByteToWideChar(CP_ACP, 0, ch, -1, wch, len);

　　len = WideCharToMultiByte(CP_UTF8, 0, wch, -1, NULL, 0, NULL, NULL);
　　if ((len <= 0) || (size < len + 1)) {
        free(wch);
        return NULL;
　　}
　　memset(str, 0, len + 1);
　　WideCharToMultiByte(CP_UTF8, 0, wch, -1, str, len, NULL, NULL);
　　free(wch);
　　return str;
}

int main(int argc, char argv[])
{
　　/* Call the aos_http_io_initialize method in main() to initialize global resources such as networks and memory. */
　　if (aos_http_io_initialize(NULL, 0) != AOSE_OK) {
　　    exit(1);
　　}
　　/* Create a memory pool to manage memory. aos_pool_t is equivalent to apr_pool_t. The code used to create a memory pool is included in the APR library. */
　　aos_pool_t pool;
　　/* Create a memory pool. The second parameter is NULL. This value indicates that the pool does not inherit other memory pool. */
　　aos_pool_create(&pool, NULL);
    /* Create and initialize options. This parameter includes global configuration information such as endpoint, access_key_id, access_key_secret, is_cname, and curl. */
　　oss_request_options_t oss_client_options;
　　/* Allocate the memory resources in the memory pool to the options. */
　　oss_client_options = oss_request_options_create(pool);
　　/* Initialize oss_client_options. */
　　init_options(oss_client_options);
　　/* Initialize the parameters. */
　　aos_string_t bucket;
　　aos_string_t object;
    aos_string_t file;
    aos_table_t *headers = NULL;
    aos_table_t *resp_headers = NULL;
    aos_status_t *resp_status = NULL;
    aos_str_set(&bucket, bucket_name);
    aos_str_set(&object, object_name);

    /* Use UTF-8 to encode the object names that contain multibyte Chinese characters. */
    char local_filename_buf[1024];
　　char * local_filename = multibyte_to_utf8(local_filename_ch, local_filename_buf, 1024);
　　aos_str_set(&file, local_filename);

　　/* Upload the object. */
    resp_status = oss_put_object_from_file(oss_client_options, &bucket, &object, &file, headers, &resp_headers);
　　/* Determine whether the object is uploaded. */
　　if (aos_status_is_ok(resp_status)) {
        printf("put object from file succeeded\n");
    } else {
        printf("put object from file failed\n");
　　}
　　/* Release the memory pool, which releases memory resources allocated for the request. */
　　aos_pool_destroy(pool);
　　/* Release the allocated global resources. */
　　aos_http_io_deinitialize();
　　return 0;
}
```

