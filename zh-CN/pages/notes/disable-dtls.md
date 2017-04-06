# 关闭DTLS

在创建Constraints时，增加key键DtlsSrtpKeyAgreement，值为false。

``` javascript
{DtlsSrtpKeyAgreement: "false"}
```
  
## 参考：

* mediaconstraintsinterface.h

* [How to disable DTLS-SRTP feature in webrtc project](https://groups.google.com/forum/#!topic/discuss-webrtc/VvFKSO5oWvk)

* [peerconnectioninterface.h#352](https://chromium.googlesource.com/external/webrtc/+/master/webrtc/api/peerconnectioninterface.h#795)

* [ARDAppClient.m#795](https://chromium.googlesource.com/external/webrtc/+/master/webrtc/examples/objc/AppRTCMobile/ARDAppClient.m#795)

* [How can I disable DTLS/SRTP on iOS?](https://groups.google.com/forum/#!msg/discuss-webrtc/g4wFumYd-_8/RZ8y2dyRAQAJ)