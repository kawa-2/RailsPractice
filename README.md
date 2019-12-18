# README

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

Heroku https://rails-environment.herokuapp.com/

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

* このリポジトリのHeroku 
https://enigmatic-citadel-01574.herokuapp.com/

------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------


## 第3章

#### 概要

・新たなRailsアプリケーションを作成。
・コントローラーを新規作成。
・ルーティングの設定。
・Railsのビューでは、ERBが使える。（provideとyield）
・テスト駆動開発を実践。
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














