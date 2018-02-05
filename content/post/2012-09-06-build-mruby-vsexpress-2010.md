---
title: "Visual Studio 2010 Express で mruby をビルドする"
date: 2012-09-06T00:00:00+09:00
slug: 42db355fb127c53d9c4fcaecd526fd0c631a9073
---
cmake を使えば、簡単にビルドすることができる。"Visual Studio 10" が使えるみたいだが、失敗したので、nmake 方法を選択している。


## 0. ビルドするためのツールを用意

- Visual Stdio 2010 Express

- [msysgit - Git for Windows - Google Project Hosting](http://code.google.com/p/msysgit/ "msysgit - Git for Windows - Google Project Hosting")
github からソースコードを取得するため。

その他ビルド用のツールは、GnuWin を使用する。  
[GnuWin | Free Development software downloads at SourceForge.net](http://sourceforge.net/projects/gnuwin32/ "GnuWin | Free Development software downloads at SourceForge.net")  
コマンドへのパスは通しておく。

- bison-2.4.1
- libintl-0.14.4
- regex-2.7
- cmake-2.8.9-win32-x86


## 1. mruby のソースコードを取得する

```
$ git clone https://github.com/mruby/mruby.git
```


## 2. ビルドする

[Visual Studio コマンド プロンプト (2010)] を起動して、ソースを取得したディレクトリに移動する。

さらに、以下のコマンドを実行する。

```
> cd build
> cmake -G "NMake Makefiles" ..
> nmake
```


## 3. 動作を確認してみる

```
> mirb.exe
mirb - Embeddable Interactive Ruby Shell

This is a very early version, please test and report errors.
Thanks :)

> puts "Welcome to mruby!"
Welcome to mruby!
 => nil
> quit
```

とりあえずここまで。
