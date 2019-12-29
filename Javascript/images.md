#### 栈

栈是一种遵从**后进先出**原则的有序集合。新添加的或待删除的元素都保存在栈的末尾，称作栈顶，另一端就叫栈底。

**1.创建栈**

我们将创建一个类来表示栈，选择数组来保存栈里的元素,然后为栈声明如下方法： 
* push(element(s))：添加一个（或几个）新元素到栈顶； 

* pop()：移除栈顶的元素，同时返回被移除的元素； 

* peek()：返回栈顶的元素，不对栈做任何修改； 

* isEmpty()：如果栈里没有任何元素就返回true，否则返回false； 

* clear()：移除栈里的所有元素； 

* size()：返回栈里的元素个数。 

  ~~~javascript
  function Stack() {
      let items = [];
      //push方法
      this.push = function (element) {
      	items.push(element)
      }
      //pop方法
      this.pop = function () {
      	return items.pop()
      }
      //peek方法---返回栈顶的元素，不对栈做任何修改；
      this.peek = function (element) {
      	return items[items.length-1]
      }
      //isEmpty方法
      this.isEmpty = function (element) {
      	return (items.length === 0)
      }
      this.clear = function () {
      	items = []
      }
      this.size = function () {
      	return items.length
      }
  }
  //使用栈
  let stack = new Stack();
  console.log(stack.isEmpty())//true
  stack.push(2)
  stack.push(5)
  console.log(stack.size())//2
  console.log(stack.peek())//5
  console.log(stack.pop())//5
  console.log(stack.peek())//2
  stack.clear()
  console.log(stack.isEmpty())//true
  ~~~

  **2.用栈实现进制换算**

~~~javascript
function baseConverter(number) {
    let stack = new Stack()
    while(number){
        stack.push(number%2)
        number = Math.floor(number/2)
    }
    let result = ''
    while(!stack.isEmpty()){
        result = result+stack.pop()
    }
    return result
}
console.log(baseConverter(100))//1100100
~~~

**3.用栈判断回文字符串**

~~~javascript
//用栈判断字符串是否是回文字符串---“回文串”是一个正读和反读都一样的字符串，比如“level”或者“noon”等等就是回文串
function isPalindrome(word) {
    let stack = new Stack()
    let len = word.length
    for(let i = 0; i<len ; i++){
    	stack.push(word[i])
    }
    let newword = ''
    while(stack.size() !== 0){
    	newword = newword + stack.pop()
    }
    if(newword === word){
    	return true
    }else{
    	return false
    }
}
console.log(isPalindrome("level"))//true
console.log(isPalindrome("levellevell"))//false
~~~

**4.用栈实现递归**

~~~javascript
 //用栈实现递归-----阶乘
 //普通方法
 function factorial(n) {
     if(n === 0){
     	return 1;
     }else{
     	return n*factorial(n-1)
 	 }
 }
 console.log(factorial(4))//24
 //用栈
 function stackfactorial(n) {
 	let stack = new Stack()
 	while(n){
 		stack.push(n--)
 	}
 	let res = 1
 	while(stack.size() !== 0){
 		res = res*stack.pop()
 	}
 	return res
 }
 console.log(stackfactorial(4))//24
~~~

#### 队列

队列是遵循**先进先出**原则的一组有序的项，队列在尾部添加新元素，并从顶部移除元素。

**1.创建队列**

创建一个类来表示队列，使用数组用于储存队列中的元素，然后为队列声明如下方法：（与栈结构非常相似） 
* enquene(element(s))：向队列尾部添加一个（或多个）新的项； 

* dequene()：移除队列的第一（即排在队列最前面的）项，并返回被移除的元素； 

* front()：返回队列中的第一个元素，队列没变动； 

* isEmpty()：如果队列中不包含任何元素，返回true，否则返回false； 

* size()：返回队列包含的元素个数。

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
          return items.length === 0
      }
      this.size = function () {
          return items.length
      }
  }
  //使用队列
  let quene = new Quene()
  console.log(quene.isEmpty())//true
  quene.enquene(2)
  quene.enquene(5)
  console.log(quene.size())//2
  console.log(quene.dequene())//2
  console.log(quene.size())//1
  quene.clear()
  console.log(quene.isEmpty())//true
  ~~~

**2.队列的改进版**

2.1 优先队列（额外创建一个特殊元素，他包含了要添加到队列的元素及其优先级）
元素的添加和移除是基于优先级的,对于优先级相同的元素，依旧遵循先进先出的原则

~~~javascript
 function PriorityQuene() {
     Quene.call(this) //继承父级构造函数
     function QueneElement(element, priority) {
         this.element = element
         this.priority = priority
     }
     console.log(this)
     this.enquene = function (element, priority) {
     	let queneElement = new QueneElement(element, priority)
     	if (this.isEmpty()) {
     	this.items.push(queneElement)
    	 } else {
     		let flag = false
     		for (let i = 0; i < this.items.length; i++) {
     			if (queneElement.priority < this.items[i].priority) {
     				this.items.splice(i, 0, queneElement) 
     				//将元素添加到数组索引为i的位置
    				 flag = true
     				 break
     			}

    		}
            if (!flag) { //说明此元素优先级最高
                this.items.push(queneElement)
            }
        }
    }
 }
    PriorityQuene.prototype = Object.create(Quene.prototype) //继承父级原型
    PriorityQuene.prototype.constructer = PriorityQuene

    let priorityQuene = new PriorityQuene()
    priorityQuene.enquene("1", 1)
    priorityQuene.enquene("2", 2)
    priorityQuene.enquene("3", 3)
    priorityQuene.enquene("4", 5)
    console.log(priorityQuene.size())
    //priorityQuene.dequene()
    priorityQuene.enquene("5",4)
~~~

![1568798283036](C:\Users\whichone\AppData\Roaming\Typora\typora-user-images\1568798283036.png)

2.2  循环队列（击鼓传花游戏）

孩子们围成一个圈，把花尽快传给旁边的人，某一刻传花停止，花在谁手里，谁就淘汰，知道最后只剩一个孩子,返回这个孩子

~~~javascript
 function hotPotato(nameList, num) {
     let quene = new Quene()
     for (let i = 0; i < nameList.length; i++) {
     	quene.enquene(nameList[i])
     }
     while (quene.size() > 1) {
     	for (let i = 0; i < num; i++) {
     		quene.enquene(quene.dequene())
     	}
     	quene.dequene()
     }
     return quene.dequene()
 }
 let name = ["小明","小李","小花","小白","小小"]
 let winner = hotPotato(name,7)
 console.log(winner)//小明
~~~

