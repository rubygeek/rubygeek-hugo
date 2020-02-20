---
categories: ruby
comments: true
date: "2007-07-27T00:00:00Z"
title: Weirdness and Coolness in Rails
---

Coolness
--------

First the cool thing... and I think I have a good reason for this... I have a model that is not tied to any particular table, but is a summary table for about 8 tables. Rather than cluttering up my controller with a bunch of stuff, I am putting into a model (keeping REST in mind). I could have put it in the lib dir as a module.. but.. I thought i'd try it in the models dir, so i don't have to require it etc. And later I realized I need a bunch of fixtures for it, so I think its pretty handy as a model. Of course, I took out the base class of active record

app/model/summary.rb
``` ruby
class Summary
   attr_reader :date_from, :date_to
   attr_writer :date_from, :date_to

  def has_dates?
    (self.date_from && self.date_to) ? true : false
  end

  def job_count
    if self.has_dates?
       Job.count(:conditions => ["completed_at BETWEEN ? and ?", self.date_from, self.date_to])
    else
      Job.count(:conditions => "completed_at IS NOT NULL")
   end
   
   ...
end

```

I might want to get the count of jobs for ALL time or a data range, so if there are dates set, it uses those. In addition to jobs, I need to do some 'not so easy" counting of other date and so i started changing my test fixtures to something like this:

test/fixtures/summary_report/jobs.yaml
``` ruby
<% 1.upto(30) do |num| %>
job_<%=num%>:
  id: <%= num %>
  created_at: <%=(Time.now - 1000).to_s(:db) %>
  finished_at: <%= Time.now.to_s(:db) %>
  name: One more number <%= num %> Awesome Job
<% end %>
```

I have a bunch of other tests and I found one of them was not quite right, and my additional fixtures for the summary report messed it up. So I made a directory in test/fixtures/summary_report and then loaded the fixed in my summary test as such:

test/unit/summary_test.rb
``` ruby
require File.dirname(__FILE__) + '/../test_helper'

class SummaryTest < Test::Unit::TestCase
  fixtures 'summary_report/jobs',
            'summary_report/people'

  def setup
    @s = Summary.new
  end

  def test_job_count_all
    assert_equal 50, @s.job_count
  end
  ...
```

So I put fixtures for a particular set of tests in a directory in fixture. Is there a better way to do this? 

Weirdness
---------

I put it in a post I made on <a href="http://www.ruby-forum.com/topic/119523#new">ruby-forum.com</a> but here it is again:

Basically, when I try to use a rails version higher tha 1.1.6 in my vendor/rails directory my migrations don't have "varchar" they have "string" !! WTF! ... I even tried sqlite and same thing. 


I've searched all over the web and mailing lists and can't find any info
on this problem.

I have a site built with Rails 1.1.6  I ran

rake rails:freeze:edge TAG=rel_1-2-3

I ran db:migrate (on a fresh database)

```
 rake db:migrate VERSION=1 --trace
(in /home/nola/projects/tardis/website/trunk)
** Invoke db:migrate (first_time)
** Invoke environment (first_time)
** Execute environment
** Execute db:migrate
== AddContact: migrating
======================================================
-- create_table(:contacts)
rake aborted!
Mysql::Error: You have an error in your SQL syntax; check the manual
that corresponds to your MySQL server version for the right syntax to
use near 'string DEFAULT NULL, `city` string DEFAULT NULL)
ENGINE=InnoDB' at line 1: CREATE TABLE contacts (`id` int(11) DEFAULT
NULL auto_increment PRIMARY KEY DEFAULT NULL, `name` string DEFAULT
NULL, `city` string DEFAULT NULL) ENGINE=InnoDB
/home/nola/projects/tardis/website/trunk/config/../vendor/rails/activerecord/lib/active_record/connection_adapters/abstract_adapter.rb:128:in
`log'
/home/nola/projects/tardis/website/trunk/config/../vendor/rails/activerecord/lib/active_record/connection_adapters/mysql_adapter.rb:243:in
`execute'
/home/nola/projects/tardis/website/trunk/config/../vendor/plugins/mysql_bigint/lib/mysql_bigint.rb:32:in
`create_table'
/home/nola/projects/tardis/website/trunk/config/../vendor/rails/activerecord/lib/active_record/connection_adapters/mysql_adapter.rb:353:in
`create_table'
/home/nola/projects/tardis/website/trunk/config/../vendor/rails/activerecord/lib/active_record/migration.rb:275:in
`method_missing'
/home/nola/projects/tardis/website/trunk/config/../vendor/rails/activerecord/lib/active_record/migration.rb:259:in
`say_with_time'
/usr/lib/ruby/1.8/benchmark.rb:293:in `measure'
/home/nola/projects/tardis/website/trunk/config/../vendor/rails/activerecord/lib/active_record/migration.rb:259:in
`say_with_time'
/home/nola/projects/tardis/website/trunk/config/../vendor/rails/activerecord/lib/active_record/migration.rb:273:in
`method_missing'
./db/migrate//001_add_contact.rb:3:in `real_up'
/home/nola/projects/tardis/website/trunk/config/../vendor/rails/activerecord/lib/active_record/migration.rb:212:in
`migrate'
/usr/lib/ruby/1.8/benchmark.rb:293:in `measure'
/home/nola/projects/tardis/website/trunk/config/../vendor/rails/activerecord/lib/active_record/migration.rb:212:in
`migrate'
/home/nola/projects/tardis/website/trunk/config/../vendor/rails/activerecord/lib/active_record/migration.rb:335:in
`migrate'
/home/nola/projects/tardis/website/trunk/config/../vendor/rails/activerecord/lib/active_record/migration.rb:330:in
`migrate'
/home/nola/projects/tardis/website/trunk/config/../vendor/rails/activerecord/lib/active_record/migration.rb:297:in
`up'
/home/nola/projects/tardis/website/trunk/config/../vendor/rails/activerecord/lib/active_record/migration.rb:288:in
`migrate'
/home/nola/projects/tardis/website/trunk/config/../vendor/rails/railties/lib/tasks/databases.rake:4
/usr/lib/ruby/gems/1.8/gems/rake-0.7.3/lib/rake.rb:392:in `execute'
/usr/lib/ruby/gems/1.8/gems/rake-0.7.3/lib/rake.rb:392:in `execute'
/usr/lib/ruby/gems/1.8/gems/rake-0.7.3/lib/rake.rb:362:in `invoke'
/usr/lib/ruby/1.8/thread.rb:135:in `synchronize'
/usr/lib/ruby/gems/1.8/gems/rake-0.7.3/lib/rake.rb:355:in `invoke'
/usr/lib/ruby/gems/1.8/gems/rake-0.7.3/lib/rake.rb:1739:in `top_level'
/usr/lib/ruby/gems/1.8/gems/rake- 0.7.3/lib/rake.rb:1739:in `top_level'
/usr/lib/ruby/gems/1.8/gems/rake-0.7.3/lib/rake.rb:1761:in
`standard_exception_handling'
/usr/lib/ruby/gems/1.8/gems/rake-0.7.3/lib/rake.rb:1733:in `top_level'
/usr/lib/ruby/gems/1.8/gems/rake- 0.7.3/lib/rake.rb:1711:in `run'
/usr/lib/ruby/gems/1.8/gems/rake-0.7.3/lib/rake.rb:1761:in
`standard_exception_handling'
/usr/lib/ruby/gems/1.8/gems/rake-0.7.3/lib/rake.rb:1708:in `run'
/usr/lib/ruby/gems/1.8/gems/rake- 0.7.3/bin/rake:7
/usr/bin/rake:1

----------------------------
```
SO Basically, its trying to put in a sql create statement like this:
```
CREATE TABLE contacts (
 `id` int(11) DEFAULT NULL auto_increment PRIMARY KEY DEFAULT NULL,
 `name` string DEFAULT NULL,
 `city` string DEFAULT NULL
) ENGINE=InnoDB
```
string? when it should put in VARCHAR(255) .. weird huh?

Here's the migration:
``` ruby
class AddContact < ActiveRecord::Migration
  def self.up
    create_table :contacts do |t|
      t.column :name, :string
      t.column :city, :string
    end
  end

  def self.down
    drop_table :contacts
  end
```

Any ideas? I've also tried with rel_1-2-0 too... and get the same
result. I can create a table with ints or no fields just fine. When I
remove vendor/rails then its fine.
