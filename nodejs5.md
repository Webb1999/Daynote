##### 62.网页中URL使用场景小总结

包括但不限于如下场景:

a标签 href

link标签href

script标签src

img标签src

 video audio标签src

form中的action

AJAX请求中的URL

##### 63.设置MIME类型

媒体类型(通常称为Multipurpose Internet Mail Extensions或MIME类型）是一种标准，用来表示文档、文件或字节流的性质和格式。
mime类型结构:[ type ] / [ subType]
例如:text/html text/css image/jpeg image/png application/json
HTTP服务可以设置响应头Content-Type来表明响应体的MIME类型，浏览器会根据该类型决定如何处理资源

下面是常见文件对应的mime类型

```
html : 'text/html' ,
css: 'text/css ' ,
js : 'text /javascript ' ,
png: 'image/png ' ,
jpg: 'image /jpeg ' ,
gif: 'image/gif' ,
mp4 : 'video /mp4' ,
mp3 : 'audio/mpeg ' ,
json : 'application/json '

```

对于未知的资源类型，可以选择 application/octet-stream类型，浏览器在遇到该类型的响应时，会对响应体内容进行独立存储，也就是我们常见的下载效果

```

const http = require('http');
const fs = require('fs');
const path = require('path');
// 声明一个变量
let mimes = {
    html: 'text/html',
    css: 'text/css ',
    js: 'text /javascript ',
    png: 'image/png ',
    jpg: 'image /jpeg ',
    gif: 'image/gif',
    mp4: 'video /mp4',
    mp3: 'audio/mpeg ',
    json: 'application/json '
}
const server = http.createServer((requset, response) => {
    let { pathname } = new URL(requset.url, 'http://127.0.0.1');
    // console.log(requset.url);
    let filepath = __dirname + '/page' + pathname;
    // 读取文件fs异步API

    response.setHeader('content-type', 'text/html;charset=utf-8');
    if (pathname === '/') {
        let html = fs.readFileSync(__dirname + '/page/index.html');
        response.end(html);
    } else {
        fs.readFile(filepath, (err, data) => {
            if (err) {
                response.statusCode = 500;
                response.end('文件读取失败');
                return;
            }
            // 获取文件的后缀名
            let ext = path.extname(filepath).slice(1);
            // console.log(ext);
            // 获取对应的类型
            let type=mimes[ext];
            if(type){
                console.log('匹配到了');
                response.setHeader('content-type',type);
            }else{
                // 没有匹配到
                response.setHeader('content-type','application/octet-stream');
            }
           
            response.end(data);
        })
    }

});
server.listen(9000, () => {
    console.log('服务已经启动.....');
});
```



##### 64.解决乱码问题

响应头的乱码设置优先级比meta标签优先级高

```

const http = require('http');
const fs = require('fs');
const path = require('path');
// 声明一个变量
let mimes = {
    html:'text/html',
    css:'text/css',
    js:'text/javascript',
    png:'image/png',
    jpg:'image/jpeg',
    gif:'image/gif',
    mp4:'video/mp4',
    mp3:'audio/mpeg',
    json:'application/json '
}
const server = http.createServer((requset, response) => {
    let { pathname } = new URL(requset.url, 'http://127.0.0.1');
    // console.log(requset.url);
    let filepath = __dirname + '/page' + pathname;
    // 读取文件fs异步API

    // response.setHeader('content-type', 'text/html;charset=utf-8');
    if (pathname === '/') {
        let html = fs.readFileSync(__dirname + '/page/index.html');
        response.end(html);
    } else {
        fs.readFile(filepath, (err, data) => {
            if (err) {
                response.statusCode = 500;
                response.end('文件读取失败');
                return;
            }
            // 获取文件的后缀名
            let ext = path.extname(filepath).slice(1);
            // console.log(ext);
            // 获取对应的类型
            let type = mimes[ext];
            if (type) {
                // console.log('匹配到了');
                if(ext==='html'){
                    response.setHeader('content-type', type + ';charset=utf-8');//里面不允许有空格或者其他的东西，要严格按照规范写
                }else{
                    response.setHeader('content-type', type );
                }

                
            } else {
                // 没有匹配到
                response.setHeader('content-type', 'application/octet-stream');
            }

            response.end(data);
        })
    }

});
server.listen(9000, () => {
    console.log('服务已经启动.....');
});
```

##### 65.完善错误处理

```

const http = require('http');
const fs = require('fs');
const path = require('path');
// 声明一个变量
let mimes = {
    html: 'text/html',
    css: 'text/css',
    js: 'text/javascript',
    png: 'image/png',
    jpg: 'image/jpeg',
    gif: 'image/gif',
    mp4: 'video/mp4',
    mp3: 'audio/mpeg',
    json: 'application/json '
}
const server = http.createServer((requset, response) => {
    if (requset.method !== 'GET') {
        response.statusCode = 405;
        response.end('<h1>405 Method Not Allowed</h1>');
        return;
    }
    let { pathname } = new URL(requset.url, 'http://127.0.0.1');
    // console.log(requset.url);
    let filepath = __dirname + '/page' + pathname;
    // 读取文件fs异步API

    // response.setHeader('content-type', 'text/html;charset=utf-8');
    if (pathname === '/') {
        let html = fs.readFileSync(__dirname + '/page/index.html');
        response.end(html);
    } else {
        fs.readFile(filepath, (err, data) => {
            if (err) {
                response.setHeader('content-type', 'text/html;charset=utf-8');
                switch (err.code) {
                    case 'ENOENT':
                        response.statusCode = 404;
                        response.end('<h1>404 Not Found</h1>');
                    case 'EPERM':
                        response.statusCode = 403;
                        response.end('<h1>403 Forbidden</h1>');//无读取权限

                };
                return;
            }
            // 获取文件的后缀名
            let ext = path.extname(filepath).slice(1);
            // console.log(ext);
            // 获取对应的类型
            let type = mimes[ext];
            if (type) {
                // console.log('匹配到了');
                if (ext === 'html') {
                    response.setHeader('content-type', type + ';charset=utf-8');//里面不允许有空格或者其他的东西，要严格按照规范写
                } else {
                    response.setHeader('content-type', type);
                }
            } else {
                // 没有匹配到
                response.setHeader('content-type', 'application/octet-stream');
            }
            response.end(data);
        })
    }
});
server.listen(9000, () => {
    console.log('服务已经启动.....');
});
```

##### 66.GET和POST场景区别

##### GET请求的情况:

 ①在地址栏直接输入url访问

②点击a链接
③link标签引入css

④script标签引入js
⑤video 与audio引入多媒体

⑥img标签引入图片
⑦form标签中的method 为get(不区分大小写)

⑧ajax中的get请求

##### POST请求的情况:

①form标签中的method为post(不区分大小写)

②AJAX的post请求

##### GET和POST是HTTP协议请求的两种方式，主要有如下几个区别

1．作用。GET主要用来获取数据，POST主要用来提交数据
2．参数位置。GET带参数请求是将参数缀到URL之后，POST带参数请求是将参数放到请求体中

3．安全性。POST请求相对GET安全一些，因为在浏览器中参数会暴露在地址栏

4.GET请求大小有限制，一般为2K，而POST请求则没有大小限制

##### 67.模块化介绍

1什么是模块化与模块﹖
将一个复杂的程序文件依据一定规则（规范)拆分成多个文件的过程称之为模块化
其中拆分出的每个文件就是一个模块，模块的内部数据是私有的，不过模块可以暴露内部数据以便其他模块使用

2什么是模块化项目?
编码时是按照模块一个一个编码的，整个项目就是一个模块化的项目
3模块化好处
下面是模块化的一些好处:

①防止命名冲突
②高复用性

③高维护性

##### 68.模块化初体验

```
me.js
// 声明一个函数
function tiemo(){
    console.log('贴膜....');
}
// 暴露数据
module.exports=tiemo;
```

```
index.js
// 导入模块
const tiemo=require('./me.js');
// 调用函数
tiemo();
```

##### 69.模块暴露数据

模块暴露数据的方式有两种:

1.module.exports = value

2.exports.name = valueI
使用时有几点注意:
 module.exports可以暴露任意数据
 不能使用exports = value的形式暴露数据，模块内部module与exports 的隐式关系exports =module .exports = {}

##### require返回的值是module.exports的值，并不是exports的值

```
// 声明一个函数
function tiemo(){
    console.log('贴膜....');
}
// 暴露数据
// module.exports=tiemo;

function xigua(){
    console.log('卖西瓜...');
}
// 暴露数据方式一
// module.exports={
//     tiemo:tiemo,//可以简写
//     // tiemo,
//     xigua
// }
// 暴露数据方式二
// exports.tiemo=tiemo;
// exports.xigua=xigua;

// module.exports可以暴露任意数据
// module.exports='iloveyou';
// module.exports=521;

// 不能使用exports = value的形式暴露数据
// exports='i love you';//会返回{}

// module.exports===exports(true)
// exporets=module.exports={}
// 就算改变了exports的值，module.exports的值也不会改变
```

##### 70.nodejs导入模块

```
在模块中使用require传入文件路径即可引入文件
const test = require(  ./me.js ');
require使用的一些注意事项:
1.对于自己创建的模块，导入时路径建议写们对路径，且不能省略./和../
2. js 和 json文件导入时可以不用写后缀，c/c++编写的node扩展文件也可以不写后缀，但是一般用不到
3.如果导入其他类型的文件，会以js文件进行处理
4．如果导入的路径是个文件夹，则会首先检测该文件夹下 package . json 文件中 main属性对应的文件,
如果main 属性不存在，或者package.json 不存在，则会检测文件夹下的 index.js 和index.json ,如果还是没找到，就会报错
5．导入node.js内置模块时，直接require模块的名字即可，无需加 ./和../
①. require还有一种使用场景，会在包管理工具章节介绍
②. module . exports . exports 以及require这些都是 CommonJS模块化规范中的内容，而 Node.js 实现了CommonJS模块化规范

```

##### 72.require导入的基本流程

1.将相对路径转为绝对路径，定位目标文件
2．缓存检测
3，读取目标文件代码
4，包裹为一个函数并执行(自执行函数)。通过arguments.callee.toString()查看自执行函数

5.缓存模块的值
6．返回module . exports的值

```
// 伪代码

function require(file,){
// 1,将相对路径转换为绝对路径
let absolutePath= path.resolve(__dirname,file);
// 2.缓存检测
if(caches[absolutePath]){
    return caches[absolutePath];
    // 3.读取文件的代码
    let code =fs.readFileSync(absolutePath).toString();
    // 4.包裹为一个函数，然后执行
    let module={};
    let exports=module.exports={};
   (function(exports,require,module,__filename,__dirname){
        const test={
            name:'haha'
        }
        module.exports=test;
        // 输出
        console.log(arguments.callee.toString());
    })(exports,require,module,__filename,__dirname)
    // 5.缓存结果
    caches[absolutePath]=module.exports;
    // 6.返回exports的值
    return module.exports;
}
}

const m =require('./me');
```

##### 73.CommonJS模块化规范

module .exports . exports 以及require这些都是CommonJS模块化规范中的内容。而Node.js是实现了CommonJS模块化规范，二者关系有点像JavaScript与 ECMAScript

##### 74.包管理工具

①包是什么
『包』英文单词是 package，代表了一组特定功能的源码集合

②包管理工具
管理『包』的应用软件，可以对「包」进行下载安装，更新，删除，上传等操作借助包管理工具，可以快速开发项目，提升开发效率
包管理工具是一个通用的概念，很多编程语言都有包管理工具，所以掌握好包管理工具非常重要

③常用的包管理工具下面列举了前端常用的包管理工具
 npm！！！！
 yarn
 cnpm

##### 75.npm介绍与安装

npm 全称 Node Package Manager，翻译为中文意思是『Node的包管理工具』npm是node.js官方内置的包管理工具，是必须要掌握住的工具

node.js在安装时会自动安装 npm，所以如果你已经安装了node.js，可以直接使用npm可以通过npm -v查看版本号测试，如果显示版本号说明安装成功，反之安装失败

##### 76.npm初始化包

创建一个空目录，然后以此目录作为工作目录启动命令行工具，执行npm init

npm init命令的作用是将文件夹初始化为一个『包』，交互式创建 package.json文件package . json是包的配置文件，每个包都必须要有package .json
package . json内容示例:

```
{
"name" : "01_npm",			#包的名字
"version" : "1.0.e" ,		#包的版本
" description" : ""，		#包的描述
"main" : "index . js"，		#包的入口文件
"scripts" : {
"test" : "echo \"Error: no test specified\" 8& exit 1""
}，
"author" : ""，				#作者
license" : "ISC"			#开源证书
}
```

初始化的过程中还有一些注意事项:
1. package name(包名)不能使用中文、大写，默认值是文件夹的名称，所以文件夹名称也不能使用中文和大写
2. version(版本号)要求x.x.x的形式定义，x必须是数字，默认值是1.0.0
3.  ISC证书与MIT证书功能上是相同的,关于开源证书扩展阅读
  http:/www.ruanyifeng.com/blog/2011/05/how to choose free software licenses.html
4. package . json可以手动创建与修改
5. 使用npm init -y或者npm init --yes极速创建package . json