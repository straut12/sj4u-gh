---
layout: page
permalink: /ref/coding/html/
title: html CSS JS
description: Intro to html and CSS and JavaScript
nav: false
toc: true
---
Web pages are useful for sharing information/data because they can be viewed with a web browser. Generally everyone knows how to use a web browser and every PC system comes with a web browser pre-installed. If you want to share the information on a website you can easily get it hosted by github pages for free.

[html](https://www.w3schools.com/html/html_intro.asp) (HyperText Markup Language) is the language web browsers use to output a web page. You can use a simple notepad editor to type html formatted text into a txt document and then open it directly with a web browser. [geekforgeeks](https://ide.geeksforgeeks.org/tryit.php) also have a handy app. When viewing a web page using a browser, right click, select 'view page source', and you'll see the html for that page. Tags or angle brackets, often in pairs (opening/closing) are used to write the html elements (<body></body>).  Some common tools used to make web pages more powerful is cascading style sheets (css), JavaScript, and PHP.  
* [CSS](https://en.wikipedia.org/wiki/CSS) is a styling tool. It gives the ability to put all of your formatting elements, colors, fonts, tables, menus, etc in one location. This helps with organization and allows you to make global changes easily.  
* [Javascript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/What_is_JavaScript) is a scripting language and can provide a user-interaction element with higher level functions. Typically used on client side.  
* [PHP](https://www.php.net/manual/en/intro-whatis.php) is also a scripting language that supports databases and runs on the server side.

How do you start building html pages?  
1. Use an on-line web site builder where you enter text and images and the builder creates the html pages and likely hosts them. This is the most common option if you're not wanting to use it in an off-line, local mode and do not care 'how it works'. Note since this will be on-line you'll need to pay for a domain name, hosting your html files, and you may have to pay for the web site builder, too.
2. The most basic off-line option is using VS code or a notepad editor to write .html pages using tags <>. You can then open it in a web browser. You can even install a web server (ie Nginx or Apache) and be able to view the html pages from any device on your network. Since the files are stored locally on your computer they will not be viewable on the web. But you could always pay for a domain name and upload them to a hosting site. While html has a lot of options for formatting and functionality the drawback is that it's difficult to read in its raw form with many angle brackets (tags) and styling elements.  
  * [Markdown](https://www.markdownguide.org/) is a simpler markup language without the angle brackets making it easy to read in its raw form. While markdown does not have advanced functions it still has options for basic formatting, image links, tables, etc. You can not open markdown directly in a web browser but you can install an extension/plugin or use a tool like pandoc/jekyll to convert it to html.  
3. So a third option is write in the markdown format and use a tool to convert the markdown to html pages
    * [pandoc](https://pandoc.org/) is a simple tool that can convert various types (ie markdown) to another type (ie html)
    * [Jekyll](https://jekyllrb.com/), written in Ruby, also converts markdown to html (blog focused) and is easily integrated with github pages for hosting.

The option I have found myself using is 3. Writing notes in markdown, using jekyll to convert to html, and github pages to host the html (for free). You can fork existing themes/templates that work well with the jekyll/github pages workflow and incorporate advanced formatting/functions with css/scss and javascript. This quickly gets you up and running with a nice looking web page at no cost.  
Here's my workflow  
1. Write my notes in [Markdown](https://www.markdownguide.org/). The specific markdown used in the al-folio theme is [kramdown](https://kramdown.gettalong.org/quickref.html)
2. Use [Jekyll](https://jekyllrb.com/) to convert the markdown to html files. It comes with a [sass-converter](../../../ref/coding/html/#sass) and [liquid](../../../ref/coding/html/#liquid) plugins for advanced functions.  
3. Then [Github pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages) will allow you to freely host the static jekyll-powered website.  

For me an important advantage of this option is I still have all my markdown files stored locally and sync'd with github.  

> You can install a web server (ie [Apache](https://ubuntu.com/server/docs/web-servers-apache) or [Nginx](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/)) on your PC/RPi and host the html files you create. This allows you to access your html files from a browser with any device on your network. Firefox/Chrome also have extensions to directly open markdown files if you want to skip the markdown-to-html conversion process. 

# html
Some HTML building blocks  
* <> are used for tags and their attributes
* paired has an opening and closing brackets. 
* The opening bracket can contain an attribute field(s)  

```html
<tag attributeA="valueA">Some content goes here
</tag>
```

empty/unpaired looks like  
```html
<img src="image.jpg">
```

Comments are done with either  
```html
<!-- comments --> or 
// single line with 
```

Basic elements and how they are nested. Note - not all elements are required.  
```html
<!DOCTYPE html> inidicates HTML5
<html>  root or all contents, visible and invisible, of the web page
<head> 
<style> used for styling ie css
selector {
   property 1: value;  (declarations)
   color:black;
}
</style>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>example</title>
</head>

<body> this is the visible page content, paragraphs, images, links, tables

Two common grouping elements to provide structure are div and span  

<div> div is block level and often used as a container for other elements. It will start on a new line and takes up the full width </div>  

<span> span is an inline element and used to mark up parts of the document. It only requires as much width as necessary </span>   

</body>
</html>  closing brackets
```

More details
Paragraph: <p></p>
Headers: <h1></h1>
Elements are identified by the start/end tags. Attributes provide additional info
```html
<element attribute=value>
```
All html elements can have attributes assigned to them.
* id:  an attribute that assigns a name to an element. Must be unique in the document and only used by one HTML element in the page. Can be used as a type of bookmark. 
```html
<p id="unique-para1">This has a unique id</p>
```
* class: an attribute assigning a class name or set of class names to an element. All html elements can have a class attribute. Used with css to identify elements for different styling. Can be used by multiple elements and often used by JavaScripts to access and modify elements with that class name. Can have multiple different classes in the same attribute so long as separated by a space.
```html
<h1 class="header-example"> A Header Title </h1>
```
JavaScript example  
```html
<script> function myFunction(){ function contents}
</script>
```
Example.  
Multiple plants listed with headers (h2)  
The javascript function will remove the headers linked to class attribute "plant"  
```html
<button onclick="myFunction()">Hide titles</button>
<h2 class="plant">Alfalfa</h2>
<p>Alfalfa is a plant commonly used for livestock feed</p>
<h2 class="plant">Clover</h2>
<p>Clover is a legume with many uses </p>
<h2>Hackberry Seed</h2>
<p>The hackberry seed contains opal </p>
<script> function myFunction() {
var x = document.getElementsByClassName("plant");
for (var i = 0; i < x.length; i++) {
x[i].style.display = "none";
}
}
</script>
```

The {% raw %}{%  %}{% endraw %} are template tags. They are used to interpolate a tag into the space. Examples include extend, include, or if conditions, loops.

The double curly brackets {% raw %}{{  }}{% endraw %} syntax is for template variables and indicate dynamic content. For example {{temperature}} indicates temperature is being calculated somewhere else and the value is changing.


---------------------------------------

# css

Formatting style using CSS

CSS is used for styling the HTML page. The syntax is
```html
selector {
   property 1: value;  (declarations)
   color:black;
}
```
The css (styling language) can be detailed in the html or placed in a separate file and referenced in <head>  

When detailed in-line it is in <style></style>. (not recommended)  
Example  
```html
<html>
<head>
  <style>
     
  </style>
</head>
```

When placed in a separate file it will be referenced by the link  
Example  
```html
<html>
<head>
  <link rel="stylesheet" href="my-formatting.css">
</head>
```

And then a file called "my-formatting.css" (in the same dir) contains styling for the "pre" tag (preformatted text)  
```html
pre {
  background-color: #bababa;
  color: black;
}
```

Then used like this..  
```html
 <body><pre><code class="lang-Python">
  example code
</code></pre>
</body></html>
```

Common CSS selectors are Elements (pre, h1, div, span, a, btn, head), Class, and ID. For css to know which you are using the class uses a . (period) and id uses a #.  
```html
pre { 
  property: value;
}
.class-name {
  property: value;
}
#id {
  property: value;
}
```

Class selector is the most common, letting you select an html element based on its class attribute. (use a . period)  

Buttons can be used as an example for showing classes. A base btn class could specify the txt color. And then specific button classes could change background color.  
css file  
```html
.btn {color: white; }
,btnBlue {background-color: blue; }
,btnRed {background-color: red; }
```
html file  
```html
<button class="btn btnBlue">  (calling both the base btn class and btnBlue class)
  Blue button 
</button>
<button class="btn btnRed">
  Red button 
</button>
```

Combining css selectors  
```html
<h1 class="blueHeader">
  Blue Header
</h1>
```

You call all h1 blue headers in the css with  
```html
h1.blueHeader {
  color: blue;
}

.ancestor .child {
  property: value;
}
```

To select use a space  
Example  
```html
<div>
  <p> Some text </p>
```

Call in css with   
```html
div p {
   property:value;
}
```

Bootstrap is a set of templates, a front-end framework using bits of html/css code, that can help speed up the development of a website. For example instead of writing a set of html/css code to make professional looking buttons for your page you can use bootstrap buttons already created.  

# javascript
JavaScript is called in the script tags  

Javascript can be called inside html multiple ways  
Placed inline using \<script>  
```html
<body>
<script>
var deliveryDate = 'May 31';
var str = 'Next shipment date is ' + deliveryDate;
setTimeout(function(){
alert(str);
},500); // 500 = 0.5 second or 500ms
</script>
</body>
```

Built-in  
```html
<a href="#" onClick="alert('Next delivery is May 30');">Built in js</a>
```
Triggered by a button and javascript located in \<script>  
```html
<body>
<button onclick = "myfunction()">Click this button to trigger the javascript function</button>
<script>
function myfunction() {
alert("Next delivery is May 30");
}
</script> 
<body>
```
Or reference javascript function in another file  
```html
<script type="text/javascript" src="fileA.js"></script>
<script type="text/javascript" src="fileB.js"></script>
```
JavaScript fileA.js  
```html
function myfunctionA() {
alert('hello');
}
```
JavaScript fileB.js can also call it  
```html
myfunctionA();
```
Another example using ajax  
```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
```

**Buttons**  
Button example with multiple actions  
```html
<input id="btn" type="button" value="Text displayed on the button" onclick="alert('this is 1st popup'); alert('this is a 2nd popup');"/>
```

# Jekyll with Github Pages

## Jekyll
[Jekyll](https://jekyllrb.com/) is a program, written in Ruby, that can convert plain text markdown files to html. One of the advantages to using Jekyll are [themes](https://jekyllrb.com/docs/themes/) that can quickly improve your website. These themes leverage off Jekyll plugins for sass and liquid so it helpful to learn some of their syntax.  

## Github Pages
[GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages) is a service that can take markdown or html, css, and javascript files in your github repo and build/publish a website. You can build these from scratch or use pre-existing jekyll themes and quickly get a professional looking site with drop-down menus, table of contents, header, footer, etc. By default, for projects, the github page will use your repo name for the url (ie http://username.github.io/repo) but there is also an option to use a custom domain.  
Types of GitHub Pages Sites
* Repo project site - http://username.github.io/repo
* User/Organization site - http://username.github.io or http://organization.github.io  When creating a repo for a user site name the repo **user**.github.io 

## Sass
[Sass](https://sass-lang.com/documentation/) is a pre-processor to css giving it additional functionality (ie mixins, functions). Sass code is written in .scss files (Jekyll will look for Sass partials in _sass dir) and then compiled to css. Jekyll comes with a sass-converter plugin to do this for you. But you could also run Sass outside of Jekyll by [installing Sass](https://sass-lang.com/install).  

## Liquid
[Liquid](https://shopify.github.io/liquid/) is a template language and you'll see it used frequently in jekyll themes. You insert it in the markdown (can be placed directly in html) with curly braces  
* {% raw %}{{}}{% endraw %} for outputting variables
* {% raw %}{% %}{% endraw %} for logic statements if/else, loops, include, highlighting code, etc  
Jekyll also has some documentation on [liquid](https://jekyllrb.com/docs/liquid/) and the [includes](https://jekyllrb.com/docs/includes/)   



-----------------------------  
-----------------------------  

(../../../ref/coding/starting-up/)