# Quick start

This topic describes how to manage OSS in the Node.js environment, such as view buckets and upload objects.

**Note:** To facilitate modification, a file named `app.js` is created in this topic. The following sample code for each operation is described in the sync mode. For more information about how to create clients in the sample code, see [Initialization](/intl.en-US/SDK Reference/Node. js/Initialization.md).

## View the list of buckets

Add the following content to the end of the `app.js` file. Use listBuckets to view buckets:

```
async function listBuckets () {
  try {
    let result = await client.listBuckets();
  } catch(err) {
    console.log(err)
  }
}

listBuckets();            
```

You can run the `node app.js` script and view the response.

For more information about bucket-related operations, visit [GitHub](https://github.com/ali-sdk/ali-oss/blob/master/test/node/bucket.test.js).

## View objects in a bucket

Modify the `app.js` file, and use list to view the objects in a bucket:

```
client.useBucket('examplebucket');
async function list () {
  try {
    let result = await client.list({
      'max-keys': 5
    })
    console.log(result)
  } catch (err) {
    console.log (err)
  }
}
list();
```

You can run the `node app.js` script and view the response.

## Upload objects

Modify the `app.js` file and use put to upload a single object:

```
client.useBucket('examplebucket');

async function put () {
  try {
    let result = await client.put('exampleobject.txt', 'D:\\localpath\\examplefile.txt');
    console.log(result);
   } catch (err) {
     console.log (err);
   }
}

put();           
```

## Download objects

Modify the `app.js` file, and use get to download a single object:

```
client.useBucket('examplebucket');

async function get () {
  try {
    let result = await client.get('exampleobject.txt');
    console.log(result);
  } catch (err) {
    console.log (err);
  }
}

get();            
```

## Delete objects

Modify the `app.js` file, and use delete to delete an object:

```
client.useBucket('examplebucket');

async function delete () {
  try {
    let result = await client.delete('exampleobject.txt');
    console.log(result);
  } catch (err) {
    console.log (err);
  }
}

delete();            
```

For more information about object-related operations, visit [GitHub](https://github.com/ali-sdk/ali-oss/blob/master/test/node/object.test.js).

