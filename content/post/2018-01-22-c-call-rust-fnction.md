---
title: "C から Rust の関数をコールする"
date: 2018-01-22T20:21:21+09:00
slug: 230b9f23b3a3baf3bf46e7fa901a498dcc340931
---

Rust に関数を実装し、それを C から呼び出します。


### あまり引数や戻り値を考えなくても良さそうな関数を Rust で定義する

```rust
#![crate_type = "dylib"]

#[no_mangle]
pub extern fn sayGreeting() {
   println!("Hello, world.");
}

#[no_mangle]
pub extern fn rust_add(x: i32, y: i32) -> i32 { x + y }
```

### Rust のコードをコンパイルする

```
$ rustc rustcode.rs
$ ls .
librustcode.so rustcode.rs
```

### Rust の関数をコールする C のコードを書く

```c
#include <stdio.h>

void sayGreeting(void);
int rust_add(int x, int y);

int main(int argc, char **argv)
{
    sayGreeting();

    printf("result: %d\n", rust_add(10, 15));

    return 0;
}
```

### C のコードをコンパイルする

```
$ gcc -o main sample.c -L. -lrustcode
```

### 実行する

```
$ LD_LIBRARY_PATH=./ ./main
Hello, world.
result: 25
```

お試しということで、簡単なコードで動作確認をしてみました。  
次は文字列型とかどういう感じになるんだか、気になるところ。

