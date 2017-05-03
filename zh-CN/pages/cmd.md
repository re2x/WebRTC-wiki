# WebRTC 常用命令


## iOS产生xcode项目

``` shell
gn gen out/ios64 -args="target_os=\"ios\" target_cpu=\"x64\" is_component_build=false proprietary_codecs=true ios_enable_code_signing=false" --ide=xcode
```
xcode项目文件位置 out/ios64/all.workspace ，项目包含多个TARGETS,其中AppRTCMobile是apprtc的ios版本

**iOS真机调试必须ios_enable_code_signing=true，target_cpu=arm或target_cpu=arm64。<br />
如果存在多个签名要增加参数ios_code_signing_identity=[UUID]，具体这个UUID请 [查看iOS本地签名uuid](#查看iOS本地签名uuid)**


## iOS FAT库打包（57后面的版本才可用）

``` shell
./tools-webrtc/ios/build_ios_libs.sh --extra-gn-args='proprietary_codecs=true'
```

默认生成根目录下 out_ios_libs/WebRTC.framework 文件


## windows产生vs2015编译配置

``` shell
gn gen out/win_x64 -args="target_cpu=\"x64\" proprietary_codecs=true rtc_use_h264=true use_openh264=true ffmpeg_branding=\"Chrome\"" --ide=vs
gn gen out/win_x86 -args="target_cpu=\"x86\" proprietary_codecs=true rtc_use_h264=true use_openh264=true ffmpeg_branding=\"Chrome\"" --ide=vs
```

vs项目文件位置 out/win_x64/all.sln


## Android产生编译配置

``` shell
gn gen out/debug -args='target_os="android" target_cpu="arm" is_component_build=false proprietary_codecs=true rtc_use_h264=true use_openh264=true ffmpeg_branding="Chrome"'
```
**参数说明**
* proprietary_codecs： 启用ffmpeg进行h264 decode

* rtc_use_h264： 启用ffmpeg进行h264 decode

* proprietary_codecs： 启用ffmpeg进行h264 decode


<!-- more -->


## Android aar打包（57后面的版本才可用）

``` shell
./tools-webrtc/android/build_aar.py --verbose --extra-gn-args='is_component_build=false proprietary_codecs=true rtc_use_h264=true use_openh264=true ffmpeg_branding="Chrome"'
```

默认生成根目录下 libwebrtc.aar 文件


## Android产生Android Studio编译配置

**`2017-02-23` 更新**  
最新的WebRTC项目已经支持生成AS项目，链接 [PSA: AppRTCMobile can now be developed in Android Studio](https://groups.google.com/forum/#!topic/discuss-webrtc/b7yQjvPLHaM)  ，具体做法如下：

执行如下命令：

```shell
build/android/gradle/generate_gradle.py --output-directory $PWD/out/Debug --target "//webrtc/examples:AppRTCMobile" --use-gradle-process-resources
```

生成的项目目录`out/Debug/gradle`，需要Android Studio 2.2及以上版本

具体编译方法查看 [android_studio.md](https://chromium.googlesource.com/chromium/src.git/+/master/docs/android_studio.md)

**如下内容已被废弃↓**

---

由于WebRTC本身没有可以产生AS项目的脚本，我们需要自己手动生成  
1.先执行 `Android aar打包` 命令，产生 `libwebrtc.aar`  
2.新建一个 `Android Studio Project`  
3.在新建的 `Projec`  新建一个 `Module` ，选择 `Import .JAR/.AAR Package` ，选择 `步骤1` 生成的 `libwebrtc.aar` ，Subproject Name设为`libwebrtc`  
4.在 `app` Module的 `build.gradle` 的 `dependencies` 节点增加 `compile project(":libwebrtc")`  
5.把 `webrtc/examples/androidapp` 目录下的 `res` 、 `src` 文件夹及 `AndroidMainifest` 文件复制到 `app` Module，还有 `webrtc/examples/androidapp/third_party/autobanh/lib/autobanh.jar` 复制到 `app` Module的 `libs` 目录

---


## 编译

``` shell
ninja -C out/debug
```


## 查看编译配置参数

``` shell
gn args out/debug --list
```


## 查看可用的target

``` shell
gn ls out/debug
```


## 查看iOS本地签名uuid
``` shell
xcrun security find-identity -v -p codesigning
```


## 查看iOS app签名信息
``` shell
codesign -vv -d AppRTCMobile.app 
```


## 查看iOS app provisoning信息
``` shell
mobileprovision-read -f AppRTCMobile.app/embedded.mobileprovision
```


## 获取特定版本

* 获取正式的版本：使用 `gclient sync` 下载完全部代码后，在代码目录下用 `git branch -a` 查看所有版本，并用`git checkout remotes/branch-heads/57` 获取57这个版本。
* 获取某次commit的版本：使用 `gclient sync -r src@037ee92455a6e7fc01efb4397a6cdcf8e49389b4` 获取037ee92455a6e7fc01efb4397a6cdcf8e49389b4这个commit id的版本。


## 参考

* [WebRTC Development](https://webrtc.org/native-code/development/)
* [Building a Fat WebRTC framework on iOS](https://medium.com/@atsakiridis/building-a-fat-webrtc-framework-on-ios-8610fffb2224#.v7zqct8v9)
* [Checking out and Building Chromium for Windows](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md)
* [查看chromium项目用的WebRTC版本](https://chromium.googlesource.com/chromium/src/+/master/DEPS#234)
