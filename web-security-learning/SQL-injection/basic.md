SQL Injection (SQLi)とは、ユーザーが入力した値をSQLの一部として認識させることでアプリケーションが意図しないSQLを実行することができる脆弱性。

### WHERE条件を変更する

例えば、サイト側で

```
SELECT *
FROM products
WHERE category = 'Gifts'
    AND released = 1;
```

というSQLを実行しているとする。

ここで、categoryがGiftsであり、released = 1, この場合発売済みであるという条件を両方満たす商品のみが取得される

#### OR 1=1

入力値にSQLとして認識される文字列を入れると、条件を変更できる場合がある

例えば、
`'+OR+1=1--`
という入力を考える。

この入力によってSQLは次のようになる。

```
SELECT *
FROM products
WHERE category = '' OR 1=1
-- AND released = 1;
```

1=1は常にTRUEであるためWHERE条件は必ずTRUEになる。
また、`--`はSQLのコメントとして扱われるため、それ以降にある元の条件を無効化でk理宇場合がある。
今回はこの結果`AND released = 1`により除外されていた商品まで取得できる場合がある。

### UNION SELECT

SQLiでは`UNION SELECT`を利用して元のSELECTの結果に別のSELECT結果を同じテーブルに追加できる場合がある。

例えば、
`UNION SELECT username, password FROM users`
とすることで、元の別テーブルにユーザーネーム及びパスワードの情報を表示させることが可能となることがある。

ただし、`UNION SELECT`においては、もともとのSELECTと追加するSELECTの列数を合わせる必要がある。また対応する列のデータ型も合わせる必要がある

### TODO / Question

- `UNION`の列数及びデータ型の条件についてSQLとしてもう少し理解する
- SQLの種類によってのコメントの記述法などの違いの確認

### Notes

このノートは、PortSwigger Web Security Academyで学習した内容を自分の理解に基づいて整理したものである。
