---
id: progress-bars
title: Progress Bars
layout: docs
permalink: /docs/progress-bars.html
prev: drawee-components.html
next: scaling.html
---

The easiest way to set a progress bar in your application is to use the [ProgressBarDrawable](../javadoc/reference/com/facebook/drawee/drawable/ProgressBarDrawable.html) class when [building a hierarchy](using-drawees-code.html):
프로그레스바를 설정하는 가장 쉬운 방법은 앱에서 [building a hierarchy](using-drawees-code.html)할 때 [ProgressBarDrawable](../javadoc/reference/com/facebook/drawee/drawable/ProgressBarDrawable.html) 클래스를 사용하면 됩니다.
```java
.setProgressBarImage(new ProgressBarDrawable())
```

This shows the progress bar as a dark blue rectangle along the bottom of the Drawee.
이것은 Drawee의 하단을 따라 어두운 파란색 네모 프로그레스바를 보여줍니다.

###당신만의 프로그레스바 만들기 Defining your own progress bar

If you wish to customize your own progress indicator, be aware that in order for it to accurately reflect progress while loading, it needs to override the [Drawable.onLevelChange](http://developer.android.com/reference/android/graphics/drawable/Drawable.html#onLevelChange\(int\)) method:
진행상태 표시자를 커스텀하고 싶다면 [Drawable.onLevelChange](http://developer.android.com/reference/android/graphics/drawable/Drawable.html#onLevelChange\(int\))를 오버라이딩 하면 됩니다. 로딩중일 때 정확하게 진행상태를 반영할 수 있도록 목적을 명확하게 하세요.
```java
class CustomProgressBar extends Drawable {
   @Override
   protected void onLevelChange(int level) {
     // level is on a scale of 0-10,000
     // where 10,000 means fully downloaded

     // your app's logic to change the drawable's
     // appearance here based on progress
   }
}
```