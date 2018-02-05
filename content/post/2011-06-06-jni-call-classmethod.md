---
title:  "クラスメソッドを呼ぶ - JNI サンプル"
date: 2011-06-06T00:52:28+09:00
slug: afb8e800d24ba738e8c86c2cc0a8fbab1e3cf8fd
---

Java で定義したクラスメソッドを C から、呼んでみる。

## Java のソースコード
```
$ cat ClassMethodSample.java
public class ClassMethodSample {
    public static void main (String[] args) {
        System.out.println("main");
    }

    public static void SayHello() {
        System.out.println("Java> Hello world.");
    }
}
```

## C 言語のソースコード
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#include "jni.h"

int main(int argc, char **argv)
{
    JNIEnv         *jenv;
    JavaVM         *jvm;
    JavaVMInitArgs jvm_args;
    JavaVMOption   jvm_options[1];
    jclass         klass;
    jmethodID      mid;
    int            ret;

    jvm_options[0].optionString = "-Djava.class.path=./java";

    jvm_args.version            = JNI_VERSION_1_6;
    jvm_args.options            = jvm_options;
    jvm_args.nOptions           = 1;
    jvm_args.ignoreUnrecognized = 1;

    ret = JNI_CreateJavaVM(&jvm, (void**)&jenv, &jvm_args);
    if (ret < 0) {
        fprintf(stderr, "cannot create JVM\n");
        exit(1);
    }

    klass = (*jenv)->FindClass(jenv, "ClassMethodSample");
    if (klass == 0) {
        fprintf(stderr, "cannot find class\n");
        exit(1);
    }

    // public static void say_hello()
    mid = (*jenv)->GetStaticMethodID(jenv, klass, "SayHello", "()V");
    if (mid == 0) {
        fprintf(stderr, "cannot get method id\n");
        exit(1);
    }

    (*jenv)->CallStaticVoidMethod(jenv, klass, mid, NULL);

    (*jvm)->DestroyJavaVM(jvm);

    exit(0);
}
```

## 実行結果
```
Java> Hello world.
```
