---
title:  "HBase にテストデータを登録する"
date: 2011-06-07T00:52:28+09:00
slug: 3a55ea4c6ed58d86564193933afd24d1a0f47e3f
---

HBase のスタンドアローンでローカルにインストールしたので、テストデータを登録してみる。

## 実行結果
```
create 'test', 'cf'
put 'test', 'row1', 'cf:a', 'value1'
put 'test', 'row2', 'cf:b', 'value2'
put 'test', 'row3', 'cf:c', 'value3'

$ hbase shell
HBase Shell; enter 'help<RETURN>' for list of supported commands.
Type "exit<RETURN>" to leave the HBase Shell
Version 0.90.3, r1100350, Sat May  7 13:31:12 PDT 2011

hbase(main):001:0> create 'test', 'cf'
0 row(s) in 1.7890 seconds

hbase(main):002:0> put 'test', 'row1', 'cf:a', 'value1'
0 row(s) in 0.1660 seconds

hbase(main):003:0> put 'test', 'row2', 'cf:b', 'value2'
0 row(s) in 0.0120 seconds

hbase(main):004:0> put 'test', 'row3', 'cf:c', 'value3'
0 row(s) in 0.0100 seconds

hbase(main):005:0> scan 'test'
ROW                             COLUMN+CELL
 row1                           column=cf:a, timestamp=1307359903764, value=value1
 row2                           column=cf:b, timestamp=1307359907934, value=value2
 row3                           column=cf:c, timestamp=1307359912944, value=value3
3 row(s) in 0.0310 seconds

hbase(main):006:0> exit
```

## 参考
1.2. Quick Start 
http://hbase.apache.org/book/quickstart.html
