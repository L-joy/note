---
title: JavaScript高级教程5
date: 2019-10-09 17:22:34
tags: [前端]
---

## 第五章 引用类型

引用类型的值（对象）是引用类型的一个实例：

~~~javascript
var person = {
   name : "Nick",
   age : 18
}
~~~

访问对象的属性的方法

* 点表示法   `person.name`

* 方括号语法  `person["name"]`

  区别：方括号语法的主要优点是

  * 可以通过变量来访问属性

    ~~~javascript
    var personName = "name"
    alert(person[personName])//Nick
    ~~~

  * 如果属性名中包含会导致语法错误的字符，或者属性名使用的是关键字或保留字，也可以使用方括 

    号表示法

    ~~~javascript
    person["first name"] = "Nicholas";
    ~~~

    **除非必须使用变量来访问属性，否则我们建议使用点表示法。**

 

## 第六章 对象

- ****

  **isPrototypeOf()`方法来确定对象之间是否存在指向关系。如果[[Prototype]]指向调用 isPrototypeOf()方法的对象（Person.prototype），那么这个方法就返回 true，**

- **`Object.getPrototypeOf()`方法返回[[Prototype]]的值，Object.getPrototypeOf()返回的对象实际就是这个对象的原型**

- **`hasOwnProperty()`方法，判断什么时候访问的是实例属性，什么时候访问的是原型属性**

- **`Object.getOwnPropertyNames(Person.prototype)`获取实例对象的所有实例属性**

  

-  **`Object.defineProperty()`方法：修改属性的默认特性（[[Configurable]]、[[Enumerable]]等）**

- **`Object.defineProperties()`方法：定义多个属性**

- **`Object.getOwnPropertyDescriptor()`方法,可以取得给定属性的描述符。**

1. 字面量对象

对于JS中的所有对象，都有一个内部属性`_proto_`,这个属性指向新建对象的原型，people对象字面量的原型是什么呢，它是`Object.prototype`。

![](images\personDemo.png)



当我们想执行`toString()`方法时，JS解释器做了什么呢？首先它会到`peope`对象里去找，即去找`people`里的自有属性，当找到了就去调用该方法，没找到，就到它`_proto_`所引用对象里去找，找到了，他就执行原型里的这个方法（子类属性覆盖父类属性）。所以我们说`people`对象继承了`object.prototype`里的方法，但是我更推荐大家这么说`Object.prototype`是所有对象`(people)`的原型。

若执行：

~~~javascript
people.toString = function(){
  console.log("people对象中的toString");
}
~~~

JS解释器会做什么呢，他还会到原型上找到`toString`属性然后赋值么。当然不行，这样做的话，所有继承了`Object.prototype`的对象的`toString`属性都会改变。因此赋值时JS解释器就直接看`people`对象里的自有属性有没有，如果有，改。如果没，就新建一个属性赋值。

2. 对象模板（函数）

![](images\personFunctionDemo.png)

### 对象属性特性

ECMAScript 中有两种属性：数据属性和访问器属性

#### 数据属性

数据属性有 4 个描述其行为的特性。 

- `[[Configurable]]`：表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。像前面例子中那样直接在对象上定义的属性，它们的这个特性默认值为 true。

- `[[Enumerable]]`：表示能否通过 for-in 循环返回属性。直接在对象上定义的属性，它们的这个特性默认值为 true。

- `[[Writable]]`：表示能否修改属性的值。像前面例子中那样直接在对象上定义的属性，它们的这个特性默认值为 true。 

- [[Value]]：包含这个属性的数据值。读取属性值的时候，从这个位置读；写入属性值的时候,把新值保存在这个位置。这个特性的默认值为 undefined。

  ~~~javascript
  var person = { 
    name: "Nicholas" 
  };
  ~~~

  此例中直接在对象上定义的属性的[[Configurable]]、[[Enumerable]]和[[Writable]]特性都被设置为true，而[[Value]]特性被设置为指定的值。

  要修改属性默认的特性，必须使用 ECMAScript 5 的 `Object.defineProperty()`方法，这个方法接收三个参数：属性所在的对象、属性的名字和一个描述符对象。描述符（descriptor）对象的属性必须是：configurable、enumerable、writable 和 value。设置其中的一或多个值，可以修改对应的特性值。

  例如：

  ~~~javascript
  var person={
      name:"Nick"
};
   Object.defineProperty(person,"name",{
      writable:false,//只读
      value:"Jackson"
  });
  console.log(person.name);//Jackson
  person.name = "Nerge";
  console.log(person.name);//Jackson
  ~~~
  

#### 访问器属性

访问器属性有如下 4 个特性。

* `[[Configurable]]`：表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为数据属性。对于直接在对象上定义的属性，这个特性的默认值为 true。 

* `[[Enumerable]`：表示能否通过 for-in 循环返回属性。对于直接在对象上定义的属性，这个特性的默认值为 true。 

* `[[Get]]`：在读取属性时调用的函数。默认值为 undefined。 

* `[[Set]]`：在写入属性时调用的函数。默认值为 undefined。 

  ~~~javascript
  var book = {
          _year:2019,
          edition:1
      };
      Object.defineProperty(book,"year",{
          get:function () {
              return this._year;
          },
          set:function (newValue) {
              if(newValue>2019){
                  this._year = newValue;
                  this.edition += newValue - 2019;
              }
          }
      });
      book.year = 2023;
      console.log(book.edition);//5
  ~~~

  `_year` 前面 的下划线是一种常用的记号，用于表示只能通过对象方法访问的属性。而访问器属性 year 则包含一个getter 函数和一个 setter 函数。getter 函数返回`_year `的值，setter 函数通过计算来确定正确的版本。因此， 把 year 属性修改为 2023 会导致`_year `变成 2023，而 edition 变为 5。这是使用访问器属性的常见方式，即设置一个属性的值会导致其他属性发生变化。

#### 定义多个属性

`Object.defineProperties()`方法。利用这个方法可以通过描述符一次定义多个属性。这个方法接收两个对象参数：第一个对象是要添加和修改其属性的对象，第二个对象的属性与第一个对象中要添加或修改的属性一一对应。例如：

```javascript
var book = {}; 
Object.defineProperties(book, { 
     _year: { 
         value: 2004 
     }, 
     edition: { 
         value: 1 
     }, 
     year: { 
         get: function(){ 
         	return this._year; 
         }, 
    	 set: function(newValue){ 
     			if (newValue > 2004) { 
     				this._year = newValue; 
    				this.edition += newValue - 2004; 
     			} 
     	  } 
     } 
});
```

#### 读取属性特性

`Object.getOwnPropertyDescriptor()`方法,可以取得给定属性的描述符。这个方法接收两个参数：属性所在的对象和要读取其描述符的属性名称。返回值是一个对象，如果是访问器属性，这个对象的属性有 configurable、enumerable、get 和 set；如果是数据属性，这个对象的属性有 configurable、enumerable、writable 和 value。

~~~javascript
 var books = {};
    Object.defineProperties(books,{
        _year:{
            value:2019
        },
        edition:{
            value:1
        },
        year:{
            get: function () {
                return this._year;
            },
            set: function (newValue) {
                if(newValue>2019){
                    this._year = newValue;
                    this.edition += newValue - 2019;
                }
            }
        }
    });
    var descriptor = Object.getOwnPropertyDescriptor(book,"_year");
    console.log(descriptor);
    //{value: 2023, writable: true, enumerable: true, configurable: true}
~~~



```
hasOwnProperty方法是用来检测给定的属性是否在当前对象的实例中。
```

### 原型及原型链

#### 原型的作用

* 数据共享,节省内存空间
* 实现继承

#### 原型

如果想要使用一些属性和方法,并且属性的值在每个对象中都是一样的,方法在每个对象中的操作也都是一样,那么,为了共享数据,节省内存空间,是可以把属性和方法通过原型的方式进行赋值

~~~javascript
    //构造函数
    function Person(name,age) {
      //属性
      this.name=name;
      this.age=age;
      //在构造函数中的方法
      this.eat=function () {
        console.log("吃好吃的");
      };
    }
    //添加共享的属性
    Person.prototype.sex="男";
    //添加共享的方法
    Person.prototype.sayHi=function () {
      console.log("您好啊,怎么这么帅,就是这么帅");
    };
    var per=new Person("小明",20);
    per.sayHi();
    console.dir(per);//实例对象的结构
    console.dir(Person);//构造函数的结构
~~~

![](images\原型.png)

实例对象的原型`__proto__`和构造函数的原型`prototype`指向是相同的

实例对象中的`__proto__`原型指向的是构造函数中的原型`prototype`

实例对象中`__proto__`是原型,浏览器使用的
构造函数中的`prototype`是原型,程序员使用的

#### 原型链

原型链：是一种关系,实例对象和原型对象之间的关系,关系是通过原型(`__proto__`)来联系的

![](images\原型及原型链.png)

~~~javascript
   function Person(age) {
     this.age=age;
     console.log(this);
   }
   Person.prototype.eat=function () {
     console.log(this);
     console.log("您吃了没,走着,吃点臭豆腐去");
   };
   var per=new Person(10);
   per.eat();
   console.log(per);
   //判断per的_proto_是否指向Person.prototype
   console.log(Person.prototype.isPrototypeOf(per));//true

   //确定 Object.getPrototypeOf()返回的对象实际就是这个对象的原型
   console.log(Object.getPrototypeOf(per));
   console.log(Object.getPrototypeOf(per) === Person.prototype);//true

    //判断访问的属性为来自实例对象还是原型对象
    console.log(per.name);//Jackson---此时这个属性来自原型(原型属性)
    console.log(per.hasOwnProperty("name"));//false
    per.name = "JacksonE";
    console.log(per.name);//JacksonE---此时这个属性来自实例(实例属性)
    console.log(per.hasOwnProperty("name"));//true

    //得到所有实例属性，无论它是否可枚举
    console.log(Object.getOwnPropertyNames(Person.prototype));
    //["constructor", "name", "eat"]
~~~

三个console.log输出相同，均为实例对象per。

构造函数中的`this`就是实例对象

原型对象中方法中的`this`就是实例对象

* **`isPrototypeOf()`方法来确定对象之间是否存在指向关系。如果[[Prototype]]指向调用 isPrototypeOf()方法的对象（Person.prototype），那么这个方法就返回 true，**

* **`Object.getPrototypeOf()`方法返回[[Prototype]]的值，Object.getPrototypeOf()返回的对象实际就是这个对象的原型**

* **`hasOwnProperty()`方法，判断什么时候访问的是实例属性，什么时候访问的是原型属性**
* **`Object.getOwnPropertyNames(Person.prototype)`获取实例对象的所有实例属性**

#### 原型指向是否可以改变

~~~javascript
   //人的构造函数
    function Person(age) {
      this.age=10;
    }
    //人的原型对象方法
    Person.prototype.eat=function () {
      console.log("人的吃");
    };
    //学生的构造函数
    function Student() {

    }
    Student.prototype.sayHi=function () {
      console.log("嗨,小苏你好帅哦");
    };
    //学生的原型,指向了一个人的实例对象
    Student.prototype=new Person(10);
    var stu=new Student();
	
    stu.eat();//人的吃
    stu.sayHi();//error
~~~

![](images\原型指向改变.png)

原型指向可以改变
实例对象的原型`__proto__`指向的是该对象所在的构造函数的原型对象
构造函数的原型对象(`prototype`)指向如果改变了,实例对象的原型(`__proto__`)指向也会发生改变

#### 原型最终指向

~~~javascript
    function Person() {

    }
    Person.prototype.eat=function () {
      console.log("吃东西");
    };

    var per=new Person();
    console.dir(per);
    console.dir(Person);
~~~



`per`实例对象的`__proto__`------->`Person.prototype`

`Person.prototype`的`__proto__`---->`Object.prototype`

`Object.prototype`的`__proto__`-------->null

![](images\原型最终指向.png)



#### 原型指向改变如何添加方法

如果原型指向改变了,那么就应该**在原型改变指向之后添加原型方法**

~~~javascript
   //人的构造函数
    function Person(age) {
      this.age=10;
    }
    //人的原型对象方法
    Person.prototype.eat=function () {
      console.log("人的吃");
    };
    //学生的构造函数
    function Student() {

    }
    //学生的原型,指向了一个人的实例对象
    Student.prototype=new Person(10);
    Student.prototype.sayHi=function () {
      console.log("嗨,小苏你好帅哦");
    };
    var stu=new Student();
	
    stu.eat();//人的吃
    stu.sayHi();//嗨,小苏你好帅哦
~~~

`sayHi()`方法添加到实例对象`new Person(10)`中。

#### 实例对象和原型对象中属性重名问题

~~~javascript
   function Person(age,sex) {
      this.age=age;
      this.sex=sex;
    }
    Person.prototype.sex="女";
    var per=new Person(10,"男");
    console.log(per.sex);//男
~~~

实例对象访问这个属性,应该先从实例对象中找,找到了就直接用，找不到就去指向的原型对象中找,找到了就使用,找不到就是`undefined`.

通过实例对象不能改变原型对象中的属性值

~~~javascript
per.sex="人";
~~~

改变的是实例对象中的sex属性。

改变原型对象中的属性：

~~~javascript
Person.prototype.sex="哦唛嘎的";
~~~

### 面向对象

**面向对象编程思想** : 根据需求,分析对象,找到对象有什么特征和行为,通过代码的方式来实现需求,要想实现这个需求,就要创建对象,要想创建对象,就应该显示有构造函数,然后通过构造函数来创建对象.,通过对象调用属性和方法来实现相应的功能及需求即可

首先JS不是一门面向对象的语言,**JS是一门基于对象的语言**,那么为什么学习`js`还要学习面向对象,因为面向对象的思想适合于人的想法,编程起来会更加的方便,及后期的维护....

面向对象的编程语言中有类(class)的概念(也是一种特殊的数据类型),但是JS不是面向对象的语言,所以,JS中没有类(class),但是**JS可以模拟面向对象的思想编程,JS中会通过构造函数来模拟类的概念(class)**

面向对象的特性 : 封装,继承,多态

* 封装

* 继承
* 多态

**封装**:就是包装
    一个值存储在一个变量中--封装
    一坨重复代码放在一个函数中--封装
    一系列的属性放在一个对象中--封装
     一些功能类似的函数(方法)放在一个对象中--封装
     好多相类似的对象放在一个`js`文件中---封装

**继承**: 首先继承是一种关系,类(class)与类之间的关系,JS中没有类,但是可以通过构造函数模拟类,然后通过原型来实现继承

继承也是为了数据共享,`js`中的继承也是为了实现数据共享

继承是一种关系 : 父类级别与类级别的关系

**多态**:一个对象有不同的行为,或者是同一个行为针对不同的对象,产生不同的结果,要想有多态,就要先有继承,`js`中可以模拟多态,但是不会去使用,也不会模拟

#### 继承

~~~javascript
//动物有名字,有体重,有吃东西的行为
//小狗有名字,有体重,有毛色, 有吃东西的行为,还有咬人的行为
//哈士奇名字,有体重,有毛色,性别, 有吃东西的行为,还有咬人的行为,逗主人开心的行为


//动物的构造函数
function Animal(name,weight) {
    this.name=name;
    this.weight=weight;
}
//动物的原型的方法
Animal.prototype.eat=function () {
    console.log("天天吃东西,就是吃");
};

//狗的构造函数
function Dog(color) {
    this.color=color;
}
Dog.prototype=new Animal("哮天犬","50kg");
Dog.prototype.bitePerson=function () {
    console.log("哼~汪汪~咬死你");
};

//哈士奇构造函数
function ErHa(sex) {
    this.sex=sex;
}
ErHa.prototype=new Dog("黑白色");
ErHa.prototype.playHost=function () {
    console.log("哈哈~要坏衣服,要坏桌子,拆家..嘎嘎...好玩,开心不,惊喜不,意外不");
};
var erHa=new ErHa("雄性");
console.log(erHa.name,erHa.weight,erHa.color);
erHa.eat();
erHa.bitePerson();
erHa.playHost();

~~~

![](images\继承.png)

#### 借用构造函数继承

~~~javascript
function Person(name,age,sex,weight) {
    this.name=name;
    this.age=age;
    this.sex=sex;
    this.weight=weight;
}
Person.prototype.sayHi=function () {
    console.log("您好");
};
function Student(score) {
    this.score=score;
}
//希望人的类别中的数据可以共享给学生---继承
Student.prototype=new Person("小明",10,"男","50kg");

var stu1=new Student("100");
console.log(stu1.name,stu1.age,stu1.sex,stu1.weight,stu1.score);
stu1.sayHi();

var stu2=new Student("120");
console.log(stu2.name,stu2.age,stu2.sex,stu2.weight,stu2.score);
stu2.sayHi();

var stu3=new Student("130");
console.log(stu3.name,stu3.age,stu3.sex,stu3.weight,stu3.score);
stu3.sayHi();
~~~

为了数据共享,改变原型指向,做到了继承---通过改变原型指向实现的继承

缺陷: 因为改变原型指向的同时实现继承,直接初始化了属性，继承过来的属性的值都是一样的了,所以,这就是问题。

输出结果：

![](images\借用构造函数demo1.png)

只能重新调用对象的属性进行重新赋值,

~~~javascript
stu2.name="张三";
stu2.age=20;
stu2.sex="女";
~~~

输出结果：

![](images\借用构造函数demo2.png)

**解决方案** : 继承的时候,不用改变原型的指向,直接调用父级的构造函数的方式来为属性赋值就可以了------借用构造函数:把要继承的父级的构造函数拿过来,使用一下就可以了

**借用构造函数 : 构造函数名字.call(当前对象,属性,属性,属性....); 即改变this指向**

~~~javascript
function Person(name, age, sex, weight) {
    this.name = name;
    this.age = age;
    this.sex = sex;
    this.weight = weight;
}
Person.prototype.sayHi = function () {
    console.log("您好");
};
function Student(name,age,sex,weight,score) {
    //借用构造函数
    Person.call(this,name,age,sex,weight);
    this.score = score;
}
var stu1 = new Student("小明",10,"男","10kg","100");
console.log(stu1.name, stu1.age, stu1.sex, stu1.weight, stu1.score);

var stu2 = new Student("小红",20,"女","20kg","120");
console.log(stu2.name, stu2.age, stu2.sex, stu2.weight, stu2.score);

var stu3 = new Student("小丽",30,"妖","30kg","130");
console.log(stu3.name, stu3.age, stu3.sex, stu3.weight, stu3.score);
~~~

输出结果：

![](images\借用构造函数demo3.png)

缺陷:父级类别中的方法不能继承

~~~javascript
stu1.sayHi();//报错
~~~

#### 组合继承

继承方式：

* 原型实现继承

* 借用构造函数实现继承

* 组合继承:原型继承+借用构造函数继承

  ~~~javascript
   Person.call(this,name,age,sex);
   Student.prototype=new Person();//不传值
  ~~~

  均使用。

~~~javascript
function Person(name,age,sex) {
    this.name=name;
    this.age=age;
    this.sex=sex;
}
Person.prototype.sayHi=function () {
    console.log("阿涅哈斯诶呦");
};
function Student(name,age,sex,score) {
    //借用构造函数:属性值重复的问题
    Person.call(this,name,age,sex);
    this.score=score;
}
//改变原型指向----继承
Student.prototype=new Person();//不传值
Student.prototype.eat=function () {
    console.log("吃东西");
};
var stu=new Student("小黑",20,"男","100分");
console.log(stu.name,stu.age,stu.sex,stu.score);//小黑 20 男 100分
stu.sayHi();//阿涅哈斯诶呦
stu.eat();//吃东西
var stu2=new Student("小黑黑",200,"男人","1010分");
console.log(stu2.name,stu2.age,stu2.sex,stu2.score);//小黑黑 200 男人 1010分
stu2.sayHi();//阿涅哈斯诶呦
stu2.eat();//吃东西
~~~

#### 拷贝继承

拷贝继承；把一个对象中的属性或者方法直接复制到另一个对象中

~~~javascript
function Person() {
}
Person.prototype.age=10;
Person.prototype.sex="男";
Person.prototype.height=100;
Person.prototype.play=function () {
    console.log("玩的好开心");
};
var obj2={};
//Person的构造中有原型prototype,prototype就是一个对象,那么里面,age,sex,height,play都是该对象中的属性或者方法

for(var key in Person.prototype){
    obj2[key]=Person.prototype[key];
}
console.dir(obj2);
obj2.play();//玩的好开心

~~~

#### 逆推继承寻找属性

~~~javascript
function F1(age) {
	this.age = age;
}
function F2(age) {
	this.age = age;
}
F2.prototype = new F1(10);
function F3(age) {
	this.age = age;
}
F3.prototype = new F2(20);

var f3 = new F3(30);
console.log(f3.age);//30
~~~

![](images\逆推继承.png)