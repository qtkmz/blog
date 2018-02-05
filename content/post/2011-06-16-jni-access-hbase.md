---
title:  "C で HBase にアクセスする - JNI サンプル"
date: 2011-06-16T00:52:28+09:00
slug: 21f2323cf4d5219820ee769b52ab0bffde156027
---
これまでのまとめとして、C で Hbase にアクセスするプログラムを作成してみた。C と Java のそれぞれに Makefile を用意しているので、make を実行すれば、クラスファイルと実行ファイルは作成される。

実行ファイルの引数に CLASSPATH を指定すれば動くはず。例えばこんな感じで。CLASSPATH に指定するパスが多くて嫌になる。

```
$ LD_LIBRARY_PATH=/opt/java/jdk1.6.0_22/jre/lib/i386/client ./main -Djava.class.path=./java:/home/foo/opt/hbase-0.90.3/hbase-0.90.3.jar:/home/foo/opt/hbase-0.90.3/lib/hadoop-core-0.20-append-r1056497.jar:/home/foo/opt/hbase-0.90.3/lib/commons-logging-1.1.1.jar:/home/foo/opt/hbase-0.90.3/lib/log4j-1.2.16.jar:/home/foo/opt/hbase-0.90.3/lib/zookeeper-3.3.2.jar
```

## ソースコード

```
http://qtakamitsu-snippet.googlecode.com/svn/trunk/lang/c/jni/hbase/
http://qtakamitsu-snippet.googlecode.com/svn/trunk/lang/java/hbase/MyHTable/
```

## テストデータ
```
create 'app1', 'log1', 'log2'

put 'app1','row1', 'log1:date', '2011/06/11 10:00:00'
put 'app1','row1', 'log1:host', 'example1.com'
put 'app1','row1', 'log1:path', '/doc'
put 'app1','row1', 'log1:user', ''

put 'app1','row2', 'log1:date', '2011/06/11 11:00:00'
put 'app1','row2', 'log1:host', 'example2.com'
put 'app1','row2', 'log1:path', '/cgi-bin/printenv.cgi'
put 'app1','row2', 'log1:user', 'user01'

put 'app1','row3', 'log1:date', '2011/06/11 12:00:00'
put 'app1','row3', 'log1:host', 'example3.com'
put 'app1','row3', 'log1:path', '/'
```
