##### 2.node.js的用处

前端开发三大框架：Vue，React，Angular

使其他人可以访问我们写的网页

##### 3.node.js是什么

开源，跨平台的JavaScript运行环境

##### 4.node.js的作用

①开发服务器应用

②开发工具类应用，

​	Webpack，Vite，Babel

③开发桌面端应用

​	VSCode，Figma，Postman

​	借助electron框架，而electron借助node.js

##### 7.命令行

命令 参数1 参数2

##### 8.cmd常用命令

d: 	切换到D盘

dir 查看当前路径的所有内容

cd 文件夹名 切换工作目录

cd ..切换上一级路径

dir /s  查看当前路径的所有文件

ctrl c停止查看

##### 9.node.js初体验

在集成终端中打开

代码运行:node 文件名.js

##### 10.node.js编写注意事项

①不可以使用DOM，BOM语法

②和js相同的，console和定时器

--顶级对象 global，也可以使用globalThis访问顶级对象

##### 11.Buffer（缓冲区）

一个类似于Array的对象

```
let buf=Buffer.alloc(10);
// console.log(buf);
// 会对旧数据进行清除

let buf_2=Buffer.allocUnsafe(10000);
// console.log(buf_2);
// 会包含旧的内存数据，创建速度比alloc快

let buf_3=Buffer.from('hello');
let buf_4=Buffer.from([105,108,118,101,121]);
console.log(buf_4);
// 会以十六进制存入Buffer
```

12.Buffer的操作和注意事项

```
let buf_4=Buffer.from([105,108,118,101,121,111,117]);
// console.log(buf_4.toString());

let buf_3=Buffer.from('hello');
// console.log(buf_3[0].toString(2));
// 将第一位转换为二进制，01101000
console.log(buf_3);
// <Buffer 68 65 6c 6c 6f>
buf_3[0]=95;
console.log(buf_3);
// <Buffer 5f 65 6c 6c 6f>
buf_3[0]=361;
console.log(buf_3);//舍弃高位的数字

let buf=Buffer.from('你好');//一个中文字符占三个字节
console.log(buf);
```

13.计算机基本组成

