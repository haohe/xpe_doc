#### XPE 中使用关系型数据库


（暂无说明）


示例代码（由刘太辉提供）

```
(function() {
	
    var ds = new DataSource("jdbc:mysql://115.29.34.201:3306/rcstDL?" + "user=rcstDL&password=rcstDL@123#&useUnicode=true&characterEncoding=UTF8");

    var apis = function(req, resp) {
        print(req.params);
        print("userId=" + req.userId + " and username=" + req.username);
        
        var body = JSON.parse(req.body);
        var params = req.params || {};
        
        print("PATH=" + req.path);
        print("BODY=" + JSON.stringify(body));
        print("PARAMS ID=" + params.id);
        
        var path = req.path;
        var w = 0;
        var item = {};
        if (path == "/api/db/get/test") { //查询
            w = 1;
        } else if (path == "/api/db/insert/test") { //插入
            w = 2;
        } else if (path == "/api/db/update/test") { //更新
            w = 3;
        } else if (path == "/api/db/delete/test") { //删除
            w = 4;
        }
        var conn = ds.getConnection();
        try {
            var stmnt = conn.createStatement();
            switch (w) {
                case 1:
                    var rs = stmnt.executeQuery("SELECT * FROM lngzhd_log");
                    var res = [];
                    while (rs.next()) {
                        var en = {};
                        en.com = rs.getString("com");
                        en.openid = rs.getString("openid");
                        en.dep = rs.getString("dep");
                        en.name = rs.getString("name");
                        res.push(en);
                    }

                    resp.body = JSON.stringify(res);
                    break;
                case 2:
                    count = stmnt.executeUpdate("INSERT INTO lngzhd_log(openid, com, dep, name, ln, hy, created) values('123456','test','测试','测试','2','3'," + ("" + (new Date()).getTime()).substring(0, 10) + ")");
                    if (count > 0) {
                        resp.body = '{"code":0,"msg":"插入成功"}';
                    } else {
                        resp.body = '{"code":2,"msg":"插入失败"}';
                    }

                    break;
                case 3:
                    count = stmnt.executeUpdate("UPDATE lngzhd_log SET openid='m123456' WHERE openid='123456'");
                    if (count > 0) {
                        resp.body = '{"code":0,"msg":"更新成功"}';
                    } else {
                        resp.body = '{"code":2,"msg":"更新失败"}';
                    }

                    break;
                case 4:
                    count = stmnt.executeUpdate("DELETE FROM lngzhd_log WHERE openid='m123456'");
                    if (count > 0) {
                        resp.body = '{"code":0,"msg":"删除成功"}';
                    } else {
                        resp.body = '{"code":2,"msg":"删除失败"}';
                    }

                    break;
                default:
                    resp.body = '{"code":2,"msg":"非法操作"}';
                    break;
            }
        } finally {
            conn.close();
        }
    };

    return {
        onGet: apis,
        onPost: apis
    };    
    
}());

```

