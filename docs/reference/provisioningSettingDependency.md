---
author: Procube Co.,Ltd.
---

# プロビジョニング依存設定

プロビジョニング依存設定はプロビジョニング設定によって生成されるタスク間の依存関係を定義します。

## データ型

プロビジョニング依存設定の属性は以下の通りです。

|名前 / 表示名|説明|データ型|修飾子|
|--------|---|----|---|
|name / プロビジョニング設定の名前|プロビジョニング設定の名前|string|必須、プロビジョニング設定の名前のいずれか|
|taskType / 操作命令の分類|操作命令の分類|string|\[追加・更新, 削除\] のどちらか|

## REST API パス

このオブジェクトにアクセスする場合の REST API のパスは以下の通りです。

|種別|パス|
|---|---|
|クラスの定義のパス|"/IDManager/\_classes/\_provisioningSettingDependency"|
|インスタンスのパス|プロビジョニング設定に内包されているため REST パスはない|