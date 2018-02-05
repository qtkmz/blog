---
title:  "C から Ruby を呼び出す"
date: 2011-07-06T00:52:28+09:00
slug: 3079902fac39a69902ea99ba27137ff1bc7bff04
---
C から Ruby を呼び、Hello World を出力するプログラムを書いてみた。  
必ず必要なのは、初期化処理と終了処理。あとは、Ruby を呼び出す処理。Java を呼び出すより簡単。

## バージョン
```
ruby 1.9.2p180 (2011-02-18 revision 30909) [i686-linux]
```

## ソースコード

```
#include "ruby.h"

int main(int argc, char *argv[])
{
    ruby_init();
    ruby_init_loadpath();

    rb_eval_string("puts 'Hello World'");

    return ruby_cleanup(0);
}
```


## Makefile
ruby.h や関連のヘッダーファイルへのパスを通してあげるのと、ライブラリのリンクを設定してあげること。恐らく Ruby をコンパイルした環境によって違うと思う。

```
RUBY_HOME = /home/foo/opt/ruby-1.9.2

CC       = gcc
SOURCES  = sample.c
OBJS     = $(SOURCES:.c=.o)
CFLAGS   = -I$(RUBY_HOME)/include/ruby-1.9.1 \
           -I$(RUBY_HOME)/include/ruby-1.9.1/i686-linux
LDFLAGS  =
LIBS     = -lpthread -lm -ldl -lrt -lcrypt -L$(RUBY_HOME)/lib -lruby-static
TARGET   = main

$(TARGET): $(OBJS)
        $(CC) $(LDFLAGS) -o $@ $(OBJS) $(LIBS)

.c.o:
        $(CC) $(CFLAGS) -c $<

clean:
        rm -f $(TARGET) $(OBJS)
```

## Rakefile
Ruby らしく、上記の Makefile を Rakefile に書き直してみた。

```
require 'rake/clean'
require 'rbconfig'

APP_NAME = "main"

CC       = Config::CONFIG["CC"]
SRCS     = FileList["sample.c"]
INCLUDES = "-I#{Config::CONFIG["rubyhdrdir"]} -I#{Config::CONFIG["rubyhdrdir"]}/#{Config::CONFIG["arch"]}"
LIBS     = "#{Config::CONFIG["LIBS"]} #{Config::CONFIG["LIBRUBYARG_STATIC"]}"
OBJS     = SRCS.ext('o')

task :default => APP_NAME

CLEAN.include(OBJS)
CLOBBER.include(APP_NAME)

file APP_NAME => OBJS do |t|
  sh "#{CC} -o #{t.name} #{t.prerequisites.join(' ')} #{LIBS}"
end

rule '.o' => '.c' do |t|
  sh "#{CC} #{INCLUDES} -c -o #{t.name} #{t.source}"
end
```
