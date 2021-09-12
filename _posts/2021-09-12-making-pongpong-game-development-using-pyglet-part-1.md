---
layout: post
title: Making PONGPONG - Game Development using Pyglet - Part 1
date: 2021-09-12T08:04:33.066Z
updated_date: 2021-09-12T08:04:33.101Z
description: This is the first part of the pyglet gaming series where we will
  create a pongpong game using pyglet. We are workign on this to learn the
  basics of game development in general.
published: true
tags:
  - pyglet
  - python
  - gaming
categories:
  - pyglet
  - python
  - gaming
author_name: Siddharth Chandra
author_username: siddharth
show_ads: false
show_telegram_signup: false
series_unique_code: pyglet-1
series_part: "1"
series_page_title: Python Pyglet gaming Part 1
series_title: Python Pyglet gaming Part 1
skip_from_listing: false
---
In this 3 part series, we will be making a game, using python game programming library *[pyglet](https://pypi.org/project/pyglet/)*.

---

## What is Pyglet?

*pyglet* is a cross-platform windowing and multimedia library for Python, intended for developing games and other visually rich applications. Pyglet runs under Python 3.5+. Being written in pure Python, it also works on other Python interpreters such as PyPy, while supporting development and production on Windows, MacOS and Linux operating systems.

## What is PongPong?

PongPong, is a name I thought of from PingPong, with some tweak to it, that it would not involve any Artificial Intelligence or Machine Learning and not any complex Data Structures, but simple Python classes, to create our game.

Generally speaking, PongPong is a simple yet amazing game to play around with! A game that every developer should try to make to learn several concepts behind game development.

---

## PongPong Design

PongPong will have single-player functionality with others being solid walls. If you know traditional PingPong, it has 2 walls and 2 players (both being human or one could be a computer), players either can use the keyboard's arrow keys or mouse movement to dictate movements of the paddle. 

Paddle can not move further behind the walls on both sides and also it could not move up and down, its movement is restricted to left and right.

Adopting the same functionality of Paddle from traditional PingPong, PongPong will have same function of paddle's movements with a restriction that it could only move using arrow keys.

Then, there is a Ball that we need to defend from going inside our territory or simply saying from going behind the paddle.

Ball will reflect and move in the opposite direction once touched with walls or paddle.

Ball's speed will be random, but we will try to keep it in a range so that it does not exceed our player's movement while defending its territory.

PingPong has 2 walls and 1 other player to play against, but PongPong will have 3 walls and single player functionality (to keep it simple and AI-free!).

A sample of the final output of PongPong.

![sample_pongpong.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1611049724763/bEE7ZcUWH.png)

Here the area marked in green are the walls, blue marked is ball( coloured orange) and the paddle is marked red.

Each component's design will be discussed later in their respective section while diving into code simultaneously.

The directory structure will follow the below pattern:

```bash
+---PongPong
    pongpong.py
    +---pong
        __init__.py
        ball.py
        load.py
        paddle.py
        rectangle.py
```

`pongpong.py` is our main file which will be used to trigger the game, all other files in folder `pong` contains classes for different components and have some utility functions (all are simple, I assure you).

So we have our initial design on how it will look on the outside and what all components we need to create, to sum it up we have:

1. 1 Paddle - A Rectangle
2. 1 Ball - A Circle
3. 3 Walls surrounding the boundaries for the Ball and Paddle - Rectangle Wall
4. Also, remember we need to view the final window as x-y coordinate plane, where bottom-left of the window is at (0, 0).

So, without any further ado, let's dive into programming the *PongPong* !

---

## Main PongPong file

Let's create our main file first (but only limited things that we need to get started). Name it `pongpong.py`, don't forget the project structure (`ponpong.py` file is in PongPong project directory).

```python
# ./PongPong/pongpong.py

# Variables, Considering a vertical oriented window for game
WIDTH = 600   # Game Window Width
HEIGHT = 600  # Game Window Height
BORDER = 10   # Walls Thickness/Border Thickness
RADIUS = 12   # Ball Radius
PWIDTH = 120  # Paddle Width
PHEIGHT = 15  # Paddle Height
ballspeed = (-2, -2)    # Initially ball will be falling with speed (x, y)
paddleacc = (-5, 5)   # Paddle Acceleration on both sides - left: negative acc, right: positive acc, for x-axis

```

All variables are almost self-explanatory.

`ballspeed` defines with which speed it would be moving, negative values means that it is moving in downward direction.

`paddleacc` defines the left-right movement direction acceleration whenever we click left-right arrow keys on keyboard. Consider it as the number of points it will move whenever we click arrow keys.

More things will be there in `pongpong.py` file, but for now just keep it as it is.

## Walls

Let's first create walls surrounding the paddle and ball, that is, our game boundaries.

Wall is essentially a rectangle drawn vertically (2 rectangles, 1 on left and 1 on right) and horizontally (1 on top of game window).

The walls will look something like this when completed after drawing on the main window.

![walls.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1611050051707/dz2KqEu6s.png)

For this, create a `RectangleObject` class in a python file, name it `rectangle.py`. The code would look something like this (don't forget to create this file in `pong` folder, see above for project tree):

```python
# ./PongPong/pong/rectangle.py

import pyglet

class RectangleObject(pyglet.shapes.Rectangle):

    def __init__(self, *args, **kwargs):
        super(RectangleObject, self).__init__(*args, **kwargs)

```

`RectangleObject` inherits pyglet's `Rectangle` shape class. Even though we are just initializing base class again in it's `__init__` method, it is somewhat good practice to create separate class (maybe just in case in future we may add more methods to it), but for this tutorial we are definitely not going to add any more method in it (remember what I said, it's going to be simple).

So what `pyglet.shapes.Rectangle` does ? It is a predefined class in `pyglet` library, present in `shapes` module, that provides us the power to create a Rectangle on some window. It has its own set of parameters, but to keep tutorial easy to follow, we will see those in next part of this tutorial on how to draw a rectangle that just fits our needs.

So we have our walls class ready named `RectangleObject`, inside `rectangle.py` file, inside `pong` folder, inside `PongPong` project directory. So far, so good !

Let's create our Paddle now!

## Paddle

The paddle is again a Rectangle, with additional ability to move around left-right using keyboard arrow keys.

So, let's see the code first, name the file as `paddle.py`:

```python
# ./PongPong/pong/paddle.py

import pyglet
from pyglet.window import key
from typing import Tuple


class Paddle(pyglet.shapes.Rectangle):

    def __init__(self, *args, **kwargs):
        super(Paddle, self).__init__(*args, **kwargs)

        self.acc_left, self.acc_right = 0.0, 0.0
        self.rightx = 0
        self.key_handler = key.KeyStateHandler()
        self.event_handlers = [self, self.key_handler]

```

Hmm, seems complex? Naah, it's simple! Let me explain:

- We import 3 things, `pyglet`, `key` from `pyglet.window` and `Tuple` from `typing`.

- `pyglet` as you know is our game library that we need to get things to work (or to play), `Tuple` is used for type hinting, to let other developers know what type of data is being passed in methods, more on type hinting [here](https://docs.python.org/3/library/typing.html).

- `pyglet.window` is a collection of several classes that are used for window handling, either clicking on window or doing some keyboard operation on window and many more such awesome things. What we need is keyboard functionality, hence importing `key` class.

- Create `Paddle` class that inherits `pyglet.shapes.Rectangle`, again it will be of type `Rectangle` having all its capabilities. 

- To initialize `Rectangle` class, we just initialize that in `__init__` method of `Paddle` class.

- After that, we define the acceleration of the ball with which it will move whenever we do keyboard action. Initially, the values of `self.acc_left` and `self.acc_right` will be zero, as the paddle is in rest mode, not moving in any direction. Consider acceleration as the number of points it will move in either direction. For example, if `self.acc_right = 2`, then that means the paddle will move 2 points in the right direction.

- `self.key_handler` will contain all the state handling of the keyboard, assigned to `key.KeyStateHandler()`, it will return a dictionary that contains state of keyboard keys.

- `self.event_handlers` as the name suggest, will contain the elements in a list that needs event actions, that is, those elements that will have some events to deal with, here in our case paddle needs to be moved hence an event described using `self` (the `Paddle` class itself) in `self.event_handlers` and that moving event will happen when a keyboard click event occurs hence an event described using `self.key_handler`. Both of these events will occur in a game window (that we will create later), hence needs to be pushed to a game window later, while launching the app.

So anything left ? Yeah `self.rightx`, what the hell is it ?

- In `pyglet` whenever an object is drawn on the window, like a rectangle, its origin point will be bottom-left, that is the initial `self.x` (coordinate on x-axis) will be zero or whatever we set during initialization, but remember it will represent bottom-left. So we need bottom-right x-coordinate as well (to avoid collision with the walls on the right side), no need to worry about y-coordinate, as paddle can only move left-right (represented on the x-axis in our case) and not top-bottom (represented as y-axis in our case).

The paddle will look something like this in the final window:

![paddle.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1611050086671/1B_83uDBs.png)

More on the paddle in the next part.

Let's create the PongPong ball.

## Ball

The ball will be a type of circle, with some speed to move around and colour of our choice.

Let's dive into the code then:

```python
# ./PongPong/pong/ball.py

import pyglet
import random
from typing import Tuple


class BallObject(pyglet.shapes.Circle):

    def __init__(self, *args, **kwargs):
        super(BallObject, self).__init__(*args, **kwargs)
        self.color = (255, 180, 0)
        self.velocity_x, self.velocity_y = 0.0, 0.0

```

Not going to explain what `pyglet.shapes.Circle` does (it is same as `Rectangle` class discussed earlier while creating [walls](#walls))

- We import 3 things, `pyglet`, `key` from `pyglet.window` and `Tuple` from `typing`.

- `pyglet` you know is our game library that we need to get things to work (or to play), `Tuple` is used for type hinting, to let other developers know what type of data is being passed in methods, more on type hinting [here](https://docs.python.org/3/library/typing.html).

- Create `BallObject` class that inherits `pyglet.shapes.Circle`, again it will be of type `Circle` having all its capabilities. 

- To initialize `Circle` class, we just initialize that in `__init__` method of `BallObject` class.

- We use the `color` attribute that was inherited from `pyglet.shapes.Circle` to set its colour to RGB (255, 180, 0).

- At last, we define `velocity_x` and `velocity_y` to determine the speed of the ball (on a coordinate plane, that's why we need velocity in x and y-direction).

While initializing the ball it would be at rest, but in the main window when we load all objects, during that time we will change its velocity to some value (defined in `pongpong.py` file, see [here](#main-pongpong)), but before loading it should be at rest!

It would look something like this:

![ball.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1611050249652/VIBiIVmT2.png)

Well, that's it, we have the ball in the field now!

---

## What We Know?

1. We know what PongPong is and what `pyglet` is.
2. We know the design of PongPong on how it will look after completing the code.
3. We created the main PongPong file that will run the game and define constants for the walls, the paddle and the ball.
4. We created a class for walls.
5. We created a class for a paddle, with some very interesting and much-needed variables.
6. At last we created the ball class that has some color and velocity with which it will be moving.

Well, so far so good!

If you followed this step-by-step and have some doubts, I would be very happy to get them sorted (maybe I will learn something new 😁). Make sure to drop them in the comments!

In the next part, we will see how to launch our game window and how to load these objects in the window.

---

Just starting your Open Source Journey? Don't forget to check out [Hello Open Source](https://github.com/siddharth2016/hello-open-source)

Want to `++` your GitHub Profile README? Check out [Quote - README](https://github.com/marketplace/actions/quote-readme)

Till next time!

Namaste 🙏