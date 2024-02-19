# Git 配置

## 安装git/git-lfs

## 安装meld

## gitconfig配置
配置文件(${HOME}/.gitconfig)
```bash
[http] 
    postBuffer = 1048576000
    sslVerify = false
[filter "lfs"]
    clean = git-lfs clean -- %f
    smudge = git-lfs smudge --skip -- %f
    process = git-lfs filter-process --skip
    required = true
[lfs]
    url = https://lfs.xxx.com/
[credential]
    helper = store
[user]
    name = demo
    email = demo@xxx.com
[url "ssh://git@github.com/"]
    insteadOf = https://github.com/
[merge]
    tool = meld
[mergetool "meld"]
    cmd = meld $LOCAL $BASE $REMOTE --output=$MERGED
```

## Git clean
```
git clean -dfx
```

## Git submodule
```
git submodule add xxx

git submodule init
git submodule update

git submodule deinit xxx
git rm xxx
```

## Git Tag 删除操作
```bash
git tag -d v20100
git push origin :refs/tags/v20100
git push origin --tags
```

## Git unable to update local ref 
```bash
git gc
git remote prune origin
```

## .gitignore使用
```bash
# 配置demo
# log/
```
## .gitattributes使用
```bash
# 配置demo
# *.kep filter=lfs diff=lfs merge=lfs -text
# 手动拉取
git-lfs pull
```