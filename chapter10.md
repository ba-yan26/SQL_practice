## テーブルの作成
----

#### DCLとは
- 誰にそのようなデータ操作やテーブル操作を許すかといった権限を設定するためのSQL命令の総称。権限を付与する`grant`文と剥奪する`revoke`文がある
```
grant 権限名 to ユーザー名 【grant文】
revoke 権限名 from ユーザー名 【revoke文】
```

#### テーブル作成の基本
- 基本形
```
create table テーブル名 (
  列名1 列名1の型名,
  列名2 列名2の型名,
  列名3 列名xの型名
)
```

- デフォルト値の指定を含むテーブルの作成
```
create table テーブル名 (
  列名 型名 default デフォルト値
)
```
  - 例
  ```
  create table 家計簿 (
    日付 date,
    費目id integer,
    メモ varchar(100) default '不明',
    入金額 integer default 0,
    出金額 integer default 0
  )
  ```

- テーブルの削除
  - delete文はDML(データ操作言語:格納や取り出し、更新削除など)になるのでテーブルの削除には使えない
  ```
  drop table テーブル名
  ```
  `DDLについてはロールバックができないことがあるので注意が必要`

  