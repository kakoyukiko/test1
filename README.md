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
github desktopでclone repoditoryを選び、ウェブ上のリンクと置きたいローカルパスを指定  
ウェブ上からローカルへと落とし込む  
## レポジトリにマークダウンファイルを作る
## 作ったマークダウンファイルをpushする
VScodeで編集する  
github desktopの左下のcommitボタンを押す  
github desktopの右上のpushボタンを押す  
    