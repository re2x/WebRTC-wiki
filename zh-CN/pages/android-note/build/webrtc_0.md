# WebRTC for Android-源码编译篇

## 目录
- 编译环境搭建
- 代码同步
- 编译参数配置
- 源码编译
- 踩过的问题
- 总结

#### 一、编译环境搭建
首先，WebRTC源码编译，官方也说明得很清楚，只能在Linux平台进行编译（笔者在mac os 编译过，后来因为缺少arm架构等编译依赖文件放弃，具体参考[官网](https://webrtc.org/native-code/android/)）,这里笔者使用时ubutun 14.0.1 + vmware。

当然，这整个过程中，翻墙是不可缺少的，具体可以参考笔者的博客[《shadowsocks+proxychains 让ubuntu翻墙飞起来》](http://www.jianshu.com/p/2df128aac7d4)

##### (一) 依赖工具环境配置
depot_tools 是同步代码的时候download下来的，但是笔者这里首先先说下依赖文件的环境配置，如果有需要先下载的，请跳转[depot_tools工具](http://pan.baidu.com/s/1slxxT85)
** 具体配置 **
1、添加环境变量
`````
    sudo gedit /etc/profile # 针对每个用户

    # 在最后添加下面语句
    export PATH=$PATH:/home/siven/siven/softSetup/depot_tools # depot_tools路径

`````
2、激活生效
`````
    source /etc/profile #重启全局生效，否则只针对该bash有效
`````
3、验证
`````
    which gn # 如果正常打印出gn命令路径，说明成功
`````

#### 二、代码同步
1、同步准备
`````
   mkdir webrtc 
   cd webrtc
`````
2、代码下载
`````
    #　选择android 分支版本
    fetch --nohooks webrtc_android
    #　同步代码
    gclient sync
`````

等待... 源码、第三方库、依赖文件鱼等大概16G左右（虚拟机记得配置存储大于20G喔，默认是20G）

3、依赖环境配置
`````
    # 当代码下载完整
    cd src
    ./build/install-build-deps.sh
`````
这里环境配置也许会遇到以下问题：
** Automatic java installation filed **

![webrtc-0.png](http://upload-images.jianshu.io/upload_images/2516602-407a8f765258d993.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里直面意思是在配置jdk环境的时候报错。由于ubuntu会自带openjdk 并且版本是1.7.这里webrtc代码打包的jdk环境是java8（控制台信息也建议jdk 8shifou beianz ），所以这里解决方法是卸载原来的openjdk，自己手动安装jdk,而且版本要求是1.8，这里如何配置jdk，笔者就不说明了~

#### 三、编译参数配置
1、参数配置
`````
gn gen out/Debug --args='target_os="android" target_cpu="arm"'
`````

编译不同平台：
To build for ARM64: use target_cpu="arm64"
To build for 32-bit x86: use target_cpu="x86"
To build for 64-bit x64: use target_cpu="x64"

#### 四、源码编译
##### (一) 全编译
全编译指的是所有源代码的编译，编译文件会稍多，并且编译时间会稍长
`````
ninja -C out/myWebRTC # myWebRTC 指的是编译输出文件夹
`````

##### (二) 编译Android studio 项目
1、项目编译
`````
ninja -C out/myWebRTC　AppRTCMobile
`````

2、gradle 构建文件生成
Android studio 项目是依赖gradle进行构建编译的，在上面步骤编译出来的项目并没有gradle依赖文件，因此还需要进行编译生成
`````
build/android/gradle/generate_gradle.py --output-directory $PWD/out/myWebRTC \
--target "//webrtc/examples:AppRTCMobile" --use-gradle-process-resources \
--split-projects
`````

执行结束后就会在当前 ** out/myWebRTC **目录下出现 gradle文件夹，即工程正在的目录

3、open the project
直接启用Android studio，import 当前gradle生成目录，即 out/myWebRTC/gradle

#### 五、踩过的问题
（一）环境配置踩的坑
** Automatic java installation filed **
上面环境配置已经说明解决方式

（二）工程项目踩的坑
1、编译报错
第一次Import项目的时候，会出现R文件编译报错，如下图所示

![webrtc-1.png](http://upload-images.jianshu.io/upload_images/2516602-68b461810c326dd3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

** 原因分析 **：
    对于java，命名类名、包名、或者是变量名，我们都不可以以系统关键词命名，例如int、interface、class、package等。这里R文件报错地方指向package,所以问题应该定位在Project生成R文件的时候，由于工程包名使用了package关键字，所以编译会出现报错。

** 解决 **：
    R文件包名生成跟AndroidManifest.xml有关，因此只需要修改src/build/android/AndroidManifest.xml 下的package name 即可，如下图所示：

![webrtc-2.png](http://upload-images.jianshu.io/upload_images/2516602-d9e2167a004a86b3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、运行报错
由于读者用的是小米手机,由于系统自带的miui优化，运行安装apk的时候会出下以下报错，如图所示：

![webrtc-3.png](http://upload-images.jianshu.io/upload_images/2516602-8272600210352f87.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

** 解决 **：
    只需要在IDE设置instant Run，取消Enable Instant Run即可，如图所示：

![webrtc-4.png](http://upload-images.jianshu.io/upload_images/2516602-36fe5d1c7b387c0b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 六、总结
翻墙，翻墙。没有这个前提什么都干不了~源码编译时体力活，如果编译成功后就总结下，对自己是很有帮助的，笔者就踩过很多坑~总之，多记笔记，多总结吧！

好了，下面开启webrtc之旅吧~


![webrtc-5.png](http://upload-images.jianshu.io/upload_images/2516602-bb819a700a67cc8b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

BY SIVEN - HORI-GZ
2017.4.18


