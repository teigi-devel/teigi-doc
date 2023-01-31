---
author: Procube Co.,Ltd.
---

# 属性値導出

属性値導出 \(propertyDerivation\) はインタフェースにおいて、オブジェクトを新規登録する際の属性の値の決定方法を定義するオブジェクトです。

## データ型

属性値導出の属性は以下の通りです。

|名前 / 表示名|説明|データ型|修飾子|
|--------|---|----|---|
|targetProperty / 属性名|対象となる属性の名前。ここで指定された属性の値として計算式の計算の結果が入る。|string|必須、システムID、一意|
|derivation / 計算式|属性の値を決定する[計算式](expression.md)|string|必須、計算式|
|onUpdate / 更新時導出フラグ|更新時にも属性値の導出を行うか否かを示すフラグ。false あるいは指定がない場合は追加時しか属性値の導出は行われないが、true が指定されると、更新時にも属性値の導出が行われる。onDeleteOnly と同時に指定することはできない。|boolean| |
|onDeleteOnly / 削除時のみ導出フラグ|削除時に属性値の導出を行うか否かを示すフラグ。false あるいは指定がない場合は削除時に属性値の導出は行われない。true が指定されると、削除時のみ属性値の導出が行われ、追加時および更新時には属性値の導出は行われない。|boolean| |
|conditionEx / 条件式|導出を行うか否かを決定する[計算式](expression.md)。この計算式の結果が null か false である場合は、属性値の導出は行われない。この属性の指定がない場合は無条件に導出が行われる。|string|計算式|

## REST API パス

このオブジェクトにアクセスする場合の REST API のパスは以下の通りです。

|種別|パス|
|---|---|
|クラスの定義のパス|/IDManager/\_classes/\_propertyDerivation|
|インスタンスのパス|クラスのオブジェクトに内包されており、このオブジェクトには REST で直接アクセスすることはできない。|
