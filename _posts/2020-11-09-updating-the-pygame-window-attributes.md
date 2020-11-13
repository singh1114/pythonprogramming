---
layout: post
title: Updating the pygame window attributes
date: 2020-11-09T03:48:02.696Z
updated_date: 2020-11-09T03:48:02.717Z
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
series_part: "4"
series_page_title: Updating the pygame window
series_title: Building a game using pygame
previous_slug: initializing-the-pygame-window
skip_from_listing: true
---
There are a few attributes in case of `pygame` window that we can update. Let's dive a little into so that we know about all the options that we have.

## Update the title

The first up in the list is the title of the `pygame` window. We are going to replace that with the name of our game, **PPO snake**.

```python
pygame.display.set_caption("PPO snake")
```

## Updating the icon

We can add an image as the icon of the game. I am going to add the image to a new `assets` directory.

```shell
mkdir assets
```

You can any `32X32 px` image. I took a random snake image from [flaticon](https://flaticon.com).

For updating the icon, use the following code.

```python
icon = pygame.image.load("assets/snakes.png")
pygame.display.set_icon(icon)
```

## Updating the background color

For updating the background color of the `pygame` window you can write the following code in the main while loop.

```python
    screen.fill((100, 100, 255))
    pygame.display.update()
```

It is very important to update the display after we change the color. Otherwise, the change won't reflect.

The value inside the `fill` function is the RGB value and you can use any website like [this](https://www.w3schools.com/colors/colors_rgb.asp) and add a color of your choice.

{% include lazyload.html image_src="https://i.ibb.co/jZwXXHP/Screenshot-2020-11-13-at-4-27-41-PM.png" image_alt="Updating the pygame window attributes" image_title="Updating the pygame window attributes" %}