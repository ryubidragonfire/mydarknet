# WHAT's THE DIFFERENCE?
This is largely the same as [AlexeyAB/darknet](https://github.com/AlexeyAB/darknet).

Additions: 
- facilities to write out predictions results on an image and a video to `.txt` and `.json`. 
    - Modifications are recorded in [write-out-predictions.md](https://github.com/ryubidragonfire/mydarknet/blob/master/write-out-predictions.md).
    - Output format can be found in [write-out-predictions.md](https://github.com/ryubidragonfire/mydarknet/blob/master/write-out-predictions.md).

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
### 3. Transfer-learning: Train with your own data with different classes
See [AlexeyAB/darknet](https://github.com/AlexeyAB/darknet#how-to-train-to-detect-your-custom-objects)

### 4. Labelling your own data
see [AlexeyAB/Yolo_mark](https://github.com/AlexeyAB/Yolo_mark)

## ISSUES & SOLUTIONS
Issues and solutions (where possible) are recorded [here](https://gist.github.com/ryubidragonfire/a70bc052af897179cb3670aa320e3d30).

## Reference:
- [Darknet](https://github.com/pjreddie/darknet/)
  - Neural Network in C, CUDA
- [yad2k](https://github.com/allanzelener/YAD2K)
  - 90% Keras, 10% Tensorflow
  - convert a Darnet Yolov2 model to a Keras model, using Darknet's xxx.cfg and xxx.weights
- https://github.com/experiencor
  - yolo2 in keras
  - yolo3 in keras
- https://github.com/lhk/object_detection/blob/master/yolov2/
  - yolov2 with yad2k
- [AlexeyAB](https://github.com/AlexeyAB/darknet)
  - train with your own data and classes, using Darknet
  - [Yolo-mark](https://github.com/AlexeyAB/Yolo_mark)
    - annotation for yolov2, yolov3  
  - Yolo9000 .cfg, .weights
- [吳恩達](https://github.com/enggen/Deep-Learning-Coursera/tree/master/Convolutional%20Neural%20Networks/Week3/Car%20detection%20for%20Autonomous%20Driving)
  - Intersection of Union, Non-maxima supression
  - Display
- [how-to-train-yolov2-to-detect-custom-objects](https://timebutt.github.io/static/how-to-train-yolov2-to-detect-custom-objects/) 
- https://github.com/datlife/yolov2

### Reference - object tracking:
- [Re3
: Real-Time Recurrent Regression Networks for
Visual Tracking of Generic Objects](https://arxiv.org/pdf/1705.06368.pdf)
 
### Reference - transfer learning:
- [cs231n: Transfer Learning](http://cs231n.github.io/transfer-learning/) 
