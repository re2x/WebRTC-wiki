# 关闭DTLS

在创建Constraints时，增加key键DtlsSrtpKeyAgreement，值为false。

``` javascript
{DtlsSrtpKeyAgreement: "false"}
```
  
参考：

* [How to disable DTLS-SRTP feature in webrtc project](https://groups.google.com/forum/#!topic/discuss-webrtc/VvFKSO5oWvk)

* [ARDAppClient.m#795](https://chromium.googlesource.com/external/webrtc/+/master/webrtc/examples/objc/AppRTCMobile/ARDAppClient.m#795)