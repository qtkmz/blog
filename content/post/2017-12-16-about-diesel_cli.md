---
title: "Rust の migration ツールを探して（diesel_cli 編）"
date: 2017-12-16T19:18:47+09:00
slug: a79e9a29d2d855b6954cb77130f5050dd5ed4c53
---

これはRust Advent Calendar 2017 16日目の記事です。

~~Rust でサーバーを動かしてみたく、探してみたところ[rustful](https://github.com/Ogeon/rustful) を見つけたので、使ってみました。~~
API サーバーでデータベースを扱う上で良さそうなマイグレーションツールはないかと探してみたら、diesel を利用した diesel_cli を見つけたので紹介します。
diesel_cli は diesel を利用しており、同じ人たちが開発しているようです。現在（diesel 1.0.0-beta1）は PostgreSQL や MySQL、SQLite に対応しています。


## 以下の環境で動作確認をしています
- rustc 1.22.1 (05e2e1c41 2017-11-22)
- Ubuntu 16.04.3 LTS（Windows Subsystem for Linux）


## まずは diesel_cli をインストール
cargo から diesel_cli  インストールします。

次のコマンドを実行し、コマンドをインストールします。
```
$ cargo install diesel_cli
```

この場合、PostgreSQL・MySQL・SQLite それぞれの開発パッケージが必要になります。PostgreSQL は libpq-dev , MySQL は libmysqlclient-dev, SQLite は libsqlite3-dev が必要になります。

もし、それらのどれかで十分なら、引数で必要なデータベースを指定することができます。
```
$ cargo install diesel_cli --no-default-features --features "mysql"
```

# diesel_cli を使う、プロジェクトを作成する
diesel_cli は Cargo 配下のプロジェクトで実行します。そのため、cargo でプロジェクトを作成します。

```
$ cargo new diesel_migration
```

# セットアップ
次のようにコマンドを実行します。引数にデータベースの接続情報を指定します。
データベースの種別や接続ユーザー、そのパスワード、ホスト名、データベース名を指定します。実行すると、データベースが作成され、プロジェクト直下に `migrations` ディレクトリが作成されます。マイグレーションのSQLは、この `migrations` ディレクトリ以下に保存していくことになります。


```
$ diesel setup --database-url='mysql://testuser:dbpass@localhost/deisel_demo'
Creating migrations directory at: /home/foo/diesel_migration/migrations
Creating database: deisel_demo
$ ls .
Cargo.toml  migrations  src
```

ちなみに、作成されたデータベースをのぞいてみると、`__diesel_schema_migrations` という管理用のテーブルが作成されています。

```mysql
mysql> show tables;
+----------------------------+
| Tables_in_deisel_demo      |
+----------------------------+
| __diesel_schema_migrations |
+----------------------------+
1 row in set (0.00 sec)
mysql> show create table __diesel_schema_migrations \G
*************************** 1. row ***************************
       Table: __diesel_schema_migrations
Create Table: CREATE TABLE `__diesel_schema_migrations` (
  `version` varchar(50) NOT NULL,
  `run_on` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`version`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1
1 row in set (0.00 sec)
```

# マイグレーションのSQLを作成する
`diesel` コマンドで空のマイグレーションのSQLファイルを作成することができます。

次のようにコマンドを実行します。第3引数には短いコメントのような扱いになる文字列を指定します。実行すると、`up.sql` と `down.sql` が生成されます。 `diesel_cli` からロールバックすることができ、その際に実行するSQLを定義します。`test` というテーブルを作るというマイグレーションの場合、`up.sql` には `test` テーブルを作成するSQLを書き、`down.sql` には `test` テーブルを削除する SQL を書きます。 

```
$ diesel migration generate create_test_table
Creating migrations/2017-12-16-061537_create_test_table/up.sql
Creating migrations/2017-12-16-061537_create_test_table/down.sql
```

```migrations/2017-12-16-061537_create_test_table/up.sql
CREATE TABLE test (
  title VARCHAR(8)  NOT NULL
);
```

```migrations/2017-12-16-061537_create_test_table/down.sql
DROP TABLE test;
```

# マイグレーションのSQLを反映する

次のコマンドで作成したマイグレーションのSQLを反映します。
実行すると `test` テーブルが作成されていることが確認できます。

```
$ diesel migration run --database-url='mysql://testuser:dbpass@localhost/deisel_demo'
Running migration 20171216061537
2017/12/16 15:37:22@tk3 $ mysql -u testuser -D deisel_demo -p -e 'show tables;'
Enter password:
+----------------------------+
| Tables_in_deisel_demo      |
+----------------------------+
| __diesel_schema_migrations |
| test                       |
+----------------------------+
```




# `--database-url=` が面倒な場合
`--database-url` に設定している内容を `.env` ファイルに記述することで引数を省略することができます。

```
$ echo DATABASE_URL=mysql://testuser:dbpass@localhost/deisel_demo > .env
```


# まとめ
diesel を使わなくても diesel_cli でマイグレーションを行えることはできました。

