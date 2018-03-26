# JS-SDK 授权


#### 目的说明

+ 参考[微信JS-SDK说明文档](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115)
+ 运行其[官方DEMO页面和示例](http://demo.open.weixin.qq.com/jssdk) 

#### 步骤

+ [下载示例代码](http://demo.open.weixin.qq.com/jssdk/sample.zip)
+ 按照前面范例的方式导入到 XPE 下
+ 修改代码

#### 关键代码
+  X.get('/api/jsapi?url=' + host, function(json) ...

#### 注意事项
+ var host = document.location.href; //'https://you_xpe_domain/wechat'; //注意此处修改域名
+ appId: "wxappid", //注意此处修改微信appid,


#### 修改代码

将下列代码：
```
<script>
   wx.config({
    ...
  });
</script>

```

修改为：

```
<script>
X.sub("init", function() {

    var host = document.location.href; //'https://you_xpe_domain/wechat'; //注意此处修改域名
    console.log(document.location.href);
    if (host.indexOf("#") !== -1) {
        host = host.split("#")[0];
    }
    host = host.replace("?", "%3f");
    
    reg = new RegExp("=", "g");
    host = host.replace(reg, "%3d");
    reg = new RegExp("&", "g");
    host = host.replace(reg, "%26");
    host = host.replace(":", "%3A");
    // host = encodeURIComponent(host);
    console.log(host);
    console.log(encodeURIComponent(document.location.href));
    

  X.get('/api/jsapi?url=' + host, function(json) {
        json = JSON.parse(json);
        wx.config({
            debug: false,
            appId: "wxappid", //注意此处修改微信appid,
            timestamp: json.timestamp,
            nonceStr: json.nonceStr,
            signature: json.signature,
            jsApiList: [
                'checkJsApi',
                'onMenuShareTimeline',
                'onMenuShareAppMessage',
                'onMenuShareQQ',
                'onMenuShareWeibo',
                'onMenuShareQZone',
                'hideMenuItems',
                'showMenuItems',
                'hideAllNonBaseMenuItem',
                'showAllNonBaseMenuItem',
                'translateVoice',
                'startRecord',
                'stopRecord',
                'onVoiceRecordEnd',
                'playVoice',
                'onVoicePlayEnd',
                'pauseVoice',
                'stopVoice',
                'uploadVoice',
                'downloadVoice',
                'chooseImage',
                'previewImage',
                'uploadImage',
                'downloadImage',
                'getNetworkType',
                'openLocation',
                'getLocation',
                'hideOptionMenu',
                'showOptionMenu',
                'closeWindow',
                'scanQRCode',
                'chooseWXPay',
                'openProductSpecificView',
                'addCard',
                'chooseCard',
                'openCard'
            ]
        });
        
       wx.ready(function() {
        wx.checkJsApi({
            jsApiList: [
                'onMenuShareTimeline',
                'onMenuShareAppMessage',
                'onMenuShareQQ',
                'onMenuShareWeibo',
                'onMenuShareQZone',
                'hideMenuItems',
                'showMenuItems',
                'hideAllNonBaseMenuItem',
                'showAllNonBaseMenuItem',
                'translateVoice',
                'startRecord',
                'stopRecord',
                'onVoiceRecordEnd',
                'playVoice',
                'onVoicePlayEnd',
                'pauseVoice',
                'stopVoice',
                'uploadVoice',
                'downloadVoice',
                'chooseImage',
                'previewImage',
                'uploadImage',
                'downloadImage',
                'getNetworkType',
                'openLocation',
                'getLocation',
                'hideOptionMenu',
                'showOptionMenu',
                'closeWindow',
                'scanQRCode',
                'chooseWXPay',
                'openProductSpecificView',
                'addCard',
                'chooseCard',
                'openCard'
            ],
            success: function(res) {
                // alert(JSON.stringify(res));
            }
        });

        });
        
        wx.error(function(res) {
            console.log("wx config error");
        });
    });

});    
    
</script>

```

