---
layout: post
title: Game Development using Pyglet - PongPong - Part II
date: 2021-09-12T08:25:15.398Z
updated_date: 2021-09-12T08:25:15.443Z
description: In this part, we will explore on how to create a game window and
  how to load our game objects (with no active gameplay, they will be still
  waiting for the next part !).
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
series_unique_code: pyglet-gaming
series_part: "2"
series_page_title: Python Pyglet gaming Part 2
series_title: Building a game using pyglet python
previous_slug: making-pongpong-game-development-using-pyglet-part-1
---
## What We Learned So Far?

- We know what pyglet is and how to design the PongPong game.
- We have the project structure and created main `pongpong.py` file.
- We created the Walls, Paddle and Ball classes, initialized with some very important variables (which we will use here).
- We learned the use of those variables and why are they important.

In this part, we will explore how to create a game window and how to load our game objects (with no active gameplay, they will be still waiting for the next part !).

So let's begin!

---

## Main PongPong

In the last part, we had coded the dimensions and speed of the ball when loaded initially.

It was like this:

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

Let's create our window and load the game objects:

```python
# ./PongPong/pongpong.py

import pyglet
from pong import load

class PongPongWindow(pyglet.window.Window):
    def __init__(self, *args, **kwargs):
        super(PongPongWindow, self).__init__(*args, **kwargs)

        self.win_size = (WIDTH, HEIGHT)
        self.paddle_pos = (WIDTH/2-PWIDTH/2, 0)
        self.main_batch = pyglet.graphics.Batch()
        self.walls = load.load_rectangles(self.win_size, BORDER, batch=self.main_batch)
        self.balls = load.load_balls(self.win_size, RADIUS, speed=ballspeed, batch=self.main_batch)
        self.paddles = load.load_paddles(self.paddle_pos, PWIDTH, PHEIGHT, acc=paddleacc, batch=self.main_batch)

    def on_draw(self):
        self.clear()
        self.main_batch.draw()


game_window = PongPongWindow(width=WIDTH, height=HEIGHT, caption='PongPong')
game_objects = game_window.balls + game_window.paddles

for paddle in game_window.paddles:
    for handler in paddle.event_handlers:
        game_window.push_handlers(handler)
```

We will see what is happening line-by-line:

- First, we import `pyglet` and our `load` module (we will look at this in a few points later).

- `class PongPongWindow(pyglet.window.Window):` this defines the game window class, that inherits the `Window` class functionality from `pyglet.window` (it is used to create still window in pyglet). After inheriting this, we have all its methods like `self.on_draw()`.

- Next in `__init__` method of `PongPongWindow` we initialize the base class using `super` function. What is [super](https://realpython.com/python-super/)?

- We are using `def __init__(self, *args, **kwargs):`, here `*args` and `**kwargs` are used to unpack any passed arguments and keyword arguments respectively. They are useful as we don't know which arguments we will be using later on while creating a window and initializing it using `super`.

- `self.win_size = (WIDTH, HEIGHT)` a class variable to be used for creating elements on the window to hold their positions.

- `self.paddle_pos = (WIDTH/2-PWIDTH/2, 0)` paddle's position. Here, `WIDTH/2` will return the center of the window but the paddle's coordinate starts at bottom-left, which means, if we only set paddle's position to `WIDTH/2` then its bottom-left would be at `WIDTH/2` but we don't want that (because it would feel like the paddle is not in the center). To rectify that, we need to subtract `PWIDTH/2` (half of paddle's width) from `WIDTH/2`, since we need to shift the paddle to left by half to make it in center, so that paddle's center would be at `WIDTH/2` (If it seems tricky, get a pen and a paper and draw window and paddle and see how this makes sense, but don't forget everything in pyglet space starts from bottom-left).

- `self.main_batch = pyglet.graphics.Batch()`, we create a batch now. Batch is something that groups different elements that needs to be drawn and draw then in a single call. For example, if we need to draw a rectangle and a circle, so during window creation and loading of objects, we would need to call a method like `draw` 2 times, 1 for rectangle and 1 for circle, but using a batch makes it even simpler, so if we club that rectangle and that circle in same batch, then just calling that batch's `draw` will draw both rectangle and circle in a single call. It is helpful to limit the code we write and make the code scalable.

- `self.walls = load.load_rectangles(self.win_size, BORDER, batch=self.main_batch)`, `self.balls = load.load_balls(self.win_size, RADIUS, speed=ballspeed, batch=self.main_batch)` and `self.paddles = load.load_paddles(self.paddle_pos, PWIDTH, PHEIGHT, acc=paddleacc, batch=self.main_batch)`, all these create and load the objects on game window (but still not drawn). Here, walls and ball creation takes window size `self.win_size` and paddle takes paddle position `self.paddle_pos`, followed by `BORDER` for walls, `RADIUS` for ball and `PWIDTH`, `PHEIGHT` and `paddleacc` for paddle (to know more about these constants variable please follow part 1). Then we pass the batch `self.main_batch`, this batch will contain all these walls, ball and paddle, we passed same batch to all 3 load methods. All these load functions will return a list containing `n` number of walls, balls and paddles (in our case it will be 3 walls, 1 ball and 1 paddle, but we can scale it to have `n` number of these). We will see later how these load methods work with all these passed arguments.

- `def on_draw(self):` this is a method present in the super class, we override it to draw the things we want.

- `self.clear()` clears anything and everything present in memory of window creation if present. May be helpful in simultaneous window creation, but a good practice to use this.

- `self.main_batch.draw()` then we draw the batch that contains all loaded objects (only 1 call to draw and everything will be available).

Now we move out of the class `PongPongWindow`.

- `game_window = PongPongWindow(width=WIDTH, height=HEIGHT, caption='PongPong')` now we create the game window we defined. We pass the width and height parameters with a caption to the window.

- `game_objects = game_window.balls + game_window.paddles` we define game objects that needs to be moved or that involves some position changes throughout the game. Ball and Paddle created are in a list returned by load functions.

Then comes the for loop:

```python

for paddle in game_window.paddles:
    for handler in paddle.event_handlers:
        game_window.push_handlers(handler)
```

In this for loop, for every paddle in-game window, we push its event handlers to game window to let it know that whenever some particular event occurs please know that it belongs to a certain element in window.

Secondly, why are we using a for loop to push event handlers when we have only 1 paddle ? Its because maybe in future if there is a case where we want another paddle to be made, then just adding that paddle in load function will be enough and hence promotes the scalability, as there will be no further change in the main file.

Well, so much we have covered, now let's move on to creating load functions that are used to load the objects.

### Load Functions

The load functions are the ones that we used in `PongPongWindow` class to help load the objects and store those objects in main batch.

Let's start its code.

- Importing required modules, `ball`, `paddle` and `rectangle`, these have the required classes.

```python
# ./PongPong/pong/load.py

from . import ball, paddle, rectangle
from typing import Tuple

```

- We will create `load_balls` function first. Code would look something like this:

```python

def load_balls(win_size : Tuple, radius : float, speed : Tuple, batch=None):
    balls = []
    ball_x = win_size[0]/2
    ball_y = win_size[1]/2
    new_ball = ball.BallObject(x=ball_x, y=ball_y, radius=radius, batch=batch)
    new_ball.velocity_x, new_ball.velocity_y = speed[0], speed[1]
    balls.append(new_ball)
    return balls

```

- Here, first we create a list `balls` that will contain `n` number of balls, in this case it will have only 1 ball.

- `ball_x` and `ball_y` defines the (x, y) coordinate of ball on the window, this point will be the point of ball's origin, that will be bottom-left of ball.

- `new_ball` contains `BallObject` instance, that takes `(x, y, radius, batch)`, all these are the arugments of `__init__` method of class `pyglet.shapes.Circle` that was inherited by `BallObject`. `x` and `y` contains the position values of (x, y) coordinate of ball, that would be bottom-left (I don't know how they calculate bottom-left of a circle !), then there is `radius` of the ball and the `batch` argument to specify that which batch it belongs to (remember we passed `self.main_batch` in batch in `pongpong.py` file).

- `new_ball` has attributes `velocity_x` and `velocity_y` (to know more go through part 1), here we assign them their initial value, that is, `ballspeed = (-2, -2)` from `pongpong.py`, that means, it will be falling along a line that intersects at point `(-2, -2)`.

- Finally, we append the created ball to the list `balls` and return that list.

So that's how the loading of the ball takes place in the PongPong window. Any doubt, use comments to reach out!

- Let's create a similar load function for the paddle.

```python

def load_paddles(paddle_pos : Tuple, width : float, height : float, acc : Tuple, batch=None):
    paddles = []
    new_paddle = paddle.Paddle(x=paddle_pos[0], y=paddle_pos[1], width=width, height=height, batch=batch)
    new_paddle.rightx = new_paddle.x + width
    new_paddle.acc_left, new_paddle.acc_right = acc[0], acc[1]
    paddles.append(new_paddle)
    return paddles

```

- Create `paddles` list to contain all paddles created.

- `new_paddle` contains instance of class `Paddle` that takes `(x, y, width, height, batch)`, these arguments are defined in inherited class `pyglet.shapes.Rectangle`, `x` and `y` defines bottom-left coordinate of rectangle/paddle, `width` and `height` of paddle and `batch` contains batch object in which it will reside (that is, `self.main_batch`). To know more about `Paddle` class structure, read through part 1.

- `new_paddle.rightx` contains the right most x-coordinate of paddle, that we will use to detect collision with right wall.

- `new_paddle.acc_left` and `new_paddle.acc_right` both defines the amount of points they will move whenever left and right arrow keys are pressed respectively.

- Finally, we append the created paddle to the list and return the `paddles`.

Hence, we have the paddle load function ready.

Let's look at the final function to load walls.

```python

def load_rectangles(win_size : Tuple, border : float, batch=None):
    rectangles = []
    top = rectangle.RectangleObject(x=0, y=win_size[1]-border, width=win_size[0], height=border, batch=batch)
    left = rectangle.RectangleObject(x=0, y=0, width=border, height=win_size[1], batch=batch)
    right = rectangle.RectangleObject(x=win_size[0] - border, y=0, width=border, height=win_size[1], batch=batch)
    rectangles.extend([left, top, right])
    return rectangles

```

- Create `rectangles` list to contain all the rectangles created (in this case they will act as walls).

- Create `top`, `left` and `right` variables, that will show the respective walls.

- Each wall variable is assigned to instance of `RectangleObject` that takes `(x, y, width, height, batch)`, these arguments are passed to the class inherited `pyglet.shapes.Rectangle` by `RectangleObject`. These variables are same as defined for paddle.

- After instantiating all 3 walls, we append them to `rectangles` list and return that.

Pheww! All the load functions are ready and already in use in `PongPongWindow` class in `pongpong.py` file.

Let's revisit main `pongpong.py` file to run the app.

Add the following at the end of `pongpong.py` file:

```python
# ./PongPong/pongpong.py

if __name__ == '__main__':
    pyglet.app.run()

```

Run the file and the output would look something like this:

![pongpong_still.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1611156350442/mbdW77WPB.png)

See, the ball is in the center, paddle is in the center and walls are looking good!

Hence, so far we have done awesomely amazingly well !

---

Well that was it, in this part we learned:

1. How to load our game window.
2. How to load elements in that game window and how to code those load functions.
3. How to push event handlers to game window if there are any for the elements presents.
4. How to run pyglet app, that is, using `pyglet.app.run`.

If you followed this step-by-step and have some doubts, I would be very happy to get them sorted (maybe I will learn something new 😁). Make sure to drop them in the comments!

In the next part, we will see how to make function to introduce the ability for elements to interact with each other.

---

Just starting your Open Source Journey ? Don't forget to check out [Hello Open Source](https://github.com/siddharth2016/hello-open-source)

Want to `++` your GitHub Profile README ? Check out [Quote - README](https://github.com/marketplace/actions/quote-readme)

Till next time !

Namaste 🙏