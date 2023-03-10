---
sidebar_position: 20
---

# 属性定義

属性定義は属性のデータ型や値の保持ルールを定めるオブジェクトです。

## データ型

属性定義の属性は以下の通りです。

|名前 / 表示名|説明|データ型|修飾子|
|--------|---|----|---|
|name / 名前|属性の名前|string|必須、システムID、一意|
|displayName / 表示名|属性の表示名。省略時には、名前が表示される。|string| |
|type / データ型|属性のデータ型|string|必須、\[文字列, ブール, 整数、浮動小数点数, 日時, 日付, IPアドレス, オブジェクト\] のいずれか|
|className / クラス名|属性のデータ型がオブジェクトである場合に格納されるオブジェクトのクラス名|string|必須、データ型がオブジェクトのときのみ有効、現在定義されているクラスのいずれか|
|isArray / 配列|属性が複数値を持つか否かを示す|boolean|データ型が文字列、整数、浮動小数点数または IPアドレスのときのみ有効|
|description / 説明文|属性の説明 - ガジェット上で、この属性の入力コントロールにフォーカスされたときに説明文として表示される|string| |
|required / 必須|この属性が必須であることを示す|boolean|データ型が文字列、整数、浮動小数点数、日時、日付、 IPアドレスまたはオブジェクトのときのみ有効|
|values / 列挙値|属性がとり得る値を定義する[列挙オブジェクト](enumDefinition)の配列。列挙値式と同時に指定された場合は、マージした結果が列挙値となる。|object\[\]|列挙オブジェクト、データ型が文字列、整数、浮動小数点数または IPアドレスのときのみ有効|
|valuesEx / 列挙値式|属性がとり得る値を定義する[列挙オブジェクト](enumDefinition)の配列を返す[計算式](expression.md)。列挙値と同時に指定された場合は、マージした結果が列挙値となる。|string|データ型が文字列、整数、浮動小数点数または IPアドレスのときのみ有効|
|valuesInterface / 列挙値インタフェース名|多数のオブジェクトから属性の値を選択するためのリストを取得するインタフェース|string|データ型が文字列、整数、浮動小数点数または IPアドレスのときのみ有効、現在定義されているインタフェースのいずれか|
|allowAnotherValue / 選択肢外入力|属性がとり得る値が列挙値や列挙値式で限定されいてる場合に、別の値が許容されるか否かを示す。 特に属性が他のオブジェクトの ID を含むような場合に、列挙値式でそのリンク関係を表現し、この値を false にすることでリンク切れを禁止することができる。true の場合、フォームにコンボボックスが表示される。列挙値に表示名が設定されているとドロップダウンリストで選択した表示名と、フォームに入力される値が異なるように見えるので注意が必要である。false の場合、ユーザインタフェース上はドロップダウンリストなどで入力値が制限されるが、 API 上もサーバ側でチェックされる。|boolean|データ型が文字列、整数、浮動小数点数または IPアドレスのときのみ有効|
|unique / 一意|一意性が true の場合、コレクション内のすべてのオブジェクトについて、この属性の値が一意であることを示す。ただし、オブジェクトが object\[\] の属性内のオブジェクトである場合は、その配列の中で一意となる。値がnullの場合にはチェックされない。|boolean|データ型が文字列、整数、浮動小数点数、日時、日付または IPアドレスのときのみ有効|
|uniqueIgnoreCase / 大文字小文字無視|一意性チェック時に大文字小文字を無視するかどうかを示す。 true の場合、この属性の値が大文字小文字を区別せずに一意であることを示す。値がnullの場合にはチェックされない。|boolean|一意 \( unique \) が true の場合のみ有効|
|stringRestriction / 文字列制約|使用できる文字種および文字列パターンの限定。文字列値の文字種やパターンにあわせるための正規化については、[文字列値の補正と正規化](stringRestriction)を参照のこと|string|データ型が文字列のときのみ有効、\[システムID, ID文字種, ひらがな, カタカナ, パスワード文字種, MACアドレス, メールアドレス, 計算式, 問い合わせ式, 正規表現, DN\] のいずれか|
|maxLen / 最大長|属性の値の最大長。WebUI でのチェック、LDAPスキーマや RDBスキーマの生成時にデータ型として出力、Validateチェック、ヘルプの生成|number|データ型が文字列のときのみ有効|
|minLen / 最小長|属性の値の最小長。WebUI でのチェック、LDAPスキーマや RDBスキーマの生成時にデータ型として出力、Validateチェック、ヘルプの生成|number|データ型が文字列のときのみ有効|
|pattern / 正規表現（格納値チェック用）|この属性の文字種および文字列パターンのデータベース格納値を制約するための正規表現。本属性の値（MACアドレスなどの場合には正規化後の値）の全部または一部がこの正規表現にマッチすれば、許可される。 完全一致が必要な場合には、先頭と末尾を指定した正規表現を記述しなければならない。本属性の値が設定されない場合には格納値の正規表現チェックは実行されない|string|データ型が文字列のときのみ有効、正規表現|
|patternForInput / 正規表現（入力値チェック用）|この属性の文字種および文字列パターンの入力値を制約するための正規表現。本属性の値（登録・更新リクエストで指定された値）の全部または一部がこの正規表現にマッチすれば、許可される。 完全一致が必要な場合には、先頭と末尾を指定した正規表現を記述しなければならない。本属性の値が設定されない場合には入力値の正規表現チェックは実行されない|string|データ型が文字列のときのみ有効、正規表現|
|propGroupName / 属性グループ|この属性が属する属性グループの名前。詳細は[属性のグループ化](propGroup)を参照|string\[\]| |
|derivation / 導出式|属性の値が参照時に[計算式](expression.md)によって計算される。属性の値は編集不可能である。伝播プロビジョニングによって、他の更新から伝播してマスタデータベースに値が保存される場合を除いて、データベースに値が保存されていない。導出式が設定されている属性をキーにしたソートおよび検索はできない。導出式が設定されている属性をパーティションの分割キーに指定することはできない|string|計算式|
|propagations / 更新伝播|このクラスのオブジェクトに更新があった場合に、他のオブジェクトや自オブジェクトの属性の値を自動的に更新する[更新伝播設定](propagationSetting)を定義する|object\[\]|更新伝播設定オブジェクト|
|encryption / 暗号化設定|属性を暗号化する場合に、その方式を設定する。詳細については、[暗号化設定](cryptOrHash)を参照されたい。|string|データ型が文字列のときのみ有効、SSHA か AES のいずれか|
|outputLdapSchema / LDAPスキーマ生成|OpenLDAP へのプロビジョニングで、この属性のスキーマを生成するかいなかを表すフラグ。 ただし、データ型がオブジェクトである場合は指定できず、スキーマを生成しない。|boolean|データ型がオブジェクト以外のとき有効|

## REST API パス

このオブジェクトにアクセスする場合の REST API のパスは以下の通りです。

|種別|パス|
|---|---|
|クラスの定義のパス|/IDManager/\_classes/\_propertyDefinition|
|インスタンスのパス|クラスのオブジェクトに内包されており、このオブジェクトには REST で直接アクセスすることはできない。|

