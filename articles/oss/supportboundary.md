---
title: Azure OSS PaaS サービスのサポート範囲について（PostgreSQL, MySQL, Citus） 
date: 2025-05-01 00:00:00
disableDisclaimer: false
---

# Azure OSS PaaS サービスのサポート範囲について（PostgreSQL, MySQL, Citus） 

こんにちは。Azure OSS サポートチームです。 今回の投稿では、Azure Database for PostgreSQL Flexible Server、Azure Database for MySQL Flexible Server、Azure Cosmos DB for PostgreSQL に関連するサポート範囲について、オープンソースソフトウェア（OSS）としての側面を踏まえながらご案内いたします。 お客様から多くいただく質問をベースに、すべてのお客様に対して公平かつ安定したサポートを提供するため、サポート範囲を明確にご説明します。

<br>

## OSS ベースの Azure PaaS サービスについて

まず、下記のサービスは、弊チームが担当している製品であり、いずれもオープンソースソフトウェア（OSS）をベースに構築されたPaaS のマネージドサービスです。 

・Azure Database for PostgreSQL Flexible Server 

・Azure Database for MySQL Flexible Server 

・Azure Cosmos DB for PostgreSQL  

一方、PostgreSQL や MySQL、Citus といった OSS 自体は、弊社製品ではございません。そのため、OSS 自体の仕様や挙動(例：SQL 構文の動作、オプティマイザの挙動、組み込み関数の仕様など) に関するご質問は、サポート範囲外となっております。また、Azure サポートでは各サービスに対して専任のチームが存在し、たとえば、Azure Virtual Machines や Storage に関するご質問は、それぞれの専門チームのみが対応しております。同様に、OSS 自体の仕様や挙動に関するご質問も、OSSの性質上、いかなる専任チームも存在しないため、Azure サポート全体として正式なサポート対象には含まれておりません。  

PostgreSQL、MySQL、Citus をはじめとする OSS は、ソースコードがGitHub上で公開されており、全ての方が自由に使用・開発に参加できるという特性を持っています。一方で、 OSS のライセンスにおいては、利用による不具合や結果に対して提供元が責任を負わないことが明記されています (AS IS条文)。


<br>

## [AS IS 条文]
 
・Citus OSSから抜粋 

"THERE IS NO WARRANTY FOR THE PROGRAM, TO THE EXTENT PERMITTED BY APPLICABLE LAW.  EXCEPT WHEN OTHERWISE STATED IN WRITING THE COPYRIGHT HOLDERS AND/OR OTHER PARTIES PROVIDE THE PROGRAM "AS IS" WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE." 

[Citus AS IS]([https://github.com/marcoalbarelli/PostgreSQL/blob/master/LICENSE](https://github.com/citusdata/citus/blob/main/LICENSE))

 
・MySQLから抜粋

"BECAUSE THE PROGRAM IS LICENSED FREE OF CHARGE, THERE IS NO WARRANTY FOR THE PROGRAM, TO THE EXTENT PERMITTED BY APPLICABLE LAW. EXCEPT WHEN OTHERWISE STATED IN WRITING THE COPYRIGHT HOLDERS AND/OR OTHER PARTIES PROVIDE THE PROGRAM "AS IS" WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK AS TO THE QUALITY AND PERFORMANCE OF THE PROGRAM IS WITH YOU. SHOULD THE PROGRAM PROVE DEFECTIVE, YOU ASSUME THE COST OF ALL NECESSARY SERVICING, REPAIROR CORRECTION."

[MySQL AS IS](https://github.com/mysql/mysql-server/blob/trunk/LICENSE)

 
・PostgreSQLから抜粋

"THE UNIVERSITY OF CALIFORNIA SPECIFICALLY DISCLAIMS ANY WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE. THE SOFTWARE PROVIDED HEREUNDER IS ON AN "AS IS" BASIS, AND THE UNIVERSITY OF CALIFORNIA HAS NO OBLIGATIONS TO PROVIDE MAINTENANCE, SUPPORT, UPDATES, ENHANCEMENTS, OR MODIFICATIONS."

[PostgreSQL AS IS](https://github.com/marcoalbarelli/PostgreSQL/blob/master/LICENSE)
 
例えば、弊社は、Citus OSS の開発に関与している一方、それは Citus OSS コミュニティに対する貢献であり、Citus に限らず OSS の開発には全ての人が参加できる性質から、Citus OSS の仕様や動作に関して、弊社が全ての責任を負うものではございません。上記の AS IS 条文に相当する記載は、Citus に限らずその他の OSS においても共通のものとなります。
 
従って、OSS 自体へのご質問につきましては、回答をお約束するものではありませんが、ご参考情報としてご案内を差し上げることが可能な場合がございます。その場合は、調査・回答にはお時間をいただく必要があり、またその内容についての正確性・完全性は弊社は保証いたしかねますことを、あらかじめご了承いただけますと幸いです。

<br>

## サポート範囲に含まれる内容について
 
・サーバー起動や停止に関する問題（例. サーバーが正常に起動しない、停止操作が反映されない等々） 
→ Azure がインフラ管理を行っており、起動/停止はマネージドサービスの範囲です。
 
・接続関連の問題（例. ポータル経由での接続エラー、認証情報の設定エラー等々） → 接続管理は Azure 環境下で設定されるため、Azure サポート対象です。
※弊チームはDB側の調査を行うことが可能ですが、例として、AKSから独自のDNS、ロードバランサーなど経由してAzure Database for PostgreSQL flexible serverへなど複雑なシナリオでは、クライアントやネットワーク製品の基盤部の調査であれば、別チームの対応が必要となります。また、クライアント接続設定やネットワークリソースの設定であればお客様対応範囲となります。また、異なる例として、例えばパスワードエラーであることをこちらから回答することは可能ですが、正しいパスワードをクライアントのどの箇所にどのように入れれば良いかの回答はサポート契約内で行うことができません。
 
・ネットワーク設定関連（例. VNet 統合やファイアウォール設定の問題等々） → ネットワークやファイアウォール設定は、Azure マネージド機能として提供されています。
※弊チームはDB側の調査を行うことが可能ですが、例として、AKSから独自のDNS、ロードバランサーなど経由してAzure Database for PostgreSQL flexible serverへなど複雑なシナリオでは、クライアントやネットワーク製品の基盤部の調査であれば、別チームの対応が必要となります。また、クライアント接続設定やネットワークリソースの設定であればお客様対応範囲となります。また、異なる例として、例えばパスワードエラーであることをこちらから回答することは可能ですが、正しいパスワードをクライアントのどの箇所にどのように入れれば良いかの回答はサポート契約内で行うことができません。
 
・リソース逼迫の要因調査（例. リソース使用率が高い等々） → リソースやパフォーマンスの高騰の要因がAzure側にて何か被疑箇所があるか調査可能です。
※上記の問題の切り分けの結果、アプリケーションからのワークロードを要因と判明した場合、その要因であるワークロードの軽減策の案内は、サポート契約内で行うことはできません。
 
・I/O 性能に関する問題（例. ディスクスループットが低下、書き込み速度が遅い等々） → ストレージ性能は Azure インフラに依存するため、サポート対象です。
※上記の問題の切り分けの結果、アプリケーションからのワークロードを要因と判明した場合、その要因であるワークロードの軽減策の案内は、サポート契約内で行うことはできません。
 
・リソース管理に関する問題（例. CPU やメモリ使用率が異常に高い等々） → リソース管理は、Azure のマネージド機能として対応可能です。
※上記の問題の切り分けの結果、アプリケーションからのワークロードを要因と判明した場合、その要因であるワークロードの軽減策の案内は、サポート契約内で行うことはできません。
 
・バックアップやリストア操作のエラー（例. 自動バックアップが失敗、PITR ができない等々） → バックアップ機能は Azure 側で管理されているため、サポート対象です。
 
・データ復旧操作のサポート（例. ポイントインタイムリストアが完了しない等々） → Azure マネージドサービスとしてバックアップとリストアが提供されているためです。
 
・スケーリング操作に関する問題（例. サーバーサイズ変更が反映されない等々） → スケーリング操作は、Azure ポータルや CLI から実施され、Azure 管理下にあります。
 
・高可用性（HA）関連の問題（例. フェイルオーバーが発生しない、HA 構成が反映されない等々） → Azure が管理する HA 機能であるため、サポート対象です。
 
・パラメータ設定の適用エラー（例. 設定変更が反映されない、再起動後に失敗等々） → パラメータ管理はマネージドサービス範囲です。
 
・ノードの追加や削除が反映されない（例. クラスター構成の変更が反映されない等々） → クラスター管理は Azure のマネージド機能であるため、対応可能です。
 
・クエリパフォーマンス監視ができない（例. モニタリングツールのデータが取得できない等々） → Azure Monitor との統合部分はサポート対象です。

<br>

## サポート範囲に含まれる内容について

・クエリオプティマイザの挙動に関する質問（例. 特定のクエリでインデックスが利用されない理由を知りたい等々） → クエリオプティマイザの動作は、PostgreSQL OSS 自体の設計仕様に依存しているため、Azure 側で制御できる部分ではないためサポート対象外です。
 
・拡張モジュールやプラグインの不具合に関する質問（例. pg_stat_statements が特定環境で正しく動作しない、本番環境で MySQL プラグインがクラッシュする等々） → 拡張モジュールやプラグイン自体は OSS 部分であり、Microsoft が直接管理・保証しているわけではないためサポート対象外です。
 
・内部エンジンの動作に関する技術的質問（例. VACUUM 処理中のメモリ使用量が多い理由を知りたい等々） → VACUUM やオートバキュームは、PostgreSQL OSS の内部アルゴリズムに依存し、Azure 側で制御しているわけではないためサポート対象外です。
 
・SQL 構文に関する仕様解釈の質問（例. 特定のクエリで意図した結果が得られない等々） → SQL の仕様や挙動は、PostgreSQL OSS 自体の標準機能であり、Azure が設計や実装を管理しているわけではないためサポート対象外です。
 
・InnoDB ストレージエンジンの内部挙動に関する質問（例. InnoDB が特定条件で遅くなる理由を知りたい等々） → InnoDB の挙動は、MySQL OSS 自体の仕様に依存しており、Azure 側が制御している部分ではないためサポート対象外です。
 
・バージョンアップに伴う非互換性に関する質問（例. MySQL 5.7 から 8.0 に移行した際に動作が変わった等々） → バージョンアップによる仕様変更は OSS 側で管理されているため、Azure サービスとしての制御範囲外です。
 
・SQL チューニングに関する詳細な技術サポート（例. 特定クエリのパフォーマンスが低下する原因を調査してほしい等々） → クエリチューニング自体は OSS 側の性能最適化の問題であり、Azure サービスの範囲外です。
 
・Citus 特有の関数や演算子に関する仕様解釈（例. 特定の集計クエリが分散環境で動作しない等々） → Citus OSS の拡張機能はオープンソースとして提供されているため、Azure が直接管理している部分ではないためサポート対象外です。
 
・Citus OSS バージョン固有のバグに関する質問（例. GitHub に報告されている Citus の既知バグについて詳細を教えてほしい等々） → GitHub 上で報告されたバグはOSS コミュニティで解決されるべき問題であり、Azure サポートが直接対応できる範囲外です。
 
・SQL 処理におけるパーティショニング問題（例. 特定のパーティションテーブルでクエリが遅い等々） → 分散処理やパーティショニングの挙動は Citus OSS のアルゴリズムに依存し、Azure 側で制御する範囲外です。

<br>

## 適切な問い合わせ先について(OSS に関する公式サポートリソース)
 
このご案内は、 OSS の性質および弊社サポートの提供範囲を明確にすることで、全てのお客様に対して公平で安定したサポートを提供するためのものです。ご理解のほどよろしくお願い申し上げます。OSS に関する正式で詳細な技術サポートをご希望の場合は、以下が適切なリソースとなりますため、ご検討いただけますと幸いです。

• [PostgreSQL コミュニティ](https://www.postgresql.org/support/)
• [MySQL コミュニティ](https://dev.mysql.com/support/)
• [Citus OSS](https://www.citusdata.com/support/ )

<br>

## まとめ 

Azure サービスとして提供されている部分については、Microsoft が責任を持ってサポートいたします。
一方、OSS の仕様や動作に関するご質問についてはサポート範囲外となります。 
これらの点を正しくご理解いただき、適切なサポートリソースをご活用いただけますと幸いです。 
引き続き、Azure サービスのご利用にあたりご不明な点がございましたら、サポートまでお問い合わせください。

<br>
