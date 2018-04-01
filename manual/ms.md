
<h2>XPE Messaging Service</h2>


<h3>Starting server and configurations</h3>

    <table class="table table-striped table-bordered">
        <tr>
            <th>Name</th>
            <th>Default value</th>
            <th>Description</th>
            <th>Sample</th>
        </tr>
        <tr>
            <td>timeout</td>
            <td>10000</td>
            <td>This is the heartbeat interval for MS.  The MS will send out a heartbeat out to keep a TCP connection alive for every timeout milliseconds</td>
            <td>timeout=10000</td>
        </tr>
        <tr>
            <td>xpeBase</td>
            <td>work</td>
            <td>This is the default work directory</td>
            <td>xpeBase=work</td>
        </tr>
    </table>


    
<h3>Introduction</h3>

    <p>XPE Messaging Service (MS) is a messaing platform that provides reliable and high-performance messaging services for XPE web applications. It allows complex data processing to be done using JavaScript. With MS, one can delegate expensive data operatioins
        to a cluster of backend hosts.</p>
    <p>MS can be regarded as the backbone of web applications as it can integrate a large number of applications together and deliver messages to their recipients by utilising a varity of messaing patterns.</p>
    <p>A MS is running in a separate process which can run on the same host as XPE web servers or on a remote host.  The relationship between XPE web servers and XPE MS serve is N-to-N.</p>
    
<h3>MS concepts</h3>

<h4>MS Model</h4>

<p>An application that runs insider a MS server is called a model, which consists a model file with .mml suffix, zero or more JavaScript files and other resource files typically defined under
the ms package of a web application.  </p>


<h4>Topic</h4>

<p>A model defines one or more topics.</p>

    <p>A Topic has one publisher and 0 or more subscribers.</p>
    <p>Because each subscriber may need to work at different pace so a topic needs to serve each subscriber with different messages.</p>
    <p>A Topic stores all messages published by a publisher and provides searching services to its publisher and subscribers. It allows messaging to be resumed in the event of system crash.</p>



    
<h4>Message</h4>

    <p>A message needs to be confirmed to a particular protocol so its recipients can understand it.</p>
    <p>A message is less than 64 KB. Messages are usually sequenced so they can be traced. Unsequenced messages can be delivered but there is no guarantee that they will arrive at the destination.</p>
    <p>The following protocols are supported:</p>
    <ol>
        <li>DB log (dblog): a fast log based DB synchronisation protocol and allows two or more XPE DBs to be totally synchornised.</li>
        <li>Binary (binary): key-value pairs are encoded using a cache friendly format, which can be regarded as a binary version of JSON</li>
        <li>JSON (json): the standard UTF8 encoded JSON data format.</li>
        <li>XPE 2.0 XML (xpe20): key-value pairs encoded using XML.</li>
        <li>HEP convert (hepconvert): Special format for HEP resource data conversion</li>
    </ol>


<h4>Messaging</h4>

<p>Messaging is defined as sending/receiving messages among topics,  web servers and other external systems. </p>


<h4>Principles </h4>

<ol>
    <li>Single-writer principle: any topic should only have one writer. </li>
    <li>One-way messaging: the flow of a type of message should never form a cycle.</li>
</ol>


    
<h4>Transport</h4>

    <p>The transport is identified by the corresponding URI scheme or the transport attribute. The following transports are supported</p>
    <ol>
        <li>HTTP 1.1 (http): used with Binary, JSON and XPE 2.0 XML. Example: http://example.com</li>
        <li>Web Socket(ws): used with Binary and DB log Example: ws://example.com/ws</li>
        <li>Local(no scheme): passing messages within the same MS. Example: localService</li>
    </ol>
    
    
<h4>Publisher</h4>

    <p>A Publisher publishes messages from its src to a Topic. A topic is backed by an XPE DB so messages are persisted.</p>
    <p>The main purpose of a publisher is to reconcile messages with its src.</p>
    
<h4>Subscriber</h4>

    <p>A subscriber listens to the arrival of messages on a Topic. It then converts the message to the format understood by its destination or performs a task.</p>
    
<h3>Execution Model</h3>

    <p>MS is event based. When an event detected by a publisher, it sends the event to the topic it belongs to and after performing necessary conversions.</p>
    
<h3>Configuration Details</h3> 
    
<h4>Configuration introduction</h4>

    <p>Each MS application is configured using a MS XML document. The overall structure of the document is the following:</p>
    <xmlSample>
        <ms domain="">
            <topic>
                <pub/><sub />*</topic>*
            <handler>
                <script/>*</handler>
            <mapReudce/>*
            <template/>*</ms>
    </xmlSample>
    <p>Each MS application must only have one root level ms element and it must have a domain attribute. The value of the domain uniquely determines the application. It is a good practise to have the same file name and the domain.</p>
    <p>The XML file is then copied to the "models" directory of the XPE MS server and the application will be loaded automatically by the server.</p>
    
<h4>Topic configuration</h4>

    <p>A topic is backed by a DB and consequently shares many of the attributes for configuring a DB. If a topic has the same name as a DB (without the .db suffix) defined in sitemap.xml, then the dict and primaryKey attributes do not need to be defined. For
        example, if a DB's store name is students.db and a topic's name is students, then they are considered to be the same.</p>
    <table class="table table-striped table-bordered">
        <tr>
            <th>Name</th>
            <th>Mandatory</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>name</td>
            <td>Y</td>
            <td>Name of the topic</td>
        </tr>
        <tr>
            <td>dict</td>
            <td>Y</td>
            <td>The DB dictionary schema. A list of keys and types. If the name of the topic is the same as a store already defined in sitemap.xml, then this is not required.</td>
        </tr>
        <tr>
            <td>fields</td>
            <td>N</td>
            <td>A list of fields for indexing.</td>
        </tr>
        <tr>
            <td>protocol</td>
            <td>Y</td>
            <td>The messaging protocol used by the subscriber will conform to when sending messages to the destination</td>
        </tr>
    </table>
    <p>Once a topic is defined, its search is available in JavaScript under the domain object. For example, a topic called "users" will have its searcher available as domain.users. The search object has a search method that can then be called to search the
        DB.</p>
    
<h4>Publisher configuration</h4> 
    <p>The following common attributes are shared by all subscribers:</p>
    <table class="table table-striped table-bordered">
        <tr>
            <th>Name</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>src</td>
            <td>The src of the data.</td>
        </tr>
        <tr>
            <td>protocol</td>
            <td>The messaging protocol used by the publisher to read data from src</td>
        </tr>
    </table>
    <p>The following combination of transport and protocols are allowed:</p>
    <table class="table table-striped table-bordered">
        <tr>
            <th>Transport</th>
            <th>Protocol</th>
            <th>Description</th>
            <th>Additional attributes</th>
        </tr>
        <tr>
            <td>ws</td>
            <td>dblog</td>
            <td>It is used to connect XPE 3.0 to ms server. This protocol ensures that an identical copy of the DB will be created.</td>
            <td></td>
        </tr>
        <tr>
            <td>local</td>
            <td>json</td>
            <td>It is used to connect take the output of a map reducer to store the results into DB.</td>
            <td></td>
        </tr>
        <tr>
            <td>local</td>
            <td>binary</td>
            <td>It is used to connect another subscriber or processor that produces binary message.</td>
            <td></td>
        </tr>
    </table>
    
<h4>Subscriber configuration</h4> 
    <p>The following common attributes are shared by all subscribers:</p>
    <table class="table table-striped table-bordered">
        <tr>
            <th>Name</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>dest</td>
            <td>The destination that the subscriber will send messages to</td>
        </tr>
        <tr>
            <td>protocol</td>
            <td>The messaging protocol used by the subscriber will conform to when sending messages to the destination</td>
        </tr>
    </table>
    <p>The following combination of transport and protocols are allowed:</p>
    <table class="table table-striped table-bordered">
        <tr>
            <th>Transport</th>
            <th>Protocol</th>
            <th>Description</th>
            <th>Additional attributes</th>
        </tr>
        <tr>
            <td>local</td>
            <td>json</td>
            <td>Standard JSON format provided that the destination understands the JSON message. This subscriber also supports query attribute. Typically used for further data analysis.</td>
            <td></td>
        </tr>
        <tr>
            <td>local</td>
            <td>json/uploader</td>
            <td>This is used for uploading resources to XPE. The provided JSON should include oid, path, and mimeType.
                <ol>
                    <li>oid: the original id, it is business specific and the service will just pass on to the receiving party</li>
                    <li>path: the path to the resource</li>
                    <li>mimeType: the mime type of the resource</li>
                </ol>
                <p>For example:</p> <pre>
              {"oid":"1","path":"IMG_1092.JPG","mimeType":"image/jpeg"}
          </pre>

            </td>
            <td></td>
        </tr>
        <tr>
            <td>ws</td>
            <td>dblog</td>
            <td>The native dblog format for synchornising two DBs</td>
            <td></td>
        </tr>
        <tr>
            <td>http</td>
            <td>json</td>
            <td>Standard JSON format provided that the destination understands the JSON message.</td>
            <td></td>
        </tr>
        <tr>
            <td>http</td>
            <td>json/uploader</td>
            <td>A special JSON format that instructs the subscriber to upload a file to a remote service using the chunked service.</td>
        </tr>
    </table>
    
<h4>Map Reducer</h4>

    <p>A map reducer connects a subscriber and a publisher. It receives messages from a subscriber and then processes those messages and sends results to a publisher.</p>
    <p>A map reducer usually works in three stages:</p>
    <ol>
        <li>Filtering: conditions that determine if an incoming message is of interest for further processing.</li>
        <li>Mapping: map the incoming messages to an intermediate format for further processing. For example, grouping.</li>
        <li>Reducing: perform the final calculations and generate the output</li>
    </ol>
    <xmlSample>
        <mapReduce name="userEmail">
            <processor>
                <![CDATA[ (function() { var o=n ull; function map(data) { // this method is called whenever a new message arrives // this method should check if the data is an item that is of interest, if so, map its attributes to a tempory object o=data; } function
                process() { //this get calld after calling map, one should return an empty array if nothing to return, or an array of objects for downstream. if (o) { return [o]; } return []; } return { map: map, process: process }; }()); ]]>
            </processor>
        </mapReduce>
    </xmlSample>
    
<h3>Examples</h3>

    
<h4>Creating a MS service for front end</h4>

    <p>Problem: We need to implement specific business logics for form submission</p>
    <p>Solution:</p>
    <p>In your sitemap.xml, add the following:</p>
    <xmlSample>
        <service path="/forms" store="forms.db">
            <post xpipe="http://www.xmlpipe.org/xpe/ms/request" />
            <get xpipe="http://www.xmlpipe.org/xpe/ms/request" />
            <webSocket path="forms/req/ws" protocol="dblog" xpipe="http://www.xmlpipe.org/xpe/ms/record" />
        </service>
        <webSocket path="forms/resp/ws" protocol="dblog" xpipe="http://www.xmlpipe.org/xpe/ms/record/response" store="formsResponses.db" />
    </xmlSample>
    <p>In this example, we allow a user to send get and post requests, and we will store all the requests in the forms.db. We also defined two web sockets: forms/req/ws for sending requests to MS for processing, and forms/resp/ws for the MS to send responses
        back.</p>
    <p>Next, we define the following model on the MS side by creating a msdemo.mml under src.ms package:</p>
    <xmlSample>
        <ms domain="msdemo">
            <topic dict="id:i,jobId:i,path:s,params:s,body:s,created:t" name="requests" fields="id">
                <pub src="/forms/req/ws" protocol="dblog" transport="ws" /><sub dest="handleRequest" protocol="json" mask="id,jobId,path,params,body,created" />
            </topic>
            <topic name="responses" primaryKey="id" seqKey="true" dict="id:i,jobId:i,response:s,created:t">
                <pub src="handleRequest" protocol="json" /><sub dest="/forms/resp/ws" protocol="dblog" transport="ws" />
            </topic>
            <topic name="test" dict="id:i,body,created:t">
                <!-- A local publisher, only local publisher can be written to -->
                <pub protocol="json" />
            </topic>
            <mapReduce name="handleRequest">
                <processor>
                    <![CDATA[ (function() { var o=n ull; function map(data) { o=data; } function process() { var r=[]; var data={}; data.body='text body'; //now store data to the test db //you access each topic via the domain object //update is only available on a topic
                    with local publisher var res=d omain.test.update(data); //print(JSON.stringify(res)); //simillary, you can access the searcher //searcher is available on all topics res=d omain.test.search("all=all"); //print(JSON.stringify(res)); if (o) { var resp={};
                    resp.response=o; resp.id=o.id; resp.jobId=o.jobId; r.push(resp); } return r; } return { map: map, process: process }; }()); ]]>
                </processor>
            </mapReduce>
        </ms>
    </xmlSample>
    <p>The following allows the MS to get requests from the web server and send responses back to the web server:</p>
    <xmlSample>
        <topic dict="id:i,jobId:i,path:s,params:s,body:s,created:t" name="requests" fields="id">
            <pub src="/forms/req/ws" protocol="dblog" transport="ws" /><sub dest="handleRequest" protocol="json" mask="id,jobId,path,params,body,created" />
        </topic>
        <topic name="responses" primaryKey="id" seqKey="true" dict="id:i,jobId:i,response:s,created:t">
            <pub src="handleRequest" protocol="json" /><sub dest="/forms/resp/ws" protocol="dblog" transport="ws" />
        </topic>
    </xmlSample>
    <p>Notice that when we get the requests, we publish them to the handleRequest, which is a map reduce processor defined in Javascript.</p>
    
<h4>Synchronising a remote XPE 2.0 data source</h4>

    <p>Problem: an existing site is developed with XPE 2.0 technology and we need to import data int an XPE 3.0 site.</p>
    <p>The whole application is defined using the following configuration:</p>
    <xmlSample>
        <ms domain="xpe20to30">
            <dbsync name="news" thumbWidth="400" restartInterval="20000" map="news.map" store="news.db" primaryKey="id" fields="">
                <pub src="http://example.xpe20.com/data" /><sub dest="ws://example.xpe30.com/news/ws" protocol="dblog" />
            </dbsync>
        </ms>
    </xmlSample>
    <p>In the above example above, the dbsync element defines a topic specifically for XPE 2.0. The mapping rules are defined in news.map file. The restartInterval specifies how long should the ms wait before checking the XPE 2.0 site again.</p>
    
<h4>Modules</h4> 
    <p>Additional modules are supported and they need to be deployed under work/modules directory. Restart is currently required when deploying a new module.</p>
    <p>Modules provide additional support data processing and protocols.</p>
    
<h5>Recommender Module</h5>

    <p>In the following example, a recommender has been configured as it supports the binary/userAction protocol.</p>
    <p>This module requires the topic dict to contain those fields userId, itemId:i, rating:b, created:t. Other fields are allowed but those fields must present.</p>
    <p>The recommender is triggered when the number of events exceeds the number defined in numberOfEvents. It will generate a number of recommendations for each user.</p>
    <xmlSample>
        <topic dict="id,userId,itemId:i,rating:b,created:t" name="testData" store="uc.db" primaryKey="id" storeType="binary" seqKey="true" fields="">
            <pub src="ws://example" protocol="dblog" /><sub dest="recommendations" protocol="binary/userAction" numberOfEvents="10000" />
        </topic>
        <topic dict="id,userId:l,itemId:i,rating:i,created:t" name="recommendations" store="rec.db" primaryKey="id">
            <pub src="recommendations" protocol="binary" />
        </topic>
    </xmlSample>
    
<h4>Sending Emails</h4>

    <p>Problem: When a user just registered with us, we'd like to send a welcome email to the user</p>
    <p>The whole application is defined using the following configuration:</p>
    <xmlSample>
        <ms domain="example">
            <topic dict="id:l,username,password,email,profile:j,permMask:s,group:l,created:t,lastUpdated:t" name="users" store="users.db" primaryKey="id" storeType="binary" seqKey="false" fields="id,username">
                <pub src="ws://example.com/ws/users" protocol="dblog" /><sub dest="userEmail" protocol="json" mask="username,profile,email,created,lastUpdated" />
            </topic>
            <topic name="sendMail" primaryKey="id" seqKey="true" dict="id,from,to,cc,bcc,subject,body" store="mails.db">
                <pub src="userEmail" protocol="json" /><sub dest="mail://sales@softtouchit.com" smtp.host="mail.tpg.com.au" smtp.debug="true" username="username" password="password" from="sales@softtouchit.com" />
            </topic>
            <template name="welcome" src="welcome.html" />
            <mapReduce name="userEmail">
                <processor>
                    <![CDATA[ (function() { var o=n ull; function map(data) { o=n ull; if (data.created && data.created==data.lastUpdated) { o={ }; if (data.profile&&data.profile.name) { o.name=data.profile.name; } else { o.name="Customer"; } o.to=data.email; } } function
                    process() { if (o) { o.subject="Welcome to bonsainet.com.au"; o.body=welcome.gen(o); var r=[]; r.push(o); return r; } return []; } return { map: map, process: process }; }()); ]]>
                </processor>
            </mapReduce>
        </ms>
    </xmlSample>
    <p>We first listen to the user db, so when a user's account is modified we will receive an account. If the created time and lastUpdated time is the same, then we know that the user has just registered.</p>
    <p>We also define a template using the template element. Once defined, the template is available as an object in javascript with the name defined. The object would have a gen method that takes a JSON object and generates the full text while replacing the
        varialbes in the template with the values defined in the JSON object.</p>
    <p>Once we have the user details, we extract the username and put it in an object and we will call the welcome.gen(o) to generate the text. The result will be sent to sendMail topic and the mail subscriber will then be responsible for sending the actual
        mail.</p>
    
<h4>Invoke an external REST service</h4>

    <p>Problem: a user uploads data to XPE and the data needs to be processed by an external service. For example, a purchase order.</p>
    <p>The whole application is defined using the following configuration:</p>
    <xmlSample>
        <ms domain="hep">
            <topic name="resources" primaryKey="id" store="uploads.db" dict="id,username,path,filename,mime,created:t,lastUpdated:t,size:l,access:b">
                <pub src="ws://example.org/uploads/ws" protocol="dblog" /><sub dest="http://rest-example:8080/rps/resourceProcess.htm" protocol="hepconvert" backURL="http://{address}/jpk/converts" uploadedResourceURI="http://{resource address}/upload" />
            </topic>
        </ms>
    </xmlSample>
    <p>The ms element is the top level element for this application and it requires the domain attribute. This attribute must uniquely identifies the application.</p>
    <p>The topic element defines a topic. Its name attribute uniquely identifies this topic. The primaryKey, store, and dict attributes are required to define the structure of its backing DB.</p>
    <p>The pub element defines the publisher for the topic. The src attribute defines where the data is coming from. In this case, it is a remote Web Socket address using the dblog protocol. This allows the remote db to be synchronised to the local topic DB.</p>
    <p>The sub element defines a subscriber. In this case, it will invoke a remote REST service using a special data mapper that maps the internal DB data to a special XML format used by the remote service.</p>
    
<h4>Data analysis</h4>

    <p>Problem: we want to find out how many views a video has. It is impractical to update the metadata of the article whenever a user views the video as this would cause massive performance drains.</p>
    <p>To solve the problem, we first need to have log DB on the Web frontend so whenever a user views a video, it is logged as an event. At the minimum, we need to give a unique id, parent (the id of the video) and the time the event occurred. We will skip
        how this is done at the front end. On the MS side, we just need to define an actions topic:</p>
    <xmlSample>
        <ms domain="hep">
            <topic name="actions" dict="id,parent:i,created:t" primaryKey="id" store="actions.db">
                <pub src="ws://example.com/logs/ws" protocol="dblog" /><sub dest="logAnalysis" protocol="json" query="all=all" timeout="100000" />
            </topic>...</ms>
    </xmlSample>
    <p>In the example above, we use dblog and web socket to synchornise the remote data to the topic. Next, we define a subscriber and it can perform queries on the topic db. and it will do so for every 100 seconds (timeout in milliseconds)</p>
    <p>Next, we define a map reducer processor:</p>
    <xmlSample>
        <mapReducer name="logAnalysis">
            <script src="" />
            <filter src="" />
            <processor src="..."></processor>
        </mapReducer>
    </xmlSample>
    <p>The script element allows one to import any external scripts. You can either load it by specifying an external URI in the src attribute or embed it in the element itself.</p>
    <p>The filter element must define a pass function:</p> <pre>
  <![CDATA[
     function pass(data) {
       return data.parent==1;
     }
  ]]>
         
     </pre>

    <p>Only data that passes the check will be processed by the processor.</p>
    <p>The processor has the following JavaScript. You can either embed it inside the processor element or use it externally using the src attribute.</p> <pre>
  <![CDATA[
	   
 (function() {

    var m1 = {};

	//use parent as the grouping key
    function map(data) {
	    print(data);
        var a = m1[data.parent] || [];
        a.push(1);
        m1[data.parent] = a;
    }

    function reduce(key, values) {
        var c = 0;
        values.forEach(function(v) {
            c += v;
        });
        return {
            'id': key,
            'viewNum': c
        };
    }


    function process() {
        var o = [];
        for (var key in m1) {
            o.push(reduce(key, m1[key]));
        }
        return o;
    }

    return {
        map: map,
        process: process
    };

}());
	   ]]>
        
    </pre>

    <p>Things to watch out:</p>
    <ol>
        <li>The name is the same as the dest defined in the previous subscriber so it can send JSON messages to the map reducer.</li>
        <li>The processor element embed a JS directly here or using an external one by using the src attribute.</li>
        <li>The processor JavaScript must define a map function and a process function. The map function will be called whenever a message arrives. The process function will be called when all messages have been mapped. The process function must return an array
            of objects and each object must correspond go the final form one needs to send to its final destination.</li>
    </ol>
    <p>Finally, we define:</p>
    <xmlSample>
        <topic name="resources" primaryKey="id" dict="id,viewNum:i" store="resources.db">
            <pub src="logAnalysis" protocol="binary" /><sub dest="http://{remote address}" protocol="json" />
        </topic>
    </xmlSample>
    <p>In the subscriber here, its src attribute was set to the name of the map reducer so it gets messages from the map reducer. Finally, we post each message to the remote desintation using JSON.</p>
     <h4>How to debug your map reduce app?</h4>

    <p>The easiest way is to develop the app using XPE IDE and create a blank page to include core.js and your map reduce js.</p>