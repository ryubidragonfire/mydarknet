# QUICK START FOR USING DARKNET's YOLOV3
The following are tested on AZURE LINUX DLVM, with YOLOV3

## Step by step

ref: https://github.com/AlexeyAB/darknet

### 1. compile in linux
- git clone https://github.com/ryubidragonfire/mydarknet

- In `darknet\Makefile`, change:

```
GPU=1 to build with CUDA to accelerate by using GPU (CUDA should be in /usr/local/cuda)
CUDNN=1 to build with cuDNN v5-v7 to accelerate training by using GPU (cuDNN should be in /usr/local/cudnn)
OPENCV=1 to build with OpenCV 3.x/2.4.x - allows to detect on video files and video streams from network cameras or web-cams
DEBUG=1 to bould debug version of Yolo
```

- In `darknet\`
```
make
```

### 2. Using darknet - yolov3
- Download yolov3.weights in `build/darknet/x64/` 

`wget https://pjreddie.com/media/files/yolov3.weights`

#### 2.1. Detect on a single image

`./darknet detector test build/darknet/x64/data/coco.data build/darknet/x64/yolov3.cfg  build/darknet/x64/yolov3.weights -i 0 -thresh 0.25 build/darknet/x64/data/dog.jpg`

- to turn off display window
  
  `./darknet detector test build/darknet/x64/data/coco.data build/darknet/x64/yolov3.cfg  build/darknet/x64/yolov3.weights -i 0 -thresh 0.25 build/darknet/x64/data/dog.jpg -dont_show`

#### 2.2. Detect on a video

`./darknet detector demo build/darknet/x64/data/coco.data build/darknet/x64/yolov3.cfg build/darknet/x64/yolov3.weights -i 0 -thresh 0.25 data/toy-car.mp4`

- to turn off display window
  
  ` ./darknet detector demo build/darknet/x64/data/coco.data build/darknet/x64/yolov3.cfg build/darknet/x64/yolov3.weights -i 0 -thresh 0.25 data/toy-car.mp4 -out_filename data/predicted-toy-car.avi -dont_show`

- to save predicted video
  
  `./darknet detector demo build/darknet/x64/data/coco.data build/darknet/x64/yolov3.cfg build/darknet/x64/yolov3.weights -i 0 -thresh 0.25 data/toy-car.mp4 -out_filename data/predicted-toy-car.avi`
  
##### 2.2.1. View .avi using [VLC](https://www.videolan.org/vlc/download-ubuntu.html)
- to install

```
% sudo apt-get update
% sudo apt-get install vlc browser-plugin-vlc
```
##### 2.2.2. View .avi using [SMPlayer](https://www.smplayer.info/en/downloads)
- to install

```
sudo add-apt-repository ppa:rvm/smplayer 
sudo apt-get update 
sudo apt-get install smplayer smplayer-themes smplayer-skins 
```
