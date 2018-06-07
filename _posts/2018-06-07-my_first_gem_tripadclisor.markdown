---
layout: post
title:      "My First Gem: TripadCLIsor"
date:       2018-06-07 06:24:13 +0000
permalink:  my_first_gem_tripadclisor
---


As the culmination of my study of Object-Oriented Ruby, I decided to build a web scraper for TripAdvisor.  I choose TripAdvisor primarily because it contains a wealth of information in a variety of levels. Each destination has numerous hotels, each hotel has numerous prices, etc. Since the primary goal of the project was to implement an object-oriented structure, it seemed like fertile ground for the process. I also wanted to create something that would be helpful to me and my wife as we plan our families vacations. TripAdvisor is a staple for our trip-planning process, once we know where we're going. However, I always need some inspiration when I am looking for a place to go. TripadCLIsor takes the aspects of TripAdvisor that help you explore potential destinations and brings them into an easily searchable CLI.

### Project Description
A user of the gem is presented with a choice of searching by "Theme" or by "Selected Destinations". Themes are scraped from the TripAdvisor Inspiration page and include things like "Top Romantic Getaways" or "World's Best Beaches." From this menu will be taken to a list of themes or randomly-chosen destinations, depending on their choice. Once the user chooses a destination, they are presented with a list of the destination's hotels and the best available price for them (subject to availability). Finally, they can select the hotel about which they'd like more information. The program will take them to a view where they can see a more detailed description including other offerred prices, themes into which the hotel fits, and other helpful info.


#### Basic Project Architecture
This application is comprised of 4 classes:
1. Scraper class: responsible for scraping the HTML pages via the Nokogiri and open-uri gems. This class also processes the scraped information and delivers it to the other object classes as a hash for instantiation.
2. Theme, Destination, and Hotel classes hold the scraped information by the Scraper. Themes have multiple destinations each of which have multiple hotels. Conversely, destinations can have multiple themes.

The program begins by scraping an "Inspiration" page from the source. This page contains multiple travel Themes, which are instantiated as Theme objects that include a `.page_url` instance variable. This is used to scrape theme pages for destinations which are used to instantiate Destination objects. The two are linked. 

Here's the primary scraping function

```
  #Scrapes Inspiration page for Themes and then scrapes Theme pages for Desintations
  def load_inspiration_themes
    load_start=Time.now
    puts "Loading destinations and themes..."

  
    @scraper.populate_themes                  #instantiating all Theme objects

  
    Theme.all.each do |theme|
      @scraper.populate_destinations(theme)   #instantiating all Destination objects, connecting them with themes
    end
    load_end=Time.now

    #Displaying statistics about the results of the scrape
    puts "Loaded info about #{Destination.all.length} destinations in #{load_end-load_start} seconds."
    puts "HTML Calls: #{scraper.call_count}"
    sleep(1)
  end
```

Later, when a user chooses a destination through one of two views/sorting options, the Destination page is then scraped for Hotels, providing a third level of depth for the user. They can then see 'drilled down' details from each hotel. Along the way, the user interacts with the program through a simple numerical input. Users can move between 4 different views and the program can handle invalid inputs without crashing too!

### The Process
##### Planning the Program
After exploring some alternative sources for scraping information, I decided on TripAdvisor for the reasons mentioned above. I sketched out the basic menus that I wanted to create for the user and how they might move between them. I then dove deeper into TripAdvisor to see what information and sorting was available. Frustratingly, TripAdvisor kept learning about what I would click on and lost it's neutral, impartial destinations. (After I clicked on Punta Cana as a destination, all other non-tropical destinations were gone from view when the page was reloaded.) I eventually found the [Inspiration] (https://www.tripadvisor.com/Inspiration) page that remained relatively static regardless of user data. This gave me an added layer of depth, which was a surprise. 

##### Starting to Build
The first few steps went very quickly! Thanks to great tutorial videos and documentation on [RubyGems.org](https://rubygems.org), my environment was setup very quickly and the steps I had learned in the previous coding challenges could be easily applied here. I started by building the CLI class and thinking about the flow of the program. I found that I tended to build the program from the user perspective. I built the view and associated classes first, and moved on from there. I had some previous experience with Object-oriented development, so relating the classes and building them out wasn't too difficult. 

##### Approaching the MVP
Once I had the program running reliably, I tried to add in more and more little bells and whistles. I tried to scrape more data from the pages. I toyed with making a `Menu` class (since I used so many of them!), and I tried create a fully-published Gem. However, I started to feel myself get overwhelmed with the possibilities and perfectionism began to set in. I decided to focus on what was working the way I want and tried to streamline what I had created as best I could. I am happy with the project I built and feel that I understand every aspect of it thoroughly. 


##### Reflecting on the Process
This was my first project in Ruby and my first attempt at a CLI as well. In retrospect, I would spend more time in the planning phase try to anticipate stumbling blocks to my final product. Often, the toughest bugs or hurdles were the ones that resulted from an unclear plan. Secondly, syntatical quirks were *really* easy to move past. I incorporated about 8-10 different methods or blocks that I had never used or heard of before this project. With a simple and pointed Google search, I found methods like `.detect` and `.each_with_index` which were invaluable to the project! 

One great learning experience from this project was getting comfortable with passing object instances around between functions. For example, I almost found myself making several copies of the same `Destination` object rather than adding the very same instance to multiple different locations. I understood this by thinking about borrowing a book from a friend - we don't each get our own copy! We simply have the same book, but in different ways (The book might be in my 'borrowed' array, while it is in my friends 'lent' array, for example)


##### Next Steps
There are many things I would love to add to this project, if given the time. For example, when scraping TripAdvisor, some of the data does not load fast enough to be scraped when called by `open-uri`. This caused the "Best Price" data to be inconsistent. Additionally, I would like to publish my gem to [RubyGems](https://rubygems.org) as a full-functional gem. I am close! But there are still steps to take. I know this project has taught me several specific things about Ruby as a language and about the process of building a fully-fledged project. But most importantly I learned I must keep going and focus on the main goals of the project. I will have to battle that perfectionism back in order to feel like this will get done. 

Enjoy!

[TripadCLIsor Gihub Repo](https://github.com/mrfarmer777/cli-tripadclisor)




