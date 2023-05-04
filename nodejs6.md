##### 77.npm搜索包

搜索包的方式有两种
1.命令行『npm s/search关键字』
2．网站搜索网址是https:// www.npmjs.com/

##### 78.下载安装包

我们可以通过npm install和npm i命令安装包
#格式
npm install <包名>

npm i<包名>
#示例
npm install uniq

npm i uniq
运行之后文件夹下会增加两个资源
①node_modules文件夹存放下载的包
②package-lock .json包的锁文件，用来锁定包的版本
安装uniq之后，uniq就是当前这个包的一个依赖包，有时会简称为依赖
比如我们创建一个包名字为A，A中安装了包名字是B，我们就说B是A的一个依赖包，也会说A 依赖B

```
// 导入uniq包
const uniq=require('uniq');
// 使用函数
let arr =[1,2,3,4,5,5,4,3,2,1];
const result=uniq(arr);
console.log(result);
```

##### 79.require导入npm包的基本流程

1．在当前文件夹下node_modules中寻找同名的文件夹
2．在上级目录中下的node_modules中寻找同名的文件夹，直至找到磁盘根目录

##### 80.开发依赖与生产依赖

开发环境是程序员专门用来写代码的环境，一般是指程序员的电脑，开发环境的项目一般只能程序员自己访问生产环境是项目代码正式运行的环境，一般是指正式的服务器电脑，生产环境的项目一般每个客户都可以访问

##### 生产依赖与开发依赖

我们可以在安装时设置选项来区分依赖的类型，目前分为两类:
类型								命令							补充
生产依赖				npm i -S uniq /jquery		-S等效于--save,-S是默认选项
								npm i --save uniq			包信息保存在package.json中 dependencies属性
开发依赖					npm i -D less				-D等效于--save-dev
								npm i --save-dev less		包信息保存在package.json中 devDependencies属性
举个例子方便大家理解，比如说做蛋炒饭需要大米，油，葱，鸡蛋，锅，媒气，铲子等其中锅，媒气，铲子属于开发依赖，只在制作阶段使用
而大米，油，葱，鸡蛋属于生产依赖，在制作与最终食用都会用到
所以开发依赖是只在开发阶段使用的依赖包，而生产依赖是开发阶段和最终上线运行阶段都用到的依赖包

##### 81.npm全局安装

我们可以执行安装选项-g,进行全局安装
npm i -g nodemon
全局安装完成之后就可以在命令行的任何位置运行nodemon命令该命令的作用是自动重启node应用程序

```
node 文件名		换为
nodemon 文件名
在每次代码修改后，服务会自动重启不用再手动将之前开启的服务关闭再开启
```

说明:
。全局安装的命令不受工作目录位置影响
。可以通过npm root -g 可以查看全局安装包的位置
。不是所有的包都适合全局安装，只有全局类的工具才适合，可以通过查看包的官方文档来确定安装方式，这里先不必太纠结

##### 82.修改windows执行策略

1.以管理员身份打开 powershell命令行

2.键入命令set-ExecutionPolicy remoteSigned

##### 83.环境变量path

##### 84.npm安装所有依赖

在项目协作中有一个常用的命令就是 npm i，通过该命令可以依据package . json 和package-lock. json的依赖声明安装项目依赖
npm i
npm install
---node_modules文件夹大多数情况都不会存入版本库

##### 85.npm安装指定版本包和npm删除包

项目中可能会遇到版本不匹配的情况，有时就需要安装指定版本的包，可以使用下面的命令的
##格式
npm i < <包名@版本号> >

##示例
npm i jquery@1. 11.2

-----删除依赖
项目中可能需要删除某些不需要的包，可以使用下面的命令
##局部删除
npm remove uniq

npm r uniq
##全局删除
npm remove -g nodemon

##### 86.npm配置命令别名

通过配置命令别名可以更简单的执行命令
配置 package.json中的scripts属性

```
{
。
。

"scripts" : {
		" server" : "node server .js" ，
		" start" : "node index.js"，}，
。
。
}

```


----配置完成之后，可以使用别名执行命令
npm run server
npm run start
不过start别名比较特别，使用时可以省略run
npm start
----补充说明:
 npm start是项目中常用的一个命令，一般用来启动项目
npm run有自动向上级目录查找的特性，跟require函数也一样
对于陌生的项目，我们可以通过查看scripts属性来参考项目的一些操作

##### 87.cnpm介绍

cnpm是一个淘宝构建的 npmjs.com 的完整镜像，也称为『淘宝镜像』，网址https://npmmirror.com/cnpm 服务部署在国内阿里云服务器上，可以提高包的下载速度
官方也提供了一个全局工具包 cnpm，操作命令与npm大体相同
---安装
我们可以通过npm来安装cnpm工具
npm install -g cnpm --registry=https : //registry.npmmirror.com
---操作命令
功能								命令
初始化						cnpm init  /  cnpm init

安装包						cnpm i uniq
									cnpm i -s uniq
									cnpm i -D uniq

​									cnpm i -g nodemon
安装项目依赖			cnpm i
删除							cnpm r uniq

##### 88.npm配置淘宝镜像

用npm也可以使用淘宝镜像，配置的方式有两种
·直接配置
·工具配置
----直接配置
执行如下命令即可完成配置
npm config set registry https : //registry .npmmirror.com/
---工具配置
使用nrm 配置npm 的镜像地址npm registry manager

1.安装nrm
npm i -g nrm
2．修改镜像
nrm use taobao
3.检查是否配置成功(选做)
npm config list
检查registry地址是否为https://registry.npmmirror.com/ ,如果是则表明成功
补充说明:
1.建议使用第二种方式进行镜像配置,因为后续修改起来会比较方便

2．虽然cnpm可以提高速度，但是npm也可以通过淘宝镜像进行加速，所以 npm的使用率还是高于cnpm

##### 89.yarn介绍

----yarn是由 Facebook在2016年推出的新的Javascript 包管理工具，官方网址: https://yarnpkg.com/

-----yarn官方宣称的一些特点
速度超快: yarn缓存了每个下载过的包，所以再次使用时无需重复下载。同时利用并行下载以最大化资源利用率，因此安装速度更快
超级安全:在执行代码之前，yarn会通过算法校验每个安装包的完整性
超级可靠:使用详细、简洁的锁文件格式和明确的安装算法，yarn 能够保证在不同系统上无差异的工作

-----我们可以使用npm安装yarn
npm i -g yarn

----yarn常用命令
功能               					命令
初始化							yarn init  	/	yarn init -y
安装包							yarn add uniq		生产依赖
										yarn add less --dev		开发依赖
										yarn global add nodemon		全局安装
删除包							yarn remove uniq		删除项目依赖包
										yarn global remove nodemon		全局删除包
安装项目依赖					yarn
运行命令别名					yarn<别名>			#不需要添加run

------这里有个小问题就是全局安装的包不可用，yarn 全局安装包的位置可以通过yarn global bin `来查看，
那你有没有办法使yarn全局安装的包能够正常运行?(需要配置环境变量)

##### 90.npm和yarn的选择

-----yarn配置淘宝镜像
可以通过如下命令配置淘宝镜像
yarn config set registry https://registry.npmmirror.com/

可以通过yarn config list查看yarn的配置项
-----npm和yarn选择
大家可以根据不同的场景进行选择

1.个人项目
如果是个人项目，哪个工具都可以，可以根据自己的喜好来选择

2．公司项目
如果是公司要根据项目代码来选择，可以通过锁文件判断项目的包管理工具------onpm的锁文件为package-lock . json
-----oyarn的锁文件为yarn.lock

##### 包管理工具不要混着用，切记，切记，切记

##### 91.npm发布一个包

----创建与发布
我们可以将自己开发的工具包发布到npm服务上，方便自己和其他开发者使用，操作步骤如下:

1，创建文件夹，并创建文件index.js，在文件中声明函数，使用module.exports 暴露

2.npm初始化工具包，package.json填写包的信息(包的名字是唯一的)
3.注册账号 https://www.npmjs.com/signup
4．激活账号(一定要激活账号)		填写验证码
5．修改为官方的官方镜像(命令行中运行nrm use npm )

6．命令行下npm login填写相关用户信息
7．命令行下npm publish提交包仓
---更新包
后续可以对自己发布的包进行更新，操作步骤如下

1、更新包中的代码
2．测试代码是否可用
3．修改package . json中的版本号

4．发布更新
npm publish

---删除包

执行如下命令删除包
npm unpublish
删除包需要满足一定的条件，https://docs.npmjs.com/policies/unpublish·你是包的作者
发布小于24小时
大于24小时后，没有其他包依赖，并且每周小于300下载量，并且只有一个维护者

##### 93.包管理工具扩展介绍

在很多语言中都有包管理工具，比如:
语言						包管理工具
PHP						composer
Python					pip
Java						maven
Go							go mod
JavaScript				npm/yarn/cnpm/other
Ruby						rubyGems
除了编程语言领域有包管理工具之外，操作系统层面也存在包管理工具，不过这个包指的是『软件包』
操作系统						包管理工具						网址
Centos						yum					https://packages.debian.org/stable/
Ubuntu						apt					https://packages.ubuntu.com/
MacOS					homebrew			https://brew.sh/
Windows				chocolatey			https://chocolatey.org/

##### 94.nvm介绍

---nvm全称 Node Version Manager顾名思义它是用来管理node版本的工具，方便切换不同版本的Node.js

---使用

nvm 的使用非常的简单，跟npm 的使用方法类似
---下载安装
首先先下载 nvm，下载地址 https://github.com/coreybutler/nvm-windows/releases ,选择nvm-setup.exe下载即可(网络异常的小朋友可以在资料文件夹中获取)
---常用命令
命令									说明
nvm list available			显示所有可以下载的 Nodle.js 版本
nvm list							显示已安装的版本
nvm install18.12.1		安装18.12.1版本的 Node.js
nvm install latest			安装最新版的Node.js
nvm uninistall 18.12.1	删除某个版本的Node.js
nvm use 18.12.1				切换18.12.1的 Node.js

##### 95.express框架介绍

express是一个基于Node.js平台的极简、灵活的WEB应用开发框架，官方网址: https://www.expressjs.com.cn/简单来说，express是一个封装好的工具包，封装了很多功能，便于我们开发WEB应用(HTTP服务)

-----express下载
express本身是一个npm包，所以可以通过npm安装

##### 96.express初体验

```
// 1.导入express
const express=require('express');
// 2.创建应用对象
const app =express();
// 3.创建路由
app.get('/home',(req,res)=>{
    res.end('hello express');
});
// 4.监听端口，启动服务
app.listen(3000,()=>{
    console.log('服务已经启动,端口3000正在监听');
}
)
```

##### 97.express路由

```
---什么是路由
官方定义:路由确定了应用程序如何响应客户端对特定端点的请求
---路由的使用
一个路由的组成有请求方法，路径和回调函数组成
express中提供了一系列方法，可以很方便的使用路由，使用格式如下:
app.<method>(path. callback)
代码示例:
```

```
// 1.导入express
const express=require('express');
// 2.创建应用对象
const app =express();
// 3.创建路由(请求方法为get并且请求路径为home，就执行)
app.get('/home',(req,res)=>{
    res.end('hello express');
});
// app.get('/',(req,res)=>{
//     res.end('home');
// });
app.post('/login',(req,res)=>{
    res.end('post');
});
app.all('/text',(req,res)=>{
    res.end('all');
});
// 404响应
app.all('*',(req,res)=>{
    res.end('404 Not Found');
});
// 4.监听端口，启动服务
app.listen(3000,()=>{
    console.log('服务已经启动,端口3000正在监听');
}
)
```

##### 99.获取请求报文参数（express）

--获取请求参数
express框架封装了一些API来方便获取请求报文中的数据，并且兼容原生HTTP模块的获取方式

```
// 1.导入express
const express=require('express');
// 2.创建应用对象
const app =express();
// 3.创建路由(请求方法为get并且请求路径为home，就执行)
app.get('/request',(req,res)=>{
    // console.log(req.method);
    // console.log(req.url);
    // console.log(req.httpVersion);
    // console.log(req.headers);

    // // express操作
    // console.log(req.path);
    // console.log(req.query);
    // // 获取ip
    // console.log(req.ip);
    // 获取请求头
    console.log(req.get('host'));
    res.end('hello express');
});

// 4.监听端口，启动服务
app.listen(3000,()=>{
    console.log('服务已经启动,端口3000正在监听');
}
)
```

##### 100.获取路由参数

```
// 1.导入express
const express=require('express');
// 2.创建应用对象
const app =express();
// 3.创建路由(请求方法为get并且请求路径为home，就执行)
app.get('/:id.html',(req,res)=>{
    // 获取URL路由参数
    console.log(req.params.id);
    res.setHeader('content-type','text/html;charset=utf-8');
    res.end(' 商品详情');
});

// 4.监听端口，启动服务
app.listen(3000,()=>{
    console.log('服务已经启动,端口3000正在监听');
}
)
```

##### 101.路由参数练习

---根据路由参数响应歌手的信息
路径结构如下
/singer /1.htmlI
显示歌手的姓名和图片

```
// 1.导入express
const express=require('express');
// 导入json文件
const {singers}=require('./singers.json');
// console.log(singers);//获取的为数组，带{}的是对象
// 2.创建应用对象
const app =express();
// 3.创建路由(请求方法为get并且请求路径为home，就执行)
app.get('/singer/:id.html',(req,res)=>{
    // 获取路由参数
    let {id}=req.params;
    // 在数组中寻找对应id的数据
    let result = singers.find(item=>{
        if(item.id===Number(id)){
            return true;
        }
    });
    // 判断
    if(!result){
        res.statusCode=404;
        res.end('404 Not Found');
        return;
    }
    // console.log(result);
    res.setHeader('content-type','text/html;charset=utf-8');
    res.end(
        `<!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta http-equiv="X-UA-Compatible" content="IE=edge">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Document</title>
        
        </head>
        <body>
           <h2>${result.singer_name}</h2>
           <img src='${result.singer_pic}' />
        </body>
        </html>`
    );
});

// 4.监听端口，启动服务
app.listen(3000,()=>{
    console.log('服务已经启动,端口3000正在监听');
}
)
```

102、一般响应设置

express框架封装了一些API来方便给客户端响应数据，并且兼容原生HTTP模块的获取方式

```
//获取请求的路由规则
app. get( "/response", ( req,res) => {
/ /1. express中设置响应的方式兼容HTTP模块的方式
res.statusCode = 484;
res.statusMessage = "xxx”;
res.setHeader ( " abc " , 'xyz ');
res.write('响应体');
res.end( " xxx ');

//2. express 的响应方法
res.status ( 588) ; //设置响应状态码
res.set ( "xxx ' , "yyy " );//设置响应头
res.send( '中文响应不乱码");//设置响应体
//连贯操作
res.status ( 404).set( 'xxx " , ' yyy ' ).send("你好朋友')

//3．其他响应
res.redirect( ' http: / /atguigu .com ')//重定向res.download( ' . / package . json ' );1/下载响应
res. json();//响应JSON
res.sendFile( --dirname + ' / home . html')1/响应文件内容
}) ;
```

```
// 1.导入express
const express = require('express');
// 2.创建应用对象
const app = express();
// 3.创建路由(请求方法为get并且请求路径为home，就执行)
app.get('/other', (req, res) => {
    // 获取URL路由参数
    // 原生响应
    // res.statusCode=404;
    // res.statusMessage='love';
    // res.setHeader('xxx','yyy');
    // res.write('hello express');

    // express响应
    // res.status(500);
    // res.set('aaa', 'bbb');
    // res.send('你好 Express');
    //连贯操作
    // res.status(404).set('xxx','yyy').send('你好朋友');

    // 其他响应
    // 跳转响应（重定向）
    // res.redirect('http://atguigu.com');
    // 下载响应
    // res.download(__dirname+'/package.json');
    // json响应
    // res.json({
    //     name:'尚硅谷',
    //     slogon:'让天下没有难学的技术'
    // })
    // 响应文件内容
    res.sendFile(__dirname+'/from.html');
    // res.end(' response');
});

// 4.监听端口，启动服务
app.listen(3000, () => {
    console.log('服务已经启动,端口3000正在监听');
}
)
```

##### 104.中间件介绍

---什么是中间件
中间件(Middleware）本质是一个回调函数
中间件函数可以像路由回调一样访问请求对象(request)，响应对象（response)

---中间件的作用
中问件的作用就是使用函数封装公共操作，简化代码

---中间件的类型
·全局中间件
·路由中间件
-------定义全局中间件
每一个请求到达服务端之后都会执行全局中间件函数声明中间件函数
let recordMiddleware = function( request, response , next){
//实现功能代码
// .. ...
//执行next函数(当如果希望执行完中间件函数之后，仍然继续执行路由中的回调函数，必须调用next)

next();

}

##### 105.全局中间件实践

```
// 1.导入express
const express = require('express');
const fs = require('fs');
const path = require('path');
// 2.创建应用对象
const app = express();
// 声明中间件函数
function recordMiddleware(req,res,next){
     // 获取url和ip
     let { url, ip } = req;
     console.log(url, ip);
     // 将信息保存在文件中
     fs.appendFileSync(path.resolve(__dirname, './access.log'), `${url} ${ip}\r\n`);
     res.setHeader('content-type', 'text/html;charset=utf-8');
    //  调用next
    next();
}
// 访问日志
app.use(recordMiddleware);
// 3.创建路由(请求方法为get并且请求路径为home，就执行)
app.get('/home', (req, res) => {
   
    // res.setHeader('content-type', 'text/html;charset=utf-8');
    res.end('前台首页');
});

app.get('/admin', (req, res) => {
    // res.setHeader('content-type', 'text/html;charset=utf-8');
    res.end('后台首页');
})
app.get('*', (req, res) => {

    res.end('404 Not Found');
})
// 4.监听端口，启动服务
app.listen(3000, () => {
    console.log('服务已经启动,端口3000正在监听');
}
)
```

##### 106.路由中间件实践

针对/admin/setting 的请求，要求URL携带 code=521参数，如未携带提示『暗号错误』

```

// 1.导入express
const express = require('express');

// 2.创建应用对象
const app = express();

// 声明中间件
let checkCodeMiddleware = (req, res, next) => {
    if (req.query.code === '521') {
        res.setHeader('content-type', 'text/html;charset=utf-8');
        next();
    } else {
        res.setHeader('content-type', 'text/html;charset=utf-8');
        res.end('暗号错误');
    }
}

// 3.创建路由(请求方法为get并且请求路径为home，就执行)
app.get('/home',checkCodeMiddleware,(req, res) => {

    res.setHeader('content-type', 'text/html;charset=utf-8');
    res.end('前台首页');
});

app.get('/admin',checkCodeMiddleware, (req, res) => {

    res.end('后台首页');
})
app.get('/setting',checkCodeMiddleware, (req, res) => {
    res.setHeader('content-type', 'text/html;charset=utf-8');
    res.end('设置页面');
})
app.get('*', (req, res) => {

    res.end('404 Not Found');
})

// 4.监听端口，启动服务
app.listen(3000, () => {
    console.log('服务已经启动,端口3000正在监听');
}
)
```

##### 107.静态资源中间件

```

// 1.导入express
const express = require('express');

// 2.创建应用对象
const app = express();

// 静态资源中间件设置
app.use(express.static(__dirname+'/public'));

// 3.创建路由(请求方法为get并且请求路径为home，就执行)
app.get('/home',(req, res) => {
    res.end('hello express');
});

// 4.监听端口，启动服务
app.listen(3000, () => {
    console.log('服务已经启动,端口3000正在监听');
}
)
```

##### 108.静态资源中间件注意点

注意事项:
1. index.html文件为默认打开的资源
2. 如果静态资源与路由规则同时匹配，谁先匹配谁就响应（自上而下执行）
3. 路由响应动态资源，静态资源中间件响应静态资源

##### 109.静态资源中间件练习

局域网内可以访问尚优选的网页

```

// 1.导入express
const express = require('express');

// 2.创建应用对象
const app = express();

// 静态资源中间件设置
app.use(express.static(__dirname+'/项目一：尚优选'));

// 3.创建路由(请求方法为get并且请求路径为home，就执行)


// 4.监听端口，启动服务
app.listen(3000, () => {
    console.log('服务已经启动,端口3000正在监听');
}
)
```

##### 110.获取请求体数据

```
express可以使用body -parser包处理请求体
第一步:安装
npm i body-parser
第二步:导入body-parser包
const bodyParser = require(' body-parser');
第三步:获取中间件函数
//处理querystring 格式的请求体
let urlParser = bodyParser . urlencoded( {extended:false}));
//处理JSON格式的请求体
let jsonParser = bodyParser . json();
第四步:设置路由中间件，然后使用request . body来获取请求体数据
app . post(' /login'，urlParser, (request, response)=>{
//获取请求体数据
//console . log(request . body);
//用户名
console .1og( request . body . username);
//密码
console . log( request . body . userpass);
response . send( '获取请求体数据' );
}）；
```

```
/**
 * 按照要求搭建HTTP服务
 * GET/login 显示表单网页
 * POST/login 获取表单中的用户名和密码
 */
const express=require('express');
const bodyParser = require('body-parser');
const app=express();
//处理querystring 格式的请求体
const urlParser = bodyParser.urlencoded( {extended:false});
//处理JSON格式的请求体
const jsonParser = bodyParser.json();
app.get('/login',(req,res)=>{
    // res.setHeader('content-type','text/html;charset=utf-8');
    // res.end('表单页面');
    res.sendfile(__dirname+'/from.html');
});
app.post('/login',urlParser,(req,res)=>{
    res.setHeader('content-type','text/html;charset=utf-8');
    console.log(req.body);
    res.end('获取用户的数据');

});
app.listen(3000,()=>{
    console.log('server is running......');
})
```

##### 112.防盗链实践

```

// 1.导入express
const express = require('express');

// 2.创建应用对象
const app = express();

app.use((req,res,next)=>{
    let referer=req.get('referer');
    // console.log(referer);
    if(referer){
         let url = new URL(referer);
    let hostname=url.hostname;
    console.log(hostname);
    if(hostname!=='127.0.0.1'){
        res.status(404).send('404 Not Found');
        return;
    }
    }
   
    next();
});
// 静态资源中间件设置
app.use(express.static(__dirname+'/public'));

// 3.创建路由(请求方法为get并且请求路径为home，就执行)


// 4.监听端口，启动服务
app.listen(3000, () => {
    console.log('服务已经启动,端口3000正在监听');
}
)
```

##### 113.路由模块化

```

// 1.导入express
const express = require('express');
const homeRouter=require('./routes/homeRouter');
const adminRouter=require('./routes/adminRouter');
// 2.创建应用对象
const app = express();
app.use(homeRouter);
app.use(adminRouter);
// 声明中间件


// 3.创建路由(请求方法为get并且请求路径为home，就执行)

app.get('*', (req, res) => {

    res.end('404 Not Found');
})

// 4.监听端口，启动服务
app.listen(3000, () => {
    console.log('服务已经启动,端口3000正在监听');
}
)
```

##### 114.EJS模板引擎介绍

-----什么是模板引擎
模板引擎是分离用户界面和业务数据的一种技术
-----什么是EJS
-----是一个高效的Javascript 的模板引擎。
官网: htps://ejs.co/
中文站: https://ejs.boocss.com/
-----EJS 初体验
下载安装EJS
npm i ejs --save

##### 115.模板引擎初体验

```
// 导入ejs
const ejs=require('ejs');
const fs=require('fs');

// 字符串
let china='大鸡腿';
// let str='我爱你';
// 将两个字符串拼起来

// let str2=`${str} ${china}`;
// console.log(str2);


let str =fs.readFileSync('./html.html').toString();
let result=ejs.render(str,{china:china});
console.log(result);
```

