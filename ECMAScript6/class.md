传统的`javascript`中只有对象，没有类的概念。它是基于原型的面向对象语言。原型对象特点就是将自身的属性共享给新对象。如果要生成一个对象实例，需要先定义一个构造函数，然后通过new操作符来完成。构造函数示例：

~~~javascript
function Person(name,age) {
        this.name = name;
        this.age = age;
    }
    Person.prototype.say = function () {
        return "My name is "+this.name+","+this.age+" years old";
    };
    var person = new Person("Jackson",18);
    console.log(person.say());
~~~

**构造函数生成实例的执行过程：**

```bash
1.当使用了构造函数，并且new 构造函数(),后台会隐式执行new Object()创建对象;
2.将构造函数的作用域给新对象，（即new Object()创建出的对象），而函数体内的this就代表new Object()出来的对象。
3.执行构造函数的代码。
4.返回新对象（后台直接返回）;
```

ES6引入了Class（类）这个概念，通过class关键字可以定义类。该关键字的出现使得其在对象写法上更加清晰，更像是一种面向对象的语言。如果将之前的代码改为ES6的写法就会是这个样子：

~~~javascript
class Person{//定义一个名为Person的类
        constructor(name,age){//constructor是一个构造方法，用来接收参数
            this.name = name;
            this.age = age;
        }
        say(){//这是类的一个方法，注意千万不要加function
            return "My name is "+this.name+","+this.age+" years old";
        }
    }
    var person = new Person("Jackson",19);
    console.log(person.say());
~~~

**注意**

```bash
1.在类中声明方法的时候，千万不要给该方法加上function关键字
2.方法之间不要用逗号分隔，否则会报错
```

由下面代码可以看出类实质上就是一个函数。类自身指向的就是构造函数。所以可以认为ES6中的类其实就是构造函数的另外一种写法！

~~~bash
console.log(typeof Person);//function
console.log(Person === Person.prototype.constructor);//true
~~~

以下代码说明构造函数的prototype属性，在ES6的类中依然存在着。

`console.log(Person.prototype);//输出的结果是一个对象`

实际上类的所有方法都定义在类的prototype属性上。代码证明下：

~~~javascript
Person.prototype.say=function(){//定义与类中相同名字的方法。成功实现了覆盖！
    return "我是来证明的，你叫" + this.name+"今年"+this.age+"岁了";
}
var obj=new Person("laotie",88);
console.log(obj.say());//我是来证明的，你叫laotie今年88岁了
~~~

当然也可以通过prototype属性对类添加方法。如下：

~~~javascript
Person.prototype.add = function () {
        console.log(111)
    };
    person.add();
~~~

还可以通过`Object.assign`方法来为对象动态增加方法:

~~~javascript
Object.assign(Person.prototype,{
        getName:function () {
            return this.name;
        },
        getAge:function () {
            return this.age;
        }
    });
    console.log(person.getName());
    console.log(person.getAge());
~~~

constructor方法是类的构造函数的默认方法，通过new命令生成对象实例时，自动调用该方法。

constructor方法如果没有显式定义，会隐式生成一个constructor方法。所以即使你没有添加构造函数，构造函数也是存在的。constructor方法默认返回实例对象this，但是也可以指定constructor方法返回一个全新的对象，让返回的实例对象不是该类的实例。

~~~javascript
class Student{
        constructor(){
            return new Person('YY',20);
        }
    }
    var student = new Student();
    console.log(student.name);
~~~

constructor中定义的属性可以称为实例属性（即定义在this对象上），constructor外声明的属性都是定义在原型上的，可以称为原型属性（即定义在class上)。

`hasOwnProperty()`函数用于判断属性是否是实例属性。其结果是一个布尔值， true说明是实例属性，false说明不是实例属性。

`in`操作符会在通过对象能够访问给定属性时返回true,无论该属性存在于实例中还是原型中。

~~~javascript
console.log(person.hasOwnProperty("name"));//true
console.log(person.hasOwnProperty("say"));//false
console.log("age" in person);//true
console.log("age" in student);//true
console.log("say" in person);//true
~~~

**class不存在变量提升**，所以需要先定义再使用。因为ES6不会把类的声明提升到代码头部，但是ES5就不一样,**ES5存在变量提升**,可以先使用，然后再定义。