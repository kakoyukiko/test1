# 今日やったこと
1. リモート環境にアクセス（以下の作業は指定がない限りリモート環境で行う）。
2. ローカル環境から、昨日ローカルで作成したindex.htmlファイルをリモート環境へコピー <br>
`scp -i ~~~~~~username.pem ~/Desktop/steak_site/index.html kako@11.111.111.11:/home/username`
3. リモート環境に戻る
4. docker imageにnginxがあることを確認 `docker images`
5. dockerでnginxコンテナを作成
    ```
    docker run --name kako-nginx -p 00000:80 -v /path/to/index.html/:/usr/share/nginx/html:ro -d nginx
    ```
    * ポート番号はリモート環境に入るためのポート番号：nginxのデフォルトポート番号
    * `/path/to/index.html`はHTMLファイルがあるディレクトリまでのパス
6. HTMLファイルがあるディレクトリに移動
7. `ls -l`でindex.htmlの権限情報を確認（644ならOK）
8. HTMLファイルがあるディレクトリのもう一つ上の階層に移動 `cd ..`
9. そこで`ls -l`でディレクトリの権限情報を確認
10. その他のユーザーが実行のみできるように権限を変更 (701 or 751)
11. Webブラウザで `http://<グローバルIP>:00000`を打ち込んで、自身の書いたHTMLファイルの内容が出てきたら成功
<br>

# エラー対処
最初、ブラウザで手順11を行ったら"error 403 forbidden"と出てきた。
## 理由
   + nginxを変更設定を反映していなかった。→これは`nginx -s reload`で解決
   <br>

   + 権限情報を正しく設定していなかった。
     1. HTMLファイルの権限は644にする（オーナーは読み込みと書き込み、グループや他の人は読み込みのみ行える状態）→これによりファイルの読み取りができる
     2. HTMLファイルが入っているディレクトリの権限で実行権を与える
     HTMLファイルまでの各ディレクトリで、「その他のユーザ」に実行権（x）が付与されているか確認する。今回の場合だと、「/」、「/home」、「/home/kako」のいずれかにディレクトリに実行権（x）がないと、HTMLファイルが表示されない。

参考文献：https://engineers.weddingpark.co.jp/nginx-403-forbidden/
