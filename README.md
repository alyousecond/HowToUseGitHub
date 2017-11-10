このプロジェクトはGit/GitHubをXcodeから利用する場合の手順並びにGit/GitHub用に準備したプロジェクトになります。 

1.はじめにリポジトリの準備  
=======================================================================

GitHubでリポジトリ作成

スケルトン用プロジェクトディレクトリを作成

$ cd App/Repository

$ mkdir $ProjectName$

オーファンブランチ作成

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


2.コミット
=======================================================================

確定して変更内容はローカルにcommitして、リモートにpushを行う
同時にリモートにpushを行うことも可能

commitはXcode上で実行したあとにコマンドラインでstatusを確認

$git status

必要に応じて処理を行う。

コマンドラインからpushする

$git push

3.ブランチ作成/変更/削除
=======================================================================

Xcode上からブランチを作成 [Xcode/コマンド]　　

$ git branch [ブランチ名]  

ブランチの切り替え [Xcode/コマンド]  

$ git checkout [ブランチ名]  

ブランチの削除　[Xcode/コマンド]  

$ git push --delete origin [ブランチ名]   

4.タグの設定
=======================================================================

ローカルにタグを作成

$ git tag [タグ名]

リモートにタグを反映

$ git push --tag

5.ファイルの追加/削除
=======================================================================
ファイルの追加 [Xcode/コマンド]

$ git add [ファイル名]

ファイルを管理対象から削除

git rm --cached [ファイル名]

ファイルに削除 [Xcode/コマンド]　※実態も削除されるので注意

git rm -f [ファイル名]

Xcodeでファイルの追加、削除などがうまくいかずに操作がエラーになる場合などはコマンドで確認をする

$ git status

6.ブランチパターン
=======================================================================
[ルートブランチ] 
オーファンブランチとして作成

ios: iosソース
aws: awsソース
doc: ドキュメント

※各ブランチで複数の枝はを管理する場合には、必ず__branch_name__/*** とすること。
__branch_name__/とした場合には、__branch_name__/***を作成できなくなる

[開発ブランチ]
用途：開発の主軸
名前：*/develop/[current|x.x.x]

[フィーチャーブランチ]  
用途：開発の作業用ブランチ  
*しばらくはdevelopブランチで開発作業*  
名前：*/feature/*  

派生元：*/develop/*
マージ先：*/develop/*

[リリースブランチ]  
用途：リリース準備用ブランチ  
名前：*/release/*  

派生元：*/develop/*
マージ先：*/develop, */master  

masterにマージ後にバージョンタグを生成  

[製品ブランチ]  
用途：リリース製品の主軸。リリースされたバージョンを管理するブランチ
名前：*/master  

[ホッチフィックスブランチ]  
用途：緊急リリースのためのブランチ。製品ブランチから派生。  
名前：*/hotfix/*  

派生元：*/master  
マージ先：*/develop, */master  

masterにマージ後にバージョンタグを生成

参考　http://keijinsonyaban.blogspot.jp/2010/10/a-successful-git-branching-model.html

7.開発時の流れ
=======================================================================

$ git clone -b ios/develop/current https://github.com/xxx/xxx.git

$ git checkout -b ios/feature/dev1

### 開発作業

$ git commit -a

$ git push -u origin HEAD

### Pull Request

$ git checkout ios/develop/current

$ git fetch <- リモート追跡ブランチのリポートリポジトリの情報を反映

$ git merge origin/ios/develop/current <- ローカルブランチにリポート追跡ブランチの内容を反映

### [パターン1] featureにdevelopブランチをマージする

$ git checkout ios/feature/dev1

$ git merge --no-ff ios/develop/current <- ブランチの履歴を残すためにnon fast-forwardとする


### [パターン2] featureブランチを一度削除してから、再度ブランチを作成する

$ git branch -d ios/feature/dev2 <- ローカルfeatureブランチの削除

$ git push --delete origin ios/feature/dev2 <- リモートブランチの削除

### 参考
https://qiita.com/forest1/items/db5ac003d310449743ca

8.その他
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

9.まとめ
=======================================================================
Xcode側でできることには限りがあるので、コマンドのstatus確認をする

$ git status



