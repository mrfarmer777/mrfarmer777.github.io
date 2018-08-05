---
layout: post
title:      "JavaScript - Beware the Non-Magic"
date:       2018-08-05 21:25:26 +0000
permalink:  javascript_-_beware_the_non-magic
---

Back in my self-guided coding days, I would have said that JavaScript was my native language. Though the initial learning curve was higher than that of Ruby or Python, I feel like all the syntactic breadcrumbs helped me to pay attention to the details of the code I was writing.  It was great training for the idea that the computer only performs exactly the actions you instruct it to (and only if it's all followed by a semi-colon).

Coming back to JS after diving into Ruby and Python felt a little strange. It was a bit like driving stick shift after years of automatic. After a while I was back in the swing of things. But compared to Ruby I was struck by the absence of that "Ruby Magic". Ruby seems to know what you're thinking. Want to know if any element of an array is odd? Use the `.any?` method. I love it. Javascript must be told, every step of the way,  what to do. However, it was exactly this attitude that brought me trouble in my jQuery project.

### RuShamBo with jQuery
My project is built on top of my previous asynchronous Rock-Paper-Scissors game. The 'fun' of the game would be seeing where you stack up against others and what moves you were playing. When I built the initial project, I utilized numerous instance methods to perform some basic analysis of the games people were playing.

```
#calculating your record against a certain opponent
def record
        "#{self.chal_wins}-#{self.chal_losses}-#{self.draws}"
    end


#finding the move you play most often
def favorite_throw
     throw_to_string={"r"=>"rock","p"=>"paper","s"=>"scissors"}
     throws=["r","p","s"]
     throw_arr=throws.collect{|t| self.games.count{|g| g.chal_throw==t}}
     throw_to_string[throws[throw_arr.each_with_index.max[1]]]
 end
```

With Ruby and Rails this data was available to fling all over the app! I could have it show up in a bunch of different ways just by calling it `@user.favorite_throw`. But JavaScript made me work for the same functionality. It seemed like access to this data was stuck down at the Model level, so I would have no way of getting it up to the view. This made me feel like I was going backwards - making my project *worse* than it was before. 

### Attack of the Serializers
The whole point of adding in jQuery and relying on JavaScript is so that data can be passed back and forth without page refreshes. Not to mention, multiple calls can be made asynchronously and non-blocking (the UI is never locked). This seems like it should be a perfect fit for my asynchronous game app! But to get at the data I needed some help.

ActiveModel Serializers are the most efficient way to expose this data to the jQuery front end using AJAX. Using asynchronous requests, I could ask for this information from a route (API endpoint) and have it available for use in the view. Look at all the information I have available!!

```
class UserSerializer < ActiveModel::Serializer
  attributes :id, :name, :image, :created_at
	
end

class MatchSerializer < ActiveModel::Serializer
   attributes :id, challenger_id, opponent_id

```

Lame.

The problem was that all my handy instance methods seemed out of reach! Sure, I could pull in a user's name and profile image, I could also pull in their associated matches and games! However, these models by themselves aren't interesting. Matches, for example, are just a glorified join table that consists of numbers. The real interesting stuff would come from instance methods and scopes. I toyed with the idea of recreating the instance methods client-side. For example, I could calculate the record using a javascript Match object. It seemed like too much work to put on the client's browser and I already had Ruby magic working to do this on the back end. 

That's when I found a simple solution in the strangest of places: [The Docs for the ActiveModel Serializers]! (https://github.com/rails-api/active_model_serializers/blob/0-10-stable/docs/general/serializers.md). When you list a symbol in attributes, it simply calls the get method of that attribute when serializing and serving the data. This means that all of the instance and class variables were back in play. I could easily render robust and interesting data via JSON. Once it was there it was easy to fling it around again. So my serializer looked like this:

```
class UserSerializer < ActiveModel::Serializer
  attributes :id, :name, :image, :created_at, :win_percentage, :record, :favorite_throw, :favorite_opponent
  
  
  
  has_many :matches
  class MatchSerializer < ActiveModel::Serializer
     attributes :id, :opponent_name, :active_games_count
  end
  
  
  
  has_many :games, through: :matches
  class GameSerializer < ActiveModel::Serializer
    attributes :id, :opponent_name, :status, :result    
  end
  
end
```

and the content it gave me looked more like this:

```
{
id: 3,
name: "Liz Lemon",
image: "https://upload.wikimedia.org/wikipedia/commons/5/53/Gray_-_replace_this_image_male.svg",
created_at: "2018-07-25T01:53:16.453Z",
win_percentage: 50,   
record: "6-2-4",
favorite_throw: "rock"
favorite_opponent: "Matt Farmer"
```

Much better!

### What Happens When You Assume
I almost spent a lot of time making a difficult work around for a problem that was already solved. I assumed that the non-magic of JavaScript wouldn't be able to interact well with the data Rails was magically handing to me. However, because JS is sitting on top of Rails and Ruby this magic is still there! It's just a little trickier to find. 







