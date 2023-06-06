##### 64.修改默认配置

##脚手架文件结构:
node_modules

public
--favicon.ico:页签图标
--index.html:主页面

src

--assets:·存放静态资源

-------.logo.png
--component:存放组件

-------He1loworld.vue

--App.vue:·汇总所有组件

--main.js:·入口文件
gitignore: git版本管制忽略的配置

babel.config-js: babel的配置文件

package.json:应用包配置文件
README.md:应用描述文件
package-lock.json:包版本控制文件

##关于不同版本的Vue:
- vue.jsvue.runtime.xxx.js的区别:
(1).vue.js是完整版的Vue,包含:核心功能+模板解析器。
(2).vue.runtime.xxx.js是运行版的Vue，只包含:核心功能;没有模板解析器。
- 因为vue.runtime.xxx.js没有模板解析器，所以不能使用template配置项，需要使用
render函数接收到的createElement函数去指定具体内容。

##vue.config-js配置文件
>使用vue inspect > output.js可以查看到Vue脚手架的默认配置。
>使用vue.config.js可以对脚手架进行个性化定制，详情见: https://cli.vuejs.org/zh



##### 65,ref标签属性

##ref属性
1.被用来给元素或子组件注册引用信息（id的替代者)
2.应用在html标签上(title,btn)获取的是真实DOM元素，应用在组件标签上（sch）是组件实例对象(vc)

3.使用方式:
打标识:<h1 ref="xxx">.....</h1>或<School ref="xxx"></School>获取: this.$refs.xxx

```
<template>
  <div>
    <h1 v-text="msg" ref="title"></h1>
    <button ref="btn" @click="showDOM">点击显示上方DOM元素</button>
    <School ref="sch" />
    <School />
    <School />
  </div>
</template>

<script>
// 引入School组件
import School from "./components/School.vue";
export default {
  name: "App",
  components: { School },
  data() {
    return {
        msg:'欢迎学习Vue!'
    }
  },
  methods: {
        showDOM(){
            console.log(this.$refs)
           
        }
    },
};
</script>

<style>
</style>
```

##### 66.props配置

配置项props
功能:让组件接收外部传过来的数据
(1).传递数据:
<Demo name="xxx"/>
(2).接收数据:
第一种方式（只接收）∶props: [ "name ']
第二种方式（限制类型):props:{
name:String}
第三种方式（限制类型、限制必要性、指定默认值）:

props:{
name:{
type:String，1/类型

required:true,l/必要性

default:'老王‘//默认值}
}
备注: props是只读的,Vue底层会监测你对props的修改，如果进行了修改,就会发出警告，
若业务需求确实需要修改，那么请复制props的内容到data中一份，然后去修改data中的数据。

```
<!-- 组件的结构 
快捷生成代码结构
<v
-->
<template>
  <div class="demo">
    <h1>{{ msg }}</h1>
    <h2>学生姓名：{{ name }}</h2>
    <h2>学生性别：{{ gender }}</h2>
    <h2>学生年龄：{{ myAge + 1 }}</h2>
    <button @click="updateAge">点击更改学生年龄</button>
  </div>
</template>
<!-- 组件交互相关的代码 -->（数据、方法等）
<script>
export default {
  name: "StudentName",

  data() {
    return {
      msg: "我是一个学生",
      myAge:this.age
    }
  },
  methods:{
    updateAge(){
       console.log(this.myAge)
       this.myAge++
    }

  },
  props: ["name", "gender", "age"],//简单接收
// 接受的同时对数据进行类型限制
// props:{
//     name:String ,
//     gender:String,
//     age:Number
// }
// 接受的同时对数据进行类型限制+默认值的限制，必要性的限制
// props:{
//     name:{
//          type:String,//name的类型是字符串
//     required:true, //name是必要的
    
//     },
//     gender:{
//         type:String,
//         required:true
//     },
//     age:{
//         type:Number,
//         default:99,//默认值
//     }
   
// }
};
</script>
<!-- 组件的样式 -->
<style>
.demo {
  background-color: orange;
}
</style>
```

##### 67.mixin属性（混入）

mixin(混入)
功能:可以把说个组件共用的配置提取成一个混入对象

使用方式:
---第一步定义混合,例如:
{
data(){...},

methods:{....}
}
---第二步使用混入，例如:
(1).全局混入:Vue.mixin(xxx)

(2).局部混入:mixins: [ ’xxx ']

##### 68.插件

插件
功能:用于增强Vue
本质:包含install方法的一个对象，install的第一个参数是Vue，第二个以后的参数是插件使用者传递的数据。
定义插件:
对象.install = functioh (Vue，options){
---- 1．添加全局过滤器
Vue.filter(....)
----2。添加全局指令Vue.directive( ....)

---- 3。配置全局混入(合)Vue.mixin(... .)
-----4。添加实例方法
Vue.prototype.$myMethod = function () {...}

Vue.prototype. $myProperty = xxxx
}
使用插件:Vue.use()

```
---------pulgins.js
export default{
    install(Vue){
        // 全局的过滤器
        Vue.filter('mySlice',function(value){
            return value.slice(0,4)
        })
        // 定义全局指令
        Vue.directive('fbind',{
            // 指令与元素成功绑定时调用
            bind(element,binding){
                element.value=binding.value
            },
            // 指令所在元素被插入页面时调用
            inserted(element,binding){
                element.focus()
            },
            // 指令所在的模板被重新解析时调用
            update(element,binding){
                element.value=binding.value
            }
        
        })
        // 定义全局混入
        Vue.mixin({
            data() {
                return {
                    x:200,
                    y:300
                }
            },
        })
        // 给原型添加一个方法
        Vue.prototype.hello=()=>{alert('你好啊')}

}
}
```

##### 69.scoped样式

scoped样式
作用:让样式在局部生效，防止冲突。写法:<style scoped>

```
<style lang="less">
.demo {
  background-color: orange;
}
</style>

使用less语法编译，如果不写lang，默认使用css语法
```

