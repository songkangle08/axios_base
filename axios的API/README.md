## 前置知识
- Promise
- Ajax

## json-server
- 包，快速启动服务,实现接口
- 安装：npm install json-server 
- 启动：json-server --watch db.json


## axios特点
- Make XMLHttpRequests from the browser  支持在浏览器和node使用
- Make http requests from node.js
- Supports the Promise API               支持Promise的API
- Intercept request and response         拦截请求和响应
- Transform request and response data    转换请求和响应数据
- Cancel requests                        取消请求
- Automatic transforms for JSON data     JSON数据的自动转换
- Client side support for protecting against XSRF  l客户端支持防止XSRF

## axios发送请求的方式
### axios.request(config)
```javascript
axios.request({
    method: 'GET',
    url: 'url地址'
}).then((res)=>{
    console.log(res);
})
```
### axios.get(url[,config])
### axios.delete(url[,config])
### axios.head(url[,config])
### axios.options(url[,config])
### axios.post(url[,data,[config]])
```javascript
axios.post(url,{body:"请求体"},{配置等等}).then((res)=>{
    console.log(res);
})
```
### axios.put(url[,config])
### axios.patch(url[,config])

## axios响应信息
```javascript
{
    "data":{
        "id":1,
        "title":"json-server",
        "author":"typicode"
    },
    "status":200,
    "statusText":"OK",
    "headers":{
        "cache-control":"no-cache",
        "content-length":"63",
        "content-type":"application/json; charset=utf-8",
        "expires":"-1","pragma":"no-cache"
    },
    "config":{
        "url":"http://localhost:3000/posts/1",
        "method":"get",
        "headers":{"Accept":"application/json, text/plain, */*"},
        "transformRequest":[null],
        "transformResponse":[null],
        "timeout":0,
        "xsrfCookieName":"XSRF-TOKEN",
        "xsrfHeaderName":"X-XSRF-TOKEN",
        "maxContentLength":-1,"maxBodyLength":-1
    },
    "request": XMLHttpRequest  基于XMLHttpRequest请求
}
```

## axios请求配置参数 
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

## axios默认配置
```javascript
axios.default.method = 'GET';   // 设置默认的请求类型GET
axios.default.baseURL = 'http://localhost:3000';    // 设置基础的url
axios.default.参数 = 配置值;
axios.default.timeout = 3000;
```

## axios创建实例对象
```javascript
const duanzi = axios.create({
    baseURL: 'https://localhost:3000/'
});

// 可以创建多个实例，向不同的服务器发送请求
const other = axios.create({
    baseURL: 'https://localhost:5000/'
});

duanzi({
    url: '/posts/1'
}).then((res)=>{
    console.log(res);
})
duanzi.get('/posts/1').then((res)=>{
    console.log(res);
})

// 一个项目向两个服务器发送请求，可以创建不同的实例，根据不同的实例向不同的服务器发送请求
other({
    url: '/posts/1'
}).then((res)=>{
    console.log(res);
})

// 这里axios.create与axios对象的功能基本上功能是一样的
```

## 拦截器 - interceptors
### 请求拦截器
- 发送请求前，对请求参数进行拦截处理
```javascript
// config 配置对象
axios.interceptors.request(function(config){
    // 可以修改config中的参数
    return config
},function(error){
    return Promise.reject(error);
})
```

### 响应拦截器
```javascript
// 设置响应拦截器
axios.interceptors.response(function(response){
    // 修改response
    return response
},function(error){
    return Promise.reject(error);
})
```

### axios取消请求
```javascript
const btns = document.querySelectorAll('button');
// 2. 声明全局变量
let cancel = null

// 第一个 - 获取数据
btns[0].onclick = function(){
    // 发送axios请求，get
    // 检测上一次的请求是否已经完成
    if(cancel){
        cancel();  //   取消上一下的取消
    }
    axios({
        method: 'GET',
        url: 'http://localhost:3000/posts/1',
        // 1.添加配置对象的属性
        cancelToken: new axios.cancelToken(function(c){
            // 3.将c的值赋值给cancel
            cancel = c;
            
        })
    }).then((res)=>{
        cancel = null;  // 将cancel的值设置成为null
        console.log(JSON.stringify(res));
    }).catch((err)=>{
        cancel = null;
    })
}
// 取消请求
btns[1].onclick = function(){
    // 发送axios请求，get
    cancel();
}
```


### 源码目录