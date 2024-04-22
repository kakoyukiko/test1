# Git/GitHubを扱えるようになろう
## Git/GitHubとは？ <br>
+ Git<br>
分散型バージョン管理システムの一種. 
ソースコードの保存した履歴を管理するためのツール.<br>

+ GitHub<br>
Gitを使用する開発者のプロジェクト共有やコミュニケーション円滑化のための機能を提供するプラットフォーム. <br>

## Gitを触ってみる　（ローカルリポジトリ）
+ Mac上で新しいGitリポジトリを作ってみよう<br>
    1. MacのターミナルでGitリポジトリを作りたいディレクトリに移動する
    2. 移動したディレクトリで`git init` すると、`/.git` に入る。これで、目的ディレクトリの中にgitリポジトリを作って入ることができた。<br>
<br>

+ 新規にファイルを作り、リポジトリにコミットする<br>
    1. いつものようにファイルを作る `touch aaa.txt` <br>
    2. コミットしたいファイルをインデックスにのせる `git add aaa.txt` <br>
    3. インデックスにのせたファイルをコミットする　`git commit -m "added aaa.txt"` すると、変更履歴が記録される。　-m 以降に変更内容のメッセージを記載される。
<br>

+ コミット後に色々いじる<br>
    1. コミットしたファイルに変更を加えて再度コミットする <br>
    ファイルに色々書き込む　→　インデックスにのせる　→ コミットするでOK
    <br>

    2. コミットを修正したり取り消したりする<br>
    修正は`git commit --amend -m "修正されたコミット"`でコミット名を修正できる<br>
    取り消しは`git reset --option HEAD^`で直前のコミットを取り消しできる<br>
    optionは３種類あるが詳細はこの[URL](https://www.r-staffing.co.jp/engineer/entry/20191129_1)で. よく使うのは --soft <br>
    どこのコミットを取り消しするのかはHEAD^は直前のコミット。HEAD^^は２個前。HEAD コミット名で特定のコミットを指定。
    <br>

+ ブランチを作っていじってみる<br>
    + ブランチを新しく作る `git branch ブランチ名` <br>
    ちなみに１：ブランチ名を入れなかったらブランチ一覧と現在いるブランチ(*が表示される)がわかる <br>
    ちなみに２：新しくブランチを作って入るコマンド `git checkout -b ブランチ名`で一度のコマンドでブランチを作って入ることができる <br>

    + ブランチをマージする <br>
      1. 新規ブランチでファイルを作ったり編集したりする <br>
      2. ファイルをインデックスに乗せ、コミットする <br>
      3. 合併したい先の親的なブランチの方へ移る <br>
      4. ブランチを統合！ `git merge ブランチ名` <br>
         これで指定したブランチを今いるブランチ (Head branch)に統合できる
    <br>

    + ファイル編集→コミット前に別のブランチに切り替える
      1. コミットしていない変更を退避する（stashってところに保存して、本編はまだ変更していないことにするみたいな）<br>
      `git stash` <br>
      stashするのはインデックスに乗せられたファイルのみ！ -uオプションを使うとインデックスに乗っていない変更記録もstashされる 
      2. 別のブランチに移って作業してコミットする
      3. コミットしていなかったブランチに戻る
      4. a. stashしていた変更履歴をstashから削除し作業コピーに適用 `git stash pop` <br>
         b. stash指定た変更履歴を保存したまま作業コピーに適用 `git stash apply` 
         <br>

    + コンフリクト対処
        + 複数のブランチで同じファイルを編集してコミットするを繰り返す
         + ブランチをマージしようとするとコンフリクトが発生する
         ```
         Auto-merging kako2.txt
         CONFLICT (content): Merge conflict in kako2.txt
         Automatic merge failed; fix conflicts and then commit the result.
         ```
         + 問題のあるファイル(kako2.txt)に入る
         + そこに各ブランチでの編集内容が記載されているので、残したい内容のみ残して保存
         + git statusで確認すると、まだマージが完了されていないからコミットしろという文言が出てくる
         + インデックスに乗せて`git add`、現状確認する`git status`
         ```
         All conflicts fixed but you are still merging.
         (use “git commit” to conclude merge)
         ```
         + コミットしたらマージも完了する　`git commit -m "fixed conflict"`
    
## Gitを触ってみる（Githubとの連携）
+ 他の人のGithubをいじる　<br>
    + 大前提：勝手にはいじれない！
    + じゃあどうするのか？
    1. GitHubで引っ張ってきたい人のリポジトリを開く
    2. 右上のForkボタンを押して、自分のGitHubにコピーする
    3. クローンする<br>
    自分のGitHubアカウントにフォークしたリポジトリを、`git clone`コマンドを利用して自分のローカルマシンにクローンする。
    4. ローカルでいじる　<br>
    ファイルつくったり、編集したりしてコミットする
    5. リモートリポジトリの設定を確認する `git remote -v` <br>
    これで、リモート名（Originが多い）とそのURLがわかる。もしpushしたい先のリモートリポジトリがなかったら、`git remote add リモート名 git@github.com:<ユーザー名>/<リポジトリ名>.git`で設定を加える。
    6. ローカルの変更をリモートリポジトリへpushする<br>
    `git push <リモート名> <ブランチ名>`で、ローカルリポジトリの＜ブランチ名＞の内容が、＜リモート名＞というリモートリポジトリの＜ブランチ名＞反映される。<br>
    pushにあたり、GitHubからの本人認証が必要<br>
    + 現在はパスワード認証はできなくなって、トークン認証がメイン
    + トークン認証なら
        + GitHubの設定画面でトークンを設定
        + トークンをコピーしてターミナルでペーストする
        <br>

    + SSH鍵で認証するなら
        ①originの設定
        ```
        git remote add origin git@github.com:GitHubのユーザー名/GitHubのリモートリポジトリ名.git
        ```

        ②SSHのための鍵登録
        ```
        cd ~/.ssh
        ssh-keygen -t rsa
        ```

        以下を問われる
        3問ともEnter押しといたらできる
        ↓
        ```
        Generating public/private rsa key pair.
        Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa):
        Enter passphrase (empty for no passphrase): 
        Enter same passphrase again: 
        ```
        公開鍵できたので、鍵をコピー `pbcopy < ~/.ssh/id_rsa.pub`
    + https://github.com/settings/ssh で公開鍵の登録 <br>
        + 画面右上の「Add SSH key」のボタンを押す <br>
        + タイトル自分で決めて、内容はコピーしたものをペースト
        + 保存

    7. プルリクエストをする
    + 自分のGitHubまで変更内容を保存できたので、最後は引っ張ってきた人のGitHubに内容を反映してもらうよう申請する（プルリクエスト）。
        1. 自分のコミットが含まれるブランチのページを開く。
        2. ファイルの一覧の上にある黄色のバナーで、 [比較と pull request] をクリックして、関連付けられているブランチの pull request を作成する。
        3. 相手の変更したいブランチと自分が変更内容をコミットしたブランチを指定
        4. プルリクエストのタイトルと説明を入力
        5.  [pull request の作成] をクリック
        6. あとは相手が認証してくれたら完了







