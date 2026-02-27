---
title: "Reviews: You Only Look Once: Unified, Real-Time Object Detection"
layout: publications
permalink: /publications/yolo/
---

Joseph Redmon, Santosh Divvala, Ross Girshick, and Ali Farhadi

CVPR 2016, OpenCV People's Choice Award

[PDF](/static/papers/yolo_1.pdf) - [arXiv](http://arxiv.org/abs/1506.02640) - [Slides](https://docs.google.com/presentation/d/1kAa7NOamBt4calBU9iHgT8a86RRHz9Yz2oh4-GTdX6M/edit?usp=sharing) - [Talk](https://www.youtube.com/watch?v=NM6lrxy0bxs)

---

# NIPS #

## Assigned\_Reviewer\_1 ##

### Quality Score - Does the paper deserves to be published? ###

4: An OK paper, but not good enough

### Confidence ###

4: Reviewer is confident but not absolutely certain

### Please summarize your review in 1-2 sentences ###

This paper proposes an object detection algorithm consisting in training a Deep CNN as a regressor predicting, for each class / position, the precise position of object candidates in images. I have 2 main concerns regarding this paper: i) the lack of positioning with respect to the related works ii) the experimental validation is not fully convincing.

### Comments to author(s). First provide a summary of the paper, and then address the following criteria: Quality, clarity, originality and significance. ###

This paper proposes an object detection algorithm consisting in training a Deep CNN as a regressor predicting, for each class and position range, the position of object candidates in images. The output of the network is a 7x7x(20+4) tensor (7x7 spatial bins, 20 classes, 4 corners of the per class/bin bounding box). The proposed architecture is fast (45 fps). The algorithm, by itself does not perform particularly well but can be used to boost the performance of R-CNN (+2/3% of map on pascal voc 2007).

On overall, the paper reads well, even if some terms such as IOU (I guess it's the abbreviation of intersection over union but it would be better to say it as it's not a standard abbreviation) or "background errors" (I'm not really sure of the meaning of this expression. Are they False Positive? If yes, it should be better to use False Positive instead).

Originality is what bother me the most. Using Deep CNNs as regressors for predicting object localization in images has already been done in the past. I'm not saying that the submission has no novelty, but the difference with [20] seems small and one could expect a clarification regarding the novelty. Surprisingly, [20] is not mentioned in the introduction and, despite the strong similarity, the papers simply refer to it as the closest method without explaining to which extend both differ. [8] also has involves a similar type of regression, as well as [10]. This is not mentioned in the submission, nor discuss.

Another (much smaller) concern is the experimental validation. The performance of the proposed method is not as good as the top performing ones (much more localization errors resulting in a mAP 7% lower than top competitors) but the claim is that it can be counter balanced by either i) it speed ii) its combination with Fast R-CNN. If speed is the objective, it would be necessary to show that methods such as region proposal + R-CNN are not as good & fast as the proposed method, or, more generally, to comment on other fast object detectors. If performance is the objective, the proposed method is not as good as s.o.t.a such as [8] on Pascal 2007. The performance of the combination on pascal 2012 is unfortunately not given.

In conclusion, the paper is certainly interesting but the idea of using regression for detecting object with CNN is not novel (eg. [20, 8, 10]) and the lack of positioning with respect to these approaches strongly weaken the paper. There might be novelties, but they are not stated. Regarding the experimental evaluation, the strong point for the proposed method is speed, but the paper is not really oriented toward that direction and, for example, does provide any comparisons with fast detectors.

## Assigned\_Reviewer\_3 ##

### Quality Score - Does the paper deserves to be published? ###

5: Marginally below the acceptance threshold

### Confidence ###

4: Reviewer is confident but not absolutely certain

### Please summarize your review in 1-2 sentences ###

Overall, this paper offers a different way to do object class detection with CNNs, which is substantially faster than recent state-of-the-art solutions. However, the work also has several downsides, e.g. (a) novelty is considerably reduced by the very similar architecture of [20], which already presents the same key idea of this paper; (b) lower detection performance than the nearest competitor Fast R-CNN, especially on small objects; (c) imperfect positioning with respect to previous work; (d) personally I find the overall concept of reducing detection to predicting a bounding-box for each cell in a 7x7 grid to be somewhat unappealing.

### Comments to author(s). First provide a summary of the paper, and then address the following criteria: Quality, clarity, originality and significance. ###

#### Summary / originality ####

The authors present a technique for real-time object class detection with a Convolutional Neural Network (CNN). In contrast to state-of-the-art systems such as R-CNN, the proposed technique does not rely on region proposals and instead detects objects with a single pass through the CNN. The main contribution is the idea of dividing the image into a 7x7 grid, and having each cell predict a distribution over class labels as well as a bounding-box for the object whose center falls into it. In this manner, multiple objects can be detected in the same image, without region proposals and also without resorting to simplistic regression from the whole image to a bounding-box. As a result, the method is much much faster than R-CNN, and also faster than the very recent Fast R-CNN, as the remaining bottleneck of extracting region proposals has been removed.

Unfortunately the key idea of this paper has already been proposed by [20], which also divides the image into a regular grid and predicts a bounding-box for each, in a single CNN pass. This unfortunately reduces this paper to be a re-engineering version of [20] to do well on PASCAL VOC. It is unclear where this is sufficient for NIPS. I leave it to the Area Chair to decide.

On the bright side, this paper does offer a practical advantage in terms of speed over modern state-of-the-art systems such as R-CNN. On the downside, compared to its closest competitor (Fast R-CNN), it performs considerably worse (-14% MAP). It is particularly worse on small objects, which are exactly where the most interesting bit of object detection lies.

Personally I find the main idea of the paper to be somewhat unsatisfying: by reducing object detection to predicting bounding-boxes from 49 regularly spaced cells, the whole idea of visual search falls out of the window. Moreover, as the authors admit in all fairness, it becomes impossible to detect several small nearby objects, as at most one object per grid cell can be detected. This might be suitable with a 7x7 grid on PASCAL VOC, but what about more cluttered datasets such as SUN 2012? The authors might try with a higher resolution grid, but at some point there won't be enough training data to get it to work (as opposed to region + classifier architectures which are translation and scale invariant). The considerably worse performance on small objects in PASCAL VOC (sec. 4.1) further raises concerns on this point.

Perhaps the main downside in the line of argument by the authors is that, with the advent of Fast R-CNN, the only significant computational advantage left to gain is removing the region proposal computation. This brings a gain when detecting a few classes, but when detecting many (e.g. 10k) the cost of region proposal extraction stays constant, while the cost of classification (in both models) increases. Hence, it's unclear if the advantage will remain in that scenario. Moreover, faster region proposal algorithms than Selective Search do exist (e.g. BING), so again it's unclear how much that factor really matters.

Finally, the authors also propose to combine Fast R-CNN with YOLO, bringing +2% mAP (at the price of no speed-up at all). I suspect that the same 2% could have been gained by applying standard global context re-scoring to the bounding-boxes output by Fast R-CNN (see below for citations). No baseline of this kind is offered.


#### Quality / clarity / references ####

The paper is well written and enjoyable to read. Several nice figures accompany the explanations, making them even clearer. One criticism regards references. As several points the authors could have put their work in the context of existing literature better:

- abstract: it is not really true that 45 frames/second is 'hundred to thousands of times faster than existing detection systems'. For example, Viola-Jones CVPR 2001 already had detectors in real-time (14 years ago!!). Dollar-Perona BMVC 2010 was real-time too. The comment by the authors probably refers to recent state-of-the-art systems based on CNNs and region proposal. But that is a less general statement than what they made.

- introduction: the authors correctly argue about the benefits of global image context in object detection, but they do not provide any citations to show that this is fact an old idea, and a phenomenon that was shown and exploited for object detection many times (e.g. Russell NIPS 2007, Harzallah and Schmid ICCV 2009, and most top entries in the PASCAL VOC detection challenges). In fact there is no discussion of related work on context anywhere in the paper.

- lines 200-202 the idea of predicting the objectness of a grid cell is very related to the 'objectness' work of Alexe et al CVPR 2010 / PAMI 2012 (the authors here use that word too). It should be cited.

- line 268: the authors say here that the proposed system performs NMS all concurrently inside the network itself. Instead in in sec. 2.3 they say NMS is a separate processing stage. I believe the latter is true and the former should be rephrased.

- line 298: from what I understand of the model, it is false that 'each grid cell proposes a potential bounding-box and then scores it using convolutional features'. In fact the proposed system directly outputs the class predictions (scores) and bounding-box for a cell, and those scores are not based on features inside the box, they are based on anything the CNN wants.

- line 314: the authors need to expand on [20], which is only stated as the 'most similar design', without any further description/discussion. This is a problem, as upon inspection I found that work to offer the same idea, for the same purpose, but in another kind of dataset.


Finally, some elements in the work are very ad-hoc and reduce its appeal:

- sec 2.2.2: training to predict IoU is added in an arbitrary manner, and only applied to the second stage of training (!)

- inference applies NMS as post-processing, which goes against the claim early in the paper that the technique is just a single pass over a CNN with no pre- nor post-processing.

- line 137: 24 convolutional layers is a lot, even for modern standards. No explanation nor experimental exploration is offered.

- sec. 4.4: this way of combining Fast R-CNN with YOLO is very ad-hoc.


## Assigned\_Reviewer\_4 ##

### Quality Score - Does the paper deserves to be published? ###

7: Good paper, accept

### Confidence ###

4: Reviewer is confident but not absolutely certain

### Please summarize your review in 1-2 sentences ###

A novel deep learning approach is presented for object detection. The idea is to remove object proposal generation and regress a grid based object representation for an image. The idea is novel, simple, and interesting, so I like it.

### Comments to author(s). First provide a summary of the paper, and then address the following criteria: Quality, clarity, originality and significance. ###

Many recent object detection systems rely on object proposals. This paper suggests dividing an image into grid cells and regressing the probability of objects falling into cells and also regressing the bounding box of each object. Speed is the main advantage since the network processes the image only once and detects the objects. The results are very good. The accuracy is less than state-of-the-art, but it is much faster in inference.

The idea is very simple and interesting. NIPS audience is interested in fast object detection algorithms. Also, this paper shows that by combining the results with fast RCNN, it outperforms state-of-the-art.

The main limitation is that it cannot detect many objects close to each other which might be ok in many applications. Moreover, it has difficulty in detecting small objects due to the grid size. I think adding more analysis on the gird size can be helpful for better understanding this algorithm. I know training the network is very expensive, but that would be great to see the effect of grid size in the results. The upper bound recall given the current grid size on PASCAL VOC is 93%. That would be great to see this number as a function of grid size.

There are many heuristics in training the network like choosing a particular scheduler for learning rate, switching between pre-training and fine-tuning, and so on. I think it is important to see how sensitive is the algorithm to these tricks. For instance, that would be great to report the results using a simpler set of parameters.

I did't understand why Eq 2 uses square root. Is it possible to use a form of normalization to solve the problem of large and small boxes? Also, it uses Euclidean loss for probabilities. Is this the best choice?

Overall, even though the results are still far from state-of-the-art, I think this paper is on the right track and can have a good impact.


## Assigned\_Reviewer\_5 ##

### Quality Score - Does the paper deserves to be published? ###

7: Good paper, accept

### Confidence ###

3: Reviewer is fairly confident

### Please summarize your review in 1-2 sentences ###

This paper proposed a new pipeline for object detection, YOLO. Unlike classifier-based approaches, YOLO is trained on a loss function that directly corresponds to detection performance and every step of the pipeline can be trained jointly. Although the YOLO by itself does not achieve the best performance, fusing it with Fast R-CNN does improve the performance. Overall this approach is interesting and the paper is presented well. One note is that YOLO has the main advantage in its efficiency. However, when fusing with Fast R-CNN, does the fused one still has the efficiency advantage? The table 2 should add one more row on this fused one.


## Assigned\_Reviewer\_6 ##

### Quality Score - Does the paper deserves to be published? ###

8: Top 50% of accepted NIPS papers

### Confidence ###

4: Reviewer is confident but not absolutely certain

### Please summarize your review in 1-2 sentences ###

This is a well-written paper. The proposed YOLO method has a clear advantage in speed to existing methods (including FR-CNN) and the detection results are fine. Motivations, methods, and results all make sense.

---------------------------------------

Although personally I am not a fan of end-to-end, the end-to-end argument in the authors' rebuttal does provide a a strong argument that this paper does has novel contribution to the community. Hence, I feel that the main concern of R1 and R2 are answered by the reviewer, and would like to keep my original recommendation.


## Assigned\_Reviewer\_7 ##

### Quality Score - Does the paper deserves to be published? ###

6: Marginally above the acceptance threshold

### Confidence ###

5: Reviewer is absolutely certain

### Please summarize your review in 1-2 sentences ###

This paper proposes an alternative approach to the object detection problem. The proposed method, called YOLO, uses a single convolutional network to simultaneously predict multiple bounding boxes and class probabilities. The idea is straightforward and the speed of the resulting system is much faster (at least 93x speedup), compared to previous systems. However, the drawbacks of the paper are obvious. It performs poor on small-sized objects and trends to give imprecise object locations.


## Rebuttal: ##

We thank the reviewers for their insightful comments and encouraging remarks.

To alleviate concerns on originality, we emphasize that YOLO is among the first to model object detection as an end-to-end, unified process. This is valuable as it:
1. jointly optimizes a loss function corresponding to detection (others, e.g. R-CNN and SPPnet, optimize classification and localization as disjoint pieces of a pipeline)
2. makes far fewer background mistakes
3. processes images at unprecedented speeds for its accuracy

### [R1,R3] Novelty wrt [20] ###
Yes, [20] & YOLO are similar in that they both use a 7x7 prediction grid. However the similarity ends there. YOLO deals with a totally different domain (object detection vs. grasp localization), which engenders the following contributions:
1. YOLO's formulation involves joint optimization of the classification & localization error, unlike [20]. Note that in [20], the goal was to find a single graspable area for an object. It did not need to know anything about the object extent or its center. Thus, it was a substantially easier problem as the network there just found solid edges (or handles) that were graspable on the object without having any notion of its full extent.
2. YOLO's parameterization of class probabilities is essential for training to converge & was non-existent in [20]. ([20] had no notion of classes.)
3. YOLO predicts bounding boxes & class probabilities for multiple objects of multiple classes in cluttered images. Thus, YOLO's design has several differences (larger network, larger input image, different parameterization, etc). Note than in [20], the task was highly constrained: 1 object/image on a clean white background.

We apologize for not highlighting these contributions in our draft, which unfortunately lead to YOLO being perceived as a 're-engineering' of [20].

### [R1] Novelty wrt [8,10] ###
[8,10] use selective search for generating region proposals. Their 'bounding box' regression step only predicts deviations from the selective search boxes. It cannot predict full bounding boxes, only adjust them.

### [R3] 2% gain from RCNN+YOLO vs context rescoring ###
The benefit from the combination is not context rescoring, but the elimination of false background detections. YOLO makes far fewer background detections so it suppresses RCNN's false detections (Fig 4). Any benefit from context rescoring would be orthogonal.

### [R3,R4] Grid size & recall analysis needed ###
As suggested, we experimented with different grid sizes on VOC2007 (mAP):
5x5: 53.3%
7x7: 58.8%
9x9: 56.8%.

We also calculated the max (oracle) recall for different grid sizes:
1x1 (localization):41.2%
3x3: 76.4%
5x5: 88.0%
7x7: 93.1%
9x9: 95.9%
11x11: 97.3%
14x14: 98.0%

### [R4] Analyze sensitivity to parameters ###
As suggested, we analyzed our training regimen (mAP):
Single learning rate, no IOU training: 48.7%
+multiple rates: 55.9%
+IOU training: 58.8%

### [R4] Explain square root & euclidean loss choices ###
We empirically observed (on validation set) that square root was the easiest to normalize the loss between large & small boxes. We tried optimizing IoU directly but achieved worse results (~10% mAP). We tried using softmax for classes early on but it was not effective as Euclidean loss (~5-10% mAP). We have not experimented with softmax on the most recent version of YOLO.

### [R1] Show R-CNN is not fast, and compare with fast detectors ###
Please see Table 2 for the speed comparisons.

### [R1] Performance of combination on 2012 ###
Please see Table 1 (L416)

### [R1] Performance not as good as [8] ###
Our contribution is not a single performance number. We are trying to explore and demonstrate to the vision community that there are alternatives to sliding-window or region proposal methods for object detection. Nonetheless, our fusion strategy (sect 4.4) should give a performance boost to any system using region proposals, including [8].

### [R1] Are background-errors false positives? ###
Yes

### [R3] BING is fast, so advantage is unclear ###
Yes, but BING only has high recall at .5 IOU and drops off significantly at higher IOUs. This makes it less effective (see http://arxiv.org/pdf/1502.05082v3.pdf Table 4), and less usable (none of the top performing methods use it).

### [R3] "Visual search falls out" ###
Agreed. But visual search has weaknesses. An accurate classifier when applied thousands of times per image produces many false positives. YOLO has far fewer background false positives.

### [R3] Explain 24 layers ###
Inspired by GoogleNet which is 22 layers (L135), we added 4 convolutional layers following the work of [21] (L150).

### [R3] L268, L298, Viola-Jones comment ###
Special purpose detectors (like the ones mentioned, faces and pedestrians) can be heavily optimized because there is less variability. YOLO is the fastest general object detector, especially considering our accuracy. We will rephrase to clarify this, and also include the missing citations (Alexe, Russell, Harzallah). Thanks.


## Meta\_Reviewer\_1 ##

### Meta-Review ###

This paper proposes a new object detection algorithm with Deep CNNs.

### Summary review -- [this is where you explain the final decision to the authors] ###

The paper got mixed reviews. Some reviewers liked the paper, some voted for rejection, which made this manuscript a borderline paper.

Since two of the long reviews raised some important concerns about the paper, and this year we got many very good submissions, therefore I cannot recommend this paper for publication at NIPS.

The reviewers, however, agreed that this is an interesting research direction, and I do encourage the authors to continue this work. The reviewers' suggestions might help improve the revised version of this paper.
