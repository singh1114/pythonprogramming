---
layout: post
title: Python - Writing First Program
date: 2020-10-30T09:53:32.337Z
updated_date: 2020-10-30T09:53:32.404Z
description: Python writing your first hello world program on pythonprogramming.org
published: true
tags:
  - python
  - beginners
categories:
  - python
  - beginners
author_name: Amit Mathur
author_username: amit
show_ads: false
show_telegram_signup: false
series_unique_code: python_series_beginners
series_part: "3"
series_page_title: Writing First Program
series_title: Python Programming
previous_slug: python-basic-programming-concepts
skip_from_listing: true
---
In this post, we will write a basic Python program.

## Hello World

Let’s begin with Python Basic Program to display “Hello, I am learning Python” on the console. To do that, open Python prompt/IDLE and type the below statement. It will print your desired output:

```python
print('Hello, I am learning Python….')
```

As we have seen earlier, Python follows a very easy syntax, hence you don’t need to write lines of code like other languages.

All you need to do is use parenthesis to enclose the string. Python version 2.4.3 and earlier gives you an option to skip the parenthesis as well however in Python 3.x, you must use parenthesis.

## Semicolon

You don’t need to end the statement with a semicolon. However, if you want, you can still do that. The following statement also produces the same result:

```python
print('Hello, I am learning Python….');
```

## Printing space

Below statement will print space but you won’t see any difference since it will appear blank

```python
print(" ")
```
## Python Functions?

So `print` is a function that displays the desired result for you. It takes the string as an input/argument and displays the string on the console. Let’s put some attention and time to see what functions are:

Python function is a piece of code that performs some action or causes some effect. Generally, a Python function performs two activities:

a) Cause some effect – like print function
b) Evaluates some value – like sum, sin, cos which we will see later

There are functions which can perform both the above activities also. There are three types of Python functions:

a) Python build-in functions: They come with Python itself and you can use them directly like `print`.

b) Python modules – They also come with Python but to use them, you need to import them from their parent space like ‘sin’, ‘cos’ etc.

c) User-Defined Functions – Functions which a user can code themselves like finding the speed of a vehicle if we know the distance travelled in time.

Functions may take arguments to evaluate something. Like for sin(x) function, we need to pass an argument which is the measure of an angle sin(x).

Functions may take more than one argument also. In our example, print function takes the string as the argument and displays it. We will more about functions and modules in later sessions.

## Skipping a line while printing

We can also instruct print to display content on multiple lines in the following manner,

```python
print('I am learning Python……\nThis is my first python code……')
```

Here we have used `\n`, `\` just tells that the characters or letters that follows me have a different meaning and are not part of the string. Escape them for the moment. That’s why `\` is also called as an Escape character.

`n` is just a symbol for a new line which tells that whatever is present after it should be printed on next newline. Hence the output of the above statement will be two lines, one that ends at `\` and the second that begins at n.

## Printing escape characters

If you want to print the character `\`, then you must use it twice to handle its escaping behavior

```python
print("\\")
```

You can pass multiple strings separated by a comma within a print statement. Those multiple strings will be treated as multiple arguments by print function.

```python
print("I have written some Python programs.", "Will be writing more programs")
```

This statement will output all the arguments on the same line. Also, all the arguments will be separated by space in the output. Comma automatically puts space in the output hence no need to place space across comma in the print statement.

## Using different separator between printing Strings

Use of keyword `sep` – If we want to separate different strings of `print` statement by some other character, then we have to use a keyword `sep` like below:

```python
print("I am writing Python program", "Python is easy", sep="....")
```

In the output, the two string arguments will be separated by `....` because we mentioned separator as 4 dots.

```python
print("I am writing Python program","Python is easy", sep="#")
```

This will separate the strings by `#`.

## Ending the

Use of keyword ‘end’ – As we have seen, each print statement prints the output from next line. What if we want to change that behavior? For that we have to use a keyword ‘end’ like below:

```python
print("I have written Python programs", end="!!!!")
print("Python is very interesting")
```

In the output, we will see both the print statement output coming in same as separated by `!!!!`.

By default, each print starts from next line which means that the first print statement ends with a new line character (`\n`), in our example, we instructed the first print statement to end with `!!!!` instead of a newline.

Hence the second print statement started after!!!!. So technically here you changed the end of line character to !!!!

We can also use a combination of both `sep` and `end` in print statement like below:

```python
print("Python", "learning", end="\*", sep="\_")
print("Love", "Python", sep="#")
```

In this case, arguments of the first print will be separated by `\_`. Then first print will end with `\*`. Then the strings of second print will be separated by `#`.

Please note that `sep` and `end` should come only as the last arguments after all the strings arguments. `sep` and `end` can be put in any order among themselves.

## Real-world usage

`end` keyword is used a lot of times when you want to see some loop's output in a single line.

```python
for a in range(0, 100):
    print(a, end="")
```

```python
0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95 96 97 98 99
```

Don't worry if it doesn't make sense to you, We will go through them in later posts.