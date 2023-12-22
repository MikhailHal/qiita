<!--
title:   Android Kotlin Timberを別モジュールに導入してみた
tags:    Android,AndroidStudio,Kotlin,android開発
id:      e678b99ea686fa34978b
private: false
-->
# 初めに
Timberとはなんやら、みたいな話は既に様々な方が書いてくれてます。
そのため、割愛します。

# きっかけ
元々、僕が作っている個人アプリは、appモジュールのみで構成されてました。
ただ、今後Jetpack Composeを導入した際のプレビュー表示までの時間やビルド時間、今後仕事でマルチモジュール化の知見が求められるのでは？と考えて、マルチモジュールの学習を始めました。

とりあえず、お試しでログ機能を別モジュールに分けることにしたんですね。

# モジュール構成
とりあえず、いろんなサイト見て考えたモジュール構成。(これが良いのかよくわかってない)
水色の部分は全てサブモジュールになってて、狙いとしてはビルド時間を早めたい！ということくらい。

![名称未設定ファイル.drawio(2).png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3355973/c8cfc845-3176-e06b-0da9-d73037547024.png)




# 手順1. 別モジュールを用意する
1.メニューバーのFile→New→New ModuleでCreate New Moduleウィンドウを表示する
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3355973/ceab3fb6-3bde-ac9d-185b-90659db4c55e.png)

# 手順2. Android Libraryを作成する
__必ず、Android Libraryにしてください。__
TimberはAndroid特化のログライブラリになってて、内部で「android.util.Log」を参照してます。
そのため、Java/Kotlin Libraryで作成すると失敗します。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3355973/4b741770-0b54-58a5-846a-846e1edb5807.png)

# 手順3. build.gradleにimplementionを追加する
`groovy
dependencies {
    implementation 'com.jakewharton.timber:timber:5.0.1'
}
`

# 手順4. sync実行
ここは特に問題ないと思います。

# 手順5. Timberのimport
作成したモジュール内にロガー用のktファイルを作ってTimberをimportする
`kotlin
import timber.log.Timber
`
# 手順6. Timberを使う
`kotlin
fun LogInit(){
    Timber.plant();
}
`

# 終わりに
Timberを使うこと自体は簡単なんですが、Android Libraryを使用せずKotlin Libraryを使用して沼る人が一定層いそうなので共有です。
僕はAndroid開発が初心者なのもあってこの問題のせいで数時間沼りました。