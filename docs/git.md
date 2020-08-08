## Git に関するメモ

### git status をエイリアスgstにする

`~/.bashrc` に追記する。

```
alias gst='git status'
```

### git pull orign masterのorigin masterは省略できる

originはリポジトリの場所、masterはブランチを指している。おおむねデフォルトでは origin masterになっており、省略してよい。

**確認する**

各レポジトリの設定を `cat .git/config` で表示。

```
[remote "origin"]
        url = https://github.com/shimo164/shimo164.github.io.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
        remote = origin
        merge = refs/heads/master
```

> * branch.master.remote 変数が origin となっているので、git pull するときにリポジトリの指定を省略すると、origin というリモートリポジトリがデフォルトで使用されます。 
> * origin というリモートリポジトリ名が実際にどの URL を示すのかは、remote.origin.url 変数に設定されています。[Ref](https://maku77.github.io/git/other/remote-complement.html)


### Github上でディレクトリを丸ごと消す

Github上では消せないので、cloneしてローカルでディレクトリを消して(rm -r)、pushする。

```
git checkout master
git rm -r dir-name
git commit -m "Remove dir-name"
git push orign master
```
https://github.community/t5/How-to-use-Git-and-GitHub/How-to-delete-multiples-files-in-Github/td-p/4623


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

# TODO 以下見直し

コンフリクトしていなければ

`git pull origin master`
