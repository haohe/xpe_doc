<h2>XPE JSON Web Token Specification</h2>
  
<h3>Introduction</h3>
  
<p>XPE JSON Web Token (XJWT) is a simple and reliable cross domain SSO solution based on the popular JSON Web Token (JWT) approach.  However, it is not compatiable with the JWT defined in RFC 7519.
      The reason that we need to define this new specification is because the RFC 7519 is not secure and too verbose.  
  </p>
  
  <h3>The overall structure of XJWT </h3>
  
  <p>An XJWT has three parts: header, payload, and signature separated by two '.'.   </p>

  <h4>The header</h4>
  
  <p>The header has the following raw structure:</p>
  
  <pre>
      [expiry:long][type:byte][issuer id:long]
  </pre>
  
<table class="table table-striped">
        <tr>
            <th>Attribute</th>
            <th>Description</th>
            <th>Type and Values</th>
        </tr>
        <tr>
           <td>Expiry</td>
           <td>The expiry date in milliseconds .</td>
           <td>Long. Big endian.</td>
        </tr>
        <tr>
           <td>Type</td>
           <td>The type of the payload</td>
           <td>Byte. 0 - reserved.  1 - JSON. 2-SYS </td>
        </tr>
        <tr>
           <td>Issuer Id</td>
           <td>A unique id that uniquely identifies an issuer</td>
           <td>A ID must be possitive.  0 - 1000 are reserved. </td>
        </tr>
    </table>
    
<p>The raw strcutre is then base64 encoded</p>
    
<h4>The payload </h4>
    

<p>The payload consists of the following: </p>
    
<pre>
       aes256(random long + body + aes padding, aes key)
</pre>
    
<p>where aes256 is the AES256 encryption function, the random long is a randomly generated 8 byte long, body is the business data to be sent over, aes padding are additional characters for AES padding
        purpose.  The type defined previously indicates the structure of the body.
</p>

<p> The aes256 function is typically created in Java:</p>
 
```java
 cipher = javax.crypto.Cipher.getInstance("AES/CBC/NoPadding");
```

<p>Padding is calculated using the following formula:

```
  byte padding = (byte)  ((16 - (( 8 + body length + 1) & 0xF)) & 0xF);
```

<p>So, suppose we have a message body whose length is 7, padding is 0 according to the above formula. Hence, we pad the output by one 0 and the data layout will look like: </p>
  
```
 [rand number: 8 bytes][body: 7 bytes][0] = 16 bytes
```

<p>If the body length is 16, then padding is  7.   Hence, we pad the output with eight 7. the data layout will be: </p>

```
[rand number: 8 bytes][body: 16 bytes][7,7,7,7,7,7,7,7] = 32 bytes
```

<p>The raw strcutre is then base64 encoded</p>

<h4>The signature</h4> 
    
<p>The signature is computed as the following:</p>
    
<pre>
        base64(HmacSHA256(based64(raw header) +'.' + base64 (raw payload), secret key))
</pre>
    
<p>One usually obtains a HmacSHA256 function in Java using the following code:
  
```java  
  sha256_HMAC = javax.crypto.Mac.getInstance("HmacSHA256")
```

<h3>Verification and decryption </h3>
    
<ol>
        <li>The signature is vefified with the shared secret key, if not valid, the token is discarded.</li>
        <li>The header is base64 decoded and the expiry time is compared with the current time.  If it has elapsed, then the token is discarded. </li>
        <li>The header version is read and if it is not supported, then the token is invalid.</li>
        <li>The payload is based64 decoded and decrypted using the shared aes key. The first 8 bytes of the decrypted messages and the paddings are discarded. The value of the last byte is read. Note that the value of the last byte incides the number of paddings we need to further remove.  For example, if the value is 8, we then need to remove the last 9 bytes. The remaining data is returned.</li>
</ol>
    
<h3>Body data structure when type is 1 (JSON) </h3>
    
<table class="table table-striped">
        <tr>
            <th>Attribute</th>
            <th>Description</th>
            <th>Type and Values</th>
            <th>Required</th>
        </tr>
        <tr>
           <td>ti</td>
           <td>Timestamp</td>
           <td>Long. In milliseconds. </td>
           <td>N</td>
        </tr>
        <tr>
           <td>id</td>
           <td>User id</td>
           <td>Int</td>
           <td>N</td>
        </tr>
        <tr>
           <td>un</td>
           <td>username</td>
           <td>string</td>
           <td>Y</td>
        </tr>
        <tr>
           <td>em</td>
           <td>email</td>
           <td>string</td>
           <td>Y</td>
        </tr>
        <tr>
           <td>ph</td>
           <td>phone number</td>
           <td>string</td>
           <td>N</td>
        </tr>
        <tr>
           <td>dis</td>
           <td>Display name</td>
           <td>string</td>
           <td>N</td>
        </tr>
</table>

### Body data structure when type is 2 (SYS) 

 If not agreed previously, the body is simply three bytes: 'SYS' or pre-agreed format between parties.
 
