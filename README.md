# exploreHD Controls

## UVC Driver Settings

**NOTE: You must be running the Lower Bandwidth firmware on the camera to edit these settings** - <https://docs.exploredeepwater.com/software/firmware.html>

With the exploreHD, it is possible to decrease the bandwidth to get multicam support.

To setup the driver controls, run:
```
cd explorehd_camera_controls
cp Makefile.x86 Makefile
make
```
This will compile the exploreHD controls application. (NOTE: this will be replaced by user interface and custom driver system).

**NOTE: These settings must be adjusting each time. You can setup a startup script in Linux.**

You can find the device by listing with `v4l2-ctl --list-devices`

For each device, there will typically be 4 `/dev/video*` entries. The third entry will be the H.264 compatible device for the exploreHD. For this example, we will be using `/dev/video0`

### Maximum Quality (same as higher bandwidth firmware)

Disable H.264 compression:

`./explorehd_UVC_TestAP --xuset-gop 0 /dev/video0`

Enable variable bitrate:

`./explorehd_UVC_TestAP --xuset-cvm 2 /dev/video0`

### Lower Bandwidth

Enable H.264 compression:

`./explorehd_UVC_TestAP --xuset-gop 28 /dev/video0`

Disable variable bitrate:

`./explorehd_UVC_TestAP --xuset-cvm 1 /dev/video0`

Set the bitrate:

`./explorehd_UVC_TestAP --xuset-br 10000 /dev/video0`

## Setup with ArduSub Companion (No longer neccesary with the latest exploreHD firmware update. Only needed if you want more custom controls with the exploreHD)
### Setup WinSCP and PuTTy. Open WinSCP and start a new session.
*File Protocol*
```
SFTP
```
*Host name*
```
192.168.2.2
```
*Port number*
```
22
```
*User name*
```
pi
```
*Password*
```
companion
```
*Navigate to*
```
/home/pi/companion/scripts/
```
*Replace start_video.sh with current start_video.sh in this folder. Copy explorehd_camera_controls folder and put it in the scripts folder.*
*Open a new PuTTy session and login with companion as passowrd. Type the following to terminal.*
```
cd companion/scripts/explorehd_camera_controls
cp Makefile.x86 Makefile
make
```
*Restart the pi to recieve the new stream.*
## H264
### Sending Stream
```
gst-launch-1.0 -v v4l2src device=/dev/video1 ! video/x-h264, width=1920,height=1080! h264parse ! queue ! rtph264pay config-interval=10 pt=96 ! udpsink host=192.168.2.1 port=5600 sync=false
```
### Rceiving Stream
*Two commands you can use*
```
gst-launch-1.0 -v udpsrc port=5600 ! application/x-rtp, encoding-name=H264,payload=96! rtpjitterbuffer ! rtph264depay ! avdec_h264 ! autovideosink
```
```
gst-launch-1.0 -v udpsrc port=5600 caps = "application/x-rtp, media=(string)video, clock-rate=(int)90000, encoding-name=(string)H264, payload=(int)96" ! rtph264depay ! decodebin ! videoconvert ! autovideosink
```
*OBS Pipeline*
```
udpsrc port=5600 ! application/x-rtp,media=(string)video,clock-rate=(int)90000,encoding-name=(string)H264 ! rtph264depay ! avdec_h264 output-corrupt=false ! videoconvert ! video. 
```

## Streaming YUYV encoded as MJPEG
```
gst-launch-1.0 -v v4l2src device=/dev/video0 ! video/x-raw,format=YUY2,width=640,height=480 ! jpegenc ! rtpjpegpay ! udpsink host=127.0.0.1 port=5000
```
## Streaming YUYV encoded as H264
```
gst-launch-1.0 -v v4l2src  device=/dev/video0 ! video/x-raw,framerate=30/1 ! videoscale ! videoconvert ! x264enc tune=zerolatency bitrate=500 speed-preset=superfast ! rtph264pay ! udpsink host=127.0.0.1 port=5000
```

## DeepStream 
DeepStream must be installed on your machine: https://docs.nvidia.com/metropolis/deepstream/dev-guide/text/DS_Quickstart.html </br>
For more info: https://developer.nvidia.com/deepstream-sdk </br>
### Displaying Video from Camera
```
gst-launch-1.0 v4l2src device=/dev/video0 io-mode=2 ! "image/jpeg,width=1920,height=1080" ! nvjpegdec ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12' ! nveglglessink -e
```
### Receiving MJPG Stream
```
gst-launch-1.0 -v udpsrc port=5000 ! application/x-rtp, media=video, clock-rate=90000, encoding-name=JPEG, payload=26 ! rtpjpegdepay ! nvjpegdec ! nvvideoconvert ! 'video/x-raw(memory:NVMM),format=NV12' ! nveglglessink -e
```

## Recording MJPG Stream with NVENC GStreamer Plugin
```
gst-launch-1.0 -v udpsrc port=5000 ! application/x-rtp,media=video,clock-rate=90000,encoding-name=JPEG,payload=26 ! rtpjpegdepay ! jpegdec ! queue ! nvh264enc ! h264parse ! mp4mux ! filesink location=video.mp4
```
