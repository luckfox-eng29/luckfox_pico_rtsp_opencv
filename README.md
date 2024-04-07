**Read this in other languages: [English](README.md), [中文](README_CN.md).**
# luck_pico_rtsp_opencv
Test luckfox-pico using opencv-mobile to add FPS to the top left corner of the captured image from the camera and stream it via RTSP.

## Development Environment
+ luckfox-pico SDK

## Compilation
```
export LUCKFOX_SDK_PATH=<Your Luckfox-pico SDK Path>
mkdir build
cd build
cmake ..
make && make install
```

## Execution
Upload the compiled `luckfox_rtsp_opencv_demo/luckfox_rtsp_opencv` to luckfox-pico, navigate to the folder, and run:
```
./luckfox_rtsp_opencv
```
Use VLC to open the network stream `rtsp://172.32.0.93/live/0` (replace the IP address with the actual one to fetch the image).
**Note**: Before running, please close the default system rkipc program by executing `RkLunch-stop.sh`.

Try lowering the streaming resolution on luckfox-pico/mini/plus to see the program running while preserving the format.