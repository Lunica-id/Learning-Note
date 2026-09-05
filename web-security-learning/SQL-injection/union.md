### UNION SELECT

SQLiでは`UNION SELECT`を利用して元のSELECTの結果に別のSELECT結果を同じテーブルに追加できる場合がある。

例えば、
`UNION SELECT username, password FROM users`
とすることで、元の別テーブルにユーザーネーム及びパスワードの情報を表示させることが可能となることがある。

ただし、`UNION SELECT`においては、もともとのSELECTと追加するSELECTの列数を合わせる必要がある。また対応する列のデータ型も合わせる必要がある

### TODO / Question

- `UNION`の列数及びデータ型の条件についてSQLとしてもう少し理解する

### Notes

このノートは、PortSwigger Web Security Academyで学習した内容を自分の理解に基づいて整理したものである。
