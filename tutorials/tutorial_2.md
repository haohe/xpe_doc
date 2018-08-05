<h3>Tutorial 2: build a simple site</h3>
	
<p>In the last tutorial, we learned how to build and preview a web application.  In this tutorial, let's see how we build a simple web site using XPE
	    and help you understand the concept of sitemap.
</p>
	

    <h4>Step 1: Prepare resources</h4>

<p>This step requires no knowledge of XPE and everything here you do here is based on well known web standards. 
	To build a site, you need to prepare the following resources:</p>

<ol>
	<li>CSS: the style sheets</li>
	<li>Images: icons, background images etc.</li>
	<li>JavaScripts: JavaScript files</li>
	<li>HTML files: HTML files, HTML5 recommended</li>
</ol>

<p>Typically, one organises those files under a css and a js directory
		correspondingly. An images directory is created under the css
		directory, as shown below: </p>

<pre>
		css
        images
		js
		html
</pre>
	
<h4>Step 2: prepare sitemap.xml</h4>
	
<p>The file sitemap.xml defines the overall structure of a site and how URLs are mapped to resources.  Suppose that we want to map /css to the css directory, we 
	    just add the following to sitemap.xml:
</p>
	
```xml
<site domain="{domain name}" >
    <resources path="/css" dir="css" />
</site>  
```

    <p>We will do the same for js, and html</p>    
	
```xml
<site domain="{domain name}" >
          <resources path="/css" dir="css" />
          <resources path="/js" dir="js" />
          <resources path="/" dir="html" />
        </site>  
```

<h4>Step 3: build</h4>
    
    <p>Just click build.</p> 
