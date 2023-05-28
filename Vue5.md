##### 53.对组件的理解

封装

组件的定义—实现应用中局部功能代码和资源的集合

-----.模块
1.理解:向外提供特定功能的js程序，一般就是一个js文件·

2.为什么: js文件很多很复杂
3.作用:复用js，简化js 的编写，提高js运行效率

------组件(
1．理解:用来实现局部(特定)功能效果的代码集合(html/css/js/image.…..
2为什么：一个界面的功能很复杂

3.作用:I复用编码,简化项目编码，提高运行效率

-----模块化
当应用中的js都以模块来编写的，那这个应用就是一个模块化的应用。

------.组件化
当应用中的功能都是多组件的方式来编写的，那这个应用就是一个组件化的应用。

##### 54.非单文件组件

Vue中使用组件的三大步骤:
一、定义组件(创建组件)

二、注册组件
三、使用组件(写组件标签)
-------如何定义一个组件?
使用Vue.extend(options)创建，其中options和new Vue(options)时传入的那个options几乎一样，但也有点区别;区别如下:
1.el不要写，为什么?—最终所有的组件都要经过一个vm的管理，由vm中的e1决定服务哪个

2.data必须写成函数，为什么?—-避免组件被复用时，数据存在引用关系。
备注:使用template可以配置组件结构。
-------如何注册组件?
1。局部注册:靠new Vue的时候传入components选项

2.全局注册:靠Vue.component('组件名’,组件)

------编写组件标签:
<school></ school>

```
<div id="root">
        <hello></hello>
        <xuexiao></xuexiao>
        <hr>
        <xuesheng></xuesheng>
    </div>
    <div id="root2">
        <hello></hello>
  
    </div>

    <script>

        const school = Vue.extend({
            template: `
            <div>
                <h2>学校名称：{{SchoolName}}</h2>
            <h2>学校地址：{{SchoolAddress}}</h2>
                </div>
            
            `,
            data() {
                return {
                    SchoolName: '尚硅谷',
                    SchoolAddress: '北京'
                }
            }
        })
        const student = Vue.extend({
            template: `
            <div>
                <h2>学生姓名：{{studentName}}</h2>
            <h2>学生年龄：{{studentage}}</h2>
                </div>
             
            `,
            data() {
                return {
                    studentName: '张三',
                    studentage: 17
                }
            }
        })
        const hello=Vue.extend({
            template:`
            <div>
                <span>{{name}}</span>
                </div
            `,
            data(){
                return {
                    name:'hello'
                }
            }
        })
        // 全局定义组件
       Vue.component('hello',hello)
        new Vue({
            el: '#root',
            // 局部定义组件
            components: {
                xuexiao: school,
                xuesheng: student
            }
        })
        new Vue({
            el:'#root2'
        })
    </script>
```

##### 55.组件的几个注意点

------关于组件名:
一个单词组成:
第一种写法(首字母小写):school第二种写法(首字母大写):School

多个单词组成:
第一种写法(kebab-case命名):my-school
第二种写法(CamelCase命名):MySchool（需要Vue脚手架支持)

备注:
(1).组件名尽可能回避HTML中已有的元素名称，例如:h2、H2都不行。
(2).可以使用name配置项指定组件在开发者工具中呈现的名字J
--------.关于组件标签;
第一种写法:<school></school>

第二种写法:<school/>
备注:不用使用脚手架时，<school/>会导致后续组件不能渲染。
3.一个简写方式:
const school = Vue.extend(options）可简写为: const school = options

##### 56.组件的嵌套

```
<div id="root">
        <app></app>
    </div>

    <script>
        const student = Vue.extend({
            template: `
            <div>
                <h2>学生姓名：{{studentName}}</h2>
                <h2>学生年龄：{{studentage}}</h2>
                </div>   
            `,
            data() {
                return {
                    studentName: '张三',
                    studentage: 17
                }
            }
        })
        const school = Vue.extend({
            template: `
            <div>
                <h2>学校名称：{{SchoolName}}</h2>
                <h2>学校地址：{{SchoolAddress}}</h2>        
                </div>       
            `,
            data() {
                return {
                    SchoolName: '尚硅谷',
                    SchoolAddress: '北京'
                }
            },
            // components:{
            //     student
            // }

        })

        const app = {
            template: `
        <div>
            <student></student>
        <school></school>
            </div>     
        `,
            components: {
                student,
                school
            }
        }
        new Vue({
            el: '#root',
            // 局部定义组件
            components: {
                app
            }
        })

    </script>
```

##### 57.VueComponent

构造函数

关于VueComponent:
1.school组件本质是一个名为VueComponen的构造函数，且不是程序员定义的，是Vue.extend生成的.
2.我们只需要写<school/>或<school></school>，Vue解析时会帮我们创建school组件的实例对象，即vue帮我们执行的:new VueComponent(options)。
3.特别注意:每次调用Vue.extend，返回的都是一个全新的VueComponent! !!!

4.关于this指向:
(1).组件配置中:
data函数、methods中的函数、watch中的函数、computed中的函数它们的this均是【VueComponent实例对象】。

(2).new Vue()配置中:
data函数、methods中的函数、watch中的函数、computed中的函数它们的this均是【Vue实例对象】。
5.VueComponent的实例对象，以后简称vc（也可称之为:组件实例对象）。
Vue的实例对象，以后简称vm。

##### 58.Vue实例与组件实例

vm != vc

##### 59.一个重要的内置关系

1.一个重要的内置关系:vueComponent.prototype._proto_ == Vue.prototype

2.为什么要有这个关系:让组件实例对象(vc）可以访问到Vue原型上的属性、方法。

##### 60.单文件组件

School.vue

App.vue

main.js

indxe.html

##### 61.创建Vue脚手架

1.Vue脚手架是Vue官方提供的标准化开发工具(开发平台)。
2.最新的版本是4.x。
3.文档: https://cli.vuejs.org/zh/。



1．如出现下载缓慢请配置npm淘宝镜像:npm config set registry https://registry.npm.taobao.orge

2.第一步（仅第一次执行)∶全局安装@vue/cli。
npm install -g @vue/cli
第二步∶切换到你要创建项目的目录，然后使用命令创建项目
vue create xxxx
第三步:启动项目
npm run servee

Vue脚手架隐藏了所有webpack相关的配置，若想查看具体的 webpakc配置，请执
行:vue inspect > output.js

##### 62.分析脚手架结构



##### 63.render函数

main.js中引入的vue是残缺的vue

完整的vue：vue/dist/vue



关于不同版本的Vue:
1.vue.js 与vue.runtime.xxx.js的区别:
(1).vue.js是完整版的Vue，包含:核心功能+模板解析器。|
(2) .vue ,runtime.xxx.js是运行版的Vue，只包含:核心功能;没有模板解析器。

2.因为vue.runtime.xxx.js没有模板解析器，所以不能使用template配置项，需要使用render函数接收到的createElement函数去指定具体内容。

