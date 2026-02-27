---
title: "Train a Classifier on CIFAR-10"
description: "Learn how to train a classifier from scratch in Darknet."
layout: darknet
order: 3
permalink: /darknet/train-cifar/
---

This post will teach you how to train a classifier from scratch in Darknet. We'll play with the [CIFAR-10](https://www.cs.toronto.edu/~kriz/cifar.html) dataset, a 10 class dataset of small images. Let's get started!

## Install Darknet ##

If you don't have installed already, do it:

    git clone https://github.com/pjreddie/darknet
    cd darknet
    make

If it all worked, great! If something went wrong..... um..... try to fix it?

## Get The Data ##

We'll use my [mirror](/projects/cifar-10-dataset-mirror/) of the CIFAR data because we want the pictures in image format. The original dataset comes in a binary format but I want this tutorial to generalize to whatever dataset you want to work with, so we'll do it with images instead.

Let's put the data in the `data/` folder. To do that run:

    cd data
    wget https://data.pjreddie.com/files/cifar.tgz
    tar xzf cifar.tgz

Now let's look at what we have:

    ls cifar

lists two directories with our data, `train` and `test`, and a file with the labels, `labels.txt`. You can look at `labels.txt` if you want and see what kinds of classes we will learn:

    cat cifar/labels.txt

We also need to generate our paths files. These files will hold all the paths to the training and validation (or in this case testing) data. To do that, we'll `cd` into our `cifar` directory, `find` all of the images, and write them to a file, then return to our base `darknet` directory.

    cd cifar
    find `pwd`/train -name \*.png > train.list
    find `pwd`/test -name \*.png > test.list
    cd ../..

## Make A Dataset Config File ##

We have to give Darknet some metadata about CIFAR-10. Using your favorite editor, open up a new file in the `cfg/` directory called `cfg/cifar.data`. In it you should have something like this:

    classes=10
    train  = data/cifar/train.list
    valid  = data/cifar/test.list
    labels = data/cifar/labels.txt
    backup = backup/
    top=2

- `classes=10`: the dataset has 10 different classes
- `train = ...`: where to find the list of training files
- `valid = ...`: where to find the list of validation files
- `labels = ...`: where to find the list of possible classes
- `backup = ...`: where to save backup weight files during training
- `top = 2`: calculate top-n accuracy at test time (in addition to top-1)

## Make A Network Config File! ##

We need a network to train. In your `cfg` directory make another file called `cfg/cifar_small.cfg`. In it put this network:

    [net]
    batch=128
    subdivisions=1
    height=28
    width=28
    channels=3
    max_crop=32
    min_crop=32

    hue=.1
    saturation=.75
    exposure=.75

    learning_rate=0.1
    policy=poly
    power=4
    max_batches = 5000
    momentum=0.9
    decay=0.0005

    [convolutional]
    batch_normalize=1
    filters=32
    size=3
    stride=1
    pad=1
    activation=leaky

    [maxpool]
    size=2
    stride=2

    [convolutional]
    batch_normalize=1
    filters=16
    size=1
    stride=1
    pad=1
    activation=leaky

    [convolutional]
    batch_normalize=1
    filters=64
    size=3
    stride=1
    pad=1
    activation=leaky

    [maxpool]
    size=2
    stride=2

    [convolutional]
    batch_normalize=1
    filters=32
    size=1
    stride=1
    pad=1
    activation=leaky

    [convolutional]
    batch_normalize=1
    filters=128
    size=3
    stride=1
    pad=1
    activation=leaky

    [convolutional]
    batch_normalize=1
    filters=64
    size=1
    stride=1
    pad=1
    activation=leaky

    [convolutional]
    filters=10
    size=1
    stride=1
    pad=1
    activation=leaky

    [avgpool]

    [softmax]

It's a really small network so it won't work very well but it's good for this example. The network is just 4 convolutional layers and 2 maxpooling layers.

The last convolutional layer has 10 filters because we have 10 classes. It outputs a `7 x 7 x 10` image. We just want 10 predictions total so we use an average pooling layer to take the average across the image for each channel. This will give us our 10 predictions. We use a [softmax](https://en.wikipedia.org/wiki/Softmax_function) to convert the predictions into a probability distribution. This layer also calculates our error as cross-entropy loss.

## Train The Model ##

Now we just have to run the training code!

    ./darknet classifier train cfg/cifar.data cfg/cifar_small.cfg

And watch it go!

You are just telling Darknet you want to `train` a `classifier` using the following data and network cfg files. On a CPU training may take an hour or more, even for this small network. If you have a GPU you should enable GPU training by following [these instructions](/darknet/install/#cuda).

### Restarting Training ###

If you stop training you can always restart it using one of the model checkpoints it saves along the way:

    ./darknet classifier train cfg/cifar.data cfg/cifar_small.cfg backup/cifar_small.backup

## Validate The Model ##

Now we have to see how well our model is doing. We can calculate top-1 and top-2 validation accuracy using the `valid` command. We can run validation on a backup, the final weights file, or any saved epoch weight file:

    ./darknet classifier valid cfg/cifar.data cfg/cifar_small.cfg backup/cifar_small.backup

You will see a bunch of scrolling numbers that tell you your accuracy.
