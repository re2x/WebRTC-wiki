# 什么是WebRTCWebRTC 入门介绍


## 什么是WebRTC


### 背景


#### 如何实现一个点对点的音视频通话

* Native 实现  
摄像头访问、编解码、降噪、信令、数据通讯、渲染、优化

* web 实现  
Flash

#### WebRTC是什么

> 网页即时通信（英语：Web Real-Time Communication）的缩写，是一个支持网页浏览器进行**实时语音对话或视频对话的API**。它于2011年6月1日开源并在Google、Mozilla、Opera支持下被纳入万维网联盟的**W3C推荐标准**。

#### 我们说的WebRTC是

> 2010年5月，Google以6820万美元收购VoIP软件开发商Global IP Solutions的**GIPS引擎**，并改为名为“**WebRTC**”。WebRTC使用GIPS引擎，实现了基于网页的视频会议，并支持722，PCM，ILBC，ISAC等编码，同时使用谷歌自家的VP8视频解码器；同时支持RTP/SRTP传输等。


### 现状


#### 支持
[检测是否支持](https://test.webrtc.org/)

![API框架图](uploads/images/webrtc_support.png)

<!-- more -->
  
  
## WebRTC流程

* API框架图

![API框架图](uploads/images/WebRTCNativeAPIsDocument.png)

* 发起序列图

![发起序列图](uploads/images/WebRTCNativeAPIsDocument%20%281%29.png)

* 接受序列图

![接受序列图](uploads/images/WebRTCNativeAPIsDocument%20%282%29.png)

* 结束序列图

![结束序列图](uploads/images/WebRTCNativeAPIsDocument%20%283%29.png)

### 说说ICE

[互动式连接建立](https://zh.wikipedia.org/wiki/%E4%BA%92%E5%8B%95%E5%BC%8F%E9%80%A3%E6%8E%A5%E5%BB%BA%E7%AB%8B)

> 互动式连接建立是由IETF的MMUSIC工作组开发出来的一种framework，可整合各种NAT穿透技术，如STUN、TURN（Traversal Using Relay NAT，中继NAT实现的穿透）、RSIP（Realm Specific IP，特定域IP）等。该framework可以让SIP的客户端利用各种NAT穿透方式打穿远程的防火墙。


#### [NAT](https://zh.wikipedia.org/wiki/%E7%BD%91%E7%BB%9C%E5%9C%B0%E5%9D%80%E8%BD%AC%E6%8D%A2) 网络地址转换

> 也叫做网络掩蔽或者IP掩蔽（IP masquerading），是一种在IP数据包通过路由器或防火墙时重写来源IP地址或目的IP地址的技术。这种技术被普遍使用在有多台主机但只通过一个公有IP地址访问因特网的私有网络中。根据规范，路由器是不能这样工作的，但它的确是一个方便并得到了广泛应用的技术。当然，NAT也让主机之间的通信变得复杂，导致通信效率的降低。

* 利用 [pystun](https://pypi.python.org/pypi/pystun) 检测NAT类型

* Full cone NAT 全锥型NAT  

**发过，谁都随便发**

> 一旦一个内部地址（iAddr:port1）映射到外部地址（eAddr:port2），所有发自iAddr:port1的包都经由eAddr:port2向外发送。**任意外部主机都能通过给eAddr:port2发包到达iAddr:port1**

* Address-Restricted cone NAT 限制锥型NAT  

**发过给你，你随便发**

> 一旦一个内部地址（iAddr:port1）映射到外部地址（eAddr:port2），所有发自iAddr:port1的包都经由eAddr:port2向外发送。**任意外部主机（hostAddr:any）都能通过给eAddr:port2发包到达iAddr:port1的前提是：iAddr:port1之前发送过包到hostAddr:any. "any"也就是说端口不受限制**

* Port-Restricted cone NAT 端口限制锥型NAT 

**发过你这个端口，这个端口随便发**

> 一旦一个内部地址（iAddr:port1）映射到外部地址（eAddr:port2），所有发自iAddr:port1的包都经由eAddr:port2向外发送。**一个外部主机（hostAddr:port3）能够发包到达iAddr:port1的前提是：iAddr:port1之前发送过包到hostAddr:port3**

* Symmetric NAT 对称型NAT **一一对应**

> 每一个来自相同内部IP与端口，到一个特定目的地地址和端口的请求，都映射到一个独特的外部IP地址和端口。  
**同一内部IP与端口发到不同的目的地和端口的信息包，都使用不同的映射**

* 提问  

问题1: A是全锥型NAT（手机），B是端口限制锥型NAT，A、B2个人有直接通讯的可能吗？  
问题2: C是全锥型NAT，D是对称型NAT(公司)，C、D2个人有直接通讯的可能吗？


#### [STUN](https://zh.wikipedia.org/wiki/STUN) 穿透

> STUN（Session Traversal Utilities for NAT，NAT会话穿越应用程序）是一种网络协议，它允许位于NAT（或多重NAT）后的客户端找出自己的公网地址，查出自己位于哪种类型的NAT之后以及NAT为某一个本地端口所绑定的Internet端端口。这些信息被用来在两个同时处于NAT路由器之后的主机之间建立UDP通信。该协议由RFC 5389定义。

STUN 算法过程  

![STUN过程](uploads/images/stun-progress.png)


#### [TURN](https://zh.wikipedia.org/wiki/TURN) 中转
> TURN是一个client-server协议。TURN的NAT穿透方法与STUN类似，都是通过取得应用层中的公有地址达到NAT穿透。但实现TURN client的终端必须在通讯开始前与TURN server进行交互，并要求TURN server产生"relay port"，也就是relayed-transport-address。这时TURN server会建立peer，即远端端点（remote endpoints），开始进行中继（relay）的动作，TURN client利用relay port将资料传送至peer，再由peer转传到另一方的TURN client。

WebRTC源码中包含了STUN、TURN的客户端和服务端实现

## 我们的目标

* 实现基于WebRTC的，符合“联享家”和“景医卫”需求的，实时音视频框架。
* 为什么用WebRTC，因为WebRTC实现很多我们需要的功能：音视频编解码，传输，复用等等，以及它的可靠性高，稳定性好，支持多个平台。


### 我们要做什么

* 首先，能够实现需求所需要的功能（**废话**）
* 一套完整的音视频流程、机制，包括服务端
	* 独立（不和具体需求耦合）
	* 复用（能够重复用于多个项目）
	* 统一（所有平台尽量统一代码、API，减少维护学习成本）
	* 稳定
* 基于一个稳定的WebRTC版本进行开发，持续更新合并新的代码
* 实现DTMF收发，H264编解码
* 完善的文档、学习资料
* 小团队维护，持续迭代

### 第一阶段目标

* 各个开发人员学习WebRTC开发编译
* 完善所有平台H264支持
* 部署自己的ICE服务器
* 编码实现各个平台的demo app，实现各个平台相互通讯
* 输出相关笔记、文档等资料


## 代码分析


### 项目结构
根目录 src/

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

核心代码目录 src/webrtc

```
├── api
├── audio
├── base
├── call
├── common_audio
├── common_video
│   ├── h264
│   ├── include
│   └── libyuv
├── examples
│   ├── androidapp Android版AppRTCMbile
│   ├── androidjunit
│   ├── androidtests
│   ├── objc  iOS版AppRTCMbile
│   ├── peerconnection
│   ├── relayserver
│   ├── stunprober
│   ├── stunserver
│   └── turnserver
├── logging
├── media
├── modules
│   ├── audio_coding
│   ├── audio_conference_mixer
│   ├── audio_device
│   ├── audio_mixer
│   ├── audio_processing
│   ├── bitrate_controller
│   ├── congestion_controller
│   ├── desktop_capture
│   ├── include
│   ├── media_file
│   ├── pacing
│   ├── remote_bitrate_estimator
│   ├── rtp_rtcp
│   ├── utility
│   ├── video_capture
│   ├── video_coding
│   └── video_processing
├── p2p
│   ├── base
│   ├── client
│   ├── quic
│   └── stunprober
├── pc
├── sdk
│   ├── android  Android sdk封转
│   └── objc iOS sdk封转
├── stats
├── system_wrappers
│   ├── include
│   └── source
├── test
├── tools
├── video
└── voice_engine
```

### 编译

* gn 生成编译脚本等
* ninja 编译工具
* 不论是 Xcode 还是 visual studio 最终还是调用了ninja进行编译
* 增加、删除代码需要修改对应的 BUILD.gn 文件
* 参考chromium的编译	
	* [iOS 编译](https://chromium.googlesource.com/chromium/src/+/master/docs/ios/build_instructions.md#Setting-up-the-build)
	* [Android 编译](https://chromium.googlesource.com/chromium/src/+/master/docs/android_build_instructions.md)
	* [Windows 编译](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md)

具体查看 [WebRTC 编译笔记](/blog/webrtc-build-cmd/)


### 怎么看代码

* [Android Studio](https://chromium.googlesource.com/chromium/src/+/master/docs/android_studio.md) + [AppRTCMobile can now be developed in Android Studio](https://groups.google.com/forum/#!topic/discuss-webrtc/b7yQjvPLHaM) + [Visual Studi Code](https://code.visualstudio.com/)
* Xcode 加编译参数 --ide=xcode
* Visual Studio 加编译参数 --ide=vs


### 代码管理

* 同步代码 `gclient sync`
* 同步特点版本代码 `gclient sync -r git_commit_revision`
* 创建分支 `git checkout -b linzq/dev/57 remotes/branch-heads/57`
* 代码、分支管理建议用sourcetree + gitFlow流程
* 翻墙建议用 shadowsocks，然后 proxifier 或者 proxyCap 来做全局代理
* [webrtc CI日志](https://build.chromium.org/p/client.webrtc/waterfall) 当你不确定checkout的版本是否有问题的时候，看看CI日志的编译情况，可以减少很多查错的时间
* [webrtc forum](https://groups.google.com/forum/#!forum/discuss-webrtc) 版本更新、有疑问这里找
* [git 介绍](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001373962845513aefd77a99f4145f0a2c7a7ca057e7570000)


### 典型模块

* API [Java libjingle_peerconnection_java](https://chromium.googlesource.com/external/webrtc/+/master/webrtc/sdk/android/) **[objc rtc_sdk_framework](https://chromium.googlesource.com/external/webrtc/+/master/webrtc/sdk/objc/)**  **[C++ libjingle_peerconnection](https://chromium.googlesource.com/external/webrtc/+/master/webrtc/pc/)**

* [H264](https://chromium.googlesource.com/external/webrtc/+/master/webrtc/modules/video_coding/codecs/h264/)

### Demo演示

* Android **[aar包](https://chromium.googlesource.com/external/webrtc/+/master/tools-webrtc/android/build_aar.py)**  **[AppRTCMobile](https://chromium.googlesource.com/external/webrtc/+/master/webrtc/examples/androidapp/)**

* iOS **[FAT库](https://chromium.googlesource.com/external/webrtc/+/master/tools-webrtc/ios/build_ios_libs.py)**、**[AppRTCMobile](https://chromium.googlesource.com/external/webrtc/+/master/webrtc/examples/objc/AppRTCMobile/)**

* Windows **[videoloopback](https://chromium.googlesource.com/external/webrtc/+/master/webrtc/video/video_loopback.cc)**

> 遇到有坑的地方，记得做好笔记，**避免自己再次踩坑、后人受益**

### H264与WebRTC的历史

* 15年左右的版本是不只是H264的编解码的
* 16年增加了openh264实现编码，沿用原来的ffmpeg代码来实现解码
* 最新的是:
	* 非android/iOS已经实现了软编解码(openh264+ffmepg)
	* iOS 使用了硬编解码
	* Android针对高通、三星平台使用硬编解码，其他不支持

## 初步计划

1. 3月份，所有成员能够**编译、调试代码**
2. 4月份，初步完成各个平台的**Demo APP，ICE服务器部署**
3. 5月份，初步 完成可以与联享家APP、门口机APP**联调的版本**
4. 6月份，完成第一个**基本的稳定版本**（各个平台）
5. 7月份，**通用的稳定版本**


## 怎么做

* Just Do it
* 把 [webrtc.org](http://www.webrtc.org) 上面的内容都仔细看看，会有很多收获的


## 讨论

1. 说说看你们的个人理解
2. 有什么疑问


## 参考

* [wiki - WebRTC](https://zh.wikipedia.org/wiki/WebRTC)  
* [Chromium docs](https://chromium.googlesource.com/chromium/src/+/master/docs/)