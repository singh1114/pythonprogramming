---
layout: post
title: Global constants for snake game
date: 2020-11-17T19:23:44.718Z
updated_date: 2020-11-17T19:23:44.771Z
description: ""
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
series_part: "5"
series_page_title: Global constants for snake game
series_title: Building a game using pygame
previous_slug: updating-the-pygame-window-attributes
skip_from_listing: true
---
We will need to add a few constants to our game which can be used to handle the movements, total size and grid architecture of the game.

For our snake game, the snake will be any one of these grids at any given time.

I am creating a new file to keep all my constants.

`constants.py`

```python
SCREEN_WIDTH: int = 810
SCREEN_HEIGHT: int = 510

# number of grids
GRID_SIZE: int = 30

# Height and width of single grid
GRID_WIDTH: int = SCREEN_WIDTH / GRID_SIZE
GRID_HEIGHT: int = SCREEN_HEIGHT / GRID_SIZE

# Movements
MOVE_UP: tuple = (0, -1)
MOVE_DOWN: tuple = (0, 1)
MOVE_LEFT: tuple = (-1, 0)
MOVE_RIGHT: tuple = (1, 0)
```

First, we are defining the total width and height of the screen. We will use the variables defined here in the main file as well.

Then we are defining basic grid stuff. We will use these variables later during the game development.

Finally, we are creating some constants that will help us with the movement. We are using a tuple to define the 2-dimensional movement.

Finally, we can start using these constants in `main.py` file.

```python
from constants import SCREEN_WIDTH, SCREEN_HEIGHT

# width, height
SCREEN_SIZE: tuple = SCREEN_WIDTH, SCREEN_HEIGHT

pygame.display.set_mode(SCREEN_SIZE)
```

It's always great to keep your code structured and keep it well separated. It is always easier to find and make changes that way.