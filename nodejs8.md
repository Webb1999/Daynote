##### 125.案例练习

npm i lowdb@1.0.0

npm i shortid

##### 130.mongodb介绍

----Mongodb是什么
MongoDB是一个基于分布式文件存储的数据库，官方地址https://www.mongodb.com/

----数据库是什么
数据库（DataBase）是按照数据结构来组织、存储和管理数据的应用程序
----数据库的作用
数据库的主要作用就是管理数据，对数据进行增(c)、删(d)、改(u)、查（r)

----数据库管理数据的特点
相比于纯文件管理数据，数据库管理数据有如下特点:

1.速度更快
2.扩展性更强
3．安全性更强
---为什么选择Mongodb
操作语法与JavaScript类似，容易上手，学习成本低

##### 131.mongdb核心概念

Mongodb中有三个重要概念需要掌握
·数据库(database)数据库是一个数据仓库，数据库服务下可以创建很多数据库，数据库中可以存放很多集合

·集合(collection)集合类似于JS中的数组，在集合中可以存放很多文档
·文档(document)文档是数据库中的最小单位，类似于JS中的对象

![](assets/mongodb.png)

大家可以通过JSON文件来理解 Mongodb 中的概念
----一个JSON文件好比是一个数据库，一个Mongodb服务下可以有N个数据库

----JSON文件中的一级属性的数组值好比是集合
----数组中的对象好比是文档
----对象中的属性有时也称之为字段
一般情况下
。一个项目使用一个数据库
。一个集合会存储同一种类型的数据

##### 132.mongodb下载与安装

下载地址: https://www.mongodb.com/try/download/community建议选择zip类型，通用性更强
配置步骤如下:
1>将压缩包移动到C: \Program Files 下，然后解压
2>创建c: \data\db目录，mongodb 会将数据默认保存在这个文件夹

3>以mongodb 中bin目录作为工作目录，启动命令行
4>运行命令mongod

看到最后的 waiting for connections 则表明服务已经启动成功

##### 133.数据库与集合命令

-----命令行交互
命令行交互一般是学习数据库的第一步，不过这些命令在后续用的比较少，所以大家了解即可

-----数据库命令
1.显示所有的数据库
show dbs
2．切换到指定的数据库，如果数据库不存在会自动创建数据库
use 数据库名
3.显示当前所在的数据库
db
4．删除当前数据库
use 库名
db .dropDatabase()
4.2集合命令

1．创建集合
db .createCollection('集合名称'）

2．显示当前数据库中的所有集合
show collections
3．删除某个集合
db .集合名.drop()
4．重命名集合
db.集合名.renameCollection( 'neme')

##### 134.文档命令

1．插入文档
db.集合名.insert(文档对象);

2．查询文档
db.集合名.find(查询条件)
_id 是 mongodb自动生成的唯一编号，用来唯一标识文档

3．更新文档
db.集合名.update(查询条件，新的文档)
db.集合名.update( iname: '张三'},{$set : {age : 19}})
4．删除文档
db.集合名.remove(查询条件)

##### 135.数据库操作应用场景

----新增
·用户注册

·发布视频

·发布商品

·发朋友圈

·发评论

·发微博

·发弹幕
·...........

-----删除

删除评论

删除商品

删除文章

·删除视频

·删除微博

........

------更新
·更新个人信息

修改商品价格
·修改文章内容
·..........

-----查询
商品列表

视频列表

·朋友圈列表

微博列表

搜索功能
·...........

##### 136.mongoose介绍

-----介绍
IMongoose是一个对象文档模型库，官网http://www.mongoosejs.net/

----作用
方便使用代码操作mongodb 数据库

-----使用流程

##### 137.连接数据库

```
const mongoose=require('mongoose');

// 设置strictQuery为true
mongoose.set('strictQuery',true);
mongoose.connect('mongodb://127.0.0.1:27017/bilibili');
// 时间回调函数只执行一次
mongoose.connection.once('open',()=>{
    console.log('连接成功');
});//设置连接成功的回调
mongoose.connection.on('error',()=>{
    console.log('连接失败');
});//设置连接失败的回调
mongoose.connection.on('close',()=>{
    console.log('连接关闭');
});//设置连接关闭的回调

// 关闭连接
// setTimeout(()=>{
//     mongoose.disconnect();
// },2000)
```

##### 139.插入文档

```
const mongoose = require('mongoose');

//设置 strictQuery 为 true
mongoose.set('strictQuery', true);

mongoose.connect('mongodb://127.0.0.1:27017/bilibili');
// 时间回调函数只执行一次
// mongoose7.0.0版本以上不接受回调函数
mongoose.connection.once('open', () => {
    // console.log('连接成功');
    // 创建文档的结构对象
    let BookSchema = new mongoose.Schema({
        name: String,
        author: String,
        price: Number
      });
    
      //6. 创建模型对象  对文档操作的封装对象
      let BookModel = mongoose.model('books', BookSchema);
    
      //7. 新增
      BookModel.create({
        name: '西游记',
        author: '吴承恩',
        price: 19.9
      }, (err, data) => {
        //判断是否有错误
        if(err) {
          console.log(err);
          return;
        }
        //如果没有出错, 则输出插入后的文档对象
        console.log(data);
        //8. 关闭数据库连接 (项目运行过程中, 不会添加该代码)
        mongoose.disconnect();
      });

});//设置连接成功的回调
mongoose.connection.on('error', () => {
    console.log('连接失败');
});//设置连接失败的回调
mongoose.connection.on('close', () => {
    console.log('连接关闭');
});//设置连接关闭的回调
```

##### 140.字段类型

文档结构可选的常用字段类型列表
类型							描述
string						字符串
Number					数字
Boolean						布尔值
Array					数组，也可以使用[]来标识
Date						日期
Buffer						Buffer对象
Mixed			任意类型，需要使用mongoose .Schema . Types. Mixed指定
Objectld		对象ID，需要使用mongoose. Schema . Types.0bjectId指定
Decimal128		高精度数字，需要使用mongoose.schema. Types.Decimal128指定