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

* Heroku https://rails-environment.herokuapp.com/
* 第4章で使用したアプリのリポジトリ　https://github.com/kawa-2/sample_app



