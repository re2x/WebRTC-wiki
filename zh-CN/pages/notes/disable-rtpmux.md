# 关闭音视频混合RTP

在PeerConnection CreateOffer和CreateAnswer方法，对参数中的Constraints对象，增加key键googUseRtpMUX，值为false。

``` javascript
{googUseRtpMUX: "false"}
```

## 参考

* [peerconnection.cc#853](https://chromium.googlesource.com/external/webrtc/+/master/webrtc/pc/peerconnection.cc#853)
