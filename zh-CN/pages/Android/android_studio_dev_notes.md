# WebRTC 使用Android Studio开发笔记

** 最后更新 2017-05-09  ** by linzq

## 生成AS项目

具体内容请看 [Android产生Android_Studio编译配置](../cmd.md#Android产生Android_Studio编译配置)

## --split-projects 错误

如果你执行生成项目脚本出现`--split-projects`错误，例如[这个链接描述的情况](https://groups.google.com/d/msg/discuss-webrtc/b7yQjvPLHaM/N0wa9PMnEQAJ)。

可以直接在编译脚本后面带上`--split-projects`参数，或是使用[这个cl的修改](https://chromium.googlesource.com/chromium/src/+/8a4451c911ee05e56000474c58bfad89f744a35e%5E%21/#F0)。

## R文件编译出错

解决办法一： [第一次Import项目的时候，会出现R文件编译报错](../android-note/build/webrtc_0.md#五、踩过的问题)

解决办法二： 把你的Android Studio降回 `2.2` 版本，官方的说法是目前只支持2.2。后续版本暂未支持 
