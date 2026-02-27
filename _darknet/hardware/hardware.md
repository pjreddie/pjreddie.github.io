---
title: "Hardware Guide: Neural Networks on GPUs"
description: "I've had a number of people ask me what hardware I would recommend for training neural networks for vision applications. Here are some of my thoughts."
layout: darknet
order: 8
permalink: /darknet/hardware-guide/
---

Convolutional neural networks are all the rage in computer vision right now. However, since they are relatively new and the field is moving so fast around them many people are confused about how best to train them. I've had fellow grad students and industry researchers ask me what kind of hardware they should get for personal machines or servers.

NVIDIA would like you to buy their new [DIGITS Devbox](https://developer.nvidia.com/devbox), but priced at $15,000 with a 8-10 week lag time I'm not sure why anyone would want it. For about $6,000 after tax you can build your own 4 GPU box that ships in just a few days from Newegg.

## The Full Build ##

You can find my full build [here](https://secure.newegg.com/Wishlist/SharedWishlistDetail?ID=4bqY9DizYntXlVje%2fC7qLA%3d%3d&&cm_mmc=snc-email-_-sr-_-wishlist-4bqY9DizYntXlVje%2fC7qLA%3d%3d-_-06/13/2018). This is just for the box, you'll also need some GPUs. I got 4 EVGA Titan X's from Amazon.

![full build](build.png)

The base computer is $1,400, add on another $4k for GPUs and you're ready to go! A lot less than $15,000.

## GPUs ##

Probably the most important and most expensive part of your build will be the GPUs, and for good reason. GPUs are more than 100x faster for training and testing neural networks than a CPU. The bulk of our computation will be multiplying big matrices together so we want a card with high single precision performance.

<a target=_blank href="http://www.amazon.com/dp/B00UXTN5P0/ref=twister_B00XR0A96U?_encoding=UTF8&psc=1">![titan x](evga_titan_x_1.png)</a>

### Titan X ###

This is probably what you want. Designed to be NVIDIA's highest-end gaming GPU, they pack almost 7 TFLOPS of processing power for only $1,000 and you can fit 4 of them into a single machine. With 12 GB of VRAM they can run all the big models with plenty of room to spare.

People online seem to think that EVGA and ASUS are about equal for quality but that EVGA has better customer service so I'd get that one. I bought them on Amazon because they were a little bit cheaper than other options and they shipped quickly.

### The Competition: Tesla K40, K80, Other GeForce 900s ###

The K40 and K80 are more geared toward high double precision performance which we don't really care about. Their single precision performance is on par with the Titan X but for a significant markup.

Other high end GTX cards like the 980 and 980 Ti get good single precision performance at a discount compared to the Titan X. However, if you want a lot of processing power it's much simpler to build one machine with 4 Titan X's than 2 machines with 8 980s. Here's the comparison:

- Titan X: $1,000, 6.9 TFLOPS, 6.9 GFLOPS/$
- GTX 980 Ti: $670,  5.6 TFLOPS, 8.4 GFLOPS/$
- GTX 980: $500, 4.1 TFLOPS, 8.2 GFLOPS/$
- Tesla K40: $3,000, 4.3 TFLOPS, 1.4 GFLOPS/$
- Tesla K80: $5,000, 8.7 TFLOPS, 1.7 GFLOPS/$

From an efficiency standpoint the 980 Ti comes out ahead. However, you do make some sacrifices: only 6 GB of VRAM and slower overall performance. If you are strapped for cash I'd say the 980 Ti is a good option. The main takeaway is do not, for any reason, get Teslas. They are crazy expensive for what you get out. If you want overall processing power in one box than Titan X's are the best option.

## Motherboard: GIGABYTE GA-X99-UD3P ##

<a target=_blank href="http://www.newegg.com/Product/Product.aspx?Item=N82E16813128772">![motherboard](mobo.png)</a>

The most important aspect of the motherboard is that it can fit all the cards you want into it. Whatever you pick, make sure it can support 4 cards. In general look for high end gaming motherboards. With enough space for 4 double-wide cards.

## CPU: Intel Core i7-5820K ##

<a target=_blank href="http://www.newegg.com/Product/Product.aspx?Item=N82E16819117402">![cpu](cpu.png)</a>

You don't need a great CPU, but you might as well get one! With 12 effective cores this is a pretty good option.

## Power Supply: Rosewill Hercules ##

<a target=_blank href = "http://www.newegg.com/Product/Product.aspx?Item=N82E16817182251">![power supply](power.png)</a>

Each of our cards will draw around 250 watts and the rest of our computer might need some too. This means we need over a kilowatt of power! To be safe, I'd go with a real monster power supply, like the [Rosewill Hercules 1600W](http://www.newegg.com/Product/Product.aspx?Item=N82E16817182251). It's got a ton of power; the only issue is it's big! I had to take out a fan holder from my case to fit it in.

## Memory: G.SKILL Ripjaws 4 Series 32GB ##

<a target=_blank href="http://www.newegg.com/Product/Product.aspx?Item=N82E16820231796">![memory](mem.png)</a>

Lots of cheap memory. 32 GB should be sufficient, even though it means we have less RAM than VRAM!

## SSD: Mushkin Enhanced Reactor 2.5" 1TB ##

<a target=_blank href="http://www.newegg.com/Product/Product.aspx?Item=N82E16820226596">![ssd](ssd.png)</a>

You could spend a lot of money getting a 9 TB RAID setup like NVIDIA's devbox, or you could just buy one SSD. It's still a good idea to have a backup hard drive, either in the same machine or by syncing your trained models remotely. But this one SSD will be more than sufficient to store and serve your data, even to 4 GPUs simultaneously. I can run 4 of my [fastest Imagenet models](/darknet/imagenet/#reference) simultaneously off of this one disk without any delay in training. That means it's loading around 1,000 images (about 16 MB) per second! Since most models are larger anyway nowadays loading from disk will not be your bottleneck.

The only downside is size. If you are dealing with really large datasets you may want a larger hard drive for storing the data on before you preprocess it.

## Case: Rosewill Thor V2 ##

<a target=_blank href="http://www.newegg.com/Product/Product.aspx?Item=N82E16811147158">![case](case.png)</a>

This probably matters the least but it sure looks cool! You really just need a case large enough to fit all of your components comfortably. 4 GPUs take up a lot of space! You also want something cool. The Rosewill Thor packs 4 fans preinstalled and enough room that you don't have to worry about good cable management which is nice because i'm terrible with cables. It's also very highly reviewed and pretty cheap.
