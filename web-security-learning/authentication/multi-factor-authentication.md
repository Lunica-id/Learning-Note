### Multi Factor Authentication

認証要素は大きく三つあり、

- Something you know (知っているもの)
  パスワードなど
- Something you have (持っているもの)
  スマホやセキュリティキーなど
- Something you are (自身)
  指紋や顔などのいわゆるBiometrics

この中の同一要素を使用する"multi-factor"認証は複数認証とは言えない
例えば、メールで認証コードを送る方法は、単認証を複雑にしたものである
SMS認証は二段階認証ではあるがSIMの悪用の可能性は否定できないことに注意が必要
理想的なのは、直接認証コードを特定のアプリや端末が生成すること

#### My understanding

メールが複数認証とされない理由
一見二段階あるように思えるが、そもそもメールは"Something you have"ではない
特に、メールはログインさえすれば他端末で閲覧が可能であるため、どちらかといえばSomething you knowにあたり、最初のログイン画面と変わらないため
逆に、SIMもとい電話番号は基本的に一端末でしか使えないためにSomething you haveに近いのでは？

### TODO / Question

- そもそも認証コード生成とその認証はいったいどうやって行われている？

### Notes

このノートは、PortSwigger Web Security Academyで学習した内容を自分の理解に基づいて整理したものである。
現在、Burp Suiteを使用ができないためラボはまだ実施できていない
