---
author: Procube Co.,Ltd.
---

# プロビジョニングターゲット

プロビジョニングターゲットはオブジェクト毎の変更内容を管理するオブジェクトです。

## データ型

プロビジョニングターゲットの属性は以下の通りです。

|名前 / 表示名|説明|データ型|修飾子|
|--------|---|----|---|
|id / ID|プロビジョニングターゲットの ID|string|必須。ID文字種。一意|
|requestId / 要求ID|プロビジョニング要求の ID|string|必須。ID文字種|
|className / クラス名|更新対象クラスの名称|string|必須。現在定義されているクラスのいずれか。選択肢外入力可|
|targetId / 対象ID|更新対象オブジェクトの ID|string|必須|
|patches / 変更内容|属性の更新内容を表す[プロビジョニングパッチ](provPatch)オブジェクトの配列|object\[\]|必須。プロビジョニングパッチオブジェクト|
|requestInfo / プロビジョニング要求|このプロビジョニングターゲットの親となる[プロビジョニング要求](provRequest)の情報|object\[\]|プロビジョニング要求オブジェクト|

## REST API パス

このオブジェクトにアクセスする場合の REST API のパスは以下の通りです。

|種別|パス|
|---|---|
|クラスの定義のパス|/IDManager/\_classes/\_provTarget|
|インスタンスのパス|クラスのオブジェクトに内包されており、このオブジェクトには REST で直接アクセスすることはできない。|
