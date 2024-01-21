## 現在やったこと
* itemテーブルに加えて、cartテーブルと両テーブルを多対多の関係で結びつける中間テーブルcart_idテーブルを作成

* ``schema.rb``
```schema.rb
  create_table "carts", force: :cascade do |t|
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end

  create_table "item_carts", force: :cascade do |t|
    t.bigint "item_id", null: false
    t.bigint "cart_id", null: false
    t.integer "amount"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
    t.index ["cart_id"], name: "index_item_carts_on_cart_id"
    t.index ["item_id"], name: "index_item_carts_on_item_id"
  end

  create_table "items", force: :cascade do |t|
    t.string "item_id"
    t.string "name"
    t.integer "price"
    t.string "description"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end
```
* ``models/item.rb``
```models/item.rb
class Item < ApplicationRecord
  has_one_attached :image
  has_many :item_carts
  has_many :carts, through: :item_carts
end

```
* ``models/item_cart.rb``
```models/item_cart.rb
class ItemCart < ApplicationRecord
  belongs_to :item
  belongs_to :cart
end
```
* ``models/cart.rb``
```models/cart.rb
class Cart < ApplicationRecord
  has_many :item_carts
  has_many :items, :through: :item_carts
end
```
* current_cartメソッドを作成し、cart_idを作成する。
* helperメソッドでviewから呼び出せるようにする
* application.contorollerに記述することで他のコントローラーから呼び出せるようにする

```application.rb
 class ApplicationController < ActionController::Base
  helper_method :current_cart

  private
  def current_cart
    @current_cart = Cart.find_by(id: session[:cart_id])
    @current_cart = Cart.create unless @current_cart
    session[:cart_id] = @current_cart.id
    @current_cart
  end
end
```

## 現在抱えている問題
* 上記の中間テーブルを用いた多対多の３つのテーブルを設計し、どうViewとコントローラを実装して保存していけばいいのかわからない状態
* 加えて、今回はログイン機能なしで商品にてカートに保存した「状態」をセッション機能にて保持する仕様になっており、どのタイミングでセッションを発生させるのか？
* 商品一覧画面の「カートに追加」ボタンをクリックすると、カートに商品が追加されるようにする。
* Viewからパスを送る形？その際にセッションはどう取得し、そのセッションをどう結びつけ、適切なデータをどこにどう保存するのかがわからない。

## 参考記事
* 中間テーブル→ https://qiita.com/Lotuswhite/items/0794b468ecc627e01ae1
* 中間テーブルへの保存→ https://blog.to-ko-s.com/intermediate-table-save/
* 中間テーブルで使用するメソッド → https://qiita.com/k-penguin-sato/items/7a3550a0ec935b5ceb5d#many-to-manyhas_many_through
* セッション　→ https://qiita.com/ozackiee/items/4ee774c81b2a0c571c05
