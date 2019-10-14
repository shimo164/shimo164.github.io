## Git に関するメモ

いろいろなwebに書いてある説明は分かり難い・・・のでまとめていきたい。

※ローカルPCの作業ディレクトリにいるとする

* git hubでレポジトリrepoを作る

アドレスは　`https://github.com/username/repo.git`

cloneする

`git clone https://github.com/username/repo.git`

repoディレクトリが丸ごとコピーされる


* pull する
※先にreponameというディレクトリを作る

```
git pull origin master
git init
git add .
git commit -m "first commit"
git remote add origin https://github.com/username/repo.git
git push -u origin master
```
出だしが噛み合っていなければ先に ←？？噛みあう？？TODO

`git pull origin master`

をする。

**Github上でディレクトリを丸ごと消す**

Github上では消せない。cloneしてローカル上でディレクトリを消して、pushする。
https://github.community/t5/How-to-use-Git-and-GitHub/How-to-delete-multiples-files-in-Github/td-p/4623