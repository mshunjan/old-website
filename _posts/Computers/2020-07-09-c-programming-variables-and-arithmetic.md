---
title: C programming - Variables and Arithmetic
date: 2020-07-09 23:18 -0400
category:  computerscience 
---

# Recall
In my previous [post](https://cyburp.com/computerscience/an-intro-to-c-programming/), I introduced C programming. We went over writing the "Hello World" program and the inner workings of how that program functioned. the TLDR version is that a C program consists of calling a library using `#include <x>`, writing a main function `main(){}` with statements and variables that perform the function you want. 

# Variables and Arithmitec Expressions

If you ever want to include any sort of math in your code, which I undoubtedly you will, this is basic information you need to know. We're going to learn by example, by writing a program that prints a table of Farenheit and Celsius temperatures, given the formula *C* =(5/9)(*F*-32)

<pre> 
So what we want to do is produce the following:

0       -17
20      -6
40      4
60      15
80      26
100     37
120     48
140     60
160     71
180     82
200     93
220     104
240     115
260     126
280     137
300     148</pre>

In order to do this, our main function is going to  have to do some  math on a range. There are several solutions to this problem, where we could use a for loop on an array like structure, a while loop using a max and min limit, or a simple recursion function that returns our temp and steps up by 20. For now, we'll stick with the while loop, since we'll talk about those other two solutions later. Before we discuss a solution to this problem, let's identify some basics we need to know

# Types and Variables 

Unlike a language like Python, in C you must declare a variable before its use, in the format `type var1, var2 ... varn`. For example:

``` c
int lemons, apples; 
int pears; 
``` 

## Data Types

If you're following along from the last post, you'll remember how we talked about why we need a main function. Essentially it allows us to tell the compiler where to start executing our program. You might think if C functions in such a way that we must define our entry point into our program, then it must also work that way when identifying what type of data we are storing with a variable. And you would be right. A great way to think of this is how in most scripting languages, values have fixed types not variables, hence you don't have to declare the type of a variable. But this is a big tank on our performance and if you've heard anything about C, you realize this is a performance language. In later posts, we'll talk about the stack, which explains this concept in more detail, but for now, we can understand that each variable we define in C has a defined size of memory called the stack, defined by the type we give it. 

### The Available Data Types

Below is a table listing some of the key data types in C, and those bolded are the ones you'll most likely come across and use in your programs. 

|      Type              | Storage Size |          Description             |
|------------------------|:-------------|:--------------------------------:|
| Basic                  |              |                                  |
|   *integer*            |              |                                  |
|       **char**         |    1 byte    | Stores characters as ASCII or    |
|                        |              | Unicode encoded integers.        |
|                        |              |                                  |
|       unsigned char    |    1 byte    | Characters with range 0 - 255    |
|       signed char      |    1 byte    | Characters with range -128 - 127 |
|       **int**          |    2/4 bytes | Stores whole numbers with no     |
|                        |              | decimal values                   |
|       unsigned int     |    2/4 bytes | Stores only positive integers    |
|       **short**        |    2 bytes   | A.K.A short int, think of it as  |
|                        |              | signed int                       |
|       unsigned short   |    2 bytes   | Stores only positive short int   |
|       **long**         |    8 bytes   | Stores integers of larger size   |
|       unsigned long    |    8 bytes   | Stores only positive long int    |
|   *floating point*     |              |                                  | 
|       **float**        |    4 byte    | Stores decimal characters up to  |
|                        |              | 6 places                         |
|       **double**       |    8 byte    | Stores decimal characters up to  |
|                        |              | 15 places                        |
|       long double      |    10 byte   | Stores decimal characters up to  |
|                        |              | 19 places                        |
|------------------------+--------------+----------------------------------|

You might have read that the sizes of these types are machine-dependent. In reality, it is more apt to say that it is implementation defined, as in the compiler, machine and all alike parameters. Beyond, other data types exist which we will discuss in later post.

One last important thing to note is the type of the function. If you track back to our first post, you'llnotice that we declare our main function as an integer type as so: `int main(){}`. We are declaring the return type of our function in this case an integer.

# Our Problem

How do we solve this question? Let's break it down into sections. First we'll store our temperature data into different variables, one for farenheit and one for celsius. Then, we'll need some way to apply our equation on a range of farnheit values. Since we want to produce values from 0 to 300, at intervals of 20, we can apply this to move through our system. Say we start at 0, we can apply our equation to spit out the equivalent celsius temperature. However to continue to the next interval, we need some way to keep track of where we are on our range. We can do this by stepping by our interval value after each temperature conversion.
So first we'll need to declare variables  for farnheit and celsius, which we'll store wherever we are at in our table. Then we need variables for our starting and ending temps and our stepping value.

``` c
int main()
{
    int fahr, celsius;
    int lower, upper, step; 
}
```
Next we give those variables values of our lower and upper limits 

``` c
int main()
{
    int fahr, celsius;
    int lower, upper, step; 

    lower = 0; 
    upper = 300; 
    step = 20; 
    fahr = lower;  
}
```
Earlier we said we would utilize a while loop for our equation, so let's do that now. We'll have to keep iterating through our loop until we reach the upper limit, thus we have the line `while (fahr <=upper){}`. Then in our code block, we implement our equation and assign the value to our declared celsius variable. Then we want to print it to the console in the table format. We can use the function `printf()`, and use the formatting regex along with the special characters tab and newline to create this effect. Finally, to complete this function, we need to move our farenheit value to the next interval to continue our function. 

``` c
    fahr = lower;
    while (fahr <= upper) {
        celsius = 5 * (fahr-32) / 9;
        printf("%d\t%d\n", fahr, celsius);
        fahr = fahr + step;
    }
```
So here's what we have now:

``` c
#include <stdio.h> 

/* print temp table to console for range(301) */

int main()
{
    int fahr, celsius;
    int lower, upper, step; 
    lower = 0; 
    upper = 300; 
    step = 20; 

    fahr = lower;
    while (fahr <= upper) {
        celsius = 5 * (fahr-32) / 9;
        printf("%d\t%d\n", fahr, celsius);
        fahr = fahr + step;
    }
}

```

## The While Loop

As in the name, the while loop works so that whatever is in the body, in this case our equation converting fahr to celsius, only occurs if our condition is true. In plain english, it's like this:

```
while thing is true {
    do thing
}
``` 

What if it's not true? In our case, what if fahr is not <= upper? Then our loop body does not occur. In other words it does not do "do thing"

Now all that's left is to compile it and run the file and your output should look like this:

<img src= "https://cyburp.com/mshunjan.github.io/assets/images/temp_table.jpg" alt="temp_table output" height="2000" width="1400"/>

Why write the code like this? I mean surely we could have only used two variables fahr and celsius. But what if we wanted to change those values. What if this project was much larger? Evidently, writing code goes beyond just writing code. Sure your program may work but is it efficient. Does it make sense for others? Why do it this way versus another. As you learn C, you will start to notice these things and hopefully, begin to question your own code. 