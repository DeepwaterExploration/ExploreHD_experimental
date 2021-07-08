# ExploreHD_experimental
## H264

### Sending Streaming 

### Receiving Stream

## Streaming YUYV via MJPEG

## Streaming YUYV via H264

## DeepStream 
DeepStream must be installed on you machine: https://docs.nvidia.com/metropolis/deepstream/dev-guide/text/DS_Quickstart.html </br>
For more info: https://developer.nvidia.com/deepstream-sdk </br>
### Displaying Video from Camera
```
gst-launch-1.0 v4l2src device=/dev/video1 io-mode=2 ! "image/jpeg,width=1920,height=1080" ! nvjpegdec ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12' ! nveglglessink -e
```
### Receiving Mjpeg Stream
```
gst-launch-1.0 -v udpsrc port=5000 ! application/x-rtp, media=video, clock-rate=90000, encoding-name=JPEG, payload=26 ! rtpjpegdepay ! nvjpegdec ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12' ! nveglglessink -e
```

## Recording with nvidia

