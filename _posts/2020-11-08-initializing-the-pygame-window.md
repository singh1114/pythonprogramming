---
layout: post
title: Initializing the pygame window
date: 2020-11-08T15:19:35.652Z
updated_date: 2020-11-08T15:19:37.335Z
description: Initializing the game window using pygame
published: true
tags:
  - python
  - pygame
categories:
  - python
  - pygame
author_name: Ranvir Singh
author_username: ranvir_xyz
show_ads: false
show_telegram_signup: false
series_unique_code: pygame_tutorial_series
series_part: "4"
series_page_title: Initializing the game window
series_title: Building a game using pygame
previous_slug: starting-the-pygame-project
skip_from_listing: true
---
Let's start by initializing the game window so that we can start with the game dev.

## Initialize the pygame module

We can start the pygame module just by calling the init function.

```python
pygame.init()
```

## Open the game window

With just a few lines of code you can open the pygame window. Just copy and paste the following code into the `main.py` file that we created in the last part.

```python
import pygame

pygame.init()


SCREEN_SIZE: tuple = 800, 600 # width, height
BLACK_RGB: tuple = 0, 0, 0

screen = pygame.display.set_mode(SCREEN_SIZE)
screen.fill(BLACK_RGB)

is_window_open = True

while is_window_open:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            is_window_open = False
``` 

With `import pygame`, we are importing all the modules of pygame package. `pygame.init()` is used to initialize all those models.

Then we are defining a few tuples that we need to initialize the window. First up is the `SCREEN_SIZE` which defines the width and height of the screen window. Similarly, we are using `BLACK_RGB` to define the background of the screen.

We set those values using `set_mode` and `fill` methods.

You should be able to see the window even with the following code for just a few seconds. But we are going to add a few more lines of code to run the window indefinitely.

`is_window_open` is used to run that indefinite loop. Inside the loop we wait for the `QUIT` event of which occur when you hit the close button on the created window.

## Run the code

After running these few lines of code, we will be able to see the black `pygame` window and it will stay open till you hit that red close button.

{% include lazyload.html image_src="https://i.ibb.co/6sYymn3/Screenshot-2020-11-09-at-12-01-33-AM.png" image_alt="Pygame blank empty window" image_title="Pygame blank empty window" %}