#Webrtc笔记-运行demo 遇到的签名问题
>WebRTC的demo安装时候需要确保打包出来的app的provisioning文件的teamid和app的签名一致。

##1、获取Provisioning Profile的UUID,查看TeamIdentifier
```sh
mobileprovision-read -f AppRTCMobile.app/embedded.mobileprovision
```

![](http://upload-images.jianshu.io/upload_images/1528347-afbcf4b9a41be6a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



##2、查看.app的签名，检查teamIdentifier是否与上面的一致
```sh
codesign -vv -d AppRTCMobile.app
```

![](http://upload-images.jianshu.io/upload_images/1528347-e25fdc881b8fe633.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


>如果不一致，尝试手动给.app签名,方法如下,该命令行可对已签名的APP进行重签名，如果APP未被签名，需用其他命令，具体可参考[这篇文章](http://www.cocoachina.com/ios/20141017/9949.html)）

```
codesign -f -s 'iPhone Developer: Thomas Kollbach (7TPNXN7G6K)' Example.app
```
    

##3、设置一个有效的签名身份
>如果想部署webrtc的demo到一个IOS设备上，你必须设置一个有效的签名身份

* 通过运行该的命令进行验证签名身份：
`xcrun security find-identity -v -p codesigning`

* 要保证你的设备已经添加到这个team下了，通过以上命令得到签名的UUID后，`gn args out/ios_32 -shot`增加编译参数`ios_code_signing_identity="UUID"`如下图，保存并退出编辑

![](http://upload-images.jianshu.io/upload_images/1528347-5b27e901fbc8fa34.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



##4、重新运行到真机
Done之后，回到xcode，clean一下，重新运行到真机

![](http://upload-images.jianshu.io/upload_images/1528347-c460acab6f4567b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



-----
**更多详情可参考：[这一篇](./webrtc的坑，刚刚开踩.md)**

