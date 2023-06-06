##### 70-76.ToDoList案例_静态

组件化编码流程（通用)
---1.实现静态组件:抽取组件，使用组件实现静态页面效果

---⒉展示动态数据:
2.1.数据的类型、名称是什么?

2.2.数据保存在哪个组件?-
---3.交互——从绑定事件监听开始



##### 77.总结TodoList案例

1.组件化编码流程:
(1).拆分静态组件:组件要按照功能点拆分，命名不要与html元素冲突。

(2).实现动态组件:考虑好数据的存放位置，数据是一个组件在用，还是一些组件在用:
             1).一个组件在用:放在组件自身即可。
             2).一些组件在用:放在他们共同的父组件上(状态提升)。

(3).实现交互:从绑定事件开始。

2.props适用于:
(1).父组件==>子组件通信
(2).子组件==>父组件通信（要求父先给子一个函数)
3.使用v-model时要切记: v-model绑定的值不能是props传过来的值，因为props是不可以修改的!

4.props传过来的若是对象类型的值，修改对象中的属性时Vue不会报错，但不推荐这样做。

##### 78.浏览本地存储

webStorage
1.存储内容大小一般支持5MB左右〈不同浏览器可能还不一样)
⒉.浏览器端通过Window.sessionStorage和 Window.localStorage属性来实现本地存储机制。

3.相关API:
1.xxxxxStorage. setItem( ' key ' , 'value ' ) ;
该方法接受一个键和值作为参数，会把键值对添加到存储中，如果键名存在，则更新其对应的值。
2.xxxxStorage-getItem( ' person ' );
该方法接受一个键名作为参数，返回键名对应的值。

3.xxxxxStorage.removeItem( ' key ' );
该方法接受一个键名作为参数，并把该键名从存储中删除。
4.xxxxxStorage.clear()
该方法会清空存储中的所有数据。
4.备注:
1.SessionStorage存储的内容会随着浏览器窗口关闭而消失。
2.LocalStorage存储的内容，需要手动清除才会消失。
3.xxxxxStorage.getItem(xxx)如果xxx对应的value获取不到，那么getltem的返回值是null。

4.JSON.parse(null)的结果依然是null。

##### 80.组件自定义事件-绑定

```
<template>
  <div class="school">
    <h2>你好啊</h2>
    <!-- 通过父组件给子组件绑定一个自定义事件实现：子给父传递数据 -->
    <!-- <student  v-on:atguigu="getStudent"/> -->
 <!-- 通过父组件给子组件绑定一个自定义事件实现：子给父传递数据 (通过绑定ref)-->
    <student  ref="student"/>
    <!-- 通过父组件给子组件传递函数类型的props实现：子给父传递数据 -->
    <school :getSchoolName="getSchoolName"/>
    
  </div>
</template>

<script>
// 引入School组件
import Student from "./components/Student.vue";
import School from './components/School.vue'
export default {
  name: "App",
  components: { Student,School },
 methods:{
  getSchoolName(name){
    console.log(name);

  },
  getStudent(name,...params){
    console.log("被调用了！！！！" ,name,params)
  }
 },
 mounted(){
  setTimeout(() => {
    // this.$refs.student.$on('atguigu',this.getStudent)
    // 事件只触发一次
     this.$refs.student.$on('atguigu',this.getStudent)
  }, 3000);
 }
  };
  

</script>


Student.vue
 methods: {
    sendStudentName(){
      this.$emit('atguigu',this.name,666,888,999)
    }

  },
```

##### 81.组件自定义事件-解绑

```
<!-- 组件的结构 
快捷生成代码结构
<v
-->
<template>
  <div class="demo">
    
    <h2 >学生姓名：{{ name }}</h2>
    <h2>学生性别：{{ gender }}</h2>
    <h2>当前数字为：{{number}}</h2>
    <button @click="add">点我数字++</button>
   <button @click="sendStudentName">点击获取学生名</button>
    <button @click="unbind">解绑atguigu事件</button>
     <button @click="death">销毁当前Student组件实例</button>
    
  </div>
</template>
<!-- 组件交互相关的代码 -->（数据、方法等）
<script>


export default {
  name: "StudentName",

  data() {
    return {
      name:'张三',
      gender:'男',
      number:0
    }
  },
  methods: {
    add(){
      this.number++
    },
    sendStudentName(){
      this.$emit('atguigu',this.name,666,888,999);
      this.$emit('demo')
    },
    unbind(){
      // this.$off('atguigu');//解绑一个自定义事件
      this.$off(['atguigu','demo']);
      console.log('atguigu事件已被解绑');
    },
    death(){
      this.$destroy()//销毁了当前Student组件的实例，销毁后所有Student实例的自定义事件全都不奏效。

    }

  },
};
</script>
<!-- 组件的样式 -->
```

##### 82.组件自定义事件-总结

计算属性依赖于已经存在的属性

组件的自定义事件
1.一种组件间通信的方式，适用于   子组件===>父组件
⒉.使用场景:A是父组件，B是子组件，B想给A传数据，那么就要在A中给B绑定自定义事件（事件的回调在A中)。

3.绑定自定义事件:
---1.第一种方式，在父组件中: <Demo @atguigu="test"/>或<Demo v-on:atguigu="test" />

---2.第二种方式，在父组件中:
<Demo ref="demo" />
......
mounted(){
this.$refs.xxx.$on( 'atguigu ' ,this.test)
}
---3.若想让自定义事件只能触发一次，可以使用once修饰符，或 $once方法。
4.触发自定义事件:this.$emit( 'atguigu ',数据)
5.解绑自定义事件this.$off( ' atguigu ')
6.组件上也可以绑定原生DOM事件，需要使用native修饰符。
7.注意:通过 this.$refs.xx. son('atguigu',回调)绑定自定义事件时，回调要么配置在methods中，要么用箭头函数，否则this指向会出问题!

##### 84.全局事件总线1

![](assets/Vue与Vuecomponent的关系.png)

##### 85.全局事件总线2

1.一种组件间通信的方式，适用于任意组件间通信。

2.安装全局事件总线:

```
new vue({
beforeCreate() {
Vue .prototype.$bus = this //安装全局事件总线，$bus就是当前应用的vm},
。。。。。。
})
```

3.使用事件总线:
---1.接收数据:A组件想接收数据，则在A组件中给$bus绑定自定义事件，事件的回调留在A组件自身。

```
methods(){
demo(data){......}
}
.....
mounted() {
this.$bus.$on( 'xxxx',this.demo)
```

---⒉.提供数据:this.$bus.$emit( ' xxxx',数据)
4.最好在beforeDestroy钩子中，用$off去解绑当前组件所用到的事件。