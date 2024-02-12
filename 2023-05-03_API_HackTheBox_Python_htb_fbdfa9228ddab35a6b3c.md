<!--
title:   HackTheBox v4 APIの基本的な使い方
tags:    API,HackTheBox,Python,htb
id:      fbdfa9228ddab35a6b3c
private: false
-->
## HackTheBox API
HackTheBox(HTB)自体は割と有名？なので知っている人は多そう。。
んで、一応HTBにもAPIはあってユーザ情報を取得したり色々出来る。
↓以下は公式doc↓
https://documenter.getpostman.com/view/13129365/TVeqbmeq

## 使い方
色々と沼ったわけだが、
どこで沼ったかをグダグダ書くよりも先に結論から書いておくと動いたコードは以下。
今回は該当ユーザーの接続状態を取得するEPを叩いてみた。
`python
import http.client
import json

# ログイン情報を設定する
login_payload = {
    'email': 'ログイン用メールアドレス',
    'password': 'ログイン用パスワード',
    'remember':'true'
}

# ログインリクエストを作成する
login_conn = http.client.HTTPSConnection("www.hackthebox.com")
login_headers = {
    'Content-Type': 'application/json'
}

login_conn.request("POST", "/api/v4/login", json.dumps(login_payload), login_headers)

# ログインレスポンスからトークンを取得する
login_res = login_conn.getresponse()
login_data = login_res.read()
login_json = json.loads(login_data.decode("utf-8"))
auth_token = login_json['message']['access_token']

# ログイン後にAPIにアクセスするリクエストを作成する
api_conn = http.client.HTTPSConnection("www.hackthebox.com")
api_headers = {
    'Authorization': f'Bearer {auth_token}'
}

# 使用したいエンドポイント
api_conn.request("GET", "/api/v4/user/connection/status", headers=api_headers)

# APIからのレスポンスを取得する
api_res = api_conn.getresponse()
api_data = api_res.read()
print(api_data.decode("utf-8"))
`
出力結果↓
`
{"status":"0","connection":"not connected"}
`

こんな感じなんかな

## どこで沼ったか
実行したいAPIを呼ぶ前処理がよく分からんかった。
`python
# ログイン後にAPIにアクセスするリクエストを作成する
api_conn = http.client.HTTPSConnection("www.hackthebox.com")
api_headers = {
    'Authorization': f'Bearer {トークン情報}'
}

# 使用したいエンドポイント
api_conn.request("GET", "/api/v4/user/connection/status", headers=api_headers)

# APIからのレスポンスを取得する
api_res = api_conn.getresponse()
api_data = api_res.read()
print(api_data.decode("utf-8"))
`
最初は上記のような感じでこれだけやって実行すればいいんじゃないの？と思っていた。
んで、トークン情報はマイページの以下の情報貼ればいいんかなって。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3355973/e374792c-edb1-4663-1c60-f1a40f4d839b.png)

ただ、そういうわけではないっぽい？？
必ず、初めにログインリクエストを実行してレスポンスで得られたトークンを使って呼ぶっぽい。

2時間位沼ったー。