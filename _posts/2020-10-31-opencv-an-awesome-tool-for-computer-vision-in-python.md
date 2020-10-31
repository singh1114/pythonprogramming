---
layout: post
title: OpenCV- An awesome tool for computer vision in python
date: 2020-10-31T15:49:17.395Z
updated_date: 2020-10-31T15:49:17.426Z
description: Introduction to OpenCV- An awesome tool for computer vision in
  python with a program to read the QR code.
published: true
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
skip_from_listing: false
---
To those who don't know what OpenCV is, it is a library for image/video processing, machine learning and computer vision. This tool also supports C++ and Java, but I am a little biased towards python and you all know the reason why.

Also, this is my first post on this platform since I am new here and in this post, we are going to take a look at what this library is capable to do. Let's get started!

## Image Processing

OpenCV is widely used for image processing and manipulations. The image is represented as a [NumPy array](https://pythonprogramming.org/data-science-i-all-things-you-need-to-know-about-numpy/) (3D array if RGB image is being read or 2D array if Grayscale image is being read). Face-recognition is the best example of things that OpenCV can do.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/03pycgdxr5f71w5zf2f5.png)
From many useful tools to QR code reader, you can create literally tons of awesome projects with it.

Below is the video demonstration of a simple QR code reader program that I made using OpenCV.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">This is a simple <a href="https://twitter.com/hashtag/python?src=hash&amp;ref_src=twsrc%5Etfw">#python</a> program that creates a QR code of a URL, displays it, takes the screenshot of the screen, finds and decodes the QR Code on the screen and loads the site in the browser🤩<a href="https://twitter.com/hashtag/100DaysOfCode?src=hash&amp;ref_src=twsrc%5Etfw">#100DaysOfCode</a> <a href="https://twitter.com/hashtag/DEVCommunity?src=hash&amp;ref_src=twsrc%5Etfw">#DEVCommunity</a> <a href="https://twitter.com/hashtag/DevCommunityIN?src=hash&amp;ref_src=twsrc%5Etfw">#DevCommunityIN</a> <a href="https://twitter.com/hashtag/programming?src=hash&amp;ref_src=twsrc%5Etfw">#programming</a> <a href="https://t.co/H2cvd98Skl">pic.twitter.com/H2cvd98Skl</a></p>&mdash; Aditya Chaudhary👨‍💻 (@AdityaGameDev) <a href="https://twitter.com/AdityaGameDev/status/1315893570616717312?ref_src=twsrc%5Etfw">October 13, 2020</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

By using various NumPy methods you can experiment around the image properties like the specific color at any pixel/region. Below is a video demonstration of a simple color picker kind of program that opens an image and on left click, shows the hex color code of the area which is clicked and also copies the hex code to the clipboard!

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Made a simple <a href="https://twitter.com/hashtag/python?src=hash&amp;ref_src=twsrc%5Etfw">#python</a> program that opens an image and on left click shows the hex color code of the area which is clicked and also copies the hex code to clipboard🙂<a href="https://twitter.com/hashtag/100DaysOfCode?src=hash&amp;ref_src=twsrc%5Etfw">#100DaysOfCode</a> <a href="https://twitter.com/hashtag/programming?src=hash&amp;ref_src=twsrc%5Etfw">#programming</a> <a href="https://twitter.com/hashtag/DEVCommunity?src=hash&amp;ref_src=twsrc%5Etfw">#DEVCommunity</a> <a href="https://twitter.com/hashtag/DevCommunityIN?src=hash&amp;ref_src=twsrc%5Etfw">#DevCommunityIN</a> <br><br>Video-Demonstration : <a href="https://t.co/B7xVnkGQM2">pic.twitter.com/B7xVnkGQM2</a></p>&mdash; Aditya Chaudhary👨‍💻 (@AdityaGameDev) <a href="https://twitter.com/AdityaGameDev/status/1314804125721194496?ref_src=twsrc%5Etfw">October 10, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## Video-Processing

A video is nothing but tons of frames of the image. So basically things which you can do with the image can also be done with video files.

OpenCV provides you to read videos from two streams. You can either pass your webcam as a video source and experiment around your camera properties or pass a valid file path of the compatible video file.

One of the best projects which I saw on video-processing was Raghav Khanna's invisible cloak project.
Here is the video demonstration of his project

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">The Invisible Cloak<br>This is a simple python program that make you invisible when you use a RED colour cloth. <br><br>Everyone has watched harry potter! <br>So, Harry pass on this cloak to us :p<br><br>Below attached video is the example of the Invisible Cloak! <br><br>Give a ⭐ if you like this :) <a href="https://t.co/yD5wH8bihc">pic.twitter.com/yD5wH8bihc</a></p>&mdash; Raghav Khanna (@erRaghavKhanna) <a href="https://twitter.com/erRaghavKhanna/status/1285504929700777985?ref_src=twsrc%5Etfw">July 21, 2020</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Apart from these things OpenCV is widely used in game-automation, self-driving cars, smart CCTV cameras etc.

Now imagine if you have thousands of QR code images on your PC and you are given to draw the histogram graph of sites to which the different QR code belongs to. As a human being, you can't scan each and every image to get the information about the QR code.

That's where programming and OpenCV play key role.

Just crawl through that folder using **os module**, read and scan the image using **OpenCV** and by some algorithms plot a graph using **matplotlib**.

Thanks for reading!