--- 
title: Using Web APIs
date: 2020-07-17 00:47 -0400
category:  computerscience 
---

# What is a Web API? 
An <abbr title= "Application Programming Interface"> API </abbr> is a specially designed area of any website able to interact with a program. This does not mean every website has this ability, merely that any website can implement it. A designated URL is used to request the desired info. Say you wanted to build an app that gives you the top most mentioned stocks in a popular forum on a specific website. If that website has an API, you can get that data to use in your app. So the premise is that if there's data to be collected, search for an API that provides that data. Now with great power comes responsibility. Thus it is given sparingly, in that there are a limit on the requests you can do. Perhaps 300 a day, or 1000 a minute, ultimately it depends on those that run your target website. 

If you're familiar with webscraping, you might wonder what the advantage is between API's and webscraping. Think of it as acessing the entire website, versus a page of html at a time. The latter is very difficult to extract data from, and further maintain.

## How an API works

As I said before, our written program can request the desired info using a specific URL via an *API call*. Take for example the url "https://api.github.com/search/repositories?q=language:python&sort=stars". The core section "https://api.github.com" requests Github's API calls. The search "/search/repositories" section tells the API to search through all repositories on github. So how do we work with repositories to extract the information we want? First we need to signal that we are passing an argument, in this case being the ? symbol. Everything following that is an argument. q= is telling the API that we are looking for a specific query. In this case, we've indicated only repositories with Python as the primary language, followed by an argument to sort those projects by stars. You may wonder, all this is all well and interesting but how applicable it is it to other api's. The concepts are universal, the specifics are in the documentation, so be sure to look up whatever is available for your chosen API. In a future posts, we'll work with different API's, so be sure to bookmark this page for future reference.  

So I'm sure you clicked that link already, but take a look and you'll see that each repository project is seperated by a {} in the list "items". We're interested in this data.

# The Setup
Ok so we're going to need the requests library from python to work with the API, so we'll need to install it. If you're working with python3, the command is python3 -m pip install --user requests. In the future we'll be making a post regarding setting up a coding environment, so stay tuned for that. 

One key aspect of any API is understanding its rate limit. Each API should provide a method of monitoring this. Should you be exceeding the rate limits, you may want to consider premium options that are sometimes available, otherwise you're stuck with what you have. 