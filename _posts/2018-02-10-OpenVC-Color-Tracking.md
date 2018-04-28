---
title: "Tracking a Patch of Color with OpenCV"
categories:
  - post
tags:
  - Hobby
  - tech

layout: single
---

OpenCV is a really powerful tool for computer vision developers. Combined with
python, prototyping and deployment becomes really fast. Motivated by the heat in
computer vision recently, I revisited some basic features of OpenCV, and
re-discovered this little project I did a few years ago.

This program detects from a stream of video areas that have a predefined color,
in this case, green. Green areas are identified and labeled automatically, and
the centroid of the area is computed and labeled with a red dot. The results are
shown in video below:

<iframe width="640" height="360" src="https://youtu.be/VmGkj5CtbUY" frameborder="0" allowfullscreen>
</iframe>

The realization of this function involves a number of steps:

* A video stream is captured from the webcam of my laptop, and is feed to the program a frame at a time. The program processes the video stream in real time.
* The frame is smoothed using a Gaussian kernel, and is then transformed from RGB color space to HSV color space.
* The frame is thresholded by the Hue value at each pixel, creating a binary image.
* Contours are detected in the thresholded binary image, and the centroid is computed.

The HSV (Hue-Saturation-Value) color space is preferred over RGB, because the
Hue axis of HSV is directly related to the perceived color of a pixel. In
contrast, the relationship between the perceived color and the RGB values is
more complicated. In this program, "green" is defined as a Hue value between 50
and 80 (255 is maximum).

<figure>
    <a href=""><img src="/images/2018-02-10-OpenVC-Color-Tracking/hsv.png"></a>
    <figcaption>Fig.1 HSV Color Space</figcaption>
</figure>

Due to variations in lighting conditions and reflections, using a single set of
hue thresholds results in very bad tracking robustness. In this program, an
adaptive thresholding method is implemented and is found to greatly improve
tracking robustness. Namely:

1. Thresholds are initialized at `hue_min=50` and `hue_max=80`.
2. When a new frame is processed, identify the centroid of the largest green area, and store the Hue value at the centroid (name it `hue_central`).
3. Update the thresholds as follows: `hue_min=hue_central-10`, `hue_max=hue_central+10`.

The adaptive thresholding method ensures that the program consistently tracks
the largest green area in the video stream. The method fails horribly, however,
when lighting conditions change too drastically (e.g. shadows or reflections).
