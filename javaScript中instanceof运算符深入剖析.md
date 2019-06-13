### javaScript中instanceof运算符深入剖析
在javascript中使用typeof来判断数据类型，对于基本数据类型是没有问题的，但是一旦碰到复杂数据类型（引用数据类型）的时候就会有问题出现。
当使用typeof判断数据、对象的具体数据类型的时候会出现都返回object的情况。
instanceof要求开发者需要明确的确认变量为某种特定类型。
```
let str = new String('hello);
str instanceof String  //true
```
>这段代码可以解释为是在询问，str是否为String构造函数的实例

#### instanceof的基本原理
>个人理解：通俗地讲instanceof就是判断某一个实例（也可以叫做变量），是否是某个构造函数的实例，如果是咋返回true，否则返回false。

```
function Foo() {};
let new_foo = new Foo();
new_foo instanceof Foo //true
```
```
function Parent(){};
function Child(){}
Child.prototype = new Parent() //原型链继承

let new_child = new Child() //你知道这个时候new_child的constructor指向谁吗？怎么修改呢？
new_child instanceof Child //true
new_child instanceof Parent //true
```

#### instanceof进阶题目

```
console.log(Object instanceof Object); //true 
console.log(Function instanceof Function); //true

console.log(Number instanceof Number); //false 
console.log(String instanceof String); //false

console.log(Function instanceof Object); //true 
console.log(Foo instanceof Function); //true

console.log(Foo instanceof Foo); //false
```

#### instanceof的时候发生了什么？
>instanceof源码解析

```
function instanceof(L, R) {//L 表示左表达式，R 表示右表达式
 var O = R.prototype;// 取 R 的显示原型
 L = L.__proto__;// 取 L 的隐式原型
 while (true) { 
   if (L === null) 
     return false; 
   if (O === L)// 这里重点：当 O 严格等于 L 时，返回 true 
     return true; 
   L = L.__proto__;
 } 
}
```

1、Object instanceof为什么？

```
// 为了方便表述，首先区分左侧表达式和右侧表达式
ObjectL = Object, ObjectR = Object; 
// 下面根据规范逐步推演
O = ObjectR.prototype = Object.prototype 
L = ObjectL.__proto__ = Function.prototype 
// 第一次判断
O != L 
// 循环查找 L 是否还有 __proto__ 
L = Function.prototype.__proto__ = Object.prototype 
// 第二次判断
O == L 
// 返回 true
```

2、Function instanceof Function为什么？

```
// 为了方便表述，首先区分左侧表达式和右侧表达式
FunctionL = Function, FunctionR = Function; 
// 下面根据规范逐步推演
O = FunctionR.prototype = Function.prototype 
L = FunctionL.__proto__ = Function.prototype 
// 第一次判断
O == L 
// 返回 true
```

3、Foo instanceof Foo为什么？
```
// 为了方便表述，首先区分左侧表达式和右侧表达式
FooL = Foo, FooR = Foo; 
// 下面根据规范逐步推演
O = FooR.prototype = Foo.prototype 
L = FooL.__proto__ = Function.prototype 
// 第一次判断
O != L 
// 循环再次查找 L 是否还有 __proto__ 
L = Function.prototype.__proto__ = Object.prototype 
// 第二次判断
O != L 
// 再次循环查找 L 是否还有 __proto__ 
L = Object.prototype.__proto__ = null 
// 第三次判断
L == null 
// 返回 false
```



