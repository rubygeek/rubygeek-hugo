---
categories:
- design patterns
- ruby
comments: true
date: "2012-10-17T00:00:00Z"
title: Using ActiveModel to Validate Input
---

The other day I was working a project where I had to validate some input before running a query:

Say I have a database of friends and their address, and I want to find all friends with at least 3 letters of the name and an optional zipcode. The controller looks like this:

``` ruby
def index
  @name = clean_name(params[:name])
  @zip = params[:zip] 
  if @name.present? && @name.size > 2
    if @zip.present? && @zip.match(/^\d{5}$/)
      @friends = Property.where( "name like ?", "%#{@name}%").where("zip_code = ?", @zip)
    else
      @friends = Property.where( "name like ? ", "%#{@name}%")
    end
  else
    @friends = [] 
  end
end

private

def clean_name(str = "")
  str.gsub(/mrs?|miss|ms|jr|sr/i, "").strip if str.present?
end
```

Ugly huh? Too many if statement and validation and checking for blanks. I was in a rush, so I was trying to get it out there and move on with life, but when I needed the same validation in another action, I figured it was time to bite the bullet and Do It The Right Way.

I started off with a plain old ruby object (PORO) in lib directory so I can validate the params for a search query there (in past, I've put objects like this in models since its *sort* of a model, just not a database model, so .. I am 50/50 on the issue. Meh. I'll figure out the best location later if need be).

``` ruby
class SearchParams
 
  attr_reader :name, :zip_code

  def initialize(args)
    @name = clean_name(args[:name])
    @zip_code = args[:zip_code]
  end

  private

  def clean_name(str = "")
    str.gsub(/mrs?|miss|ms|jr|sr/i, "").strip if str.present?
  end

end

```

Then for the controller:


``` ruby
def index
  @search_params = SearchParams.new(params)  
  if @name.present? && @name.size > 2
    if @zip.present? && @zip.match(/^\d{5}$/)
      @friends = Property.where( "name like ?", "%#{@search_params.name}%").where("zip_code = ?", @search_params.zip)
    else
      @friends = Property.where( "name like ? ", "%#{@search_params.name}%")
    end
  else
    @friends = [] 
  end
end


```

And now that it param in its own class, its much easier to test (I personally despise testing controllers)..and that private method is out of the controller, yay!

``` ruby
describe SearchParams do
  it "should initialize the Search object" do
    @search = SearchParams.new(name: "blah", zip_code: "12345")
    @search.name.should == "blah"
    @search.zip_code.should == "12345" 
  end

  context "clean name" do

    it "should clean name modifiers from search query" do
      keywords = %w(mr mrs miss jr sr)
      keywords.each do |keyword|
        @search = SearchParams.new(name: "#{keyword} Bob", zip_code: "12345")
        @search.name.should == "Bob"
      end     

      keywords.each do |keyword|
        @search = SearchParams.new(name: "blah #{keyword.upcase!}", zip_code: "12345")
        @search.name.should == "Bob"
      end     
    end

  end
end
```

That first test is just sort of a sanity test, to make sure everything is working. I may delete it later. Then I test cleaning the modifiers both lowercase and uppercase to confirm the case insensitivity of the regex (maybe not necessary). The each loop there actually makes several tests in loop.

Better but not by much. Lets add validations from [http://api.rubyonrails.org/classes/ActiveModel/Validations/ClassMethods.html](Active Model) and see if we can cut down on those ugly if statements.

``` ruby
class SearchParams
  include ActiveModel::Validations
  validates :name, presence: true, length: { minimum: 3 }
  validates :zip_code, format: /^\d{5}$/, allow_blank: true
 
  attr_reader :name, :zip_code

  def initialize(args)
    @name = clean_name(args[:name])
    @zip_code = args[:zip_code]
  end

  private

  def clean_name(str = "")
    str.gsub(/mrs?|miss|ms|jr|sr/i, "").strip if str.present?
  end
end
```

Awesome. Now the controller code: 

``` ruby

def index
  @search_params = SearchParams.new(params)

  if @search_params.valid?
    if @search_params.zip_code.present?
      @friends = Friend.where( "name like ?", "%#{@search_params.name}%").where("zip_code = ?", @search_params.zip_code)
    else
      @friends = Friend.where( "name like ? ", "%#{@search_params.name}%")
    end
  else
    @friends = []
  end

end
```

Looks better, we can just do a .valid? call to see if the name is of valid length and the zipcode (if its provided) meets the the requirements of 5 digits. Nicely refactored and now its easier to test. Now we can test the validations:

``` ruby
context "validations" do 
  it "should not be valid for name length of 1" do
    @search = SearchParams.new(name: "b")
    @search.should_not be_valid
  end

  it "should be valid for name length of 3" do
    @search = SearchParams.new(name: "bla")
    @search.should be_valid
  end

  it "should allow blank for zip_code" do
    @search = SearchParams.new(name: "blah", zip_code: "")
    @search.should be_valid
  end

  it "should not allow 1 for zip_code" do
    @search = SearchParams.new(name: "blah", zip_code: "1")
    @search.should_not be_valid
  end
end
```

BTW since there is a method called valid? and it returns a boolean (obviously with the ? at end) in rspec you can just do be_whatever to call that method in your tests.

Beautiful!! ... I sleep better tonight knowing I've made beautiful code :) :) :) :)