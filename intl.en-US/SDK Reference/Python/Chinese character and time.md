# Chinese character and time

This topic describes the Chinese characters and time used by OSS Python SDK.

## Chinese character

Errors may occur if Chinese characters are used in the Python code. Therefore, you must declare the Chinese character encoding at the beginning of the code. For example:

```language-python
# -*- coding: utf-8 -*-

```

-   Data type

    Python 2.x supports the following two data types:

    |Data type|Description|
    |:--------|:----------|
    |str|Character strings, corresponding to the bytes type in Python 3.x|
    |unicode|Unicode streams, of which the length is measured in characters. For example, the length of `u'中文'` is 2.|

    Python 3.x supports the following two data types:

    |Data type|Description|
    |:--------|:----------|
    |str|Character strings, corresponding to the unicode type in Python 2.x|
    |bytes|Byte streams, of which the length is measured in bytes. For example, the length of `b'中文'` depends on the encoding, which is 6 if it is encoded with UTF-8.|

-   Input and output conventions

    The following table describes the conventions of input parameter types for OSS Python SDK.

    |Input|Data type|Remarks|
    |:----|:--------|:------|
    |OSS object name|str|If the input type is bytes, it must be encoded with UTF-8.|
    |Local file name|str, unicode|If the input type is bytes, it must be encoded with UTF-8, such as the yourLocalFile parameter in bucket.get\_object\_to\_file.|
    |Input data stream|bytes|For example, the data parameter in bucket.put\_object.|

    The following table describes the conventions of output parameter types for OSS Python SDK.

    |Output|Data type|Remarks|
    |:-----|:--------|:------|
    |Results parsed from the XML|str|For example, a string in the result obtained by bucket.list\_object.|
    |Downloaded content|bytes|Python SDK regards that the bytes type is encoded with UTF-8 by default. Therefore, make sure that the Python source file is also encoded with UTF-8.|

-   Type conversion functions

    OSS Python SDK provides three functions for type conversion.

    |Function|Description|
    |:-------|:----------|
    |to\_bytes|- Converts the unicode type to the str type in Python 2.x. Other types are returned without changes.

 - Converts the str type to the bytes type in Python 3.x. Other types are returned without changes. |
    |to\_unicode|- Converts the str type to the unicode type in Python 2.x. Other types are returned without changes.

 - Converts the bytes type to the str type in Python 3.x. Other types are returned without changes. |
    |to\_string|Acts as to\_bytes in Python 2.x. Acts as to\_unicode in Python 3.x.|


## Time

OSS Python SDK converts all timestamp strings retrieved from the server to [Unix Time](https://en.wikipedia.org/wiki/Unix_time), that is, the number of seconds since UTC 00:00 on January 1, 1970. For example, the last\_modified time in the results returned by bucket.get\_object is the Unix Time of the int type.

You can use the datetime.datetime.fromtimestamp\(\) method to convert the time, obtaining the timestamp string.

