---
categories: ruby
comments: true
date: "2012-09-13T00:00:00Z"
title: Caps in Ruby are Constants
---

Everything in Ruby that starts with a capital letter is a constant, meaning you can't change its value. That means:
```ruby
API_USER
API_PASSWORD 

Talk
```
Are constants, here they are as you normally might see them:

```ruby
API_USER = 'devuser'
API_PASSWORD = "h1%3am1"

Class Talk < ActiveRecord::Base
end
```

I've seen people use  const_get("string") to get the objectived (yes I make up my own words, I don't know what you would call this? perhaps constantized? keep reading) of a string like: 

```ruby
>> t = Object.const_get("Talk")
=> Talk(id: integer, conference_id: integer, person_id: integer, title: string, description: text, slides_url: string, video_url: string, start_time: datetime, end_time: datetime, created_at: datetime, updated_at: datetime)
>> intro_talk = t.new(title: "Intro to Rails")

t.ancestors[0]
=> Talk(id: integer, conference_id: integer, person_id: integer, title: string, description: text, slides_url: string, video_url: string, start_time: datetime, end_time: datetime, created_at: datetime, updated_at: datetime)
```

Crazy, huh? We look up the constant "Talk" (a subclass of ActiveRecord) and assign it to 't' and use that to instantiate a new record. I don't know why you'd do this, but this is what it's doing and since today I learned that any uppercased ruby identifier is an constant that all makes perfect sense. We can look at the first ancestors and see its ancestor is Talk. 
<!--more-->
I learned doing the [RubyKoans](http://www.rubykoans.com) that constants of the same name have the same object_id and here it looks like it does
```ruby
>> t.object_id
=> 70166169198120
>> Talk.object_id
=> 70166169198120
```

If you are in rails, you can do "string".constantize
```ruby
>> "Talk".constantize.object_id
=> 70247114179900
>> Object.const_get("Talk").object_id
=> 70247114179900
```

Hrm. Lets look at the source of constantize [docs](http://api.rubyonrails.org/classes/ActiveSupport/Inflector.html):
```ruby
# File activesupport/lib/active_support/inflector/methods.rb, line 213
def constantize(camel_cased_word)
  names = camel_cased_word.split('::')
  names.shift if names.empty? || names.first.empty?

  constant = Object
  names.each do |name|
    constant = constant.const_defined?(name) ? constant.const_get(name) : constant.const_missing(name)
  end
  constant
end
```

Right there at the heart of the method, sure enough, is const_get and it returns that value unless its not defined. I do not however understand they first do   constant = Object ? But maybe a reader does.. 

Short post, but its what I was pondering today when I heard all Uppercased identifiers are constants in ruby... and now I understand how and why const_get works. 

