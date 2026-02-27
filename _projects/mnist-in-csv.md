---
title: "MNIST in CSV"
description: "MNIST is a great dataset in awful packaging. Here's a CSV instead of that crazy format they are normally available in. Enjoy!"
layout: projects
order: 1
permalink: /projects/mnist-in-csv/
---

Here's the [train set](https://data.pjreddie.com/files/mnist_train.csv) and [test set](https://data.pjreddie.com/files/mnist_test.csv).

The format is:

    label, pix-11, pix-12, pix-13, ...

where `pix-ij` is the pixel in the `ith` row and `jth` column.

For the curious, this is the script to generate the csv files from the [original data](http://yann.lecun.com/exdb/mnist/).

    def convert(imgf, labelf, outf, n):
        f = open(imgf, "rb")
        o = open(outf, "w")
        l = open(labelf, "rb")

        f.read(16)
        l.read(8)
        images = []

        for i in range(n):
            image = [ord(l.read(1))]
            for j in range(28*28):
                image.append(ord(f.read(1)))
            images.append(image)

        for image in images:
            o.write(",".join(str(pix) for pix in image)+"\n")
        f.close()
        o.close()
        l.close()

    convert("train-images-idx3-ubyte", "train-labels-idx1-ubyte",
            "mnist_train.csv", 60000)
    convert("t10k-images-idx3-ubyte", "t10k-labels-idx1-ubyte",
            "mnist_test.csv", 10000)
