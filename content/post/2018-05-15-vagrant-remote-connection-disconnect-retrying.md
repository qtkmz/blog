---
title: "vagrant up で 「default: Warning: Remote connection disconnect. Retrying...」が出力し続け、失敗する"
date: 2018-05-15T21:12:34+09:00
slug: ea944285ce773cbe9be046319f26f6792e4e7c0d
---

Ubuntu 18.04 LTS がリリースされたので、新しく開発環境を用意しようと、久しぶりに Virtual Box や Vagrant を更新したら、ちょっとした問題が起きたので、状況ととりあえずの対応策を残しておく。

## 事象
`vagrant up` を実行したときに `default: Warning: Remote connection disconnect. Retrying...` が出力され続け、タイムアウトを起こす。VM の起動は成功している。

```
PS C:\Users\tk3\vms\ubuntu-18.04-lts> vagrant up
...
    default: Warning: Remote connection disconnect. Retrying...
    default: Warning: Remote connection disconnect. Retrying...
    default: Warning: Remote connection disconnect. Retrying...
    default: Warning: Remote connection disconnect. Retrying...
    default: Warning: Remote connection disconnect. Retrying...
    default: Warning: Remote connection disconnect. Retrying...
    default: Warning: Remote connection disconnect. Retrying...
Timed out while waiting for the machine to boot. This means that
Vagrant was unable to communicate with the guest machine within
the configured ("config.vm.boot_timeout" value) time period.

If you look above, you should be able to see the error(s) that
Vagrant had when attempting to connect to the machine. These errors
are usually good hints as to what may be wrong.

If you're using a custom box, make sure that networking is properly
working and you're able to connect to the machine. It is a common
problem that networking isn't setup properly in these boxes.
Verify that authentication configurations are also setup properly,
as well.

If the box appears to be booting properly, you may want to increase
the timeout ("config.vm.boot_timeout") value.
```


## 対応

`vagrant up` 実行後に生成される `private_key` ファイルが読み込めずに失敗していることが分かった。  


以下のようなユーザーに権限が付与されているが、実行しているユーザーとは異なっている。  
とりあえず動かすには `private_key` が生成されたら、このファイルのアクセス権限に実行しているユーザーを追加してあげればいい。
<a href="http://f.hatena.ne.jp/qtakamitsu/20180515205532"><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/q/qtakamitsu/20180515/20180515205532.png" alt="20180515205532"></a>


ちなみに、実行しているユーザーのSIDが `S-1-5-21-2806389219-2107435262-1618673610-1001` で、今回追加されていたユーザーのSIDは `S-1-5-21-2806389219-2107435262-1618673610` 。  
末尾に `-1001` がついているかいないかの違い。この差がなんなのかは、もう少し見ていく必要がありそう。  
アクセス権限を変更すれば問題ないので、とりあえずここまで。


```
PS C:\Users\tk3> PsGetsid -nobanner \\tk3
SID for \\tk3:
S-1-5-21-2806389219-2107435262-1618673610

PS C:\Users\tk3> PsGetsid -nobanner tk3\tk3
SID for tk3\tk3:
S-1-5-21-2806389219-2107435262-1618673610-1001

```

今回の調査に使った PsGetsid は [こちら](https://docs.microsoft.com/en-us/sysinternals/downloads/psgetsid) からダウンロードできます。


