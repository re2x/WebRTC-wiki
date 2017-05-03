# WebRTC工具 gn 简介

gn是chromium项目的depot_tools工具中的一个命令，根据.gn文件生成ninja所需的文件。

gn位置位于depot_tools/gn，是个脚本，实际上是执行Python脚本 `depot_tools/gn.py`

``` python
base_dir=$(dirname "$0")
PYTHONDONTWRITEBYTECODE=1 exec python "$base_dir/gn.py" "$@"
```

* [gn介绍](https://www.chromium.org/developers/gn-build-configuration)
* [快速入门](https://chromium.googlesource.com/chromium/src/+/master/tools/gn/docs/quick_start.md)
* [完整文档](https://chromium.googlesource.com/chromium/src/+/master/tools/gn/docs)
* [单页版手册](https://chromium.googlesource.com/chromium/src/+/master/tools/gn/docs/reference.md)