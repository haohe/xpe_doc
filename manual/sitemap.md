# Sitemap 参考

<h3>Introduction</h3>

<p>The sitemap.xml defines the sitemap of a site. It provides an overview of a web application and assembles all pages and resources together.</p>
<p>A typical sitemap.xml has the following structure:</p>
    
```xml
        <site domain="dev.test">
            <common>...</common>
            <page template="templates/single.xml" path="" />*
            <resources dir="images" path="images" />*
            <services>...</services>
        </site>
```        
<p>One will immediately notices the following sections:</p>
<p>Common section: this section contains common page headers that will be embedded into all pages.</p>
<p>Page section: a web application contains one or more pages.</p>
<p>Resources section: this mapps static resources to be delivered directly without any processing.</p>
<p>Services section: this section defines REST style services.</p>
    
<h3>The common section</h3>

<p>In the common section, all contents will be incorporated into each page.</p>
    
    
```xml        
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,  initial-scale=1" />
    <title>XPE Cloud IDE</title>
        <link rel="stylesheet" href="/css/common.css" />
        <link rel="shortcut icon" href="/images/favicon.ico" type="image/x-icon" />
        <combine>
            <script src="/js/core.js" />
            <script src="/js/sha256.js" />
            <script src="/js/login.js" />
            <script src="/js/session.js" />
        </combine>
    </head>
```    
    
    
<p>In the example, an html head is defined and the head will be merged with all head sections of each page. All normal html5 head tags are allowed.</p>
<p>The combine element will instruct XPE to combine all scripts together to become one single file in order to reduce the number questions for faster loading.</p>
    
<h3>Using group</h3>    

<p>The concept of group extends from common.  For example, a number of pages need to use the same set of css and javascripts but not all pages do.  In situations like this,
    you first declare a group and assign it a uniquie id.  Subsequently, you refer to the group by referencing this id as shown in the example below.  The framework will
    automatically merge the head section and body sections.
</p>    
    
```xml
    <group id="backend">
        
        <head>
            <link rel="stylesheet" href="/css/pagination.css" />
            <link rel="stylesheet" href="/css/common.css" />
            <link rel="stylesheet" href="/css/form.css" />
            <link rel="stylesheet" href="/css/panel.css" />
            <link rel="stylesheet" href="/css/desktop.css" />
            <link rel="stylesheet" href="/css/leftmenu.css" />
            <script src="/js/desktop.js" />
            <script src="/js/ie8.js" />
            <script src="/js/tinyscrollbar.js" />
            <script src="/js/leftmenu.js" />
            <script src="/js/paginationSamePage.js" />
            <script src="/js/cusImageSelector.js" />
            <script src="/js/getDate.js" />
        </head>
        <load href="content/header.xml" to="header" />
        <load href="content/leftmenu.xml" to="menu" />
        <load href="content/footer.xml" to="footer" />
    </group>
    <!-- 首页 -->
    <page template="templates/single.xml" path="" title="首页">
        <group ref="backend"></group>
        
        <head>
            <link rel="stylesheet" href="/css/panel.css" />
        </head>
        <load href="content/aboutSite.xml" to="panel" />
    </page>
```
    
    
<h3>The services section</h3>

<p>In this section, various services can be configured:</p>

```xml
<services>
            <service store="tickets.db" storeType="binary" primaryKey="id" fields="id" dict="id,projId,title,description,owner,body">
                <get path="/json/ticket" xpipe="http://www.xmlpipe.org/xpe/db/unique/record/get" />
                <del path="/json/ticket/del" xpipe="http://www.xmlpipe.org/xpe/db/unique/record/del" />
                <post path="/json/tickets" xpipe="http://www.xmlpipe.org/xpe/db/unique/record/post" />
                <get path="/json/tickets" xpipe="http://www.xmlpipe.org/xpe/db/search" mask="id,projId,title,description,owner.body" />
            </service>
        </services>
```        
    
<p>Normally, services are grouped together when they are about the same representation so they can share the same configruations. In the example above, typical services associated with a ticket system are defined together.</p>
<p>Configs defined at the top level service </p>
    
    
<h3>Tag references</h3>
<div class="table-responsive">
    <table class="table table-striped table-bordered">
        <tr>
            <th>Tag</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>async</td>
            <td>asynchronous service</td>
        </tr>
        <tr>
            <td>childMenu</td>
            <td>child menu</td>
        </tr>
        <tr>
            <td>combine</td>
            <td>combine scripts and css into one file</td>
        </tr>
        <tr>
            <td>common</td>
            <td>common sections for all pages</td>
        </tr>
        <tr>
            <td>content</td>
            <td>top level container</td>
        </tr>
        <tr>
            <td>default</td>
            <td>default template if no results are found</td>
        </tr>
        <tr>
            <td>get</td>
            <td>http get</td>
        </tr>
        <tr>
            <td>group</td>
            <td>group a block commonly used definitions to be reused by reference</td>
        </tr>
        <tr>
            <td>hide</td>
            <td>hide the page for any menu generations</td>
        </tr>
        <tr>
            <td>load</td>
            <td>load an external fragment into target</td>
        </tr>
        <tr>
            <td>metaInfo</td>
            <td>meta info about search results</td>
        </tr>
        <tr>
            <td>override</td>
            <td>overrides template fragment</td>
        </tr>
        <tr>
            <td>page</td>
            <td>defines a page</td>
        </tr>
        <tr>
            <td>post</td>
            <td>http post</td>
        </tr>
        <tr>
            <td>repeat</td>
            <td>repeating the included template</td>
        </tr>
        <tr>
            <td>resources</td>
            <td>static resources</td>
        </tr>
        <tr>
            <td>service</td>
            <td>defines a service</td>
        </tr>
        <tr>
            <td>services</td>
            <td>a collection of services</td>
        </tr>
        <tr>
            <td>siblingMenu</td>
            <td>sibling menu</td>
        </tr>
        <tr>
            <td>site</td>
            <td>site info</td>
        </tr>
        <tr>
            <td>webSocket</td>
            <td>define a web socket connection</td>
        </tr>
        <tr>
            <td>xlet</td>
            <td>defines an xlet</td>
        </tr>
        <tr>
            <td>xmlSample</td>
            <td>XML Samples, any XML fragment will be rendered in html</td>
        </tr>
    </table>
</div>    