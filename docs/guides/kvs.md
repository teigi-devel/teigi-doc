---
sidebar_position: 1
---

# KVS

、 Web サーバと同様に Key の拡張子でその MIME Type を識別します。オブジェクトは json か YAML で記述され、拡張子は json, yaml, yml のいずれかになります。

### マージ機能

オブジェクトのマージにおいては以下を実装する。
- オブジェクトの追加・削除
- 属性値の上書き
- 内包オブジェクトの再帰的マージ
- 順序付き配列への要素の追加、削除


オブジェクトは Key Value Store に格納される。
プリフィックスからサーバ上のディレクトリへの対応が取れる。
remote を指定すると git との連携が自動的に行われる。
git の差分情報から差分レイヤーを生成する機能あり？
Layer マージとは別に個別オブジェクトの派生元指定によるマージもできる
## 版管理

イベントから発生するプロビジョニングがイベント発生時点の内容を参照するようにする。
これは、動的 Layer によって実装される。