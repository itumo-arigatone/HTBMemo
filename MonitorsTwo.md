# HackTheBoxとは
userフラグとrootフラグを取得するゲームのようなもの。

# 事前準備
vpnにつなげます。

右上のconnect to htb > Starting Point > OpenVPN > download VPN

sudo openvpn ~~~.ovpn
右上が2 connectionsという表記に変わる

# 実施
Machinesに行く。

好きな難易度のマシーンを選ぶ

今回はMonitorsTwo

join machineをクリック

IPアドレスが出る

nmap
```
nmap -sV 10.10.11.211
```
sVで詳細見れる

22(ssh)と80(http)が空いている。

サイト行ってみる

右下。cacti 1.2.22に脆弱性があるかみてみる

https://iototsecnews.jp/2023/01/14/most-internet-exposed-cacti-servers-exposed-to-hacking/

https://github.com/sAsPeCt488/CVE-2022-46169

ハッキングする系のスクリプトはそれ自体がマルウェアかもしれないのでソースを見ておいた方が良い

今回のは任意のコマンドが実行できる脆弱性のため、リバースシェルを行います。

リバースシェル: https://tech.kusuwada.com/entry/2019/10/30/044325

これでサーバーに侵入できます。あとはフラグを探すのですが、準備時間が足りずフラグを見つけることができませんでした。

https://www.linkedin.com/pulse/htb-monitorstwo-writeup-divyanshu-sharma
