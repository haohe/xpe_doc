<h2>XPE DB Sync Protocol Introduction</h2>
    	
<p>This speclification defines the data synchronisation protocol used bewtween a client (data source) and an XPE server. </p>
    	
<p>The main mechanism is "push", where the client pushes data to the server whenever a new data is available.</p>
    	
<h2>REST interfaces</h2>
    	
<p>The XPE server must provide a URI which will uniquely identifies the data synchornisation service to be used by a client. A different URI will
    	    imply a different service and the implied data format will be specific to that service.
</p>
    	
<h3>GET</h3>
<p>Each URI should provide a GET service so when HTTP GET request is received, it must return the following message:</p>
    	
<pre>{"acked":$number}</pre>
    	
<p>Where $number is the  sequence number of the last message the service has received. </p>
    	
<h3>POST</h3>
    	
<h4>Update a data record</h4>
    	
<p>A client will need to send any data update to the service using HTTP POST with the following format:</p>
    	
<pre>[{"seq":$number,"upd":{ ...}}]</pre>
    	
<p>where $number is the non-negative sequence number starting from 1. The body of upd is specific to the data record, typically a list of key-value pairs.</p>
    	
<p>The server will respond:</p>
    	
<pre>{"acked":$number}</pre>

<p>Where $number is the  sequence number of the last message the service has received. </p>
    	
<h4>Delete a data record </h4>

<p>A client will need to send any data update to the service using HTTP POST with the following format:</p>
    	
<pre>[{"seq":$number,"del":{ ...}}]</pre>
    	
<p>where $number is the non-negative sequence number starting from 1. The body of upd is specific to the data record, typically a list of key-value pairs used to
    	identify the record.</p>
<p>The server will respond:</p>
    	
<pre>{"acked":$number}</pre>

<p>Where $number is the  sequence number of the last message the service has received. </p>
    	
<h3>Errors</h3>
    	
<p>The returned messages from the server will optionally contain a code and a msg attribute. When an error occurs, the server will stop receiving
    	further messages until the error is fixed.
</p>
    	
<pre>{"acked":$number, "code":1, "msg":"invalid json"}</pre>
    	
<p>where the acked sequence number is the last successfully processed message.  So the message sequence number that caused the error would be the acked number plus one.</p>
    	


