## 1.SpringBoot学习

1.尚硅谷】SpringBoot2零基础入门教程（spring boot2干货满满）

视频地址：https://www.bilibili.com/video/BV19K4y1L7MT/

官方笔记连接：https://www.yuque.com/atguigu/springboot

整理版本：[springBoot2上](./assets/atguigu/SpringBoot2/1.md)               [springBoot2下](./assets/atguigu/SpringBoot2/2.md)

2.手工部署项目

![1679933246353](assets/picture/springBoot/1679933246353.png)

linux 后台 ps -ef |grep ‘java -jar’   然后kill -9 +进程号

3spring Cache

![1681127041424](assets/picture/springBoot/1681127041424.png)

4.mysql主从复制

![1681630065375](assets/picture/springBoot/1681630065375.png)

配置主从复制

```shell
#第一步修改mysql数据库的配置文件/ect/my.cnf

[mysqld]
log-bin=mysql-bin #[必须]启用二进制日志
server-id=100 #[必须]服务器唯一ID 100 随便改


#第二步 重启mysql服务
systemctl restart mysqld

#第三步 执行以下语句
# 查看mysql 密码规则
SHOW VARIABLES LIKE 'validate_password%';
#设置密码最低长度
set global validate_password.length=4;
#设置密码等级
set global validate_password.policy=0;
#创建用户
create user 'webb'@'%' identified with mysql_native_password by 'webb19990525';

#查看密码加密 方式
select host,user,plugin,authentication_string from user;

# 授权replication slave 给用户
grant replication slave on *.* to 'webb'@'%';

# 回收授权
REVOKE replication slave on *.* from 'webb'@'%';

# 删除用户
DROP user 'webb'@'%';

# 查询用户repl授权情况
show GRANTS FOR 'webb'@'%';

# 刷新权限
FLUSH PRIVILEGES;

#注:上面SOL的作用是创建一个用户webb,密码为webb1990525，并且给webb用户授予REPLICATION SLAVE权限。常用于建立复制时所需要用到的用户权限，也就是slave必须被master授权具有该权限的用户，才能通过该用户复制。

#第四步 登录mysql数据库 执行以下sql 记录结果中的file和position值
show master status;

#修改从库slave 
server-id=101 #[必须]服务器唯一ID 101 随便改

#重启mysql服务
systemctl restart mysqld

#从库执行 以下操作
change master to master_host='1.12.224.96',master_user='webb',master_password='webb19990525',master_log_file='mysql-bin.000002',master_log_pos=8400;

start slave; #若之前启动过 需关闭后重新启动\

#查看是否配置成功 登录从库
show slave status;
#查看slave—io-running 和slave-sql-running 是否为yes
```

![1681630655732](assets/picture/springBoot/1681630655732.png)

![1681630681092](assets/picture/springBoot/1681630681092.png)

```xml
 <!--  读写分离-->
        <dependency>
            <groupId>org.apache.shardingsphere</groupId>
            <artifactId>sharding-jdbc-spring-boot-starter</artifactId>
            <version>4.0.0-RC1</version>
        </dependency>
 
spring:
  shardingsphere:
    datasource:
      names:
        master,slave
      # 主数据源
      master:
        type: com.alibaba.druid.pool.DruidDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://192.168.138.100:3306/rw?characterEncoding=utf-8
        username: root
        password: root
      # 从数据源
      slave:
        type: com.alibaba.druid.pool.DruidDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://192.168.138.101:3306/rw?characterEncoding=utf-8
        username: root
        password: root
    masterslave:
      # 读写分离配置
      load-balance-algorithm-type: round_robin #轮询
      # 最终的数据源名称
      name: dataSource
      # 主库数据源名称
      master-data-source-name: master
      # 从库数据源名称列表，多个逗号分隔
      slave-data-source-names: slave
    props:
      sql:
        show: true #开启SQL显示，默认false
  main:
    allow-bean-definition-overriding: true
```

