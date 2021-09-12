---
layout: post
title: Game Development using Pyglet - PongPong - Part III
date: 2021-09-12T08:44:26.565Z
updated_date: 2021-09-12T08:44:26.617Z
description: Pyglet final tutorial, In this post we complete the game
  development and by the end of it, you can play it as well.
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
series_part: "3"
series_page_title: Python Pyglet gaming Part 3
series_title: Building a game using pyglet python
previous_slug: game-development-using-pyglet-pongpong-part-ii
---
In this 3 part series, we will be making a game, using python game programming library *[pyglet](https://pypi.org/project/pyglet/)*.

---

## What We Built So Far?

- We created a design for PongPong Game and did some prepwork.
- We built the walls, paddle and ball using python classes.
- We then created the `PongPongWindow` class in main `pongpong.py` file to launch the game window.
- We saw how to add event handlers on PongPong game window for them to show effect while playing.
- We learned about batch object creation in pyglet and how to use it for single call draw of various elements on game window.
- We learned how to load all the game elements by making some easy to use functions in the file `load.py`.

If above points doesn't make sense, please go through part 1 and part 2, links above.

In this part, we will see how to make the objects interact with each other and will learn some fundamentals of pyglet game development.

So let's begin!

---

## Some Fundamentals

In pyglet, the initial window that we see is actually a frame created for us to view. Consider frame as a static image, once created it stays in memory as it is, unless or until updated.

For moving objects, pyglet does something similar to like we know from how videos work. A video of 1 second generally has 30 images, updating one after another rapidly so as to create an object-moving experience for the viewer. You may have heard about 60FPS experience people share now and then, that means they saw 60 frames/images flashing rapidly in front of their eyes, all of that in a tiny 1 second and that's why it creates a more engaging experience that people love (because your brain is seeing more information in the same amount of time).

The same goes for games like PUBG, where more the FPS, the more engaging the user experience would be.

So, we know to move objects, we need to update the frame 30 times or 60 times or 120 times per second, where each new frame contains a different position for objects so as to create an effect of motion.

Remember, we are going to update the frame and not the window, in pyglet once a window is created, it stays there for its lifetime, only the frames, that is, the objects whatever we have created/loaded inside the window will update according to their properties.

So, let's move on to create an update function in `pongpong.py` file to update frames!

## PongPong Window Update

Let's dive into the code first:

```python
# ./PongPong/pongpong.py

def update(dt):
    global game_objects, game_window

    for obj1 in game_objects:
        for obj2 in game_objects:
            if obj1 is obj2:
                continue
            obj1.update(game_window.win_size, BORDER, obj2, dt)


if __name__ == '__main__':
    pyglet.clock.schedule_interval(update, 1/120.0)
    pyglet.app.run()

```

Breakdown of code:

- Lets look at the main condition first, here we have added one new statement compared to last part, `pyglet.clock.schedule_interval(update, 1/120.0)`, this calls `schedule_interval` that takes 2 parameters, one is function name and another is time in seconds. `schedule_interval` of `clock` module calls passed function (1st parameter) in every `n` seconds (2nd parameter). Passed function takes 1 argument, that would be the time in seconds or differential time `dt` that is equivalent to 2nd parameter of `schedule_interval`.
- `pyglet.clock.schedule_interval(update, 1/120.0)` here `update` function is called in every 1/120 seconds, that is, we are going to see 120 frames per second (don't worry, the updation of objects are very minimal in each frame so 120 FPS is doable here).

Lets move on to the `update` function.

- `update` function takes 1 argument as discussed above, that is, `dt` the differential time, it would be `1/120.0`, the 2nd parameter of `schedule_interval` function.
- Next, `global game_objects, game_window`, we get global objects and game window that we created in part 2. Even though we don't need `global` keyword here, as `update` method will know that `game_window` and `game_objects` are global during runtime, but somewhere down the line, if we want to be consistent with pythonic code, then its better to be explicit than implicit (check [Zen Of Python](https://www.python.org/dev/peps/pep-0020/)).
- Then comes the for loop to update the frame.

```python

for obj1 in game_objects:
        for obj2 in game_objects:
            if obj1 is obj2:
                continue
            obj1.update(game_window.win_size, BORDER, obj2, dt)

```

- Here, since there are multiple dynamic objects (2 in our case, 1 paddle and 1 ball, walls are static) present and each object in some way or another interacts with each other (ball interact with walls as well, but since walls are static, we can just get their interaction as similar to how the ball interact with window borders).

- We iterate for each object in `game_objects`, inside that iteration we again iterate for objects in `game_objects` and ignore whenever the same object is encountered, that is, only distinct object interaction would be possible. Again the use of for loop, in this case, does not seem obvious, but when it comes to scaling the code, if we are introducing more objects, like 2 paddles or 2 balls then this for loop resolves many upcoming future errors.

- After ignoring the same object, then for loop execute the `update` method of outer loop object `obj1`, both objects have same update declaration, that is, it takes 4 parameters (excluding 1 self parameter) namely, `win_size` the game window dimensions `game_window.win_size`, `border` walls border `BORDER` (see, I got in flow and relied on python's implicit behaviour 😅, not using `global BORDER` at the start of function), `other_object` the second object (`obj2`) with which the first object (`obj1`) will interact and last parameter is `dt` the differential time, this `dt` will be used to identify the next position of objects (in our case, for the ball).

Well, with this `update` function, the frame of the window will update every 1/120 seconds with a new position for the ball and paddle.

- Paddle's position depends on keyboard click, but we are not fast enough to click it 120 times in a second, so each click will be stored in a buffer until paddle's update method is called and that would be 120 times in 1 second approximately, so it would result in a smooth experience without any delay in shifting paddle's position.

Now, let's explore the update method of paddle class and ball class.

## Paddle Update

Again, let's have a look at code first (the method is inside `Paddle` class, check part 1 for this class definition):

```python
# ./PongPong/pong/paddle.py

    def update(self, win_size : Tuple, border : float, other_object, dt):

        newlx = self.x + self.acc_left
        newrx = self.x + self.acc_right

        if self.key_handler[key.LEFT]:
            self.x = newlx
        elif self.key_handler[key.RIGHT]:
            self.x = newrx

        self.rightx = self.x + self.width

        if self.x < border:
            self.x = border
            self.rightx = self.x + self.width
        elif self.rightx > win_size[0]-border:
            self.x = win_size[0]-border-self.width
            self.rightx = self.x + self.width

```

- It is the update method with the same signature that we discussed above. So whenever this update method is called, paddle position will get updated.

- `newlx = self.x + self.acc_left` it defines the position of paddle (bottom-left coordinate) whenever left arrow key is clicked. `newlx` contains addition of current x-coordinate and number of points it would move when we click left arrow key.

- `newrx = self.x + self.acc_right` it defines the position of paddle (bottom-left coordinate) whenever right arrow key is clicked. `newrx` contains addition of current x-coordinate and number of points it would move when we click right arrow key.

- We have not assigned the new x-coordinate yet, we just computed different results for each action.

- We then check for actual key press event if it has occurred or not. To catch the event we can use key handler dictionary we got while initialising the paddle class (see part 1).

- If left arrow key is pressed then `self.key_handler[key.LEFT]` will return `true` or `1` to let us know that left arrow key event has occurred.

- If right arrow key is pressed then `self.key_handler[key.RIGHT]` will return `true` or `1` to let us know that right arrow key event has occurred.

- Then, if one of the conditions got true, we assign x-coordinate to the new values we computed earlier. If left key event then `self.x` is assigned to `newlx` or if right key event then `self.x` is assigned to `newrx`.

- If there is no left or right arrow key press, then `self.x` will remain same as previous value.

- After setting new position for x-coordinate that is bottom-left, we assign bottom-right x-coordinate `rightx` of the paddle (actually there is no attribute like `rightx`, it is something we are keeping track of to accommodate right side collision with walls, left side collision can identified using `self.x` itself).

- Then comes collision condition with walls. For left wall collision, if x-coordinate or bottom-left of paddle gets value less than the border value (border starts at bottom-left of window, that is a rectangle whose bottom-left would be at zero, hence taking border value makes sense as we need bottom-right of that rectangle to accommodate collision, `bottom-right=0(bottom-left)+border`), then that means beyond left wall the paddle should not go, hence we have to restrict the paddle's position to border value for as long as pressing left key takes place or until right key event occurs which changes the paddle position towards right.

- Last if the condition does the same thing but for right wall collision. Bottom-right of right wall/rectangle will be at window's width position, but we need bottom-left of that rectangle (you know why, right ?), to compute that, we can just subtract border value from window's width and we get bottom-left of right wall.

- If last condition gets true, then we restrict paddle's right x-coordinate position to not go beyond the right wall's bottom-left. To accomplish this we assign x-coordinate or bottom-left value of paddle to contain `win_size[0]-border-self.width`, here `win_size[0]-border` will give us the bottom-left of the right wall and subtracting `self.width` from that will give us the new updated bottom-left of the paddle.

- If no wall is encountered then there will be no adjustment to paddle's position against walls, paddle will just move freely wherever it wants, until it faces a wall.

We have not included the interaction of paddle and ball in paddle class, why is that ? Because, in paddle class we are more focused towards the nature of update a paddle will go through, and if a ball hits paddle, paddle does not need any update in that case, like what was needed for a wall collision with paddle.

Lets move on to ball class update method !

## Ball Update

Again, lets have a look at code first (the method is inside `BallObject` class, check part 1 for this class definition):

```python
# ./PongPong/pong/ball.py

    def update(self, win_size: Tuple, border: Tuple, other_object, dt) -> None:
        speed = [2.37, 2.49, 2.54, 2.62, 2.71, 2.85, 2.96, 3.08, 3.17, 3.25]    # more choices more randomness
        rn = random.choice(speed)
        newx = self.x + self.velocity_x
        newy = self.y + self.velocity_y

        if newx < border + self.radius or newx > win_size[0] - border - self.radius:
            self.velocity_x = -(self.velocity_x/abs(self.velocity_x))*rn
        elif newy > win_size[1] - border - self.radius:
            self.velocity_y = -(self.velocity_y/abs(self.velocity_y))*rn
        elif (newy-self.radius < other_object.height) and (other_object.x <= newx <= other_object.rightx):
            self.velocity_y = -(self.velocity_y/abs(self.velocity_y))*rn
        else:
            self.x = newx
            self.y = newy

```

- It is the update method with the same signature that we discussed above. So whenever this update method is called, ball's position will get updated.
- In update method, we first define some random values, that will act as the rate of speed of ball, whenever it hits a wall or paddle. We could have a constant value, but having random set of values we get more surprises. One other thing we could have done was to used python's built in random module to generate values in a given range. One thing to note is that, do not increase rate of speed gradually or even at any time to a very high value, because it may cause the system to fail and gameplay will get gruesome !
- Next we choose a random rate of speed from our collection of values.
- Then we assign new values to x and y coordinate, similar to what we did in paddle's case. We just change ball's position with `velocity_x` and `velocity_y` points on both axis respectively.

Then we check for several conditions.

- `newx < border + self.radius or newx > win_size[0] - border - self.radius` in this condition we check for left or right wall collision, which happens on x-axis only. When collision happens the only factor we need to consider is the point of contact and where the change should happen, while colliding with left/right wall the only change is needed in x velocity, as the point of contact was along width and not height.

- `velocity_x` gets rounded to value of negative 1 and then multiplied with rate of speed to compute new speed of ball along x-axis (speed will remain same along y-axis), that is, the number of points it will cover with every passing frame.

- `newy > win_size[1] - border - self.radius` similarly we are accounting top wall collision here, as the point of contact will be along height and not width, we change `velocity_y` accordingly.

- `velocity_y` gets rounded to value of negative 1 and then multiplied with rate of speed to compute new speed of ball along y-axis (along x-axis it will remain same), that is, the number of points it will cover with every passing frame.

- Next comes the condition for collision with paddle, `(newy-self.radius < other_object.height) and (other_object.x <= newx <= other_object.rightx)` in our case the `other_object` denotes paddle, as passed from PongPong main file, here we check if ball collides with paddle along y-axis and it should hit the paddle area only, that we identity using `other_object.x <= newx <= other_object.rightx` expression only x-axis. If this condition mets, then we only change y velocity, keeping x velocity intact.

- Finally if the ball does not collide with any other object, then just leave it moving freely as it want !

Here, we have taken negative sign in front of velocity value, it is because whenever a collision occurs the direction of the ball changes, to accommodate that change we need a negative sign in place !

Well then, we have covered the last part as well, go ahead and run main `pongpong.py` file to play around with the PongPong 🧙‍♂️

It would look something like this:

![pongpong_final.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1611665103182/2Hzdcmlk2.gif)

Check out [this](https://github.com/siddharth2016/PongPong) repository for complete working code and leave a star if you loved reading through this mini 3 part series of game development !

## Parting Note And Next Steps

I really loved writing my experience and how I approached the problem, hoping you found it insightful, learned something new and now know basics of developing a game like PongPong. 

I really enjoyed making this game when I went ahead and completed a challenge from [Zero To Mastery](https://zerotomastery.io/) monthly challenge. It was fun learning something new, the basics of game development and how to view a game as just like a geometry plane to work with was simply mind blowing for me. The concept of motion in videos is something I already knew from my experience with OpenCV, it is also an awesome library to know if you want to get into image processing or computer vision. Motivation behind this was to completely learn something new and not to work with the good old [pygame](https://www.pygame.org/news) !

As next steps you can take the following tasks (only if you want to):

1. Add a condition on what happens if the ball crosses the paddle and went straight to infinity !
2. Add a score card that, on every touch with paddle the score went up and if paddle loses the ball to infinity, the score suffers !
3. Increase speed of ball by adding more constants, see what happens then !
4. Add mouse functionality instead of keyboard arrow keys (I was trying this, but got stuck at some point).
5. Change colours, window size, add more balls, add an opponent (may need ML to work with) and other ideas you get, just go ahead and implement those, no ones going to stop you, its your work that you enjoy making !

Well then, that was it from me (for now 😉) !

Currently working on couple of projects that I intend to complete by year end, its going to be amazing !

Would love to connect with you on [GitHub](https://github.com/siddharth2016), [LinkedIn](https://www.linkedin.com/in/siddharth-chandra1/) and [Twitter](https://twitter.com/CodeKaro_) 🤝

---

Just starting your Open Source Journey ? Don't forget to check out [Hello Open Source](https://github.com/siddharth2016/hello-open-source)

Want to `++` your GitHub Profile README ? Check out [Quote - README](https://github.com/marketplace/actions/quote-readme)

Till next time!

Namaste 🙏