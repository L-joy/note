#### 组合继承

组合继承：原型继承+借用构造函数继承

~~~javascript
function Quene() {
     let items = []
     this.enquene = function (element) {
     	items.push(element)
     }
     this.dequene = function () {
     	//删除数组中第一项
     	return items.shift()
     }
     this.front = function () {
     	return items[0]
     }
     this.isEmpty = function () {
     	console.log('234')
     }
     this.clear = function () {
     	items = []
     }
     this.size = function () {
     	return items.length
     }
}
Quene.prototype.test = function () {
	console.log("Quenetest")
}
~~~

**构造函数继承**

~~~javascript
function PriorityQuene() {
    Quene.call(this) //只针对构造函数本身的继承，还需继承原型
    // 修改父类构造函数内的方法或属性，直接在子类的构造函数中覆盖即可
    this.isEmpty = function () {
        console.log('123')
    }
}
~~~

**原型继承方法**

1. 直接赋值

   ~~~javascript
   PriorityQuene.prototype = Quene.prototype
   ~~~

   缺点：引用类型，在操作对象原型时候，直接改变堆内存中对象的方法

   ![1568796398629](C:\Users\whichone\AppData\Roaming\Typora\typora-user-images\1568796398629.png)

2. 创建对象

   ~~~javascript
   PriorityQuene.prototype = new Quene()
   ~~~

   缺点：这种继承借助原型并基于已有的对象创建新对象，同时还不必因此创建自定义类型，但是构造函数两次继承，不是很好

3. 创建新对象

   ~~~javascript
   PriorityQuene.prototype = Object.create(Quene.prototype)
   PriorityQuene.prototype.constructor = PriorityQuene
   //修改父类原型中的方法需要在继承父类原型的基础上进行方法重写
   PriorityQuene.prototype.test = function (){
       console.log("PriorityQuenetest")
   }
   ~~~

   使用`Quene`原型对象及其属性去创建一个新的对象，并将这个对象的`constructor`指向`PriorityQuene`函数本身，不存在重复继承的问题

4. 深拷贝

   ~~~javascript
   for (var i in Quene.prototype){
   	PriorityQuene.prototype[i] = Quene.prototype[i]
   }
   ~~~

   将Quene的原型链遍历给PriorityQuene对象，实现原型的深度拷贝，双方互不影响

   

**修改父类中函数的方法**

1. 修改父类构造函数内的方法或属性，直接在子类的构造函数中覆盖即可（构造函数继承例中）
2. 修改父类原型中的方法需要在继承父类原型的基础上进行方法重写（原型继承方法三例中）