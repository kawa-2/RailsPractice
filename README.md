# README

* 第1章で作成したアプリのリポジトリ<br>
https://github.com/kawa-2/environment

* 第2章で作成したアプリのリポジトリ<br>
https://github.com/kawa-2/toy_app

* 第3章で作成したアプリのリポジトリ<br>
https://github.com/kawa-2/sample_app

* 第4章以降のアプリのリポジトリ<br>
https://github.com/kawa-2/sample_app


## 第1章

#### 学習事項

* RailsとRubyのバージョンが合わないと動かない為、実行するPJによってバージョンを確認する<br>
今回、Railsバージョン5.1.6を動かすには、rubyバージョン2.3.1では動かなかった為、2.6.5をインストール
* Gemfileの内容を変更したらbundle install の実行を忘れずに。
* rails new hello_app :Railsアプリ構造でhello_appというディレクトリを生成することができる。
* rails serverでRailsサーバーを実行し、http://localhost:3000/ で確認。
* Herokuへpushすれば同時にデプロイされる（デプロイコマンド: git push heroku master）

#### 苦戦したところ

* 開発環境づくりに苦戦。
Dockerで環境構築は初心者にはハードルが高く感じてしまい断念。（今後Dockerでの環境構築をできるようにする）<br>
最終的にローカルで環境構築を行った。

* ローカル環境構築手順：https://railsgirls.jp/install
brew install yarn を実行
→linkを貼る権限がないとのことでエラーがでる。（/usr/local/include に書き込み権限がない）
→権限を変更しようとしましたがそもそも変更の権限がないため、新たにディレクトリを作成し書き込み可能にした。
```
sudo mkdir /usr/local/include
sudo chmod 775 /usr/local/include
sudo chown <user名>:admin /usr/local/include
```

#### メモ

* Heroku https://rails-environment.herokuapp.com/
* 第1章で作成したアプリのリポジトリ　https://github.com/kawa-2/environment

------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------

## 第2章

#### 概要

toyアプリケーションを新規作成し、scaffold機能を使用しユーザーとマイクロポストのデータを作成。<br>
作成したwebアプリケーションでユーザー登録、マイクロポストが新規作成できる仕組みを学習。<br>
コードに、文字数制限・空でcreateできないようバリデーションを追加。

#### 学習事項

* 生成したファイルをリモートリポジトリへプッシュする方法
```
$ git remote add origin git@〜〜:<username>/<ディレクトリ名>.git
$ git push -u origin --all
```
* Railsはアプリケーションの構成にMVC (Model-View-Controller) を採用している
* scaffoldはアプリケーションの雛形を生成してくれるジェネレーター<br>
以下コマンドでUserのデータモデルが生成される（以下はnameとemailをパラメータに追加している）
```
$ rails generate scaffold User name:string email:string
```
* REST(REpresentational State Transfer)アーキテクチャ<br>
└ RESTは、インターネットそのものやWebアプリケーションなどを構築するためのアーキテクチャのスタイルの1つ<br>
RailsにおけるRESTとは、アプリケーションを構成するコンポーネント (ユーザーやマイクロポストなど) を「リソース」としてモデル化することを指す。<br>
CRUD（クラッド）、HTTP requestメソッドに対応<br>
→　このスタイルを採用することで設計が楽になる。


#### つまづき箇所

* rails consoleの使い方<br>
rails consoleはRailsアプリケーションを対話的に操作することができる。
ただ、CREAT、INSERTなどができてしまうので、確認したいだけであればirbを使った方が良い。

* Herokuへのpushが　$ git push heroku だと下記メッセージが出てpushできず。<br>
$ git push heroku master と書けばpushできた。<br>
```
fatal: You are pushing to remote 'heroku', which is not the upstream of
your current branch 'master', without telling me what to push
to update which remote branch.
```



#### メモ

* マイグレーション
Rails4以前のバージョンではrailsコマンドではなくrakeコマンドを使う必要がある。
```
Rails5
 $ rails db:migrate

Rails4以前
 $ rake db:migrate
```

* このリポジトリのHeroku https://enigmatic-citadel-01574.herokuapp.com/

* 第2章で作成したアプリのリポジトリ　https://github.com/kawa-2/toy_app

------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------


## 第3章

#### 概要

・新たなRailsアプリケーションを作成。<br>
・コントローラーを新規作成。<br>
・ルーティングの設定。<br>
・Railsのビューでは、ERBが使える。（provideとyield）<br>
・テスト駆動開発を実践。<br>
・同じコードを繰り返し記述しないようにする。


#### 学習事項

* キャメルケース (単語の頭文字を大文字にしてつなぎ合わせた名前)<br>
StaticPagesコントローラ名をスネークケース (単語間にアンダースコアを加えて繋ぎ合わせた名前) にしたファイルstatic_pages_controller.rbを自動的に生成します。
```
$ rails generate controller StaticPages
```

* 生成したコードを元に戻したい場合<br>
コントローラを生成した後で、生成したコードを削除したくなった場合など<br>
└ コントローラの場合
```
  $ rails generate controller StaticPages home help
削除  $ rails destroy  controller StaticPages home help
```
└ モデルの場合
```
生成  $ rails generate model User name:string email:string
削除  $ rails destroy model User
```
└ マイグレーションの場合
```
変更  $ rails db:migrate
1つ前の状態に戻す  $ rails db:rollback
最初の状態に戻す   $ rails db:migrate VERSION=0
```

* テスト駆動開発のサイクル<br>
「red ・ green ・REFACTOR」<br>
「失敗するテストを最初に書く」「次にアプリケーションのコードを書いて成功させる (パスさせる）」「必要ならリファクタリングする」

* assert_select<br>
アプリケーションのビューのテストを行う際に使うアサーションメソッド<br>
assert_selectメソッドでは、特定のHTMLタグが存在するかどうかをテストすることができる<br>
```
例）assert_select "title", "Home | Ruby on Rails Tutorial Sample App"<br>
<title>タグ内に「Home | Ruby on Rails Tutorial Sample App」という文字列があるかどうかをチェック
```

* provideメソッド、yieldメソッド<br>
```
記述例
<% provide(:title, "Home") %>
<title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>
```

* ルーティングの設定<br>
config/routes.rbに記述<br>
```
記述例
Rails.application.routes.draw do
  root 'static_pages#home'
end
```

* Guardによるテストの自動化<br>
特定のファイルが変更されたら、特定のテストファイルを実行する仕組みを実行することが可能。<br>
Gemfileにguardを追加し
```
group :test do
  gem 'guard',  '2.13.0'
  gem 'guard-minitest',  '2.4.4'
end

```
guardを初期化
```
$ bundle exec guard init
```
Guardfileを下記のようにカスタマイズ(今回作成したページに特化しているので注意)
https://railstutorial.jp/chapters/static_pages?version=5.1#code-guardfile



#### メモ

* rails generate コマンドで生成されるファイル一覧参照<br>
https://maeharin.hatenablog.com/entry/20130212/rails_generate

* 空ファイル作成コマンド<br>
```
$ touch [ファイルディレクトリ＆ファイル名]
```

* Railsサーバーを立ち上げる
```
$ rails server
```

* テスト実行コマンド
```
$ rails test
```

* sample_appのHeroku <br>
https://still-river-30792.herokuapp.com/

* 第3章で作成したアプリのリポジトリ<br>
https://github.com/kawa-2/sample_app


------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------

## 第4章

#### 概要

* Rubyのメソッド呼び出し・定義、配列、ハッシュについて学習
* Rubyのクラス定義、クラス継承について学習

#### 学習事項

* ヘルパー<br>
viewにおけるメソッド。<br>
app/helpers/application_helper.rbこのファイル内に機能を書き込むことで<br>
見た目に影響するファイルapp/views/layouts/application.html.erbにその効果が現れる。<br>
RailsではApplicationHelperを自動でinclude(読み込み)してくれる。

* コンストラクタ<br>
インスタンスを作成した時に自動的に実行されるメソッド
```
>> s = "foobar"       # ダブルクォートは実は文字列のコンストラクタ
=> "foobar"
>> s.class
=> String
```

* ブロック

```
>> (1..5).each { |i| puts 2 * i }
2
4
6
8
10
=> 1..5
```
{ |i| puts 2 * i }が、ブロックと呼ばれる部分<br>
|i|では変数名が縦棒「|」に囲まれていますが、これはブロック変数に対して使うRubyの構文で、ブロックを操作するときに使う変数を指定します。



#### 基本メソッド（メソッド＝関数）

* puts<br>
文字列を出力する<br>
※putsを使って出力すると、改行文字である\nが自動的に出力の末尾に追加されます<br>
```
>> puts "foo" 
foo
```

* empty?<br>
中身が空か、文字列の中身が空の場合にtrueを返す。入れ物が存在するのが前提。<br>
※メソッドがtrueまたはfalseという論理値 (boolean) を返すことを、末尾の疑問符で示す慣習がある

* include?<br>
文字列に指定した文字列が含まれるか。配列の要素に指定した文字列が存在するか。<br>
※正規表現を使用して一致した文字列を取得したり、複数の文字列を検索することはできない。
```
オブジェクト名.include?("検索文字列")
```


* nil?<br>
レシーバがnilの場合はtrueを、nil以外の場合はfalseを返します。

* to_s<br>
文字列以外のオブジェクトを文字列に変換する。
```
オブジェクト.to_s
例えば数字を変換する場合は　10.to_s # => "10"
```
```
メソッドチェーンの例
nil.to_s.empty? 
```

* reverse<br>
要素の順番を反転させる

* split<br>
文字列を自然に変換した配列に分割する
```
>>  "foo bar     baz".split     # 文字列を3つの要素を持つ配列に分割する
=> ["foo", "bar", "baz"]
```

* 「破壊的」メソッド<br>
配列の内容を変更したい場合使用。元のメソッドの末尾に「!」を追加したもの。
```
=> [42, 8, 17]
>> a.sort!
=> [8, 17, 42]
>> a
=> [8, 17, 42]
```

* push (または同等の<<演算子)<br>
配列の末尾に引数を要素として追加するメソッド

* join<br>
配列を連結して一つの文字列にする(splitと逆の動作)
```
>> a
=> [42, 8, 17, 6, 7, "foo", "bar"]
>> a.join                       # 単純に連結する
=> "4281767foobar"
>> a.join(', ')                 # カンマ+スペースを使って連結する
=> "42, 8, 17, 6, 7, foo, bar"
```

* to_a<br>
レシーバ自身を返す<br>
例)範囲オブジェクトを、配列に変換する
```
>> (0..9).to_a            # 丸カッコを使い、範囲オブジェクトに対してto_aを呼ぶ
=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

* %w<br>
配列を作る。<br>
```
>> a = %w[foo bar baz quux] 
=> ["foo", "bar", "baz", "quux"]
```

* downcase<br>
文字列に含まれる大文字を小文字に変換
```
文字列.downcase
```

* each<br>
主に配列の要素分の処理を繰り返し行いたい場合に使用するメソッド<br>
下記は基本的なeachの記述
```
オブジェクト.each do |変数|
  繰り返し処理
end
```
```
オブジェクト.each { |変数|
  繰り返し処理
}
```

* map<br>
配列の要素の数だけ繰り返し処理を行うメソッド。
```
配列の入った変数.map {|変数名| 処理内容 }
```

* inspect<br>
オブジェクトや配列などをわかりやすく文字列で返してくれるメソッド
```
オブジェクトや配列など".inspect

下記のように省略した記述が可能
>> p :name    # 'puts :name.inspect' と同じ
:name
```

* merge<br>
```
{ "a" => 100, "b" => 200 }.merge({ "b" => 300 })
=> {"a"=>100, "b"=>300}
```

* superclass<br>
クラス階層を調べることが可能
```
>> s = String.new("foobar")
=> "foobar"
>> s.class                        # 変数sのクラスを調べる
=> String
>> s.class.superclass             # Stringクラスの親クラスを調べる
=> Object
>> s.class.superclass.superclass  # Ruby 1.9からBasicObjectが導入
=> BasicObject
>> s.class.superclass.superclass.superclass
=> nil
```

* self<br>
オブジェクトそのものを指す
```
>> class Word < String             # WordクラスはStringクラスを継承する
>>   # 文字列が回文であればtrueを返す
>>   def palindrome?
>>     self == self.reverse        # selfは文字列自身を表します
>>   end
>> end
=> :palindrome?
```


####  知っておくと使えそう

* インデント番号<br>
-1を使うと、配列の長さを知らなくても配列の最後の要素を指定することができ、これにより配列を特定の開始位置の要素から最後の要素までを一度に選択することができます。

* Railsコンソールでもメソッドを定義することができる

* Rubyのメソッドには「暗黙の戻り値がある」<br>
メソッド内で最後に評価された式の値が自動的に返されることを意味する。


#### わからなかった点

* メソッドの定義の演習3<br>

結果、nilでtrueとなっているけどnil(空)なのか？
```
>> def palindrome_tester(s)
>>  if s==s.reverse
>>  puts "It's a palindrome!"
>> else
>>  puts "It's not a palindrome."
>>  end
>> end
=> :palindrome_tester
>> palindrome_tester("racecar")
It's a palindrome!
=> nil
>> palindrome_tester("onomatopoeia")
It's not a palindrome.
=> nil
>> palindrome_tester("racecar").nil?
It's a palindrome!
=> true
```

↓この答えは、<br>
putsは戻り値がないのでnilになる。nilなので.nil?の答えはtrueになる


* 演習4<br>
https://railstutorial.jp/chapters/rails_flavored_ruby?version=5.1#code-string_shuffle
```
>> def string_shuffle(s)
>>   s.split('').shuffle.join
>> end
>> string_shuffle("foobar")
=> "oobfra"
```
split(配列)にする必要がわからない<br>
↓この理由は<br>
.shuffleが配列(Array)に対してしか使えないクラスだから。文字列(String)には使えないクラス。
標準クラス・モジュール参考→ https://ref.xaio.jp/ruby/classes


#### メモ

* Heroku https://still-river-30792.herokuapp.com/
* 第4章で使用したアプリのリポジトリ　https://github.com/kawa-2/sample_app



------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------

## 第5章

### 概要

レイアウトを作成。
Bootstrapを組み込み、第4章までで作成したアプリにスタイルを追加。
さらにパーシャル、Railsのルーティング、Asset Pipelineについて学習。
作成したページにおいてのリンクテスト方法についても学習。

### 学習事項

* 条件付きコメント<br>
Microsoft Internet Explorer (IE) のバージョンが9より小さい場合 (if lt IE 9) にのみ、下記行を実行。<br>
IEのバージョンが9未満の場合にのみHTML5 shimを読み込める。<br>
```
<!--[if lt IE 9]>
      <script src="//cdnjs.cloudflare.com/ajax/libs/html5shiv/r29/html5.min.js">
      </script>
<![endif]-->
```

* Asset Pipeline<br>
JavaScriptやCSSのアセットを最小化 (スペースや改行を詰めるなど＝minify) または圧縮して連結するもの<br>
アセットディレクトリ、マニフェストファイル、プリプロセッサエンジンという、3つの主要な機能が理解の対象となる。

* アセットディレクトリ<br>
Railsのアセットパイプラインでは、静的ファイルを目的別に分類する、標準的な３つのディレクトリが使われている。<br>
・app/assets: 現在のアプリケーション固有のアセット<br>
・lib/assets: あなたの開発チームによって作成されたライブラリ用のアセット<br>
・vendor/assets: サードパーティのアセット

* マニフェストファイル<br>
アセットディレクトリに配置されたCSSとJavaScriptのファイルをどのようにまとめるか指示するもの<br>
→実際に処理を行うのはSprocketsというgem<br>
※CSSとJavaScriptには適用されますが、画像ファイルには適用されない。
```
・アプリケーション固有のCSS用マニフェストファイル
app/assets/stylesheets/application.css
----------
*= require_tree .
# app/assets/stylesheetsディレクトリ (サブディレクトリを含む) 中のすべてのCSSファイルが、アプリケーションCSSに含まれるようにしています。
*= require_self
# CSSの読み込みシーケンスの中で、application.css自身もその対象に含めています。
```

* プリプロセッサエンジン<br>
それぞれのファイルを読み込んで（ブラウザが読み込めるように）変換する仕組み<br>
プリプロセッサエンジンは、繋げて実行する (chain) ことができる。
```
foobar.js.erb.coffee
上の拡張子の場合は、CoffeeScriptとERbの両方で実行されます (コードは右から左へと実行されますので、この例ではCoffeeScriptが最初に実行されます)。
```

* ルーティング<br>
ルートURLを定義すると、root_pathやroot_urlといったメソッドを通してURLを参照することができる。<br>
前者はルートURL以下の文字列を、後者は完全なURLの文字列を返します。
```
root_path -> '/'
root_url  -> 'http://www.example.com/'
```

* 名前付きルート<br>
名前付きルート（○○_path）<br>
ルーティング用のファイル (config/routes.rb) で設定

* getルールを使って名前付きルートを定義
```
#routes.rbでgetルールを使って定義（helpページの例）
get 'static_pages/help'
#↑これを　こうする↓
get  '/help', to: 'static_pages#help'
#getリクエストが　/helpに送信されたとき,　static_pagesコントローラーの　helpアクションが　呼び出される
 
#上記を定義することで名前付きルートが使えるようになる
help_path -> '/help'
help_url  -> 'https://○○○○○.vfs.cloud9.ap-northeast-1.amazonaws.com/help'
```


* リンクのテスト<br>
リンクテストを自動化<br>
統合テストを使うと、アプリケーションの動作を端から端まで (end-to-end) シミュレートしてテストすることができる。
```
1.ルートURL (Homeページ) にGETリクエストを送る.
2.正しいページテンプレートが描画されているかどうか確かめる.
3.Home、Help、About、Contactの各ページへのリンクが正しく動くか確かめる.
```
```
・assert_templateメソッド
assert_template後にあるURLがビューを描画しているかをテストする。
```
```
・assert_selectメソッド
assert_select "a[href=?]", root_path, count: 2
特定のリンクが存在するかどうかを確認
Railsは自動的にはてなマーク "?" をabout_pathに置換
countでリンクのコスも調べられる
```


* Rakeタスクとは<br>
Rakeタスクとは、ターミナルなどのコマンドライン上からアプリケーションを実行できる機能の一つ。<br>
```
今回のアプリで統合テストが通るかのコマンド
$ rails test:integration
```
Rakeとは：Rubyで書かれたビルドツール（プログラムを制御・統合するもの）


### 覚えておくと使えそう

* 画像をダウンロードする
$ curl -o app/assets/images/rails.png -OL railstutorial.jp/rails.png


### メモ
* Heroku https://still-river-30792.herokuapp.com/
* 第4章で使用したアプリのリポジトリ　https://github.com/kawa-2/sample_app


------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------

## 第6章

### 概要

ユーザー登録ページを作成。
本章では、ユーザー用のデータモデルの作成と、データを保存する手段の確保について学習。
バリデーションでデータに関して文字数制限等加えることが可能。
また、セキュアなパスワードの追加方法についても学習。


### 学習事項

* Railsでは、データモデルとして扱うデフォルトのデータ構造のことをモデル (Model) と呼ぶ

* Active Record<br>
データベースとやりとりをするデフォルトのRailsライブラリ。<br>
データオブジェクトの作成/保存/検索のためのメソッドを持っている。

* Userモデルを生成(例)<br>
$ rails generate model User name:string email:string<br>
※コントローラ名には複数形 Users モデル名には単数形 User を用いる。<br>
※マイグレーションファイルも生成される。
```
(usersテーブルを作るための) Userモデルのマイグレーション
db/migrate/[timestamp]_create_users.rb

class CreateUsers < ActiveRecord::Migration[5.0]
  def change
    create_table :users do |t|
      t.string :name
      t.string :email

      t.timestamps
    end
  end
end

・t.timestampsコマンド
created_atとupdated_atのマジックカラムを作成
```

* コンソールでのモデルの生成<br>
User.new

* コンソールでのモデルの保存<br>
user.save

* コンソールでのモデルの生成＆保存<br>
User.create

* Active Recordでの、オブジェクトを検索方法

1)<br>
User.find(3)<br>
id=3のユーザー<br>
2)<br>
User.find_by(email: "mhartl@example.com")<br>
3)<br>
User.first<br>
データベースの最初のユーザー<br>
4)<br>
User.all<br>
すべてのUserオブジェクトが返ってくる


* データベース更新方法<br>
1)<br>
・例)user.email = "mhartl@example.net"<br>
上記変更をデータベースに保存するためには最後に、user.save を実行する<br>
2)<br>
・user.update_attributes(name: "The Dude", email: "dude@abides.org")<br>
成功時には更新と保存を続けて同時に行う<br>


* バリデーション<br>
制約を課すことが可能。<br>
・存在性 (Presence)、長さ (length)の検証、フォーマット (format)の検証、一意性 (uniqueness)の検証を今回行なった。

* ユーザーの認証<br>
認証が行われる手順：パスワードの送信、ハッシュ化、データベース内のハッシュ化された値との比較。

* セキュアなパスワード<br>
セキュアなパスワードを実装するのに必要なhas_secure_password(パスワードをハッシュ化)を使えるようにするには、<br>
password_digest属性をモデルに追加し、Gemfileにbcryptを追加する<br>


### 覚えておきたいコマンド

* マイグレーション実行コマンド
```
$ rails db:migrate
```

* サンドボックスモード
```
$ rails console --sandbox
```
変更をコンソール終了時に取り消してくれる


### メモ

* HerokuでUserモデルを使用する為には、Heroku上でもマイグレーションを走らせる必要がある<br>
```
$ heroku run rails db:migrate
```

------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------


## 第7章

### 概要

* ユーザー登録機能を追加。<br>
* webページにフォームを作成し登録情報を送信可能に。<br>
* ユーザーを新規作成しユーザー情報をデータベースに保存。<br>
* 作成されたユーザーのプロフィールページを作成。<br>
* ユーザー登録失敗・成功テスト。<br>
* 本番環境のSSL化。


### 学習事項

* デバック情報をレイアウトに表示させる方法(開発環境でのみ)<br>
```
<%= debug(params) if Rails.env.development? %>
※トラブルが発生してそうなコードの近くに仕込む
```

* RESTfulなUsersリソースで必要となるすべてのアクションが利用できるようにするには<br>
Usersリソースをconfig/routes.rbに追加する<br>
記述：resources :users

* デフォルトでは、ヘルパーファイルで定義されているメソッドは自動的にすべてのビューで利用できる。


* 埋め込みRuby<br>
```
埋め込みRuby記述
<%= f.label :name %>
<%= f.text_field :name %>

HTMLに展開された場合
<label for="user_name">Name</label>
<input id="user_name" name="user[name]" type="text" />
```

* SSLを有効化

本番環境用の設定ファイル config/environments/production.rb に
「本番環境ではSSLを使うようにする」という下記記述する
```
config.force_ssl = true
```


### メソッド

* any?<br>
要素が1つでもある場合はtrue、ない場合はfalseを返す。
empty?とは逆。

* content_tag<br>
htmlタグを生成することができる。

* params変数<br>
Railsで送られてきた値を受け取るためのメソッド

* yメソッド<br>
yml形式でオブジェクトの中身を出力できる

* putsメソッドを使ってparamsハッシュの中身をYAML形式で表示<br>
puts a.to_yaml

* flashメソッド<br>
何かの処理を行った際に『ログインに成功しました。』などwebページに表示するメッセージ<br>
4つのスタイルをもつ　：success、info、warning、danger<br>
※通常コントローラーからビューに変数を渡す時は変数の頭に@を付けるルールがあるが、flashメッセージには不要



### メモ

* 環境について<br>
```
$ rails console,$ rails serverのデフォルト環境はdevelopment(開発環境)
$ rails console testとすればテスト環境になる
$ rails server --environment productionとすれば本番サーバーとなる
$ rails db:migrateは本番環境
$ heroku run rails consoleは本番環境
```

* /users に対してHTTPのPOSTリクエスト送信する、といった指示<br>
```
<form action="/users" class="new_user" id="new_user" method="post">
```


* データベースリセット<br>
```
$ rails db:migrate:reset
```




------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------


## 第8章

### 概要
* webサイトでユーザーがログインやログアウトを行えるようにする。<br>
※ブラウザがログインしている状態を保持し、ユーザーによってブラウザが閉じられたら状態を破棄するといった仕組み (認証システム (Authentification System))<br>
* ログインフォームで無効な値を送信した場合の処理の実装。<br>
* ログイン中の状態での有効な値の送信をフォームで扱えるようにする。


### 学習事項


* ログインページ<br>
ログインページではnewで新しいセッションを出力し、そのページでログインするとcreateでセッションを実際に作成して保存し、ログアウトするとdestroyでセッションを破棄する、といった仕組み。

* GET login_pathとPOST login_pathの違い<br>
GET login_path： "/login"へアクセスされた際に"session#new"アクションを実行。<br>
POST login_path： "session#create"アクションの情報を"/login"へ送信。

* ユーザーのブラウザ内の一時cookiesに暗号化済みのユーザーIDが自動で作成する<br>
sessions_helper.rbに下記の通り記載<br>
```
 # 渡されたユーザーでログインする
  def log_in(user)
    session[:user_id] = user.id
  end
```

* flashとflash.nowの違い<br>
・flash  ：redirect_toを使う場合に設定<br>
・flash.now  ：renderを使う場合に設定。<br>
レンダリングが終わっているページで特別にフラッシュメッセージを表示することができる。<br>
flash.nowはflashとは異なり、リクエストが発生したときに消滅する。



* ヘルパーモジュールについて<br>
Railsの全コントローラの親クラスであるApplicationコントローラに<br>
各viewで使用するヘルパーのモジュールを読み込ませれば、どのコントローラーでも使えるようになる。
```
[例]
ApplicationコントローラにSessionヘルパーモジュールを読み込ませる
app/controllers/application_controller.rb
上記ファイルに下記コードを記述
include SessionsHelper
```


### メソッド

* session<br>
session[:user_id] = user.id
このコードを実行すると、ユーザーのブラウザ内の一時cookiesに暗号化済みのユーザーIDが自動で作成される。
cookiesメソッドとは対照的に、sessionメソッドで作成された一時cookiesは、ブラウザを閉じた瞬間に有効期限が終了する。


* find<br>
idの検索を行える

* find_by<br>
idの値がなくてもnilを返す。返ってくる結果は最初にヒットした1件のみ。<br>
※id及びid以外の条件が分かっている場合、その条件に該当する最初のデータを取得したい場合に使用する。



### コマンド


* 現状のルーティングを確認することができる。<br>
```
$ rails routes
```

* grepコマンド<br>
ファイルに特定の文字列が存在するか検索するときに使える。<br>
```
$ grep 検索文字列 ファイル名
```
※パイプ（｜）と組み合わせて、他のコマンドの出力結果から必要な箇所だけを絞り込んで表示する際によく使われる。<br>
ファイルは指定しなくてもOK


### メモ


* 認可モデル (Authorization Model)<br>
ユーザー限定ページなど、制限や制御の仕組みのこと。

* セッション<br>
HTTPはステートレス (Stateless) なプロトコル。<br>
HTTPのリクエスト１つ１つは、それより前のリクエストの情報をまったく利用できない、独立したトランザクションとして扱われる。<br>
セッションはHTTPプロトコルと階層が異なる (上の階層にある) ので、HTTPの特性とは別に (若干影響は受けるものの) 接続を確保できる。<br>
ユーザーのパソコンのWebブラウザとRailsサーバーなど に別途設定

* cookies<br>
ユーザーのブラウザに保存される小さなテキストデータ。<br>
あるページから別のページに移動した時にも破棄されないので、ここにユーザーIDなどの情報を保存できる。



### 苦戦したところ

* digestメソッドの理解<br>
以下、digestメソッド実装の細かな流れメモ<br>
```
---
・自分でfixtureファイルを作成してデータを追加する

有効な名前とメールアドレス、パスワードを用意する
Sessionsコントローラのcreateアクションに送信されたパスワードと比較できるようにする必要もある
→password_digest属性をユーザーのfixtureに追加

---
・digestメソッドを独自に定義する
has_secure_passwordでbcryptパスワードが作成されるので、同じ方法でfixture用のパスワードを作成します。


---
#secure_passwordのソースコードのパスワード生成部分
#string→ハッシュ化する文字列　cost→ハッシュを算出するための計算コスト
BCrypt::Password.create(string, cost: cost)
 
#テスト中は最小で本番環境ではしっかりなコストパラメータの計算的な事
cost = ActiveModel::SecurePassword.min_cost ? BCrypt::Engine::MIN_COST :
                                              BCrypt::Engine.cost
---
↓
上記を利用したfixture向けのdigestメソッドをUserモデルに追加
app/models/user.rb
---
  # 渡された文字列のハッシュ値を返す
  def User.digest(string)
    cost = ActiveModel::SecurePassword.min_cost ? BCrypt::Engine::MIN_COST :
                                                  BCrypt::Engine.cost
    BCrypt::Password.create(string, cost: cost)
  end
---
↓
digestメソッドを定義したので有効なユーザーを表すfixtureが作成できるようになる<br>

↓
test/fixtures/users.yml
---
michael:
  name: Michael Example
  email: michael@example.com
  #fixtureではERbを利用できる
  password_digest: <%= User.digest('password') %>
---

```



------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------

## 第9章

### 概要

* 永続的セッションを作成する。
* [remember me] チェックボックスを使って、ユーザーの任意でログイン情報を記憶する機能を追加。


### 学習事項


* 下記方針で永続的セッションを作成
```
1. 記憶トークンにはランダムな文字列を生成して用いる。
2. ブラウザのcookiesにトークンを保存するときには、有効期限を設定する。
3. トークンはハッシュ値に変換してからデータベースに保存する。
4. ブラウザのcookiesに保存するユーザーIDは暗号化しておく。
5. 永続ユーザーIDを含むcookiesを受け取ったら、そのIDでデータベースを検索し、記憶トークンのcookiesがデータベース内のハッシュ値と一致することを確認する。
```

* 記憶トークンと記憶ダイジェストをユーザーごとに関連付けて、永続的セッションができる。<br>
以下流れ<br>
```
ランダムな文字列を生成し、Userモデルの属性にデータを記憶させる（記憶トークン）
※SecureRandomモジュールにあるurlsafe_base64メソッドを使用
↓
ユーザーの暗号化済みIDと記憶トークンをブラウザの永続cookiesに保存して、永続セッションを作成する準備が整った
↓
cookiesメソッドを使用
cookies.permanent.signed[:user_id] = user.id
ユーザーIDをcookiesに保存する（暗号化された状態にする為にsignedを、永続化する為にpermanentを追加）
↓
ページビューで下記のようにcookiesからユーザーを取り出せるようになる
User.find_by(id: cookies.signed[:user_id])
※cookies.signed[:user_id]では自動的にユーザーIDのcookiesの暗号が解除され、元に戻る
↓
bcryptを使ってcookies[:remember_token]がremember_digestと一致することを確認
```


* ログアウトできるようにするには<br>
ユーザーを忘れるためのメソッドを定義(user.forget)<br>
user.forgetメソッドによって、user.rememberが取り消される。具体的には、記憶ダイジェストをnilで更新する。


* 三項演算子<br>
三項演算子を使うと、if-thenの分岐構造を1行で表すことができる

```
if params[:session][:remember_me] == '1'
  remember(user)
else
  forget(user)
end

↓

params[:session][:remember_me] == '1' ? remember(user) : forget(user)
```

### メソッド

* authenticate?メソッド<br>
引数の文字列がパスワードと一致するとUserオブジェクトを、間違っているとfalseを返すメソッド<br>
※has_secure_passwordを追加すると使える


### メモ

* 「トークン」とは<br>
パスワードの平文と同じような秘密情報。<br>
パスワードとトークンとの一般的な違いは、パスワードはユーザーが作成・管理する情報であるのに対し、トークンはコンピューターが作成・管理する情報である点。

















