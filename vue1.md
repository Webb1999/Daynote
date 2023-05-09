##### 2.Vue简介（尤雨溪）

一套用于构建用户界面的渐进式JavaScript框架

Vue可以自底向上逐层的应用

简单应用:只需一个轻量小巧的核心库

复杂应用:可以引入各式各样的Vue插件

---Vue的特点

1.采用组件化模式，提高代码复用率、且让代码更好维护。

2声明式编码，让编码人员无需直接操作DOM，提高开发效率。

3.使用虚拟DOM+优秀的Diff算法（比较），尽量复用DOM节点。

4.学习Vue之前要掌握的JavaScript基础知识?
--ES6语法规范
--ES6模块化

--包管理器

--原型、原型链

--数组常用方法

--axios
--promise

##### 3.vue官方网站使用指南

vue组件库

https://awesomejs.dev/for/vue/

##### 4.搭建vue开发环境

---开发版本

##### 5.hello小案例

---初识Vue: 
1.想让Vue工作，就必须创建一个Vue实例,且要传入一个配置对象;
2.root容器里的代码依然符合html规范，只不过混入了一些特殊的Vue语法;

3.root容器里的代码被称为【Vue模板】;

```
<!-- 准备容器 -->
    <div id="root">
        <h1>Hello,{{name}}</h1>
    </div>
    <script>
        // 阻止vue在启动时生成生产提示
        Vue.config.productionTip=false;
        // 创建Vue实例
        new Vue({
            el:'#root',//元素：el用于指定当前Vue实例为哪个容器服务，值通常为css选择器字符串。
            data:{//存储数据，数据供el指定的容器使用
                name:'大西瓜',
                // age:18
            }

        })
    </script>
```

##### 6.分析Hello案例

4.Vue实例和容器是一一对应的;
5.真实开发中只有一个Vue实例，并且会配合着组件一起使用;
6.{{xxx}}中的xxx要写js表达式,且xxx可以自动读取到data中的所有属性;

7.一旦data中的数据发生改变，那么页面中用到该数据的地方也会自动更新;
---注意区分:js表达式和js代码(语句)
		1.表达式:一个表达式会产生一个值，可以放在任何一个需要值的地方:
(1). a
(2). a+b

(3). demo(1)
(4). x === y ? 'a' : "b'
		2.js代码(语句)
(1). if(){0}

(2). for(){}

##### 7.模板语法

Vue模板语法有2大类:

1.插值语法:
---功能:用于解析标签体内容。
---写法:{{xxx}}，xxx是js表达式，且可以直接读取到data中的所有属性。

2.指令语法:
---功能:用于解析标签(包括:标签属性、标签体内容、绑定事件.....）.
---举例: v-bind:href="xxx”或简写为:href="xxx"，xxx同样要写js表达式，
且可以直接读取到data中的所有属性。
---备注: Vue中有很多的指令，且形式都是: v-????，此处我们只是拿v-bind举个例子。

```
<div id="root">
        <h1>模板语法</h1>
        <h2>你好，{{name}}</h2>
        <a v-bind:href="school.url" v-bind:x="hello">点我去{{school.name}}</a>
        <a v-bind:href="school.url" :x="hello">点我去{{school.name}}2</a>
    </div>
    <script>
        // 阻止vue在启动时生成生产提示
        Vue.config.productionTip = false;
        // 创建Vue实例
        new Vue({
            el: '#root',//元素：el用于指定当前Vue实例为哪个容器服务，值通常为css选择器字符串。
            data: {//存储数据，数据供el指定的容器使用
                name: 'Jack',
                // age:18
                school: {
                    name: '尚硅谷',
                    url: 'http://www.atguigu.com',
                    hello: '你好'
                }

            }

        })
    </script>
```



##### 8.数据绑定

Vue中有2种数据绑定的方式:
1.单向绑定(v-bind):数据只能从data流向页面。
2.双向绑定(v-model):数据不仅能从data流向页面，还可以从页面流向date.备注:
1.双向绑定一般都应用在表单类元素上(如:input、select等)
2.v-model:value可以简写为v-model，因为v-model默认收集的就是value值。

```
<div id="root">
        单项数据绑定：<input type="text" v-bind:value="name"><br>
        双项数据绑定：<input type="text" v-model:value="name"><br>
        <!-- 简写 -->
        单项数据绑定：<input type="text" v-bind:value="name"><br>
        双项数据绑定：<input type="text" v-model="name">
        <!-- 这种写法是错误的，v-model只支持表单类元素（输入类元素） -->
        <!-- <h2 v-model:x="name">你好啊</h2> -->
    </div>
    <script>
        // 阻止vue在启动时生成生产提示
        Vue.config.productionTip = false;
        // 创建Vue实例
        new Vue({
            el:'#root',
            data:{
                name:'尚硅谷'
            }
        })
    </script>
```

9.el与data的两种写法
1.el有2种写法
					(1).new Vue时候配置el属性。
					(2).先创建Vue实例，随后再通过vm.$mount( ' #root')指定el的值。
2.data有2种写法
					(1).对象式

​					(2).函数式
如何选择:目前哪种写法都可以，以后学习到组件时，data必须使用函数式，否则会报错。
3.一个重要的原则:
由Vue管理的函数，一定不要写箭头函数，一旦写了箭头函数，this就不再是Vue实例了。

```
 <div id="root">
        <h2>你好，{{name}}</h2>

    </div>
    <script>
        // 阻止vue在启动时生成生产提示
        Vue.config.productionTip = false;
        const v=new Vue({
            // 对象式
            // data:{
            //     name:'大鸡腿'
            // }
            // 函数式---使用箭头函数返回的this是window
            data:function (){
                console.log('@@@',this);
                return {
                    name:'大鸡腿'
                }
            }
            
        })
        // console.log(v);
        v.$mount('#root');
    </script>
```

10.MVVM模型
1.M:模型(Model) : data中的数据

2.V:视图(View）:模板代码

3.VM:视图模型(ViewModel):Vue实例

-----观察发现:
1.data中所有的属性,最后都出现在了vm身上。
2.vm身上所有的属性及 Vue原型上所有属性，在Vue模板中都可以直接使用。