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

##### 23.文件流式读取

流式读取：createreadStream（一次读取一块）

```
// 1.引入fs
const fs=require('fs');
// 2.创建读取流对象
const rs=fs.createReadStream('../资料/笑看风云.mp4');
// 3.绑定Data事件 chunk 块
rs.on('data',chunk=>{
    console.log(chunk.length);//65536
    // console.log(chunk.toString());会乱码
})
// 4.end 可选事件
rs.on('end',()=>{
    console.log('读取完成');
})
```

24.fs练习，复制文件

```
方式二更好
方式一整体读，然后写入，站的内存空间较大
方式二读一点写一点，理想状态下，内存空间只占64kb，然而读取的速度要比写入的速快，占的内存空间还是要比方式一的小
```

```
// 1.导入fs
const fs=require('fs');
const process=require('process');
// 方式一：readfile
// 2.读取文件内容
// let data=fs.readFileSync('观书有感.txt');
// 3.写入文件
// fs.writeFileSync('观书有感2.txt',data);
// console.log(process.memoryUsage());//30515200 29.10156MB

// 方式二 流式操作
// 2.创建读取流对象
const rs=fs.createReadStream('观书有感.txt');
// 3.创建写入流对象
const ws =fs.createWriteStream('观书有感3.txt');
// 绑定data事件
// rs.on('data',chunk=>{
//     ws.write(chunk);
// });
// rs.on('end',()=>{
//     console.log(process.memoryUsage());//30674944 29.25MB
// })

rs.pipe(ws);//管道
```

##### 25.fs文件重命名与移动

```
fs.rename(oldePath,newPath,callback)
fs.renameSync(oldPath,newpath)
```

```
// 1.导入fs
const fs = require('fs');
// 2.调用rename方法
// fs.rename('观书有感.txt','论语.txt',err=>{
//     if(err){
//         console.log("操作失败");
//         return;
//     }
//     console.log("操作成功");
// });
// 文件的移动
fs.rename('./data.txt', '../资料', err => {//newPath不用写文件名
    if (err) {
        console.log("操作失败");
        return;
    }
    console.log("操作成功");
})
```

##### 26.文件删除

```
// 1,导入fs
const fs=require('fs');
// 2.调用unlink方法 unlinkSync
// fs.unlink('观书有感2.txt',err=>{
//     if(err){
//         console.log('删除失败');
//         return ;
//     }
//     console.log('删除成功');
// })

// rm方法  rmSync
fs.rm('论语.txt',err=>{
    if(err){
        console.log('删除失败');
        return ;
    }
    console.log('删除成功');
})
```

##### 27.fs文件夹操作

```
mkdir/mkdirSync(path[,options],callback) 创建文件夹
readdir /readdirSync 读取文件夹
rmdir/rmdirSync 删除文件夹 
```

```
// 1.导入fs
const fs=require('fs');
// 2.创建文件夹 make directory
// fs.mkdir('./html',err=>{
//     if(err){
//         console.log('创建失败');
//         return;
//     }
//     console.log('创建成功');
// });

// 2-2递归创建文件夹
// fs.mkdir('./b/c',{recursive:true},err=>{
//     if(err){
//         console.log('创建失败');
//         return;
//     }
//     console.log('创建成功');
// });

// 2-3 文件夹的读取
// fs.readdir('./资料',(err,data)=>{
//     if(err){
//         throw err;
//     }
//     console.log(data);
// })
// 读取当前文件夹
// fs.readdir('./',(err,data)=>{
//     if(err){
//         throw err;
//     }
//     console.log(data);
// })
// 2-4 删除文件夹
// fs.rmdir('./html',err=>{
//     if(err){
//         throw err;
//     }
//     console.log('删除成功');
// })
// 递归删除 不推荐使用
// fs.rmdir('./a',{recursive:true},err=>{
//     if(err){
//         throw err;
//     }
//     console.log('删除成功');
// })
// 建议使用
fs.rm('./a',{recursive:true},err=>{
    if(err){
        throw err;
    }
    console.log('删除成功');
})
```

##### 28.查看资源状态

```
fs.stat(path[,options],callback)
fs.statSync(path[,options])
```

```
// 1.导入fs
const fs=require('fs');
// 2.stat方法
fs.stat('./资料/data2.txt',(err,data)=>{
    if(err){
        console.log('失败');
        return;
    }
    // console.log(data);
    // console.log(data.isFile());
    console.log(data.isDirectory());
})

```

##### 29.fs路径

```
// 1.导入fs模块
const fs=require('fs');
// 相对路径
fs.writeFileSync('./index.html','love');
fs.writeFileSync('index.html','love');
fs.writeFileSync('../index.html','love');

// 绝对路径
fs.writeFileSync('D:/index.html','love');
// c盘权限不够，写不了
```

##### 30.fs相对路径的bug

相对路径参照物：命令行的工作目录

```
console.log(__dirname);
全局变量保存的是所在文件的所在目录的绝对路径
fs.writeFileSync(__dirname+'./index.html','love');
```

