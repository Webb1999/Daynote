##### 86.ToDoList案例-事件总线

12_src_ToDoList_全局事件总线

##### 87.消息订阅与发布-pubsub

1.一种组件间通信的方式，适用于任意组件间通信。

2.使用步骤:
---1.安装pubsub: npm i pubsub-js
---2.引入: import pubsub from 'pubsub-js '
---3.接收数据:A组件想接收数据，则在A组件中订阅消息，订阅的回调留在A组件自身。

```
methods(){
demo(data){......}}
... . ..
mounted() {
this.pid = pubsub.subscribe( 'xxx ' ,this.demo) //订阅消息.}
```

4.提供数据:pubsub.publish( ' xxx',数据)
5.最好在beforeDestroy钩子中，用PubSub .unsubscribe(pid)去<span style="co1or : red">取消订阅。</span>/

##### 88.ToDoList案例-pubsub

##### 89.ToDoList案例-编辑

##### 90.$nextTick

1.语法:this.$nextTick(回调函数)
2.作用:在下一次DOM更新结束后执行其指定的回调。
3.什么时候用:当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行。

##### 91.动画效果

```
<template>
  <div>
    <button @click="isShow=!isShow">显示/隐藏</button>
    <transition name="hello" appear>
     <h2 v-show="isShow" >你好啊！</h2>
    </transition>
   
  </div>
</template>

<script>
export default {
name:'Test',
data() {
    return {
        isShow:true
    }
},
}
</script>

<style>
h2{
    background-color: orange;
}
.hello-enter-active{
    animation: atguigu 1s;

}
.hello-leave-active{
    animation: atguigu 1s reverse;
}
@keyframes atguigu{
    from{
        transform: translateX(-100%);
    }
    to{
        transform: translateX(0px);
    }
}
</style>
```



##### 92.过渡效果

```
<template>
  <div>
    <button @click="isShow=!isShow">显示/隐藏</button>
    <transition name="hello" appear>
     <h2 v-show="isShow" >你好啊！</h2>
    </transition>
   
  </div>
</template>

<script>
export default {
name:'Test',
data() {
    return {
        isShow:true
    }
},
}
</script>

<style>
h2{
    background-color: orange;
    /* transition: 1s linear; */
}
.hello-enter,.hello.leave-to{
    transform: translateX(-100%);
}
.hello-enter-active,.hello-leave-active{
    transition: 1s linear;
}
.hello-leave,.hello-enter-to{
    transform: translateX(0px);
}
</style>
```

##### 93.多个过渡效果

```
 <transition-group name="hello" appear>
     <h2 v-show="isShow"  key="1">你好啊！</h2>
     <h2 v-show="!isShow" key="2">尚硅谷！</h2>
    </transition-group>
```

##### 94.集成第三方动画

```
<template>
  <div>
    <button @click="isShow=!isShow">显示/隐藏</button>
    <transition-group 
    name="animate__animated animate__bounce" 
    enter-active-class="animate__swing"
    leave-active-class="animate__backOutUp"
    appear
    >
     <h2 v-show="isShow"  key="1">你好啊！</h2>
     <h2 v-show="!isShow" key="2">尚硅谷！</h2>
    </transition-group>
   
  </div>
</template>

<script>
import 'animate.css';
export default {
name:'Test',
data() {
    return {
        isShow:true
    }
},
}
</script>

<style>
h2{
    background-color: orange;
    /* transition: 1s linear; */
}
</style>
```

##### 95.总结过渡与动画效果

1. 作用：在插入、更新或移除 DOM元素时，在合适的时候给元素添加样式类名。

2. 图示：

   ![](assets/过渡与动画.png)

3. 写法：

   1. 准备好样式：

      - 元素进入的样式：
        1. v-enter：进入的起点
        2. v-enter-active：进入过程中
        3. v-enter-to：进入的终点
      - 元素离开的样式：
        1. v-leave：离开的起点
        2. v-leave-active：离开过程中
        3. v-leave-to：离开的终点

   2. 使用```<transition>```包裹要过度的元素，并配置name属性：

      ```vue
      <transition name="hello">
      	<h1 v-show="isShow">你好啊！</h1>
      </transition>
      ```

   3. 备注：若有多个元素需要过度，则需要使用：```<transition-group>```，且每个元素都要指定```key```值。

