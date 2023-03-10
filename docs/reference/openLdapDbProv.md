---
author: Procube Co.,Ltd.
---

# OpenLDAP データベースプロビジョニング

OpenLDAP データベース設定オブジェクトを Repository データベースに作成し、 プロビジョニング設定の種別で OpenLDAPDB を指定し、クラス毎設定にOpenLDAP データベース設定（ \_openLdapDbSetting）クラスを 追加することにより、 OpenLDAP データベース設定オブジェクトの内容から OpenLDAP のサーバ上の LDAP データベースを設定できます。

## OpenLDAP データベース設定

OpenLDAP データベースプロビジョニングを実行するためには、最初に OpenLDAP データベース設定オブジェクトを作成し、 OpenLDAP のデータベースのパラメータを指定する必要があります。

CAUTION:

番号は既存の bdb（RHEL7以降はhdb） を変更する場合は2を、追加する場合は3以降を順番に追加してください。番号の割り当ては OpenLDAP 側で自動的に行われますので、実態と合わない番号を設定すると制御できなくなります。

CAUTION:

データベース設定の削除では OpenLDAP の制限により実際には OpenLDAP サーバから設定が削除されません。手作業で削除して、OpenLDAP サーバを再起動していただく必要があります。

OpenLDAP データベース設定の属性は以下の通りです。

|名前 / 表示名|説明|データ型|修飾子|
|--------|---|----|---|
|number / 番号|LDAP データベースの設定の番号。OLC 上のデータベースのエントリの dn は olcDatabase=\{*番号*\}bdb,cn=config となる。データベースのタイプは bdb\(hdb\) のみをサポートしている。 同じ番号のエントリが既にある場合は、設定されている属性が上書きされる。OpenLDAP インストール時は、 番号 0, 1 は、それぞれ、config, monitor のデータベースで使用されており、既定の bdb が 2 を使用している。 従って、既定の bdb を上書きして使用する場合は、番号を 2 とし、データベースを追加する場合は、 3 を使用すべきである。|number|-   必須、一意|
|displayName / 表示名|OpenLDAP データベース設定の表示名|string| |
|description / 説明|OpenLDAP データベース設定の説明 -   ヘルプの生成に使用される|string| |
|dnSuffix / DN サフィックス|LDAP データベースの DN サフィックス|string|-   必須、一意|
|dbDirectory / DBディレクトリ|LDAP データベースを配置するディレクトリ。 データベースを新規に追加する場合は、プロビジョニングに先立ってこのディレクトリを作成し、そのオーナーとグループには ldap を指定しておかなければならない。|string|-   必須、ID文字種、一意|
|rootDN / rootDN|データベース管理者が BIND する際の DN \( DN サフィックスの直下の DN でなければならない）|string|-   必須|
|rootPW / rootPW|データベース管理者が BIND する際のパスワード|string|-   必須、パスワード文字種、SSHA 暗号化設定|
|acl / アクセス制御リスト|データベースへのアクセスに関するルール（ OpenLDAPアクセス制御エントリ）のリストで、 アクセス毎に先頭から順にマッチングを行い、 マッチングに成功したルールによりアクセスが拒否/許可される|object|-   OpenLDAPアクセス制御エントリ|
|indexedAttribute / インデックス付与属性|データベース上でインデックスを生成する属性のリスト。インデックスは等価検索用のものが設定される。 属性名にアンダースコア '\_' が含まれる場合はハイフン '-' に置換される。|string|-   配列、システムID|
|limits / 検索上限設定リスト|データベース検索での制限に関する設定（ OpenLDAP 検索上限設定）のリスト|object|-   OpenLDAP 検索上限設定|

上記の中で OpenLDAPアクセス制御エントリの属性は以下の通りです。

|名前 / 表示名|説明|データ型|修飾子|
|--------|---|----|---|
|dn / 対象DN|アクセス対象のDN（指定しなければ、すべてのエントリが対象となる）|string||
|dnType / 対象DNタイプ|アクセス対象のエントリの DN と対象DN に指定した文字列とのマッチング条件を指定する。|**regex / 正規表現**|対象DN を正規表現としてマッチングする。|
|**base / ベース**|対象DN のエントリのみに適用される。|
|**one / 直下**|対象DN の直下のエントリに適用される。|
|**subtree / サブツリー**|対象DN とその配下のエントリに適用される。|
|**children / 子孫**|対象DN を含まず、その配下のエントリに適用される。|
|string|-   \[正規表現, ベース、直下, サブツリー, 子孫\] のいずれか、対象DN を指定したときのみ有効かつ必須|
|filter / フィルタ|アクセス対象に対するフィルタ式（例：```
(&(objectClass=user)(accountLocked=false))
```

）|string| |
|attrs / 属性|アクセスが許される属性（ entry と children は予約語であり指定できない）|string|-   配列|
|entryFlag / エントリフラグ|エントリ自身へのアクセスを許可するか否かを示すフラグ（エントリの追加・削除を行うためにはこのフラグが true で書き込み権が必要）|boolean||
|childrenFlag / 配下フラグ|子エントリへのリンクに対するアクセスを許可するか否かを示すフラグ（子エントリの追加・削除を行うためにはこのフラグが true で書き込み権が必要）|boolean||
|rule / ルール|対象エントリに対するアクセスを制御するルール（アクセスしようとしているエントリがこのオブジェクトの定義にマッチングしなければ、ルールは適用されない）|object|-   必須、OpenLDAPアクセス制御ルール|

上記の中で OpenLDAPアクセス制御ルールの属性は以下の通りです。

|名前 / 表示名|説明|データ型|修飾子|
|--------|---|----|---|
|who / アクセス者|LDAPプロトコルで BIND 認証している実体を識別する方法を指定する。 指定がない場合は、すべての実体が対象となる。|**anonymous / 匿名**|匿名\(認証されていない\)ユーザ|
|**users / ユーザ**|認証されたユーザ|
|**self / 自身**|目的のエントリの DN で BIND 認証したユーザ。|
|**dn / DN指定**|実体DN で指定された DN で BIND 認証したユーザ。|
|string||
|dn / 実体DN|BIND 認証したユーザのDN|string|アクセス者が DN指定の場合のみ有効かつ必須|
|dnType / 実体DNタイプ|BIND認証で用いた DN の値と実体DN に指定した文字列とのマッチング条件を指定する。|**regex / 正規表現**|BIND認証で用いた DN の値に対して、実体DN を正規表現としてマッチング可能な場合に適用される。|
|**base / ベース**|実体DN の値で BIND している場合に適用される。|
|**one / 直下**|実体DN の直下のDNで BIND している場合に適用される。|
|**subtree / サブツリー**|実体DN とその配下のDNで BIND している場合に適用される。|
|**children / 子孫**|実体DN を含まず、その配下のDNで BIND している場合に適用される。|
|string|-   \[正規表現, ベース、直下, サブツリー, 子孫\] のいずれか、実体DN を指定したときのみ有効かつ必須|
|dnattr / DN照合属性|アクセスの対象となるエントリにここで指定された名前の属性があり、かつ、BIND認証で使用した DN と一致する場合に適用される|string||
|group / グループ|ここで指定された DN のエントリの member 属性の値のうちの一つが BIND認証で使用した DN と一致する場合に適用される|string||
|peername / 接続元名|接続元IPアドレスがここに指定したアドレスに一致する場合に適用される。 パーセント記号（%）に続けてマスク値を指定することで、ネットワークアドレスを指定できる（例：192.168.1.0%255.255.255.0）。 この場合、そのネットワークに属する IPアドレスから接続された場合に適用される。|string||
|level / 許可レベル|ルールが適用された場合にアクセスが許可されるレベルを指定する。|**none / 拒否**|すべてのアクセスを拒否する。|
|**auth / 認証**|そのエントリに BIND 認証する場合にのみ許可される。|
|**compare / 比較**|そのエントリの属性の値を特定の値と比較することが許可される。また、認証も許可される。|
|**search / 検索**|そのエントリに対する検索が許可される。また、比較、認証も許可される。|
|**read / 読み出し**|そのエントリを読み出すことが許可される。また、検索、比較、認証も許可される。|
|**write / 書き込み**|そのエントリに書き込むことが許可される。また、読み出し、検索、比較、認証も許可される。|
|string|-   \[拒否、認証、比較、検索、読み出し、書き込み\] のいずれか、必須|

上記の中で OpenLDAP 検索上限設定の属性は以下の通りです。

|名前 / 表示名|説明|データ型|修飾子|
|--------|---|----|---|
|selectorType / セレクター|検索上限設定の対象となるユーザまたは対象エントリの特定方法を指定する。|**anonymous / 匿名**|匿名\(認証されていない\)ユーザ|
|**users / ユーザ**|認証されたユーザ|
|**dnSelf / 実体DN指定**|指定された DN で BIND 認証したユーザ|
|**dnThis / 対象DN指定**|指定された DN で特定されるエントリ|
|**group / グループ**|指定された DN のグループのユーザ|
|string|-   \[匿名, ユーザ、実体DN指定, 対象DN指定, グループ\] のいずれか|
|dn / DN|セレクターが実体DN指定の場合はユーザの DN。対象DN指定の場合は検索対象のエントリの DN。グループの場合は member 属性を持つエントリの DN|string|-   セレクターが実体DN指定、対象DN指定またはグループを指定したときのみ有効かつ必須|
|dnType / DNタイプ|DN に指定した文字列とのマッチング条件を指定する。|**regex / 正規表現**|DN を正規表現としてマッチングする。|
|**base / ベース**|DN のエントリのみに適用される。|
|**onelevel / 直下**|DN の直下のエントリに適用される。|
|**subtree / サブツリー**|DN とその配下のエントリに適用される。|
|**children / 子孫**|DN を含まず、その配下のエントリに適用される。|
|string|-   \[正規表現, ベース、直下, サブツリー, 子孫\] のいずれか、セレクターが実体DN指定または対象DN指定を指定したときのみ有効かつ必須|
|sizeLimit / 件数上限|検索上限件数。空白の場合は指定なし、0の場合は unlimited となる。|number| |
|timeLimit / 時間制限|検索時間制限（秒）。空白の場合は指定なし、0の場合は unlimited となる。|number| |

## その他の設定値

OpenLDAP データベースプロビジョニングでは、エントリを新規に生成する場合は、 OpenLDAP データベース設定オブジェクトに設定されている項目以外に 以下の項目を設定します。

```
objectClass: olcDatabaseConfig
objectClass: olcBdbConfig
olcAddContentAcl: FALSE
olcLastMod: TRUE
olcMaxDerefDepth: 15
olcReadOnly: FALSE
olcSyncUseSubentry: FALSE
olcMonitoring: TRUE
olcDbCacheSize: 1000
olcDbCheckpoint: 1024 15
olcDbNoSync: FALSE
olcDbDirtyRead: FALSE
olcDbIDLcacheSize: 0
olcDbLinearIndex: FALSE
olcDbMode: 0600
olcDbSearchStack: 16
olcDbShmKey: 0
olcDbCacheFree: 1
olcDbDNcacheSize: 0
```

```
objectClass: olcDatabaseConfig
objectClass: olcHdbConfig
```

## キャッシュ設定について

OpenLDAP データベースプロビジョニングでは、エントリを新規に生成または変更される場合、 常に以下の項目を設定し、キャッシュサイズを 320MB に変更します。

```
olcDbConfig: {0}set_flags DB_LOG_AUTOREMOVE
olcDbConfig: {1}set_cachesize 0 268435456 1
olcDbConfig: {2}set_lg_bsize 2097152
```

LDAP データベースの設定の番号が 2 の場合は、OpenLDAP サーバに root 権限でログインし、下記のコマンドを実行することによってキャッシュサイズ変更が反映されます。3 以降の番号で新規に生成される場合は不要です。

```
# service slapd stop
# cd /var/lib/ldap/
# db_recover
# service slapd start
```
