# Vue-resource实现ajax请求和跨域请求

vue-resource是Vue提供的体格http请求插件，如同jQuery里的$.ajax，用来和后端交互数据的。



在使用时，首先需要安装vue-resource插件



1.在项目跟目录上安装： 

 npm install vue-resource  



2.引入resource插件

1. import VueResource from 'vue-resource';  
2. Vue.use(VueResource)  



3.发送请求：

​	this.$http.get("http://www.vrserver.applinzi.com/aixianfeng/apihome.php").then(function(res){  

​        console.log(res)  

​    })  



ES6写法：

​		this.$http.get('url', [options]).then((res) => {  

​				// 处理成功的结果

​		}, (res) => { 

​				// 处理失败的结果

​		});



在发送请求后，使用then方法来处理响应结果，then方法有两个参数，第一个参数是响应成功时的回调函数，第二个参数是响应失败时的回调函数。

then方法的回调函数也有两种写法，第一种是传统的函数写法，第二种是更为简洁的ES 6的Lambda写法：



POST请求：

​		this.$http.post("http://www.vrserver.applinzi.com/aixianfeng/apihome.php",{name:"abc"}, 	{emulateJSON:true}).then(  

​           function (res) {  

​               // 处理成功的结果  

​                alert(res.body);  

​            },function (res) {  

​            // 处理失败的结果  

​            }  

​      );



JSONP请求：

new Vue({ ready() {   

​	 this.$http.jsonp('/url', {name:"abc"}) .then(function (res){   

​    	  console.log(res)   

​    }, function (res) {   

​      	  console.log(res)   

​     });   

​    }   

})

吐槽一下，现在应该没有用到JSON的了吧，有的话真呵呵呵了。



**支持的HTTP方法**

vue-resource的请求API是按照REST风格设计的，它提供了7种请求API：

- get(url, [options])
- head(url, [options])
- delete(url, [options])
- jsonp(url, [options])
- post(url, [body], [options])
- put(url, [body], [options])
- patch(url, [body], [options])

除了jsonp以外，另外6种的API名称是标准的HTTP方法。当服务端使用REST API时，客户端的编码风格和服务端的编码风格近乎一致，这可以减少前端和后端开发人员的沟通成本。

| 客户端请求方法         | 服务端处理方法 |
| ---------------------- | -------------- |
| this.$http.get(...)    | Getxxx         |
| this.$http.post(...)   | Postxxx        |
| this.$http.put(...)    | Putxxx         |
| this.$http.delete(...) | Deletexxx      |



**options对象**

发送请求时的options选项对象包含以下属性：

| 参数         | 类型                   | 描述                                                         |
| ------------ | ---------------------- | ------------------------------------------------------------ |
| url          | string                 | 请求的URL                                                    |
| method       | string                 | 请求的HTTP方法，例如：'GET', 'POST'或其他HTTP方法            |
| body         | Object, FormDatastring | request body                                                 |
| params       | Object                 | 请求的URL参数对象                                            |
| headers      | Object                 | request header                                               |
| timeout      | number                 | 单位为毫秒的请求超时时间 (0 表示无超时时间)                  |
| before       | function(request)      | 请求发送前的处理函数，类似于jQuery的beforeSend函数           |
| progress     | function(event)        | ProgressEvent回调处理函数                                    |
| credientials | boolean                | 表示跨域请求时是否需要使用凭证                               |
| emulateHTTP  | boolean                | 发送PUT, PATCH, DELETE请求时以HTTP POST的方式发送，并设置请求头的X-HTTP-Method-Override |
| emulateJSON  | boolean                | 将request body以application/x-www-form-urlencoded content type发送 |



**emulateHTTP的作用**

如果Web服务器无法处理PUT, PATCH和DELETE这种REST风格的请求，你可以启用enulateHTTP现象。启用该选项后，请求会以普通的POST方法发出，并且HTTP头信息的X-HTTP-Method-Override属性会设置为实际的HTTP方法。

Vue.http.options.emulateHTTP = true;



**emulateJSON的作用**

如果Web服务器无法处理编码为application/json的请求，你可以启用emulateJSON选项。启用该选项后，请求会以application/x-www-form-urlencoded作为MIME type，就像普通的HTML表单一样。

Vue.http.options.emulateJSON = true;



response对象包含以下属性：

| 方法       | 类型    | 描述                                          |
| ---------- | ------- | --------------------------------------------- |
| text()     | string  | 以string形式返回response body                 |
| json()     | Object  | 以JSON对象形式返回response body               |
| blob()     | Blob    | 以二进制形式返回response body                 |
| 属性       | 类型    | 描述                                          |
| ok         | boolean | 响应的HTTP状态码在200~299之间时，该属性为true |
| status     | number  | 响应的HTTP状态码                              |
| statusText | string  | 响应的状态文本                                |
| headers    | Object  | 响应头                                        |