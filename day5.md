# Webページを表示させよう
## Webページがブラウザに反映される仕組みとは？
+ 登場人物
    + ユーザー<br>
    使う人
    + Webブラウザ<br>
    WEBサイトを閲覧するために使うソフト
    + DNSサーバ
    メインとそれに対応するIPアドレスの管理をするシステム
    + Webサーバ
    WEBブラウザからのリクエストに応じて静的画面や画像などのホームページのデータをWebブラウザーに送ってくれるサーバー
+ 仕組みの図を見てみる
![仕組みを描いてみた](https://github.com/kakoyukiko/test1/blob/main/%E3%82%A6%E3%82%A7%E3%83%95%E3%82%99%E3%83%98%E3%82%9A%E3%83%BC%E3%82%B7%E3%82%99%E3%81%8B%E3%82%99%E8%A6%8B%E3%81%88%E3%82%8B%E7%90%86%E7%94%B1.png)
<br>

## mac上で簡単なHTMLファイルを書いて、ブラウザで開いてみる
主に[この記事](https://qiita.com/harakazu_nanfg/items/4fb0b0fffe4d1a80b058)を参考にした。
1. ターミナルを起動する
2. デスクトップディレクトリに移動　`cd Desktop`
3. サイトの親ディレクトリを作成 `mkdir steak_site`
4. サイトの親ディレクトリに移動 `cd steak_site`
5. HTMLファイルを作成 `touch index.html`
6. CSSディレクトリを作成 `mkdir css`
7. CSSファイルをCSSディレクトリの中に作成 `touch css/style.css` <br>
 CSSファイルたちはCSSディレクトリ下に作成するというコツがある。
8. メインで2種類の方法がある（今回はaでやったが、bの方が良さそう） <br>
a: nanoというテキストエディタでHTMLを開いてみる `nano index.html` <br>
b: 作った親ディレクトリをVSCodeにドラッグする <br> 
9. Chromeを開く
a: HTMLファイルを開くコマンド `open index.html`
b: VScodeの「index.html」をChromeにドラッグする
10. .htmlファイル内で以下のような内容を書き込んでおく
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My HTML Page</title>
</head>
<body>
    <h1>Hello, World!</h1>
    <p>This is a simple HTML page.</p>
</body>
</html>
```
11. Chromeを読み直したらページにHello World! This is a simple HTML page.と出てくる

## macにnginxを導入して起動し、ローカルで書いたHTMLファイルがwebブラウザに表示されるようにする

1. homebrewを使用して、nginxをインストール `brew install nginx`
2. nginxを起動 `(start) nginx`
3. どのポートを開いているのかlsofで確認 `lsof -c nginx -P | grep LISTEN` <br>
右の方に書いてある数字(8080)がポート番号なので、http://localhost:8080 にアクセスすると表示される、という仕組み。
4. nginxの設定ファイルをいじる <br>
   nginxの設定ファイルは、Macの場合/usr/local/etc/nginx/nginx.conf というパスに置かれている。<br>
   どのバスにあるのかは、webサーバの公式サイトに書いてあるらしい<br>
   `vim /usr/local/etc/nginx/nginx.conf`で設定ファイルに入る <br>
   次に、目的のHTMLファイルを提供するように書き加える <br>
   ```
   server {
       listen 8080;
       server_name localhost;

       location / {
        root /Users/brains2024/Desktop/steak_site (#index.htmlがあるディレクトリパスを指定)
        index index.html;
       }
    }
   ```

5. 設定ファイルの書き方に問題がないかテストする`nginx -t`
6. OKだったらnginxの設定ファイルを再読み込み `sudo nginx -s reload`
7. Chromeを開きindex.htmlのWebブラウザで先ほど作成したHTMLの画面が見えてたらOK
<br>

### エラーが出た時は...
よく、Chromeで開いてみたら404 not foundが出たりする。どこでエラーが出たのか調べるために、error.log機能を用いる。
error.logは　`/usr/local/var/log/nginx`パスの中にある。<br>
`vim error.log`で入るとerror履歴が出てくる。一番下が最新のもの。これでどこでエラーが出ているのかがわかる。
<br>

## mac上のDockerでnginxコンテナを起動し、ローカルで書いたHTMLファイルがwebブラウザで表示されるようにする

1. docker imageでnginxイメージをpullする `docker pull nginx`
2. コンテナを作る(細かいオプションは後で解説)
    ```
    docker run --name kako-nginx -p 8080:80 -v /Users/brains2024/Desktop/steak_site/:/usr/share/nginx/html:ro -d nginx
    ```
    <br>

    |-option|意味|
    |-|-|
    |--name|コンテナ名|
    |-p| ポート指定|
    |-v|バインドマウント|
    |-d|バックグラウンドで実行| 

    <br>

    + -p <ホストのポート>:<コンテナのポート>でローカルのポート番号が聞かれたらコンテナのポート番号へアクセスを振替
    <br>

    + バインドマウント：ホスト側のディレクトリをコンテナ内のディレクトリと共有する。コンテナ側のどこのディレクトリを指定したらいいかわからないときは、大体Dockrehubの[公式image HP](https://hub.docker.com/_/nginx)に載っている。
    <br>

    + バックグラウンド：プログラムがユーザーの見えないところで実行される
    <br>

3. ブラウザで確認する
   ブラウザで`http://localhost:8080/` にアクセスして、Nginxコンテナから提供されているHTMLファイルが表示されることを確認する。
<br>

## To do
+ CSSやJavaScriptを使ってWebページを華やかにしてみる。
+ Webサーバーにnginx公式Dockerイメージを使わず、ubuntu公式イメージを元にして同様の機能をもつイメージを作成してみる。また、作成したイメージをtar.gz形式として出力する。
+ docker-composeについて調べ、Webサーバーのコントロールに使ってみる。

## 今日の新しい学びの復習







