# vCenter Server
## 概要
- 実験日: 2022/09/23,2022/09/30
- vCenter Server

目次
- [vCenter Server](#vcenter-server)
  - [概要](#概要)
  - [1. vCenter Serverについて](#1-vcenter-serverについて)
    - [1.1 vCenter Serverとは](#11-vcenter-serverとは)
    - [1.2 vCenter Serverの機能](#12-vcenter-serverの機能)
  - [2. 今回の実験](#2-今回の実験)
    - [2.1 概要](#21-概要)
    - [2.2 vCenter Serverのダウンロード手順](#22-vcenter-serverのダウンロード手順)

## 1. vCenter Serverについて

### 1.1 vCenter Serverとは
- vCenter Serverとは､複数のEsxiを束ねて1つの管理画面にて操作することを可能にする統合管理プラットフォーム

### 1.2 vCenter Serverの機能
- vSphere Update Manager 
- vCenter Server Appliance 
- 1つのvCenter Serverで､最大で2000のホスト数及び35000台の仮想マシン
- GUIベースのvSphere Clientにて操作可能

## 2. 今回の実験

### 2.1 概要
&nbsp;今回の実験では､研究室の環境([詳細はこちら](./00-VM-Machines.md))を利用して､vCenter Serverの構築を目指した｡ 

&nbsp;目的として､vCenter Serverのインストール方法や設定方法の理解になれることである｡

### 2.2 vCenter Serverのダウンロード手順

1. vCenterをVMwareからダウンロードする｡

2. ダウンロードフォルダなどに保存したISOイメージをマウントする｡

3. マウントされた中の`VMware VCSA/vcsa-ui-installer/win32/install.exe`ファイルを実行する｡
![stage1の概要](images/20221014vCenterServerInstall/01stage1Summary.png)
*図1 stage1の概要*

4. vCenter Serverのデプロイターゲット欄には､ダウンロード先には､[esxi2のホスト](./00-VM-Network-Overview.md)､Esxiホスト名= `192.168.100.21`､ユーザ名=`root`､パスワード=`password`を入力する｡(esxi2にインストールした際に設定したユーザ名とパスワードを利用してください)

5. vCenter Serverの仮想マシンのセットアップ欄では､仮想マシン名 = `vCenter Server`､rootパスワード=`password` を入力する｡

6. 自分の環境に適したサイズを選択する。今回の場合は、実験用のため最小を選択する。
!["Deploy](images/20221014vCenterServerInstall/02stage1Deploy.png)
*図2 デプロイサイズの選択*

7. vCenterを入れるデータストアを選択する。この際に､`シンディスクモードの有効化`をする必要がある｡(有効化しないと､シックプロビジョニングが選択されるため､仮想ディスク作成時に指定したサイズ分の領域を確保する｡その際に､指定したサイズが大きいため有効化しなければならない｡)
![データストアの選択](images/20221014vCenterServerInstall/03stage1Datastore.png)

8. vCenter自身のFQDN = `esxi4.nlab.local`､vCenterのipアドレス=`192.168.100.21`、サブネットマスク=`255.255.255.0`、デフォルトゲート=`192.168.100.1`、DNSサーバー=`192.168.100.2`を入力した｡([詳細はこちら](./00-VM-Network-Overview.md))
![ネットワークの設定](images/20221014vCenterServerInstall/04stage1Network.png)
*図3 ネットワークの設定*


9. ステージ１のインストールが終了するとステージ２の設定をする
![ステージ1の設定(前半)](images/20221014vCenterServerInstall/05stage1ConfigFirstHalf.png)
*図4 ステージ1の設定(前半)*
![ステージ1の設定(後半)](images/20221014vCenterServerInstall/06stage1LastHalf.png)
*図5 ステージ1の設定(後半)*

10. ステージ2のvCenter Serverの構成にて､時刻同期モード = `NTPサーバーと時間を同期する`を選択し､NTPサーバー = `192.168.100.2` を設定する｡その際に､各Esxi1~4に`192.168.100.1x`にてログインし､管理､システム､日付と時刻に進みNTPサーバー場所を指定する必要がある｡
![Esxi NTP](images/20221014vCenterServerInstall/07esxiNTPConfig.png)

11. SSO構成欄には､Single-Sign-Onドメイン名=`hoge.local`Single-Sign-Onパスワード=`password`を入力する｡また､SSHアクセスを有効にする｡
![ステージ2の設定](images/20221014vCenterServerInstall/08stage2Config.png)
*図6 ステージ2の設定*

12. ステージ2のインストールが完了するとログインする｡
13. esxi4.nlab.localを検索し､`12`にて設定したユーザー名 = `administrator@hoge.local`､パスワード=`password`を入力しログインを行う｡

以上の手順を踏むことによって､vCenter Serverを導入することが出来る｡