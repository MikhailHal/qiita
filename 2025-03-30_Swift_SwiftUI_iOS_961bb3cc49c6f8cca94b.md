<!--
title:   iOS14+で使えるTextのリンク付与
tags:    Swift,SwiftUI,iOS
id:      961bb3cc49c6f8cca94b
private: false
-->
# Textに対するリンク付与について
SwiftUI Text関数で表示する内容について一部分だけリンク化したい、そういった経験はありませんか？
例えば、表示文章内に"ログイン"という部分文字列があり、当該部分のみ押下時ログイン画面へ遷移などしたい場合です。

本アプローチはそういった場合に適用が可能です。

# 手順
`Swift
Text("既にアカウントがありますか？その場合は[ログイン](navigateToLogin)しましょう！")
    .environment(\.openURL, OpenURLAction { specifiedParam in
        switch specifiedParam.description {
        case "navigateToLogin":
            // ログイン画面へ遷移
        default:
            Void()
        }
        return .handled
    })
`

このようにリンク化したい部分をmd形式のように表すことができます。
更に優れているのはリンク毎に付与した引数を受け取ってハンドリングすることができる点です。
この特性を利用して、URLを開く以外にも様々な用途に応用することが出来ます。(想定された使い方かどうかは別だけど...)

# ソース
[Apple Developerサイト]("https://developer.apple.com/documentation/swiftui/openurlaction")に記載されていました。
こちらは単純にURLを開く方法の紹介がされていますね。