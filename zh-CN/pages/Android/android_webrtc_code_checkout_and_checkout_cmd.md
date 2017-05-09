# WebRTC Android 代码获取/检查命令顺序

命令：

``` shell
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
export PATH=`pwd`/depot_tools:"$PATH"  # 修改环境变量，建议加到~/.bashrc
mkdir webrtc-checkout
cd webrtc-checkout  
fetch --nohooks webrtc_android  # fetch webrtc android的代码库基础配置
cd src
git fetch  # fetch webrtc git代码库
git checkout branch-heads/59 # checkout59的release分支
gclient sync  # 同步59分支相关的工具链、依赖库
./build/install-build-deps.sh # 编译环境安装
./build/install-build-deps-android.sh # Android编译环境安装
gn gen out/Debug --args='target_os="android" target_cpu="arm" is_debug=false dcheck_always_on=true symbol_level=1 is_component_build=false'  # 产生编译配置
ninja -C out/Debug # 编译整个项目
ninja -C out/Debug AppRTCMobile # 编译AppRTCMobile apk，输出目录out/Debug/apks/AppRTCMobile.apk
build/android/gradle/generate_gradle.py --output-directory $PWD/out/Debug --target "//webrtc/examples:AppRTCMobile" --use-gradle-process-resources --split-projects # 产生Android Studio工程
```

如果能够顺利执行完以上命令，那么代码获取是完整的，你可以开始安心撸码了。