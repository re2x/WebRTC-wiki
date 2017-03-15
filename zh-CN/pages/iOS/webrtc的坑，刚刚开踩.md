#webrtc的坑，刚刚开踩
>此篇是源于自己跟团队小伙伴在运行源码demo时一些稀奇古怪问题的坑，望众基共勉之。由于比较杂，所以主要针对命令行跟踪所遇到的问题出发做笔记。

## mobileprovision-read -f app/embedded.mobileprovision
 该命令行用于查看app对应的provisonning文件是否和签名对应，其中有几个key可供对比使用：
    
```
1、com.apple.developer.team-identifier
2、Name
3、ProvisionedDevices
4、TeamName
5、UUID
```
    1、该命令行中有许多类似key值的string与其相同，用于存储开发者团队的TeamIdentifier
    2、为provisonning文件的文件名
    3、当前项目开发者团队组中授权的设备UUID数组
    4、开发者团队名
    5开发者团队的ID   

▲ 附带贴一个链接（.mobileprovision文件查询工具)
 <https://github.com/0xc010d/mobileprovision-read>
▲ 关于provisonning文件，以'iOS Team'开头的文件，现在只能在xcode中进行管理，developer apple中不显示
▲ 对比两个APP中的embedded.mobileprovision是否一样，使用

```
md5 embedded.mobileprovision
```
    例：MD5 (embedded.mobileprovision) = 91a3dc5a9c4ec0edc035231ced41dbfe
▲附上一个地址/Users/ducky/Library/MobileDevice/Provisioning Profiles

##关于webrtc的源码管理（from大佬）

```
gclient是用来同步代码，在和src同级目录会有一个隐藏.gclient文件，里面记录了基本的代码拉取设置
src里面的各个目录、甚至子目录，基本上都是一个独立的git库
gclient sync的命令回去检查整个项目的完整情况，并同步代码
如果gclient sync无法通过，一般都不是代码的问题，是因为工具链或依赖库和当前代码需要的不一致
gclient是用来同步代码和工具链的
gn 是用来产生ninja所需的配置文件
ninja 才是编译的
代码里面有很多*.gni，可以认为是和make脚本差不多的，是告诉ninja，我要编译某个项目
例如AppRTCMobile，需要哪些代码文件、以来哪些库
```

##git status
该命令行可查看自己的对该git的改动

##gn clean < out_dir>
删除输出目录的内容，除了args.gn和创建一个足以重新生成构建的忍者构建环境。
个人见解：该命令行作用应与xcode中clean操作功能相识，clean后out中目录相关目录会被删除，
但不需要重新gn新的一份，可直接用ninja编译。

##gn args < out_dir> [--list] [--short] [--args]
该命令行其实在生成项目篇有提及到，--list可查看所有参数默认值，若指定为--short则只给出当前设置的参数。
其实根据gn的官方文档，该命令行有很强大检索功能，但这点我一直设置错误，待补充。

##ios_enable_code_signing=false
这是gn项目时的一个设置参数，当没有签名时，设置false可不签名，仍可以编译，但不能部署到真机。
应该是打包出来的AppRTCMobile.app里面没有了_CodeSignature文件夹（来自大佬的推测）

##xcrun security find-identity -v -p codesigning
该命令行可查询当前环境中可用的有效签名。

##ios_code_signing_identity
同是gn项目的设置参数，用于设置证书ID。
▲此处证书为iPhone Developer证书。

##iOS Code Signing
由于项目中自动签名一直出错，需要对其中的.APP文件进行重签名，此处用到iOS Code Signing。
贴上一个相关学习网址<http://www.cocoachina.com/ios/20141017/9949.html>

```
1、codesign -vv -d /Users/linzq/WebRTC/src/out/arm/AppRTCMobile.app 
    该命令行用于查询APP的签名信息
2、$ codesign -f -s 'iPhone Developer: Thomas Kollbach (7TPNXN7G6K)' Example.app
    该命令行可对已签名的APP进行重签名，如果APP未被签名，需用其他命令。上面的网址有详细介绍，此处跳过。
```


##证书匹配的两个点

```
1、( mobileprovision-read -f AppRTCMobile.app/embedded.mobileprovision )
```
![](http://upload-images.jianshu.io/upload_images/1636820-586e30c759d81d0c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
与 (codesign -vv -d AppRTCMobile.app )
```
![](http://upload-images.jianshu.io/upload_images/1636820-45674e12d219a60c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
2、通过命令得到签名的UUID后，增加编译参数（ ios_code_signing_identity="UUID" ）
```
![](http://upload-images.jianshu.io/upload_images/1636820-dff5934262b59480.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
与 ( xcrun security find-identity -v -p codesigning ）  
```
![](http://upload-images.jianshu.io/upload_images/1636820-4490b4d9dca8fc6a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

