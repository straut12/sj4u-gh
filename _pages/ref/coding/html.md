---
layout: page
permalink: /ref/coding/html/
title: web html css js
description: Intro to html and css and JavaScript
nav: false
toc: true
---
# Intro
html (or markdown) formatted pages are useful for sharing information/data because they can be viewed with a web browser. Generally everyone knows how to use a web browser and every PC system comes with a web browser pre-installed. If you want to share the information in the form of a website you can easily get it hosted by github pages for free.

* [html](https://www.w3schools.com/html/html_intro.asp) (HyperText Markup Language) is the language web browsers use to output a web page. You can use a simple notepad editor to type html formatted text into a txt document and then open it directly with a web browser. [geekforgeeks](https://ide.geeksforgeeks.org/tryit.php) also have a handy app. When viewing a web page using a browser, right click, select 'view page source', and you'll see the html for that page. Tags or angle brackets, often in pairs (opening/closing) are used to write the html elements (<body></body>).  Some common tools used to improve the appearance and usefulness of web pages is cascading style sheets (css), JavaScript, and PHP.  
* [css](https://en.wikipedia.org/wiki/CSS) is a styling tool. It gives the ability to put all of your formatting elements, colors, fonts, tables, menus, etc in one location. This helps with organization and allows you to make global changes easily.  
* [JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/What_is_JavaScript) is a scripting language and can provide a user-interaction element with higher level functions. Typically used on client side or front end (compared to the back end or server side using java, python, ruby, etc).  However Node.js is a run-time environment that will allow javascript to run on server side. 
* [PHP](https://www.php.net/manual/en/intro-whatis.php) is also a scripting language that supports databases and runs on the server side. PHP is older and javascript(Node.js) or Python(flask) are server/back-end frameworks that are other alternatives.

**Client/FrontEnd** - Runs on the client machine/browser. Interface between user and server to make web pages interactive. Sends requests for data to the server and interacts with local memory.  
**Server/BackEnd** - Runs on the server machine. Processes the user input, runs operations on the database/files on the server. Structures the web applications.  

|Client/FrontEnd Scripting and Formatting|Server/BackEnd|
|:--------------------------------------:|:-------------------:|
|HTML, CSS, JavaScript, React, Angular, TypeScript, AJAX|Python, PHP, Java, Ruby, C++|

> [codepen.io](https://codepen.io/) is a great way to test html, css, js code. You get see your code for all three in separate window panes and there are live updates as you type.

How do you start building html pages?  
1. Use an on-line web site builder where you enter text and images and the builder creates the html pages and likely hosts them. This is the most common option if you're not wanting to use it in an off-line, local mode and do not care 'how it works'. Note since this will be on-line you'll need to pay for a domain name, hosting your html files, and you may have to pay for the web site builder, too.
2. The most basic off-line option is using VS code or a notepad editor to write .html pages using tags <>. You can then open it in a web browser. You can even install a web server (ie Nginx or Apache) and be able to view the html pages from any device on your network. Since the files are stored locally on your computer they will not be viewable on the web. But you could always pay for a domain name and upload them to a hosting site. While html has a lot of options for formatting and functionality the drawback is that it's difficult to read in its raw form with many angle brackets (tags) and styling elements.  
  * [Markdown](https://www.markdownguide.org/) is a simpler markup language without the angle brackets making it easy to read in its raw form. While markdown does not have advanced functions it still has options for basic formatting, image links, tables, etc. You can not open markdown directly in a web browser but you can install an extension/plugin or use a tool like pandoc/jekyll to convert it to html.  
3. So a third option is write in the markdown format and use a tool to convert the markdown to html pages
    * [pandoc](https://pandoc.org/) is a simple tool that can convert various types (ie markdown) to another type (ie html)
    * [Jekyll](https://jekyllrb.com/), written in Ruby, also converts markdown to html (blog focused) and is easily integrated with github pages for hosting.

The option I have found myself using is 3. Writing notes in markdown, using jekyll to convert to html, and github pages to host the html (for free). You can fork existing themes/templates that work well with the jekyll/github pages workflow and incorporate advanced formatting/functions with css/scss and JavaScript. This quickly gets you up and running with a nice looking web page at no cost.  
Here's my workflow  
1. Write my notes in [Markdown](https://www.markdownguide.org/). The specific markdown used in the al-folio theme is [kramdown](https://kramdown.gettalong.org/quickref.html)
2. Use [Jekyll](https://jekyllrb.com/) to convert the markdown to html files. It comes with a [sass-converter](../../../ref/coding/html/#sass) and [liquid](../../../ref/coding/html/#liquid) plugins for advanced functions.  
3. Then [Github pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages) will allow you to freely host the static jekyll-powered website.  

For me an important advantage of this option is I still have all my markdown files stored locally and sync'd with github.  

> You can install a web server (ie [Apache](https://ubuntu.com/server/docs/web-servers-apache) or [Nginx](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/)) on your PC/RPi and host the html files you create. This allows you to access your html files from a browser with any device on your network. Firefox/Chrome also have extensions to directly open markdown files if you want to skip the markdown-to-html conversion process. 

# html
Some html components  
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

Basic elements and how they are nested. Note - not all elements are required. Based on examples from w3schools  
The html code below will generate the page below..
```html
<!DOCTYPE html>
<html>
<head>
<title>html example</title> 
<style>

* {
  box-sizing: border-box;
}

body {
  font-family: Arial, Helvetica, sans-serif;
}

/* Style the header */
header {
  background-color: #666;
  padding: 30px;
  text-align: center;
  font-size: 35px;
  color: white;
}

.flex-container{ /* Use flex container to make heights the same for blocks*/
    width: 100%;
    min-height: 300px;          
    display: flex; /* Standard syntax */
}

/* Create two columns/boxes that floats next to each other */
nav {
  float: left;
  width: 20%;
  background: #ccc;
  padding: 20px;
}

/* Style the list inside the menu */
nav ul {
  list-style-type: none;
  padding: 0;
}

article {
  float: left;
  padding: 20px;
  width: 80%;
  background-color: #f1f1f1;
}

/* Clear floats after the columns */
section::after {
  content: "";
  display: table;
  clear: both;
}

/* Style a side bar */
aside {
  width: 30%;
  padding-left: 15px;
  margin-left: 15px;
  float: right;
  font-style: italic;
  background-color: lightgray;
}

/* Style the footer */
footer {
  background-color: #777;
  padding: 10px;
  text-align: center;
  color: white;
}

table {
  width: 60%;
}

td, th {
  text-align: center;
}

/* Responsive layout - makes the two columns/boxes stack on top of each other instead of next to each other, on small screens */
@media (max-width: 600px) {
  nav, article {
    width: 100%;
    height: auto;
  }
}
</style>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>

<body> this is the visible page content, paragraphs, images, links, tables

<header>
  <h2> This is a header </h2>
</header>

<section class="flex-container">
  <nav>
    <ul>
      <li><a href="#">Nav1</a></li>
      <li><a href="#">Nav2</a></li>
      <li><a href="#">Nav3</a></li>
    </ul>
  </nav>
  
  <article>
    <h1>Article header</h1>
    <p>This is content for the article. It should be independent and self contained.</p>
    <table>
        <tr>
          <th>Index</th> 
          <th>Time</th>
          <th>Device</th>
          <th>Location</th> 
          <th>Humidity</th>
          <th>Temp</th>
        </tr>
        <tr ng-repeat="x in msg.payload | limitTo:15">
          <td>1</td>
          <td>05:10</td>
          <td>>esp32</td>
          <td>BldgA</td> 
          <td>53%</td>
          <td>78.3</td>
        </tr>
      </table>
  </article>
</section>

<section>
  <p> This is more content that will have a side bar below it. </p>
  <aside>
    <p> This is content inside the aside. It is indirectly related to the main content </p>
  </aside>
  <p> And this is the content after the side bar </p>
</section>
<footer>
  <p> This is a Footer </p>
</footer>
</body>
</html>
```  
Generate this web page  

<div class="row">
    <div class="col mt-3 mt-md-0">
        {% include figure.html path="assets/img/coding/html.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


More details  
Block vs inline display types  
* block - takes up full width available. Examples are paragraph (p) and division (div)  
* inline - does not start a new line. Example is span  

* div - used to group similar sets of content together or apply the same CSS style or scripting effect to other document elements.  

Pre tag: <pre> preserves space and line breaks. Useful to put code examples inside a pre to keep the text displayed exactly as written.  
Paragraph: <p></p>  
Headers: <h1></h1>  
Elements are identified by the start/end tags. Attributes provide additional info  

```html
<element attribute=value>
```
All html elements can have attributes assigned to them.  

id:  an attribute that assigns a name to an element. Must be unique in the document and only used by one html element in the page. Can be used as a type of bookmark. 

```html
<p id="unique-para1">This has a unique id</p>
```
## Class  
class: an attribute assigning a class name or set of class names to an element. All html elements can have a class attribute. Used with css to identify elements for different styling. Can be used by multiple elements and often used by JavaScripts to access and modify elements with that class name. Can have multiple different classes in the same attribute so long as separated by a space.  

```html
<h1 class="header-example"> A Header Title </h1>
```

JavaScript example with class  
```html
<script> function myFunction(){ function contents}
</script>
```
Example.  
Multiple plants listed with headers (h2)  
The JavaScript function will remove the headers linked to class attribute "plant"  

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

## Table  
Elements of a table  
- tr: row
- th: header
- td: table cell
- other elements, caption, colgroup, thead, tfoot, tbody  

```html
<!DOCTYPE html>
<html>
<head>
<style>
table {
  width: auto;
  border-collapse: collapse;
  overflow: auto;
  height:100px;
  display:block; /* block level start on new line and take up as much horz space as can*/
}

th, td {
  padding: 5px;
}
</style>
<title>Scrollable Table</title>
</head>
<body>
<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Age</th>
      <th>Gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>John Doe</td>
      <td>25</td>
      <td>Male</td>
    </tr>
    <tr>
      <td>Jane Doe</td>
      <td>27</td>
      <td>Female</td>
    </tr>
    <tr>
      <td>Peter Jones</td>
      <td>30</td>
      <td>Male</td>
    </tr>
  </tbody>
</table>
</body>
</html>
```

**More html**  
```html
<section> Similar to a div </section>
<nav> Provides a block of navigation links </nav>
```

---------------------------------------

# css

Formatting style using css

css is used for styling the html page. The syntax is
```html
selector {
   property 1: value;  (declarations)
   color:black;
}
```  

The css (styling language) can be detailed in the html (multiple methods) or placed in a separate file and referenced in <head>  

1. Inline: use the style attribute inside the element  
```html
<table style="width:100%; border: 1px solid"> ..define table.. </table>)
2. Internal: define it in the head section using  
```html
<style>
table, th, td {
  width:100%;
  border: 1px solid;
  border-collapse: collapse;
}
</style>
```
3. External: use <link> element and rel="stylesheet" to link to an external css file (preferred)  

Example  
```html
<html>
<head>
  <link rel="stylesheet" href="my-formatting.css">
</head>
```

And then a file called "my-formatting.css" (in the same dir) contains styling  
Example for the "pre" tag (preformatted text)  
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
If the selector is a class use .class (ie .myclass)  
If the selector is an element name you call it by the name (ie h1)  
If using the id attribute use # (ie #box1 for id="box1")  
If specific class of an element use element.class (ie h1.center)  
The universal selector/wildcard is *. Selects all HTML elements on the page  
Can group selectors with commas (ie tr, td)  

Common css selectors are Elements (pre, h1, div, span, a, btn, head), Class, and ID. For css to know which you are using the class uses a . (period) and id uses a #.  
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

# JavaScript
JavaScript is called in the script tags. It can go inside or outside the body of the html, built-in, or in a separate file.  

If it is inside the body, it will be executed when the document is loaded. If it is outside the body, it will be executed when the user interacts with the document.    
  
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
Triggered by a button and JavaScript located in \<script>  
```html
<body>
<button onclick = "myfunction()">Click this button to trigger the JavaScript function</button>
<script>
function myfunction() {
alert("Next delivery is May 30");
}
</script> 
<body>
```
Or reference JavaScript function in another file  
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

# PHP
PHP is an older scripting language that runs on the server side. You can install a php server extension in vs code and execute .php files. Make sure to create your project in a folder and open the folder instead of opening an individual file (otherwise you'll get 404 file not found error). Once you install the php server extension and create a .php file you can start the server and view the page in a browser.  

1. Install php server in vs code
2. Create folder with .php file (ie index.php) and open the folder
3. Use php tags for scripting
4. Start the php server and go to the web page

To insert php in your html just use the php tags. Example  
```html
<!DOCTYPE html>
<html>
<body>

<h1>This web page includes PHP</h1>

<?php
$txt = "Hello world!";
$x = 5;
$y = 10.5;

echo $txt;
?>

<p>
    Some more text
</p>

<?php
echo "Some math ";
echo $x + $y;
?>

</body>
</html>
```
# PythonAnywhere

Pythonanywhere makes it easy to upload an interactive dash to the web. It has a web terminal that you can use to git clone your project to the pythonanywhere server and then deploy it and view the dash charts on a web page. It is free up to 512MB. I used 93MB (18%) after installing some base packages and adding one small project.  
1. Create your dash project and upload it to github. (make sure your python files is called app.py)
2. Create free [pythonanywhere](https://www.pythonanywhere.com/) account and login
3. Open a Bash console. If you're going to use ssh then create the pythonanywhere ssh keys using ```ssh-keygen -t rsa -b 4096```, add it to ssh-agent ```eval "$(ssh-agent -s)"``` and ```ssh-add ~/.ssh/id_rsa```. Now copy key contents in ~/.ssh/id_rsa.pub to github settings/SSH and GPG keys.  If you do not use ssh you can use the html clone option.
4. Git clone your project to pythonanywhere ```git clone --depth=1 https://github.com/PROJECT-ABC.git```
5. Go to the directory and remove any files that aren't necessary.
6. Install any python packages that are necessary for your project.
7. Go to the Web tab and add a new web app. Select **Flask** and the python web framework.
8. In the Quickstart new Flask project page make sure to update the path with your project directory.
9. After setup double check the source code location and click on the working directory to update it to match. This will make sure any data files in the directory can be opened.
10. Click the [WSGI config file](https://help.pythonanywhere.com/pages/DashWSGIConfig/) and update the from flask_app import app section to the following. Then save and reload the web page.
```console
from app import app
application = app.server
```



# Github Pages with Jekyll
## Github Pages
[GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages) is an option in your github repo that can take markdown or html, css, and JavaScript files in your project and use them to build and publish a website. You have the option of creating from scratch or using pre-existing jekyll gem-based themes that will quickly jump start to a professional looking site with drop-down menus, table of contents, header, footer, etc.  

> For projects the github page will default to your repo name for the url (ie http://username The _config.yml 'base' .github.io/repo) but there is also an option to use a custom domain name if you already own one.  

**Enabling** github pages  
First decide on the type of GitHub Pages Site to host
* Repo project site - url will be **http://username.github.io/repo**.  Html pages are kept in the same repo as the project but the website content is usually stored in a separate branch like gh-branch or in a docs folder on the main branch. Do not specify repo-name as baseurl in _config.yml if using CNAME custom domain name.     
* or User/Organization site - url will be **http://username.github.io** or http://organization.github.io  When creating a repo for a user site the repo should be named **user.github.io**  

Once you know which type you want then enable **GitHub Pages**   
1. Make sure you have a README.md or index.html file in the repo to act as the home page
2. Go to your repo, then **settings**  
3. On the left sidebar is **Pages**  
4. Select the **Branch** (main) and **Save**
5. Go back to your README.md or index.html and make a change (ie type the text Hello World) and commit
6. Go to **Pages** again and it will show the url for your web site (it should follow the url naming convention above)

**Using a custom domain name**  
Github pages makes it possible to use a custom domain name for your url. Note you will only be able to link one github user account to an apex or subdomain name. You can not link different github repos (in the same github account) to different subdomains.  
It will be helpful to know some domain terms  
* **Apex** domain is the name without the subdomain. ie *yoursite.com*  
* **Subdomain** is an additional part to your main domain name. ie the *docs* in docs.yoursite.com. And the subdomain docs.yoursite.com can point to a completely different website. Another example is *www* in www.yoursite.com. It's worth noting *www* almost always points/re-directs to the main domain name instead of a different site.  

So you have a couple options for linking to your custom domain name  
1. Link to the apex domain (ie yoursite.com. Can also configure the *www* subdomain variant)
2. Link to a subdomain (ie docs.yoursite.com)

For either of these you will ..  
* First need a custom domain name (purchase from Namecheap, GoDaddy, Google Domains, etc)  
* Setup your github repo as a project or user site and enable **Pages** (see **enable** section above)  

Now you can setup a link to the apex or subdomain  

**Option 1. Configure apex domain**  
Configure apex domain. GitHub also recommends setting up the *www* subdomain while you're at it. GitHub Pages will automatically create the redirect from www to your apex domain name.
* On github go to **Pages** and enter your domain name in the **Custom domain** name field (yoursite.com)
  * Create a file called CNAME with the domain name in it (yoursite.com)
* Go to your DNS provider and under manage DNS Domain add the A Records for [github IP addresses](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)
  * Type is **A Record**, Host name is **@**, and value/target is **185.199.108.153**
  * Type is **A Record**, Host name is **@**, and value/target is **185.199.109.153**
  * Type is **A Record**, Host name is **@**, and value/target is **185.199.110.153**
  * Type is **A Record**, Host name is **@**, and value/target is **185.199.111.153**
* To setup up the *www* subdomain add a CNAME record  
  * Type is **CNAME**, Host is **www**, and value/target is **user.github.io**
    * Note - In my example above only www was required in the host field because the apex domain name was automatically appended by the DNS provider.
* Go back to github and under **Pages** confirm the DNS check was successful and that **Enforce HTTPS** is enabled.

**Option 2. Configure subdomain**  
Configure subdomain, docs.yoursite.com.  
* On github go to **Pages** and enter your domain name in the **Custom domain** name field (docs.yoursite.com)
  * Create a file called CNAME with the domain name in it (docs.yoursite.com)
* Go to your DNS provider and under manage DNS Domain add a CNAME record. Example below is for a *docs* subdomain  
  * Type is **CNAME**, Host name is **docs**, and value/target is **user.github.io**
    * Note - In my example above only **docs** was required in the host field because the apex domain name was automatically appended by the DNS provider.
* Go back to github and under **Pages** confirm the DNS check was successful and that **Enforce HTTPS** is enabled.

## Jekyll
[Jekyll](https://jekyllrb.com/) is a program, written in Ruby, that can convert plain text markdown files to html. One of the advantages to using Jekyll are [themes](https://jekyllrb.com/docs/themes/) that can quickly improve your website. This includes formatting based on the size of the display, syntax highlighting, etc. These themes leverage off Jekyll plugins for sass and liquid so it helpful to learn some of their syntax. However before trying a highly customized theme it is a good idea to try the jekyll default **minima** theme  

Jekyll has a good [step-by-step tutorial](https://jekyllrb.com/docs/step-by-step/01-setup/) to get familiar with all the steps before diving in   

> Jekyll is written in Ruby. [RubyGems](https://guides.rubygems.org/) is a package manager for Ruby. A Gems file is used to describe dependencies for the Ruby program. It is possible to build a site from scratch by creating your own home page and using the jekyll commands **build** and **serve** to create/view the html. However I jumped ahead to using gem-based themes. The steps below use the jekyll **new** command which starts off with a gem-based theme minima and eliminates a lot of setup work.

First install dependencies including Ruby (example below is for Ubuntu)
* ```sudo apt-get install ruby-full build-essential zlib1g-dev```
* Set up a gem installation directory for your user account. These commands will add environment variables to configure the gem installation path
```console
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```
* Install Jekyll and Bundler  
```gem install jekyll bundler```  

Now setup GitHub repo and run Jekyll commands to create your static web site  
* Follow the github section above to create a github repo and enable GitHub Pages  
* git clone the repo to your local system  
```git clone git@github.com:user/repo.git```  
* **cd** into the repo project directory  
* Build the jekyll site using a gem-based theme with the **jekyll new** command (**jekyll new PATH** creates a new Jekyll site with default gem-based theme in the current dir. You can use --force to over ride the existing README.md file you created for testing)  
```jekyll new . --force```  
* You will see that jekyll created a **Gemfile** and some initial directories. The Gemfile shows **minima** is being used as the theme. It also gives some instructions on using **bundle install** for updating Jekyll and running **bundle exec jekyll serve** for testing your web site.  
```bundle exec jekyll serve --livereload```
* (the jekyll serve command will build the html files to _site dir and create a local web server. the --livereload flag will have your website update automatically as you change your project files)
* After running the serve command You will see a **_site** directory is now created. Your README.md file will be copied there but a index.html will also be created and it will over-ride your README.md as the home site. An assets dir is also created for the minima css formatting.
* Ctrl-click on the local server address http://127.0.0.1:4000/ link to see the default Jekyll page open in a web browser  
* Open the **_config.yml** file and edit **url:** to point to your user (ie https://username.github.io)
  * If doing a project site you can update the base url to point to your repo. But I noticed for custom domain names I had to leave the base url blank for my website to get the theme formatting.
  * You will also notice the config file has Build settings where you set the theme
* You can make changes, add pages in the _posts folder following the naming convention and watch it show up on your home blog page. Depending on the change you may need to hit refresh.  
* When finished testing. Commit/sync your changes to github.
* Go to github.com and under **actions** you should see the site be deployed and turn green.
* Go to **Pages** of your repo and click on the link under **Visit site**. You may have to hit reload on your browser to get the theme formatting to update.

Some quick notes on commands and files  
* The Gemfile amd Gemfile.lock are used by bundler to keep track of the gems. If you open Gemfile.lock you'll see the sass-converter and liquid plugins mentioned previously.
* **bundle install** or just **bundler** will install gems listed in Gemfile  
* **bundle update** installs but will also upgrade installed gems, too. 
* Best practice is to normally use **bundle install** and save **bundle update** for when you're ready to accept updates from the theme developer (ie changes to stylesheets or includes could be made).

Important aspect to gem-based themes  
* Your project folder was updated with a _site folder that showed the processed html files (converted from .md) but many of the folders/files used to create it are not shown.  
* The gem-based theme folder/files (ie assets, _includes, _layouts, _sass) are in a separate location where they can keep receiving updates via **bundle update**. If you want to see where these hidden folders are located run ..  
```bundle info --path <theme>```  
* For example under _layouts you'll see the home.html layout which has the liquid template for looping thru files in the _post directory and automatically updating the blog 
* You can 'disconnect' the theme from getting updates and customize them yourself by copying them from the gem-based location returned above to your project folder. The folder/files in your project folder will over-ride the gem theme location and basically convert your site to a 'regular' theme. (note - this does not work for 'remote' themes. Remote theme folders are located in a github branch) 

**Jekyll folder structure**  
If you follow the gem theme link from running ```bundle info --path <theme>``` you'll find additional folders. The Jekyll site has a good description of these [folders](https://jekyllrb.com/docs/structure/).  
* Note - Files or folders beginning with ., _ , # or ~ in the source directory will not be included in the destination folder. If you want them copied over you'll need to explicitly specify them in the config file **include**.  
* _config.yml - configuration file  
* **_site** - The Jekyll generated or processed files that make up the finished website will be placed here (.html files, main.css, js, images, etc)
* **_posts** - Blog posts following the format YEAR-MONTH-DAY-title.md.  
* **_data** - Jekyll will load all data files (.yml, .yaml, .json, .csv) and they will be accessible via `site.data`. Example if there's a file members.yml under the directory, then you can access contents of the file through site.data.members.
* **.jekyll-cache** - A copy of the generated pages and markup/markdown are kept here for faster serving. Created during jekyll serve. 
* **index.html** - any html, markdown, md files that have a front matter section will be processed by Jekyll

Files stored in the gem theme folder by default  
* [assets](https://jekyllrb.com/docs/assets/)
  * Used by many themes to keep assets (css, images, and js) organized. 
  * The *main.scss* file inside assets will import individual .scss files inside _sass. (note a nested import will behave slightly different)
* [_sass](https://jekyllrb.com/docs/configuration/sass/) - The sass partials imported into main.scss which will then be processed into a single stylesheet main.css that defines the styles to be used for the site. In other words Sass partials, file that begin with an underscore _, are meant to be imported, not compiled to a css file.
* [_layouts](https://jekyllrb.com/docs/layouts/) - Templates for wrapping the content. Each page can use a different layout specified in the front matter. The liquid tag {% raw %}{{ content }}{% endraw %} is used to inject content into the web page.
* [_includes](https://jekyllrb.com/docs/includes/) - Partials that can be mixed and matched in different layouts. The liquid tag {% raw %}{% include filename %}{% endraw %} can be used to include the partial in _includes/filename

> [YAML](https://www.redhat.com/en/topics/automation/what-is-yaml) is used for configuration files (similar to XML and JSON). YAML does not handle tabs. Make sure you use 2 spaces for indenting lines in the YAML files. A file that has YAML front matter (a block of variables at the top of the file between a pair of triple dashes) will be processed by Jekyll. The variables in the YAML front matter can be accessed using Liquid tags. The front matter can even be empty (there does not have to be any variables specified) and Jekyll will process it and put it in the destination folder of the same name as the source.  

Following the previous steps will give you some background on setting up jekyll with the default gem-based minima theme. However there are many more themes with advanced formatting like drop down menus and table of contents. The guides below are for two themes I have tried out ([al-folio](https://github.com/alshedivat/al-folio) and [minimal-mistakes](https://github.com/mmistakes/minimal-mistakes)). Always refer to the theme github site for more detailed instructions. (there are some differences when working with gem-based themes and remote themes where formatting files are stored in a github branch)  

Options for installing custom themes  
* Fork an existing theme on GitHub
* Install with Gem-based method  

If theme has remote option  
* Install with remote-theme method  

If copying from theme already installed locally
* Copy all files from one of your existing GitHub Pages projects

**Fork an existing theme on GitHub**  
1. In github fork the theme and save in your account under a new name
2. On your local PC git clone the newly forked repo ```git clone git@github.com:user/repo.git```  
3. cd to the repo project dir
4. Install the gem files with ```bundle install```
5. Test with ```bundle exec jekyll serve```
6. Update _config.yml
  * Project page - set url to https://username.github.io and baseurl to /repo-name
  * Personal site - set url to https://username.github.io and leave baseurl empty (make sure the repo name is user.github.io)
7. Commit/sync changes and go to github to check actions and verify site
8. Likely will need to go to **Pages** and change Branch to **gh-pages** depending on theme

> Often themes will use a separate branch, **gh-pages**, to hold the built html files. You will need to change the Branch under **Pages** to show **gh-pages** branch is the publishing source.  

**Install with Gem-based method**  
(this example is for minimal-mistakes-jekyll theme)
1. Follow directions above to setup a default minima theme
2. Update the Gemfile with new theme name  
```gem "minimal-mistakes-jekyll"```
3. Update gems with ```bundler```
4. Set theme in _config.yml ```theme: minimal-mistakes-jekyll```
5. You can get updates by running ```bundle update```
6. The default minima theme calls for layouts under different names (ie post and page) so you will have to update these to point to layout listed in minimal-mistakes-jekyll
7. To see location of gem-based files and layouts run ```bundle info --path minimal-mistakes-jekyll```
8. Test with ```bundle exec jekyll serve```
9. Update _config.yml
  * Project page - set url to https://username.github.io and baseurl to /repo-name
  * Personal site - set url to https://username.github.io and leave baseurl empty (make sure the repo name is user.github.io)
10. Commit/sync changes and go to github to check actions and verify site
11. Likely will need to go to **Pages** and change Branch to **gh-pages** depending on theme

**Install with remote-theme method**  
1. Fork from existing theme (ie [mmistakes starter](https://github.com/mmistakes/mm-github-pages-starter/generate))
2. When you have a base jekyll theme installed update it with remote theme locations
3. Update the Gemfile with new plugins if required and run ```bundler```
  * [jekyll remote theme plugin](https://github.com/benbalter/jekyll-remote-theme)
4. Update _config.yml with remote_theme location 
5. Test with ```bundle exec jekyll serve```
6. Update _config.yml
  * Project page - set url to https://username.github.io and baseurl to /repo-name
  * Personal site - set url to https://username.github.io and leave baseurl empty (make sure the repo name is user.github.io)
7. Commit/sync changes and go to github to check actions and verify site
8. Likely will need to go to **Pages** and change Branch to **gh-pages** depending on theme  

**Copying from one of your existing GitHub Pages projects**  
1. Create GitHub repo (including README.md)
2. Turn on GitHub Pages
3. git clone to local and cd to dir
4. Copy projects files, **including hidden folders**, from the existing project to your new project
5. Install gems with ```bundle install```
6. Test with ```bundle exec jekyll serve```
7. Update _config.yml
  * Project page - set url to https://username.github.io and baseurl to /repo-name
  * Personal site - set url to https://username.github.io and leave baseurl empty (make sure the repo name is user.github.io)
8. Commit/sync changes and go to github to check actions and verify site
9. Likely will need to go to **Pages** and change Branch to **gh-pages** depending on theme

## Sass
[Sass](https://sass-lang.com/guide) is a preprocessor to css giving it additional functionality (ie variables, mixins, functions). The Sass code is written in .scss files (Jekyll will look for Sass partials in _sass dir) and then compiled to css. Jekyll comes with a sass-converter plugin to do this for you. But you could also run Sass outside of Jekyll by [installing Sass](https://sass-lang.com/install).  
* The scss syntax is a superset of css. All valid css is also valid scss.
* Sass file that begin with an underscore _ are partials and meant to be imported, not compiled to a css file. Partials contain snippets of code to help organize.
* Mixin - a generic OOP term. A class that contains methods for other classes. A design pattern in which some method of a base class uses a method it does not define. In Sass a mixin lets you make a group of css declarations you want to reuse in multiples places.  

Some Sass examples  
scss syntax
```scss
$grey-color:          #828282;
$grey-color-dark:     #1C1C1D;

body{
  font: $grey-color;
  color: $grey-color-dark;
}
```
is compiled to css syntax  
```css
body {
  font: #828282;
  color: #1C1C1D;
}
```
Mixin Example  
scss syntax  
```scss
@mixin theme($theme: DarkGray) {
  background: $theme;
  box-shadow: 0 0 1px rgba($theme, .25);
  color: #fff;
}

.info {
  @include theme;
}
.alert {
  @include theme($theme: DarkRed);
}
.success {
  @include theme($theme: DarkGreen);
}
```
is compiled to css syntax  
```css
.info {
  background: DarkGray;
  box-shadow: 0 0 1px rgba(169, 169, 169, 0.25);
  color: #fff;
}

.alert {
  background: DarkRed;
  box-shadow: 0 0 1px rgba(139, 0, 0, 0.25);
  color: #fff;
}

.success {
  background: DarkGreen;
  box-shadow: 0 0 1px rgba(0, 100, 0, 0.25);
  color: #fff;
}
```
## Liquid
[Liquid](https://shopify.github.io/liquid/) is a template language and you'll see it used frequently in jekyll themes. You insert it in the markdown (can be placed directly in html) with curly braces  
* {% raw %}{{}}{% endraw %} for outputting variables
* {% raw %}{% %}{% endraw %} for logic statements if/else, loops, include, highlighting code, etc  
Jekyll also has some documentation on [liquid](https://jekyllrb.com/docs/liquid/) and the [includes](https://jekyllrb.com/docs/includes/)  




-----------------------------  
-----------------------------  

(../../../ref/coding/starting-up/)