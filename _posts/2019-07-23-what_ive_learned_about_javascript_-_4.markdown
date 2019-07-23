---
layout: post
title:      "What I've learned about JavaScript - 4"
date:       2019-07-23 18:39:43 +0000
permalink:  what_ive_learned_about_javascript_-_4
---

This article builds up on the previous 3, where I have tried to explain the **basic concepts** of JS, the **general principles** of the language - scope, scope chain and hoisting - **data structures** - JS objects and arrays - **looping & iteration** and extra concepts about **functions**. 


## 6. Object-Oriented JS

### Creating Objects

So far we have mainly checked **primitive** data types like numbers and strings. We have also seen that it is sometimes convenient to represent data with **objects**.  But we reach a point where we have several objects with the same **keys**, just having different **values**. 

It would be interesting to be able to build objects with the same **attributes** (keys), while **assigning** different values to those keys. To do so we use a **constructor function**. It operates as a factory for new objects. 

```
function User(name, age, hometown) {
  this.name = name;
	this.age = age;
	this.hometown = hometown;
}
const u = new User('userName', 'userAge', 'userHometown');
// returns {name: 'userName', age: 'userAge', hometown: 'userHometown'}
```

So we created a function called ```User```. The name of the function is captalised as a **convention** to indicate this will be used differently. When we call this function with ```new```, JS **creates** a new object and this new object is immediately **returned** from the function. 

Our function takes in three **arguments**. We need to use these three arguments to **assign attributes** to the newly created object. To access the newly created object we use the keyword ```this```. It allows us to **reference** the object receiving the method call from **inside** the method call. 

The object that is returned from the **constructor** function behaves just like any other object. We can create as many objects as we want. 


### Object Methods

To add some **behaviour** to our objects we need to add object **methods**. For each object to have the same behaviour, we simply need to write the method in the constructor. 

To **reference** data in a method call, we simply use the ```this``` keyword. 

```
function User (name, email) {
   this.name = name;
   this.email = email;
   this.sayHello = function () {
      console.log(`Hello World, my name is ${this.name}`)
   }
};
let ben = new User ('Ben', 'Ben@gmail.com')
ben.sayHello();
// returns "Hello World, my name is Ben"
}
```


### Prototypes

So, all the objects built with the constructor function have a ```sayHello()``` method. For each object the method seems to be pointing to the **same** function. However, when we compare them, they are **not equal**. This means that they are not the same function and do not point to the same **location in memory**. So all these functions are the same but we **generate** one every time we create a new object with our constructor. 

But JS objects hav something called a **prototype**. 

```
function User (name, email) {
   this.name = name;
   this.email = email;
};
User.prototype.sayHello = function () {
   console.log(`Hello World, my name is ${this.name}`);
};
let ben = new User ('Ben', 'Ben@gmail.com')
ben.sayHello();
// returns "Hello World, my name is Ben"
}
```

By moving the ```sayHello()``` method **outside** of the constructor function we keep it fron being **re-declared** every time an object is created. We now only have **one location** in memory for ```sayHello()``` no matter how many time the constructor is used. 

If there is an **object method** and a **prototype method** with the same **name**, the newly created object is searched for matching properties first. If no match is found, then the object's prototype is checked. 

So we keep the **assignment** of properties that point to **data** inside the constructor, the assignment of properties that point to **functions** outside of the constructor. 


### Classes

What we have seen so far works well, but it would be ideal if we could have the constructor and the methods **grouped** together. ES2015 provides an abstraction for OO with the ```class``` keyword. 

```
class User {
   constructor(name, email) {
      this.name = name;
      this.email = email;
   }
   
	 sayHello() {
      console.log(`Hello, my name is ${this.name}`);
   }
}
```

N.B. We **instantiate** new User the exact same way. 

We can easily **inherit** from classes. So without defining anything in the new class, we automatically get all the features of the User class. We can also  **overwrite** inherited methods by defining another with the same name. The methods defined inside the **extended** class are not accessible to the initial class. By using the ```super``` keyword, we call the ```.sayHello()``` method of the **superclass**, ```User``` in our case. 

```
class Teacher extends User {
   teachMath() {
      return `My name is ${this.name} and 1 + 1 = 2.`
   }

   sayHello() {
	    super.sayHello();
      console.log(`Hey there, my name is ${this.name}`);
   }
}
```


### This

```this``` can be unpredictable. But generally speaking: 

- **Outside** of any function, it refers to the **global object**. In browsers it's the **window**. 

- Inside an **object method**, it refers to the object that **received** the method call

- Inside a **standalone function**, it will either default to the **global object** or be **```undefined```**

```
let person = {
   greet: function () {
      return this;
   }
}

person.greet() == person
// returns true

let greetFn = person.greet;

greetFn == person;
// returns false

greetFn();
// returns window
```

So when we call ```person.greet()```. ```this``` refers to the ```person``` object. When we assign ```person.greet()``` to local variable ```greetFn```, this variable does not refer to ```person``` at all because it has no idea what ```person``` is. It only **points** to the function. When calling ```greetFn()``` we are not calling the property **on the object**, we are simply **invoking** the function. When the function is invoked, it is a simple **standalone function**, where ```this``` refers to the global object. 

Another occurrence where ```this``` becomes the **global object** is when we call functions within functions. The function declared within the method is not itself a **method**, but a simple **function**. 

A **callback** is a function inside a function. So ```this``` is systematically the global object in a callback. 

**Arrow functions** however, do not have their own ```this```. Instead they use whatever ```this``` is defined within the **scope** their in. 

Methods defined inside **classes** behave similarly. The inner function's ```this``` defaults back to ```undefined```, as it does with regular functions. There again, with an **arrow function**, this would not be redefined. 


### ```this```, ```bind()```, ```call()```, ```apply()```

Since functions are objects, we can **borrow** functions from one object and use them on another one. We can also **hold onto** a function with specific arguments and call it **later**. 

Usually, when we invoke a function, we call it by its **name** like ```function()```. But sometimes, we want to modify how a function is called by modifying the **```this``` property**. 

JS gives us two ways to call a function with an **explicit** value for ```this```. 

```
function.call(explicitValueForThis);
```

A function is an object, so a function can have **properties**, like ```this```. IT can also have its own **methods**, like ```call()```. Instead of invoking the function directly, we invoke its ```call()``` method. The first **argument** for ```call()``` is the object we wish to **assign** to ```this``` for the function. For a simple function - with no **argument** - we can use ```call()``` or ```apply()``` interchangeably. 

If the function has **arguments**, things differ slightly: 

- For ```call()```: the first argument is the object we want for ```this```. Then any function argument must be listed in **order**. 

- For ```apply()```: the first argument is the same, but it expects to receive an **array** of function arguments. 

JS provides the ```arguments``` object within a function that contains all arguments **passed** to that function. This is important because even without having any arguments upon **declaration**, it is still possible to pass some. 

```arguments``` is not really an **array**, so it does not have access to all array **methods** like ```slice()```. We would then use ```Array.prototype.slice.call(arguments);```. This is known as **borrowing** a function. It is a great way to use functions of another object without having to write them **explicitely** in the object. 

Now, sometimes, we need to be able to **hold onto** the function and **delay invoking** it until later. This is where ```bind()``` comes in. 

With ```bind()```, the first argument will be the value for ```this``` in the **target** function, then any argument comes in order after that. With ```bind()```, we **create** a new function with that ```this``` value set, and can **execute** it at any point later in time. 

Since it is a **new** function created, we need to be able to hold onto it. So we need to **bind** the function into a new function that we can then **call** at will. 

With ```setTimeout``` and ```setInterval```, we can use bind for **asynchronous execution**. 


### Object Relations

So far, we have seen how to build different types of objects using the **class syntax**. Let's say we have a ```User``` class and and ```Item``` class. If we have a user with multiple items we want to be able to **associate** them. We can choose between **many-to-many** or **has-many-belongs-to** relationships. 

To represent data we can store them in a JS object called ```store```. The store will represent all the **instances** that have been **instantiated**. In our case of users and items, the ```store``` has two keys, each of them pointing to an array. 

To be able to **interact** with our data, we need to link **instances** to a **store**. Each item object should have an **id** that should increment everytime a new item is built. 

```
let itemId = 0;
class Item {
   constructor(price, name) {
      this.id = ++itemId;
      this.name = name;
      this.price = price;
   }
}
```

Above, we use ```++itemId``` instead of ```itemId++``` so that it returns the value after **incrementing**. 

Then we need to **insert** new objects into the **store** and allow the ability to **associate** a user with an item. 

```
let store = {items: [], users: []}
let userId = 0;
class User {
   constructor(name) {
      this.id = ++userId;
      this.name = name;
      store.users.push(this);
   }
}
let itemId = 0;
class Item {
   constructor(price, name, user) {
      this.id = ++itemId;
      this.name = name;
      this.price = price;
      if(user) {
         this.setUser(user);
      }
      store.items.push(this);
   }
   setUser(user) {
      this.userId = user.id;
   }
}

```
