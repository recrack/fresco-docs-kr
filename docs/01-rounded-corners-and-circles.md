---
id: rounded-corners-and-circles
title: Rounded Corners and Circles
layout: docs
permalink: /docs/rounded-corners-and-circles.html
prev: scaling.html
next: using-controllerbuilder.html
---

모든 아미지가 네모는 아닙니다. 앱은 자주 부드럽고, 가장자리가 둥글게 처리되거나 아예 둥글게 필요한 경우가 있습니다. Drawee 는 비트맵 복사를 메모리 오버헤드 없이 다양한 시나리오로 제공합니다.

### 무엇을

Images can be rounded in two shapes:
이미지는 2가지 형태로 둥글게 처리될 수 있습니다.

1. As a circle - set `roundAsCircle` to true.
2. As a rectangle, but with rounded corners. Set `roundedCornerRadius` to some value.
3. 원모양으로 처리 - `roundAsCircle` 속성을 true로 설정
4. 네모지만 가장자리르 둥글게 처리 - `roundedCornerRadius` 에 값을 설정

Rectangles support having each of the four corners have a different radius, but this must be specified in Java code rather than XML.

네모는 각 가장자리가 다른 곡률을 가지도록 할 수 있습니다. 하지만 이 일은 XML보단 Java 코드에서 꼭 설정되어야만 합니다.

### 어떻게

이미지를 둥글게 처리하는 방벙은 2가지가 있습니다.
Images can be rounded with two different methods:

1. `BITMAP_ONLY` - Uses a shader to draw the bitmap with rounded corners. This is the default rounding method. This works only on the actual image and the [placeholder](drawee-components.html). Other branches, like failure and retry images, are not rounded. Furthermore, this rounding method doesn't support animations.
2. `OVERLAY_COLOR` - Draws rounded corners by overlaying a solid color, specified by the caller. The Drawee's background should be static and of the same solid color. Use `roundWithOverlayColor` in XML, or `setOverlayColor` in code, for this effect.
3. `BITMAP_ONLY` - 쉐이더를 사용해 비트맵을 그리고 가장자리를 둥글게 처리합니다. 이것은 기본 메소드입니다. actual 이미지와 [placeholder](drawee-components.html)에만 동작합니다. 다른 것들 - 이미지 그리기 실패, 재시도 이미지 -는 지원하지 않습니다. 또 애니메이션도 지원되지 않습니다.
4. `OVERLAY_COLOR` - 가장자리를 호출자가 지정한 특정 색깔로 덮어 그립니다. Drawee의 배경은 같은 색깔로 채워집니다. XML의 `roundWithOverlayColor` 나, 코드에서 `setOverlayColor`를 사용하세요. - 선호 : 실제 사용해보고 보완하기. static and of same solid color?


### XML 에서

The `SimpleDraweeView` class will forward several attributes over to `RoundingParams`:
`SimpleDraweeView` 클래스는 몇가지 `RoundingParams` 속성을 제공합니다.
```xml
<com.facebook.drawee.view.SimpleDraweeView
   ...
   fresco:roundedCornerRadius="5dp"
   fresco:roundBottomLeft="false"
   fresco:roundBottomRight="false"
   fresco:roundWithOverlayColor="@color/blue"
   fresco:roundingBorderWidth="1dp"
   fresco:roundingBorderColor="@color/red"
```

### 코드에서

[constructing a hierarchy](using-drawees-code.html)에서 [RoundingParams] 인스턴스를 `GenericDraweeHierarchyBuilder:`로 전달할 수 있습니다.

```java
RoundingParams roundingParams = RoundingParams.fromCornersRadius(7f);
roundingParams.setOverlayColor(R.color.green);
// alternatively use fromCornersRadii or asCircle
genericDraweeHierarchyBuilder
    .setRoundingParams(roundingParams);
```
또 대부분의 둥글게 처리하는 파라미터를 바꿀 수 있습니다.
You can also change most of the rounding parameters on the fly:

```java
RoundingParams roundingParams =
    mSimpleDraweeView.getHierarchy().getRoundingParams();
roundingParams.setBorder(R.color.red, 1.0);
roundingParams.setRoundAsCircle(true);
mSimpleDraweeView.getHierarchy().setRoundingParams(roundingParams);
```
한 예외는 `RoundingMethod`는 동적으로 바꿀 수 없다는 것입니다. 그런데도 시도한다면 `IllegalStateException`을 발생시킵니다.
The one exception to this is that the `RoundingMethod` cannot be changed when changing dynamically. Attempting to do so will throw an `IllegalStateException.`


### Caveats

There are some limitations when `BITMAP_ONLY` (the default) mode is used:
기본 설정인 `BITMAP_ONLY`모드에는 몇가지 제한이 있습니다:
- Not all image branches are rounded. Only the placeholder and the actual image are rounded. We are in the process of adding support for rounding backgrounds.
- 모든 이미지 종류가 둥글게 되진 않습니다. placeholder와 actual 이미지만 둥글게 됩니다. 다른 것들은 현재 작업 중이에요.
- Only images that resolve to `BitmapDrawable` or `ColorDrawable` can be rounded. Rounding `NinePatchDrawable`, `ShapeDrawable` and other such drawables is not supported (regardless whether they are specified in XML or programmatically).
- `BitmapDrawable`이나 `ColorDrawable`로 쓸 수 있는 것만 둥글게 처리될 수 있습니다. `NinePatchDrawable`, `ShapeDrawable` 그리고 다른 Drawable을 둥글게 처리하도록 지원하진 않습니다. (XML이나 코드에서 쓸 수 있도록 간주하지 않음)
- Animations are not rounded.
- 애니메이션도 둥글게 처리 안되요.
- Due to a limitation of Android's `BitmapShader`, if the image doesn't fully cover the view, isntead of drawing nothing, edges are repeated. One workaround is to use a different scale type (e.g. centerCrop) that ensures that the whole view is covered.
- 안드로이드의 `BitmapShader` 한계 때문에, 만약 아무것도 그리지 않는 대신 이미지가view를 전부 덮지 않는다면, 가장자리는 반복됩니다.? 한가지 차선책은 뷰 전체가 덮이도록 다른 scale 타입(e.g centerCrop)을 사용하는 겁니다.

`OVERLAY_COLOR`모드는 앞서 언급한 제한이 없습니다. 하지만 단순한 색깔로 가장자리를 덮기 때문에 정적거나 같은 색깔의 뷰 배경에서만 보기에 좋습니다.

Drawee는 내부적으로 `CLIPPING` 모드를 구현합니다. 하지만 이 모드는 어떤 `Canvas`에선 사용가능하지 않거나 노출되지 않기 때문에 경로클리핑(path clipping)은 지원하지 않습니다. 또, canvas 클리핑은 픽셀단pixelated)로 가장자리를 둥글게 처리하는 안티앨리어싱을 지원하지 않습니다.
Drawee internally has an implementation for `CLIPPING` mode, but this mode has been disabled and not exposed as some `Canvas` implementation do not support path clipping. Furthermore, canvas clipping doesn't support antialiasing which makes the rounded edges very pixelated.

마지막으로, 모든 이슈들은 임시 비트맵으로 회피할 순 있습니다. 하지만 이는 심각한 메모리 오버헤드때문에 지원되지 않습니다.
Finally, all of those issues could be avoided by using a temporary bitmap, but this imposes a significant memory overhead and has not been supported because of that.

위에서 설명한 것 처럼, 가장자리를 둥글게 처리하는 최적의 솔루션은 없습니다. 하나를 선택해야만 한다면 앞에서 언급한 것과 트레이드오프 해야 될 것 입니다.
As explained above, there is no really good solution for rounding corners on Android and one has to choose between the aforementioned trade-offs.