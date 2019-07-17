---
layout: post
title:      "What I've learned about JavaScript - 2"
date:       2019-07-17 09:28:33 -0400
permalink:  what_ive_learned_about_javascript_-_2
---


This article builds up on the previous one where I've given a summary of basic JS concepts as I understood them. 


## 2. General Principles
### Scope

The genral concept of scope is: where is something **available**? In the context of JS it is about where declared variables and methods are avaiable. A very good example for understanding the concept of scope is Slack. A Slack organisation is organised in **channels**. Some channels or organisation-wide - *ie* channel and messages can be viewed by everyone - and some or smaller than that. A member of a channel can see all the channel's messages but cannot see the messages of a channel they are not a member of. A channel then forms its own scope. The exact same principle applies in JS regarding where the variabes are **declared** and where they can be **accessed** from in the program. 

Just like a message on Slack is sent in a specific channel, each piece of JS code is run in a specific **execution context**. In a JS execution context, we have access to all functions and variables declared in it. We can then write an expression that references variables or calls a function declared in the same context. 

The **global execution context** is the context that implicitly wraps all the JS code in a project. So variables and functions declared there are accessible through the **whole program**. 

When we decare a nnew function and write some code in that function's body, the function creates **its own execution context**. 

A **block statement** also created its own **scope** as long as variables are declared with `let` or `const`. 

N.B. Variables declared without `let`, `const` or `var` are always **globally-scoped** even if it is created inide a function. 


### Scope Chain

Every JS function has access to what is called a **scope chain**. It includes references to the function's **outer scope** - *ie* the scope in which the function was declared - the outer scope's outer scope and so on until the global execution context is reached. 

When a new function is **declared** in the global execution context, its outer scope is the global scope. When that function in **called**, it can access all variables and functions declared in the global scope. When **invoked**, that function creates and new scope and **retains a reference** to the outer scope in which it was declared. Inside that function, we have access to the variables and functions declared there. 

```
const globVar = 1;

function firstFunct() {
   const firstVar = 2;
   return firstVar + globVar;
}

firstFunc();
//=> 3
```

When the JS engine cannot find a **local match** for the reference, it looks in the outer scope. Because of the way functions can look for references in their outer scopes, it is said they have access to a **scope chain**. 

N.B. What matters for the scope chain is not where the function is called but where it is **declared**. 

N.B. The scope chain only goes in one direction. 


##### The JS Engine and Identifier Resolution

**Reminder**: when declaring a variable or function we give it a name or **identifier**. 

When the JS code runs in the browser, the engine actually makes two distinct passes over our code: 

- The **compilation** phase: in this phase the engione goes through our code line by line: 

  - When it reaches a **variable** declaration, it allocates memory and sets up a reference to the variable's identifier

  - When it reaches a **function** delcration it does 3 things: 

      - Allocates **memory** and sets up a **reference** to the identifier

      - Creates a new **execution context** with a new scope

      - Adds a **reference** to the parent scope to the **scope chain**

- The **execution** phase: there again it goes through the code line by line but this time it **runs** our codes, assigning values to variables and calling functions. One of the tasks is **matching** the identifiers with the corresponding values stored in memory. 


Remember: in terms of scope, what matters is where a function was **declared**, not where it is invoked. When a fucntion is declared, it asks: "Where was I created". The answer is the outer environment that gets stored in the new function's scope chain. This is called **lexical scoping** or **lexical environment**. 


### Hoisting

Since the JS engine reads files from top to bottom, it would make sense to declare functions before we call them. However, doing it the other way around works fine because the engine goes through the code twice, first to *compile* the code, then execute it. 

The term for this is **hoisting**, because it seems like all declarations are being hoisted to the top of the current scope. It is accurate in the sense that declarations are being **evaluated** before the rest of the code is executed. However the physical **location** of the code does not actually change. Best practice: simply declare your functions at the very top of the code. 


##### Variable hoisting

Let's start with an example: 

```
function myFunc () {
  console.log(hello);
  var hello = "World!";
}
```

When executed, this prints ```undefined```. 

In JS, hpoisting only applies to variable **declarations**, not **assignements**. 

In our example, during the **compilation** phase, the JS engine initialises the ```hello``` variable and stores it in memory. At this point, no value is assigned to ```hello``` and it will contain ```undefined``` until it is assigned a different value during the **execution** phase. 

To make it more obvious let's rearrange the code: 

```
function myFunc () {
  var hello;
  console.log(hello);
  hello = "World!"
}
```

With the above syntax - the behaviour remains identical - what happens is clearer: by the time the engine reaches the ```console.log()``` part, the variable is still assigned ```undefined```. 

There are two ways of avoiding **variable hoisting** by the JS engine: 

- If for some reason you have to declare your variable with ```var```, juste declare everything at the **top of the scope**. 

- Just **do not use ```var```**. Although variables declared with ```let```  or ```const``` do technically get hoisted, the JS engine does not allow them to be referenced before they've been initialised. 



## 3. Data structures

> A data structure is a specialised format for organising, processing, retrieving and storing data - WhatIs

### Arrays

Following the basics section, we know about data types. Now, if we need to organise data, arrays are among the most useful data structures. 

An array is an **ordered** list of items, surrounded by ```[]```: ```['This', 'is', 'an ', 'array', 'yeay!']```. Items of the array can be of **any data type** and an array can be assigned to a **variable**. 

Arrays can be called **expressive** because the tell other programmers that their elements go together. 


##### Manipulate Arrays

**Access Elements**: 

Each element of an array is assigned a **unique index value** that matches its **location** in the array. The first element is indexed 0. The rest follows **incrementation** by 1. So the 18th element has an index of 17. 

Accessing an element is done using the **computed member access operator** - or **bracket notation** - as follows: ``` array[index] ```. 

**Add Elements**:

Several ways to add elements: 
- Destructively, at the **beginning** of the array: ```.unshift()```

- Destructively, at the **end** of the array: ```.push()```

- **Spread operator** to spread the contents of an existing array is non-destructive: ```[...array]``` creates a new array that contains the same elements as ```array```. It can be used to add elements at the **beginning** or at the **end** of the initial array. 

**Remove Elements**: 

Several ways to remove elements: 
- Destructively, at the **beginning** of the array: ```.shift()```

- Destructively, at the **end** of the array: ```.pop()```
- Non-destructively with ```.slice()```. This methods returns a portion of the initial array without altering it: 

 - With **no arguments**: returns copy of the original array with all elements. The two arrays point to different objects in **memory**:  

   ```
const first = [2, 4, 6, 8];
const copyOfFirst = first.slice();
```

 - With **arguments**: 
   - **2** arguments: index where the ```slice``` should begin and index before which it should end 

   - **Single** argument: index where it should begin all the way to the end of the array. With a **negative** index, the JS engine knows to start counting from the last element: ``` array.slice(-3)``` returns a new array with the last three elements of the original one. 

- Destructively with ```.splice()```: 
 - **Single** argument: removes a part of the original array, beginning at the provided index and continuing to the end. 

   ```
const greeting = ['hello', 'there', 'how', 'are', 'you?'];
greeting.splice(2);
// returns ['how', 'are', 'you?']
greeting;
// returns ['hello', 'there']
```

   ```
const greeting = ['hello', 'there', 'how', 'are', 'you?'];
greeting.splice(-2);
// returns ['are', 'you?']
greeting;
// returns ['hello', 'there', 'how']
```

 - **2** arguments: the first arguments is the **index** at which to start the splicing, the second indicates the **number of elements** that will be removed from the array. 

**Replace Elements**: 

- ```.splice()``` with **3** arguments: 

 - First argument: **index** at which to start

 - Second argument: **number** of array elements to remove

 - Rest of the arguments: items to be **inserted** in the array to replace the elements that were removed. You can insert **as many elements** as you want to replace the removed element(s). There can even be **0** elements removed, in this case ```.splice()``` returns an empty array. 

- **Computed Member Access Operator**: it is simpler than ```.splice()``` for just one element but it is still **destructive**. 

- Combination of **```.slice()```** and **spread operator** will leave the initial array untouched. 

 ```
const arr = ['1', '2', '3', '4', '5'];
const newArr = [...arr.slice(0, 1), 'a', 'b', 'c', ...arr.slice(3)];
arr;
// returns ["1", "2", "3", "4", "5"]
newArr;
// returns ["1", "a", "b", "c", "4", "5"]
```

##### Nested Arrays

In the previous example, if we had not use the **spread operator**, we would have ended up with a nested array: 

```
const arr = ['1', '2', '3', '4', '5'];
const newArr = [arr.slice(0, 1), 'a', 'b', 'c', arr.slice(3)];
newArr;
// returns [["1"], "a", "b", "c", ["4", "5"]]
```

So, arrays let us store data of **any type**, including other arrays. It is very convenient for a game board because it lets us access different locations by specifying **coordinates**. Nesting can be potentially **unlimited**, but it then becomes difficult to access data.  

### Objects

Arrays are great for **simple, ordered, datasets**. However, they are not so great for modelling more complex structures. That's where **objects** come in. 

N.B. What is called an object in JS is not what we know as objects in **OO programming**. It is similar to what is called a **hash** in Ruby. JS was initially created as a lightweight language for DOM-based programming. So it was not thought as an OO language. It later gained a class-based and instance-based system. 

If you look back at the data types section, an **JS object** was described as: *a **collection of properties** bound together by curly braces, similar to a hash in Ruby. The properties can point to **values of any data type** - even other objects*. 

Objects can be: 
- Empty: ```{}```

- Have a single key-value pair: ```{key: value}```

- Multiple key-value pairs: 

 ```
{
 key1: value1,
 key2: value2
}
```

##### Access a Value Stored in an Object

There are two ways of **accessing** data in an object: 
- The **dot notation**: ```object.propertyName;```. When accessing an **non-existent property** the dot notation will return ```undefined```

- The **bracket notation** - or computer member access operator: ```object['propertyName'];```. Should be used in 2 situations: 

 - With **non-standard keys**: if you need to use a non-standard string as a key, ie a string that does not follow the same naming **conventions** as functions and variables - camel case, starts with lowercase letter, no space or punctuation.

 - To access proprties **dynamically**: we can place any expression in the brackets, JS will **compute** its value to find out which property to access. The real strength of the bracket notation is its ability to compute the **value of variables**. It is also true during the **creation** of a new object, if using square brackets for the property name. 

   ```
const channels = {
      global: '#general',
      coaching: '#coders-coach-corner',
      javaScript: '#rails-js-section'
};
let channelType = 'coaching';
channels[channelType];
// returns "#coders-coach-corner"
channels.channelType;
// returns undefined, it is looking for a property called 'channelType'
```

```
const globalChannel = 'global';
const coachingChannel = 'coaching';
const javaScriptChannel = 'javaScript';
const channels = {
[globalChannel]: '#general',
[coachingChannel]: '#coders-coach-corner',
[javaScriptChannel]: '#rails-js-section'
}
channels;
// returns the appropriately created object
```

##### Add a Property to an Object

Either **dot or bracket notations** can be used to add properties. Multiple properties can have the same **value** but **keys** must be unique or the **last** one's value will be taken into account. Existing properties can be **overwritten** by assigning a new value to an existing key, but this is **destructive** for the original object. This process can easily be encapsulated in a **function**. 

It is possible to **non-destructively** edit an object by using the spread operator to create a new object: 

```
function nonDestructivelyUpdateObj(obj, key, value) {
  const newObj = {...obj};
  newObj[key] = value;
  return newObj;
}
```

N.B. We have used ```const``` to **declare** our object, and yet we have **modified** a key-value pair. We are allowed to modify an object declared with ```const``` as long as no new value is assigned to the **name**. 

This is fine, but it only allows us to modify a **single property**, and would need to be **rewritten** for each change. The ```Object.assign()``` methods allows us to **combine** properties from **multiple** objects into a single one. The return value of this method is the **initial object**, after all additional objects have been **merged** in. Remember that if several keys havet he same name, the last one will win over all others. 

A common pattern is to provide an **empty object** as initial object to leave the object to update intact. 

```
function nondestructUpdateObj(obj, key, value) {
  return Object.assign({}, obj, {[key]: value});
}
```

Another method for objects is ```Object.keys()```. When an object is passed as an argument, the return value is an **array** containing all the keys at the top level of the object. As a note, the order in which the keys are returned in the array is **not consistent** accross browsers so it should not be relied on. 


##### Remove a Property fron an Object

Removing a property is very easy: ```delete object.propertyName```. 


##### What Relationship Between Arrays and Objects?

Going back to the **data types** section, there is no array data type. That is because arrays are a special type of **objects**. This is confirmed when using the ```typeof``` operator on an array. 

It is possible to set properties on arrays just like on any object: 

```
const myArr = [];
myArr.summary = 'Empty array!'
myArr;
// returns [summary: "Empty array!"]
```

We can as easily **access** and **modify** properties, just like on a usual object. To see what's special about arrays, let's push some values in it: 

```
myArr.push(2, 3, 5, 7);
// returns 4
myArr;
// returns [2, 3, 5, 7, summary: "Empty array!"]
```

Now, if we check the **length** of the array, it will return 4. On of the special features of the array is that it stores **array-style** elements separately from **object-style** properties. The ```.length``` property describes how many items are in this special list and object-style properties are not included. 

Now, what happens if we add a new property that has a key of 0?JS creates a new array-like item. The same happens if we give a key of '0'. What this means is that **all keys** in objects and **all indexes** in arrays are actually **strings**.  Even if we write an integer, the JS engine **converts** that to a string. Every key that is not made of integers or integers converted in strings is considered a simple **object property**.  

So as general rules: 
- For **accessing** elements in arrays use **integers**

- Be careful with setting **object-style** properties on arrays

- Remember that **all object keys**, incl. array indexes are **strings** (this will be useful when we start iterating over objects)


