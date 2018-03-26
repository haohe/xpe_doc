## vue 能使用吗?

+ service
```
    <resources dir="html" path="html" />
    <resource src="html/vue.html" path="vue" />
```

+ html
```
<!DOCTYPE html>
<html>
	<head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, inital-scale=1.0, maximum-scale=1.0, user-scalable=no;" />
        <meta http-equiv="X-UA-Compatible" content="IE=EDGE" />
		<title>Vue Demo</title>
		<script src="https://cdn.jsdelivr.net/npm/vue"></script>
		<script src="/js/core.js" ></script>
	</head>
	<body>
		
	    <div id="app">
			<h3>{{ message }}</h3>
	    </div>

		<script type="text/javascript">
		    var app = new Vue({
		      el: '#app',
		      data: {
		        message: 'Hello Vue!'
		      }
		    })
		</script>
	</body>
</html>
```