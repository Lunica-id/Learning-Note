### HTTP Basic Authentication

HTTP Basic Authenticationとは、HTTPのリクエストにユーザー名とパスワードを含めて認証する仕組み
認証情報は、
`username:password`
のような形式のものを、Base64でエンコードをした状態でAuthorizationヘッダーに含まれる。
`Authorization: Basic YWxpY2U6c2VjcmV0MTIz`
ただし、Base64はあくまでエンコードであり、暗号化ではないためデコードを行えば元のユーザー名およびパスワードを得ることができる
そのため、HTTPSと組み合わせて別の暗号化を行う必要がある(HSTS:HTTPSで通信するよう指示する仕組み等を使用する)

#### Brute forceへの対策

HTTP BasicはあくまでWebサーバーが情報をただ受け取って認証するだけの仕組みであるため防御機構が含まれていない
さらにHSTSに非対応なブラウザだった場合に暗号化されていない送受信される危険性もある
他にもBase64した値は常に同じであるため予測ができてしまう可能性もある
そのため、HSTSを導入するほかに、アプリ単位でIPアドレスブロックや二段階認証等の別の防御策を取り入れる必要がある

### TODO / Question

- HSTSについてさらに詳しく理解する
- なぜユーザーネームやパスワードのエンコードが必要なのかの理解

### Notes

このノートは、PortSwigger Web Security Academyで学習した内容を自分の理解に基づいて整理したものである。
現在、Burp Suiteを使用ができないためラボはまだ実施できていない
