1. container的属性

* display:flex

* flex-flow:<‘flex-direction’> || <‘flex-wrap’> 下面两个属性的结合
* flex-direction:row|column|row-reverse|column-reverse
* flew-wrap:nowrap|wrap|wrap-reverse 换行

* justify-content:flex-start | flex-end | center | space-between | space-around | space-evenly 主轴的展示方式
* align-items: stretch | flex-start | flex-end | center | baseline 
交叉轴的展示方式(一行)
*   align-content: flex-start | flex-end | center | space-between | space-around | stretch;
交叉轴的展示方式(多行)

2.children的属性
* order: <integer>; /* default is 0 */ 在container中的顺序

* flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
  以下三个的结合,第二第三是可选的,默认是0 1 auto
* flex-grow: <number>; /* default 0 */ 占据剩余空间
* flex-shrink: <number>; /* default 1 */ 缩小的能力
* flex-basis: <length> | auto; /* default auto */  (e.g. 20%, 5rem, etc.) 设置默认大小 auto代表看我的宽高属性

*   align-self: auto | flex-start | flex-end | center | baseline | stretch;
重写align-items这个属性




