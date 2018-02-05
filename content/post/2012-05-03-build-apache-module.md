---
title:  "Apache モジュールを作る"
date: 2012-05-03T00:00:00+09:00
slug: 4feb53165b0bd4e0313037db404efb01dfaa9654
---
テンプレートで生成したものを使うのみ。特に動作を変えたりはしない。  
モジュール作りを始めるのなら、今回の場合なら mod_foo.c を編集すればいい。  

## 前提
すべて自前ビルド
* apr-1.4.6.tar.gz
* apr-util-1.4.1.tar.gz
* httpd-2.4.2.tar.gz

## 初期ファイルを生成する
モジュール名は foo

```
$ apxs -g -n foo
Creating [DIR]  foo
Creating [FILE] foo/Makefile
Creating [FILE] foo/modules.mk
Creating [FILE] foo/mod_foo.c
Creating [FILE] foo/.deps
```

foo というディレクトリが作成されている。
```
$ ls
foo
```

## モジュールをコンパイルする
apxs コマンドを使用しているけれど、make コマンドでも大丈夫。

```
$ cd foo
$ apxs -c mod_foo.c
```


もしくは

```
$ make
```


## モジュールをインストールする
作成したモジュールを Apache にインストールする。
コマンドの実行には、権限に注意する。

```
$ apxs -i mod_foo.c
```


## モジュールの設定を行う

```
$ vi httpd.conf
LoadModule foo_module modules/mod_foo.so
<Location /foo>
SetHandler foo
</Location>
```


## Apache を再起動する

```
$ apachectl restart
```


## 動作を確認する

```
$ curl -i http://localhost:5555/foo/
HTTP/1.1 200 OK
Date: Thu, 03 May 2012 06:46:25 GMT
Server: Apache/2.4.2 (Unix)
Content-Length: 31
Content-Type: text/html

The sample page from mod_foo.c
```
