---
title: TabLayout 在宽屏幕或者搞分辨率上不能平均分配的问题
tags: Android
---

## 一、问题描述

当TabLayout 在宽屏幕或者高分辨的设备上，如平板横屏的时候，tab的宽度超过一定值后，就不在平均分配宽度，而是居中显示的问题。此时设置
app:tabMode="fixed"或者top_table.setTabMode(TabLayout.MODE_FIXED);不在起作用。

## 二、解决办法
设置当TabLayout 的属性，app:tabMaxWidth="0dp" 此值即可解决，如下

<android.support.design.widget.TabLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:tabMaxWidth="0dp"
            app:tabGravity="fill"
            app:tabMode="fixed" />


