# Resize images

You can use the resize parameters to adjust the size of images stored in OSS. This topic describes the parameters used to resize images and provides examples on how to resize images.

## Parameters

Operation: `resize`

The following table lists the parameters that you can configure when you resize images.

-   Resize an image based on the specified height and width

    |Parameter|Required|Description|Valid value|
    |:--------|--------|:----------|:----------|
    |m|Yes|Specifies the type of the resize operation. Default value: lfit.|    -   lfit: resizes the source image proportionally as large as possible within a rectangle based on the specified width and height.
    -   mfit: resizes the source image proportionally as small as possible beyond a rectangle based on the specified width and height.
    -   fill: resizes the source image proportionally as small as possible beyond a rectangle, and then crops the source image based on the specified width and height.
    -   pad: resizes the source image based on the specified width and height, and fills the empty space by using the specified color.
    -   fixed: forcibly resizes the source image based on the specified width and height.
For more information, see the examples following the table.|
    |w|Yes|Specifies the width to which the image is to be resized.|\[1,4096\]|
    |h|Yes|Specifies the height to which the image is to be resized.|\[1,4096\]|
    |l|Yes|Specifies the length of the longer side to which the image is to be resized. **Note:** Longer side refers to the side with the larger ratio of source size to requested size. Shorter side refers to the side with the smaller ratio of source size to requested size. For example, if a source image is resized from 400 × 200 pixels to 800 × 100 pixels, the source-to-requested ratios are 0.5 \(400/800\) and 2 \(200/100\). Therefore, 0.5 is smaller than 2, the 200-pixel side is the longer side, and the 400-pixel side the shorter side.

|\[1,4096\]|
    |s|Yes|Specifies the length of the shorter side to which the image is to be resized.|\[1,4096\]|
    |limit|No|Specifies whether to process the requested resized image when it is larger than the source image.|0 and 1. Default value: 1.    -   1: returns the source image without resizing the image.
    -   0: resizes the source image based on the specified parameter. |
    |color|Yes \(only when the value of `m` is pad\)|When you set the resize type to pad, you can select a color to fill the empty space.|RGB color values. For example, 000000 indicates black, and FFFFFF indicates white. Default value: FFFFFF \(white\). |

    Example: The size of the source image is 200 × 100 pixels. The w parameter is set to 150 pixels, and the h parameter is set to 80 pixels. The source image is resized in different methods when you specify different resize types.

    -   lfit

        -   Proportional resizing: The value of w/h of the source image must be equal to that of the resized image. Therefore, if w is 150 pixels, h is 75 pixels. If h is 80 pixels, w is 160 pixels.
        -   Maximum image within a rectangle based on the specified width and height: The value of w\*h of the resized image cannot exceed 150 × 80 pixels.
        The size of the resized image is 150 × 75 pixels based on the preceding conditions.

        ![lfit](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7812863061/p137017.png)

    -   mfit

        -   Proportional resizing: the value of w/h of the source image must be equal to that of the resized image. Therefore, if w is 150 pixels, h is 75 pixels. If h is 80 pixels, w is 160 pixels.
        -   Minimum image beyond a rectangle based on the specified width and height: The resized image must be a minimum rectangle whose size is greater than 150 × 80 pixels.
        The size of the resized image is 160 × 80 pixels based on the preceding conditions.

        ![mfit](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7812863061/p137027.png)

    -   fill

        The fill parameter resizes the source image proportionally as small as possible beyond a rectangle, and crops the source image based on the specified width and height. The source image is resized to 160 × 80 pixels, and w is centered and cropped to 150 pixels to obtain a resized image of 150 × 80 pixels.

        ![fill](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7812863061/p137049.png)

    -   pad

        The pad parameter resizes the source image based on the specified width and height, and fills the empty space. The source image is resized to 150 × 75 pixels, and h is centered and filled to 80 pixels to obtain a resized image of 150 × 80 pixels.

        ![pad](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7812863061/p137053.png)

    -   fixed

        The fixed parameter resizes the image based on the specified width and height. If the width and height ratio of the image is different from that of the source image, the image is deformed.

        ![fixed](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8812863061/p137056.png)

-   Resize an image proportionally

    |Parameter|Required|Description|Valid value|
    |---------|--------|-----------|-----------|
    |p|Yes|Resizes an image by percentage.|\[1,1000\]A percentage smaller than 100 indicates that the image is scaled down. A percentage greater than 100 indicates that the image is scaled up. |


## Usage notes

-   Limits on source images
    -   Only JPG, PNG, BMP, GIF, WebP, and TIFF images are supported. GIF images can be resized based on the specified width and height but cannot be resized proportionally. GIF images become static images when you resize the images proportionally.
    -   The size of the source image cannot exceed 20 MB.
    -   The width or height of the source image cannot exceed 30,000 pixels. The total pixel number of the source image cannot exceed 250 million.

        The total pixel number of a dynamic image, such as a GIF image, is calculated by using the following formula: `Width × Height × Number of image frames`. The total pixel number of a static image, such as a PNG image, is calculated by using the following formula: `Width × Height`.

-   Limits on resized images

    The width or height of the resized image cannot exceed 16,384 pixels. The total pixel number of the resized image cannot exceed 16,777,216.

-   If the width or height of the resized image is specified:
    -   The source image is resized proportionally when proportional resizing is performed. For example, if you resize the height of a source image from 200 × 100 pixels to 100 pixels, the width of the source image is resized to 50 pixels.
    -   The source image is resized based on the specified width and height. For example, if you resize the height of a source image from 200 × 100 pixels to 100 pixels, the width of the source image is resized to 100 pixels.
-   If you set the m parameter to mfit and specify a value for the l parameter that indicates the length of the longer side or the s parameter that indicates the length of the shorter side, the longer side or the shorter side of the image is scaled based on the specified value of l or s.
-   By default, if the size of the resized image is larger than that of the source image, the source image is returned. You can add the `limit_0` parameter to enlarge the image. Example: `https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_500,limit_0`

## Examples

An image in the bucket named image-demo in the China \(Hangzhou\) region is used in this example. The following URL is used to access the image over the Internet:

[https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg)

![Source image](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8812863061/p139183.png)

-   Resize the image proportionally
    -   Based on the width or height

        Configure parameters to resize the image:

        -   Resize the source image to a height of 100 pixels: `resize,h_100`
        -   Specify the resize type: `m_lfit`
        The URL used to process the image is in the following format: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,h\_100,m\_lfit](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,h_100,m_lfit)

        ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3856348951/p2414.jpg)

    -   Based on the longer side

        Resize the source image based on the longer side of 100 pixels: `resize,l_100`

        The URL used to process the image is in the following format: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,l\_100](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,l_100,m_mfit)

        ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3856348951/p2415.jpg)

-   Resize the image based on the specified width and height

    Configure parameters to resize the image:

    -   Resize the source image to a width and a height of 100 pixels: `resize,h_100,w_100`
    -   Specify the resize type: `m_fixed`
    The URL used to process the image is in the following format: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m\_fixed,h\_100,w\_100](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m_fixed,h_100,w_100)

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3856348951/p2416.jpg)

-   Crop the image based on the specified width and height

    Configure parameters to resize the image:

    -   Resize the source image to a width and a height of 100 pixels: `resize,h_100,w_100`
    -   Specify the resize type: `m_fill`
    The URL used to process the image is in the following format: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m\_fill,h\_100,w\_100](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m_fill,h_100,w_100)

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4856348951/p2421.jpg)

-   Resize the source image based on the specified width and height and fill the empty space

    Configure parameters to resize the image:

    -   Resize the source image to a width and a height of 100 pixels: `resize,h_100,w_100`
    -   Specify the resize type: `m_pad`
    -   Fill the empty space in red: `color_FF0000`
    The URL used to process the image is in the following format: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m\_pad,h\_100,w\_100,color\_FF0000](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m_pad,h_100,w_100,color_FF0000)

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4856348951/p2423.jpg)

-   Resize the image proportionally

    Configure parameters to resize the image:

    Resize the source image by 50%: `resize,p_50`

    The URL used to process the image is in the following format: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,p\_50](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,p_50)

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4856348951/p2425.jpg)


## FAQ

How do I access a resized image if the ACL of the image is private?

You must add signature information to the URL of the resized image to access the image. For more information about how to obtain the URL of an object, see [How do I obtain the URL of an uploaded object?](/intl.en-US/Developer Guide/Objects/FAQ/How do I obtain the URL of an uploaded object?.md)

