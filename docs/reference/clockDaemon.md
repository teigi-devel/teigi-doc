---
author: Procube Co.,Ltd.
---

# クロックデーモン

クロックデーモンによりインポートのトリガや自動更新タスクを起動することができます。

## インポート

[インポート設定](importSetting)の内容に従って、インポートを実行します。

## 自動更新バッチ

[自動更新バッチ](updateBatch)の内容に従って、バッチ処理を実行します。 自動更新バッチでは、長期間パスワードを変更していないユーザに警告メールを発信したり、有効期限切れのユーザのアカウントを一時停止したりすることができます。

## クロックデーモン設定のデータ型

クロックデーモン設定のデータ型は以下の通りです。

|名前 / 表示名|説明|データ型|修飾子|
|--------|---|----|---|
|name / 名前|クロックデーモン設定の名前|string|必須、システムID、一意|
|displayName / 表示名|クロックデーモン設定の表示名|string| |
|description / 説明文|クロックデーモン設定の説明。ヘルプの生成に使用される|string| |
|clockType / 起動周期タイプ|クロックデーモンの起動周期のタイプ。**monthly**: 毎月実行される月次バッチ。**daily**: 毎日実行される日次バッチ。**interval**: 一定期間毎に実行されるバッチ|string|必須、\[monthly, daily, interval\] のいずれか|
|day / 起動日|月次バッチにおいて、バッチが実行される日|number|起動周期タイプが monthly のときのみ有効|
|hour / 起動時間|月次バッチ、日次バッチにおいて、バッチが実行される時間|number|起動周期タイプが monthly または daily のときのみ有効|
|minutes / 起動分|月次バッチ、日次バッチにおいて、バッチが実行される分|number|起動周期タイプが monthly または daily のときのみ有効|
|intervalSeconds / 起動間隔|一定期間毎バッチにおいて、バッチが実行される間隔（秒）|number|起動周期タイプが interval のときのみ有効|
|updateBatch / 自動更新バッチ|クロックデーモンが起動された際に実行する[自動更新バッチ](updateBatch)の名前リスト|string\[\]|定義されている自動更新バッチのいずれか|
|import / インポート設定|クロックデーモンが起動された際に実行する[インポート設定](importSetting)の名前リスト|string\[\]|定義されているインポート設定のいずれか|
|provisioning / プロビジョニング設定|クロックデーモンが起動された際に実行する[プロビジョニング設定](provSetting)の名前リスト。全件出力フラグが true のプロビジョニング設定のみ選択できます。|string\[\]|定義されているプロビジョニング設定のいずれか|

## REST API パス

このオブジェクトにアクセスする場合の REST API のパスは以下の通りです。

|種別|パス|
|---|---|
|クラスの定義のパス|"/IDManager/\_classes/\_clockDaemon"|
|インスタンスのパス|システム設定に内包されているため REST パスはない|

