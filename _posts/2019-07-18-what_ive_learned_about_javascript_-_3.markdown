---
layout: post
title:      "What I've learned about JavaScript - 3"
date:       2019-07-18 11:56:11 +0000
permalink:  what_ive_learned_about_javascript_-_3
---

This article builds up on the previous two where I have tried to explain the **basic concepts** of JS, the **general principles** of the language - scope, scope chain and hoisting - and **data structures** - JS objects and arrays. 

## 4. Looping and Iteration

Sometimes, we need to do things **repeatedly**. We could call the same function as many time as we need. But that would be a lot of writing. So this is where **loops** come in. Loops are used to run the same block of code a given number of times. 

There are two main types of loops in JS: 

- The ```for``` loop: is often used to iterate over every element in an array. The basic structure is: 
```
for ([initialisation];[condition];[iteration]) {
   [loop body]
}
```

  - **Initialisation** is typically used to initialise a counter variable

  - **Condition** contains an expression **evaluated** before each pass. If it evaluates to ```true``` then the loop body is **executed**, otherwise the loop **exits**. 

  - **Iteration** is a statement executed at the end of each iteration. It is typically incrementing or descrementing the counter variable created in initialisation. 

  - **Loop body** is the block of code executed a each pass

  ```
for (let age = 1; age < 16; age++) {
      console.log(`My cat is ${age} years old`)
}
```

- The ```while``` loop: is similar to the for loop but only requires **condition** and **loop body** statements. The **initialisation** and **iteration** statements are now in the loop body. **Iteration** should not be neglected, because if the condition always evaluates to true, an **infinite loop** is created. Becaue of that ```while``` loops are often used when we want a loop that runs an **indeterminate** amount of times. 

```
while ([condition]) {
   [loop body]
}
```

So,  hwo to choose between ```for``` and ```while```? A general guide is to try and use a ```for``` loop first and change it to a ```while``` loop if the first one does not fit the purpose. With the ```while``` loop bear in mind you need to update the **condition** for every pass so that the loop eventually exits. 

__

When we create a ```for``` loop on an **array**, the condition is based on the array's length. Although it works it is using a **looping construction** to perform **iteration**. 

The difference between the two is that **looping** is the process of repeatedly executing a set of statements until a **condition** is met, whereas **iterating** is the process of executing a set of statement once on each element of a **collection**. 

More recent version of JS introduced the ```for...of``` statement. No counter, no bracket notation to access elements which is way cleaner. 

```
const myArr = [2, 4, 6, 8];
for (const element of myArr) {
   console.log(element);
}
```

In ```for``` and ```while``` loops, **```let```** is required because we need to **increment** the counter value. The ```for...of``` statement does noot require **reassignement**. So each time the engine reaches the loop body, it assigns the next element of the collection to a **new ```element``` variable**. When reaching the end oft he block, the block-scoped variable vanishes and the engine goes back to the top of the block and the process is repeated until all elements have been passed through the block. 

A **string** is, in effect, an ordered collection, *ie* an **array**. So it is entirely possible to iterate over a string with ```for...of```. 

To iterate over the properties of an **object**, we use the **```for...in```** statement. It only passes the **key**, and not the entire property into the block. From the key, accessing the **value** is a simple case of using the bracket notation. Since **variables** do not work with the **dot operator** - it treats the variable name as a literal key - we cannot use it in the ```for...in``` statement. 

N.B. Using ```for...in``` does not guarantee elements will be dealt with in order, so it should not be used with an **ordered collection** or array. 

__


With the section on objects, we know that keys can point ot any **data type**, including other objects. It comes in useful when we need to organise properties in an object. This is called **nesting** and is valid for all **data structures**. 

**Iterating** over nested data structures is a little bit trickier than iteration over flat structures. To do so, we will use **recursion**. In essence, a recursive function is a function that **calls itself**. It works with combinations of nested objects and arrays. 

```
function deepIterator(target) {
   if (typeof target === 'object') {
      for (const key in target) {
         deepIterator(target[key]);
      }
   } else {
      console.log(target)
   }
}
```

__

Working with arrays, we will encounter common **patterns**, like transforming every element of the array to **another value**, or searching through an array and only returning elements that match a certain **condition**. 

The latter is referred to as **filtering**. To do so we will use **callback** function, ie functions that are passed in as arguments of functions. 

Before getting into filtering, let's talk about **pure** and **impure** functions. If a pure function is invoked repeatedly the function will always return the same result, it is **predictable**. In addition, calling the function has **no external side effect** like making a network or database or altering any object passed as an argument. Impure functions are the exact **opposite**. 

When possible, impure functions should be **avoided** because: 
- Predictablecode is good, because it will make **writing tests** much easier. 

- Because pure functions have no external side effects it makes **debugging** easier. Let's imagine we have an error because an array does not contain the correct properties: 

  - If that array is returned from a pure function we check the **inputs**. If they are corret then the bug is **inside** the function. If they are incorrect, we simply need to figure out why.

  - If, however, the array is returned by an impure function, it is a treasure hunt that begins to find out **how** each impure function modifies the array. 

N.B. The fewer places an object is **modified**, the fewer places we need to check when **debugging**. 

So we want our filter to return a **new array** with the appropriate values instead of **destructively** modify the original array. It would look something like: 

```
function filter (collection, callback) {
   const newCollection = [];
   for (const user of collection) {
      if (callback(user)) {
         newCollection.push(user);
      }
   }
   return newCollection;
}
```

It is a clone of the built-in JS ```.filter()``` array method: 

```
array.filter(function (num) { return num > 3; });
```

Another built-in JS array method is ```.map()``` which **transforms** every element in an array to **another value**.  Writing it manually would be: 

```
function map(array, callback) {
   const newArr = [];
   for (const element of array) {
      newArr.push(callback(element)); 
   }
   return newArr;
}
```

**Non-destructive** update is an important concept to practice. **Destructively** modifying objects at **multiple** points in the code is one of the biggest sources of **bugs**.

```
array.map(callback);
```



## 5. Functions Deepdive

#### Statement vs. Expressions

A **statement** is a unit of code that accomplishes something but that does not produce a value. Many keywords are statements: 

- Variable **declaration**: ```const```, ```let```, ```var```

- **Iteration**: ```for```, ```for...in```, ```for...of```, ```while```

- **Controle flow**: ```if...else```, ```switch```, ```break```


An **expression** is a unit of code that produces a value. 


#### Function Expressions

So far we have learned about function **declarations**: ```function myFunc() {}```. However, the ```function``` keyword is adaptable: 

- When it is the first thing in a line of code, it forms a function **declaration** that creates a new function in **memory**, but doesn't produce a value just yet.

- When it is placed anywhere else, it forms a function **expression** that both creates the function in memory and returns that function as a value. 


#### Functions are Objects

Functions are known as **first-class citizens** of the language. A function can be: 

- **Assigned** to a variable. 

   ```
const myFunc = function() {
return 9 + 3;
};
myFunc();
// returns 12
myFunc() + 1;
// returns 13 
myFunc;
// returns f () {return 9 + 3}
myFunc + 1;
// returns f () {return 9 + 3}1
```

  - Function expressions are not **hoisted**. The JS engine ignores them during the **compilation** phase and evaluates them during the **execution** phase. Remember that the variable declaration will be hoisted but that the assignement only happens in the execution phase.

  - Like all JS objects, functions are **passed by reference**. When a new function is **created**, it is stored in a specific location in memory. However, when we **assign** a function expression to a variable we are not storing the entire function in memory. We are storing a **reference** to that location where the function lives. If we assign the value of that first variable to a **new variable**, the latter points to the **same location** in memory. 
If we **modify** the function, we change the object **in memory**, and every **reference** to the function will be modified accordingly.  However, if we simply reassign the variable to a new function it no longer points to the same location in memory. 

- **Stored** in a data structures: 

  - They can be stored and accessed in **arrays**

  - They can be stored as **properties** of an object. When a function is stored in an object we call it a **method**. So if you need to use the **dot operator** to invoke a function then it is a method. 

- The **return value** of another function: functions can return any type of value including functions themselves since they are just objects. We can also **store** the returned functions in variables to reference and call them. 


- Passed as an **argument** to another function


#### Anonymous Function Expressions

In a function **declaration**, the JS engine requires the presence of an **identifier**. For function expressions if it optional. Without an identifier, a function expression is referred to as an **anonymous** function expression. There is, however, an advantage in naming them: better **stack traces**. 


####  Iterate with ```.forEach()```

```.forEach()``` is a built-in method to iterate over **arrays**. It accepts one argument: a **callback** function. It is invoked once for each element and 3 arguments are passed to it: the current element, the index of that element and the array itself. 


#### Sort an array in place with ```.sort()```

The ```.sort()``` method **destructively** reorganises elements in an array according to their **unicode** value. However, it does not work so well when all words aren't equally capitalised. Inded the unicode value of uppercase letters is lower than the value for lowercase letters. 

To solve this we can add a **callback** to ```.sort()``` and it needs to have two **arguments**: the two elements it is being asked to compare. 

```
array.sort(function(a, b) {
   // comparison code here
});
```

The **comparison** function must return one of three values: 

- If ```a``` should come before ```b```, return a **negative** number

- If ```a``` and ```b``` are equal return 0

- If ```a``` should come after ```b```, return a **positive** number

If we are comparing **strings**, then we cam use the **```localeCompare()```** method. 

```
array.sort(function(a, b) {
   return a.localeCompare(b);
});
```

We can even make is **cleaner** by splitting the callback in its own **variable**: 

```
const comparator = function(a, b) {
   return a.localeCompare(b);
};
array.sort(comparator);
```

When sorting an array a **numbers**, the default comparison algorithm will not work. The ```.sort()``` method forces the numbers into **strings** and then compares each character. The unicode value of '1' is lower than the value of '2', so any number starting with '1' will be sorted before '2'. 

```
const nbSorter = function (num1, num2) {
  return num1 - num2;
};
arrayOfNb.sort(nbSorter);
```

Here the **subtraction** operation will return either a positive or negative number or 0 if numbers are equals. 


#### How to use ```.reduce()```

Sometimes, we need to **aggregate** results from an array's elements. We want to **reduce** the array to a single value. The solution to this problem is the ```.reduce()``` method. It takes two arguments: a **callback** function that will reduce each array element to a single value and the **initial value** the reduction should start from. 

```
array.reduce( function (accumulator, currentValue) {
  accumulator + currentValue;
});
```

#### More on functions that return functions

When a function returns another function, the **returned function** still has access to all **variables** declared in the **outer environment**. Because it retains access to these variables even after the initial function has **finished running**, we say that the returned finction has **closed** over those variables. It has formed a **closure**. 

Each **invocation** of the initial function with different **arguments** will create an **entirely separate closure** that retains **access** to the set of arguments passed in for that particular invocation. 
