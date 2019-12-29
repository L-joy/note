#### Set集合和Map集合

##### Set集合

Set集合是一种**没有重复元素**、有序列表，一般的我们不会像访问数组一样逐一的访问每个元素，通常的做法是**检测给定的值在某个集合中是否存在**；通过Set集合可以快速访问其中的数据，更有效的追踪各种离散值。

Set集合中**没有重复元素**，也就是说Set集合中每个元素都是唯一的，不会重复。

###### set集合的使用

* 创建Set

  ```javascript
  let set = new Set();
  ```

* 添加元素

  ```javascript
  set.add(5);
  set.add('6');
  ```

  也可以添加对象, 并且也不会被强制转换，如果添加的值重复，则直接忽略

* 删除元素

  ```javascript
  set.delete(5);
  set.clear();//删除所有元素
  ```

* 检测set中是否存在某个值

  ```javascript
  set.has(5);
  ```

* set长度

  ```javascript
  set.size
  ```

###### 利用set集合实现数组去重

~~~javascript
let arr = [1,2,2,3,4,4,6];
let set = new Set(arr);
console.log(Array.from(set));//[1, 2, 3, 4, 6]
console.log([...set]);////[1, 2, 3, 4, 6]
~~~

###### 实现并集、交集、差集

~~~javascript
let set1 = new Set([1,2,3]);
let set2 = new Set([2,3,4,5,6]);
let union = new Set([...set1,...set2]);
console.log([...union]);//[1, 2, 3, 4, 5, 6]
var intersect = new Set([...set1].filter(x => {
    return set2.has(x)
}));
console.log([...intersect]);//[2, 3]
var difference = new Set([...set1].filter(x => {
    return !set2.has(x)
}));
    console.log([...difference]);//[4, 5, 6]
~~~

###### set集合的`forEach()`

其回调函数接收三个参数：

* set集合中下一次索引位置

* 与第一个参数一样的值

* 被遍历的Set集合本身

  ```javascript
  let set = new Set([1,2,3]);
  set.forEach(function (val, key, ownerSet) {    
      console.log(val,key,ownerSet);
  })
  //1 1 Set(3) {1, 2, 3}
  //2 2 Set(3) {1, 2, 3}
  //3 3 Set(3) {1, 2, 3}
  ```

Set类型可以用来处理列表中的值，但是不适用于处理键值对这样的信息结构。Map集合来解决类似的问题

##### Map集合

Map数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值(包括对象)都可以当作键。也就是说，**Object结构提供了“字符串—值”的对应，Map结构提供了“值—值”的对应，是一种更完善的Hash结构实现**

Map类型是一种储存着许多键值对的有序列表，其中的键名和对应的值支持所有的数据类型。**键名的等价性判断是通过调用Object.is()方法实现的**，所以数字5与字符串"5"会被判定为两种类型，可以分别作为独立的两个键出现在程序中，**这一点与对象不一样，因为对象的属性名总会被强制转换成字符串类型**

**Map集合中将+0和-0视为相等，与Object.is()结果不同**

###### map集合的使用

* 创建Map集合

  ~~~javascript
  let map = new Map();
  ~~~

* 添加元素

  ~~~javascript
  map.set("name","Jack");
  map.set("age",18);
  ~~~

* 获取元素

  ~~~javascript
  console.log(map.get("name"));
  ~~~

  **如果调用get()方法时传入的键名在Map集合中不存在，则会返回undefined**

  在Map集合中，可以将对象作为键值

  ~~~javascript
  let key = {"name":"Jackson","age":18};
  map.set(key,12);
  console.log(map.get(key));//12
  ~~~

* 删除元素

  ```javascript
  map.delete("name");
  map.clear();//删除所有元素
  ```

- 检测Map中是否存在某个值

  ```javascript
  map.has("name");//false
  ```

- map长度

  ~~~javascript
  map.size
  ~~~

- 初始化集合

  ~~~javascript
  let map1 = new Map([["name","JACKSON"],["age",19]]);
  ~~~

###### 同名属性

**Map的键实际上是跟内存地址绑定的，只要内存地址不一样，就视为两个键**。这就解决了同名属性碰撞（clash）的问题，**扩展别人的库的时候，如果使用对象作为键名，就不用担心自己的属性与原作者的属性同名**

~~~javascript
let map2 = new Map();
let key1 = ['a'];
let key2 = ['a'];
    map2
        .set(key1,111)
        .set(key2,222);
console.log(map2.get(key1));//111
console.log(map2.get(key2));//222
~~~

###### 遍历

Map结构原生提供三个遍历器生成函数和一个遍历方法

```
keys()：返回键名的遍历器
values()：返回键值的遍历器
entries()：返回所有成员的遍历器
forEach()：遍历 Map 的所有成员
```

~~~javascript
    const map3 = new Map([
        ['F','no'],
        ['T','yes']
    ]);
    for (let key of map3.keys()){
        console.log(key);//F    T
    }
    for (let value of map3.values()){
        console.log(value);//no   yes
    }
    for (let item of map3.entries()){
        console.log(item);
    }
    for (let [key,value] of map3){
        console.log([key,value]);
    }
    map3.forEach((value,key,map) => {
        console.log(value,key);
    })
~~~

`forEach`方法还可以接受第二个参数，用来绑定`this`

~~~javascript
const reporter = {
  report: function(key, value) {
    console.log("Key: %s, Value: %s", key, value);
  }
};

map.forEach(function(value, key, map) {
  this.report(key, value);
}, reporter);
~~~

###### 将集合转为数组

* Map结构转为数组结构，比较快速的方法是使用扩展运算符（`...`）

~~~
 [...map3]
~~~

* `filter`方法，可以实现 Map 的遍历和过滤

  ~~~javascript
  const map4 = new Map();
  map4.set(1,'a');
  map4.set(2,'b');
  map4.set(3,'c');
  const map5 = new Map([...map4].filter(([k,v]) => k<3));
  console.log(map5);//Map(2) {1 => "a", 2 => "b"}
  const map6 = new Map([...map4].map(([k,v]) => [k*2,'_'+v]));
  console.log(map6);//Map(3) {2 => "_a", 4 => "_b", 6 => "_c"}
  ~~~

###### 二维数组去重

~~~javascript
    //方法一
    let arr = [[1],[2],[1,2],[2,1],[1],[3]];
    let map = new Map();
    arr.forEach(item => {
        item.sort((a,b) => a-b);
        map.set(item.join(),item);
    });
    console.log([...map.values()]);
~~~

~~~
   //方法二
   let arr = [[1],[2],[1,2],[2,1],[1],[3]];
    let obj = {};
    arr.forEach(item => {
        item.sort((a,b) => a-b);
        obj[item] = item;
    });
    console.log(Object.values(obj));
~~~







##### `WeakMap`集合

`WeakSet`是引用Set集合，相对地，`WeakMap`是弱引用Map集合，也用于存储对象的弱引用

**`WeakMap`集合中的键名必须是一个对象，如果使用非对象键名会报错**；集合中保存的是这些对象的弱引用，如果在弱引用之外不存在其他的强引用，引擎的垃圾回收机制会自动回收这个对象，同时也会移除`WeakMap`集合中的键值对。但是只有集合的键名遵从这个规则，**键名对应的值如果是一个对象，则保存的是对象的强引用，不会触发垃圾回收机制**

`WeakMap`集合最大的用途是保存Web页面中的DOM元素，例如，一些为Web页面打造的JS库，会通过自定义的对象保存每一个引用的DOM元素

###### `weakMap`集合的使用

* 创建

  ~~~javascript
  let map = new WeakMap();
  ~~~

* 添加

  ~~~javascript
  let element = document.querySelector(".element");
  map.set(element,"div");
  console.log(map.get(element));//div
  ~~~

  **列表的键名必须是非null类型的对象，键名对应的值则可以是任意类型**。

* 删除

  ~~~javascript
  element.parentNode.removeChild(element);
  element = null;//该WeakMap在此处为空
  ~~~

  在这个示例中储存了一个键值对，键名element是一个DOM元素，其对应的值是一个字符串，将DOM元素传入get()方法即可获取之前存过的值。如果随后从`document`对象中移除DOM元素并将引用这个元素的变量设置为null，那么`WeakMap`集合中的数据也会被同步清除

**`WeakMap`集合也不支持size属性，从而无法验证集合是否为空**

* 初始化方法

  ~~~javascript
  let key1 = {},
      key2 = {},
      map = new WeakMap([[key1, "Hello"], [key2, 42]]);
  ~~~

  **如果给`WeakMap`构造函数传入的诸多键值对中含有非对象的键，会导致程序抛出错误**

  `has()`方法可以检测给定的键在集合中是否存在；

  `delete()`方法可移除指定的键值对。

  不支持`clear()`方法