# mac環境構築

## アカウント作成
githubアカウント作成画面 (https://github.com/) でサインアップ

## できればターミナルを使って以下をインストールする
1. homebrew  
`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`   
2. git  
`brew install git`      
3. github desktop   
`これはウェブでダウンロードした`      
4. docker desktop  
`これはウェブでダウンロードした`    
5. docker  
    `brew install --cask docker`
6. docker-compose  
    `brew install docker-compose`  
7. visual studio code  
    `brew install visual studio code`  

## コマンドで確認する  
    docker ps  
    docker-compose -v  
    git -v  
    brew -v  

# git初歩とマークダウン  
## github desktopで新しいリポジトリを作成し、ウェブとsyncさせる　　
チュートリアルをやってから自分で新しくリポジトリを作成する  
## 作ったリポジトリをcloneする
+ github desktopでclone repositoryを選び、ウェブ上のリンクと置きたいローカルパスを指定  
+ ウェブ上からローカルへと落とし込む  
## レポジトリにマークダウンファイルを作る
## 作ったマークダウンファイルをpushする
+ VScodeで編集する  
+ github desktopの左下のcommitボタンを押す  
+ github desktopの右上のpushボタンを押す  
# OSネットワーク
## ネットワーク通信を行う
### リモート（インスタンス）環境へのアクセス
1. 秘密鍵をダウンロード（拡張子は.pem）  
2. ターミナルに入る  
3. ターミナルで以下のコマンドを入力し、秘密鍵を配置
```
    cd  
    mkdir .ssh  
    chmod 700 .ssh/  
    mv Downloads/~~~~.pem .ssh/  
    chmod 600 .ssh/~~~~.pem  
```
4. 初期ログインを行う  
    `ssh -i ~/.ssh/~~~~.pem *username* @ *IPアドレス*`  
    + 初回はパスワードを設定する。  
    + 2回パスワードを入力し、正常に設定されると自動的にログアウト
5. もう一度ログイン
`ssh -i ~/.ssh/~~~~.pem username@IPアドレス`
6. ターミナルの環境が *username*@*IPアドレス*になってたらOK