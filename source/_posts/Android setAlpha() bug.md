---
title: Android getBackground().setAlpha引发的Bug
date: 2018-05-23 14:00:43
tags: [Android,Android Studio]
---

# 问题描述
使用getBackground().setAlpha，导致其他布局引用相同地方的背景透明度都跟着改变了，

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/id_header"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="@drawable/header_bg">
    
    </RelativeLayout>
```

我有多个地方引用到了这个布局，然后我再一个地方使用了这个方法改变了背景透明度header.getBackground().setAlpha(255)(不透明)，那么在别的地方使用到这个布局地方的背景也跟着改变了透明，这是为什么呢：默认情况下，所有的从同一资源（R.drawable.***等等)加载的实例其实是都共享的实例状态，如果你在一个地方更改了这个实例状态，那么在其余新使用的地方都会使用这个实例状态。

如何解决这种情况呢，从网上看到这个方法：

```java
/**
 * Make this drawable mutable. This operation cannot be reversed. A mutable
 * drawable is guaranteed to not share its state with any other drawable.
 * This is especially useful when you need to modify properties of drawables
 * loaded from resources. By default, all drawables instances loaded from
 * the same resource share a common state; if you modify the state of one
 * instance, all the other instances will receive the same modification.
 *
 * Calling this method on a mutable Drawable will have no effect.
 *
 * @return This drawable.
 * @see ConstantState
 * @see #getConstantState()
 */
public Drawable mutate() {
  return this;
}
```
翻译一下注释吧：让这个drawable可变，这个操作是不可逆的。一个可变Drawable可以保证不与其它的Drawable分享一个状态。当你需要修改资源中的Drawable的属性时这个方法是非常有用的，因为默认情况下加载相同资源的所有Drawable实例拥有同一个状态，如果你在一个地方改变了状态，其它的实例也会跟着改变。

所以我们在调用改变透明度的地方只需要在调用这个这个方法就好了

```java
header.getBackground().mutate().setAlpha(255);
```

