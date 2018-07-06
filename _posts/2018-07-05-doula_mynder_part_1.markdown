---
layout: post
title:      "Doula Mynder (Part 1)"
date:       2018-07-06 01:56:34 +0000
permalink:  doula_mynder_part_1
---

## A Sinatra Development Project

### Description and Motivation
I've been wanted to tackle this project for a long time: A client management app for my wife, Kelly, who is a professional [Birth Doula](http://www.tolabor.com/about-tolabor/faqs/). She has been involved with Birthwork for over 2 years and I am always amazed at how she stays organized. She keeps track of so much vital information for multiple clients simultaneously. At times, this information needs to be available on the spur of the moment. As her business has grown, the need for a more efficient solution has too.

As it turns out, many of the products I've found are either too large or too small. My wife, like many of her colleagues, does doula work part time, and freelance. This means there isn't any money in the budget for large-scale healthcare management solutions that are out there. Additionally, many of the freelance time management apps are intended for consultants and creative professionals. They are typically missing the critical emotional and health data components that she needs for her business. 

On its surface, this app seems simple enough - we're just passing data around some views. However, I was never able to teach myself enough code to bring it to fruition. After gaining some valuable experience with Sinatra and ActiveRecord I feel like I finally have the right tools for the job!

### Project Goals
The nature of the project matches very well with the requirements for `has_many` and `belongs_to` relationships. Doulas (`users`) will have multiple `clients`, `clients` can have multiple `births`, and each client-birth can have multiple `entires` (things like birth plans and preferences as well as payment and billable hour details). 

Beyond the goals required for the project, I'd also like to accomplish the following tasks:
* implement some basic front-end design through use of Bootstrap 4
* implement TDD with Rspec
* improve the frequency and quality of commits throughout the project
* continue to improve the project beyond the scope of the Flatiron coursework.


### Keeping it Real
I've been hoping to work on this project for long enough that my ideas might still be a bit outside of my abilities. However, I have routinely seen that this is the best way to learn something new. Start with the vision and keep going until it becomes a reality. I know that to complete this project as I intend, I'm going to have to learn quite a bit more about CSS, ORM, and working with dates and times in Ruby! It's going to be tough, but fun!

Checkout the progreess [here. ](https://github.com/mrfarmer777/doula-mynder)

