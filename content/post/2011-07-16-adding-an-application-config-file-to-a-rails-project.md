---
categories: ruby
date: "2011-07-16T00:00:00Z"
description: Easy way to add a configuration
title: Adding an application config file to a rails project
---

Recently I worked on a project where I needed certain config variables for staging, production, test etc and I wanted to store all these in a yaml file. I could set them in the different environments files but I prefer them all in one file. 

Create a file in the config/initializers .. I called it 00_load_app_config.rb so I can be sure it gets loaded first. In it I have only a single line
<!--more-->
``` ruby
APP_CONFIG = YAML.load_file("#{Rails.root}/config/app_config.yml")[Rails.env]
```
Then I create a yaml config file at config/app_config.yml

``` yaml
base: &common
    company_name: 'The Awesomest Company Evar!'
    api_username: 'api_user'
    api_password: 'bob123'

development:
    <<: *common
    api_hostname: 'http://api-development.myawesomesite.com'
    twitter_app_key: 'fff'
    twitter_app_secret: 'fff'

staging:
    <<: *common
    api_hostname: 'http://staging-api.myawesomesite.com'
    twitter_app_key: 'sss'
    twitter_app_secret: 'sss'

test:
    <<: *common
    api_hostname:  'http://test-api.myawesomesite.com'
    twitter_app_key: 'eee'
    twitter_app_secret: 'eee'

production:
    <<: *common
    api_username: 'api_user_prod'
    api_password: 'bobprod123'
    api_hostname: 'http://production-api.myawesomesite.com'
    twitter_app_key: 'yyy'
    twitter_app_secret: 'yyy'

```
The variables in the common section are applied to every environment and overridden like in the case of production, which overrides the api_username and api_password. In each environment they pull the company_name from the common block. 

To use a value in your rails app, use the APP_CONFIG array, like

``` ruby
APP_CONFIG['website_name'] or APP_CONFIG['twitter_app_key']
```
It will return the value for the environment currently in Rails.env 

Thanks to my friend Rath who showed me this, I changed the name from config to app_config because I liked it to be more specific, but use whatever name you like! 
