# Oracle Database R2 インストール

マニュアル  
https://docs.oracle.com/cd/E82638_01/index.htm  

インストレーションガイド  
https://docs.oracle.com/cd/E82638_01/LADBI/toc.htm  

## インストールサーバチェックリスト

| チェック内容 | タスク |
| :-- | :-- |
| ディスプレイ | 1024 x 768以上のディスプレイ解像度 |
| 最小RAM | 2GBのRAMを推奨します。<br /> Oracle Grid Infrastructureのインストールには、8GB以上のRAMが必要です。|
| Linux x86-64オペレーティング・システムの要件 | 次のLinux x86-64カーネルがサポートされています。<br />Oracle Linux 7およびUnbreakable Enterpriseカーネル3: 3.8.13-35.3.1.el7uek.x86_64以上<br />Oracle Linux 7.2およびUnbreakable Enterpriseカーネル4: 4.1.12-32.2.3.el7uek.x86_64以上<br />Oracle Linux 7およびRed Hat互換カーネル: 3.10.0-123.el7.x86_64以上<br />Red Hat Enterprise Linux 7: 3.10.0-123.el7.x86_64以上<br />Oracle Linux 6.4およびUnbreakable Enterpriseカーネル2:2.6.39-400.211.1.el6uek.x86_64以上<br />Oracle Linux 6.6およびUnbreakable Enterpriseカーネル3: 3.8.13-44.1.1.el6uek.x86_64以上<br />Oracle Linux 6.8およびUnbreakable Enterpriseカーネル4: 4.1.12-37.6.2.el6uek.x86_64以上<br />Oracle Linux 6.4およびRed Hat互換カーネル: 2.6.32-358.el6.x86_64以上<br />Red Hat Enterprise Linux 6.4: 2.6.32-358.el6.x86_64以上<br />SUSE Linux Enterprise Server 12 SP1: 3.12.49-11.1以上<br />パッケージの最小要件のリストは、システム要件に関する項を確認してください。<br /> |
| /tmpディレクトリに割り当てるディスク領域 | /tmpディレクトリに1GB以上の領域。 |
| RAMに対して相対的なスワップ領域割当(Oracle Database) | 1GBから2GB: RAMのサイズの1.5倍<br />2GBから16GB: RAMのサイズに等しい<br />16GBより大きい: 16GB<br />注意: LinuxサーバーでHugePagesを有効にする場合は、スワップ領域を計算する前に、HugePagesに割り当てられるメモリー分を使用可能なRAMから差し引く必要があります。|

**Oracleインベントリ(oraInventory)およびOINSTALLグループの要件**  

* アップグレードの場合は、Oracle Universal Installer (OUI)によって/etc/oraInst.locファイルから既存のoraInventoryディレクトリが検出され、既存のoraInventoryが使用されます。  
* 新規インストールで、oraInventoryディレクトリを構成していない場合、Oracle Grid InfrastructureインストールのOracleベースよりもディレクトリ・レベルが1つ上のOracleインベントリがインストーラによって作成され、インストール所有者のプライマリ・グループがOracle Inventoryグループとして指定されます。  

Oracleインベントリ・ディレクトリは、システムにインストールされているOracleソフトウェアの中央インベントリです。プライマリ・グループがOracle Inventoryグループであるユーザーは、中央インベントリに書込みできるOINSTALL権限が付与されます。  
OINSTALLグループは、サーバー上のすべてのOracleソフトウェア・インストール所有者のプライマリ・グループである必要があります。Oracleインストール所有者によって書込み可能である必要があります。

**グループおよびユーザー**  
インストールを開始する前に、セキュリティ計画に必要なグループおよびユーザー・アカウントを作成することをお薦めします。インストール所有者には、リソース制限設定などの要件があります。グループおよびユーザーの名前には、ASCII文字のみを使用する必要があります。  


## X windows System の用意

**クライアント側**  

windows用 X Window Serverをインストール  

例）VcXsrv  
https://sourceforge.net/projects/vcxsrv/  


**サーバ側**  

```
# yum groupinstall "GNOME Desktop"
# systemctl reboot
```

確認  

```
# firefox &
```

WindowsローカルのX Windows Serverでfirefoxが起動することを確認  

## 必要なパッケージのインストール  

| 項目 | 要件 |
| :-- | :-- |
| Red Hat Enterprise Linux7 | binutils-2.23.52.0.1-12.el7 (x86_64)<br />compat-libcap1-1.10-3.el7 (x86_64)<br />compat-libstdc++-33-3.2.3-71.el7 (i686)<br />compat-libstdc++-33-3.2.3-71.el7 (x86_64)<br />glibc-2.17-36.el7 (i686)<br />glibc-2.17-36.el7 (x86_64)<br />glibc-devel-2.17-36.el7 (i686)<br />glibc-devel-2.17-36.el7 (x86_64)<br />ksh<br />libaio-0.3.109-9.el7 (i686)<br />libaio-0.3.109-9.el7 (x86_64)<br />libaio-devel-0.3.109-9.el7 (i686)<br />libaio-devel-0.3.109-9.el7 (x86_64) <br />libgcc-4.8.2-3.el7 (i686)<br />libgcc-4.8.2-3.el7 (x86_64)<br />libstdc++-4.8.2-3.el7 (i686)<br />libstdc++-4.8.2-3.el7 (x86_64)<br />libstdc++-devel-4.8.2-3.el7 (i686)<br />libstdc++-devel-4.8.2-3.el7 (x86_64)<br />libxcb-1.9-5.el7 (i686)<br />libxcb-1.9-5.el7 (x86_64)<br />libX11-1.6.0-2.1.el7 (i686)<br />libX11-1.6.0-2.1.el7 (x86_64)<br />libXau-1.0.8-2.1.el7 (i686)<br />libXau-1.0.8-2.1.el7 (x86_64)<br />libXi-1.7.2-1.el7 (i686)<br />libXi-1.7.2-1.el7 (x86_64)<br />libXtst-1.2.2-1.el7 (i686)<br />libXtst-1.2.2-1.el7 (x86_64)<br />make-3.82-19.el7 (x86_64)<br />net-tools-2.0-0.17.20131004git.el7 (x86_64)(Oracle RACおよびOracle Clusterware用)<br />nfs-utils-1.3.0-0.21.el7.x86_64 (Oracle ACFS用)<br />smartmontools-6.2-4.el7 (x86_64)<br />sysstat-10.1.5-1.el7 (x86_64)<br />gcc-c++-4.8.2 |

i686パッケージはパッケージ名に指定してインストール  

```
# yum compat-libstdc++-33.i686
```

## カーネルパラメータ調整

| パラメーター | 設定値 |
| :-- | :-- |
| fs.file-max | 6815744 |
| fs.aio-max-nr | 1048576 |
| kernel.sem | 250 32000 100 128 |
| kernel.shmmni | 4096 |
| kernel.shmall | 1073741824 |
| kernel.shmmax | 4398046511104 |
| kernel.panic_on_oops | 1 |
| net.core.rmem_default | 262144 |
| net.core.rmem_max | 4194304 |
| net.core.wmem_default | 262144 |
| net.core.wmem_max | 1048576 |
| net.ipv4.conf.all.rp_filter | 2 |
| net.ipv4.conf.default.rp_filter | 2 |
| net.ipv4.ip_local_port_range | 9000 65500 |

```
# vim /etc/sysctl.conf
fs.file-max = 6815744
fs.aio-max-nr = 1048576
kernel.sem = 250 32000 100 128
kernel.shmmni = 4096
kernel.shmall = 1073741824
kernel.shmmax = 4398046511104
kernel.panic_on_oops = 1
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
net.ipv4.conf.all.rp_filter = 2
net.ipv4.conf.default.rp_filter = 2
net.ipv4.ip_local_port_range = 9000 65500
```

## ユーザー制限の上限解除

```
# vim /etc/security/limits.conf
@oinstall soft nofile 1024
@oinstall hard nofile 65536
@oinstall soft nproc 16384
@oinstall hard nproc 16384
@oinstall soft stack 10240
@oinstall hard stack 32768
@oinstall soft memlock 134217728
@oinstall hard memlock 134217728
```

## インストールユーザーの作成

**グループの作成**  

```
# groupadd -g 1100 oinstall
# groupadd -g 1101 dba
```

**ユーザー作成**  

```
# useradd -u 1100 oracle -g oinstall -G dba
# passwd oracle
oracle
```

OSに対象アカウントで接続  

```
$ vim .bash_profile
umask 022
```

## hosts 設定

```
# vim /etc/hosts
10.128.59.110   shmdodb-01
```

コンピュータ自体でpingコマンドを実行すると、pingコマンドによりそのコンピュータのIPアドレスが戻される事を確認  

## パッケージのダウンロード
http://www.oracle.com/technetwork/jp/database/enterprise-edition/downloads/index.html  

Oracleアカウントが必要  

```
$ unzip linuxx64_12201_database.zip
# mv database /opt
$ cd /opt/database
$ ./runInstaller
Oracle Universal Installerを起動中です...

一時領域の確認中: 500MBを超えている必要があります.   実際 66141MB    問題なし
スワップ領域の確認中: 150MBを超えている必要があります.   実際 3815MB    問題なし
モニターの確認中: 少なくとも256色表示するよう設定されている必要があります.    実際 16777216    問題なし
Oracle Universal Installerの起動を準備中 /tmp/OraInstall2018-03-20_09-14-48PM. お待ちください...
```

**セキュリティアップデートの構成**  

<img src="images/oracle_database_12cr2_install01.png" width="50%">  

**インストールオプション**  
ここでは「データベース・ソフトウェアのインストール」を選択し、DBの作成は別途後で行う  

<img src="images/oracle_database_12cr2_install02.png" width="50%">  

**データベースインストールオプションの選択**  
用途に応じて選択  

<img src="images/oracle_database_12cr2_install03.png" width="50%">  

**データベース・エディションの選択**  
用途に応じて選択  

<img src="images/oracle_database_12cr2_install04.png" width="50%">  

**インストール場所の指定**  

* Oracleベース  

Oracle12cからOracleパッケージのディレクトリ構成が整理されています  
Oracleベースは全てのOracle製品にて使用されるためt適当に考えない  

`/opt/oracle/data/`

Oracleベースは`oracle`ユーザー権限で書き込みができる必要があります  


* ソフトウェアの場所  

Oracleベース配下に作成する  

`/opt/oracle/data/product/12.2.0/dnhome_1`

<img src="images/oracle_database_12cr2_install05.png" width="50%">  

**インベントリの作成**  
Oracle製品のインベントリファイルを作成するディレクトリと管理グループを指定  
Oracleベース配下  
こちらもOracle製品共通で利用される  

`/opt/oracle/oraInventory`  
`oinstall`  

<img src="images/oracle_database_12cr2_install06.png" width="50%">  

**権限あるオペレーティング・システム・グループ**  
OracleDBに対して管理者権限を得られるグループの指定  

<img src="images/oracle_database_12cr2_install07.png" width="50%">  

**前提条件のチェック**  

<img src="images/oracle_database_12cr2_install08.png" width="50%">  

**サマリー**  

<img src="images/oracle_database_12cr2_install09.png" width="50%">  

**スクリプトの実行**  

<img src="images/oracle_database_12cr2_install10.png" width="50%">  

rootユーザーにて実行  

```
# /opt/oracle/oraInventory/orainstRoot.sh
権限を変更中 /opt/oracle/oraInventory.
グループの読取り/書込み権限を追加中。
全ユーザーの読取り/書込み/実行権限を削除中。

グループ名の変更 /opt/oracle/oraInventory 宛先 oinstall.
スクリプトの実行が完了しました。
```

```
# /opt/oracle/data/product/12.2.0/dbhome_1/root.sh
Performing root user operation.

The following environment variables are set as:
    ORACLE_OWNER= oracle
    ORACLE_HOME=  /opt/oracle/data/product/12.2.0/dbhome_1

Enter the full pathname of the local bin directory: [/usr/local/bin]:
   Copying dbhome to /usr/local/bin ...
   Copying oraenv to /usr/local/bin ...
   Copying coraenv to /usr/local/bin ...


Creating /etc/oratab file...
Entries will be added to the /etc/oratab file as needed by
Database Configuration Assistant when a database is created
Finished running generic part of root script.
Now product-specific root actions will be performed.
Do you want to setup Oracle Trace File Analyzer (TFA) now ? yes|[no] :
yes
Installing Oracle Trace File Analyzer (TFA).
Log File: /opt/oracle/data/product/12.2.0/dbhome_1/install/root_shmdodb-01_2018-03-20_22-16-01-239144330.log
Finished installing Oracle Trace File Analyzer (TFA)
```

* TFA  
  https://docs.oracle.com/cd/E82638_01/ATNMS/understanding-tfa.htm  


**終了**  

<img src="images/oracle_database_12cr2_install11.png" width="50%">  

oracleユーザーのプロファイルに以下を定義しておくと便利  

```
$ vim .bash_profile
# Oracle Add
umask 022
ORACLE_BASE=/opt/oracle/data
ORACLE_HOME=$ORACLE_BASE/product/12.2.0/dbhome_1

PATH=$PATH:$HOME/.local/bin:$HOME/bin:$ORACLE_HOME/bin

export PATH
```

## DBCAによるDB作成  

http://www.oracle.com/technetwork/jp/database/enterprise-edition/documentation/sidb12201-inst-linux-x64-ja-v10-3627443-ja.pdf  

**データベース操作**  

```
$ dbca
```

<img src="images/oracle_database_12cr2_dbca01.png" width="50%">  

**作成モード**  
拡張構成を選択  

<img src="images/oracle_database_12cr2_dbca02.png" width="50%">  

**デプロイ・タイプ**  

* データベースタイプ  
  Oracle単一インスタンス・データベース
* テンプレートデータベース  

<img src="images/oracle_database_12cr2_dbca03.png" width="50%">  

**データベースの識別**  

<img src="images/oracle_database_12cr2_dbca04.png" width="50%">  

**記憶域オプション**  

<img src="images/oracle_database_12cr2_dbca05.png" width="50%">  

**高速リカバリオプション**  

<img src="images/oracle_database_12cr2_dbca06.png" width="50%">  

**ネットワーク構成**  

* リスナー名  
  LISTENER
* ポート  
  1521

<img src="images/oracle_database_12cr2_dbca07.png" width="50%">  

**データベースオプション**  

<img src="images/oracle_database_12cr2_dbca08.png" width="50%">  

**構成オプション**  

自動管理メモリで上限を1048MB（/dev/shmサイズに依存）  

<img src="images/oracle_database_12cr2_dbca09.png" width="50%">  

最大接続数 300

<img src="images/oracle_database_12cr2_dbca10.png" width="50%">  

キャラセットを AL32UTF8

<img src="images/oracle_database_12cr2_dbca11.png" width="50%">  

専用モード

<img src="images/oracle_database_12cr2_dbca12.png" width="50%">  

サンプルスキーマはいらないんじゃね？

<img src="images/oracle_database_12cr2_dbca13.png" width="50%">  

**管理オプション**  

EnterpriseManager 5500 ポート  

<img src="images/oracle_database_12cr2_dbca14.png" width="50%">  

**ユーザー資格情報**  
インスタンス管理者情報  

sys/****  
system/****  

<img src="images/oracle_database_12cr2_dbca15.png" width="50%">  

**作成オプション**

<img src="images/oracle_database_12cr2_dbca16.png" width="50%">  

記憶領域のカスタマイズ  

<img src="images/oracle_database_12cr2_dbca17.png" width="50%">  

**終了**  

<img src="images/oracle_database_12cr2_dbca18.png" width="50%">  

## 接続

リスナーへの疎通確認  

```
$ tnsping shmdodb
$ lsnrctl status
```


環境変数の設定  

```
$ export ORACLE_SID=shmdodb
$ export ORACLE_HOME=/opt/oracle/data/product/12.2.0/dbhome_1
$ /opt/oracle/data/product/12.2.0/dbhome_1/bin/sqlplus system/***** as sysdba
$ export NLS_LANG=Japanese_Japan.AL32UTF8
$ sqlplus sys/**** as sysdba
```

インスタンス起動  

```sql
startup
```

※多分起動してる  

## 表領域の作成

```sql
CREATE BIGFILE TABLESPACE SCHEME1TBS DATAFILE '/opt/oracle/data/oradata/SHMDODB/datafile/shmdodb_scheme1tbs.dbf' SIZE 40G AUTOEXTEND ON NEXT 10M MAXSIZE UNLIMITED;

CREATE TABLESPACE SCHEME2TBS DATAFILE '/opt/oracle/data/oradata/SHMDODB/datafile/shmdodb_scheme2tbs.dbf' SIZE 5G AUTOEXTEND ON NEXT 10M MAXSIZE UNLIMITED;
```

標準のブロックサイズ数を超えるサイズのテーブルスペースを作る場合は`bigfile`で作る  
> ORA-01144:ファイル・サイズ(5242880ブロック)が最大値4194303ブロックを超えています。  

```sql
SELECT * FROM DBA_DATA_FILES;
```

## スキーマ（ユーザー）の作成

```sql
CREATE USER SCHEME1 IDENTIFIED BY "scheme1tbs" DEFAULT TABLESPACE SHMTBS TEMPORARY TABLESPACE TEMP;

ユーザーが作成されました。

CREATE USER SCHEME2 IDENTIFIED BY "scheme2tbs" DEFAULT TABLESPACE CONTBS TEMPORARY TABLESPACE TEMP;

ユーザーが作成されました。
```

確認  

```sql
select username,default_tablespace,temporary_tablespace from dba_users;
```

## その他調整  

* パスワード有効期限の解除  

```sql
select * from dba_profiles where resource_name = 'PASSWORD_LIFE_TIME';
alter profile DEFAULT limit PASSWORD_LIFE_TIME unlimited;
alter profile DEFAULT limit PASSWORD_GRACE_TIME unlimited;
```

* アーカイブログ  
開発環境は停止する  

```sql
ARCHIVE LOG LIST;

データベース・ログ・モード     アーカイブ・モード
自動アーカイブ                 有効
アーカイブ先                    USE_DB_RECOVERY_FILE_DEST
最も古いオンライン・ログ順序   15
アーカイブする次のログ順序    17
現行のログ順序               17

SHUTDOWN IMMEDIATE;
STARTUP MOUNT;
ALTER DATABASE NOARCHIVELOG;
ALTER DATABASE OPEN;
```

## インスタンスの停止

```sql
SHUTDOWN IMMEDIATE
```

```
$ lsnrctl stop
```

## システム権限の追加

システム権限の確認  


```sql
SELECT PRIVILEGE FROM SESSION_PRIVS ORDER BY PRIVILEGE;
```

システム権限をユーザーに追加する  

```sql
GRANT CREATE DATABASE LINK, CREATE PROCEDURE, CREATE SEQUENCE, CREATE SESSION, CREATE SYNONYM, CREATE TABLE, CREATE TRIGGER, CREATE VIEW TO scheme1;
GRANT CREATE DATABASE LINK, CREATE PROCEDURE, CREATE SEQUENCE, CREATE SESSION, CREATE SYNONYM, CREATE TABLE, CREATE TRIGGER, CREATE VIEW TO scheme2;
```

sqlplusでログインできる事を確認  

```
$ sqlplus scheme1/*****

SQL*Plus: Release 12.2.0.1.0 Production on 木 4月 5 17:28:28 2018

Copyright (c) 1982, 2016, Oracle.  All rights reserved.



Oracle Database 12c Standard Edition Release 12.2.0.1.0 - 64bit Production
に接続されました。
SQL>
```

## 表領域への書き込み権限

表領域に対するオブジェクト制限指定  
下記は対象表領域に対して無制限のオブジェクト権限を追加している。  

```sql
alter user scheme1 quota unlimited on scheme1tbs;
alter user scheme2 quota unlimited on scheme2tbs;
```