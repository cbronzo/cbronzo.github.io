---
layout: post
title:      "Using Sinatra to Create a Travel Tracker!"
date:       2018-09-06 12:27:07 -0400
permalink:  my_sinatra_mvc_application
---

The past few weeks (yes, weeks!) I have been working on my second portfolio project using Sinatra, the MVC model, CRUD (create, read, update, delete) & active record. I created an application that allows users to track the number of states they have traveled to in the US. This was the first application I have made where a database stores information that a user inputs -- such as their login information, their user id, the states they have been to and even their favorite memory of that state. Not only does the database store the information, but the information captured can be updated/edited, and deleted. There also had to be user authentication and validation and for logins. A lot of working parts had to come together to make this possible!

![State Tracker Home Page](https://i.imgur.com/0ysjqZZ.png)


While building this, each piece of my code related to another piece of my code -- which was definitely confusing at times, but all the more exciting when it all came together like a true jigsaw puzzle. The MVC architecture separated my logic into three separate parts: models, views & controllers. My models contained my classes `User`, `State` and `UserState`, and this is where I created the relationship of `has_many` since users can visit/have many states, and a state can have many users who have visited it. 

User Model:

```
class User < ActiveRecord::Base
  has_many :user_states
  has_many :states, through: :user_states


  validates :email, uniqueness: true
  validates :username, uniqueness: true
  validates :name, presence: true
  validates :password, presence: true

  has_secure_password

  def slug
    username.downcase.gsub(" ","-")
  end

  def self.find_by_slug(slug)
    User.all.find{|user| user.slug == slug}
  end
end

```


In the controller, I had an application controller that had a few helper methods, but the main two controllers were my States Controller and my User Controller. Here, the controllers contain most of the logic -- telling the application what to do with the information given. For example, if a user inputs login information, it will first verify that the login is valid and then it will tell the application which route to go to in order to show a user's profile page. If a user adds a state, it will tell the database to store the record and send the updated data to the states view page. 

Piece of States Controller

```
class StatesController < ApplicationController
  use Rack::Flash

  get '/states' do
    if logged_in?
        @user = current_user
        @states = current_user.states

      erb :'states/states'
    else
      redirect to '/login'
    end
  end


  get '/states/new' do
    if logged_in?
      erb :'/states/create'
    else
      redirect to '/login'
    end
  end

  post '/states' do
      if logged_in? && params[:state_name] != ""
          params[:state_name].each do |state_name|
          @state = State.find_or_create_by(:state_name => state_name)
            if @state.users.include?(current_user)
              flash[:message] = "You have already added this state."
            else
              @state.users << current_user
            end
          end
          redirect to "states/states"
      else
        redirect to "/states/new"
      end
  end
```


The view pages are what the user sees on the browser, it simply displays the data.

Example of Show State View:

```
<p><h2><%= @state.state_name %></h2></p>

  <% if @state.users.count != 0 %>
        <%if @state.users.count == 1%>
      <h5>Only <%= @state.users.count %> traveler has visited <%= @state.state_name %>! That must be you!</h5>
          <% else %>
          <h5>Wow! <%= @state.users.count %> travelers have visited <%= @state.state_name %>!</h5>
          <i><h6>That's <%= ((@state.users.count.to_f / User.all.count)*100).round %>% of all of our travelers!</h6></i>

        <%  end %>
  <% else %>
    <h5>No travelers have visited yet.</h5>

  <% end %>
```


Active record "*maps database tables to Ruby classes*" (Flatiron), which made it very simple to add records and manipulate them. When a user is created, the database `Users` adds a new row for the record containing an id, username, password, name & email. 

![User Table](https://i.imgur.com/Wi4KB1W.png)


In another table called `States`, an id and a state name are recorded. 

![State Table](https://imgur.com/N9CY8oZ.png)

This is great, but how do we connect the two so they relate in the many-to-many relationship?

Enter the** join table**. The join table does exactly as it sounds: it joins the tables together so they can be associated. This table is called `UserStates`, and a new class of `UserState` is created which `belongs_to` a user and a state. In the join table,  there is an id, the `user_id` and the `state_id` (and also a memory column for users to input a favorite memory of a particular state). This allows the database to track and associate certain states with users and vice versa. If a user with a `user_id` of 5 chooses ten different states, there will be ten rows added to the database - all with the `user_id` of 5, but each with a different `state_id`. It goes the other way too if California has a `state_id` of 7, and 45 users have visited, there will be 45 rows all with the `state_id` of 7, and each with a  different `user_id`. 


![Join Table](https://i.imgur.com/s7YRPNb.png)

The join table is truly where the magic happens and what allows the site to properly display the correct information. The models, views, and controllers separate the logic but all come together to create an application that is able to properly record, associate and then display the data in a working site. This project took me much longer than I expected but learning the basics of how active record works has been transformative. I understand its benefits and uses and see a whole world of opportunity going forward! Now on to Rails!

