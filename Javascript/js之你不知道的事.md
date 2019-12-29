https://www.jianshu.com/p/cd3fee40ef59?utm_campaign=haruki&utm_content=note&utm_medium=reader_share&utm_source=weixin

### 内存空间

https://www.jianshu.com/p/996671d4dcc4

#### 三种数据结构

与C/C++不同，JavaScript中并没有严格意义上区分栈内存与堆内存。因此我们可以简单粗暴的理解为JavaScript的所有数据都保存在堆内存中。

##### 堆(heap)

堆数据结构是一种树状结构。它的存取数据的方式，则与书架与书非常相似。

书虽然也整齐的存放在书架上，但是我们只要知道书的名字，我们就可以很方便的取出我们想要的书，而不用像从乒乓球盒子里取乒乓一样，非得将上面的所有乒乓球拿出来才能取到中间的某一个乒乓球。好比在JSON格式的数据中，我们存储的`key-value`是可以无序的，因为顺序的不同并不影响我们的使用，我们只需要关心书的名字。

##### 栈(stack)-----先进后出，后进先出

栈的存取方式

![栈](images\stack.png)

##### 队列(queue)------先进先出(FIFO)

![队列](images\queue.png)

#### 变量对象



#### 基本数据类型

JavaScript的5种基本数据类型（undefined，Null，Number，String，Boolean）（ES6新加一种基本数据类型Symbol，先不考虑）都是按值访问，直接保存在变量对象中

#### 引用数据类型与堆内存

引用数据类型的值是保存在堆内存中的对象。

JavaScript不允许直接访问堆内存中的位置，因此我们不能直接操作对象的堆内存空间。在操作对象时，实际上是在操作对象的引用而不是实际的对象。因此，引用类型的值都是按引用访问的。这里的引用，我们可以理解为保存在变量对象中的一个地址，该地址与堆内存的实际值相关联。

例子：

~~~javascript
var a1 = 0;   // 变量对象
var a2 = 'this is string'; // 变量对象
var a3 = null; // 变量对象

var b = { m: 20 }; // 变量b存在于变量对象中，{m: 20} 作为对象存在于堆内存中
var c = [1, 2, 3]; // 变量c存在于变量对象中，[1, 2, 3] 作为对象存在于堆内存中
~~~

![example](images\example.png)

因此当我们要访问堆内存中的引用数据类型时，实际上我们首先是从变量对象中获取了该对象的地址引用（或者地址指针），然后再从堆内存中取得我们需要的数据。

#### 面试题目

~~~javascript
// demo01.js
var a = 20;
var b = a;
b = 30;
//问此时a为多少
~~~

在变量对象中的数据发生复制行为时，系统会自动为新的变量分配一个新值。`var b = a`执行之后，a与b虽然值都等于20，但是他们其实已经是相互独立互不影响的值了。具体如图。所以我们修改了b的值以后，a的值并不会发生变化。

![demo01](images\demo01.png)

~~~javascript
// demo02.js
var m = { a: 10, b: 20 }
var n = m;
n.a = 15;

// 这时m.a的值是多少
~~~

在demo02中，我们通过`var n = m`执行一次复制引用类型的操作。引用类型的复制同样也会为新的变量自动分配一个新的值保存在变量对象中，但不同的是，这个新的值，仅仅只是引用类型的一个地址指针。当地址指针相同时，尽管他们相互独立，但是在变量对象中访问到的具体对象实际上是同一个。如图所示。

因此当我改变n时，m也发生了变化。这就是引用类型的特性。

![demo02](images\demo02.png)

### 执行上下文图解

每次当控制器转到可执行代码的时候，就会进入一个执行上下文。**执行上下文可以理解为当前代码的执行环境，它会形成一个作用域。**

**执行上下文就是函数执行的环境**，每一个函数执行时，都会给对应的函数创建这样一个执行环境。

![](images\执行上下文内部结构.png)

例如：

~~~javascript
var color = 'blue';

function changeColor() {
    var anotherColor = 'red';

    function swapColors() {
        var tempColor = anotherColor;
        anotherColor = color;
        color = tempColor;
    }

    swapColors();
}

changeColor();
~~~

第一步，首先是全局上下文入栈。全局上下文入栈之后，其中的可执行代码开始执行，直到遇到了`changeColor()`，这一句激活函数`changeColor`创建它自己的执行上下文，

因此第二步就是`changeColor`的执行上下文入栈。`changeColor`的上下文入栈之后，控制器开始执行其中的可执行代码，遇到`swapColors()`之后又激活了一个执行上下文。

因此第三步是`swapColors`的执行上下文入栈。

在`swapColors`的可执行代码中，再没有遇到其他能生成执行上下文的情况，因此这段代码顺利执行完毕，`swapColors`的上下文从栈中弹出。

`swapColors`的执行上下文弹出之后，继续执行`changeColor`的可执行代码，也没有再遇到其他执行上下文，顺利执行完毕之后弹出。这样，`ECStack`中就只身下全局上下文了。

全局上下文在浏览器窗口关闭后出栈。

![](images\context.png)

执行上下文总结：

- 单线程
- 同步执行，只有栈顶的上下文处于执行中，其他上下文需要等待
- 全局上下文只有唯一的一个，它在浏览器关闭时出栈
- 函数的执行上下文的个数没有限制
- 每次某个函数被调用，就会有个新的执行上下文为其创建，即使是调用的自身函数，也是如此。

### 变量对象（VO）

当调用一个函数时（激活），一个新的执行上下文就会被创建。而一个执行上下文的生命周期可以分为两个阶段。

- **创建阶段**
  在这个阶段中，执行上下文会分别创建变量对象，建立作用域链，以及确定this的指向。
- **代码执行阶段**
  创建完成之后，就会开始执行代码，这个时候，会完成变量赋值，函数引用，以及执行其他代码。

![](images\变量对象p1.png)

##### 变量对象（Variable Object）

变量对象的创建，依次经历了以下几个过程。

1. 建立arguments对象。检查当前上下文中的参数，建立该对象下的属性与属性值。
2. 检查当前上下文的函数声明，也就是使用function关键字声明的函数。在变量对象中以函数名建立一个属性，属性值为指向该函数所在内存地址的引用。如果函数名的属性已经存在，那么该属性将会被新的引用所覆盖。
3. 检查当前上下文中的变量声明，每找到一个变量声明，就在变量对象中以变量名建立一个属性，属性值为undefined。如果该变量名的属性已经存在，为了防止同名的函数被修改为undefined，则会直接跳过，原属性值不会被修改。

![](images\变量对象创建过程.png)



在上面的规则中我们看出，`function`声明会比`var`声明优先级更高一点。(函数表达式是由`var`申明的，所以变量被申明为`undefined`)

例1：

~~~javascript
function test(){
    console.log(a);//undefined
    console.log(foo());//2
    var a = 1;
    function foo(){
        return 2;
    }
}
test();
~~~

全局作用域中运行`test()`时，test()的执行上下文开始创建。为了便于理解，我们用如下的形式来表示

~~~javascript
// 创建过程
testEC = {
    // 变量对象
    VO: {},
    scopeChain: {}
}

// 因为本文暂时不详细解释作用域链，所以把变量对象专门提出来说明

// VO 为 Variable Object的缩写，即变量对象
VO = {
    arguments: {...},  //注：在浏览器的展示中，函数的参数可能并不是放在arguments对象中，这里为了方便理解，我做了这样的处理
    foo: <foo reference>  // 表示foo的地址引用
    a: undefined
}
~~~

未进入执行阶段之前，变量对象中的属性都不能访问！但是进入执行阶段之后，变量对象转变为了活动对象，里面的属性都能被访问了，然后开始进行执行阶段的操作。

~~~javascript
// 执行阶段
VO ->  AO   // Active Object
AO = {
    arguments: {...},
    foo: <foo reference>,
    a: 1,
    this: Window
}
~~~

因此，上面的例子demo1，执行顺序就变成了这样

~~~javascript
function test() {
    function foo() {
        return 2;
    }
    var a;
    console.log(a);
    console.log(foo());
    a = 1;
}

test();
~~~

例2：

~~~javascript
function test1(){
    console.log(foo);//foo(){...}
    console.log(bar);//undefined
    var foo = "Hello";
    console.log(foo);//hello
    var bar = function (){
        return "world";
    }
    function foo(){
    	return "hello";
    }
}
test1();
~~~

~~~javascript
// 创建阶段
VO = {
    arguments: {...},
    foo: <foo reference>,
    bar: undefined
}
// 这里有一个需要注意的地方，因为var声明的变量当遇到同名的属性时，会跳过而不会覆盖
~~~

~~~javascript
// 执行阶段
VO -> AO
VO = {
    arguments: {...},
    foo: 'Hello',
    bar: <bar reference>,
    this: Window
}
~~~

##### 全局上下文的变量对象

以浏览器中为例，全局对象为window。
全局上下文有一个特殊的地方，它的变量对象，就是window对象。而这个特殊，在this指向上也同样适用，this也是指向window。

~~~javascript
// 以浏览器中为例，全局对象为window
// 全局上下文
windowEC = {
    VO: Window,
    scopeChain: {},
    this: Window
}
~~~

##### 用变量对象的创建过程解释变量提升

##### 变量对象和活动对象有什么区别

他们其实都是同一个对象，只是处于执行上下文的不同生命周期。变量对象在执行上下文的创建阶段，在未进入执行阶段之前，变量对象中的属性都不能访问！但是进入执行阶段之后，变量对象转变为了活动对象，里面的属性都能被访问了，然后开始进行执行阶段的操作。不过只有处于函数调用栈栈顶的执行上下文中的变量对象，才会变成活动对象。

### 闭包

**闭包指的是：能够访问另一个函数作用域的变量的函数**。清晰的讲：闭包就是一个函数，这个函数能够访问其他函数的作用域中的变量。

~~~javascript
function outer() {
    var  a = "变量";
    var inner = function () {
        console.log(a);
    }
    return inner;// inner 就是一个闭包函数，因为他能够访问到outer函数的作用域
}
~~~

**为什么闭包函数能够访问其他函数的作用域** ?

从堆栈的角度看待`js`函数：
　　基本变量的值一般都是存在栈内存中，而对象类型的变量的值存储在堆内存中，栈内存存储对应空间地址。基本的数据类型: Number 、Boolean、Undefined、String、Null。

![](images\闭包demo1.png)

当我们执行 b={m:30}时，堆内存就有新的对象{m：30}，栈内存的b指向新的空间地址( 指向{m：30} )，而堆内存中原来的{m：20}就会被程序引擎垃圾回收掉，节约内存空间。

`js`函数也是对象，它也是在堆与栈内存中存储的，我们来看一下转化：

![](images\闭包demo2.png)

**
栈是一种先进后出的数据结构：
1 在执行fn前，此时我们在全局执行环境(浏览器就是window作用域)，全局作用域里有个变量a；
2 进入fn，此时栈内存就会push一个fn的执行环境，这个环境里有变量b和函数对象fn1，这里可以访问自身执行环境和全局执行环境所定义的变量
3 进入fn1，此时栈内存就会push 一个fn1的执行环境，这里面没有定义其他变量，但是我们可以访问到fn和全局执行环境里面的变量，因为程序在访问变量时，是向底层栈一个个找，如果找到全局执行环境里都没有对应变量，则程序抛出`underfined`的错误。

4 随着fn1()执行完毕，fn1的执行环境被杯销毁，接着执行完fn()，fn的执行环境也会被销毁，只剩全局的执行环境下，现在没有b变量，和fn1函数对象了，只有a 和 fn(函数声明作用域是window下)
**

在函数内访问某个变量是根据函数作用域链来判断变量是否存在的，而函数作用域链是程序根据函数所在的执行环境栈来初始化的，所以上面的例子，我们在fn1里面打印变量b，根据fn1的作用域链的找到对应fn执行环境下的变量b。所以**当程序在调用某个函数时，做了一下的工作：准备执行环境，初始函数作用域链和arguments参数对象**

执行demo1：

~~~javascript
var inner = outer();//获取闭包函数
inner();
~~~

当程序执行完var inner = outer()，其实outer的执行环境并没有被销毁，因为他里面的变量a仍然被被inner的函数作用域链所引用，当程序执行完inner(), 这时候，inner和outer的执行环境才会被销毁调；《JavaScript高级编程》书中建议：由于闭包会携带包含它的函数的作用域，因为会比其他函数占用更多内容，过度使用闭包，会导致内存占用过多。

#### 闭包的坑

##### 引用的变量可能发生变化

~~~javascript
function outer() {
    var result = [];
    for (var i = 0; i < 10; i++) {
    	result[i] = function () {
        	console.log(i);
    	}
	}
	return result;
}
var inner = outer();
for (var i = 0; i < inner.length; i++) {
    inner[i]();//输出10个10
}
~~~

因为每个闭包函数访问变量`i`是outer执行环境下的变量`i`，随着循环的结束，`i`已经变成10了

**如何解决：**

~~~javascript
function outer() {
    var result = [];
    for (var i = 0; i < 10; i++) {
        result[i] = f(i);
    }
    return result;
}
function f(num) {
    return function () {
        console.log(num);
    }
}
var inner = outer();
for (var i = 0; i < inner.length; i++) {
    inner[i]();
}
~~~

相当于：

~~~javascript
function outer() {
    var result = [];
    for (var i = 0; i < 10; i++) {
        result[i] = function f(num) {
            return function () {
                console.log(num);// // 此时访问的num，是上层函数执行环境的num，数组有10个函数对象，每个对象的执行环境下的number都不一样
            }
        }(i);
    }
    return result;
}

var inner = outer();
for (var i = 0; i < inner.length; i++) {
    inner[i]();
}
~~~

在这个版本中，我们没有直接把闭包赋值给数组，而是定义了一个匿名函数，并将立即执行该匿名函数的结果赋给数组。这里的匿名函数有一个参数 num，也就是最终的函数要返回的值。在调用每个匿名函数时，我 们传入了变量 i。由于函数参数是按值传递的，所以就会将变量 i 的当前值复制给参数 num。而在这个匿名函数内部，又创建并返回了一个访问 num 的闭包。这样一来，result 数组中的每个函数都有自己num 变量的一个副本，因此就可以返回各自不同的数值了。

##### this指向问题

~~~javascript
var name = "window";
var obj = {
    name : "object",
    getName : function () {
        return function(){
            console.log(this.name);
        }
    }
}
obj.getName()();//window
~~~

每个函数在被调用时都会自动取得两个特殊变量：this 和 arguments。内部函数在搜索这两个变量时，只会搜索到其活动对象为止，因此永远不可能直接访问外部函数中的这两个变量。不过，把外部作用域中的 this 对象保存在一个闭包能够访问到的变量里，就可以让闭包访问该对象了.

~~~javascript
var name = "window";
var obj = {
    name : "object",
    getName : function () {
        var that = this;
        return function(){
            console.log(that.name);
        }
    }
}
obj.getName()();//object
~~~

##### 内存泄露问题

~~~javascript
function showId() {
    var el = document.getElementById("app");
    el.onclick = function () {
        console.log(el.id);// 这样会导致闭包引用外层的el，当执行完showId后，el无法释放
    };
}
showId();//app
~~~

**如何解决：**

~~~javascript
function showId() {
    var el = document.getElementById("app");
    var id = el.id;
    el.onclick = function () {
        console.log(id);// 这样会导致闭包引用外层的el，当执行完showId后，el无法释放
    };
    el = null;
}
showId();//app
~~~

#### 闭包应用

##### 用闭包模仿块级作用域

~~~javascript
for (var i = 0; i < 10 ; i++) {
    console.log(i);
}
console.log(i);
~~~

这个函数中定义了一个 for 循环，而变量 `i` 的初始值被设置为 0。在 Java、C++等语言中，变量` i` 只会在 for 循环的语句块中有定义，循环一旦结束，变量` i `就会被销毁。可是在` JavaScrip `中，变量` i `是定义在 `window`的活动对象中的，因此从它有定义开始，就可以在函数内部随处访问它。

**匿名函数可以用来模仿块级作用域**并避免这个问题。

~~~javascript
(function(){ 
 //这里是块级作用域
})();
~~~

~~~javascript
var b = function () {
    for (var j = 0; j < 10 ; j++) {
        console.log(j);
    }
}
b();
console.log(j);//j is not defined,j会被销毁
~~~

相当于：

~~~javascript
(function () {
    for (var j = 0; j < 10 ; j++) {
        console.log(j);
    }
})();
console.log(j);

~~~

### this指向

#### call/apply的作用

##### 修改this指向

~~~javascript
function fn(num1, num2) {
    console.log(this.a + num1 + num2);
}
var obj = {
    a: 20
}

fn.call(obj, 100, 10); // 130
fn.apply(obj, [20, 10]); // 50
~~~

##### 将类数组对象转换为数组

~~~javascript
function exam(a, b, c, d, e) {

    // 先看看函数的自带属性 arguments 什么是样子的
    console.log(arguments);

    // 使用call/apply将arguments转换为数组, 返回结果为数组，arguments自身不会改变
    var arg = [].slice.call(arguments);

    console.log(arg);
}

exam(2, 8, 9, 10, 3);
~~~

##### 灵活修改指向

~~~javascript
var foo = {
    name: 'joker',
    showName: function() {
      console.log(this.name);
    }
}
var bar = {
    name: 'rose'
}
foo.showName.call(bar);
~~~

##### 实现继承

~~~javascript
// 定义父级的构造函数
var Person = function(name, age) {
    this.name = name;
    this.age  = age;
    this.gender = ['man', 'woman'];
}

// 定义子类的构造函数
var Student = function(name, age, high) {

    // use call
    Person.call(this, name, age);
    this.high = high;
}
Student.prototype.message = function() {
    console.log('name:'+this.name+', age:'+this.age+', high:'+this.high+', gender:'+this.gender[0]+';');
}

new Student('xiaom', 12, '150cm').message();
~~~

##### 在向其他执行上下文的传递中，确保this的指向保持不变

例如：

~~~javascript
var obj = {
    a: 20,
    getA: function() {
        setTimeout(function() {
            console.log(this.a)
        }, 1000)
    }
}

obj.getA();
~~~

我们期待的是getA被obj调用时，this指向obj，但是由于匿名函数的存在导致了this指向的丢失，在这个匿名函数中this指向了全局，因此我们需要想一些办法找回正确的this指向。

* 方法一：就是使用一个变量，将this的引用保存起来

~~~javascript
var obj = {
    a: 20,
    getA: function() {
        var self = this;
        setTimeout(function() {
            console.log(self.a)
        }, 1000)
    }
}
~~~

* 方法二：借助闭包与apply方法，封装一个bind方法

~~~javascript
function bind(fn, obj) {
    return function() {
        return fn.apply(obj, arguments);
    }
}

var obj = {
    a: 20,
    getA: function() {
        setTimeout(bind(function() {
            console.log(this.a)
        }, this), 1000)
    }
}

obj.getA();
~~~

* 方法三：使用ES5中已经自带的bind方法

~~~javascript
var obj = {
    a: 20,
    getA: function() {
        setTimeout(function() {
            console.log(this.a)
        }.bind(this), 1000)
    }
}
~~~





