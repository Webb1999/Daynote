## 1.redis学习

1.尚硅谷Redis零基础到进阶，最强redis7教程，阳哥亲自带练（附redis面试题）

视频地址：https://www.bilibili.com/video/BV13R4y1v7sP/

脑图笔记：[redis 7脑图](./assets/atguigu/尚硅谷Redis7教程/脑图/Redis脑图（基础篇+高级篇）.html )

Redis面试常考经典题30道 [Redis面试常考经典题30道.html](./assets/atguigu/尚硅谷Redis7教程/Redis面试常考经典题30道.html)



## 2.记录笔记

### 1.redis启动与关闭

```shell
默认端口6379 密码设置为111111
#启动服务器 后跟启动加载配置文件
redis-server /myredis/redis.conf
# 查看redis 进程
ps -ef|grep redis |grep -v grep
#客户端连接服务器 -a 后跟密码 -p 跟端口
redis-cli -a 111111 -p 6379
#退出 不关闭服务端 quit
#退出 关闭服务端 shutdown
#退出 单实例redis-cli -a 111111 shutdown
#退出 多实例 指定端口 redis-cli -a 111111 -p 6379 shutdown
```
