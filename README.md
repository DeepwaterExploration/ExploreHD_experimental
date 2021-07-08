# ExploreHD_experimental
## H264
### Sending Streaming 
```
```
### Rceiving Stream
```
```
## Streaming YUYV via MJPEG
```
gst-launch-1.0 -v v4l2src device=/dev/video0 ! video/x-raw,format=YUY2,width=640,height=480 ! jpegenc ! rtpjpegpay ! udpsink host=127.0.0.1 port=5000
```
## Streaming YUYV via H264
```
gst-launch-1.0 -v v4l2src  device=/dev/video0 ! video/x-raw,framerate=30/1 ! videoscale ! videoconvert ! x264enc tune=zerolatency bitrate=500 speed-preset=superfast ! rtph264pay ! udpsink host=127.0.0.1 port=5000
```

## DeepStream 
DeepStream must be installed on you machine: https://docs.nvidia.com/metropolis/deepstream/dev-guide/text/DS_Quickstart.html </br>
For more info: https://developer.nvidia.com/deepstream-sdk </br>
### Displaying Video from Camera
```
gst-launch-1.0 v4l2src device=/dev/video0 io-mode=2 ! "image/jpeg,width=1920,height=1080" ! nvjpegdec ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12' ! nveglglessink -e
```
### Receiving MJPG Stream
```
gst-launch-1.0 -v udpsrc port=5000 ! application/x-rtp, media=video, clock-rate=90000, encoding-name=JPEG, payload=26 ! rtpjpegdepay ! nvjpegdec ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12' ! nveglglessink -e
```

## Recording MJPG Stream with nvidia
gst-launch-1.0 -v udpsrc port=5000 ! application/x-rtp, media=video, clock-rate=90000, encoding-name=JPEG, payload=26 ! rtpjpegdepay ! jpegdec ! nvh264enc ! h264parse ! mp4mux ! filesink location=video3.mp4
