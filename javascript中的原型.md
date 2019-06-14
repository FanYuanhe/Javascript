### javascript中的原型
#### 1、带着一下几点规则来看本篇文章

1. 所有的对象都有"__proto__"属性，该属性对应该对象的原型,当一个函数被用作构造函数来创建实例时，该函数的prototype属性值将被作为原型赋值给所有对象实例（也就是设置实例的__proto__属性）
2. 所有的函数对象都有"prototype"属性，该属性的值会被赋值给该函数创建的对象的"__proto__"属性
3. 所有的原型对象都有"constructor"属性，该属性对应创建所有指向该原型的实例的构造函数
4. 函数对象和原型对象通过"prototype"和"constructor"属性进行相互关联

#### 2、接下来我们看一个例子
```
function Parent(){
    this.name = "fanyuanhe";
}

var p = new Parent(); //实例化一个Parent
```
#### 3、提出问题
>1、`p.__proto__` 是什么？
>```
>{constructor: ƒ}
>```
>>**`__proto__`是什么呢？**
>>>`__proto__`通常叫做隐式原型
>>>所有的对象都有`__proto__`属性，它本身就是一个对象(打括号的格式)。
>>>`__proto__`这个属性对应的就是创建该对象的原型

>2、`p.prototype` 是什么？
>```
>undefined //p是一个函数对象的实例，没有prototype属性
>```
>>**`prototype`是什么呢？**
>>>原型属性
所有的函数都有prototype属性，当创建该函数对象的实例的时候，该属性的值会被赋给，具体实例的`__proto__`属性，所以`p.__proto__ === Parent.prototype`

>3、`p.constructor` 是什么？
>```
>ƒ Parent(){
>   this.name = "fanyuanhe";
>}
>```
>>**`constructor`是什么？**
>>>学名构造函数
所有的原型对象都有constructor属性。
**那这里p是一个实例啊？为啥会有constructor属性呢？**
1、实例本身确实是没有constructor属性的，这是一个绝对正确的结论。
2、这里能访问到主要是因为原型链的作用，p实例本身没有，下一步会向他的原型上面查找，找到了就返回了


>4、`Parent.__proto__` 是什么？
>```
>ƒ () { [native code] }
>```
>>**`ƒ () { [native code] }`这是个什么鬼？**
>>>让我们套用上面已知的逻辑`__proto__`属性，对应的是该对象的原型，也就是`Function.prototype`。
**这里可能会有疑问为什么是`Function.prototype`？**
因为我们看到的Parent构造函数本质上还是由Function构造函数实例化出来的，`Function.prototype` 就是`ƒ () { [native code] }`，所以在定义Parent的时候将`Function.prototype`的值赋值给了`Parent.__proto__`

>5、`Parent.prototype` 是什么？
>```
>{constructor: ƒ}
>```

>6、`Parent.constructor` 是什么？
>```
>ƒ Function() { [native code] }
>```
>>Parent本真是没有constructor属性的，还是同样的道理沿着原型找到就返回，实际上是`Parent.prototype.constructor`

#### 4、用一张图片表示原型关系
![avatar](https://github.com/FanYuanhe/Javascript/blob/master/images/593627-20151030202435591-481237570.png)
