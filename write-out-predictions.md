# Modifications for V3
## Write out bboxes, predicted class, probablities for an image
 
- `src/image.c` line 203 

  `void draw_detections_v3(image im, detection *dets, int num, float thresh, char **names, image **alphabet, int classes)`
  
  - in `draw_detections_v3()` after line 264, output `left`, `right`, `top`, `bot`
  
  - in `draw_detections_v3()` after line 225, output `names[j]`, `dets[i].prob[j]`
  
- output example in xxx.txt
```
diningtable 0.801663 46 88 1810 1002
car 0.649124 736 300 1026 481
```

- output example in xxx.json
```
{
    "predictions": [
        {
            "class": "diningtable",
            "prob": 0.801663,
            "left": 46,
            "top": 88,
            "right": 1810,
            "bot": 1002
        },
        {
            "class": "car",
            "prob": 0.649124,
            "left": 736,
            "top": 300,
            "right": 1026,
            "bot": 481
        }
    ]
}
```
 
## Write out bboxes, predicted class, probablities for a video
- `src/image.c` line 346 `ifdef OPENCV`

Based on `void draw_detections_cv_v3(IplImage* show_img, detection *dets, int num, float thresh, char **names, image **alphabet, int classes)`, the following are created:

- `void draw_detections_cv_v3_and_write_to_txt(IplImage* show_img, detection *dets, int num, float thresh, char **names, image **alphabet, int classes)`

- `void draw_detections_cv_v3_and_write_to_json(IplImage* show_img, detection *dets, int num, float thresh, char **names, image **alphabet, int classes)`

`draw_detections_cv_v3()`, `draw_detections_cv_v3_and_write_to_txt()` and `draw_detections_cv_v3_and_write_to_json` are used in `demo.c` to predict on video.

Output has the same format as shown in **Write out bboxes, predicted class, probablities for an image** section.

- `demo.c`
In `demo.c`, where first frame is denoted `#0`,

``` c
if (output_video_writer && show_img) {
   frame_count++;
   cvWriteFrame(output_video_writer, show_img);
   printf("\n cvWriteFrame \n");
   printf("frame_count: %d \n", frame_count);
}
```
The original code above only writes out frame `#2` to `#(lastframe-2)`. This can be shown by printing out `frame_count`.

The added function `draw_detections_cv_v3_and_write_to_txt()` writes out `#1` to `#lastframe`


# To Do
## Log file for training and validation loss

- `src/image.c` line 508 `ifdef OPENCV`

  `IplImage* draw_train_chart(float max_img_loss, int max_batches, int number_of_lines, int img_size)`

- `src/image.c` line 554 `ifdef OPENCV`

  `void draw_train_loss(IplImage* img, int img_size, float avg_loss, float max_img_loss, int current_batch, int max_batches)`
