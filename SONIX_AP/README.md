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
### Set Bitrate
```
./SONiX_UVC_TestAP --xuget-br /dev/video1
./SONiX_UVC_TestAP --xuset-br 1000 /dev/video1
./SONiX_UVC_TestAP --xuset-br 1000000 /dev/video1
./SONiX_UVC_TestAP --xuset-br 10000000 /dev/video1
```

## H264
*Note: in this case /dev/video2 is the video capture device that supports H264*</br>
### Record H264
```
./SONiX_UVC_TestAP /dev/video2 -c -f H264 -r
```
### Set Bitrate
```
./SONiX_UVC_TestAP --xuget-br /dev/video1
./SONiX_UVC_TestAP --xuset-br 1000 /dev/video1
./SONiX_UVC_TestAP --xuset-br 1000000 /dev/video1
./SONiX_UVC_TestAP --xuset-br 10000000 /dev/video1
```

