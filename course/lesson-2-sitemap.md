## XPE sitemap

In this lesson, we will learn how to use define sitemap to define a simple application.

### Create and build a new project in the web IDE

Click the IDE link and right click the left panel to create a new project. You can see [this tutorial for details](../tutorials/tutorial_1.md)

You will need to go through the following tutorials:

1. [Build a simple site](../tutorials/tutorial_2.md)
1. [Build a simple site using the template engine](../tutorials/tutorial_3.md)
1. [Create a simple service](../tutorials/tutorial_4.md)

### XML and HTML

Both XML and HTML are markup languages while HTML specialised in defining a web page.  HTML has predefined tags and for convinence alllows for non-ending tags.  XML, on the other hand, needs to be well-formed.  It is much easier to process and validate XML documents due to its simplicity and consistency.

One can write HTML page as a conforming XML document and this is the XHTML standard.  

XPE template engines assembles XML documents and generate concise HTML5 pages. 

### How to work with VUE?

VUE is a popular JS framwork.  However, it has introduced many custom attributes that are not defined in HTML5 and have made it very diffcult to conform to XML.  Consequently, one cannot use XPE template engine if VUE is used.

We therefore need to work directly with HTML5.

You can clone the vue demo project from git@github.com:softtouchit/vue_demo.git 

First, we create a new html package under src and we add the following to the sitemap.xml

```xml
    <resource src="html/hello.html" path="" />
    <resources dir="html" path="" />
```

The resource tag will map the hello.html file direct to the default URL root path.  The resources tag will map all files under html relative to the URL root path so the hello.html can also be reached as /hello.html .

We now create a new file, hello.html, under the html package:

```html
<html>
    
    <head>
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
        <script src="js/hello.js"></script>
    </head>
    
    <body>
        
     <!--#include file="header.html" -->
        
    <div id="app">
      {{ message }}
    </div>
        
    </body>
    
</html> 
```

We support the include directive so you can combine multiple files to create a new page.  This allows common html components to be separated out and only use them as needed.

For example, we save the following as header.html

```html
<div id="header" >
    test header
</div> 
```

Next, we add the vue js to the js package:

```javascript
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})

```

Now, click build and preview, you should see your vue application running.
