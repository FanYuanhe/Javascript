### javascript中使用call、apply、bind修改this指向
#### 一、改变this指向有哪些方法？
1. 使用 ES6 的箭头函数
2. 在函数内部使用 _this = this
3. 使用 apply、call、bind

##### 1、ES6箭头函数
```
() => {} 等价于 function(){}
```
>ES6新增的箭头函数，规定函数内部的`this`对象，**永远指向它定义的时候所在的对象**，而不是像es5那样指向最后一个调用它的对象。
>话说箭头函数不能当作构造函数使用，也就是不能使用new命令，会报错

```
 var name = "windowsName";

    var obj = {
        name : "Cherry",

        func1: function () {
            console.log(this.name)     
        },

        func2: function () {
            setTimeout( () => { //使用箭头函数
                this.func1()
            },100);
        }

    };

    obj.func2()     // Cherry
```

##### 2、`_this = this`方法
>事先将指向该对象的this存储在一个变量中，这样在我们使用的时候this就不会改变了

```
 var name = "windowsName";

    var obj = {
        name : "Cherry",

        func1: function () {
            console.log(this.name)     
        },

        func2: function () {
            var _this = this;
            setTimeout(function(){
                _this.func1();
            },100);
        }

    };

    obj.func2()     // Cherry
```

>例子中我们事先将指向了obj对象的this，保存在了_this变量中，所以在我们调用`_this.func1()`的时候_this就是指向了obj。

##### 3、使用call、apply、bind来处理
```
func.call(“this的指向”，参数1，参数2，...)

func.apply("this的指向"，[参数1，参数2，...])

func.bind("this的指向")()
```
>`bind`方法返回了一个新的函数，需要我们自己执行一下。`call`接收多个参数列表，`apply`接受一个数组作为参数

```
var obj = {
    num: 1,
    getNum: function(num1,num2){
        console.log(num1,num2,this.num)
    }
}

var num = 2;

var getNum = obj.getName;

getNum(333,666) // 333 666 2
obj.getName(333,666) // 333 666 1
```

>如果我就是很任性的想`getNum(333,666)`输出`333 666 1`该怎么办呢？
>`那我们就涉及到了要改变this指向的问题了？`

```
getName.call(Obj,333,666) //333 666 1

getName.apply(obj,[333,666]) //333 666 1

getName.bind(obj, 333, 666)()
```
>`call、bind`本质上还是直接执行我们的`getName`方法，但是`bind`稍有不同，它为我们生成了一个新的函数，所以我们要`getName.bind(obj, 333, 666)()`自己执行一下才可以
