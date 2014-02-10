---
layout: post
title: "Androidでメディアファイルのメタ情報を入手する"
date: 2014-02-10 16:28:20 +0900
comments: true
categories: Android
---
画像ファイル（JPEG）にはExif情報が含まれていることがありますが、音楽ファイルや動画ファイルにはExif情報は存在せず、ExifInterfaceクラスを使ってメタ情報を取り出すことができません（ExifInterfaceの使い方については[こちら](http://techbooster.jpn.org/andriod/multimedia/3098/)）。

Android2.3.3(API 10)から、`MediaMetadataRetriever`クラスが追加されました。このクラスを使って、メディアファイルのメタ情報を入手することができます。

<!-- more -->

## 使い方

 まずはMediaMetadataRetrieverのインスタンスを生成し、`MediaMetadataRetriever#setDataSource()`でファイルのソースを指定します。下記ではpathで指定していますが、URIやFileDiscriptorでも指定することができます。

```
 MediaMetadataRetriever mmr = new MediaMetadataRetriever();
 mmr.setDataSource(path);
```

 そして、`MediaMetadataRetriever#extractMetadata()`で欲しいメタ情報のKeyを指定し、入手します。例えば、動画の向き（ROTATION）を知りたい場合は下記のように入力します。

```
 String rotationAmount = mmr.extractMetadata(MediaMetadataRetriever.METADATA_KEY_VIDEO_ROTATION);
```

 メタ情報のKeyを知りたい場合は、[公式のリファレンス](http://developer.android.com/reference/android/media/MediaMetadataRetriever.html#METADATA_KEY_VIDEO_ROTATION)で確認できます。KeyによってはAPI LEVELが高いもの（14以降）もありますので、注意が必要です（上記のMETADATA_KEY_VIDEO_ROTATIONのAPI LEVELは17）。API LEVELとVERSION_CODEの対応も[公式のガイド](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)で確認できます。

 まとめてメソッドにするとこんな感じ。

```
private int getVideoRotation(String path) {
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN_MR1) {
        MediaMetadataRetriever mmr = new MediaMetadataRetriever();
        mmr.setDataSource(path);
        String rotationAmount = mmr.extractMetadata(MediaMetadataRetriever.METADATA_KEY_VIDEO_ROTATION);
        if (TextUtils.isEmpty(rotationAmount) == false) {
            return Integer.parseInt(rotationAmount);
        }
    }
    return UNKNOWN;
}
```

## リンク

* [MediaMetadataRetrieverを使ってメディアファイルのメタ情報を取得する](http://techbooster.jpn.org/andriod/multimedia/3125/)

* [MediaMetadataRetriever](http://developer.android.com/reference/android/media/MediaMetadataRetriever.html)

