<!--
title:   AOSP(AndroidOpenSourceProject)を使って自作OSが作りたい-part1
tags:    Android,AndroidStudio,Docker,android開発,aosp
id:      950b659c74f889579652
private: false
-->
# はじめに
本記事は実際にAOSP開発に挑戦しながら並行して書いてる。
そのため、更新頻度が遅かったり、無駄な工程が入っているかもしれないが許容してほしい。
※もちろん、誤りがあると分かれば後から修正する

また、非常に手間が多い為、記事の本数が多くなる。

# 本記事part1のゴール
* Dockerのインストール
* 外付けssdに仮想コンテナを作成

# AOSPとは
Android OSに関するプロジェクト名のことを指す。
実は、Androidはオープンソースになっており、インターネットに公開されている。
公式ではAOSPについて以下のように説明されている。
(https://source.android.com/docs/setup/about/faqs?hl=ja#what-is-the-android-open-source-project)

# 開発環境
★ホスト
* OS : Windows11 22H2(OSビルド：22621.1702)
* CPU : 11th Gen Intel(R) Core(TM) i5-1135G7
* RAM : 8GB(これ足ります...？)
* ROM : 外付けSSD 1TB
(https://www.amazon.co.jp/gp/product/B093H3PRRP/ref=ppx_yo_dt_b_asin_title_o01_s00?ie=UTF8&th=1)

**AOSP開発は非常に容量が必要となる。**
**ネット情報によれば250GBほしいとか言われてる。**

★仮想コンテナ
* OS : Ubuntu 18.04
※RAM等はホストOSに依存する

環境構築に今回はDockerを用いる。
また、この後対応していくが外付けSSD内に環境を構築していくためDockerにてコンテナを作る際に少し工夫が必要。

# 環境構築

### Dockerインストール
これについては本記事とは異なるので割愛。
この辺り読んでインストーラ落として入れればいいかな。
(https://docs.docker.jp/docker-for-windows/install.html)
※上記、"★仮想コンテナ"にも記載したが、本記事でコンテナに入れるOSはUbuntu18.04にしておく

### コンテナを外付けSSDに移動する
この段階でCドライブ内にUbuntu18.04を持ったコンテナが出来上がっている想定。

1 - dockerを停止する
下記の"Quit Docker Desktop"を実行
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3355973/ccdfa905-7dc0-662c-c122-a192c10450df.png)

2 - wslを停止する
cmdを起動して以下のコマンドを実行
`bat:cmd
wsl --shutdown
`

3 - cmdを起動し以下のコマンドを実行
`bat:cmd
wsl -l -v
`
出力結果↓
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3355973/91baa4d6-03ea-b364-16da-1e49998f039a.png)

重要なのは、"docker-desktop-data"というやつ。
こいつを移動させる。

4 - docker-desktop-dataをtar形式でエクスポート
cmdを起動し以下のコマンドを実行
`bat:cmd
wsl --export docker-desktop-data  xx>
`
xxには保存先のパスが入るが、ここで外部ストレージを指定する。
自分の場合は以下のように指定した
"D:\DockerCore\ext4.vhdx"
**注意：必ずファイル名まで指定すること**

5 - ディストリビューションの登録を解除
cmdを起動し以下のコマンドを実行
`bat:cmd
wsl --unregister docker-desktop-data
`
6 - ディストリビューションの登録
cmdを起動し以下のコマンドを実行
`bat:cmd
wsl --import docker-desktop-data xx yy
`
xx ... フォルダパス(tarファイルまでは指定しない)
yy ... ファイルパス(tarファイルまで指定)

自分の場合は以下のようになった。
xx ... D:\DockerCore\
yy ... D:\DockerCore\ext4.vhdx

一旦、ここまでで準備完了。
とりあえず、ここまでやればコンテナ内でデータ諸々やってもDドライブ内の容量を使ってくれるので、心配なさそう。

次回から、AOSPを落としたりドライバ入れたりと諸々やっていくよ。