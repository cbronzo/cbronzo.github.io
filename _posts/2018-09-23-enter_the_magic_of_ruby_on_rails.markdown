---
layout: post
title:      "Enter the Magic of Ruby on Rails!"
date:       2018-09-23 23:17:19 +0000
permalink:  enter_the_magic_of_ruby_on_rails
---


<center><iframe src="https://giphy.com/embed/12NUbkX6p4xOO4" width="480" height="440" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/shia-labeouf-12NUbkX6p4xOO4"></a></p></center>

In my last post, I wrote about how I used Sinatra to build a web application that allowed users to track the US states they had been to. The site itself was very simple, but the code behind it took quite a while to figure out and to get working properly. I really enjoyed Sinatra and thought it was the real deal...but then I learned about Rails (and the magic of it), and realized just how naive I was.

Rails blows Sinatra out of the water! If I had used the Rails framework for my Sinatra project, I could have been done in just a few hours instead of days! I'm glad I learned the basics of using Sinatra, but Rails is my new best friend and I love how it simplifies the amount of coding so you can really dive into the features you want to build out.

Rails is truly magical -- at times I'm baffled how it can call just one method and invoke hundreds of lines of code. It is a lifesaver and I'm excited to learn more about its functionally! I'm just about halfway through learning about Rails, but it is so evident that it will change the way, speed and quality of what I code from here on out.

Let's go over some (magical) examples of Rails:
<br>

**Ex 1)** *Generators: *

Rails has many different generators that help build out the core functionally of what you need (aka it will do the heavy lifting for you). 

```
rails generate <name of generator> <options>
```

The one that I expect to use the most is:
```
rails generate resource <options>
```

Say I wanted to generate the files needed for my State Tracker, I would do:

```
rails g resource State name:string 
```

This creates a migration file (with the database table), a model file (that inherits from ActiveRecord::Base) & a controller file (that inherits from the ApplicationController), as well as a view folder and the correct routes needed in the routes.rb file. It also creates a few other files, but the ones mentioned are all ones that I had to manually create beforehand. This saves a large amount of time and energy and allows you to start coding the good stuff sooner.

<br>

**Ex. 2)** *Routing:*

In Sinatra, our routes were defined in our controller file, while in Rails we put our routes in the config file. This makes the routing more readable but also allows us to write the route in a much simpler fashion. If I had wanted to route to my login page on my Sinatra project I would have coded this in my UserController:

```
get '/login' do
    if logged_in?
      redirect to '/states'
    end
    erb :'/users/login'
  end
```


However, in Rails I can just put this in my config file:


`get 'login', to: 'users#login'`

and Rails will know to look in the UsersController for the login method for '/login'. But Rails lets us make it even simpler by allowing me to also say:

`resources :users`

and this will tell Rails to route ALL my methods in my UsersController to their corresponding views/pages. In Sinatra, I would have to write out every single route in their corresponding controller. How annoying!

If I had just wanted it to route to my login, I could have easily put:

```
`resources :users, only:  :login
```


Rails makes this long process take just a couple of seconds. Magic, right!

<br>

**Ex. 3)** *Forms: *

Rails has great helper methods called `form_for`  and `form_tag` to create forms very efficiently.

For instance, if I were creating a form to input information about one's dog using HTML (which is how it is used in Sinatra), it would look like this (watered down):

```
<form action="/new"  method="POST">
  <input type="text" name="dog[name]" id="dog_name" ><br>
  <input type="text" name="dog[breed]" id="dog_breed"><br>
	<input type="text" name="dog[age]" id="dog_age" ><br>
  <input type="submit" value="Submit" />
</form>
```


In Rails, I can use `form_for` and just simply write this instead:

```
<%= form_for @dog do |f| %>
  <%= f.text_field :name %>
  <%= f.text_field :breed %>
	<%= f.text_field :age %>
  <%= f.submit %>
<% end %>
```


It creates the exact same result! It's truly amazing and cuts down on some much time.

<br>

And this is just some of the basics or Rails -- I can't wait to see what other surprises are in store!


