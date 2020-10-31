---
layout: post
title: OpenCV- An awesome tool for computer vision in python
date: 2020-10-31T15:49:17.395Z
updated_date: 2020-10-31T15:49:17.426Z
published: false
tags:
  - python
  - machinelearning
  - open-cv
categories:
  - python
  - machinelearning
  - open-cv
author_name: Aditya Chaudhary👨‍💻
author_username: aditya
show_ads: false
show_telegram_signup: false
---
To those who don't know what OpenCV is, it is a library for image/video processing, machine learning and computer vision. This tool also supports C++ and Java, but I am a little biased towards python and you all know the reason why.

Also, this is my first post on this platform since I am new here and in this post, we are going to take a look at what this library is capable to do. Let's get started!

1. **Image Processing** : OpenCV is widely used for image processing and manipulations. The image is represented as a [NumPy array](https://pythonprogramming.org/data-science-i-all-things-you-need-to-know-about-numpy/) (3D array if RGB image is being read or 2D array if Grayscale image is being read). Face-recognition is the best example of things that OpenCV can do.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/03pycgdxr5f71w5zf2f5.png)
From many useful tools to QR code reader, you can create literally tons of awesome project with it.

Below is the video demonstration of a simple QR code reader program that I made using  OpenCV



By using various numpy methods you can experiment around the image properties like the specific color at any pixel/region. Below is a video demonstration of a simple color picker kind of program that opens an image and on left click, shows the hex color code of the area which is clicked and also copies the hex code to the clipboard!
{% twitter 1314804125721194496 %}

2. **Video-Processing** : A video is nothing but tons of frames of image. So basically things which you can do with image can also be done with video files. OpenCV provides you to read video from two streams. You can either pass your web cam as a video source and experiment around your camera properties or pass a valid file path of the compatible video file. One of the best projects which I saw on video-processing was Raghav Khanna's invisible cloak project.
Here is the video demonstration of his project
{%  twitter 1285504929700777985 %}

Apart from these things openCV is widely used in game-automation, self-driving cars, smart CCTV cameras etc.

Now imagine if you have thousands of QR code images in your PC and you are given to draw the histogram graph of sites to which the different QR code belongs to. As a human being you can't scan each and every image to get the information about the QR code.
That's where programming and OpenCV plays key role.
Just crawl through that folder using **os module**, read and scan the image using **OpenCV** and by some algorithms plot a graph using **matplotlib**

Thanks for reading!
