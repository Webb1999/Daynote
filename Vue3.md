##### 22.监视属性

监视属性watch:
1.当被监视的属性变化时，回调函数自动调用，进行相关操作
2.监视的属性必须存在，才 能进行监视! !
3.监视的两种写法:
(1). new Vue时传入watch配置
(2)。通过vm. $watch监视

```
 <div id="root">
        <h1>天气很{{info}}</h1>
        <button @click="isHot=!isHot">切换天气</button>
        <button @click="click">切换天气</button>
    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            data: {
                isHot: true,
                // window
            },
            computed: {
                info() {
                    return this.isHot ? '炎热' : '凉爽';
                }
            },
            methods: {
                click() {
                    this.isHot = !this.isHot;
                }
            },
            // watch:{ 
            //     isHot:{
            //         immediate:true,
            //         handler(newValue,oldValue){
            //             console.log('is_Hot被修改了',newValue,oldValue);
            //         }
            //     }
            // }
        })
        vm.$watch('isHot', {
            immediate: true,
            handler(newValue, oldValue) {
                console.log('is_Hot被修改了', newValue, oldValue);
            }
        })
    </script>
```

##### 23.深度监视

深度监视:
(1).Vue中的watch默认不监测对象内部值的改变(一层)
(2).配置deep:true可以监测对象内部值改变(多层)。
备注:
(1).Vue自身可以监测对象内部值的改变，但Vue提 供的watch默认不可以!
(2).使用watch时根据数据的具体结构，决定是香采用深度监视。

```
 <div id="root">
        <h1>天气很{{info}}</h1>
        <button @click="isHot=!isHot">切换天气</button>
        <button @click="click">切换天气</button>
        <hr>
        <h2>a的值为{{number.a}}</h2>
        <button @click="number.a++">点击改变a值</button>
        <h2>b的值为{{number.b}}</h2>
        <button @click="number.b++">点击改变b值</button>
    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            data: {
                isHot: true,
                number:{
                    a:1,
                    b:1
                }
                // window
            },
            computed: {
                info() {
                    return this.isHot ? '炎热' : '凉爽';
                }
            },
            methods: {
                click() {
                    this.isHot = !this.isHot;
                }
            },
            watch:{ 
                isHot:{
                    // immediate:true,
                    handler(newValue,oldValue){
                        console.log('is_Hot被修改了',newValue,oldValue);
                    }
                },
                 // 监视多级结构中某个属性的变化
                // 'number.a':{
                //     handler(){
                //         console.log('a被改变了');
                //     }
                // },
                // 监视多级结构中所有属性的变化
                number:{
                    deep:true,
                     handler(){
                        console.log('number被改变了');
                     }
                }
            }
        })
       
    </script>
```

##### 24.监视属性-简写

```
// 正常写法
                // isHot:{
                //     // immediate:true,
                //     handler(newValue,oldValue){
                //         console.log('is_Hot被修改了',newValue,oldValue);
                //     }

                // },
                // 简写
                // isHot(newValue, oldValue) {
                //     console.log('is_Hot被修改了', newValue, oldValue);
                // }
                
                
                
                // 正常写法
        // vm.$watch('isHot', {
        //     immediate: true,
        //     handler(newValue, oldValue) {
        //         console.log('is_Hot被修改了', newValue, oldValue);
        //     }
        // })
        // 简写
        vm.$watch('isHot',function(newValue,oldValue){
            console.log('is_Hot被修改了', newValue, oldValue);
            
        })
```

##### 25.watch对比computed

computed和watch之间的区别:
1. computed能完成的功能，watch都可以完成。

2. watch能完成的功能，computed不一 定能完成，例如: watch可以进行异步操作。
  -------两个重要的小原则:
  1.所有被Vue管理的函数，最好写成普通函数，这样this的指向才是vm或组件实例对象。
  2.所有不被Vue所管理的函数(定时器的回调函数、ajax的回调函数等，promise的回调函数)，最好写成箭头函数，
  这样this的指向才是vm或组件实例对象。

  ```
  watch: {
                  firstname(val) {
                      setTimeout(() => {
                          // 如果这里不写成箭头函数，this输出的是window，因为setTimeout由js
                          // 调用，使用箭头函数后他没有自己的this就会向外找，firtName的this是
                          // vue，就输出的是vue
                          console.log(this);
                          this.fullName = val + '-' + this.lastname
                      }, 1000)
  
                  },
                  lastname(val) {
                      this.fullName = this.firstname + '-' + val
                  }
  
              }
  ```

##### 26.绑定css样式

```
 <style>
        .normal {
            border: 1px solid black;
            width: 500px;
            height: 200px;
        }

        .base {
            background-color: skyblue;
        }

        .happy {
            background-color: orange;
        }

        .sad {
            background-color: green;
        }

        .guigu1 {
            border: 2px solid skyblue;
            background-color: blueviolet;
        }

        .guigu2 {
            font-size: 50px;
        }

        .guigu3 {
            border-radius: 20px;
        }
    </style>
</head>

<body>
    <div id="root">
        <!-- 绑定class样式--字符串写法，适用于:样式的类名不确定，需要动态指定
 -->
        <div class="normal" :class="mood" @click="changemood">{{name}}</div>
        <!-- 绑定class样式--数组写法，适用于:要绑定的样式个数不确定、名字也不确定
 -->
        <div class="normal" :class="classArr">{{name}}</div>
        <!-- 绑定class样式--对象写法，适用于:要绑定的样式个数确定、名字也确定，但要动态决定用不用
 -->
        <div class="normal" :class="classObj">{{name}}</div>
    </div>

    <script>
        new Vue({
            el: '#root',
            data: {
                name: '大鸡腿',
                mood: 'sad',
                classArr: ['guigu1', 'guigu2', 'guigu3'],
                classObj:{
                    guigu1:false,
                    guigu2:true
                }
            },
            methods: {
                changemood() {
                    const arr = ['happy', 'sad', 'base'];
                    // 随机生成一个0-2的数
                    const index = Math.floor(Math.random() * 3);
                    this.mood = arr[index];

                }
            }

        })
    </script>
</body>
```

##### 27.綁定style样式

绑定样式:
1. class样式
写法:class="xxx."
xxx可以是字符串、对象、数组。
字符串写法适用于:类名不确定，要动态获取。
对象写法适用于:要绑定多个样式，个数不确定，名字也不确定。
数组写法适用于:要绑定多个样式，个数确定，名字也确定，但不确定用不用。
2. sty1e样式
:style=" {fontSize: xxx}" 其中xxx是动态值。
:style="[a,b]"其中a、b是样式对象。

```
<div id="root">
        <!-- 绑定class样式--字符串写法，适用于:样式的类名不确定，需要动态指定
 -->
        <div class="normal" :class="mood" @click="changemood">{{name}}</div>
        <!-- 绑定class样式--数组写法，适用于:要绑定的样式个数不确定、名字也不确定
 -->
        <div class="normal" :class="classArr">{{name}}</div>
        <!-- 绑定class样式--对象写法，适用于:要绑定的样式个数确定、名字也确定，但要动态决定用不用
 -->
        <div class="normal" :class="classObj">{{name}}</div>
        <!-- 绑定style样式--对象写法
 -->
        <div class="normal" :style="[styleObj2,styleObj1]">{{name}}</div>
    </div>

    <script>
        new Vue({
            el: '#root',
            data: {
                name: '大鸡腿',
                mood: 'sad',
                classArr: ['guigu1', 'guigu2', 'guigu3'],
                classObj: {
                    guigu1: false,
                    guigu2: true
                },
                styleObj1:{
                    fontSize:'60px',
                    color:'red'
                },
                styleObj2:{
                    backgroundColor:'skyblue'
                }

            },
            methods: {
                changemood() {
                    const arr = ['happy', 'sad', 'base'];
                    // 随机生成一个0-2的数
                    const index = Math.floor(Math.random() * 3);
                    this.mood = arr[index];

                }
            }

        })
    </script>
```

##### 28.条件渲染

 条件渲染:
1.v-if
写法:
{1).v-if=" 表达式”
(2).v-else-if="表达式”
(3).v-else="表达式”
适用于:切换频率较低的场景。
特点:不展示的DOM元素直接被移除。
注意: v-if可以和:v-else-if、 v-else起使用， 但要求结构不能被“打断”。

2.v-show
写法: v-show="表达式”
适用于:切换频率较高的场景。
特点:不展示的DOM元素未被移除，仅仅是使用样式隐藏掉
3.备注:使用v-if的时候，元素可能无法获取到，而使用v-show 定可以获取到。

```
 <div id="root">
        <!-- <h2 v-show="false">我爱吃{{name}}</h2>
        <h2 v-show="1===1">我爱吃{{name}}</h2> -->

        <!-- <h2 v-if="false">我爱吃{{name}}</h2> -->

        <h2>当前n的值为：{{n}}</h2>
        <button @click="n++">点击n的值+1</button>
             <!-- <div v-if="n===1">Angular</div>
            <div v-if="n===2">React</div>
            <div v-if="n===3">Vue</div> -->
             
        <!-- <div v-if="n===1">Angular</div>
        <div v-else-if="n===2">React</div>
        <div v-else-if="n===3">Vue</div> -->
        
            <!-- v-if和template的配合使用 -->
            <template v-if="n===2">
               <h2>nihao</h2>
               <h2>shangguigu
                <h2>beijing</h2>
               </h2>
            </template>
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                name:'大鸡腿',
                n:0
            }
        })
    </script>
```

##### 29.列表渲染

v-for指令
1.用于展示列表数据
2.语法: v-for=" (item, index) in xxx” :key="yyy"
3.可遍历:数组、对象、字符串(用的很少)、指定次数(用的很少)

```
 <div id="root">
        <!-- 遍历数组 -->
        <ul>
            <li v-for="(p,index) in persons" :key="index">
                {{p.name}}-{{p.age}}
            </li>
        </ul>
        <!-- 遍历对象 -->
        <h2>汽车信息</h2>
        <ul>
            <li v-for="(value,k) of cars" :key="k">
                {{k}}-{{value}}
            </li>
        </ul>
        <!-- 遍历字符串 -->
        <h2>遍历字符串</h2>
        <ul>
            <li v-for="(char,index) of str" :key="index">
                {{char}}-{{index}}
            </li>
        </ul>
        <!--遍历指定次数  -->
        <h2>遍历指定次数</h2>
        <ul>
            <li v-for="(number,index) of 6" >
                {{number}}-{{index}}
            </li>
        </ul>
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                persons:[
                    {id:'001',name:'张三',age:23},
                    {id:'002',name:'李四',age:19},
                    {id:'003',name:'王五',age:8}
                ],
                cars:{
                    name:'奥迪',
                    price:'70万',
                    color:'黑色'
                },
                str:'Hello'
            }
        })
    </script>
```

##### 30.key作用与原理

面试题: react、vue中的key有什么作用? (key的内部原理)
----1.虚拟DOM中key的作用:
key是虚拟DOM对象的标识，当状态中的数据发生变化时，Vue会根据[新数据]生成[新的虚拟DOM]，
随后Vue进行[新虑拟DOM]与[旧虛拟DOM]的差异比较，比较规则如下:
----2.对比规则:
(1).旧虚拟DOM中找到了与新虚拟DOM相同的key:
①.若虛拟DOM中内容没变，直接使用之前的真实DOM!
②.若虚拟DOM中内容变了，则生成新的真实DOM，随后替换掉页面中之前的真实DOM。
(2).旧虚拟DOM中未找到与新虚拟DOM相同的key
创建新的真实DOM，随后渲染到到页面。
----3.用index作为key可能会引发的问题:
1.若对数据进行:逆序添加、逆序删除等破坏顺序操作:
会产生没有必要的真实DOM更新==>界面效果没问题，但效率低。
2.如果结构中还包含输入类的DOM:
会产生错误DOM更新==>界面有问题。

----4.开发中如何选择key?:
1.最好使用每条数据的唯一标 识作为key,比如id、 手机号、身份证号、学号等唯一值。
2.如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，
使用index作为key是没有问题的。

```
<div id="root">
        <!-- 遍历数组 -->
        <ul>
            <button @click="add">点击添加一个老刘</button>
            <li v-for="(p,index) in persons" :key="p.id">
                {{p.name}}-{{p.age}}
                <input type="text">
            </li>
        </ul>
       
        
    </div>
    <script>
        new Vue({
            el:'#root',
            data:{
                persons:[
                    {id:'001',name:'张三',age:23},
                    {id:'002',name:'李四',age:19},
                    {id:'003',name:'王五',age:8}
                ],
                cars:{
                    name:'奥迪',
                    price:'70万',
                    color:'黑色'
                },
                str:'Hello'
            },
            methods:{
                add(){
                    const p={id:'004',name:'老刘',age:70};
                    this.persons.unshift(p);
                }
            }
        })
    </script>
```

