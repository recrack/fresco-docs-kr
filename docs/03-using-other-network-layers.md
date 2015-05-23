---
id: using-other-network-layers
title: Using Other Network Layers
layout: docs
permalink: /docs/using-other-network-layers.html
prev: closeable-references.html
next: using-other-image-loaders.html
---

기본적으로 안드로이드에서 이미지 파이프 라인은 [HttpURLConnection](https://developer.android.com/training/basics/network-ops/connecting.html) 네트워크 라이브러리를 사용합니다. 앱들은 자신들이 사용하는 네트워크 레이어를 대신해서 사용하고 싶어합니다.

### Using OkHttp
OkHttp 사용법

[OkHttp](http://square.github.io/okhttp)는 많이 사용되는 오픈소스 네트워크 라이브러리 입니다. 이미지 파이프라인 백앤드는 안드로이드 기본 라이르버리 대신 OkHttp를 사용합니다.

#####  OkHttp in Gradle

사용하는 방법은 프로젝트의 `build.gradle` 파일에서 `dependencies` 섹션을 수정해야 합니다. [다운로드](index.html) 페이지에서 제공되어지는 Gradle dependencies를 사용하면 **안됩니다.**
대신에 이것을 사용하세요:

```groovy
dependencies {
  // your project's other dependencies
  compile "com.facebook.fresco:fresco:{{site.current_version}}+"
  compile "com.facebook.fresco:imagepipeline-okhttp:{{site.current_version}}+"
}
```

##### Eclipse에서 OkHttp 사용하기

Eclipse users should depend on **both** the `fresco` and `imagepipeline-okhttp` directories in the `frescolib` tree as described in the [Eclipse instructions](index.html#eclipse-adt).
Eclipse 사용자는 `fresco` 와 `imagepipeline-okhttp`를 `frescolib` 에 ...??

##### 이미지 파이프라인을 OkHttp와 사용하는 방법

당신은 이미지 파이프라인을 조금 다르게 구성을 해야합니다. `ImagePipelineConfig.newBuilder` 대신 `OkHttpImagePipelineConfigFactory`를 사용합니다.

```java
Context context;
OkHttpClient okHttpClient; // build on your own
ImagePipelineConfig config = OkHttpImagePipelineConfigFactory
    .newBuilder(context, okHttpClient)
    . // other setters
    . // setNetworkFetcher is already called for you
    .build();
Fresco.initialize(context, config);
```


### 당신이 사용하는 Network fetcher와 사용하는 방법(옵션)

Network layer에서 완벽하게 제어하는 방법은 당신의 앱에서 제공되어야합니다. Network 통신에 대해 제어하기 위해서는 [NetworkFetcher](../javadoc/reference/com/facebook/imagepipeline/producers/NetworkFetcher.html)를 subclass로 사용해야합니다. 또한 Data structure 구체적인 정보를 요청하기 위해서는 [FetchState](../javadoc/reference/com/facebook/imagepipeline/producers/FetchState.html)를 subclass로 사용하면 됩니다.

Our default implementation for `HttpURLConnection` can be used as an example. See [its source code](https://github.com/facebook/fresco/blob/master/imagepipeline-backends/imagepipeline-okhttp/src/main/java/com/facebook/imagepipeline/backends/okhttp/OkHttpNetworkFetcher.java).

당신은 반드시 이미지 파이프라인을 [구성할때](configuring-image-pipeline.html), network producer를 이미지 파이프라인에 통과시켜야합니다.

```java
ImagePipelineConfig config = ImagePipelineConfig.newBuilder()
  .setNetworkFetcher(myNetworkFetcher);
  . // other setters
  .build();
Fresco.initialize(context, config);
```
