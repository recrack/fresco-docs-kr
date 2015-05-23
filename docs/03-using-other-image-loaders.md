---
id: using-other-image-loaders
title: Using Other Image Loaders
layout: docs
permalink: /docs/using-other-image-loaders.html
prev: using-other-network-layers.html
---

Drawee is not tied to a particular image loading mechanism and can be used with other image loaders.
Drawee는 particular 이미지 로딩 메카니즘과 밀접하지 않습니다. 그리고 다른 이미지 로더들과 함께 사용이 가능합니다.

However, some of its features are only available on the Fresco image pipeline. Any feature in the preceding pages that required using an [ImageRequest](image-requests.html) or [configuration](configure-image-pipeline.html) may not work with a different loader.
그러나, 어떤 feauture들은 오직 Fresco의 이미지 파이프라인에서만 가능합니다. 다른 feature는 ....

### Using Drawee with Volley ImageLoader
Drawee 를 Volley 이미지 로더와 함께 사용하는 방법

We have an backend for Drawee that allows Volley's [ImageLoader](https://developer.android.com/training/volley/request.html) to be used instead of Fresco's image pipeline. 
Drawee를 위한 백엔드는 Volley의 [ImageLoader](https://developer.android.com/training/volley/request.html) 를 Fresco의 이미지 파이프 라인 대신해서 사용가능하게 허용됩니다.

We only recommend this for apps that already have a significant investment in Volley ImageLoader.

사용하는 방법은 프로젝트의 `build.gradle` 파일에서 `dependencies` 섹션을 수정해야 합니다. [다운로드](download-fresco.html) 페이지에서 제공되어지는 Gradle dependencies를 사용하면 **안됩니다.**
대신에 이것을 사용하세요:

```groovy
dependencies {
  // your project's other dependencies
  compile: "com.facebook.fresco:drawee-volley:{{site.current_version}}+"
}
```

#### Volley 이미지로더  초기화

Do not call `Fresco.initialize`. You must do yourself for Volley what it does with the image pipeline:
`Fresco.initialize` 를 호출하면 안됩니다. 반드시 Volley의 이미지 파이프라인을 사용해야 합니다.

```java
Context context;
ImageLoader imageLoader; // build yourself
VolleyDraweeControllerBuilderSupplier mControllerBuilderSupplier
    = new VolleyDraweeControllerBuilderSupplier(context, imageLoader);
SimpleDraweeView.initialize(mControllerBuilderSupplier);
```

`VolleyDraweeControllerBuilderSupplier`가 scope를 벗어나게 하면 안됩니다. 당신은 `SimpleDraweeView.setImageURI.` 를 언제 사용할때 까지 빌드 컨트롤들을 언제나 사용해야 합니다.

#### Using DraweeControllers with Volley ImageLoader
DraweeControllers 를 Volley 이미지로더와 함께 사용하는 방법

`Fresco.newControllerBuilder` 대신 호출하는 방법,

```java
VolleyController controller = mControllerBuilderSupplier
    .newControllerBuilder()
    . // setters
    .build();
mSimpleDraweeView.setController(controller);
```

### Using Drawee with other image loaders
Drawee를 다른 이미지 로더와 함께 사용하는 방법

다른 Drawee 백엔드들은 준비되지 않았습니다. [Volley 예제](https://github.com/facebook/fresco/tree/master/drawee-backends/drawee-volley/src/main/java/com/facebook/drawee/backends/volley) 모델을 참고하면 가능합니다.

