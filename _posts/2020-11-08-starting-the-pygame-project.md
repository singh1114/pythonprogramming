---
layout: post
title: Starting the pygame project
date: 2020-11-08T13:01:50.515Z
updated_date: 2020-11-08T13:01:50.534Z
description: Starting the pygame project with virtual env and install pygame
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
series_part: "3"
series_page_title: Starting the project
series_title: Building a game using pygame
previous_slug: final-game-using-pygame
next_slug: initializing-the-pygame-window
skip_from_listing: true
---
Let's get the project started.

This part of the tutorial is going to be the same for any of the Python projects that you are going to start.

The first thing that you have to do is install Python to get started.

You can install Python from the official [Python website](https://www.python.org/) if you are using Windows. Most of the Mac and Linux systems come with Python preinstalled.

You can check if Python is installed in your system just by using the command.

```shell
python --version
```

In this tutorial, I am going to use Python `3.8`. But you can follow it with whatever version you have. Please leave your comments when things are not working for your Python version or system.

## Installing virtualenv

Virtual environment is a way of differentiating different development systems.

For example, you can be making different games at a time and you don't want to mix the installation of one system with the other.

You can create a virtualenv using the following command if you are using Python 3.

```shell
python3 -m venv venv
```

For starting the virtualenv, just use the command,

```shell
source venv/bin/activate
```

## Installing pygame

```shell
pip install pygame
```

> If you are using PyCharm you can add set your virtual environment as the interpreter using the Pycharm settings/preferences.

With everything setup, we are ready to start with the tutorial and building our first game using PyGame.

## Checking installation

We can check if the installation was successful by creating a file called `main.py` and adding the following code.

```python
import pygame
```

Try running it using `python main.py`. If you see a similar response on the console then it is working fine.

```shell
pygame 2.0.0 (SDL 2.0.12, python 3.8.6)
Hello from the pygame community. https://www.pygame.org/contribute.html
```