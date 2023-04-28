##### 31.批量重命名

```
// 1.导入fs
const fs = require('fs');
// 2.读取code文件夹
const files = fs.readdirSync('./code2');
// console.log(files);
// 3.遍历数组
files.forEach(item => {
    // 拆分文件名
    let data = item.split('.');
    let [num, name] = data;
    // 判断
    if (Number(num) < 10) {
        num = '0' + num;
    }
    // 创建新的文件名
    let newname = num + '.' + name;
    // console.log(newname);
    // 重命名
    fs.renameSync(`./code2/${item}`, `./code2/${newname}`);//注意这个地方的单引号是波浪号下面的斜单引号，单引号不起作用
})
```

##### 32.path模块

```
path.resolve 拼接规范的绝对路径
path.sep 获取操作系统的路径分隔符
path.parse 解析路径并返回对象
path.basename 获取路径的基础名称
path.dirname 获取路径的目录名
path.extname 获取路径的扩展名
```

```
// 导入fs模块
const fs=require('fs');
const path =require('path');
// 写入文件
// fs.writeFileSync(__dirname+'/index.html','love');
// console.log(__dirname+'/index.html');//不规范

// console.log(path.resolve(__dirname,'./index.html'));
// console.log(path.resolve(__dirname,'index.html'));
// console.log(path.resolve(__dirname,'/index.html'));//C:\index.html

// sep 分隔符
// console.log(path.sep);//wendows \ Linux /

// parse 解析路径
// console.log(__filename);//文件的绝对路径
let str='C:\\Users\\33117\\Documents\\Daynote\\nodejs\\code\\32.PATH模块.js';
// console.log(path.parse(str));

// basename
// console.log(path.basename(str));

// dirname
// console.log(path.dirname(str));

// extname
console.log(path.extname(str));
```

##### 33.HTTP协议（超文本传输协议）

##### 浏览器-服务器

##### 34.HTTP报文

fiddler

访问一个网站会发送很多请求

##### 35.请求报文结构

请求行（第一行）

请求头（第二行到空行结束）

空行

请求体

##### 36.请求行

请求方法  url  HTTP版本号

请求方法：

```
GET 用于获取数据
POST 用于新增数据
PUT/PATCH 用于更新数据
DELETE 用于删除数据
HEAD/OPTIONS/CONNECT/TRACE 使用相对较少
```

url用来定位资源

协议名 ：//主机名 端口号 路径（资源） 查询字符串（额外的参数，要求）

##### 37.请求头

键值对组成

记录浏览器相关的信息，交互的行为（记住格式就可以了）

developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers(mdn对HTTP头信息的参考)

##### 38.请求体

格式很灵活

##### 39.HTTP响应报文

相应行

HTTP版本号 响应状态码 响应状态的描述

```
状态码 含义
200   请求成功
403   禁止请求
404   找不到资源
500   服务器内部错误
```

developer.mozilla.org/zh-CN/docs/Web/HTTP/Status(mdn对响应状态信息的参考)

响应头

空行

相应体

##### 40.响应头与响应体

响应头：与服务器相关的内容（有自定义的响应头）

响应体的内容格式非常灵活，常见的响应体格式有：

1.HTML

2.CSS

3.Javascript

4.图片

5.视频

6.JSON

##### 41.IP（数字标识，32bit 8bit一组） ->寻找网络设备

 ip本身是一个数字标识

ip用来标志网络设备，实现设备间通信

##### 42.IP的分类

共享IP（区域，家庭）->公网IP，广域网IP（除了下面的那些之外）

局域网IP可以被复用

192.168.0.0~192.168.255.255

172.16.0.0~172.31.255.255

10.0.0.0~10.255.255.255

本地回环IP地址 127.0.0.1 ->永远指向本机

127.0.0.1~127.255.255.254

##### 43.端口：实现不同主机应用程序之间的通信

应用程序的数字标识

一台计算机可以有65536个端口

##### 44.创建一个HTTP服务端

```
// 1.导入http模块
const http=require('http');
// 2.创建服务对象
const server=http.createServer((request,response)=>{
    response.end('Hello HTTP Server');//设置响应体
});
// 3.监听端口，启动服务
server.listen(9000,()=>{
    console.log('服务已经启动.....');
});
```

向9000这个端口发送请求

127.0.0.1:9000

##### 45.HTTP服务注意事项

①命令行Ctrl+c停止服务

②当服务启动后，更新代码必须重启服务器才能生效

③响应内容中文乱码的解决办法

```
response.setHeader('content-type','text/html;charset=utf-8');
```

④端口号被占用

```
Error：listen EADDRINUSE:address already in use :::9000
```

1)关闭当前正在运行监听端口的服务

2)修改其他端口号

⑤HTTP协议默认端口是80，

HTTPS协议的默认端口是443，

HTTP服务开发常用端口有3000,8080,8090,9000等

如果端口被其他程序占用，可以使用资源监视器找到占用端口的程序，然后使用任务管理器关闭对应的程序

##### 46.浏览器查看HTTP报文

shift+ctrl+i  ->网络-点击地址-标头

负载/载荷-查看请求体

响应-查看响应体

##### 47.提取HTTP请求报文

```
含义       语法
请求方法	request.methodI
请求版本	request.httpVersion
请求路径	request.url
URL路径		require('url').parse(request.url).pathname
URL查询字符串	require('url').parse(request.url, true).query
请求头		request.headers
请求体		request.on( 'data', function(chunk){}) <br/>
request.on('end"', function(){});
```

注意事项:
1. request.url 只能获取路径以及查询字符串，无法获取URL中的域名以及协议的内容
2. request.headers将请求信息转化成一个对象，并将属性名都转化成了『小写』
3. 关于路径:如果访问网站的时候，只填写了IP地址或者是域名信息，此时请求的路径为『/』
4. 关于favicon.ico:这个请求是属于浏览器自动发送的请求

```
// 1.导入http模块
const http=require('http');
// 2.创建服务对象
const server=http.createServer((request,response)=>{
    // 获取请求的方法
    // console.log(request.method);
    // 获取请求的url
    // console.log(request.url);//只包含url中的路径与查询字符串
    // 获取HTTP协议版本号
    // console.log(request.httpVersion);
    // 获取HTTP的请求头
    console.log(request.headers);//获取的为对象
    response.end('http');//设置响应体
   
});
// 3.监听端口，启动服务
server.listen(9000,()=>{
    console.log('服务已经启动.....');
});
```

##### 48.提取HTTP请求报文

```
// 1.导入http模块
const http=require('http');
// 2.创建服务对象
const server=http.createServer((request,response)=>{
    // 声明一个变量
    let body='';
    // 绑定时间
    request.on('data',chunk=>{
        body+=chunk;
    });
    // 绑定end事件
    request.on('end',()=>{
        console.log(body);
        response.end('Hello');
    });
   
   
});
// 3.监听端口，启动服务
server.listen(9000,()=>{
    console.log('服务已经启动.....');
});
```

##### 49.获取请求路径与查询字符串

```
// 1.导入http模块
const http = require('http');
// 导入url模块
const url = require('url');
// 2.创建服务对象
const server = http.createServer((request, response) => {
    // 解析request.url
    let res = url.parse(request.url,true);
    // console.log(res);
    // 路径
    // let pathname = res.pathname;
    // console.log(pathname);
    // 查询字符串
let keyword = res.query.keyword;
console.log(keyword);
    // console.log(request.url);

    response.end('url');
});
// 3.监听端口，启动服务
server.listen(9000, () => {
    console.log('服务已经启动.....');
});
```

##### 50.获取请求路径与查询字符串的另一种方式

```
// 1.导入http模块
const http = require('http');
// 导入url模块
// const url = require('url');
// 2.创建服务对象
const server = http.createServer((request, response) => {
    // 实例化url的对象
    // let url = new URL('http://www.xxx.com/search?a=100&b=200');
    let url=new URL(request.url,'http://127.0.0.1');
    // 输出路径
    console.log(url.pathname);
    // 输出keyword查询字符串
    console.log(url.searchParams.get('keyword'));
    response.end('url new');
});
// 3.监听端口，启动服务
server.listen(9000, () => {
    console.log('服务已经启动.....');
});
```

