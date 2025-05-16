---
title: Azure Database for PostgreSQL フレキシブルサーバー の メンテナンスについて
date: 2023-05-30 00:00:00
tags:
  - PostgreSQL
disableDisclaimer: false
---

# Azure Database for PostgreSQL フレキシブルサーバー の メンテナンスについて

こんにちは、日本マイクロソフト サポートの岡本です。

本記事では Azure Database for PostgreSQL フレキシブルサーバー の 予定メンテナンスについて、メンテナンスの説明および影響と推奨事項を記載しております。
なお、本情報の内容（添付文書、リンク先などを含む）は、作成日時点でのものであり、予告なく変更される場合があります。


<br>

## 予定メンテナンスとは

Azure Database for PostgreSQL フレキシブルサーバーでは、管理対象のデータベースを安全で安定した最新状態に保持するために定期的なメンテナンスが実行されます。
メンテナンス中に、サーバーでは新しい機能、更新プログラム、パッチが取得されます。

メンテナンス、並びにフェールオーバーの動作に関しましては、長期間のシステムダウンを防ぎながら、Azure 基盤の状態を最新に保つため必要な動作でございます。


<br>

## メンテナンスによる影響

メンテナンスの適用によりフェールオーバーをトリガーした場合、Azure Database for PostgreSQL フレキシブルサーバーで利用されているデータベースサーバーの切り替え（ ＝内部的なフェールオーバー ）が自動的に実施されます。
この際、実行中の処理は中断され、トランザクションはロールバックされ、全ての既存の接続に瞬断が発生し切り替わりの間は新規接続も失敗いたします。
短時間で接続可能な状態になることが想定されておりますが、フェールオーバー発生時のトランザクション状況により、接続可能になるまでに時間を要する場合がございます。

そのため、対象の Azure Database for PostgreSQL フレキシブルサーバー への一時的な接続問題とメンテナンス期間が重なっている場合はメンテナンスによって発生したフェールオーバーによって接続断が発生したと判断することが可能です。

<br>

## 推奨事項

- メンテナンス期間を指定する

予定メンテナンスではカスタムスケジュールを設定することでメンテナンス期間を選択することができます。
メンテナンス期間を選択することで、ワークロード多い時間帯を避けることで予定メンテナンスによる影響を小さくすることができます。

下記のリンクでは、メンテナンススケジュールの設定方法及びイベントの通知に関してご案内がございますので、併せてご活用ください。
[Azure Database for PostgreSQL の予定メンテナンス設定の管理 - フレキシブル サーバー](https://learn.microsoft.com/ja-jp/azure/postgresql/flexible-server/how-to-maintenance-portal)

<br>

- 一時的なエラーを対応するために再試行ロジックを実装する

Azure Database for PostgreSQL フレキシブルサーバー のような PaaS のデータベースをご利用される場合、メンテナンスなどによるフェールオーバー発生時の一時的な接続断の対策として、アプリケーション側で接続を再試行するためのリトライロジックを実装いただくことを推奨しております。

下記のリンクではリトライロジックを実装する際の再試行回数や間隔等の情報について言及していますので、併せてご確認ください。
[一時的な障害の処理 - Best practices for cloud applications](https://learn.microsoft.com/ja-jp/azure/architecture/best-practices/transient-faults)

リンク先は Azure Database for PostgreSQL シングルサーバーを対象としておりますが、Azure Database for PostgreSQL フレキシブルサーバーも同様にお考えいただけますと幸いです。
[Azure Database for PostgreSQL - Singlser Server の一時的な接続エラーに対処する](https://learn.microsoft.com/ja-jp/azure/postgresql/single-server/concepts-connectivity)



<br>

上記内容がお役に立ちましたら幸いです。

<br>
