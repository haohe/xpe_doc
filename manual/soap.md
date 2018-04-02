<h2>XPE SOAP Introduction</h2>

<p>XPE SOAP wraps SOAP Web Services as JSON based services. XPE SOAP will convert a JSON request into SOAP XML request and invoke the remote SOAP Web Services accordingly. SOAP XML responses will be converted into JSON responses as well for easy consumption.</p>

<h3>Configurations</h3>

<table class="table table-striped">
 <tr>
            <th>Attribute</th>
            <th>Description</th>
            <th>Example</th>
        </tr>
        <tr>
            <td>ns</td>
            <td>The name space of the request payload</td>
            <td>The simplest form would be ns="http://example.com". To declare multiple namespaces, ns="xmlns:n1='http://namesapce1.com' xmlns:n2='http://namespace2.com'"</td>
        </tr>
        <tr>
            <td>endPoint</td>
            <td>The actual SOAP service address</td>
            <td></td>
        </tr>
        <tr>
            <td>wsdl</td>
            <td>The path to the WSDL file</td>
            <td>The URL address to the remote WSDL file.</td>
        </tr>
    </table>

<h3>Rules for converting JSON to XML</h3>
    	
<p>In the following examples, SOAP envolopes are removed for simplicity.  Each request will be embedded into SOAP automatically.</p>

<table class="table table-striped">
        <tr>
            <th>Rule</th>
            <th>JSON</th>
            <th>XML</th>
        </tr>
        <tr>
            <td>JSON Objects will be converted into XML elements</td>
            <td>
```json
                        {"name":"value"}
```
            </td>
            <td>
```xml
                    <name>value</name>
```
            </td>
        </tr>
        <tr>
            <td>Namespace with prefix can be declared.  </td>
            <td>
            xmlns:n1='http://namesapce1.com'
```json
                    {"n1:name":"value"}
```
            </td>
            <td>
```xml                    <n1:name xmlns:n1='http://namesapce1.com'>value</n1:name>
```            </td>
        </tr>
        <tr>
            <td>Namespace with prefix can be declared.  </td>
            <td>
            xmlns:n1='http://namesapce1.com'
```json
{"n1:name":"value"}
```
        </td>
            <td>
```xml
                    <n1:name xmlns:n1='http://namesapce1.com'>value</n1:name>
```
            </td>
        </tr>
        <tr>
            <td>Nested objects are converted into parent-child elements  </td>
            <td>
 ```json
{"queryStbInfo":{"in0":"?","in1":"?"}}
```
            </td>
            <td>
```xml
                    <queryStbInfo><in0>?</in0><in1>?</in1></queryStbInfo>
```
            </td>
        </tr>
        <tr>
            <td>Arrays are converted into a parent with repeating child elements</td>
            <td>
                
```json
{
    "ser:calculateProductsFee": {
        "ser:in0": "?",
        "ser:in1": {
            "dto:OrderProductInfo": [
                {
                    "dto:orderCycles": "?",
                    "dto:productCode": "?",
                    "dto:productId": "?"
                },
                {
                    "dto:orderCycles": "?",
                    "dto:productCode": "?",
                    "dto:productId": "?"
                }
            ]
        }
    }
}                
```
            </td>
            <td>
```xml
<ser:calculateProductsFee xmlns:ser="http://service.call.sms.star.com" xmlns:dto="http://dto.service.call.sms.star.com"><ser:in0>?</ser:in0><ser:in1><dto:OrderProductInfo><dto:orderCycles>?</dto:orderCycles><dto:productCode>?</dto:productCode><dto:productId>?</dto:productId></dto:OrderProductInfo><dto:OrderProductInfo><dto:orderCycles>?</dto:orderCycles><dto:productCode>?</dto:productCode><dto:productId>?</dto:productId></dto:OrderProductInfo></ser:in1></ser:calculateProductsFee>                    <queryStbInfo><in0>?</in0><in1>?</in1></queryStbInfo>
```
</td>
        </tr>
    </table>
    
    
<h3>Rules for converting SOAP to JSON</h3>
    	

<table class="table table-striped">
        <tr>
            <th>Rule</th>
            <th>SOAP</th>
            <th>JSON</th>
        </tr>
        <tr>
            <td>XML elements will be converted into JSON objects and SOAP envelop will be removed</td>
            <td>
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ser="http://service.call.sms.star.com" xmlns:out="http://outter.model.sms.star.com">
   <soapenv:Header/>
   <soapenv:Body>
      <ser:queryBillOfCustomerResponse>
         <ser:out>
            <!--Zero or more repetitions:-->
            <out:Bill>
               <!--Optional:-->
               <out:accountCode>?</out:accountCode>
               <!--Optional:-->
               <out:accountItemType>?</out:accountItemType>
               <!--Optional:-->
               <out:accountName>?</out:accountName>
               <!--Optional:-->
               <out:billCycle>?</out:billCycle>
               <!--Optional:-->
               <out:billState>?</out:billState>
               <!--Optional:-->
               <out:business>?</out:business>
               <!--Optional:-->
               <out:createDate>?</out:createDate>
               <!--Optional:-->
               <out:discountFee>?</out:discountFee>
               <!--Optional:-->
               <out:fee>?</out:fee>
               <!--Optional:-->
               <out:id>?</out:id>
               <!--Optional:-->
               <out:originalFee>?</out:originalFee>
               <!--Optional:-->
               <out:serviceCode>?</out:serviceCode>
               <!--Optional:-->
               <out:writeOffState>?</out:writeOffState>
            </out:Bill>
         </ser:out>
      </ser:queryBillOfCustomerResponse>
   </soapenv:Body>
</soapenv:Envelope>
```
            </td>
            <td>
```json
{
    "ser:queryBillOfCustomerResponse": {
        "ser:out": {
            "out:Bill": [
                {
                    "out:accountCode": "?",
                    "out:accountItemType": "?",
                    "out:accountName": "?",
                    "out:billCycle": "?",
                    "out:billState": "?",
                    "out:business": "?",
                    "out:createDate": "?",
                    "out:discountFee": "?",
                    "out:fee": "?",
                    "out:id": "?",
                    "out:originalFee": "?",
                    "out:serviceCode": "?",
                    "out:writeOffState": "?"
                }
            ]
        }
    }
}
```
            </td>
        </tr>
        
<tr>
<td>Repeating XML elements will be converted into JSON array if the schema indicates so</td>
<td>
```xml                
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <soap:Body>
        <queryProductsOrderHistoryResponse xmlns="http://service.call.sms.star.com">
            <out>
                <ns1:ProductOrderInfo xmlns:ns1="http://dto.service.call.sms.star.com">
                    <belongedPackage xmlns="http://dto.service.call.sms.star.com" xsi:nil="true" />
                    <billEndDate xmlns="http://dto.service.call.sms.star.com" xsi:nil="true" />
                    <billStartDate xmlns="http://dto.service.call.sms.star.com">20141001000000</billStartDate>
                    <createDate xmlns="http://dto.service.call.sms.star.com">20140901090710</createDate>
                    <pricePlan xmlns="http://dto.service.call.sms.star.com">0002普通客户邯郸价格(主机)--26元</pricePlan>
                    <pricePlanDes xmlns="http://dto.service.call.sms.star.com">0002普通客户邯郸价格(主机)--26元</pricePlanDes>
                    <productCode xmlns="http://dto.service.call.sms.star.com">04010002</productCode>
                    <productName xmlns="http://dto.service.call.sms.star.com">邯郸主机基本收视维护费-0002</productName>
                    <serviceOpenDate xmlns="http://dto.service.call.sms.star.com">20140901000000</serviceOpenDate>
                    <serviceStopDate xmlns="http://dto.service.call.sms.star.com" xsi:nil="true" />
                    <state xmlns="http://dto.service.call.sms.star.com">1</state>
                </ns1:ProductOrderInfo>
                <ns1:ProductOrderInfo xmlns:ns1="http://dto.service.call.sms.star.com">
                    <belongedPackage xmlns="http://dto.service.call.sms.star.com">邯郸_哥俩好套餐(998元)</belongedPackage>
                    <billEndDate xmlns="http://dto.service.call.sms.star.com">20150831235959</billEndDate>
                    <billStartDate xmlns="http://dto.service.call.sms.star.com">20140901000000</billStartDate>
                    <createDate xmlns="http://dto.service.call.sms.star.com">20140901090717</createDate>
                    <pricePlan xmlns="http://dto.service.call.sms.star.com">环球影院动感影院视尚影院一年包-100元</pricePlan>
                    <pricePlanDes xmlns="http://dto.service.call.sms.star.com">环球影院动感影院视尚影院一年包-100元</pricePlanDes>
                    <productCode xmlns="http://dto.service.call.sms.star.com">0200005015</productCode>
                    <productName xmlns="http://dto.service.call.sms.star.com">套餐子产品-环球、动感、视尚影院一年包-100元</productName>
                    <serviceOpenDate xmlns="http://dto.service.call.sms.star.com">20140901000000</serviceOpenDate>
                    <serviceStopDate xmlns="http://dto.service.call.sms.star.com">20150831235959</serviceStopDate>
                    <state xmlns="http://dto.service.call.sms.star.com">0</state>
                </ns1:ProductOrderInfo>
                <ns1:ProductOrderInfo xmlns:ns1="http://dto.service.call.sms.star.com">
                    <belongedPackage xmlns="http://dto.service.call.sms.star.com">邯郸_哥俩套餐(998元)</belongedPackage>
                    <billEndDate xmlns="http://dto.service.call.sms.star.com">20160930235959</billEndDate>
                    <billStartDate xmlns="http://dto.service.call.sms.star.com">20141001000000</billStartDate>
                    <createDate xmlns="http://dto.service.call.sms.star.com">20140901090717</createDate>
                    <pricePlan xmlns="http://dto.service.call.sms.star.com">498元收看24个月-498元</pricePlan>
                    <pricePlanDes xmlns="http://dto.service.call.sms.star.com">498元收看24个月-498元</pricePlanDes>
                    <productCode xmlns="http://dto.service.call.sms.star.com">0200004044</productCode>
                    <productName xmlns="http://dto.service.call.sms.star.com">498元收看24个月-4044</productName>
                    <serviceOpenDate xmlns="http://dto.service.call.sms.star.com">20140901000000</serviceOpenDate>
                    <serviceStopDate xmlns="http://dto.service.call.sms.star.com" xsi:nil="true" />
                    <state xmlns="http://dto.service.call.sms.star.com">1</state>
                </ns1:ProductOrderInfo>
                <ns1:ProductOrderInfo xmlns:ns1="http://dto.service.call.sms.star.com">
                    <belongedPackage xmlns="http://dto.service.call.sms.star.com" xsi:nil="true" />
                    <billEndDate xmlns="http://dto.service.call.sms.star.com">20150831235959</billEndDate>
                    <billStartDate xmlns="http://dto.service.call.sms.star.com">20140901000000</billStartDate>
                    <createDate xmlns="http://dto.service.call.sms.star.com">20140901090725</createDate>
                    <pricePlan xmlns="http://dto.service.call.sms.star.com">2320赠送凤凰两套一年包-0元</pricePlan>
                    <pricePlanDes xmlns="http://dto.service.call.sms.star.com">2320赠送凤凰两套一年包-0元</pricePlanDes>
                    <productCode xmlns="http://dto.service.call.sms.star.com">0200002320</productCode>
                    <productName xmlns="http://dto.service.call.sms.star.com">赠送凤凰两套一年包-2320</productName>
                    <serviceOpenDate xmlns="http://dto.service.call.sms.star.com">20140901000000</serviceOpenDate>
                    <serviceStopDate xmlns="http://dto.service.call.sms.star.com">20150831235959</serviceStopDate>
                    <state xmlns="http://dto.service.call.sms.star.com">0</state>
                </ns1:ProductOrderInfo>
            </out>
        </queryProductsOrderHistoryResponse>
    </soap:Body>
</soap:Envelope>
```
            </td>
            <td>
```json
{
    "queryProductsOrderHistoryResponse": {
        "out": {
            "ns1:ProductOrderInfo": [
                {
                    "belongedPackage": null,
                    "billEndDate": null,
                    "billStartDate": "20141001000000",
                    "createDate": "20140901090710",
                    "pricePlan": "0002普通客户邯郸价格(主机)--26元",
                    "pricePlanDes": "0002普通客户邯郸价格(主机)--26元",
                    "productCode": "04010002",
                    "productName": "邯郸主机基本收视维护费-0002",
                    "serviceOpenDate": "20140901000000",
                    "serviceStopDate": null,
                    "state": "1"
                },
                {
                    "belongedPackage": "邯郸_哥俩好套餐(998元)",
                    "billEndDate": "20150831235959",
                    "billStartDate": "20140901000000",
                    "createDate": "20140901090717",
                    "pricePlan": "环球影院动感影院视尚影院一年包-100元",
                    "pricePlanDes": "环球影院动感影院视尚影院一年包-100元",
                    "productCode": "0200005015",
                    "productName": "套餐子产品-环球、动感、视尚影院一年包-100元",
                    "serviceOpenDate": "20140901000000",
                    "serviceStopDate": "20150831235959",
                    "state": "0"
                },
                {
                    "belongedPackage": "邯郸_哥俩套餐(998元)",
                    "billEndDate": "20160930235959",
                    "billStartDate": "20141001000000",
                    "createDate": "20140901090717",
                    "pricePlan": "498元收看24个月-498元",
                    "pricePlanDes": "498元收看24个月-498元",
                    "productCode": "0200004044",
                    "productName": "498元收看24个月-4044",
                    "serviceOpenDate": "20140901000000",
                    "serviceStopDate": null,
                    "state": "1"
                },
                {
                    "belongedPackage": null,
                    "billEndDate": "20150831235959",
                    "billStartDate": "20140901000000",
                    "createDate": "20140901090725",
                    "pricePlan": "2320赠送凤凰两套一年包-0元",
                    "pricePlanDes": "2320赠送凤凰两套一年包-0元",
                    "productCode": "0200002320",
                    "productName": "赠送凤凰两套一年包-2320",
                    "serviceOpenDate": "20140901000000",
                    "serviceStopDate": "20150831235959",
                    "state": "0"
                }
            ]
        }
    }
}  
```
            </td>
        </tr>
        
    </table>    
    
    
