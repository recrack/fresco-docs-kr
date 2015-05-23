---
id: intro-image-pipeline
title: Introduction to the Image Pipeline
layout: docs
permalink: /docs/intro-image-pipeline.html
prev: gotchas.html
next: configure-image-pipeline.html
---
** 확인해 볼것 : ImageRequest 문서의 Lowest permitted request level과 중복되는 내용임 **
이미지 파이프라인은 안드로이드 장치에서 렌더링할 수 있는 형식으로 이미지를 가져올 수 있게 필요한 모든 것을 제공합니다.

불러올 이미지를 받았을 때 파이프라인은 아래와 같은 단계를 수행합니다.


1. 비트맵 캐시를 찾아서 찾으면 돌려줍니다.
2. 다른 쓰레드에게 역활을 넘깁니다.
3. 인코딩된 메모리 캐시를 체크합니다. 발견하면 디코딩하고 변환해 돌려줍니다. 비트맵 캐시에 저장합니다.
3. 디스크 캐시를 체크합니다. 발견하면 디코딩하고 변환해 돌려줍니다. 비트맵 캐시에 저장합니다. 비트맵캐시와 인코드된 메모리에 저장합니다.
4. 네트워크(혹은 다른 원본 소스)를 체크합니다. 발견하면 디코드하고 변환하여 돌려줍니다. 세가지 캐시(메모리, 비트맵, 디스크캐시)에 저장합니다.

This being an image library, an image is worth a thousand words:
이것은 이미지 라이브러리가 되고, 이런 이미지는 수천개의 단어보다 더 가치있습니다.

![Image Pipeline Diagram](../static/imagepipeline.png "Image Pipeline")

(The 'disk' cache as pictured includes the encoded memory cache, to keep the logic path clearer.) See [this page](caching.html) for more details on caching.
그림의 `disk`캐시는 로직 경로를 보다 명확하게 하기 위해 인코드된 메모리 캐시를 포함합니다. 캐싱에 관한 더 자세한 사항은 [이 페이지](caching.html)를 참고하세요.

파이프라인은 [로컬 파일]을 네트워크상의 파일처럼 읽을 수 있습니다. PNG, GIF나 WEbP는 JPEG처럼 사용할 수 있습니다.



### 옛날 플랫폼의 WebP

WebP는 종종 유용한 이미지 형식이지만 안드로이드 3.0이전 버전 까찐 지원하지 않았습니다. 확장된 WebP 형식은 안드로이드 4.1.2까지 지원되지 않고요.
The image pipeline transcodes unsupported WebP images into JPEG for you. This allows you to use WebP in your app all the way back to Android 2.3.
이미지 파이프라인은 지원하지 않는 WebP이미지를 JPEG로 변환합니다. 프레스코로 안드로이드 2.3까지 WebP를 사용할 수 있습니다.