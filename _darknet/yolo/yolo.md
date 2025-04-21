---
title: "YOLO: Real-Time Object Detection"
description: You only look once (YOLO) is a state-of-the-art, real-time object detection system.
layout: darknet
logo: /static/img/yologo.png
order: 1
permalink: /darknet/yolo/
---

You only look once (YOLO) is a state-of-the-art, real-time object detection system. On a Pascal Titan X it processes images at 30 FPS and has a mAP of 57.9% on COCO test-dev.

<iframe width="100%" height="415" src="https://www.youtube.com/embed/MPU2HistivI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

<style>
td:first-child {text-align:left;}
td {text-align:right; padding:0em .5em;}
th {padding:0em .5em;}
table {
margin:0em auto 3em; font-size:1em;
}
@media (max-width: 640px) {
  table {
    font-size: .75em;
  }
.model {
  font-size: .5em;
}
}
</style>

## Comparison to Other Detectors ##

YOLOv3 is extremely fast and accurate. In mAP measured at .5 IOU YOLOv3 is on par with Focal Loss but about 4x faster. Moreover, you can easily tradeoff between speed and accuracy simply by changing the size of the model, no retraining required!

![](chart.png)

## Performance on the COCO Dataset ##

<div>
<table>
  <tr>
    <th style="text-align:left;">Model</th>
    <th style="text-align:right;">Train</th>
    <th style="text-align:right;">Test</th>
    <th style="text-align:right;">mAP</th>
    <th style="text-align:right; ">FLOPS</th>
    <th style="text-align:right; ">FPS</th>
    <th style="text-align:right; ">Cfg</th>
    <th style="text-align:right; ">Weights</th>
  </tr>


 <tr>
<td>SSD300</td>
<td>COCO trainval</td>
<td>test-dev</td>
<td>41.2</td>
<td>-</td>
<td>46</td>
<td colspan = "2"><a href="https://arxiv.org/abs/1512.02325">link</a></td>
</tr>
 <tr>
<td>SSD500</td>
<td>COCO trainval</td>
<td>test-dev</td>
<td>46.5</td>
<td>-</td>
<td>19</td>
<td colspan="2"><a href="https://arxiv.org/abs/1512.02325">link</a></td>
</tr>

<tr>
<td>YOLOv2 608x608</td>
<td>COCO trainval</td>
<td>test-dev</td>
<td>48.1</td>
<td>62.94 Bn</td>
<td>40</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/yolov2.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/yolov2.weights">weights</a></td>
</tr>
<tr>
<td>Tiny YOLO</td>
<td>COCO trainval</td>
<td>test-dev</td>
<td>23.7</td>
<td>5.41 Bn</td>
<td>244</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/yolov2-tiny.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/yolov2-tiny.weights">weights</a></td>
</tr>

<tr>
          <td colspan="8"><hr/></td>
</tr>


<tr>
<td>SSD321</td>
<td>COCO trainval</td>
<td>test-dev</td>
<td>45.4</td>
<td>-</td>
<td>16</td>
<td colspan = "2"><a href="https://arxiv.org/abs/1701.06659">link</a></td>
</tr>
<tr>
<td>DSSD321</td>
<td>COCO trainval</td>
<td>test-dev</td>
<td>46.1</td>
<td>-</td>
<td>12</td>
<td colspan = "2"><a href="https://arxiv.org/abs/1701.06659">link</a></td>
</tr>
<tr>
<td>R-FCN</td>
<td>COCO trainval</td>
<td>test-dev</td>
<td>51.9</td>
<td>-</td>
<td>12</td>
<td colspan = "2"><a href="https://arxiv.org/abs/1605.06409">link</a></td>
</tr>
<tr>
<td>SSD513</td>
<td>COCO trainval</td>
<td>test-dev</td>
<td>50.4</td>
<td>-</td>
<td>8</td>
<td colspan = "2"><a href="https://arxiv.org/abs/1701.06659">link</a></td>
</tr>
<tr>
<td>DSSD513</td>
<td>COCO trainval</td>
<td>test-dev</td>
<td>53.3</td>
<td>-</td>
<td>6</td>
<td colspan = "2"><a href="https://arxiv.org/abs/1701.06659">link</a></td>
</tr>
<tr>
<td>FPN FRCN</td>
<td>COCO trainval</td>
<td>test-dev</td>
<td>59.1</td>
<td>-</td>
<td>6</td>
<td colspan = "2"><a href="https://arxiv.org/abs/1612.03144">link</a></td>
</tr>
<tr>
<td>Retinanet-50-500</td>
<td>COCO trainval</td>
<td>test-dev</td>
<td>50.9</td>
<td>-</td>
<td>14</td>
<td colspan = "2"><a href="https://arxiv.org/abs/1708.02002">link</a></td>
</tr>
<tr>
<td>Retinanet-101-500</td>
<td>COCO trainval</td>
<td>test-dev</td>
<td>53.1</td>
<td>-</td>
<td>11</td>
<td colspan = "2"><a href="https://arxiv.org/abs/1708.02002">link</a></td>
</tr>
<tr>
<td>Retinanet-101-800</td>
<td>COCO trainval</td>
<td>test-dev</td>
<td>57.5</td>
<td>-</td>
<td>5</td>
<td colspan = "2"><a href="https://arxiv.org/abs/1708.02002">link</a></td>
</tr>
<tr>
<td>YOLOv3-320</td>
<td>COCO trainval</td>
<td>test-dev</td>
<td>51.5</td>
<td>38.97 Bn</td>
<td>45</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/yolov3.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/yolov3.weights">weights</a></td>
</tr>
<tr>
<td>YOLOv3-416</td>
<td>COCO trainval</td>
<td>test-dev</td>
<td>55.3</td>
<td>65.86 Bn</td>
<td>35</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/yolov3.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/yolov3.weights">weights</a></td>
</tr>
<tr>
<td>YOLOv3-608</td>
<td>COCO trainval</td>
<td>test-dev</td>
<td>57.9</td>
<td>140.69 Bn</td>
<td>20</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/yolov3.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/yolov3.weights">weights</a></td>
</tr>
<tr>
<td>YOLOv3-tiny</td>
<td>COCO trainval</td>
<td>test-dev</td>
<td>33.1</td>
<td>5.56 Bn</td>
<td>220</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/yolov3-tiny.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/yolov3-tiny.weights">weights</a></td>
</tr>
<tr>
<td>YOLOv3-spp</td>
<td>COCO trainval</td>
<td>test-dev</td>
<td>60.6</td>
<td>141.45 Bn</td>
<td>20</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/yolov3-spp.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/yolov3-spp.weights">weights</a></td>
</tr>

</table>
</div>

## How It Works ##

Prior detection systems repurpose classifiers or localizers to perform detection. They apply the model to an image at multiple locations and scales. High scoring regions of the image are considered detections.

We use a totally different approach. We apply a single neural network to the full image. This network divides the image into regions and predicts bounding boxes and probabilities for each region. These bounding boxes are weighted by the predicted probabilities.

![](sayit.jpg)

Our model has several advantages over classifier-based systems. It looks at the whole image at test time so its predictions are informed by global context in the image. It also makes predictions with a single network evaluation unlike systems like [R-CNN](https://github.com/rbgirshick/rcnn) which require thousands for a single image. This makes it extremely fast, more than 1000x faster than R-CNN and 100x faster than [Fast R-CNN](https://github.com/rbgirshick/fast-rcnn). See our [paper](/static/papers/YOLOv3.pdf) for more details on the full system.

### What's New in Version 3? ###

YOLOv3 uses a few tricks to improve training and increase performance, including: multi-scale predictions, a better backbone classifier, and more. The full details are in our [paper](/static/papers/YOLOv3.pdf)!

## Detection Using A Pre-Trained Model ##

This post will guide you through detecting objects with the YOLO system using a pre-trained model. If you don't already have Darknet installed, you should [do that first](/darknet/install/). Or instead of reading all that just run:

    git clone https://github.com/pjreddie/darknet
    cd darknet
    make

Easy!

You already have the config file for YOLO in the `cfg/` subdirectory. You will have to download the pre-trained weight file [here (237 MB)](/static/files/yolov3.weights). Or just run this:

    wget https://data.pjreddie.com/files/yolov3.weights

Then run the detector!

    ./darknet detect cfg/yolov3.cfg yolov3.weights data/dog.jpg

You will see some output like this:

    layer     filters    size              input                output
        0 conv     32  3 x 3 / 1   416 x 416 x   3   ->   416 x 416 x  32  0.299 BFLOPs
        1 conv     64  3 x 3 / 2   416 x 416 x  32   ->   208 x 208 x  64  1.595 BFLOPs
        .......
      105 conv    255  1 x 1 / 1    52 x  52 x 256   ->    52 x  52 x 255  0.353 BFLOPs
      106 detection
    truth_thresh: Using default '1.000000'
    Loading weights from yolov3.weights...Done!
    data/dog.jpg: Predicted in 0.029329 seconds.
    dog: 99%
    truck: 93%
    bicycle: 99%
![](dog.png)

Darknet prints out the objects it detected, its confidence, and how long it took to find them. We didn't compile Darknet with `OpenCV` so it can't display the detections directly. Instead, it saves them in `predictions.png`. You can open it to see the detected objects. Since we are using Darknet on the CPU it takes around 6-12 seconds per image. If we use the GPU version it would be much faster.

I've included some example images to try in case you need inspiration. Try `data/eagle.jpg`, `data/dog.jpg`, `data/person.jpg`, or `data/horses.jpg`!

The `detect` command is shorthand for a more general version of the command. It is equivalent to the command:

    ./darknet detector test cfg/coco.data cfg/yolov3.cfg yolov3.weights data/dog.jpg

You don't need to know this if all you want to do is run detection on one image but it's useful to know if you want to do other things like run on a webcam (which you will see [later on](#demo)).

### Multiple Images ###

Instead of supplying an image on the command line, you can leave it blank to try multiple images in a row. Instead you will see a prompt when the config and weights are done loading:

    ./darknet detect cfg/yolov3.cfg yolov3.weights
    layer     filters    size              input                output
        0 conv     32  3 x 3 / 1   416 x 416 x   3   ->   416 x 416 x  32  0.299 BFLOPs
        1 conv     64  3 x 3 / 2   416 x 416 x  32   ->   208 x 208 x  64  1.595 BFLOPs
        .......
      104 conv    256  3 x 3 / 1    52 x  52 x 128   ->    52 x  52 x 256  1.595 BFLOPs
      105 conv    255  1 x 1 / 1    52 x  52 x 256   ->    52 x  52 x 255  0.353 BFLOPs
      106 detection
    Loading weights from yolov3.weights...Done!
    Enter Image Path:

Enter an image path like `data/horses.jpg` to have it predict boxes for that image.

![](horses.png)

Once it is done it will prompt you for more paths to try different images. Use `Ctrl-C` to exit the program once you are done.

### Changing The Detection Threshold ###

By default, YOLO only displays objects detected with a confidence of .25 or higher. You can change this by passing the `-thresh <val>` flag to the `yolo` command. For example, to display all detection you can set the threshold to 0:

    ./darknet detect cfg/yolov3.cfg yolov3.weights data/dog.jpg -thresh 0

Which produces:

![](all.png)

So that's obviously not super useful but you can set it to different values to control what gets thresholded by the model.

### Tiny YOLOv3 ###

We have a very small model as well for constrained environments, `yolov3-tiny`. To use this model, first download the weights:

    wget https://data.pjreddie.com/files/yolov3-tiny.weights

Then run the detector with the tiny config file and weights:

    ./darknet detect cfg/yolov3-tiny.cfg yolov3-tiny.weights data/dog.jpg

## <a name=demo></a>Real-Time Detection on a Webcam ###

Running YOLO on test data isn't very interesting if you can't see the result. Instead of running it on a bunch of images let's run it on the input from a webcam!

To run this demo you will need to compile [Darknet with CUDA and OpenCV](/darknet/install/#cuda). Then run the command:

    ./darknet detector demo cfg/coco.data cfg/yolov3.cfg yolov3.weights

YOLO will display the current FPS and predicted classes as well as the image with bounding boxes drawn on top of it.

You will need a webcam connected to the computer that OpenCV can connect to or it won't work. If you have multiple webcams connected and want to select which one to use you can pass the flag `-c <num>` to pick (OpenCV uses webcam `0` by default).

You can also run it on a video file if OpenCV can read the video:

    ./darknet detector demo cfg/coco.data cfg/yolov3.cfg yolov3.weights <video file>

That's how we made the YouTube video above.

## <a name=train-voc></a>Training YOLO on VOC ##

You can train YOLO from scratch if you want to play with different training regimes, hyper-parameters, or datasets. Here's how to get it working on the Pascal VOC dataset.

### Get The Pascal VOC Data ###

To train YOLO you will need all of the VOC data from 2007 to 2012. You can find links to the data [here](/projects/pascal-voc-dataset-mirror/). To get all the data, make a directory to store it all and from that directory run:

    wget https://pjreddie.com/media/files/VOCtrainval_11-May-2012.tar
    wget https://pjreddie.com/media/files/VOCtrainval_06-Nov-2007.tar
    wget https://pjreddie.com/media/files/VOCtest_06-Nov-2007.tar
    tar xf VOCtrainval_11-May-2012.tar
    tar xf VOCtrainval_06-Nov-2007.tar
    tar xf VOCtest_06-Nov-2007.tar

There will now be a `VOCdevkit/` subdirectory with all the VOC training data in it.

### Generate Labels for VOC ###

Now we need to generate the label files that Darknet uses. Darknet wants a `.txt` file for each image with a line for each ground truth object in the image that looks like:

    <object-class> <x> <y> <width> <height>

Where `x`, `y`, `width`, and `height` are relative to the image's width and height. To generate these file we will run the `voc_label.py` script in Darknet's `scripts/` directory. Let's just download it again because we are lazy.

    wget https://pjreddie.com/media/files/voc_label.py
    python voc_label.py

After a few minutes, this script will generate all of the requisite files. Mostly it generates a lot of label files in `VOCdevkit/VOC2007/labels/` and `VOCdevkit/VOC2012/labels/`. In your directory you should see:

    ls
    2007_test.txt   VOCdevkit
    2007_train.txt  voc_label.py
    2007_val.txt    VOCtest_06-Nov-2007.tar
    2012_train.txt  VOCtrainval_06-Nov-2007.tar
    2012_val.txt    VOCtrainval_11-May-2012.tar

The text files like `2007_train.txt` list the image files for that year and image set. Darknet needs one text file with all of the images you want to train on. In this example, let's train with everything except the 2007 test set so that we can test our model. Run:

    cat 2007_train.txt 2007_val.txt 2012_*.txt > train.txt

Now we have all the 2007 trainval and the 2012 trainval set in one big list. That's all we have to do for data setup!

### Modify Cfg for Pascal Data ###

Now go to your Darknet directory. We have to change the `cfg/voc.data` config file to point to your data:

      1 classes= 20
      2 train  = <path-to-voc>/train.txt
      3 valid  = <path-to-voc>2007_test.txt
      4 names = data/voc.names
      5 backup = backup

You should replace `<path-to-voc>` with the directory where you put the VOC data.


### Download Pretrained Convolutional Weights ###

For training we use convolutional weights that are pre-trained on Imagenet. We use weights from the [darknet53](/darknet/imagenet/#darknet53) model. You can just download the weights for the convolutional layers [here (76 MB)](/static/files/darknet53.conv.74).

    wget https://data.pjreddie.com/files/darknet53.conv.74


### Train The Model ###

Now we can train! Run the command:

    ./darknet detector train cfg/voc.data cfg/yolov3-voc.cfg darknet53.conv.74

## <a name=train-coco></a>Training YOLO on COCO ##

You can train YOLO from scratch if you want to play with different training regimes, hyper-parameters, or datasets. Here's how to get it working on the [COCO dataset](http://mscoco.org/dataset/#overview).

### Get The COCO Data ###

To train YOLO you will need all of the COCO data and labels. The script `scripts/get_coco_dataset.sh` will do this for you. Figure out where you want to put the COCO data and download it, for example:

    cp scripts/get_coco_dataset.sh data
    cd data
    bash get_coco_dataset.sh

Now you should have all the data and the labels generated for Darknet.

### Modify cfg for COCO ###

Now go to your Darknet directory. We have to change the `cfg/coco.data` config file to point to your data:

      1 classes= 80
      2 train  = <path-to-coco>/trainvalno5k.txt
      3 valid  = <path-to-coco>/5k.txt
      4 names = data/coco.names
      5 backup = backup

You should replace `<path-to-coco>` with the directory where you put the COCO data.

You should also modify your model cfg for training instead of testing. `cfg/yolo.cfg` should look like this:

    [net]
    # Testing
    # batch=1
    # subdivisions=1
    # Training
    batch=64
    subdivisions=8
    ....

### Train The Model ###

Now we can train! Run the command:

    ./darknet detector train cfg/coco.data cfg/yolov3.cfg darknet53.conv.74

If you want to use multiple gpus run:

    ./darknet detector train cfg/coco.data cfg/yolov3.cfg darknet53.conv.74 -gpus 0,1,2,3

If you want to stop and restart training from a checkpoint:

    ./darknet detector train cfg/coco.data cfg/yolov3.cfg backup/yolov3.backup -gpus 0,1,2,3

## <a name=openimages></a> YOLOv3 on the Open Images dataset ##

    wget https://data.pjreddie.com/files/yolov3-openimages.weights

    ./darknet detector test cfg/openimages.data cfg/yolov3-openimages.cfg yolov3-openimages.weights


## What Happened to the Old YOLO Site? ##

If you are using YOLO version 2 you can still find the site here: [/darknet/yolov2/](/darknet/yolov2/)

## Cite ##

If you use YOLOv3 in your work please cite our paper!

    @article{yolov3,
      title={YOLOv3: An Incremental Improvement},
      author={Redmon, Joseph and Farhadi, Ali},
      journal = {arXiv},
      year={2018}
    }

