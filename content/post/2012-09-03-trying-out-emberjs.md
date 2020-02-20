---
categories: javascript
comments: true
date: "2012-09-03T00:00:00Z"
title: Trying out Emberjs Models and views
---

I've read about emberjs and I've tried [CodeSchool Backbone](http://www.codeschool.com/courses/anatomy-of-backbonejs) and the concepts are interesting, I like the concept of a model updating some text on the screen without having to write $('#myelement').text('updated content') and a friend was mentioning how much they like the emberjs models. So, I wanted to try and in a way I could only worry about a few pieces (that router and junk still kinda makes me crazy for frontend since I do that on the back end.. maybe I just need to do more). I was reading the [Ember Documentation](http://emberjs.com/documentation/) and they have some small examples that inspired me. So I set out to try somethings:

I downloaded the [Starter Kit](https://github.com/downloads/emberjs/starter-kit/starter-kit.1.0.pre.zip) from [emberjs.com](http://www.emberjs.com) and it had a skeleton index and app.js file ready to go. Nice. I first played around with making a model and calling a method on it.
<!--more-->
```javascript
Person = Ember.Object.extend({
    workout: function() {
        alert(this.get('name') + " is working out now!");
    }
});

var person = Person.create({
    name: "Nola Stowe"
});
```

So I can then do:
```javascript
person.workout();
```
and be able to call my method. 

I wanted to try observers next, so I added a workout time to my model and an alert that came up every time it got changed. I added the observer code then tried in the console:

```javascript
/* trying out observers, adding an observer onto the person object  */
person.addObserver('workoutTime', function(){
    alert("your workout time has changed to: " + this.get('workoutTime'));
});

/*  trying out setting the workoutTime */
person.set('workoutTime', 6);
person.set('workoutTime', 8);
```

Cool. Whenever I called set the observer fired. Gotcha, I thought i could do <tt>person.workoutTime = 3</tt> and it would work..but no. You have to call <tt>person.set('workoutTime', 3)</tt>. Ok, now I want to make a view and have it automatically update whenever the workoutTime was changed. So I added a view model and a template:

```javascript
App.MyView = Ember.View.extend({
    'templateName' : 'workouts',
    'person' : person
});
```

Then in index.html


Too lazy to even use a css style block, much less external stylesheet, I inlined some styles so it would be easier to see and not all smashed up in the top left corner. The script tag defines the template, which if you see the name is specified in the model MyView along with the instance variable person. 

The UI shows two buttons and the name of the person and what time their workout starts. 

Now to add some jquery

```javascript
$(document).ready(function(){
  workoutView = App.MyView.create();
  workoutView.append();
  $('#change_button_up').on('click', function(){
      workoutTime = person.get('workoutTime');
      person.set('workoutTime', workoutTime + 1);
  });
  $('#change_button_down').on('click', function(){
      workoutTime = person.get('workoutTime');
      person.set('workoutTime', workoutTime - 1);
  });
});
```

I could refactor into a method that takes "+1" or "-1" but hey, it works. Kinda cool? Now maybe to learn some more things...

[full source of my example code](https://github.com/rubygeek/rubygeek/tree/master/javascript/trying_ember)

Update: For those wanting to make a full app, @trek  suggests [http://trek.github.com/](http://trek.github.com/) as a structure. Thanks
