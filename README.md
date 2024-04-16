# mac環境構築

## アカウント作成
    github

## できればターミナルを使って以下をインストールする
    homebrew  
         `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`   
    git  
         `brew install git`    
    github desktop   
         これはウェブでダウンロードした    
    docker desktop  
         これはウェブでダウンロードした  
    docker  
         `brew install --cask docker`
    docker-compose  
         `brew install docker-compose`  
    visual studio code  
         `brew install visual studio code`  

## コマンドで確認する  
    > `docker ps`  
    > `docker-compose -v`  
    > `git -v`  
    > `brew -v`  

#　git初歩とマークダウン  
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
    