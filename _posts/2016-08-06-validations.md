---
layout: post
title: Post.new.valid?
---

Today I would like to share with you, my next step in Rails journey. Which is as you can tell from title validations. To be specific how to make rails app to check if a newly created object is valid in the context of its model and how validation works under the hood.

I was asked to create a simple application with only just one model, controller. Then add to it some validations. I chose to make simple "blog" app with only Post model. There was also one more restriction that [Andrzej Krzywda](https://twitter.com/andrzejkrzywda) gave me. No scaffolds and generators except for migration generator. That meant that I had to create a model, controller and all view files from scratch for all basic actions. As it turned out it was a great way to learn how it all works. In the end, I figured out that I could also create migrations without using a generator. From now on all my apps during that course will be created that way.

### Model
I decided to split the whole process into more manageable chunks and the first one was easy. Create model with some simple validations. At the beginning, I decided what fields should Post have and how they should be validated. So, a blog post should have at least title, which I thought should not only be present - it doesn't make sense to create posts without a title. Right? But also should have length constraints. I thought that way too short title doesn't make any sense and too long, on the other hand, make it hard to understand what is the post about. Next thing that is even more important is actual content of it. What's the point of creating empty posts? And the last thing which I thought should be optional was an author.

```language-ruby
class Post < ActiveRecord::Base
  validates :title, presence: true, length: { in: 2..50 }
  validates :content, presence: true
end

```

Code for that model is very simple, but the purpose of that task was to learn how validation works under the hood and not to make the most bulletproof model. Ok, the first thing I did learn was that each model inherits from ActiveRecord::Base class. That class includes multiple modules and one of them is the module responsible for validations. One of the methods in that module is validation, which in very simple words runs all validations for given fields and checks if any errors were found. Since I am at the beginning of my learning process that was all that I should know for now.

### Controller
```language-ruby
def create
  @post  = Post.new(post_params)
  if @post.save
    redirect_to @post
  else
    render 'new'
  end
end
```

Next thing was to create controller that was responsible for all CRUD operations on my posts. From a validation perspective, the most important part was code that you could find in create and update methods. To be even more specific #save and #update_attributes methods. Which in very simple words works like this. First, it checks if an object that called it is valid and later if that's true create SQL query that inserts it to database. If the object is not valid it rerenders proper template with listed errors and marked invalid fields.

### View
```language-html
<% if @post.errors.any? %>
  <div id="error_explanation">
    <h2><%= pluralize( @post.errors.count, "error") %> need to be fixed before saving that post.</h2>
    <ul>
      <% @post.errors.full_messages.each do |message| %>
        <li><%= message %></li>
      <% end %>
    </ul>
  </div>
<% end %>
```

Ok, just last part that needed to be done. Actual views with a proper error displaying. To make that happened first we need to check if our object has any errors using @object.erros.any? and later thanks to cool method pluralize return number of errors with some fancy text using. Last part is to iterate over the collection of all error messages for our object and display them in a list.

You can find finished app on [Heroku](https://damp-anchorage-94507.herokuapp.com/posts) and [Github](https://github.com/LukeP91/validation_app).

{% include twitter_plug.html %}
