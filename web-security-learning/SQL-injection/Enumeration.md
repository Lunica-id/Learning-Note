SQLiを利用してデータベースの構造を調べる時に、information_schemaが利用できる場合がある

Schema: 何かしらの考えや理論をわかりやすくするための図、概念図等
データベースにおいては、データベースにおいてデータがどのように構成され、保存され、アクセスするのかについてを定義するもの。構造を整理するためのもの。

### information_schema

information_schemaは、データベースやテーブル、列などに関するメタデータを一覧のように参照するためのもの。

例えば、`information_schema.TABLES`, `inforamtion_schema.COLUMNS`などがある。

### テーブル名を調べる

`information_schema.tables`からテーブル名を取得する

```
+UNION+SELECT+table_name,+'a'+FROM+information_schema.tables--
```

ここではtable_nameの列を取得することでデータベースに存在するテーブルを確認する。　なお'a'は列数をそろえるためのものであり、もし元々のSELECTが一列の場合は必要なく、3列以上の場合は適宜その分の列を足す必要がある。

### カラム名を調べる

目的のテーブルが見つかったら、`information_schema.columns`からそのテーブルのカラム名(=列名)を調べる。

```
'+UNION+SELECT+column_name,+ 'a'
+FROM+information_schema.columns
+WHERE+table_name='pg_user'--
```

流れとしては、

```
information_schema
        ↓
テーブル名を調べる
        ↓
目的のテーブルを見つける
        ↓
カラム名を調べる
        ↓
目的のカラムを見つける
        ↓
データを取得する
```

### TODO / Question

- information_schemaの仕組みや具体的にどのようなテーブルがあるのかを通常のSQLの範囲でもう少し理解する

### Notes

このノートは、PortSwigger Web Security Academyで学習した内容を自分の理解に基づいて整理したものである。
