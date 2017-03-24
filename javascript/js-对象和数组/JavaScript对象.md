# javascript对象

## 1.简要概括

### 1.1对象是啥？

对象是JavaScript的基本数据类型。对象是一种复合值，它将很多值（原始值或者其他对象）聚合在一起，可以通过名字访问这些值。属性名是字符串，所以我们可以把对象看成是从字符串到值得映射。然而对象不仅仅是字符串到值得映射，除了可以保持自有的属性，javascript对象还可以从一个称为原型的对象继承属性。对象的方法通常是继承的属性。这种“原型式继承”是javascript的核心特征。

除了字符串、数字、true、false、undefined和null之外，javascript中的值都是对象。

对象是可变的，我们通过引用而非值来操作对象。如果变量x是指向一个对象的引用，那么var y = x; 变量y也是指向同一个对象的引用，而非这个对象的副本。通过变量y修改这个对象也会对变量x造成影响。

### 1.2顺带讲讲JS的引用赋值与传值赋值

要说js赋值方式时首先要说明js的数值类型：基本类型和引用类型。

- 基本类型

  基本的数据类型有：undefined,boolean,number,string,null。基本类型存放在栈区，访问是按值访问的，就是说你可以操作保存在变量中的实际的值。

```javascript
var a = 10
var b = a
console.log(a+','+b) //10,10
a++
console.log(a+','+b) //11,10
```

当基本类型的数据赋值时，赋的是实际的值，a和b是没有关联关系的，b由a复制得到，相互独立。

![a](.\img\a.png)

- 引用类型

  引用类型指的是对象。可以拥有属性和方法，并且我们可以修改其属性和方法。引用对象存放的方式是：在栈中存放对象变量标示名称和该对象在堆中的存放地址，在堆中存放数据。

  对象使用的是引用赋值。当我们把一个对象赋值给一个新的变量时，赋的其实是该对象的在堆中的地址，而不是堆中的数据。也就是两个对象指向的是同一个存储空间，无论哪个对象发生改变，其实都是改变的存储空间的内容，因此，两个对象是联动的。

  ![b](.\img\b.png)

```javascript
//第一种情况
var a = [1,2,3]
var b = a
a = [4,5,6]
console.log(a) //[4,5,6]
console.log(b)  //[1,2,3]
//第二中情况
var a = [1,2,3]
var b = a
a.pop()
console.log(a) //[1,2]
console.log(b) //[1,2]
```

第一种情况 a=[4,5,6]改变的是a引用本身，没有改变数组的对象，a和b没有了关系。

第二仲情况 a.pop()改变的是数组对象，a引用没有改变。

b=a 该操作后，b直接指向数组对象，不是b指向a，a再指向数组。所以改变a引用并不会对b造成影响，改变数组对象可以。

![c](.\img\c.jpg)
##2.创建对象

### 2.1对象直接量

创建对象最简单的方式就是在JavaScript代码中使用对象直接量。对象直接量是由若干名/值对组成的映射表，名/值对中间用冒号分隔，名/值对之间用逗号分隔，整个映射表用花括号括起来。

```javascript
var empty = {}  //没有任何属性的对象
var point = {x:0, y:0} //两个属性
var point2 = {x:point.x, y:point.y}  //更复杂的值
var book = {
  "main title": "javaScript",  //属性名字里面有空格，必须用字符串表示
  "sub-title": "the definitive guide",  //属性名字里面有连字符，必须用字符串表示
  "for": "all audience",  //for是保留字，因此必须用引号
  author: {     //这个属性的值是一个对象
    firstName: "wu",
    surName: "mama"
  }
}
```

在ES5中，保留字可以用做不带引号的属性名。

对象直接量是一个表达式，这个表达式的每次运算都创建并初始化一个新的对象。每次计算对象直接量的时候，也都会计算它的每个属性的值。也就是说，如果在一个重复调用的函数中的循环体内使用了对象直接量，它将创建很多新对象，并且每次创建的对象的属性值也可能不同。

### 2.2通过new创建对象

new运算符创建并初始化一个新对象。关键字new后跟随一个函数调用。这里的函数称为构造函数，构造函数用以初始化一个新创建的对象。JavaScript语音核心中的原始类型都包含内置构造函数。

```javascript
var empty = new Object()  //创建一个空对象，和var empty = {}一样
var a = new Array()  //创建一个空数组，和var a = []一样
var b = new Date()  //创建一个表示当前时间的Date对象
var c = new RegExp('js') //创建一个可以进行模式匹配的正则表达式
```

### 2.3原型

每一个JavaScript对象（null除外）都和另一个对象相关联。“另一个”对象就是我们熟知的原型，每一个对象都从原型继承属性。

所有通过对象直接量创建的对象都具有同一个原型对象，并可以通过JavaScript代码Object.prototype获得对原型对象的引用。通过关键字new和构造函数调用创建的对象的原型就是构造函数的prototype属性的值。因此，同使用{}创建对象一样，通过new Object()创建的对象也继承自Object.prototype。同样，通过new Array()创建的对象的原型就是Array.prototype，通过new Date()创建的对象的原型就是Date.prototype。

没有原型的对象为数不多，Object.prototype就是其中之一。它不继承任何属性。其他原型对象都是普通对象，普通对象都具有原型。所有的内置构造函数（以及大部分自定义的构造函数）都具有一个继承自Object.prototype的原型。例如，Date.prototype的属性继承自Object.prototype，因此由new Date()创建的Date对象的属性同时继承自Date.prototype和Object.prototype。这一系列链接的原型对象就是所谓的“原型链”。

### 2.4Object.create()

Object.create()是一个静态函数，而不是提供给某个对象调用的方法。使用它的方法很简单，只需传入所需的原型对象即可：var o = Object.create({x:1, y:2})  //o继承了属性x和y

可以通过传入参数null来创建一个没有原型的新对象，但是通过这种方式创建的对象不会继承任何东西，甚至不包括基础方法，比如toString()，也就是说，它将不能和“+”运算符一起正常工作。var o = Object.create(null)  //o不继承任何属性和方法

如果想创建一个普通的空对象（比如通过{}或者new Object()创建的对象），需要传入Object.prototype:  var o = Object.create(Object.prototype)  //o和{}和new Object()一样

可以通过任意原型创建新对象，换句话说，可以使任意对象可继承，这是一个强大的特性。

## 3.属性的查询和设置

对象可以通过点(.)或方括号([])运算符来获取属性的值。运算符左侧应当是一个表达式，它返回一个对象。对于点(.)来说，右侧必须是一个属性名称命名的简单标识符。对于方括号([])来说，方括号内必须是一个计算结果为字符串的表达式，这个字符串就是属性的名字。

```javascript
var book = {
  "main title": "javascript",
  "author": {
    firstName: "wu",
    surName: "mama"
  }
}
var author = book.author  //得到book的"author"属性
var name = author.firstName //获得author的fristName属性  wu
var title = book['main title']  //javascript
```

### 3.1属性的访问错误

属性访问并不总是返回或设置一个值。当查询一个不存在的属性并不会报错，如果在对象o自身的属性或继承的属性中均未找到属性x，属性访问表达式o.x返回undefined。

```javascript
var book = {
  "main title": "javascript",
  "author": {
    firstName: "wu",
    surName: "mama"
  }
}
book.subtitle  //undefined：属性不存在
```

但是，如果对象不存在，那么试图查询这个不存在的对象的属性就会报错。

```javascript
var len = book.subtitle.lenght
```

下面提供了两种避免出错的方法：

```javascript
var len = undefined
if(book){
  if(book.subtitle){
    len = book.subtitle.length
  }
}
```

```javascript
var len = book && book.subtitle && book.subtitle.lenght
```

## 4.删除属性

delete运算符可以删除对象的属性。delete只是断开属性和宿主对象的联系，而不会去操作属性中的属性。

```javascript
var a = {
  p:{x:1}
}
b = a.p
delete a.p 
```

执行这段代码之后，b.x的值依然是1。由于已经删除的属性的引用依然存在，因此在javascript的某些实现中，可能因为这种不严谨的代码而造成内存泄露。所以在销毁对象的时候，要遍历属性中的属性，依次删除。

```javascript
delete book.author  //book不再有属性author
delete book['main title']  //book不再有属性main title
```

delete运算符只能删除自有属性，不能删除继承属性（要删除继承属性必须从定义这个属性的原型对象上删除它，而且这会影响到所有继承自这个原型的对象）。当delete表达式删除成功或没有任何副作用（比如删除不存在的属性）时，它返回true。如果delete后不是一个属性访问表达式，delete同样返回true。

```javascript
var o = {x:1}  //o有一个属性x，并继承属性toString
delete o.x  //删除x，返回true
delete o.x  //什么都没做（x已经不存在了），返回true
delete o.toString  //什么也没做（toString是继承来的），返回true
delete 1  //无意义，返回true
```

delete不能删除那些可配置性为false的属性（尽管可删除不可扩展对象的可配置属性）。某些内置对象的属性是不可配置的，比如通过变量声明和函数声明创建的全局对象的属性。在严格模式中，删除一个不可配置属性会报一个类型错误。在非严格模式中（ES3中），在这些情况下的delete操作会返回false。

```javascript
delete Object.prototype  //不能删除 属性是不可配置的
var x = 1   //声明一个变量
delete this.x  //不能删除这个属性
function f(){}  //声明一个函数
delete this.f  //不能删除函数
```

在非严格模式下，删除全局对象的可配属性时，可以省略对全局对象的引用，直接在delete操作符后跟随要删除的属性名即可：

```javascript
this.x = 1  //创建一个可配置的全局属性（没有var）
delete x  //将它删除
```

然而在严格模式中，delete后跟随一个非法的操作数（比如x），则会报一个语法错误。因此必须显式对象及其属性：

```javascript
delete x  //在严格模式下语法报错
delete this.x  //正常工作
```

## 5.检测属性

javascript对象可以看做属性的集合，我们经常会检测集合中成员的所属关系——判断某个属性是否存在于某个对象中。可以通过in运算符、hasOwnProperty()和propertyIsEnumerable()方法来完成这个工作，甚至仅通过属性查询也可以做到这一点。

in运算符的左侧是属性名（字符串），右侧是对象。如果对象的自有属性或继承属性中包含这个属性则返回true：

```javascript
var o = {x:1}
"x" in o  //true "x"是o的属性
"y" in o  //false "y"不是o的属性
"toString" in o  //true o继承toString 属性
```

对象的hasOwnProperty()方法用来检测给定的名字是否是对象的自有属性。对于继承属性它将返回false:

```javascript
var o = {x:1}
o.hasOwnProperty('x')  //true o有有一个自有属性x
o.hasOwnProperty('y')  //false o中不存在属性y
o.hasOwProperty('toString')  //false toString是继承属性
```

propertyIsEnumerable()是hasOwnProperty()的增强版，只有检测到是自有属性且这个属性的枚举性为true时它才返回true。某些内置属性是不可枚举的。通常由JavaScript代码创建的属性都是可以枚举的，除非在ES5中使用一个特殊的方式来改变属性的可枚举性。

```javascript
var o = {}
o.x = 1
o.propertyIsEnumerable('x')   //true o有一个可枚举的自有属性x
Object.prototype.propertyIsEnumerable('toString')  //false  不可枚举
```

除了使用in运算符之外，另一种更简便的方法是使用“!==”判断一个属性是否是undefined。

```javascript
var o = {x:1}
o.x !== undefined  //true o中有属性x
o.y !== undefined  //false o中没有属性y
o.toString !== undefined  //true o继承了toString属性
```

然而有一种场景只能使用in运算符而不能使用"!=="。in可以区分不存在的属性和存在但值为undefined的属性。

```javascript
var o = {x:undefined}
o.x !== undefined  //false 属性存在，但值为undefined
o.y !== undefined  //false 属性不存在
"x" in o   //true 属性存在
"y" in o   //false 属性不存在
delete o.x  //删除了属性x
"x" in o  //false 属性不存在
```

注意，上述代码中使用的是"!=="运算符，而不是"!="。"!=="可以区分undefined和null。有时则不必作这种区分：

```javascript
//如果o中含有属性，且x的值不是undefined和null，o.x乘以2
if(o.x != null) o.x *= 2
//如果o中含有属性x，且x的值不能转换为false，o.x乘以2
//如果x是undefined、null、false、" "、0或NaN，则它保持不变
if(o.x) o.x *= 2
```

## 6.枚举属性

除了检测对象的属性是否存在，我们还会经常遍历对象的属性。通常使用for/in循环遍历。

```javascript
var o = {x:1, y:2, z:3}  //三个可以枚举的自有属性
o.propertyIsEnumerable('toString')  //false 不可枚举
for(p in o)  //遍历属性
console.log(p)  // 输出x y z 不会输出toString
```

## 7.序列化对象

对象序列华是指将对象的状态转换为字符串，也可以将字符串还原为对象。ES5提供了内置函数Json.stringify()和JSON.parse()用来序列化和还原javascript对象。

```javascript
o = {
  x:1,
  y:{
    z:[false,null,""]
  }
}   //定义一个对象
s = JSON.stringify(o)  //s为 '{"x":1,"y":{"z":[false,null,""]}}'
p = JSON.parse(s)     //p是o的深拷贝
```

JSON的语法是javascript语法的子集，它并不能标示javascript里的所有值。支持对象、数组、字符串、无穷大数字、true、false和null，并且它们可以序列化和还原。NaN、Infinity和-Infinity序列化的结果是null，日期对象序列化的结果是ISO格式的日期字符串，但JSON.parse()依然保留它们的字符串形态，而不会将它们还原为原始日期对象。函数、RegExp、Error对象和undefined值不能序列化和还原。JSON.stringify()只能序列化对象可枚举的自有属性。对于一个不能序列化的属性来说，在序列化后的输出字符串中会将这个属性省略掉。JSON.stringify()和JSON.parse()都可以接收第二个可选参数，通过传入需要序列化或还原属性列表来定制自定义的序列化或还原操作。

## 8.对象方法

### 8.1toString()方法

toString()方法没有参数，它将返回一个标示调用这个方法的对象值的字符串。在需要将对象转换为字符串的时候，javascript都会调用这个方法。

```javascript
//数组
var array = ["CodePlayer", true, 12, -5]
console.log( array.toString() ) // CodePlayer,true,12,-5

// 日期
var date = new Date(2013, 7, 18, 23, 11, 59, 230)
console.log( date.toString() ) // Sun Aug 18 2013 23:11:59 GMT+0800 (中国标准时间)

// 日期2
var date2 = new Date(1099, 7, 18, 23, 11, 59, 230)
console.log( date2.toString() ) // Fri Aug 18 1099 23:11:59 GMT+0800 (中国标准时间)

// 数字
var num =  15.26540
console.log( num.toString() ) // 15.2654

// 布尔
var bool = true
console.log( bool.toString() ) // true

// Object
var obj = {name: "张三", age: 18}
console.log( obj.toString() ) // [object Object]

// HTML DOM 节点
var eles = document.getElementsByTagName("body")
console.log( eles.toString() ) // [object NodeList]
console.log( eles[0].toString() ) // [object HTMLBodyElement]
```

### 8.2toLocaleString()方法

除了基本的toString()方法之外，对象都包含toLocaleString()方法，这个方法返回一个标示这个对象的本地化字符串。Object中默认的toLocalString()方法并不做任何本地化自身的操作，它仅调用toString()方法并返回对应值。Date和Number类对toLocaleString()方法做了定制，可以用它对数字、日期和时间做本地化的转换。Array类的toLocaleString()方法和toString()方法很像，唯一的不同是每个数组元素会调用toLocaleString()方法转换为字符串，而不是调用各自的toString()方法。

```javascript
//数组
var array = ["CodePlayer", true, 12, -5]
console.log( array.toLocaleString() ) // CodePlayer,true,12,-5

// 日期
var date = new Date(2013, 7, 18, 23, 11, 59, 230)
console.log( date.toLocaleString() ) // 2013/8/18 下午11:11:59

// 日期2
var date2 = new Date(1099, 7, 18, 23, 11, 59, 230)
console.log( date2.toLocaleString() ) // 1099/8/18 下午11:17:42

// 数字
var num =  15.26540
console.log( num.toLocaleString() ) // 15.2654

// 布尔
var bool = true
console.log( bool.toLocaleString() ) // true

// Object
var obj = {name: "张三", age: 18}
console.log( obj.toLocaleString() ) // [object Object]

// HTML DOM 节点
var eles = document.getElementsByTagName("body")
console.log( eles.toLocaleString() ) // [object HTMLCollection]
console.log( eles[0].toLocaleString() ) // [object HTMLBodyElement]
```

### 8.3toJSON()方法

Object.prototype实际上没有定义toJSON()方法，但对于需要执行序列化的对象来说，JSON.stringify()方法会调用toJSON()方法。如果在待序列化的对象中存在这个方法，则调用它，返回值即是序列化的结果，而不是原始的对象。

### 8.4valueOf()方法

valueOf()方法和toString()方法非常类似。但往往当javascript需要将对象转换为某种原始值而飞字符串的时候才会调用它，尤其是转换为数字的时候。如果在需要使用原始值的上下文中使用了对象，javascript就会自动调用这个方法。

# javascript数组

数组是值的有序集合。每个值叫做一个元素，而每个元素在数组中有一个位置，以数字表示，称为索引。javascript数组是无类型的：数组元素可以是任意类型，并且同一个数组中的不同元素也可能有不同的类型。数组的元素甚至也可能是对象或其他数组，这允许创建复杂的数据结构，如对象的数组和数组的数组。javascript数组是动态的：根据需要他们会增长或缩短，并且在创建数组时无须声明一个固定的大小或者在数组大小变化时无须重新分配空间。

### 1.创建数组

- 使用数组直接量是创建数组最简单的方法，在方括号中将数组元素用逗号隔开即可。

```javascript
var empty = []  //没有元素的数组
var primes = [2,3,4,5,6]  //有5个数值的数组
var misc = [1,true,"a", ] //3个不同类型的元素和结尾的逗号
```

数组直接量中的值不一定要是常量，它们可以是任意的表达式：

```javascript
var base = 1024
var table = [base, base+1, base+2, base+3]
```

它可以包含对象直接量或者其他数组直接量：

```javascript
var a = [[1,{x:1, y:2}],[2,{x:3,y:4}]]
```

如果省略数组直接量中的某个值，省略的元素将被赋予undefined值：

```javascript
var count = [1,,3] //数组有3个元素，中间的那个元素值为undefined
var undefs = [,,]  //数组有两个元素，都是undefined
```

数组直接量的语法允许有可选的结尾的逗号，所以[,,]只有两个元素而非三个。

- 调用构造函数Array()是创建数组的另一种方法。可以用3中方式调用构造函数。

  - 调用时没有参数：var a = new Array()
  - 调用时有一个数值参数，它指定长度：var a = new Array(10)
  - 显式指定两个或多个数组元素或者数组的一个非数组元素：var a = new Array(1,2,3,4,"test")
 ### 2.数组元素的读和写

使用[]操作符来访问数组中的一个元素。数组的引用位于方括号的左边。方括号是一个返回非负整数值的任意表达式。使用该语法既可以读又可以写数组的一个元素。

```javascript
var a = ["world"]  //只有一个元素的数组
var value = a[0]  //world  读第0个元素
a[1] = 110  //写第一个元素
a[a[2]] = a[0] //读第0个和第2个元素，写第3个元素
```

数组的特别之处在于，当使用小于2的32次方的非负整数作为属性名时数组会自动维护其length属性值。如上，创建仅有一个元素的数组，然后在索引1,2和3处分别进行赋值。当我们这么做时数组的length属性值变为：a.length  为4。

所有的数组都是对象，可以为其创建任意名字的属性。

可以使用负数或非整数来索引数组。数组转换为字符串，字符串作为属性名来用，既然名字不是非负整数，它就只能当做常规的对象属性，而非数组的索引。同样，如果凑巧使用了是非负整数的字符串，它就当做数组索引，而非对象属性，当使用的一个浮点数和一个整数相等时情况也是一样的。

```javascript
a[-1.23] = true //这将创建一个名为“-1.23”的属性
a["1000"] = 0 //这是数组的第1001个元素
a[1.00]  //和a[1]相等
```

### 3.稀疏数组

稀疏数组就是包含从0开始的不连续索引的数组。通常，数组的length属性值代表数组中的元素个数。如果数组是稀疏的，length属性值大于元素的个数。

```javascript
a = new Array(5)  //数组没有元素，但是a.length是5
a = []  //创建一个空数组，length = 0 
a[1000] = 0  //赋值添加一个元素，但是设置length为1001
```

当在数组直接量中省略值时不会创建稀疏数组。省略的元素在数组中是存在的，其值为undefined。

```javascript
var a1 = [,,,]  //数组是[undefined,undefined,undefined]
var a2 = new Array(3)  //该数组根本没有元素
0 in a1  //true a1在索引0处有一个元素
0 in a2  //false a2在索引0处没有元素
```

### 4.数组长度

每个数组有一个length属性，就是这个属性使其区别常规的JavaScript对象。在数组中肯定找不到一个元素的索引值大于或等于它的长度。

设置length属性为一个小于当前长度的非负整数n时，当前数组中那些索引值大于或等于n的元素将从中删除：

```javascript
a = [1,2,3,4,5] 
a.length = 3  //a为[1,2,3]
a.length = 0  //删除所有的元素 a为[]
a.length = 5  //长度为5，但是没有元素，就像new Array(5)
```

在ES5中，可以用Object.defineProperty()让数组的length属性变成只读的：

```javascript
a = [1,2,3]
Object.defineProperty(a,"length",{writable:false})  //让length属性只读
a.length = 0  //a不会改变
```

### 5.数组元素的添加和删除

添加数组元素最简单的方法就是为新索引赋值：

```javascript
a = []  //开始是一个空数组
a[0] = "zero" //然后向其中添加元素
a[1] = "one"
```

- push()

也可以用push()方法在数组末尾增加一个或多个元素：

```javascript
a = []
a.push('zero')  //a = ['zero']
a.push('one','two')  //a = ['zero','one','two']
```

- unshift()

在数组尾部添加一个元素与给数组a[a.length]赋值是一样的。可以使用unshift()方法在数组的首部插入一个元素，并且将其他元素依次移到更高的索引处。

- delete

可以像删除对象属性一样使用delete运算符来删除数组元素：

```javascript
a = [1,2,3]
delete a[1] //a在索引1的位置不再有元素
1 in a  //false 数组索引1并未在数组中定义
a.length  //3 delete操作并不影响数组长度
```

删除数组元素与为其赋undefined值是类似的。注意，对一个数组元素使用delete不会修改数组的length属性，也不会将元素从高索引处移下来填充已删除属性留下来的空白。如果从数组中删除一个元素，它就变成稀疏数组。

- pop()

数组有pop()方法(它和push()一起使用)，后者一次使减少长度1并返回被删除元素的值。还有一个shift()方法(它和unshift()一起使用)，从数组头部删除一个元素。和delete不同的是shift()方法将所有元素下移到比当前索引低1的位置。

- splice()

splice()是一个通用的方法来插入、删除或替换数组元素。它会根据需要修改length属性并移动元素到更高或较低的索引处。

```javascript
arrayObject.splice(index,howmany,item1,.....,itemX)
```

index：必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置。

howmany：必需。要删除的项目数量。如果设置为 0，则不会删除项目。

item1,.....itemX：可选。向数组添加的新项目。

```javascript
var arr = new Array(6)
arr[0] = "George"
arr[1] = "John"
arr[2] = "Thomas"
arr[3] = "James"
arr[4] = "Adrew"
arr[5] = "Martin"
arr.splice(2,0,"William") //George,John,William,Thomas,James,Adrew,Martin
```

### 6.数组遍历

使用for循环是遍历数组元素最常见的方法：

```javascript
var keys = Object.keys(o) //获得o对象属性名组成的数组
var values = []  //在数组中存储匹配属性的值
var len = keys.length 
for(var i = 0;  i < len; i++){  
  var key = keys[i]  //获取索引处的键值
  values[i] = o[key]  //在values数组中保存属性值
}
```

ES5定义了一些遍历数组元素的新方法，按照索引的顺序按个传递给定义的一个函数。这些方法最常用的就是forEach()方法：

```javascript
var data = [1,2,3,4,5]
var sum = 0 //要得到数据的平方和
data.forEach(function(x){  //把每个元素传递给此函数
  sum += x*x //平方相加
})
console.log(sum)  //55 1+4+9+16+25
```

### 7.多维数组

JavaScript不支持真正的多维数组，但可以用数组的数组来近似。访问数组的数组中的元素，只要简单地使用两次[]操作符即可。

使用二位数组做一个九九乘法表：

```javascript
var table = new Array(10)  //表格有10行
var len = table.length
for(var i = 0; i < len; i++){
  table[i] = new Array(10)  //每行有10列
}
//初始化数组
for(var row = 0; row < len ;row++){
  for(var col = 0; col < table[row].length; col++){
    table[row][col] = row*col
  }
}
var product = table[5][7]  //35
```

### 8.数组方法

- join()

Array.join()方法将数组中所有元素都转化为字符串并连接在一起，返回最后生成的字符串。可以指定一个可选的字符串在生成的字符串中来分隔数组的各个元素。如果不指定分隔符，默认使用逗号。

```javascript
var a = [1,2,3]
a.join() //'1,2,3'
a.join(" ")  //'1 2 3'
a.join("")  //'123'
var b = new Array(10) //长度为10的空数组
b.join('-')  //'---------'9个连字号组成的字符串
```

- reverse()

Array.reverse()方法将数组中的元素颠倒顺序，返回逆序的数组。它采取了替换，换句话说，它不通过重新排列的元素创建新的数组，而是在原先的数组中重庆排列它们。

```javascript
var a = [1,2,3]
a.reverse().join()  //现在的a是[3,2,1]
```

- sort()

Array.sort()方法将数组中的元素排序并返回排序后的数组。

```javascript
var a = new Array('b','c','a','e')
a.sort()
console.log(a.sort())  //'a,b,c,e'
```

如果数组包含undefined元素，它们会被排到数组的尾部。

为了按照其他方式而非字母表顺序进行数组排序，必须给sort()方法传递一个比较函数。

```javascript
var a = [33,4,1111,22]
a.sort()  //字母表顺序 1111,222,33,4
a.sort(function(a,b){
  return a-b //根据顺序，返回负数、0、正数
})
a.sort(function(a,b){
  return b-a  //数值大小相反的顺序
})
```

- concat()

concat()方法创建并返回一个新数组，它的元素包括调用concat()的原始数组的元素和concat()的每个参数。如果这些参数中的任何一个自身是数组，则连接的是数组的元素，而非数组本身。但要注意，concat()不会递归扁平化数组的数组。concat()也不会修改调用的数组。

```javascript
var a = [1,2,3]
a.concat(4,5)  //[1,2,3,4,5]
a.concat([4,5]) //[1,2,3,4,5]
a.concat([4,5],[6,7]) //[1,2,3,4,5,6,7]
a.concat(4,[5,[6,7]]) //[1,2,3,4,5,[6,7]]
```

- slice()

Array.slice()方法返回指定数组的一个片段或子数组。它的两个参数分别指定了片段的开始和结束的位置。返回的数组包含第一个参数指定的位置和所有到但不含第二个参数指定的位置之间的所有数组元素。如果只指定一个参数，返回的数组将包含从开始位置到数组结尾的所有元素。如参数中出现负数，它表示相对于数组中最后一个元素的位置。

```javascript
var a = [1,2,3,4,5]
a.slice(0,3) //[1,2,3]
a.slice(3) //[4,5]
a.slice(1,-1) //[2,3,4]
a.slice(-3,-2) //[3]
```

- splice()

Array.splice()方法是在数组中插入或删除元素的通用方法。不同于slice()和concat()，splice()会修改调用的数组。

splice()能够从数组中删除元素、插入元素到数组中或者同时完成这两种操作。在插入或删除点之后的数组元素会根据需要增加或减小它们的索引值，因此数组的其他部分任然保持连续的。splice()的第一个参数制定了插入和（或）删除的起始位置。第二个参数指定了应该从数组中删除的元素的个数。如果省略第二个参数，从起始点开始到数组结尾的所有元素都将被删除。splice()返回一个由删除元素组成的数组，或者如果没有删除元素就返回一个空数组。

```javascript
var a = [1,2,3,4,5,6,7,8]
a.splice(4)  //返回[5,6,7,8] a是[1,2,3,4]
a.splice(1,2)  //返回[2,3] a是[1,4]
a.splice(1,1)  //返回[4] a是[1]
```

splice()的前两个参数指定了需要删除的数组元素。紧随其后的任意个数的参数指定了需要插入到数组中的元素，从第一个参数指定的位置开始插入。

```javascript
var a = [1,2,3,4,5]
a.splice(2,0,'a','b')  //返回[] a是[1,2,'a','b',3,4,5]
a.splice(2,2,[1,2],3) //返回['a','b'] a是[1,2,[1,2],3,3,4,5]
```

- push()和pop()

push()和pop()方法允许将数组当做栈来使用。push()方法在数组的尾部添加一个或多个元素，并返回数组新的长度。pop()方法则相反：它删除数组的最后一个元素，减少数组长度并返回它删除的值。注意，两个方法都修改并替换原始数组而非生成一个修改版的新数组。

```javascript
var a = []
a.push(1,2) //a:[1,2] 返回2
a.pop() //a:[1] 返回2
a.push(3) //a:[1,3]  返回2
a.pop()  //a:[1]  返回3
a.pop([4,5])  //a:[1,[4,5]] 返回2
a.pop()   //a:[1]  返回[4,5]
a.pop()  //a: []
```

- unshift()和shift()

unshift()和shift()方法的行为非常类似于push()和pop()，不一样的是前者是在数组的头部而非尾部进行元素的插入和删除操作。unshift()在数组的头部添加一个或多个元素，并将已存在的元素移动到更高索引的位置来获得足够的空间，最后返回数组新的长度。shift()删除数组的第一个元素并将其返回，然后把所有随后的元素下移一个位置来填补数组头部的空缺。

```javascript
var a = []
a.unshift(1)  //a:[1] 返回1
a.unshift(22) //a:[22,1]  返回2
a.shift()  //a:[1]  返回22
a.unshift(3,[4,5])  //a:[3,[4,5],1] 返回3
a.shift()  //a:[[4,5],1]  返回3
a.shift()  //a:[1] 返回[4,5]
a.shift()  //a:[]  返回1
```

- toString()和toLocaleString()

数组和其他JavaScript对象一样拥有toString()方法。针对数组，该方法将其每个元素转化为字符串并且输出用逗号分隔的字符串列表。

```javascript
[1,2,3].toString()  //'1,2,3'
['a','b','c'].toString()  //'a,b,c'
[1,[2,'c']].toString()  //'1,2,c'
```

toLocaleString()是toString()方法的本地化版本。它调用元素的toLocaleString()方法将每个数组元素转化为字符串。

### 9.ES5中的数组方法

- forEach()

forEach()方法从头至尾遍历数组，为每个元素调用指定的函数。forEach()使用三个参数调用该函数：数组元素、元素的索引和数组本身。

```javascript
var data = [1,2,3,4,5]
//计算数组元素的和值
var sum = 0;
data.forEach(function(value){
  sum += value
})
console.log(sum)  //15
//每个数组元素的值自加1
data.forEach(function(v,i,a){
  a[i] = v + 1
})
console.log(data)  //[2,3,4,5,6]
```

- map()

map()方法将调用的数组的每个元素传递给指定的函数，并返回一个数组，它包含该函数的返回值。

```javascript
a = [1,2,3]
b = a.map(function(x){
  return x*x
})
console.log(b)  //[1,4,9]
```

注意，map()返回的是新数组，它不修改调用的数组。

- filter()

filter()方法返回的数组元素是调用的数组的一个子集。传递的函数是用来逻辑判定的：该函数返回true或false。

```javascript
a = [5,4,3,2,1]
minvalues = a.filter(function(x){
  return x < 3
})  // [2,1]
everyother = a.filter(function(x,i){
  return i%2 == 0
})  //[5,3,1]
```

- every()和some()

every()和some()方法是数组的逻辑判定：它们对数组元素应用指定的函数进行判定，返回true或false。every()方法当且仅当针对数组中所有元素调用判定函数都返回true，它才返回true。

```javascript
a = [1,2,3,4,5]
a.every(function(x){
  return x < 10
})//true  所有的值都小于10
a.every(function(x){
  return x % 2 === 0 
})//false 不是所有的值都是偶数

```

some()方法当数组中至少有一个元素调用判定函数返回true，它就返回true，并且当且仅当数值中的所有元素调用判定函数都返回false，它才返回false。

```javascript
a = [1,2,3,4,5]
a.some(function(x){
  return x % 2 === 0 
})//true a含有偶数值
a.some(isNaN)//false a不包含非数值元素
```

- reduce()和reduceRight()

reduce()和reduceRight()方法使用指定的函数将数组元素进行组合，生成单个值。

reduce()方法接收一个函数callbackfn作为累加器，数组中的每个值（从左到右）开始合并，最终为一个值。

语法：
array.reduce(callbackfn,[initialValue])
reduce()方法接收callbackfn函数，而这个函数包含四个参数：

function callbackfn(preValue,curValue,index,array){}
preValue: 上一次调用回调返回的值，或者是提供的初始值（initialValue）
curValue: 数组中当前被处理的数组项
index: 当前数组项在数组中的索引值
array: 调用 reduce()方法的数组
而initialValue作为第一次调用 callbackfn函数的第一个参数。

reduce()方法为数组中的每一个元素依次执行回调函数callbackfn，不包括数组中被删除或从未被赋值的元素，接受四个参数：初始值（或者上一次回调函数的返回值），当前元素值，当前索引，调用 reduce() 的数组。

回调函数第一次执行时，preValue 和 curValue 可以是一个值，如果 initialValue 在调用 reduce() 时被提供，那么第一个 preValue 等于 initialValue ，并且curValue 等于数组中的第一个值；如果initialValue 未被提供，那么preValue 等于数组中的第一个值，curValue等于数组中的第二个值。

```javascript
var arr = [0,1,2,3,4]
arr.reduce(function (preValue,curValue,index,array) {
    return preValue + curValue
}) // 10
```

![d](.\img\d.png)
上面的示例reduce()方法没有提供initialValue初始值，接下来再上面的示例中，稍作修改，提供一个初始值，这个值为5。这个时候reduce()方法会执行五次回调，每次参数和返回的值如下：

```javascript
var arr = [0,1,2,3,4]
arr.reduce(function (preValue,curValue,index,array) {
    return preValue + curValue
}, 5) //15
```

![e](C:\Users\wuqin\Desktop\javascript对象和数组\img\e.png)
reduceRight()方法的功能和reduce()功能是一样的，不同的是reduceRight()从数组的末尾向前将数组中的数组项做累加。

- indexOf()和lastIndexOf()

indexOf()和lastIndexOf()搜索整个数组中具有给定值的元素，返回找到的第一个元素的索引或者如果没有找到就返回-1。indexOf()从头至尾搜索，而lastIndexOf()则反向搜索。

```javascript
a = [0,1,2,1,0]
a.indexOf(1)  //1
a.lastIndexOf(1)  //3
a.indexOf(3) //-1 没有值为3的元素
```



### 10.数组类型

给定一个未知的对象，判定它是否为数组。

```javascript
Array.isArray([])  //true
Array.isArray({})  //false
```


