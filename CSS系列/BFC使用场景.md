# BFC使用场景

## 什么是BFC
BFC(Block Formatting Context)，简单讲，它是提供了一个独立布局的环境，每个BFC都遵守同一套布局规则。例如，**在同一个BFC内，盒子会一个挨着一个的排**，相邻盒子的间距是由margin决定且垂直方向的margin会重叠。而float和clear float也只对同一个BFC内的元素有效。

## 创建BFC的属性
即当元素CSS属性设置了下列之一时，即可创建一个BFC:

float：left|right
position：absolute|fixed
display: table-cell|table-caption|inline-block
overflow: hidden|scroll|auto

http://www.cnblogs.com/jununx/p/3336553.html 自适应两栏布局实现方式