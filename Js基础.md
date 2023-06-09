# Js基础

### 1.标识符

js中所有自主命名的都可以称为标识符

----例如：变量名、函数名、属性名

----命名一个标识符需要遵守的规则：

​						--1.标识符可以包含字母、数字、_ 、$

​						--2.标识符不能以数字开头

​						--3.标识符不能是ES中的关键字或保留字，

​								错误例子：var var = 123；

![](E:\git-workspace\Daynote\assets\js\关键字与保留字.png)

![](E:\git-workspace\Daynote\assets\js\不建议使用的标识符.png)

​						--4.标识符都采用驼峰命名法

​												-首字母小写，单词开头大写，例：helloWorld     xxxYyyZzz

----JS底层保存标识符时实际采用Unicode编码，理论上，所有的utf-8中含有的内容都可以作为标识符，中文也能作为变量名，但是不建议

### 2.字符串

----数据类型值得是字面量的类型，在JS中一共有六种数据类型

​						String 字符串（基本数据类型）

​						Number 数值（基本数据类型）

​						Boolean 布尔值（基本数据类型）

​						Null 空值（基本数据类型）

​						Undefined 未定义（基本数据类型）

​						Object 对象（引用数据类型）

----String字符串：

​					--需要用引号引起来,""或者‘’，

​					--双引号单引号都行，但是不要混着用，

​					--引号不能嵌套，双引号里不能当双引号，单引号里不能放单引号，可以使用\作为转义字符，用于表示特殊字符

```
var str = "hello";
str = "我说\"今天天气真不错！""；
```

![](E:\git-workspace\Daynote\assets\js\转义字符.png)

----Number数值：所有的数值都是Number类型

​						--包括整数和浮点数（小数）

​						--js中可以表示的数字的最大值 Number.MAX_VALUE

​																最小值Number.MIN_VALUE

​										如果使用number表示的数字超过最大值，输出Infinity，正无穷，字面量，可以直接使用

​						--NaN 是一个特殊的数字，表示不是一个数字，也可以直接使用

​						--js中整数的运算基本可以保证准确

​								浮点数运算可能会得到一个不准确的结果

```
可以使用运算符typeof检查一个变量的类型
var a = 123;数值123
var b = "123";字符串123
console.log(typeof b);
```

----Boolean布尔值：

​					--只有两个，true：真   false：假，主要用来逻辑判断

----Null空值：

​					--只有一个值，null

​					--专门用来表示一个为空的对象，使用typeof会返回Object

----Undefined未定义：

​					--只有一个值，Undefined

​					--当声明一个变量，但是不给他赋值时它的值就为undefined

​					--使用typeof返回Undefined

### 3.强制类型转换

----指的是讲一个数据类型强制转换为其他的数据类型，主要转换为String Number Boolean

----转换为String

​				---方式一：调用被转换数据类型的toString（）方法

​						该方法不会影响到原变量，会将转换的结果返回

​				---方式二：调用String（）函数，将被转换的数据作为参数传递给函数

​					-对于Number和Boolean实际上调用的还是toString（）方法

​					-但是对于Null和Undefined不会调用toString（）方法

​								将null直接转换为“null”

```
强制转换为String
var a = 123;
//a = true;
//null undefined这两个值没有toString（）方法，调用会报错
var b = a.toString();//b的类型为String
a = a.toString();
a = null;
a = String(a);//a的类型为String

```

----转换为Number

​				---方式一：使用Number（）函数		

​									1.串数字字符串，直接转换为数字

​									2.有非数字内容，转换为NaN	

​									3.空串，全是空格的串，转换为0

​									4.Boolean true转换为1 ，false转换为0

​									5.Null转换为0

​									6.Undefined转换为NaN

​				---方式二：对付字符串

​									parseInt（）把一个字符串转换为一个整数、将字符串中有效内容取出来，然后转换为Number

​									parseFloat（） 把一个字符串转换为浮点数

​									如果对非String 使用会先转换为String，然后再操作

```
强制转换为Number
var a = "123";
a = Number(a);//a为Number类型
a = "abc";
a = Number(a);//a为Number类型,结果为NaN

a = "123px";
a = parseInt(a);//结果为123
a = "123.456px";
a = parseFloat(a);//结果为123.456

a = true;
a = parseInt(a);//结果为NaN
```

----其他进制的数字

​					---16进制，以0x开头

​					---8进制，以0开头

​					---2进制，以0b开头，不是所有浏览器都支持

----转换为Boolean

​						---使用Boolean（）函数

​									数字转换成布尔值：除了0和NaN都是true

​									字符串转换成布尔值：除了空串其余都是true

​									null、undefined：false

​									对象也会转换成true

​						---隐式类型转换：为任意数据类型做两次非运算

```
var a = 234;
a = Boolean(a);//结果为true
a = 0;
a = Boolean(a);//结果为false
```

### 4.基础运算符

----也叫操作符，可以对一个值或多个值进行运算

​			--比如，typeof就是一个运算符，用来获得一个值的类型，将该值的类型以字符串形式返回

----算术运算符   + ，- ，*， /， %

​		--当对非number类型的值进行运算时，会将这些值转换为Number类型然后再进行运算

​		--任何值和NaN进行运算，都得NaN

​		--如果对两个字符串进行加法运算，会拼串操作，将两个字符串合并成一个

​		--任何值和字符串做加法运算，都会先转换为字符串，然后进行拼串操作

​		--除了加法，其他运算都会将字符串转换为Number

```
var str = "hahaha" +
		"hahuahuaha" +
		"jifjieojoiwvj" ；
		
将任意数据类型转换为String
var a = 123;
a = a + "";
//这是一种隐式的类型转换，由浏览器自动完成，还可以通过-0，*1，/1，将其转换为Number
```

### 5.一元运算符

----只需要一个操作数，+ 正号， - 负号

​					对非Number类型的数，会将其先转换成Number类型		

```
var a = 1 + "2" + 3;//结果：123
a = 1 + + "2" + 3;//结果：6
```

### 6.自增和自减

----自增：使变量在自身基础上加一（++），对一个变量自增以后，原变量的值会立即增加1

​			a++ 原变量的值（自增前的值），++a 原变量的值加1（自增后的值）

 ----自减：使变量在自身基础上减一

​			a--       自减前的值                           --a     自减后的值

```
var d = 20;
d = d++ + ++d + d;20 + 22 + 22 =64

练习：(全对)
var n1 = 10, n2 = 20;
var n = n1++;
console.log('n=' + n);10
console.log('n1 = ' + n1);11
n = ++n1;
console.log('n = '+ n);12
console.log('n1 = '+ n1);12
n = n2--;
console.log('n = '+ n);20
console.log('n2 ='+ n2 );19
n = --n2;
console.log('n = '+ n);18
console.log('n2 ='+ n2);18
```

### 7.逻辑运算符

----三种逻辑运算符， ！非，&& 与，|| 或

​					--！ 对一个值进行非运算:指对一个布尔值进行取反操作

​							对一个值进行两次取反，值不变，对非布尔值进行运算，会将其转换为布尔值，然后再进行运算，所以可以利用该特点将其他数据类型转换为布尔类型，可以为一个任意数据类型去犯两次，转换为布尔值，

​					--&& 对符号两侧的值进行与运算并返回结果

​								只要有一个false就返回false，只有两个值同时为true，返回true

​							 如果第一个值为false，不会在看第二个，短路的“与''

​					--||  两个都是false，返回false。只要有一个true，返回true

​							短路的或，第一个是true，就不会检查第二个

```
var a = true;
a = !a;//a = false

var b = 10;
b = !b;//结果为false，布尔类型

true && alert("看我出不出来!");//出来
false && alert("看我出不出来！");//不出来

false || alert（"出来不"）;//出来
true || alert("出来不");//不出来
```

----非布尔值的与和或运算

​					--对于非布尔值进行与或运算时，先转换为布尔值，在进行运算，并返回原值， 

​                      &&（找false）：1.如果第一个值为true，返回第二个（第一个值为true必须看第二个所以返回第二个），

​                      							 2.如果第一个是false，返回第一个（第一个为false就不看第二个值了所以返回第一个）

​						3.第一个值为true，返回第一个值，第二个值为false返回第二个

​						||（找true）：1.如果第一个为true，返回第一个

​													2.如果第一个为false，返回第二个值

​							

```
var a = 5 && 6;//返回6
a = 0 && 1;//返回0
a = 0 && NaN;//返回0
a = NaN && 0;//返回NaN

a = 1 || 2;//返回1
a = NaN || 1;//返回1
```

##### 8.赋值运算符

---- = 将右侧值赋值给左侧变量

---- +=    a = a + 5 等价于 a += 5

---- -= 同上

---- *= 同上

---- /= 同上

---- %= 同上

### 9.关系运算符

----比较大小关系 ，如果关系成立返回true，不成立返回false

​			-- > 大于号

​			-- < 小于号

​			-- >= 大于等于

   		-- <= 小于等于

----非数值的情况，先转换为数字，在进行比较

​			任何值和NaN做任何比较都是false

​			如果符号两边的值都是字符串，不会将其转换为数字进行比较，会比较字符串里的字符编码，比较字符编码时，一位一位比较第一位成立则直接输出，如果两位一样，则比较下一位，用它进行英文排序

​			比较中文没有意义 

​			如果比较两个字符串型的数字，会得到不可预期的结果，在比较时，一定要转型

### 10.Unicode编码

----在字符串中使用转义字符输入Unicode编码

​			\u四位编码

----在网页中使用Unicode编码

​			&#编码（需要十进制的编码，十六转十）