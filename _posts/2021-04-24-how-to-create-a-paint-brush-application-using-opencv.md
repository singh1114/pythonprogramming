---
layout: post
title: How To Create A Paint Brush Application Using OpenCV
date: 2021-04-24T18:50:30.903Z
updated_date: 2021-04-24T18:50:30.920Z
description: Creating a paintbrush application is one of the most fun activities
  while learning computer vision and image processing with a little bit of GUI
  concepts. Using OpenCV for such an application saves us a lot of time by not
  needing to know the basics of GUI programming, so we can only be left with
  image processing tasks at hand.
published: true
image: https://i.ibb.co/Ltfb1tY/carbon.png
tags:
  - opencv
  - python
categories:
  - opencv
  - python
author_name: Siddharth Chandra
author_username: siddharth
show_ads: false
show_telegram_signup: false
skip_toc: false
---
{% include lazyload.html image_src="https://i.ibb.co/Ltfb1tY/carbon.png" image_alt="Create A Paint Brush Application Using OpenCV" image_title="Create A Paint Brush Application Using OpenCV" %}

Creating a paintbrush application is one of the most fun activities while learning computer vision and image processing with a little bit of GUI concepts. Using OpenCV for such an application saves us a lot of time by not needing to know the basics of GUI programming, so we can only be left with image processing tasks at hand.

In this article, we are going to make a paint application that features the brush functionality with customizations like:

* Changing brush size using a trackbar.

* Changing paint color using a trackbar.

* Viewing the selected color on the window.

We will also see how current trackbar values can be displayed on the window, so we have a general idea of the color variation.

{% include note.html title="Issue with macOS trackbar values" description="Somehow the in-built create trackbar method of OpenCV does not show selected trackbar values on macOS, hence we will display the values on the image window itself." %}

If you do not have OpenCV installed, I suggest you visit [here](https://blog.codekaro.info/lets-draw-opencv-logo-using-opencv) and check the **Prerequisites** steps on the process of installation.

Without any further ado, let's start making our first paintbrush application.

## Import everything we need

Create a python file, name it whatever you like. Add below code in it (make sure you have OpenCV installed):

```python

import cv2
import numpy as np

draw = False
window_name = "Paint Brush Application"
bgr_track = {'B': 0, 'G': 0, 'R': 0}

img = np.zeros((512,512,3), np.uint8)
cv2.namedWindow(window_name)

# Initial color window, showing black color, same as paint area
cv2.rectangle(img, color_win_position[0], color_win_position[1], (0,0,0), -1)
```

* First 2 lines import necessary libraries. Here, we are going to import `cv2` and `numpy` with an alias `np`.

* Next 3 lines define some constants that we are going to use later. `draw` says if the mouse is drawing on image window, `window_name` describes the name of the application window that is `Paint Brush Application` and `bgr_track` is a dictionary that we will use to keep track of previous `BGR` values (you will know later why we are doing this).

* Next 2 lines create an area for our paint application and set a name to the window. `img` contains a numpy array of 3 dimensions where each value is zero, meaning it will be a black colored area. `cv2.namedWindow` attaches a name to the image we just created, we can later use this window name to place all other things like trackbar.

* The last line of this code snippet creates a rectangle window which we will be using for viewing our color that we are choosing from the trackbar.

Now, add the below code:

```python

font = cv2.FONT_HERSHEY_SIMPLEX
img = cv2.putText(img, "R: ", (10, 30), font, 0.5, (255,255,255), 1)
img = cv2.putText(img, "G: ", (90, 30), font, 0.5, (255,255,255), 1)
img = cv2.putText(img, "B: ", (170, 30), font, 0.5, (255,255,255), 1)

img = cv2.putText(img, "0", (30, 30), font, 0.5, (255,255,255), 1)
img = cv2.putText(img, "0", (110, 30), font, 0.5, (255,255,255), 1)
img = cv2.putText(img, "0", (190, 30), font, 0.5, (255,255,255), 1)
```

* Now, we put text on the image window that will work as the counter for values of the BGR trackbar. Since on macOS, the track bar is not showing selected values, so we need this functionality. Also, take it as a side activity that we can do.

* We put text using the `cv2.putText` method of OpenCV and set a font as well. We can set any font we want, here we are using Hershey simplex font.

* We are placing text like `R: ` and `0` separately because we need to change only number values and not constants like `R: `.

* We can select any random position for these counters on the image and check which best suits our needs.

So far, we have all the variables that we need, now let's move onto creating some interesting functions!

## Functions - All We Need

Add below python code snippet to the same file where we left above:

```python

def nothing(x):
    pass

def update_R_value(x):
    global font, img, bgr_track
    img = cv2.putText(img, f"{bgr_track['R']}", (30, 30), font, 0.5, (0,0,0), 1)
    img = cv2.putText(img, f"{x}", (30, 30), font, 0.5, (255,255,255), 1)
    bgr_track['R'] = x

def update_G_value(x):
    global font, img, bgr_track
    img = cv2.putText(img, f"{bgr_track['G']}", (110, 30), font, 0.5, (0,0,0), 1)
    img = cv2.putText(img, f"{x}", (110, 30), font, 0.5, (255,255,255), 1)
    bgr_track['G'] = x

def update_B_value(x):
    global font, img, bgr_track
    img = cv2.putText(img, f"{bgr_track['B']}", (190, 30), font, 0.5, (0,0,0), 1)
    img = cv2.putText(img, f"{x}", (190, 30), font, 0.5, (255,255,255), 1)
    bgr_track['B'] = x
```

* We create a function like `nothing` that does nothing just do a simple pass. We can use it to fulfill our need to pass it as a parameter where it is required but we want it to not do anything.

* Next 3 functions are the same with just a difference of `B`, `G` and `R`. Let's explore `update_R_value`.

* Here we are trying to update the counter value of `R: 0` on the image window.

* First, we get all global variables and try to replace the already present value of `0` against `R: `.

In OpenCV, there is no feature that lets `cv2.putText` keep track of previous text on the same position so it could just replace it. Here we need to first put the same text as the previous value, like `0` in this case, and change color similar to the background of the paint area (here it is black) so that it is like that it vanished from the paint area.

Now we just put a new trackbar value, which is passed to the function as `x` with the color white in the same place. Lastly, we update the `bgr_track` dictionary that keeps the track history of the last value and the same process repeats.

The same kind of thing is going on with the other 2 functions, just for different color trackbar values.

In simple terms, take the below scenario:

* Let's say we have a list - `l = [0, 0, 0]`. Here all the values were `-1` initially representing the start stage of the list. It is a special kind of list where we can only change its values only if they are `-1` first and nothing else.

* What we are going to do now is that, replace the first index (`R` color value let's say) with a new value.

* So we try to replace `0` knowing its position with `-1` because if we replace it directly it overlaps and doesn't work. So what we do instead is `l[0] = -1` and then after setting it to `-1`, it is back to its original state from where we can do anything to its value. Now we do the actual replace `l[0] = x`.

So what just happened? If you need more insight on this please visit my answer on StackOverflow [here](https://stackoverflow.com/a/67161084/9219103).

Let's move on to mouse events, which will help us to draw using a paintbrush.

```python

def draw_circle(event, x, y, flags, param):
    global draw, img

    if event == cv2.EVENT_LBUTTONDOWN:
        draw = True

    elif event == cv2.EVENT_MOUSEMOVE:
        if draw:
            cv2.circle(img, (x,y), cv2.getTrackbarPos("Brush Size", window_name),
                       (cv2.getTrackbarPos("B", window_name),
                        cv2.getTrackbarPos("G", window_name),
                        cv2.getTrackbarPos("R", window_name)),
                       -1)

    elif event==cv2.EVENT_LBUTTONUP:
        draw = False
        cv2.circle(img, (x,y), cv2.getTrackbarPos("Brush Size", window_name),
                       (cv2.getTrackbarPos("B", window_name),
                        cv2.getTrackbarPos("G", window_name),
                        cv2.getTrackbarPos("R", window_name)),
                       -1)
```

* This method `draw_circle` will be passed to the mouse event callback method of OpenCV. It takes 5 parameters - `event` that gives what is the event type, `x` and `y` the position of mouse, `flags` that can give us any flags that mouse events raise and some other `param` related to mouse callbacks. We can ignore `flags` and `params` for this case.

* We have 3 conditions here. `if event == cv2.EVENT_LBUTTONDOWN` checks for if the left mouse button down event occurred or not, if it happens then we will set `draw` to `True`.

* `elif event == cv2.EVENT_MOUSEMOVE` this condition checks if the mouse is moving, we simply don't want a moving mouse to draw on the paint area but we want a left button down moving the mouse to draw on the paint area, that's where the `draw` variable comes in, which will be checked to see if a left mouse button is actually down or not. Then it creates a circle of defined radius that it gets from the `Brush Size` trackbar, the color of the circle from the `BGR` trackbar and color fill is done via the last `-1` parameter value. This creates a circle while moving a mouse and this happens so smoothly that it shows an effect of a paintbrush.

* `elif event==cv2.EVENT_LBUTTONUP` is for when the mouse button is not clicked and we don't want to draw on the paint area. Here, we set `draw` to `False` and draw the last circle on positions given.

Now, let's put trackbar and mouse event callback. Also, we will create a loop to continuously display the changes to the image window.

## Trackbar, Mouse Event Call and Main Loop

Add the following code to the same file:

```python

cv2.createTrackbar("R", window_name, 0 ,255, update_R_value)
cv2.createTrackbar("G", window_name, 0, 255, update_G_value)
cv2.createTrackbar("B", window_name, 0, 255, update_B_value)
cv2.createTrackbar("Brush Size", window_name, 1, 8, nothing)
cv2.setMouseCallback(window_name, draw_circle)
```

* Here, we create a track bar for `R`, `G`, `B` color values and for `Brush Size` adjustment. `cv2.createTrackbar` method takes the following arguments, trackbar name `R`, window name where the trackbar needs to be put (here `window_name`), start value `0`, end value `255` of trackbar and last is a callback function which calls `update_R_value` whenever trackbar value gets changed. All 4 trackbars are similar to this structure and are self-explanatory now.

* `cv2.setMouseCallback` takes 2 parameters to set a mouse callback event, first one is the image window `window_name` on which this callback event should be tracked and the second is the callback function to call when a mouse event occurs.

*Notice here, that we don't have separate callback methods for MOUSE_UP or MOUSE_DOWN, but only a single one that captures all events with a passed parameter to callback function `event` and later we can use conditionals to filter them out.*

Now, add the following code to the same file for our main loop:

```python

while(1):
    cv2.imshow(window_name, img)
    key = cv2.waitKey(1) & 0xff
    if key==ord('q'):
        break

    b = cv2.getTrackbarPos("B", window_name)
    g = cv2.getTrackbarPos("G", window_name)
    r = cv2.getTrackbarPos("R", window_name)
    cv2.rectangle(img, color_win_position[0], color_win_position[1], (b,g,r), -1)

cv2.destroyAllWindows()
```

* Here, we want the loop to continue until it encounters a keyboard keypress event. We are going to quit the application whenever the `Q` key is clicked.

* Inside the loop, we are checking for keyboard quit condition and also getting `BGR` values and displaying them in a rectangle at `color_win_position`.

* Finally, if the program exits the loop, then we simple destroy the unwanted image window using the `cv2.destroyAllWindows` method of OpenCV.

Well, that's complete our code.

For complete script, please visit [here](https://github.com/siddharth2016/Opencv-Python-Computer-Vision/blob/master/paintApplication.py).

Now, let's look at the demo.

## Conclusion Demo

### Displaying colour trackbar values.

![display_color_value.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1618918273706/tzJAGC7px.gif)

### Displaying colour trackbar colours.

![color_trackbar.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1618918287732/a_DSuP5dQ.gif)

### Changing brush size.

![brush_size.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1618918301839/aefBGvmmj.gif)

### Hello World, CodeKaro!

![miscellaneous.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1618918315829/xoXv0xxCx.gif)

I know, my paint skills are very poor!😰

## What Next?

Well, that's it from me. If you want to explore further, the following activities can be done to add more feature to the paint application:

* Add utility to select brush type. Add rectangle, diamond, hexagon, etc shaped brushes.

* Try to include no-fill brush utility.

* Try to add a function that creates a circle or rectangle with a given size with a double click of the mouse on the paint area.

* Add a utility to clear the area and restart the work, by not having to close and restart the program.

---

- Just starting your Open Source Journey? Don't forget to check [Hello Open Source](https://github.com/siddharth2016/hello-open-source)

- Need inspiration or a different perspective on the Python projects or just out there to explore? Check [Awesome Python Repos](https://github.com/siddharth2016/awesome-python-repos)

- Want to make a simple and awesome game from scratch? Check out [PongPong](https://github.com/siddharth2016/PongPong)

- Want to `++` your GitHub Profile README? Check out [Quote - README](https://github.com/marketplace/actions/quote-readme)

Till next time!

Namaste 🙏