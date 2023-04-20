## 1.SpringBoot学习

### 1.1 [尚硅谷]SpringBoot2零基础入门教程（spring boot2干货满满）

视频地址：https://www.bilibili.com/video/BV19K4y1L7MT/

官方笔记连接：https://www.yuque.com/atguigu/springboot

整理版本：[springBoot2上](./assets/atguigu/SpringBoot2/1.md)               [springBoot2下](./assets/atguigu/SpringBoot2/2.md)

### 1.2 @Autowired与@Resource的区别

@Autowired根据type注入，@Resource根据name注入，本质上均实现了注入效果，只是依据不同，那么为什么在Controller中使用@Autowired就没问题呢，原因在于两个地方注入Bean的类型不一样。

个人思考如下：
一般来说，注入controller的service虽然一般来说都是注入一个接口，但是该接口有实现类，并且使用@Service进行关联，所以注入类型应该也可以视为一个类，但是mybatis仅需提供Dao接口，也就是说，注入service的dao只是一个接口，而没有实现类，虽然mybatis能够通过Dao接口和xml文件实现与数据库的操作，但是@Autowired并没有这个识别功能，可能它就认为你类型不匹配，无法使用通过类型注入的方法。通过一个简单的方法验证通过service的实现类给取消了实现接口的语句‘implements UserService’，然后controller 里面的注入方法也报错

## 2.SpringBoot项目学习

1 [瑞吉外卖系统](../webb-java/reggie.md)

