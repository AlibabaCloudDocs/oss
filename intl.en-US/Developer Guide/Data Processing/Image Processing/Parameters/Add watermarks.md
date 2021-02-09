# Add watermarks

You can add text or image watermarks to an image stored in OSS by adding watermark parameters. This topic describes the parameters used to add watermarks to an image and provides examples on how to add watermarks to an image.

## Parameters

Operation name: watermark

The following table describes the parameters you can configure.

-   Basic parameters

    |Parameter|Required|Description|Valid value|
    |---------|--------|-----------|-----------|
    |t|No|The opacity of the text or image watermark.

|\[0,100\] Default value: 100. A value of 100 indicates that the watermark is opaque. |
    |g|No|The position of the watermark on the image.

|    -   nw: upper left
    -   north: upper middle
    -   ne: upper right
    -   west: middle left
    -   center: center
    -   east: middle right
    -   sw: lower left
    -   south: lower middle
    -   se: lower right
The following figure shows the positions of watermarks based on coordinates.|
    |x|No|The horizontal margin, which is the horizontal distance between the watermark and the image edge. This parameter takes effect only when the watermark is on the upper left, middle left, lower left, upper right, middle right, or lower right of the image.

|\[0,4096\] Default value: 10

Unit: px |
    |y|No|The vertical margin, which is the vertical distance between the watermark and the image edge. This parameter takes effect only when the watermark is on the upper left, upper middle, upper right, lower left, lower middle, or lower right of the image.|\[0,4096\] Default value: 10

Unit: px |
    |voffset|No|The vertical offset from the middle line. When the watermark is in the middle left, center, or middle right of the image, you can designate the vertical offset of the watermark along the middle line.|\[-1000,1000\] Default value: 0

Unit: px |

    You can use the horizontal margin, vertical margin, and vertical offset from the middle line to adjust the position of a watermark on an image. You can also use these parameters to adjust the watermark layout when the image has multiple watermarks.

    The following figure shows the positions of watermarks based on coordinates.

    ![origin](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4956348951/p2648.png)

-   Image watermark parameters

    |Parameter|Required|Description|Valid value|
    |---------|--------|-----------|-----------|
    |image|Yes|The complete name of the image watermark object. The object name must be encoded in Base64. For more information, see [Watermark encoding](#section_dfu_kwj_v90).For example, if the panda.png watermark object is in the image folder of a bucket, the object name to encode is image/panda.png. The encoded object name is `aW1hZ2UvcGFuZGEucG5n`.

**Note:** You can use images only in the current bucket as watermark images.

|The Base64-encoded string.|

-   Parameters for watermark image preprocessing

    You can preprocess a watermark image by calling the [Resize images](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Resize images.md), [Custom crop](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Custom crop.md), [Indexed cut](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Indexed cut.md), [Rounded rectangle](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Rounded rectangle.md), and [Rotate](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Automatic rotation.md) operations. In addition, the P parameter is supported when you resize a watermark image.

    |Parameter|Description|Valid value|
    |---------|-----------|-----------|
    |P|The proportion of the resized watermark image to the source image. The value of this parameter specifies the scale percentage of the watermark from the source image. For example, if you set the parameter value to 10 for a source image of 100 × 100 pixels, the size of the watermark image is 10 × 10 pixels. If the source image is 200 × 200 pixels, the size of the watermark image is 20 × 20 pixels.|\[1,100\]|

-   Text watermark parameters

    |Parameter|Required|Description|Valid value|
    |---------|--------|-----------|-----------|
    |text|Yes|The content of the text watermark. The text content must be encoded in Base64. For more information, see [Watermark encoding](#section_dfu_kwj_v90).|The Base64-encoded string. The string can be up to 64 characters in length.|
    |type|No|The font of the text watermark. The font name must be Base64 encoded.|For more information about the supported fonts and the encoded strings for the fonts, see [Font types and encoded strings](#table_ipy_1vu_isp).Default value: wqy-zenhei \(encoded value: d3F5LXplbmhlaQ\) |
    |color|No|The color of the text watermark. The valid values for this parameter are RGB color values.|RGB color values. For example, 000000 indicates black, and FFFFFF indicates white. Default value: 000000. A value of 000000 indicates that the color of the text is black. |
    |size|No|The size of the text watermark.|\(0,1000\] Default value: 40

Unit: px |
    |shadow|No|The opacity of the shadow for the text watermark.|\[0,100\]Default value: 0. A value of 0 indicates no shadows are added to the text. |
    |rotate|No|The degree by which the text is rotated clockwise.|\[0,360\]Default value: 0. A value of 0 indicates that the text is not rotated. |
    |fill|No|Specifies whether to tile the source image with the text watermarks.|0 and 1     -   1: The source image is tiled with the text watermarks.
    -   0: The default value, which indicates that source image is not tiled with the text watermarks. |

    The following table describes the valid values of the type parameter and the encoded strings of these values.

    |Parameter value|Font name|Encoded value|
    |---------------|---------|-------------|
    |wqy-zenhei|WenQuanYi Zen Hei|d3F5LXplbmhlaQ|
    |wqy-microhei|WenQuanYi Micro Hei|d3F5LW1pY3JvaGVp|
    |fangzhengshusong|Fangzheng Shusong|ZmFuZ3poZW5nc2h1c29uZw|
    |fangzhengkaiti|Fangzheng Kaiti|ZmFuZ3poZW5na2FpdGk|
    |fangzhengheiti|Fangzheng Heiti|ZmFuZ3poZW5naGVpdGk|
    |fangzhengfangsong|Fangzheng Fangsong|ZmFuZ3poZW5nZmFuZ3Nvbmc|
    |droidsansfallback|DroidSansFallback|ZHJvaWRzYW5zZmFsbGJhY2s|

-   Text-and-image watermark parameters

    |Parameter|Required|Description|Valid value|
    |---------|--------|-----------|-----------|
    |order|No|The order of the text watermark and image watermark.|0 and 1     -   0: The image watermark is before the text watermark. This is the default value.
    -   1: The text watermark is before the image watermark. |
    |align|No|The alignment of the text watermark and image watermark.|0, 1, and 2     -   0: The text watermark and the image watermark are aligned based on top alignment.
    -   1: The text watermark and the image watermark are aligned based on center alignment.
    -   2: The text watermark and the image watermark are aligned based on bottom alignment. This is the default value. |
    |interval|No|The spacing between the text watermark and image watermark.|\[0,1000\]Default value: 0

Unit: px |


## Watermark encoding

When you add watermarks to an image, the content, color, and font of a text watermark and the image name of an image watermark must be URL-encoded in Base64. Encoding method:

1.  Encode the content by using Base64.
2.  Replace part of the encoded string.
    -   Replace the plus signs \(+\) in the result with hyphens \(-\).
    -   Replace the forward slashs \(/\) in the result with underscores \(\_\).
    -   Omit the equal signs \(=\) that are at the end of strings in the result.

We recommend that you use [URL-safe Baes64 encoding tools](https://simplycalc.com/base64url-encode.php) to encode the content, color, and font of a text watermark and the image name of an image watermark.

**Note:** The encoded strings for watermarks can be used only as the parameters configured to add watermarks. Do not include the encoded strings for watermarks in the signature strings.

## Usage notes

-   You can use watermark images only in the current bucket. Online or local images must be uploaded to the current bucket before these images can be used for watermarking.
-   Only JPG, PNG, BMP, GIF, WebP and TIFF watermark images are supported.
-   Traditional Chinese is not supported for text watermarks.
-   Before you add multiple watermarks to an image, take note of the following items:
    -   You can add up to three watermarks to an image.
    -   A watermark image can be repeatedly used. However, the watermark must be placed at different positions on the source image each time.
    -   Positions of each watermark cannot completely overlap.

## Examples

An image in the bucket named image-demo-oss-zhangjiakou in the China \(Zhangjiakou\) region is used in this example. The following URL is used to access the image over the Internet:

[https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg)

![Source image](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8812863061/p139183.png)

-   Add an image watermark

    Take the following steps to configure parameters:

    -   Resize the source image example.jpg to 300 × 300 pixels: `resize,w_300,h_300`
    -   Set the quality of the source image to 90%: `quality,q_90`
    -   Add the image watermark panda.png: `watermark,image_cGFuZGEucG5n` \(cGFuZGEucG5n is an encoded value for panda.png.\)
    -   Set the opacity of the image watermark to 90%: `t_90`
    -   Set the position of the image watermark to lower right, the horizontal margin to 10 pixels, and the vertical offset from the middle line to 10 pixels: `g_se,x_10,y_10`
    The following URL is used to process the image: [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_300,h\_300/quality,q\_90/watermark,image\_cGFuZGEucG5n,t\_90,g\_se,x\_10,y\_10](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_300,h_300/quality,q_90/watermark,image_cGFuZGEucG5n,t_90,g_se,x_10,y_10)

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4956348951/p2654.jpg)

-   Preprocess the image watermark and add it to the source image

    Take the following steps to configure parameters:

    -   Resize the source image example.jpg to 300 pixels wide: `resize,w_300`
    -   Resize the image watermark panda.png to 30% of the original size: `image_cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA` \(`cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA` is a Base64 encoded value for `panda.png? x-oss-process=image/resize,P_30`.\)
    -   Set the opacity of the image watermark to 90%, the position of the image watermark to lower right, the horizontal margin to 10 pixels, and the vertical offset from the middle line to 10 pixels: `t_90,g_se,x_10,y_10`
    The following URL is used to process the image: [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_300/watermark,image\_cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA,t\_90,g\_se,x\_10,y\_10](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_300/watermark,image_cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA,t_90,g_se,x_10,y_10)

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4956348951/p2651.jpg)

-   Add a text watermark

    Take the following steps to configure parameters:

    -   Resize the source image example.jpg to 300 × 300 pixels: `resize,w_300,h_300`
    -   Set the text font of the text watermark to WenQuanYi Zen Hei: `type_d3F5LXplbmhlaQ` \(d3F5LXplbmhlaQ is a Base64 encoded value for WenQuanYi Zen Hei.\)
    -   Use "Hello World" as the text content: `text_SGVsbG8gV29ybGQ`
    -   Set the color of the text watermark to white and the size of the text to 30 pixels: `color_FFFFFF,size_30`
    -   Set the opacity of the shadow of the text watermark to 50%: `shadow_50`
    -   Set the position of the text watermark to lower right, the horizontal margin to 10 pixels, and the vertical offset from the middle line to 10 pixels: `g_se,x_10,y_10`
    The following URL is used to process the image: [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_300,h\_300/watermark,type\_d3F5LXplbmhlaQ,size\_30,text\_SGVsbG8gV29ybGQ,color\_FFFFFF,shadow\_50,t\_100,g\_se,x\_10,y\_10](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_300,h_300/watermark,type_d3F5LXplbmhlaQ,size_30,text_SGVsbG8gV29ybGQ,color_FFFFFF,shadow_50,t_100,g_se,x_10,y_10)

    ![Image processing 5](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0077333061/p135882.png)

-   Add a text-and-image watermark

    Take the following steps to configure parameters:

    -   Resize the source image example.jpg to 300 × 300 pixels: `resize,w_300,h_300`
    -   Set the quality of the source image to 90%: `quality,q_90`
    -   Resize the image watermark panda.png to 30% of the original size: `image_cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA`
    -   Use "Hello World" as the text content: `text_SGVsbG8gV29ybGQ`
    -   Set the color of the text watermark to white, the opacity of the shadow to 50%, the position of the text watermark to lower right, the horizontal margin to 10 pixels, and the vertical offset from the middle line to 10 pixels: `g_se,x_10,y_10,color_FFFFFF,shadow_50`
    -   Specify that the image watermark is before the text watermark. Set the spacing between the text watermark and image watermark to 10 pixels: `order_0,interval_10`
    -   Set the text watermark and the image watermark to be aligned based on bottom alignment: `align_2`
    -   Set the opacity of the text-and-image watermark to 100%: `t_100`
    The following URL is used to process the image: [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_300,h\_300/quality,q\_90/watermark,image\_cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA,text\_SGVsbG8gV29ybGQ,order\_0,interval\_10,align\_2,t\_100,g\_se,x\_10,y\_10,color\_FFFFFF,shadow\_50](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_300,h_300/quality,q_90/watermark,image_cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA,text_SGVsbG8gV29ybGQ,order_0,interval_10,align_2,t_100,g_se,x_10,y_10,color_FFFFFF,shadow_50)

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3323863061/p2659.jpg)

-   Add multiple watermarks to the source image

    If you want to add multiple watermarks to the source image, use forward slashes \(/\) to separate the operation performed to configure each watermark. Add multiple watermarks to an image, including four different text watermarks and five image watermarks. Three different image watermarks are used.

    The following URL is used to process the image: [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/watermark,image\_cGljcy9KZWxseWZpc2guanBnP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMjA,g\_nw,x\_10,y\_10/watermark,image\_cGljcy9Lb2FsYS5qcGc\_eC1vc3MtcHJvY2Vzcz1pbWFnZS9yZXNpemUsUF8yMA,g\_se,x\_10,y\_10/watermark,image\_cGljcy9UdWxpcHMuanBnP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMjA,g\_west,x\_10,y\_10/watermark,image\_cGljcy9Lb2FsYS5qcGc\_eC1vc3MtcHJvY2Vzcz1pbWFnZS9yZXNpemUsUF8yMA,g\_se,x\_100,y\_100/watermark,image\_cGljcy9UdWxpcHMuanBnP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMjA,g\_west,x\_80,y\_100/watermark,text\_V2F0ZXJtYXJrIDE,size\_20,x\_10,y\_200/watermark,text\_V2F0ZXJtYXJrIDI,color\_0000b7,size\_20,g\_sw,x\_100,y\_50/watermark,text\_V2F0ZXJtYXJrIDM,color\_b3b324,size\_20,g\_ne,x\_10,y\_10/watermark,text\_V2F0ZXJtYXJrIDQ,color\_b3b324,size\_20,g\_east,x\_10,y\_10](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/watermark,image_cGljcy9KZWxseWZpc2guanBnP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMjA,g_nw,x_10,y_10/watermark,image_cGljcy9Lb2FsYS5qcGc_eC1vc3MtcHJvY2Vzcz1pbWFnZS9yZXNpemUsUF8yMA,g_se,x_10,y_10/watermark,image_cGljcy9UdWxpcHMuanBnP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMjA,g_west,x_10,y_10/watermark,image_cGljcy9Lb2FsYS5qcGc_eC1vc3MtcHJvY2Vzcz1pbWFnZS9yZXNpemUsUF8yMA,g_se,x_100,y_100/watermark,image_cGljcy9UdWxpcHMuanBnP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMjA,g_west,x_80,y_100/watermark,text_V2F0ZXJtYXJrIDE,size_20,x_10,y_200/watermark,text_V2F0ZXJtYXJrIDI,color_0000b7,size_20,g_sw,x_100,y_50/watermark,text_V2F0ZXJtYXJrIDM,color_b3b324,size_20,g_ne,x_10,y_10/watermark,text_V2F0ZXJtYXJrIDQ,color_b3b324,size_20,g_east,x_10,y_10)

    ![Watermark 1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3323863061/p162254.jpg)


## FAQ

How do I use online or local images as watermark images?

When OSS Image Processing \(IMG\) is used to add image watermarks to a source image, ensure that the watermark images and the source image are from the same bucket. Before you use online or local images to watermark a source image, upload the images to the bucket that contains the source image.

