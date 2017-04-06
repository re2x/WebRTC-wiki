# 数据只发不收

Constraint设置 `offerToReceiveVideo: false`，会在产生的SDP中增加 `a=sendonly`。

## 参考：

* mediaconstraintsinterface.h

* [OfferToReceiveVideo : false doesn't set a=sendonly](https://bugs.chromium.org/p/webrtc/issues/detail?id=1553)

* [Send only / Receive only with WebRTC](https://groups.google.com/forum/#!topic/discuss-webrtc/LFlVKoxhSO8)

