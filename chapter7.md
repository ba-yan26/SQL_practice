## サブクエリ　副お問い合わせ
----
- サブクエリの動作
  - `内側にあるselect文が実行され結果に化ける。その後、外側のSQLが実行される`

- selectをネストする
  - select文で何らかの検索結果を得て、得られた具体的な値を用いてさらにselectやupdateなどを実行したい場合

```
select 費目, 出金額
  from 家計簿
  where 出金額 = (select max(出金額) from 家計簿)
```

#### 単一行副お問い合わせとは
  - 検索結果が1行1列の一つの値となる副お問い合わせを指す
  - select文の選択列リストやfrom句、updateのset句、また1つの値との判定を行うwhere句の条件式などに記述することができる

- set句を利用した例
```
update 家計簿集計
  set 平均 = (select avg(出金額)
            from 家計簿アーカイブ
            where 出金額 > 0
            and 費目 = '食費')
  where 費目 = '食費'
```

- 選択列リストで利用した例
```
select 日付, メモ, 出金額,
      (select 合計 from 家計簿集計
      where 費目 = '食費') as 過去の合計額
from 家計簿アーカイブ
where 費目 = '食費'
```

#### 複数行のお問い合わせとは
  - 検索結果がn行1列の複数の値となる副お問い合わせ(ただしnは1以上)
  - 複数の値との判定を行うwhere句の条件式や、select文のfrom句に記述することができる

- in演算子で利用する例
```
select *
from 家計簿集計
where 費目 in(select distinct 費目 from 家計簿)
```

- anu/all演算子で利用する例
```
select *
from 家計簿
where 費目 = '食費'
and 出金額 < any(select 出金額 from 家計簿アーカイブ where 費目 = '食費')
```