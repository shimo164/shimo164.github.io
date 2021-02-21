**Git Memo**

- [git status をエイリアスgstにする](#git-status-をエイリアスgstにする)
- [git pull origin masterのorigin masterは省略できる](#git-pull-origin-masterのorigin-masterは省略できる)
- [各レポジトリの設定を `cat .git/config` で表示](#各レポジトリの設定を-cat-gitconfig-で表示)
- [Github上にあるディレクトリを丸ごと消す](#github上にあるディレクトリを丸ごと消す)
- [github上のrepositoryをローカルに同期する](#github上のrepositoryをローカルに同期する)

## git status をエイリアスgstにする

タイポしてしまうので

`~/.bashrc` に追記する。

```
alias gst='git status'
```

## git pull origin masterのorigin masterは省略できる

originはリポジトリの場所、masterはブランチを指している。多くの場合、デフォルトでは origin masterになっており、省略してよい。


## 各レポジトリの設定を `cat .git/config` で表示

```
[remote "origin"]
        url = https://github.com/user/user.github.io.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
        remote = origin
        merge = refs/heads/master
```

> * branch.master.remote 変数が origin となっているので、git pull するときにリポジトリの指定を省略すると、origin というリモートリポジトリがデフォルトで使用されます。
> * origin というリモートリポジトリ名が実際にどの URL を示すのかは、remote.origin.url 変数に設定されています。[[[Ref]]](https://maku77.github.io/git/other/remote-complement.html)


## Github上にあるディレクトリを丸ごと消す

Github上では消せない。cloneしてローカルでディレクトリを消して(rm -r)、pushする。[[[Ref]]](https://github.community/t5/How-to-use-Git-and-GitHub/How-to-delete-multiples-files-in-Github/td-p/4623)

```
git checkout master
git rm -r dir-name
git commit -m "Remove dir-name"
git push origin master
```


## github上のrepositoryをローカルに同期する

githubでレポジトリrepoがあるとする

アドレスは　`https://github.com/username/repo.git`

cloneすると、repoディレクトリが丸ごとコピーされる

`git clone https://github.com/username/repo.git`

pull してから開始

```
git pull origin master
git init

(編集する)

git add .
git commit -m "first commit"
git remote add origin https://github.com/username/repo.git
git push -u origin master
```
