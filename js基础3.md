#### 16.if语句

```
if（条件表达式）{
	语句
}else{
	语句
}先对if后的条件表达式进行求值判断，若为true，执行if后的语句，若为false，执行else的语句
```

```
if（条件表达式）{
	语句
}else if（条件表达式）{
	语句
}.......else{
	语句
}
```

#### 17.练习1

从键盘输入小明的期末成绩：

​	当成绩为100，奖励一辆车

​	当成绩为[80-90],奖励一部手机

​	当成绩为[60-80]，奖励一本练习册

​	其他什么都没有

​	prompt（）；弹出一个提示框，会带有一个文本框，在文本框输入一段内容，该函数需要一个字符串作为参数，该字符串会作为提示框的提示文字

```
  <script>
        var a = prompt("请输入小明的期末成绩(0-100):");
        if(a>100 || a<0 || isNaN(a)){
        alert("请输入正确的成绩！！！");
        }else{
            if(a==100){
            console.log("奖励一辆车");
        }else if(a>=80 && a<=99){
            alert("奖励一台手机");
        }else if(a<=80 && a>=60){
            alert("奖励一本练习册");
        }else{
            alert("什么都没有");
        } 
        }
       
    </script>
```



#### 18.练习2

男大当婚，女大当嫁，需要提出一定条件：

高，180cm以上；富，1000万以上；帅，500以上

这三个条件同时满足：我一定要嫁给他

三个条件有为真的情况，嫁吧，比上不足比下有余

三个条件都不满足，不嫁！！！！

```
<script>
        var height = prompt("请输入身高为(cm)：");
        var wealth = prompt("请输入财富为(万)：");
        var value = prompt("请输入帅气值为：");
        if(height > 180 && wealth > 1000 && value > 500){
            alert("我一定要嫁给他");
        }else if(height>180 || wealth>1000 || value>500){
            alert("嫁吧，比上不足比下有余");
        }else{
            alert("不嫁！！！！！");
        }

    </script>
```

#### 19.练习3

由键盘输入三个整数分别存入变量num1、num2、num3，对他们进行排序，并且从小到大输出

```
 <script>
        var num1 = +prompt("请输入第一个值");//转换成number类型
        var num2 = +prompt("请输入第二个值");
        var num3 = +prompt("请输入第三个值");
        var tent;
        if(num1>num2){
            tent = num2;
            num2 = num1;
            num1 = tent;
        }
         if(num2>num3){
            tent = num3;
            num3 = num2;
            num2 = tent;
        }
         if(num1 > num2){
            tent = num2;
            num2 = num1;
            num1 = tent;
        }
        alert(num1 + "，" + num2 + "，" + num3);
    </script>
```

prompt（）函数返回值是String类型

#### 20.条件分支语句

switch语句

switch（条件表达式）{

​		case 表达式：

​				语句；

​				break；

​				........

​				default：（默认的）

​				语句；

​				break；

}

如果所有比较结果都为false执行default的语句

#### 21.练习

大于等于60输出合格，小于60输出不合格

parseInt（）取整数

```
    <script>
        var score = prompt("请输入一个成绩");
        switch(parseInt(score/10)){
            case 10:
            case 9:
            case 8:
            case 7:
            case  6:
                alert("成绩合格！！");
                break;
            default:
                alert("不合格！！！！！");
                break;
        }
    </script>
    
    
    switch（true）{
    		case score >= 60:
    			alert("成绩合格！！");
                break;
            default:
                alert("不合格！！！！！");
                break;
    }(js独有的)
```

#### 22.while循环

while（条件表达式）{

​				语句

}

先判断在执行

do{

​		语句

}while（条件表达式）

先执行在判断

#### 23.while练习

假如投资的年利率为5%，试求从1000块增长到5000块，需要多少年