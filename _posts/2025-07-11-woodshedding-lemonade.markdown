---
layout: post
title: "Woodshedding: Lemonade Stand"
sub-title: "Practicing the basics"
header-img: "img/gridboard.jpg"
tags: [ruby, practice, code kata]
---

### Preface: Woodshedding?
This week's RailsConf has reminded me how badly I need to practice coding. Nearly every speaker I saw advocated for the need to practice explicitly or implicitly. To that end, I'm spending time shedding some coding fundamentals in Ruby. Not to be confused with developer [bike shedding](https://patrickkarsh.medium.com/bikeshedding-in-software-development-df72e8bfe431), for musicians the term "shedding" (or ["woodshedding"](https://en.wikipedia.org/wiki/Woodshedding)) refers to the idea of heading out to the woodshed with your instrument because you sound _bad_. It implies that you've got real work to do and you need some intense practice. This typically happens out where you can make your mistakes freely, without worrying about who can hear you. As you might guess, shedding is difficult but essential and I've been neglecting the practice for far too long. 

### Code Kata: Lemons to Lemonade
As I write this, I'm currently in the midst of an 11-hour layover at the Philadelphia airport thanks to a weather-related flight cancellation. As such, I'm trying to make lemonade out of lemons by using this time to practice a basic kata by implementing a simple Lemonade Stand CLI in Ruby.

#### Kata A/C

- The user can input how many packages of lemonade they purchased and how much each one cost
- The user can input how many cups they purchased and how much they cost
- The user can input the price they will sell lemonade for
- The user can quickly "sell" a cup by simply touching a key and be presented with:
  - the gross revenue
  - the current profit
  - the number of cups sold
  - the number of cups remaining

#### Practice Goals

- To practice basic Ruby OO patterns
- To practice basic TDD principles
- To practice small and manageable design tasks


#### Reflection

I ended up just using 3 classes to get the basics going:
- Cup Inventory
  - stores the cups available to sell and the cup costs
- Lemonade Inventory
  - stores the lemonade available to sell and the cost of it
- Store
  - manages the cups of lemonade sold from the inventory
  - calculates profit from inventory costs
  - determines when the store is out of inventory
- _water, ice, and other costs are neglected because it's dawn at the airport food court. Chill._


##### Joyful Ruby

I wanted to work with plan old Ruby for this. I worked largely without AI completions or anything as well. This forces me to think about what I am doing and why. While deeply engaged, I am reminded of why I love Ruby. It feels like a very thin wrapper around my ideas. It makes things so easy to convey and always feels good to write. This isn't news, but is important to call out.

##### I am _Rusty!_

I found myself looking up the simplest things: getting started with RSpec in plain Ruby, how to manually import files, how to get user input in the CLI in Ruby. Even if I give myself some grace considering that I did this at 3:45 AM, that shows that I'm spending my time well to practice these fundamentals. These are the things I want to be automatic again.

##### Make a Decision and Live With It

This isn't production code, it's practice or a sketch. But I spent a lot of time second-guessing my approach even to a simple project. How should I structure the classes for this? Is that a stupid idea? What if I did it this way? The basic OO patterns are still not automatic for me either, so I spend some time swirling around before I finally just chose a direction and started moving. 

#### Conclusion

Heading out to the woodshed is a great idea and I want to continue finding time to do it! I want good OO practices and patterns to be top of mind or "under my fingers" to borrow more musician-speak. The only way to get better at coding is to _code_. So that's what I'm doing! In that way: mission accomplished.




