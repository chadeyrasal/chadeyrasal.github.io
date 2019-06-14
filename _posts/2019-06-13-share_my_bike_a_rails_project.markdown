---
layout: post
title:      "Share My Bike, a Rails project"
date:       2019-06-13 14:24:07 -0400
permalink:  share_my_bike_a_rails_project
---


Flatiron School's third project consists in building a Rails app with a series of requirements that build up on top of what has been taught in the Rails section as well as all previous subjects. 

The **Share My Bike** project consists of an AirBnb like web application that allows you to rent bicycles instead of homes. 

For this project there are **5** main requirements: 

**The models must include *belongs_to*, *has_many* and *has_many_through* [ActiveRecord associations](https://guides.rubyonrails.org/association_basics.html) and the join table built for the *many_to_many* relationship must include attributes other than the mandatory foreign keys. **

This requirements seemed rather simple to implement. So I went for it and built my models and matching database tables: 
* **User**: owner or renter of bicycle
* **Bicycle**: each bicycle listed would be available to rent
* **Trip**: every time a bicycle is rented, this constitutes a new trip, that can be evaluated later by the user who rented the bicycle
* **Neighborhood**: each bicycle is listed in a specific neighborhood
* **City**: each neighborhood belongs to a city, which limits the risk of homonyms
* **Country**: each city belongs to a country, which also limits the risk of homonyms

The implementation of the relationships was rather straightforward: I had my has_many and belongs_to and the trips table was going to be the join table between bicycles and users. 

But then: **problem**. Depending on the use case, one model would need to have a different relationship with another model. I hear you, it is confusing. It was for me when I realised it. Let me explain: 

* I wanted my app to be built as the skeleton of something that could, potentially, one day become a **real product**
* To conceive it like a real product I tried to **put myself in a user's shoes**: a user would want, with the same account, to be able to list a bicycle they own *AND* rent a bicycle from another user. 
* So you see the issue: on the one hand I have a user - let's refer to them as owner - that has many bicycles, and on the other hand a user - or renter - that has many bicycles, through the trips / bookings that they have made. 
* I started looking for solutions, starting with my old friend Google. As always, I found the most difficult part was to figure out how to **formulate** the problem. I came up with a question that was grammatically correct and seemed to grasp the core issue. 
* That lead me to two options: Single Table Inheritance (STI) and Polymorphic Classes. [This article](https://www.freecodecamp.org/news/single-table-inheritance-vs-polymorphic-associations-in-rails-af3a07a204f2/) explains the differences, the pros and cons of each methods and gives explicit use cases. 
* Unfortunately none of the exampes and explanation I found about these two options seemed to match what I was after: 
   - The **STI** would require two tables: an owner table and a renter table that would both inherit from a User class. So there would be no code duplicate but I would end up with **two different tables** for users, which was the **opposite of my primary objective**. There were possible workarounds, but the very fact that I needed them at the beginning of building my app did not seem ideal or clean.
   - It is still not perfectly clear how **Polymorphic Associations** work exactly. In addition, the whole process of adding the **-able** suffix to a class name just does not sound right to me. One day in the future, though, I will probably need to use these. I am confident that, by then, I will have a greater maturity as a developer that will allow me to take in this kind of concepts more naturally.  
* With regards to the situation, I figured it was time to ask for help. It was worth it, since the instructor I spoke to lead me into the right direction - thank you Kevin! To reach my goal I would have to [customize](https://api.rubyonrails.org/classes/ActiveRecord/Associations/ClassMethods.html) my ActiveRecord associations: 

```
class Bicycle < ApplicationRecord
    belongs_to :owner, class_name: "User"
    has_many :trips
    has_many :renters, class_name: "User", through: :trips
end
```

```
class Trip < ApplicationRecord
    belongs_to :bicycle
    belongs_to :renter, class_name: "User"
end
```

```
class User < ApplicationRecord
    has_many :bicycles, foreign_key: :owner_id
    has_many :trips, through: :bicycles
    has_many :reservations, foreign_key: :renter_id, class_name: "Trip"
end
```

And that solved my problem. As a note, it was all very experimental and I was not sure at all how I was going to use it. There were a few moments of doubt but it was, in the end, rather straightforward. 


**The models must include reasonable validations and forms should display validation errors**

Building reasonable validations was rather easy. I did not need validations for all my models, only for those that users could **interact** with directly: 

* User: signup form
* Bicycle: all attributes of the bicycle
* Trip: a start date and end date

Validations were for **presence** of attributes, and **uniqueness** of user email address, so nothing too difficult. 

To build the forms that would be subject to validations I used the Rails [form_for helper](https://guides.rubyonrails.org/active_record_validations.html#displaying-validation-errors-in-views) that adds an extra div around the form field if there is a validation error. The styling of this extra *div* - that has a class of *field-with-errors* - is rather simple to write. There is no scpecific ActionView helper to display the error messages on the view but with a few Rails methods, all messages are displayed on top of the view when there is a need. 

**At least one class level ActiveRecord scope method must be included**

I kept the scope methods for the end since I wanted to have a descent **dataset** and only needed them to display data on the homepage. I wanted to write two scope methods: 

* The **most popular** bicycles - the bicycles that have the largest number of trips booked
* The **latest listed** bicycles

The second one was simpler than the first one. It took me just a few tries to have it working. The first one was another level of difficulty. It was not just ordering data from the bicycles table according to an attribute of this table. It involved ordering data according to the count of objects that were related to each instance through an ActiveRecord association. 

It took a lot of time, [documentation reading](https://guides.rubyonrails.org/active_record_querying.html), experimenting and imagination to come up with something that worked. It does the job for sure, and I have tried to refactor it but I am certain that there is a much simpler way of doing it that I just have not found yet. But isn't there always a simpler way that you have not thought about? I think there is...



**The app must provide standard as well as third-party authentication systems**

The **standard** authentication system was not too tricky to create, as it is very similar to how the Sinatra authentication system works. I nonetheless took the time to watch tutorials and read about best practice to make sure I had written good quality code. 

The **third-party** authentication system was a little bit more difficult. I initially wanted my users to be able to login with Facebook and Twitter to cover as many users as possible. Although OmniAuth via GitHub has a reputation of being easy to implement it did not really make sense to use it for an app designed to rent bicycles... 

I implemented the **Facebook** authentication without issues. The **Twitter** authentication did not happen in the end because my request for creating a developer account was denied. I am still working on solving that for the future. 


**The app must have nested resources including a nested new route and a nested index / show route**

The nesting part was really fun. I buit most of it using **Behavior Driven Development**. Every single line of code I wrote, I tested in the browser. I took the time to build a good enough dataset to make the BDD possible. So the whole process was quite smooth. 

The choice for the **show** / **index** nested resources was instinctive and did not require much thinking. The **new** nested resource required a little bit more thinking. The nested new resource was to create a new bicycle nested under a specific user. It did not seem like an obvious choice and nothing else did. Moving further into writing my app, a very simple one came along. Since the link to book a bicycle for a trip is on the bicycle show page and the trip belongs to a bicycle, I nested the new trip form under the bicycle show page. It works really well with the logic of the app. 

I ended up nesting way more resources than just the required new and show / index, but it made sense in the context of the app so I went for it. 

**A final note on planning your code**

> It's all over the place, I am not managing to do anything, what do I do? Do I use Single Table Inheritance or Polymorphic Associations? Which resources do I nest? How do I create trips?

This is literally what I asked myself after a few days of working on the project. I had so far always managed to work out a plan in my head and abstract it enough not to have to go back to basics: 
* A pen and paper
* Lists of the required features and of the extra features taht would be nice to implement
* Drawings of the database tables and their relationships
* Drawings of each page of the web app, what I wanted it to look like, what kind of data I wanted to display and what links / buttons made sense for the users

For the duration of this project I have had to think a little bit like the product manager, the UI designer, the UX designer, and the developer. That was really nice, even on such a small scale. I look forward to what's next!
