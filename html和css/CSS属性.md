#### `css`属性

#### `CSS`文本

##### font-variant

把段落设置为小型大写字母字体

small-caps(小型大写字母字体)

![](C:\Users\whichone\Desktop\前端学习笔记\html和css\image\font-variant.png)

##### word/letter-spacing

word-spacing：每个单词之间的间距

letter-spacing：每个字母之间的间距

![](C:\Users\whichone\Desktop\前端学习笔记\html和css\image\wordletter-spacing.png)

##### text-transform

转换不同元素的文本，控制文本的大小写

capitalize(文本中的每个单词以大写字母开头)|uppercase(仅有大写字母)|lowercase(仅有小写字母)

##### text-decoration

定义文本的下划线，可能的取值：

underline|overline|line-through|blink(blink：闪烁，只在火狐显示)

![](C:\Users\whichone\Desktop\前端学习笔记\html和css\image\text-decoration.png)

##### vertical-align

设置元素的垂直对齐方式

##### text-indent

设置文本缩进

##### direction

设置文本方向（ltr/rtl）

##### text-shadow

设置文本阴影（横向距离，纵向距离，颜色）

text-shadow:5px 5px rgba(255,0,0,0.5)；

![](C:\Users\whichone\Desktop\前端学习笔记\html和css\image\text-shadow.png)

##### white-space

设置元素中空白的处理方式

![](C:\Users\whichone\Desktop\前端学习笔记\html和css\image\white-space.png)

#### `CSS`列表

##### list-style-type

设置列表项标记的类型

![](C:\Users\whichone\Desktop\前端学习笔记\html和css\image\list-style-type.png)

![](C:\Users\whichone\Desktop\前端学习笔记\html和css\image\list-style-type-demo.png)

##### list-style-position

设置在何处放置列表项标记（inside|outside（默认））

##### list-style-image

使用图像来替换列表项的标记（url（“”）|none）

##### list-style

简写属性（list-style: list-style-image  list-style-type  list-style-position）

#### CSS表格

##### border-collapse

是否将表格的边框合并为一个单一的边框（collapse(合并)|separate(分开)）

##### border-spacing

当表格边框分开时，设置相邻单元格间的距离

##### caption-side

设置表格标题的位置（top（默认顶部）|bottom（底部））

##### empty-cells

设置是否显示表格中的空单元格，仅针对分离模式（hide|show（默认)）

##### table-layout

显示表格单元格、行、列的算法规则

（auto（默认，列宽度由单元格内容决定）|fixed（列宽由表格宽度和列宽度设定））

#### CSS轮廓

##### outline-color

设置轮廓颜色

##### outline-style

设置轮廓样式（dotted(点状)|dashed(虚线)|solid(实线)|double(双线)）

##### outline-width

设置轮廓宽度（thin(细轮廓)|medium(默认，中等轮廓)|thick(粗轮廓)|length(自定义轮廓宽度)）

##### outline

设置所有的轮廓属性

outline: outline-color outline-style outline-width;

#### 伪元素

CSS伪元素用于向某些选择器设置特殊效果

##### :first-line

用于向**文本的首行**设置特殊样式。只能用于块级元素

##### :first-letter

用于向**文本的首字母**设置特殊样式

##### :before

在元素的内容前面插入新内容