# 访问CDN加速域名报错“You are forbidden to list buckets”

本文介绍开启CDN回源私有Bucket的情况下，访问CDN加速域名出现报错的原因及解决方法。

问题现象：开启CDN回源私有Bucket的情况下，访问CDN加速域名报错`You are forbidden to list buckets`。

![CDN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2222864161/p245112.png)

问题原因：在开启CDN回源至私有Bucket的情况下，访问CDN加速域名相当于GetBucket\(ListObjects\)请求，默认会被CDN拒绝。

解决方法：在CDN侧对根域名URL重写为指向根域名URL下的某个文件，例如将CDN加速域名`www.cdndomain.com`重写为`www.cdndomain.com/index.html`。有关重写规则的具体操作，请参见[配置重写](/intl.zh-CN/域名管理/缓存配置/配置重写.md)。

