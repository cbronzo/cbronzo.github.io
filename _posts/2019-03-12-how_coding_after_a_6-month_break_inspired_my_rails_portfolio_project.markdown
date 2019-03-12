---
layout: post
title:      "How Coding After a 4-Month Break Inspired my Rails Portfolio Project"
date:       2019-03-12 02:01:50 -0400
permalink:  how_coding_after_a_6-month_break_inspired_my_rails_portfolio_project
---

I **never** thought I would get to the other side of this project -  it seemed like an insurmountable project. Why you may ask? 


Simple: I took a one week leave of absence. That turned in to two weeks, then three and then well *four* months...

Back in October, I had a family member visiting for a week, so I took some time off since I knew in order to do this project I needed to give it its full attention. But then I started to become overwhelmed every time I thought of starting the project, and so add in some health struggles, a lot of travel (including being detained in China) and some procrastination and well, it quickly snowballed. And the more you fall behind, the hardest it is to start.

I was truly terrified to even log in to Learn.co, as I realized I didn't even know the magnitude of what I didn't know (or forgot) in that time off. But one day in late February, I started and just never stopped. And a week and a half later - I am done! I did it, I have conquered the Rails mountain and I'm ready to tackle the next one.

Throughout this process, I began working on setting goals so I could get myself back on track. I've always wished for a way to publicly announce my goals so others could hold me accountable. So for my Rails project, I created GOAL CHEER, a community-based goal tracking site. Users can set, create and post their goals publicly as well as receive 'cheer' and support from others. The idea is like a reddit for goals.

![GOAL CHEER Homepage](https://imgur.com/a/ZCN7ZmR)

What better way to jump back into coding than by making an application that I actually needed to even start! And the time off truly inspired me to channel my fears & procrastination and transform it into a positive, proactive and tangible application. Once I found this new motivation, I never looked back and I'm happy to announce that I have completed my first goal on GOAL CHEER: "Finish my Rails Project." I may be the only person that cheered myself on but that is also all I needed.






**The Application GOAL CHEER:**



Rails is truly magical, as I wrote in my last blog post, and so it was so wonderful using all of its glory & magic to create routes, relationships, controllers, models, views and so on.

First I had to come up with my models:

Users, Posts, Comments & Cheers

They all have unique relationships with each other, such as a belongs to relationship, a has many relationship and a many-to-many relationship. This was a difficult aspect of the coding because these relationships had to be right so my databases and join tables were set up correctly.




**Models**

```
class Post < ApplicationRecord
  belongs_to :user
  has_many :users, through: :comments
  has_many :comments
  has_many :users, through: :cheers
  has_many :cheers

class User < ApplicationRecord
  has_many :posts
  has_many :comments
  has_many :commented_goals, class_name: 'Post', foreign_key: 'post_id', through: :comments
  has_many :cheers
  has_many :cheered_goals, class_name: 'Post', foreign_key: 'post_id', through: :cheers

class Cheer < ApplicationRecord
  belongs_to :post
  belongs_to :user
  belongs_to :cheerleader, class_name: 'User', foreign_key: 'user_id'
	
	
class Comment < ApplicationRecord
  belongs_to :post
  belongs_to :user
  belongs_to :commenter, class_name: 'User', foreign_key: 'user_id'


```



I also used the 'devise' gem, which made user authentication so incredibly easy, but it also came with a ton of files and code that took a while to understand and use properly.  

But the most important part of my project was correctly setting up my Posts Controller as this controller handled all of the posts, which are the main part of the entire site. 

[Post (Goal) Index Page](https://imgur.com/a/vg8iFpg)


```

class PostsController < ApplicationController
  before_action :authenticate_user!
  before_action :set_post, only: [:show, :edit, :update, :destroy]
  before_action :post_creator, only: [:edit, :update, :destroy]


  def index
    if params[:user_id].present?
      @posts = Post.where(user_id: params[:user_id])
    else
      @posts = Post.all
    end

    if params[:sort_by].present?
      @posts = @posts.send(params[:sort_by].to_sym)
    end


  end

  def new
    @post = Post.new
  end

  def create
    @post = current_user.posts.new post_params
    if @post.save
      redirect_to user_post_path(current_user.id, @post.id), notice: 'Post created'
    else
      flash[:alert] = @post.errors.full_messages.join("; ")
      render :new
    end
  end

  def show
  end

  def edit
  end

  def update
    if @post.update post_params
      redirect_to post_path(@post), notice: "Post Updated"
    else
      render :edit
    end
  end

  def destroy
    if @post.destroy
      redirect_to posts_path, notice: "Post Deleted"
    else
      redirect_to post_path(@post), error: 'Unable to Delete'
    end
  end


  private

  def set_post
    @post = Post.find params[:id]
  end

  def post_creator
    @post = Post.find params[:id]
      if current_user.id == @post.user.id
      else
        redirect_to root_url
      end
  end

  def post_params
    params.require(:post).permit(:title, :description, :due_date, :ongoing, :difficulty, :category, :priority, :progress, :question)
  end


end

```



And the most basic but integral part of this project was setting up my routes. I used quite a bit of nested resources and went a level deeper than I should've, but in doing so, I learned so much. I truly was able to visualize how routes, resources and paths all related. 


```

Routes:
  devise_scope :user do
    get 'login', to: 'devise/sessions#new'
    get 'logout', to: 'devise/sessions#destroy'
  end

  root 'static#welcome'

  resources :users do
    resources :posts do
      resources :comments
    end
  end

  resources :posts do
    resources :comments
  end

  resources :posts do
    resources :cheers

    member do
      get '/cheer' => 'cheers#create', as: 'cheer'
    end
  end
	
```

I really enjoyed learning about how rails generates paths to use. I had to relearn a lot of this, but in doing so - I nailed it! I understand it way more than when I originally learned it.

Thus, this project - which first gave me overwhelming anxiety at how far I had fallen behind now gives me incredible confidence as I was able to reinforce the knowledge I had before and truly learn the aspects I didn't understand initially. This time off actually gave me a fresh start and allowed me the ability to see it from new eyes. 

Don't ever beat yourself up if you are struggling, it may be just what you need.

Sometimes a set back is truly just setting you up so you can hit it out of the park - just remember to set goals and ask for help to get where you want to go! 




