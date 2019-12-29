#### Promise对象

promise是异步编程的一种解决方法。所谓promise，简单说是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果，有了Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。

**promise对象的特点**

1. 对象的状态不受外界影响，promise对象代表一个异步操作，有三种状态，pending（进行中）、fulfilled（已成功）、rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态，这也是promise这个名字的由来“承诺”；

2. promise对象的状态改变，只有两种可能：从pending变为fulfilled，从pending变为rejected。这时就称为resolved（已定型）。如果改变已经发生了，你再对promise对象添加回调函数，也会立即得到这个结果，这与事件（event）完全不同，事件的特点是：如果你错过了它，再去监听是得不到结果的。

**promise的用法**

promise是一个构造函数，这个构造函数里有两个参数，分别是：resolve（成功之后回调的函数），reject（失败之后回调的函数）

~~~javascript
var promise = new Promise(function(resolve,reject){
     //异步函数
     if(异步函数成功){
        resolve(value)
     }else{
        reject(error)
     }
})
~~~

promise实例生成之后，可以用then方法分别指定resolved和rejected的回调函数

~~~javascript
promise.then(
   function(value){
      //成功时的输出
   }
   function(error){
      //失败时的输出
   }
)
~~~

**promise对象实现ajax操作的例子**

![1567775755468](C:\Users\whichone\AppData\Roaming\Typora\typora-user-images\1567775755468.png)

**vue中Promise对象用法**

~~~javascript
Promise.all([
	//需要异步一起执行的方法
]).then(res=>{
	//res里面存放的是数组，上面有多少个方法就有多少个index，每个index是上面对应的方法的返回值
})
~~~

