# QUICK START FOR SYNC WEEK APRIL 2018
YOLOv3 is tested on AZURE LINUX DLVM, with YOLOV3

## Step by step

ref: https://github.com/AlexeyAB/darknet

### compile in linux
- git clone https://github.com/AlexeyAB/darknet.git

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

- Download yolov3.weights in `build/darknet/x64/` 

`wget https://pjreddie.com/media/files/yolov3.weights`

- Check that it works. CD to `darknet/`

`./darknet detector test build/darknet/x64/data/coco.data build/darknet/x64/yolov3.cfg  build/darknet/x64/yolov3.weights -i 0 -thresh 0.25 build/darknet/x64/data/dog.jpg`
