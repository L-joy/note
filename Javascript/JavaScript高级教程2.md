---
layout: 'n'
title: JavaScript高级教程
date: 2019-07-16 10:21:38
tags: [前端]

---

## 第二章  在HTML中使用JavaScript

本章内容

* 使用&lt;script&gt;元素

* 嵌入脚本与外部脚本

* 文档模式对 JavaScript 的影响

* 考虑禁用 JavaScript 的场景 

  <!--more-->

### 2.1 &lt;script&gt;元素

向 HTML 页面中插入 JavaScript 的主要方法，就是使用&lt;script&gt;元素 ;HTML 4.01为&lt;script&gt;定义了6个属性：

* async：可选。表示应该立即下载脚本，但不应妨碍页面中的其他操作，比如下载其他资源或等待加载其他脚本。只对外部脚本文件有效。 
* charset：可选。表示通过 src 属性指定的代码的字符集。由于大多数浏览器会忽略它的值，因此这个属性很少有人用 
* defer：可选。表示脚本可以延迟到文档完全被解析和显示之后再执行。只对外部脚本文件有效。 IE7 及更早版本对嵌入脚本也支持这个属性。 
* language：已废弃。原来用于表示编写代码使用的脚本语言（如 JavaScript、 JavaScript1.2 或 VBScript）。大多数浏览器会忽略这个属性，因此也没有必要再用了。 
* src：可选。表示包含要执行代码的外部文件。 
* type：可选。可以看成是 language 的替代属性；表示编写代码使用的脚本语言的内容类型（也称为 MIME 类型）。 

使用&lt;script&gt;元素的方式有两种：直接在页面中嵌入 JavaScript 代码和包含外部 JavaScript文件。 

* 在使用&lt;script&gt;元素嵌入 JavaScript 代码时，只须为&lt;script&gt;指定 type 属性。然后，像下面这样把 JavaScript 代码直接放在元素内部即可： 

~~~javascript
<script type="text/javascript">
   function sayHi(){
      alert("Hi!");
   }
</script>
~~~

   在使用&lt;script&gt;嵌入 JavaScript 代码时，记住不要在代码中的任何地方出现"&lt;/script&gt;"字符串。 否则会报错，可以用转义字符"/"来解决这个问题：

~~~javascript
<script type="text/javascript">
   function sayScript(){
     alert("<\/script>");
   }
</script>
~~~

* 如果要通过&lt;script&gt;元素来包含外部 JavaScript 文件，那么 src 属性就是必需的。这个属性的值是一个指向外部 JavaScript 文件的链接，例如： 

~~~javascript
<script type="text/javascript" src="example.js" />
~~~

#### 2.1.1 标签的位置

* 传统做法：

  ~~~javascript
  <!DOCTYPE html>
  <html>
    <head>
      <title>Example HTML Page</title>
      <script type="text/javascript" src="example1.js"></script>
      <script type="text/javascript" src="example2.js"></script>
    </head>
    <body>
       <!-- 这里放内容 -->
    </body>
  </html>
  ~~~

  可是，在文档的<head>元素中包含所有 JavaScript 文件，意味着必须等到全部 JavaScript 代码都被下载、解析和执行完成以后，才能开始呈现页面的内容（浏览器在遇到&lt;body&gt;标签时才开始呈现内容）。 导致浏览器在呈现页面时出现明显的延迟，而延迟期间的浏览器窗口中将是一片空白。 

* 为避免这个问题：

  ~~~javascript
  <!DOCTYPE html>
     <html>
     <head>
       <title>Example HTML Page</title>
     </head>
     <body>
       <!-- 这里放内容 -->
       <script type="text/javascript" src="example1.js"></script>
       <script type="text/javascript" src="example2.js"></script>
     </body>
  </html>
  ~~~

  这样，在解析包含的 JavaScript 代码之前，页面的内容将完全呈现在浏览器中。而用户也会因为浏览器窗口显示空白页面的时间缩短而感到打开页面的速度加快了。 

#### 2.1.2 延迟脚本

&lt;script&gt;的defer属性表明脚本在执行时不会影响页面的构造。 也就是说，脚本会被延迟到整个页面都解析完毕后再运行。因此，在&lt;script&gt;元素中设置defer 属性，相当于告诉浏览器立即下载，但延迟执行。 

~~~javascript
<!DOCTYPE html>
   <html>
   <head>
     <title>Example HTML Page</title>
     <script type="text/javascript" defer="defer" src="example1.js"></script>
     <script type="text/javascript" defer="defer" src="example2.js"></script>
   </head>
   <body>
      <!-- 这里放内容 -->
   </body>
</html>
~~~

defer 属性只适用于外部脚本文件。 

在 XHTML 文档中，要把 defer 属性设置为 defer="defer"。 

#### 2.1.3 异步脚本

&lt;script&gt;的async属性与 defer 属性类似都用于改变处理脚本 的行为。同样与 defer 类似， async 只适用于外部脚本文件，并告诉浏览器立即下载文件。但与 defer不同的是，标记为 async 的脚本并不保证按照指定它们的先后顺序执行。例如： 

~~~javascript
<!DOCTYPE html>
  <html>
  <head>
    <title>Example HTML Page</title>
    <script type="text/javascript" async src="example1.js"></script>
    <script type="text/javascript" async src="example2.js"></script>
  </head>
  <body>
    <!-- 这里放内容 -->
  </body>
</html>
~~~

在以上代码中，第二个脚本文件可能会在第一个脚本文件之前执行。因此，确保两者之间互不依赖非常重要。指定 async 属性的目的是不让页面等待两个脚本下载和执行，从而异步加载页面其他内容。为此，建议异步脚本不要在加载期间修改 DOM。 

在 XHTML 文档中，要把 async 属性设置为 async="async"。 

#### 2.1.4 在XHTML中的用法

可扩展超文本标记语言，即 XHTML（ Extensible HyperText Markup Language），是将 HTML 作为XML 的应用而重新定义的一个标准。 编写 XHTML 代码的规则要比编写 HTML 严格得多 ；

保证让HTML代码在 XHTML 中正常运行的方法，就是用一个 CData 片段来包含 JavaScript 代码。在 XHTML（ XML）中， CData 片段是文档中的一个特殊区域，这个区域中可以包含不需要解析的任意格式的文本内容。因此，在 CData 片段中就可以使用任意字符而且不会导致语法错误。引入 CData 片段后的 JavaScript 代码块如下所示： 

~~~javascript
<script type="text/javascript"><![CDATA[
   function compare(a, b) {
     if (a < b) {
        alert("A is less than B");
     } else if (a > b) {
        alert("A is greater than B");
     } else {
        alert("A is equal to B");
     }
  }
]]></script>
~~~

在兼容 XHTML 的浏览器中，这个方法可以解决问题。但实际上，还有不少浏览器不兼容 XHTML，因而不支持 CData 片段。怎么办呢？再使用 JavaScript 注释将 CData 标记注释掉就可以了： 

~~~javascript
<script type="text/javascript">
//<![CDATA[
   function compare(a, b) {
      if (a < b) {
         alert("A is less than B");
      } else if (a > b) {
         alert("A is greater than B");
      } else {
        alert("A is equal to B");
      }
  }
//]]>
</script>
~~~

### 2.2 嵌入代码和外部文件

在HTML中嵌入JavaScript代码推荐尽可能使用外部文件，优点：

* 可维护性 
* 可缓存 ：如果有两个页面都使用同一个文件，那么这个文件只需下载一次。因此，最终结果就是能够加快页面加载的速度。 
* 适应未来 ：通过外部文件来包含 JavaScript 无须使用前面提到 XHTML 或注释 hack。 HTML 和XHTML 包含外部文件的语法是相同的。 

### 2.3 &lt;noscript&gt;元素

当浏览器不支持 JavaScript 时如何让页面平稳地退化 ，解决方案就是创造一个&lt;noscript&gt;元素 ,用以在不支持JavaScript 的浏览器中显示替代的内容。这个元素可以包含能够出现在文档&lt;body&gt;中的任何 HTML 元素 ,除&lt;script&gt;元素；

包含在&lt;noscript&gt;元素中的内容只有在下列情况下才会显示出来：

* 浏览器不支持脚本；

* 浏览器支持脚本，但脚本被禁用。

  符合上述任何一个条件，浏览器都会显示&lt;noscript&gt;中的内容。而在除此之外的其他情况下，浏览器不会呈现&lt;noscript&gt;中的内容。 

  ~~~javascript
  <html>
    <head>
      <title>Example HTML Page</title>
      <script type="text/javascript" defer="defer" src="example1.js"></script>
      <script type="text/javascript" defer="defer" src="example2.js"></script>
    </head>
    <body>
      <noscript>
        <p>本页面需要浏览器支持（启用） JavaScript。
      </noscript>
  </body>
  </html>
  ~~~

  这个页面会在脚本无效的情况下向用户显示一条消息。而在启用了脚本的浏览器中，用户永远也不
  会看到它——尽管它是页面的一部分。 

