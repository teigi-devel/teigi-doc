---
sidebar_position: 130
---

# ID Manager 2 との比較

ID Manager 2 と比較して改善される項目を記載する

## 100万ユーザ収容

ID Manager 2 では10万件ユーザの収容が目標値であったが、 teigi から派生する ID Manager 3では、100万件の収容をめざす。
これは、主を集約トランザクション（後述）をネタとする。

## 更新伝播の自動検出

teigi では、導出式と更新伝播の組み合わせはではなく、 mongo DB の  aggregation  の機能を使用して、更新時、参照時に結合する仕組みを使用する。


## パッケージの提供

構成情報を分野向けにまとめてパッケージとして提供する。企業向けID管理パッケージ、大学向けネットワーク管理パッケージなど。

## 非同期トランザクションのサポート

時間がかかるトランザクションについては Web UI の前で待つ必要をなくして、 Azure Portal のように通知アイコンでメッセージとして結果を見るようにする。

## Sandbox のブランチ化

IDM2 の Sandbox のような編集中のスナップショットは git のブランチのように複数並行してもてるようにする。 commit 時に自動的にマージする。編集の追い越しで変更がっ競合して場合は、マージに失敗する。利用者は競合点を確認した上で強行突破できる。
ブランチのコミット時に _history 属性にそのオブジェクトの値をコミット前に戻すオペレーションとプロビジョニング要求ID（=teigiのトランザクションID）を保持しており、プロビジョニング要求を検索しなくても変更履歴がロールバック可能な状態でオブジェクトに残るようにする。

## Master の MVCC 化

タスクエグゼキュータから master を参照する場合にプロビジョニング要求IDで現在の値とは別にプロビジョニング要求の変更前後の値を
_rollbacks 属性から算出する。
タスクエグゼキュータの実装で削除の同期をサポートする場合にはオブジェクトの論理削除時に

## 集約トランザクション

## パスワードなどの秘密情報を別管理

パスワード、個人が保有するメールアドレス、クレジットカード情報など個人のプライバシーに係る情報を ID Manager の管理外とすることでその同期や暗号化の問題を軽減する。
LDAP, Active Directory, Azure AD などに対してプロビジョニングを行っている場合、そこのパスワードは管理せず、ID Manager からは以下のプロビジョニングを行う。
* 初期エントリの作成
* パスワードのリセット
* 無効化されたアカウントの情報の削除
また、従来のパスワード関連の機能について以下の通り。
* パスワード変更画面は Access Manager からのパスワード変更強要を含めて別アプリケーションとして ID Manager から切り離す
* 切り離したパスワード変更アプリはLDAP, AD のパスワード変更画面として提供する
* パスワード変更アプリは ID Managerにはパスワードを書き込まず、 プロビジョニング要求管理やタスク管理は行わない
* ADパスワード同期オプションを使用している場合は AD → LDAPへの同期を行い、 ID Manager はパスワード変更に関与しない。
* パスワード変更以外の個人情報はプロフィールアプリとして個人が保有するメールアドレス、クレジットカード情報なども管理する。→ infoScoop と共用かも

## My Page 機能の分離

My Pageのようなポータル機能は ID Manager では提供せず infoScoop にその機能を委ねる

## ワークフローのサポート

ワークフロー機能をサポートする。ID Manager ２ でも自動更新バッチで状態遷移することでワークフロー機能をサポートしているが、状態遷移をオブジェクトで記述できるようにして、自動更新バッチから切り離す/自動生成する。

## メンテナンス対応機能

特定の期間、Web UI、APIからの更新を禁止したり、Web UI にソーリページを表示する機能を提供し、データーベースのマイグレーションや初期データ投入時に利用者からの更新との競合を防ぐ。

## 通知機能

イベントやメッセージの着信時にメール、Slack で通知する機能を実装する。 ID Manager 2 では自動更新バッチで実装していたが、通知機能として分離する/自動生成する。

## 未読管理

メッセージの未読管理機能を提供する。

## 整合性確保の仕組み

初期データ移行や障害発生時にプロビジョニング先との不整合が出た際に内容を比較して合わせるためのプロビジョニングを生成する機能を具備する。

## ドキュメンテーションの自動生成

teigi オブジェクトからマニュアルを生成する機能を提供する。
マークダウン記法によるリッチなドキュメンテーションをサポートする。このため、マークダウンのプレビュー機能を提供する。

### API 仕様

API仕様については Swagger.yml とそのコンパイルされた Web ページを提供する。
Swagger の description では markdownn が使用できるため、teigi のオブジェクト内のマークダウンで記載された内容をそのまま展開できる。

参考: https://www.baeldung.com/swagger-format-descriptions

### 仕様書、説明書

機能仕様書はパッケージに添付された Jinja2 テンプレートに個別パラメータを埋め込んでマークダウンファイルを生成する。
Docusaurus で外枠を提供し、Web から参照できるようにする。納品物用に PDF 生成機能を付与する。

参考: https://nuxnik.com/docusaurus-pdf-generator/