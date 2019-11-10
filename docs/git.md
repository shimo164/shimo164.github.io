## Git に関するメモ

### git status をエイリアスgstにする

`~/.bashrc` に追記する。

```
alias gst='git status'
```


### Github上でディレクトリを丸ごと消す

Github上では消せないので、cloneしてローカルでディレクトリを消して(rm -r)、pushする。

```
git checkout master
git rm -r dir-name
git commit -m "Remove dir-name"
git push orign master
```
https://github.community/t5/How-to-use-Git-and-GitHub/How-to-delete-multiples-files-in-Github/td-p/4623

# TODO 以下見直し

### github上のrepositoryをローカルに

* githubでレポジトリrepoがあるとする

アドレスは　`https://github.com/username/repo.git`

* clone

`git clone https://github.com/username/repo.git`

repoディレクトリが丸ごとコピーされる

* pull して同期する

```
git pull origin master
git init
git add .
git commit -m "first commit"
git remote add origin https://github.com/username/repo.git
git push -u origin master
```
コンフリクトしていなければ

`git pull origin master`
