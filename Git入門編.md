# Git入門編

## Gitって何?
- ファイルの状態を好きな時に更新履歴として保存しておくことができる。
- 一度編集したファイルを過去の状態に戻したり、編集差分を表示したりすることができる
- PC上には選択した(またはそこから編集した)状態のファイルのみ存在する
- Gitの構成
  - リポジトリ
    - ディレクトリ
      - ファイル

## リポジトリって何?
- ファイルやディレクトリを保存しておくための入れ物
- リポジトリの種類
  - リモートリポジトリ
    - 専用サーバーに配置して複数人で共有するためのリポジトリ
    - ローカルリポジトリの作業内容を公開したいときは、リモートリポジトリにアップロードして公開する
    - 他の人の作業内容を取得することもできる
  - ローカルリポジトリ
    - ユーザー1人が利用するために、自分の手元のマシン上に配置するリポジトリ
    - 普段の作業はこちらで行う
- リポジトリを作成するには
  - 全く新しいリポジトリを作成する
  - リモートリポジトリをコピーして作成する

## リポジトリに対する操作にはどんなものがある?
### コミット
- ファイルやディレクトリの追加・変更をリポジトリに記録する操作
- 実行するとリポジトリ内で、前回のコミットから現在のコミットまでの差分を記録したコミット(リビジョン)と呼ばれるものができる(要するに操作名と記録そのものが両方コミットという名前になっている)
- 時系列順につながった状態で格納されている
- 最新のコミットからたどっていくことで、過去の変更履歴やその内容を知ることができる
- コミットの情報から計算された重複のない英数字40桁の名前が付けられている
  - この名前を指定することで、コミットを指定することができる
- コミットメッセージは入力必須
- **Tips**
  - バグ修正や機能追加などの変更は、できるだけ分けてコミットする
  - 標準的なコミットメッセージの書き方
      ```
      1行目 : コミットでの変更内容の要約
      2行目 : 空行
      3行目以降 : 変更した理由
      ```
### プッシュ
- ローカルリポジトリの変更履歴を、リモートリポジトリにアップロードする
- リモートリポジトリ内の変更履歴がローカルリポジトリのものと同じ状態になる
### クローン
- リモートリポジトリの内容を丸々ダウンロードしてきて、別のマシンにローカルリポジトリとして作成できる
- 変更履歴も複製されているので、もともとのリポジトリと全く同じように履歴の参照やコミットを実行できる
### プル
- リモートリポジトリの変更内容を、ローカルリポジトリにダウンロードする
- ローカルリポジトリ内の変更履歴がリモートリポジトリのものと同じ状態になる

### マージ
- 他の履歴の変更を取り込む(統合する)操作
  - 最後にプルしてからプッシュするまでの間に、他の人がプッシュしてリモートリポジトリを更新してしまっていた場合に実行(実行しないと自分のプッシュは拒否される)
  - マージを行わないまま履歴を上書きし、ほかの人がプッシュした変更が失われてしまうことを防ぐ
  - 基本はGitが自動でマージしてくれるが、自動ではマージできない場合もある
    - リモートリポジトリとローカルリポジトリでファイルの同じ場所を変更していた場合、どちらの変更を取り込むか自動では判断できない

### フォーク
- プロジェクトを丸々指定したアカウントにコピーしてくる機能
- GitHubの機能であって、Gitそのものの機能ではない
- プロジェクトを持っている相手に対して貢献する意思がある場合に行う
- そうでない場合は単にURLからクローンする(※貢献する意思がない場合は誤解を招くのでフォークしないこと)

## ワークツリーとインデックスって何?
- ワークツリー
  - ユーザーが実際に作業しているディレクトリのこと 
  - 状態を直接リポジトリにではなくインデックスに記録する
- インデックス
  - リポジトリとワークツリーの間に存在する
  - リポジトリにコミットを準備するための場所
  - コミットでファイルの状態を記録するために、登録する必要がある
- ワークツリーとインデックスを採用する利点
  - **ワークツリー内の不要なファイルを含めずにコミットを行える**
  - **ファイルの一部の変更だけをインデックスに登録してコミットすることができる**

参考画像:
![ワークツリーとインデックス](https://backlog.com/ja/git-tutorial/assets/img/intro/intro4_1.png)

## Gitを使ってみよう(チュートリアル)
以下はGit Bashがインストールされている状態での説明とする

### 初期設定
ユーザー名とメールアドレスの設定
```
$ git config --global user.name "userName"
$ git config --global user.email "mailAddress"
```

Gitの出力をカラーリング
```
$ git config --global color.ui auto`
```

エイリアス(別名・通称)を設定
```
$ git config --global alias.co checkout`
```

日本語を含んだファイル名を正しく表示する
```
$ git config --glibal core.quotepath off
```

### 新規リポジトリの作成
ディレクトリをGitリポジトリに設定(ここではtutorialディレクトリ)
```
$ mkdir tutorial
$ cd tutorial
$ git init
Initialized empty Git repository
in /Users/yourname/Desktop/tutorial/.git/
```

### ファイルのコミット
※tutorialディレクトリにsample.txt(内容:サル先生のGitコマンド)が存在するものとする

```
#ワークツリーとインデックスの状態を確認するコマンド
$ git status
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#     sample.txt
nothing added to commit but untracked files present (use "git add" to track)
```

```
$ git add sample.txt
# git add .ですべてのファイルをインデックスに登録できる
$ git status
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#     new file:   sample.txt
#
```

```
#-mオプションはコミットメッセージを記述できる
$ git commit -m "first commit"
[master (root-commit) 116a286] first commit
0 files changed, 0 insertions(+), 0 deletions(-)
create mode 100644 sample.txt

$ git status
# On branch master
nothing to commit (working directory clean)
```

```
#リポジトリの変更履歴を確認するコマンド
$ git log
commit ac56e474afbbe1eab9ebce5b3ab48ac4c73ad60e
Author: eguchi <eguchi@nulab.co.jp>
Date:   Thu Jul 12 18:00:21 2012 +0900
    first commit
```

### リモートリポジトリへのプッシュ

```
#URLのリポートリポジトリをoriginという名前で登録
$ git remote add origin https://github.com/inop27/tutorial.git
```

```
#originのマスターブランチ(後で出てくる?)にコミットをプッシュ
#実行オプションで-uを指定すると、次回以降はそのリモートリポジトリ名(origin)ブランチ名(master)を省略できる
#ただし、最初に空のリモートリポジトリにpushするときはどちらも省略できない
$ git push -u origin master

```

### リモートリポジトリをクローン

```
#カレントディレクトリにURLのリモートリポジトリをtutorial2という名前でクローン(複製)
#この操作ではリモートにつけたoriginという名前は使用できない(カレントディレクトリでgit remote addしていても)
$ git clone https://github.com/inop27/tutorial.git tutorial2
```

### リモートリポジトリからプル
```
#orriginからmasterブランチにプル
$git pull origin master
```

### 競合を解決する
※コンフリクト(競合)が発生したという前提であるとする

```
#マージ中に競合が発生したことを表すメッセージが表示される
#Merge conflict in sample.txt
$ git pull origin master
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 383 bytes | 2.00 KiB/s, done.
From https://github.com/inop27/tutorial
 * branch            master     -> FETCH_HEAD
   689475b..bbaa1c2  master     -> origin/master
Auto-merging sample.txt
CONFLICT (content): Merge conflict in sample.txt
Automatic merge failed; fix conflicts and then commit the result.
```

sample.txtを以下のように変更
```
サル先生のGitコマンド
add 変更をインデックスに登録する
<<<<<<< HEAD
commit インデックスの状態を記録する
=======
pull リモートリポジトリの内容を取得する
>>>>>>> 4c0182374230cd6eaa93b30049ef2386264fe12a
```
↓
```
サル先生のGitコマンド
add 変更をインデックスに登録する
commit インデックスの状態を記録する
pull リモートリポジトリの内容を取得する
```

※どちらかを消すのではなく、どちらも取り入れる場合もある

```
#--graphオプションはテキストで履歴の流れを確認
#--onelineオプションはコミットの情報を1行で表示
$ git log --graph --oneline
*   d845b81 マージ
|\
| * 4c01823 pullの説明を追加
* | 95f15c9 commitの説明を追加
|/
* 3da09c1 addの説明を追加
* ac56e47 first commit
```



---
参考ページ:   
[サル先生のGit入門　入門編](https://backlog.com/ja/git-tutorial/intro/01/)