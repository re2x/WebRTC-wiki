# 基于Release分支的开发

## 开发端配置

至少执行了一次[完整的代码拉取](https://webrtc.org/native-code/development/)

切换为内网git服务器，内网git地址为：ssh://webrtc@192.168.51.107/webrtc

执行
```
# 修改git远程库url为内网地址
git remote set-url origin ssh://webrtc@192.168.51.107/webrtc
# checkout 59版本
git checkout -b branch_59 branch_59
# 同步59版本的工具链和依赖库
gclient sync --jobs 16
```

通过执行以上命令，切换到了内网服务器，并同步了release版本59相关的工具链和依赖库

代码提交请参考[git手册](http://gitref.org/zh/creating/)，编译方面和使用官方库一直

## git服务器配置

拉取webrtc.git代码

```
git clone https://chromium.googlesource.com/external/webrtc.git
```

修改 `.git/config` 文件，在 ` [remote “origin”] `节点增加

```
fetch = +refs/branch-heads/*:refs/remotes/branch-heads/*
```

执行如下命令，拉取release分支

``` bash
# Make sure you are in 'src'.
# This part should only need to be done once, but it won't hurt to repeat it.  The first
# time might take a while because it fetches an extra 1/2 GB or so of branch commits. 
# gclient sync --with_branch_heads

# You may have to explicitly 'git fetch origin' to pull branch-heads/
git fetch

# Checkout the branch 'src' tree.  目前最新的release版本为59
git checkout -b branch_59 branch-heads/59

# Checkout all the submodules at their branch DEPS revisions.
#gclient sync --jobs 16
```

### 修改git库为bare
```
git config --bool core.bare false
```
<!--

## 切换回master分支

``` bash
# Make sure you are in 'src'.
git checkout -f master
gclient sync --jobs 16
```

## git修改远程仓库地址
方法有三种：

1.修改命令
```
git remote set-url origin [url]
```

2.
```
git remote rm origin
git remote add origin [url]
```

3.直接修改.git/config文件
-->


## 参考

* [Working with Release Branches](https://www.chromium.org/developers/how-tos/get-the-code/working-with-release-branches)
* [git修改远程仓库地址](https://ddnode.com/2015/04/14/git-modify-remote-responsity-url.html)