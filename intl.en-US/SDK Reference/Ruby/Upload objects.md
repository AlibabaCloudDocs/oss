# Upload objects

This topic describes how to upload objects.

The OSS Ruby SDK provides rich interfaces for object upload. You can upload an object from OSS through any of the following methods:

-   Upload a local file to the OSS
-   Stream upload
-   Resumable upload
-   Append upload
-   Upload callback

## Upload a local file

The following code uses the `Bucket#put_object` interface with the `:file` parameter specified to upload a local file to OSS:

```
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket = client.get_bucket('my-bucket')
bucket.put_object('my-object', :file => 'local-file')
            
```

## Stream upload

During uploading a large file, we often need to upload the file in a streaming way instead of processing and uploading all the content at the same time. We want to upload a part of content at a time. In particular, when the content to be uploaded comes from the Internet and cannot be retrieved at the same time, stream upload is the only option left. Through `Bucket # put_object` interface and specify `block` parameter to upload the stream-generated content to the OSS:

```
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket = client.get_bucket('my-bucket')
bucket.put_object('my-object') do |stream|
  100.times { |i| stream << i.to_s }
end
            
```

## Resumable upload

If the network jitters or the program crashes when a large object is being uploaded, the whole upload operation fails. You have to re-upload the objects, wasting resources. When the network is unstable, you may need to make multiple retries. The `Bucket#resumable_upload` interface can be used to achieve resumable upload. The interface has the following parameters:

-   key: The name of the object on OSS mapped to the uploaded file.
-   file: The local path of the file to be uploaded.
-   opts: An optional parameter. It mainly includes:
    -   :cpt\_file: The path of the checkpoint file. If not specified, the path of the checkpoint object is by default the `file.cpt` under the same directory of the local file. The `file` is the name of the local file.
    -   :disable\_cpt: If this parameter is set to true, the upload progress is not recorded during upload, and resumable upload is not performed when upload fails.
    -   :part\_size: The size of each part. The default size is 4 MB.
    -   &block: If a block function is imported when the Bucket\#resumable\_upload interface is called, the upload progress is handled by the block function.

For more information about the parameters, see [API documentation](http://www.rubydoc.info/gems/aliyun-sdk/).

The principle of resumable upload is to divide the object to be uploaded into multiple parts and upload them separately. When all the parts are uploaded, the upload of the entire object is completed. The progress of the current upload is recorded during the uploading process\(in the checkpoint object\). If the upload of any part fails during the process, the next upload attempt starts from the recorded position in the checkpoint object. This requires that the same checkpoint object with the previous upload must be used in the next call. When the upload is completed, the checkpoint object is deleted.

```
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket = client.get_bucket('my-bucket')
bucket.resumable_upload('my-object', 'local-file') do |p|
  puts "Progress: #{p}"
end

bucket.resumable_upload(
  'my-object', 'local-file',
  :part_size => 100 * 1024, :cpt_file => '/tmp/x.cpt') { |p|
  puts "Progress: #{p}"
}
            
```

**Note:**

-   The SDK records the upload intermediate states in the cpt object. Therefore, make sure that you have the write permission on the cpt object.
-   The cpt object records the intermediate state information of the upload and has a self-checking function. You cannot edit the object. Upload fails if the cpt object is corrupted. When the upload is completed, the checkpoint object is deleted.
-   Upload fails if the local file is changed during the upload process.

## Append upload

OSS supports the appendable object type. The `Bucket#append_object` interface is used to upload appendable objects. When calling this method, you must specify the append position of the object. If the object is newly created, the append position is 0. If the object already exists,the append position must be set to the object length before the append.

-   If the object to be appended does not exist, calling `append_object` creates an appendable object.
-   If the object to be appended exists, calling `append_object` appends the content to the end of the object.

```
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket = client.get_bucket('my-bucket')
# Create an appendable object.
bucket.append_object('my-object', 0) {}

# Append content to the end of the object.
next_pos = bucket.append_object('my-object', 0) do |stream|
  100.times { |i| stream << i.to_s }
end
next_pos = bucket.append_object('my-object', next_pos, :file => 'local-file-1')
next_pos = bucket.append_object('my-object', next_pos, :file => 'local-file-2')
            
```

**Note:**

-   You can append content to appendable object \(objects created with `append_object`\) only.
-   You cannot copy appendable objects.

## Upload callback

You can specify an “upload callback” when uploading a file. After the file is successfully uploaded to OSS, OSS initiates an HTTP POST request to the server address that you have provided. The request is equal to a notification mechanism. You can perform operations according to the received callback results. For more information about upload callback, see [OSS Upload Callback](/intl.en-US/Developer Guide/Objects/Upload files/Upload callback.md).

Currently, only the `put_object` and `resumable_upload` interfaces of the OSS support upload callback.

```
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket = client.get_bucket('my-bucket')

callback = Aliyun::OSS::Callback.new(
  url: 'http://10.101.168.94:1234/callback',
  query: {user: 'put_object'},
  body: 'bucket=${bucket}&object=${object}'
)

begin
  bucket.put_object('files/hello', file: '/tmp/x', callback: callback)
rescue Aliyun::OSS::CallbackError => e
  puts "Callback failed: #{e.message}"
end
            
```

In the preceding code, a file is uploaded through the `put_object` interface with an upload callback specified. The callback body contains the bucket and object information for this upload. Upon receiving the callback, the application server determines that the file has been successfully uploaded to the OSS.

The usage of the r`esumable_upload` interface is similar:

```
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket = client.get_bucket('my-bucket')

callback = Aliyun::OSS::Callback.new(
  url: 'http://10.101.168.94:1234/callback',
  query: {user: 'put_object'},
  body: 'bucket=${bucket}&object=${object}'
)

begin
  bucket.resumable_upload('files/hello', '/tmp/x', callback: callback)
rescue Aliyun::OSS::CallbackError => e
  puts "Callback failed: #{e.message}"
end
            
```

**Note:**

-   The callback URL must not contain the query string which must be specified in the `:query` parameter.
-   In the event that the file is successfully uploaded but callback execution fails, the client throws `CallbackError`. To ignore the error, you must explicitly catch the exception.
-   For detailed examples, see [callback.rb](https://github.com/aliyun/aliyun-oss-ruby-sdk/blob/v0.3.0/examples/aliyun/oss/callback.rb).
-   For servers that support callback, see [callback\_server.rb](https://github.com/aliyun/aliyun-oss-ruby-sdk/blob/v0.3.0/rails/aliyun_oss_callback_server.rb).

