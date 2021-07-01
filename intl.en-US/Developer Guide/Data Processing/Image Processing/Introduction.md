# Introduction

You can add Image Processing \(IMG\) parameters to GetObject requests to process image objects stored in Object Storage Service \(OSS\). For example, you can add image watermarks to images or convert image formats.

## Video tutorial

The following video shows you how to process image objects. 

## Parameters

OSS allows you to directly use one or more parameters to process images. You can also encapsulate multiple IMG parameters in a style to batch process images. When multiple IMG parameters are specified, OSS processes the image in the order of the parameters. The following table describes the parameters that you can configure to process images.

|IMG operation|Parameter|Description|
|-------------|---------|-----------|
|[Resize images](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Resize images.md)|resize|Resizes images to a specified size.|
|[Incircle](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Incircle.md)|circle|Crops images based on the center point of images to ellipses of the specified size.|
|[Custom crop](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Custom crop.md)|crop|Crops rectangular images of the specified size.|
|[Indexed cut](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Indexed cut.md)|indexcrop|Cuts images along the specified horizontal or vertical axis and selects one of the images.|
|[Rounded rectangle](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Rounded rectangle.md)|rounded-corners|Crops images to rounded rectangles based on the specified rounded corner size.|
|[Automatic rotation](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Automatic rotation.md)|auto-orient|Auto-rotates images for which the auto-orient parameter is configured.|
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

We recommend that you use image styles to process multiple images at a time. For more information, see [Image style](/intl.en-US/Developer Guide/Data Processing/Image Processing/Image style.md).

## Implementation methods

You can use object URLs, API operations, and SDKs to process images. For more information, see [IMG implementation modes](/intl.en-US/Developer Guide/Data Processing/Image Processing/IMG implementation modes.md).

## Limits

When you use IMG, take note of the following items:

-   Limits on source images
    -   Only JPG, PNG, BMP, GIF, WebP, and TIFF images are supported.
    -   The size of the source image cannot exceed 20 MB.
    -   For the rotate operation, the height or width of the source image cannot exceed 4,096 pixels. For other operations, the width or height of the source image cannot exceed 30,000 pixels, and the total pixel number of the source image cannot exceed 250 million.

        The total pixel number of a dynamic image, such as a GIF image, is calculated by using the following formula: `Width × Height × Number of image frames`. The total pixel number of a static image, such as a PNG image, is calculated by using the following formula: `Width × Height`.

-   Limits on resized images

    The width or height of the resized image cannot exceed 16,384 pixels. The total pixel number of the resized image cannot exceed 16,777,216.

-   Limits on image styles

    You can create up to 50 image styles in each bucket. To create more than 50 styles for a bucket,contact the [technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).


## Billing

When you use IMG, the following fees are incurred:

-   Image processing fees: Image processing fees are only charged after you exceed the free quota. The fees are charged based on the size of the source images. For more information about image processing fees, see [Data processing fees](/intl.en-US/Pricing/Billing items and methods/Data processing fees.md).
-   Request fees: A GetObject request is generated each time when you use IMG to process an image. You are charged based on the number of generated requests. For more information about request fees, see [API operation calling fees](/intl.en-US/Pricing/Billing items and methods/API operation calling fees.md).
-   Traffic fees: You are charged for the outbound traffic over the Internet based on the size of the source images. For more information about traffic fees, see [Traffic fees](/intl.en-US/Pricing/Billing items and methods/Traffic fees.md).

## Versions

IMG provides two API versions: the later API version and the earlier API version. This topic describes the APIs of the later version. APIs of the earlier version are not updated. For more information about the compatibility between the later and earlier versions of APIs, see [FAQ on using old and new versions of APIs and domain names](/intl.en-US/Developer Guide/Data Processing/Image Processing/FAQ on using old and new versions of APIs and domain names.md).

