---
title:  JNI の "Hello, world" (C から Java のメソッドを呼ぶ)
date: 2011-05-22T00:52:28+09:00
slug: b25e853c004278d1079b786cdbdaf2013a8dcf8b
---
Java 側のメソッドを C 側から呼んでみた。手順を踏まないといけないところが面倒だ。  
引数の渡し方とか、戻り値の受け取り方とか、クラスを使った場合など、ちょっと追加で調べてみたいかも。


## ソースコード

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

    klass = (*jenv)->FindClass(jenv, "HelloWorld");
    if (klass == 0) {
        fprintf(stderr, "cannot find class\n");
        exit(1);
    }

    // public static void main (String[] args)
    mid = (*jenv)->GetStaticMethodID(jenv, klass, "main", "([Ljava/lang/String;)V");
    if (mid == 0) {
        fprintf(stderr, "cannot get method id\n");
        exit(1);
    }

    (*jenv)->CallStaticVoidMethod(jenv, klass, mid, NULL);

    (*jvm)->DestroyJavaVM(jvm);

    exit(0);
}
```

## コンパイル
```
gcc -I$JAVA_HOME/include -I$JAVA_HOME/include/linux -Wall -c sample.c
gcc -L$JAVA_HOME/jre/lib/i386/client -ljvm -o main sample.o
```

## Java 側のソースコード
```
public class HelloWorld {
    public static void main (String[] args) {
        System.out.println("Hello world");
    }
}
```

## 実行
```
LD_LIBRARY_PATH=$JAVA_HOME/jre/lib/i386/client ./main
```

## 実行結果
```
Hello world
```
