---
layout: post
title: "The-Power-Of-REST (1)"
date: 2019-12-03 00:18:39 +0900
comments: true
categories: 
---

## RESTの力

今年2019年は、3月にphperkaigi 2019で**RESTの力**、12月にはphpカンファレンス 2019で**RESTの制約**の講演を行いました。

講演時間の短い`RESTの力`ではREST制約の中からキャッシュ制約とハイパーメディア制約を中心に紹介して、「プレゼンテーションそのものがハイパーメディアであり情報は勇気的に結合されて真の力を発揮する」というストーリーを語りたいと思いました。

それから9ヶ月の講演では、残りの制約を含めた9つ全ての制約を紹介して「表現に含まれるハイパーメディアコントロールでアプリケーション状態をコントロール」するRESTのより深い理解ができるものになれば良いと考えました。

* RESTの力 (30 min) https://fortee.jp/phperkaigi-2019/proposal/777a19ee-2d1a-483d-a457-f72ef0bf5fbe
* RESTの制約 (60 min) https://fortee.jp/phpcon-2019/proposal/9fa39956-77db-4a1d-a5b9-db7bf608ae55

{% img /images/phperkaigi2019/the-power-of-rest.001.jpeg %}

この画像はWikipediaの[ハイパーテキスト](https://ja.wikipedia.org/wiki/%E3%83%8F%E3%82%A4%E3%83%91%E3%83%BC%E3%83%86%E3%82%AD%E3%82%B9%E3%83%88)
で公開されてる画像でキャプションには「Hypertext Editing System (HES) IBM 2250 ディスプレイ・コンソール – ブラウン大学、1969年」とあります。

以下は[Hypertext Editing System](https://ja.wikipedia.org/wiki/Hypertext_Editing_System)からの引用です。

> Hypertext Editing System (HES) は、1967年、ブラウン大学の Andries van Dam の指導のもと、テッド・ネルソンらが行った初期のハイパーテキスト研究プロジェクト。HES は先駆的なハイパーテキストシステムであり、データを「リンク」と「分岐するテキスト」で構成している。分岐するテキストは自動的にメニューを構成し、所定の領域内のポイントは「ラベル」と呼ばれる名前を持つことができ、その名前でスクリーン上でアクセスすることができる。

> このシステムは近い将来、利便性向上のために改良・適用されるだろう。ハイパーテキストの様々な機能を実現するサブプログラム群や新機能を追加するサブプログラム群が含まれる。
  
>  1. 複数の対話利用ユーザーを相手にするためのファイル管理の問題
>  1. テキストの属性を知るには、ユーザーがタイプするかライトペンを使えばよい。既存のプログラムで、ユーザーは属性判定の真理値関数やキーワード検索でテキストを検索することができる（たとえば、犬か猫に言及しているが、ハムスターには言及していない部分を探すなど）。これらキーワードの索引も自動的に生成可能である（既存のプログラムで）。
>  1. ユーザーは属性を注釈やラベルに割り当てることもできる。タグに割り当てる属性はシステムが恒久的に定義するもの（「文献目録」タグや「注釈」タグ）とユーザーが定義するものがある。システム定義のタグ属性は他の「文献目録設定」プログラムなどで利用される。全てのタグは属性によってインデックス付けされる。
>  1 スクリーン上でのハイパーテキストの自動的ルーティングや印刷時の自動順序選択など新たな機能の追加が考えられる。ユーザーはとりうる代替案を指定する。これはブッシュの「連想の航跡」とかエンゲルバートのトレイルマーカーに相当する。
>  1. 他にもユーザーの仕事を単純化したり明確化する様々な機能が考えられる。複数のウィンドウをスクリーン上に生成し、様々な文書を同時に表示して作業できるようになるだろう。例えば、ある文章をコピーして別の文書に埋め込むなどである。
>  1. ハイパーテキストが複雑化すると何をしているか見失う可能性がある。そこで我々は文書構造をグラフ的に表示する方法を考えている。部品の少ない状態のグラフは簡単に描けるが、数百のリンクを描くにはどうすべきだろうか？
>  1. ユーザー定義の操作モードの種類は増えていく。そこで活動状況レイアウトに戻る機能が必要となる。ウィンドウのレイアウトとしてはスタック状にして、必要なウィンドウが前面に表示されるようにする。
>  1. ブラウン大学で開発済みの Sketchpad プログラムと連結した拡張グラフィックス機能を追加する。
>  1. 編集履歴を保持し、文書の内容をその任意の時点の状態に戻す機能が考えられる。
>  1. 我々はライトペンよりも適したユーザーインターフェイスに強い関心を持っている。データタブレット、SRIのマウス、指でポイント可能な透過スクリーンなどが操作性向上をもたらすと思われる。テキスト編集ハードウェア向けの機器設計は今後重要な分野となる。
>  1. 長期的には、このようなシステムはあらゆるテキスト処理に使われるようになると見込まれる。ネルソン（Nelson, 1967）が主張するように印刷物を代替するようになるかどうかは予測できない。しかし、その実用性と有用性は明白である。
    
2019年のインターネットを知ってる我々にとってこの1967年の初期のハイパーテキスト研究プロジェクトの考察は大変興味深いもので驚くべきものです。

現代のハイパーテキストシステム（WWW)では1のファイル管理も問題はクライアント・サーバ制約や階層化制約、キャッシュ制約によって解決がなされています。
GUIによるコンピューティングの予見もライトペンのようなキーボードに加えた新しい入力デバイスの必要性や、開発されたばかりの"Sketchpad プログラム"への注目に見ることができます。

バージョニングシステムを持った文章管理や、文章構造をグラフで捉えることとその可視化、複数の対話利用ユーザー、まるで半世紀後の今のWWWを語ってるようです。
その彼らは「ネルソン（Nelson, 1967）が主張するように印刷物を代替するようになるかどうかは予測できない」としつつも「その実用性と有用性は明白である」との確信を得てます。

そして半世紀が経ち、世界の情報の大部分がハイパーテキスストで繋がれました。この白黒写真の背後には壮大なストーリーがあり、縦型モニタのハイパーテキストをタッチペンで操作してる様子には未来を感じます。好きな写真でいくつもの講演で同じこの写真を使ってます。

{% img /images/phperkaigi2019/the-power-of-rest.003.jpeg %}
{% img /images/phperkaigi2019/the-power-of-rest.004.jpeg %}
{% img /images/phperkaigi2019/the-power-of-rest.005.jpeg %}

「ハイパーテキスト」の概念に初めて言葉を与えたのはテッドネルソンです。1963年にハイパーテキスト、それをテキスト以外のメディアにも拡張したハイパーメディアは1965年に発表されました。

そのテッドネルソンのYouTubeの映像があります。

https://www.youtube.com/watch?v=Bqx6li5dbEY

> "the world is a system of ever-changing relationship and structures struck me as a vast truth"

> "It is so difficult to visualize the world as a system of ever changing interconnections."

> "Humanity has no decent writing tools!" 

>> "I'm the only sane person in the computer world.

「世界は絶えず変化する関係と構造のシステムである」... 5才の時にボートから手を水に入れた時のその手にまとわりついて水の流れからその着想を得たのは驚きですが、以来"インターコネクション"が自分の思索の中心となったと。
情熱を持ってそのアイデアを語る、その良さがこの映像には溢れてます。この映像を何十回見たかわかりません。

{% img /images/phperkaigi2019/the-power-of-rest.006.jpeg %}
{% img /images/phperkaigi2019/the-power-of-rest.007.jpeg %}

ハイパーメディアの大衆化がなされたのは1987年登場の[HyperCard](https://ja.wikipedia.org/wiki/HyperCard)で、私は学生だったのですが無理をして高価だったMacintosh SE/30を購入し夢中になった覚えがあります。 オブジェクト指向に初めて触れたのもHyperCardでした。

{% img /images/phperkaigi2019/the-power-of-rest.008.jpeg %}

{% img /images/phperkaigi2019/the-power-of-rest.009.jpeg %}
{% img /images/phperkaigi2019/the-power-of-rest.010.jpeg %}

1991年に初めてのWWWサーバーが立ち上がります。バーナーズリーは確かにWebの父であり、人類史に残るような偉大な功績がある事は間違いないと思いますが
インターネットというインフラにハイパーテキストというソフトウエアを動かしたもので両者はそれまでにあったものです。
単に一人の偉人が生み出したというだけのものではなく、HESから脈絡と続いた多くの人の夢とアイデアが結実した物という事を伝えたいと思いました。

{% img /images/phperkaigi2019/the-power-of-rest.011.jpeg %}

Webを支える3つの技術。

構造化された文章を相互接続するハイパーテキストマークアップ言語 HTML、世界のあらゆる情報にIDを与えるURI。分散ハイパーテキストシステムを可能にするアプリケーションプロトコル HTTP。
当たり前となってるこれらの技術は1つ1つ大変興味深いものです。

HTMLはテキストではなく構造と接続性を与えられていてそれが大きな力になっています。しかし接続性は大変不完全なシステムです。バージョン管理はされて無いですし、接続の保証もありません。一方的に参照できるだけのものです。

全ての情報にURIをつけるという試みは大変野心的だったもののように思えます。世界レベルでユニークキーが発行できるのですが、誰の何の許可も入りません。呟きの1つ1つにIDがついてそれらは全てユニークです。
Webが全てFlashでできたパラレル世界があったとしたら情報共有がどれだけ不便でしょうか。

分散コンピューティングはインターネットのネイチャーで、特にハイパーテキストシステムに必須では無いような気もしますが大変相性の良いものであったと思います。
革新的だったHyperCardもアップル1企業の製品に過ぎず、異なるアーキテクチャのマシンをつなぐことができませんでした。WWW登場直後にはそういう風にHyperCardとの比較した記事を見かけたように思います。


{% img /images/phperkaigi2019/the-power-of-rest.012.jpeg %}

PHP登場。その最初の投稿です。
https://groups.google.com/forum/#!msg/comp.infosystems.www.authoring.cgi/PyJ25gZ6z7A/M9FkTUVDfcwJ

そのラスマス氏に2015年にインタビューをしています。

https://gihyo.jp/news/report/2015/12/1401

よくあるインタビュー後の握手写真（？）のような物を楽しみにしてたのですが、インタビュー終了時にカメラマンの方はもういませんでいた。丸めた背中の写真ばかりというのは英語での出版がなかったこと含めて残念な思い出です。
インタビューは読む価値のある満足いくものができたと思います。

{% img /images/phperkaigi2019/the-power-of-rest.013.jpeg %}

96年3月、アップルを追い出された後の若きJobsが世界初のWebアプリケーションサーバをリリース。コンテンツをダイナミックに生成する新しい時代を宣言します。

https://www.youtube.com/watch?v=DmCu97u35Ek

「Web Act 1ではスタティックなコンテンツをブラウズできるだけだったものが、これからのWeb Act 2の時代ではユーザーのリクエストに応じて、今まで存在しなかったページが"on the fly"で生成されるんだ！」

今では当然の事を高らかに宣言します。Webの始まりは1991年ですが"、"ダイナミックWebサービス"の時代は1996年に始まったと言って良いのかもしれません。そう考えるとまだ今年でもまだ23年です。
私たちの巨大な産業がいかに若いということだと思います。（ちなみにこのプレゼンテーションで驚くべのはプレゼンが終わった後の技術的なQAも全てJbosが受けてるところです。びっくりします。）

https://ja.wikipedia.org/wiki/WebObjects

WebObjectは高価でした。最初はエンタープライズバージョンが700万円もしましたが、後に5万円に値下げされ、最後は無料になりました。
AppleでもiTunes StoreやApple Online Storeで長い間使われていましが、今はどうなってるか分かりません。

世界最初のORMと言われるEnterprise Objects Frameworkが統合されていました。

https://ja.wikipedia.org/wiki/Enterprise_Objects_Framework

{% img /images/phperkaigi2019/the-power-of-rest.014.jpeg %}

2000年にフィールディングのREST論文が発表されますが、Webの成功の後に書かれた論文です。理論や設計を元にHTTPが実装されたのではなく、先にHTTPを作ってから書かれたものです。

{% img /images/phperkaigi2019/the-power-of-rest.015.jpeg %}
{% img /images/phperkaigi2019/the-power-of-rest.016.jpeg %}
{% img /images/phperkaigi2019/the-power-of-rest.017.jpeg %}

その彼が2008年に「REST APIにハイパーテキスト駆動出なくてなはならない」というブログ記事を書きます。
ただのHTTP APIがREST APIと呼ばれてる事にストレスを感じると。

https://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven

{% img /images/phperkaigi2019/the-power-of-rest.018.jpeg %}
{% img /images/phperkaigi2019/the-power-of-rest.019.jpeg %}
{% img /images/phperkaigi2019/the-power-of-rest.020.jpeg %}

どういう事でしょうか？
Webのアーキテキチャにはいくつか種類があり、ハイパーメディアスタイルのREST APIと、単なるリモートサーバーのストレージ操作や関数実行のRPC("REST" API)とは違うのです。

誤用が一般化してるため、混乱を避けるために本来のREST APIはHypermedia APIと呼ばれるようになってます。

{% img /images/phperkaigi2019/the-power-of-rest.021.jpeg %}
{% img /images/phperkaigi2019/the-power-of-rest.022.jpeg %}
{% img /images/phperkaigi2019/the-power-of-rest.023.jpeg %}
{% img /images/phperkaigi2019/the-power-of-rest.024.jpeg %}
{% img /images/phperkaigi2019/the-power-of-rest.025.jpeg %}

URIに関して終わりのない議論が続けられますが、"RESTFul URI"というものは本来ありません。
RESTにとって本当に大事なのはリンクです。URIは本来クライアント側で組み立てるものではありません。

{% img /images/phperkaigi2019/the-power-of-rest.026.jpeg %}
{% img /images/phperkaigi2019/the-power-of-rest.027.jpeg %}

REST API にとってのバージョンニングのベストクプラクティスは何でしょうか？

{% img /images/phperkaigi2019/the-power-of-rest.028.jpeg %}

「しない事」です。

