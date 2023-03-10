---
author: Procube Co.,Ltd.
---

# プロビジョニングパッチ

プロビジョニングパッチは属性毎の変更内容を管理するオブジェクトです。

## データ型

プロビジョニングパッチの属性は以下の通りです。

|名前 / 表示名|説明|データ型|修飾子|
|--------|---|----|---|
|propertyName / 属性名|更新対象属性名|string｜必須｜システムID｜クラスに定義されている属性のいずれか｜
|diff / 差分|更新内容を表す [JSONDiffPatchオブジェクト](JSONDiffPatch)オブジェクトの文字列表記|string｜必須｜

## REST API パス

このオブジェクトにアクセスする場合の REST API のパスは以下の通りです。

|種別|パス|
|---|---|
|クラスの定義のパス|/IDManager/\_classes/\_provPatch|
|インスタンスのパス|クラスのオブジェクトに内包されており、このオブジェクトには REST で直接アクセスすることはできない。|
