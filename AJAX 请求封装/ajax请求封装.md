函数就可以理解为一个想要做的事情，函数体中约定了这件事情做的过程，直到调用时才开始工作。
将函数作为参数传递就像是将一个事情交给别人，这就是委托的概念

代码示例：

```js
function ajax (method, url, params, done) {   // 统一转换为大写便于后续判断   method = method.toUpperCase()     // 对象形式的参数转换为 urlencoded 格式   var pairs = []   for (var key in params) {     pairs.push(key + '=' + params[key])   }   var querystring = pairs.join('&')     var xhr = window.XMLHttpRequest ? new XMLHttpRequest() : new  ActiveXObject('Microsoft.XMLHTTP')     xhr.addEventListener('readystatechange', function () {     if (this.readyState !== 4) return       // 尝试通过 JSON 格式解析响应体     try {       done(JSON.parse(this.responseText))     } catch (e) {       done(this.responseText)     }   })     // 如果是 GET 请求就设置 URL 地址 问号参数   if (method === 'GET') {     url += '?' + querystring   }     xhr.open(method, url)     // 如果是 POST 请求就设置请求体   var data = null   if (method === 'POST') {     xhr.setRequestHeader('Content‐Type', 'application/x‐www‐form‐urlencoded')     data = querystring   }   xhr.send(data) } 
```

调用：

```js
ajax('get', './get.php', { id: 123 }, function (data) {   console.log(data) })   ajax('post', './post.php', { foo: 'posted data' }, function (data) {   console.log(data) })
```

 ## jQuery 中的 AJAX 

jQuery 中有一套专门针对 AJAX 的封装，功能十分完善，经常使用，需要着重注意。
参考：
http://www.jquery123.com/category/ajax/

http://www.w3school.com.cn/jquery/jquery_ref_ajax.asp

 $.ajax

```js
$.ajax({   url: './get.php',   type: 'get',   dataType: 'json',   data: { id: 1 },   beforeSend: function (xhr) {     console.log('before send')   },   success: function (data) {     console.log(data)   },   error: function (err) {     console.log(err)   },   complete: function () {     console.log('request completed')   }
  })

```

常用选项参数介绍：
url：请求地址 

type：请求方法，默认为 get 

dataType：服务端响应数据类型

contentType：请求体内容类型，默认 application/x-www-form-urlencoded data：需要传递到服务端的数据，如果 GET 则通过 URL 传递，如果 POST 则通过请求体传递 

timeout：请求超时时间

 beforeSend：请求发起之前触发 

success：请求成功之后触发（响应状态码 200）

 error：请求失败触发

 complete：请求完成触发（不管成功与否）

$.get 
GET 请求快捷方法
$.post 
POST 请求快捷方法