---
title: "Reviews: Real-Time Grasp Detection Using Convolutional Neural Networks"
layout: publications
permalink: /publications/grasp-detection/
---

Joseph Redmon and Anelia Angelova

ICRA 2015

[PDF](/static/papers/grasp_detection_1.pdf) - [arXiv](http://arxiv.org/abs/1412.3128) - [Slides](https://docs.google.com/presentation/d/1Zc9-iR1eVz-zysinwb7bzLGC2no2ZiaD897_14dGbhw/edit?usp=sharing)

---

# Reviewer 2 #

The paper applied convolutional neural network to identify
graspable parts. The paper is overall well organized and
easy to read. The main contribution claimed in the paper is
the 88% accuracy on Cornell Grasp Detection Database at a
13 fps processing rate.

1. The proposed method was compared with the recent works
[1,2], and the authors argued for a significant performance
improvement. However, to the reviewer, the proposed method
is inferior to the latest work from the same group as in
[1,2].

[*] Deep Learning for Detecting Robotic Grasps, Ian Lenz,
Honglak Lee, Ashutosh Saxena. To appear in International
Journal of Robotics Research (IJRR), 2014.
http://pr.cs.cornell.edu/deepgrasping/

In the work [*], the accuracy of baseline methods is almost
over 90%. The reviewer noticed that the cited performance
in Table 1 is different from that in [*]. Are they tested
on different databases? Please clarify.

2. Though real-time performance is one of the great merits,
the method in this paper and that in [*] are essentially
neural networks. So the reviewer considers that this
advantage is just a tradeoff between accuracy and model
complexity.

3. MultiGrasp detection is a contribution of this paper.
There are multiple graspable points on most of the objects
in the database, and the database has multiple labelled
ground-truth grasps. How are the direct regression and
MultiGrasp models evaluated with reference to the database
if the number of output is different?


# Reviewer 4 #

This is a very well written paper that seems to make a
significant contribution to grasping and manipulation by
dramatically increasing the speed and accuracy of grasp
detection.  Below are some specific comments and
suggestions, but overall the paper is interesting and
important to the field.

"we implicitly assume that a good 2-D grasp can be
projected back to 3-D and executed by a robot viewing the
scene"

It would be interesting to further explore and discuss how
the method could be adapted to estimate the full gripper
pose, rather than a 2D projection of the pose.

The description of the 3 methods is clean and precise but
still basically impenetrable by someone with little
experience with CNN's.	Perhaps this is ultimately
unavoidable, but this reader would appreciate some extra
effort to make the descriptions more novice-friendly.  The
following sentences in particular seemed overburdened with
jargon:

"Our network has five convolutional layers followed by
three fully connected layers. The convolutional layers are
interspersed with normalization and maxpooling layers at
various stages."

"For each fold of cross-validation, we train each model for
25 epochs. We use a learning rate of 0.0005 across all
layers and a weight decay of 0.001. In the hidden layers
between fully connected layers we use dropout with a
probability of 0.5 as an added form of regularization."

In the chart in table 1, it is not clear how the numbers
for Jiang and Lenz are obtained.  Are they from the
authors' own implementation of these algorithms? If so, the
details of the implementation undoubtedly affect processing
speed.

This reader would also be interested to know more about the
tradeoffs of using a GPU for grasp detection in practice.
Using a GPU is obviously advantageous for training and
testing in this paper (processing a large batch of
independent images is massively parallel).  But the case of
a real robot seeking to grasp a single nearby object
doesn't seem very parallel.  In this case, it would seem
that the thing that matters is how quickly a single image
can be processed, so a high-speed CPU would be more
appropriate. Perhaps the MultiGrasp algorithm itself has
parallelizable tasks within the processing of one image.
Please clarify if this is the case.

Typos:

"our models outperforms"

This run-on sentence needs a comma: "RGB-D sensors like the
Kinect are cheap and the extra depth information is
invaluable for robots that interact with a 3-D environment"


# Meta-Reviewer #

One of the reviewers commented on the performance of
algorithms in this paper and other two papers of the same
group. I agree that discuss/clarification will make this
paper stronger. The other reviewer also mentioned a few
things that could help to improve the manuscripts.
