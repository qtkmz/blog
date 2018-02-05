---
title: "HTTP 経由の操作にて、svn コマンドが失敗する"
date: 2012-06-09T00:00:00+09:00
slug: 110b564bfc6098ce34ef5c852d57bf0dadc52aa7
---
ソースコードからビルドしてみたけれど、svn コマンドだけ失敗する。
mod や svnadmin は、すべては試してないけれど、こちらは動作する。

失敗したときのコマンドは以下

```
$ svn co http://localhost:5555/svn-repos/abc
svn: E170000: 'http://localhost:5555/svn-repos/abc' 用の URL スキームを認識できません
```


## 調べてみると、以下のようなことらしい

[Subversionでhttp(s)のURLスキームを認識しない問題を解決する。 - Lazy Technology](http://d.hatena.ne.jp/trench/20080124/1201169696 "Subversionでhttp(s)のURLスキームを認識しない問題を解決する。 - Lazy Technology")


今回初めて知ったのだけれど、neon は http や WebDAV 用のライブラリで、SSL や GSSAPI に対応しているみたい。

[neon HTTP and WebDAV client library](http://www.webdav.org/neon/ "neon HTTP and WebDAV client library")


## neon のパッケージをインストールする

```
# yum install neon-devel
```


## 再度、configure からやり直してみる
configure の出力内容をみていると、以下のようなメッセージが。
neon library が認識されるようになった。認識されなくても、configure が失敗するわけではないので、きちんと認識されたかどうかは確認しておいた方がいいかも。

```
$ ./configure --with-apxs
...
configure: checking neon library
checking for neon-config... /usr/bin/neon-config
checking neon library version... 0.29.3
...
```


## 再度、チェックアウトしてみる

今度はうまくいっているようだ。add などをしてみたけれど、きちんと動作している。

```
$ svn co http://localhost:5555/svn-repos/abc
リビジョン 0 をチェックアウトしました。
```

