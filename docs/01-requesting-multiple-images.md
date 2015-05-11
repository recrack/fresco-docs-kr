---
id: requesting-multiple-images
title: Requesting Multiple Images (Multi-URI)
layout: docs
permalink: /docs/requesting-multiple-images.html
prev: animations.html
next: listening-download-events.html
---
이 메소드는 [이미지 리퀘스트 설정하기 setting your own image request](using-controllerbuilder.html) 페이지를 필요로 합니다.
The methods on this page require [setting your own image request](using-controllerbuilder.html).

### 낮은 해상도에서 높은 해상도로. Going from low to high resolution

높은 해상도의 이미지를 보여주는 것은 느립니다. placeholder를 잠시 보여주게 해놓고 빨리 작은 썸네일을 우선적으로 다운받아야 합니다.

2가지 URI를 설정할 수 있습니다. 하나는 작은 해상도 이미지고 다른 하나는 높은 해상도용 입니다.

```java
Uri lowResUri, highResUri;
DraweeController controller = Fresco.newDraweeControllerBuilder()
    .setLowResImageRequest(ImageRequest.fromUri(lowResUri))
    .setImageRequest(ImageRequest.fromUri(highResUri))
    .setOldController(mSimpleDraweeView.getController())
    .build();
mSimpleDraweeView.setController(controller);
```

### 썸네일 미리보기 사용하기

*이 옵션은 로컬 URI만 지원하고 JPEG 형식용입니다.
*This option is supported only for local URIs, and only for images in the JPEG format.*

JPEG가 썸네일이 저장된 EXIF 메타데이터를 갖고 있다면, 이 이미지 파이프라인은 중급의 결과를 돌려줍니다. 당신의 Drawee는 처음에 썸네일을 미리 보여주고, 불러오기와 복호화(loading and decoding)가 완료되면 전체 이미지를 보여줍니다.
If your JPEG has a thumbnail stored in its EXIF metadata, the image pipeline can return that as an intermediate result. Your Drawee will first show the thumbnail preview, then the full image when it has finished loading and decoding.

```java
Uri uri;
ImageRequest request = ImageRequestBuilder.newBuilderWithSource(uri)
    .setLocalThumbnailPreviewsEnabled(true)
    .build();

DraweeController controller = Fresco.newDraweeControllerBuilder()
    .setImageRequest(request)
    .setOldController(mSimpleDraweeView.getController())
    .build();
mSimpleDraweeView.setController(controller);
```


### 사용가능한 이미지 먼저 불러오기


Most of the time, an image has no more than one URI. Load it, and you're done.
대부분 이미지는 URI를 하나 가집니다. 불러오고 완료하죠.

But suppose you have multiple URIs for the same image. For instance, you might have uploaded an image taken from the camera. Original image would be too big to upload, so the image is downscaled first. In such case, it would be beneficial to first try to get the local-downscaled-uri, then if that fails, try to get the local-original-uri, and if even that fails, try to get the network-uploaded-uri. It would be a shame to download the image that we may have already locally. 
같은 이미지에 여러개의 URI를 가지고 있다고 가정해보죠. 카메라로 찍은 이미지로 예를 들어봅시다. 원본 이미지는 업로드하기엔 너무 큽니다. 그래서 이미지는 다운스케일이 먼저 필요합니다. 이 경우에 로컬의 다운스케일된 URI가 필요합니다. 다운스케일된 URI를 갖고오지 못하면 로컬의 원본이미지 uri를 가져옵니다. 이마저도 실패하면 네트워크에 올라가 있는 uri를 갖고 옵니다. 로컬에 이미 갖고 있는 이미지를 다시 다운로드하는 것은  창피한 일이죠.

The image pipeline normally searches for images in the memory cache first, then the disk cache, and only then goes out to the network or other source. Rather than doing this one by one for each image, we can have the pipeline check for *all* the images in the memory cache. Only if none were found would disk cache be searched in. Only if none were found there either would an external request be made.
이미지 파이프라인은 보통 메모리 캐시에 있는 이미지를 먼저 찾습니다. 그 다음이 디스크 캐시, 마지막이 네트워크나 다른 소스에서 찾습니다. 각각의 이미지에 대해 이 짓을 하는 것보다 우리가 메모리 캐시에 있는 *모든 이미지*의 파이프라인을 검사해볼 수 있습니다. 아무것도 찾지 못하면 외부요청이 만들어 집니다.??

Just create an array of image requests, and pass it to the builder.
이미지를 요청하는 배열을 만들어 봅시다. 그리고 빌더에 값을 전달해봐요.

```java
Uri uri1, uri2;
ImageRequest request = ImageRequest.fromUri(uri1);
ImageRequest request2 = ImageRequest.fromUri(uri2);
ImageRequest[] requests = { request1, request2 };

DraweeController controller = Fresco.newDraweeControllerBuilder()
    .setFirstAvailableImageRequests(requests)
    .setOldController(mSimpleDraweeView.getController())
    .build();
mSimpleDraweeView.setController(controller);
```

단지 한개의 요청만 화면에 표시됩니다. 메모리나 디스크, 네트워크 계층 어디서든 처음으로 발견된 것만 돌려주게 됩니다. 파이프라인은 배열의 요청 순서를 선호하는 순서로 가정합니다.
Only one of the requests will be displayed. The first one found, whether at memory, disk, or network level, will be the one returned. The pipeline will assume the order of requests in the array is the preference order.


### 커스텀 `DataSource`와 `공급자` 지정하기 Specifying a custom `DataSource` `Supplier`

Drawee 컨트롤러를 빌딩하면서 커스텀 `DataSource`와 `Supplier`를 유연하게 지정 가능합니다. 당신만의 공급자나 이미 존재하는 것을 당신이 선호하는 방식으로 작성할 수도 있습니다. `FirstAvailableDataSourceSupplier` 와 `IncreasingQualityDataSourceSupplier` 에서 구현 예제를 보세요. `AbstractDraweeControllerBuilder` 에서 이 공급자들이 어떻게 함께 작성되는지 볼 수 있습니다.
For even more flexibility, it is possible to specify a custom `DataSource` `Supplier` while building a Drawee controller. You can implement your own supplier or just compose the existing ones in whichever way you like. See  `FirstAvailableDataSourceSupplier` and `IncreasingQualityDataSourceSupplier` for an example implementation. See `AbstractDraweeControllerBuilder` for how those suppliers can be composed together.

