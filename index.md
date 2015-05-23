---
layout: default
title: Fresco | An image management library.
id: home
hero: true
---

# About Fresco

Fresco는 안드로이드 어플리케이션에서 화면을 표시하기위한 강력한 시스템 입니다. 별도의 작업을 하지 않아도 이미지 로딩과 화면표시에 대한 처리를 합니다.

Fresco의 *이미지 파이프라인*은 네트워크나 로컬저장소나 로털리소스에서 이미지들을 로드합니다. 3가지 레벨로 데이터와 CPU에 저장합니다. (2개의 메모리와 내부저장소)

Fresco의 *Drawee*들은 이미지들이 자동으로 로드되고 도착할때까지 placehonder에 보여집니다. 로드된 이미지들은 화면이 꺼질 때, 자동으로 메모리에서 해제됩니다.

Fresco는 안드로이드 2.3(Gingerbread)버전과 이후 버전을 지원합니다.

# Features

### Memory

이미지 압축 - 안드로이드 `Bitmap` - 많은 메모리가 필요합니다. Java의 garbabe collector를 자주 사용하기 때문에 앱이 느려집니다. 안드로이드 5.0에서 garbage collector가 개선되지 않아 매우 나쁘다고 보입니다.

안드로이드 4.x과 이전 버전에서 Fresco는 안드로이드에서 이미지 영역의 메모리에 초점을 맞췄습니다. 이미지가 화면에 보이지 않으면 자동으로 메모리 해제를 합니다. 당신이 작성한 어플리케이션을 빠르게 그리고 crash로 인해 발생하는 고통이 적어집니다.

앱에서 Fresco를 사용하면 메모리 footprint를 유지하기 위해 신경을 쓰지 않아도 저사양의 디바이스에서 실행할 수 있습니다.

### Streaming

Progressive JPEG images have been on the Web for years. 저해상도의 이미지를 처음 다운 받아 화면에 표시를 하고 고해상도의 이미지를 다운받아 보여줍니다. 이 것은 느린 네트워크를 사용하는 사용자들에게 유용합니다.

안드로이드는 이미지 라이브러리들의 스트리밍에 대해 제공을 하지 않습니다. Fresco는 스트리밍을 제공합니다. URI만 제공이 되어지면 당신의 앱에서는 자동으로 URI의 데이터가 도착을 하면 화면에 업데이트를 합니다.

### Animations

앱에서 애니메이션 GIF와 WebP를 사용하는 것은 매우 도전적인 일입니다. 각 프레임들의 애니메이션 프레임마다 큰`Bitmap`을 사용합니다. Fresco는 프레임들의 loading과 disposing에 대해 관리를 합니다.

### Drawing

Fresco는 디스플레이 장치에 `Drawees`를 사용합니다. 유용한 기능들에 대해서 제공합니다. :

* Scale the image to a custom focus point, instead of the center
* 이미지에 라운드코너 또는 원형 이미지로 표현이 가능합니다.
* 네트워크 실패로 이미지 로딩이 안된경우 placeholder를 선택하면 이미지를 다시 로드합니다.
* 이미지에 사용자 백그라운드, 오버레이, 또는 프로그래스바를 표현합니다.
* 이미지를 누르면 사용자 오버레이가 가능합니다.

### Loading 

Fresco's image pipeline lets you customize the load in a variety of ways:

* Specify several different uris for an image, and choose the one already in cache for display
* 저해상도 이미지를 처음보여주고 고해상도 이미지가 다운이 완료되면 교체를 합니다.
* 이미지가 당신의 앱으로 도착을 하면 이벤트를 보냅니다.
* 이미지에 EXIF thumbnail이 있으면, 최초에 한번 이미지가 전체 로드되면 보여줍니다. (디바이스에있는 이미지만)
* 이미지 사이즈 조정 또는 화면 전환
* 다운 받은 이미지 수정
* WebP 이미지들의 decode는 안드로이드의 이전버전에서는 fully 지원하지 않습니다.
        
# More information

* Our [blog post](https://code.facebook.com/posts/366199913563917) announcing Fresco and explaining the technical design
* Our [talk](https://developers.facebooklive.com/videos/542/move-fast-ensuring-mobile-performance-without-breaking-things) at the F8 conference (begins at 25:00, or -14:35 remaining)
* [Download](docs/index.html) Fresco
* Our [documentation](docs/getting-started.html)
* Our source code on [GitHub](https://github.com/facebook/fresco)
