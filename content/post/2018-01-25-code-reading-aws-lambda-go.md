---
title: "ソースコードリーディング: aws-lambda-go"
date: 2018-01-25T00:21:26+09:00
slug: 2b66b03cefc2db86b5adaa77ab673ea23d7cc4e1
---
おおまかな動きを知るために、ざっとコードリーディング。  

- ソースコードリーディング [aws/aws-lambda-go](https://github.com/aws/aws-lambda-go)
  - 正式に AWS Lambda で Go 言語がサポートされた。同時に、Go 言語で使うためのライブラリも GitHub に公開された。そのライブラリが https://github.com/aws/aws-lambda-go
  - このライブラリがどのようなものかを知るために、このソースコードを読んでみる。
  - 全体の流れとしては、aws/aws-lambda-go を import し、 自作の関数を作り、lambda.Start() に関数を渡す。これをコンパイルして、実行ファイルを zip に圧縮し、Lambda にデプロイするという手順となっている。
  - lambda.Start() に登録する関数を、ユーザー定義関数とする
  - GOOS=linux との指定から、実行ファイルは Linux の 64bit
    - 以下に `Note that only 64-bit binaries are supported on AWS Lambda.` と記述があるから、32bitではない
      - Ref. Lambda Execution Environment and Available Libraries - AWS Lambda https://docs.aws.amazon.com/lambda/latest/dg/current-supported-versions.html
  - AWS Lambda からの呼び出し
    - AWS Lambda はzipから実行ファイルを取り出し、実行しているだけ？
  - Using Global State to Maximize Performance より、init() が使える
    - [Lambda Function Handler (Go) - AWS Lambda](https://docs.aws.amazon.com/ja_jp/lambda/latest/dg/go-programming-model-handler-types.html)
  - lambda.Start() に登録できる関数の型は以下の通り
    - コメントに書いてある https://github.com/aws/aws-lambda-go/blob/master/lambda/entry.go#L25-L33
      - func ()
      - func () error
      - func (TIn) error
      - func () (TOut, error)
      - func (TIn) (TOut, error)
      - func (context.Context) error
      - func (context.Context, TIn) error
      - func (context.Context) (TOut, error)
      - func (context.Context, TIn) (TOut, error)
  - lambda.Start() でやっていること
    - ユーザー定義関数の処理をする RPC 通信をする TCP サーバーを起動している
      - ポート番号は環境変数 _LAMBDA_SERVER_PORT で決まる。なしはデフォルト
      - 実行ファイルを実行すると、RPCサーバーとして起動する。ローカルでも実行可能
      - この RPC サーバーには、ユーザー定義関数と Ping() が実装される
        - Ping() は https://github.com/aws/aws-lambda-go/blob/master/lambda/function.go#L19-L22
        - PingResponse を返すだけの関数
        - PingResponse は https://github.com/aws/aws-lambda-go/blob/master/lambda/messages/messages.go#L8-L9
      - 仕組みとしては コンテナで RPC サーバーが起動し、外部とやり取りをしている
  - 対応しているイベント
    - 場所 https://github.com/aws/aws-lambda-go/tree/master/events
    - 一覧
      - API Gateway
      - Cognito Events
      - Config Events
      - DynamoDB Events
      - Kinesis Events
      - Kinesis Firehose Events
      - S3 Events
      - SNS Events

