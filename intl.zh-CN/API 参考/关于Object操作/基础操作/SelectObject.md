# SelectObject

SelectObject用于对目标文件执行SQL语句，返回执行结果。

**说明：**

-   此操作要求您对该Object有读权限。
-   正确执行时，该API返回206。如果SQL语句不正确，或者和文件不匹配，则会返回400错误。
-   关于SelectObject的功能介绍请参见开发指南中的[SelectObject](/intl.zh-CN/开发指南/对象/文件（Object）/管理文件/使用SelectObject查询文件.md)。

## 请求语法

请求语法分为 CSV 和 JSON 两种格式。

-   请求语法（CSV）

    ```
    POST /object?x-oss-process=csv/select HTTP/1.1 
    HOST: BucketName.oss-cn-hangzhou.aliyuncs.com 
    Date: time GMT
    Content-Length: ContentLength
    Content-MD5: MD5Value 
    Authorization: Signature
    <?xml  version="1.0"  encoding="UTF-8"?>
    <SelectRequest>    
        <Expression>base64 encode的SQL语句，例如c2VsZWN0IGNvdW50KCopIGZyb20gb3Nzb2JqZWN0IHdoZXJlIF80ID4gNDU=</Expression>
        <InputSerialization>
            <CompressionType>None|GZIP</CompressionType>
            <CSV>            
                <FileHeaderInfo>
                    NONE|IGNORE|USE
                </FileHeaderInfo>
                <RecordDelimiter>base64 encode的字符</RecordDelimiter>
                <FieldDelimiter>base64 encode的字符</FieldDelimiter>
                <QuoteCharacter>base64 encode的字符</QuoteCharacter>
                <CommentCharacter>base64 encode的字符</CommentCharacter>
                <Range>line-range=start-end|split-range=start-end</Range>
                <AllowQuotedRecordDelimiter>true|false</AllowQuotedRecordDelimiter>
            </CSV>   
            </InputSerialization>
            <OutputSerialization>
                 <CSV>
                 <RecordDelimiter>base64 encode的字符</RecordDelimiter>
                 <FieldDelimiter>base64 encode的字符</FieldDelimiter>
    
                </CSV>
                <KeepAllColumns>false|true</KeepAllColumns>
                <OutputRawData>false|true</OutputRawData>
                <EnablePayloadCrc>true</EnablePayloadCrc>
                <OutputHeader>false</OutputHeader>
           </OutputSerialization>
         <Options>
            <SkipPartialDataRecord>false</SkipPartialDataRecord>
            <MaxSkippedRecordsAllowed>
            max allowed number of records skipped
            </MaxSkippedRecordsAllowed>
        </Options>
    </SelectRequest>
    ```

-   请求语法（JSON）

    ```
    POST /object?x-oss-process=json/select HTTP/1.1 
    HOST: BucketName.oss-cn-hangzhou.aliyuncs.com 
    Date: time GMT
    Content-Length: ContentLength
    Content-MD5: MD5Value 
    Authorization: Signature
    <?xml  version="1.0"  encoding="UTF-8"?>
    <SelectRequest>    
        <Expression>
            base64 encode的SQL语句，例如c2VsZWN0IGNvdW50KCopIGZyb20gb3Nzb2JqZWN0IHdoZXJlIF80ID4gNDU=
        </Expression>
        <InputSerialization>
            <CompressionType>None|GZIP</CompressionType>
            <JSON>
                <Type>DOCUMENT|LINES</Type>
                <Range>
                line-range=start-end|split-range=start-end
                </Range>
                <ParseJsonNumberAsString> true|false
                </ParseJsonNumberAsString>
            </JSON>
        </InputSerialization>
        <OutputSerialization>
            <JSON>
                <RecordDelimiter>
                    Base64 of record delimiter
                </RecordDelimiter>
            </JSON>
            <OutputRawData>false|true</OutputRawData>
                     <EnablePayloadCrc>true</EnablePayloadCrc>
        </OutputSerialization>
        <Options>
            <SkipPartialDataRecord>
                false|true
            </SkipPartialDataRecord>
            <MaxSkippedRecordsAllowed>
                max allowed number of records skipped
               </MaxSkippedRecordsAllowed>
            </Options>
    </SelectRequest>
    ```


## 请求元素

|名称|类型|描述|
|:-|:-|:-|
|SelectRequest|容器|保存Select请求的容器

子节点：Expression，InputSerialization，OutputSerialization

父节点：None |
|Expression|字符串|以Base64 编码的SQL语句

子节点：None

父节点：SelectRequest |
|InputSerialization|容器|输入序列化参数（可选）

子节点：CompressionType、CSV、JSON

父节点：SelectRequest |
|OutputSerialization|容器|输出序列化参数（可选）

子节点：CSV、JSON、OutputRawData

父节点：SelectRequest |
|CSV\(InputSerialization\)|容器|输入CSV的格式参数（可选）

子节点：FileHeaderInfo、RecordDelimiter、FieldDelimiter、QuoteCharacter、CommentCharacter、 Range

父节点：InputSerialization |
|CSV\(OutputSerialization\)|容器|输出CSV的格式参数（可选）

子节点： RecordDelimiter、 FieldDelimiter

父节点：OutputSerialization |
|JSON\(InputSerialization\)|容器|输入JSON的格式参数（可选）

子节点：Type、Range、ParseJsonNumberAsString |
|JSON\(OutputSerialization\)|容器|输出JSON的格式参数（可选）

子节点：RecordDelimiter |
|Type|枚举|指定输入JSON的类型：DOCUMENT 、 LINES |
|OutputRawData|bool，默认false|指定输出数据为纯数据（可选）

子节点：None

父节点：OutputSerialization

**说明：**

-   您在请求中指定OutputRawData值时，OSS服务端会按照请求中的要求返回数据。
-   您在请求中不指定OutputRawData值时，OSS服务端会自动选择一种格式返回。
-   当您显式地指定OutputRawData为True时，如果该SQL长时间内没有返回数据，则HTTP请求可能因没有数据返回而超时。 |
|CompressionType|枚举|指定文件压缩类型：None\|GZIP

子节点：None

父节点：InputSerialization |
|FileHeaderInfo|枚举|指定CSV文件头信息（可选）

取值：

-   Use：该CSV文件有头信息，可以用CSV列名作为Select中的列名。
-   Ignore：该CSV文件有头信息，但不可用CSV列名作为Select中的列名。
-   None：该文件没有头信息，为默认值。

子节点：None

父节点：CSV （输入） |
|RecordDelimiter|字符串|指定换行符，以Base64编码。默认值为`\n`（可选）。未编码前的值最多为两个字符，以字符的ANSI值表示，例如在Java中使用`\n`表示换行。

子节点：None

父节点：CSV （输入、输出），JSON（输出） |
|FieldDelimiter|字符串|指定CSV列分隔符，以Base64编码。默认值为`,`（可选）。未编码前的值必须为一个字符，以字符的ANSI值表示，例如在Java中使用`,`表示逗号。

子节点：None

父节点：CSV （输入，输出） |
|QuoteCharacter|字符串|指定CSV的引号字符，以Base64编码。默认值为`\”`（可选）。在CSV中引号内的换行符，列分隔符将被视作普通字符。未编码前的值必须为一个字符，以字符的ANSI值表示，例如在Java中使用`\”`表示引号。

子节点：None

父节点：CSV （输入） |
|CommentCharacter|字符串|指定CSV的注释符，以Base64编码。默认值为空（即没有注释符）。|
|Range|字符串|指定查询文件的范围（可选）。支持两种格式：

**说明：** 使用Range参数查询的文件需要有select meta。select meta详情请参考[CreateSelectObjectMeta](#CreateSelectObjectMeta)。

-   按行查询：line-range=start-end。例如line-range=10-20表示扫描第10行到第20行。
-   按Split查询：split-range=start-end。例如split-range=10-20表示扫描第10到第20个split。

其中start和end均为inclusive。其格式和range get中的range参数一致。

仅在文档是CSV或者JSON Type为LINES时使用。

子节点：None

父节点：CSV （输入）、JSON（输入） |
|KeepAllColumns|bool|指定返回结果中包含CSV所有列的位置（可选，默认值为false）。但仅仅在select 语句里出现的列会有值，不出现的列则为空，返回结果中每一行的数据按照CSV列的顺序从低到高排列。例如以下语句：

`select _5, _1 from ossobject.`

如果KeepAllColumns = true，假设一共有6列数据，则返回以下数据：

Value of 1st column,,,,Value of 5th column,\\n

子节点：None

父节点：OutputSerialization（Csv） |
|EnablePayloadCrc|bool|在每个Frame中会有一个32位的crc32校验值。客户端可以计算相应payload的Crc32值进行数据完整性校验。

子节点：None

父节点：OutputSerialization |
|Options|容器|额外的可选参数。

子节点：SkipPartialDataRecord、MaxSkippedRecordsAllowed

父节点：SelectRequest |
|OutputHeader|bool|在返回结果开头输出CSV头信息。

默认false。

子节点：None

父节点：OutputSerialization |
|SkipPartialDataRecord|bool|忽略缺失数据的行。当该参数为false时， OSS会忽略缺失某些列（该列值当做null）而不报错。当该参数为true时，该行数据因为不完整而被整体跳过。当跳过的行数超过指定的最大跳过行数时OSS会报错并停止处理。

默认false。

子节点：None

父节点：Options |
|MaxSkippedRecordsAllowed|Int|指定最大能容忍的跳过的行数。当某一行数据因为不匹配SQL中期望的类型、或者某一列或者多列数据缺失且SkipPartialDataRecord为True时，该行数据会被跳过。如果跳过的行数超过该参数的值，OSS会停止处理并报错。

**说明：** 如果某一行是非法CSV行，例如在一列中间连续含有奇数个quote字符，则OSS会马上停止处理并报错，因为该错误很可能会影响对整个CSV文件的解析。即该参数用来调整对非整齐数据的容忍度，但不应用于非法的CSV文件。

默认 0。

子节点：None

父节点：Options |
|ParseJsonNumberAsString|bool|将JSON中的数字（整数和浮点数）解析成字符串。目前JSON中的浮点数解析时会损失精度，如果要完整保留原始数据，则推荐用该选项。如果需要进行数值计算，则可以在SQL中cast成需要的格式，例如int、double、decimal。

默认值： false

子节点：None

父节点：JSON |
|AllowQuotedRecordDelimiter|bool|指定CSV内容是否含有在引号中的换行符。 例如某一列值为"abc\\ndef" （此处\\n为换行）， 则该值需设置为true。当该值为false时，select支持header range的语义，可以更高效的进行分片查询。

默认值：true

子节点：None

父节点：InputSerialization |

## 返回Body

-   当请求响应中HTTP Status是4xx，表明该请求没有通过相应的SQL语法检查或者目标文件有问题，返回错误信息的Body与Get API返回的错误信息一致。
-   当请求响应中HTTP Status是5xx，表明服务器内部错误，返回的错误信息的格式和Get API返回的错误信息一致。
-   当请求成功返回206时：
    -   若header x-oss-select-output-raw值为true，则表明返回结果是对象数据（而不是Frame包装的），客户端可以完全按照Get API的方式获取数据。
    -   若x-oss-select-output-raw的值为false，请求结果以Frame形式返回。
-   Frame的格式为`Version|Frame-Type | Payload Length | Header Checksum | Payload | Payload Checksum<1 byte><--3 bytes--><---4 bytes----><-------4 bytes--><variable><----4bytes---------->`。

    **说明：** Checksum均为CRC32。Frame里所有的整数均以大端编码（big endian），且Version目前为1。


## Frame Type

SelectObject分为Data Frame、Continuous Frame以及End Frame三种不同的Frame Type，详细信息请参见下表。

|名称|Frame-Type值|Payload格式|描述|
|--|-----------|---------|--|
|Data Frame|8388609|offset \| data<-8 bytes\><---variable-\>|包含Select请求返回的数据，并用offset来汇报进展。offset为当前扫描位置（从文件头开始的偏移），值为8位整数。|
|Continuous Frame|8388612|offset<----8 bytes--\>|用以汇报当前进展以及维持HTTP连接。如果该查询在5s内未返回数据，则返回一个Continuous Frame。|
|End Frame|8388613|offset \| total scanned bytes \| http status code \| error message<--8bytes-\><--8bytes---------\><----4 bytes--------\><-variable------\>|用来返回最终的状态，扫描的字节数以及可能的出错信息。 -   offset为扫描后最终的位置偏移。
-   total scanned bytes为最终扫描过的数据大小。
-   http status code为最终的处理结果。

**说明：** 返回status code的原因在于SelectObject为流式处理，因而在发送Response Header时仅处理了第一个Block。如果第一个Block数据和SQL匹配，则在Response Header中的Status为206。如果如下的数据非法，且已无法更改Header中的Status，则只能在End Frame里包含最终的Status及其出错信息。此时客户端应视其为最终状态。

-   error message为错误信息，包括所有跳过的行号及其总行数。

**说明：** error message格式为`ErrorCodes.DetailMessage`。其中ErrorCodes包含一个或者多个ErrorCode，多个ErrorCode之间用逗号隔开。ErrorCodes和DetailMessage之间用句号（.）隔开。 |

## 样例请求

样例请求分为 CSV 和 JSON 两种格式。

-   样例请求（CSV）

    ```
    POST /oss-select/bigcsv_normal.csv?x-oss-process=csv%2Fselect HTTP/1.1
    Date: Fri, 25 May 2018 22:11:39 GMT
    Content-Type:
    Authorization: OSS LTAIJPLocA0fD:FC/9JRbBGRw4o2QqdaL246Px****
    User-Agent: aliyun-sdk-dotnet/2.8.0.0(windows 16.7/16.7.0.0/x86;4.0.30319.42000)
    Content-Length: 748
    Expect: 100-continue
    Connection: keep-alive
    Host: host name
    <?xml version="1.0"?>
    <SelectRequest>
        <Expression>c2VsZWN0IGNvdW50KCopIGZyb20gb3Nzb2JqZWN0IHdoZXJlIF80ID4gNDU=
        </Expression>
        <InputSerialization>
            <Compression>None</Compression>
            <CSV>
                <FileHeaderInfo>Ignore</FileHeaderInfo>
                <RecordDelimiter>Cg==</RecordDelimiter>
                <FieldDelimiter>LA==</FieldDelimiter>
                <QuoteCharacter>Ig==</QuoteCharacter>
                <CommentCharacter>Iw==</CommentCharacter/>
            </CSV>
        </InputSerialization>
        <OutputSerialization>
            <CSV>
                <RecordDelimiter>Cg==</RecordDelimiter>
                <FieldDelimiter>LA==</FieldDelimiter>
                <QuoteCharacter>Ig==</QuoteCharacter>            
            </CSV>
            <KeepAllColumns>false</KeepAllColumns>
                <OutputRawData>false</OutputRawData>
        </OutputSerialization>
    </SelectRequest>
    ```

-   样例请求（JSON）

    ```
    POST /oss-select/sample_json.json?x-oss-process=json%2Fselect HTTP/1.1
    Host: host name
    Accept-Encoding: identity
    User-Agent: aliyun-sdk-python/2.6.0(Darwin/16.7.0/x86_64;3.5.4)
    Accept: */*
    Connection: keep-alive
    date: Mon, 10 Dec 2018 18:28:11 GMT
    authorization: OSS AccessKeySignature
    Content-Length: 317
    <SelectRequest>
        <Expression>c2VsZWN0ICogZnJvbSBvc3NvYmplY3Qub2JqZWN0c1sqXSB3aGVyZSBwYXJ0eSA9ICdEZW1vY3JhdCc=
        </Expression>
        <InputSerialization>
        <JSON>
            <Type>DOCUMENT</Type>
        </JSON>
        </InputSerialization>
        <OutputSerialization>
        <JSON>
            <RecordDelimiter>LA==</RecordDelimiter>
        </JSON>
        </OutputSerialization>
        <Options />
    </SelectRequest>
    ```


## SQL语句正则表达式

SQL语句正则表达式为`SELECT select-list from table where_opt limit_opt`。

**说明：** 其中，SELECT、OSSOBJECT以及WHERE为关键字不得更改。

```
select_list: column name
            | column index (例如_1, _2. 仅Csv文件有column index)
            | json path (例如s.contacts.firstname 仅应用在JSON文件)
            | function(column index | column name)
            | function(json_path) (仅应用在JSON文件)
            | select_list AS alias
```

**说明：** 支持的function为AVG、SUM、MAX、MIN、COUNT、 CAST（类型转换函数）。其中COUNT后面只能用\*。

```
table: OSSOBJECT

      | OSSOBJECT json_path（仅应用于JSON文件）

对于CSV文件，table必须为OSSOBJECT。对于JSON文件（包括DOCUMENT或者LINES），table可以在OSSOBJECT后指定json_path。

json_path: ['string '] （string若没有空格或者*，则引号可以去掉；等价于.'string '）

          | [n] （应用在数组元素中，表示第n个元素，从0开始）

          | [*] （应用在数组或者对象中，表示任意子元素）

          | .'string ' （string若没有空格或者*，则引号可以去掉）

          | json_path jsonpath （json_path可以连接，例如 [n].property1.attributes[*]）
```

```
Where_opt:
| WHERE expr
expr:
| literal value
| column name
| column index
| json path (仅应用于JSON文件)
| expr op expr
| expr OR expr
| expr AND expr
| expr IS NULL
| expr IS NOT NULL
| (column name | column index | json path) IN (value1, value2,….)
| (column name | column index | json path) NOT in (value1, value2,…)
| (column name | column index | json path) between value1 and value2
| NOT (expr)
| expr op expr
| (expr)
| cast (column index |column name | json path | literal as INT|DOUBLE|)
```

-   op：包括`>`、`<` 、`>=`、`<=`、`!=`、 `=`、`，`、`LIKE`、`+`、`-`、`*`、`/`、`%`以及字符串连接`||`。
-   cast：对于同一个column，只能cast成一种类型。
-   聚合和Limit的混用：`Select avg(cast(_1 as int)) from ossobject limit 100`，其含义是指在前100行中计算第一列的AVG值。与MySQL不同的是在SelectObject中聚合永远只返回一行数据，因而对聚合来说无需限制其输出规模。SelectObject中limit将先于聚合函数执行。

## SQL语句限制

SQL语句限制如下：

-   目前仅仅支持UTF-8编码的文本文件以及GZIP压缩过的UTF-8文本。GZIP文件不支持deflate格式。
-   仅支持单文件查询，不支持join、order by、group by、having。
-   Where语句里不能包含聚合条件（e.g. where max\(cast\(age as int\)\) \> 100这个是不允许的）。
-   支持的最大的列数是1000，SQL中最大列名长度不能超过1024字节。
-   在LIKE语句中，支持最多5个%通配符。\*和%是等价的，表示0或多个任意字符。LIKE支持Escape关键字，用于将Pattern中的%\*?特殊字符Escape成普通字符串。
-   在IN语句中，最多支持1024个常量项。
-   Select后的Projection可以是列名，CSV列索引（\_1, \_2等），或者是聚合函数，或者是CAST函数；不支持其他表达式。 例如select \_1 + \_2 from ossobject是不允许的。
-   支持的CSV单行及单列的最大字符数均为256KB。
-   支持的 from 后json path指定的JSON最大节点为512KB，最大深度为10层，其数组内元素的个数最多为5000。select以及where后的字段必须是来自from后json path对应的节点。
-   JSON文件的SQL中，select 或者where条件的表达式中不能有\[\*\]数组通配符，数组通配符只能出现在from后的json path中。（例如select s.contacts\[\*\] from ossobject s是不允许的，但是select \* from ossobject.contacts\[\*\]是可以的）。
-   SQL最大长度为16KB，where后面表达式个数最多20个，表达式深度最多10层，聚合操作最多100个。

## 数据的容错处理

以下列举了CSV的某些行缺失了某些列、JSON缺失某些Key、CSV的某些列类型不匹配SQL，以及JSON中某些Key类型不匹配SQL等常见的数据容错处理方法。

-   CSV的某些行缺失了某些列

    当SkipPartialDataRecord没有被指定或者其值为False时，OSS会把缺失的列都当做null来计算SQL中的表达式。

    当SkipPartialDataRecord指定为True时，则OSS会忽略该行数据。此时，如果MaxSkippedRecordsAllowed未指定或者指定的值小于忽略的行数，则OSS会通过http status code或者End Frame的形式返回400错误。

    当SQL为`select _1, _3 from ossobject`，而CSV的某一行指定为 “张小,阿里巴巴”。

    -   当SkipPartialDataRecord指定为False时，返回内容为”张小,\\n”
    -   当SkipPartialDataRecord指定为True时，该行被跳过。
-   JSON缺失某些Key

    和CSV一样，JSON的某些对象可能未包括SQL中指定的Key。

    -   当SkipPartialDataRecord没有被指定或者其值为False时，此时OSS会把缺失的Key当做Null来计算SQL中的表达式。
    -   如果SkipPartialDataRecord为true时，OSS则会忽略该JSON节点的数据 。此时，如果MaxSkippedRecordsAllowed未指定或者指定的值小于忽略的行数，则OSS会通过http status code或者End Frame返回400错误。
    例如SQL为 `select s.firstName, s.lastName , s.age from ossobject.contacts[*] s`，某个JSON节点为\{“firstName”:”小”，”lastName”:”张”\}。

    -   当SkipPartialDataRecord为False或未指定时，该节点的返回值为\{“firstName”:”小”, “lastName”:”张”\}。
    -   当SkipPartialDataRecord为True时则跳过该行。
    **说明：** 对于JSON返回数据中Key的处理，SelectJson的输出只能为JSON LINES。输出结果中的Key值按以下规则确定。

    -   当SQL为`select * from ossobject…`时，如果\*返回的内容是一个JSON对象（\{…\}），则原样返回；如果\*不是一个JSON对象（例如是字符串或者数组），则会使用一个DummyKey \_1来表示。

        当数据为\{“Age”:5\}`select * from ossobject.Age s where s = 5`，此时\*对应的内容5不是一个JSON对象，故返回的内容为\{“\_1”:5\}。假设SQL为`select * from ossobject s where s.Age = 5` ，此时\*对应的内容是\{“Age”:5\}，故原样返回。

    -   当SQL没有使用select \*而是指定了列时，则返回的内容格式为`{“{列1}”:值, “{列2}”：值…..}`。 其中\{列n\}的生成方法如下：

        -   当select中指定了该列的Alias时用Alias。
        -   如果该列是一个JSON对象的Key时，用该Key作为输出的Key值。
        -   如果该列是JSON数组的某个元素时或者是聚合函数，用该列在输出中的序号（从1开始）加上前缀\_作为输出值的key。
        当数据为\{”contacts”:\{“Age”:35, “Children”:\[“child1”, “child2”,”child3”\]\}\}：

        -   当SQL为`select s.contacts.Age, s.contacts.Children[0] from ossobject s` ，由于Age是输入JSON对象的Key，但Children\[0\]是数组Children的第一个元素，在输出中是第二列，故为”\_2”:“child1”，则输出为 \{“Age”:35, “\_2”:”child1”\}。
        -   当SQL为`select max(cast(s.Age as int)) from ossobject.contacts s`，由于该列是聚合函数，故而用该列在输出中的序号加上前缀\_1表示Key，则输出为\{“\_1”:35\}。
        -   如果指定了Alias`select s.contacts.Age, s.contacts.Children[0] as firstChild from ossobject`，则输出为\{“Age”:35, “firstChild”:”child1”\}。
    -   JSON文件和SQL中Key的匹配是大小写敏感的，例如select s.Age 和select s.age是不同的。
-   CSV的某些列类型不匹配SQL

    当CSV中某些行的数据类型和SQL中指定的数据类型不匹配时，该行数据会被忽略。和上面情况类似，其行为受到MaxSkippedRecordsAllowed的影响。当忽略行数超过上限时，OSS将处理中转并返回400。

    当SQL为`select _1, _3 from ossobject where _3 > 5`，某CSV行为`张小，阿里巴巴，待入职`时，由于第三列不是整数类型，则该行被跳过。

-   JSON中某些Key类型不匹配SQL

    当SQL为`select s.name from ossobject s where s.aliren_age > 5`，JSON节点是`{"Name":"张小", "aliren_age":待入职}`，则该节点被跳过。


## CreateSelectObjectMeta

CreateSelectObjectMeta API用于获取目标文件总的行数，总的列数（对于CSV文件），以及Splits个数。如果该信息不存在，则会扫描整个文件，分析并记录下CSV文件的上述信息。重复调用该API则会保存上述信息而不必重新扫描整个文件。

**说明：**

-   CreateSelectObjectMeta操作要求您对该Object有写权限。
-   如果该API执行正确，返回200。如果目标文件不是合法CSV或者JSON LINES文件，或者指定的CSV分隔符和目标CSV不匹配，则返回400。

-   请求语法
    -   请求语法（CSV）

        ```
        POST  /samplecsv?x-oss-process=csv/meta
        <CsvMetaRequest>
            <InputSerialization>
                <CompressionType>None</CompressionType>
                <CSV>
                    <RecordDelimiter>base64 encode的字符</RecordDelimiter>
                    <FieldDelimiter>base64 encode的字符</FieldDelimiter>
                    <QuoteCharacter>base64 encode的字符</QuoteCharacter>
                </CSV>
            </InputSerialization>
            <OverwriteIfExists>false|true</OverwriteIfExists>
        </CsvMetaRequest>
        ```

    -   请求语法（JSON）

        ```
        POST  /samplecsv?x-oss-process=json/meta
        <JsonMetaRequest>
            <InputSerialization>
                <CompressionType>None</CompressionType>
                <JSON>
                    <Type>LINES</Type>
                </JSON>
            </InputSerialization>
            <OverwriteIfExists>false|true</OverwriteIfExists>
        </JsonMetaRequest>
        ```

-   请求元素

    |名称|类型|描述|
    |:-|--|:-|
    |CsvMetaRequest|容器|保存创建Select csv Meta请求的容器。

子节点：InputSerialization

父节点：None |
    |JsonMetaRequest|容器|保存创建 Select json meta请求的容器。

子节点：InputSerialization

父节点：None |
    |InputSerialization|容器|输入序列化参数（可选）

子节点：CompressionType、 CSV、JSON

父节点：CsvMetaRequest、JsonMetaRequest |
    |OverwriteIfExists|bool|重新计算SelectMeta，覆盖已有数据。（可选，默认是false，即如果Select Meta已存在则直接返回）

子节点：None

父节点：CsvMetaRequest、JsonMetaRequest |
    |CompressionType|枚举|指定文件压缩类型（可选）。目前不支持任何压缩，故只能为None。

子节点： None

父节点：InputSerialization |
    |RecordDelimiter|字符串|指定CSV换行符，以Base64编码。默认值为’\\n’（可选）。未编码前的值最多为两个字符，以字符的ANSI值表示，例如在Java里用\\n表示换行。

子节点：None

父节点：CSV |
    |FieldDelimiter|字符串|指定CSV列分隔符，以Base64编码。默认值为`，`（可选）

未编码前的值必须为一个字符，以字符的ANSI值表示，例如Java里用`，`表示逗号。

子节点：None

父节点：CSV （输入，输出） |
    |QuoteCharacter|字符串|指定CSV的引号字符，以Base64编码。默认值为\\”（可选）。在CSV中引号内的换行符，列分隔符将被视作普通字符。未编码前的值必须为一个字符，以字符的ANSI值表示，例如Java里用\\”表示引号。

子节点：None

父节点：CSV （输入） |
    |CSV|容器|指定CSV输入格式。

子节点：RecordDelimiter，FieldDelimiter，QuoteCharacter

父节点：InputSerialization |
    |JSON|容器|指定JSON输入格式。

子节点：Type

父节点：InputSerialization |
    |Type|枚举|指定JSON类型。

合法值 ：LINES |

-   返回Body

    和SelectObject 类似，CreateSelectObjectMeta API也以Frame的形式返回。

    |名称|Frame-Type值|Payload格式|描述|
    |:-|:----------|:--------|:-|
    |Meta End Frame（Csv）|8388614|offset \| total scanned bytes \| status\| splits count \| rows count \| columns count \| error message

<-8 bytes\><------8 bytes------\><--4bytes\><--4 bytes--\><--8 bytes\><--4 bytes---\><variable size\>

|用来汇报CreateSelectObjectMeta API最终的状态。

    -   offset：8位整数，扫描结束时的文件偏移。
    -   total scanned bytes：8位整数，最终扫描过的数据大小。
    -   status：4位整数，最终的status。
    -   splits\_count：4位整数，总split个数。
    -   rows\_count：8位整数，总行数。
    -   cols\_count：4位整数，总列数。
    -   error\_message：详细的错误信息。若无错误，则error\_message为空。 |
    |Meta End Frame（Json）|8388615|offset \| total scanned bytes \| status\| splits count \| rows count \| error message

<-8 bytes\><------8 bytes------\><--4bytes\><--4 bytes--\><--8 bytes\><variable size\>

|用来汇报CreateSelectObjectMeta API最终的状态。

    -   offset：8位整数，扫描结束时的文件偏移。
    -   total scanned bytes：8位整数，最终扫描过的数据大小。
    -   status：4位整数，最终的status。
    -   splits\_count：4位整数，总split个数。
    -   rows\_count：8位整数，总行数。
    -   error\_message：详细的错误信息。若无错误，则error\_message为空。 |

-   样例请求
    -   样例请求（CSV）

        ```
        POST /oss-select/bigcsv_normal.csv?x-oss-process=csv%2Fmeta HTTP/1.1
        Date: Fri, 25 May 2018 23:06:41 GMT
        Content-Type:
        Authorization: OSS AccessKeySignature
        User-Agent: aliyun-sdk-dotnet/2.8.0.0(windows 16.7/16.7.0.0/x86;4.0.30319.42000)
        Content-Length: 309
        Expect: 100-continue
        Connection: keep-alive
        Host: Host
        <?xml version="1.0"?>
        <CsvMetaRequest>
            <InputSerialization>
                <CSV>
                    <RecordDelimiter>Cg==</RecordDelimiter>
                    <FieldDelimiter>LA==</FieldDelimiter>
                    <QuoteCharacter>Ig==</QuoteCharacter>
                </CSV>
            </InputSerialization>
            <OverwriteIfExisting>false</OverwriteIfExisting>
        </CsvMetaRequest>
        ```

    -   样例请求（JSON）

        ```
        POST /oss-select/sample.json?x-oss-process=json%2Fmeta HTTP/1.1
        Date: Fri, 25 May 2018 23:06:41 GMT
        Content-Type:
        Authorization: OSS AccessKeySignature
        User-Agent: aliyun-sdk-dotnet/2.8.0.0(windows 16.7/16.7.0.0/x86;4.0.30319.42000)
        Content-Length: 309
        Expect: 100-continue
        Connection: keep-alive
        Host: Host
        <?xml version="1.0"?>
        <JsonMetaRequest>
            <InputSerialization>
                <JSON>
                    <Type>LINES</Type>
                </JSON>
            </InputSerialization>
            <OverwriteIfExisting>false</OverwriteIfExisting>
        </JsonMetaRequest>
        ```


## 支持的时间格式

自动识别是指无需指定日期格式就能把符合以下格式之一的字符串转成timestamp类型。例如cast­\('20121201' as timestamp\)会被解析成2012年12月1日。

以下格式为自动识别的格式：

|格式|说明|
|:-|:-|
|YYYYMMDD|年月日|
|YYYY/MM/DD|年/月/日|
|DD/MM/YYYY/|日/月/年|
|YYYY-MM-DD|年-月-日|
|DD-MM-YY|日-月-年|
|DD.MM.YY|日.月.年|
|HH:MM:SS.mss|小时:分钟:秒.毫秒|
|HH:MM:SS|小时:分钟:秒|
|HH MM SS mss|小时 分钟 秒 毫秒|
|HH.MM.SS.mss|小时.分钟.秒.毫秒|
|HHMM|小时分钟|
|HHMMSSmss|小时分钟秒毫秒|
|YYYYMMDD HH:MM:SS.mss|年月日 小时:分钟:秒.毫秒|
|YYYY/MM/DD HH:MM:SS.mss|年/月/日 小时:分钟:秒.毫秒|
|DD/MM/YYYY HH:MM:SS.mss|日/月/年 小时:分钟:秒.毫秒|
|YYYYMMDD HH:MM:SS|年月日 小时:分钟:秒|
|YYYY/MM/DD HH:MM:SS|年/月/日 小时:分钟:秒|
|DD/MM/YYYY HH:MM:SS|日/月/年 小时:分钟:秒|
|YYYY-MM-DD HH:MM:SS.mss|年-月-日 小时:分钟:秒.毫秒|
|DD-MM-YYYY HH:MM:SS.mss|日-月-年 小时:分钟:秒.毫秒|
|YYYY-MM-DD HH:MM:SS|年-月-日 小时:分钟:秒|
|YYYYMMDDTHH:MM:SS|年月日T小时:分钟:秒|
|YYYYMMDDTHH:MM:SS.mss|年月日T小时:分钟:秒.毫秒|
|DD-MM-YYYYTHH:MM:SS.mss|日-月-年T小时:分钟:秒.毫秒|
|DD-MM-YYYYTHH:MM:SS|日-月-年T小时:分钟:秒|
|YYYYMMDDTHHMM|年月日T小时分钟|
|YYYYMMDDTHHMMSS|年月日T小时分钟秒|
|YYYYMMDDTHHMMSSMSS|年月日T小时分钟秒毫秒|
|ISO8601-0|年-月-日T小时:分钟+小时:分钟，或者年-月-日T小时:分钟-小时:分钟

这里+表示时区在标准时间UTC前面，-表示在后面。注意这个格式可用ISO8601-0来表示 |
|ISO8601-1|年-月-日T小时:分钟:秒+小时:分钟，或者年-月-日T小时:分钟-小时:分钟

这里+表示时区在标准时间UTC前面，-表示在后面. 这个格式可用ISO8601-1来表示 |
|CommonLog|例如28/Feb/2017:12:30:51 +0700|
|RFC822|例如Tue, 28 Feb 2017 12:30:51 GMT|
|?D/?M/YY|日/月/年，此处日月均可用1位或者2位表示|
|?D/?M/YY ?H:?M|日/月/年/小时:分钟，此处日月分钟小时均可用1位或者2位表示|
|?D/?M/YY ?H:?M:?S|日/月/年/小时:分钟:秒，此处日月秒分钟小时均可用1位或者2位表示|

由于以下格式会引起歧义，因此需要指定格式。如cast\('20121201' as timestamp format 'YYYYDDMM'\)会将20121201解析成2012年1月12日。

|格式|说明|
|:-|:-|
|YYYYDDMM|年日月|
|YYYY/DD/MM|年/日/月|
|MM/DD/YYYY|月/日/年|
|YYYY-DD-MM|年-日-月|
|MM-DD-YYYY|月-日-年|
|MM.DD.YYYY|月.日.年|

## ErrorCode

SelectObject通过以下两种形式返回ErrorCode。

-   一种是和其他OSS请求一样，在HTTP响应Headers里返回http status code，并在body里返回Error信息。此类错误通常是存在明显的输入错误（例如输入的SQL不正确），或者明显的数据错误。
-   另一种ErrorCode是在响应Body的End Frame中里返回。这种情况通常是SelectObject处理数据的过程中，检测到数据不正确或者和SQL不匹配（例如在某一行里原本是整数列现在是一个字符串，且SQL里指定了该列是整数），此时往往部分数据已处理完毕并已发送到客户端，且http status code仍然是206。

在下表的ErrorCode中，有些ErrorCode（例如InvalidCSVLine）可能以http status code的形式返回，也可能在End Frame中的status code返回，以何种形式返回ErrorCode取决于发生错误的CSV行在文件的具体位置。

|ErrorCode|描述|HTTP Status Code|Http Status Code in End Frame|
|---------|--|----------------|-----------------------------|
|InvalidSqlParameter|非法的SQL参数。 请求中SQL为空或者SQL长度超过最大值或者SQL不是以base64编码。

|400|无|
|InvalidInputFieldDelimiter|非法的CSV列分隔符（输入）。 该参数不是以Base64编码或者解码后的长度大于1。

|400|无|
|InvalidInputRecordDelimiter|非法的CSV行分隔符（输入）。该参数不是以Base64编码或者解码后长度大于2。|400|无|
|InvalidInputQuote|非法的CSV Quote字符（输入）。该参数不是以Base64编码或者解码后长度超过1。|400|无|
|InvalidOutputFieldDelimiter|非法的CSV列分隔符（输出）。该参数不是以Base64编码或者解码后的长度大于1。|400|无|
|InvalidOutputRecordDelimiter|非法的CSV行分隔符（输出）。该参数不是以Base64编码或者解码后长度大于2。|400|无|
|UnsupportedCompressionFormat|非法的Compression参数。该参数值不是NONE或者GZIP（此处大小写无关）。|400|无|
|InvalidCommentCharacter|非法的CSV注释字符。该参数值不是以Base64编码或者解码后的值超过1。|400|无|
|InvalidRange|非法的Range参数。该参数不是以line-range=或者split-range=作为前缀，或者range值本身不符合HTTP range规范。|400|无|
|DecompressFailure|Compression参数是GZIP且解压失败。|400|无|
|InvalidMaxSkippedRecordsAllowed|MaxSkippedRecordsAllowed参数不是整数。|400|无|
|SelectCsvMetaUnavailable|指定了Range参数，但目标对象没有CSV meta时，此时应该先调用Create Select object MetaAPI。|400|无|
|InvalidTextEncoding|对象不是以UTF-8编码。|400|无|
|InvalidOSSSelectParameters|同时指定了EnablePayloadCrc为True以及OutputRawData为True时，两个参数冲突，返回此错误。|400|无|
|InternalError|OSS系统出现错误。|500或者206|无或者500|
|SqlSyntaxError|Base64解码后的SQL语法错误。|400|无|
|SqlExceedsMaxInCount|SQL中IN语句包含的值数目超过1024。|400|无|
|SqlExceedsMaxColumnNameLength|SQL中列名超过最大长度时1024。|400|无|
|SqlInvalidColumnIndex|SQL中列Index小于1或者大于1000。|400|无|
|SqlAggregationOnNonNumericType|SQL中聚合函数应用在非数值列中。|400|无|
|SqlInvalidAggregationOnTimestamp|SQL中SUM/AVG聚合应用在日期类型列。|400|无|
|SqlValueTypeOfInMustBeSame|SQL中IN语句包含不同类型的值。|400|无|
|SqlInvalidEscapeChar|SQL LIKE语句中Escape的字符为?或%或\*。|400|无|
|SqlOnlyOneEscapeCharIsAllowed|SQL LIKE语句中Escape字符长度大于1。|400|无|
|SqlNoCharAfterEscapeChar|SQL LIKE语句中Escape字符后面没有被Escape字符。|400|无|
|SqlInvalidLimitValue|SQL Limit语句后的数字小于1。|400|无|
|SqlExceedsMaxWildCardCount|SQL LIKE语句中通配符\*或者%的个数超过最大值。|400|无|
|SqlExceedsMaxConditionCount|SQL Where语句中条件表达式个数超过最大值。|400|无|
|SqlExceedsMaxConditionDepth|SQL Where语句中条件树的深度超过最大值。|400|无|
|SqlOneColumnCastToDifferentTypes|SQL语句中同一列被Cast成不同类型。|400|无|
|SqlOperationAppliedToDifferentTypes|SQL条件语句中一个操作符应用在两个不同类型的对象。例如\_col1 \> 3如果其中col1是字符串时就会报这个错误。|400|无|
|SqlInvalidColumnName|SQL语句中使用列名且该列名并不在CSV文件Header中出现。|400|无|
|SqlNotSupportedTimestampFormat|SQL CAST语句中指定了时间戳格式，且该格式不支持。|400|无|
|SqlNotMatchTimestampFormat|SQL CAST语句中指定了时间戳格式，但该格式和提供的时间戳字符串不匹配。|400|无|
|SqlInvalidTimestampValue|SQL CAST语句中未指定时间戳格式，且提供的时间戳字符串无法cast成时间戳类型。|400|无|
|SqlInvalidLikeOperand|SQL LIKE语句中左边不是列名或者列Index或者左边的列不是字符串类型，或者右边字符串。|400|无|
|SqlInvalidMixOfAggregationAndColumn|SQL Select语句中同时包含聚合操作以及非聚合操作的列名、列Index。|400|无|
|SqlExceedsMaxAggregationCount|SQL Select语句中包含的聚合操作超过最大值。|400|无|
|SqlInvalidMixOfStarAndColumn|SQL Select语句中同时包含\*以及列名、列Index。|400|无|
|SqlInvalidKeepAllColumnsWithAggregation|SQL包含聚合操作，同时KeepAllColumns参数为True。|400|无|
|SqlInvalidKeepAllColumnsWithDuplicateColumn|SQL包含重复的列名、列索引，同时KeepAllColumns参数为True。|400|无|
|SqlInvalidSqlAfterAnalysis|SQL过分复杂导致分析后无法支持。|400|无|
|InvalidArithmeticOperand|SQL中算术操作应用在非数字类型的常量或者列。|400|无|
|SqlInvalidAndOperand|SQL中AND操作连接的两个表达式不是bool类型。|400|无|
|SqlInvalidOrOperand|SQL中OR操作连接的两个表达式不是bool类型。|400|无|
|SqlInvalidNotOperand|SQL中NOT操作应用在非bool类型的表达式。|400|无|
|SqlInvalidIsNullOperand|SQL中IS NULL应用在常量上。|400|无|
|SqlComparerOperandTypeMismatch|SQL中比较操作应用在两个不同类型的对象。|400|无|
|SqlInvalidConcatOperand|SQL中字符串连接操作（\|\|）连接两个常量。|400|无|
|SqlUnsupportedSql|SQL过分复杂从而生成的SQL计划超过最大值。|400|无|
|HeaderInfoExceedsMaxSize|SQL指定了输出Header信息，且Header信息超过最大允许的长度。|400|无|
|OutputExceedsMaxSize|SQL的输出有放大导致输出结果中单行超过最大长度。|400|无|
|InvalidCsvLine|CSV行是非法时（包括长度超过最大允许的长度），或者被跳过的行数超过MaxSkippedRecordsAllowed。|206或者400|400或者无|
|NegativeRowIndex|SQL中作为数组Index的值为负数。|400|无|
|ExceedsMaxNestedColumnDepth|SQL中的JSON属性嵌套层数超过最大值。|400|无|
|NestedColumnNotSupportInCsv|嵌套的属性（含有数组\[\]或者.）不支持在CSV的SQL中。|400|无|
|TableRootNodeOnlySupportInJson|只有JSON文件支持在From ossobject后指定根节点路径。|400|无|
|JsonNodeExceedsMaxSize|JSON文件的SQL的根节点大小超过允许最大值。|400或者206|无或者400|
|InvalidJsonData|非法的JSON数据（格式不正确）。|400或者206|无或者400|
|ExceedsMaxJsonArraySize|JSON文件的SQL的根节点中的某个数组的元素个数超过最大值。|400或者206|无或者400|
|WildCardNotAllowed|JSON文件的SQL中wild card \*不能出现select或者where的JSON属性中。例如`select s.a.b[*] from ossobject where a.c[*] > 0`是不支持的。|400|无|
|JsonNodeExceedsMaxDepth|JSON文件的SQL根节点深度超过最大值。|400或者206|无或者400|

