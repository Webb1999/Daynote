##### 42.全局作用域

指的是一个变量作用的范围

①直接编写在script标签中的js代码，都在全局作用域

​	在页面打开时创建，关闭时销毁

​	在全局作用域中有一个全局对象window，我们可以直接使用，代表的是浏览器的窗口，由浏览器创建

​	在全局作用域中，我们创建的变量都会作为window对象的属性保存

​	创建的函数都会作为window的方法保存

​	全局作用域中的变量都是全局变量，在页面的任意部位都可以访问得到

②函数作用域

​	调用函数时创建函数作用域，函数执行完毕以后，函数作用域销毁，每调用一次函数就会创建一个新的函数作用域，他们之间是互相独立的

​	函数作用域中可以访问全局变量，全局作用域不能访问函数作用域中的变量

​	当在函数作用域中操作一个变量时，会在自身作用域中寻找，如果有就直接用，没有就去上一级的作用域去找，直到找到全局作用域，如果全局作用以依然没有，就报错找不到

​	想访问全局变量，直接使用window对象

​	在函数作用域中也有声明提前的特性，使用var关键字声明的变量，会在函数中所有的代码执行之前被声明，函数声明也会在函数声明之前被声明

​	在函数中不使用var声明的变量都会称为全局变量

​	定义形参就相当于在函数作用域中声明一个变量

--变量的声明提前：

​		使用var关键字声明的变量，会在所有代码执行之前被声明（但是不会赋值），但是如果声明变量时不适用var关键字，则变量不会被声明提前

--函数的声明提前：

​		使用函数声明形式创建的函数function 函数（）{}

​		会在所有的代码执行之前就被创建，在函数声明前可以调用

​		使用函数表达式var 函数=function（）{}创建的函数，不会被声明提前，不能被声明前调用

##### 43.this

​	解析器在调用函数每次都会向函数内部传递一个隐含的参数，这个隐含的参数就是this，this值得是一个对象，这个嗯对象爱那个称为函数执行的上下文对象

​	根据函数调用的方式不同，this指向的对象不同

​	①以函数形式调用时，this永远是window

​	② 以方法形式调用，this是调用方法的对象

##### 44.使用工厂方法创建对象

```
function creatPerson(name,age,gender){
	var obj = new Object();
	obj.name = name;
	obj.age = age;
	obj.gender = gender;
	obj.SayName = function(){
		alert("this.name");
	};
	return obj;//返回新的对象
}
var obj2 = creatPerson("孙悟空",34,"男");
var obj2 = creatPerson("白骨精",340,"女");
var obj2 = creatPerson("沙和尚",384,"男");
var obj2 = creatPerson("猪八戒",24,"男");
```

##### 45.构造函数

```
创建一个构造函数
function Person(name,age,gender){
	this.name=name;
	this.age=age;
	this.gender;
	this.sayName=function(){
	alert(this.name);
	}
}
构造函数和普通函数的区别就是调用方式的不同
普通函数是直接调用，而构造函数需要使用new关键字来调用
var per = new Person("孙悟空",18,"男"); 
var per2 = new Person("玉兔精",16,"女"); 
var per3 = new Person("唐僧",48,"男"); 
console.log(per);

console.log(per instanceof Person);
```

使用instanceof检查一个对象是否是一个类的实例

