#ios
このプロジェクトはGit/GitHubをXcodeから利用する場合の手順並びにGit/GitHum用に準備したプロジェクトになります。 

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

# Xcodeでgit管理を行っている場合
プロジェクトディレクトリを全てproduct配下に移動する

# Xcodeでgit管理を行ったいない場合

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

[開発ブランチ]  
用途：開発の主軸ブランチ  
名前：*/develop  

[フィーチャーブランチ]  
用途：開発の作業用ブランチ  
*しばらくはdevelopブランチで開発作業*  
名前：*/feature/*  

派生元：*/develop  
マージ先：*/develop  

[リリースブランチ]  
用途：リリース準備用ブランチ  
名前：*/release/*  

派生元：release  
マージ先：*/develop, */master  

masterにマージ後にバージョンタグを生成  

[製品ブランチ]  
用途：リリースされたバージョンを管理するブランチ  
名前：*/master  

[ホッチフィックスブランチ]  
用途：緊急リリースのためのブランチ。製品ブランチから派生。  
名前：*/hotfix/*  

派生元：*/master  
マージ先：*/develop, */master  

masterにマージ後にバージョンタグを生成  

7.まとめ
=======================================================================
Xcode側でできることには限りがあるので、コマンドのstatus確認をする

$ git status
