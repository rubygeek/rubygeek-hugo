---
categories: ruby
comments: true
date: "2007-02-09T00:00:00Z"
title: Compiling Ruby 1.8.5 on Ubuntu or I'm not afraid of make
---

If there is one thing I learned from perl -- is not to be afraid of make! I installed countless perl modules... and when I first started using Linux heavily, I had to compile some apps! Yikes. But not afraid no more...

I go to the RubyOnRails.com download page to see what the latest version of Ruby is, so I can install it on my Ubuntu VM (downloaded from vmware list of virtual appliances, and running with VMWare player). I was pretty sure it was 1.8.4 but I know there is one version that won't work... and whoa, its version 1.8.5 .. I look in my ubuntu package manager .. and they only have 1.8.4 - always up for a challenge i thought I'd try to compile it myself.

I had to install "build-essential" to get all the goodies "ld" in particular, that it complained about the first time I ran make. Here's what I did:

Downloaded source from www.rubyonrails.com/down

sudo apt-get install build-essential

tar xvfzp ::downloaded file from rails:: and CD into that dir

sudo ./configure

sudo make

sudo make test

sudo make install

(don't forget the sudo! or you may get strange errors!)

Then, to double check and revel in your accomplishment do:

ruby -v

Ahh! wham-bam and you are on the latest and greatest release version! Also while I was at it, I peeked at the source, poked around a bit and was thankful there are a great deal of people smarter than I who wrote this wonderful language.