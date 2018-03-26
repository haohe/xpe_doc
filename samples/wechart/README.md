## 微信

- 准备工作
    + 参考微信公众平台技术文档[入门指引](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1472017492_58YV5)
    + 微信公众平台里的需要 https 访问，但目前 XPE 不支持 https， 可采用 nginx 做一个反向代理 
    + 在微信平台服务器的配置中的URL 应为 https://your_xpe_domain/wechat
    + XPE 的 sitemap 中配置微信服务, 注意下列属性的配置要和微信平台保持一致：
        + token="token" 
        + appid="wxappid" 
        + secret="wxsecret" 
        + openId="wx_original_id"
    
- XPE 的 sitemap 中配置微信服务

```
        <service token="token" appid="wxappid" secret="wxsecret" openId="wx_original_id">
            <!--used by wechat server to post events -->
            <post path="/wechat" xpipe="http://www.xmlpipe.org/xpe/wechat/notify" notify="http://www.xmlpipe.org/xpe/wechat/updateUser" />
            <get path="/wechat" xpipe="http://www.xmlpipe.org/xpe/wechat/verify" />
            
            <!--you can search by MsgId,FromUserName,MsgType,Event,EventKey -->
            <get path="/json/event" xpipe="http://www.xmlpipe.org/xpe/wechat/event/get" store="eventStore.db" />
            
            <!--User info store -->
            <get path="/json/userinfo" xpipe="http://www.xmlpipe.org/xpe/wechat/userinfo/get" />
            <get path="/json/userinfo/search" xpipe="http://www.xmlpipe.org/xpe/wechat/userinfo/search" />
            
            <!-- this pipe must be placed first -->
            <get path="/wechat/reply/rule" xpipe="http://www.xmlpipe.org/xpe/wechat/rules/get" />
            <post path="/wechat/reply/rules" xpipe="http://www.xmlpipe.org/xpe/wechat/rules/update" />
            <get path="/wechat/reply/rules" xpipe="http://www.xmlpipe.org/xpe/wechat/rules/search" />
            <del path="/wechat/reply/rule/del" xpipe="http://www.xmlpipe.org/xpe/wechat/rules/del" />
            
            <!-- award rules -->
            <post path="/wechat/awardrules" xpipe="http://www.xmlpipe.org/xpe/wechat/awardrules/update" />
            <get path="/wechat/awardrule" xpipe="http://www.xmlpipe.org/xpe/wechat/awardrules/get" />
            <del path="/wechat/awardrule" xpipe="http://www.xmlpipe.org/xpe/wechat/awardrules/del" />
            <get path="/wechat/awardrules" xpipe="http://www.xmlpipe.org/xpe/wechat/awardrules/search" />
            
            <!--menu-->
            <post path="/menu" xpipe="http://www.xmlpipe.org/xpe/wechat/menu" />
            
            <!--pay-->
            <post path="/mmpay" total_amount="200" maxNumberOfPays="2" startTime="10:00" endTime="22:00" mch_id="9994440" client_ip="10.29.10.13" key="key" xpipe="http://www.xmlpipe.org/xpe/wechat/pay" store="payLog2.db"  notify="http://www.xmlpipe.org/xpe/wechat/pay/notify" />
            <async name="pay" xpipe="http://www.xmlpipe.org/xpe/wechat/pay/backend" />
            
            <!--Web oauth2-->
            <get path="/wechat/oauth" xpipe="http://www.xmlpipe.org/xpe/wechat/oauth" notify="http://www.xmlpipe.org/xpe/wechat/oauth/notify" />
            <get path="/wechat/oauth/userinfo" xpipe="http://www.xmlpipe.org/xpe/wechat/sns" notify="http://www.xmlpipe.org/xpe/wechat/direct/notify" />
            <get path="/jsapi" xpipe="http://www.xmlpipe.org/xpe/wechat/jsapi" notify="http://www.xmlpipe.org/xpe/wechat/direct/notify" />
            <async name="jsapi" xpipe="http://www.xmlpipe.org/xpe/wechat/jsapi/backend" />
            <async name="oauth" xpipe="http://www.xmlpipe.org/xpe/wechat/oauth/backend" />
            <async name="sns" xpipe="http://www.xmlpipe.org/xpe/wechat/sns/backend" />
            
            <!--qrcode-->
            <get path="/qr" xpipe="http://www.xmlpipe.org/xpe/wechat/qr" notify="http://www.xmlpipe.org/xpe/wechat/qr/notify" />
            <post path="/qr" xpipe="http://www.xmlpipe.org/xpe/wechat/qr" notify="http://www.xmlpipe.org/xpe/wechat/qr/notify" />
            <async name="qrcode" xpipe="http://www.xmlpipe.org/xpe/wechat/qr/backend" />
            
            <!--addgroup-->
            <post path="/groups/update" xpipe="http://www.xmlpipe.org/xpe/wechat/groups" notify="http://www.xmlpipe.org/xpe/wechat/direct/notify" />
            <post path="/groups/create" xpipe="http://www.xmlpipe.org/xpe/wechat/groups" notify="http://www.xmlpipe.org/xpe/wechat/direct/notify" />
            <get path="/groups/get" xpipe="http://www.xmlpipe.org/xpe/wechat/groups" notify="http://www.xmlpipe.org/xpe/wechat/direct/notify" />
            <post path="/groups/getid" xpipe="http://www.xmlpipe.org/xpe/wechat/groups" notify="http://www.xmlpipe.org/xpe/wechat/direct/notify" />
            <post path="/groups/members/update" xpipe="http://www.xmlpipe.org/xpe/wechat/groups" notify="http://www.xmlpipe.org/xpe/wechat/direct/notify" servicePath="members/update" />
            <async name="groups" xpipe="http://www.xmlpipe.org/xpe/wechat/groups/backend" />
            
            <!-- backend services -->
            <async name="getuser" xpipe="http://www.xmlpipe.org/xpe/wechat/getUser" />
            <async name="send" xpipe="http://www.xmlpipe.org/xpe/wechat/send" />
            <async name="createMenu" xpipe="http://www.xmlpipe.org/xpe/wechat/createMenu" />
        </service>
```

