---
title: "スタートアップのCTOとして何を考え何を変えたか"
emoji: "🏋🏻"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["ポエム","マネジメント","プロジェクト","リファクタ"]
published: true
---

## はじめに
平下CTO@sweeepです。sweeepにフリーランスとして関わったのが2021年5月、CTOに就任したのがその年の6月半ばでした。まだ1年経過していませんが、sweeepのCTOに就任してから何を考えて何を変えたのかについて書いていきたいと思います。
以下の内容となっております。
1. タスクファーストからチームファーストへ
2. 会計指標で考える
3. 技術負債とアーキテクチャの刷新
4. 現在地と将来展望

## 1. タスクファーストからチームファーストへ
---

### タスクファーストで社内受託状態
就任当初はBizからDevへ直接要望や修正を依頼するという、タスクファースト状態でした。Notionに積まれた要望や修正のタスクをこなす日々。はっきりってうまくいってなかったです。

![](/images/ca773bfae331ff/task.png)

その時の開発者の心の声：
* 変更した箇所に対し、使えなくなったと言われるけど、どっちが正しい？
* こっちの仕様の方がいいと思うけど、マネージャーからなので言いづらい（言えない）
* これは本当に必要？何のためにやってる？

社内受託状態で疲弊していました。また、個別最適≠全体最適ではないため、プロダクトの価値がTotalで下がっていても要望した側の責任が曖昧な状態でした。

### チームファーストへ変更
なので就任後すぐに、まずはチームファーストに変えました。
BizとDevの間にPdMを配置し、Biz ↔ PdM ↔ DevとしてPdMに交通整理をしてもらいしました。

![](/images/ca773bfae331ff/team.png)

PdMが全体最適の視点で、プロダクトの価値をTotalで上げるもの、対応が必要な修正など取捨選択するように変更しました。

## 2. 会計指標で考える
---
### 会計指標で抽象化
どういった効果を期待したか会計指標で抽象化して考えてみます。以下のROXX松本さんのスライドが秀逸なので、引用します。

> [プロダクトを中心に考える#とは / focus on the product - Speaker Deck](https://speakerdeck.com/kotamat/focus-on-the-product)
> ![](/images/ca773bfae331ff/accounting.png)


上記会計指標に、先ほどのBiz ↔ PdM ↔ Devをマッピングします。

![](/images/ca773bfae331ff/role.png)

以下のような役割をBiz ↔ PdM ↔ Devで担い、それぞれの責任範囲が明確になりました。
* Devはアジリティを上げ、生産性G/P(Gross/Productivity)を上げることでプロダクトの価値へつなげます。
* PdMはDevの生産性を元に効率的にプロダクト価値を高め、B/S(Balance/Sheet)の将来価値をストックする役割を担っています。
* Bizはプロダクト価値を元に現在価値のP/L(Profit/Loss)、現在のキャッシュを稼ぐことを担っています。

開発の生産性を高め、プロダクトの価値を高め、売上につなげる、ことを期待しました。

### タスクファーストで何が起こっていたのか？
タスクファーストの状態ではG/P、B/S、P/Lはそれぞれ以下となっていました。
* G/P：社内受託状態、また、アーキが組織構成にあっていなかったので技術負債（後述）がたまり低下
* B/S：G/P低下のため拡大できない（むしろ個別最適≠全体最適ではないためB/S減）
* P/L：ゆるやかにP/Lも下がる

![](/images/ca773bfae331ff/before.png)


## 3. 技術負債とアーキテクチャの刷新
---
### 技術負債について
また、プロダクトもローンチから3年経過し、技術負債がかなりたまっていました。
* コード：大規模化。同時に開発するとコンフリクトやデグレを起こしやすい状態
* DB：大量のテーブル・データ。こちらもデータのデグレでコンフリクトを起こしやすい状態

![](/images/ca773bfae331ff/old-archi.png)

少人数で開発していた時は問題なかったのですが、開発人員増・開発規模増につれてツラミがましていました。

### アーキテクチャーの刷新
先日、文書のオンライン受け取りと電子保管などの新サービス ー クラウドキャビネットAI [sweeep Box](https://lp.box.sweeep.ai/)を立ち上げました。
https://lp.box.sweeep.ai/

アーキテクチャーを以下のように刷新しています。

* マイクロサービス化
各サービスとDBを分離し、コンフリせずにそれぞれを少人数で開発できるようにしました。

![](/images/ca773bfae331ff/new-archi.png)


* スキーマ駆動開発
スキーマ定義ファイルからクライアントとサーバーのプログラムを自動生成したものを使用し、クライアントとサーバー間で齟齬が生じないようにしました。また、定義ファイルを元にMockを作成することで、サーバーの開発が終わっていなくてもクライアントがMockへアクセスし通信部分の開発や動作確認できるようにしています。

![](/images/ca773bfae331ff/schema.png)

## 4. 現在地と将来展望
---
### sweeep Boxの現在地

sweeep Boxはスクラムで開発し（後述）G/P、B/S、P/Lは以下変化しました。
* G/P：組織構成にあったアーキ選定とスクラム開発でアップ
* B/S：G/Pがアップすればプロダクト価値は蓄積される
* P/L：プロダクト価値が上がれば、売上はついてくるはず

![](/images/ca773bfae331ff/after.png)

まだ、仮説・検証している段階ではありますが、いずれG/P → B/S → P/Lと数字に反映されると思ってトライしています。

### チームファーストからプロダクトファーストへ
sweeep Boxはリリース後、開発スプリント→QAスプリントの流れでスクラム開発しています。

> [アジャイル開発とウォーターフォール開発の違いは何？アジャイル開発の手法や意味も要チェック | Backlogブログ](https://backlog.com/ja/blog/what-is-agile-and-waterfall/)
> ![](/images/ca773bfae331ff/scrum.png)

```
A機能開発スプリント　
→　A機能QA後デプロイ　&　B機能開発スプリント
→　B機能QA後デプロイ　&　C機能開発スプリント
→　C機能QA後デプロイ　&　D機能開発スプリント
...
```

次の開発スプリントとQAスプリントを並行実施することでアジリティを高め、週次での新機能リリースを続けてプロダクト価値を高めています。
### スタートアップとベンチャーの違い
あらためて、なぜ開発のアジリティを上げ生産性をあげる必要があるのでしょうか？スタートアップでは資金が枯渇する前にできる限りの仮説・検証トライすることでプロダクトの価値を最大化し、サービスをスケールさせ、次のラウンドへいく確度を高める必要があります。一般的なスタートアップはベンチャーとは以下の違いがあります。
* スタートアップは調達した資金を元に赤字を掘りながら、事業をスケールさせる
* 調達した資金が枯渇する前に事業をスケールさせて次の資金調達ラウンドへ行く
* 必ずスケールする保証はないので仮説・検証 → ピボットが必要

> [スタートアップとは何か？5分でざっくり掴めます。｜note](https://note.com/shomaesho/n/na0e95bb63e02)
> ![](/images/ca773bfae331ff/startup.jpg)

これまでの取組みで中規模の変更であれば数スプリント、大規模変更（ピボット）でも2-3か月あれば対応可能な体制が取れていると実感しており、より確度は高まっていると思います。

### 今後の展望
開発体制とアーキテクチャの刷新により、生産性を上げる仕組みは整いました。今後はG/Pをさらに倍増させる仕組みを作り、仮説・検証を繰り返すことでプロダクト価値を高め、サービスの成長曲線を描いていきます。
* G/P：プロダクトファースト・増員で2〜3倍
* B/S：G/P拡大させ、仮説・検証を繰り返すことでプロダクト価値も拡大
* P/L：成長曲線を描く

![](/images/ca773bfae331ff/future.png)

課題としては、
* プロダクトファーストの継続とブラッシュアップ
* 増員する仕組み作り
* 増員してもG/Pが下がらないようにする

などがあります。

## おわりに
今回はスタートアップのCTOとして何を考え、何を変えたのかについてまとめました。まだ道半ばですが手ごたえは感じており、会社やチームとともにCTOとしてもさらなる成長を目指します。

### 参考サイト

文中で引用させていただいたROXX松本さんのスライドです。

https://speakerdeck.com/kotamat/focus-on-the-product

また、以下のnoteの記事も秀逸です。

> [CTOの頭の中：技術を財務で表現する｜Shin Takeuchi｜note](https://note.com/singtacks/n/nb7a63ad40c17)
> ![](/images/ca773bfae331ff/asset.jpg)

https://note.com/singtacks/n/nb7a63ad40c17

ここまで読んでいただきありがとうございました！