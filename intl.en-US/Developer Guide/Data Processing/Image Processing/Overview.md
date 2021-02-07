# Overview

You can add Image Processing \(IMG\) parameters in the GetObject operation to process image objects stored in OSS. For example, you can add image watermarks or convert image formats.

## Parameters

OSS allows you to directly use one or more parameters to process images, or encapsulate multiple IMG parameters in a style to process multiple images at a time. OSS processes the images in the order of the parameters when multiple IMG parameters exist. The following table lists the parameters of IMG.

|IMG operation|Parameter|Description|
|-------------|---------|-----------|
|[Resize images](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Resize images.md)|resize|Resizes images to a specified size.|
|[Incircle](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Incircle.md)|circle|Crops images based on the center point of images to ellipses of the specified size.|
|[Custom crop](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Custom crop.md)|crop|Crops rectangular images of the specified size.|
|[Indexed cut](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Indexed cut.md)|indexcrop|Cuts images along the specified horizontal or vertical axis and selects one of the images.|
|[Rounded rectangle](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Rounded rectangle.md)|rounded-corners|Crops images to rounded rectangles based on the specified rounded corner size.|
|[Automatic rotation](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Automatic rotation.md)|auto-orient|Auto-rotates images that include the auto-orient parameter.|
|[Rotate](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Rotate.md)|rotate|Rotates images clockwise based on the specified angle.|
|[Blur](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Blur.md)|blur|Blurs images.|
|[Brightness](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Brightness.md)|bright|Adjusts the brightness of images.|
|[Sharpen](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Sharpen.md)|sharpen|Sharpens images.|
|[Contrast](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Contrast.md)|contrast|Adjusts the contrast of images.|
|[Gradual display](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Gradual display.md)|interlace|Configures gradual display for the JPG images.|
|[Quality transformation](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Quality transformation.md)|quality|Adjusts the quality of images in the JPG and WebP formats.|
|[Format conversion](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Format conversion.md)|format|Converts image formats.|
|[Add watermarks](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Add watermarks.md)|watermark|Adds image or text watermarks to images.|
|[Query the average tone](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Query the average tone.md)|average-hue|Queries the average tone of images.|
|[Query image information](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Query image information.md)|info|Queries image information, including basic information and EXIF information.|

We recommend that you use image style to process multiple images at a time. For more information, see [Image style](/intl.en-US/Developer Guide/Data Processing/Image Processing/Image style.md).

## Implementation methods

You can use object URLs, API operations, and SDKs to process images. For more information, see [IMG implementation modes](/intl.en-US/Developer Guide/Data Processing/Image Processing/IMG implementation modes.md).

## Limits

When you use IMG, take note of the following items:

-   Limits on source images
    -   Only JPG, PNG, BMP, GIF, WebP, and TIFF images are supported.
    -   The size of the image object cannot exceed 20 MB.
    -   The width or height of the source image cannot exceed 4,096 pixels when you rotate images.
    -   Each side of the source image cannot exceed 30,000 pixels.
    -   The source image cannot exceed 250 million pixels in total.

        The pixel number of a static image is calculated by using the following formula: `Width x Height`. The pixel number of a dynamic image, such as a GIF image, is calculated by using the following formula: `Width x Height x Number of image frames`.

-   Limits on resized images
    -   Each side of a resized image cannot exceed 16,384 pixels.
    -   The image cannot exceed 16,777,216 pixels in total.
-   Limits on image styles

    You can create up to 50 image styles in each bucket. If you need to create more than 50 styles, contact[technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).


## Versions

IMG provides two API versions: the later API version and the earlier API version. This topic describes the APIs of the later version. The APIs of the earlier version are not updated. For more information about the compatibility between the later and earlier versions of APIs, see [FAQ on using old and new versions of APIs and domain names](/intl.en-US/Developer Guide/Data Processing/Image Processing/FAQ on using old and new versions of APIs and domain names.md).

