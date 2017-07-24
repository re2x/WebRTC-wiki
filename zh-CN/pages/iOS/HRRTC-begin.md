# iOS WebRTC.framwwork开发、使用入门

## 准备工作

1. 按照[基于Release分支的开发](../develop_with_release.md)的步骤，checkout 完整的branch_59分支代码及其相关的工具链
2. 切换到iOS分支，`git checkout branch_59_iOS`，具体版本日志看[这里](http://192.168.51.107/log/webrtc%2F.git/branch_59_iOS)

## 编译调试

1. 参考[常用命令](../cmd.md)，生成项目编译配置文件
```
gn gen out/ios64 -args="target_os=\"ios\" target_cpu=\"arm64\" is_component_build=false ios_enable_code_signing=false" --ide=xcode
```
2. 打开生成的`out/ios64/products.xcodeproj`，项目中的target：`rtc_sdk_framework_objc`即
`WebRTC.framework`的配置

## API

`WebRTC.framework`中的最重要的是`WebrtcEngine`、`HRMediaParameter`类和`WebrtcDelegate`协议

`WebrtcEngine`：入口，主要方法`initWithDelegate`,`startEngine`,`stopEngine`

`HRMediaParameter`：启动参数配置

`WebrtcDelegate`：通知连接成功的回调,`onIceConnected`连接成功

``` objc
@interface WebrtcEngine : NSObject

@property(nonatomic, readonly) HRMediaParameter *mediaParameter;
@property(nonatomic, strong) RTCCameraPreviewView *localVideoView;
@property(nonatomic, strong) __kindof UIView<RTCVideoRenderer> *remoteVideoView;
@property(nonatomic, assign) BOOL shouldStats;

- (instancetype)initWithDelegate:(id<WebrtcDelegate>) delegate;
- (void)startEngine:(HRMediaParameter *)mediaParameter;
- (void)stopEngine;
- (void)switchCamera;
- (void)pauseVideo;
- (void)resumeVideo;

@end
```

``` objc
@protocol WebrtcDelegate <NSObject>

@required
- (void)onIceConnected;

@optional
- (void)onGetStats:(NSArray *)stats;

@end
```

## demo

项目中的target：`HRRTCMobile` 是针对WebrtcEngine调用的demo

## 使用WebRTC.framework

1. 使用[iOS FAT库打包](../cmd.md)的脚本生成FAT库  
`./tools-webrtc/ios/build_ios_libs.sh --extra-gn-args='proprietary_codecs=true'`
2. 参考demo的代码和Framework的调用实现功能

如果编译 FAT库是出现如下错误，这是因为Xcode 8.3的问题，需要修改`third_party/usrsctp/usrsctplib`库的代码。相关代码修改的patch在`patch/fix_usrsctplib_ios_fat_build_error.diff`，可直接`cd third_party/usrsctp/usrsctplib`，然后执行`git apply ../../../patch/fix_usrsctplib_ios_fat_build_error.diff`修改代码。

``` shell
../../third_party/usrsctp/usrsctplib/usrsctplib/netinet/sctp_output.c:6020:30: error: taking address of packed member 'time_entered' of class or structure 'sctp_state_cookie' may result in an unaligned pointer value [-Werror,-Waddress-of-packed-member]
        (void)SCTP_GETTIME_TIMEVAL(&stc.time_entered);
                                    ^~~~~~~~~~~~~~~~
../../third_party/usrsctp/usrsctplib/usrsctplib/netinet/sctp_constants.h:1028:46: note: expanded from macro 'SCTP_GETTIME_TIMEVAL'
#define SCTP_GETTIME_TIMEVAL(x) gettimeofday(x, NULL)
                                             ^
1 error generated.
```


## 发布到私有cocoapods仓库 HoriSpecs

1、签出HRWebRTC 二进制文件库，`git clone http://192.168.51.107/r/iOS/Cocoapods/WebRTC.git`
2、使用编译framework编译命令，、编译并生成 `WebRTC.framework`
3、复制`WebRTC.framework`到clone后的库目录，并覆盖原有的
4、编辑文件`HRWebRTC.podspec`，修改`s.version`的参数为最新的版本号，例如`0.2.0`
5、打tag `git tag [版本号]`
6、`git push --tag`
7、发布版本到私有仓库HoriSpecs `pod repo push HoriSpecs HRWebRTC.podspec`