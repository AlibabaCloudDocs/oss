# Resize images

You can use the resize parameters to adjust the size of images stored in OSS. This topic describes the parameters and examples to resize images.

## Parameters

Operation: `resize`

The following table lists the parameters.

-   Resize an image based on the specified height and width

    |Parameter|Required|Description|Valid value|
    |:--------|--------|:----------|:----------|
    |m|Yes|Specifies the resize type. Default value: lfit.|    -   lfit: resizes the source image proportionally as large as possible within a rectangle based on the specified width and height.
    -   mfit: resizes the source image proportionally as small as possible beyond a rectangle based on the specified width and height.
    -   fill: resizes the source image proportionally as small as possible beyond a rectangle, and then crops the source image based on the specified width and height.
    -   pad: resizes the source image based on the specified width and height, and fills the empty space.
    -   fixed: forcibly resizes the source image based on the specified width and height.
For more information, see the example below the table.|
    |w|Yes|Specifies the width of the requested thumbnail.|\[1,4096\]|
    |h|Yes|Specifies the height of the requested thumbnail.|\[1,4096\]|
    |l|Yes|Specifies the dimension of the longer side of the requested thumbnail.**Note:** Longer side refers to the side that has a greater ratio of source size to requested size. Shorter side refers to the side that has a smaller ratio of source size to requested size. For example, if a source image is resized from 400 × 200 pixels to 800 × 100 pixels, the source-to-requested ratios are 0.5 \(400/800\) and 2 \(200/100\). Therefore, 0.5 is smaller than 2, the 200-pixel side is the longer side, and the 400-pixel side the shorter side.

|\[1,4096\]|
    |s|Yes|Specifies the dimension of the shorter side of the requested thumbnail.|\[1,4096\]|
    |limit|No|Specifies whether to process the requested thumbnail when it is larger than the source image.|0 and 1. Default value: 1.    -   1: returns the source image, which indicates that the image is not resized based on the specified parameter.
    -   0: resizes based on the specified parameter. Default value: 1. Default value: 1. |
    |color|Yes \(only when `m is pad`\)|When you set the resize type to pad, you can select a color to fill the empty space.|The color values in the format of RGB. For example, 000000 indicates black, and FFFFFF indicates white. Default value: FFFFFF \(white\). |

    Example: the size of the source image is 200 × 100 pixels.The w resize parameter is set to 150 pixels, and the h resize parameter is set to 80 pixels. Thumbnails based on different resize types:

    -   lfit

        -   Proportional resizing: the w/h of the source image must be equal to that of the thumbnail. Therefore, if w is 150 pixels, h is 75 pixels. If h is 80 pixels, w is 160 pixels.
        -   Maximum image within a rectangle based on the specified width and height: The value of w\*h of the thumbnail cannot exceed 150 × 80 pixels.
        The thumbnail size is 150 × 75 pixels based on the preceding conditions.

        ![lfit](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7812863061/p137017.png)

    -   mfit

        -   Proportional resizing: the w/h of the source image must be equal to that of the thumbnail. Therefore, if w is 150 pixels, h is 75 pixels. If h is 80 pixels, w is 160 pixels.
        -   Minimum image beyond a rectangle based on the specified width and height: the thumbnail must be a minimum rectangle whose size is greater than 150 × 80 pixels.
        The thumbnail size is 160 × 80 pixels based on the preceding conditions.

        ![mfit](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7812863061/p137027.png)

    -   fill

        The fill parameter resizes the source image proportionally as small as possible beyond a rectangle, and crops the source image based on the specified width and height. The source image is resized to 160 × 80 pixels, and w is centered and cropped to 150 pixels to obtain a thumbnail of 150 × 80 pixels.

        ![fill](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7812863061/p137049.png)

    -   pad

        The pad parameter resizes the source image based on the specified width and height, and fills the empty space. The source image is resized to 150 × 75 pixels, and h is centered and filled to 80 pixels to obtain a thumbnail of 150 × 80 pixels.

        ![pad](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7812863061/p137053.png)

    -   fixed

        The fixed parameter resizes the image based on the specified width and height. If the width and height ratio of the image is different from that of the source image, the image is deformed.

        ![fixed](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8812863061/p137056.png)

-   Resize proportionally

    |Parameter|Required|Description|Valid value|
    |---------|--------|-----------|-----------|
    |p|Yes|Resizes an image by percentage.|\[1,1000\]A percentage smaller than 100 indicates that the image is scaled down. A percentage greater than 100 indicates that the image is scaled up. |


## Usage notes

-   Limits on source images:
    -   Only JPG, PNG, BMP, GIF, WebP, and TIFF images are supported. GIF images can be resized based on the specified width and height. Proportional resizing is not supported. GIF images become static images when you resize images proportionally.
    -   The object size cannot exceed 20 MB.
    -   Each side of the source image cannot exceed 30,000 pixels.
    -   The source image cannot exceed 250 million pixels in total.
-   Limits on thumbnails: The area of the thumbnail cannot exceed 4,096 × 4,096 pixels in size, and neither size can be greater than 4,096 × 4 pixels.
-   If the width or height of a thumbnail is specified:
    -   The source image is resized proportionally when proportional resizing is performed. For example, if a source image is 200 × 100 pixels, the height is resized to 100 pixels, and the width is resized to 50 pixels.
    -   The source image is resized based on the specified width and height. For example, if a source image is 200 × 100 pixels, the height is resized to 100 pixels, and the width is resized to 100 pixels.
-   If the size of the thumbnail is larger than that of the source image, the source image is returned by default. You can add the `limit_0` parameter to enlarge the image.

    Example: `https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_500,limit_0`


## Examples

The image-demo bucket that is located in the China \(Hangzhou\) region is used as an example. Public endpoint of the image:

[https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg)

![Source image](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8812863061/p139183.png)

-   Resize proportionally
    -   Based on the width and height

        Requirements and parameters:

        -   Resize the source image to a height of 100 pixels: `resize,h_100`
        -   Resize type: `m_lfit`
        The URL used to process the image is in the following format: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,h\_100,m\_lfit](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,h_100,m_lfit)

        ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3856348951/p2414.jpg)

    -   Based on the longer side and shorter side

        Requirements and parameters:

        -   Resize the source image based on the longer side of 100 pixels to a thumbnail: `resize,l_100`
        -   Resize type: `m_mfit`
        The URL used to process the image is in the following format: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,l\_100,m\_mfit](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,l_100,m_mfit)

        ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3856348951/p2415.jpg)

-   Resize based on the specified width and height

    Requirements and parameters:

    -   Resize the source image to a width and a height of 100 pixels: `resize,h_100,w_100`
    -   Resize type: `m_fixed`
    The URL used to process the image is in the following format: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m\_fixed,h\_100,w\_100](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m_fixed,h_100,w_100)

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3856348951/p2416.jpg)

-   Crop based on the specified width and height

    Requirements and parameters:

    -   Resize the source image to a width and a height of 100 pixels: `resize,h_100,w_100`
    -   Resize type: `m_fill`
    The URL used to process the image is in the following format: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m\_fill,h\_100,w\_100](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m_fill,h_100,w_100)

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4856348951/p2421.jpg)

-   Resize the source image based on the specified width and height and fill the empty space

    Requirements and parameters:

    -   Resize the source image to a width and a height of 100 pixels: `resize,h_100,w_100`
    -   Resize type: `m_pad`
    -   Fill the empty space in red: `olor_FF0000`
    The URL used to process the image is in the following format: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m\_pad,h\_100,w\_100,color\_FF0000](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m_pad,h_100,w_100,color_FF0000)

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4856348951/p2423.jpg)

-   Resize proportionally

    Requirements and parameters:

    Resize the source image by 50%: `resize,p_50`

    The URL used to process the image is in the following format: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,p\_50](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,p_50)

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4856348951/p2425.jpg)


