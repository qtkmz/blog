---
title:  "HBase をインストール (stand-alone, HBase only)"
date: 2011-06-07T00:52:28+09:00
slug: 3a55ea4c6ed58d86564193933afd24d1a0f47e3f
---

Cent OS 5.5 (32bit) に HBase 0.90.3 を入れてみる。
0.20 のときは、起動する際にパスワードを聞かれていた気がするけれど、聞かれなくなっていた。

# 0. 準備
HBase は 1.6 以降の Java が必要なため、インストールしておく。
実行するユーザーに環境変数 JAVA_HOME を設定する。

```
export JAVA_HOME=/opt/java/jdk1.6.0_22
```

## 1. HBase パッケージをダウンロードする

[[HBase - HBase Home|http://hbase.apache.org/]] から、パッケージをダウンロードする。

## 2. インストール環境を構築する
インストールしたい場所に、hbase-0.90.3.tar.gz を置き、ファイルを展開する。

```
$ tar zxf hbase-0.90.3.tar.gz
$ cd hbase-0.90.3
```

/home/hbase/opt/hbase-0.90.3/data にデータを保存するようにする。

```
$ vim conf/hbase-site.xml
<configuration>
  <property>
    <name>hbase.rootdir</name>
    <value>file:///home/hbase/opt/hbase-0.90.3/data</value>
  </property>
</configuration>
```

## 3. 起動してみる

[HBaseインストール先]/bin/start-hbase.sh にあるファイルを実行する。

<<<
$ ./bin/start-hbase.sh
starting master, logging to /home/hbase/opt/hbase-0.90.3/bin/../logs/hbase-hbase-master-centos5.5.out
>>>

## 4. 停止してみる

[HBaseインストール先]/bin/stop-hbase.sh にあるファイルを実行する。

```
$ ./bin/stop-hbase.sh
stopping hbase.....
```

## 参考
1.2. Quick Start 
http://hbase.apache.org/book/quickstart.html

