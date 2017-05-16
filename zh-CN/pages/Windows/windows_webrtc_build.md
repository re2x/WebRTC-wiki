# Windows WebRTC 源码下载、编译

** 最后更新：2017-5-10 ** by linzq

## 准备工作

系统要求：

* **Windows 7** 或以上，64位系统
* **100G 空间，必须是NTFS**格式，fat32不支持
* 最好 **8G 或以上内存**

工具要求：

* 按[Install depot_tools](http://dev.chromium.org/developers/how-tos/install-depot-tools)的步骤，安装好depot\_tools及vc发行包，并把depot\_tools目录加到PATH。

    关于depot_tools更多的信息，查看[Using depot_tools](http://dev.chromium.org/developers/how-tos/depottools)

* 安装 Visual Studio 2015 Update 3 或更高的版本

* 安装 Windows 10 SDK (10.0.14393)

* <del>修改 `控制面板` -> `区域和语言` -> `管理` -> `更改系统区域设置` -> `英语(美国)` ，确定后重启。</del>  **新版本无需修改**

更多内容，请看[Checking out and Building Chromium for Windows](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md#System-requirements)

## 源码下载

``` shell
mkdir webrtc-checkout
cd webrtc-checkout
fetch --nohooks webrtc
gclient sync

```

以上步骤需要翻墙，特别是fetch步骤要一次性顺利执行完，不然最好删除`webrtc-checkout`目录，然后重新执行，不然会出现很多错误。

完成以上命令后，可按[WebRTC Android 代码获取/检查命令顺序](../Android/android_webrtc_code_checkout_and_checkout_cmd.md) ，检查下载的代码是否完整

## 编译

``` shell
gn gen out/win_x86 -args="target_cpu=\"x86\" proprietary_codecs=true rtc_use_h264=true use_openh264=true ffmpeg_branding=\"Chrome\"" --ide=vs
```

命令执行完后，打开 `out/win_x86/all.sln` 即可

更多请查看[windows产生vs2015编译配置](../cmd.md#windows产生vs2015编译配置)

## 调试开发

### 修改git地址

根据 [基于Release分支的开发](../develop_with_release.md) 修改git服务器地址

### 项目介绍

请参考 [WebRTC 入门介绍](../intro.md#Demo演示)

** 待续 **