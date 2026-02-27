---
title: "Tiny Darknet"
description: "Image classification made tiny."
layout: darknet
order: 4
permalink: /darknet/tiny-darknet/
---

I've heard a lot of people talking about [SqueezeNet](https://arxiv.org/abs/1602.07360).

SqueezeNet is cool but it's JUST optimizing for parameter count. When most high quality images are 10MB or more why do we care if our models are 5 MB or 50 MB? If you want a small model that's actually FAST, why not check out the [Darknet reference network](/darknet/imagenet/#reference)? It's only 28 MB but more importantly, it's only 800 million floating point operations. The original [Alexnet](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf) is 2.3 billion. Darknet is 2.9 times faster and it's small and it's 4% more accurate.

So what about SqueezeNet? Sure the weights are only 4.8 MB but a forward pass is still 2.2 billion operations. Alexnet was a great first pass at classification but we shouldn't be stuck back in the days when networks this bad are also this slow!

But anyway, people are super into SqueezeNet so if you really insist on small networks, use this:

## Tiny Darknet ##
<style>
td:first-child {text-align:left;}
td {text-align:right; padding:0em .5em;}
th {padding:0em .5em;}
table {
margin:0em auto 3em; font-size:1em;
}
@media (max-width: 448px) {
  table {
    font-size: .75em;
  }
.model {
  font-size: .5em;
}
}


</style>

<table>
  <tr>
    <th style="text-align:left;">Model</th>
    <th style="text-align:right;">Top-1</th>
    <th style="text-align:right;">Top-5</th>
    <th style="text-align:right;">Ops</th>
    <th style="text-align:right; ">Size</th>
  </tr>
 <tr>
<td>AlexNet</td>
<td>57.0</td>
<td>80.3</td>
<td>2.27 Bn</td>
<td>238 MB</td>
</tr>
<tr>
<td>Darknet Reference</td>
<td style="font-weight:bold;">61.1</td>
<td style="font-weight:bold;">83.0</td>
<td style="font-weight:bold;">0.81 Bn</td>
<td>28 MB</td>
</tr>
<tr>
<td>SqueezeNet</td>
<td>57.5</td>
<td>80.3</td>
<td>2.17 Bn</td>
<td>4.8 MB</td>
</tr>
<tr>
<td>Tiny Darknet</td>
<td>58.7</td>
<td>81.7</td>
<td>0.98 Bn</td>
<td style="font-weight:bold;">4.0 MB</td>
  </tr>
</table>

The real winner here is clearly the Darknet reference model but if you *insist* on wanting a small model, use Tiny Darknet. Or train your own, it should be easy!

Here's how to use it in Darknet (and also how to install Darknet):

    git clone https://github.com/pjreddie/darknet
    cd darknet
    make
    wget https://data.pjreddie.com/files/tiny.weights
    ./darknet classify cfg/tiny.cfg tiny.weights data/dog.jpg

Hopefully you see something like this:

    data/dog.jpg: Predicted in 0.160994 seconds.
    malamute: 0.167168
    Eskimo dog: 0.065828
    dogsled: 0.063020
    standard schnauzer: 0.051153
    Siberian husky: 0.037506


Here's the config file: [tiny.cfg](https://github.com/pjreddie/darknet/blob/master/cfg/tiny.cfg)


The model is just some 3x3 and 1x1 convolutional layers:


<pre class=model>
layer     filters    size              input                output
    0 conv     16  3 x 3 / 1   224 x 224 x   3   ->   224 x 224 x  16
    1 max          2 x 2 / 2   224 x 224 x  16   ->   112 x 112 x  16
    2 conv     32  3 x 3 / 1   112 x 112 x  16   ->   112 x 112 x  32
    3 max          2 x 2 / 2   112 x 112 x  32   ->    56 x  56 x  32
    4 conv     16  1 x 1 / 1    56 x  56 x  32   ->    56 x  56 x  16
    5 conv    128  3 x 3 / 1    56 x  56 x  16   ->    56 x  56 x 128
    6 conv     16  1 x 1 / 1    56 x  56 x 128   ->    56 x  56 x  16
    7 conv    128  3 x 3 / 1    56 x  56 x  16   ->    56 x  56 x 128
    8 max          2 x 2 / 2    56 x  56 x 128   ->    28 x  28 x 128
    9 conv     32  1 x 1 / 1    28 x  28 x 128   ->    28 x  28 x  32
   10 conv    256  3 x 3 / 1    28 x  28 x  32   ->    28 x  28 x 256
   11 conv     32  1 x 1 / 1    28 x  28 x 256   ->    28 x  28 x  32
   12 conv    256  3 x 3 / 1    28 x  28 x  32   ->    28 x  28 x 256
   13 max          2 x 2 / 2    28 x  28 x 256   ->    14 x  14 x 256
   14 conv     64  1 x 1 / 1    14 x  14 x 256   ->    14 x  14 x  64
   15 conv    512  3 x 3 / 1    14 x  14 x  64   ->    14 x  14 x 512
   16 conv     64  1 x 1 / 1    14 x  14 x 512   ->    14 x  14 x  64
   17 conv    512  3 x 3 / 1    14 x  14 x  64   ->    14 x  14 x 512
   18 conv    128  1 x 1 / 1    14 x  14 x 512   ->    14 x  14 x 128
   19 conv   1000  1 x 1 / 1    14 x  14 x 128   ->    14 x  14 x1000
   20 avg                       14 x  14 x1000   ->  1000
   21 softmax                                        1000
   22 cost                                           1000
</pre>
