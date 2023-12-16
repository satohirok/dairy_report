## 1:RailsのためのRuby入門
* メソッド:Rubyのオブジェクトの振る舞いのこと
* Rubyのようなオブジェクト思考の言語では、意味的な共通性のあるデータを１つオブジェクトに集約して扱えるようにクラスを作る。
* ローカル変数:その場限りの一時的変数。メソッド内で定義した変数はそのメソッド内でしか使うことができない。
  * railsにおけるローカル変数:1つのメソッドの中で「一時的」に使うデータを参照するために使う。
* インスタンス変数:オブジェクトのどのメソッド内からも利用できる変数。
  * 特定のオブジェクト内部で使い回したり、そのオブジェクトに属するデータとして外部からゲッターを通じて利用されるために用いる。
* 真偽:nilとfalseが偽、それ以外が真

## 2:Railsのアプリケーション
* gem:railsを用いた開発において、便利なライブラリ群のこと
* bundler:開発で使用するgemをまとめて管理
  * bundle install:Gemfileに記述したgemをインストールするコマンド。Gemfile.lockが同時に生成され、gemの依存関係が記録される。
  * bundle exec:Bundlerが管理するgemを利用できる状態でコマンドを実行する。
* layoutファイル:アプリケーションのヘッダーやフッタなどの共通レイアウトがまとめられているファイル。
* database.yml:データベース接続するためのDB接続情報設定ファイル。
  

## 3:タスク管理アプリケーションを作ろう
* 雛形作成手順(docker)
  * docker-compose run web rails new システム名 -d postgresql
  * docker-compose build
  * docker-compose up -d でコンテナを立ち上げる
  * docker-compose run web　rails db:createでデータベースの作成
* テンプレートエンジンであるslimはruby3.2以上だとインストールできなかった。
* bootstrap導入手順
  * gemに記述→bundle
  * app/assets/stylesheets/application.css→application.scssに書き換える
  * 上記に``@import "bootstrap"``を記述
* エラーメッセージの日本語化
  * Gemfileに``gem "rails-i18n", "~> 7.0.0"``を記述→bundle
  * config/application.rbに下記を記述
  ```
  config.i18n.available_locales = %i[ja en]
  config.i18n.default_locale = :ja
  ```
  * config/locales/ja.ymlを作成   
* simple_formt:改行文字を含むテキストをブラウザ上で表示させる時に使われるヘルパー
  * 文字列を``<p>``で囲む
  * 改行には``<br/>``を付与
  * 連続した改行については、``</p><p>``を付与
