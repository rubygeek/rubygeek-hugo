---
categories: null
comments: true
date: "2017-02-04T00:00:00Z"
title: Setting up a new Ruby or Rails Project
---

A checklist to follow when starting a new project if you are using rbenv or rvm:

Make sure this is in your `.bashrc` or `.zshrc` 

```
alias be="bundle exec"
```

Then any time you would need `bundle exec` you can just use `be`. Or alternatively, Hal Fulton pointed out you can do `bundle exec bash` to get a bash shell that would be the same as using `be` each time :)

1. Create a directory for your project and change into it.
1. Create a Ruby version file:  `echo "2.4.0" > .ruby_version`.
1. Change out of directory and back in, and ensure your version of ruby is correct with `ruby -v`.
1. Make sure Bundler is installed: `gem install bundler`.
1. Create a Gemfile `bundle init`.
1. Setup git repo with `git init`.
1. Create a README file and put the name of your project and what it is used for.
1. Add all files  `git add .`.
1. Commit `git commit -m "inital commit"`.

Then if you are making a ruby gem:

1. Use `bundle gem myawesomegem`or do it by hand (helps you to remember.. hehe)
1. Create a gemspec `touch myawesomelibrary.gemspec`.
1. Make directory: `mkdir lib`.
1. Make library file: `touch lib/myawesomelibrary.rb`.
1. Make test directory: `mkdir test`.
1. Make test file: `touch test/myawesomelibrary_test.rb`.

Gem spec template:

```
Gem::Specification.new do |s|
  s.name        = 'myawesomelibrary'
  s.version     = '0.0.0'
  s.date        = '2017-01-01'
  s.summary     = "My awesome library summary"
  s.description = "My awesome library description"
  s.authors     = ["Awesome Programmer"]
  s.email       = 'myself@awesomeprogramming.com'
  s.files       = ["lib/myawesomelibrary.rb"]
  s.homepage    =
    'http://rubygems.org/gems/myawesomelibrary'
  s.license       = 'MIT'
end
```     

If you are making a rails app:

Add the version of Rails you want to use to the Gemfile ie: `rails "5.0.1"` and `bundle install`.
Then do `rails new .` to create a rails app with the same name as current directory.





