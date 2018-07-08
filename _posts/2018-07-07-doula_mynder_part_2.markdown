---
layout: post
title:      "Doula Mynder (Part 2)"
date:       2018-07-07 16:56:47 -0400
permalink:  doula_mynder_part_2
---


## Early Wins

### Starting from Scratch
The hardest thing to face as a developer is a blank project. This is especially true if you don't feel confident about setting up all of the project requirements from scratch. When it comes to practice labs, much of the work is already done for you. The `gemfile` is made, `config.ru` is all set, and things are generally configured and just waiting for you to put your code in. 

For this project, I decided to challenge myselt to set up the entire environment from scratch, from memory. It was an exercise that forced me to not just remember what I needed, but why I needed it in the first place. I started out with the gemfile and tried to remember all of the gems that I would need to build my app.  I also tried to include little notes to myself about what each gem was providing for me. I found this to be really helpful.

``` ruby
source "http://www.rubygems.org"

gem 'sinatra'             #for handling of routing and server actions
gem 'activerecord', '4.2.7.1', :require => 'active_record' #for ORM
gem 'sinatra-activerecord', :require => 'sinatra/activerecord' #enabling AR to interact with Sinatra
gem 'pry'                 #for debugging
gem 'rake'                #for task automation
gem 'tux'                 #for ORM debugging
gem 'require_all'         #for environment.rb requiring
gem 'sqlite3'             #for database management
gem 'thin'                #basic server for shotgun  !forgot this one initially
gem 'shotgun'             #for live view of running app
gem 'bcrypt'              #for password encryption
gem 'rack-flash3'         #for flash messages   !couldn't remember the exact name, had to look it up.
```

After the gemfile, I stubbed out the app folders and building the `config.ru` file. I am proud to say that the only thing I initially forgot was the `use Rack::MethodOverride` I would need for my delete and patch requests later. Finally, I setup a simple route action to see if it was working and...it wasn't. Turns out I hadn't required the `environment.rb` file properly. But fixing that did the trick.

All in all, I found the exercise to be really helpful. Though it took longer,  I feel like it helped me understand each piece of the environment and why it was necessary. Yes, I did have to consult other labs and online resources. But I now have a much deeper understanding of the requirements of my coding environment.

## TDD Implementation
A personal goal for this project was also to implement Test-Driven Design. I learned from my CLI project that spending a few extra minutes creating robust tests would help speed things up in the long run. However, I was intimidated by getting both my app and the testing suite up and running. However, I employed the same technique I described above. I tried to write as much of the testing code as I could from memory. Only after I had exhausted what I knew did I consult other code. With just a little bit of help I was able to get a small set of tests written, failing, and eventually passing reliably. Here's an example test:

```
describe "New Client form" do
it "does not create a new client if name is left blank" do
      user=User.create(username:'sallyride',name:"Sally Ride",company_name:"Rocket Doulas",password:"password")
      visit "/login"
      fill_in(:username, :with =>"sallyride")
      fill_in(:password, :with =>"password")
      click_button "submit"

      og_client_count=user.clients.count

      visit '/clients/new'
      fill_in(:client_name, :with=>"")
      fill_in(:client_age, :with=>34)
      fill_in(:client_partner_name, :with=>"Chanandelor Bong")
      fill_in(:client_address, :with=>"123 W. Elm St., Montana, NB")
      fill_in(:client_num_children, :with=>5)
      click_button "submit"
      expect(page.body).to include("New clients must have a name")
      expect(user.clients.count).to eq(og_client_count)
    end
```

Unfortunately, I did abandon writing tests after getting my objects and basic CRUD actions up and running. I found that I was spending more time researching how to _test_ the action that I was actually building it. It created the illusion that I was building two projects at once: the test suite and the project itself. However, I do see the immense value of building these tests in advance of the project. It encourages you to begin with the end in mind. Now that I know more about Rspec and how to build tests quickly, my return on time investment in building will be even better and I'll be able to build a more robust series of tests.



## The MVP

I decided to tackle building my project as follows:

1. Setup User objects a including password encryption with bcrypt.
2. Setup Client object actions and properly relate them to Users.
3. Handle form validations
3. Create Dashboard view 
4. Implement Bootstrap for responsive styling


The essential feature of the app was the 'dashboard' - a view in which a doula could see all of her clients details at a glance. She could get an idea of where everyone was in their pregnancy so that she could send out pertinent updates and check-ins. She could also manage new information that came from her clients. Once the object relationships were set up, this view could be easily created. The idea of this view had actually helped inspire me to do this project and I felt it was a critical part of the minimum viable product. First things first.

#### 1 and 2.  Setting up Objects and Relationships

Setting up the Users and relating them to Clients went really well; only minor hangups. I found that drawing out each table (or mocking it up in a spreadsheet) really helped me visualize not only the migrations, but the forms I would need to accept the data. Once I had those set up, I felt like I knew exactly what to build. Implementing bcrypt was a little scary at first, since it feels kind of black-boxy for me right now. However, the methods I needed to properly  encrypt passwords for me users were clear and easy to implement.

#### 3. Validations

Implementing field validations was surprisingly time-consuming! I resigned myself to relying on Chrome and Firefox implementation of rendering HTML5. So, it was easy to implement front end field requirements like so: 

```html
<input class="form-control" type="text" name="client[name]" id="client_name" value="<%=@client.name%>" required/>
```

However, one of my crafty high-schoolers showed me how to easily circumvent this protection. So, while I felt this was sufficient for many of the fields, the mission-critical (or database critical) fields like username and password had a redundant protection in the application controller as well.

```ruby
post "/signup" do
		if !params[:username].empty? && !params[:password].empty?
      @user=User.create(params)
			...
      
```

It wasn't until much later in the process that I realized that User's `username`s must be unique in order to maintain database integrity as well. Some quick searches through ActiveRecord's documentation helped me implement this with ease! 

```ruby
class User < ActiveRecord::Base
  validates_uniqueness_of :username
  has_secure_password
  has_many :clients
end
```

the `validates_uniqueness_of` macro prevents new objects from being created without a unique username. The macro doesn't solve all the problems, but for my purposes it was more than enough. Then, by checking if a User isntance was given an id, I could create logic to handle duplicate usernames as pass messages back to the user using Flash messages. 

```ruby
if @user.id.nil?
        flash[:message]="Sorry, that username is already in use. Please choose a different username."
        redirect "/signup"
      else
```

Though my form validations are not air-tight, I feel that they will ensure the integrity of the database from really damaging data being submitted.

#### 4. The Dashboard

This view took the most time to create, but it was a critical part of the app. The view is essentially an index view of the client objects that belong to the user. In order to create this view I had to address the following problems:

1. Selecting only those clients created by and belonging to the active user (no problem thanks to ActiveRecord.)
2. Pulling the requisite information to the view using ERB. 
3. Working with `DateTime` objects to display how many weeks along each client was.
4. Implementing Bootstrap 4 to create the vision I had in mind.

I only ran into a little bit of trouble working with DateTime objects. Since client progress is typically measured in weeks (an unusual measurement to make) and since some pregnancies can stretch over a new calendar year, I found myself using quite a bit of code to calculate a client's current progres. Initially I did this using ERB in the view itself. Then I remembered I can use **helper methods** and this is a perfect candidate.  The method below takes in a client and returns how many weeks along they are with a one-line calculation. Since 40 weeks of gestation is typically what is used to calculate a due date, my wife suggested I use that to calculate progress. 

```
def get_progress(client)
      if !client.due_date.nil?
        progress=40-(client.due_date.cweek-DateTime.now.cweek+(52*(client.due_date.year-DateTime.now.year)))
      else
        progress=0
      end
      progress
    end
```


Lastly, working with Bootstrap 4 was fun, but took quite a bit of time! Starting with a vision in mind made it difficult to accept limitations of the framework (or of my own experience with it.) However, using lots of research and a few little hacks, I was able to make the dashboard look much like I had intended. Most notably,  I was suprised how easy it was to create the little progress bars! Bootstrap for has a class for everything! 

![](https://i.imgur.com/5cE2lh0.png)

I really enjoyed making this project and I look forward to adding to it in the near future!


