---
layout: post
title:      "What I've learned about JavaScript - 1"
date:       2019-07-12 17:37:11 +0000
permalink:  what_ive_learned_about_javascript_-_1
---


After months of studying and playing around with Ruby, the curriculum took me to JavaScript. Here is a summary of what I have learned about it so far. In the rest of the article I will refer to JavaScript as JS. 


## Back to Basics


### A short introduction

It is commonly said and written that JS is the programming language of the web. Along with **HTML** - *HyperText Markup Language*, for the content - and **CSS** - *Cascading Style Sheets*, for the styling - it is used in most of the web pages. JS handles most of the **dynamic behaviour** of websites. It reacts to user **events** - scrolling, clicking, zooming in or out and so on - and changes the page's content **without requiring a full reload** of the page. HTML elements that change when interacted with are targetted by their *class* or *id*, just like in CSS. 

When you hover a button and it wiggles in anticipation, when you see new notification alerts without refreshing the page, when content progressively appears at the botton of the page when you scroll, when you zoom in or out on Google Maps, it's all JS. JS is a very popular language and is the only **programming languag**e that can run in every major browser - HTML is a **markup language** and CSS is a **style sheet language**. 

JS is a **flexible** language that allows us to program in many different styles - functional or object-oriented - mixing and matching concepts to **adapt** the each situation's need. 


### A history

The firsy version of JS was built in about 10 days in **1995**. There were other competitors trying to build the language of the web, who did not succeed. After that, there were only **minor improvements** to JS. 

Around the turn of the century, Microsoft decided they wanted to give their customers a way to **access their emails in the browser**. To do so, they added a JS functionality that allowed an **already loaded page to fire off requests for additional data**. The process of making requests for additional data became known as *Asynchronous JavaScript and XML*, or **AJAX**. JS became very popular in 2004 when Gmail came around with a heavy dependence on AJAX. 

Even though AJAX brought a lot of functionalities, JS was still different to program, different browsers implemented things differently and everyone had they own idea of how the language should be. in 2009, everyone agreed on **standardisation**. There came **ES5** - fifth version of JS, sometimes referred to as **Harmony**. After harmonisation came the need for **modern language features** that or major languages had. These concerns were addressed from 2015 with **ES6**. 


### What about compilation? 

As a note, JS and Java have pretty much nothing in common and the name JavaScript was chosen for **marketing** reasons, since Java was highly popular in 1995. Unlike Java, JS code doesn't need **compilation**. This means that the code written by the developer is the **same as** the code that the web browser parses and run, whereas a compiled language, the code the developer writes is a human readable code that needs to be **translated**, *ie* compiled, for the machine to understand it. The latter means that reading the source code and debugging is less straightforward. 


### In-browser dev tools

All major browsers come with their own **built-in developer tools** that can be used to inspect, modify and debug the content of a web page. The **console** is an environement in the browser where we can type and run random JS code, in the context of the current browser window. We say the console is **sandboxed**, because it can only access the resources that are loaded on the ucrrent page. 

To make **debugging** easier, JS comes with a number of built-in features. One of the simplest is the ```alert()``` function which simply creates an alert in the browser window. 


### About syntax and best practices

In JS, ending each **statement** with a **semicolumn** is optional. When running some code in the browser, the JS engine makes several passes over the same code: 
- The first pass **parses** the syntax of the code. The engine proceeds to *automatic column insertion* but semicolumns are part of the JS syntax, whether you insert them yourself or the engine does it for you.  

- The second pass **initialises** variables and functions and **stores** them in memory. 
- The third pass **executes** the code line by line.

Most programmers consider the first and second passes the **compilation phase**. JS makes sure the code is readable and sets it up for running. The same way, the third phase is referred to as the **execution phase**. Teh code is then run top to bottom and left to right. 


### Data types

One of the most profound philosophical problems is about **abstraction** and how humans do it. Abstraction is about making a subject general rather than based on real life examples or instances. In JS **concrete instances of data** can be categorised into abstract names, called *data types* or *types*.

In JS, everything is **data** except: 
- Operators: ```+```, ```-```, ```!```, ```=```... 

- Reserved words: ```function```, ```for```, ```const```...

Every piece of data falls into one of JS's **seven** data types: 
- **Numbers**: some programming languages divide numbers into **several categories that allow better precision is calculations**: integers, decimals, doubles, floats... When JS was created, a deep level of precision for calculations was not required. Consequently, all numbers are stacked into **one category**. As a note, this lack of precision kept JS from entering **industries** where **precision** is essential like banking or engineering. 

- **Strings**: are how we represent text in JS. A string consists in a pair of **matching** single quotes, double quotes or backticks with **0 or more** characters in between. 
For **string interpolation**, use backticks and ```${}```. 

- **Booleans**: a boolean can only be one of two possible values - *true* or *false*. 

- **Symbols**: relatively new data type that is mainly used as an alternate way to add properties to an abject. 

- **Objects**: objects are a **collection of properties** linked by curly braces, similar to a hash in Ruby. A property can point to a **value of any type**, including other objects. In JS's perspective, an **array** is a special type of object where the properties are numbers - the array's indexes. 

- ```null```: represents an **intentionally absent object,** *eg* if a piece of code returns an object when successful, we might want it to return ```null``` when failing. When calling the ```typeof``` operator on ```null``` it returns a data type of *object*. 

- ```undefined```: does not mean something is strictly not defined, but rather that is it **not assigned a value yet**. Any **variable** that is declared but not defined will return ```undefined```. 

N.B. The ```typeof``` operator accepts one argument - the piece of data we want to know the type of - and returns the type. Since ```typeof``` is an operator there is no need for parentheses. 

N.B. 6 of the 7 data types are called *primitive*. This means that they all **represent single values**, whereas an *object* represents a **collection of values**.   

What happens when we mix up data types? Some languages like Python are very **strict** about it and will refuse to compile a program if types and mixed. Other languages like Ruby will **attempt a convertion** of one of the data types to have only one type to handle, but will throw errors for non obvious cases. JS, however, will **return something in any case** even if that something makes little or no sense. Sometimes it makes sense: 

``` 'Give me' + 5 + '!'; ``` will return ```"Give me 5!"```

Some other times it is rather comical: 

``` null ** 2 ```, *ie* ```null``` to the power of 2, will return ```0```. 

``` undefined ** null```, *ie* undefined to the power of null, will return ```1```. 

```{} + {}```, *ie* empty object + empty object, will return ```"[object Object][object Object]"``` which is a string and not an object. 


### Variables

*A variable is a container in which we can store values for later retrieval. *

In order to retrive the correct piece of stored data, we **name** variables - *ie* we give them an *identifier* - following these 3 rules: 
- Start every variable name by a **lowercase letter**.

- Don't use **spaces**, **camelcase** - variableName - them and do not **snake** them - variable_name. 

- Don't use JS **reserved** or future reserved words. 

N.B. In JS, **case matters**. javaScript, javascript, JavaScript and JAVASCRIPT are four different names. 

```var``` is a special word in JS. It means that the word that comes next is a **variable identifier**. 

A variable is first **declared** and then **assigned**. Either one after the other: 

```var pi;```

```pi = 3.14159;```

or do both at the same time: 

``` var pi = 3.14159;```

Both methods are commonly used. 

To retrieve a declared variable, simply refer to its name: ```pi;```. 

#### About variable values

Upon **declaration**, all variables are **automatically** assigned the value of ```undefined```. It is only after a new value is **assigned** that the variable will contain something else. 

When writing JS code it is bet practice to **never** manually assign ```undefined``` to a variable. Encountering and ```undefined``` is a strong signal that the variable was **declared** before being **referenced**. It is valuable information when debugging.  

Once a variable has been declared with ```var```, we can **reassign** it as many times as we want. The data stored in a variable might **change** over time, but its current value can be **retrieved** at any point. 

#### ```const``` vs ```let``` vs ```var```

```var``` is present in code written before 2015 and ES5. However it comes with a lot of **scope issues**. For example, with ```var``` no error is thrown if we declare two variables with the same **identifier**. Since there is no reason to declare a variable twice it is usually a mistake that should throw an error. 

```let``` was introduced in ES5. It allows us to **reassign** a variable as much as we like but throws an **error** if we try to declare the same variable twice. 

```const``` is a good default to have. With it, a variable cannot be **redeclared** or **reassigned**. The latter is good for 3 reasons: 
- When assigning a **primitive value**, we know it will always contain the **exact same** value. 

- When assigning an **object**, the variable will always point to the **same object**, although the object itself can be **modified** over time. 

- When a developer sees a variable declared with ```const``` they know it will point ot the exact **same object** or have the exact **same value** every single time it is **referenced** in the program. 

N.B. With ```let``` it is possible to declare a variable without assigning it a value. However, since ```const``` does not allow reassignment, a value must be assigned at the same time as declaration. 

As a general rule for declaring variables, use ```const``` as **often** as possible, unless you know that it will have to be **reassigned** at some point, in which case ```let``` should be used. Do not use ```var``` because of the **scope issues** it creates. 


### Control Flow

Control flow is an **essential** concept in all programming languages. According to Wikipedia: 

> In computer science, control flow (or flow of control) is the order in which individual statements, instructions or function calls of an imperative program are executed or evaluated

Control flow starts with **comparison**. The value returned by a comparison is always **true** or **false**. 

In JS, there are 4 built-in equality operators: 
- Loose equality operator ``` == ```
- Strict equality operator ``` === ``` 
- Loose inequality operator ``` != ``` 
- Strict inequality operator ``` !== ```

It is always preferred to use **strict operators**. Indeed, the loose operators will **return true** even if the **data types** are **different**. 

There are also 4 built in relational operators: 
- Greater than ``` > ```
- Greater than or equals ``` >= ```
- Less than ``` < ```
- Less than or equals ``` <= ```

These operators work in a similar way to the equality operators. However, be careful of **type conversion** when comparing numbers and non numbers. 

Strings are compared with each other **lexicographically**. This means character per character left to right. The **Unicode value** of each character is compared leading to ``` '88' > '9' ``` returning ``` false ```. 

Generally speaking, try and stick to comparing **numerical values** with each other. 

An **expression** is a unit of code that returns a value. **Primitive values** - all data types except objects - are expressions. So are **string and arithmetic** operations. The same goes for **comparison**,  **assignment** and **referencing** operations. Variable **declarations**, however, are not expressions. 

**Block** statements - *ie* pairs of curly braces used to group JS statements - play a role in **conditional** statements. They return the value of the last evaluated expression inside the curly braces. 

The **truthiness** and **falsiness** indicate what happens when the value is converted into a **boolean**. In JS **falsy** values are: 
- ``` false ```
- ``` null ```
- ``` undefined ```
- ``` 0 ```
- ``` NaN ```
- empty strings

Every other value is **truthy**. 

To check whether a value is truthy or falsy, pass it to the global ```Boolean``` object. 

There are 3 main **conditional statements** in JS:
- ``` if...else ```: fairly straightforward structure. As soon as one of the conditions returns a **truthy value** the block of code is executed and the conditional **exits**. 

     ```
if (condition) {
} else if (condition) {
} else {
}
```

- ``` switch ```: is the solution to **run multiple blocks** in the same control flow. It is generally used if a conditional that hinges on a **single value** but requires a **specific handler** for each case is needed. 

     ```
switch (expression) {
     case value1: 
        // statements
        break;
     case value2: 
        // statements
        break;
     default: 
        // statements
        break;
}
```
- **Ternary operator**: is a good way to represent and ``` if...else ``` statement on one line. 

   ``` condition ? expression1 : expression2;```
	 
The last part of control flow is **logical operators**: 
- ``` ! ``` is used to **negate** an expression

- ``` !! ``` is used to convert an expression to a boolean
- ``` && ``` and ``` || ``` are used to link conditions


### Intro to Functions

Functions are the most important unit of code in JS. They serve as ways to **group related bits of JS** together. 

We have already talked about abstraction earlier in this article. Functions **combine** series of steps under a **new name**. Consequently they are **abstractions**. 

> A function is an object that contains a sequence of JavaScript statements. We can execute or *call* it multiple times - Flatiron School
> 

Call a function means run the independent pieces that constitute it. To do so, add ``` () ``` after its name. Before calling a function, it has to be **declared**. When declaring a function, the ideal is to have it as **general** as possible, in order to be able to use the same function for as many situations as possible. Using **parameters**, **arguments** and **string interpolation** is a way of doing it. 

Sometimes, it can be helpful to send something back to the place where the function was executed. That's where **return values** come in. When a function is called, it returns the value found on the right of the ``` return ``` keyword when it encounters it. Once the ``` return ``` keyword is reached, nothing else happens. Return values can be saved to **variables** or used as **inputs** to other functions. 
