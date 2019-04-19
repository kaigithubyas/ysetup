保田セットアップブック
====
概要

仮想マシン立て直し後の環境構築を省力化するために作ったプレイブックです。

パッケージインストール、エイリアス設定、vim エディタのカラーテーマ変更を自動で行います。


## パッケージインストール

・以下のパッケージを yum install します。

epel-release

ansible

git

vim


## エイリアス設定

入力の省力化に便利なエイリアス一式を設定します。/etc/bashrc の末尾に書き込まれるため、この変更は全ユーザに適用されます。

・cd .. を省力化、 typo 対策

alias ..='cd ..'

alias ..2='cd ../..'

alias ..3='cd ../../..'

alias cd..='cd ..'


・cd 成功時に自動的に ls

function cdl() {

  \cd "$@" && ls
  
}

alias cd='cdl'


・mkdirc で mkdir -p でネスト化したディレクトリを作成した後、最深部へ移動。

function mkdirc(){

  mkdir -p "$@" && \cd "$@"
  
}


・その他短縮系

alias hs='history | grep'

alias a='ansible-playbook'

alias al='ansible-playbook -i localhost, -c local'

alias vi='vim'


## vim

・vim のカラーテーマを molokai に変更

以下のディレクトリの配下に colors ディレクトリを pull し、molokai テーマファイルを置きます。ない場合自動的に作ります。

/root/.vim

/home/vagrant/.vim

/etc/vimrc


・etc/vimrc に以下の内容を書き込みます。ない場合は作成されます。

　molokai 適用するための設定と vim エディタでバックスペースが利かなくなった時対策の処置です。
 
colorscheme molokai

syntax on

set background=dark

set backspace=indent,eol,start


・256色対応のため、環境変数 TERM をxterm-256colorに変更します。/etc/bashrc に以下の内容が書き込まれます。

EXPORT TERM=xterm-256color



## 使い方

1. playbook をリモートリポジトリから pull する。（このページからダウンロードする、と読み替えて差し支えないです。）カレントディレクトリに ysetup ディレクトリが作られます。
 
$ git clone https://github.com/kaigithubyas/ysetup

2. カレントディレクトリを変更

$ cd ysetup

3. playbook を実行。

ローカルホスト宛に適用したい場合

$ ansible-playbook -i localhost, -c local ysetup.yml

エイリアスだけほしい場合
$ ansible-playbook -i localhost, -c local aliases.yml

別の仮想マシンに適用したい場合は hosts ファイルを編集の上、以下のコマンドを実行。

$ ansible-playbook -i hosts ysetup.yml

