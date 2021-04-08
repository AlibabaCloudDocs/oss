# Query the ACL of an object

Object access control lists \(ACLs\) include private, public read, and public read/write. This topic describes how to query the ACL of an object.

## Object ACLs

The following table describes the three types of object ACLs.

|Object ACL|Description|
|----------|-----------|
|Private|Only the owner or authorized users of the bucket can read and write the object.|
|Public read|Only the object owner or authorized users can write the object. Other users, including anonymous users can only read the object. Exercise caution when you specify this ACL.|
|Public read/write|All users, including anonymous users can read and write the object. Exercise caution when you specify this ACL.|

For more information about object ACLs, see [ACL](/intl.en-US/Developer Guide/Data security/Access and control/ACL.md).

## Examples

The following code provides an example on how to query the ACL of the exampleobject.txt object in the examplebucket bucket.

```
// Specify the name of the bucket and the full path of the object. The full path of the object cannot contain bucket names.
GetObjectACLRequest request = new GetObjectACLRequest("examplebucket", "exampleobject.txt");
// Query the ACL of the object.
oss.asyncGetObjectACL(request, new OSSCompletedCallback<GetObjectACLRequest, GetObjectACLResult>() {
    @Override
    public void onSuccess(GetObjectACLRequest request, GetObjectACLResult result) {
        Log.d("GetObjectACL", "Success!");
        Log.d("ObjectAcl", result.getObjectACL());
        Log.d("Owner", result.getObjectOwner());
        Log.d("ID", result.getObjectOwnerID());
    }

    @Override
    public void onFailure(GetObjectACLRequest request, ClientException clientException, ServiceException serviceException) {
        // Request exceptions.
        if (clientException != null) {
            // Client-side exceptions such as network errors.
            clientException.printStackTrace();
        }
        if (serviceException != null) {
            // Server-side exceptions.
            Log.e("ErrorCode", serviceException.getErrorCode());
            Log.e("RequestId", serviceException.getRequestId());
            Log.e("HostId", serviceException.getHostId());
            Log.e("RawMessage", serviceException.getRawMessage());
        }
    }
});
```

For more information about how to query the ACL of an object, see [GetObjectACL](/intl.en-US/API Reference/Object operations/ACL/GetObjectACL.md).

