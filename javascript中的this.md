### javascript中的this
#### 1、先来看一个例子
```
var obj = {
    name: "fanyuanhe",
    getName: function(){
        console.log(this.name);
    }
}

var getName = obj.getName;

var name = "qianqian";

getName();
obj.getName();
```
#### 2、运行结果
```
getName(); //"qianqian"
```
>1、this永远指向最后调用他它的那个对象
>2、变量getName虽然是由obj.getName得来，但在这里并未直接执行obj.getName,而是赋值给了变量getName
>3、getNam方法最后调用是在最外层（window），所以可以理解为最后调用实际上为window.getName()
>4、这时候this指向的就是window，所以this.name就是window.name,即"qianqian"
```
obj.getName() //"fanyuanhe"
```
>1、套用上面的原理，this永远指向最后调用他的那个对象
>2、那我们可以看到最后是在obj层面调用的this.name
>3、所以this指向obj也可以叫window.obj，obj是最后调用this的
>4、所以this.name就是obj.name

#### 究竟为什么会得出上面的运行结果呢？
>JavaScript 语言之所以有this的设计，跟内存里面的数据结构有关系。

>#### 当对象的属性值是一个基本数据类型时
>```
>var obj = { name: "fanyuanhe" }
>```
>1、上面的语句的原理是，首先javascript会在**内存**中存储一个`{ name: "fanyuanhe" }`对象，下一步其实是将该对象在内存中的地址赋值给了obj。
>2、后续如果需要读取obj.name，是要去内存地址中获取obj的地址，然后再从该地址读取出想要的属性。

>#### 当对象的属性值是一个函数的时候
>```
>var obj = { getName: function(){console.log(this.name)} }
>```
>1、这时javascript引擎会将这个函数单独保存在内存中，然后将保存函数的内存地址，赋值给该对象的属性。
2、**由于函数是一个单独储存在内存中的值，所以它可以在任意环境下执行**
3、**javascript中允许函数调用其他环境的变量，所以这个时候就需要我们用某个东西代表当前的执行环境，所以`this`就出现了**

###### **当我们在执行**
```
var getName = obj.getName;
getName();
```
>
就变成了（伪代码）
```
//window环境下单独执行getName
var name = "qianqian"
function getName() {console.log(window.name)}
```
##### **当我们执行**
```
obj.getName()
```
就变成了（伪代码）
```
//obj环境下单独执行getName()
function getName() {console.log(obj.name)}
```



