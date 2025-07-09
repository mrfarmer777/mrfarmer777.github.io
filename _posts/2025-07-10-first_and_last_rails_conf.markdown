---
layout: post
title: "My First and Last RailsConf"
sub-title: "A reminder about the Rails community"
header-img: "img/gridboard.jpg"
tags: [rails, webdev, conferences]
---

## Past, Present, and Future

It's a bit strange to have my first RailsConf also be the last. After nearly 20 years, the conference is coming to an end. Ruby Central is passing the torch on to other organizations to organize the community's conferences. Folks have a mix of sadness and optimism. There's an undercurrent of the unknown in many of the talks and in the atmosphere around the conference. Every mention of coding as a profession has to acknowledge the looming presence of AI and its potential impact.  As someone who started learning Ruby and Rails in 2019, well after it was marked as "dead" for the third time, it would be easy to feel like I'm on a sinking ship. However, that's not the feeling I'm left with. Why?

### It's the People

RailsConf gives me the same feeling that drew me into coding in 2014. It's several rooms filled with _really_ smart people. More than that: smart people who care about each other, the community, and what they do everyday. Back when I was first inspired by coding, it was my students who lifted me up. They were all trying to work together to solve a problem. They were all so talented and focused on solutions that there simply wasn't time for competitiveness and doubts. In much the same way, it's a room filled with people that are so skilled at solving problems that finding a solution is simply a matter of time (and to some extent money). 

This possibly isn't unique to Rubyists. Coders are, by definition, problem solvers. What seems to make the Ruby community special is that the problems get solved "the right way". What I love as someone who came from the arts is that the "right way" involves words like "happiness," "beauty," and "elegance." Sure, the code has to work and be efficient. But writing in Ruby reminds us that this code is written for humans by humans (for the moment at least). RailsConf, like every Rails and Ruby gathering I've attended, puts this fact front and center. 

That all being said, here are some highlights I've found from the talks I attended so far. 

### Highlights

#### Mechanical Sympathy
Two consecutive, unrelated talks on Ruby Internals and 10 Database Mistakes and How to Avoid them, made reference to an idea of mechanical sympathy. In short, this refers to understanding the design of a system so as to operate it in a manner consistent with that design. If you know how a tool was designed, you are better equipped to operate it in a way that gives you the most benefit. A simple example: the best way to use a hammer is from the end of the handle. 

The database talk mentions this explicitly. If you take the time to explore how your database models the data, you will be able to use the database in a more efficient and performant way. A simple example: don't use the database like it's a queue, because it's not intended to work that way.

The other talk illustrated this idea more subtly by building a basic Ruby compiler. By illustrating how compilers lex, parse, and execute code you can learn how to write code that's easier for it to digest. 


#### The Rails Front-end We Need
The Rails front end technologies have changed a lot, even since I've been a part of the community. In 2019, Webpacker and various bundling technologies were a key part of what I was taught about managing front-end complexity. However, at my current job, we are all-in on View Components, Hotwire, and Stimulus. Is it perfect? No. But it does feel like the closest I've come to writing a front end that I enjoy.

Svyatoslav Kryukov's talk about Rails front-end evolution reminded me that Rails has always tried to put developer happiness at the forefront of development. Though the exact implementation has changed quite a bit, the goal is to provide the best available defaults while still allowing flexibility. When I look back at the various points along the history, the Rails default front end approach has consistently been defensible, if not the best available at the time.

This was further illustrated when DHH later likened Turbo (which I love) to Webpacker in that he can't wait until it's no longer needed and it can be unshipped. In that way, the front end we need, but not what we deserve. 

#### Cool People Talking about Cool Stuff
At the opening of day 2 (Community Day) we were treated to an informal chat among some Rails luminaries. It was a mix of nostalgia, insights, inside jokes, and calls to action. Perhaps because my brain was reeling from the knowledge shot at me from the day before, but I appreciated this less technical window into the cool folks that maintain the Rails codebase. I found it refreshing and inspiring. As someone who struggles with some imposter syndrome, it was awesome to hear the message from these folks that not only do I _belong_ here, that I'm _needed_ here. It was a reminder that even smart, capable folks need help and that the community takes contributions from _everyone_. 

It was a nice reminder of Conway's Law that the structure of software will mirror the structure of the human organization that created it. This law usually applied more literally, for example, to the boundaries of an application. But I kept thinking of the group of people on the panel and how they help to make the Rails community what it is. The codebase might carry on if they left, but the community would definitely change.

### <3 Ruby

One last theme that comes up is a love and appreciation for the experience of writing Ruby code. It was fun to share and relive the experience of learning Ruby for the first time. In talking with colleagues there was a common feeling of being "spoiled" by Ruby. A feeling that doing the same thing in any other language feels like unnecessary mental work. One of the talks even mentioned how Rails (and by extension Ruby) "stayed out of the way" and let the speaker's team focus on the unique problems they were actually trying to solve. Even if I had gotten nothing else out of the talks, that reminder was worth the trip!

