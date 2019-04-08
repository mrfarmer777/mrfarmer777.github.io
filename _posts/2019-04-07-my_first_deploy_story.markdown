---
layout: post
title: "My First Deploy Story"
sub-title: "Getting Mustang Match off the ground"
header-img: ""
---

### Let's Actually DO something!
As a self-taught coder for the first four years, I created lots of little one-off projects, mainly in my role as a teacher. I made lots of small tutorials, examples, boilerplates and challenges for students in my classes and clubs to explore. In this teaching role, I had plenty of experience planning, building, iterating, and refining these projects with and for students. However I never really had to take any one of them that final 40 ft and actually deploy them to the masses; for use by people beyond those that helped build it. 


### The Project: Mustang Match On Rails!
The student programming club I sponsor has, nearly every year, put out some iteration of this project for use by our school. The goals of this project are simple:

1. Students can take a brief, fun survey about themselves and their interests
2. Students get matched with other students based on these answers
3. Everyone has fun, learns about our club, and finds that coding is cool!

I love the possibilities of walking through this project with students every year! It is relatively straight-forward for students to understand on its face. But brings up several tricky little problems (How do we handle alternative gender preferences or non-binary genders?) We can employ simple algorithms to match students or get really fancy based upon the students involved. Best of all: the project isn't too stereotypical; even students with no interest in coding have _plenty_ of thoughts on the questions that should be asked and how students should be matched. 

This year, I wanted to finally incorporate some form of security to this project. I also wanted students to have more control over the entire project from back to front given the diverse set of skills we had. I decided that we should use Rails to build this project (instead of the dubious stack we used in the past - Google Sheets shared as json and vanilla js, yikes.)


### Getting Ready to Deploy
After several weekly meetings dedicated to perfecting the questions and the algorithm, I setup the bones of the backend of the application. We worked on the questions and the front end some more and after quite a few weeks of work, we had something that was ready! We had the primary functionality ready to go. When a new user took the survey, they would instantly be "matched" with all of their potential matches (based on gender and gender preferences) and their matches could then be reported in order of their "match score" right away. Cool!

We deployed to Heroku using free-tier dynos and a basic postgresql database server. We were finally ready to let this out to the masses - and this time things would be secure!


### Things were good! A little too good...
In the first day of the project, we had over 150 students (out of our school of 2000) take the survey. We had the school buzzing. People were sharing pictures of their "match" page and having the fun we had hoped for. However, it was about 10:30 am when I got several emails from Heroku informing me that our app was using more resources than were allowed. **Down time was imminent!**

My first thought was too much traffic headed to the basic dynos, but we had done some stress testing on them in advance and had no issues. It turns out that it was our own fault!

Our database server was using nearly its entire capacity of 5000 rows already! At first I though, that can't be right. We've only had 150 students take the survey even if they all match favorably with each other that can't be that many rows.

Then I did math.

Assume we've only got two genders. Assume 75 girls and 75 boys take the survey. This means each of those 75 girls will be matched with 75 boys. That gives us:

``` ruby
75*75 
>>> 5625 matches!!!
```

Zoinks. But it gets worse. Our matches are not mutual because (as I was repeatedly told) your best match may not think you are _their_ best match. So we had to match all 75 boys with those 75 girls separately. So the real total was:

```
(75*75)*2
>>>12250 matches!!
```

"Down time imminent" indeed. Between teaching my classes I consulted with the leadership of the club to come up with ways to mitigate the problem. We were also taking on new users by the minute. We were up over 300 users (and 60000 matches) before we came up with a solution


### Calm Down, Matchmaker
At the outset, we were only hoping to report the top 20 matches for every student. however, some more quick math revealed that we'd still be over our quota at this rate. We decided to just report the top 5 matches for each student. This enabled us to handle up to 1000-ish students within the limits of our free-tier server. Though this isn't what we wanted, it's what we had money for! 


### Hard-fought Win
After all was said and done, we had 765 of our students participate in the survey and visit the app several times over the 2 week period it was open. We had a lot of fun talking about what worked and what we'll do better next year. We got a few students introduced to Rails as well as HTML/CSS. It was a net win for the club that we can pick up next year. 

I learned what truly deploying a project can be like. It was stressful, but really rewarding! It was fun to try and solve the problems that popped up and work together to do so. It also felt good to know that the project helped students learn and engage in conversations about CS. Lastly, it made me appreciate the level of care and planning that go into launching larger-scale services and projects as well.