---
author: Procube Co.,Ltd.
---

# バリデーション定義

バリデーション定義では、オブジェクトの新規登録時および更新時にチェックを行う計算式を定義できます。

## データ型

バリデーション定義の属性は以下の通りです。

|名前 / 表示名|説明|データ型|修飾子|
|--------|---|----|---|
|validateEx / チェック式|チェックを行う[計算式](expression.md)。この計算式の結果が null か false である場合は、エラーとなる|string|必須、計算式|
|descriptionEx / メッセージ式|エラーメッセージを生成する[計算式](expression.md)。エラーの原因となった情報を埋め込むことが可能である。|string|計算式|
|isWarning / 警告フラグ|エラーレベルが警告か否か。この値が true である場合、 API で \_ignoreWarning パラメータを指定することでエラーを無視して登録できる。|boolean| |

## 警告レベルのバリデーションの無視

警告フラグが true となっているバリデーション定義でエラーになった場合、 API の Query文字列に \_ignoreWarning を指定することで、無視して新規登録や更新を実行できます。 ガジェットから新規登録時や更新時に警告レベルのエラーが出た場合は、エラーメッセージの最後に「エラーを無視して登録しますか？」という質問文を付与し、ユーザがエラーを無視するか否かを選択できるようにします。

## 複数のエラーがあった場合の API からの返却値

既定の論理エラーを含めて複数のエラーがあった場合、それらのエラーメッセージをマージしてエラーを返却します。また、すべてのエラーが警告レベルである場合は、エラーレベルを警告として返却します。

## アクセス権

エラーをチェックするためにエラーが発生したインタフェースとは別のインタフェースにアクセスすることが可能です。このチェックのためのインタフェースアクセスは管理者権限で実行され、アクセス制御リストの内容に関わらず、すべてのインタフェースにアクセス可能です。

## REST API パス

このオブジェクトにアクセスする場合の REST API のパスは以下の通りです。

|種別|パス|
|---|---|
|クラスの定義のパス|/IDManager/\_classes/\_validationDefinition|
|インスタンスのパス|クラスのオブジェクトに内包されており、このオブジェクトには REST で直接アクセスすることはできない。|

