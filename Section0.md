#Section 0 講義前のセットアップ

##0-0 朝8時起床。お風呂等々すませ朝の9時30分には402教室へ移動 

##0-1 virtualBoxのインストール

~~[公式サイト](https://www.virtualbox.org/)からDownload→VirtualBox5.0.20 for Linux hosts(2016年5月現在のversion)をクリック。
起動しようとしたが原因が分からずVirtualBoxが起動しなかったので~~

ターミナル上で

    sudo apt install virtualbox

でインストール後ターミナル上で

    virtualbox

なんなく起動しました。


##0-2 Vagrantのインストール
[vagrant公式サイト](https://www.vagrantup.com/downloads.html)からDEBIANの64-bitをインストール。

    vagrant -v

でバージョンが1.8.1か確認。

先生に見せる。
