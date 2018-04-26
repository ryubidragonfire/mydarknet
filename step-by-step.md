# WHAT's THE DIFFERENCE?
This is largely the same as [AlexeyAB/darknet](https://github.com/AlexeyAB/darknet).

This is a record of my experience. Please let me know if you spotted any mistakes/anomallies :) !

Additions: 
- facilities to write out predictions results on an image and a video to `.txt` and `.json`. 
    - Modifications are recorded in [write-out-predictions.md](https://github.com/ryubidragonfire/mydarknet/blob/master/write-out-predictions.md).
    - Output format can be found in [write-out-predictions.md](https://github.com/ryubidragonfire/mydarknet/blob/master/write-out-predictions.md).

# YOU MAY ALSO NEED
## Connecting from Windows to Linux VM 
- [Putty](https://support.rackspace.com/how-to/connecting-to-linux-from-windows-by-using-putty/) to access Linux VM from window through command line. And/Or,
- [X2Go](https://docs.microsoft.com/en-us/azure/machine-learning/data-science-virtual-machine/linux-dsvm-intro#how-to-access-the-linux-data-science-virtual-machine) to access a graphical desktop of the Linux VM.

## Text Editing
- from `Putty`

    - use `vi your-file-name` to edit, [cheatsheet](http://www.atmos.albany.edu/daes/atmclasses/atm350/vi_cheat_sheet.pdf)

    - use `cat your-file-name`, to view only
        
- from `X2Go`, could make your life easier

## Save `.mp4` to individual frames
- install `ffmpeg`

`sudo apt install ffmpeg`

- do
`ffmpeg -i your-folder/your-video.mp4 -vf fps=1 your-video%04d.jpg -hide_banner `

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

In `darknet\`,

`./darknet detector test build/darknet/x64/data/coco.data build/darknet/x64/yolov3.cfg  build/darknet/x64/yolov3.weights -i 0 -thresh 0.25 build/darknet/x64/data/dog.jpg`

- to turn off display window
  
  `./darknet detector test build/darknet/x64/data/coco.data build/darknet/x64/yolov3.cfg  build/darknet/x64/yolov3.weights -i 0 -thresh 0.25 build/darknet/x64/data/dog.jpg -dont_show`
  
- running either of the command above will produce:

    - predicted result in `.json`, e.g. `build/darknet/x64/data/dog-predicted.json`
    
    - predicted result in `.jpg`, e.g. `build/darknet/x64/data/dog-predicted.jpg`

#### 2.2. Detect on a video

In `darknet\`,

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

#### 2.3. Detect on a list of images
` ./darknet detector test build/darknet/x64/data/coco.data build/darknet/x64/yolov3.cfg build/darknet/x64/yolov3.weights -i 0 -thresh 0.25 <build/darknet/x64/data/test-images.txt> ./out/result.txt -dont_show` 

In this case, `./out/result.txt` can be ignored. Individual output files xxx.json can be found in the same directory where the test images are. 

### 3. Transfer-learning: Train with your own data with different classes
See [AlexeyAB/darknet](https://github.com/AlexeyAB/darknet#how-to-train-to-detect-your-custom-objects)

### 4. Labelling your own data
see [AlexeyAB/Yolo_mark](https://github.com/AlexeyAB/Yolo_mark)

### 5. YOLO9000
Ref: https://github.com/AlexeyAB/darknet#using-yolo9000

Detection of 9418 objects:

#### 5.1. Using YOLO9000

- get `yolo9000.weights` from http://pjreddie.com/media/files/yolo9000.weights

- in `yolo9000.cfg`, the last 2 lines:
    - set `tree` to path to `9k.tree` , can be found at https://raw.githubusercontent.com/AlexeyAB/darknet/master/build/darknet/x64/data/9k.tree
    - set `map` to path to `coco9k.map` , can be found at https://raw.githubusercontent.com/AlexeyAB/darknet/master/build/darknet/x64/data/coco9k.map

- in `combine9k.data`, which can be found at https://raw.githubusercontent.com/AlexeyAB/darknet/master/build/darknet/x64/data/combine9k.data
    - set `labels` to path to `9k.labels` (9418 labels of objects: https://raw.githubusercontent.com/AlexeyAB/darknet/master/build/darknet/x64/data/9k.labels)
    - set `names` to path to `9k.names` (9418 names of objects: https://raw.githubusercontent.com/AlexeyAB/darknet/master/build/darknet/x64/data/9k.names)
    - set `map` to path to `inet9k.map` (map 200 categories from ImageNet to WordTree `9k.tree`: https://raw.githubusercontent.com/AlexeyAB/darknet/master/build/darknet/x64/data/inet9k.map)
    
#### 5.1. Fine-tuning YOLO9000 with your own data using same class labels as YOLO9000

- Use [Yolo-mark](https://github.com/AlexeyAB/Yolo_mark) to annotate your images, together with the [`9k.names`](https://raw.githubusercontent.com/AlexeyAB/darknet/master/build/darknet/x64/data/9k.names)

- set `train` to path to `your-text-file-that-contain-a-list-of-training-images.txt`.

#### 5.2. Transfer-learn YOLO9000 with your own data using different class labels from YOLO9000

- I have no idea yet. Please comment if you would like to share.

## ISSUES & SOLUTIONS
Issues and solutions (where possible) are recorded [here](https://gist.github.com/ryubidragonfire/a70bc052af897179cb3670aa320e3d30).

## LINUX COMMANDS
Commonly used linux commands can be found in my [gist](https://gist.github.com/ryubidragonfire/4cab2ac5731c96fcbdfd333bc758588e)

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
