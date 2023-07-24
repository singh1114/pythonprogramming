---
layout: post
title: Managing Personal Finances Made Easy with Splitwired: A Guide to Analyzing and Visualizing Expenses
date: 2023-07-24T17:37:34.614Z
updated_date: 2023-07-24T17:37:34.614Z
description: Discover how to take control of your personal finances with Splitwired, a powerful project that utilizes Splitwise APIs to dynamically analyze and visualize your expenses. Learn how to leverage Python, Flask, and Gunicorn to build the backend and React with Plotly for the frontend. Find out how to generate graphs, create expense tables, and even convert currencies using Yahoo Finance data. Take charge of your finances like never before with Splitwired and make informed financial decisions effortlessly.
published: true
image: https://i.ibb.co/F53sSjG/carbon.png
tags:
  - flask
  - python
  - bot
categories:
  - flask
  - python
  - bot
author_name: Daniele Falchetti
author_username: daniele
show_ads: false
show_telegram_signup: false
skip_from_listing: false
---
About a year ago, I started following some blogs that talk about personal finance. I immediately became interested in the topic and, after reading a lot of stuff, I felt the need to start putting into practice what I had learned from all those articles. The first obstacle was that I didn't have any data on my expenses, no Excel sheet, no database... or maybe I did. Actually, yes, because practically my wife and I have been using this fantastic application called [**Splitwise**](https://secure.splitwise.com/) on a daily basis. It simplifies our lives a lot.

Okay, I have the data, but (second obstacle) I can't read it dynamically. The only thing Splitwise offers me with a free account is the ability to download a CSV file containing all the expenses of a single group. It's not enough for me to create graphs, apply formulas, and so on.

I immediately thought of using the Splitwise APIs, which are available for free: [https://dev.splitwise.com/](https://dev.splitwise.com/) ❤️ ... and so the [**Splitwired**](https://github.com/falcosan/Splitwired/tree/main) project began.

#### Backend

On the official [Splitwise API page](https://dev.splitwise.com/), there is a list of SDKs created by the community. While using these kits is not mandatory since Splitwise provides everything necessary, I chose to use the library by [@namaggarwa](https://github.com/namaggarwal). The SDK is written in [Python](https://www.python.org/), has good [documentation](https://splitwise.readthedocs.io/en/latest/), is continuously updated, and therefore, I consider it the best in the list.

The project was built with [Flask](https://flask.palletsprojects.com/) and served with [Gunicorn](https://flask.palletsprojects.com/) on [Render](https://render.com/). The backend part is entirely located within the `/app` folder.

The `auth.py` file is used to create the user authentication system in Flask using [Flask-Login](https://flask-login.readthedocs.io/en/latest/). An instance of `LoginManager` is initialized and added to the Flask application. The `Authentication` class is defined, which contains a dictionary of authenticated users and two class methods to verify the presence of the user and match the username and password. The user\_loader function loads the data of the authenticated user, while the unauthorized function handles `unauthorized` users by redirecting them to the login page.

The `config.py` file contains the `Config` class used to initialize four attributes: `api_key`, `environment`, `consumer_key`, and `consumer_secret`. These attributes are required to authenticate the Splitwise instance.

The `data.py` file provides basic methods to access shared expense data using the `splitwise` library. After creating an instance of `Splitwise` using the values of the aforementioned `config` attributes, there are two functions for retrieving data: the `data_groups` function to get the data of the groups the user belongs to, and the `data_expenses` function to return the shared expense data.

The `utils.py` file contains functions related to data manipulation and the final creation of the expense list. The `generate_expense` method serves this purpose. Based on a series of parameters, it generates an expense table, graphs, and a CSV file with the expense data.

The parameters of the function are:

*   `csv`: a boolean value indicating whether to generate the CSV file or not.
    
*   `expenses`: the list of "expense" objects obtained from the `getExpenses` function of Splitwise.
    
*   `filename`: a string indicating the name of the CSV file to generate if `csv` is True.
    
*   `category`: an integer value indicating the ID of the category to filter expenses to include in the table (only applies to the CSV file table since, in the application, I prefer to do the category filtering on the client side).
    
*   `personal`: a boolean value indicating whether to generate the table and chart only for personal expenses (i.e., all expenses from all groups the selected user is involved in).
    
*   `chart`: a string or a list indicating the types of charts to generate.
    

It's important to note that the computational cost for executing the function with the `personal` parameter activated will be quite high. This is because this parameter triggers the process of creating the expense table by checking all the groups where the specified user ID is present, thus obtaining all the expenses in which that user has participated.

A recently added feature is the currency converter. I often go to the United States, and when I'm there, I prefer to pay in dollars rather than euros to avoid paying currency exchange fees. Therefore, I added the `costs_conversions` function that allows me to convert and record my expenses in euros that I made and added to Splitwise in dollars. The conversion is done by scraping the [Yahoo Finance](https://finance.yahoo.com/) portal using the [yfinance](https://pypi.org/project/yfinance/) library, which provides currency values at a specific historical moment. By providing the expense date, I can obtain the desired result. It is possible to convert any currency to euros. If you want to convert an expense to a currency other than euros, you just need to modify the `ticker` variable in the `set_currency_conversion` function.

```python
import yfinance as yf

def set_currency_conversion(
    amount: float,
    curr_from: str,
    conv_date: date,
):
    ticker = f"{curr_from}EUR=X"
    currency_data = yf.Ticker(ticker).history(
        period="1d", start=conv_date, end=conv_date + timedelta(days=1)
    )
    if currency_data.empty or currency_data["Close"].size == 0:
        return set_currency_conversion(
            amount, curr_from, conv_date=conv_date - timedelta(days=1)
        )
    exchange_rate = currency_data["Close"][-1]
    return str(exchange_rate * amount)
```

Well, in the end, the `generate_expense` function returns a dictionary with three keys: _table_ for the table, _data_ for the data, and _chart_ for the chart (if `chart` is specified). If `csv` is set to True, the `generate_csv` function will be executed, generating a CSV file containing the expense data. The file can be downloaded from the download area at the top of the page.

#### Frontend

From the frontend perspective, there isn't much to say, just a few notes.

I started by configuring [Webpack](https://webpack.js.org/) and setting up [React](https://react.dev/). Then, I installed [TanStack Table](https://tanstack.com/table/v8/docs/adapters/react-table) for table management and [Plotly](https://plotly.com/javascript/react/) for creating graphs.

Finally, I added a service worker and a manifest.json file to enable installing the Progressive Web App (PWA) on a smartphone.

#### Conclusion

Splitwise is truly a valuable application. Over the past few years, it has made my life easier by handling all the calculations of debts and credits for me. Now, with this new little "extension" I created, I have achieved much more comprehensive control over my finances.
