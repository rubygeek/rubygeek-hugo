---
categories:
- jquery
- javascript
- css
comments: true
date: "2011-01-23T00:00:00Z"
title: Creating a Image Gallery in jQuery
---

For one of my projects, I wanted to have a link that will open an image gallery in a lightbox. I want thumbnails at the bottom, that would display above the thumbs but larger. I found alot...alot of them in jquery. I found blogposts that <a href="http://javabyexample.wisdomplug.com/web-programming/47-javascript/85-30-best-jquery-photo-plugins-sliders-slideshow-galleries-and-scrollers.html">review 30</a> (and <a href="http://www.queness.com/post/222/10-jquery-photo-gallery-and-slider-plugins">and 10</a> and <a href="http://www.webdesignbooth.com/15-jquery-plugins-for-a-better-photo-gallery-and-slideshow/">and 15</a> and <a href="http://blueprintds.com/2009/01/20/top-14-jquery-photo-slideshow-gallery-plugins/">and 14</a> and there is more!) of them at a time! So, do I try and modify one.. or .. do I write one myself?

I was also inspired by <a href="http://www.garann.com">Garann</a> (fellow DevChix) because she decided to write her <a href="http://www.garann.com/dev/2011/a-wishlist-for-contenteditable/">own text editor</a> (something I probably wouldn't attempt...but she said it was not too bad! so maybe it wouldn't be that bad after all??)

So simple interface:
<table style="width:auto;"><tr><td><a href="http://picasaweb.google.com/lh/photo/Xt0VDIPWOZFi-fY6pEP2_38S6LLzbFiuM5YJao6mtuU?feat=embedwebsite"><img src="http://lh5.ggpht.com/_Svdtpc2IQy4/TTyhF7t5KuI/AAAAAAAAB3k/AJ-m12f--P0/s144/image_gallery_1.png" height="350" /></a></td></tr><tr><td style="font-family:arial,sans-serif; font-size:11px; text-align:right"></td></tr></table>

Clicking the link will open the gallery, looking like this:
<table style="width:auto;"><tr><td><a href="http://picasaweb.google.com/lh/photo/hp88eghU9UUViqGeR0zVF38S6LLzbFiuM5YJao6mtuU?feat=embedwebsite"><img src="http://lh6.ggpht.com/_Svdtpc2IQy4/TTyhFgozFwI/AAAAAAAAB3g/OLs7-2Imf_c/s144/image_gallery_2.png" height="350"  /></a></td></tr><tr><td style="font-family:arial,sans-serif; font-size:11px; text-align:right"></td></tr></table>
<!--more-->
``` html
<html>
  <head>
    <title>sample</title>
   <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.4.4/jquery.min.js" type="text/javascript"></script>
   <style type="text/css">
     #gallery { 
 			display: none;
			position: absolute;
			top: 10;
			left: 30;
			width: 500px;
			height: 400px;
			padding: 16px;
			border: 2px solid red;
			background-color: white;
			z-index:1002;
 
     }
     #gallery img { display: block; }
     #gallery a { padding: 5px 3px 0 0; float: left; display: block; }

     #lightbox { 
     	display: none;
			position: absolute;
			top: 0%;
			left: 0%;
			width: 100%;
			height: 100%;
			background-color: black;
			z-index:1001;
			-moz-opacity: 0.8;
			opacity:.80;
			filter: alpha(opacity=80);
     }
    
     #gallery #thumb_scroller {
         position: absolute;
         width: 400px;
         overflow-y: hidden;
         overflow-x: scroll;
         xoverflow:auto;
         height: 90px;
         z-index: 100;
     }
     a#close_link {
       display: block;
       float: right;
       clear:both;
       background: white;
       padding: 3px;
      }
     #gallery2 {
         position: absolute;
         width: 200px;
         overflow-y: hidden;
         overflow-x: scroll;
         xoverflow:auto;
         height: 90px;
         z-index: 100;
         dir: rtl;

     }

#scrollingContainer{
  height: 90px;
  width: 200px;
  overflow-y: hidden;
  overflow-x: scroll;
  position:absolute;
  top:50px;
  left:50px;
  width: 200px;
}

#scrollingContainer DIV.scroller{
  position: relative;
  width:600px;
}


   <!-- Lightbox code from: -->
   <!-- http://www.emanueleferonato.com/2007/08/22/create-a-lightbox-effect-only-with-css-no-javascript-needed/-->
   </style>
   <script type="text/javascript">
     $(document).ready(function() {
       
      $('#gallery_link').click( function() {
         $('#lightbox').show();
         $('#gallery').show();
      });


      $('#close_link').click( function() {
        $('#lightbox').hide();
        $('#gallery').hide();   
      }); 

      $('#gallery a').click( function() {
        $('#big').attr('src', $(this).attr('data-photo'));      
      });
     });
   </script>
  </head>

  <body>
   <div id="lightbox"></div>
   This is a page where there will be a popup gallery of some images.
  <br/><br/>

  <a id="gallery_link" href="#">View</a>
    
  <br/><br/>

  <div id="gallery">
    <a href="#" id="close_link">Close</a>
    <img id="big" height="300" src="photos/image1.jpg">
    <div id="thumb_scroller">
      <a href="#" data-photo="photos/image1.jpg"><img src="photos/thumb_image1.jpg" /></a>
      <a href="#" data-photo="photos/image2.jpg"><img src="photos/thumb_image2.jpg" /></a>
      <a href="#" data-photo="photos/image1.jpg"><img src="photos/thumb_image1.jpg" /></a>
      <a href="#" data-photo="photos/image2.jpg"><img src="photos/thumb_image2.jpg" /></a>
      <a href="#" data-photo="photos/image1.jpg"><img src="photos/thumb_image1.jpg" /></a>
      <a href="#" data-photo="photos/image2.jpg"><img src="photos/thumb_image2.jpg" /></a>
    </div>
  </div>

  <div id="scrollingContainer">
  <div class="scroller">
      <a href="#" data-photo="photos/image1.jpg"><img src="photos/thumb_image1.jpg" /></a>
      <a href="#" data-photo="photos/image2.jpg"><img src="photos/thumb_image2.jpg" /></a>
      <a href="#" data-photo="photos/image1.jpg"><img src="photos/thumb_image1.jpg" /></a>
      <a href="#" data-photo="photos/image2.jpg"><img src="photos/thumb_image2.jpg" /></a>
      <a href="#" data-photo="photos/image1.jpg"><img src="photos/thumb_image1.jpg" /></a>
      <a href="#" data-photo="photos/image2.jpg"><img src="photos/thumb_image2.jpg" /></a>
  </div> 

  </body>
</html>
```


I can optimize the javascript some.. but just to show how easy it is..

The thumbnails are surrounded with an Anchor tag that has an attribute data-photo with the filename of the larger image. In the click function, It is grabs that value when you click the image and sets the src attribute of the #big id to the larger image. Easy huh! too easy.. cool stuff... 

When I put this into production, I might have to tweak some things.. but..  I was happy with how easy it is to make an image gallery. Plugins have the advantage of being "drop-in" but in this case none of them fit my needs, so b

Special thanks to for the Emanuele Feronato for <a href="http://www.emanueleferonato.com/2007/08/22/create-a-lightbox-effect-only-with-css-no-javascript-needed/">lightbox with css script</a>