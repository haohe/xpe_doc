<h3>Tutorial 3: build a simple site using the template engine</h3>
	

<h3>The sitemap.xml file</h3>
    
<p>The sitemap.xml is a file that provides detailed build instructions to the XPE Site Compiler.  You can think it as the
    makefile for C programming.</p>
    
<p>The top level element of a sitemap.xml is shown below:</p>

```xml
   <site domain="{domain name}" >
          ...
   </site>  
```   
     
<p>The site element accepts one attribtue: domain, whose value is the domain of the site you are trying to build.  Please note that
     www.example.com is considered to be the same as example.com.  However, dev.example.com is different from example.com.</p>
     
<p>The site element has the following sections:</p>
<ol>
   <li>common: common headers used by all pages</li>
   <li>page: define the main hierarchy of a site</li>
   <li>resources: other static resources needed</li>
   <li>services:  REST style services</li>
</ol>
     
<h4>The common section</h4>
     
<p>Let's consider the following example:</p>
     
```xml
<common>
   <head>
       <meta charset="utf-8" />
       <meta name="viewport" content="width=device-width,  initial-scale=1" />
       <title>SoftTouch Information Technology Pty Ltd - the SOA software expert</title>
       <link rel="stylesheet" href="/css/common.css" />
       <link rel="shortcut icon" href="/images/favicon.ico" />
       <script src="/js/core.js"></script>
       <script src="/js/signon.js"></script>
   </head>
   <load href="content/footer.xml" to="south" />
   <load href="content/logo.xml" to="logo" />
   <load to="menu">
       <menu />
   </load>
</common>
```
     
<p>In the example above, one would notice those tags:</p>
<ol>
 <li>head: the head section is just like the head section of an HTML document and in fact, it supports all the HTML tags in the HTML head
      section.</li>
 <li>load: this is an instruction to tell sitec to load the content to the corresponding page with the given id specified.</li>
 <li>menu: this tag instructs sitec to generate a navigation menu.</li>
</ol>
     
<h4>The page section</h4>
     
```xml
<page template="templates/home.xml" path="" title="Home" fn="index.html">
   <head>
       <link rel="stylesheet" href="/css/home.css" />
       <script src="/js/jquery.js"></script>
       <script src="/js/slider.js"></script>
       <script src="/js/site.js"></script>
   </head>
   <load href="content/slides.xml" to="slide" />
   <load href="content/featured.xml" to="featured" />
</page>
```
     
<p>Each page has several important attributes:</p>
<ol>
 <li>templae:this points to the page template file, relative to the sitemap.xml</li>
 <li>path: this is the URI path to be accessed by end users.  Leave it empty for home page.</li>
 <li>title: the title of the page.</li>
 <li>fn: generated output filename</li>
</ol>
     
<p>A page can be nested inside another page as shown below:</p>
     
```xml
<page path="products" title="Products">
   <page path="clearboard" template="templates/product.xml" title="ClearBoard"
            fn="clearboard.html">
            ...
   </page>
</page>       
```
     
<p>The URI path of the clearboard page will become /products/clearboard as sitec will concate all the ancestor's paths. </p>
<p>A page may be empty as the "products" page in the above example.  In this case, the page merely serves as a logical container 
     that holds a number of pages together.  Hence, it does not need template and fn.</p>
     
<h4>The structure of a page</h4>
     
<p>Let's consider a full example of a page shown below:</p>
     
     
```xml
       <page path="benefits" template="templates/product.xml" title="Benefits"
                fn="benefits.html">
           <head>
               <link rel="stylesheet" href="/css/second.css" />
           </head>
           <load to='page_meta'>
               <meta />
           </load>
           <load href="content/products/clearboard/benefits.xml" to="page_content" />
           <load href="content/products/clearboard/downloads.xml" to="sticky" />
           <load to="content_right" >
               <siblingMenu/>
           </load>
       </page>
```
    
<p>The structure of a page is similar to that of a HTML page.  It has a head section, which contains all the head information for a page
    and all HTML head tags are supported.  Please note that all the head info defined in the common section will be copied all to pages.
</p>        
    
    <h4>The structure of a template</h4>
    
<p>A template file is just a logical structure of a page so it can reused by many pages with similar logical structure.
    The following example shows the template for all product pages:</p>
```xml
     
<content>
    
<head>
   <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1" />
   <title>XPE - the Cloud Development Platform for the Web</title>
   <link rel="stylesheet" href="/css/bootstrap.min.css" />
   <link rel="stylesheet" href="/css/doc.css" />
   <meta name="viewport" content="width=device-width, initial-scale=1" />
</head>
     
<div id="layout">

<div id="top" >
   <div id="logo"/>
   <div id="login" />
</div>

<div id="menu" class="nav" />

<div id="content_left">
   <div id="page_meta">
   </div>
   <div id="page_content">
   </div>
</div>

<div id="content_right" class="float">
</div>
    
<div id="sticky">
</div>

<div id="south">
</div>
</div> 
</content>
```
    
<p>Again, the structure of a template mimics that of a HTML page.  It optionally has a head section so you can add commonly used javascript and css into this section.</p>
    
<p>Each div has an id, which uniquely identifies the div.  Notice the load tags used in each page declaration?  The load tag 
    instructs sitec to copy contents to the div with the specified id.</p>

    <h4>Relationship between a page and a template</h4>

<p>Simply put, a page is an instance of template.  A template can be reused to create many pages.  A template defines the logical
layout of a class of pages.  Consequently, templates are simple and typically consist of a number of "divs" and each "div" is assigned
with an id.</p>

<h4>The load command</h4>

<p>The "load" command can be used inside a page element and it can be used to fill in contents into a div of a template. For example,</p>

```xml
<load href="content/products/clearboard/downloads.xml" to="sticky" />
```

<p>The above load commands loads the XML document downloads.xml to the "sticky" div in the template.</p>

<p>The load command can also work without the href attribute.  For example:</p>

```xml
<load  to="sticky" >
  <p>This is sticky.</p>
</load>
```

<p>This loads the child elements of load to the "sticky" div so it becomes:</p>

```xml
...
<div  id="sticky" >
  <p>This is sticky.</p>
</div>
...
```


<h4>Static resources</h4>

<p>All static resources are defined using the "resources" element.  For example:</p>

```xml
<resources path="css" dir="css" />
<resources path="js" dir="js" />
```

<p>The above definition will automatically map all css files under your src/css directory to /css URL path.  For example, src/css/common.css will be mapped to /css/common.css .</p>

<h4>Javascript framework</h4>
<p>You can use any javascript framework you want and all you need to do is to copy those javascripts to the src/js directory so they are mapped.  Alternatively, you can 
also use their CDN links.   If you use the XPE.js framework, you do not need to put XPE.js javascripts to the src/js directory as sitec will automatically copy them.  However, if
    you do, your version will be used instead.
</p>
