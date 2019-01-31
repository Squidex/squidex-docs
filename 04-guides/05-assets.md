# Assets

## How to retrieve content?

You can retrieve the content of an asset using the id from the following url:

    http://<YOUR-DOMAIN>/api/assets/{id}

Read the API reference: https://cloud.squidex.io/api/docs#operation/AssetContent_GetAssetContent

> NOTE: If you install Squidex by your own you can the API reference from https://MY-DOMAIN/api/doc. This does not include your the documentation for your app content, that can be retrieved from https://MY-DOMAIN/api/content/MY-APP/docs

## How to resize an image?

You can resize an image using the endpoint described above using the following parameters:

* `width` (number): The target width of the asset, if it is an image.
* `height` (number): The target height of the asset, if it is an image.
* `mode` (string): The resize mode when the width and height is defined. Default: `Max`

You have to pass in either the `width` or `height` parameter. if the asset is not an image these parameters have no effect.

Squidex supports the following resize modes:


* `Crop`: Crops the resized image. This will stretch the resized image to fit the bounds of its container.
* `Pad`: Pads the resized image to fit the bounds of its container. If only one dimension is passed, will maintain the original aspect ratio.
* `BoxPad`: Pads the image to fit the bound of the container without resizing the original source. When downscaling, performs the same functionality as `Pad`.
* `Max`: Constrains the resized image to fit the bounds of its container maintaining the original aspect ratio.
* `Min`: Resizes the image until the shortest side reaches the set given dimension. Upscaling is disabled in this mode and the original image will be returned if attempted. 
