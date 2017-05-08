### Webrtc笔记-编译Framework动态库报错

1. 在使用运行*build_ios_libs.sh*打包生成基于iOS平台的动态库时，如果使用的是**Xcode8.3**以上的版本，会出现如下错误：

    ![运行build_ios_libs.sh报错](uploads/images/build_ios_libs_error.png)

    这是因为clang编译器版本的问题，在WebRTC官方给出了一个补丁[issue2833833002\_1\_10001.diff](https://codereview.chromium.org/download/issue2833833002_1_10001.diff)。

2. 如何打上补丁
	2.1 假设把issue2833833002\_1\_10001.diff下载到\~/Download目录
	2.2 进入webrtc工程的build目录：cd webrtc工程路径/build
	2.3 打补丁：patch -p2 -i \~/downloaded/issue2833833002\_1\_10001.diff

3. 最后再编译就ok了。


