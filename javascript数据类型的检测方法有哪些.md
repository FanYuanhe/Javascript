### javascript有哪些判断数据类型的方法？
#### 1、js中有哪些数据类型？

```
基本数据类型
    * string
    * number
    * undefined
    * null
    * boolean
    * symbol  ES6新增数据类型,代表一个唯一的值,不能使用new命令，会报错。例：let a = Symbol()
复杂数据类型
    * object
        Array
        Object
        Function
        Date
        RegExp
        ...
```
#### 2、如何判断数据类型？
```
let str = 'str',
    num = 123,
    und = undefined,
    bool = true,
    sym = Symbol(),
    nul = null,
    arr = [1,2,3],
    obj = {name:'fanyuanhe'},
    fun = function(){console.log(1)}
```
##### typeof方法
```
typeof(str) "string"
typeof(num) "number"
typeof(und) "undefined"
typeof(sym) "symbol"
typeof(bool) "boolean"
typeof(nul) "object" 从一开始出现JavaScript就是这样的
typeof(arr) "object"
typeof(obj) "object"
typeof(fun) "function"
```
>typeof() 方法是js提供的一种判断数据类型的方法，括号在使用中可以省略，如图所示typeof可以判断出基本数据类型和function，但是不能准确的判断出null、array、object等数据类型。

##### instanceof方法
>简单来说instanceof方法判断的原理为，判断某个变量是否为某个方法的实例。
><u>更重的一点是 instanceof 可以在继承关系中用来判断一个实例是否属于它的父类型。</u>
>instanceof 方法相关请看[JavaScript中instanceof运算符深入剖析.md](https://github.com/FanYuanhe/Javascript/blob/master/JavaScript%E4%B8%ADinstanceof%E8%BF%90%E7%AE%97%E7%AC%A6%E6%B7%B1%E5%85%A5%E5%89%96%E6%9E%90.md)

```
str instanceof String //false
num instanceof Number //false
num instanceof Symbol //false
arr instanceof Array //true
obj instanceof Object //true
fun instanceof Function //true
bool instanceof Boolean //false
```
>instanceof方法不能判断出<u>没有使用new</u>方法实例化出来的<u>基本数据类型</u>，可以判断出没有使用new方法定义的复杂数据类型的变量

##### Object.prototype.toString.call()
>在任何值上调用 Object 原生的 toString() 方法，都会返回一个 [object NativeConstructorName] 格式的字符串。每个类在内部都有一个 [[Class]] 属性，这个属性中就指定了上述字符串中的构造函数名。

```
Object.prototype.toString.call(str) "[object String]"
Object.prototype.toString.call(num) "[object Number]"
Object.prototype.toString.call(bool) "[object Boolean]"
Object.prototype.toString.call(nul) "[object Null]"
Object.prototype.toString.call(und) "[object Undefined]"
Object.prototype.toString.call(arr) "[object Array]"
Object.prototype.toString.call(obj) "[object Object]"
Object.prototype.toString.call(fun) "[object Function]"
```
>jquery的$.type()内部原理就是用的Object.prototype.toString.call()

