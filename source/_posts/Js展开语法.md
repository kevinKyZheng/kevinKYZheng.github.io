
Redux:下面两种方法主要用于更新store中state值
# 展开语法
函数调用或者 数组构造时,将数组或者字符串在语法层面展开;
构建字面量对象,将对象按照key-value展开

例如
1. 函数调用 
```
const numbers = [1,2,3]
sum(...numbers)  //6
```
2. 构建字符串或者数组

```
数组拼接
const a = [...numbers,4,5] //1,2,3,4,5

//赋值
const b = [...numbers]//浅拷贝,只遍历一层
```

3. 构建字面量
```
//没有的key会添加,存在的key会覆盖
const obj1 = {"a":1,"b":2}
const obj2 = {"b":3,"c":4}
{...obj1,...obj2}//{ a: 1, b: 3, c: 4 }

//替换个别值
{...obj1,a:3}//{"a":3,"b":2}

```

# Object.assign
语法
>Object.assign(target,...source)
参数:
target:目标对象
source:源对象
返回:
target

1. 拷贝(浅拷贝)
```
const obj1 = {"a":1,"b":2}
Object.assign({},obj1) //{"a":1,"b":2}
```
2. 合并
```
Object.assign(obj1,obj2)//obj1值被改变
Object.assign({},obj1,obj2)//1值不变
```