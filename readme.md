# Node開発環境用Vagrantfile

## モチベーション
webpackなどを使ったタスクランナー環境をvagrant上に短時間で簡単に構築できるように作りました。
## 概要
vagrant 環境を利用してnodeのタスクランナーを使った開発を行うための
環境をプロビジョニングするVagrantfileです。
Vagrantfileを配置してvagrant upするだけで環境がnodeによる開発環境が作成されます。
ゲストOS(ubuntu)のフォルダ内でnodeを使って開発できます。
フォルダ(/dockerd)はSambaでファイル共有されるので、ホストマシンから自由にアクセスできます。
フォルダは Apache2の8080でweb共有されるので、webサーバでの動作確認が行えます。
n を使って node のバージョンを管理できます。

## 使用法
ダウンロードしたVagrantdを配置したフォルダから vagrant up するだけです。
```sh
  $ ### 使用例：
  $ mkdir vagrantUbuntuDirectory
  $ cd vagrantUbuntuDirectory
  $ git clone https://github.com/shinjixxxxx/Vaglantfile-ubuntu.git
  $ vagrant up
  $ vagrant ssh
  server vagrantd_ubuntu:user $ cd /dockerd
  server vagrantd_ubuntu:user $ mkdir projedt
  server vagrantd_ubuntu:user $ cd projedt
  server vagrantd_ubuntu:user $ npm init
  server vagrantd_ubuntu:user $ npm imstall react react-dom
  server vagrantd_ubuntu:user $ npm imstall -D webpack webpack-cli babel-loader @babel/core @babel/preset-env @babel/preset-react
  server vagrantd_ubuntu:user $ mkdir src
  server vagrantd_ubuntu:user $ touch src/index.jsx
  server vagrantd_ubuntu:user $ touch webpack.config.js
```
  
  
---
## 詳細
linuxの環境とファイルシステムでnodeを稼働するため、
windowsやmacintoshなどの環境の違いによって生じる問題を回避します。
ホストマシンの環境に影響されないようにゲストマシンのファイルはsambaで共有します。
Samba共有にはdockerのdeperson/sambaを使用します。

- 開発ディレクトリ（/vagrantd）を http://192.168.33.10:8080 で見えるようにします。
- 開発ディレクトリ（/vagrantd）を deperson/sambaでwindows ネットワーク共有で
  ホストのシステムとファイル共有します。

### 開発ディレクトリ
8080ポートでhttpアクセスできます。
sambaでファイル共有されます。
- /dockerd
### ファイルへのアクセス
#### Macintoshの場合
ファインダーのメニューから
　移動 ＞ サーバーへ接続
を選んで以下のurlからアクセス。
smb://192.168.33.10
#### windowsの場合
エクスプローラのアドレスに以下のurlからアクセス。
¥¥192.168.33.10
### httpアクセス
```
  $ curl http://192.168.33.10:8080
```

### インストールソフト 
最低限の開発環境として以下のアプリがインストールされます。
- apache2
- php
- node
- npm 
- n
- docker
- emacs

## 環境
```
vagrant@ubuntu-bionic:~$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 18.04.4 LTS
Release:	18.04
Codename:	bionic
```
