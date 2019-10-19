---
title: "Single Image Depth Estimation using PyTorch"
categories:
  - post
tags:
  - tech

layout: single
---

There are a few ways to measure distances to objects: LIDAR, stereo vision,
ultrasonic sensors, and so on. What about just using a single RGB image and
estimate the depth of each pixel, using a convolution neural network (CNN)?
There has been many publications on this topic, such as [1,2,3]. Here, together
with a friend, I implemented the algorithm of [2], trained it from scratch and
tested it, using PyTorch. In addition, we trained

The code is in my GitHub repository: <a
href="https://github.com/YifanJiangPolyU/single_image_depth_pytorch">https://github.com/YifanJiangPolyU/single_image_depth_pytorch</a>.

Although [2] estimates both per-pixel depth and surface normals, we only trained
depth estimation here.

This work is done in collaboration with <a
href="https://github.com/tongguanc">Guanchun Tong</a>

## The Neural Network

This is a multi-scale CNN model that estimates the depth of each pixel in a RGB
input image.

The model consists of 3 different scales. Each scale is a set of convolution
and pooling layers that processes a scaled-down copy of the original input
image. The input image (304x228) is first processed by the scale 1 and 2,
creating a small-size estimation (74x55). Then, the small-sized estimation is
concatenated with the scale-down version of the input image, and is further
processed by scale 3, creating an estimation of a larger size (147x109). The
structure is exactly the same as [2], and is illustrated in figure below:

<figure style="width: 800px" class="align-center">
  <img src="/images/2018-06-19-single-image-depth/model.png">
  <figcaption>Fig 1. Model Structure </figcaption>
</figure>

## Experiments

### Dataset

The NYUDepth v2 dataset [4] is used to train this model. Unfortunately, due to
disk space limitations, the entire dataset could not be used for training.
Instead, a densely labeled subset of NYUDepth v2 is used (containing 1499
images). The dataset is available <a
href="https://cs.nyu.edu/~silberman/datasets/nyu_depth_v2.html">here</a>.

95% of the images in the densely labeled subset of NYUDepth v2 are used for
training, and the remaining 5% are used for validation.

### Loss Functions

The loss functions used for training are taken from [1].

### Training

The training takes place in a few stages. First, scale 1 and 2 are trained as follows:

<figure style="width: 200px" class="align-center">
  <img src="/images/2018-06-19-single-image-depth/flow-chart-training.png">
  <figcaption>Fig 2. Training Flowchart, Scale 1&2 </figcaption>
</figure>

Then, scale 3 is trained. The depth loss described earlier is simply
back-propagated. Scale 3 is trained for at most 100 epochs, and the training is
terminated early if performance stops improving.

## Results

The resulting accuracy is worse compared to [2], which is to be expected since
the training dataset is much smaller. Baselines of comparisons are detailed in
[1]. The results are shown below:

<figure style="width: 600px" class="align-center">
  <img src="/images/2018-06-19-single-image-depth/table-res.png">
  <figcaption>Fig 3. Depth Prediction Results </figcaption>
</figure>

Results for few sample images are compared below. Our model captures large-scale
geometries correctly, but almost all details are lost. In comparison, [2]
achieves good estimation even for fine details.

<figure style="width: 800px" class="align-center">
  <img src="/images/2018-06-19-single-image-depth/compare-depth.png">
  <figcaption>Fig 4. From left to right: Input RGB image, ours, Eigen[2], ground truth </figcaption>
</figure>

Histograms below show the distribution of per-pixel depth estimation errors. In
general, [2] has much smaller estimation errors.

<figure style="width: 800px" class="align-center">
  <img src="/images/2018-06-19-single-image-depth/compare-hist.png">
  <figcaption>Fig 5. Distribution of per-pixel estimation errors. From left to right: Ours, Eigen[2] </figcaption>
</figure>

## Reference

1. E. David. et al. Depth map prediction from a single image using a multi-scale
deep network. NIPS, 2014.

2. E. David and F. Rob. Predicting depth,surfacen ormals and semantic labels
with a common multi-scale convolutional architecture. ICCV, 2015.

3. C. Godard, O. M. Aodha, and G. J. Brostow. Unsupervised monocular depth
estimation with left-right consistency. CVPR, 2017.

4. P. Kohli, N. Silberman, D. Hoiem, and R. Fergus. Indoor segmentation and
support inference from rgbd images. ECCV, 2012.
