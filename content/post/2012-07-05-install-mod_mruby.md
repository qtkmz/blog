---
title: "mod_mruby をインストール"
date: 2012-07-05T00:00:00+09:00
slug: d651e28a34914194010faf90df07fd2aa6e0054d
---
## 0. 環境

- CentOS release 6.2 (Final) / x86_64
- httpd-2.4.2 (自前ビルド)


## 1. mruby をビルド

```
$ git clone https://github.com/mruby/mruby.git
$ cd mruby
$ make CFLAGS="-g -O3 -fPIC"
```

## 2. json-c をビルド & インストール

```
$ git clone https://github.com/matsumoto-r/json-c.git
$ cd json-c
$ git checkout json-c-0.10
$ ./configure --prefix=$HOME/opt/json-c
$ make
$ make install
```

make install では json_object _iterator.h だけがインストールされないので、手動でインストールする。
もしくは、Makefile の libjsoninclude_HEADERS に追加して make install を実行する。


### 2.1 おまけ作業 - pkgconfig のパス設定
prefix を指定した場合、pc ファイルが参照されないので、設定を行う。

環境変数を設定

#### .bashrc
```
export PKG_CONFIG_PATH=$HOME/opt/json-c/lib/pkgconfig

$ . ~/.bashrc
```

動作確認
```
$ pkg-config --libs json
-L/home/foo/opt/json-c/lib -ljson
$ pkg-config --cflags json
-I/home/foo/opt/json-c/include/json
```


##3. mod_mruby をビルド & インストール

自分の環境に合わせて、make 時に、apxs コマンドのパスmruby のパスjson-c のパスの 3 つを指定する。

```
$ git clone https://github.com/matsumoto-r/mod_mruby.git
$ cd mod_mruby
$ make install \
MRUBY_PREFIX='$(HOME)/build/mruby'  \
APXS='$(shell which apxs)'  \
INC='-I. -I$(MRUBY_PREFIX)/src -I$(MRUBY_PREFIX)/include $(shell pkg-config --cflags json)' \
LIB='-lm $(MRUBY_PREFIX)/lib/libmruby.a -lm $(MRUBY_PREFIX)/mrblib/mrblib.o -lm $(shell pkg-config --libs json)'
...省略...
[activating module `mruby' in /home/foo/opt/apache-2.4.2/conf/httpd.conf]
```

インストールはこれで、おしまい。

