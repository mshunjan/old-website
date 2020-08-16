---
title: Intro to JavaScript
date: 2020-07-30 11:54 -0400
category: computer science 
---
  
 # Explaining JavaScript
 First of all lets dispel a common misconception. Javascript is not some offshoot of the Java programming languages, in fact it has almost nothing to do with it. Originally it was a marketing ploy to call it javascript, but just no that there is really no relation. Javascript is a versatile language. It runs on pretty much any device featuring a JS engine which reads your script, compiles it to machine code and executes it. What's great about JS is that its supported by pretty much every browser and therefore it is very popular, with lots of support.  

 # On datatypes
 Variables are dynamically typed, meaning variables aren't bound to a type like in C. Some cool things. For strings, you can also use backticks, but with backticks, if you add ${some code}, that code evaluates and becomes part of the string.

 # Important operators

classic &&, || and ! 

if vs ?

```javascript
x = 0
if (x == True) {
    console.log('not empty');
}
else {
    console.log('empty');
}

(x==0) ? console.log('empty') : console.log('non empty');
```

you got ?? for returning the first defined value example:

```javascript
b = 8;
// note how a is undefined  
x = a ?? b;
console.log(x);
// should return 8
```
The while loop and for loops
```javascript
let x = 0;
let y = 9;

while (x<y) { 
    console.log(x++);
} 

// OR

do {
    console.log(x++);
} 

// versus 

for (let i = 0; i<3; i++) {
    console.log(i);
}

// can skip beggining statement
let i = 0 

for (; i<3; i++) {
    console.log(i)
} 

```

we can break or continue loops 

```javascript
let i = 0 

for (; i<4; i++) {
    if (i<2){
        continue;
    }
    else if (typeof i == String){
        break;
    } 
    else{
        console.log(i);
    }
} 
```
An important functionality is that we can name loops like so:

```javascript 
outer: 
for (let i=0; i<4; i++) {
            for (let j = 0; j < 3; j++) {

    let input = prompt(`Value at coords (${i},${j})`, '');

    // if an empty string or canceled, then break out of both loops
    if (!input) break outer; // (*)

    // do something with the value...
  }
} 
```

To replace if checks, we can use the switch statement. Use a switch when you have a condition like "choose one of these based on the variables value". 

```javascript
x = 0
switch(x){
    case True:
        console.log('not empty');
        break; // so doesnt continue the arguments, since true means any value >1
    case 1:
    case 2:
        console.log('you ca do this too')
        break;
    default:
        console.log('empty'); 
}
```
Functions exist in javascript

```javascript
function examplefuncname() {
    ;// code something 
}
examplefuncname();

let examplefuncname2 = function() {
    ;// code something
};
```

More advanced functions


```javascript 
function summer(a=1,b=2) {
    return a + b; 
}
console.log(summer());
``` 

Arrow functions are another way of doing functions apart from function declarations and function expressions.

```javascript 
let func = (parameters) => expression 

// example

let sum = (a,b) => a + b;

// arrrow functions can also be multile
let summer = (a,b) => {
    body;
};
```   

javascript specials -> summarizing what we've learned

```javascript
// statements are boundaried (delimeted) by semicolons, can also use a line brake but refrain from using this if possible
// can interact via prompt, confirm or alert
```

# Debugging 

Perhaps the most important thing any developer should know is how to debug their code. When you write your code, you are bound to run into bugs. If you've never debugged, you might have simply looked through your code over and over again until you finally find your mistake. I would say that's very inefficient, especially if you're working on a big project. Luckily most developer tools nowadays have included debuggers to help ease this process for you. For web devs, chrome has such features. 

First launch developer tools with F12 and you'll come upp to something that looks like this:

(img)

Let's familarize ourselves with the layout. First of all we have the sources tab. Click it to see all the files that makeup the webpage. Click any file to open it up and see the source code. Below, you'll see a series of symbols for the debugger. Finally, if you press ESC, it should bring up the console.

REMEMBER TO INSERT GIFS OF HOW TO DEBUG

Here's the thing, and this is my opinion on debugging. If you know what your coding, you really don't need it. If you understand conceptually and in syntax what is going on in a given code block, you shouldn't have any unexpected bugs, at least ideally. Of course if you are working on a massive project, then yea, use a debugger, but most of the time, you probably wont need it.

# Objects 

A special data type which stores more complex entries. The way the an object works is that somewhere in the allocated memory is stored our object, and we set a reference to it using our variable, for instance "user" in the example down below. 

```javascript
// example object

let user = {
    name: "Daniel",
    age: 22
};
```

## Copying vs duplicating
Below here is an example of copying, not duplicating. We're making another reference to the object stored in memory. 

```Javascript
let user = {name: "Tom Holland"};
let admin = user;
```

Duplicating an object is a different process, where in you need to create a whole new object and then copy all the properties from the original by iterating using a `for(let key in user) {}` loop.

# The spread operator

By now, you've probably come across some sort of array. In regards to these data types, it can be quite annoying to access each element, especially if its a long list. Well, there exists an operator called a spread operator that spreads the contents of an array by its elements.

```javascript
const numbers = [1,2,3,4];

console.log(numbers); // [ 1, 2, 3, 4 ]
console.log(...numbers); // 1, 2, 3, 4 
```

# Memory management

We don't do this, the JS engine does this, but let's understand what's going on in the background. In JS, there are values guaranteed to be stored in memory such as local, global variables and parameters, referred to as root values. The idea is that these can be reached, and therefore there must also be values that can't be reached. Each JS engine has a background process that removes these unreachable values. Let's say you made a variable with a value in it. If you change the value, the JS engine will throw out the old value forever. We just learnt about objects. Let's think about what would happen if we had two variable referencing an object, such as with our `let admin = user` example. If we set user to null after this statement, would this mean our garbage collector removes the object in memory? NO! Remember we have 2 references, therefore, it would simply remove the user reference. Let's go a little bit mroe complicated. When we think of an object and its references, you think of incoming signals when referencing it. Say we have the object:

```javascript 

let user = {
    name: "Daniel",
    age: 22,
    contact: {
        email: 'daniel@email.com',
        phoneNumber: '647-777-2121'
    }
}; 
```
If we delete any references incoming, that is to say contact in our example above, than those object properties are no longer accessible. We can access nested objects with dot notation.

As long as you remember the map of how nested  and interconnected objects are referenced, you will be fine. Find the root, and follow the trunk into the branches. So you might wonder how the garbage collector finds.


## Object methods

```javascript 

let user = {
    name: "Daniel",
    age: 22,
    sayName: function() {
        console.log('Hi my name is' + user.name);
        }
    };

user.sayName(); 

//  OR

let user = {
    name: "Daniel",
    age: 22,
    sayName() {
        console.log('Hi my name is' + this.name);
        }
    };

user.sayName(); 
```


What if we want multiple users? It's annoying to have to make an object for each user like above. We could store arrays in each property or alternatively, we could use an object constructor (really depends on the problem).

```Javascript

function User(name, age) {
    this.name = name;
    this.age = age; 
}

let user1 = new User("Danny", 24)

console.log(user1)
```

# The ?. operator

This is a new feature.
Say we want information about something but it does not exist, like a user forgot to enter their email, we can use ?. (optional chaining) after our value. Example:

```Javascript
let user = {};
alert(user?.address?.street);
```

# Symbol Type 
So our object property keys can be either string/symbol type (only these 2). Symbols are **unique** identifers created as such:

```Javascript
// Symbol(description)
let mysym = Symbol("example id");
```
You can't alert this value because it will atempt to convert the symbol to a string, which cannot occur in JS. Also note that you can't see this in the current release of node (v10). Why use symbols over strings? The unique identifier allows for us to have seperate
