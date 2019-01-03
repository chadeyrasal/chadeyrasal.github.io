---
layout: post
title:      "My very first app, what a feeling..."
date:       2019-01-03 12:42:43 -0500
permalink:  my_very_first_app_what_a_feeling
---


Today, I finished writing my very first app. Although it is just a CLI application and I have relied quite a lot on the code I wrote previously through all the labs and exercises, what a feeling of accomplishment!

When I first started thinking about what I could create that would be (sort of) useful, original, correspond my personal interest and represent where I live, I admit I was a bit skeptical. But a Sunday trip to the local Farmers Market in East London gave me the idea: find a website that lists all Farmers Markets in the United Kingdom and do something with it. 

That part turned out to be rather easy: 2 minutes on Google and I found a website that seemed to offer pretty much everything I wanted. 

Then I started thinking about how I was going to present all this information to the user. There are more than 300 markets listed on the website I used and giving that out the user seemed of little interest and totally user un-friendly. The logic would be to group markets by location and present details of each market to the user, but only if the user was asking for it. 

Postcodes in the UK are used for many activities of everyday life. For example, when someone gives you the address of a restaurant, or their address, they do not give you the full address but rather the postcode and door number. And that's everything you need. Finding an exploitable list of postcodes turned out to be a little bit more complicated than I had thought initially, and i rapidly went from the idea of postcode, to the idea of district, way easier to manage. 

The structure of the application was rather obvious: a Controller class to organise the general flow of my application, a Market class to regroup all the instances of markets I could find online, a District class to regroup all UK district and a Scraper class to scrape the data I needed from the two websites I planned to use. 

With that in mind I watched all the videos that were suggested for this project and created my first gem using bundler. Not knowing it would affect the structure of my gem I created a gem in two words: farmers-markets. After a few hours working on it I thought to myself: oh god, what have I done, I have folders and files all over the place and I am totally lost with all these dependencies to modify. I ended up creating a brand new gem called farmers_markets, and that made my life way easier. Indeed, creating a gem with a '-' creates a more complicated structure. I had a 'farmers' folder and inside this folder a 'markets' folder. This is a mistake I did once and will not do again. From that moment my life became way easier. 

I started writing the code I needed to have a decent structure and control flow. I created a menu that would interact with data I mad up. Everything was going well until I started making the data real by scraping the website listing all districts. It turned out to be a complete nightmare and none of the CSS Selectors I was using were giving me the data I was after. Since the data I needed was also present on the website listing the markets but needed reformating, I decided to put the districts website on hold and focus on the other one. 

Now that the application is finished I realise it was a good move since I managed to do everything I wanted without using the website that was causing me so much trouble. After finishing writing all the code I needed I realised I did not even need that District class after all. After triple checking it was not breaking anything, I removed the District class as part of the refactoring process. 

Looking at my code now I am quite pleased and proud of what I achieved. I have tried to make it as DRY as possible and have cleaned up all the lines of code that were making my eyes bleed. But I am sure that looking back in a few months I will see many new ways of refactoring this code. I find that it is part of the magic of code: every day I have new ideas and I am able to abstract more than the day before. 

Completing this project has given me a confirmation that learning how to code was what I was looking for: I have fun, I learn at least one thing every single day and I get to build things. What else could I wish for?

