##### 116.ejs列表渲染

```
const xiyou = ['唐僧', '孙悟空', '猪八戒', '沙僧'];
const ejs = require('ejs');
const fs =require('fs');
// let str='<ul>';
// xiyou.forEach(item=>{
//     str+=`<li>${item}</li>`
// })
// str+='</ul>';
// console.log(str);

// let result = ejs.render(`<ul>
// <% xiyou.forEach(item=>{%>
//     <li><%= item %></li>
//     <% }) %>
// </ul>`,{xiyou})
let html=fs.readFileSync('./02.xiyou.html').toString();
    let result=ejs.render(html,{xiyou});
console.log(result);
```

##### 117.ejs条件渲染

```
/**
 * 通过islogin来决定输出的内容
 * true 输出【<span>欢迎回来</span>】
 * false 输出【<button>登录</button> <nutton>注册</button>】
 */

const ejs=require('ejs');
let islogin=false;
// if(islogin){
//     console.log('<span>欢迎回来</span>');
// }else{
//     console.log('<button>登录</button> <button>注册</button>');
// }
const fs=require('fs');
let html=fs.readFileSync('./03.home.html').toString();
let result=ejs.render(html,{islogin});
console.log(result);
```

##### 118.express中使用ejs

```
const express=require('express');
const app=express();
const path =require('path');

// 设置模板引擎
app.set('view engine','ejs');
// 设置模板文件的存放位置
app.set('views',path.resolve(__dirname,'./views'));
app.get('/home',(req,res)=>{
    let title='hahah';
    res.render('home',{title});
    // res.end('hello express');
});
----home是ejs文件
app.listen(3000,()=>{
    console.log('端口3000正在监听中,服务已启动.....');
});
```

##### 119.express- generator

Express应用程序生成器
通过应用生成器工具express-generator可以快速创建一个应用的骨架。
你可以通过npx (包含在 Node.js 8.2.0及更高版本中)命令来运行Express应用程序生成器。
$ npx express- generator
对于较老的Node版本，请通过npm将Express应用程序生成器安装到全局环境中并使用:
$ npm install -g express-generator
$ express
-h参数可以列出所有可用的命令行参数:
$ express -h

##### 120.查看文件上传报文

##### 121.处理文件上传

npm i formidable

```
var express = require('express');
var router = express.Router();
const formidable=require('formidable');

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});

// 显示网页的表单
router.get('/portarit',(req,res)=>{
  res.render('portarit');
});
// 处理文件上传
router.post('/portarit',(req,res)=>{
const form=formidable({
  multiples:true,
  // 设置上传文件的保存目录
  uploadDir:__dirname+'/../public/images',

  // 保存文件后缀
  keepExtensions:true,
});
  form.parse(req,(err,fields,files)=>{
    if(err){
      next(err);
      return;
    }

  //  服务器保存该图片的访问URL
  let url='/images/'+files.portrait.newFilename;

   res.send(url);
  });
});
module.exports = router;

```

##### 122.案例实践：记账本

##### 125.lowdb介绍

```
const low=require('lowdb');
const FileSync=require('lowdb/adapters/FileSync');

const adapter=new FileSync('db.json');
const db=low(adapter);

db.defaults({ posts:[],user:{} }).write()
// 写入数据
// db.get('posts').push({id: 2,title: '今天天气不错'}).write();
// 在数组前面插入数据
// db.get('posts').unshift({id: 2,title: '今天天气不错'}).write();

// 获取数据
// console.log(db.get('posts').value());

// 删除数据
// let res =db.get('posts').remove({id: 2,}).write();
// console.log(res);

// 获取单条数据
// let res=db.get('posts').find({id :1}).value();
// console.log(res);

// 更新数据
// db.get('posts').find({id :1}).assign({title: '今天下雨啦'}).write();
```

