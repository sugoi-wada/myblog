---
layout: post
title: "ImageView内の画像のサイズを取得する"
date: 2014-10-25 15:56:56 +0900
comments: true
categories:  android
---

ImageViewを

```
android:layout_width="match_parent"
android:layout_height="match_prent"
```

のように縦か横もしくは両方を最大サイズで指定した場合、ImageViewの`getMeasuredWidth()`や`getMeasuredHeight()`で取得できるサイズは実際にImageView内に表示されているImageのサイズではなく、ImageViewの最大サイズが取得されてしまいます。ので、実際に表示されているImageのサイズを取得するのは一工夫必要です。

<!--more-->

まず、図で説明すると、欲しいサイズは青色で示した部分です。

![Imageview](/images/imageview/imageview.jpg)

前提として、ImageViewのサイズが確定していることが条件になります。もし、ImageViewのサイズが確定していない部分(Fragmentの起動時のコールバック群など)では、[前回の記事](http://iamemustan.com/2014/10/25/view-tree-observer/)を参考にしてください。

本題ですが、以下のメソッドで簡単に取得できます（mImageViewは適宜置き換えてください）。

```java
private RectF getImageRect() {
    // Get rectangle of the bitmap (drawable) drawn in the imageView.
    RectF imageRect = new RectF();
    imageRect.right = imageRect.left + mImageView.getDrawable().getIntrinsicWidth();
    imageRect.bottom = imageRect.top + mImageView.getDrawable().getIntrinsicHeight();

    // Translate and scale the imageRect according to the imageview's scale-type, etc.
    Matrix m = mImageView.getImageMatrix();
    m.mapRect(imageRect);
    return imageRect;
}
```

このメソッドで取得したRectFが、画面上のImageのRectFですので、`rect.width()`や`rect.height()`でサイズ取得が可能です。

ここで行っているのは、ImageViewのDrawableのサイズを取得しています。それを、ImageViewのmapRectで、マッピングさせています。

ImageViewというのは、setされたImageをそのままDrawableクラスにして保持しています。そのDrawableを画面上のImageViewのサイズに合わせて拡縮したり移動しています。それと同じことを行えば、画面上のImageのサイズがわかるというわけです。

お疲れ様でした。