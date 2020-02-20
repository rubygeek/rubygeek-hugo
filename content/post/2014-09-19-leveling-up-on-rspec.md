---
categories: null
comments: true
date: "2014-09-19T00:00:00Z"
title: Leveling up on Rspec
---


The other day I saw there was a Rspec option in `.rspec` I didn't know about and I thought what else do I not know about Rspec? I set myself to dig around in the command line options and source code to see if I could figure out anything else cool. Here are a few of the things I've learned:  

### Require all the time

This is what triggered my curiosity, finding this in `.rspec`:

```
--require spec_helper
```

This will add that require to the top of all your test files, and since all your test files have that require at the top, this cleans up the tests files (and you dont forget it to add it). Cool right? This should go in `.rspec` in the project folder.

### Local Options

I knew about `~/.rspec` (in your home directory) and `.rspec` (in the root of project directory), but digging around in the Rspec source there is also `.rspec-local` which looks to be the place to put personal preferences for the current project. I imagine this might be useful if the team wanted to use a certain formatter and you liked a different one. According to repo this was added 2 years ago, I had no idea! This would be one of the files you would put in your `.gitignore`.

### Dry Run

Want to see how your wording looks with the format documentation (I like to make it read like a happy ending story). 

Use this 

```
rspec --dry-run -f documentation
```

You can also add this into a config file, but most likely you'll just want to run it occasionally so command line is the way to go here.

### Fail Fast

Tired of running the test suite and seeing some pass and some fail, but having to wait till all the tests are done? Add `--fail-fast` to your command line (or `.rspec-local`) to have the tests stop after the first failure. This is one option I think would be good to keep in local since it might be personal preference.

### Run one test

There are many ways to tell Rspec that you want to just run a single test:

Passing `--example` and the name of the test (it "test name") 

```
rspec --example "should group in words of 3"
```

Or just enough of the name to make it unique. Also you can use -e instead of --example

```
rspec -e "should group in"
```

Or you can specify the line number, but be aware this might change as you modify line above this.

```
rspec spec/lib/memory_spec.rb:11
```

### Run Focus Test

An alternative to Run one test is to use the focus. First set it up in the `spec_helper` to run only the focus: true if it exists anywhere, otherwise it runs all the specs so you can totally leave this in the spec_helper file all the time if you like it!

```
# in spec_helper.rb
config.filter_run :focus
```

Then in your test add `focus: true` after the name, like so:

```ruby
it "should group in words of 3", focus: true do
```

I think in future I am going to use focus: true if i am running tests on the command line as I develop them, would make it easier then running the test name or line number because those might change as I develop. 


### Test Formatters

The default report style for Rspec is fine, I like format --documentation and some time ago I found this one by Tim Pope [Fivemat](https://github.com/tpope/fivemat). Add that to your gem file then add in your Rspec config 'fivemat'

```
add to .rspec 
--format Fivemat
or specify on command line
rspec -f Fivemat
```

Looks like this: 

```
Memory ............
User ......
Address ..
```

Name of the class, plus a green dot for each passing test. Also for test failures there is less noise so this is a favorite of mine.

Update, added: [NyanCat Formatter](http://www.mattsears.com/articles/2011/11/16/nyan-cat-rspec-formatter)

I certainly feel like I've Leveled Up my Rspec and hope you have too! :) Code on!!



