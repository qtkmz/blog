---
title:  "Mono ソースコードをビルドする"
date: 2012-06-03T00:00:00+09:00
slug: f1f67d5e857bade714ebf8411d84267d5da76050
---
Linux 上で .NET を動かしてみたいということで、mono をインストールしてみた。
他にもいろいろとライブラリをインストールするようだけれど、既に導入済みなのか、単体でも動作した。

ただ、ビルドに時間がかかる。make だけで約20分かかる。


## ソースコードをビルドする

```
$ tar jxf mono-2.11.1.tar.bz2
$ cd mono-2.11.1
$ ./configure --prefix=$HOME/opt/mono-2.11.1
$ make
$ make install
```


## mono の環境設定を行う

### ~/.bashrc

```
export MONO_PREFIX=$HOME/opt/mono-2.11.1
export DYLD_LIBRARY_FALLBACK_PATH=$MONO_PREFIX/lib:$DYLD_LIBRARY_FALLBACK_PATH
export LD_LIBRARY_PATH=$MONO_PREFIX/lib:$LD_LIBRARY_PATH
PATH=$MONO_PREFIX/bin:$PATH
$ . ~/.bashrc
```


## mono のバージョンを確認する

```
$ mono --version
Mono JIT compiler version 2.11.1 (tarball 2012年  6月  2日 土曜日 18:31:54 JST)
Copyright (C) 2002-2012 Novell, Inc, Xamarin Inc and Contributors. www.mono-project.com
        TLS:           __thread
        SIGSEGV:       altstack
        Notifications: epoll
        Architecture:  amd64
        Disabled:      none
        Misc:          softdebug
        LLVM:          supported, not enabled.
        GC:            Included Boehm (with typed GC and Parallel Mark)
```

おしまい

