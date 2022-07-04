# Git

## Git用語
### origin
リモートリポジトリである`git@github.com:hoge/fuga.git`の別名

## リモート追跡ブランチ (remote-tracking branch) 
* origin/masterはリモートリポジトリのmasterを追跡するリモート追跡ブランチである
*追跡する側のブランチ*

## 上流ブランチ (upstream branch)
origin/master はローカルのmasterブランチの上流ブランチである
origin/feature/#1_hogeはローカルのfeature/#1_hogeブランチの上流ブランチである
*追跡される側のブランチ*

## マージする前のブランチ動作確認

git fetch origin
git merge origin/feature/xxxx
(docker-compose down)
(docker-compose up -d)


## コミット前の変更を退避
### 退避
git stash

### 追跡対象に含まれていないファイルも退避
git stash -u

-u は　--include-untracked の略です。
新規作成ファイル(追跡対象に含まれていないファイル)も退避することができます。

### メッセージを付けて退避
git stash -m "message"

### 退避した作業一覧
```
$ git stash list
stash@{0}: WIP on test: xxxx
stash@{1}: WIP on commit-sample: xxxx

# stash@{n}から始まる1行が1回分のstashになります。
# WIP onのあとはブランチ名です。
# xxxxはstashをしたときのHEADのコミットハッシュとコミットメッセージになります。
```

### 退避した作業の詳細
$ git stash show stash@{0}

### 退避した作業を戻す
$ git stash apply stash@{0}

### 退避した作業を消す
$ git stash drop stash@{0}

### 退避した作業を戻す + 消す
$ git stash pop stash@{0}

### 退避した作業をすべて消す
$ git stash clear


## 必要なコミットのみマージ

cherry-pickは、別のブランチから今いるブランチへ、必要なコミットだけをコピーできる便利なコマンドです。

### 特定のコミットを取り込む
$ git cherry-pick [コミットID]

### 複数のコミットを取り込む
$ git cherry-pick [コミットIDその1]..[コミットIDその2]

### 取り込みたいけどコミットはしたくない場合
cherry-pickを実行すると、その内容がそのままコミットされます。コミットせずに作業ディレクトリだけに変更を留めたい場合は、オプション「-n」を使います。

$ git cherry-pick -n [コミットID]



## git add 取り消し
### 特定のファイルのみ取り消し

git rm --cached -r file_name

### 全てのファイルを取り消し

git rm --cached -r .

## git commit 取り消し (直前のコミットをなかった事にする)

git reset HEAD^

※ HEAD^：直前のコミットを意味する。

### コミット取り消した上でワークディレクトリの内容も書き換えたい場合

git reset --hard HEAD^


## gitの管理対象から外す
git rm --cached <ディレクトリ or ファイル名>



## ローカルの変更を元に戻す方法

### ローカルで弄った変更を戻す

git checkout <filename>

### 全て元に戻す

git checkout .

## リモートリポジトリに強制一致

git fetch origin
git reset --hard origin/master

## 特定のファイルの変更履歴を確認する

git log -p path/to/file.txt


## Dockerfileのbuild時にgit labのプライベートリポジトリをgit cloneする

https://asobod11138.com/2019/01/27/dockerfile%E3%81%AEbuild%E6%99%82%E3%81%ABgit-lab%E3%81%AE%E3%83%97%E3%83%A9%E3%82%A4%E3%83%99%E3%83%BC%E3%83%88%E3%83%AA%E3%83%9D%E3%82%B8%E3%83%88%E3%83%AA%E3%82%92git-clone%E3%81%99%E3%82%8B/


## git labのプライベートリポジトリにgit pushする
* アクセストークン作成
write_repository権限を付与したもの

* originのURLを変更
git remote set-url origin  https://oauth2:YOURACCESSTOKEN@gitlab.com/lions-data/products/ld-aiot-people-counting-platform.git

* git push


## 複数のGitHubアカウントを使う場合
### リモートリポジトリのURL設定
x git remote add origin git@github.com:The-ROOM4D/tsumura.git
o git remote add origin git@github4d:The-ROOM4D/tsumura.git

git@github.com <- の`github.com`を~/.ssh/configで設定しているHostにする
(GitHubアカウントごとに別の名前でHostを設定しておく)

e.g.

```txt:~/.ssh/config
Host githubkomiya
  User git
  Port 22
  HostName github.com
  IdentityFile ~/.ssh/id_private_rsa
  TCPKeepAlive yes
  IdentitiesOnly yes
  UseKeychain yes
  AddKeysToAgent yes

Host github4d
  User git
  Port 22
  HostName github.com
  IdentityFile ~/.ssh/id_work_rsa
  TCPKeepAlive yes
  IdentitiesOnly yes
  UseKeychain yes
  AddKeysToAgent yes
```

例えば上記の場合以下のようにクローンする

x git clone git@github.com:The-ROOM4D/tsumura.git
o git clone git@github-work:The-ROOM4D/tsumura.git


### GitHubユーザーを設定
git config で設定したemailにGitHubアカウントのアドレスを設定すると、設定したアカウントでログインされる。
`~/.gitconfig`を書き換える。

```sh
git config --global user.email n.komiya@the-room.company
```

リポジトリ毎にアカウントを切り替える場合はこう。
`./.git/config`を書き換える。

```sh
git config --local user.email n.komiya@the-room.company
```


## git rm
git rm --cached [file1 file2...]

git rm --cached .


## git mv
```sh
#ファイル名変更
#a.txt -> b.txtに変更
git mv a.txt b.txt

#ディレクトリ名変更
#a -> b
git mv a/ b/

#ファイル位置変更
#./a.txt -> ./b/a.txt
git mv a/a.txt b/a.txt
```

## git push -u

```sh
git push -u(--set-upstream) origin master
```

ローカルリポジトリの現在のブランチの上流をorigin masterに規定したことにる。
次からは、
  git push だけで git push origin master
  git pull だけで git pull origin master 
と同じ意味になる。


## GitHubでブランチ名を変更
ブランチ名をhogeからfugaに変更する
```sh
# 1.ローカルのブランチ名を変更する
git branch -m hoge fuga

# ※現在使用しているブランチ名を変更する場合はこれでもでOK
git branch -m fuga

# 2.リモートのブランチを削除
git push origin :hoge

# 3.ブランチ名を変更したローカルブランチをプッシュ
git push origin fuga
```


## コミットメッセージの変更
```sh
git add -A
git commit -m "htmlを修正"
#あっ　今のコミット内容、cssの修正だった！書き直したい…！（心の声）
git commit --amend -m "cssを修正"
#これで直前のコミットが更新されました！やったー！
```

> ■ 注意
> 一度pushしたものをamendやrebaseなどで改変しては、原則いけません（コミットログを追いかけての解説）。たとえミスがあってもamendはせずに、後のコミットで補いましょう。
>
> 解決策としては、
>
> fetchしてから、リモートの先頭にマージする（推奨）
> リポジトリを使う人全員に確認をとった上で、git push -fで無理やりプッシュ（危険）
> ということになります。


## .gitkeep .gitignore
空のディレクトリに置いておく場合

* gitkeep
ファイルが追加されたら、そのファイルをGitでの管理対象にしたい

* gitignore
ファイルが追加されたら、そのファイルをGitでの管理対象にしたくない
(ログの出力先, キャッシュ置き場など)


## エラー　/ トラブルシュート
### fatal: refusing to merge unrelated histories
共通の祖先をもたないブランチブランチをマージしようとすると起こるエラー



### バッファ不足

```
error: RPC failed; curl 56 GnuTLS recv error (-54): Error in the pull function.
```

や

```
RPC failed; curl 18 transfer closed with outstanding read data remaining
```

* ファイルサイズが大きい場合などに起こる
  -> バッファサイズの設定を大きくする

```
git config --global http.postBuffer 16M
```

or

~/.gitconfigに追記

```
[user]
	name = Xxxxxx Xxxxx
	email = xxxx@gmail.com
...
# 以下を追記
[http]
	postBuffer = 64M
```

ダメなら

```sh
git config --global http.postBuffer 2M
git clone https://github.com/※※※※/hogehoge.git --depth 1
```

* --depth オプションは指定したコミット数でcloneする
  * --depth 1で最新のコミットだけクローンすることが可能
  
  
## ブランチを間違えてコミットした場合の対処法

```sh
# コミット履歴を確認
git log

# --softでオプションで作業ディレクトリの変更内容は残したままコミットの取り消しができる  
# 取り消したいコミットの一つ前のコミットハッシュを指定
git reset --soft コミットのハッシュ値

# 変更部分を退避
# コンフリクトせずにcheckoutできる場合はstashしなくてOK
git stash

# ブランチ移動
git checkout ブランチ名

# 退避ファイルを復元
git stash pop
```

* 【Git】ブランチ間違えてコミットしてた！？
https://song-of-life.hatenablog.com/entry/2019/01/24/134049#%E9%96%93%E9%81%95%E3%81%88%E3%81%9F%E3%83%96%E3%83%A9%E3%83%B3%E3%83%81%E3%82%92%E3%82%B3%E3%83%9F%E3%83%83%E3%83%88%E3%81%99%E3%82%8B%E5%89%8D%E3%81%AE%E7%8A%B6%E6%85%8B%E3%81%AB%E6%88%BB%E3%81%99

* キメろgit stash
https://qiita.com/fukajun/items/41288806e4733cb9c342



## 既存のディレクトリをgit管理にしてリモートリポジトリと紐づける

```sh
# git管理にする
git init

# リポジトリと紐付け
git remote add origin https://gitlab.com/lions-data/information-sharing/ld-naoki-komiya/twitter-clone.git

# エンプティコミット作成
git commit --allow-empty -m "first commit"

# masterブランチをmainブランチに変更
# git branch -m (old_branch) new_branch
# old_branchを省略すると現在のブランチ名が書き換えられる
# -Mは強制書き換え
git branch -M main

# -u (--set-upstream) 指定したブランチを上流ブランチとして設定する
git push -uf origin main
```


# ホワイトリスト
https://qiita.com/officemove/items/b0409cb1ee946edadc3e

.gitignore
```
# 【1】 最初に全ファイル禁止
# すべてのファイルをignoreするがフォルダはしない
# gitは空フォルダは無視されるので影響はない
*
!*/

# 【2】 次に任意のファイルやフォルダを許可
# このファイルと同階層のフォルダ階層以降を許可
!/lib/**
# このファイルと同階層以降の「??？_m」というフォルダ階層以降を許可
!**/*_m/**
# このファイルと同階層のファイルだけ許可
!/README.md
!/.gitignore


# 【3】 最後に常に除外するファイルやフォルダを禁止
# OSが自動で作るファイル
.DS_Store
Thumbs.db
```

# Git戦略
* タスク単位でissueを作成 
* branch名_#issue番号でブランチを作成
  * e.g., `feature/#12_repeater_table`
* コミットメッセージにはissue番号を含める
  * e.g., `"リピート分析画面/リピーターテーブルコンポーネント作成 #12"`
* 実装完了 -> PR作成
* PRのコメントに`close #issue番号`を記載 (マージされるとissueも自動でクローズします)
  * cf. https://docs.github.com/ja/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue