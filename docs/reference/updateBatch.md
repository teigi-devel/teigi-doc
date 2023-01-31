---
author: Procube Co.,Ltd.
---

# 自動更新バッチ

自動更新バッチはクロックデーモンから起動されるバッチ処理を定義できます。

## 機能

自動更新バッチにより、メールの発信処理とオブジェクトの更新処理を実行することが可能です。たとえば、以下のようなバッチ処理が定義できます。

-   パスワード変更滞留者に対する警告メールの発信
-   有効期限切れアカウントの一時停止

## データ型

ガジェット定義の属性は以下の通りです。

|名前 / 表示名|説明|データ型|修飾子|
|--------|---|----|---|
|name / 名前|バッチ処理の名前|string|必須、システムID、一意|
|displayName / 表示名|バッチ処理の表示名|string| |
|description / 説明文|バッチ処理の説明 -   ヘルプの生成に使用される|string| |
|interfaceName / インタフェース名|インポート処理がアクセスするインタフェース名 -   バッチ処理実行時にこのインタフェースの全件取得を実行し、各オブジェクトに対して、メール発信と自動更新の処理を行う|string|必須|
|mailServer / メールサーバ|メール発信に利用する SMTP サーバの FQDN -   この属性が指定されていない場合は、メール発信処理は行わない|string| |
|mailPort / ポート番号|メール発信に利用する SMTP サーバのポート番号|number| |
|mailSecure / 暗号化フラグ|メール発信に利用する SMTP サーバへの接続に SSL 暗号化を使用するか否かを示すフラグ。587番ポートで STARTTLS を使用する場合は最初に非暗号化で接続するので false にしておく。|boolean| |
|authUser / SMTP認証ID|SMTP認証のためのID|string| |
|authPassword / SMTP認証パスワード|SMTP認証のためのパスワード|string| |
|mailTo / メールTo|メールの宛先を記述するテンプレート-   テンプレート内では、 "<%= 属性名 %\>" の形式が記述されると、インタフェースから取得されたオブジェクトの属性の値で置き換えられる。テンプレート内で "<% \_.each\(属性名, function\(item\) \{ %\>" と "<% \}\); %\>" で挟んで、繰り返し内容を記述することで、配列をメール内の繰り返しテキストに変換することができる。繰り返し内容の中では、 "<%= item.属性名 %\>" の形式が記述されると、配列の個々の要素の属性の値で置き換えられる。複数行になる場合は複数の宛先に配信される。Toフィールドが空になる場合は、メール送信は行われないが、自動更新は実行される。|string| |
|mailFrom / メールFrom|メールの送信元。テンプレートには非対応|string| |
|mailCc / メールCc|メールのCcを記述するテンプレート-   テンプレート内では、 "<%= 属性名 %\>" の形式が記述されると、インタフェースから取得されたオブジェクトの属性の値で置き換えられる。テンプレート内で "<% \_.each\(属性名, function\(item\) \{ %\>" と "<% \}\); %\>" で挟んで、繰り返し内容を記述することで、配列をメール内の繰り返しテキストに変換することができる。繰り返し内容の中では、 "<%= item.属性名 %\>" の形式が記述されると、配列の個々の要素の属性の値で置き換えられる。複数行になる場合は複数の宛先に配信される。|string| |
|mailBcc / メールBcc|メールのBccを記述するテンプレート-   テンプレート内では、 "<%= 属性名 %\>" の形式が記述されると、インタフェースから取得されたオブジェクトの属性の値で置き換えられる。テンプレート内で "<% \_.each\(属性名, function\(item\) \{ %\>" と "<% \}\); %\>" で挟んで、繰り返し内容を記述することで、配列をメール内の繰り返しテキストに変換することができる。繰り返し内容の中では、 "<%= item.属性名 %\>" の形式が記述されると、配列の個々の要素の属性の値で置き換えられる。複数行になる場合は複数の宛先に配信される。|string| |
|mailHeaders / メール追加ヘッダー|メールの追加ヘッダーを記述する。":"で区切られたメールのヘッダー形式で記述しなければならない。テンプレートには非対応|string| |
|mailSubject / メールタイトル|メールのタイトルを記述するテンプレート-   テンプレート内では、 "<%= 属性名 %\>" の形式が記述されると、インタフェースから取得されたオブジェクトの属性の値で置き換えられる。テンプレート内で "<% \_.each\(属性名, function\(item\) \{ %\>" と "<% \}\); %\>" で挟んで、繰り返し内容を記述することで、配列をメール内の繰り返しテキストに変換することができる。繰り返し内容の中では、 "<%= item.属性名 %\>" の形式が記述されると、配列の個々の要素の属性の値で置き換えられる|string| |
|mailTemplate / メール本文|メールの本文を記述するテンプレート-   テンプレート内では、 "<%= 属性名 %\>" の形式が記述されると、インタフェースから取得されたオブジェクトの属性の値で置き換えられる。テンプレート内で "<% \_.each\(属性名, function\(item\) \{ %\>" と "<% \}\); %\>" で挟んで、繰り返し内容を記述することで、配列をメール内の繰り返しテキストに変換することができる。繰り返し内容の中では、 "<%= item.属性名 %\>" の形式が記述されると、配列の個々の要素の属性の値で置き換えられる。複数行になる場合は複数の宛先に配信される。|string| |
|doAggregate / メール集約フラグ|インタフェースで取得されたオブジェクトに対して、まとめたメールを出すか否かを示すフラグ -   この属性が true の場合は、メールテンプレートに "\_list" という名前の変数が渡され、その内容はインタフェースから取得されたオブジェクトの配列である。メール送信に失敗した場合は、すべての自動更新が実行されない。。この属性が false の場合は、インタフェースから取得されたオブジェクトごとにメールテンプレートが呼び出されて、メール送信が行われる。メールテンプレートではそのオブジェクトの属性を直接利用するか、あるいは"\_list"はそのオブジェクトのみを有する配列として使用できる。メール送信に失敗した場合は、失敗したオブジェクトのみ自動更新が実行されない。|boolean| |
|autoUpdate / 自動更新|バッチ処理の実行時にオブジェクトの内容を更新する[属性値導出](propertyDerivation)のリスト -   この属性が空の場合は、自動更新処理は行わない。属性値導出の結果、すべてのオブジェクトで値の変化がなかった場合はプロビジョニング要求はキャンセルされる。更新時導出フラグと条件式の値は使用されない|object\[\]|属性値導出オブジェクト|

## REST API パス

このオブジェクトにアクセスする場合の REST API のパスは以下の通りです。

|種別|パス|
|---|---|
|クラスの定義のパス|"/IDManager/\_classes/\_updateBatch"|
|インスタンスのパス|"/IDManager/\_sandboxUpdateBatch/" + this.name|
