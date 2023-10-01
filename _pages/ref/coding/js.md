---
layout: page
permalink: /ref/coding/js/
title: JavaScript
description: Intro to JavaScript
nav: false
toc: true
---
JavaScript is used in web projects and NodeRed functions. It is a scripting language and there are two methods for executing it.
* Web browser. Many web sites include javascript so all modern browsers can run it. 
* Or you can run it outside of a browser by installing [nodejs](https://nodejs.org/en/download/). Node.js is a run-time environment for javascript on the server side.  

A big advantage that javascript provides (vs PHP) is asynchronous coding (async function query_api()). While waiting for results from the API other actions can take place.  

**OOP**  
OOP is wrapping variables and functions in objects.  js is a prototype based OOP which means it technically does not have classes. But modern js syntax introduced classes as a *syntactic sugar* to mimic classes from other programming languages. But at its core it's prototype based inheritance.
* Classes have to be specified in specific order. js can not use the class until it is defined in the code.
* Encapsulation - the bundling of data and the methods that act on that data in objects. Access to that data from outside can be restricted.
* Constructor is a special method on js class. When called later using the **new** keyword, constructor will create one new blank js object and assign it properties and values based on the blueprint inside. It will run once we instantiate the class using the **new** keyword.
* Objects in js are reference data types which means that unlike primitive data types they are dynamic.
* Inheritance - if property/method is not found in the child it will look in the parent
* Instantiation - create the object. return the handler.
* Initialization (constructor) - set the initial/default values.

# Syntax  
[w3schools](https://www.w3schools.com/js/default.asp) is a good reference site for syntax

**Variables**

* Older method -> **var**
* ES6 added **let** and **const**
* Better method -> Use **const**, otherwise if variable changes use **let**. 
* Best practice is to minimize the variable's scope.
* **let**
    * Variables defined with let cannot be re-declared. With var you can redeclare
    * Variables defined with let must be declared before use.
    * Variables defined with let have block scope.
* Scope - Before ES6 (2015), js only had global scope and function scope.
    *  **let** and **const** introduced block scope. 
    * Variables declared inside a { } block cannot be accessed from outside the block  
Use **typeof** to check for undefined


-----------------------------  
-----------------------------  