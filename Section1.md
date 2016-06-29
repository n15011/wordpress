#Section1 基本のサーバー構築

##Section1-1 CentOS 7のインストール

###virtualboxへのインストール

CentOSの公式サイトよりCentOS 7 Minimal ISO(x86_64)のISOファイルをダウンロードし、 VirtualBox上にインストールしてください。

先生からUSBを借りCentOS-7-x86_64-Minimal-1511.isoを自分のパソコンにコピーします。(コピー先は自分が把握できる場所に)

[Section0-1](https://github.com/n15011/wordpress/blob/master/Section0.md)でやったように

    $ virtualbox


でvirtualboxが立ち上がると思います
**新規**をクリック、その次にエキスパートモードを選択します。

###名前とメモリの設定

次に仮想マシンの名前を**Centos**にするとタイプとバージョン自動で切り替わってくれます
名前を入力後タイプが**Linux**バージョンが**Red Hat(64-bit)**に切り替わってることを確認して次の画面へ。

メモリサイズを**1G(1024MB)**にします。


###ハードディスクの作成
ハードディスクのファイルタイプ**VDI(VirtualBox Disk Image)**を選択物理ハードディスクにあるストレージ可変サイズを選択

###ファイルの場所とサイズ
ここで仮想ハードディスクのサイズを**8G**に設定します

###ネットワークアダプターの設定
仮想マシンの電源はつけずに**設定**を選択します

設定→ネットワーク→アダプター2を選択
ネットワークアダプターにチェックを入れ割り当てを**ホストオンリーアダプター**にします。

では作成したCentOSを立ち上げてみましょう!

##CentOSインストールまでの設定

###起動ハードディスクの選択
起動ハードディスクを選択しろと言われるので**CentOS-7-x86_64-Minimal-1511.iso**を選択し数分待ちます。

###言語選択
言語を選択(お好みで。僕は日本語をおすすめします)
言語選択が終わる画面が切り替わりインストールの開始を押します。

しばらく時間がかかるのでその間に*ROOTパスワード*と*ユーザーの作成*を行います。

###ROOTパスワード作成
CentOSにログインするときに必要なパスワードです。
作り直せるし時間に余裕あるかつめんどくさい手間暇を惜しまない人は簡単なパスワードでいいかもしれない。ちなみに一文字でも可能でした。

###ユーザー作成
ユーザー作成の注意事項として**このユーザーを管理者にする**という項目にはチェックしときましょう。あとは適当に。学籍番号おすすめです。

##IPアドレスの取得とSSH接続の確認
/etc/sysconfig/network-script/ifcfg-enp08というファイルがあるのでviで開きます。(スーパーユーザーじゃないと書き込みできませんで注意)

    $su -

でスーパーユーザーに切り替えます。上記に記述してあるファイルを開きます。

    ONBOOT=no

の部分を

    ONBOOT=on

にします。

    $service network restart

でIPアドレスが取得されてるはずです。

    $ip a

で確認してみてください。

ホスト側のターミナルから

    ssh Virtualbox名@ipアドレス

で接続が確認できることを確認しましょう。

##yumとwgetのproxy設定

###yumのproxy設定

    /etc/yum.conf    の一行目以外に

    proxy=http://172.16.40.1:8888

を追加。保存して終了します。これでyumが使えるようになっているはずです。

##wgetのインストールとproxy設定

###wgetインストール

    $ yum install wget

でwgetをインストールします。

###wgetのproxy設定

    /etc/wgetrc   

というファイルがあるので下記を追加

    http_proxy=http://172.16.40.1:8888

    https_proxy=https://172.16.40.1:8888


##Apacheインストールと起動

###Apacheのインストール

   $ yum install httpd

インストール完了です。それでは起動してみましょう。


   $ service httpd start

でスタートさせて

   $ service httpd status

で**active(running)**という結果が出てたらおｋです。


##phpインストール

###phpインストール

   $ yum install php php-mbstring php-mysql


##mysqlのインストールと起動と設定

###mysqlのインストール

   $ yum -y install http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm

で公式リポジトリを取ってくる

   $ yum install mysql mysql-server mysql-devel

でmysqlをインストール

###mysqlの起動

   $ service mysql start

Apacheと同様に起動してるか確認

   $ service mysql status

**active (running) **と表示されてたらおｋです。

###mysqlのパスワード設定

動いてることは確認できたのでログインする時のパスワードを設定しましょう。

   $ mysql_secure_installation

で新しいパスワードを設定します。


###mysqlにデータベース作成

mysqlにログインしたら

   $ create database 任意のデータベース名;

でデータベースを作成します

   $ grant all on wpdb.* to '任意のユーザー名'@'localhost' identified by '任意のパスワード';

で終わりです。
