# Webrtc笔记-获取源码

##1、开发者环境
>**Webrtc源码支持直接在编译器上调试了，以macOS环境为例**


##2、获取webrtc源码（翻墙）
>PS：由于国情，下载代码过程，包括gclient sync
同步代码都必须翻墙！推荐使用Shadowsocks+Proxifier进行全局代理：[方法](http://www.jianshu.com/p/754863f67022)

**( 1 )、获取depot_tools工具**

```
$ git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
```

**( 2 )、添加depot_tool路径到全局环境变量里面**

* `vi ~/.bash_profile` 编辑全局环境配置文件（我的是在根目录,如果没有，在根目录下创建一个吧）

* 把depot_tool的路径添进去：`PATH="${PATH}:depot_tool的绝对路径"`

* 然后执行 `. ~/.bash_profile`  就生效了

**( 3 )、下载源码**

* 新建文件夹，然后cd到这个文件目录下

* `fetch --nohooks webrtc_ios`获取源码

* 然后是漫长的等待，如图，≈6G 

![](http://upload-images.jianshu.io/upload_images/1528347-3f79007d2276e7db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



##3、创建分支
* 建议创建一个新的本地分支
`git new-branch <branch name>`

* git管理建议使用[SourceTree](https://www.sourcetreeapp.com/)

* 把下载好的src拖进[SourceTree](https://www.sourcetreeapp.com/)可以详细看版本的更新日志

![](http://upload-images.jianshu.io/upload_images/1528347-02e8bad86a47d498.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##4、源码结构
～漫长的代码下载后能会获取到一个src文件目录：如下
```
├── base
├── build  编译相关脚本
├── build_overrides  覆盖编译参数
├── buildtools
├── data  测试数据
├── infra
├── resources 需要的资源文件
├── testing
├── third_party 用到的第三方库ffmpeg/openh264...代码，5G左右
├── tools
├── tools-webrtc 一些编译脚本
└── webrtc 核心代码
```
核心代码目录src/webrtc
```
├── api
├── audio
├── base
├── call
├── common_audio
├── common_video
│   ├── h264
│   ├── include
│   └── libyuv
├── examples
│   ├── androidapp Android版AppRTCMbile
│   ├── androidjunit
│   ├── androidtests
│   ├── objc  iOS版AppRTCMbile
│   ├── peerconnection
│   ├── relayserver
│   ├── stunprober
│   ├── stunserver
│   └── turnserver
├── logging
├── media
├── modules
│   ├── audio_coding
│   ├── audio_conference_mixer
│   ├── audio_device
│   ├── audio_mixer
│   ├── audio_processing
│   ├── bitrate_controller
│   ├── congestion_controller
│   ├── desktop_capture
│   ├── include
│   ├── media_file
│   ├── pacing
│   ├── remote_bitrate_estimator
│   ├── rtp_rtcp
│   ├── utility
│   ├── video_capture
│   ├── video_coding
│   └── video_processing
├── p2p
│   ├── base
│   ├── client
│   ├── quic
│   └── stunprober
├── pc
├── sdk
│   ├── android  Android sdk封转
│   └── objc iOS sdk封转
├── stats
├── system_wrappers
│   ├── include
│   └── source
├── test
├── tools
├── video
└── voice_engine
```




