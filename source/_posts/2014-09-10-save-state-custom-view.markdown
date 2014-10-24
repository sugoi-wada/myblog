---
layout: post
title: "カスタムViewの状態をView自身で保存する"
date: 2014-09-10 19:41:53 +0900
comments: true
categories: Android
---

画面回転したときにViewの状態が戻ってしまいます。それを阻止するために、ActivityやFragmentのコールバックで値を保存する方法があるのですが、カスタムビューはカスタムビュー自身に保存させたいですよね。その方法を紹介します。

<!--more-->

まずはカスタムビュー内のインナークラスとして`BasedSavedState`を継承したクラスを作成します。メンバ変数には保存したい変数を定義します。Paracebleが実装されたオブジェクトかプリミティブ型ならおっけーです。

```java
    private static class SavedState extends BaseSavedState {
        int something;	//保存したいものを変数定義する（ParacebleオブジェクトならOK）

        public SavedState(final Parcelable superState) {
            super(superState);
        }

        public SavedState(final Parcel in) {
            super(in);
            something = in.readInt(); //復帰
        }

        @Override public void writeToParcel(final Parcel out, final int flags) {
            super.writeToParcel(out, flags);
            out.writeInt(something); //保存
        }

        @SuppressWarnings("hiding")
        public static final Parcelable.Creator<SavedState> CREATOR = new Parcelable.Creator<SavedState>() {
            public SavedState createFromParcel(Parcel in) {
                return new SavedState(in);
            }

            public SavedState[] newArray(int size) {
                return new SavedState[size];
            }
        };
    }
}
```

カスタムビュー内に保存と復帰のコールバックメソッドを定義します。

```java
    private SavedState mSavedState;

    @Override protected Parcelable onSaveInstanceState() {
        Parcelable parent = super.onSaveInstanceState();
        SavedState ss = new SavedState(parent);
        ss.something = <保存したい値>;

        return ss;
    }

    @Override protected void onRestoreInstanceState(final Parcelable state) {
        if (!(state instanceof SavedState)) {
            super.onRestoreInstanceState(state);
            return;
        }

        SavedState ss = (SavedState) state;
        super.onRestoreInstanceState(ss.getSuperState());
        mSavedState = ss;
    }
```
あとは保存した値を使うときに`mSavedState`がnullじゃない場合に使えばおっけーです。
ありがとうございました。
