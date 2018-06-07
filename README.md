このプロジェクトはGit/GitHubをXcodeから利用する場合の手順並びにGit/GitHub用に準備したプロジェクトになります。 

1.はじめにリポジトリの準備  
=======================================================================

GitHubでリポジトリ作成

スケルトン用プロジェクトディレクトリを作成

```$ cd App/Repository```

```$ mkdir $ProjectName$```

オーファンブランチ作成
(どこからも派生したいないルートツリー）

$ cd $ProjectName$

$ git init

$ git remote add origin https://github.com/alyousecond/$ProjectName$.git

$ git checkout --orphan ios/master  
$ echo "#ios" > README.md  
$ git add .  
$ git commit -m 'first commit'  
$ git push -u origin HEAD  

$ git checkout --orphan doc/master  
$ echo "#doc" > README.md  
$ git add .  
$ git commit -m 'first commit'  
$ git push -u origin HEAD  

必要に応じてオーファンブランチを作成していく。
たとえば、aws/svrなど。

## Xcodeでgit管理を行っている場合
プロジェクトディレクトリを全てproduct配下に移動する

## Xcodeでgit管理を行っていない場合

ios/Productディレクトリでcloneを作成

$ cd App/Product/$ProjectName$/ios

$ git clone https://github.com/alyousecond/$ProjectName$.git

Xcodeからプロジェクトファイルのコピー

ソース以外のdocなども必要に応じてcloneを作成

$ cd App/Product/$ProjectName$/doc

$ git clone https://github.com/alyousecond/$ProjectName$.git


2.コミット・切り戻し・取り消し
=======================================================================

確定して変更内容はローカルにcommitして、リモートにpushを行う
同時にリモートにpushを行うことも可能

commitはXcode上で実行したあとにコマンドラインでstatusを確認

$git status

必要に応じて処理を行う。

コマンドラインからpushする

$git push

3.コミットの切り戻し・取り消し
=======================================================================
直前のコミットを取り消したい場合（コミットがなかったことにする）

$ git reset --hard HEAD^

-- hard の場合はファイルも全て元に戻される
-- softの場合はファイルの変更点はそのままで、コミットだけが元に戻されるgit

コミットした内容を元に戻すが、コミットしたことは記録として残しておく場合

$git revert <コミットID>


4.ブランチ作成/変更/削除
=======================================================================

Xcode上からブランチを作成 [Xcode/コマンド]　　

$ git branch [ブランチ名]  

ブランチの切り替え [Xcode/コマンド]  

$ git checkout [ブランチ名]  

ローカルブランチの削除　[Xcode/コマンド]

$ git branch -D [ブランチ名]

リモートブランチの削除　[Xcode/コマンド]

$ git push --delete origin [ブランチ名]   

5.タグの設定
=======================================================================

ローカルにタグを作成

$ git tag [タグ名]

リモートにタグを反映

$ git push --tag

6.ファイルの追加/削除
=======================================================================
ファイルの追加 [Xcode/コマンド]

$ git add [ファイル名]

ファイルを管理対象から削除

git rm --cached [ファイル名]

ファイルに削除 [Xcode/コマンド]　※実態も削除されるので注意

git rm -f [ファイル名]

Xcodeでファイルの追加、削除などがうまくいかずに操作がエラーになる場合などはコマンドで確認をする

$ git status

7.ブランチパターン
=======================================================================
[ルートブランチ] 
オーファンブランチとして作成

ios: iosソース
aws: awsソース
doc: ドキュメント

※各ブランチで複数の枝はを管理する場合には、必ず__branch_name__/*** とすること。
__branch_name__/とした場合には、__branch_name__/***を作成できなくなる


[製品ブランチ]  
名前：*/master  
用途：リリース製品の主軸。リリースされたバージョンを管理するブランチ  
派生元：なし（ルートツリー）  

　[開発ブランチ]
　名前：*/develop/[current|x.x.x]
　用途：リリース後の開発の主軸（リリースするまでは不要）
　派生元：*/master
　マージ先：*/master

　　[フィーチャーブランチ]
　　名前：*/feature/*
　　用途：開発の中長期作業用ブランチ（*リリース後に開発のTermが複数になる場合*）
　　派生元：*/develop/*
　　マージ先：*/develop/*

　　[リリースブランチ]
　　名前：*/release/*
　　用途：リリース準備用の作業を行うブランチ
　　派生元：*/develop/*
　　マージ先：*/develop/*, */master
　　
　　*masterにマージ後にバージョンタグを生成


　[ホッチフィックスブランチ]
　名前：*/hotfix/*
　用途：緊急リリースのためのブランチ。製品ブランチから派生。
　派生元：*/master
　マージ先：*/develop, */master
　
　*masterにマージ後にバージョンタグを生成

参考　http://keijinsonyaban.blogspot.jp/2010/10/a-successful-git-branching-model.html

8.開発時の流れ
=======================================================================
$ git checkout master もしくは git checkout ios/develop/current  // 派生元にブランチに切り替え

$ git checkout -b ios/develop/foobar // 新しいブランチの作成

### コミット

$ git commit -m "Fix typo" -a // Whyをコミットコメントに記入する

$ git push -u origin HEAD

### ローカルのマージ作業（リモートのマージはPull Requestで実施）

$ git checkout ios/develop/current

$ git fetch <- リモート追跡ブランチのリポートリポジトリの情報を反映

$ git merge origin/ios/develop/current <- ローカルブランチにリポート追跡ブランチの内容を反映

もしくは

$ git pull

$ git checkout ios/develop/dev1 // マージ先のブランチをチェックアウト

$ git merge ios/develop/current // ios/develop/current をios/develop/dev1にマージ

### [パターン1] featureにdevelopブランチをマージする

$ git checkout ios/feature/dev1

$ git merge --no-ff ios/develop/current <- ブランチの履歴を残すためにnon fast-forwardとする


### [パターン2] featureブランチを一度削除してから、再度ブランチを作成する

$ git branch -d ios/feature/dev2 <- ローカルfeatureブランチの削除

$ git push --delete origin ios/feature/dev2 <- リモートブランチの削除

### コミットをせずに一時的に変更点を対比

$ git stash save (保存時のコメント) // 保存する際にコメントを記載しておくと後からわかりやすい

$ git stash list // stashの確認

$ git stash apply stash@{0} // 指定したスタッシュ名から復帰させる

$ git stash pop stash@{0} // 指定したスタッシュ名から復帰させて、そのstashを削除する

$ git stash drop stash@{0} // 指定したスタッシュ名を削除する

*stashコマンドで復活させる場合は、保存した際のブランチをチェックアウトしておくことを忘れないように！

### 参考
https://qiita.com/forest1/items/db5ac003d310449743ca

9.その他
=======================================================================
Author情報の変更

$ git config --global --edit

or

$ git config --global user.name "Your Name"

$ git config --global user.email you@example.com

除外ファイルの設定　.gitignore

.DS_Store

'# Include only project data

*.xcodeproj/*

!*.xcodeproj/project.pbxproj

既に管理対象に入っているファイルがある場合はキャッシュから除外する

$ git rm --cached /path/to/file.txt

Push時のデフォルト動作の設定を変更する

$ git config --global push.default upstream // デフォルトをupstreamに設定

10.まとめ
=======================================================================
Xcode側でできることには限りがあるので、コマンドのstatus確認をする

$ git status



