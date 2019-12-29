#### 页面/资源加载过程

1. URL解析

2. 获取解析的域名，去DNS服务器上查找该域名对应的`ip`

3. 带着请求信息去该`ip`地址上请求资源,然后在服务器上把返回的资源下载下来

4. 浏览器根据拿到资源的不同做不同的解析

   ![](image\资源加载过程.png)

* URL结构

  ![](image\URL结构.png)

* DNS查询

  ![](image\DNS查询.png)

DNS缓存用于减少DNS查询量

* 资源请求

  ![](image\资源请求.png)

* 浏览器解析

  ![](image\浏览器解析.png)

#### ES6

Babel : ES6->ES5的转换器

##### `let，const`

* let定义常量，`const`定义变量

* 不能重复定义

* 块级作用域

* 不存在变量提升

  ~~~javascript
  console.log(b);//undefined
  var b = 2;
  console.log(a);//报错
  let a = 1;
  console.log(c);//报错
  const c = 3;
  ~~~

##### 箭头函数

* 参数 => 表达式/语句

* 继承外层作用域

* 不能用作构造函数

* 没有prototype属性

  ~~~javascript
      //箭头函数
      let value = 2;
      let double = x => 2 * x;
      let treble = x => {
          return 3 * x;
      };
      console.log(double(value));//4
      console.log(treble(value));//6
  
      //没有独立作用域
      var obj = {
          commonFn : function () {
              console.log(this);//obj
          },
          arrFn : () => {
              console.log(this);//window
          }
      };
      obj.commonFn();
      obj.arrFn();//this 指向了obj所在作用域，指向window
  
      //不能用作构造函数
      let Animal = () => {
  
      };
      let animal = new Animal();
      console.log(animal);//报错：Animal is not a constructor
  
      //没有prototype
      let commonFn = function () {};
      let arrowFn = () => {};
      console.log(commonFn.prototype);
      console.log(arrowFn.prototype);//undefined
  ~~~

##### 模板字符串

* 反引号标识``

* 支持多行字符串

* 支持变量和表达式

  ~~~javascript
     //模板字符串
      //基本用法
      let str = `
      <div>
         <h1 class="title">123</h1>
       </div>
      `;
      document.querySelector('body').innerHTML = str;
  
      //嵌套变量
      let name = 'Jackson';
      let str1 = `
      <div>
         <h1 class="title">${name}</h1>
       </div>
      `;
      document.querySelector('body').innerHTML = str1;
  
      //嵌套函数
      let getName = () => {
          return "Jackson test";
      };
      let str2 = `
      <div>
         <h1 class="title">${getName()}</h1>
       </div>
      `;
      document.querySelector('body').innerHTML = str2;
  
      //循环嵌套
      let names = ['Jackson','Rose','Joe'];
      let str3 = `
      <ul>
      ${
          names.map(name => `<li>I am ${name}</li>`).join('')
      }
      </ul>
      `;
      document.querySelector('body').innerHTML = str3;
  ~~~

##### Promise

* Promise对象
* 关键词：resolve,reject,then

~~~javascript
//Promise对象
    //Promise结构
    new Promise((resolve, reject) => {
        //异步函数
        $.ajax({
            url: 'http://happymmall.com/user/login.do',
            type: 'post',
            success(res) {
                resolve(res);
            },
            error(err) {
                reject(err);
            }
        });
    }).then((res) => {
        console.log('success:',res);
    },(err) => {
        console.log('error:',err);
    });

    //链式promise
    var promiseFn1 = new Promise((resolve, reject) => {
        $.ajax({
            url: 'http://happymmall.com/user/login.do',
            type: 'post',
            success(res) {
                resolve(res);
            },
            error(err) {
                reject(err);
            }
        });
    });
    var promiseFn2 = new Promise((resolve, reject) => {
        $.ajax({
            url: 'http://happymmall.com/cart/get_cart_product_count.do',
            type: 'post',
            success(res) {
                resolve(res);
            },
            error(err) {
                reject(err);
            }
        });
    });
    promiseFn1.then(() =>{
        console.log('promiseFn1 success');
        return promiseFn2;
    }).then(() =>{
        console.log('promiseFn2 success');
    });
~~~

##### 面向对象-类

* 关键词：class

* 语法糖：对应function

* 构造函数，constructor

* 类的继承：1. extends：类的继承；2. super：调用父类的构造函数

  ~~~javascript
    // class constructor
      class Animal{
          constructor(name){
              this.name = name;
          }
          getName(){
              return this.name;
          }
      }
      let animal = new Animal('Jackson1');
      console.log(animal);
      console.log(animal.getName());
  
      // 类的继承
      class Animal{
          constructor(){
              this.name = "animal";
          }
          getName(){
              return this.name;
          }
      }
      class Cat extends Animal{
          constructor(){
              super();
              this.name = 'cat';
          }
      }
      let animal = new Animal();
      console.log(animal.getName());//animal
      let cat = new Cat();
      console.log(cat.getName());//cat
  ~~~

##### 面向对象-对象

* 对象里属性的简写

* 对象里方法的简写

* 属性名可以为表达式

* 其他扩展

  ~~~javascript
      // 对象的用法
      // var name = 'Jackson',
      //     age = 19;
      // var obj = {
      //     name : name,
      //     age : age
      //     getName : function () {
      //         return this.name;
      //     },
      //     getAge : function () {
      //         return this.age;
      //     }
      // };
  
      //ES6中对象的用法
      let name = 'Jackson',
          age = 19;
      let obj = {
          //变量名可以直接用作对象的属性名称
          name,
          age,
          //对象里的方法可以简写
          getName(){
              return this.name;
          },
          //表达式作为属性名或者是方法名
          ['get'+'Age'](){
              return this.age;
          }
      };
      console.log(obj);
      console.log(obj.getName());//Jackson
      console.log(obj.getAge());//19
  
      //Object对象的扩展
      console.log(Object.keys(obj)); //["name", "age", "getName", "getAge"]
      console.log(Object.assign({a: 1}, {b: 2}));//{a: 1, b: 2}
      console.log(Object.assign({a: 1}, {a: 2, b: 4}));//{a: 2, b: 4}
  ~~~

##### ES6模块化

![](image\ES6模块化.png)

关键词：export，import

~~~html
index.html
<script type="module" src="index.js">
</script>
~~~

~~~javascript
index.js
import {str,obj,fn} from './module'
import foo from './module'
console.log(str);
console.log(obj);
console.log(fn);
console.log(foo);
~~~

~~~javascript
module.js
let str = 'string';
let obj = {
    name: 'Jackson'
};
let fn = () => {
    console.log('module test');
};
export {
    str,
    obj,
    fn
}
export default {a:1}
~~~

#### 本地存储

##### cookie

![](image\cookie.png)

cookie字段

![](image\cookie字段.png)

##### session

![](image\session.png)

##### localStorage和sessionStorage

![](image\localStorage.png)

~~~javascript
// 添加localStorage
window.localStorage.setItem('name','Jackson');
// 查看localStorage
window.localStorage.getItem('name');
~~~

![](image\sessionStorage.png)

#### 前端框架分析

三大框架：Angular JS、React、Vue

![](image\Angular.png)

![](image\React.png)

![](image\Vue.png)

##### 三大框架对比

![](image\三大框架对比.png)

#### 开发环境搭建

##### git安装和项目建立

* 安装git

* 配置`gitconfig`

~~~
vim ~/.gitconfig
~~~

![](image\配置config.png)

~~~
#点击ESC，输入`:wq`进行保存退出
#查看配置信息
cat ~/.gitconfig
~~~

![](image\查看config.png)

* 建立git项目

  * 生成公钥

    ![](image\生成公钥.png)

    获取`.ssh`文件夹中的`id_rsa.pub`文件中的内容

  * 在码云中建立仓库，并在管理--->部署公钥管理---->公钥管理里添加上步文件中的内容

  * 在本地新建文件夹，并将远程仓库克隆下来

    ~~~
    mkdir doc
    cd docs/
    git clone git@gitee.com:JoeHappyMMall/happymmall.git
    #切换到有.git的文件夹happymmall配置每次上传要忽略的文件
    cd happymmall/
    vim .gitignore
    #添加
    .DS_Store
    node_modules
    dist
    *.log
    ESC--->:wq #保存退出
    #将本地的gitignore上传至码云
    git add.
    git commit -m "initial"
    git push
    ~~~

##### yarn的安装

![](image\yarn.png)

![](image\yarn语法.png)

~~~
#安装yarn
npm install yarn -g
yarn init#在文件夹中出现一个package.json文件
#将这个文件上传至远程仓库（同上传gitignore一样）

~~~

##### `webpack`

环境搭建过程

~~~
#将远程仓库中文件克隆下来
git clone "git@gitee.com:JoeHappyMMall/happymmall.git"
#下载node_modules
npm install express
#下载yarn
npm install yarn
#安装webpack
yarn add webpack@3.10.0 --dev
#安装htmlwebpackplugin
yarn add html-webpack-plugin@2.30.1 --dev
#安装babel
yarn add babel-core@6.26.0 babel-preset-env@1.6.1 babel-loader@7.1.2 --dev
#安装react
yarn add babel-preset-react@6.24.1 --dev
yarn add react@16.2.0 react-dom@16.2.0
#配置css样式
yarn add style-loader@0.19.1 css-loader@0.28.8 --dev
#安装extract-text-webpack-plugin插件将css文件独立打包
yarn add extract-text-webpack-plugin@3.0.2 --dev
#安装sass-loader及其依赖node-sass
yarn add sass-loader@6.0.6 --dev
yarn add node-sass@4.7.2 --dev/ npm install node-sass
#处理图片
yarn add file-loader@1.1.6 url-loader@0.6.2 --dev
#配置字体图标
yarn add font-awesome
#下载webpack-dev-server
yarn add webpack-dev-server@2.9.7 --dev

~~~

#### React

视图层框架

- 一个构建用户界面的框架
- 声明式的框架
- 数据驱动DOM，再用事件反馈给数据

组件化开发

- 组件组合而不是继承
- state&&props
- 生命周期

JSX语法

- 一种JS扩展的表达式
- 带有逻辑的标记语法，有别于HTML模板
- 对样式，逻辑表达式和事件的支持

虚拟DOM

- 对DOM进行模拟
- 比较操作前后的数据差异
- 如果有数据差异，统一操作DOM

优点：简洁、灵活、高效

#### 代码提交

~~~
git status
git merge origin master
git pull
git add .
git commit -m "111"
git push
git tag tag-product
git push tag-product

~~~

