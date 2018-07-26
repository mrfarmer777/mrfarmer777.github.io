---
layout: post
title:      "Bi-Directional Self-Referential Relationships"
date:       2018-07-25 22:27:25 -0400
permalink:  bi-directional_self-referential_relationships
---

### They're scarier than they sound.

### Background
[Ru-sham-bo](https://github.com/mrfarmer777/ru-shambo) is a play on Roshambo (Rock, Paper, Scissors, but with Ruby!)  In essence, it is a multiplayer game that flows in very much the same was as Words with Friends. You start by making a match with another player. Once you have a match, both players can start games that belong to this match. From there, players can compete to get on top of the leaderboard and vie for the best record and the highest "Ru-Rank". This little game is in the spirit of fun and community.

### A Critical Hurtle

To build this project, I needed to allow users to create `matches` (connections very similar to friendships in social networking). However, I also wanted these matches to have several `games` as child objects. 

![Initial model relationships](https://i.imgur.com/kJcgdN5.png)

The relationships seemed complicated, but I thought they were critcal to my app. I did some research on the benefits and drawbacks of `has_and_belongs_to_many` relationships, but they were almost universally panned for their inflexibility. Several posts I found suggested I use some `has_many through` relationships. Since these are required, recommended, and suggested by my section lead I decided that was the way to go. So my objects were going to be related like this.


![Imgur](https://i.imgur.com/5FIfeRm.png)

Wonderful! Problem solved and project requirement met. Two birds with one stone (I hate birds apparently). But a problem came up when I would save new matches to the database. Take this example entry in the `matches` join table. When I write a new match to the table, one user will see themselves as the 'Challenger' and the other user will see themselves as the 'Opponent'. 

![Imgur](https://i.imgur.com/5aGkicA.png)

This won't do! When I try to have users access their games, I have no way of knowing which matches they are the 'opponent' and which they are the 'challenger.' The matches should behave the same way no matter which one you are. So, I stumbled on an [article](https://collectiveidea.com/blog/archives/2015/07/30/bi-directional-and-self-referential-associations-in-rails) that helped me get this right. 

#### A Clever Solution

In order to make sure the relationship would be bi-directional (each user would be able to interact with the match from their own point of view), I would have to write two matches to the join table each time. Each match would have an 'inverse' or 'reciprical' match. So my join table now looks like this:

![Imgur](https://i.imgur.com/xe5Ua9H.png)

Great! Another problem solved! But this opened up more problems (or birds, I guess). Now, I've got to make sure that when one match is created, the other is automatically created. I've also got to make sure that when a match is destroyed, so is its bizzarro-world twin. The article above helped me implement an automated solution. 

``` ruby
#Ensure a single one-way match can exist between each any 2 users (reciprication ok)
    validates :opponent_id, uniqueness: {scope: :challenger_id}
    
    #Handling automatic reciprocity for matches
    #After each match is created, create its inverse automatically
    after_create :create_inverse, unless: :has_inverse?
    #Destroy them both when one is destroyed.
    after_destroy :destroy_inverses, if: :has_inverse?
    
    #creates a second, inverse match for reciprocity with opponent
    def create_inverse
        self.class.create(inverse_match_options)
    end
    
    #checks if a match already has an inverse set up
    def has_inverse?
        self.class.exists?(inverse_match_options)
    end
    
    
    #destroys all inverses when a instance is destroyed
    def destroy_inverses
        inverses.destroy_all
    end
    
    #finds all inverse matches for a given match, 
    #WE CAN ONLY HAVE ONE MATCH PER PAIR!
    def inverses
        self.class.where(inverse_match_options)
    end
    
    #returns reversed params for an inverse match
    def inverse_match_options
        {challenger_id: opponent_id, opponent_id: challenger_id}
    end
		```
		
Now, after every match is created an inverse match is automatically created as well. When a match is destroyed, so is its counterpart - all without me having to manage each one. Take that birds!

### Didn't I solve this already?!

The victory was short-lived however, when I realized that I wanted each of these matches to have *child objects* in the form of `games`. (Whose choice was that anyway?) The exact same problem had come around again! Now that I had 2 match objects for every pair of `users`, I would need two `game` objects for every game, but they'd each be linked to a different `matches` which were each linked to different `users`. 

Simply put, when I made a `game` I could easily generate an inverse `game` too, but how would I know which `match` to link it to? If only I had some method that knew which `match`-B went with which `match` -A. 

### #inverses and recip_game_id to the rescue
Here's how I made it work:

When a new game is created from a match the following steps are taken:
1. The `gameA` instance is created and linked to `matchA` automatically by the HTML form.
2. When the `gameA` is created, a helper method would automatically create a reciprical `gameB`. 
3. Next, `matchA.inverses` is called (it was originally used to avoid an infinite loop of making inverses), but it returns the reciprical match that we can use to link up to `gameB`
4. Each game has an attribute `recip_game_id` that is assigned so each game knows about its twin directly!
5. When both games have a `recip_game_id` present, the loop is completed.

It's confusing, but this is a diagram that helped me try to figure it out:

![Game Creation Loop](https://i.imgur.com/Tk01v5A.png)


### Lessons Learned

Though this problem was very frustrating, it was also really engaging to solve. I love when there are multiple ways to solve a problem; weighing their relative merits is really interesting to me. I am certain that there exists a more eloquent way to do the same thing I've done here. However, I am happy with the depth of understanding of AR relationships I gained from this implementation. Though it makes my database twice as large, it has held up this far an enabled me to build a project of which I am very proud!


