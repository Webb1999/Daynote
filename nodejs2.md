##### 13.计算机基本组成

内存：读取快，断电易丢失数据

硬盘：读取慢，断电不易丢失数据

主板：

##### 14.程序运行的基本流程

程序一般保存在硬盘，运行会加载进内存，由CPU读取并执行

##### 15.进程与线程

多个线程组合成进程

##### 16.fs模块

fs：文件系统

```
// writefile异步写入
// 1,导入模块
const fs =require('fs');
// 2.写入文件
fs.writeFile('./座右铭.txt','三人行，则必有我师',err=>{
    if(err){
        console.log('写入失败');
        return;
    }
    console.log('写入成功');
});
console.log（1+1）；
```

##### 17.fs异步和同步

异步效率较高

```
同步写入
const fs =require('fs');
fs.writeFileSync('./data.txt','data');
```

```
// writefile异步写入
// 1,导入模块
const fs =require('fs');
// 2.写入文件
fs.writeFile('./座右铭.txt','三人行，则必有我师',err=>{
    if(err){
        console.log('写入失败');
        return;
    }
    console.log('写入成功');
});
console.log（1+1）；

结果：2
	写入成功
不会等待当前进程执行完毕会先执行后面的进程
```

##### 18.fs文件追加写入

```
const fs = require('fs');
// fs.appendFile('./座右铭.txt', ',择其善者而从之，择其不善者而改之', err => {
//     if (err) {
//         console.log("写入失败");
//         return;
//     }
//     console.log("写入成功");
// });
// fs.appendFileSync('./座右铭.txt','\r\n温故而知新,可以为师矣');
fs.writeFile('./座右铭.txt','love love',{flag:'a'},err=>{
    if (err) {
                console.log("写入失败");
                return;
            }
            console.log("写入成功");
});
// 不写{flag：'a'}是追加不了内容的，只会把原先的内容覆盖
用于程序的日志，记录用户访问信息
```

##### 19.fs文件流式写入

```
// 1.导入fs
const fs=require('fs');
// 2.创建写入流对象
const ws=fs.createWriteStream('./观书有感.txt');
// 3.write
ws.write('半亩方塘一鉴开\r\n');
ws.write('天光云影共徘徊\r\n');
ws.write('问渠那得清如水\r\n');
ws.write('唯有源头活水来\r\n');
// 4.关闭通道（加不加都可以，在写完后会自动将通道关闭）
ws.close();

// writefile是一次性的，流式写入更适合写入频繁的场景
```

##### 20.文件写入场景

①下载文件

②安装软件

③保存程序日志，如git

④编辑器保存文件

⑤视频录制

当数据持久化保存数据时，需要文件写入

##### 21.fs文件读取

异步读取：readfile

同步读取：readfileSync

流式读取：createreadStream

```
// 1.导入fs
const  fs=require('fs');
// 2.异步读取
// fs.readFile('./观书有感.txt',(err,data)=>{
//     if(err){
//         console.log('读取错误');
//         return;
//     }
//     console.log(data.toString());
// });

// 同步读取
 let data=fs.readFileSync('./观书有感.txt');
console.log(data.toString());
```

##### 22.读取文件应用场景

①电脑开机

②程序运行

③编辑器打开文件

④查看图片

⑤播放视频、音乐

⑥git查看日志

⑦上传文件

⑧查看聊天记录