##### 11. Object.defineProperty

```
<script>
        let number=18;
        let person = {
            name: '张三',
            sex: '男'
        }
        Object.defineProperty(person, 'age', {
            // value:18,
            // enumerable:true,//控制属性是否可以被遍历（枚举），默认值是false
            // writable:true,//控制属性是否可以修改，默认值是false
            // configurable:true,//控制属性是否可以被删除，默认值是flase

            // 当有人读取person的age属性时，get函数（getter）就会被调用，返回的就是age的值
            get(){
            // get: function () {//可以简写
                return number;
            },
            // 当有人修改person的age属性时，set函数（setter）就会被调用，且会收到具体的值
            set(value){
                console.log('有人修改了age属性，值为',value);
                number=value;
            }
        })
        console.log(person);
    </script>
```

##### 12.理解数据代理

```
<!-- 数据代理:通过一个对象代理对另一个对象中属性的操作(读/写)-->
 <script>
    let obj1={x:200};
    let obj2={y:300};
    Object.defineProperty(obj2,'x',{
        get(){
            return obj1.x;
        },
        set(value){
            obj.x=value;
        }
    })
 </script>
```

##### 13.Vue中的数据代理

![](assets/数据代理.png)

1.Vue中的数据代理:
通过vm对象来代理data对象中属性的操作（读/写)

2.Vue中数据代理的好处:
更加方便的操作data中的数据
3.基本原理:
通过object.defineProperty()把data对象中所有属性添加到vm上.

为每一个添加到vm上的属性，都指定一个getter/setter。
在getter/setter内部去操作（读/写)data中对应的属性。

##### 14.事件处理

事件的基本使用:
1.使用v-on:xxx或@xxx绑定事件,其中xxx是事件名;

2.事件的回调需要配置在methods对象中,最终会在vm上;
3.methods中配置的函数,不要用箭头函数!否则this就不是vm了;
4.methods中配置的函数，都是被Vue所管理的函数，this的指向是vm或组件实例对象;

5.@click="demo”和@click="demo($event)”效果一致，但后者可以传参;

```
<div id="root">
        <h1>欢迎来到{{name}}学习</h1>
        <button v-on:click="showInfo(66,$event)">点我一下</button>
        <!-- 简写 -->
        <button @click="showInfo(66,$event)">点我一下</button>
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                name:'尚硅谷'
            },
            methods:{
                showInfo(number,a){
                    alert('你好同学！'+number);
                }
            }
            
        })
    </script>
```

##### 15.事件修饰符

Vue中的事件修饰符:
1.prevent:阻止默认事件（常用）;

2.stop:阻止事件冒泡（常用）;

3.once:事件只触发一次（常用）;

4.capture:使用事件的捕获模式;
5.self:只有event.target是当前操作的元素是才触发事件;
6.passive:事件的默认行为立即执行，无需等待事件回调执行完毕;

```
<div id="root">
        <h1>欢迎来到{{name}}学习</h1>
        <!-- 最值默认事件 -->
        <a href="http://www.atguigu.com" @click.prevent="showInfo">点我提示信息</a>
        <!-- 阻止事件冒泡 -->
        <div class="demo1" @click="showInfo">
            <button v-on:click.stop="showInfo(66,$event)">点我一下</button>
        </div>
        <!-- 事件只触发一次 -->
        <button v-on:click.once="showInfo(66,$event)">点我一下</button>
        <!-- 使用事件的捕获模式 -->
        <div class="box1" @click.capture="showMsg(1)">
            box1
            <div class="box2" @click="showMsg(2)">
                box2
            </div>
        </div>
        <!-- 只有event.target是当前操作的元素是才触发事件; -->
        <div class="demo1" @click.self="showInfo">
            <button v-on:click="showInfo">点我一下</button>
        </div>
        <!-- 事件的默认行为立即执行，无需等待事件回调执行完毕; -->
        <ul class="list" @wheel.passive="demo">
            <li>1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>
        </ul>
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                name:'尚硅谷'
            },
            methods:{
                showInfo(e){
                    alert('你好同学！');
                    // console.log(e.target);
                },
                showMsg(number){
                    console.log(number);
                },
                demo(){
                    // for(let i=0;i<100;i++){
                    //     console.log('#');
                    // }
                    console.log('累坏了');
                }
            }
            
        })
    </script>
```

##### 16.键盘事件

1.Vue中常用的按键别名;
回车=>enter
删除=> dele(捕获“删除”和“退格”键)

退出=>esc
空格=>space

换行=>tab

上=>up

下 =>down

左=>left

右=> right
2.Vue未提供别名的按键，可以使用按健原始的key值去绑定，但注意要转为kebab-case（短横线命名)
3.系统修饰键(用法特殊):ctr1、alt、 shift、meta
(1).配合keyup使用:按下修饰键的同时，再按下其他键，随后释放其他键，事件才被触发。

(2).配合keydown使用:正常触发事件。
4.也可以使用keycode去指定具体的按键(不推荐)
5.Vue.config.keyCodes.自定义键名=键码,可以去定制按键别名

```
 <div id="root">
        <h1>欢迎来到{{name}}学习</h1>
        <input type="text" placeholder="按下回车提示输入" @keyup.ctrl="showInfo">

    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                name:'尚硅谷'
            },
            methods:{
                showInfo(e){
                    console.log(e.target.value);
                    console.log(e.key,e.keyCode);
                }
            }
            
        })
    </script>
```

##### 17.事件总结

修饰符可以连续写

@keyup.ctrl.y

##### 18.姓名案例-差值语法

```
<div id="root">
        姓：<input type="text" v-model="firstname"><br><br>
        名：<input type="text" v-model="lastname"><br><br>
        姓名：<span>{{firstname.slice(0,3)}}-{{lastname}}</span>    
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                firstname:'张',
                lastname:'三'
            }     
        })
    </script>
```

##### 18.姓名案例-methods实现

```
<div id="root">
        姓：<input type="text" v-model="firstname"><br><br>
        名：<input type="text" v-model="lastname"><br><br>
        姓名：<span>{{fullName()}}</span>
    </div>
    <script>
        new Vue({
            el: '#root',
            data: {
                firstname: '张',
                lastname: '三'
            },
           methods:{
            fullName(){
                return this.firstname+'-'+this.lastname;
            }
           }
        })
    </script>
```

##### 19.姓名案例-计算属性

计算属性:
1.定义:要用的属性不存在,要通过己有属性计算得来。
2.原理:底层借助了objcet.defineproperty方法提供的getter和setter.

3.get函数什么时候执行?
		(1).初次读取时会执行一次。
		(2).当依赖的数据发生改变时会被再次调用。
4.优势:与methods实现相比，内部有缓存机制（复用），效率更高，调试方便。

 5.备注:
1.计算属性最终会出现在vm上，直接读取使用即可。
2.如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时依赖的数据发生改变。

```
 <div id="root">
        姓：<input type="text" v-model="firstname"><br><br>
        名：<input type="text" v-model="lastname"><br><br>
        姓名：<span>{{fullName}}</span>
    </div>
    <script>
       const vm= new Vue({
            el: '#root',
            data: {
                firstname: '张',
                lastname: '三'
            },
           computed:{
            fullName:{
                get(){
                    console.log('get被调用');
                    return this.firstname+'-'+this.lastname;
                },
                set(value){
                    const arr=value.split('-');
                    this.firstname=arr[0];
                    this.lastname=arr[1];
                }
            }
           }
        })
    </script>
```

##### 20.计算属性-简写

```
// 简写----只考虑读取不考虑修改时才可以简写
            fullName(){
                return this.firstname+'-'+this.lastname;
            }
```

##### 21.天气案例

绑定事件的时候: @xx="yyy" yyy可以写一些简单的语句

```
<div id="root">
        <h1>天气很{{info}}</h1>
        <button @click="isHot=!isHot">切换天气</button>
        <button @click="click">切换天气</button>
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                isHot:true
            },
            computed:{
                info(){
                    return this.isHot ? '炎热' :'凉爽';
                }
            },
            methods: {
                click(){
                    this.isHot=!this.isHot;
                }
            },

        })
    </script>
```

