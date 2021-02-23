## HTTP
[HTTP-MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/HTTP)

### HTTP请求交互的基本过程
![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e4839d31d03845bca7f3112d566a9a37~tplv-k3u1fbpfcp-watermark.image)
- 1.前后应用从浏览器端向服务器发送HTTP请求（请求报文）。
- 2.后台服务器接收到请求后，调度服务器应用处理请求，向浏览器端返回HTTP响应（响应报文）。
- 3.浏览器端接收到响应，解析显示响应体/调用监视回调。

### HTTP请求报文（浏览器交给服务器的）
#### 请求行
- method： 请求方式  （GET，POST）
- url：请求地址	（http://localhost:3000/）
- ......
#### 请求头
- HOST：	主机（域名）
- Cookie: Cookie  （浏览器发请求自动携带cookie）
- Content-Type:  内容的类型（application/x-www-from-urlencoded ｜ application/json）
	- 告诉服务请求体数据的格式的类型格式
- ......

#### 请求体
- urlencoded格式 		// username=1&pwd=123
- JSON格式			// {username:1,pwd:123}		

### HTTP响应报文
#### 响应状态行
- status：响应状态码    
	- 200，301，302，401，404，500等
- statusText：响应文本
#### 响应头
- Content-Type: text/html；charset=utf-8；  什么格式的文本
- Set-Cookie：设置cookie
#### 响应体
- html 文本/json 文本/js/css/图片

### 请求体参数格式
- Content-Type:application/x-www-from-urlencoded;charset=utf-8;  (urlencoded格式)
	- 用于键值对参数，参数的键值用=连接，参数之间用&连接
    - 例如：username=1&pwd=123
- Content-Type:application/json;charset=utf-8;	（json格式）
	- 用于json字符串参数
    - 例如：{name:"1",pwd:123}
- Content-Type:multipart/form-data;	
	- 用于文件上传请求
    
### 不同类型的请求及其作用
- GET： 从服务器读取数据
- POST：向服务器添加新数据
- PUT： 更新服务器端已有数据
- DELETE：删除服务器端数据
- OPTIONS: 预检请求（问一下服务器是否允许跨域啊等等）

### REST-API的分类
- REST-API：（restful）
	- 发送请求进行CRUD哪个操作由请求方式来决定
    - 同一个请求路径可以进行多个操作
    - 请求方式会用到GET/POST/PUT/DELETE
- 非REST-API：（restless）
	- 请求方式不决定请求的CRUD操作
    - 一个请求路径只对应一个操作
    - 一般只有GET/POST请求

## XHR的理解与使用
[XMLHttpRequest-MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest)

XMLHttpRequest（XHR）对象用于与服务器交互。通过 XMLHttpRequest 可以在不刷新页面的情况下请求特定 URL，获取数据。这允许网页在不影响用户操作的情况下，更新页面的局部内容。XMLHttpRequest 在 AJAX 编程中被大量使用。

### XHR的理解
- 使用XMLHttpRequest对象可以与服务器交互，也就是发送ajax请求
- 前端可以获取到数据，无需让整个的页面刷新
- 这使得Web页面可以只更新页面的局部，而不影响用户的操作。

### 区别一般的http请求与Ajax请求
- ajax请求是一种特别的http请求。
- 对服务器端来说，没有任何区别，区别在于浏览器端
- 浏览器端发送请求：只有XHR或fetch发出的才是ajax请求，其他所有的都是非ajax请求
- 浏览器端接收响应
	- 一般请求：浏览器一般会直接显示响应体数据，也就是我们常说的刷新/跳转页面
    - ajax请求：浏览器不会对界面进行任何的更新操作，只是调用监视的回调函数并传入响应相关数据
    
### XHR中的API
- XMLHttpRequest(): 创建XHR对象的构造函数
- status： 响应状态码，比如200，404等
- statusText：响应状态文本  
	- 200 ok
    - 404 not found
- readyState：标识请求状态的只读属性
	- 0: 初始
    - 1: open() 之后
    - 2: send()	之后
    - 3: 请求中
    - 4: 请求完成
- onreadystatechange： 绑定readyState改变的监听
- responseType： 指定响应数据类型，如果是**json**，得到响应后自动解析响应
- response：响应体数据，取决于responseType的指定
- timeout：指定请求超时时间，默认为0代码没有限制
- ontimeout：绑定超时的监听
- onerror: 绑定请求网络错误的监听
- open(): 初始化一个请求，参数为：（method，url[,saync（默认是异步:true）]）
- send(data): 发送请求
- abort(): 中断请求,中断响应结果
- getResponseHeader(name): 获取指定名称的响应头值
- getAllResponseHeaders(): 获取所有响应头组成的字符串
- setRequestHeader(name,value): 设置请求头
- load(): XMLHttpRequest请求成功完成时触发。也可以使用 onload 属性.
- loadend(): 当请求结束时触发, 无论请求成功 ( load) 还是失败 (abort 或 error)。也可以使用 onloadend 属性。

```javascript
let data = {}
const xhr = new XMLHttpRequest();
xhr.open('POST','http://localhost:3000/user',true);
xhr.onreadystatechange = function(){
  if(xhr.readyState === 4){
    	if(xhr.status === 200){
        	console.log(response)
        }
    }
}
xhr.send(data)
```

### 封装XHR请求，类似与axios
- 函数的返回值是promise，成功的结果为response，异常结果为error
- 能处理多种类型的请求： GET/POST/PUT/DELETE
- 函数的参数为一个配置对象
	```
    {
    	url: "",	// 请求地址
        method: "",	// 请求方式
        params:{},	// get/delete请求的query参数
        data:{},	// post或者delete请求的请求体参数
    }
    ```
- 响应json数据自动解析为js
```javascript
function myAjax({ url,method='GET',params={},data={} }){
    if(!url) return;
    method = method.toUpperCase();
    
    let queryString = '';
    let queryStringArr = [];
    // let method = 'GET';
    // let params = {name:1,id:1}
    // let url = "http://localhost:3000/user?name=1"
    if(method === 'GET'){
    	for(let key in params){
            if(params.hasOwnProperty(key)){
            	let str = key+'='+params[key];
            	queryStringArr.push(str)
            }
        }
    }
    if(queryStringArr.length!=0){
    	if(url.includes('?')){
            queryString = `&${queryStringArr.join('&')}`
        }else{
            queryString = `?${queryStringArr.join('&')}`
        }
    }
    url += queryString;
   
   // // 返回一个peomise
   retunr new Promise((resolve,reject)=>{
    	// 1. 执行异步ajax请求
        // 创建xhr对象
    	const xhr = new XMLHttpRequest();
        // 打开连接（初始化请求，没有请求）
        xhr.open(method,url,true);   // true为异步 false为同步
        // 绑定状态
        xhr.onreadystatechange = function(){
        	if(xhr,readyState !== 4) return;
        	if(xhr.readyState === 4){
            	// 如果响应状态码在[200,299)之间代码成功，否则失败(axios中的源码是这样写的)
            	if(xhr.status>=200 && xhr.status<300){
                    // 2.1 如果成功了，调用resolve()
                    resolve({
                    	statusText: xhr.statusText,
                        data: JSON.parse(xhr.response),
                        status: xhr.status
                    })
                }else{  
                    // 2.2 如果失败了，调用reject()
                    reject(new Error("request error status is "+ xhr.status))
                }
            }
        }
        // 发送请求
        if(method === 'GET' || method === 'DELETE'){
            xhr.send(null);
        }else if(method === 'POST' || method === 'PUT'){
            xhr.setRequestHeader('Content-Type':'application/json;charset=utf-8');     // 告诉请求头发送的是json格式的数据
            xhr.send(JSON.stringify(data));  // 发送json格式请求体
        } 
    })
}


myAjax({
    url: "/xxx",
    method: "",
    params:{},
    data:{},
}).then((res)=>{
    console.log(res)
},(error)=>{
    console.log(error.message)
})
```

## axios的理解与使用
[axios-源码-前端最流行的ajax请求库](https://github.com/axios/axios) 
### axios的特点
- 基于promise的异步ajax请求库
- 浏览器端/node端都可以使用
- 支持请求/响应**拦截器 interceptors**
- 支持请求取消
- 请求/响应 **数据转换 transformRequest｜ transformResponse**
- 批量发送多个请求

### axios常用语法
axios(config): 通常/最本质的发任意类型请求的方式
axios(url[,config]): 可以指定


### axios发送请求的方式
- axios.request(config)
```javascript
axios.request({
    method: 'GET',
    url: 'url地址'
}).then((res)=>{
    console.log(res);
})
```
- axios.get(url[,config])
- axios.delete(url[,config])
- axios.head(url[,config])
- axios.options(url[,config])
- axios.post(url[,data,[config]])
```javascript
axios.post(url,{body:"请求体"},{配置等等}).then((res)=>{
    console.log(res);
})
```
- axios.put(url[,config])
- axios.patch(url[,config])

### 常用语法API
- axios.defaults.xxx： 请求的默认全局对象
- axios.interceptors.request.use(): 添加请求拦截器
- axios.interceptors.response.use(): 添加响应拦截器

- axios.create([config]): 创建一个新的axios（它没有下面的功能）

- axios.Cancel(): 用于创建取消请求的错误对象
- axios.CancelToken(): 用于创建取消请求的token对象
- axios.isCancel(): 是否是一个取消请求的错误

- axios.all(promises): 用于批量执行多个异步请求
- axios.spread(): 用来指定接收所有成功数据的回调函数的方法


```javascript
import axios from 'axios'
// 指定默认配置
axios.defaults.baseURL = 'http://localhost:3000';

axios({
    url: 'http://localhost:3000/posts'
    method: 'get',
    params:{id: 1},
    data:{},
})
```

### axios请求配置参数 
```JavaScript
{
    url: '/user',                       // 请求路径
    method: '',                         // 请求方式
    baseURL: 'http://localhost:3000/',  // 请求的基本路径   内部自动将baseURL与url结合
    transformRequest: [function(data,headers){
        return data;
    }],     // 处理请求数据进行处理
    transformResponse: [function(data,headers){
        return data;
    }],     // 对响应的结果进行预处理，处理完成再进行发送
    headers:{ "X-Requested-With":"XMLHttpRequest" },    // 请求的头信息
    params:{
        ID: 12345
    },      // 设置url的参数  如http://localhost:3000/user?id=12345
    paramsSerializer: function(params){

    },
    data:{

    },      // 请求参数设置
    data: 'Country=Brasil&City=Belo Horizonte',  // url参数传递
    timeout: 1000, 
    withCredentials: false,         // 跨越请求，是否携带cookie
    adapter: function (config) {
        /* ... */
    },
    auth: {
        username: 'janedoe',
        password: 's00pers3cret'
    },                  //
    responseType: 'json',     // 响应体结果的格式
    responseEncoding: 'utf8', // 字符集设置
    xsrfCookieName: 'XSRF-TOKEN', // default

    // `xsrfHeaderName` is the name of the http header that carries the xsrf token value
    xsrfHeaderName: 'X-XSRF-TOKEN', // default

    onUploadProgress: function (progressEvent) {
        // Do whatever you want with the native progress event
    },          // 上传的回调

    maxContentLength: 2000,

    // `maxBodyLength` (Node only option) defines the max size of the http request content in bytes allowed
    maxBodyLength: 2000,
    validateStatus: function (status) {         
        return status >= 200 && status < 300; // default
    },          // 设置成功状态

}
```

## 难点语法的理解与使用
### axios.create(conig)
- 根据指定配置创建一个新的axios，也就是每个新的axios都有自己的配置
- 新的axios只是没有取消和批量发送请求的方法，其他语法都是一致的
- 返回一个函数，和axios的功能基本相同，也有不一样的
![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3a25f344f5d4429096d7ed4acfadddda~tplv-k3u1fbpfcp-watermark.image)

```javascript
// 针对不同的接口服务器请求
const instance1 = axios.create({
    baseURL: 'http://localhost:3000'
})
const instance2 = axios.create({
    baseURL: 'http://localhost:4000'
})

// 使用install发送请求
instance1({
    url: '/posts',		//  这个请求url为http://localhost:3000
})
instance1.get('/xxx')

instance2({
    url: '/user'		// 这个请求url为http://localhost:4000
})
instance2.post('/xxx',{id:1}))
```

#### 为什么要设计这个语法axios.create(conig)
- 需求：项目中有部分接口需要的配置与另一个部分接口需要的配置不太一样，如何处理？
- 解决：创建2个新的axios，每个都有自己特有的配置，分别应用到不同要求的接口请求中.

### 拦截器函数
- 请求拦截器 axios.interceptors.request.use()
	- 两个及两个请求拦截器执行顺序
- 响应拦截器 axios.interceptors.response.use()
	- 两个及两个响应拦截器执行顺序

```javascript
// 请求拦截器 - 1 (回调函数) 
axios.interceptors.request.use((congif)=>{
	console.log("request interceptors1")
	return congif;
},(error)=>{
	return Promise.reject(error);
})

// 请求拦截器 - 2 (回调函数) 
axios.interceptors.request.use((congif)=>{
	console.log("request interceptors2")
	return congif;
},(error)=>{
	return Promise.reject(error);
})

// 响应拦截器 - 1 
axios.interceptors.response.use((response)=>{
    console.log("response interceptors1")
    return response
},(error)=>{
    return Promise.reject(error);
})

// 响应拦截器 - 2 
axios.interceptors.response.use((response)=>{
    console.log("response interceptors1")
    return response
},(error)=>{
    return Promise.reject(error);
})


// 1. 先发送请求，发送的时候才执行
axios.get('http://localhost:3000/user/')
.then((response)=>{
	console.log(response)
},(error)=>{
	console.log(error)
})

```

#### 拦截器函数/ajax请求/请求的回调函数的调用顺序
- 1.发送请求
- 2.请求拦截器
	- 请求拦截器**后添加先执行，先添加后执行**
- 3.ajax请求
- 4.响应拦截器
	- 响应拦截器**先添加先执行，后添加后执行**
- 5.自己指定的回调函数

### 取消请求
#### 基本流程
- 1. 配置cancelToken对象
- 2. 缓存用于取消请求的cancel函数
- 3. 在后面特定的时机调用cancel函数取消请求，在错误回调中判断如果是error是cancel，做相对应的处理。

#### 实现功能

```html
<div class="container">
    <h2 class="page-header">axios取消请求</h2>
    <button class="btn btn-primary">发送请求1</button>
    <button class="btn btn-primary">发送请求2</button>
    <button class="btn btn-primary">取消请求</button>
</div>
<script src="https://cdn.bootcdn.net/ajax/libs/axios/0.21.1/axios.js"></script>
<script>
// 添加
axios.interceptors.request.use(function(config){
    if(cancel){
        cancel("取消请求");  //   取消上一下的取消
    }
   config.cancelToken = new axios.cancelToken(function(c){
        // c是用于取消当前请求的函数
        // 3.将c的值赋值给cancel
        cancel = c;
    })
    return config
});

axios.interceptors.response.use(function(resopnse){
	cancel = null;
	return resopnse;
},(error)=>{
    if(axios.isCancel(err)){
        console.log("请求1取消")
        // 中断peomise链
        return new Promise(()=>{})
    }else{	// 请求出错了
        console.log("请求1失败了")
        cancel = null;
        return Promise.reject(err)
    }
})


const btns = document.querySelectorAll('button');
// 2. 声明全局变量
let cancel = null;

// 第一个 - 获取数据
btns[0].onclick = function(){
    // 发送axios请求，get
    // 检测上一次的请求是否已经完成
    if(cancel){
        cancel("取消请求");  //   取消上一下的取消
    }
    axios({
        method: 'GET',
        url: 'http://localhost:3000/posts/1',
        // 1.添加配置对象的属性
        cancelToken: new axios.cancelToken(function(c){
            // c是用于取消当前请求的函数
            // 3.将c的值赋值给cancel
            cancel = c;
        })
    }).then((res)=>{
        cancel = null;  // 将cancel的值设置成为null
        console.log(JSON.stringify(res));
    }).catch((err)=>{
    	console.log('取消请求走这里了')
        if(axios.isCancel(err)){
        	console.log("请求1取消")
        }else{	// 请求出错了
            console.log("请求1失败了")
        	cancel = null;
        }
    })
}

// 第一个 - 获取数据
btns[1].onclick = function(){
    // 发送axios请求，get
    // 检测上一次的请求是否已经完成
    if(cancel){
        cancel("取消请求");  //   取消上一下的取消
    }
    axios({
        method: 'GET',
        url: 'http://localhost:3000/posts/2',
        // 1.添加配置对象的属性
        cancelToken: new axios.cancelToken(function(c){
            // c是用于取消当前请求的函数
            // 3.将c的值赋值给cancel
            cancel = c;
        })
    }).then((res)=>{
        cancel = null;  // 将cancel的值设置成为null
        console.log(JSON.stringify(res));
    }).catch((err)=>{
    	console.log('取消请求走这里了')
        if(axios.isCancel(err)){
        	console.log("请求1取消")
            // 
        }else{	// 请求出错了
            console.log("请求1失败了")
        	cancel = null;
        }
    })
}


// 取消请求
btns[2].onclick = function(){
    if(typeof cancel === 'function'){
	    cancel("请求取消");  // 请求取消，进入失败的回调
    }else{
        console.log("没有可取消的函数")
    }
} 
</script>
```
注意：**如果我是一个被取消的请求，cancel不能置为null，因为请求取消也会走失败的回调，失败的回调如果把cancel=null，同步的cancel赋值为函数之后又被赋值为null了。**

## axios源码分析
|_ /dist               输出
|_ /lib/               项目源码目录
  |_ /adapters/        定义请求的适配器 xhr， http
    |_ http.js         实现http适配器（包装http包）
    |_ xhr.js          实现xhr适配器（包装xhr对象）
  |_ /cancel           定义取消功能
  |_ /core             一些核心功能
    |_ Axios.js        axios的核心主类
    |_ dispatchRequest.js       用来调用http请求适配器方法发送请求的函数
    |_ InterceptorManager.js    拦截器的管理其
    |_ settle.js                根据http响应状态，改变promise的状态
  |_ /helpers          一些辅助方法
  |_ axios.js          对外暴露接口
  |_ default.js        axios的默认配置
  |_ utils.js          公共工具
|_
|_
|_
|_
|_ 
|_ package.json         项目信息
|_ index.d.ts           配置typeScript的声明文件
|_ index.js             入口文件


### axios与Axios的关系
- 1.从语法上来说： axios不是Axios的实例
- 2.从功能上来说：axios是Axios的实例
- 3.axios是Axios.prototype.request函数bind()返回的函数
- 4.axios作为对象有Axios原型上的所有方法，有Axios对象上所有的属性







