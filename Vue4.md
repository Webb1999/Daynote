##### 31.列表的过滤

```
<div id="root">
        <!-- 遍历数组 -->
        <input type="text" placeholder="请输入名字" v-model="keyword">
        <ul>
            
            <li v-for="(p,index) in filpersons" :key="p.id">
                {{p.name}}-{{p.age}}-{{p.sex}}
                
            </li>
        </ul>
       
        
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                keyword:'',
                persons:[
                    {id:'001',name:'周冬雨',age:23,sex:'女'},
                    {id:'002',name:'马冬梅',age:19,sex:'女'},
                    {id:'003',name:'周杰伦',age:28,sex:'男'},
                    {id:'004',name:'温兆伦',age:30,sex:'男'},
                ],
                // filpersons:[]
                
            },
            // 用watch实现
            // watch:{
            //     keyword:{
            //         immediate:true,
            //         handler(val){
            //             this.filpersons=this.persons.filter((p)=>{
            //                 return p.name.indexOf(val)!==-1;
            //             });
                       
            //         }
            //     }
            // },
            // 用computed实现
            computed:{
                filpersons(){
                  return  this.persons.filter((p)=>{
                        return p.name.indexOf(this.keyword)!==-1
                    })
                }
            }
        })
    </script>
```

##### 32.列表排序

```
<div id="root">
        <!-- 遍历数组 -->
        <input type="text" placeholder="请输入名字" v-model="keyword">
        <button @click="sortType=2">年龄升序</button>
        <button @click="sortType=1">年龄降序</button>
        <button @click="sortType=0">原顺序</button>
        <ul>
            
            <li v-for="(p,index) in filpersons" :key="p.id">
                {{p.name}}-{{p.age}}-{{p.sex}}
                
            </li>
        </ul>
       
        
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                keyword:'',
                //0原顺序，1降序，2升序
               sortType:0,
                persons:[
                    {id:'001',name:'周冬雨',age:23,sex:'女'},
                    {id:'002',name:'马冬梅',age:19,sex:'女'},
                    {id:'003',name:'周杰伦',age:28,sex:'男'},
                    {id:'004',name:'温兆伦',age:25,sex:'男'},
                ],
                // filpersons:[]
                
            },
         computed:{
                filpersons(){
                    const arr=this.persons.filter((p)=>{
                        return p.name.indexOf(this.keyword)!==-1
                    })
                    if(this.sortType){
                        arr.sort((p1,p2)=>{
                            return this.sortType===1? p2.age-p1.age:p1.age-p2.age;
                        })
                    }
                    return arr;
                }
         }
            
        })
    </script>
```

##### 33.更新时的一个问题

Vue监测数据改变的原理

```
 <div id="root">
       <button @click="upDataMei">更改马冬梅的数据信息</button>
        <ul>
            
            <li v-for="(p,index) in persons" :key="p.id">
                {{p.name}}-{{p.age}}-{{p.sex}}
                
            </li>
        </ul>
       
        
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                keyword:'',
                //0原顺序，1降序，2升序
               sortType:0,
                persons:[
                    {id:'001',name:'周冬雨',age:23,sex:'女'},
                    {id:'002',name:'马冬梅',age:19,sex:'女'},
                    {id:'003',name:'周杰伦',age:28,sex:'男'},
                    {id:'004',name:'温兆伦',age:25,sex:'男'},
                ],
                // filpersons:[]
                
            },
            methods:{
                upDataMei(){
                    this.persons[1]= {id:'002',name:'马老师',age:50,sex:'男'};
                }
            }
            
        })
    </script>
```

##### 34.Vue监测数据改变的原理

```
 <script>
        let data={
            name:'尚硅谷',
            address:'北京'
        }
        // 创建一个监视的实例对象，用于监视data中属性的变化
        const obs=new Observer(data);
        console.log(obs);
        let vm={};
        vm._data=data=obs;
        function Observer(obj){
            // 汇总对象中所有的属性形成一个数组
            const keys=Object.keys(obj);
            // 遍历
            keys.forEach((k)=>{
                Object.defineProperty(this,k,{
                    get(){
                        return obj[k];
                    },
                    set(val){
                        console.log('name被改了，我要去忙了');
                        obj[k]=val;
                    }
                })
            })
        }
    </script>
```

##### 35.Vue.set()方法

Vue.set(target,key,val)

vm.$ser(target,key,val)

```
<div id="root">
        <button @click="addSex">点击添加一个性别属性</button>
        <h2>姓名：{{students.name}}</h2>
        <h2 v-if="students.sex">性别：{{students.sex}}</h2>
        <h2>年龄：{{students.age}}</h2>
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                students:{
                    name:'华华',
                    age:17,
                   
                }
            },
            methods:{
                addSex(){
                    Vue.set(this.students,'sex','男')
                }
            }
        })
    </script>
```

##### 37.总结Vue监测数据

Vue监视数据的原理:
1. vue会监视data中所有层次的数据。

2. 如何监测对象中的数据?
    通过setter实现监视,且要在new Vue时就传入要监测的数据。
    (1).对象中后追加的属性,Vue默认不做响应式处理
    (2).如需给后添加的属性做响应式,请使用如下API:
    Vue.set(target.propertyName /index,value）或

  vm.$set(target，propertyName/index,value)

3. 如何监测数组中的数据?
    通过包裹数组更新元素的方法实现,本质就是做了两件事:

  (1)．调用原生对应的方法对数组进行更新。
  (2).重新解析模板，进而更新页面。

4. 在Vue修改数组中的某个元素一定要用如下方法:
    1.使用这些API:push()、pop()、shift()、unshift()、splice()、sort()、reverse()2.Vue.set()或vm.$set()
    特别注意:Vue.set()和 vm.$set(）不能给vm或 vm的根数据对象添加属性!!中,

```
<div id="root">
        <button @click="student.age++">年龄+1</button><br>
        <button @click="addSex">添加性别属性</button><br>
        <button @click="student.sex='未知'">修改性别属性</button><br>
        <button @click="addFriend">在列表首位添加一个朋友</button><br>
        <button @click="updataFirstFriendName">修改第一个朋友的名字为：张三</button><br>
        <button @click="addhobby">添加一个爱好 </button><br>
        <button @click="updataFirstHobby">修改第一个爱好为：开车</button><br>
        <h1>学生信息</h1>
        <h3>姓名:{{student.name}}</h3>
        <h3>年龄:{{student.age}}</h3>
        <h3 v-if="student.sex">性别：{{student.sex}}</h3>
        <h3>爱好:</h3>
        <ul>
            <li v-for="(h,index) in student.hobby" :key="index">
                {{h}}
            </li>
        </ul>
        <h3>朋友们:</h3>
        <ul>
            <li v-for="(f,index) in student.friends" :key="index">
                {{f.name}}-{{f.age}}
            </li>
        </ul>
    </div>
    <script>
        new Vue({
            el: '#root',
            data: {
                student: {
                    name: 'tom',
                    age: 19,
                    hobby: ['抽烟', '喝酒', '烫头'],
                    friends: [
                        { name: 'jerry', age: 35 },
                        { name: 'tony', age: 45 }
                    ]
                }
            },
            methods: {
                addSex(){
                    Vue.set(this.student,'sex','男')
                },
                addFriend(){
                    this.student.friends.unshift({name:'jack',age:90})
                },
                updataFirstFriendName(){
                    this.student.friends[0].name='张三'
                },
                addhobby(){
                    this.student.hobby.push('打游戏')
                },
                updataFirstHobby(){
                    // this.student.hobby.splice(0,1,'开车')
                    // this.$set(this.student.hobby,0,'开车')
                    Vue.set(this.student.hobby,0,'开车')
                }
            },
        })
    </script>
```

##### 38.收集表单数据

收集表单数据:
若:<input type="text"/>，则v-model收集的是value值，用户输入的就是value值。

若:<input type="radio" />，则v-model收集的是value值，且要给标签配置value值。

若:<input type="checkbox" />
1.没有配置input的value属性，那么收集的就是checked(勾选or未勾选，是布尔值)

2.配置input的value属性:
(1)v-model的初始值是非数组，那么收集的就是checked（勾选or未勾选，是布尔值)

(2)v-model的初始值是数组，那么收集的的就是value组成的数组
备注:v-model的三个修饰符;
lazy:失去焦点再收集数据
number:输入字符串转为有效的数字

trim:输入首尾空格过滤

```
<div id="root">
        <form @submit.prevent="demo">
            姓名：<input type="name" v-model.trim="userInfo.account"><br><br>
            密码：<input type="password" v-model="userInfo.password"><br><br>
            年龄：<input type="number" v-model.number="userInfo.age"><br><br>
            性别：
            男<input type="radio" name="sex" v-model="userInfo.sex" value="male">
            女<input type="radio" name="sex" v-model="userInfo.sex" value="famale"><br><br>
            爱好：
            学习<input type="checkbox" v-model="userInfo.hobby" value="study">
            打游戏<input type="checkbox" v-model="userInfo.hobby" value="game">
            吃饭<input type="checkbox" v-model="userInfo.hobby" value="eat">
            <br><br>
            所属校区
            <select v-model="userInfo.city">
                <option value="">请选择校区</option>
                <option value="beijing">北京</option>
                <option value="shenzhen">深圳</option>
                <option value="shanghai">上海</option>
                <option value="wuhan">武汉</option>
            </select>
            <br><br>
            其他信息：
            <textarea v-model.lazy="userInfo.other"></textarea><br><br>
            <input type="checkbox" v-model="userInfo.agree">阅读并接受<a href="#">《用户协议》</a>
            <button>提交</button>
        </form>
    </div>
    <script>
        new Vue({
            el: '#root',
            data: {
                userInfo: {
                    account: '',
                    password: '',
                    age:'',
                    hobby: [],
                    sex: 'famale',
                    city: '',
                    other: '',
                    agree: ''
                }


            },
            methods: {
                demo() {
                    
                    console.log(JSON.stringify(this.userInfo));
                }
            }
        })
    </script>
```

##### 39.过滤器

过滤器:
定义:对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）。

语法:
1.注册过滤器:Vue.filter(name,callback)或new Vue{filters:{3}
2.使用过滤器:{{ xxx│过滤器名}}或v-bind:属性= “xxx│过滤器名"

备注:
1.过滤器也可以接收额外参数、多个过滤器也可以串联

2.并没有改变原本的数据,是产生新的对应的数据

```
<div id="root">
       
        <h2>显示格式化后的时间为</h2>
        <!-- 计算属性实现 -->
        <h2>现在是：{{fmTime}}</h2>
        <!-- methods实现 -->
        <h2>现在是：{{gettime()}}</h2>
        <!-- 过滤器实现 -->
        <h2>现在是：{{time | timefornow}}</h2>
        <h2>现在是：{{time | timefornow('YYYY-MM-DD') | mySlice}}</h2>

    </div>
    <div id="root2">
        <h2>{{msg | mySlice}}</h2>
    </div>
    <script>
        // 全局的过滤器
        Vue.filter('mySlice',function(value){
            return value.slice(0,4)
        })

        new Vue({
            el:'#root',
            data:{
                time:1621561377603
            },
            computed:{
                fmTime(){
                    return dayjs(this.time).format('YYYY-MM-DD HH:mm:ss')
                }
            },
            methods:{
                gettime(){
                    return dayjs(this.time).format('YYYY-MM-DD HH:mm:ss')
                }
            },
            filters:{
                timefornow(value,str){
                    return dayjs(value).format(str)

                },
                mySlice(value){
                    return value.slice(0,4)
                }
            }
        })
        new Vue({
            el:'#root2',
            data:{
                msg:'hello guigu'
            }
        })
    </script>
```

##### 40.v-text指令

我们学过的指令:
v-bind : 单向绑定解析表达式，可简写为:xXx
v-model;双向数据绑定
V- for;遍历数组/对象/字符串
V- on:绑定事件监听，可简写为@
v-if:条件渲染(动态控制节点是否存存在)
v-else;条件渲染(动态控制节点是否存存在)
V- show :条件渲染(动态控制节点是否展示)

v-text指令:
1.作用:向其所在的节点中渲染文本内容。
2.与插值语法的区别: v-text会替换掉节点中的内容，{{xx}}则不会。

```
<div id="root">
        <div v-text="name"></div>
        <div v-text="str"></div>
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                name:'尚硅谷',
                str:'<h3> 你好</h3>'
            }
        })
    </script>
```

##### 41.v-html指令

v-html指令:
1.作用:向指定对点中渲染包含htm1结构的内容。
2.与插值语法的区别:
(1).v-html会替换掉节点中所有的内容，{{xx}}则不会。
(2) .v- html可以识别html结构。
3.严重注意: v-html有安全性问题! ! ! !
(1).在网站上动态渲染任意HTML是非常危险的，容易导致XSS攻击。
(2).定要在可信的内容上使用v-html,永不要用在用户提交的内容上!

##### 42.v-cloak指令

v-cloak指令（没有值):
1.本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性。

2.使用css配合v-cloak可以解决网速慢时页面展示出{{xxx}}的问题。

```
<style>
[v-cloak]{
display:none;//当存在v-cloak属性时，所有内容被隐藏，没有后再显示出来
}
```

##### 43.v-once指令

v-once指令:
1.v-once所在节点在初次动态渲染后,就视为静态内容了。（内容只使用一次，之后不会被改变）
2.以后数据的改变不会引起v-once所在结构的更新，可以用于优化性能。

##### 44.v-pre指令

v-pre指令:
1.跳过其所在节点的编译过程。
2.可利用它跳过:没有使用指令语法、没有使用插值语法的节点，会加快编译。

##### 45.自定义指令-函数式

需求1:定义一个v-big指全，和v-text功能类似。但会把绑定的数值放大10倍。
需求2:定义一个v-fbind指令，和v-bind功能类似，但可以让其所绑定的input元素默认获取焦点。

##### 47.自定义指令-总结

自定义指令总结:
一、定义语法:
(1).局部指令:
new Vue({                                              new Vue({
directives:{指令名:配置对象}或                  directives{指令名:卫调函数}
})                                                               })
(2).全局指令:
Vue.directive(指令名,配置对象）或Vue.directive(指令名,回调函数)
二、配置对象中常用的3个回调:
(1).bind:指令与元素成功绑定时调用。
(2).inserted:指令所在元素被插入页面时调用。

(3).update:指令所在模板结构被重新解析时调用。
三、备注:
1.指令定义时不加v-，但使用时要加v-;
2.指令名如果是多个单词，要使用kebab-case命名方式，不要用camelCase（驼峰命名法）命名。

```
 <div id="root">
        <h2>当前的n值是：<span v-text="n"></span></h2>
        <h2>放大十倍后的值：<span v-big="n"></span></h2>
        <button @click="n++">点击n+1</button>
        <input type="text" v-fbind:value="n">
    </div>
    <div id="root2">
        <h2>放大十倍后的值：<span v-big="n"></span></h2>
    </div>
    <script>
        // 定义全局指令
        Vue.directive('big',function(element,binding){
            element.innerText=binding.value * 10
        })
        new Vue({
            el:'#root',
            data:{
                n:1
            },
            directives:{
                // big函数何时调用
                // 1.指令与元素成功绑定时（一上来），2.指令所在的模板重新被解析时
                // big(element,binding){
                //     element.innerText=binding.value * 10
                // },
                'fbind':{
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
                }
            }

        })
        new Vue({
            el:'#root2',
            data:{
                n:10
            }
        })
    </script>
```

##### 48.引出生命周期

生命周期:
1.又名:生命周期回调函数、生命周期函数、生命周期钩子。

2.是什么:Vue在关键时刻帮我们调用的一些特殊名称的函数。
3.生命周期函数的名字不可更改，但函数的具体内容是程序员根据需求编写的。

4.生命周期函数中的this指向是m或组件实例对象

```
 <div id="root">
        <h2 :style="{opacity}">欢迎来到尚硅谷</h2>
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                opacity:1
            },
            mounted(){
                setInterval(()=>{
                    this.opacity -=0.01
                    if(this.opacity<=0){
                        this.opacity=1
                    }
                },16)
            }
        })
    </script>
```

##### 49.生命周期-挂载流程

![](assets/生命周期-挂载流程.png)

##### 50.生命周期-更新流程

![](assets/生命周期-更新流程.png)

##### 51.生命周期-销毁流程

![](assets/生命周期-销毁流程.png)

##### 52.生命周期-总结

常用的生命周期钩子:
1.mounted:发送ajax请求、启动定时器、绑定自定义事件、订阅消息等【初始化操作】。

2.beforeDestroy:清除定时器、解绑自定义事件、取消订阅消息等【收尾工作】。
关于销毁Vue实例
1.销毁后借助Vue开发者工具看不到任何信息。
2.销毁后自定义事件会失效,但原生DOM事件依然有效。
3.一般不会在beforeDestroy操作数据，因为即便操作数据，也不会再触发更新流程了。

```
<div id="root">
        <h2 :style="{opacity}">欢迎来到尚硅谷</h2>
        <button @click="stop">点我停止变换</button>
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                opacity:1
            },
            methods: {
                stop(){
                   this.$destroy()
                }
            },
            mounted(){
                this.timer=setInterval(()=>{
                    this.opacity -=0.01
                    if(this.opacity<=0){
                        this.opacity=1
                    }
                },16)
                
            },
            beforeDestroy() {
                     clearInterval(this.timer)
                },
        })
    </script>
```

##### 

