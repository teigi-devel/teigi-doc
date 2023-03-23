---
sidebar_position: 30
---

# 日付と日時

日付と日時に関するスキーマ定義について説明します。

## 日時

日時はデータベース内ではUnix時間(協定世界時 (UTC) での1970年1月1日午前0時0分0秒から形式的な経過秒数)を浮動小数点数で保存しています。表示時にユーザのタイムゾーンに合わせて変換されます。入力文字列はその逆の変換を行われます。JSON形式で文字列化される場合には UTC であることに注意してください。

## 時刻とタイムスパン

時刻およびタイムスパンはデータベース内では秒数を浮動小数点数で保存しています。表示時に HH:mm:SS 形式に変換されます。入力文字列はその逆の変換が行われます。

## 日付

日付はUnix時間でその日付の 00:00:00 の日時で表現されます。時刻やタイムスパンとの加減算や比較を行う際には、ローカルタイムゾーンの 00:00:00 にずらしてから計算を行います。結果の型は日時型になります。日時型から日付型へのキャストは、直近のローカルタイムゾーンの 00:00:00 の日付に変換されます。つまり、切り捨て後タイムゾーンのオフセットが戻されます。
JSON形式で文字列化される場合には YYYY-MM-DD 形式になり、ローカルタイムゾーンの 00:00:00 を指していることに注意してください。特に有効期限などの演算においては、本節に記載内容の変換を考慮して実装する必要があります。