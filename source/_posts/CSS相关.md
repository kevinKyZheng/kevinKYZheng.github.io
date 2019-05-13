# 布局主要需要下面的两项
使用flex的话,一般就用不到position

1. display

默认inline,block,inline-block,flex

* inline 前后没换行
* block 前后有换行
* inline-block 区别于inline,可以设置宽高,可以设置top和bottom的margin和padding

2. position

默认是static,relative absolute fixed sticky,配合top,left,bottom,right使用

relative 不改变当前布局的前提下,修改位置

CSS属性
1. outline和border区别

outline 不占用空间;可以是非矩形的;
border 可以设置圆角,可以设置其中一条边

2. overflow
内容溢出元素框的时候的表现
可选值有:visible,hidden,scroll,auto
可见,裁剪后不可见,裁剪后可滚动,如果裁剪就滚动

3. text相关

text-align,text-weight,text-size,text-overflow,

4. 2D变换
transform
位置:translate(x,y),translateX,translateY;
旋转:rotate()
缩放:scale

5. 多媒体类型查询
@media screen and (min-width: 480px) 
有待完善





