---
layout: post
title:      "Building My First CLI Application from Scratch!"
date:       2018-07-18 20:40:27 -0400
permalink:  building_my_first_cli_application_from_scratch
---

This week, I built my own custom Command Line Interface (CLI) Application that pulled data from an external website and I coded it with Ruby by using Object Orientation! I felt very fairly confident and was thinking I could finish it in just a few hours -- but  I grossly underestimated how long it would take me for my first time! I even created a local repository when I didn't need to...apparentley we haven't been taught that just yet. No wonder it took me so long....


The building of it wasn't necessarily the hard part, the de-bugging was! I would fix one thing, and it would break another....or I would get so close to completion and then realize I need to refactor an entire set of code, and by doing that it would affect other related pieces. Sometimes I was missing something as simple as a comma and would spend forever grooming through lines of code, only for it to be such a simple fix. Haha, I learned so much by building this CLI but mostly I learned about the actual process of coding, and how much more of a mental game it truly is.


**What does my CLI do?**

I scraped the data from "[Orange County's 21 Hottest Restaurants, Spring 2018](https://la.eater.com/maps/orange-county-restaurants-santa-ana-irvine-dana-point-newport-beach-costa-mesa)" on Eater LA's site. The page lists the restaurants as well as the details of the said restaurant, and so I wanted my CLI to give a user the list of these restarurants, and then allow them to choose which ones they would like to learn more about.

When the gem is run, it first outputs:

```
Welcome to 'Orange County's 21 Hottest Restaurants' Guide! 
Type 'guide' to see the list of OC Restaurants.
Otherwise, type 'exit'.
```

Then a user can either type 'guide' or 'exit.' If they type guide, they'll see:

```
YUM! Here are 21 of the OC's best & hottest restaurants! Dig in!
Please select a restaurant by entering its number (1-21) to learn more!
To exit, type 'exit'.
1. Portos Bakery & Cafe
2. Pour Vida
3. Garlic and Chives
4. Burritos La Palma - Santa Ana
5. Taco Maria
6. Vaca
7. Mesa
8. Blackmarket Bakery
9. Oo Toro Sushi
10. Bluegold & LSXO
11. SeaSalt Woodfire Grill
12. Puesto
13. SOCIAL
14. Lido Bottle Works
15. Pizzeria Sapori
16. The Royal Hen
17. Ironwood
18. Tackle Box Local Grub Shack
19. Marche Moderne
20. Hendrix Restaurant and Bar
21. Bourbon Steak Orange County
```

They then have the option to select a restaurant by number, or exit. And if they type an invalid response here, or anywhere in the CLI, they will get an error message to try again.

Say, a user enters '15' for Pizzeria Sapori (which I can see from my apartment - woohoo), this is what they would see:

```
Below, you will find the details for Pizzeria Sapori.
Description: The old-school Italian destination Sapori got a nice boost with the opening of Pizzeria Sapori next door. The attached casual offshoot features blistery pies and plenty to drink, making it a draw for marina types and locals looking to relax while still dining well.
Address: 1080 Bayside Dr Newport Beach, CA 92660
Phone Number: (949) 644-4220
Website: http://www.saporinb.com/
To select another restaurant, please enter its number, or enter 'back' to go back to the guide.
To exit, type 'exit'.
```

Some of the restaurant details were not listed in the Eater LA article, so in that case, I had to make a conditional where if nothing was provided, it would put "Sorry, no __  available."

From here, they can just keep entering numbers to see other restaurants' info or they can hit 'back' or 'exit.' 

It may seem simple, and rudimentary but a lot of logic (and time) went into creating this.

**The Code:**

Without getting into all the specifics, the main files I had to code were my Scraper, CLI & Restaurant files. Of course, I had other files that created my environment, or executed my CLI and so on...but the meat of the project was in the other three files.

I started off with my Scraper file by creating a Scraper Class. I then started the long journey of learning how to scrape from Eater LA's site and extract *only* the data I needed. The end result of the code looks like this:

```
class Scraper

  def scrape_restaurants
    doc = Nokogiri::HTML(open("https://la.eater.com/maps/orange-county-restaurants-santa-ana-irvine-dana-point-newport-beach-costa-mesa"))
    array = []
      doc.css('section.c-mapstack__card').each do |restaurant|
        header = restaurant.css('.c-mapstack__card-hed')
          if header.size != 0
            oc_hash = {}
            oc_hash[:name] = header.css('h1').text.strip.gsub(/\d+. /,"")
            oc_hash[:description] = restaurant.css('.c-entry-content p').text
            oc_hash[:phone] = restaurant.css('.desktop-only').text
            oc_hash[:address] = restaurant.css('.c-mapstack__address').text
              website_link = restaurant.css('.c-mapstack__phone-url > a')
              if !website_link.empty?
                oc_hash[:website] = website_link.attr("href").value
              else
                oc_hash[:website] = ""
              end
            array << oc_hash
          end
        end
      Best_Restaurant.create_from_collection(array)
  end
end
```

I used Nokogiri for the first time, which is a gem for web scraping, and then I iterated over "section.c-mapstack__card" since it contained all of the elements I needed. I created a hash to hold the data that would be scraped for each of the restaurant's attributes. This hash was then shoveled into an array, which would be passed into another method (Best_Restuarant.create_from_collection(array)) that I created in the Restaurant file.

In the Restaurant file, I used my object orientation knowledge to code the following. I created getters and setters by using attr_accessor, and then I made a class variable of @@all save the restaurant objects. For every new instance of a restaurant, it would initialize a hash, which was created by using mass assignment. Then I used four class methods using 'self' to refer to the Best_Restaurant class. The last method 'def self.create_from_collection(array)' uses all the logic from the other class methods. It iterates over the array and does this for each element: self.new(hash), save and self.create(hash). The code in here looks the simplest, but it's actually the most abstract. I find object orientation to be a bit confusing, but this project helped drive that knowledge home.

```
class Best_Restaurant

attr_accessor :phone, :name, :address, :description, :website

  @@all = []

  def initialize(hash)
    hash.each {|key, value| self.send(("#{key}="), value)}
  end

  def self.all
    @@all
  end

  def save
    @@all << self
  end

  def self.create(hash)
    restaurant = self.new(hash)
    restaurant.save
    restaurant
  end

  def self.create_from_collection(array)
    array.each do |hash|
      self.create(hash)
    end
  end

end
```


Finally, I worked on my CLI class which heavily relied on the code in the other two files. For the most part, it recieves data from the other classes and then uses lots of conditionals for potential inputs from the user. The meat of this code is actually strings of plain english that the user will see.

Here's what it looks like:

```
class CLI
  attr_accessor :name

  def initailize(name)
    @name = name
  end

  def start
    puts "Welcome to 'Orange County's 21 Hottest Restaurants' Guide! "
    Scraper.new.scrape_restaurants
    menu
  end

  def menu
    puts "Type 'guide' to see the list of OC Restaurants."
    puts "Otherwise, type 'exit'."
    input = gets.strip
      if input == "guide"
        list_restaurant_name
      elsif input == "exit"
        puts "You are now exiting the program. Thank you, goodbye!"
        exit
      else
        error
        menu
      end
  end

  def list_restaurant_name
    puts "YUM! Here are 21 of the OC's best & hottest restaurants! Dig in!"
    puts "Please select a restaurant by entering its number (1-21) to learn more!"
    puts "To exit, type 'exit'."
    Best_Restaurant.all.each.with_index(1) do |restaurant, index|
      puts "#{index}. #{restaurant.name}"
    end
    input = gets.strip
      if input.to_i.between?(1,21)

        index = input.to_i - 1
        restaurant_attributes(index)
      elsif input == "exit"
        puts "You are now exiting the program. Thank you, goodbye!"
        exit
      else
        error
        list_restaurant_name
      end
  end

  def restaurant_attributes(index)
    restaurant_info = Best_Restaurant.all[index]
    puts "Below, you will find the details for #{restaurant_info.name}."
    puts "Description: #{restaurant_info.description}"

        if restaurant_info.address == ""
          puts "Address: Sorry, no address available."
        else
          puts "Address: #{restaurant_info.address}"
        end

        if restaurant_info.phone == ""
          puts "Phone: Sorry, no phone number available."
        else
          puts "Phone Number: #{restaurant_info.phone}"
        end

        if restaurant_info.website == ""
          puts "Website: Sorry, no website available."
        else
          puts "Website: #{restaurant_info.website}"
        end

      puts "To select another restaurant, please enter its number, or enter 'back' to go back to the guide."
      puts "To exit, type 'exit'."
      input = gets.strip
        if input == "back"
          list_restaurant_name
        elsif input.to_i.between?(1,21)
          index = input.to_i - 1
          restaurant_attributes(index)
        elsif input == "exit"
          exit
        else
          error
          restaurant_attributes(index)
        end
    end

  def error
    puts "~ERROR, PLEASE TRY AGAIN~"
  end

end
```


I'm sure this code could be written in a much more elegant way, but I'm so proud that I was able to accomplish this, and the lessons I learned will take me to bigger and better projects!
