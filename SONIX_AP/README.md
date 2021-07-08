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

## Save MJPG frames 
*Note: in this case /dev/video0 is the video capture device that supports MJPG*</br>
```
./SONiX_UVC_TestAP /dev/video0 -c -f mjpg -S
```

## Record h264 video
*Note: in this case /dev/video2 is the video capture device that supports H264*</br>
```
./SONiX_UVC_TestAP /dev/video2 -c -f H264 -r
```

## Extension Unit (XU) controls

### Add XU ctrls to uvc driver (if uvc driver doesn't support)
```
./SONiX_UVC_TestAP /dev/video2 -a
```

### Get and set bitrate
```
./SONiX_UVC_TestAP --xuget-br /dev/video1
./SONiX_UVC_TestAP --xuset-br 1000 /dev/video1
./SONiX_UVC_TestAP --xuset-br 1000000 /dev/video1
./SONiX_UVC_TestAP --xuset-br 10000000 /dev/video1
```

### Set framerate
```
./SONiX_UVC_TestAP --fr 30 /dev/video1
./SONiX_UVC_TestAP --msfr 30 /dev/video1
```
