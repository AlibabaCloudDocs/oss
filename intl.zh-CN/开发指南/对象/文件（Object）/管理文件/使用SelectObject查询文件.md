# 使用SelectObject查询文件

您可以使用SelectObject用目标文件执行SQL语句，返回执行结果。

## 背景介绍

目前Hadoop 3.0已经支持OSS在EMR上运行Spark、Hive、Presto等服务，以及阿里自研的MaxCompute、HybridDB以及新上线的Data Lake Analytics都支持从OSS直接处理数据。

OSS提供的GetObject接口决定了大数据平台只能把OSS数据全部下载到本地然后进行分析过滤，在很多查询场景下浪费了大量带宽和客户端资源。

SelectObject接口是对上述问题的解决方案。其核心思想是大数据平台将条件、Projection下推到OSS层，让OSS做基本的过滤，从而只返回有用的数据。客户端一方面可以减少网络带宽，另一方面也减少了数据的处理量，从而节省了CPU和内存用来做其他更多的事情。这使得基于OSS的数据仓库、数据分析成为一种更有吸引力的选择。

## 细节分析

以下内容是对SelectObject支持的文件类型、支持的SQL语法等的详细介绍。

-   SelectObject支持的文件类型

    **说明：** SelectObject和Normal类型文件配合性能更佳。Multipart以及Appendable类型的文件由于其内部结构差异导致性能较差。

    -   RFC 4180标准的CSV（包括TSV等类CSV文件，文件的行列分隔符以及Quote字符都可自定义）。
    -   JSON文件，且文件编码为UTF-8。JSON支持DOCUMENT和LINES两种文件。
        -   DOCUMENT是指整个文件是单一的JSON对象。
        -   LINES表示整个文件由一行行的JSON对象组成，每一行是一个JSON对象（但整个文件本身并不是一个合法的JSON对象），行与行之间以换行分隔符隔开。OSS Select可以支持常见的\\n，\\r\\n等分隔符，且无需用户指定。
    -   标准存储类型和低频访问存储类型的文件。归档存储和冷归档存储类型文件需要先执行解冻操作。
    -   OSS完全托管加密、KMS托管主密钥加密的文件。
-   支持的SQL语法
    -   SQL 语句： Select From Where
    -   数据类型：string、int\(64bit\)、double\(64bit\), decimal\(128\) 、timestamp、bool
    -   操作： 逻辑条件（AND,OR,NOT\)， 算术表达式（+-\*/%\)， 比较操作（\>,=, <, \>=, <=, !=），String 操作 \(LIKE, \|\| \)

        **说明：** like模糊匹配时对字母大小写敏感。

-   分片查询

    和GetObject提供的基于Byte的分片下载类似，SelectObject也提供了分片查询的机制，包括以下两种分片方式：

    -   按行分片：常用的分片方式，然而对于稀疏数据来说，按行分片可能会导致分片时负载不均衡。
    -   按Split分片：Split是OSS用于分片的一个概念，一个Split包含多行数据，每个Split的数据大小大致相等。
    **说明：** 按Spit分片比按行分片更加高效。

-   数据类型

    OSS中的CSV数据默认都是String类型，用户可以使用CAST函数实现数据转换，比如下面的SQL查询将\_1和\_2转换为int后进行比较。

    `Select * from OSSOBject where cast (_1 as int) > cast(_2 as int)`

    同时，对于SelectObject支持在Where条件中进行隐式的转换，比如下面的语句中第一列和第二列将被转换成int：

    `Select _1 from ossobject where _1 + _2 > 100`

    对于JSON文件，如果在SQL中未指定cast函数，则其类型根据JSON数据的实际类型而定，标准JSON内建的数据类型包括null、bool、int64、double、string等类型。


## 常见的SQL用例

常见的SQL用例包括CSV及JSON两种。

-   常见的SQL用例（CSV）

    |应用场景|SQL语句|
    |:---|:----|
    |返回前10行数据|select \* from ossobject limit 10|
    |返回第1列和第3列的整数，并且第1列大于第3列|select \_1, \_3 from ossobject where cast\(\_1 as int\) \> cast\(\_3 as int\)|
    |返回第1列以'陈'开头的记录的个数（注：此处like后的中文需要用UTF-8编码）|select count\(\*\) from ossobject where \_1 like '陈%'|
    |返回所有第2列时间大于2018-08-09 11:30:25且第3列大于200的记录|select \* from ossobject where \_2 \> cast\('2018-08-09 11:30:25' as timestamp\) and \_3 \> 200|
    |返回第2列浮点数的平均值，总和，最大值，最小值|select AVG\(cast\(\_2 as double\)\), SUM\(cast\(\_2 as double\)\), MAX\(cast\(\_2 as double\)\), MIN\(cast\(\_2 as double\)\) |
    |返回第1列和第3列连接的字符串中以'Tom'为开头以’Anderson‘结尾的所有记录|select \* from ossobject where \(\_1 \|\| \_3\) like 'Tom%Anderson'|
    |返回第1列能被3整除的所有记录|select \* from ossobject where \(\_1 % 3\) = 0|
    |返回第1列大小在1995到2012之间的所有记录|select \* from ossobject where \_1 between 1995 and 2012|
    |返回第5列值为N,M,G,L的所有记录|select \* from ossobject where \_5 in \('N', 'M', 'G', 'L'\)|
    |返回第2列乘以第3列比第5列大100以上的所有记录|select \* from ossobject where \_2 \* \_3 \> \_5 + 100|

-   常见的SQL用例（JSON）

    假设JSON文件如下：

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
    }，…… #此处省略其他类似的节点
    ]}
    ```

    SQL用例如下：

    |应用场景|SQL语句|
    |----|-----|
    |返回所有age是27的记录|select \* from ossobject.contacts\[\*\] s where s.age = 27 |
    |返回所有的家庭电话|select s.number from ossobject.contacts\[\*\].phoneNumbers\[\*\] s where s.type = “home” |
    |返回所有单身的记录|select \* from ossobject s where s.spouse is null |
    |返回所有没有孩子的记录|select \* from ossobject s where s.children\[0\] is null **说明：** 目前没有专用的空数组的表示方法，用以上语句代替。 |


## 使用场景

SelectObject通常用于大文件分片查询、JSON文件查询、日志文件分析等场景。

-   大文件分片查询

    如果确定该CSV文件列中不含有换行符，则基于Bytes的分片由于不需要创建Meta，其使用最为简便。如果列中包含换行符或者是JSON文件时，则使用以下步骤：

    1.  调用CreateSelectObjectMeta API获得该文件的总的Split数。理想情况下如果该文件需要用SelectObject，则该API最好在查询前进行异步调用，这样可以节省扫描时间。
    2.  根据客户端资源情况选择合适的并发度n，用总的Split数除以并发度n得到每个分片查询应该包含的Split个数。
    3.  在请求Body中用诸如split-range=1-20的形式进行分片查询。
    4.  合并结果。
-   查询JSON文件时，在SQL的From 语句中尽可能的缩小From后的JSON Path 范围。

    如下是JSON文件示例：

    ```
    { contacts:[
            {“firstName”:”John”, “lastName”:”Smith”, “phoneNumbers”:[{“type”:”home”, “number”:”212-555-1234”}, {“type”:”office”, “number”:”646-555-4567”}, {“type”:”mobile”, “number”:”123 456-7890”}], ”address”:{“streetAddress”: “21 2nd Street”, “city”:”New York”, “state”:NY, “postalCode”:”10021-3100”}
             }
    ]}
    ```

    如果要查找所有postalCode为 10021开头的streetAddress，SQL可以写为`select s.address.streetAddress from ossobject.contacts[*] s where s.address.postalCode like ‘10021%’`或者`select s.streetAddress from ossobject.contacts[*].address s where s.postalCode like ‘10021%’`

    由于`select s.streetAddress from ossobject.contacts[*].address s where s.postalCode like ‘10021%’`的JSON Path更加精确，因此性能更优。

-   在JSON文件中处理高精度浮点数

    在JSON文件中需要进行高精度浮点数的数值计算时，建议设置ParseJsonNumberAsString选项为true, 同时将该值cast成Decimal。比如一个属性a值为123456789.123456789，用`select s.a from ossobject s where cast(s.a as decimal) > 123456789.12345`就可以保持原始数据的精度不丢失。


## API和SDK

-   API：[SelectObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/SelectObject.md)
-   Java ：[查询文件](/intl.zh-CN/SDK 示例/Java/管理文件/查询文件.md)
-   Python：[查询文件](/intl.zh-CN/SDK 示例/Python/管理文件/查询文件.md)

