---
title:  "mruby をビルドしてみた"
date: 2012-04-22T00:00:00+09:00
slug: 4ee4a84b50f489f1b9202a56722dbc73f0ae54f3
---
手順はそのまんま。  
少し前のものでは、SIZEOF_ ST_ INDEX_T の定義がなくて失敗していたけれど、今はすんなりコンパイルできる。  
バージョン情報の表示が間違っているけれど。


## 環境
* CentOS release 6.2 (Final)
* Linux centos62 2.6.32-220.7.1.el6.x86_64


## 手順と動作確認

```
$ git clone https://github.com/mruby/mruby.git
$ cd mruby
$ make
$ cd bin
$ ./mruby -v
ruby 1.8.7 (2010-08-16 patchlevel 302) [i386-mingw32]
Usage: ./mruby [switches] programfile
  switches:
  -b           load and execute RiteBinary (mrb) file
  -c           check syntax only
  -v           print version number, then run in verbose mode
  --verbose    run in verbose mode
  --version    print the version
  --copyright  print the copyright
$ cat hello.rb
print "Hello, world\n"
$ ./mruby ./hello.rb
Hello, world
```

[Rails Hub情報局: ついに軽量Rubyの「mruby」のソースコードが公開！](http://el.jibun.atmarkit.co.jp/rails/2012/04/rubymruby-2004.html "Rails Hub情報局: ついに軽量Rubyの「mruby」のソースコードが公開！")
