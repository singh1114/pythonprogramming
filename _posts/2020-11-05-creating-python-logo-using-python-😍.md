---
layout: post
title: Creating Python Logo using Python 😍
date: 2020-11-05T18:26:25.500Z
updated_date: 2020-11-05T18:26:25.526Z
published: true
image: https://i.ibb.co/Rpxgw9F/carbon-5-1.png
tags:
  - python
  - beginners
categories:
  - python
  - beginners
author_name: Ranvir Singh
author_username: ranvir_xyz
show_ads: false
show_telegram_signup: false
---
{% include lazyload.html image_src="https://i.ibb.co/Wc7JkHw/carbon-5-2.png" image_alt="Creating Python Logo using Python" image_title="Creating Python Logo using Python" %}

Not really a Python Logo, we are going to create a Python logo using Python. Our output is going to look something like this.

{% include lazyload.html image_src="https://i.ibb.co/f4LK5cp/Screenshot-2020-11-05-at-11-50-58-PM.png" image_alt="Python logo using characters" image_title="Python logo using characters" %}

Let's get started.

## Install dependencies

The only dependency we need to do this is `colorama`. It is used to show text with different colors. For example, if you want to show something really important in the code, you can show it using red.

{% include lazyload.html image_src="https://i.ibb.co/HP8pqp9/Screenshot-2020-11-06-at-12-02-18-AM.png" image_alt="colorama red color" image_title="colorama red color" %}

We can simply install this using `pip`.

```python
pip install colorama
```

## Defining characters to color

We have to define a good length of colors so that we can print the logo with as much space as we want.

```python
fgh ="""ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ
ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWXYZ"""
```

## Defining the data structure and define colors

We will define the data structure with color change information in a dictionary. This will contain the information whenever there is a change in the color for a given row number.

```python
final_dict = {
    # row: { "column with color change": "color after this coloum"...}
    "0": {
        "9": Fore.CYAN,
        "16": Fore.BLACK
    },
    "1": {
        "6": Fore.CYAN,
        "19": Fore.BLACK
    },
    "2": {
        "6": Fore.CYAN,
        "8": Fore.BLACK,
        "10": Fore.CYAN,
        "19": Fore.BLACK
    },
    "3": {
        "6": Fore.CYAN,
        "19": Fore.BLACK
    },
    "4": {
        "6": Fore.CYAN,
        "19": Fore.BLACK
    },
    "5": {
        "2": Fore.CYAN,
        "19": Fore.YELLOW,
        "23": Fore.BLACK
    },
    "6": {
        "1": Fore.CYAN,
        "19": Fore.YELLOW,
        "24": Fore.BLACK
    },
    "7": {
        "1": Fore.CYAN,
        "19": Fore.YELLOW,
        "24": Fore.BLACK
    },
    "8": {
        "1": Fore.CYAN,
        "19": Fore.YELLOW,
        "24": Fore.BLACK
    },
    "9": {
        "1": Fore.CYAN,
        "11": Fore.YELLOW,
        "24": Fore.BLACK
    },
    "10": {
        "1": Fore.CYAN,
        "9": Fore.YELLOW,
        "24": Fore.BLACK
    },
    "11": {
        "1": Fore.CYAN,
        "9": Fore.YELLOW,
        "24": Fore.BLACK
    },
    "12": {
        "2": Fore.CYAN,
        "11": Fore.YELLOW,
        "24": Fore.BLACK
    },
    "13": {
        "3": Fore.CYAN,
        "9": Fore.YELLOW,
        "23": Fore.BLACK
    },
    "14": {
        "6": Fore.YELLOW,
        "19": Fore.BLACK
    },
    "15": {
        "6": Fore.YELLOW,
        "19": Fore.BLACK
    },
    "16": {
        "6": Fore.YELLOW,
        "15": Fore.BLACK,
        "17": Fore.YELLOW,
        "19": Fore.BLACK
    },
    "17": {
        "7": Fore.YELLOW,
        "18": Fore.BLACK
    }
}
```

> Please use `Fore.WHITE` if you are not using dark mode and please tell me why aren't you using the dark mode.

Don't worry if this doesn't make any sense as of now. You will see the usage just in a second.

## Color the characters

```python
def get_dict_color(row, column, last_color):
    row = str(row)
    column = str(column)
    if final_dict.get(row).get(column):
        return final_dict.get(row).get(column)
    return last_color

row = 0
last_color = Fore.BLACK
for b in fgh.split('\n'):
    column = 0
    for c in b:
        last_color = get_dict_color(row, column, last_color)
        print(last_color + c, end="")
        column += 1
    print('\n', end="")
    row += 1
    if row == 18:
        break
```

This is a pretty simple Python code that is used to color the characters. In this code, we are looping over `fgh` variables which contain all the characters.

In each row of characters for each column of characters, we are calling the `get_dict_color` function which either returns the new color if the color is changing according to the defined data structure or returns the last color, we were using.

Run it, and we will get that we first showed you.

There might be a lot of implementations and we would like to hear how you implemented it. 