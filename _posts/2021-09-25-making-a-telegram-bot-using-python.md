---
layout: post
title: Making a Telegram bot using Python
date: 2021-09-25T17:37:34.614Z
updated_date: 2021-09-25T17:37:34.665Z
description: In this post, we will start creating a bot from scratch,
  discovering all possible settings to utilize all power of Bot API and creating
  a real-world application. All this by using Python and Telegram Bot.
published: true
image: https://i.ibb.co/b2F1vKx/carbon.png
tags:
  - telegram
  - python
  - bot
categories:
  - telegram
  - python
  - bot
author_name: Gurkirat Singh
author_username: gurkirat
show_ads: false
show_telegram_signup: false
skip_from_listing: false
---
## Prerequisite Knowledge and Requirements

1. Basic python knowledge
2. Telegram Account

## About this post

In this post, we will start creating a bot from scratch, discovering all possible settings to utilize all power of Bot API and creating a real-world application.

All this will be done by using Python and Telegram Bot.

## Codes / Projects

All the snippets and projects related to this course will be found in [`projects`](https://github.com/guides-to/BUILDING-TELEGRAM-BOT-IN-PYTHON/blob/master/projects) directory.

{% include note.html title="Furthur improvements" description="If you will be making any educational project regarding this course, I would like to encourage you to send a PR regarding the same to GitHub repo." %}

## Some examples of telegram bots

```
- Simple telegram accounts operated by an application
- Not actually designed for chatting
- Can be used to simplify the task
  - Teach
  - Play
  - Search
  - Broadcast
  - Remind
  - Connect
- Users can interact with them via
  - Message
  - Emoticons
  - Commands
  - Inline requests
```

## Setting up Telegram Bot

1. Open telegram app and search for `BotFather`
2. Send `/start` command to start and associate the account with yours
3. Send `/help` command to show the help
4. Send `/newbot` command and follow the instruction
5. Send `/mybots` command to list your bot, select the desired bot
6. Click on **API Token**

## Understanding Other Bot Settings

1. Send `/mybots` command to list your bot, select the desired bot
2. Click on **Bot Settings** and you will see various options.
3. Click on [**Inline Mode**](https://core.telegram.org/bots/inline). If you want the bot to accept inline messages beyond the commands.
4. Now you can click on **Back to Bot** button and click on **Edit Bot** button. Here you can customize your bots like _setting a bot description (what it does, what it is used for etc)_, _setting a bot about_ and _profile picture_.

{% include note.html title="Note: Changing the profile picture of bot" description="While changing the profile picture, botfather expects you to upload a valid image file." %}

## Limitation of the Bot

1. Only user can initiate the chat
2. The download limit is 20MB and upload limit is 50MB only
3. Send messages limit is capped up to 30 msg per second. However, you can ask **@botsupport** to increase it.

With this limitation, comes another power, bots can edit their own messages and another's messages in the channel where the bot has admin rights.

## Getting messages from Telegram

There are two ways to get user messages from telegram.

**Polling**
It's a way of asking Telegram every _X_ seconds whether any new message has come, and receive messages or asks later from telegram.

**Web Hooks**
It's a way in which telegram sends all updates to the URL that has been configured to accept messages as soon as the user sends new messages.

## HTTP Interface of Bots API

Documentation: https://core.telegram.org/bots/api <br>
Available Method: https://core.telegram.org/bots/api#available-methods <br>
Base URL: https://api.telegram.org/bot[BOT_API_KEY]/[methodName] <br>

### Getting bot description

Method Name: [`getMe`](https://core.telegram.org/bots/api#getme)

In my case,

```json
{
  "ok": true,
  "result": {
    "id": 920296790,
    "is_bot": true,
    "first_name": "TELE BOT",
    "username": "tel_ebot",
    "can_join_groups": true,
    "can_read_all_group_messages": false,
    "supports_inline_queries": false
  }
}
```

### Getting new messages

Available Methods: https://core.telegram.org/bots/api#getting-updates
Method Name: [`getUpdates`](https://core.telegram.org/bots/api#getupdates)

In my case

```json
{
  "ok": true,
  "result": [
    {
      "update_id": 643663200,
      "message": {
        "message_id": 3,
        "from": {
          "id": 517852228,
          "is_bot": false,
          "first_name": "Gurkirat",
          "last_name": "Singh",
          "username": "username_of_the_sender",
          "language_code": "en"
        },
        "chat": {
          "id": 517852228,
          "first_name": "Gurkirat",
          "last_name": "Singh",
          "username": "username_of_the_sender",
          "type": "private"
        },
        "date": 1586779436,
        "text": "hello"
      }
    }
  ]
}
```

More methods on Get Updates: https://core.telegram.org/bots/api#getting-updates

## Sending Messages to the User

Method Name: [`sendMessage`](https://core.telegram.org/bots/api#sendmessage)

In this section, we will create an echo bot that will be listening to all the messages sent by the user. We will then read the last message (`-1` index in the python list) and send the same message with the format **You send me {MESSAGE_TEXT}** message.

The code is self-explanatory, and I have added some comments. However, if you couldn't get anything, leave a comment.

```python
import urllib.request as request
from urllib.error import HTTPError
from http.client import HTTPResponse
from typing import Dict, List, Union
import json
from datetime import datetime
import signal
import os

signal.signal(signal.SIGINT, signal.SIG_DFL)


class TelegramEcho:
    def __init__(self, TG_KEY: str):
        self.TG_URL = "https://api.telegram.org/bot{key}/{method}"
        self.TG_KEY = TG_KEY

        self.__last = None
        self.__last_time = None
        pass

    def run(self):
        """
        method to handle the incoming message and the send echo message to the user
        """
        while True:
            try:
                # getting the incoming data
                incoming = self.__handle_incoming()

                # checking if incoming message_id is same as of last, then skip
                if self.__last == incoming["message"]["message_id"]:
                    continue
                else:
                    self.__last = incoming["message"]["message_id"]

                # adding more validation to prevent messaging the last message whenever the polling starts
                if not self.__last_time:
                    self.__last_time = incoming["message"]["date"]
                    continue
                elif self.__last_time < incoming["message"]["date"]:
                    self.__last_time = incoming["message"]["date"]
                else:
                    continue

                # finally printing the incoming message
                self.__print_incoming(incoming)

                # now sending the echo message
                outgoing = self.__handle_outgoing(
                    incoming["message"]["chat"]["id"],
                    incoming["message"]["text"])

                # finally printing the outgoing message
                self.__print_outgoing(outgoing)

                pass
            except (HTTPError, IndexError):
                continue
            pass
        pass

    def __handle_incoming(self) -> Dict[str, Union[int, str]]:
        """
        method fetch the recent messages
        """

        # getting all messages
        getUpdates = request.urlopen(
            self.TG_URL.format(key=self.TG_KEY, method="getUpdates"))

        # parsing results
        results: List[Dict[str, Union[int, str]]] = json.loads(
            getUpdates.read().decode())["result"]

        # getting the last error
        return results[-1]

    def __print_incoming(self, incoming: Dict[str, Union[int, str]]):
        """
        method to print the incoming message on console
        """
        print("[<<<] Message Recieved on %s" % datetime.fromtimestamp(
            incoming["message"]["date"]).strftime("%Y-%m-%d %H:%M:%S"))
        print("\tText: %s" % incoming["message"]["text"])
        print("\tFrom: %s" %
              incoming["message"]["from"].get("first_name", "") + " " +
              incoming["message"]["from"].get("last_name", ""))
        print("\tMessage ID: %d" % incoming["message"]["message_id"])
        print("-" * os.get_terminal_size().columns)
        pass

    def __handle_outgoing(self, chat_id: int,
                          message_txt: str) -> Dict[str, Union[int, str]]:
        """
        method to send the echo message to the same chat
        """

        # making the post data
        _data: Dict[str, Union[int, str]] = {
            "chat_id":
            chat_id,
            "text":
            "You sent me \"{MESSAGE_TEXT}\"".format(MESSAGE_TEXT=message_txt)
        }

        # creating the request
        _request: request.Request = request.Request(
            self.TG_URL.format(key=self.TG_KEY, method="sendMessage"),
            data=json.dumps(_data).encode('utf8'),
            headers={"Content-Type": "application/json"})

        # sending HTTP request, for sending message to the user
        sendMessage: HTTPResponse = request.urlopen(_request)
        result: Dict[str, Union[int, str]] = json.loads(
            sendMessage.read().decode())["result"]
        return result

    def __print_outgoing(self, outgoing):
        """
        method to print outgoing data on the console
        """
        print("[>>>] Message Send on %s" % datetime.fromtimestamp(
            outgoing["date"]).strftime("%Y-%m-%d %H:%M:%S"))
        print("\tText: %s" % outgoing["text"])
        print("\tFrom: %s" % outgoing["from"]["first_name"])
        print("\tMessage ID: %d" % outgoing["message_id"])
        print("-" * os.get_terminal_size().columns)
        pass

    pass


if __name__ == "__main__":
    tg = TelegramEcho("YOUR API KEY")
    tg.run()
```

Make sure you add the API Key before running this code. After running the above code, you will get the output like below,

{% include lazyload.html image_src="https://i.ibb.co/WfymnkJ/image.png" image_alt="Telegram bot message sending using python" image_title="Telegram bot message sending using python" %}

## Telegram Bot Libraries

As you have seen, telegram provides a plethora of methods and APIs to handle the bot through any language, the open-source community has developed a python client using these API under the hood to increase developer's productivity.

The one I will be using is [python-telegram-bot](https://github.com/python-telegram-bot/python-telegram-bot). While making this course, it has approx _10k stars_.

**Installing the Module**

```sh
# via pip
pip install -U python-telegram-bot

# via conda
conda install -c conda-forge python-telegram-bot
```

**Glance of the Usage**

In this I will be implementing `get_me` method, returning the bot details.

```python
from telegram import Bot

# initializing the bot with API
bot = Bot("API KEY")

# getting the bot details
print(bot.get_me())
```

How fascinating, the developers encapsulated all the backend, letting other developers write neat and clone. It not only provides neat code but also makes payment processing, handlers, and other more features even beyond sending HTTP Requests.

The complete documentation of python-telegram-bot: https://python-telegram-bot.readthedocs.io/en/stable/.

## Using Handlers on Commands

I hope you would have created some commands, now let's make the bot handle those.

The library `python-telegram-bot` provides various [Handlers](https://python-telegram-bot.readthedocs.io/en/latest/telegram.html#handlers) from which I will be using [CommandHandler](https://python-telegram-bot.readthedocs.io/en/latest/telegram.ext.commandhandler.html#telegram-ext-commandhandler)

```python
from telegram.ext.commandhandler import CommandHandler
from telegram.ext.updater import Updater
from telegram.ext.dispatcher import Dispatcher
from telegram.update import Update
from telegram.ext.callbackcontext import CallbackContext
from telegram.bot import Bot

# initializing an updator
# documentation: https://python-telegram-bot.readthedocs.io/en/latest/telegram.ext.updater.html#telegram.ext.Updater
updater = Updater("API KEY",
                  use_context=True)

# getting the dispatcher required to handle the command /start and send message back to the user
# documentation: https://python-telegram-bot.readthedocs.io/en/latest/telegram.ext.dispatcher.html#telegram.ext.Dispatcher
dispatcher: Dispatcher = updater.dispatcher


def start(update: Update, context: CallbackContext):
    """
    the callback for handling start command
    """
    # getting the bot from context
    # documentation: https://python-telegram-bot.readthedocs.io/en/latest/telegram.bot.html#telegram-bot
    bot: Bot = context.bot

    # sending message to the chat from where it has received the message
    # documentation: https://python-telegram-bot.readthedocs.io/en/latest/telegram.bot.html#telegram.Bot.send_message
    bot.send_message(chat_id=update.effective_chat.id,
                     text="You have just entered start command")


# register a handler (here command handler)
# documentation: https://python-telegram-bot.readthedocs.io/en/latest/telegram.ext.dispatcher.html#telegram.ext.Dispatcher.add_handler
dispatcher.add_handler(
    # it can accept all the telegram.ext.Handler, CommandHandler inherits Handler class
    # documentation: https://python-telegram-bot.readthedocs.io/en/latest/telegram.ext.commandhandler.html#telegram-ext-commandhandler
    CommandHandler("start", start))

# starting polling updates from Telegram
# documentation: https://python-telegram-bot.readthedocs.io/en/latest/telegram.ext.updater.html#telegram.ext.Updater.start_polling
updater.start_polling()
```

## Posting HTML Message

In this I will show how to send an HTML formatted message to the user. I will be using [`telegram.ParseMode.HTML`](https://python-telegram-bot.readthedocs.io/en/stable/telegram.parsemode.html#telegram.ParseMode.HTML) parser for with [`send_message`](https://python-telegram-bot.readthedocs.io/en/stable/telegram.bot.html#telegram.Bot.send_message) method.

Updating just one send_message line in the above code.

```python
from telegram.ext.commandhandler import CommandHandler
from telegram.ext.updater import Updater
from telegram.ext.dispatcher import Dispatcher
from telegram.update import Update
from telegram.ext.callbackcontext import CallbackContext
from telegram.bot import Bot
from telegram.parsemode import ParseMode

updater = Updater("API KEY",
                  use_context=True)

dispatcher: Dispatcher = updater.dispatcher


def start(update: Update, context: CallbackContext):
    """
    the callback for handling start command
    """
    bot: Bot = context.bot

    # Added HTML Parser to the existing command handler
    # documentation: https://python-telegram-bot.readthedocs.io/en/stable/telegram.parsemode.html#telegram.ParseMode.HTML
    bot.send_message(
        chat_id=update.effective_chat.id,
        text=
        "Hello User, You have used <b>start</b> command. Search about developer on google, <a href='https://www.google.com/search?q=tbhaxor'>@tbhaxor</a>",
        parse_mode=ParseMode.HTML,
    )


dispatcher.add_handler(CommandHandler("start", start))

updater.start_polling()
```

The message user will receive will be the rendered HTML you have passed in `text` argument

## Using Keyboards to Build Menus

In this, you will embrace the power of another tool provided by telegram, Keyboard. Telegram has two types plain keyboard and inline, the first is attached to the group or chat. The second one is attached to the message

### Simple Keyboard

In this, I will be using [`telegram.ReplyKeyboardMarkup`](https://python-telegram-bot.readthedocs.io/en/stable/telegram.replykeyboardmarkup.html) to add new keyboard to the chat and [`telegram.ReplyKeyboardRemove`](https://python-telegram-bot.readthedocs.io/en/stable/telegram.replykeyboardremove.html) to remove the keyboard from the chat

```py
from telegram.ext.updater import Updater
from telegram.update import Update
from telegram.ext.callbackcontext import CallbackContext
from telegram.ext.commandhandler import CommandHandler
from telegram.replykeyboardmarkup import ReplyKeyboardMarkup
from telegram.replykeyboardremove import ReplyKeyboardRemove
from telegram.ext.messagehandler import MessageHandler
from telegram.ext.filters import Filters

updater = Updater("API KEY", use_context=True)


def start(update: Update, context: CallbackContext):
    """
    method to handle the /start command and create keyboard
    """

    # defining the keyboard layout
    kbd_layout = [['Option 1', 'Option 2'], ['Option 3', 'Option 4'],
                       ["Option 5"]]

    # converting layout to markup
    # documentation: https://python-telegram-bot.readthedocs.io/en/stable/telegram.replykeyboardmarkup.html
    kbd = ReplyKeyboardMarkup(kbd_layout)

    # sending the reply so as to activate the keyboard
    update.message.reply_text(text="Select Options", reply_markup=kbd)


def remove(update: Update, context: CallbackContext):
    """
    method to handle /remove command to remove the keyboard and return back to text reply
    """

    # making a reply markup to remove keyboard
    # documentation: https://python-telegram-bot.readthedocs.io/en/stable/telegram.replykeyboardremove.html
    reply_markup = ReplyKeyboardRemove()

    # sending the reply so as to remove the keyboard
    update.message.reply_text(text="I'm back.", reply_markup=reply_markup)
    pass


def echo(update: Update, context: CallbackContext):
    """
    message to handle any "Option [0-9]" Regrex.
    """
    # sending the reply message with the selected option
    update.message.reply_text("You just clicked on '%s'" % update.message.text)
    pass


updater.dispatcher.add_handler(CommandHandler("start", start))
updater.dispatcher.add_handler(CommandHandler("remove", remove))
# adding the message handler with filter to handle the Option [0-9] regex input
# documentation for MessageHandler: https://python-telegram-bot.readthedocs.io/en/stable/telegram.ext.messagehandler.html
# documentation for Filter: https://python-telegram-bot.readthedocs.io/en/stable/telegram.ext.filters.html#telegram.ext.filters.Filters
updater.dispatcher.add_handler(MessageHandler(Filters.regex(r"Option [0-9]"), echo))

updater.start_polling()
```

### Inline Keyboard

In this I will be using [`telegram.ext.CallbackQueryHandler`](https://python-telegram-bot.readthedocs.io/en/stable/telegram.ext.callbackqueryhandler.html) which returns a [`telegram.CallbackQuery`](https://python-telegram-bot.readthedocs.io/en/stable/telegram.callbackquery.html#telegram-callbackquery) when user presses an [`telegram.InlineKeyboardButton`](https://python-telegram-bot.readthedocs.io/en/stable/telegram.inlinekeyboardbutton.html)

```py
from telegram import InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext.updater import Updater
from telegram.ext.commandhandler import CommandHandler
from telegram.ext.callbackqueryhandler import CallbackQueryHandler
from telegram.callbackquery import CallbackQuery
from telegram.ext.callbackcontext import CallbackContext
from telegram.update import Update
from telegram.message import Message
import sys

# creating updater
updater: Updater = Updater("API KEY",
                           use_context=True)


def error(update: Update, context: CallbackContext):
    """Log Errors caused by Updates."""
    sys.stderr.write("ERROR: '%s' caused by '%s'" % context.error, update)
    pass


def start(update: Update, context: CallbackContext):
    """
    callback method handling /start command
    """

    # creating list of input buttons
    # documentation: https://python-telegram-bot.readthedocs.io/en/stable/telegram.inlinekeyboardbutton.html
    keyboard = [[
        InlineKeyboardButton("Option 1", callback_data='1'),
        InlineKeyboardButton("Option 2", callback_data='2')
    ], [InlineKeyboardButton("Option 3", callback_data='3')]]

    # creating a reply markup of inline keyboard options
    # documentation: https://python-telegram-bot.readthedocs.io/en/stable/telegram.inlinekeyboardmarkup.html
    reply_markup = InlineKeyboardMarkup(keyboard)

    # sending the message to the current chat id
    # documentation: https://python-telegram-bot.readthedocs.io/en/stable/telegram.message.html#telegram.Message.reply_text
    update.message.reply_text('Please choose:', reply_markup=reply_markup)
    pass


def button(update, context):
    """
    callback method handling button press
    """
    # getting the callback query
    # documentation: https://python-telegram-bot.readthedocs.io/en/stable/telegram.callbackquery.html
    query: CallbackQuery = update.callback_query

    # CallbackQueries need to be answered, even if no notification to the user is needed
    # Some clients may have trouble otherwise. See https://core.telegram.org/bots/api#callbackquery
    # documentation: https://python-telegram-bot.readthedocs.io/en/stable/telegram.callbackquery.html#telegram.CallbackQuery.answer
    query.answer()

    # editing message sent by the bot
    # documentation: https://python-telegram-bot.readthedocs.io/en/stable/telegram.callbackquery.html#telegram.CallbackQuery.edit_message_text
    query.edit_message_text(text="Selected option: {}".format(query.data))


# adding listeners
updater.dispatcher.add_handler(CommandHandler('start', start))  # handling /start command
updater.dispatcher.add_handler(CallbackQueryHandler(button))  # handling inline buttons pressing
updater.dispatcher.add_error_handler(error)  # error handling

# started polling
updater.start_polling()
```

## Sending Force Replies

Upon recieving messages from user, telegram bot will display a reply interface to the user like if user has selected the bot message to reply back. This could be used in creating step-by-step user friendly interface.

I will be using [`telegram.ForceReply`](https://python-telegram-bot.readthedocs.io/en/stable/telegram.forcereply.html) to send force reply

```py
from telegram.forcereply import ForceReply
from telegram.ext.filters import Filters
from telegram.ext.updater import Updater
from telegram.ext.messagehandler import MessageHandler
from telegram.ext.callbackcontext import CallbackContext
from telegram.update import Update

updater = Updater("API KEY", use_context=True)

def echo(update: Update, context: CallbackContext):
    # sending the force reply to the user
    # documentation: https://python-telegram-bot.readthedocs.io/en/stable/telegram.forcereply.html
    update.message.reply_text(reply_markup=ForceReply(selective=True), text="Reply to this message")
    pass

updater.dispatcher.add_handler(MessageHandler(Filters.text, echo))

updater.start_polling()
```

## Chat Action

Now suppose you created a bot that processes some files / information that could take some time. The user might think bot is broken and close it. The telegram provides very cool option **ChatActions** to send user messages immediately after his/her submission.

I will be using [`telegram.Bot.send_chat_action`](https://python-telegram-bot.readthedocs.io/en/stable/telegram.bot.html#telegram.Bot.send_chat_action) along with [`telegram.ChatAction`](https://python-telegram-bot.readthedocs.io/en/stable/telegram.chataction.html).

```py
from telegram.bot import Bot
from telegram.update import Update
from telegram.ext.updater import Updater
from telegram.ext.callbackcontext import CallbackContext
from telegram.ext.messagehandler import MessageHandler
from telegram.ext.filters import Filters
from telegram.chataction import ChatAction
from time import sleep

updater = Updater("API KEY", use_context=True)

def echo(update: Update, context: CallbackContext):
    # sending the chat action, under the name of bot it will show Typing...
    # documentation: https://python-telegram-bot.readthedocs.io/en/stable/telegram.chataction.html
    context.bot.send_chat_action(chat_id=update.effective_chat.id, action=ChatAction.TYPING)
    
    # simulating some long processing
    sleep(3)

    # sending reply when it's done
    update.message.reply_text(text="Hey ya!! You sent me '%s'" % update.message.text)

updater.dispatcher.add_handler(MessageHandler(Filters.text, echo))

updater.start_polling()
```

## Deep Linking
Telegram also supports deeplinking. It helps to create a refereal system to promote your bot or products via bot. 

I will be using [`telegram.utils.helpers.create_deep_linked_url`]( https://python-telegram-bot.readthedocs.io/en/stable/telegram.utils.helpers.html#telegram.utils.helpers.create_deep_linked_url) to create a deep link.

```python
from telegram.bot import Bot
from telegram.utils.helpers import create_deep_linked_url
from telegram.ext.commandhandler import CommandHandler
from telegram.ext.callbackcontext import CallbackContext
from telegram.update import Update
from telegram.ext.updater import Updater
import re

updater = Updater("API KEY", use_context=True)

def generate(update: Update, context: CallbackContext):
    """
    method to create a deep link and send it to the current user for sharing
    """

    # generating a sharable link with the payload
    # documentation: https://python-telegram-bot.readthedocs.io/en/stable/telegram.utils.helpers.html#telegram.utils.helpers.create_deep_linked_url
    url = create_deep_linked_url(context.bot.get_me().username, update.message.chat.username)
    update.message.reply_text(text="Share it with your friends: %s.\n Copy the link and share it with them" % url)
    pass


def start(update: Update, context: CallbackContext):
    """
    method to run on 
    """

    # extracting the payload with /start
    _ = re.findall(r"(?:/start )(.+)", update.message.text)

    # checking if it exists and sending message accordingly
    if len(_) > 0:
        update.message.reply_text(text="You have been refered by: %s" % _[0])
        pass
    else:
        update.message.reply_text(text="Hello, It seems you are new to this bot")
        pass

    update.message.reply_text(text="Use /generate to create your own referal")
    pass


updater.dispatcher.add_handler(CommandHandler("generate", generate))
updater.dispatcher.add_handler(CommandHandler("start", start))

updater.start_polling()
```


## Conclusion

Thanks for reading this course, I would encourage you to explore this wonderful telegram client yourself. I would be happy to see your creations after pursuing this tutorial. In future, if I planned to add more snippets and projects, you know where to find it. Yes, [here](https://github.com/guides-to/BUILDING-TELEGRAM-BOT-IN-PYTHON/blob/master/projects)

If you need any help regarding this, you can join their telegram group https://t.me/pythontelegrambotgroup, or you can contact me on any of the following