<!--
title:   SwiftUI + Firebase使用時にPreviewが使えない悪魔的な罠
tags:    Firebase,GoogleCloud,Swift,SwiftUI,Xcode
id:      05c5e29cb43a5d639211
private: true
-->
# 環境
Xcode: 16.2
Swift: 6.0
Firebase: 11.9.0

# 問題
レイアウトファイルを開いても下記のようにロードが終わらない。
時折、「クラッシュしました」のような旨のメッセージが表示されたりもする。

# 原因
GoogleService-Info.plist(以降plistファイルと表記)が正しく配置されていない場合に起こり得る。
Firebaseを導入する際に、AppDelegateクラスにて下記のような記述を追記している。
`Swift
FirebaseApp.configure()
`
プレビュー時に上記処理が実行されるため、plistファイルが配置されていないことで正しく実行されない。

通常、Firebaseを使用しているにも関わらず、plistファイルを配置しないことは考えられない。
しかし、下記のような場合に直面しやすい。そして、プレビューとplistファイルの直接的な関係が見えにくいため気が付きにくい。
・GitHubへplistファイルをプッシュしていない(運用方針としては正しいです)
・clone直後、plistを配置する前に簡単にプレビュー内で画面のレイアウト感とかを確かめたい

# 解決策
## ケース1. plistファイルをリモートへプッシュ
無論、plistファイルのプッシュは非推奨です。
しかし、GitHubのプライベートリポジトリやGitLabなど無関係の第三者がアクセスできない環境であれば検討する余地はあります。

## ケース2. プレビュー時のみ処理を分岐
下記のように変更することで、プレビュー時のみFirebase初期化を回避することができます。
`Swift
#if DEBUG
    if ProcessInfo.processInfo.environment["XCODE_RUNNING_FOR_PREVIEWS"] != "1" {
        FirebaseApp.configure()
    }
#else
    FirebaseApp.configure()
#endif
`

## ケース3. clone直後にplistファイルを配置する
こちらは通常通りに配置するだけですので割愛します。

# まとめ
解決に苦労しました....
特に、私の場合はiOS開発は初心者のため、プレビューの流れなどは全く意識していませんでした。
そのため、プレビューとplistファイルの間接的な関係性は全く想定出来ていませんでした。