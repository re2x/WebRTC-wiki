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

直接对 `third_party/ffmpeg` 应用 [ffmpeg_patch.diff](http://192.168.51.107/commit/webrtc/93cae127a2872d4b85f287f7ccddd7b6e4518cf2)
``` shell
cd third_party/ffmpeg
git apply ../../ffmpeg_patch.diff
```