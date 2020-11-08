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

