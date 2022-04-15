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

- テーブル定義の変更
  - 列の追加
  ```
  alter table テーブル名 add 列名 型 制約
  ```
  - 列の削除
  ```
  alter table テーブル名 drop 列名 型 制約
  ```

- 全件のデータを高速に削除する
  - テーブルの全行を削除する場合、`truncate table文`を利用する
  - 実行結果は`delete from テーブル名`と同じだが動作が違う
    - deleteはwhere句で指定した行だけ削除できるがtruncateは必ず全行削除する
    - deleteはDMLだが、truncateはDDLに属する命令
    - deleteはロールバックに備えて記録を残しながら仮削除していくが、truncateは記録を残さずに行を削除する(よってロールバックできない)
    - deleteは記録を残すため低速だが、truncateは高速。

#### 制約
- create table文中における制約のして
```
create table テーブル名(
  列名 型 制約の指定
)
```
- 制約の指定の例
  - not null制約：nullの格納は許可されない。not null制約はdefault指定と組み合わせて利用されることがほとんど。
  - unique制約：ある列の内容が重複してはならない場合に制約をかける。unique制約がかかっていても、nullが格納された行が複数存在することは許される。
  - check制約：ある列に格納される値が妥当であるかを細かく判定したい場合は、check制約を用いる。checkの後ろのカッコ内に記述した条件式が真となるような値だけが格納を許される。

- 主キー制約の指定(1)
```
create table テーブル名
  列名 型名 primary key, ←主キー制約
  列名 varchar(40) unique
```

- 主キー制約の指定(2,複合主キー)
```
create table テーブル名
  列名 integer,
  列名 varchar(40) unique,
  primary key(列名, 列名)
```

#### 外部キーと参照整合性
- 外部キーが指し示す先にきちんと行が存在してリレーションシップが成立していることを参照整合性という
- 参照整合性の崩壊を引き起こすデータ操作
  1. 他の行から参照されている行を削除してしまう
  2. 他の行から参照されている行の主キーを変更してしまう
  3. 存在しない行を参照する行を追加してしまう
  4. 存在しない行を参照する行に更新してしまう


- 外部キー制約：参照整合性が崩れるようなデータ操作をしようとした場合にエラーを発生させ、強制的に処理を中断させる制約
  - 外部キー制約の指定
  ```
  create table テーブル名 (
    列名 型 references 参照先テーブル名(参照先列名)
  )
  ```
  -  例(家計簿テーブル)
  ```
  create table 家計簿
    日付 date not null,
    費目id integer references 費目(id),
    メモ varchar(100) default '不明' not null,
    入金額 integer default 0 check(入金額 >= 0),
    出金額 integer default 0 check(出金額 >= 0)
  ```


  #### 
  create table 学生
    学籍番号 integer primary key,
    名前 varchar(30) not null
    生年月日 date not null
    血液型 char(2) 
    学部ID char(1) references 学部(id)