# フルスタックエンジニアが教える即戦力Railsエンジニア養成講座

## 目的
* MVCを正しく理解してコードが書けるようになる
* 保守性の高い綺麗なコードが書けるようになる
* Rspecなどのテストコードが書けるようになる
* gemを使って効率よく開発できるようになる。
* DBやインフラの知識(Postgres,Docker)
* フロントのコーディング(HTML/CSS,JavaScript)

## Ruby
* インタプリンタ言語
* 日本人が作っているので情報が充実している
* 構文の自由度が高い
* Rubyでは(）のない引数のメソッドは省略するのが好ましい書き方
* Symbolは整数なので、処理速度が高速になる
* %wは配列を作成する際、１つの要素を,ではなく、空白で区切る
* 親クラスを継承した子クラスでメソッドのオーバーライドを行い、そこから親クラスのメソッドを呼び出すにはsuperを記載する。

## rails
* rails5と7で<%link%>のdeleteメソッドの記述が異なる
* verや環境の違いによって動かないはザラにあるので、統一した環境下で動作できるdockerは偉大
* railsではbundlerというツールを指定して、アプリに必要なgemの依存関係の管理やインストールなどを行なう。
* railsの基本理念
  * 同じことを繰り返さない（１箇所にメソッドを整理して記述し再利用できるようにすることで保守性を高める）
  * 設定よりも規約が優先される。
* MVC
  * Controller:routeを介してユーザーからのリクエストを受けて、ModelとViewの仲介を行い、最終的にユーザーにWebページのコンテンツを返す。
  * Model:データの管理やデータの処理を行うコードを記述する。
  * View:ブラウザ表示のhtmlを作成する部分を担当する。erbを用いてhtmlの中にrubyを埋め込んで記述することができる。
* Gemfile
  * Gemfileにインストールしたいgemを記述→``bundle install``コマンドにて対象のgemがインストールされる。
  * dockerのコンテナ内で実行するために``docker compose run web bundle install``にて実行するとGemfile.lockが生成される。
  * Gemfile.lockに基づいてgemがインストールされるようにDockerfileにコピー命令を実行する必要がある。
 
* Bootstrap
  * 複数箇所記述する場所があるので後で復習しておく。→アウトプットのときにヤバくなったら（https://www.udemy.com/course/rails-kj/learn/lecture/39866104#questions）を確認する。
  * JSにおいても設定が必要
* CSS
  * 共通レイアウトの編集ファイルはapp > views > layouts > application.html.erb
  * app > assets > stylesheets > base.scss
  * aplication.scssファイルに記述したもの@importで読み込む。
* rails　generate
  * rails db:migrateでテーブルを作成
  * rails db:rollbackで直前のmigrateを取り消し
* rest
  * railsで使用するHTTPメソッドは、GET(取得),POST(作成),PATCH(部分更新),PUT(置き換え),DELETE(削除)の5つ
* rails
  * コントローラではmodelのクラスを使用することができる
  * form_withヘルパーメソッドを使用することで、サーバー間の通信に必要なname,idなどをより少ない記述で作成することができる。
  * コントローラー内で定義したインスタンス変数(@sample = Sample.new)はviewで使用することができる。
  * rails/info/routesで現在定義済みのルーティング情報を確認することができる。
  * viewに変数を渡す必要がない場合@変数名のインスタンス変数は用いず、ローカル変数を用いる。
  * scssでのスタイリングはapplication.scssで読み込むことで初めて適用される。
  * ActiveRecordとは、RubyとSQLの翻訳機のようなイメージ
  * 中間テーブル:２つの多対多のテーブルを繋ぐための中継的役割をするテーブル。
  * 多対多: 関連するテーブルのidを複数お互いに持っている状態
  * https://qiita.com/ramuneru/items/db43589551dd0c00fef9 (中間テーブル解説記事)
  * 
