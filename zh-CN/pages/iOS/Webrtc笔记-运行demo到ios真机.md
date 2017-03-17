# Webrtc笔记-运行demo到ios真机

>接下来编译源码
>**目标：运行ios的demo到真机上**

##ios生成xcode项目

**可产生在真机或者模拟器上面运行的项目**

/src文件目录下执行:

* **真机项目：**

```sh
gn gen out/ios_64 --args='target_os="ios" target_cpu="arm64"' --ide=xcode
```
* **模拟器项目：**

```sh
gn gen out/ios_sim --args='target_os="ios" target_cpu="x64"' --ide=xcode
```

gn属性 | 描述
--------- | -------------
target_os | 默认值是运行脚本的任何操作系统，运行到ios系统即赋值“ios”
target_cpu| 根据设备的系统架构将其设置为“arm”或“arm64”或"x64"

执行命令成功后，会看到以下的提示


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1528347-9452136ed01503de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


并且在`out/对应的文件夹`下生成了xcode项目,如下图


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1528347-83693c1e19f56675.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开`all.xcworkspace`就在ide中看到完整的WebRTC项目了！

关于gn的一些操作可以[参考这里](https://chromium.googlesource.com/chromium/src/+/master/tools/gn/README.md)

##运行demo到真机上
>打开`all.xcworkspace`后会看到很多target，其中`AppRTCMobile`就是官方的demo

这里的Identity、Signing不需要修改，也不需要勾选自动签名

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1528347-727afccd40592b22.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选择设备后`command+R`


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1528347-b9c9ee0f11d60387.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**如果没问题的话会直接看到真机上已经安装并运行了WebRTC的demo**



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1528347-3b247571dbcdbcd2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/320)


在两台真机上安装该app，输入同一个Room name之后Start call就能互通了（需要翻墙）

##运行到真机遇到的问题
**可能会遇到以下这些报错**

报错 | 描述
--------- | -------------
![](http://upload-images.jianshu.io/upload_images/1528347-ed368100530b0685.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) |检查一下是否生成的cpu架构不符合设备
![](http://upload-images.jianshu.io/upload_images/1528347-0fcce1f019362792.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)| 证书的签名问题 尝试XCode>Preferences>account 更新provisioning profiles
![](http://upload-images.jianshu.io/upload_images/1528347-e0e30630e75aebcd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|打包出来的app的provisioning文件 和teamid和app的签名不一致  附上[方法](./Webrtc笔记-运行demo 遇到的签名问题.md)



