---
layout: post
title:      "ActiveRecord's Scandalous Amount of Relationships!"
date:       2019-03-21 01:05:56 -0400
permalink:  activerecords_scandoulous_amount_of_relationships
---


<center><iframe src="https://giphy.com/embed/3lgGnnTQFi05a" width="480" height="300" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/mrw-girl-bar-3lgGnnTQFi05a"></a></p></center>


ActiveRecord is not loyal. *Sigh.....*


ActiveRecord has a plethora of relationships and doesn't care about morals. It instead uses them how it pleases without a care in the world about how any of the other relationships may feel.

This may not be desirable in the dating world, but it sure is when it comes to Rails! It gives us incredible correlations of data that would otherwise be independent of each other and a true nightmare to figure out.

Here's a quick overview of the ActiveRecord Relationships/Associations and how they can be extremely effective in simplifying code, logic, and databases.

ActiveRecord is a design pattern that provides us with Object Relational Mapping (ORM) and gives us access to the relationships between databases by querying. Because of 'Rails Magic', ActiveRecord gives us easy methods to call upon to correspond certain objects and data to yield certain results.

To start off with, ActiveRecord first has associations/relations with our models. For example, my childhood dogs would have associations of:

```
Class Dog < ApplicationRecord

  belongs_to :owner

end
```

Both Casper & Kody *belong* to me (yes, just 10 year-old me, no one else!)

```
Class Owner < ApplicationRecord

  has_many :dogs

end
```


And I don't just have one dog, but two cuties, so I have many dogs!


Of course, this is some of the simplest associations but we can also have more complication associations such as a many-to-many relationship. All in all, these associations allow ActiveRecord to understand how our data relates and if a join table needs to be created.

Once the models are set up and we have established our relationships, we then have access to a handful of ActiveRecord methods, such as:

`#new/#build, #create, #find, #any, #empty?, #delete_all, #find_or_create_by, #inspect, #reload, #size` and many more.


In my recent Rails Portfolio project, I commonyl used ` #new, #find & #empty?`

Here are snippets of my code from my [Goal Tracking Application](https://github.com/cbronzo/goal-tracker) as examples:

```
class PostsController < ApplicationController

  def create
    @post = current_user.posts.new post_params
        if @post.save
          redirect_to user_post_path(current_user.id, @post.id), notice: 'Post created'
        else
          flash[:alert] = @post.errors.full_messages.join("; ")
          render :new
       end
  end
		
end
```

In this method #create, I use ```current_user.posts.new post_params``` where #new initializes a new record with the correct relation. In this case, I needed to instantiate a new Posts record that was set to the current user. The post_params were passed through so it new what attributes this new post record would need. The #new method really leverages ActiveRecord's magic by creating a new record, with an id, and with a post_id that corresponds to the user_id!  From this, we can tell which user created each post and which post belongs to what user. Thanks, #new!



```
class CheersController < ApplicationController


    def index
       if !params[:post_id]
         @cheer = Cheer.all.count
       else
         @post = Post.find_by_id(params[:post_id])
         @cheer = @post.cheers.count
      end
	 end
	 
end
```
	
In this method for #index I use ```Post.find_by_id()``` This method gave me the ability to quickly query my Posts database and search for a certain id, and in this case,In my show view, I have ```@post.category.empty?``` and the #empty? the method gives me the ability to check and see if the category record in the Posts Table is empty or not for a particular post. In this case, I used this to see if a user gave a post a category, and if they did not I did not want the category headline to show in their post. Having the ActiveRecord method truly makes this a super simple process by checking the correct data table and specific record to find if there is data or not. 



So ActiveRecord may not always be in a monogamous relationship, but in this case, it's totally acceptable, and in fact, it's APPRECIATED! So thanks for doing all the heavy lifting ActiveRecord and taking any heat for us! ;) that id would be the ```params[:post_id]```. So it went into that database and scanned for the post_id that would be passed in, allowing me to have access to a post and it's associated id at any moment. Super helpful!



Posts/Show.html.erb

  ```
<% if !@post.category.empty? %>
  Category
  <%= @post.category %><br><br>
<% end %>
	```

In my show view, I have ```@post.category.empty?``` and the #empty? method gives me the ability to check and see if the category record in the Posts Table is empty or not for a particular post. In this case I used this to see if a user gave a post a category, and if they did not I did not want the category headline to show in their post. Having the ActiveRecord method truly makes this a super simple process by checking the correct datatable and specific record to find if there is data or not. 



So ActiveRecord may not always be in a monogomous relationship, but in this case it's totally accpetable, and in fact, its APPRECIATED! So thanks for doing all the heavy lifting ActiveRecord and taking any heat for us! ;)




