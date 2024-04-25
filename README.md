# docker上でwebサーバ（nginx）が稼働し、ブラウザからHTMLページが閲覧できる

## 手順

### ローカルでHTMLファイルを作ってページを表示させよう編

1. ローカルでHTMLファイルを作る
   1. ターミナルを起動する
   2. デスクトップディレクトリに移動 `cd Desktop`
   3. サイトの親ディレクトリを作成 `mkdir steak_site`
   <br>
2. サイトの親ディレクトリに移動 `cd steak_site`
3. HTMLファイルを作成 `touch index.html`
4. CSSディレクトリを作成 `mkdir css`
5. CSSファイルをCSSディレクトリの中に作成 `touch css/style.css` <br>
 *CSSファイルたちはCSSディレクトリ下に作成するというコツがある。
 <br>

6. HTMLファイルに書き込むには2種類の方法がある（今回はaでやったが、bの方が良さそう） 
<br>

   a: テキストエディタでHTMLを開く `vim index.html` 
   <br>

   b: 作った親ディレクトリをVSCodeにドラッグする 
   <br>

7. Chromeを開く(これも２種類方法がある) <br>
   a: HTMLファイルを開くコマンド `open index.html` <br>
   b: VScodeの「index.html」をChromeにドラッグする <br>

8. .htmlファイル内で以下のような内容を書き込んでおく
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

9. Chromeを読み直す → ページにHello World! This is a simple HTML page.と出てくる
<br>

### リモート環境のDocker上にあるファイルをブラウザで表示させよう編

1. リモート環境にアクセス
    ```
    ssh -i ~/.ssh/~~~~.pem username@IPアドレス
    ```
    <br>

2. ローカル環境から、ローカルで作成したindex.htmlファイルをリモート環境へコピー <br>
   ```
   scp -i ~~~~~~username.pem ~/Desktop/steak_site/index.html kako@11.111.111.11:/home/username
   ```

3. リモート環境に戻る
4. docker imageにnginxがあることを確認 `docker images` <br>
   なかったらpullする `docker pull nginx`
5. dockerでnginxコンテナを作成
    ```
    docker run --name kako-nginx -p 00000:80 -v /path/to/index.html/:/usr/share/nginx/html:ro -d nginx
    ```
    * -p リモート環境に入るためのポート番号：nginxのデフォルトポート番号
    * `/path/to/index.html`はHTMLファイルがあるディレクトリまでのパス（ファイル名まで書く！）
    * `/usr/share/nginx/html`はリモート環境におきたいディレクトリパス
    * roはread only
    * -dはバックグラウンドでの起動
    <br>

6. HTMLファイルがあるディレクトリに移動
7. `ls -l`でindex.htmlの権限情報を確認（644ならOK）
8. HTMLファイルがあるディレクトリのもう一つ上の階層に移動 `cd ..`
9. そこで`ls -l`でディレクトリの権限情報を確認
10. その他のユーザーが実行のみできるように権限を変更 (711 or 751)
11. Webブラウザで `http://<グローバルIP>:00000`を打ち込んで、自身の書いたHTMLファイルの内容が出てきたら成功
<br>