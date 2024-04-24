---
title: HA 構成が有効な Azure Database for PostgreSQL フレキシブル サーバーの一部の環境にて、一時表が作れない事象の対処法と修正について
date: 2024-04-24 00:00
---

※本情報の内容（添付文書、リンク先などを含む）は、作成日時点でのものであり、予告なく変更される場合があります。


Azure Database for PostgreSQL フレキシブル サーバー の一部の環境にて、一時表が作れない事象が発生しております。

上記の事象は下記の条件に全てに当てはまる Azure Database for PostgreSQL フレキシブル サーバーの一部の環境にて発生する場合があることを確認しております。

・HA 構成が有効である
・サーバー パラメーター azure.enable_temp_tablespaces_on_local_ssd が有効化されている
・サーバー作成日が 2023 年 11 月 23 日から 2024 年 3 月 24 日の期間である

上記の事象が確認された場合は、下記の対応策にて、問題の回避を行うことが可能です。

・サーバー パラメーター azure.enable_temp_tablespaces_on_local_ssd を無効化する

上記のサーバーパラメーターの無効化により、若干のパフォーマンス影響がある場合がございますが、影響の度合いにつきましてはご利用いただくワークロードに依存するため、確認を行うためには、サーバーパラメーターの無効化を検討しているサーバーを復元元とした復元を実施し、復元先でサーバーパラメーターの無効化を実施後、パフォーマンスへの影響の検証を実施されることを推奨いたします。

この度は、大変ご迷惑をおかけしております。

上記の事象の修正は今後数週間での適用を計画しており、修正の適用が完了次第、本記事でのアナウンスにおいて、更新を行う予定でございます。