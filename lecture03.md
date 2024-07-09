# APサーバーについて
- 名前：Puma 
- バージョン：6.4.2
- AP サーバーを終了させた場合、引き続きアクセスできますか？→できない。

# DB サーバーについて
- 名前：MySQL
- バージョン：8.4.1
- DB サーバーを終了させた場合、引き続きアクセスできますか？→できない。
- Rails の構成管理ツールの名前：Bundler

# 今回の課題から学んだこと、感じたこと
## 学んだこと
Bundler　  
→構成管理ツール。phpにおけるcomposerみたいな感じ。Gemfileに記載してある種類とバージョンのGemをインストールしてくれる。依存性や競合関係も解決してくれる。

Gem  
→モジュール。RailsもGemらしい。

モジュール   
→他のプログラムで利用されることを目的とした、クラスや関数等のプログラム

パッケージ、ライブラリ   
→モジュールがひとまとまりになったもの。ただし、言語等によって、言葉の使い方や定義が異なるらしい

RubyGems   
→Rubyのリモートリポジトリに当たるもの。構成管理ツールであるBundlerを使用して、必要なライブラリを自動でダウンロードする際に、このRubyGemsを通してダウンロードされる。

bin/dev   
→rails sコマンドの代わりになるもの。サーバーを起動するために使用するコマンド。Rails6までは、rails sで良かった様だが、Rails７以上では、異なるプロセス管理ツールを使用するため、bin/devを使用するらしい。

Puma起動コマンド   
→種類がたくさんある。

### Rubyのインストールについて
Rubyのバージョン確認   
→ruby -v

rvm   
→Rvmはルビーのバージョンを切り替えることができる仕組み

Rubyのバージョン切り替えコマンド   
→rvm install 3.2.3

### Bundlerのインストールについて
Bundlerのバージョン確認   
→bundle -v

Bundlerのバージョン変更   
→gem install bundler -v 2.3.14

### Railsのインストールについて
Railsのバージョン確認   
→gem list -e rails

Railsのインストールコマンド   
Gem install rails -v 7.1.3.2

### Nodeのインストールについて
Nodeのバージョン確認   
→node -v

Nodeのバージョン切り替え   
nvm install v17.9.1

### yarnのインストールについて
yarnのインストール   
→npm install -g yarn

yarnのバージョン変更   
→yarn set version 1.22.19

### 容量を確認するコマンド
df -h   
コマンド後の表示例↓   
devtmpfs        468M     0  468M   0% /dev   
tmpfs           477M     0  477M   0% /dev/shm   
tmpfs           477M  516K  476M   1% /run   
tmpfs           477M     0  477M   0% /sys/fs/cgroup   
/dev/xvda1       10G  8.1G  1.9G  82% /   
tmpfs            96M     0   96M   0% /run/user/1000   
tmpfs            96M     0   96M   0% /run/user/0      
 この表示では、/dev/xvda1の容量がすでに82%使用済みであることを示してる。

### ボリュームの割り当て状況を確認するコマンド   
lsblk   
コマンド後の表示例↓   
NAME    MAJ:MIN RM SIZE RO TYPE MOUNTPOINT   
xvda    202:0    0  16G  0 disk    
└─xvda1 202:1    0  10G  0 part /   
この表示では、xvdaに16Gの容量があるけど、xvda1には10Gしか割り当てられてないよと書いてある。

### xvda1のファイルサイズを拡張させる方法
まずパーテーションサイズを拡張する。   
Sudu growpart /dev/xvda 1   
上記コマンドの実行後↓      
CHANGED: partition=1 start=4096 old: size=20967391 end=20971487 new: size=33550303 end=33554399   
これで、パーテーションは拡張できた。次は、ファイルサイズを変更する。   
sudo resize2fs /dev/xvda1   
その後、lsblkコマンドを実行すると、下記表示となる。   
NAME    MAJ:MIN RM SIZE RO TYPE MOUNTPOINT   
xvda    202:0    0  16G  0 disk    
└─xvda1 202:1    0  16G  0 part /   
ちゃんと、xvda1に16Gが割り当てられている。

### EBSとは
Amazon Elastic Block Storeのこと。Amazon Elastic Compute Cloud (Amazon EC2) 向けに設計された、使いやすく、スケーラブルで、高性能なブロックストレージサービス。
ブロックストレージということで、SSDやHDDのようなストレージのようなものとして使える。 この上にXFSやEXT4などのファイルシステムを構築することでファイルの保存領域としても使える。

### xvda
Amazon EC2（Amazon Elastic Compute Cloud）のルートボリュームに利用されているEBSボリューム。EC2の起動時に作成される。

### cURL
cURL（client for URL）とは、LinuxなどのUNIX系OSでよく利用されるコマンドラインツールの一つで、URLで示されるネットワーク上の場所に対して様々なプロトコル（通信手順）を用いてアクセスできるもの。MITライセンスに基づきオープンソースソフトウェアとして公開されている。

### mysql -u root -p
MySQLにログインするときのコマンド。これはrootユーザーでのログイン方法。セキュリティ的に適切ではない。
上記コマンド後に、パスワードを入力して、ログインできる。

### MySQLを終了させる方法
exitで終了する。

### bin/setupとは
rails new すると bin/setup スクリプトも生成される。これは Rails が用意してくれている開発環境セットアップ用のスクリプトで、Railsガイドには「アプリケーションの初期設定時に設定を自動化するためのコードの置き場所」と書いてある。
このコマンドを実行することで、セットアップができる。binディレクトリ直下にsetupというスクリプトが用意されているとということ。

### application.html.slim
Rubyの記述方法。htmlを少ない記述で表現できる。

## 感じたこと
エラーになった際に、エラー文をしっかり読み解く大切さを学んだ。   
課題を進める上で、基礎的な知識などが足りていない為、課題を進めるための学習も必要である様に感じた。