---
layout: post
title:      "Doula Mynder (Part 2)"
date:       2018-07-07 20:56:46 +0000
permalink:  doula_mynder_part_2
---


## Early Wins
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


