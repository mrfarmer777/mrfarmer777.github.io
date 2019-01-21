---
layout: post
title:      "ActiveRecord Scopes"
date:       2019-01-21 06:32:50 +0000
permalink:  activerecord_scopes
---

### Inside the Black Box
In an effort to "build up my weaknesses until they become strong points", I am diving in to concepts that I initially misunderstood to some degree. I refer to these concepts as "black boxes" because, at some point, I didn't know how they really worked. You can build on a lot of these black boxes without running into trouble, until something breaks any you have no idea why. Maybe this is how you feel about your car or your refrigerator or your interpersonal relationships. For me, the first target will ActiveRecord scopes specifically because they eluded me during a livecoding session (L's were taken, many "umms" were said.) 

But since we cannot learn about something we haven't first wondered about, let's start with a problem that scopes can solve for us.

### The Problem: So Many Records, So Many Permutations
Let's say you want to make a match-making program where people of various genders want to be matched with others of a of various genders based on their answers to survey questions. We also want to handle a variety of gender pairings allowing users to select more than one gender to be matched with (because everyone deserves to have fun with this!) Evaluating the pairing as a potential "match" is computationally costly so we don't want to match everyone with everyone else. Let's narrow things down and only evaluate pairings that match the gender preferences of users involved. No problem, if you're a woman interested in men just grab all the male users. We can just do this with a User class method that returns all the male users
```
#models/user.rb

#basic class method
def self.males
    User.where( gender: "male")
end
```

Maybe you're fancy and thinking ahead, so you want to just build a class method to return users of an arbitrary gender.

```
#models/user.rb

#more flexible class method
def self.filter_by_gender(g)
    User.where( gender: g )
end
```

Sweet, this works fine and returns the users we want. But let's look further down the line. To really make good matches we have to find users of the gender(s) we are seeking who are also seeking the gender we ourselves express. For that matter, we also don't want to match people with themselves. After all, people tend to agree with, you know, *themselves* on a survey about their interests. This means we need to make a really narrow and flexible selection of our users based on several criteria that nest within each other. 

*So for a male users seeking male matches, we will need to find other males (not the original user) who are also seeking males.* 

This can be done with class methods, but let's do it with scopes!

### Enter the Scopes
Let's begin by accomplishing the same task as above with scopes.
```
#models/user.rb

#Users with scopes
class User < ApplicationRecord

    #Retrieve male users
    scope :males, -> { where(gender: "male") }
		
		#Retrive users with desired gender
		scope :preferred_gender, -> (g) { where(gender: g) }
end
```

And you can quickly call them like so

```
User.males   #Returns ActiveRecord::Relation that includes the male user instances, all other relationships intact too
```

On their face, scopes are the same as class methods with more sugary syntactic goodness. For all intents and purposes, they are the same thing. They allow us to create quick, flexible selections from our users using the great sql helpers (where, select, group, etc.) What's more, you can chain scopes together and find just "males who aren't me are also into males and indicated they're ready to be matched."

```
scope :other_males_seeking_males, -> { males.not_me.where( seeking_males: true, ready_for_match: true) }
```


### My Black Box
I didn't really digest the syntax of scopes at first so I didn't see why anyone would use them. When I was asked to refactor a class method as a scope, I had a very difficult time doing so. Instead, I was stuck coding out the specific class methods I wanted to eventually chain together and I had difficulty deciding which class method should call which other class method or if it should all be one class method. This can slow you down and ain't nobody got time for that.


Working with scopes now, it is much easier to create numerous reusable, dynamic selections from large tables for computationally expensive calculations. Making lean scopes that perform only the necessary selections for the desired step helps everything go faster and move more efficiently.

