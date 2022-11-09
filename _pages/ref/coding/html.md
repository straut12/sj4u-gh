---
layout: page
permalink: /ref/coding/html/
title: html CSS JS
description: Intro to html and CSS and JavaScript
nav: false
toc: true
---
Why learn html? Creating html web pages are handy for sharing information/data because they can be viewed with a web browser. Generally everyone knows how to use a web browser and every PC system comes with a web browser pre-installed. And if you want to share the information on a website you can easily get it hosted by github pages for free.

html (HyperText Markup Language) is the language web browsers use to output a web page. You can use a simple notepad editor to type html formatted text into a txt document and then open it directly with a web browser. (geekforgeeks also have a handy app).  

When viewing a web page using a browser, right click, select 'view page source', and you'll see the html for that page. Tags or angle brackets, often in pairs (opening/closing) are used to write the html elements (<body></body>).  Two common tools used to make web pages more powerful is cascading style sheets (css) and JavaScript. CSS is helpful to put all of your formatting elements, colors, fonts, etc in one location. Javascript provides a user-interaction element and higher level functions.  

html has advanced functions and formatting but can be difficult to read in its raw form with the many angle brackets, functions, and formatting elements. Markdown is a much simpler markup language and by design much easier to read. This makes it useful for creating quick web pages that do not require a lot of formatting or advanced functions. Although it does not have advanced features there are still options for basic formatting, image links, tables, etc. This can make it useful for turning a simple text documents with notes into a web page.  

So how do you start building html pages?  
* Use a web site builder where you enter text and images and it builds the html pages for you and likely hosts them. This is the most common option if you're not wanting to use it in an off-line, local mode and do not care 'how it works'.
* You can use VS code or a notepad editor to write .html pages using tags <>. You can then open it in a web browser. Due to the tag syntax this will be slow.
* Write in the [markdown](https://www.markdownguide.org/) format and use a tool to convert the markdown pages to html pages
    * [pandoc](https://pandoc.org/) is a simple tool that can convert various types (ie markdown) to another type (ie html)
    * [Jekyll](https://jekyllrb.com/) can convert markdown to html

The workflow I have been happiest with is the combination of jekyll and github pages. Jekyll with github pages can quickly convert my easy-to-read markdown notes/files to a web site.
* [Jekyll](https://jekyllrb.com/) can take markdown with css and liquid templates and produce a complete static website
* [github pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages) will allow you to freely host the static jekyll-powered website
With this option I still have all my markdown files stored locally and sync'd with github.  

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
<h1 id="unique-para1">This has a unique id</h1>
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

href, src, style, alt, lang, title, {% raw %}{{ }}{% endraw %}
​
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





-----------------------------  
-----------------------------  

(../../../ref/coding/starting-up/)

<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/coding/flappybird.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="row justify-content-center float-right">
    <div class="col-4-auto mt-3 mt-md-0">
        {% include figure.html path="assets/img/coding/flappybird.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

----------------------------
Images
can you col-#  col-sm-#   col-md-#   col-lg-#
Use auto to auto size around image
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).