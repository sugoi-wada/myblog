---
layout: post
title: "FragmentでViewのサイズを取得する"
date: 2014-10-25 14:34:54 +0900
comments: true
categories: Android
---

FragmentのonCreate()やonResume()などの起動時のコールバックでviewのサイズを取得しようとしてview.getMeasuredWidth()などを実行しても必ず0が返却されます。これは、まだその時点でViewのサイズが確定していないからです。

<!--more-->

そこで以下のように書きます。（mImageViewは適宜置き換えてください）

```java
ViewTreeObserver viewTreeObserver = mImageView.getViewTreeObserver();
viewTreeObserver.addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {
    @Override public void onGlobalLayout() {
        if (Build.VERSION.SDK_INT < Build.VERSION_CODES.JELLY_BEAN) {
            mImageView.getViewTreeObserver().removeGlobalOnLayoutListener(this);
        } else {
            mImageView.getViewTreeObserver().removeOnGlobalLayoutListener(this);
        }
        //ここにサイズ計測後に必要な処理を書く
        Log.d(SampleFragment.class.getName(),"w = " + mImageView.getMeasuredWidth() + " h = " + mImageView.getMeasuredHeight());
    }
});
```

計測したいViewに対して`getViewTreeObserver()`を実行し、`addOnGlobalLayoutListener()`でサイズが変更されたときのコールバック処理を書きます。

このコールバックはそのViewのサイズが変わる度に呼び出されますので、最初に一回だけ実行する場合（動的にサイズが変わったりしない場合）は、コールバック内の最初にすぐ`removeOnGlobalLayoutListener()`（JELLY_BEAN未満の端末は`removeGlobalLayoutListener()`）を呼んで、解除しておきます。

ちなみにこの方法は、Fragment以外でも、どの場所でも有効ですので、Viewのサイズが決まったら〇〇したいという場合に役に立ちます。

## 別の方法

ViewTreeObserver以外の方法でも実現できます。Handlerを使った方法です。UIThreadで処理を行いたいときに利用することがあったりしますが、Viewのサイズが確定してから呼ばれるため、ここでもサイズ確定後の処理を書くことができます（mImaveViewは適宜置き換えてください。また、こちらもonVewCreated()以外の場所でも問題なく動きます）。

```java
private Handler mHandler = new Handler(Looper.getMainLooper());

@Override public void onViewCreated(final View view, final Bundle savedInstanceState) {
    // ...
    mHandler.post(new Runnable() {
        @Override public void run() {
            //ここにサイズ計測後に必要な処理を書く
            Log.d(SampleFragment2.class.getName(), "w = " + mImageView.getMeasuredWidth() + " h = " + mImageView.getMeasuredHeight());
        }
    });
}
```

AndroidAnnotationsのフレームワークを使用している場合は、`@UIThread`と書いたメソッド内では、Handlerを用いた処理が行われているので、同様のことを行うことができます。

## サンプルコード

https://github.com/sugoi-wada/GetViewSizeSample

以上です。お疲れ様でした。
