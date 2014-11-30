ec-cubeAPI
=======

## 通信方式

HTTP メソッドをリソースの操作とする。
  　POST：create
  　GET：read
  　PUT：update
  　DELETE：delete

## URL

```
/api/[リソース]/
```

  例）
  * GET ~/api/customer/
    + 顧客IDの一覧を取得する

  * POST ~/api/customer/
    + 新規の顧客データを作成する

  * [DB系]
    + /api/db/customer/
    + /api/db/product/
    + /api/db/order/

  * [振る舞い系] ※要検討
    + カート情報確認
    + カートに入れる
    + カート削除
    + お届け先の指定
    + 支払方法、お届け日時の指定
    + 購入情報最終確認
    + ログイン	/api/login/
    + ログアウト	/api/logout/
    + メール送信

## パラメータ

パラメータはJSON形式で渡す。

-----------------------------

#### GETパラメータ例

```
~/api/customer/?json=
{
    offset: 100,
    limit: 50,
    sort: {"id":"asc", "create_date":"desc"}, 
    filter: {
        "name": ["test", "aaaa", "test example"],          // name に test と example を両方含む
         -> where name = test or name = aaaa or (name = test and name = expamle)
        "kana": ["hoge"]
         -> and kana = "hoge"
    },
    include: [ "id", "name", "*"]      // 取得するカラム
}
```


#### POSTパラメータ例

```
~/api/customer/?json=
{
    include: [ {"name":"ほげほげ"}, {"kana":"ホゲホゲ"}]
}
```


#### PUTパラメータ例
```
~/api/customer/?json=
{
    filter: {
        "id": "1",
        "name": "test example",          // name に test と example を両方含む
        "name": [ "1test", "example" ]   // name が test または example に完全一致
    },
    include: [ {"name":"ほげほげ"}, {"kana":"ホゲホゲ"}]
}
```

#### DELETEパラメータ例
```
~/api/customer/?json=
{
    filter: {
        "id": "1",
        "name": "test example",          // name に test と example を両方含む
        "name": [ "1test", "example" ]   // name が test または example に完全一致
    }
}
```

-----------------------------

## レスポンス

HTTPステータスコード「200,201」：OK (POSTの場合のみ、201)
HTTPステータスコード「200,201以外」：NG

-----------------------------

| APIの処理の結果 | 返されるHTTPレスポンス | ステータス・コード | Body |
|-----------------|------------------------|--------------------|------|
| 対象データの取得・削除に成功した場合 | 200 OK | 対象となるデータが格納される {success: true, customer:{"id":xxxx,…}, error_message: ""} |
| 対象データの追加に成功した場合 | 201 Created | {success: true, error_message: ""} |
| 対象データの更新に成功した場合 | 200 OK | {success: true, error_message: ""} |
| 対象データが存在しない場合 | 404 Not Found | {success: true, error_message: ""} |
| リクエストに検証エラーが存在する場合 | 400 Bad Request | 検証エラー情報が格納される {success: false, error_message: "xxxx"} |
| メソッド内で例外が発生した場合 | 500 Internal Server Error | エラー情報が格納される {success: false, error_message: "xxxx"} |

-----------------------------


## レスポンスフォーマット

Json形式

## 認証

OAuth 2.0？
→今回の合宿ではこの部分は無視

## コードサンプル

-----------------------------
/api/db.php（ルーティング）
    require(DB_API_Custoer.php)
    
    obj = new DB_API_Custoer();
    
    obj->get();
    obj->post();
    obj->put();
    obj->delete();
-----------------------------

-----------------------------
    class DB_API_Custoer {
      function get() {
      }
    
      function post() {
      }
    
      function put() {
      }
    
      function delete() {
      }
    }
-----------------------------


## 参考

http://developer.sakura.ad.jp/cloud/api/1.1/

