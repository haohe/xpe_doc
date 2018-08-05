<h3>Tutorial 4: create a simple service</h3>
	
<p>In this tutorial, we are going to develop a system that manages students in a school.  A student has the following information that needs to be stored:
</p>
	
 <table class="table table-striped">
	 <tr><th>Field</th><th>Type</th></tr>
	 <tr><td>studentNo</td><th>String</th></tr>
	 <tr><td>name</td><th>String</th></tr>
	 <tr><td>age</td><th>Integer</th></tr>
	 <tr><td>sex</td><th>Char</th></tr>
</table>
	

<p>First, let's create a new project and open your sitemap.xml file.  Add a new services section to your sitemap.xml so it looks like the following:</p>
	
```xml

<site domain="dev.test">
 <common>
     <head>
         <meta charset="utf-8" />
         <meta name="viewport" content="width=device-width,  initial-scale=1" />
         <meta http-equiv="X-UA-Compatible" content="IE=EDGE" />
         <title>Tutorial 4</title>
     </head>
 </common>
 <page template="templates/single.xml" path="">
     <head></head>
     <load href="content/aboutSite.xml" to="content" />
 </page>
    
 <resources dir="images" path="images" />
 <resources dir="css" path="css" />
 <resources dir="js" path="js" />
 <resources dir="fonts" path="fonts" />
    
    
 <services>
  <service store="students.db" storeType="binary" primaryKey="id" fields="id,studentNo,name" dict="id,studentNo,name,age:i,sex,created:t" seqKey="true">
     <post path="/students" xpipe="http://www.xmlpipe.org/xpe/db/unique/record/post" />
     <get path="/students" xpipe="http://www.xmlpipe.org/xpe/db/search" />
  </service>
 </services>
    
</site>

```

<p>Click build and preview.  Change the path of your browser to  /students.  Your browser should return the following:</p>
   
<pre>
{

    "meta": {
        "total": "0",
        "start": "1",
        "size": "0"
    },
    "data": [ ]

}</pre>

<p>Congratulations, you have successfully created a new database and its associated services.</p>

<p>Let's explain what we have just done in details. First, all services should be declared under the services section in sitemap.xml .</p>
  
<p>
      Next, we declare a database using the service element:
</p>
  
  ```xml
  <service store="students.db" storeType="binary" primaryKey="id" seqKey="true" dict="id:i,studentNo,name,age:i,sex:B,created:t" fields="id,studentNo,name"  >
     ...
  </service>
  ```
  
<p>The store attribute declares the name of the database store and by convention, the name should end with ".db". The storeType should always be declared as "binary". A store here
      corresponds to a table concept in a traditional database.
</p>
  
<p>Each table should have a primary key so we declare our primaryKey as "id". By setting seqKey to true, we want the system to create the id for us automatically.</p>
  
<p>Next, we simply list all of our fields for a student in the dict attribute.  Note that added two special fields: "id" and "created".  The id field is the primaryKey so it should
      be the first field by convention.  The type of the field is defined using the ":" notation.  For example, ":i" means integer type.  If no type is specified, then string type is assumed. 
      For detailed information about XPE DB, please refer to<a href="/documentation/modules/xpedb"> here for details</a>.
</p>
  
<p>Finally, we list the fields that we wish to search in the fields attribute.</p>
  
<p>Now, we need to declare a number of services we need for the given database.  Following the REST style, a student is a resource so the url /students would refer to a collection 
      of students.
</p>
  
<p>To add students to the table, we add a post service to the /students URL.  This means that we invoke the POST HTTP method to add a student to the table.</p>
  
  ```xml
     <post path="/students" xpipe="http://www.xmlpipe.org/xpe/db/unique/record/post" />
  ```
  
<p>The path attribute is the URL path of the service and the xpipe attribute maps the url to the predefined service.</p>
  
<p>To get stored records back, we also need to map a search service to the same URL. However, we must use HTTP GET method this time.</p>
  
  ```xml
       <get path="/students" xpipe="http://www.xmlpipe.org/xpe/db/search" />
  ```
  
<p>Now, we need to actually add some records to the database.  To do that, we first need to create a simple form to collect data and once a user clicks the submit button, we will
  post the data to the /students service.</p>
  
<p>First, let's create a simple form by adding the following to the aboutSite.xml</p>
  
  ```xml
      
   <form onsubmit='return false;' id='form'>
        Student No:<input name="studentNo" /><br/>
        Name:<input name="name" /><br/>
        Age:<input name="age" /><br/>
        Sex:<input name="sex" /><br/>
     <button id='save'>Save</button>
   </form>
      
   <p id="status"></p>
      
  ```
  
<p>We disabled the form's automatic submit feature because we want to call the service directly using the AJAX pattern. Next, we add the UI logics to add a student record. Right
      click on the js folder and add a new file called addStudent.js.  Open this file, you will see:
</p>

```javascript
/**
 * My module:
 *  description about what it does
 */
X.sub("init", function() {
	//fill in your code here
});
```

<p>The X object is a special Javascript object defined in core.js supplied with the framework.  From our experience, we've found that an event-driven architecture is most suitable
      for creating UI applications.  Each component in UI is a module and it should only react to events.  Each component can also generate events.  However, it should not care how those
      events are handled.  This approach allows UI components to be loosely coupled and greatly increases reusability. 
</p>

<p>The above code defines an anonmous function which will react to the "init" event, which is generated with the page is fully loaded.</p>
  
<p>Now, let's modify the file so it looks like:</p>

```javascript
/**
 * My module:
   To add a student to the collection
 */
X.sub("init", function() {
  
  X('status').innerHTML="Loaded";

});
```

<p>The X object can be used to select an element in the page.  The above code will display "Loaded".</p>
  
<p>Let's go back to sitemap.xml and modify the page element:</p>

```xml
 <page template="templates/single.xml" path="">
     <head>
         <script src="js/core.js" />
         <script src="js/addStudent.js" />
     </head>
     <load href="content/aboutSite.xml" to="content" />
 </page>
```

<p>Click Build and Preview and you should see the following:</p>
   
   <img src="/images/t41.png" width="400" /> 

   
<p>When the submit button is clicked, we want to get the data from the from and send it to the service, so we add the following to addStudent.js and it becomes:</p> 
  
  
```javascript
/**
 * My module:
   To add a student to the collection
 */
X.sub("init", function() {
  
  var status = X('status');
  status.innerHTML="Loaded";
  

  X('save').onclick=function() {
    var student={};
    var form = X('form');
    
    student.studentNo=form.studentNo.value;
    student.name=form.name.value;
    student.age=form.age.value;
    student.sex=form.sex.value;
    
    X.post("/students", student, function(resp) {
        status.innerHTML=resp;
    });
   
  };


});
```

<p>When the save button is clicked, we create an empty student object and then copy the value of each from field to the object. We next then post the JSON object to the /students service.
      Once the service returns its result, we print out the response.
</p>

<p>Click build and preview, we can then fill the form and click save. This is what I saw from my browser:</p>
   
<img src="/images/t42.png" width="400" /> 
   
<p>Notice that the service responded with an error code of 0, which indicates that no errors were detected and the record was stored correctly.</p>
   
<p>Now, change your browser's URL path to /students?all=all, you will get something like the following:</p>
   
```json
    
{

    "meta": {
        "total": "1",
        "start": "1",
        "size": "1"
    },
    "data": [
        {
            "id": "0",
            "studentNo": "314",
            "name": "Test2",
            "age": 21,
            "sex": "F",
            "created": 1477914067225
        }
    ]

}
```
