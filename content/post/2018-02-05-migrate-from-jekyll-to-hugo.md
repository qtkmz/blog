---
title: "Jekyll のデータを Hugo のデータに変換"
date: 2018-02-05T23:01:23+09:00
slug: 915ef89790bb945306f66c2fd39cfa961ec5a01b
---
昔作ったブログのデータを変換しました。  
[libqtkmz.so](https://qtkmz.github.io/) では Jekyll を使っていたので、そのデータを変換し、今のブログにマージ。

変換は自動で行えるようスクリプトを作成。次回使う機会があるかどうかは微妙な仕様のスクリプトですが。  
機能的には以下のような処理をしています。

- layout, categories は引き継がない
- 日付は iso8601 形式に変換
- ファイル名の日付と date の日付が異なっていると警告メッセージを出力する
- slug を設定
- ファイルの拡張子は md に統一する

{{< gist qtkmz b154a232285b983aa7cabff57c7fc785 >}}


使い方は、第1引数に Jekyll データのあるディレクトリを、第2引数には変換後のデータを出力するディレクトリを指定します。  
出力するディレクトリは、なければ作ります。ファイル名の日付と date の日付が異なっている場合は警告メッセージを表示するようになっています。

```
$ ./main.rb _posts out
Not match date: _posts/2011-06-07-test-data-hbase.md : 2016-02-16T00:52:28+09:00
Not match date: _posts/2011-06-12-jni-class-instance.md : 2011-05-22T00:52:28+09:00
Not match date: _posts/2011-10-29-arakawa-line-walk-rally.md : 2011-05-22T00:52:28+09:00
Not match date: _posts/2011-12-31-test-shots-leica-x1.md : 2011-05-22T00:52:28+09:00
Not match date: _posts/2012-04-24-c-call-lua.md : 2011-05-22T00:52:28+09:00
Not match date: _posts/2016-02-28-quiver.markdown : 2016-02-29T00:00:00+09:00
```
