<h2>XPE DB Introduction</h2>

<h3>What is XPE DB and how it works?</h3>

<p>XPE DB is designed from ground up using an event driven and log based architecture, which allows fast updates up to the maximum IO bandwidth of the underlying hardware storage. Even wiht modest hardware, XPE DB can achieve one million updates per seconds.</p>
<p>Inside XPE DB, even update generates a DB update event, which is published to a number of listeners. Some of those listeners are speciality indexers.</p>
<p>Search is performed by one or more indexers. To achieve the best performance, one should consider the usage of a DB and then chooses the most suitable index.</p>
<h3>Data model</h3>

<p>XPE DB stores records. A record has one more fields. Each field can be considered as a name value pair. The following data types are supported:</p>
<h3>Data types and schema</h3>
     
<p>XPE DB supports a flexible and extensible schema.  You can add more fields at the end or rename a field.  However, you cannot remove an existing field. </p>

<p>XPE DB supports the following data types (all values are inclusive) with string type as the default type:</p>

<table class="table table-striped">
        <tr>
            <th>Name</th>
            <th>Size (byte)</th>
            <th>Abbreviation</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>byte</td>
            <td>1</td>
            <td>b</td>
            <td>-128 to 127. '1' will be parsed and stored as 1</td>
        </tr>
        <tr>
            <td>Unsiged byte</td>
            <td>1</td>
            <td>B</td>
            <td>0 to 255</td>
        </tr>
        <tr>
            <td>short</td>
            <td>2</td>
            <td>h</td>
            <td>-32,768 to 32,767</td>
        </tr>
        <tr>
            <td>unsigned short</td>
            <td>2</td>
            <td>H</td>
            <td>0 to 65535</td>
        </tr>
        <tr>
            <td>int</td>
            <td>4</td>
            <td>i</td>
            <td>-2^31 to 2^31-1</td>
        </tr>
        <tr>
            <td>unsigned int</td>
            <td>4</td>
            <td>I</td>
            <td>0 to 2^32-1</td>
        </tr>
        <tr>
            <td>date</td>
            <td>4</td>
            <td>d</td>
            <td>YYYYMMDD format stored as int</td>
        </tr>
        <tr>
            <td>long</td>
            <td>8</td>
            <td>l</td>
            <td>-2^63 to 2^63-1</td>
        </tr>
        <tr>
            <td>timestamp</td>
            <td>8</td>
            <td>t</td>
            <td>0 to 2^63-1, milliseconds since the 19700101</td>
        </tr>
        <tr>
            <td>String</td>
            <td>upto 1^16-2 bytes</td>
            <td>s</td>
            <td>Binary blobs. Treated as is.</td>
        </tr>
        <tr>
            <td>UTF8 String</td>
            <td>upto 1^16-2 bytes</td>
            <td>u</td>
            <td>UTF8 encoded string</td>
        </tr>
        <tr>
            <td>JSON</td>
            <td>upto 1^16-2 bytes</td>
            <td>j</td>
            <td>UTF8 encoded JSON</td>
        </tr>
    </table>
    <p>Schemas of a DB can be defined using the following notation:</p> <pre>
        {field name}[:type][![default values]][,{fieldname}:[type][![default values]]]
    </pre>
    
    <p>So a schema consists of at least one field and up to 65535 fields can be defined.  However, any db with more than 128 fields will be very unusual and is strongly discouraged. </p>
    <p>A field must have a name and optionally a type.  If not specified, the type is defaulted to 's'.  If a field ends with '!', then this field is required.  Default value of a field can be defined
        after '!'.  Default values are useful when extending a schema and one wants to give all records of a new field with the same value.  
    </p>

    <p>For example, the following schema is used for storing user comments:</p>
    <xmlSample>
        <services dict="id:i,created:t,parent:I!,state:b!1,author,comments"></services>
    </xmlSample>
    <p>In the example above, each comment has a id (int type), a created time (timestamp type), a parent reference (unsigned int type), a state (byte type), author and comments. Both author and comments are binary blobs.
        The parent ends with a ! so the field is required.  The state field has a value of 1 after the ! so its default value is 1.
    </p>
     <h3>Limits of XPE DB</h3>

    <table class="table table-striped">
        <tr>
            <th>Description</th>
            <th>Limit</th>
        </tr>
        <tr>
            <td>Maximum number of records</td>
            <td>2^31-1</td>
        </tr>
        <tr>
            <td>Maximum record size</td>
            <td>64K bytes</td>
        </tr>
        <tr>
            <td>Maximum number of keys</td>
            <td>16384. Will be less subject to the usages of key values. Each key requires 4 bytes minimum.</td>
        </tr>
    </table>
     <h3>Unique Key DB</h3>

    <p>XPE DB supports several different types of DB and the most common one is a Unique Key DB. This type of DB must have a primary key for each record and each key must be unique.</p>
     <h4>Special fields</h4>

    <p>The following fields carry special meaning and if a dict contains them, they will be handled according to the following table:</p>
    <table class="table table-striped">
        <tr>
            <th>Name</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>created:l</td>
            <td>If defined, the field will be populated by the current time in millis when the record is created</td>
        </tr>
        <tr>
            <td>lastUpdated:l</td>
            <td>If defined, the field will be populated by the current time in millis when the record is updated</td>
        </tr>
        <tr>
            <td>access:b</td>
            <td>If present, this field defines the access control policy for the given record</td>
        </tr>
        <tr>
            <td>ownerId:i</td>
            <td>If present, its value is set to the userid of the owner to which this record belongs</td>
        </tr>
        <tr>
            <td>groupId:i</td>
            <td>If present, its value is set to the group id of the group to which this record belongs, currently, only the first 64 are supported. It will be extended to support more groups.</td>
        </tr>
    </table>
     <h4>Access Control</h4>

    <p>System supports up to 63 system groups.</p>
    <p>A user can belong to 0 or more groups.</p>
    <p>A user can be uniquely identified by a username, userId (int), or an email.</p>
    <p>A DB is owned by a group. A DB cannot be owned by an individul user.</p>
    <p>A record in a DB or a DB itself can define its own access control policy. An access control policy is defined by six chars using the 'rw' notation: '[owner][group][other]'. For example, 'rw----' means that the resource is read and write accessible by
        the owner only.</p>
    <p>To enable access control on a DB, one first defines the access policy for the DB by adding the access:b to the dictionary</p>
    
    <p>The meaning of read and write access are explained below:</p>
    

    <table class="table table-striped">
        <tr>
            <th>Access</th>
            <th>Target</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Read</td>
            <td>Record</td>
            <td>Can read the individual record</td>
        </tr>
        <tr>
            <td>Write</td>
            <td>Record</td>
            <td>Can modify and delete the individual record</td>
        </tr>
        <tr>
            <td>Read</td>
            <td>DB</td>
            <td>Can search records within the DB so metadata of each record may be visible to the given user who has read access to a DB. By combining filter mask, it possible
                to hide certain information from a group of users.
            </td>
        </tr>
        <tr>
            <td>Write</td>
            <td>DB</td>
            <td>Can add a new record to the DB. Deletion of an individual record is subject to the access policy of each individual record.</td>
        </tr>
    </table>       
    
    <h4>Extended Group</h4>

    <p>There are only up to 63 system groups, which are normally predefined during the development time. Extended groups can be created at runtime subject to group creation access control policy.
        Extended groups are defined in a group db.  
    </p>
    
    <p>Graph DB Dict</p>
        <table class="table table-striped">
        <tr>
            <th>Field</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>id</td>
            <td>system allocated unique id</td>
        </tr>
        <tr>
            <td>type:b</td>
            <td>type of this link. 0-create. 1-has. </td>
        </tr>
        <tr>
            <td>access:b</td>
            <td>Access of this group</td>
        </tr>
        <tr>
            <td>src:i</td>
            <td>The id of the source</td>
        </tr>
        <tr>
            <td>srcTitle</td>
            <td>The title of the source for display</td>
        </tr>
        <tr>
            <td>target:i</td>
            <td>The id of the target</td>
        </tr>
        <tr>
            <td>targetTitle</td>
            <td>Short description of the link</td>
        </tr>
        <tr>
            <td>ownerId:i</td>
            <td>Whoever creates this link</td>
        </tr>
        <tr>
            <td>created:l</td>
            <td>The created timestamp</td>
        </tr>
        <tr>
            <td>lastUpdated:l</td>
            <td>The lastUpdated timestamp</td>
        </tr>
        </table>
        
    
    
    <p>Operations on a group</p>
        <table class="table table-striped">
        <tr>
            <th>Operation</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Create</td>
            <td>Can read the individual record</td>
        </tr>
        <tr>
            <td>Join</td>
            <td>Join group</td>
        </tr>
        </table>

    
    <p>Sample configuration of a DB:</p>
    
    <xmlSample>
       <service access="rwrw-w" recAccess="rwrw--" group="staff" store="issues.db" storeType="binary" primaryKey="id" seqKey='true' sorted="id"  dict="id,projId,parentId,title,description,access:b,createDate:t" />
    </xmlSample>
    
    <p>Configuration explained:</p>
    
    <table class="table table-striped">
        <tr>
            <th>Name</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>access</td>
            <td>Using the rw format, this config defines the access policy for the DB. </td>
        </tr>
        <tr>
            <td>recAccess</td>
            <td>The access policy for each record in the DB</td>
        </tr>
        <tr>
            <td>group</td>
            <td>The group of the DB.  Its value must be one of those groups defined in the users db.</td>
        </tr>
    </table>    
    
    <p>Write right to DB means one can add a new record to the DB.</p>
    <p>Read right to DB means one can search </p>
    
    <p>Note that we define an "access" attribtue for the DB.  As it is defined, this DB allows all users in group "1" to be able to add to this DB.  If group is not 
        defined, then everyone can write records to this DB.  Everyone can read the DB.
    </p>
    
    <p>When a record is added to the DB, it inherits the access policy and the groupId of the DB.</p>


    <ol>
        <li>The dict of a DB must include access:b and ownerId:i and optionally groupId:i. Those fields are used by the DB to bookkeep access control information</li>
        <li>Define the groupId of the DB. A DB</li>
        <li>Define the access policy in the DB config with defaultAccessPolicy="rwrwrw" format. The first two chars for owner, the second for group, and the rest is for others.</li>
    </ol>
     <h4>Indices</h4>

    <p>A DB is not very useful if one cannot search the DB. What makes XPE DB different is that we assume that a developer knows her data so she will be able to choose the most suitable index for it. The end result is that XPE DB is able to perform very fast
        queries typically at the cost of O(1) time. The performance is so high that traditional SQL based DB simply cannot match.</p>
    <p>The following indices are supported:</p>
    <table class="table table-striped">
        <tr>
            <th>Name</th>
            <th>Description</th>
            <th>Usage</th>
            <th>Example</th>
        </tr>
        <tr>
            <td>Inverted index</td>
            <td>This is the most common index. Most suitable if the data set has a relatively small number of possible values and each value can potentially have a number of records associated with it. Do not use this index if each value is unique. For example, do not
                use it to index a username when you design a user db because each username only one record. On the other hand, it is the perfect index to index a tag for your records.</td>
            <td>fields="{field}..."</td>
            <td>fields="tag,state"</td>
        </tr>
        <tr>
            <td>Sorted Inverted index</td>
            <td>If you want to find out a list of records tagged with Java sorted by ratings, this index allows you to find out the answer instantly. The notation is to add the field it will sort on after a ^ notation. In the example we just mentioned, this becomes
                tag^ratings.</td>
            <td>fields="{field^sortfield}..."</td>
            <td>fields="tag^ratings"</td>
        </tr>
    </table>
     <h4>How to use</h4>

    <p>In your sitemap.xml, add the following:</p>
    <xmlSample>
        <service store="issues.db" storeType="binary" primaryKey="id" seqKey='true' sorted="id" fields="title,owner,projId^id,parentId,submitter,state^id" dict="id,projId,parentId,title,description,owner,difficulty:b,submitter,type:b,state:b,priority:b,createDate:t">
            <get path="/json/issue" xpipe="http://www.xmlpipe.org/xpe/db/unique/record/get" /><del path="/json/issue" xpipe="http://www.xmlpipe.org/xpe/db/unique/record/del" />
            <post path="/json/issues" xpipe="http://www.xmlpipe.org/xpe/db/unique/record/post" />
            <get path="/json/issues" xpipe="http://www.xmlpipe.org/xpe/db/search" mask="id,projId,parentId,title,description,owner,difficulty,submitter,type,state,priority" />
        </service>
    </xmlSample>
    <p>At the top level, we have defined common attributes to be shared by all the pipes. We then defined 4 pipes:
        <ol>
            <li>The get pipe is used for getting the details of one record so you supply the id of the record to get it back.</li>
            <li>The del pipe is used for deleting a record</li>
            <li>The post pipe is used for creating a record, one simply supplies a valid JSON file to add a new record. If a id is supplied, then the existing record is replaced. If no id is supplied, then a new id is generated by the system.</li>
            <li>The search pipe is used for advanced searches.</li>
        </ol>
    </p>
