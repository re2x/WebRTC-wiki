# 关闭音视频混合RTP

在创建Constraints时，增加key键googUseRtpMUX，值为false。

``` javascript
{googUseRtpMUX: "false"}
```

## 参考

* [peerconnection.cc#853](https://chromium.googlesource.com/external/webrtc/+/master/webrtc/pc/peerconnection.cc#853)
