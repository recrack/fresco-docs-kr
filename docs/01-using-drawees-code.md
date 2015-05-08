---
id: using-drawees-code
title: Using Drawees in Code
layout: docs
permalink: /docs/using-drawees-code.html
prev: using-drawees-xml.html
next: drawee-components.html
---

### 이미지 바꾸기

쉬운 방법은 그냥 호출하는 거에요.

```java
mSimpleDraweeView.setImageURI(uri);
```
[controller builder](using-controllerbuilder.html)에 더 많은 설정들이 있어요.

### 상속 커스터마이징 Customizing the hierarchy

대부분의 앱은 [XML](using-drawees-xml.html)에 있는 파라미터를 상속하여 커스터마이징 방법을 기술합니다. 하지만 몇 가지 경우엔 더 많은 것이 필요할 수도 있어요.

우린 빌더 인스턴스를 만들고 뷰에 값을 설정할 수 있습니다 :

```java
List<Drawable> backgroundsList;
List<Drawable> overlaysList;
GenericDraweeHierarchyBuilder builder =
    new GenericDraweeHierarchyBuilder(getResources());
GenericDraweeHierarchy hierarchy = builder
    .setFadeDuration(300)
    .setPlaceholderImage(new MyCustomDrawable())
    .setBackgrounds(backgroundList)
    .setOverlays(overlaysList)
    .build();
mSimpleDraweeView.setHierarchy(hierarchy);
```
같은 뷰에서 `setHierarchy`를 2번 이상 호출하면 **안됩니다**. 이 뷰가 재활용되었어도 안되요. 이 상속받은 속성은 생성하는데 비용이 많이 들고, 재사용되기 위함입니다. `setController` 나 `setImageURI` 로 이미지를 바꾸세요.
Do **not** call `setHierarchy` more than once on the same view, even if the view is recycled. The hierarchy is expensive to create and is intended to be used more than once. Use `setController` or `setImageURI` to change the image shown in it.

### 상속받은 속성 수정하기
### Modifying the hierarchy in-place

몇몇 속성들은 앱이 구동 중 바뀔 수도 있습니다.
이게 필요할 거에요!

```java
GenericDraweeHierarchy hierarchy = mSimpleDraweeView.getHierarchy();
```

<a name="change_placeholder"></a>
#### 대기이미지 바꾸기

resource id 로 대기이미지도 바꿀 수 있습니다.

```java
hierarchy.setPlaceholderImage(R.drawable.placeholderId);
```

아니면 멀쩡한 [Drawable](http://developer.android.com/reference/android/graphics/drawable/Drawable.html) 도 있네요.

```java
Drawable drawable;
// create your drawable
hierarchy.setPlaceholderImage(drawable);
```

#### 화면에 표시할 이미지 바꾸기

[scale type](scaling.html) 속성을 바꿀 수 있습니다.

```java
hierarchy.setActualImageScaleType(ScalingUtils.ScaleType.CENTER_INSIDE);
```

`focusCrop` 으로 scale type을 사용하려면 포커스 포인트가 필요합니다.
If you have chosen scale type `focusCrop,` you'll need to set a focus point:

```java
hierarchy.setActualImageFocusPoint(point);
```

컬러 필터도 추가할 수 있어요.

```java
ColorFilter filter;
// create your filter
hierarchy.setActualImageColorFilter(filter);
```

#### 외곽 둥글게 처리하기
라운딩 메소드를 제외한 모든 [rounding related params](rounded-corners-and-circles.html) 는 수정할 수 있습니다. 상속받은 속성의 `RoundParams` 객체로 수정할 수 있어요.
All of the [rounding related params](rounded-corners-and-circles.html), except the rounding method, can be modified. You get a `RoundingParams` object from the hierarchy, modify it, and set it back again:

```java
RoundingParams roundingParams = hierarchy.getRoundingParams();
roundingParams.setCornersRadius(10);
hierarchy.setRoundingParams(roundingParams);
```