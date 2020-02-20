---
categories: css
comments: true
date: "2010-03-24T00:00:00Z"
title: CSS Frameworks Comparison
---
A few weeks ago I set out to compare Blueprint and YUI Grid. I built a page with blueprint. Then built it with YUI2 Grid and then thought about it awhile. 

At first I was sold on blueprints CSS. Thats cool, thats tight! It just seemed to be so clever. But when i read the following blog I realized, hey blueprint's syntax "span-4, span-20" was putting the implementation into the markup and why is that much different than doing
```
<td colspan="4"></td><td colspan="20"></td>
```
which we all run in horror when someone uses a table to layout a page.

I  quote from:

  [http://foohack.com/2007/08/blueprint-css-framework-vs-yui-grids/](http://foohack.com/2007/08/blueprint-css-framework-vs-yui-grids/)

<blockquote>If it is important enough to define your grid layout in the markup, then you should be using a table. The whole point of CSS layouts is to separate the layout information from the structural semantic markup</blockquote>

.. yuck!! I much prefer YUI2 grid and they have a nifty generator [layout generator](http://developer.yahoo.com/yui/grids/builder/). I like that, I want to be able to define my basic layout and get on with life! I don't want to figure out how many "columns" this is and that. The class names for YUI are kind of non-intuitive (ie: yui-t7 and yui-g), but thats ok. They do use "hd" for head, "bd" for body, and "ft" for footer. So that is good enough for me!  

You can easily assign accessibility tag of "role" to your layout too in the generator, by clicking on the regions drop downs and selecting things like banner, navigation, main, etc.

The following is a 75/25 two column layout

```
<body>
<div id="doc" class="yui-t7">
   <div id="hd" role="banner">
     <h1>Header</h1>
   </div>
   <div id="bd" role="navigation">
     <div class="yui-ge">
       <div role="main" class="yui-u first">
	  <!-- YOUR DATA GOES HERE -->
      </div>
      <div role="complementary" class="yui-u">
	 <!-- YOUR DATA GOES HERE -->
      </div>
    </div>
   </div>
   <div id="ft" role="contentinfo">
     <p>Footer</p>
   </div>
</div>
</body>

```

And the same converted to haml:
```
  #doc.yui-t7
    #hd{ :role => "banner" }
      %h1
        Header
    #bd{ :role => "navigation" }
      .yui-ge
        .yui-u.first{ :role => "main" }
          <!– YOUR DATA GOES HERE –>
        .yui-u{ :role => "complementary" }
          <!– YOUR DATA GOES HERE –>
    #ft{ :role => "contentinfo" }
      %p
        Footer
```

Pretty slick, huh?