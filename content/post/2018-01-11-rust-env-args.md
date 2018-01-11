---
title: "Rust で引数処理"
date: 2018-01-11T23:18:32+09:00
slug: 573ebd8bf5df1c2165b1fbdd2ab52724b0868ee3
---
Rust でコマンドライン引数を処理してみる。  
std::env の動きを探ってみる。

```rust
use std::env;

fn main() {
    // for を使ってみる
    for arg in env::args() {
        println!("{}", arg);
    }

    // vec にしてみる
    let args: Vec<String> = env::args().collect();
    println!("count: {}", args.len());
    for i in 0..args.len() {
        println!("args[{}] = {}", i, args[i]);
    }

    // iterator を使ってみる。3文字より長い文字列の引数をカウントする
    let n = env::args().filter(|arg| arg.len() > 3).count();
    println!("match: {:?}", n);
}
```

以下が実行結果。

```console
$ cargo run aaaaa bbb c dddd
   Compiling env_args v0.1.0 (file:///home/tk3/devel/g/dojo/rust/sample/env_args)
    Finished dev [unoptimized + debuginfo] target(s) in 1.12 secs
     Running `target/debug/env_args aaaaa bbb c dddd`
target/debug/env_args
aaaaa
bbb
c
dddd
count: 5
args[0] = target/debug/env_args
args[1] = aaaaa
args[2] = bbb
args[3] = c
args[4] = dddd
match: 2
```


