---
layout: post
title: "gitのサブモジュールの使い方"
date: 2014-03-08 18:43:30 +0900
comments: true
categories: git
---
gitのサブモジュールの追加と削除を簡単にまとめ。

<!--more-->

なお、本記事のgitのバージョンはこちら。

```
$ git --version
git version 1.8.5.2
```

## サブモジュールを追加

```
$ git submodule add git://xxxxxxx.git DIR
```

※DIRは任意

ブランチを加える場合は-bでブランチを指定する。

```
$ git submodule add -b branch_name git://xxxxxxx.git
```

## サブモジュールを削除

```
$ git submodule deinit path/to/submodule
$ git rm path/to/submodule
$ rm -rf .git/modules/path/to/submodule
```

## サブモジュールの初期化・更新

サブモジュールを含むリポジトリをクローンしたときは、まず始めにサブモジュールの初期化が必要。

```
$ git submodule init
```

更新する場合は

```
$ git submodule update
```

とする。

## 参考

+ [git submodule 使い方](http://transitive.info/article/git/command/submodule/)
+ [gitでサブモジュールを削除する（バージョンごとの方法まとめ）](http://takaaki-kasai-tech.blogspot.jp/2014/02/how-to-remove-git-submodule-using-each-version.html)
