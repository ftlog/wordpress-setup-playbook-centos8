# Wordpress + Nginx + MariaDB + CentOS8 + Let's Encrypt

## 概要

CentOS8でWordPress環境を構築するPlaybookを作成しました。

以下の設定に対応しています。

1. remiレポジトリ
1. nginx
1. PHP
1. MariaDB
1. WordPress
1. ファイアウォール
1. サービス起動
1. Let's Encrypt

※ Let's Encrypt設定時に、外部からWeb(80/443)アクセスできる必要があります。

## 動作確認環境

■ コントロールノード  
＜ansible＞  
Raspbian GNU/Linux 10 (buster)  
ansible 2.10.2  
$ ansible-galay collection install ansible.posix  
Python 3.7.3  

＜AWX＞  
CentOS Linux release 8.2.2004 (Core)  
Docker version 19.03.13   
docker-compose version 1.27.4  
AWX 15.0.1
- ansible 2.9.14
- python 3.6.8

■ ターゲットノード  
さくらのVPS 標準OS CentOS8 x86_64

## 設定方法
※AWXから実行する場合は、テンプレートの追加変数に設定してください。  

### group_vars/www.yml または host_vars/server01.yml
※ファイル名は、hostsに合わせて変更してください。

接続情報を記載します。 
```yaml
ansible_ssh_user: user01
ansible_become_password: user01_passowrd
ansible_ssh_private_key_file: /path/to/id_rsa_vps

```

WordPressをインストールする場所、サーバ名(FQDN)、Let's Encryptで登録するメールアドレスを記載します。  
※ wp_dirで指定したディレクトリ配下に、wordpressディレクトリが作成されます。
```yaml
wp_dir: /usr/share/nginx
server_name: server01.example.com
admin_mail: server01@example.com
```
DB接続情報を記載します。  
DB_LOGIN_USER以外は適宜変更してください。  

```yaml
DB_LOGIN_USER: root
DB_LOGIN_PASSWORD: wordpress
DB_NAME: wordpress
DB_USER: wordpress
DB_PASSWORD: wordpress
```

### hosts

設定するサーバーのリストを記載します。  
IPアドレスまたは名前解決可能なホスト名を指定します。
```yaml
[www]
server01
```

## 使い方

git cloneをします。

```
$ git clone https://github.com/ftlog/wordpress-setup-playbook-centos8
$ cd wordpress-setup-playbook-centos8
```

設定が終わった後、以下のコマンドを実行します。  
※enable_ssl.ymlは、WordPressの設定が終わった後、外部公開時に実行してください。

```
$ ansible-playbook site.yml
$ ansible-playbook enable_ssl.yml
```
## ライセンス

[MIT License](https://opensource.org/licenses/mit-license.php)