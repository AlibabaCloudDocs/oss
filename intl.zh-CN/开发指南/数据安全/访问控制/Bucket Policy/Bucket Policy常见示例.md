# Bucket Policy常见示例

Bucket Policy是阿里云OSS推出的针对Bucket的授权策略，您可以通过Bucket Policy授权其他用户访问您指定的OSS资源。例如，您可以对同账号以及跨账号下的不同RAM用户，或者匿名用户等授予访问或管理Bucket资源的不同权限，例如只读、读写权限等。

## 示例一：授予指定RAM用户对某个Bucket的读写权限

以下示例用于授权UID为`27737962156157xxxx`以及`20214760404935xxxx`的RAM用户拥有目标存储空间examplebucket的读写权限：

```
{    
    "Version": "1",
    "Statement": [{
        "Effect": "Allow",
        "Action": [
            "oss:GetObject",
            "oss:PutObject",
            "oss:GetObjectAcl",
            "oss:PutObjectAcl",
            "oss:ListObjects",
            "oss:AbortMultipartUpload",
            "oss:ListParts",
            "oss:RestoreObject",
            "oss:GetVodPlaylist",
            "oss:PostVodPlaylist",
            "oss:PublishRtmpStream",
            "oss:ListObjectVersions",
            "oss:GetObjectVersion",
            "oss:GetObjectVersionAcl",
            "oss:RestoreObjectVersion"
        ],
        "Principal": [
            "27737962156157xxxx",
            "20214760404935xxxx"
        ],
        "Resource": [
            "acs:oss:*:174649585760xxxx:examplebucket/*"
        ]
      }, {
        "Effect": "Allow",
        "Action": [
            "oss:ListObjects",
            "oss:GetObject"
        ],
        "Principal": [
            "27737962156157xxxx",
            "20214760404935xxxx"
        ],
        "Resource": [
            "acs:oss:*:174649585760xxxx:examplebucket"
        ],
        "Condition": {
            "StringLike": {
                "oss:Prefix": [
                    "*"
                ]
            }
        }
      }]      
}
```

## 示例二：授予指定用户拥有某个Bucket下指定目录的只读权限

以下示例用于授权UID为`20214760404935xxxx`的RAM用户拥有目标存储空间examplebucket下`hangzhou/2020`和`shanghai/2015`目录的只读权限。

```
{
     "Version": "1",
    "Statement": [
        {
            "Action": [
                "oss:GetObject",
                "oss:GetObjectAcl",
                "oss:GetObjectVersion",
                "oss:GetObjectVersionAcl"
            ],
            "Effect": "Allow",
            "Principal": [
                "20214760404935xxxx"
            ],
            "Resource": [
                "acs:oss:*:174649585760xxxx:examplebucket/hangzhou/2020/*",
                "acs:oss:*:174649585760xxxx:examplebucket/shanghai/2015/*"
            ]
        },
        {
            "Action": [
                "oss:ListObjects",
                "oss:ListObjectVersions"
            ],
            "Condition": {
                "StringLike": {
                    "oss:Prefix": [
                        "hangzhou/2020/*",
                        "shanghai/2015/*"
                    ]
                }
            },
            "Effect": "Allow",
            "Principal": [
                "20214760404935xxxx"
            ],
            "Resource": [
                "acs:oss:*:174649585760xxxx:examplebucket"
            ]
        }
    ],
}
```

## 示例三：授予匿名用户仅拥有列举某个Bucket下所有文件的权限

以下示例用于授予匿名用户仅拥有列举目标存储空间examplebucket下所有文件的权限：

```
{   
   "Version": "1",
    "Statement": [
        {
            "Action": [
                "oss:ListObjects",
                "oss:ListObjectVersions"
            ],
            "Effect": "Allow",            
            "Principal": [
                "*"
            ],            
            "Resource": [
                "acs:oss:*:174649585760xxxx:examplebucket"
            ]
        },
    ],    
}
```

## 示例四：拒绝指定IP地址段对某个Bucket执行任意操作

以下示例用于拒绝源IP地址不在`192.168.0.0/16`范围内的匿名用户对存储空间examplebucket执行任意操作。

```
{
    "Version": "1",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": "oss:*",
            "Principal": [
                "*"
            ],
            "Resource": [
                "acs:oss:*:174649585760xxxx:examplebucket"
            ],
            "Condition":{
                "NotIpAddress": {
                    "acs:SourceIp": ["192.168.0.0/16"]
                }
            }
        }
    ]
}
```

