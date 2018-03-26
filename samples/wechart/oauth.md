## 网页授权

### 说明：
+ 在src/html 目录下创建文件  weixin_login.html 和 weixin_auth.html
+ 在微信下 用地址 .../login 访问
+ 地址自动跳转到 .../auth
+ 跳转后的路径中获得授权 code，根据 code 就可调用 XPE 服务获得 openid, 再通过 openid 获得用户微信信息  

#### 关键代码
```
/wechat/oauth?code=' + code
/wechat/oauth/userinfo?openId=' + openid + '&access_token=' + resp.access_token
```

#### 请求网页授权

+ service

```
    <resources dir="html" path="html" />
    <resource src="html/weixin_login.html" path="login" />
```

+ weixin_login.html

```
<!DOCTYPE html>
<html>
	<head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, inital-scale=1.0, maximum-scale=1.0, user-scalable=no;" />
        <meta http-equiv="X-UA-Compatible" content="IE=EDGE" />
		<title></title>
		<script src="/js/core.js" ></script>
	</head>
	<body>
		
        <h3>Test</h3>
 		<br/>
        <button id='start'>Start</button>
 		<br/>
		<div>
		    <p id='result'>
		    </p>
		</div>

		<script>
            X('start').onclick = function(e) {
                
               var url = 'https://your_web_domain/auth';
                url = encodeURIComponent(url);
                //url = "https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx2aca2bca1d061ed7&redirect_uri=" + url + "&response_type=code&scope=snsapi_base&state=123#wechat_redirect";
                url = "https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx2aca2bca1d061ed7&redirect_uri=" + url + "&response_type=code&scope=snsapi_userinfo&state=123#wechat_redirect";

                window.location.replace(url, true);
             };
		</script>
	</body>
</html>

```

#### 被授权网页

+ service
```
    <resources dir="html" path="html" />
    <resource src="html/weixin_auth.html" path="auth" />
```

+ weixin_auth.html

```
<!DOCTYPE html>
<html>
	<head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, inital-scale=1.0, maximum-scale=1.0, user-scalable=no;" />
        <meta http-equiv="X-UA-Compatible" content="IE=EDGE" />
		<title></title>
		<script src="/js/core.js" ></script>
	</head>
	
	<body>
		<h3>Show User Info</h3>
		<br/>
		<div id='result'></div>
		
		<script>
		    
           function getQuery() {
                var url =  window.location.href;
            	var search = url.substring(url.lastIndexOf("?") + 1);
            	var obj = {};
            	var reg = /([^?&=]+)=([^?&=]*)/g;
            	search.replace(reg, function(rs, $1, $2) {
            		var name = decodeURIComponent($1);
            		var val = decodeURIComponent($2);
            		val = String(val);
            		obj[name] = val;
            		return rs;
            	});
            	return obj;
            }

        	var qs = getQuery();
            var code = qs.code;
            
            if (code){
                X.get('/wechat/oauth?code=' + code, function(respText) {
                    var resp = JSON.parse(respText);
                    openid = resp.openid;
                    if (openid) {
                        X.get('/wechat/oauth/userinfo?openId=' + openid + '&access_token=' + resp.access_token, function(respText) {
                            var resp = JSON.parse(respText);
                            //可获取到微信用户的详细信息
                            console.log(respText);
                            X('result').innerHTML = respText;
                        });
                        
                    } else {
                        X('result').innerHTML = "openid == null";
                    }
                });
            } else {
                X('result').innerHTML = "code == null";
            }
		</script>
		
	</body>
</html>
```

