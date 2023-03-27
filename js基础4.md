#### 23.while练习

假如投资的年利率为5%，试求从1000块增长到5000块，需要多少年

```
   <script>
        var capital = 1000,i = 0;
        while(capital<=5000){
            capital=(capital*0.05 + capital);
            i++;
        }
        console.log("本金："+ capital+ "，需要"+i+"年");
        //本金：5003.188542033786，需要33年
    </script>
```

24.成绩分类改进

```
 <script>
        var a = prompt("请输入小明的期末成绩");
        while(a>100 || a<0 || isNaN(a)){
        alert("请输入正确的成绩！！！");
        a = prompt("请输入小明的期末成绩");
        }
        if(a==100){
            console.log("奖励一辆车");
        }else if(a>=80 && a<=99){
            alert("奖励一台手机");
        }else if(a<=80 && a>=60){
            alert("奖励一本练习册");
        }else{
            alert("什么都没有");
        } 
    </script>
```

##### 25.for循环

for（初始化表达式；条件表达式；更新表达式）{

​			语句

}

##### 26.for循环练习

①打印1-100之间所有奇数之和

```
<script>
        var sum=0;
        for(var i=1;i<=100;i++){
            console.log(i);
            sum=sum+i;
            i=i+1;
            
        }
        alert("奇数和为："+sum);
        console.log("奇数和为："+sum);
    </script>
```

②水仙花数

```
<script>
        var sole = 0, ten = 0, hundred = 0;

        for (var i = 100; i < 1000; i++) {
            var sum = 0;
            hundred = parseInt(i / 100);
            ten = parseInt((i - hundred * 100) / 10);
            sole = parseInt((i - hundred * 100 - ten * 10));
            //sole=parseInt(i%10);
            sum = sole * sole * sole + ten * ten * ten + hundred * hundred * hundred;
            if (i == sum) {
                console.log(i);
            }
        }
    </script> 
```

③质数

```
<script>
        var num = prompt("请输入一个整数");
        if(num <=0){
            alert("输入错误！！");
        }else {
            var flag = true;
            for(var i =2;i<num;i++){
                if(num%i ==0){
                    flag = false;
                    break;
                }
            }
        }
        if(flag){
            alert(num + "是质数");
        }else{
            alert(num+"不是质数");
        }
    </script>
```

##### 27.嵌套的for循环

```
 <script>
        var i ,j;
        for(i=0;i<5;i++){
            for(j=0;j<=i;j++){
                document.write("*");
            }
            document.write("<br />");
        }
    </script>
    
```

```html
<script>
        var i ,j;
        for(i=0;i<5;i++){
            for(j=0;j<5-i;j++){
                document.write("*");
            }
            document.write("<br />");
        }
    </script>
```

28.嵌套for循环练习

①99乘法表

<script>
        var i,j;
        for(i=1;i<10;i++){
            for(j=1;j<=i;j++){
                document.write(j+"*"+i+"="+i*j+"&nbsp&nbsp&nbsp&nbsp");
            }
            document.write("<br />");
        }
    </script>

②打印1-100之间质数

<script>
        // var num = prompt("请输入一个整数");
        for (var i = 1; i <= 100; i++) {
            var flag = true;
            for (var j = 2; j < i; j++) {
                if (i % j == 0) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                document.write(i + "是质数<br />");
            } 
        }
    </script>

改进：j<Math.sqrt(i);对i开平方

##### 29.break和continue

break关键字会立刻中职离他最近的那个循环语句

可以为循环语句常见一个label，来标识当前的循环

label：循环语句

使用break语句时，可以在break后跟一个label，这样break会结束指定循环而不是最近的

```
outer:
for(var i=0;i<5;i++){
console.log("@外层循环"+i);
	for(var j=0;j<5;j++){
	break outer;
		console.log("内层循环"+j);
	}
}//输出："@外层循环0"
```

continue用来跳过当次循环



console.time("计时器的名字")//测试程序性能

console.timeEnd("计时器的名字")//终止计时器

##### 30.对象的简介

  ①内建对象

​	由es标准定义的的对象，在任何的es的实现中都可以使用

②宿主对象

​	由js的运行环境提供的对象，主要指浏览器提供的对象

③自定义对象

​	由开发人员自己创建的对象