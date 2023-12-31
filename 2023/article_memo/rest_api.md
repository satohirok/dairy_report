# REST API メモ
- Web API:機能やデータを外部から呼び出して、利用できるように定めた規約の作り方 + サービスを実装すること
    - 独自機能ではなく一般的ば機能を外付けで実装することが多い。
    - 第三者が情報を利活用して新たなサービスを作り出す。
    - 冪等:何度実行しても同じ状態が再現されること。

- REST:分散型システムにおける設計ルール
    - 分散型システム:多数のコンピュータをネットワークで接続し、作業を分担しながら稼働しているシステム。
    - クライアント/サーバー:画面(UI)とデータで役割を分離する。クライアント側がトリガー、サーバー側が受け身。
    - 階層下システム:Web,AP,DBと役割ごとに階層を分けて管理するシステム。
    - コードオンデマンド:クライアントコードをダウンロードして実行できる。リリース後にクライアンコードを変更できる。
    - 統一インターフェイス
        - リソースの識別:サーバー側に保存されたデータをURIを用いて識別する。
        - 表現を用いたリソース操作:断面情報を利用してサーバー上のデータを操作する。
        - 自己記述:メッセージ内容が何であるか、ヘッダーに記述されている。
        - HATEOAS:レスポンスに現在の状態を踏まえて関連するハイパーリンクを含ませること。
        - メリット:システムの構造全体が簡素化されてわかりやすくなる。提供するサービスに集中でき、独自の進化ができる。異なるブラウザでも同じような画面を表示できる。
        - デメリット:標準化によって効率が犠牲になる。
    - ステートレス:前の状態を保存せず、それぞれの状態ごとに独立して意味が成立する。
        - サーバーはリクエストだけでコンテキストを理解することができる。
        - メリット:単一のリクエスト以外見る必要がないので、監視が容易。障害発生したリクエストだけ回復すればいいので、障害復旧が容易。リクエスト全体でサーバーリソースを共有する必要がないので、スケールが容易。
        - デメリット:単一のリクエストで完結させるため、リクエストデータに重複がある。
    - キャッシュ制御:クライアント側でキャッシュを管理すること。
    - REST API成熟レベル
        - Level0:HTTPを使っている
        - Level1:リソースの概念を導入
        - Level2:HTTPの動詞を導入
        - Level3:HATEOASの概念を導入
- REST WebAPIサービス設計(基本)
    - URI設計で考慮すること
        - シンプルで覚えやすいパスを設定する。
        - 省略文字を使用しないようにして、人が読んでも理解できるようにする。
        - 大文字小文字が混在しないようにする。(小文字で統一することが多い)
        - 単語はハイフンで繋げる。
        - 単語は複数形を利用する。
        - エンコードを必要とする文字を使わない。
        - サーバー側のアーキテクチャを反映しない。
        - 改造しやすいようにする。
        - ルールを統一すること。
    - HTTPメソッドとURI
        - HTTPメソッド(GET,POST,PUT,DELETE)とWeb APIを組み合わせることで、操作を変えることが可能となる。
    - リソースを特定するパラーメータ
        - クエリパラメータ:URLの末尾にある”?”に続くキーバリュー(省略可能)
        - パスパラメータ:URL中に埋め込まれるパラメータ(一意なリソース)
    - スタータスコード:処理結果の概要を把握する
        - 100番台:情報
            - 100:サーバーからリクエストの最初の部分を受け取り、まだサーバーから拒否されてない
            - 101:プロトコルの切り替えを要求
        - 200番台:成功
            - 200:リクエストが成功したことを示す
            - 201:リクエストが成功し、新しいリソースが作成されたことを示す。
            - 202:非同期jobを受け付けたことを示す。
            - 204:リクエストは成功したが、レスポンスデータがないことを示す。
        - 300番台:リダイレクト(REST APIではあまり使用しない)
        - 400番台:クライアントサイドに起因するエラー
            - 400:クライアントサイドに起因するその他エラー
            - 401:認証されていないことを示す。
            - 403: リソースに対してアクセスが許可されていない。
            - 404:リクエストされたリソースが存在しないことを示す。
            - 409:リソースが競合して、処理が完了できなかったことを示す。
            - 429:アクセス回数が制限回数を超えたため、処理できなかったことを示す。(レートリミット)
        - 500番台:サーバーサイドエラー
            - 500:サーバーサイドのアプリケーションにエラーが発生したことを示す。
            - 503:サービスが一時的に利用できないことを示す。
    - HTTPメソッドとステータスコード
        - GET:データ取得をするメソッド
            - 200:OK(正常)
            - 304:Not Modified(キャッシュ利用)
            - 400:Bad Request(クライアント側のリクエスト不備)
            - 401:Unauthorized(認証エラー)
            - 403:Forbidden(認可エラー)
            - 404:Not Found(該当データなし)
            - 500:Internal Server Error(サーバー障害)
            - 503:Service Unavailable(サーバー側が高負荷で応答負荷)
        - POST:データ登録をするメソッド
            - 200:OK(レスポンスに登録済データを含む)
            - 201:Created(レスポンスボディーが空、Locationに新しいリソースのURL)
            - 202:Accepted(非同期処理の受付が完了)
            - 400:Bad Request(クライアント側のリクエスト不備)
            - 409:Conflict(データが衝突)
        - PUT:データ更新/データ登録
            - 204:No Content(データ更新でレスポンスボディーにデータなし)
            - 404:Not Found(データ更新しようとした際に、該当データが存在しない
        - DELETE:データ削除するメソッド
            - 200:OK(データを含める機会はないのであまり使わない)
            - 202:Accepted(非同期通信)
            - 204:No Content(データ削除が成功)
    - レスポンスのデータフォーマット
        - XML:テキスト形式、タグで記述し、入れ子にし、属性をつけることができる。
        - JSON:テキスト形式、JSを元にしたフォーマット。XMLに比べてデータ量が減らせる。オブジェクトは入れ子にできる。
        - JSONP:テキスト形式、データフォーマットのように見えるが「JavaScriptコード」。クロスドメインでデータを受け渡すことができる。
    - データフォーマットの指定方法
        - クエリパラメータ:実サービスで利用が多い。
        - 拡張子:あまり見かけない
        - リクエストヘッダー:URIがリソースであることを考えると、リクエストヘッダーが一番王道？
    - データの内部構造
        - エンベロープ(レスポンスボディー内のメタ情報)は使用しない→レスポンスのヘッダー情報と役割が被る。構造はシンプルにして可読性を高めること。
        - オブジェクトはできるだけフラットにする。→ネストを減らし、レスポンスの容量を減らす。
        - ページネーションをサポートする情報を返す。→情報更新の可能性を考慮して、次をどこから取得するのか、キーとなる情報を返す。
        - 命名規則は統一
        - 日付はRFC3339(W3C-DTF)形式を使う。
        - 大きな数値(64bit整数)は文字列で返す。
    - エラー表現
        - エラー詳細は追加情報としてレスポンスボディーに入れる
        - エラーの際にHTMLが返らないようにする。:レスポンスフォーマットが変わるとクライアントアプリ側で処理できないケースがある。
        - サービス閉塞時は ”503” + “Retry-After”:クライアント側から見ていつから再会して良いかわかる。
- REST WebAPIサービス設計(応用)
    - APIにバージョンを含める
        - メリット:特定バージョン指定でアクセスできるので、クライアント側で突然エラーにならない。
        - デメリット:複数バージョンを並列稼働させるため、ソースコードやデータベースの管理が複雑になる。
        - 利用者の利便性を考慮して、APIバージョンを含めたURLの設計を行う。
        - APIバージョンを入れる場所
            - パス:実例上一番多いケース
            - クエリ
            - ヘッダー:非推奨→サービス固有の接頭辞をつける
        - APIのバージョンの付け方
            - セマンティックバージョニング
                - メジャー:後方互換しない修正
                - マイナー:後方互換する機能追加
                - パッチ:後方互換するバグ修正
            - APIは後方互換しなくなったタイミングでつけるのがおすすめ(メジャーバージョンのみ利用)
    - OAuthとOpenID Connectの違い
        - 認証と認可
            - 認証:本人を特定する
            - 認可:アクセス制御(ログインした後、特定のページにアクセスするかどうか)
        - OAuth:認可
        - OpenID Connect:OAuth + 本人情報取得
    - JSON Web Token
        - 特徴:署名による改ざんチェック。データの中身はJSON形式
        - 用途:認証結果をクライアントサイドで保持(ステートレスな通信の実現
        - 構成三要素
            - ヘッダー:署名で利用するアルゴリズムなどを定義
            - ペイロード:保存したいデータの実態(Sessionに保存するデータのようなもの)
            - 署名:改竄されていないかを確認するために署名
    - 大量アクセス対策
        - WebサービスをAPI化すると簡単に大量アクセスするプログラムが書ける
            - 時間あたりのアクセス制限をかける(レートリミット)
        - レートリミットで考慮すること
            - 誰に:APIキー、ユーザーID
            - 何に:単一機能、機能群、API全体
            - 制限回数
            - 単位時間
        - アクセル制限の緩和: アクセス元ごとに一時的に設定変更できる仕組みを考慮する
            - サービス利用が多く、自社にとって優良顧客である場合
            - キャンペーンなど、一時的に負荷増大がある場合

    - キャッシュ制御
        - 有効期限による制御
            - Expires:キャッシュとしていつまで利用可能かの期限を指定。過去日を指定すると「リソースが有効期限切れ」であることを示す
            - Cache-Control + Date:Cahe-Controlでキャッシュの「可否」「期限」を指定する。
        - 検証による制御
            - Last-Modified + ETag:リソースの最終更新日時を指定する。ETagに特定バージョンを示す文字列を指定
         
    - セキュリティ
        - APIはどこから呼ばれるのか
            - スマホアプリ、Webページ(Scriptタグ、JavaScript)、外部システム(バッチ)
            - APIにもWebサービスと同じようにセキュリティ対策が必要。
        - 脆弱対策
            - XSS:悪意のあるユーザーが正規のサイトに不正なスクリプトを挿入することで、正規のユーザーの情報を不正に引き出したり操作できてしまう問題
                - 対策:レスポンスヘッダーを追加する
            - CSRF:本来拒否しなければならないアクセス元(許可しないアクセス元)からくるリクエストを処理してしまう問題
                - 対策:許可しないアクセス元からのリクエストを拒否する。攻撃者に推測されてにくいトークンの発行/照合処理を実
            - HTTP:通信経路が暗号化されないので盗聴されやすい。
                - 対策:常時HTTPS（SSL/TLS）を利用した通信にする。Webで安全に通信するプロトコル。
            - JSON Web Token(JWT):クライアント側の内容の確認/編集が簡単にできるため、サーバー側の検証が不十分だと改竄された情報を正規の情報として受け入れてしまう。
                - 対策:ヘッダーのアルゴリズムにnone以外を指定して署名を暗号化する。ペイロードのaudに想定する利用者を指定して受信時に検証する。

## OpenAPI 3.0 / Swagger
* OpenAPI:WSDLやXMLと比較されるような"フォーマット"を意味する。このフォーマットを記述すると、「機械可読なREST API仕様」が記述できる。JSON,YAMLで記述する。
* OpenAPI Specification:OpenAPIを記述するための"書式ルール"のこと
* Swagger:OpenAPIを作成、表示、作成するためのツール群のこと。
*  
