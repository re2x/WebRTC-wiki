# WebRTC支持Andrroid H264编解码

** 最后更新：2017-05-05  **  by [linzq](mailto:rex@re2x.com) 

## gn 命令参数

``` shell
gn gen out/debug -args='target_os="android" target_cpu="arm" is_component_build=false proprietary_codecs=true rtc_use_h264=true use_openh264=true ffmpeg_branding="Chrome"'
```

** 注意参数顺序，顺序不对可能会产生错误 **

# 实现其他平台的H264硬编硬解

具体修改请看 [本次提交](http://192.168.51.107/commit/webrtc/24240686795944ea980c60d9140c8d1742f532cf)

## 修改ffpmeg，实现h264软解码

直接对 `third_party/ffmpeg` 应用 [ffmpeg_patch.diff](http://192.168.51.107/blob/webrtc/f2cb08299ac29c60c6a7fdce2908673b6f6d2bef/patch%2Fffmpeg_patch.diff)
``` shell
cd third_party/ffmpeg
git apply ../../ffmpeg_patch.diff
```
## x86架构问题补充

如果你的`target_cpu="x86"`的话，编译的时候会出现
```
warning: shared library text segment is not shareable
```
x86架构出现此问题原因暂时不得而知，但是可以通过修改编译参数忽略这个warning。
默认是把此warning视为error，故会出现编译失败

可对`build`目录应用 [fix_x86_h264_build_error_patch.diff](http://192.168.51.107/blob/webrtc/f2cb08299ac29c60c6a7fdce2908673b6f6d2bef/patch%2Ffix_x86_h264_build_error_patch.diff),
通过增加 `"-Wl,--warn-shared-textrel"` 参数来忽略这个warning
