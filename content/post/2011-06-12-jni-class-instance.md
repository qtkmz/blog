---
title:  "インスタンスメソッドを扱う - JNI サンプル"
date: 2011-06-12T00:52:28+09:00
slug: 94194dd662533cb2d87b6828d0c212c78855e582
---
インスタンスを生成して、値を設定したり、取得したりしてみた。

やっぱり C から扱うといろいろと面倒な手続きが増えるなー。

## Java
```
public class Person {

    private int age;
    private String name;

    public void SetAge(int age) {
        this.age = age;
    }

    public int GetAge() {
        return this.age;
    }

    public void SetName(String name) {
        this.name = name;
    }

    public String GetName() {
        return this.name;
    }

    public static void main (String[] args) {
        Person p = new Person();

        p.SetAge(22);
        p.SetName("hoge");

        System.out.println("Persopn: " + p.GetName() + "(" + p.GetAge() + ")");
    }
}
```


## C
```

#include <stdio.h>
#include <stdlib.h>

#include "jni.h"

int main(int argc, char **argv)
{
    JNIEnv         *jenv;
    JavaVM         *jvm;
    JavaVMInitArgs jvm_args;
    JavaVMOption   jvm_options[1];
    int            ret;
    jobject        obj_person;

    jclass         person_class;
    jmethodID      person_mid_init;
    jmethodID      person_mid_set_age;
    jmethodID      person_mid_get_age;
    jmethodID      person_mid_set_name;
    jmethodID      person_mid_get_name;

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

    person_class = (*jenv)->FindClass(jenv, "Person");
    if (person_class == 0) {
        fprintf(stderr, "cannot find class\n");
        exit(1);
    }

    person_mid_init = (*jenv)->GetMethodID(jenv, person_class, "<init>", "()V");
    person_mid_set_age = (*jenv)->GetMethodID(jenv, person_class, "SetAge", "(I)V");
    person_mid_get_age = (*jenv)->GetMethodID(jenv, person_class, "GetAge", "()I");
    person_mid_set_name = (*jenv)->GetMethodID(jenv, person_class, "SetName", "(Ljava/lang/String;)V");
    person_mid_get_name = (*jenv)->GetMethodID(jenv, person_class, "GetName", "()Ljava/lang/String;");

    if (person_mid_init == 0 || person_mid_set_age == 0 || person_mid_get_age == 0 ||
        person_mid_set_name == 0 || person_mid_get_name == 0) {
        fprintf(stderr, "cannot get method id\n");
        exit(1);
    }

    obj_person = (*jenv)->NewObject(jenv, person_class, person_mid_init);
    if (obj_person == 0) {
        fprintf(stderr, "<init> failed\n");
        exit(1);
    }

    // 値を設定する
    {
        jstring js_name = (*jenv)->NewStringUTF(jenv, "hoge moge");

        (*jenv)->CallIntMethod(jenv, obj_person, person_mid_set_age, 22);
        (*jenv)->CallObjectMethod(jenv, obj_person, person_mid_set_name, js_name);

        (*jenv)->DeleteLocalRef(jenv, js_name);
    }


    {
        jint ret_age;
        jstring ret_name;
        const char *name;

        ret_age = (*jenv)->CallIntMethod(jenv, obj_person, person_mid_get_age, NULL);
        ret_name = (*jenv)->CallObjectMethod(jenv, obj_person, person_mid_get_name, NULL);

        name = (*jenv)->GetStringUTFChars(jenv, ret_name, 0);

        printf("Person = %s(%d)\n", name, ret_age);

        (*jenv)->ReleaseStringUTFChars(jenv, ret_name, name);
    }

    (*jvm)->DestroyJavaVM(jvm);

    exit(0);
}
```
