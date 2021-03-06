---
layout: post
title: Scheduling a tweet using Python and Django
date: 2020-10-25T17:37:26.355Z
updated_date: 2020-10-25T17:37:26.410Z
description: Scheduling a tweet using Python and Django, using tweet model,
  tweepy and many other things.
published: true
image: https://i.ibb.co/m6zzWm4/carbon-10-1.png
tags:
  - python
  - django
  - webapp
categories:
  - python
  - django
  - webapp
author_name: Ranvir Singh
author_username: ranvir_xyz
show_ads: false
show_telegram_signup: false
---
{% include lazyload.html image_src="https://i.ibb.co/mtLdCSr/carbon-10-2.png" image_alt="Scheduling a tweet using Python and Django" image_title="Scheduling a tweet using Python and Django" %}

I have recently created a [new account on Twitter](https://twitter.com/pythonprogramm9) and looking for ways to increase my followers. To achieve this, I read a lot of articles on how to get more followers.

Each and every article had one common thing in all of them.

**Tweet regularly**

First, I want to clarify that I am not a big fan of all the sh*t that goes on over Twitter where a user who started programming 7 days ago acts as an expert and is having a ton of followers and on the other hand, people with a lot of experience has none.

It doesn't matter if I like it or not, I have to increase the number of followers to sound more genuine and the key ingredient for that is `tweeting regularly`.

But it's hard to find time for tweeting, especially when it doesn't come naturally to you. It's pretty hard to come up with the content itself and it is even hard to tweet about it.

To solve this, I tried to come up with a web app in which I can schedule tweets for say the next seven days or so.

I recently read the post written by my fellow writer on how to send tweets using `tweepy` and was really confident that I can do it.

{% include linked_post.html url="how-to-create-a-webscraping-twitter-bot-in-python" %}

## The problem statement

It is really important to understand what we want to achieve otherwise we end up building something totally different.

So, I divided the projects into small chunks so that it is easily achievable.

* **Persistence**

We wanted to store things like `tweet data`, `tweeting date and time` and keeping the log of what all tweets have already been `shared`.

* **Scheduling**

I wanted to use something simple to start with. `Consumer-producers` type scheduling was too much for this small project.

* **Documentation**

Documentation is an essential part of the open-source software if you want others to use what you have built. This post itself is a part of the documentation process.

## Persistence

I decided to use the already present `sqlite` database to store the related data for tweets.

This is how the `models.py` file looks for the `scheduler` app.

```python
from django.db import models


class TwitterSchedulerModel(models.Model):
    tweet = models.TextField(max_length=240)
    tweet_at = models.DateTimeField()
    created_at = models.DateTimeField(auto_now_add=True, editable=False)
    sent = models.BooleanField(default=False)

```

For those who don't understand much about Django, this part of the code says that we want to create a table with 4 things added to it.

* tweet - The tweet itself
* tweet_at - When does someone want to tweet.
* created_at - A datetime thing to know when you created the data entry( very helpful for debugging purposes).
* send - Whether the tweet was sent or not.

## Scheduling

I decided to use [apscheduler](https://apscheduler.readthedocs.io/en/stable/) for this small use case.

This is how the code of the scheduler looks like.

```python
import logging
import arrow
import os
import tweepy

from twitter.models import TwitterSchedulerModel

logger = logging.getLogger(__name__)


def send_tweets(consumer_key, consumer_secret, access_token,
                access_token_secret):
    expired_tweets = TwitterSchedulerModel.objects.filter(
        sent=False, tweet_at__lte=arrow.utcnow().datetime)
    auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
    auth.set_access_token(access_token, access_token_secret)
    api = tweepy.API(auth)
    if expired_tweets.count() == 0:
        logging.info("No tweet to send as of now")
    for db_tweet in expired_tweets:
        logging.info('sending tweet')
        api.update_status(db_tweet.tweet)
        logging.info('tweet sent')
        db_tweet.sent = True
        db_tweet.save()


def tweet_scheduler():
    if (os.environ.get('CONSUMER_KEY') or os.environ.get('CONSUMER_SECRET') or
       os.environ.get('ACCESS_TOKEN') or os.environ.get('TOKEN_SECRET')):
        consumer_key = os.environ.get('CONSUMER_KEY')
        consumer_secret = os.environ.get('CONSUMER_SECRET')
        access_token = os.environ.get('ACCESS_TOKEN')
        access_token_secret = os.environ.get('TOKEN_SECRET')
        send_tweets(
            consumer_key, consumer_secret, access_token, access_token_secret)
    else:
        raise NotImplementedError('Set environment variables correctly')
```

The `tweet_scheduler` is the entry function. First, we are checking whether the environment variables are set or not.

After that, we are calling the `send_tweet` function where we get all the database entries for which `sent_tweet` time has already passed and use the `tweepy` library to send the tweet. Finally, we update the database to save the updated value.

## How to install and set it up for yourself

### Clone the repo

The first step is to clone the repo.

[https://github.com/singh1114/schedule-tweet](https://github.com/singh1114/schedule-tweet)

```shell
git clone git@github.com:singh1114/schedule-tweet.git
```

### Install the requirements

```shell
cd schedule-tweet
```

Turn on the `virtualenv` and install the requirements. Please read a little about virtual environment if you don't know about it.

```shell
pip install -r requirements.txt
```

### Run migrations to set up the database

```shell
cd scheduletweet
python manage.py migrate
```

### Create a super user

```shell
python manage.py createsuperuser
```

### Export the creds

Now if you were following the tutorial correctly and if you have read the `tweepy` post that I shared earlier, you should have your Twitter API key and other creds handy.

If you haven't read it already, you can visit [https://developer.twitter.com/](https://developer.twitter.com/) and create an app to get all the credentials or you can read [this post](https://pythonprogramming.org/how-to-create-a-webscraping-twitter-bot-in-python/) if you are facing problems to figure it out. If you are still facing any issue, feel free to ping me anywhere.

Once you have them you have to export them to your shell,

```shell
export CONSUMER_KEY="#"
export CONSUMER_SECRET="#"
export ACCESS_TOKEN="#"
export TOKEN_SECRET="#"
```

You have to keep exporting them whenever you open a new shell window or you can [use direnv](https://ranvir.xyz/blog/dir-env-to-create-environment-variables-in-ubuntu/) to make your life easier.

### Run the server

```shell
python manage.py runserver
```

### Open the admin window and schedule a tweet after sometime

Visit [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin) to login with `superuuser` creds and [twitterschedulermodel](http://127.0.0.1:8000/admin/twitter/twitterschedulermodel/) to create a new scheduled tweet.

{% include lazyload.html image_src="https://i.ibb.co/Hhmyk1b/Screenshot-2020-10-26-at-1-16-28-AM.png" image_alt="Scheduling a tweet in django admin" image_title="Scheduling a tweet in django admin" %}

If the time has passed and the tweet is still in the `not_sent` state, it will trigger the tweet in the next 5 minutes when the scheduler runs.

That's it for today's post. If you find an issue while trying it out, do let me know in the comments section, I will be happy to help.

## Possible improvements

The current system doesn't allow us to send images in the tweets, we can accommodate for that as well. I don't think that will be hard. Feel free to raise Pull Requests to the repo if you find any issue.

## Usages

We have used the software to send 50 tweets as of now and gained almost 18 followers to our [profile](https://twitter.com/pythonprogramm9) organically.