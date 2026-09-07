### HTTP Basic Authentication

HTTP Basic Authenticationとは、HTTPのリクエストにユーザー名とパスワードを含めて認証する仕組み
比較的簡単に利用できるためよく使われている
認証情報は、
`username:password`
のような形式のものを、Base64でエンコードをした状態でAuthorizationヘッダーに含まれる。
`Authorization: Basic YWxpY2U6c2VjcmV0MTIz`
ただし、Base64はあくまでエンコードであり、暗号化ではないためデコードを行えば元のユーザー名およびパスワードを得ることができる
そのため、HTTPSと組み合わせて別の暗号化を行う必要がある(HSTS:HTTPSで通信するよう指示する仕組み等を使用する)

ただし、

- 何らかの理由(1)でHSTSを実装していない場合暗号化されずに送受信される危険性があること
- HTTP basic authenticationにはそもそもBrute Forceへの防御策がないこと(あくまでWebサーバーがただ受け取って認証するだけの仕組みであるため)
- 静的なトークン(=Base64でエンコードした値は常に同じ)であること

からとても安全な方法とは言いにくい

### TODO / Question

- HSTSについてさらに詳しく理解する　HSTSを実装していない状態とはどのような状態なのか、どの段階(ブラウザなのか、ウェブサイト自体なのか)の話なのか
- なぜユーザーネームやパスワードのエンコードが必要なのかの理解

### Notes

このノートは、PortSwigger Web Security Academyで学習した内容を自分の理解に基づいて整理したものである。
現在、Burp Suiteを使用ができないためラボはまだ実施できていない
