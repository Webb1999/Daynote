##### 89.正则表达式语法

检查一个字符串中是否含有.
.表示任意字符，需要使用转义字符
\\表示\，\.表示.
var reg =/\./;
var reg =new RegExp("\\.");
var reg = new RegExp("\\\\");
检查字符串中有没有child这个单词
reg =/\bchild\b/;//\d表示独立的单词
去除字符串前后空格
var str="    hello    ";
str=str.replace(/\s/g,"");
str = str.repace(/^\s*|\s*$/g,"");//留中间的空格

##### 90.邮件的正则

电子邮件
hello    .nihao    @   abc   .com.cn
任意字母数字下划线    。任意字母数字下划线  @  任意 字母数字  .任意字母(2-5位)   .任意字母（2－5位）
^\w{3,}  (\.\w+)*  @   [A-z0-9]+  (\.[A-z]{2,5}){1,2} $

##### 91.DOM(文档 对象 模型)

用于操纵网页
浏览器已经为我们提供了文档节点，对象是window属性
可以在页面直接使用，文档节点代表的事整个网页
<button id="btn">我是一个按钮</button>

<script>
console.log(document);
获取button对象
var btn = document.getElementById("btn");
修改按钮的文字
btn.innerHTML="I'm Button";
</script>
#####  92.事件的简介

在事件对应属性中设置一些js代码，当事件被触发时，这些代码会执行

ondbllick 双击触发

onmousemove 鼠标移入触发

可以为按钮的对杨事件绑定处理函数来响应事件

<body>

​    *<!-- 结构和行为耦合，不推荐使用 -->*

​    <button id="btn" onclick="alert('讨厌，你点我干嘛');">我是一个按钮</button>

    <script>

​        *// 获取按钮对象*

​        var btn = document.getElementById("btn");

​        *// 单击响应函数*

​        btn.onclick = function () {

​            alert("还点我~");

​        };

​    </script>

</body>

##### 93.文档的加载

浏览器在加载一个网页时，按照自上向下的顺序加载，读取到一行就运行一行，如果将script标签写在页面的上面，在代码执行是，页面还没有加载，会报错

--onload事件在整个页面加载后触发

​		为window绑定onload事件，改时间对应的响应函数会在页面加载完成后执行，这样可以确保我们的代码执行时，所有的DOM对象已经加载完毕

94.DOM查询

--getElementsByTagName（）通过标签名获取一组元素节点对象，返回类数组对象

--getElementsByName（）通过name属性获取一组元素节点对象，返回类数组对象

--innerHTML 获取属性内部的html代码，用于获取元素内部的HTML代码，对自结束标签，这个属性没有意义

--读取元素节点属性，直接使用元素.属性名，所有的属性都可以，唯独class属性不可以采用这种方式，要用元素.className

95.图片切换的练习

```
 <style>
        * {
            margin: 0;
            padding: 0;
        }

        #outer {
            margin: 50px auto;
            width: 500px;
            padding: 10px;
            background-color: greenyellow;
            /* 设置文本居中 */
            text-align: center;
        }

        img {
            width: 400px;
            height: 521px;
            display: block;
            margin: 0 auto;
        }
    </style>
    <script>
        window.onload = function () {
            // 点击按钮切换图片
            var prev = document.getElementById("prev");
            var prenext = document.getElementById("prevnext");
            // 要切换就要修改img的src
            // 会返回数组，只取一个元素
            var img = document.getElementsByTagName("img")[0];
            // 创建一个数组，存放src路径
            var imgArr = ["./img/懒羊羊1.png", "./img/喜羊羊.jpg", "./img/小灰灰.jpg"];
            // 创建一个变量，保存当前正在显示的图片的索引
            var index = 0;
            // 设置提示文字
            var info=document.getElementById("info");
            info.innerHTML=index+1 +"/" + imgArr.length;
            prev.onclick = function () {
                // alert("下一张");
                // 切换下一张，index自加

                index++;
                if (index > imgArr.length - 1) {
                    index = 0;
                }
                img.src = imgArr[index];
                info.innerHTML=index+1 +"/" + imgArr.length;
            }
            prevnext.onclick = function () {
                // 切换上一张，index自减
                // alert("上一张");

                index--;
                if (index < 0) {
                    index = imgArr.length - 1;
                }
                img.src = imgArr[index];
                info.innerHTML=index+1 +"/" + imgArr.length;
            }


        }
    </script>
</head>

<body>
    <div id="outer">
        <img src="./img/懒羊羊1.png" alt="懒羊羊">
        <p id="info"></p>
        <br>
        <button id="prev">下一张</button>
        <button id="prevnext">上一张</button>
    </div>
</body>
```

