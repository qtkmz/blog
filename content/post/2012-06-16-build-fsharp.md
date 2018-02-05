---
title: "F# ソースコードをビルドする"
date: 2012-06-16T00:00:00+09:00
slug: c9bd485256453450ccf0fea0a10c014e074866d7
---
github に F# のソースコードが公開されているようなので、ビルドしてみました。
先日、mono を用意したので、それを利用します。

[fsharp/fsharp](https://github.com/fsharp/fsharp "fsharp/fsharp")


## ビルドに必要なパッケージをインストール

```
# yum install autoconf
# yum install automake
```

## ソースコードを取得

```
$ git clone https://github.com/fsharp/fsharp.git
```


## ビルド

オプションをつけて、configure を実行。オプションはお好みで。
ただ、GAC ディレクトリが見つけられなかったようなので、オプションを指定。
mono 側をインストールする時に prefix をいじってなければ問題ないかも。

```
$ ./configure --prefix=$HOME/opt/fsharp-2.0 --with-gacdir=$MONO_PREFIX/lib/mono/gac
```

mono と同じく時間がかかるので注意。約20分ほどかかった。

```
$ make
$ make install
```


## 動作を確認

インタプリタを動かしてみる。実行ファイル名が配布されているものと違うので注意。

```
$ fsharpi

Microsoft (R) F# 2.0 Interactive build (private)
Copyright (c) 2002-2011 Microsoft Corporation. All Rights Reserved.

For help type #help;;

> printfn "Hello world\n";;
Hello world

val it : unit = ()
> #quit;;

- Exit...
```

おしまい

