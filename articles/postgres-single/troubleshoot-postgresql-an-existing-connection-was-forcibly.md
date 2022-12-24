---
title: 'An existing connection forcibly closed by the remote host' について
date: 2022-12-24 12:00:00
tags:
  - PostgreSQL
disableDisclaimer: false
---

# 'An existing connection was forcibly closed by the remote host' について

※本記事は [Troubleshoot PostgreSQL: ‘An existing connection was forcibly closed by the remote host’](https://techcommunity.microsoft.com/t5/azure-database-for-postgresql/troubleshoot-postgresql-an-existing-connection-was-forcibly/ba-p/925164) の抄訳となります。


PostgreSQLのログに "could not receive data from client. "のようなエラーが表示された後、すぐに次のようなエラーが発生します。
>'An existing connection was forcibly closed by the remote host' 

この2つのエラーは相互に関連しており、クライアント側の接続処理の問題と関連していることがよくあります。

 

このようなタイプのエラーは、さまざまなコンポーネントが関与しているため、残念ながらトラブルシューティングが難しい場合があります。
このブログ記事では、このエラーが発生する原因と、それを回避するためのベストプラクティスについて説明します。

 

## 既に無効になった接続

クライアントアプリケーションが、コネクションプールから取得した接続を使用してクエリを実行しようとした場合を考えます。アプリケーションでその接続オブジェクトを使用しようとすると、接続が古くなっており既に無効になっていた場合、クライアントアプリケーションは開いている接続を介してクエリーデータを送信しようとし、例外が発生します。 クライアント側のアプリケーションが例外を捕捉し、TCP 接続が閉じられます。PostgreSQL サーバーはクライアント側の接続が閉じられたことを検出し、以下のようなエラーが返されます。
>WSAECONNRESET (10054) Connection reset by peer: An existing connection was forcibly closed by the remote host.
ここでいう「remote host(リモートホスト)」とは、Postgresサーバーではなくクライアントのことです。

 
### 詳細について
何が問題を引き起こしているかを正確に理解するためには、問題が発生したときにクライアント側でネットワークトレースをキャプチャすることが有効です。ネットワークトレースは、NetMon、WireShark、Fiddlerなどの様々なアプリケーションでキャプチャすることができます。


Linuxクライアントを使用していて、上記のツールが動作しない場合、以下のコマンドのように手動でネットワークダンプを生成することができます。


>tshark -i any -n -b filesize:204800 -w `date +%y%m%d-%H:%M:%S`.pcap -b files:1000

または

>tcpdump -i any -w `date +%y%m%d-%H:%M:%S`.pcap -G 300 -W 1000


注：ネットワークキャプチャを有効にした場合、ディスク容量を監視し、容量不足にならないように必要に応じてファイルを削除してください。

 

 

## 解決方法

- Pgbouncerの使用
pgBouncerは、アプリケーションとデータベースの間に位置するコネクションプーラーです。データベースへの新しい接続が必要な場合、アプリケーションはコネクションプールから接続を取得し、それを再利用し続けます。一定の時間が経過すると、接続は解放されます。つまり、アプリケーションがデータベースへの接続を取得した後、一定時間以上使用しない場合、実際にはアイドル接続として扱われ、再利用されるため新たな接続が消費されません。
詳細については、以下でもご説明していますのでご参照ください
[Not all Postgres connection pooling is equal](https://techcommunity.microsoft.com/t5/azure-database-for-postgresql/not-all-postgres-connection-pooling-is-equal/ba-p/825717)

- 過渡的なエラーを処理するために再試行ロジックを実装する
クラウド上でアプリケーションを設計・開発する場合、一過性のエラーが発生する可能性があるため、ベストプラクティスとしてリトライロジックを実装します。この場合、クライアントクエリを再試行することで、使用可能な接続を見つけることができます。詳しくはこちらをご覧ください。
[Azure Database for PostgreSQL - Singlser Server の一時的な接続エラーに対処する](https://learn.microsoft.com/ja-jp/azure/postgresql/single-server/concepts-connectivity)
 

- アイドル状態の接続を最小限にする
接続の管理は、PostgreSQLのユーザとの会話でよく出てくる話題です。PostgreSQLの接続はコストがかかります。アイドルでもアクティブでも、それぞれの接続は一定のメモリを消費します（1接続あたり10MB程度）。アイドル状態の接続とは、アプリケーションから接続を取得し、それを使用しないまま保持するものです。アプリケーションのコネクションプーラも、1つまたは複数のアイドル接続を消費することがあります。詳細については、以下を参照してください。
[Connection handling best practice with PostgreSQL](https://techcommunity.microsoft.com/t5/azure-database-for-postgresql/connection-handling-best-practice-with-postgresql/ba-p/790883)

statement_timeout と idle_in_transaction_session_timeout を適切に設定する方法については、以下を参照してください。
[Tracking and Managing Your Postgres Connection](https://www.craigkerstiens.com/2017/09/18/postgres-connection-management/)

- アプリケーションからKeep-alive を送信する
接続が一定時間アクティブでない状況で、プールされた接続がアイドルのためタイムアウトしないようにすることができます。

- アプリケーションのリソース使用率をチェックする
クライアント側でリソースの負荷が高い（CPUが高い、IOPSが高い、コンテキストスイッチが高い）と、速度が低下することがあります。この遅れにより、接続が長時間にわたって開かれ、最終的に接続が切断される可能性があります。
