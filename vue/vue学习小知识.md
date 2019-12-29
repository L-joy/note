冒泡事件：

~~~html
<div class="inner" @click="divHandel" >
   <input type="button" value="点击" @click="btnHandel">
</div>
~~~

~~~javascript
divHandel(){
      console.log("点击inner")
},
 btnHandel(){
      console.log("点击input")
}
~~~

点击input按钮同时输出“点击inner”和“点击input”

阻止冒泡事件——使用.stop阻止冒泡：

~~~html
<div class="inner" @click="divHandel" >
    <input type="button" value="点击" @click.stop="btnHandel">
</div>
~~~

点击input按钮输出“点击input”

实现捕获事件——使用.capture实现捕获触发事件的机制 

即从外向内触发输出：

~~~html
<div class="inner" @click.capture="divHandel" ><input type="button" value="点击" @click="btnHandel">
</div>
~~~

点击input按钮先输出“点击inner”再输出“点击input”

默认事件——使用.prevent阻止默认行为：

例如a标签的默认跳转

~~~html
<a href="http://www.baidu.com" v-on:click.prevent="linkClick">百度</a>
~~~

