
**其他语言版本: [English](README.md), [中文](README_CN.md).**
# luck_pico_rtsp_opencv
测试 luckfox-pico 使用 opencv-mobile 添加在摄像头采集图像左上角添加 fps 并进行 rtsp 推流。

## 开发环境
+ luckfox-pico sdk

## 编译
```
export LUCKFOX_SDK_PATH=<Your Luckfox-pico Sdk Path>
mkdir build
cd build
cmake ..
make && make install
```

## 运行
将编译生成的`luckfox_rtsp_opencv_demo/luckfox_rtsp_opencv`上传到 luckfox-pico 上，进入文件夹运行
```
./luckfox_rtsp_opencv
```
使用 vlc 打开网络串流 rtsp://172.32.0.93/live/0（按实际情况修改IP地址拉取图像）
**注意**：运行前请关闭系统默认 rkipc 程序，执行 `RkLunch-stop.sh` 关闭
在 luckfox-pico / mini / plus 上尝试降低推流分辩率保存程序运行。
