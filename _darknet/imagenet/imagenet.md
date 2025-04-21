---
title: ImageNet Classification
description: Classify images with popular models like ResNet and ResNeXt.
layout: darknet
order: 2
permalink: /darknet/imagenet/
---

You can use Darknet to classify images for the 1000-class [ImageNet challenge](http://image-net.org/challenges/LSVRC/2015/index). If you haven't installed Darknet yet, you should [do that first](https://pjreddie.com/darknet/install/).

## <a name="classify"></a>Classifying With Pre-Trained Models ##

Here are the commands to install Darknet, download a classification weights file, and run a classifier on an image:

    git clone https://github.com/pjreddie/darknet.git
    cd darknet
    make
    wget https://data.pjreddie.com/files/darknet19.weights
    ./darknet classifier predict cfg/imagenet1k.data cfg/darknet19.cfg darknet19.weights data/dog.jpg

This example uses the Darknet19 model, you can read more about it [below](#darknet19). After running this command you should see the following output:

    layer     filters    size              input                output
        0 conv     32  3 x 3 / 1   256 x 256 x   3   ->   256 x 256 x  32  0.113 BFLOPs
        1 max          2 x 2 / 2   256 x 256 x  32   ->   128 x 128 x  32
        2 conv     64  3 x 3 / 1   128 x 128 x  32   ->   128 x 128 x  64  0.604 BFLOPs
        3 max          2 x 2 / 2   128 x 128 x  64   ->    64 x  64 x  64
        4 conv    128  3 x 3 / 1    64 x  64 x  64   ->    64 x  64 x 128  0.604 BFLOPs
        5 conv     64  1 x 1 / 1    64 x  64 x 128   ->    64 x  64 x  64  0.067 BFLOPs
        6 conv    128  3 x 3 / 1    64 x  64 x  64   ->    64 x  64 x 128  0.604 BFLOPs
        7 max          2 x 2 / 2    64 x  64 x 128   ->    32 x  32 x 128
        8 conv    256  3 x 3 / 1    32 x  32 x 128   ->    32 x  32 x 256  0.604 BFLOPs
        9 conv    128  1 x 1 / 1    32 x  32 x 256   ->    32 x  32 x 128  0.067 BFLOPs
       10 conv    256  3 x 3 / 1    32 x  32 x 128   ->    32 x  32 x 256  0.604 BFLOPs
       11 max          2 x 2 / 2    32 x  32 x 256   ->    16 x  16 x 256
       12 conv    512  3 x 3 / 1    16 x  16 x 256   ->    16 x  16 x 512  0.604 BFLOPs
       13 conv    256  1 x 1 / 1    16 x  16 x 512   ->    16 x  16 x 256  0.067 BFLOPs
       14 conv    512  3 x 3 / 1    16 x  16 x 256   ->    16 x  16 x 512  0.604 BFLOPs
       15 conv    256  1 x 1 / 1    16 x  16 x 512   ->    16 x  16 x 256  0.067 BFLOPs
       16 conv    512  3 x 3 / 1    16 x  16 x 256   ->    16 x  16 x 512  0.604 BFLOPs
       17 max          2 x 2 / 2    16 x  16 x 512   ->     8 x   8 x 512
       18 conv   1024  3 x 3 / 1     8 x   8 x 512   ->     8 x   8 x1024  0.604 BFLOPs
       19 conv    512  1 x 1 / 1     8 x   8 x1024   ->     8 x   8 x 512  0.067 BFLOPs
       20 conv   1024  3 x 3 / 1     8 x   8 x 512   ->     8 x   8 x1024  0.604 BFLOPs
       21 conv    512  1 x 1 / 1     8 x   8 x1024   ->     8 x   8 x 512  0.067 BFLOPs
       22 conv   1024  3 x 3 / 1     8 x   8 x 512   ->     8 x   8 x1024  0.604 BFLOPs
       23 conv   1000  1 x 1 / 1     8 x   8 x1024   ->     8 x   8 x1000  0.131 BFLOPs
       24 avg                        8 x   8 x1000   ->  1000
       25 softmax                                        1000
    Loading weights from darknet19.weights...Done!
    data/dog.jpg: Predicted in 0.769246 seconds.
    42.55%: malamute
    22.93%: Eskimo dog
    12.51%: Siberian husky
     2.76%: bicycle-built-for-two
     1.20%: mountain bike

Darknet displays information as it loads the config file and weights, then it classifies the image and prints the top-10 classes for the image. Kelp is a mixed breed dog but she has a lot of malamute in her so we'll consider this a success!

You can also try with other images, like the bald eagle image:

    ./darknet classifier predict cfg/imagenet1k.data cfg/darknet19.cfg darknet19.weights data/eagle.jpg

Which produces:

    ...
    data/eagle.jpg: Predicted in 0.707070 seconds.
    84.68%: bald eagle
    11.91%: kite
     2.62%: vulture
     0.08%: great grey owl
     0.07%: hen

Pretty good!

If you don't specify an image file you will be prompted at run-time for an image. This way you can classify multiple in a row without reloading the whole model. Use the command:

    ./darknet classifier predict cfg/imagenet1k.data cfg/darknet19.cfg darknet19.weights

Then you will get a prompt that looks like:

    ....
    25: Softmax Layer: 1000 inputs
    Loading weights from darknet19.weights...Done!
    Enter Image Path:

Whenever you get bored of classifying images you can use `Ctrl-C` to exit the program.

## Validating On ImageNet ##

You see these validation set numbers thrown around everywhere. Maybe you want to double check for yourself how well these models actually work. Let's do it!

First you need to download the validation images, and the cls-loc annotations. You can get them [here](http://image-net.org/download-images) but you'll have to make an account! Once you download everything you should have a directory with `ILSVRC2012_bbox_val_v3.tgz`, and `ILSVRC2012_img_val.tar`. First we unpack them:

    tar -xzf ILSVRC2012_bbox_val_v3.tgz
    mkdir -p imgs && tar xf ILSVRC2012_img_val.tar -C imgs

Now we have the images and the annotations but we need to label the images so Darknet can evaluate its predictions. We do that using this bash [script](https://github.com/pjreddie/darknet/blob/master/scripts/imagenet_label.sh). It's already in your `scripts/` subdirectory. We can just get it again though and run it:

    wget https://data.pjreddie.com/files/imagenet_label.sh
    bash imagenet_label.sh

This will generate two things: a directory called `labelled/` which contains renamed symbolic links to the images, and a file called `inet.val.list` which contains a list of the paths of the labelled images. We need to move this file to the `data/` subdirectory in Darknet:

    mv inet.val.list <path-to>/darknet/data

Now you are finally ready to validate your model! First re-`make` Darknet. Then run the validation routine like so:

    ./darknet classifier valid cfg/imagenet1k.data cfg/darknet19.cfg darknet19.weights

Note: if you don't compile Darknet with [OpenCV](https://pjreddie.com/darknet/install/#opencv) then you won't be able to load all of the ImageNet images since some of them are weird formats not supported by [`stb_image.h`](https://github.com/nothings/stb/blob/master/stb_image.h).

If you don't compile with [CUDA](https://pjreddie.com/darknet/install/#cuda) you can still validate on ImageNet but it will take like a reallllllly long time. Not recommended.


## <a name="pretrained"></a>Pre-Trained Models ##

Here are a variety of pre-trained models for ImageNet classification. Accuracy is measured as single-crop validation accuracy on ImageNet. GPU timing is measured on a Titan X, CPU timing on an Intel i7-4790K (4 GHz) run on a single core. Using multi-threading with `OPENMP` should scale linearly with # of CPUs.


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

<table id="compare">

  <thead>
    <th style="text-align:left;">Model</th>
    <th style="text-align:right;">Top-1</th> 
    <th style="text-align:right;">Top-5</th>
    <th style="text-align:right;">Ops</th>
    <th style="text-align:right; ">GPU</th>
    <th style="text-align:right; ">CPU</th>
    <th style="text-align:right; ">Cfg</th>
    <th style="text-align:right; ">Weights</th>
  </thead>

 <tr>
<td><a href="#alexnet">AlexNet</a></td>
<td>57.0</td> 
<td>80.3</td>
<td>2.27 Bn</td>
<td>3.1 ms</td>
<td>0.29 s</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/alexnet.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/alexnet.weights">238 MB</a></td>
</tr>

 <tr>
<td><a href="#reference">Darknet Reference</a></td>
<td>61.1</td> 
<td>83.0</td>
<td style="font-weight:bold;">0.96 Bn</td>
<td style="font-weight:bold;">2.9 ms</td>
<td style="font-weight:bold;">0.14 s</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/darknet.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/darknet.weights">28 MB</a></td>
</tr>

 <tr>
<td><a href="#vgg">VGG-16</a></td>
<td>70.5</td> 
<td>90.0</td>
<td>30.94 Bn</td>
<td>9.4 ms</td>
<td>4.36 s</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/vgg-16.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/vgg-16.weights">528 MB</a></td>
</tr>

 <tr>
<td><a href="#extraction">Extraction</a></td>
<td>72.5</td> 
<td>90.8</td>
<td>8.52 Bn</td>
<td>4.8 ms</td>
<td>0.97 s</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/extraction.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/extraction.weights">90 MB</a></td>
</tr>

 <tr>
<td><a href="#darknet19">Darknet19</a></td>
<td>72.9</td> 
<td>91.2</td>
<td>7.29 Bn</td>
<td>6.2 ms</td>
<td>0.87 s</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/darknet19.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/darknet19.weights">80 MB</a></td>
</tr>

 <tr>
<td><a href="#darknet19_448">Darknet19 448x448</a></td>
<td>76.4</td> 
<td>93.5</td>
<td>22.33 Bn</td>
<td>11.0 ms</td>
<td>2.96 s</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/darknet19_448.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/darknet19_448.weights">80 MB</a></td>
</tr>

 <tr>
<td><a href="#resnet18">Resnet 18</a></td>
<td>70.7</td> 
<td>89.9</td>
<td>4.69 Bn</td>
<td>4.6 ms</td>
<td>0.57 s</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/resnet18.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/resnet18.weights">44 MB</a></td>
</tr>

 <tr>
<td><a href="#resnet34">Resnet 34</a></td>
<td>72.4</td> 
<td>91.1</td>
<td>9.52 Bn</td>
<td>7.1 ms</td>
<td>1.11 s</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/resnet34.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/resnet34.weights">83 MB</a></td>
</tr>

 <tr>
<td><a href="#resnet50">Resnet 50</a></td>
<td>75.8</td> 
<td>92.9</td>
<td>9.74 Bn</td>
<td>11.4 ms</td>
<td>1.13 s</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/resnet50.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/resnet50.weights">87 MB</a></td>
</tr>


 <tr>
<td><a href="#resnet101">Resnet 101</a></td>
<td>77.1</td> 
<td>93.7</td>
<td>19.70 Bn</td>
<td>20.0 ms</td>
<td>2.23 s</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/resnet101.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/resnet101.weights">160 MB</a></td>
</tr>

 <tr>
<td><a href="#resnet152">Resnet 152</a></td>
<td>77.6</td> 
<td>93.8</td>
<td>29.39 Bn</td>
<td>28.6 ms</td>
<td>3.31 s</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/resnet152.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/resnet152.weights">220 MB</a></td>
</tr>

 <tr>
<td><a href="#resnext50">ResNeXt 50</a></td>
<td>77.8</td> 
<td>94.2</td>
<td>10.11 Bn</td>
<td>24.2 ms</td>
<td>1.20 s</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/resnext50.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/resnext50.weights">220 MB</a></td>
</tr>

 <tr>
<td><a href="#resnext101">ResNeXt 101 (32x4d)</a></td>
<td>77.7</td> 
<td>94.1</td>
<td>18.92 Bn</td>
<td>58.7 ms</td>
<td>2.24 s</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/resnext101-32x4d.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/resnext101-32x4d.weights">159 MB</a></td>
</tr>

 <tr>
<td><a href="#resnext152">ResNeXt 152 (32x4d)</a></td>
<td>77.6</td> 
<td>94.1</td>
<td>28.20 Bn</td>
<td>73.8 ms</td>
<td>3.31 s</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/resnext152-32x4d.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/resnext152-32x4d.weights">217 MB</a></td>
</tr>

 <tr>
<td><a href="#densenet201">Densenet 201</a></td>
<td>77.0</td> 
<td>93.7</td>
<td>10.85 Bn</td>
<td>32.6 ms</td>
<td>1.38 s</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/densenet201.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/densenet201.weights">66 MB</a></td>
</tr>

<tr>
<td><a href="#darknet53">Darknet53</a></td>
<td>77.2</td> 
<td>93.8</td>
<td>18.57 Bn</td>
<td>13.7 ms</td>
<td>2.11 s</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/darknet53.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/darknet53.weights">159 MB</a></td>
</tr>

<tr>
<td><a href="#darknet53_448">Darknet53 448x448</a></td>
<td style="font-weight:bold;">78.5</td> 
<td style="font-weight:bold;">94.7</td>
<td>56.87 Bn</td>
<td>26.3 ms</td>
<td>7.21 s</td>
<td><a href="https://github.com/pjreddie/darknet/blob/master/cfg/darknet53_448.cfg">cfg</a></td>
<td><a href="https://data.pjreddie.com/files/darknet53_448.weights">159 MB</a></td>
</tr>
</table>

<link rel="stylesheet" href="/static/css/tablesort.css">

<script src='/static/js/tablesort.js'></script>

<!-- Include sort types you need -->
<script src='/static/tablesort.number.js'></script>
<script src='/static/tablesort.filesize.js'></script>

<script>
  new Tablesort(document.getElementById('compare'));
</script>

<style>
table{
    border-collapse: collapse;
}
th{
    border-bottom:2px solid #0ff;
    border-collapse: collapse;
}
th[role=columnheader]:not(.no-sort) {
	cursor: pointer;
}

th[role=columnheader]:not(.no-sort):after {
	content: '';
	float: right;
	margin-top: 7px;
	border-width: 0 4px 4px;
	border-style: solid;
	border-color: #0ff transparent;
	visibility: hidden;
	opacity: 0;
	-ms-user-select: none;
	-webkit-user-select: none;
	-moz-user-select: none;
	user-select: none;
}

th[aria-sort=ascending]:not(.no-sort):after {
	border-bottom: none;
	border-width: 4px 4px 0;
}

th[aria-sort]:not(.no-sort):after {
	visibility: visible;
	opacity: 0.7;
}

th[role=columnheader]:not(.no-sort):hover:after {
	visibility: visible;
	opacity: 1;
}
</style>

### <a name="alexnet"></a>AlexNet ###

The model that started a revolution! The [original](http://www.cs.toronto.edu/~fritz/absps/imagenet.pdf) model was crazy with the split GPU thing so this is the model from some [follow-up work](http://arxiv.org/abs/1404.5997).

- Top-1 Accuracy: 57.0%
- Top-5 Accuracy: 80.3%
- Forward Timing: 3.1 ms/img
- CPU Forward Timing: 0.29 s/img
- [cfg file](https://github.com/pjreddie/darknet/blob/master/cfg/alexnet.cfg)
- [weight file (238 MB)](http://data.pjreddie.com/files/alexnet.weights)

### <a name="reference"></a>Darknet Reference Model ###

This model is designed to be small but powerful. It attains the same top-1 and top-5 performance as AlexNet but with 1/10th the parameters. It uses mostly convolutional layers without the large fully connected layers at the end. It is about twice as fast as AlexNet on CPU making it more suitable for some vision applications.

- Top-1 Accuracy: 61.1%
- Top-5 Accuracy: 83.0%
- Forward Timing: 2.9 ms/img
- CPU Forward Timing: 0.14 s/img
- [cfg file](https://github.com/pjreddie/darknet/blob/master/cfg/darknet.cfg)
- [weight file (28 MB)](http://data.pjreddie.com/files/darknet.weights)

### <a name="vgg"></a>VGG-16 ###

The [Visual Geometry Group](http://www.robots.ox.ac.uk/~vgg/research/very_deep/) at Oxford developed the VGG-16 model for the ILSVRC-2014 competition. It is highly accurate and widely used for classification and detection. I adapted this version from the [Caffe](http://caffe.berkeleyvision.org/) pre-trained [model](https://gist.github.com/ksimonyan/211839e770f7b538e2d8#file-readme-md). It was trained for an additional 6 epochs to adjust to Darknet-specific image preprocessing (instead of mean subtraction Darknet adjusts images to fall between -1 and 1).

- Top-1 Accuracy: 70.5%
- Top-5 Accuracy: 90.0%
- Forward Timing: 9.4 ms/img
- CPU Forward Timing: 4.36 s/img
- [cfg file](https://github.com/pjreddie/darknet/blob/master/cfg/vgg-16.cfg)
- [weight file (528 MB)](http://data.pjreddie.com/files/vgg-16.weights)

### Extraction<a name="extraction"></a> ###

I developed this model as an offshoot of the [GoogleNet model](http://arxiv.org/abs/1409.4842). It doesn't use the "inception" modules, only 1x1 and 3x3 convolutional layers.

- Top-1 Accuracy: 72.5%
- Top-5 Accuracy: 90.8%
- Forward Timing: 4.8 ms/img
- CPU Forward Timing: 0.97 s/img
- [cfg file](https://github.com/pjreddie/darknet/blob/master/cfg/extraction.cfg)
- [weight file (90 MB)](http://data.pjreddie.com/files/extraction.weights)

### Darknet19<a name="Darknet19"></a> ###

I modified the Extraction network to be faster and more accurate. This network was sort of a merging of ideas from the Darknet Reference network and Extraction as well as numerous publications like Network In Network, Inception, and Batch Normalization.

- Top-1 Accuracy: 72.9%
- Top-5 Accuracy: 91.2%
- Forward Timing: 6.2 ms/img
- CPU Forward Timing: 0.87 s/img
- [cfg file](https://github.com/pjreddie/darknet/blob/master/cfg/darknet19.cfg)
- [weight file (80 MB)](http://data.pjreddie.com/files/darknet19.weights)

### Darknet19 448x448<a name="darknet19_448"></a> ###

I trained Darknet19 for 10 more epochs with a larger input image size, `448x448`. This model performs significantly better but is slower since the whole image is larger.

- Top-1 Accuracy: 76.4%
- Top-5 Accuracy: 93.5%
- Forward Timing: 11.0 ms/img
- CPU Forward Timing: 2.96 s/img
- [cfg file](https://github.com/pjreddie/darknet/blob/master/cfg/darknet19_448.cfg)
- [weight file (80 MB)](http://data.pjreddie.com/files/darknet19_448.weights)


### Resnet 50<a name="resnet50"></a> ###

For some reason people love these networks even though they are so sloooooow. Whatever. [Paper](https://arxiv.org/abs/1512.03385)

- Top-1 Accuracy: 75.8%
- Top-5 Accuracy: 92.9%
- Forward Timing: 11.4 ms/img
- CPU Forward Timing: 1.13 s/img
- [cfg file](https://github.com/pjreddie/darknet/blob/master/cfg/resnet50.cfg)
- [weight file (87 MB)](https://data.pjreddie.com/files/resnet50.weights)

### Resnet 152<a name="resnet152"></a> ###

For some reason people love these networks even though they are so sloooooow. Whatever. [Paper](https://arxiv.org/abs/1512.03385)

- Top-1 Accuracy: 77.6%
- Top-5 Accuracy: 93.8%
- Forward Timing: 28.6 ms/img
- CPU Forward Timing: 3.31 s/img
- [cfg file](https://github.com/pjreddie/darknet/blob/master/cfg/resnet152.cfg)
- [weight file (220 MB)](https://data.pjreddie.com/files/resnet152.weights)


### Densenet 201<a name="densenet201"></a> ###

I love DenseNets! They are just so deep and so crazy and work so well. Like Resnet, still slow since they are sooooo many layers but at least they work really well! [Paper](https://arxiv.org/abs/1608.06993)

- Top-1 Accuracy: 77.0%
- Top-5 Accuracy: 93.7%
- Forward Timing: 32.6 ms/img
- CPU Forward Timing: 1.38 s/img
- [cfg file](https://github.com/pjreddie/darknet/blob/master/cfg/densenet201.cfg)
- [weight file (67 MB)](https://data.pjreddie.com/files/densenet201.weights)
