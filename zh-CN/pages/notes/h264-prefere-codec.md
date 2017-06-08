# 调整H264为首选视频编码

修改 `internalencoderfactory.cc` 代码中 `InternalEncoderFactory`类的构造函数，将如下代码移到最前面
``` cpp
  if (webrtc::H264Encoder::IsSupported()) {
    cricket::VideoCodec codec(kH264CodecName);
    // TODO(magjed): Move setting these parameters into webrtc::H264Encoder
    // instead.
    codec.SetParam(kH264FmtpProfileLevelId,
                   kH264ProfileLevelConstrainedBaseline);
    codec.SetParam(kH264FmtpLevelAsymmetryAllowed, "1");
    supported_codecs_.push_back(std::move(codec));
  }
```

# 参考
* http://blog.csdn.net/foruok/article/details/69525039