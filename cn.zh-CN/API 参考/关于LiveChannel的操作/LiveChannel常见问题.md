# LiveChannel常见问题

本文主要介绍使用LiveChannel过程中遇到的常见问题及解决方案。

## OSS livechannel推流过程

下图为OSS livechannel推流过程图解，了解推流过程后，有助于排查livechannel推流过程中遇到的问题。

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0315459951/p35194.png)

更多详情请参见[PutLiveChannel](/cn.zh-CN/API 参考/关于LiveChannel的操作/PutLiveChannel.md)、[PutLiveChannelStatus](/cn.zh-CN/API 参考/关于LiveChannel的操作/PutLiveChannelStatus.md)。

## 案例：录制M3u8缺失

问题分析：默认录制成品的M3u8只有最后3片，遵循了HLS协议的默认规则。

解决方案：使用PostVodPlaylist接口将指定时间范围内的ts文件汇聚到M3u8索引。

-   `EndTime`必须大于`StartTime`，且时间跨度不能大于1天。
-   OSS会查询指定时间范围内的所有指定LiveChannel推流生成的ts文件，并将其拼装为一个播放列表。

## 案例：录制的M3u8文件失败

问题分析：录制的M3u8文件成功推流到OSS才算录制成功。

解决方案：建议到客户端抓包查看是否有publish succees的标志，此标志表示与OSS音频包交互成功。所以出现客户端有推流记录，但是没有录制视频的情况，可以尝试抓包分析。

## 案例：客户端无法推流到OSS

使用ffmpeg推流不成功：

```
ffmpeg -re -i 0_20180525105430445.aac -acodec aac -strict -2 -f flv rtmp://xxx.oss-cn-beijing.aliyuncs.com/live/test_1000?Expires=1540458859&OSSAccessKeyId=LTAlujianb****&Signature=qwh31xQsanmao6ygCFJgo****%3D&playlistName=playlist.m3u8
```

解决方案：

-   使用ffmpeg推流不成功时，建议用最原始的命令推流，不添加复杂的参数。
-   推流URL中包含and符号（&）时，请使用双引号（" "）将整个URL括起来。例如，`ffmpeg -re -i 0_20180525105430445.aac -acodec aac -strict -2 -f flv "rtmp://xxx.oss-cn-beijing.aliyuncs.com/live/test_1000?Expires=1540458859&OSSAccessKeyId=LTAlujianb****&Signature=qwh31xQsanmao6ygCFJgo****%3D&playlistName=playlist.m3u8"`。
-   尝试更换为OBS进行推流测试，否则无法确认是否因ffmpeg问题导致的推流失败。

如果推流后延时较长，可以在[PutLiveChannel](/cn.zh-CN/API 参考/关于LiveChannel的操作/PutLiveChannel.md)接口中调整FragDuration和FragCount参数。

## 案例：录制M3u8文件卡顿

当转储类型为HLS时，写入当前ts文件的音视频数据时长达到`FragDuration`指定的时长后，OSS会在收到下一个关键帧时切换到下一个ts文件。如果指定`max(2*FragDuration, 60s)`后仍未收到下一个关键帧，OSS将强制切换文件，此时可能引起播放卡顿。

## 案例：录制M3u8文件没有音频或视频

以下几种情况会出现音频或者视频没有录制的情况：

-   没有发送`AVC header`或者`AAC header`，请通过抓包查看。
-   `RTMP message`长度小于2或者`sequence header`过小。
-   音频`Message`超过缓冲区大小。
-   `codec_ ctx`是解码上下文的关键信息，如果携带的音视频数据异常，也会导致录制失败。

## 案例：ffmpeg推流到OSS录制没有音频

解决方案：

-   直接查看ffmpeg记录的日志，确定客户端是否已发送`aac_header`。
-   在客户端进行RTMP抓包，查看客户端是否已发送`aac_header`。

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0315459951/p35199.png)

