---
layout: post
title: How to record your screen using Python 🐍
date: 2020-10-31T19:53:43.876Z
updated_date: 2020-10-31T19:53:43.916Z
published: true
image: https://i.ibb.co/Xxf2qCn/carbon-3-1.png
tags:
  - python
  - opencv
  - image-processing
categories:
  - python
  - opencv
  - image-processing
author_name: Aditya Chaudhary👨‍💻
author_username: aditya
show_ads: false
show_telegram_signup: false
---
{% include lazyload.html image_src="https://i.ibb.co/vq05T32/carbon-3-2.png" image_alt="How to record your screen using Python" image_title="How to record your screen using Python" %}


In this article, we are going to take a look at how we can record our screen using Python.

Before getting into it, one should first understand what actually a video is. Basically, a video is nothing but a sequence of images (called frames) displayed at a given frequency.

So a workaround to make a screen recorder would be to capture each frame of the screen and write it as a video file.

To do this python comes with handy libraries. For the screen recording, we will be using OpenCV, [PyAutoGUI](https://pypi.org/project/PyAutoGUI/), and [Numpy](https://pythonprogramming.org/data-science-i-all-things-you-need-to-know-about-numpy/).

Read more about OpenCV in our last article.

{% include linked_post.html url="opencv-an-awesome-tool-for-computer-vision-in-python" %}

So let's get started!

Make sure you have all the above mentioned necessary libraries installed.

1. **OpenCV**: To install OpenCV, run `pip install opencv-python`
2. **PyAutoGui**: To install PyAutoGui, run `pip install pyautogui`
3. **Numpy**: To install Numpy, run `pip install numpy`

You are all set!

The first step is to import all the modules needed.

```python
import cv2 #OpenCV
import pyautogui
import numpy as np
```

The next step is to specify the video codec using `fourCC`. It is a 4-byte code used for specifying the video codec. For the sake of simplicity we can pass the `FourCC` code for AVI video format which is `*"XVID"`.

```python
codec = cv2.VideoWriter_fourcc(*"XVID")
```

In the next step, we will be creating a `videoWriter` object.
The video writer method of `cv2` module needs 4 mandatory arguments.

First is the `filename`, second being the `codec`, third being the FPS (Frame per second) and the last being the video (or Screen) resolution.

For a better screen recording, 60 FPS is a good frame rate. You can experiment around the FPS value to get the optimum results.
 
```python
out = cv2.VideoWriter("Recorded.avi", codec , 60, (1366, 768)) #Here screen resolution is 1366 x 768, you can change it depending upon your need
```

The next step is to implicitly create a window using `namedWindow()` method with the `WINDOW_NORMAL` flag so that the user can resize the window depending upon the need.

```python
cv2.namedWindow("Recording", cv2.WINDOW_NORMAL)
cv2.resizeWindow("Recording", 480, 270) #Here we are resizing the window to 480x270 so that the program doesn't run in full screen in the beginning
```

Now we need to capture a screenshot of each frame of the screen, write it to the video file and display the screen being recorded.

Here is the code snippet,

```python
while True:
    img = pyautogui.screenshot() #capturing screenshot
    frame = np.array(img) #converting the image into numpy array representation
    frame = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB) #converting the BGR image into RGB image
    out.write(frame) #writing the RBG image to file
    cv2.imshow('Recording', frame) #display screen/frame being recorded
    if cv2.waitKey(1) == ord('q'): #Wait for the user to press 'q' key to stop the recording
        break

out.release() #closing the video file
cv2.destroyAllWindows() #destroying the recording window
```

Notice that the screenshot captured is in BGR format but in order to write the frame to the video file, we need to convert it into RGB format using `cvt.Color()` method with `COLOR_BGR2RBG` flag.

The condition `if cv2.waitKey(1) == ord('q')` waits for the user to press 'q' key to quit the recording.

Lastly, we have closed the video file and destroyed the recording window by `release()` and `destroyAllWindows()` method respectively.


**PS**: Linux users please don't be happy after hearing "Destroy all Windows"😂

You are all done!

It's time to test the results.

Normal-sized window

![Normal Sized window ](https://dev-to-uploads.s3.amazonaws.com/i/fh8vwjx57bmvgegv2wdm.png)

Maximized window: (Horrible infinite loop)
![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/hfd9ctc4nixfzrltr8k1.png)

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">I was making a screen recorder in <a href="https://twitter.com/hashtag/python?src=hash&amp;ref_src=twsrc%5Etfw">#python</a> using <a href="https://twitter.com/hashtag/opencv?src=hash&amp;ref_src=twsrc%5Etfw">#opencv</a> and it gave me some weird output😳<br><br>Seems like a portal to any parallel Universe.👁️<a href="https://twitter.com/hashtag/100DaysOfCode?src=hash&amp;ref_src=twsrc%5Etfw">#100DaysOfCode</a> <a href="https://twitter.com/hashtag/codingnewbies?src=hash&amp;ref_src=twsrc%5Etfw">#codingnewbies</a> <a href="https://twitter.com/hashtag/DevCommunityIN?src=hash&amp;ref_src=twsrc%5Etfw">#DevCommunityIN</a> <a href="https://twitter.com/hashtag/DEVCommunity?src=hash&amp;ref_src=twsrc%5Etfw">#DEVCommunity</a> <a href="https://t.co/Z6m3N1IW9N">pic.twitter.com/Z6m3N1IW9N</a></p>&mdash; Aditya Chaudhary👨‍💻 (@AdityaGameDev) <a href="https://twitter.com/AdityaGameDev/status/1319363186101866496?ref_src=twsrc%5Etfw">October 22, 2020</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Thanks for reading!