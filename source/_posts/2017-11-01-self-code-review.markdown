---
layout: post
title: "Self Code Review"
date: 2017-11-01 17:26:28 +0900
comments: true
categories:
---

> 3つのエントリポイント
>
> 何はともあれ開始地点であるエントリポイントを見てみます。 webからのアクセス用と、CLI用とで3つありました。
> それぞれの違いは単純にコンテキストを変えているだけ。 そして bootstrap.php を呼んでいるのみ。

エントリポイントではコンテキスト(`$context`)を指定して`bootstrap.php`ファイルを読み込みます。

３つのファイルは単に例としてあるだけなので不使用なファイルは削除しても構いません。同様に新しいコンテキストファイルを追加するのも自由です。

要件によって条件を指定したファイルを使うこともできます。
例えば`/api/`で始まるパスの時にはAPIとしてJSONを返し、その他はHTMLで返すサービスの時はこのようにします。

```php Webコンテキストによるアプリケーションコンテキストの変更
$context = strpos($_SERVER['REQUEST_URI'], '/api/') !== false ? 'hal-api-app' : 'html-app';
```

> bootstrap.phpでは何をしているのか？
> このように非常に短いスクリプトで全体の流れを記述しているのみ。

bootstrapはブートの部分でもありますが、解説されてるようにスクリプト全体を記述しています。
さらに短く記述するとこのようになります。

```php bootstrap.php
$context = PHP_SAPI === 'cli' ? 'cli-hal-app' : 'hal-app';

$app = (new Bootstrap)->getApp('MyVendor\MyApp', $context);
$request = $app->router->match($GLOBALS, $_SERVER);
try {
    $page = $app
        ->resource
        ->{$request->method}
        ->uri($request->path)($request->query)
        ->transfer($app->responder, $_SERVER);
    exit(0);
} catch (\Exception $e) {
    $app->error->handle($e, $request)->transfer();
    exit(1);
}
```

バッチ処理など特定のリソースが呼び出されるならルーターも不要なのでこのようになります。

```php ミニマムなブートストラップコード
try {
    (new Bootstrap)->getApp('MyVendor\MyApp', 'prod-cli')
        ->resource
        ->post
        ->uri('/path/to/command')()
        ->transfer($app->responder, $_SERVER);
    exit(0);
} catch (\Exception $e) {
    $app->error->handle($e, $request)->transfer();
    exit(1);
}
```

さらに単にコンソールでechoする出力で例外処理も不要ならこのように一行になります。

```php 一行ブートストラップコード
echo (new Bootstrap)
->getApp('MyVendor\MyApp', 'prod-cli')
->resource
->post
->uri('/path/to/command')();
```

複数のアプリを組み合わせた出力を得たい場合には以下のようにできます。

```php 複数アプリを統合
$name = (new Bootstrap)->getApp('MyVendor\MyApp1', 'prod-cli')->resource->get->uri('/user')('id' => 'bear')['name'];
$role = (new Bootstrap)->getApp('MyVendor\MyApp2', 'prod-cli')->resource->get->uri('/role')('id' => 'bear')['role'];

echo json_encode(['name' => $name, 'role' => $role]);
```

アプリケーション全体を実行するスクリプトがフレームワーク本体にではなく、アプリケーションにあるので自由にカスタマイズできます。

> $app とは何者なのか？

DIのベストプラクティスとしてGoogleのGuiceでは以下の方法を勧めています。

> Your code should deal directly with the Injector as little as possible. Instead, you want to bootstrap your application by injecting one root object. The container can further inject dependencies into the root object's dependencies, and so on recursively. In the end, your application should ideally have one class (if that many) which knows about the Injector, and every other class should expect to have dependencies injected.


> 開発者のコードは、可能な限りInjectorを直接使うのを避けなければなりません。代わりに、1つのルートオブジェクトを注入してアプリケーションをブートストラップします。このルートオブジェクトのクラスは、依存する他のオブジェクト(`$app->router`や`$app->resourece`)を取得するためのDIする必要がり、依存するオブジェクトのクラスも同様に依存するオブジェクトのためのDIが必要です。
その代わりにルートの一つのオブジェクトに注入します。コンテナは、依存関係をルートオブジェクトの依存関係に注入を再帰的に行うことができます。
あなたのアプリケーションはInjectorについて知っている1つのクラスだけを持つのが理想です。その他のすべてのクラスは依存関係を注入することを期待するべきです。

つまり可能な限りインジェクターをユーザーが直接使うことを避けるべきです。それはアンチパターンのサービスロケーターです。
インジェクターを知るクラスを少なくなければなりません。BEAR.Sundayでは`Bootstrap`クラスの他にリソースのファクトリークラスがインジェクターを知っています。
オブジェクトは他のオブジェクトの依存になっている訳で、その一番ルートのオブジェクトが`$app`です。

`bootstrap.php`ではその`$app`のプロパティだけを使ってアプリケーションを実行します。

> AppInjector？

アプリケーションの`名前`と`コンテキスト`とインターフェイス（また抽象クラス）指定すると、インスタンスを取得することができるのがAppInjector（アプリケーション・ディペンデンシー・インジェクター）です。
BEAR.Sundayでは複数のアプリケーションがそれぞれ名前空間を持ち同時に存在できます。

`AppInjector`はプロダクションコードでは（Gooogle Guiceのベストプラクティスの通り）Bootstrapで一度使われるだけですが、テストに有用です。

無名クラスを使って以下のように、モックやスタブを束縛することができます。


```php 無名クラスで上書き束縛
public function testAnonymousClassBinding()
    $injector = new AppInjector('FakeVendor\HelloWorld', 'hal-app');
    $module = new class extends AbstractModule {
        protected function configure()
        {
            $this->bind(FooInterface::class)->to(Foo::class);
        }
    };
app');
    $index = $injector->getOverrideInstance($module, Index::class);
    $name = $index(['id' => 1])->body['name'];
    $this->assertSame('BEAR', $name);
}
```

[http://bearsunday.github.io/manuals/1.0/ja/test.html](http://bearsunday.github.io/manuals/1.0/ja/test.html)


> $contexts = cli-hal-api-app であれば`MyVendor\MyPackage\Module\AppModule`...の順番で読み込まれる。しかし優先順位はその逆である。

これはGofのデコレーターパターンです。

最初に`AppModule`で束縛されたDIとAOPの設定を外側で"デコレート" 変更しています。

モードで振る舞いを変更するのではなく、後読み優先のモジュールで束縛したクラスを変更して振る舞いを変えています。
`cli-hal-api-app`であれば`AppModule`でされている束縛はその後の`ApiModule`や`CliModule`で変更することができます。


```php
public function foo()
{
  $isDebug = Config::get('app.debug');
  if ($isDebug) {
    $this->logger->log($text);
  }
}
```

などとメソッド内でモードを判定して、振る舞いを変えるのではなく

```php
public function __construct(LoggerInterface $logger)
{
  $this->logger = $logger;
}

public function foo()
{
  $this->logger->log($text);
}
```

として`LoggerInterface`に対する束縛をモジュールで変更します。

> $requestとは？
> HTTPリクエストから、BEARで扱う形式への変換、マッピングというのがこの処理の肝なのではないだろうか。

その通りです。

ルーターの結果の値オブジェクト [RouterMatch](https://github.com/bearsunday/BEAR.Sunday/blob/1.x/src/Extension/Router/RouterMatch.php)です。
`$method`、`$path`、`$query`の値を保存します。`$path`がリソースクラスに、$methodがリソースクラスのメソッドに、
名前付き引数（named parameters)の$queryがPHPの（順序）引数(oredered parameters)に変換されます。

リソースクラスではWebコンテキストがどのような値を持っているのかを調べ回るようなコードは避けるべきで、Webコンテキストの値はクラスの外側で全て単なるPHPの値に変換しておくことが重要です。
リソースオブジェクトはWebコンテキストの値に関して無知にします。そうすることコンソールとWebのどちらでも実行が可能でテストが容易なコードになります。

```php
$page = $app->resource->{$request->method}->uri($request->path)($request->query);
```

> $pageとは？
>
パスで指定されたPHPのリソースオブジェクトクラスは、PSR7などでも採用されているイミュータブルオブジェクトとして扱われます。ユーザーは`$this`でオブジェクトのcode/headers/bodyの状態を変更します。

Webコンテキスト（`$_SERVER`の値など外部環境の値）を関数呼び出し
