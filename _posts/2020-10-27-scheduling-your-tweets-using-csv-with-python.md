---
layout: post
title: Scheduling your tweets using CSV with Python
date: 2020-10-27T18:01:29.985Z
updated_date: 2020-10-27T18:01:30.111Z
description: Scheduling your tweets using CSV with Python, Pandas, tweepy and arrow
published: true
image: https://i.ibb.co/0Z6rvhW/carbon-1.png
tags:
  - python
  - pandas
categories:
  - python
  - pandas
author_name: Ranvir Singh
author_username: ranvir_xyz
show_ads: false
show_telegram_signup: true
---
{% include lazyload.html image_src="https://i.ibb.co/Z6mw1sp/carbon-2.png" image_alt="Scheduling your tweets using CSV with Python" image_title="Scheduling your tweets using CSV with Python" %}

I have recently created a [new account on Twitter](https://twitter.com/pythonprogramm9)( please follow if you like what I post) and looking for ways to increase my followers. To achieve this, I read a lot of articles on how to get more followers.

Each and every article had one common thing in all of them.

**Tweet regularly**

First, I want to clarify that I am not a big fan of all the sh*t that goes on over Twitter where a user who started programming 7 days ago acts as an expert and is having a ton of followers and on the other hand, people with a lot of experience has none.

It doesn't matter if I like it or not, I have to increase the number of followers to sound more genuine and the key ingredient for that is `tweeting regularly`.

But it's hard to find time for tweeting, especially when it doesn't come naturally to you. It's pretty hard to come up with the content itself and it is even hard to tweet about it.

Now I have written a Django app that can do the same thing. Basically, you add tweet's data in the database and the script can read everything from there and send the tweets automatically. So, if you know Django, you can head over to the other post and follow that instead. This post is for the people who don't want any complexity.

{% include linked_post.html url="scheduling-a-tweet-using-python-and-django" %}

This post is not using Django and you can easily follow even if you only know basic Python.

## Prerequisites

We will need to install a few libraries that will make our life easier.

```shell
pip install arrow
pip install tweepy
pip install pandas
pip install apscheduler
```

We use `arrow` to do some date-time based conversions, `tweepy` to handle tweeting, pandas for working on `CSV`s and `apscheduler` to schedule the process.

We are using Python 3.8 while running the code in the post. 

## Twitter credentials

We will need to have Twitter credentials to send tweets programmatically. For that, you can head to the [Twitter developers](https://developer.twitter.com) dashboard and create an App.

{% include lazyload.html image_src="https://i.ibb.co/7xcw2WD/Screenshot-2020-10-27-at-11-54-21-PM.png" image_alt="Twitter create dev app" image_title="Twitter create dev app" %}

We will need API key, API secret, Access token and Token secret.

{% include lazyload.html image_src="https://i.ibb.co/Jm4Ns3N/Screenshot-2020-10-27-at-11-59-08-PM.png" image_alt="Twitter tokens" image_title="Twitter tokens" %}

Once you have them, you can export them as environment variables.

```shell
export CONSUMER_KEY=""
export CONSUMER_SECRET=""
export ACCESS_TOKEN=""
export TOKEN_SECRET=""
```

You have to keep exporting them whenever you open a new shell window or you can [use direnv](https://ranvir.xyz/blog/dir-env-to-create-environment-variables-in-ubuntu/) to make your life easier.

If you are still facing any issue, you can head over to our other post which explains this in little more detail.

{% include linked_post.html url="how-to-create-a-webscraping-twitter-bot-in-python" %}

## The code

We will have to create a file to keep all of our code.

```shell
touch csv_tweet.py
```

then copy this code to the file.

```python
import logging
import arrow
import tweepy
import pandas as pd
from apscheduler.schedulers.background import BlockingScheduler

logging.basicConfig(filename='./SchedulerLog.txt', level=logging.INFO)
logger = logging.getLogger(__name__)


def send_tweets(consumer_key, consumer_secret, access_token,
                access_token_secret, tweet):
    auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
    auth.set_access_token(access_token, access_token_secret)
    api = tweepy.API(auth)
    logging.info('sending tweet')
    api.update_status(tweet)
    logging.info('tweet sent')

def switch_to_arrow(df):
    df['tweet_at'] = arrow.get(df['tweet_at'])
    return df

def get_non_sent_tweets(consumer_key, consumer_secret, access_token, access_token_secret):
    df = pd.read_csv('tweets.csv')
    new_df = df.apply(switch_to_arrow, axis=1)
    applicable_rows = new_df[(new_df['sent']==False) & (new_df['tweet_at'] < arrow.utcnow())]
    if len(applicable_rows) == 0:
        return
    for index, data in applicable_rows.iterrows():
        send_tweets(
            consumer_key, consumer_secret, access_token, access_token_secret, data['tweet'])
        new_df.loc[new_df['sno'] == data['sno'], 'sent'] = True
    new_df.to_csv('tweets.csv', index=False)


def tweet_scheduler():
    if (os.environ.get('CONSUMER_KEY') or os.environ.get('CONSUMER_SECRET') or
       os.environ.get('ACCESS_TOKEN') or os.environ.get('TOKEN_SECRET')):
        consumer_key = os.environ.get('CONSUMER_KEY')
        consumer_secret = os.environ.get('CONSUMER_SECRET')
        access_token = os.environ.get('ACCESS_TOKEN')
        access_token_secret = os.environ.get('TOKEN_SECRET')
        get_non_sent_tweets(consumer_key, consumer_secret, access_token, access_token_secret)
    else:
        raise NotImplementedError('Set environment variables correctly')

if __name__ == '__main__':
    scheduler = BlockingScheduler()
    scheduler.add_job(
        tweet_scheduler, 'interval', minutes=5)
    logger.info('starting scheduler')
    scheduler.start()
```

Wow, that's a lot of code, let's break it down a little and go over what each function does.

```shell
tweet_scheduler is the entry function, all it does is check if the Twitter creds are present in the environment variables or not.

get_non_sent_tweets does most of the work.
1. read the data from the CSV
2. calls the switch_to_arrow function which converts the tweet_at field to a more comparison-suited arrow object.
3. Then it checks if any of the rows exists whose tweet_at time has passed and its sent key is still False.
4. Send tweets for these rows.
5. Update the CSV.

Finally, in the `__main__` part we schedule the entry function to run every 5 minutes.
```

## The CSV file

```shell
sno,tweet,tweet_at,sent
1,First Tweet,2020-10-26 18:55,False
2,Second Tweet,2020-10-26 19:00,True
3,Second Tweet,2020-10-29 19:00,False
```

This is how the CSV file looks like.

One thing that we have to keep in mind is the timezone being used. Most probably it's going to be UTC. So, keep it in mind.

## Send the tweets

You can start the script using the command

```shell
python csv_tweet.py
```

We will be able to see the logs coming in `SchedulerLog.txt` file in the same directory. We can tail for the new logs using the command,

```shell
tail -f SchedulerLog.txt
```

{% include lazyload.html image_src="https://i.ibb.co/9mxbF04/Screenshot-2020-10-28-at-12-33-49-AM.png" image_alt="App scheduler logs" image_title="App scheduler logs" %}

## Further steps

One of the further things that we can do with this is running this as a web app, but that is something that we have [already done](https://pythonprogramming.org/scheduling-a-tweet-using-python-and-django/).

Not sure what else can we do with this. Let me know if you have something in mind.

## How I am using it?

I get a notification when someone posts something related to Python on HackerNews. I create a new entry in this CSV with the Twitter handle of the user who posted with 2 hrs gap of the last tweet time and the text is tweeted after that time is passed.

Let me know if someone needs this script.

Subscribe to get more such posts delivered to your inbox.