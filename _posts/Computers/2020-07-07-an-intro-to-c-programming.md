--- 
title: An Intro to C Programming
date: 2020-07-07 13:49 -0400  
category: computerscience 
---

# Preface

As any seasoned programmer knows, there is always something more to learn. In my case, I felt like I did not truly understand the inner workings of the code that I wrote. Almost magical, pseudocode just does what I want it to in plain english. And if I don't understand, there's probably some forum out there with an answer. But I don't want to code like that any more. It's *time* to learn what's behind the curtains. And what better way to learn with one of the best lower-level languages: **C**. I will be basing these tutorials on The C Programming Language 2,
by Kernighan and Ritchie. These guys not only made the C programming language, but also worked in the design of the Unix OS. If anyone knows what's going on, it's these guys.  

# Hello World

If you're familiar with programming at all, you know that it's tradition to start off your journey with a program called *Hello World*, where you make a program that prints out "Hello World". 

<p> Here's the code: </p>

``` c
#include <stdio.h>
    main()
    {
        printf('hello, world\n');
    }
```

## The Guts

Let's take a look at what this code means. First we have the line:
``` c
#include <stdio.h> 
```
This first line will likely be used for most of your C source files. Essentially it is telling the C compiler what library to include, 
in this case being the standard input/output library. We'll go into the details later, but all you need to know is that this library implements
a simple model of text input and output, handling whatever it needs to, in order to make that happen. 

Next lets look at the main() function:
``` c 
    main()
    {
        printf('hello, world\n');
    }
```
First of all, what we have here in this code block is a collection of a function and variables. The function, being `main()` is a block of code that runs when called and can do something. That block consists of **statements** that specify *computational operations*. Statements can use stored values called **variables** do perform operations as well. This concept is the same
in pretty much every coding language. Where *C* differs is that every program must have a `main()` function. This is because the program begins executing here. In programming, this referes to the **entry point**, being where the programs first instructions begin. 

### Why do we need a main() function?

When you run a program in C, your operating system is essentially telling your computer "this program is now in charge". That's pretty scary. For this to occur, it needs to know where control first begins, thus our `main()` function becomes the entry point for this to occur. You may have noticed there are () and {}. The () will take up parameters, versus the {}, which contain the instructions for carrying out the program. In the case of our `main()` program, it takes up no arguments, therefore it simple executes the code block in the {}. 

### The Statement

``` c
printf('hello, world\n');
```
The above statement calls the function printf from our standard library and prints the given characters in the string/character stream. Why add the newline character `\n`? It 
adds a newline, which if you work with the terminal directly and run the compiled executable, if absent, will print very weirdly. Here's a screenshot of what that would look like:

``` bash
./hello 
```

<img src= "https://cyburp.com/mshunjan.github.io/assets/gifs/hello_world.gif" alt="hello_world" width="40" height="40"/>

You might expect the first output if written in python, but this is because the print functions work differently between these two languages. `printf` does not supply a newline,
thus the newline character is necessary. Finally, note the semicolon, which indicates the end of the statement.