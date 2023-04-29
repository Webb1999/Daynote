##### 51.HTTP请求练习

```
请求类型  请求地址  响应体结果
get		/login    登录页面
get		/reg 	  注册页面
```

```
const http=require('http');
const server=http.createServer((requset,response)=>{
    // 获取请求的方法
    // let method=requset.method;
    let {method}=requset;
    // 获取请求的路径
    let {pathname}=new URL(requset.url,'http://127.0.0.1');
    response.setHeader('content-type','text/html;charset=utf-8');
    if(method==='GET' && pathname==='/login'){
        response.end('登录页面');
    }else if(method==='GET' && pathname==='/reg'){
        response.end('注册页面');
    }else{
        response.end('Not Found');
    }
    // console.log(method);
    // console.log(pathname);
    // response.end('practise');

});
server.listen(9000,()=>{
    console.log('服务已经启动.....');
});
```

##### 52.设置HTTp响应报文

```
设置响应状态码   response.statusCode
设置响应状态描述   response.statusMessage(用的非常少)
设置响应头信息    response.setHeader('头名'，‘头值’)
设置响应体 		response.write('xx') <br />
				response.end('xxx')

write和 end 的两种使用情况:
//1. write和 end 的结合使用 响应体相对分散
response.write( 'xx ') ;
response.write( 'xx ') ;
response.write( 'xx' );
response.end();//每一个请求，在处理的时候必须要执行end方法的

//2．单独使用end方法响应体相对集中
response . end( ' xx×’);

```

```
const http=require('http');
const server=http.createServer((requset,response)=>{
    // 获取请求的方法
    // let method=requset.method;
    let {method}=requset;
    // 获取请求的路径
    let {pathname}=new URL(requset.url,'http://127.0.0.1');
    response.setHeader('content-type','text/html;charset=utf-8');
    if(method==='GET' && pathname==='/login'){
        response.end('登录页面');
    }else if(method==='GET' && pathname==='/reg'){
        response.end('注册页面');
    }else{
        response.end('Not Found');
    }
    // console.log(method);
    // console.log(pathname);
    // response.end('practise');

});
server.listen(9000,()=>{
    console.log('服务已经启动.....');
});
```

##### 53.HTTP响应练习

搭建一个HTTP服务，响应一个4行3列的表格，并且要求有隔行换色效果，点击某个单元格能高亮显示

```

const http = require('http');
const server = http.createServer((requset, response) => {
    response.end(`
    <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
    td{
     padding: 20px 40px;   
    }
    table tr:nth-child(odd){
        background-color: #bfa;
    }
    table,td{
        border-collapse: collapse;
    }
</style>
<script>
window.onload=function(){
    let tds=document.querySelectorAll('td');
    tds.forEach(item=>{
        item.onclick=function(){
            this.style.backgroundColor="yellow"
        }
      
    })
};
</script>
</head>

<body>
<table border="1">
<tr>
    <td>1</td>
    <td>2</td>
    <td>3</td>
</tr>
<tr>
    <td>4</td>
    <td>5</td>
    <td>6</td>
</tr>
<tr>
    <td>7</td>
    <td>8</td>
    <td>9</td>
</tr>
</table>
</body>
</html>
`);


});
server.listen(9000, () => {
    console.log('服务已经启动.....');
});
```

##### 54.响应文件内容

```
----对上一个练习的优化（读取文件）
const http = require('http');
const fs =require('fs');
const server = http.createServer((requset, response) => {
    let html=fs.readFileSync(__dirname + '/from.html');
    response.end(html);


});
server.listen(9000, () => {
    console.log('服务已经启动.....');
});
```

##### 55.网页资源加载基本过程

资源加载并行处理

##### 56.HTTP响应练习扩展

css的响应是html

js的响应也是html

```

const http = require('http');
const fs =require('fs');
const server = http.createServer((requset, response) => {
    let {pathname}=new URL(requset.url,'http://127.0.0.1');
    // console.log(requset.url);
    if(pathname==='/'){
        let html=fs.readFileSync(__dirname + '/from.html');
        response.end(html);
    }else if(pathname==='/from.css'){
        let css=fs.readFileSync(__dirname+'/from.css');
        response.end(css);
    }else if(pathname==='/from.js'){
        let js=fs.readFileSync(__dirname+'/from.js');
        response.end(js);
    }else{
        response.statusCode=404;
        response.end('Not Found');
    }
    
    


});
server.listen(9000, () => {
    console.log('服务已经启动.....');
});
```

##### 57,静态资源与动态资源

静态资源是指内容长时间不发生改变的资源，例如图片，视频，CSS文件，Js文件，HTML文件，字体文件等

动态资源是指内容经常更新的资源，例如百度首页，网易首页，京东搜索列表页面等

##### 58.搭建静态资源服务

创建一个HTTP 服务，端口为900日，满足如下需求

GET/index.html			响应page/index.html的文件内容
GET/css/app.css			响应page/css/ app.css的文件内容
GET/images/logo.png	响应page/images/ logo.png的文件内容

```


const http = require('http');
const fs =require('fs');
const server = http.createServer((requset, response) => {
    let {pathname}=new URL(requset.url,'http://127.0.0.1');
    console.log(requset.url);
    let filepath =__dirname+'/page'+pathname;
    // 读取文件fs异步API

    response.setHeader('content-type','text/html;charset=utf-8');
   if(pathname==='/'){
    let html=fs.readFileSync(__dirname + '/page/index.html');
    response.end(html);
   }else{
    fs.readFile(filepath,(err,data)=>{
        if(err){
            response.statusCode=500;
            response.end('文件读取失败');
            return;
        }
        response.end(data);
    })
   }
    
});
server.listen(9000, () => {
    console.log('服务已经启动.....');
});
```

##### 59.静态资源目录与网站根目录

HTTP服务在哪个文件夹中寻找静态资源，那个文件夹就是静态资源目录，也称之为网站根目录
思考: vscode中使用live-server访问HTML时，它启动的服务网站根目录是谁?

以打开的文件架为根目录

##### 60.网页URL之绝对路径

网页中的URL
网页中的URL主要分为两大类:	相对路径与绝对路径12.2.1绝对路径
绝对路径可靠性强，而且相对容易理解，在项目中运用较多
形式												特点
http://atguigu.com/web			直接向目标资源发送请求，容易理解。网站的外链会用到此形式
//atguigu.com/web						与页面URL的协议拼接形成完整URL再发送请求。大型网站用的比较多
/web												与页面URL的协议、主机名、端口拼接形成完整URL再发送请求。中小型网站

##### 60.网页URL之相对路径

相对路径在发送请求时，需要与当前页面URL路径进行计算，得到完整URL后，再发送请求，学习阶段用的较多例如当前网页url为http://www.atguigu.com/course/h5.html

形式							最终的URL
./css/app.css				http://www.atguigu.com/course/css/app.css
js/app.js						http://www.atguigu.com/course/js/app.js
../img/logo.png			http://www.atguigu.com/img/logo.png
../../mp4/show.mp4	http://www.atguigu.com/mp4/show.mp4

相对路径不可靠，以当前根目录作参考