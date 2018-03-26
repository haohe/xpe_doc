## 其他范例


示例代码（由刘太辉提供）
（暂无说明）


#### 前端登录后，后端服务中如何获得Session信息？
```
1.在配置ms的时候，添加userStore="users.db"
	<webSocket path="api/session/my/ws" xpipe="http://www.xmlpipe.org/xpe/ms/http" name="api_session_my_ws"/> 
	<service path="/api/session/my/*" sendTo="api_session_my_ws" userStore="users.db">
	    <post xpipe="http://www.xmlpipe.org/xpe/ms/http/request" />
	    <get  xpipe="http://www.xmlpipe.org/xpe/ms/http/requesth"  />
	</service>

2.handler中传入的参数req中会包含req.userId和req.username
	 var apis = function(req, resp){
                    ...
		        }
3.可以根据获取到的userId或者username来get到用户的详细信息

```
#### Faceted search 的例子

```
<service store="resources.db" 
		storeType="binary" 
		seqKey="true" 
		maxJoinSize="20480" 
		primaryKey="id" 
        
		facetHierarchy="
			rdbid:rdb, 
			subject02:subject02_lk_display, 
			resourceType:resourceType_lk_display, 
			mediaType:mediaType_lk_display" 
        
		facetFields="
			rdbid, 
			courseID, 
			author, 
			authorOrg, 
			status, 
			title, 
			courseTitle"

        sorted="id,viewNum,collectionNum,commentNum,downloadNum,created" 
        
		fields="
			id, 
			uuid, 
			subject02_lk_display, 
			isSelf, 
			rdbid^id, 
			subject02^id, 
			rdbid^viewNum, 
			rdbid^collectionNum, 
			rdbid^commentNum, 
			rdbid^downloadNum, 
			subject02^viewNum, 
			subject02^collectionNum, 
			subject02^commentNum, 
			subject02^downloadNum, 
			mediaType^viewNum, 
			mediaType^collectionNum, 
			mediaType^commentNum, 
			mediaType^downloadNum, 
			resourceType^viewNum, 
			resourceType^collectionNum, 
			resourceType^commentNum, 
			resourceType^downloadNum, 
			courseID^viewNum, 
			courseID^collectionNum, 
			courseID^commentNum, 
			courseID^downloadNum, 
			author^viewNum, 
			author^collectionNum, 
			author^commentNum, 
			author^downloadNum, 
			authorOrg^viewNum, 
			authorOrg^collectionNum, 
			authorOrg^commentNum, 
			authorOrg^downloadNum, 
			status^viewNum, 
			status^collectionNum, 
			status^commentNum, 
			status^downloadNum, 
			title^viewNum, 
			title^collectionNum, 
			title^commentNum, 
			title^downloadNum, 
			courseTitle^viewNum, 
			courseTitle^collectionNum, 
			courseTitle^commentNum, 
			courseTitle^downloadNum, 
			original"

        dict="
			id, 
			rdbid, 
			rdb, 
			subject, 
			subject_lk_display, 
			_mediaType, 
			_view_total_count, 
			author, 
			authorOrg, 
			copyright, 
			courseID, 
			courseTitle, 
			coverStatus, 
			createDate, 
			createdSite, 
			dimension, 
			duration, 
			grade, 
			grade_lk_display, 
			keywords, 
			language, 
			level, 
			level_lk_display, 
			mediaType, 
			mediaType_lk_display, 
			newSourceID, 
			orgCategory, 
			orgCategory_lk_display, 
			owner, 
			pages, 
			previewURL, 
			publishDate, 
			recommendStatus, 
			recommendStatus_lk_display, 
			resOrder, 
			resourceBatch, 
			resourceOrgUUID, 
			resourceSelect, 
			resourceType, 
			resourceType_lk_display, 
			size, 
			source, 
			status, 
			status_lk_display, 
			subject01, 
			subject01_lk_display, 
			subject02, 
			subject02_lk_display, 
			title, 
			uploadDate, 
			uploaderFlag, 
			uuid, 
			viewer, 
			viewNum:i!0, 
			collectionNum:i!0, 
			commentNum:i!0, 
			downloadNum:i!0, 
			isSelf, 
			coverFile, 
			tid, 
			oid, 
			score:i, 
			created:t, 
			lastUpdated:t, 
			remarks, 
			original"
>


```

#### 微信服务
```
1. sitemap中定义微信服务，参考代码：
            <service token="微信中配置的token" appid="微信的appid" secret="微信的app Secret" openId="微信的原始ID ">
            <async name="updateAccount" xpipe="http://www.xmlpipe.org/xpe/wechat/updateAccount" />
            <get path="/wechat/account" xpipe="http://www.xmlpipe.org/xpe/wechat/accounts/get" />
            <del path="/wechat/account" xpipe="http://www.xmlpipe.org/xpe/wechat/accounts/del" />
            <post path="/wechat/accounts" xpipe="http://www.xmlpipe.org/xpe/wechat/accounts/update" />
            <get path="/wechat/accounts" xpipe="http://www.xmlpipe.org/xpe/wechat/accounts/search" />
            <!--used by wechat server to post events -->
            <post path="/wechat" xpipe="http://www.xmlpipe.org/xpe/wechat/notify" notify="http://www.xmlpipe.org/xpe/wechat/updateUser" />
            <get path="/wechat" xpipe="http://www.xmlpipe.org/xpe/wechat/verify" />
            <!--you can search by MsgId,FromUserName,MsgType,Event,EventKey -->
            <get path="/json/event" xpipe="http://www.xmlpipe.org/xpe/wechat/search" store="eventStore.db" />
            <!--User info store -->
            <get path="/json/userinfo" xpipe="http://www.xmlpipe.org/xpe/wechat/userinfo/get" />
            <get path="/json/userinfo/search" start="-1" xpipe="http://www.xmlpipe.org/xpe/wechat/userinfo/search" />
            <!-- this pipe must be placed first -->
            <get path="/wechat/reply/rule" xpipe="http://www.xmlpipe.org/xpe/wechat/rules/get" />
            <post path="/wechat/reply/rules" xpipe="http://www.xmlpipe.org/xpe/wechat/rules/update" />
            <get path="/wechat/reply/rules" start="-1" xpipe="http://www.xmlpipe.org/xpe/wechat/rules/search" />
            <del path="/wechat/reply/rule/del" xpipe="http://www.xmlpipe.org/xpe/wechat/rules/del" />
            <!-- award rules -->
            <post path="/wechat/awardrules" xpipe="http://www.xmlpipe.org/xpe/wechat/awardrules/update" />
            <get path="/wechat/awardrule" xpipe="http://www.xmlpipe.org/xpe/wechat/awardrules/get" />
            <del path="/wechat/awardrule" xpipe="http://www.xmlpipe.org/xpe/wechat/awardrules/del" />
            <get path="/wechat/awardrules" xpipe="http://www.xmlpipe.org/xpe/wechat/awardrules/search" />
            <!--menu-->
            <post path="/menu" xpipe="http://www.xmlpipe.org/xpe/wechat/menu" />
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
            <!-- backend services -->
            <async name="getuser" xpipe="http://www.xmlpipe.org/xpe/wechat/getUser" />
            <async name="send" xpipe="http://www.xmlpipe.org/xpe/wechat/send" />
            <async name="createMenu" xpipe="http://www.xmlpipe.org/xpe/wechat/createMenu" />
            <async name="pay" xpipe="http://www.xmlpipe.org/xpe/wechat/pay/backend" />
        </service>
 
2.	微信oauth2，scope=snsapi_base形式，js代码

/**
 * My module:
 *  获取openid
 */
X.sub("init", function() {
    /**
     * oauth2.0
     */
    var openid = X.cookie.get('xoid');
    var href = window.location.href;
    var code = X.qs.code;
    function getOpenid(evt, res) {
        if (!openid || openid === null || openid.length === 0 || typeof openid === 'undefined') {
            if (typeof code === 'undefined') {
                reUrl();
            } else {
                X.get('/wechat/oauth?code=' + X.qs.code, function(respText) {
                    var resp = JSON.parse(respText);
                    openid = resp.openid;
                    if (typeof openid === 'undefined') {
                        reUrl();
                    } else {
                        X.cookie.add('xoid', openid);
                        X.pub('checkOpenid');
                    }
                });
            }
        }
    }

    X.sub('getOpenid', getOpenid);

    function reUrl() {
        var params = "?";
        for (var p in X.qs) {
            if (p !== 'code' && p !== 'state') {
                params += p + '=' + X.qs[p] + '&';
            }
        }
        var url = href.split('?')[0];
        url += params.substring(0, params.length - 1);
        goOauth(url);
    }

    function goOauth(link) {
        link = encodeURIComponent(link);
        var url = "https://open.weixin.qq.com/connect/oauth2/authorize?appid=微信appid&redirect_uri=" + link + "&response_type=code&scope=snsapi_base&state=123#wechat_redirect";
        window.location.replace(url, true);
    }
});

3.	当scope= snsapi_userinfo形式，sitemap中需要增加下面的服务
<get path="/wechat/oauth/userinfo" xpipe="http://www.xmlpipe.org/xpe/wechat/sns" notify="http://www.xmlpipe.org/xpe/wechat/direct/notify" />
 
get-openid.js代码修改如下：
/**
 * My module:
 *  获取openid
 */
X.sub("init", function() {
    /**
     * oauth2.0
     */
    var openid = X.cookie.get('xoid');
    var href = window.location.href;
    var code = X.qs.code;
    function getOpenid(evt, res) {
        if (!openid || openid === null || openid.length === 0 || typeof openid === 'undefined') {
            if (typeof code === 'undefined') {
                reUrl();
            } else {
                X.get('/wechat/oauth?code=' + X.qs.code, function(respText) {
                    var resp = JSON.parse(respText);
                    openid = resp.openid;
                    if (typeof openid === 'undefined') {
                        reUrl();
                    } else {
                        X.cookie.add('xoid', openid);
                        X.get('/wechat/oauth/userinfo?openId=' + openid + '&access_token=' + resp.access_token, function(respText) {
                            var resp = JSON.parse(respText);
                            //可获取到微信用户的详细信息
                        });
                    }
                });
            }
        }
    }

    X.sub('getOpenid', getOpenid);

    function reUrl() {
        var params = "?";
        for (var p in X.qs) {
            if (p !== 'code' && p !== 'state') {
                params += p + '=' + X.qs[p] + '&';
            }
        }
        var url = href.split('?')[0];
        url += params.substring(0, params.length - 1);
        goOauth(url);
    }

    function goOauth(link) {
        link = encodeURIComponent(link);
        var url = "https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx784973bd7a4ef619&redirect_uri=" + link + "&response_type=code&scope=snsapi_base&state=123#wechat_redirect";
        window.location.replace(url, true);
    }
});
 
4.	其他的服务，基本可以参考微信的使用文档来进行参数配置，详细的使用在之后会给出案例
```

#### 手机验证码登录
##### 1.	mml中配置
```
<!--用户表-->
    <topic name="users" dict="id:l,username,password:a,email,profile:a,permMask:s,group:l,created:t,lastUpdated:t,mode:b,token,phone" secret="c89234" aesKey=" 90uf3O/rlfKeSGtW7ypC6Jw40S/JDvg5mZ+TzAe1PMI=" primaryKey="id" storeType="binary" seqKey="true" fields="id,username,email">
        <pub protocol="json"></pub>
<sub dest="ws/users" protocol="dblog" transport="ws" />
</topic>
<!--SMS验证码-->
    <topic name="smscode" storeType="binary" primaryKey="phone" fields="phone^id" dict="phone,code,lastUpdated:t,created:t">
        <pub protocol="json"></pub>
    </topic>
```
##### 2.	注册
```
前端用户输入手机号，点击获取验证码，执行ms服务，发送验证码服务参考代码：
/**
 * My module:
 * Handle sms api
 */ (function() {
    var MD5 = function() {
        //md5函数，省略
    };
    var userid = 'xxxxx';
    var pwd = 'xxxxx';
    pwd = MD5(pwd);
    pwd = pwd.toUpperCase();

    var process = function(req, resp) {
        resp.body = '{"code":1,"msg":"非法操作"}';
    };


    var smsapi = function(req, resp) {
        if (req.body) {
            var o = JSON.parse(req.body);
            var parame = req.params;
            var path = req.path;
            var api = "";
            var r = "";
            var str = "";
            var action;
            if (path == "/api/sms/send") {
                action = 1;
            }
            switch (action) {
                case 1: //发送验证码
                    o.phone = o.phone || "";
                    if (!o.phone) {
                        resp.body = '{"code":3, "msg":"请填写正确的手机号"}';
                        return;
                    }
                    //验证是否已经发送过验证码，如已发送，则判断是否已过期
                    var c = domain.smscode.search("phone=" + o.phone);
                    c.meta.total = c.meta.total || {
                        "total": "0"
                    };
                    if (c.meta.total !== "0") {
                        var d = new Date();
                        var t = d.getTime();
                        var p = t - c.lastUpdated;
                        //计算相差分钟数, 120秒禁止再次生成
                        var m = Math.floor(p / 1000);
                        if (m < 120) {
                            resp.body = '{"code":4, "msg":"操作过于频繁"}';
                            return;
                        }
                    }
                    var code = "";
                    for (var i = 0; i < 6; i++) {
                        code += Math.floor(Math.random() * 10);
                    }
                    var item = {};
                    item.phone = o.phone;
                    item.code = code;
                    var res = domain.smscode.update(item);
                    if (res.code === 0) {
                        //调用sms接口，进行短信发送
                        api = 'http://yunxintong.net:8091/zysms/smsSend.do';
                        var content = '【测试】验证码：' + code + '，您正在注册/登陆xx平台帐号，需要进行手机验证。';
                        var enc = Java.type('java.net.URLEncoder');
                        var msg = enc.encode(content, 'gbk'); //GBK转码服务
                        str = 'userid=' + userid + '&pwd=' + pwd + '&mobile=' + o.phone + '&content=' + msg;
                        r = http(api).post(str);
                        resp.body = '{"code":0, "msg":"' + res.id + '"}';
                    } else {
                        resp.body = '{"code":2,"msg":"参数错误"}';
                        break;
                    }
                    break;
                default:
                    resp.body = '{"code":2,"msg":"参数错误"}';
                    break;
            }
        } else {
            resp.body = '{"code":1,"msg":"缺少参数"}';
        }
    };

    return {
        onGet: process,
        onPost: smsapi
    };
}());

当用户收到验证码，输入验证码并点击注册，执行ms服务，参考代码如下：

(function() {

    var process = function(req, resp) {
        resp.body = '{"code":1,"msg":"非法操作"}';
    };

    var api = function(req, resp) {
        if (req.body) {
            var o = JSON.parse(req.body);
            var parame = req.params;
            var path = req.path;
            var action;
            if (path == "/api/user/register") {
                action = 1;
            }
            switch (action) {
                case 1: //注册用户
                    o.phone = o.phone || "";
                    o.code = o.code || "";
                    if (!o.phone) {
                        resp.body = '{"code":3, "msg":"请填写正确的手机号"}';
                        return;
                    }
                    if (!o.code) {
                        resp.body = '{"code":3, "msg":"请填写正确的验证码"}';
                        return;
                    }
                    //验证是否已经发送过验证码，如已发送，则判断是否已过期
                    var c = domain.smscode.search("phone=" + o.phone);
                    c.meta.total = c.meta.total || {
                        "total": "0"
                    };
                    //已发送
                    if (c.meta.total !== "0") {
                        //判断过期及验证码是否正确
                        // ....
                        //如果验证通过，则注册用户
                        var data = {};
                        data.username = o.phone;
                        var u = domain.users.search("username=" + body.username);
                        u.meta = u.meta || {
                            "total": "0"
                        };
                        if (u.meta.total != '0') {
                            var ur = u.data[0];
                            data.id = ur.id;
                        }
                        resp.body = JSON.stringify(domain.users.update(data));
                    } else {
                        resp.body = '{"code":3, "msg":"请先获取验证码"}';
                    }
                    break;
                default:
                    resp.body = '{"code":2,"msg":"参数错误"}';
                    break;
            }
        } else {
            resp.body = '{"code":1,"msg":"缺少参数"}';
        }
    };

    return {
        onGet: process,
        onPost: api
    };
}());
```
##### 3.	登录
```
登录步骤，填写手机号-----获取验证码----验证验证码

其他步骤省略，SSO登录参考代码：
/**
 * My module:
 * Login api
 */ (function() {

    var process = function(req, resp) {
        resp.body = '{"code":1,"msg":"非法操作"}';
    };

    var api = function(req, resp) {
        if (req.body) {
            var o = JSON.parse(req.body);
            var parame = req.params;
            var path = req.path;
            var action;
            if (path == "/api/user/login") {
                action = 1;
            }
            switch (action) {
                case 1: //获取authtoken
                    //....
                    //....
                    //一系列的验证后进行下面的获取token
                    var ru = domain.users.search("username=" + o.username);
                    var item = {};
                    item.ti = (new Date()).getTime();
                    item.mk = 0;
                    item.id = ru.data[0].id; //用户id
                    item.un = ru.data[0].username; //用户username
                    item.dis = ru.data[0].profile.name; //用户name
                    item.iss = "";
                    item.em = ru.data[0].email; //用户邮箱
                    item.ph = null;
                    item.cc = null;
                    item.al = null;
                    //生成authtoken，设置过期时间为7天
                    u = domain.users.jwt().encrypt(JSON.stringify(item), new Date().getTime() + 7 * 24 * 60 * 60 * 1000);
                    resp.body = '{"code":0, "token":"' + u + '"}';
                    break;
                default:
                    resp.body = '{"code":2,"msg":"参数错误"}';
                    break;
            }
        } else {
            resp.body = '{"code":1,"msg":"缺少参数"}';
        }
    };

    return {
        onGet: process,
        onPost: api
    };
}());

//前台拿到生成的token后，操作代码：
function openPage(url, method, params) {
        //var dynamicForm = document.getElementById("dynamicForm");
        var dynamicForm = document.createElement("form");
        dynamicForm.setAttribute("method", method);
        dynamicForm.setAttribute("action", url);
        dynamicForm.innerHTML = "";
        document.body.appendChild(dynamicForm);
        for (var i = 0; i < params.length; i++) {
            var input = document.createElement("input");
            input.setAttribute("type", "hidden");
            input.setAttribute("name", params[i].paramName);
            input.setAttribute("value", params[i].paramValue);
            dynamicForm.appendChild(input);
        }
        dynamicForm.submit();
}

openPage(decodeURIComponent("/user/login"), 'post', [{
                                            paramName: 'token',
                                            paramValue: resp.token
                                        }, {
                                            paramName: 'redirectTo',
                                            paramValue: decodeURIComponent(document.location.href)
                                        }, {
                                            paramName: 'error',
                                            paramValue: "/"
                                        }
                                    ]);
发送获取到的token到user的登录服务上，其中有3个参数，token：生成的authtoken ，
redirectTo：登录成功后跳转页面，error：登录失败跳转页面

sitemap中users.db参考：
 

```
