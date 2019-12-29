#### 数组

##### 创建和初始化数组的方法

~~~javascript
let array = **new** *Array*()
let array = []
~~~

##### 数组的方法

##### **push,pop(后进后出)**

~~~javascript
let arr = ['2','3','4','1','56','34','0']

//push,pop(后进后出)
arr.push('24') //["2", "3", "4", "1", "56", "34", "0", "24"]
arr.pop() //["2", "3", "4", "1", "56", "34"]
~~~

##### **unshift,shift(前进前出)**

~~~javascript
let arr = ['2','3','4','1','56','34','0']
//unshift,shift(前进前出)
arr.unshift('12') //["12", "2", "3", "4", "1", "56", "34", "0"]
arr.shift() // ["3", "4", "1", "56", "34", "0"]
~~~

##### **splice()**

splice(“起始索引”,“删除个数”,"添加元素")------删除/添加

~~~javascript
arr.splice(1,2) //从索引为1的位置开始删除两个
//["2", "1", "56", "34", "0"]
arr.splice(1,0,'8')
console.log(arr)//["2", "8", "3", "4", "1", "56", "2", "34", "0", "2"]
~~~

##### **slice() **

可从已有的数组中返回选定的元素

~~~javascript
arr = arr.slice(1, 5) //返回从索引1到索引5（不包括索引5）对应的元素组成的新数组
//["3", "4", "1", "56"]
~~~

##### **lastIndexOf(key,index)**

返回与给定参数相等的数组元素索引的最大值

~~~javascript
let arr = ['2','3','4','1','56','2','34','0','2']
let l = arr.lastIndexOf('2',7)//从索引为8的位置开始从后往前搜索'2'
console.log(l)//5
~~~

##### **map(function)**

方法对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组

~~~javascript
let arr = ['2','3','4','1','56','2','34','0','2']
let arrF = arr.map((item) => {
             return (item+2)
})
console.log(arrF)//["22", "32", "42", "12", "562", "22", "342", "02", "22"]
~~~

##### **reduce()**

**接收一个函数作为参数，做数组的累加器，或者拼接数组项作为字符串,不改变原数组**

~~~javascript
let strArr1 = [2,5,13]
let strArr2 = ['s','r','f']
function compute(total,currentValue) {
  return total+currentValue
}
let res1 = strArr1.reduce(compute)
let res2 = strArr2.reduce(compute)
console.log(res1+"----"+res2)//20----srf
~~~

##### from()

`Array.from`可以接受三种类型的参数

* `Array.from(obj,mapFn)`

  `obj`指的是数组对象、类似数组对象或者是set对象，`map`指的是对数组中的元素进行处理的方法。

  ~~~javascript
  //将数组中布尔值为false的成员指为0
  console.log(Array.from([1, , 2, 3, 3], x => x || 0));
  // [1, 0, 2, 3, 3]
  //将一个类似数组的对象转为一个数组，并在原来的基础上乘以2倍
  let arrayLike = {
      '0':'2',
      '1':'4',
      '2':'5',
      length:3
  };
  console.log(Array.from(arrayLike, (x) => {
     return x * 2;
  } ));//[4, 8, 10]
  
  //将一个set对象转为数组，并在原来的基础上乘以2倍
  console.log(Array.from(new Set([1, 2, 3, 4]), x => x*2));
  //[2, 4, 6, 8]
  
  ~~~

* `Array.from({length:n},Fn)`

  第一个参数指定了第二个参数执行的次数。可以将各种值转化为真正的数组。

  ~~~javascript
  console.log(Array.from({length: 3}, () => 'jack'));// ["jack", "jack", "jack"]
  console.log(Array.from({length: 3}, item => (item = {'name': 'Jack', 'age': 18})));
  // 0: {name: "Jack", age: 18}
  // 1: {name: "Jack", age: 18}
  // 2: {name: "Jack", age: 18}
  console.log(Array.from({length: 2}, (v, i) => v = {index: i}));
  //0: {index: 0}
  //1: {index: 1}
  ~~~

* 接受一个字符串

  ~~~javascript
  console.log(Array.from('abc'));//["a", "b", "c"]
  ~~~

##### filter()

主要原理是 filter会把传入的函数依次作用于每个元素，然后根据返回值是 true 还是false决定保留还是丢弃该元素。

~~~javascript
//在一个Array中过滤掉小于2的数据，得到大于2的数据，
var arr1 = [1,2,3,4,5,6];
arr1 = arr1.filter(function (x) {
    return x>2;
});
console.log(arr1);
~~~





### 字符串

#### 字符串的方法和属性：

**所有字符串方法都会返回新字符串。它们不会修改原始字符串。**

##### 查找字符串中的字符串

* `indexOf()`方法返回字符串中指定文本*首次*出现的索引（位置）

* `lastIndexOf()` 方法返回指定文本在字符串中*最后*一次出现的索引

* `lastIndexOf()` 方法向后进行检索（从尾到头），这意味着：假如第二个参数是 50，则从位置 50 开始检索，直到字符串的起点。（`str.lastIndexOf("111",50);`)

* search() 方法返回字符串中指定文本第一次出现的位置

两种方法，`indexOf()` 与 `search()`，是**相似的**。这两种方法是不相等的。区别在于：

- `search() `方法无法设置第二个开始位置参数。
- `indexOf() `方法无法设置更强大的搜索值（正则表达式）。

##### 提取部分字符串

* slice()提取字符串的某个部分并在新字符串中返回被提取的部分.该方法设置两个参数：起始索引（开始位置），终止索引（结束位置）。

  * 如果某个参数为负，则从字符串的结尾开始计数。
  * 如果省略第二个参数，则该方法将裁剪字符串的剩余部分

* substring() 类似于 slice()。不同之处在于 substring() 无法接受负的索引。

* substr() 类似于 slice()。不同之处在于第二个参数规定被提取部分的**长度**。

  * 如果省略第二个参数，则该 substr() 将裁剪字符串的剩余部分。

  * 如果首个参数为负，则从字符串的结尾计算位置。第二个参数不能为负，因为它定义的是长度。

  * ~~~javascript
    var str = "Apple, Banana, Mango";
    var res = str.substr(-5);//Mango
    ~~~

##### 替换字符串内容

* replace() 方法用另一个值替换在字符串中指定的值，replace() 方法不会改变调用它的字符串。它返回的是新字符串。

  * replace() *只替换首个匹配*

  * replace() 对大小写敏感。如需执行大小写不敏感的替换，请使用正则表达式 /i（大小写不敏感，请注意正则表达式不带引号。）：

  * ~~~javascript
    str = "Please visit Microsoft!";
    var n = str.replace(/MICROSOFT/i, "W3School");
    ~~~

  * 如需替换所有匹配，请使用正则表达式的 g 标志（用于全局搜索）：

  * ~~~javascript
    str = "Please visit Microsoft and Microsoft!";
    var n = str.replace(/Microsoft/g, "W3School");
    ~~~

##### 转换为大写和小写

* `toUpperCase() `把字符串转换为大写
* `toLowerCase() `把字符串转换为小写

##### 提取字符串中的字符

* chatAt(position)

  charAt() 方法返回字符串中指定下标（位置）的字符串

* charCodeAt(position)

  charCodeAt() 方法返回字符串中指定索引的字符 unicode 编码：

##### concat()连接字符串

`concat()` 方法可用于代替加运算符。下面两行是等效的：

~~~javascript
var text = "Hello" + " " + "World!";
var text = "Hello".concat(" ","World!");
~~~

##### trim() 方法删除字符串两端的空白符

Internet Explorer 8 或更低版本不支持 **trim()** 方法。如需支持 IE 8，您可搭配正则表达式使用 replace() 方法代替：

~~~javascript
var str = "     Hello World!     ";
console.log(str.trim());
console.log(str.replace(/^[\s\uFEFF\xA0]+|[\s\uFEFF\xA0]+$/g, ''));
~~~

##### 把字符串转换为数组---split()

~~~javascript
var txt = "a,b,c,d,e";
console.log(txt.split(","));//["a", "b", "c", "d", "e"]
console.log(txt.split(" "));//["a,b,c,d,e"]
console.log(txt.split(/\s+/));//["a,b,c,d,e"]
console.log(txt.split("|"));//["a,b,c,d,e"]
~~~

如果分隔符是 ""，被返回的数组将是间隔单个字符的数组：

~~~javascript
var txt1 = "hello";
console.log(txt1.split(""));//["h", "e", "l", "l", "o"]
~~~

### 对象

#### 判断对象中是否含有某个键值的方法

1. 使用in关键字

~~~javascript
var o = {x:1};
console.log("x" in o);//true
console.log("y" in o);//false
console.log("toString" in o);//true---继承属性
~~~

该方法可以判断对象的自有属性和继承属性是否存在

2. 使用对象的`hasOwnProperty()`方法

~~~javascript
console.log(o.hasOwnProperty("x"));//true
console.log(o.hasOwnProperty("y"));//false
console.log(o.hasOwnProperty("toString"));//false
~~~

该方法只能判断自有属性是否存在，对继承属性返回false

3. 使用undefined判断

~~~
console.log(o.x !== undefined);//true
console.log(o.y !== undefined);//false
console.log(o.toString !== undefined);//true
~~~

该方法有一个问题，如果x对应的值为undefined则返回false

4. 在条件语句中判断

~~~javascript
if (o.x){
   console.log(true);
}
~~~

