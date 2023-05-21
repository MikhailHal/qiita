<!--
title:   Android PageObjectデザインパターンについて - ページ部
tags:    Android,Kotlin,PageObjectDesignPattern,UI,デザインパターン
id:      83f4f8a037058cff8509
private: false
-->
# 概要
本記事は、デザインパターンの一種である"PageObject"パターンを紹介します。
おそらく、以下の二部に分かれますと思います。
　・ページ部
　・シナリオ部

また、今回使用する言語はKotlinを想定しています。

# PageObjectとは？
Seleniumの公式docを貼り付けておくのでそちらを参照してみてください
↓↓
https://www.selenium.dev/ja/documentation/test_practices/encouraged/page_object_models/

一応自分なりに解説してみると...。
アプリケーションは様々な画面から構成されていますよね。
例えばショッピングサイトでは、ログイン画面、ユーザー情報画面、商品画面など...。
挙げれば果てしないですが、とにかく様々な画面で構成されています。

PageObjectでは、それらの各ページを1つのオブジェクトとして捉えるのです。
今の段階では、何言ってるのやら...という感じかもしれませんがコードを読めばある程度は想像できます。
また、PageObjectは以下の2つのファイルに分かれます。
・ページ部
・シナリオ部

各部がどういった役割なのかは下に記載します。

# 考え方
今回は、お試しなので簡単なログイン画面にPageObjectを適用しステップバイステップで考えてみましょう。
① ファイルを用意する
以下2つのファイルを用意してみます。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3355973/d1b7bdcd-7bbf-7abe-ecd4-a365a5323c66.png)

★LoginPageOperations(ページ部)
　　→　ログインページで行えるユーザー操作をカプセル化して記載します。例えば、ログインするためにユーザーは必ずパスワードを入力しますし、その後にログインボタンを押しますよね。
そういったユーザーが行う処理を記載します。

★LoginPageTests(シナリオ部)
　　→　LoginPageOperationファイルにて用意した処理を呼んでテストシナリオを記載するファイルです。
後でコードで解説するためサラッとですが、ページ部から処理を呼んでユーザーが行う操作をシナリオ化します。

2. LoginPageOperationsを記載する
` Kotlin
class LoginPageOperations {
    /**
     * Emailアドレス入力機能
     * @param email セットするEmailアドレス
     * @return LoginPageオブジェクト
     */
    fun setEmail(email: String) : LoginPageOperations{
        Espresso.onView(ViewMatchers.withId(R.id.edittext_email)).perform(
            ViewActions.replaceText(email)
        )
        return this
    }

    /**
     * Password入力機能
     * @param password セットするパスワード
     * @return LoginPageオブジェクト
     */
    fun setPassword(password: String) : LoginPageOperations{
        Espresso.onView(ViewMatchers.withId(R.id.edittext_password)).perform(
            ViewActions.replaceText(password)
        )
        return this
    }

    /**
     * ログイン機能
     *
     * 本テストは事前にEmail及びPasswordがセットされていることが前提である。
     * そのため、必ずテスト関数 : setEmail() & テスト関数 : setPassword()をコールすること
     *
     * @return LoginPageオブジェクト
     */
    fun login() : LoginPageOperations{
        //ログインボタン押下
        Espresso.onView(ViewMatchers.withId(R.id.LoginButton))
            .perform(ViewActions.click())
        return this
    }
}
`
ここで注目してほしい点があります。
* メソッドの分け方
上記のコード内には
setEmail => Emailアドレスのセット処理
setPassword => パスワードのセット処理
login => ログイン処理
上記3つの処理がありますね。
これは、ユーザーが行える操作単位に分割したものです。
ユーザーがログインする為には、メールアドレスのセット→パスワードのセット→ログイン実行の3ステップが必要なわけです。
これらをシナリオ側から呼ぶことでテストを実行していきます。

* 各メソッドの戻り値
戻り値を見てみると全て
`kotlin
return this
`
としていますよね。これはメソッドチェーンというものを行うためです。
後のシナリオコードを読めば分かりますが非常に可読性が優れています。
後で詳しく記載しますが、以下のようにシナリオコードを書けます。
`kotlin
loginPageOperation
    .setEmail("メアド")
    .setPassword("パスワード")
    .login()
`
メアドセット→パスワードセット→ログインを順番でやっていることが分かりますよね。
こういった書き方が出来るのがPageObjectの特徴でもあります。
ここでページ部については完成ですね。
次は、シナリオ部になります。
ページ部で書いたコードを呼んでテストを行っていきます！