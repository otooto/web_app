# webアプリケーション製作ログ

自分の技術向上の為にwebアプリケーションの作成を行った。その為の議事録とメモ

# 作業方針

この[サイト](https://jsprimer.net/use-case/todoapp/entrypoint/)に沿ってTODOアプリの開発方法を学ぶ。そして最終的には自分専用のTODOアプリを開発する。

# 環境

+ wsl2

+ ubuntu20.04

+ Google Chrome

# first step

+ ローカルサーバの用意
+ ローカルサーバ上でのスクリプト実行

## ローカルサーバの用意

JavaScriptモジュールを使用するためにはサーバが必要。今回はnginxを使用する。

最初はubuntuのアップデートを行う

```
sudo apt update
sudo apt upgread
```

次にnginxのインストールと実行
```
sudo apt -y install nginx

sudo nginx

sudo nginx 0s stop(reload)
```
ブラウザ側から`localhost`をURLとして入力するとnginxのデフォルトページが表示される。

なおWSL2ではsystemctlコマンドが使用できない

```
$ systemctl
System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down
```
こうなるとsystemctl関連のドキュメントを利用する際に支障がでるので実行できるようにする。

ここの[記事](https://shikiyura.com/2020/06/execute_systemctl_on_wsl2/)を参照する。

# ローカルサーバ上でのスクリプト実行

nginxの用意は出来た。しかし現時点ではデフォルトのページを表示するだけで、自身の作成しているページとの連携ができない。

そこでnginxのドキュメントルートの設定を変更する。

まずは設定ファイルを調べる。ファイルは以下になる。
```
cat /etc/nginx/sites-available/default
```
nginx起動時にhtmlのファイルを指定している部分は以下になる

```
root /var/www/html;

# Add index.php to the list if you are using PHP
index index.html index.htm index.nginx-debian.html;
```
実際に`/var/www/html`を調べるとデフォルトページに書かれている内容のhtmlファイルが存在する。

ここを自分の設定ファイルがあるディレクトリする。ディレクトリのpathは、対象ディレクトリがある位置で`pwdコマンド`を入力すれば分かる

```
root /home/username/HTML/TODO;

index index.html
```
この状態で最後nginxを再起動する。systemctl関連をしてないなら再度ubuntuを起動する。

ブラウザで`localhost`を入力すると、今度は自分が作ったindex.hmtlの内容が表示される。

## Chromeのデベロッパーツールから確認

localhostを開いたChromeのページで、自身のアカウントアイコン隣にある下へ向かっている三点をクリック

その他 → デベロッパーツールを開くとデベロッパーツールが展開

Consoleを開くと `index.js: loaded`を見ることができる。

ここでエラーが出ている場合は、何か設定が足りないかファイル記述が間違っている。

# second step

+ コードの模写
+ CSSをローカルへ
+ nginxを起動し、localhost側から接続


## コードの模写

実際にここはサイトにある通りにコードを模写しただけ。

## CSSをローカルへ

`index.html`に記述内容にCSSファイルを参照している場所がある。サイトのままではネット上のURLを引っ張ているがそれをローカルから参照する。CSSファイルは`indexファイル`があるディレクトリと同じにする。

```
<link
      href="./index.css"
      rel="stylesheet"
    />

```

##　nginxを起動し、localhost側から接続

ここがちょっとした罠だった。

ubuntu上から`html`を実行することがコマンドで出来るのだが、その方式では直接PATHをブラウザ上に反映しているので、`nginx`が無視されてしまう。

その為に、TODOリストの追加機能が動作しなかった。

正式な手順としては

+ nginxを起動する
+ localhostからアクセスする

これによって、サイト上の想定された動作を自身の環境からでも行えた。



