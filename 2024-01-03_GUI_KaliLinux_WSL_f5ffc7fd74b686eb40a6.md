<!--
title:   【WSL】真っ白なKali LinuxをGUI化したい！
tags:    GUI,KaliLinux,WSL
id:      f5ffc7fd74b686eb40a6
private: false
-->
# 初めに
今回はLinuxディストリビューションの一種である、Kali LinuxをGUI化します。
色々事情あって、何度もKaliを構築し直すのですがその度に忘れるのでもう備忘録化しよかなと。

# 環境

# ホストOS
* OS : Windows 10 22H2

# Kali Linux
本当に真っ白。
power shellで
`powershell
wsl --install kali-linux
`
と入力してアカウント作った段階です。

# やり方
## 手順1　apt更新
`bash
sudo apt update -y
`
とりあえず、更新しないと何も始まらない。

## 手順2　Win-kexのインストール
Windows上でGUIが使えるWin-Kexをインストールします。
`bash
sudo apt install kali-win-kex -y
`

インストールに要する時間は長いので少し待ちましょう。

## 手順3　Win-kexを用いたGUI起動
### 手順3-1　kex起動
`bash
kex --win -s -m
`
を実行してみます。
パスワードの設定などはご自身で設定してください

### 手順3-2　パスワード入力
パスワード設定が完了すると以下のようなウィンドウが立ち上がります。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3355973/0fddc032-8e3d-01f2-4f34-77c72cdb82ac.png)
上記には、手順3-1で設定したパスワード設定を入力します。
そして"OK"を押下します。

# 手順4　起動完了
ここまで行うと以下のように起動が完了します。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3355973/1e8da618-e6af-1552-7360-f75e12811977.png)

# Win-kexの扱い方
* Win-kexの起動
`bash
kex --win -s -m
`

* Win-kexの状態確認
`bash
kex status
`

* Win-kexのキル
`bash
kex kill