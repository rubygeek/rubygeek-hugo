---
categories: active_support
comments: true
date: "2014-03-24T00:00:00Z"
published: false
title: Diving Into ActiveSupport
---

I was browsing around ApiDock.com and happened upon the StringInquirier  class and I thought OH HAY how will that work with poros (plain ruby objects)? I like to use those whenever I can instead of ActiveRecord models since it doesn't load up the database it is faster and easier to test. So I set my head to try it out:


So lets say you have a User object and methods for their status and language.

```ruby
require 'spec_helper'
require_relative '../user'

describe User do
  it "should return status of user" do
    user = User.new(status: "active", language: 'english')
    expect(user.status_active?).to be_true
  end

  it "should return true for language if english" do
    user = User.new(status: 'active', language: 'english')
    expect(user.language_english?).to be_true
  end

  it "should return false for language if its not spanish" do
    user = User.new(status: 'active', language: 'english')
    expect(user.language_spanish?).to be_false
  end
end
```

The values of status and language are strings (maybe in a real app, these would be integers, but just humor me). So one way you could do this is to make methods like this:

```ruby
class User
  attr_reader :status, :language

  def initialize(params)
    self.status = params[:status] 
    self.language = params[:language]
  end

  def status_active?
    @status == "active"
  end

  def language_spanish?
    @language == "spanish"
  end

  def language_english? 
    @language == "english"
  end  
```

So adding 3 methods to the poro to handle them, maybe in a real app you might just use `active?` `spanish?` `english?` and if you wanted the class to support french or piglatin, you'd have to write more methods. 

Take a look at using `ActiveSupport::StringInquirer`

``` ruby
require 'spec_helper'
require_relative '../user'

describe User do
  it "should return status of user" do
    user = User.new(status: "active", language: 'english')
    expect(user.status.active?).to be_true
  end

  it "should return true for language if english" do
    user = User.new(status: 'active', language: 'english')
    expect(user.language.english?).to be_true
  end

  it "should return false for language if its not spanish" do
    user = User.new(status: 'active', language: 'english')
    expect(user.language.spanish?).to be_false
  end
```

Almost the same as the first set of tests except we have `user.language.spanish?` instead of `user.langauge_spanish?`

Now lets take a look at the code:

```ruby
require 'rubygems'
require 'active_support/string_inquirer'

class User
  attr_reader :status, :language

  def initialize(params)
    self.status = params[:status] 
    self.language = params[:language]
  end

  def status=(status)
    @status = ActiveSupport::StringInquirer.new(status)
  end

  def language=(language)
    @language = ActiveSupport::StringInquirer.new(language)
  end
end
```

It's a little shorter, we don't have to make methods for each status and each language. Calling the methods are a little longer, so win-win! 

And here are some examples of using the setters after initialization:

```ruby
context "should change the value and still work" do
  before do
    @user = User.new(status: 'active', language: 'english')
  end
  it "should start out as english" do
    expect(@user.language.english?).to be_true
  end
  it "should change to spanish" do
    @user.language = "spanish"
    expect(@user.language.spanish?).to be_true
  end
  it "should not be english anymore" do
    @user.language = "spanish"
    expect(@user.language.english?).to be_false
  end
end
```

So yeah, its also cool to look at how that method is designed:

```ruby activesupport (4.0.4)
class StringInquirer < String
  private

    def respond_to_missing?(method_name, include_private = false)
      method_name[-1] == '?'
    end

    def method_missing(method_name, *arguments)
      if method_name[-1] == '?'
        self == method_name[0..-2]
      else
        super
      end
    end
end
```

Hmm, have to think on this one. The first method, is called before the second I think. I think it is basically saying "HAY IS THIS THIS THING ON?!" if the method that is called `status.active?` and the last character is a ? (meaning it won't get called if user.blah is called). Then it checks the method name again method_missing to make user it ends in ? ..and then I am not sure what is up. 







