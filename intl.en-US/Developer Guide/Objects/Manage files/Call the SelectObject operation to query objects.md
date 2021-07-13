# Call the SelectObject operation to query objects

You can call the SelectObject operation to execute SQL statements on an object and return the execution result.

## Background information

Hadoop 3.0 is supported on Object Storage Service \(OSS\). You can directly read and process data in OSS when you run services such as Spark, Hive, and Presto in E-MapReduce. Alibaba Cloud services such as MaxCompute, HybridDB for MySQL, and Data Lake Analytics \(DLA\) are also supported.

However, the current GetObject operation provided by OSS requires the big data platform to download all OSS data locally for analysis and filtering. As a result, large amounts of bandwidth and client resources are wasted in many query scenarios.

To resolve this issue, OSS provides the SelectObject operation. SelectObject allows OSS to preliminarily filter data by using conditions and projections provided by the big data platform. As a result, only useful data is returned to the big data platform. This way, the client can consume fewer bandwidth resources and process less data to maximize CPU and memory resources. This makes OSS-based data warehousing and data analysis a better option.

## Detail analysis

The following section describes the object types and SQL statements supported by SelectObject in detail.

-   Object types supported by SelectObject

    **Note:** Use SelectObject for normal objects. We recommend that you do not use SelectObject to query Multipart and Appendable objects. The differences in their internal structures may deteriorate query performance.

    -   CSV objects \(and CSV-like objects such as TSV objects\) that conform to RFC 4180. You can customize row and column delimiters and quote characters in CSV objects.
    -   UTF-8 encoded JSON objects. SelectObject supports JSON objects in DOCUMENT and LINES formats.
        -   A JSON DOCUMENT object contains a single object.
        -   A JSON LINES object consists of lines of objects separated by row delimiters. However, the complete JSON object itself may not be valid. SelectObject supports typical delimiters such as \\n and \\r\\n. You do not need to specify these delimiters.
    -   Standard and Infrequent Access \(IA\) objects. You must restore Archive and Cold Archive objects before you access them.
    -   Objects fully managed and encrypted by OSS or encrypted by using customer master keys \(CMKs\) managed by Key Management Service \(KMS\).
-   Supported SQL syntax
    -   SQL statement: SELECT FROM WHERE
    -   Data types: string, int\(64bit\), double\(64bit\), decimal\(128\), timestamp, and bool.
    -   Operators: logical operators \(AND, OR, and NOT\), arithmetic operators \(+, -, \*, /, and %\), comparison operators \(\>, =, <, \>=, <=, and !=\),and string operators \(LIKE and \|\|\).

        **Note:** The matching is case-sensitive when you use LIKE for fuzzy matches.

-   Multipart query

    SelectObject supports multipart query similar to byte-based multipart download supported by the GetObject operation. Data is split into parts by row or split.

    -   By row: This method is used in most cases but may result in unbalanced loads when sparse data is split.
    -   By split: A split includes multiple rows. Each split is about the same size.
    **Note:** The method of splitting data by split is more efficient.

-   Data types

    In OSS, data in CSV objects is of the STRING type by default. You can use the CAST function to convert the data type. For example, the following SQL statement converts the data in the first and second columns to the INTEGER type and compares them:

    `Select * from OSSOBject where cast (_1 as int) > cast(_2 as int)`

    In addition, SelectObject allows you to implicitly convert the data type in a WHERE clause. For example, the following SQL statement converts the data in the first and second columns to the INTEGER type:

    `Select _1 from ossobject where _1 + _2 > 100`

    If you do not use the CAST function, the data type of a JSON object is determined by the type of data in the object. A standard JSON object can support data types such as NULL, BOOLEAN, Int64, DOUBLE, and STRING.


## SQL statement examples

SQL statement examples are provided for CSV and JSON objects.

-   SQL statement examples for CSV objects

    |Scenario|SQL statement|
    |:-------|:------------|
    |Return the first 10 rows.|SELECT \* FROM ossobject limit 10|
    |Return integers in the first and third columns, in which the values of the integers in the first column are greater than those in the third column.|SELECT \_1, \_3 FROM ossobject WHERE CAST\(\_1 AS INT\) \> CAST\(\_3 AS INT\)|
    |Return the number of records in which the data in the first column starts with X. A Chinese character specified after LIKE must be UTF-8 encoded.|SELECT COUNT\(\*\) FROM ossobject WHERE \_1 LIKE 'X%'|
    |Return all records in which the time of the data in the second column is later than 2018-08-09 11:30:25 and the data in the third column is greater than 200.|SELECT \* FROM ossobject WHERE \_2 \> CAST\('2018-08-09 11:30:25' AS TIMESTAMP\) AND \_3 \> 200|
    |Return the average value, sum, the maximum value, and the minimum value of the floating-point numbers in the second column.|SELECT AVG\(CAST\(\_2 AS DOUBLE\)\), SUM\(CAST\(\_2 AS DOUBLE\)\), MAX\(CAST\(\_2 AS DOUBLE\)\), MIN\(CAST\(\_2 AS DOUBLE\)\) |
    |Return all records in which the strings concatenated by the data in the first and third columns start with Tom and end with Anderson.|SELECT \* FROM ossobject WHERE \(\_1 \|\| \_3\) LIKE 'Tom%Anderson'|
    |Return all records in which the data in the first column is divisible by 3.|SELECT \* FROM ossobject WHERE \(\_1 % 3\) = 0|
    |Return all records in which the data in the first column ranges from 1995 to 2012.|SELECT \* FROM ossobject WHERE \_1 BETWEEN 1995 AND 2012|
    |Return all records in which the data in the fifth column is N, M, G, or L.|SELECT \* FROM ossobject WHERE \_5 IN \('N', 'M', 'G', 'L'\)|
    |Return all records in which the product of the data in the second and third columns is greater than the sum of 100 and the data in the fifth column.|SELECT \* FROM ossobject WHERE \_2 \* \_3 \> \_5 + 100|

-   SQL statement examples for JSON objects

    An example of a JSON object:

    ```
    {
      "contacts":[
    {
      "firstName": "John",
      "lastName": "Smith",
      "isAlive": true,
      "age": 27,
      "address": {
        "streetAddress": "21 2nd Street",
        "city": "New York",
        "state": "NY",
        "postalCode": "10021-3100"
      },
      "phoneNumbers": [
        {
          "type": "home",
          "number": "212 555-1234"
        },
        {
          "type": "office",
          "number": "646 555-4567"
        },
        {
          "type": "mobile",
          "number": "123 456-7890"
        }
      ],
      "children": [],
      "spouse": null
    }, …… # Other similar nodes are omitted.
    ]}
    ```

    The following table describes the SQL statement examples.

    |Scenario|SQL statement|
    |--------|-------------|
    |Return all records in which the value of age is 27.|SELECT \* FROM ossobject.contacts\[\*\] s WHERE s.age = 27 |
    |Return all home phone numbers.|SELECT s.number FROM ossobject.contacts\[\*\].phoneNumbers\[\*\] s WHERE s.type = "home" |
    |Return all records in which the value of spouse is null.|SELECT \* FROM ossobject s WHERE s.spouse IS NULL |
    |Return all records in which the number of children is 0.|SELECT \* FROM ossobject s WHERE s.children\[0\] IS NULL **Note:** The preceding statement is used because an empty array cannot be specified in other ways. |


## Scenarios

In most cases, SelectObject is used for the multipart query, JSON object query, and analysis of log objects.

-   Query large objects in multipart query.

    If columns in a CSV object do not include line feeds, you can divide the object into parts based on bytes. This method is the simplest because you do not need to create Select Meta for the object. To query a JSON or CSV object where columns include line feeds, perform the following steps:

    1.  Call the CreateSelectObjectMeta operation to obtain the total number of splits for the object. Before you call SelectObject for the object, asynchronously call the CreateSelectObjectMeta operation to shorten the scanning time.
    2.  Select the appropriate concurrency level \(n\) based on resources on the client. Divide the total number of splits by concurrency level \(n\) to obtain the number of splits to contain in each query.
    3.  Set parameters, such as split-range=1-20, in the request body to perform multipart query.
    4.  Combine the results.
-   When you query a JSON object, narrow down the JSON path range in the FROM clause.

    An example of a JSON object:

    ```
    { contacts:[
            {"firstName":"John", "lastName":"Smith", "phoneNumbers":[{"type":"home", "number":"212-555-1234"}, {"type":"office", "number":"646-555-4567"}, {"type":"mobile", "number":"123 456-7890"}], "address":{"streetAddress": "21 2nd Street", "city":"New York", "state":NY, "postalCode":"10021-3100"}
             }
    ]}
    ```

    To query all streetAddress data of records in which the postal code starts with 10021, execute the following SQL statement: `SELECT s.address.streetAddress FROM ossobject.contacts[*] s WHERE s.address.postalCode LIKE '10021%'` or `SELECT s.streetAddress FROM ossobject.contacts[*].address s WHERE s.postalCode LIKE '10021%'`.

    The performance of the JSON path is better because it is more accurate when you execute `SELECT s.streetAddress FROM ossobject.contacts[*].address s WHERE s.postalCode LIKE '10021%'`.

-   Process high-precision floating-point numbers in JSON objects.

    If you want to calculate high-precision floating-point numbers in a JSON object, we recommend that you set the ParseJsonNumberAsString parameter to true, and use the CAST function to convert the parsed data to the DECIMAL type. For example, if the value of attribute a is 123456789.123456789, you can execute `SELECT s.a FROM ossobject s WHERE CAST(s.a AS DECIMAL) > 123456789.12345` to maintain the accuracy of attribute a.


## APIs and SDKs

-   API: [SelectObject](/intl.en-US/API Reference/Object operations/Basic operations/SelectObject.md)
-   OSS SDK for Java: [Query objects](/intl.en-US/SDK Reference/Java/Manage objects/Query objects.md)
-   OSS SDK for Python: [Query objects](/intl.en-US/SDK Reference/Python/Manage objects/Query objects.md)

