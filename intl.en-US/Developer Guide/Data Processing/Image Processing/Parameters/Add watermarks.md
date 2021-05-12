# Add watermarks

You can add text or image watermarks to an image stored in Object Storage Service \(OSS\) by using Image Processing \(IMG\) parameters. This topic describes and provides examples on how to use parameters to add watermarks to an image.

## Usage notes

-   Only images stored in the current bucket can be used as watermarks. To use online or local images as watermarks, you must first upload the images to the current bucket.
-   Only JPG, PNG, BMP, WebP, and TIFF images can be used as watermarks.
-   You can add up to three different image watermarks to a single image, and the positions of each image watermark cannot be completely overlapped.
-   Traditional Chinese characters cannot be used as text watermarks.

## Parameters

Operation: watermark

The following table lists the parameters that you can configure when you add watermarks to images.

-   Basic parameters

    |Parameter|Required|Description|Valid value|
    |---------|--------|-----------|-----------|
    |t|No|The opacity of the watermark.|\[0,100\] Default value: 100. A value of 100 indicates that the watermark is opaque. |
    |g|No|The position of the watermark on the image.|    -   nw: upper left
    -   north: upper middle
    -   ne: upper right
    -   west: middle left
    -   center: center
    -   east: middle right
    -   sw: lower left
    -   south: lower middle
    -   se: lower right
For the precise position that each value indicates, see the figure in the following section.|
    |x|No|The horizontal margin is the horizontal distance between the watermark and the image edge. This parameter takes effect only when the watermark is on the upper left, middle left, lower left, upper right, middle right, or lower right of the image.|\[0,4096\] Default value: 10

Unit: px |
    |y|No|The vertical margin is the vertical distance between the watermark and the image edge. This parameter takes effect only when the watermark is on the upper left, upper middle, upper right, lower left, lower middle, or lower right of the image.|\[0,4096\] Default value: 10

Unit: px |
    |voffset|No|The vertical offset from the middle line. When the watermark is in the middle left, center, or middle right of the image, you can designate the vertical offset of the watermark along the middle line.|\[-1000,1000\] Default value: 0.

Unit: px |

    You can use the horizontal margin, vertical margin, and vertical offset from the middle line to adjust the position of a watermark on an image. You can also use these parameters to adjust the watermark layout when the image has multiple watermarks.

    The following figure shows the positions of watermarks based on coordinates.

    ![origin](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4956348951/p2648.png)

-   Image watermark parameters

    |Parameter|Required|Description|Valid value|
    |---------|--------|-----------|-----------|
    |image|Yes|The complete name of the image object that you want to use as a watermark. The object name must be Base64-encoded. For more information, see [Encode watermarks](#watermark). For example, if you want to use an image object named panda.png in the image directory of the current bucket as a watermark, the object name to encode is image/panda.png and the encoded object name is `aW1hZ2UvcGFuZGEucG5n`.

**Note:** Only objects stored in the current bucket can be used as watermarks.

|Base64-encoded strings.|

-   Parameters for watermark image preprocessing

    You can preprocess a watermark image by calling the [Resize images](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Resize images.md), [Custom crop](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Custom crop.md), [Indexed cut](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Indexed cut.md), [Rounded rectangle](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Rounded rectangle.md), and [Rotate](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Automatic rotation.md) operations. In addition, the P parameter is supported when you resize a watermark image.

    |Parameter|Description|Valid value|
    |---------|-----------|-----------|
    |P|The size of the watermark image relative to the source image. The value of this parameter specifies the size of the watermark as a percentage of the source image. For example, if you set this parameter to 10 for a source image of 100 × 100 pixels, the size of the watermark image is 10 × 10 pixels. If the source image is 200 × 200 pixels, the size of the watermark image is 20 × 20 pixels.|\[1,100\]|

-   Text watermark parameters

    |Parameter|Required|Description|Valid value|
    |---------|--------|-----------|-----------|
    |text|Yes|The content of the text watermark. The text content must be Base64-encoded. For more information, see [Encode watermarks](#watermark).|Base64-encoded strings. The string can be up to 64 characters in length.|
    |type|No|The font of the text watermark. The font name must be Base64-encoded.|For more information about the supported fonts and the encoded strings for the fonts, see [Font types and encoded strings](#table_ipy_1vu_isp). Default value: wqy-zenhei \(encoded value: d3F5LXplbmhlaQ\) |
    |color|No|The color of the text watermark. The valid values for this parameter are RGB color values.|RGB color values. For example, 000000 indicates black, and FFFFFF indicates white. Default value: 000000. A value of 000000 indicates that the color of the text is black. |
    |size|No|The size of the text watermark.|\(0,1000\] Default value: 40.

Unit: px |
    |shadow|No|The opacity of the shadow for the text watermark.|\[0,100\]Default value: 0. A value of 0 indicates no shadows are added to the text. |
    |rotate|No|The degree by which the text is rotated clockwise.|\[0,360\]Default value: 0. A value of 0 indicates that the text is not rotated. |
    |fill|No|Specifies whether to tile the source image with the text watermarks.|0 and 1. Default value: 0.     -   1: The source image is tiled with the text watermarks.
    -   0: The source image is not tiled with the text watermarks. |

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
    |order|No|The order of the text watermark and image watermark.|0 and 1. Default value: 0.     -   0: The image watermark is on the top of the text watermark.
    -   1: The text watermark is on the top of the image watermark. |
    |align|No|The alignment of the text watermark and image watermark.|0, 1, and 2. Default value: 2.     -   0: The text watermark and the image watermark are aligned based on top alignment.
    -   1: The text watermark and the image watermark are aligned based on center alignment.
    -   2: The text watermark and the image watermark are aligned based on bottom alignment. |
    |interval|No|The spacing between the text watermark and image watermark.|\[0,1000\]Default value: 0.

Unit: px |


## Encode watermarks

The content, color, and font of a text watermark and the image name of an image watermark must be a URL-safe string that is Base64-encoded. Perform the following steps to encode watermarks:

1.  Encode the content by using Base64.
2.  Replace part of the encoded string.
    -   Replace the plus signs \(+\) in the encoded string with hyphens \(-\).
    -   Replace the forward slashes \(/\) in the encoded string with underscores \(\_\).
    -   Omit the equal signs \(=\) that are at the end of the encoded string.

We recommend that you use [URL-safe Baes64 encoding tools](https://simplycalc.com/base64url-encode.php) to encode the content, color, and font of a text watermark and the image name of an image watermark.

**Note:** Encoded strings can be used only as parameters in specific watermark operations. Do not use encoded strings in signature strings.

## Example 1: Add a text watermark to an image.

In the following examples, the source image named example.jpg is stored in a bucket named image-demo that is located in the China \(Zhangjiakou\) region. The URL used to access the image is [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg).

![Source image](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8812863061/p139183.png)

The following examples show how to add a text watermark to example.jpg:

-   Add the string "Hello World" to the image as a text watermark

    Base64-encode the string "Hello World" and convert the encoded result to a URL-safe string. For more information, see [Encode watermarks](#watermark). The encoded URL-safe string is `SGVsbG8gV29ybGQ`. Therefore, you can use the following URL to add the text watermark to example.jpg: [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/watermark,text\_SGVsbG8gV29ybGQ](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/watermark,text_SGVsbG8gV29ybGQ).

    ![exampleobject](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7680180261/p262542.jpg)

-   Configure multiple IMG parameters when you add a text watermark to the image

    In this example, IMG parameters are configured to perform the following operations on the source image and text watermark "Hello World" that you want to add to the source image:

    -   Resize the source image example.jpg to 300 × 300 pixels. IMG parameter: `resize,w_300,h_300`
    -   Set the text font of the text watermark to WenQuanYi Zen Hei. IMG parameter: `type_d3F5LXplbmhlaQ` \(d3F5LXplbmhlaQ is the Base64-encoded value for WenQuanYi Zen Hei.\)
    -   Add the string "Hello World" to the source image as a text watermark. IMG parameter: `text_SGVsbG8gV29ybGQ`
    -   Set the color of the text watermark to white and the size of the text to 30 pixels. IMG parameter: `color_FFFFFF,size_30`
    -   Set the opacity of the shadow of the text watermark to 50%. IMG parameter: `shadow_50`
    -   Set the position of the text watermark to lower right, the horizontal margin to 10 pixels, and the vertical offset from the middle line to 10 pixels. IMG parameter: `g_se,x_10,y_10`
    You can use the following URL to configure multiple IMG parameters when you add the text watermark to the image: [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_300,h\_300/watermark,type\_d3F5LXplbmhlaQ,size\_30,text\_SGVsbG8gV29ybGQ,color\_FFFFFF,shadow\_50,t\_100,g\_se,x\_10,y\_10](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_300,h_300/watermark,type_d3F5LXplbmhlaQ,size_30,text_SGVsbG8gV29ybGQ,color_FFFFFF,shadow_50,t_100,g_se,x_10,y_10)

    ![Image processing 5](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0077333061/p135882.png)


## Example 2: Add an image watermark to the source image.

The following examples show how to add image watermarks to the source image named example.jpg:

-   Add an image named panda.png to the source image as an image watermark

    Base64-encode the image name panda.png and convert the encoded result to a URL-safe string. The encoded URL-safe string is `cGFuZGEucG5n`. Therefore, you can use the following URL to add panda.png to example.jpg as an image watermark: [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/watermark,image\_cGFuZGEucG5n](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/watermark,image_cGFuZGEucG5n).

    ![1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7680180261/p262558.jpg)

-   Configure multiple IMG parameters when you add an image watermark to the source image

    In this example, IMG parameters are configured to perform the following operations on the source image and the image named panda.png that you want to add to the source image as an image watermark:

    -   Resize the source image example.jpg to 300 × 300 pixels. IMG parameter `resize,w_300,h_300`
    -   Set the quality of the source image example.jpg to 90%. IMG parameter: `quality,q_90`
    -   Add the image watermark panda.png. IMG parameter: `watermark,image_cGFuZGEucG5n` \(cGFuZGEucG5n is the Base64-encoded value for panda.png.\)
    -   Set the opacity of the image watermark to 90%: `t_90`
    -   Set the position of the image watermark to lower right, the horizontal margin to 10 pixels, and the vertical offset from the middle line to 10 pixels. IMG parameter: `g_se,x_10,y_10`
    You can use the following URL to configure multiple IMG parameters when you add the image watermark to the source image: [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_300,h\_300/quality,q\_90/watermark,image\_cGFuZGEucG5n,t\_90,g\_se,x\_10,y\_10](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_300,h_300/quality,q_90/watermark,image_cGFuZGEucG5n,t_90,g_se,x_10,y_10)

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4956348951/p2654.jpg)

-   Preprocess the image watermark and add it to the source image

    In this example, IMG parameters are configured to perform the following operations on the source image and the image named panda.png that you want to add to the source image as an image watermark:

    -   Resize the width of the source image example.jpg to 300 pixels. IMG parameter: `resize,w_300`
    -   Resize the image watermark panda.png to 30% of the original size. IMG parameter: `image_cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA` \(`cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA` is the Base64-encoded value for `panda.png?x-oss-process=image/resize,P_30`.\)
    -   Set the opacity of the image watermark to 90%, the position of the image watermark to lower right, the horizontal margin to 10 pixels, and the vertical offset from the middle line to 10 pixels. IMG parameter: `t_90,g_se,x_10,y_10`
    You can use the following URL to configure multiple IMG parameters when you add the image watermark to the source image: [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_300/watermark,image\_cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA,t\_90,g\_se,x\_10,y\_10](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_300/watermark,image_cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA,t_90,g_se,x_10,y_10)

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4956348951/p2651.jpg)


## Example 3: Add text and image watermarks to the source image.

The following examples show how to add text and image watermarks to the source image named example.jpg:

-   Add an image named panda.png to example.jpg as an image watermark and a string "Hello World" to example.jpg as a text watermark

    Based on the encoded results of the image name panda.png and the string "Hello World", you can use the following URL to add panda.png and "Hello World" to example.jpg as an image watermark and a text watermark: [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/watermark,image\_cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA,text\_SGVsbG8gV29ybGQ](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/watermark,image_cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA,text_SGVsbG8gV29ybGQ).

    ![3](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8680180261/p262821.jpg)

-   Add multiple image and text watermarks to example.jpg

    In this example, two text watermarks \(Watermark 1 and Watermark 2\) and three image watermarks \(Jellyfish.jpg, Koala.jpg, and Tulips.jpg\) are added to the source image example.jpg. When you add multiple watermarks to the source image, use forward slashes \(/\) to separate the operations performed to configure each watermark.

    -   Add the image Jellyfish.jpg to the source image as an image watermark. Resize the image watermark to 20% of the original size. Set the position of the image watermark to upper left, the horizontal margin to 10 pixels, and the vertical offset from the middle line to 10 pixels. You can configure the following IMG parameters to perform the preceding operations: `watermark,image_cGljcy9KZWxseWZpc2guanBnP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMjA,g_nw,x_10,y_10`. `cGljcy9KZWxseWZpc2guanBnP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMjA` is the Base64-encoded value of the image name Jellyfish.jpg.
    -   Add the image Koala.jpg to the source image as an image watermark. Resize the image watermark to 20% of the original size. Set the position of the image watermark to lower right, the horizontal margin to 10 pixels, and the vertical offset from the middle line to 10 pixels. You can configure the following IMG parameters to perform the preceding operations: `watermark,image_cGljcy9Lb2FsYS5qcGc_eC1vc3MtcHJvY2Vzcz1pbWFnZS9yZXNpemUsUF8yMA,g_se,x_10,y_10`. `cGljcy9Lb2FsYS5qcGc_eC1vc3MtcHJvY2Vzcz1pbWFnZS9yZXNpemUsUF8yMA` is the Base64-encoded value of the image name Koala.jpg.
    -   Add the image Tulips.jpg to the source image as an image watermark. Resize the image watermark to 20% of the original size. Set the position of the image watermark to middle left, the horizontal margin to 10 pixels, and the vertical offset from the middle line to 10 pixels. You can configure the following IMG parameters to perform the preceding operations: `watermark,image_cGljcy9UdWxpcHMuanBnP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMjA,g_west,x_10,y_10`. `cGljcy9UdWxpcHMuanBnP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMjA` is the Base64-encoded value of the image name Tulips.jpg.
    -   Add the text watermark "Watermark 1" to the source image. Set the size of the text to 20 pixels. Set the position of the text watermark to upper right, the horizontal margin to 10 pixels, and the vertical offset from the middle line to 200 pixels. You can configure the following IMG parameters to perform the preceding operations: `watermark,text_V2F0ZXJtYXJrIDE,g_ne,size_20,x_10,y_200`. `V2F0ZXJtYXJrIDE` is the Base64-encoded value of the string "Watermark 1".
    -   Add the text watermark "Watermark 2" to the source image. Set the size of the text to 20 pixels and the color of the text to dark blue. Set the position of the text watermark to lower left, the horizontal margin to 100 pixels, and the vertical offset from the middle line to 50 pixels. You can configure the following IMG parameters to perform the preceding operations: `watermark,text_V2F0ZXJtYXJrIDI,color_0000b7,size_20,g_sw,x_100,y_50`. `V2F0ZXJtYXJrIDI` is the Base64-encoded value of the string "Watermark 2".
    Based on the preceding IMG parameters, you can use the following URL to add two text watermarks \(Watermark 1 and Watermark 2\) and three image watermarks \(Jellyfish.jpg, Koala.jpg, and Tulips.jpg\) to the source image example.jpg: [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/watermark,image\_cGljcy9KZWxseWZpc2guanBnP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMjA,g\_nw,x\_10,y\_10/watermark,image\_cGljcy9Lb2FsYS5qcGc\_eC1vc3MtcHJvY2Vzcz1pbWFnZS9yZXNpemUsUF8yMA,g\_se,x\_10,y\_10/watermark,image\_cGljcy9UdWxpcHMuanBnP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMjA,g\_west,x\_10,y\_10/watermark,text\_V2F0ZXJtYXJrIDE,size\_20,g\_ne,x\_10,y\_200/watermark,text\_V2F0ZXJtYXJrIDI,color\_0000b7,size\_20,g\_sw,x\_100,y\_50](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=image/watermark,image_cGljcy9KZWxseWZpc2guanBnP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMjA,g_nw,x_10,y_10/watermark,image_cGljcy9Lb2FsYS5qcGc_eC1vc3MtcHJvY2Vzcz1pbWFnZS9yZXNpemUsUF8yMA,g_se,x_10,y_10/watermark,image_cGljcy9UdWxpcHMuanBnP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMjA,g_west,x_10,y_10/watermark,text_V2F0ZXJtYXJrIDE,size_20,x_10,y_200/watermark,text_V2F0ZXJtYXJrIDI,color_0000b7,size_20,g_sw,x_100,y_50)

    ![3](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8680180261/p262578.jpg)


## FAQ

How do I use online or local images as watermark images?

When IMG is used to add image watermarks to a source image, make sure that the watermark images and the source image are stored in the same bucket. To use online or local images to a source image as watermarks, you must first upload the images to the bucket in which the source image is stored.

