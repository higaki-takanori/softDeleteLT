DBについて勉強していく中で得た知識をまとめていく。

<br>

# 論理削除はやめよう

論理削除は実装が簡単だから、ついつい手を出してしまいがちだが、色々デメリットが多いのでやめよう。

| id | name | email | created_at | updated_at | is_deleted |
| --- | --- | --- | --- | --- | --- |
| 1 | 檜垣 | higaki@example.com | 2022-12-01 12:00:00 | 2022-12-01 12:00:00 | 0 |
| 2 | 佐藤 | sato@example.com | 2022-12-01 12:00:00 | 2022-12-01 12:00:00 | 0 |
| 3 | 本村 | motomura@example.com | 2022-12-01 12:00:00 | 2022-12-01 12:00:00 | 1 |

# （代替案１）削除日時にする。←おすすめしない

フラグだけでは情報が少ないので、いつ削除されたかを判別できるように削除日時を更新する。

例）

デフォルトを9999年1月1日12時0分0秒にして、削除処理が行われた時間に更新する。

| id | name | email | created_at | updated_at | deleted_at |
| --- | --- | --- | --- | --- | --- |
| 1 | 檜垣 | higaki@example.com | 2022-12-01 12:00:00 | 2022-12-01 12:00:00 | 9999-01-01 12:00:00 |
| 2 | 佐藤 | sato@example.com | 2022-12-01 12:00:00 | 2022-12-01 12:00:00 | 9999-01-01 12:00:00 |
| 3 | 本村 | motomura@example.com | 2022-12-01 12:00:00 | 2022-12-01 12:00:00 | 2022-11-12 14:12:54 |

削除：deleted_at < 現在時刻

Not削除：現在時刻 < deleted_at

# （代替案２）状態遷移でモデリングする。

state machineを用いることで、

enum型 or varchar型の状態カラムを追加する。

# （代替案３）履歴テーブルをトリガーで作成する。

削除となるレコードが多い場合。

ログとしてDBに残しておきたい。

ユーザーには削除されたように見えるけど、本当は削除したくない。

状態やフラグにテーブルに一つしか貼れないインデックスを取られたくない。

# （代替案４）更新も削除もしない

エンタープライズシステムにおいて、無限にスケールしないといけない訳ではない。

例）ユーザーは社員ぐらいで膨大にならない。

起こった事実のみをDBに格納していく。

データの更新は改ざんになるのでしない。

事実はDBの中に必ずある。

データが膨大になるけど、お金で解決しろ。

常にorder byで最新のデータを取得しないといけない。

# 「誤った操作を元に戻したい」が解決できていない問題

誤操作しにくい画面にしよう。

遅延レプリケーション

# 参考

[27. 論理削除とは何か？どのような解法があるのか？ w/ twada](https://fukabori.fm/episode/27)