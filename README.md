# template-UI

template for starting to practice frontend

## 前提

* npmがインストールされている
* VSCodeのようなエディタがインストールされている

## ゴール

openapiで記述した定義ファイルから、フロントエンド開発時に使えるモックサーバを建てる。  

## 手順

### openapi定義ファイルの作成

`openapi.yaml`を作成する。  
今回は[Basic Structure - Swagger](https://swagger.io/docs/specification/basic-structure/)から記述を拝借してちょっとだけ加筆。  
(のちに追記するかも)  

作りたいフロントのページに必要なデータ構造に合わせて、今後ここにそれっぽいパスを定義していく。  

### Prismのインストール

Prismは、openapi定義のファイルからモックサーバを建てられるOSSツール。

```bash
$npm install -D prism

# 既にpackage.jsonがリポジトリにある場合は:
$npm install
```

### モックサーバの起動

```bash
$npx prism mock ./openapi.yaml -p 8080
# [8:07:10 PM] › [CLI] …  awaiting  Starting Prism…
# [8:07:10 PM] › [CLI] ℹ  info      GET        http://127.0.0.1:8080/user
# [8:07:10 PM] › [CLI] ℹ  info      GET        http://127.0.0.1:8080/users
# [8:07:10 PM] › [CLI] ▶  start     Prism is listening on http://127.0.0.1:8080
```

起動時にエンドポイントのURL一覧が表示されるので、早速叩いてみる。  
(`http`コマンドが便利。macでhomebrewが入ってるなら`brew install httpie`でインストール可能)

```bash
$http GET http://127.0.0.1:8080/user
# HTTP/1.1 200 OK
# Access-Control-Allow-Credentials: true
# Access-Control-Allow-Headers: *
# Access-Control-Allow-Origin: *
# Access-Control-Expose-Headers: *
# Connection: keep-alive
# Content-Length: 11
# Content-type: application/json
# Date: Wed, 20 Oct 2021 11:07:45 GMT

"hoge taro"
```

```bash
$http GET http://127.0.0.1:8080/users
# HTTP/1.1 200 OK
# Access-Control-Allow-Credentials: true
# Access-Control-Allow-Headers: *
# Access-Control-Allow-Origin: *
# Access-Control-Expose-Headers: *
# Connection: keep-alive
# Content-Length: 39
# Content-type: application/json
# Date: Wed, 20 Oct 2021 11:08:30 GMT

[
    "hoge taro",
    "huga jiro",
    "piyo saburo"
]
```

あとはフロント側で呼び出すときのbase_url(ローカル開発用)に`http://127.0.0.1:8080/`を設定するだけ。  

* 毎回`$npx prism mock ./openapi.yaml -p 8080`も面倒だから、dockerコンテナで立ち上げておいたら楽かもしれない
* フロントの環境もdockerコンテナにまとめて……とかしたらもっと楽かもしれない

元気があったらフロント側のサンプルも追加します。

## 参考

おすすめのopenapiを記述するGUIエディタ: [stoplight](https://stoplight.io/studio/)
