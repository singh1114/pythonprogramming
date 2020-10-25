---
layout: post
title: Sending tweet to your ISP when your internet speed down
date: 2020-10-25T10:15:57.817Z
updated_date: 2020-10-25T10:15:57.844Z
description: Sending tweet tagging your ISP when your internet speed goes down
  on pythonprogramming.org
published: true
image: https://i.ibb.co/dcK7D0y/carbon-9-1.png
tags:
  - python
  - imageprocessing
categories:
  - python
  - imageprocessing
author_username: ""
show_ads: false
show_telegram_signup: true
skip_toc: false
include_mathjax: false
---
{% include lazyload.html image_src="https://i.ibb.co/PwjTf8s/carbon-9-2.png" image_alt="Sending tweet when your internet speed goes down" image_title="Sending tweet when your internet speed goes down" %}

A lot of people liked the last post on [pythonprogramming.org](https://pythonprogramming.org) and shared a lot of love on [Reddit](https://www.reddit.com/r/Python/comments/jhh8oa/monitor_your_internet_with_python/).

{% include linked_post.html url="monitor-your-internet-with-python" %}

Some comments were really great and gave really actionable advice on what else we can do with this.

One of which was, `sending tweet tagging your ISP about the lower internet speed`.

This inspired me to quickly write this small method which can create an image using `pillow` library with some text in it related to internet speed.

This is how the image looks.

{% include lazyload.html image_src="https://i.ibb.co/crkc9TJ/pil-text-font.png" image_alt="Monitor your internet with python" image_title="Monitor your internet with python" %}

Read more about the [tweet here](https://twitter.com/ranvirsingh1114/status/1320311041046163456?s=20).

## Install pillow and tweepy

```shell
pip install pillow
pip install tweepy
```

{% include linked_post.html url="how-to-create-a-webscraping-twitter-bot-in-python" %}

## Extend the code

```python
import speedtest
import tweepy
from PIL import Image, ImageDraw, ImageFont
import datetime
s = speedtest.Speedtest()
import time
MIN_SPEED = 100


def create_image(downspeed, upspeed):
    img = Image.new('RGB', (1080, 750), color = (73, 109, 137))
    fnt = ImageFont.truetype('/Library/Fonts/Arial.ttf', 50)
    speed_string = f"Internet speed dropped \n Downspeed: {downspeed} \n Upspeed: {upspeed}"
    d = ImageDraw.Draw(img)
    d.text((300,325), speed_string, font=fnt, fill=(255, 255, 0))
    img.save('pil_text_font.png')
    return speed_string

def tweet_image(downspeed, upspeed):
    speed_string = create_image(downspeed, upspeed)
    consumer_key = "#"
    consumer_secret = "#"
    access_token = "#"
    access_token_secret = "#"
    auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
    auth.set_access_token(access_token, access_token_secret)
    api = tweepy.API(auth)
    print('sending tweet')
    api.update_with_media('pil_text_font.png', status=speed_string)
    print('tweet sent')

while True:
    time_now = datetime.datetime.now().strftime("%H:%M:%S")
    downspeed = round((round(s.download()) / 1048576), 2)
    upspeed = round((round(s.upload()) / 1048576), 2)
    print(f"time: {time_now}, downspeed: {downspeed} Mb/s, upspeed: {upspeed} Mb/s")
    if downspeed < MIN_SPEED or upspeed < MIN_SPEED:
        tweet_image(downspeed, upspeed)
    time.sleep(10)
```

I have added two new functions to the old code where we were already computing the internet speed.

These functions do exactly what their name suggests.

`create_image` creates the image using `pillow` and `tweet_image` tweets an image using `tweepy`.

Code for `create_image` is taken from [here](https://code-maven.com/create-images-with-python-pil-pillow).

You can configure the `MIN_SPEED` according to what your ISP( in MB/sec) has promised and add their Twitter handle in the `speed_string`.

Hope you liked the post. Subscribe to the newsletter so that can get notifications of such posts.

One thing that I realized after writing this is what would happen if the internet is down. The tweet might not go and there might be an error raised by `tweepy`. It might be a good thing to test before using it live.