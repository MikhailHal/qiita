<!--
title:   【Xcode 16 / Swift 6.0】SPMを用いたSwiftLintの導入方法について
tags:    SPM,Swift,SwiftLint,iOS
id:      f107d37acdb451ce4bc6
private: false
-->
# 環境内容
Xcode: 16.2
Swift: 6.0.3

# 手順
1. "Add Package Dependencies"を押下
![スクリーンショット 2025-02-16 21.29.35.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3355973/35859c34-8e03-4894-9a45-3891edea600b.png)

2. 右上の"Search or Enter Package URL"へ https://github.com/realm/SwiftLint を追加
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3355973/9b4517c0-dea2-4cb6-911e-4a762fe36ec8.png)

3. "Add Package"を押下する
4. "Add to Target"をどちらもNoneにする
:::note alert
"None"にしないと後程の手順でビルドエラー頻発します！！
:::
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3355973/f4e2e09b-18f5-4725-ad9a-4cbed93c3a2a.png)
5. "Add Package"を押下する
6. ルートプロジェクトを押下
7. SwiftLintを適用したいTARGETSを押下
8. "Build Phases"を開く
9. "Run build Tool Plug-ins"を開く
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3355973/9392fd03-e149-4db7-965c-981c69783949.png)
10. "+"アイコンを押下
11. "SwiftLintBuildToolPlugin"を選択し、"Add"を押下
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3355973/0e928cd5-61cf-421b-b891-424c9c0fc953.png)
12. 静的解析に引っかかりそうな命名などを行いビルドしてエラーが発生したら成功
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3355973/11f1b15a-d4b0-4d68-8851-31066d084bfb.png)

# まとめ
規約については、.swiftlint.ymlを使用すれば変更が可能。

SwiftLintの導入は意外と難しい。
特に、手順4の際、普段通りTargetを指定してしまうと解決に非常に時間を要する。