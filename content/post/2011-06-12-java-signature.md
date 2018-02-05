---
title:  "シグネチャを調べる"
date: 2011-06-12T00:52:28+09:00
slug: 94194dd662533cb2d87b6828d0c212c78855e582
---
JNI を扱うとき、メソッドのシグネチャを指定しなければならないが、簡単にコマンドで調べられることがわかった。これで、一括で抜き出したい時などに楽ができそう。

```
$ javap -s Person
Compiled from "Person.java"
public class Person extends java.lang.Object{
public Person();
  Signature: ()V
public void SetAge(int);
  Signature: (I)V
public int GetAge();
  Signature: ()I
public void SetName(java.lang.String);
  Signature: (Ljava/lang/String;)V
public java.lang.String GetName();
  Signature: ()Ljava/lang/String;
public static void main(java.lang.String[]);
  Signature: ([Ljava/lang/String;)V
}
```
