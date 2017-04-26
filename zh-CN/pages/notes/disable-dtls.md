# 关闭DTLS

在创建Constraints时，增加key键DtlsSrtpKeyAgreement，值为false。

``` javascript
{DtlsSrtpKeyAgreement: "false"}
```

关闭 SDES

修改`webrtcsessiondescriptionfactory.cc:134`的SetSdesPolicy方法参数为`cricket::SEC_DISABLED`，如下

``` cpp
  SetSdesPolicy(dtls_enabled ? cricket::SEC_DISABLED : cricket::SEC_REQUIRED);
```

改为

``` cpp
  SetSdesPolicy(cricket::SEC_DISABLED);
```

## 参考：

* mediaconstraintsinterface.h

* [How to disable DTLS-SRTP feature in webrtc project](https://groups.google.com/forum/#!topic/discuss-webrtc/VvFKSO5oWvk)

* [peerconnectioninterface.h#352](https://chromium.googlesource.com/external/webrtc/+/master/webrtc/api/peerconnectioninterface.h#795)

* [ARDAppClient.m#795](https://chromium.googlesource.com/external/webrtc/+/master/webrtc/examples/objc/AppRTCMobile/ARDAppClient.m#795)

* [How can I disable DTLS/SRTP on iOS?](https://groups.google.com/forum/#!msg/discuss-webrtc/g4wFumYd-_8/RZ8y2dyRAQAJ)