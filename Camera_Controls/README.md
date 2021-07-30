# Compile
```
cd SONIX_AP
cp Makefile.x86 Makefile
make
```
# Using the executable
## Printing help
```
./SONiX_UVC_TestAP -h
```

## Finding Your Video Devices
List your video devices with the following command:
```
v4l2-ctl --list-devices
```
The output should be similar to this: 
```
exploreHD USB Camera: exploreHD (usb-0000:00:14.0-2):
        /dev/video0
        /dev/video1
        /dev/video2
        /dev/video3
```
After finding the video devices for your exploreHB USB camera.</br>
Run this command with your respective /dev/video devices and determine which video devices support MJPG and H264: 
```
v4l2-ctl -d /dev/video0 --list-formats
```
Output: 
```
ioctl: VIDIOC_ENUM_FMT
        Type: Video Capture

        [0]: 'MJPG' (Motion-JPEG, compressed)
        [1]: 'YUYV' (YUYV 4:2:2)
```
Command run: 
```
v4l2-ctl -d /dev/video2 --list-formats
```
Output:
```
ioctl: VIDIOC_ENUM_FMT
        Type: Video Capture

        [0]: 'H264' (H.264, compressed)

```
## Setting Up XU ctrls
```
./SONiX_UVC_TestAP /dev/video2 -a
```

## MJPG 
### Save MJPG Frames
*Note: in this case /dev/video0 is the video capture device that supports MJPG*</br>
```
./SONiX_UVC_TestAP /dev/video0 -c -f mjpg -S
```
### Get Bitrate
```
./SONiX_UVC_TestAP --xuget-mjb /dev/video0
```
### Set Bitrate (bits/second)
```
./SONiX_UVC_TestAP --xuset-mjb 30000000 /dev/video0
```

## H264
*Note: in this case /dev/video2 is the video capture device that supports H264*</br>
### Record H264
```
./SONiX_UVC_TestAP /dev/video2 -c -f H264 -r
```
### Get Bitrate 
```
./SONiX_UVC_TestAP --xuget-br /dev/video2
```
### Set Bitrate (bits/second)
```
./SONiX_UVC_TestAP --xuset-br 30000000 /dev/video2
```

