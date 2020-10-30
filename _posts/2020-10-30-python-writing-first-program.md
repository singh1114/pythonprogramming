---
layout: post
title: Python - Writing First Program
date: 2020-10-30T09:53:32.337Z
updated_date: 2020-10-30T09:53:32.404Z
published: true
show_ads: false
show_telegram_signup: false
series_unique_code: python_series_beginners
series_part: "3"
series_page_title: Writing First Program
series_title: Python Programming
previous_slug: python-basic-programming-concepts
skip_from_listing: true
---
[![Amazon Great Indian Mobile Festival](https://i2.wp.com/mytechdevice.com/wp-content/uploads/2020/10/amazon-mobile.jpg?resize=640%2C221&ssl=1)

![Amazon Great Indian Mobile Festival](https://i2.wp.com/mytechdevice.com/wp-content/uploads/2020/10/amazon-mobile.jpg?resize=640%2C221&ssl=1)

](https://www.amazon.in/mobile-phones/b/?ie=UTF8&node=1389401031&ref_=nav_cs_mobiles&_encoding=UTF8&tag=mytechdevice-21&linkCode=ur2&linkId=c256eee1b61588f7f2743b3c16d60915&camp=3638&creative=24630)  
  

Hello Friends…. Thanks for visiting my blog again. If you have read my previous blogs, you would have got a fair idea of what Python is and what are its features. Now let’s not waste time and go through this tutorial on Python Basic Program.

**1.** Let’s begin with Python Basic Program to display “Hello, I am learning Python” on the console. To do that, open Python prompt/IDLE and type below statement. It will print your desired output:

print(“Hello, I am learning Python….”)

As we have seen earlier, Python follows a very easy syntax; hence you don’t need to write lines of code like other languages. All you need to do is use parenthesis to enclose the string. Python version 2.4.3 and earlier gives you an option to skip the parenthesis as well however in Python 3.x, you must use parenthesis.

**2.** You don’t need to end the statement with a semicolon. However, if you want, you can still do that. Below statement also produces the same result:

print(“Hello, I am learning Python….”);

**3.** You can also choose to enclose the string using single apostrophe:

print(‘Hello, I am learning Python….’)

**4.** If you don’t enter any string in parenthesis, then an empty line will be printed…try it once!!!! Both these statements will display a blank line.

print()  
print(“”)

**5.** Below statement will print a space but you won’t see any different since it will appear blank

print(“ “)

Let’s take a short break from Python Tutorial 3- Writing First Program and think a question **“What is print exactly?”** So ‘print’ is a function which displays a desired result for you. It takes the string as an input/argument and displays the string on the console. Let’s put some attention and time to see what functions are:

Python function is a piece of code which performs some action or causes some effect. Generally, Python function performs two activities:

a) Cause some effect – like print function  
b) Evaluates some value – like sum, sin, cos which we will see later

There are functions which can perform both the above activities also. There are three types of Python functions:

a) Python build-in functions: They come with Python itself and you can use them directly like ‘print’.  
b) Python modules – They also come with Python but to use them, you need to import them from their parent space like ‘sin’, ‘cos’ etc.  
c) User Defined Functions – Functions which a user can code themselves like finding the speed of a vehicle if we know the distance travelled in time.

Functions may take arguments to evaluate something. Like for sin(x) function, we need to pass an argument which is the measure of an angle sin(x). Functions may take more than one argument also. In our example, print function takes the string as the argument and displays it. We will more about functions and modules in later sessions.

Let’s come back to our Python Tutorial 3 – Writing First Program

**6.** You can put more than one print statement in a code. Below code will produce two lines of output having string of each print statement. Note that each print statement will start printing from a new line. Remember that you cannot use two print statement on the same line

print("I am learning Python…..")  
print("This is my first python code……")

**7.** What if we want to insert a blank line between the two statements? We have already seen it. Try below:

print("I am learning Python…..")  
print()  
print("This is my first python code……")

**8.** We can also instruct print to display content on multiple lines in below manner:

print(“I am learning Python……\\nThis is my first python code……”)

Here we have used \\n. \\ just tells that the characters or letters that follows me have a different meaning and are not part of the string. Escape them for the moment. That’s why \\ is also called as an Escape character.  
‘n’ is just a symbol for a new line which tells that whatever is present after me should be printed on next new line.Hence the output of the above statement will be two lines, one that ends at \\ and second that begins at n.

**9.** If you want to print character \\, then you must use it twice to handle its escaping behaviour:

print(“\\\\”) – This will print \\

**10.** You can pass multiple strings separated by comma within a print statement. Those multiple strings will be treated as multiple arguments by print function.

print("I have written some Python programs.","Will be writing more programs")

This statement will output all the arguments on the same line. Also, all the arguments will be separated by space in the output. Comma automatically puts space in the output hence no need to place space across comma in the print statement.

**11.** Use of keyword ‘sep’ – If we want to separate different strings of print statement by some other character, then we have to use a keyword ‘sep’ like below:

print("I am writing Python program","Python is easy",sep="....")

In the output, the two string arguments will be separated by ‘….’ because we mentioned separator as 4 dots.

print("I am writing Python program","Python is easy",sep="#")

This will separate the strings by #

**12.** Use of keyword ‘end’ – As we have seen, each print statement prints the output from next line. What if we want to change that behavior? For that we have to use a keyword ‘end’ like below:

print("I have written Python programs",end="!!!!")  
print("Python is very interesting")

In the output, we will see both the print statement output coming in same like separated by !!!!.  
By default, each print starts from next line which means that first print statement ends with a new line character (\\n), in our example we instructed the first print statement to end with!!!! instead of new line. Hence second print statement started after!!!!. So technically here you changed the end of line character to !!!!

**13.** We can also use combination of both ‘sep’ and ‘end’ in print statement like below:

print("Python","learning",end="\*",sep="\_")  
print("Love","Python",sep="#")

In this case, arguments of first print will be separated by \_. Then first print will end with \*. Then strings of second print will be separated by #.  
Please note that ‘sep’ and ‘end’ should come only as the last arguments after all the strings arguments. ‘sep’ and ‘end’ can be put in any order among themselves.

Link to previous session: [https://mytechdevice.com/python-tutorial-basic-programming-concepts/](https://mytechdevice.com/python-tutorial-basic-programming-concepts/)

### Share this:

*   [Facebook](https://mytechdevice.com/python-tutorial-3-writing-first-program/?share=facebook&nb=1 "Click to share on Facebook")
*   [LinkedIn](https://mytechdevice.com/python-tutorial-3-writing-first-program/?share=linkedin&nb=1 "Click to share on LinkedIn")
*   [Tumblr](https://mytechdevice.com/python-tutorial-3-writing-first-program/?share=tumblr&nb=1 "Click to share on Tumblr")
*   [Twitter](https://mytechdevice.com/python-tutorial-3-writing-first-program/?share=twitter&nb=1 "Click to share on Twitter")
*   [WhatsApp](https://mytechdevice.com/python-tutorial-3-writing-first-program/?share=jetpack-whatsapp&nb=1 "Click to share on WhatsApp")
*   [Pinterest1](https://mytechdevice.com/python-tutorial-3-writing-first-program/?share=pinterest&nb=1 "Click to share on Pinterest")

### Like this:

Like Loading...

[Python Tutorial 4 - Python Literals](https://mytechdevice.com/python-tutorial-4-python-literals/ "Python Tutorial 4 - Python Literals")August 28, 2020In "Learn Python"

[Python while loop](https://mytechdevice.com/python-while-loop/ "Python while loop")September 26, 2020In "Learn Python"

[Python Tutorial 2 - Basic Programming Concepts](https://mytechdevice.com/python-tutorial-2-basic-programming-concepts/ "Python Tutorial 2 - Basic Programming Concepts")July 30, 2020In "Learn Python"