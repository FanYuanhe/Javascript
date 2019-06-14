### javascript中函数的继承
#### 1、构造函数
```
function Parent(name = "fanyuanhe") {
    this.name = name; 基本属性
    this.arr = [1,2,3], 引用属性
    this.func = function(){ console.log(this.name) }
}

Parent.prototype.sayName = function(){ console.log(this.name) }
```

#### 2、各种继承方式及优缺点
>下面提到的继承方法中的相关实例，均继承自内容一部分的Parent构造函数，请先认真理解Parent。
##### 原型链继承
>核心：
>1、将父类构造函数的实例作为子类的原型

>优点：
>1、子类可以继承父类原型上定义的方法

>缺点：
>1、当创建子类构造函数实例的时候，不能向父类构造函数传递参数。
>2、所有的子类实例都共享了父类的引用属性，如arr等。
```
function Child() {}
child.prototype = new Parent(); //实现原型继承

let child1 = new Child(); //不能向父类构造函数传参数
console.log(child1.arr) //[1,2,3]

child1.arr.push(4);
console.log(child1.arr) //[1,2,3,4]

let child2 = new Child();
console.log(child2.arr) //[1,2,3,4] //child2共享了child1修改后的arr，也就是Parent构造函数的arr

child2.sayName() //"fanyuanhe" //可以调用父类原型上的方法
```

>`console.log(child1.constructor);` 这时我们发现child1.constructor的值是Parent,正常情况下我们希望它应该是Child
>可以使用 `Child.prototype.constructor = Child` 进行修改

##### 借用构造函数继承
>核心：
>1、通俗的讲就是在子类构造函数中执行父类构造函数，以便于达到创建子类实例的时候，子类实例可以继承父类的方法

>优点：
>1、达到实例化子类的时候向父类传递参数的目的。
>2、避免了子类实例共享父类的引用属性的情况。

>缺点：
>1、父类的方法不能复用，每次都是重新生成的，child1.func === child2.func //false
>2、子类实例不能调用父类构造函数原型上的方法

```
function Child (name){
    Parent.call(this, name); //实现借用构造函数继承
}

let child1 = new Child("哈哈哈");
console.log(child1.name) //“哈哈哈”

child1.arr.push(4);
console.log(child1.arr) //[1,2,3,4]

let child2 = new Child("嘿嘿嘿"); // 实现创建子类实例的时候向父类传递参数
console.log(child2.arr) //[1,2,3] 并不是所有的子类都共享了父类的应用属性

child1.func === child2.func //false 父类的方法不能复用，因为每次父类都是重新执行生成新的

child2.sayName() //报错 child2.sayName is not a function 子类不能调用父类原型上的方法
```

##### 组合式继承
>核心：
>1、使用原型链继承实现父类原型上的方法共享给子类实例
>2、使用就用构造函数继承的方法实现向父类传递参数

>优点：
>1、父类构造函数原型上的方法可以共享个每个子类的实例
>2、创建子类实例的时候可以向父类传递参数
>3、不共享父类的引用属性，如arr等

>缺点：
>1、在实现过程中调用了两次父类的构造函数，会存在一份多余的父类实例属性

```
function Child(name){
    Parent.call(this, name); //借用构造函数继承，第一次调用父类
}

Child.prototype = new Parent(); //原型链继承， 第二次调用父类

let child1 = new Child("哈哈哈");
child1.sayName() //“哈哈哈”
child1.arr.push(4);
child1.arr //[1,2,3,4]

let child2 = new Child("嘿嘿嘿");
child2.arr //[1,2,3]
```
>`console.log(child1.constructor);` 这时我们发现child1.constructor的值是Parent,正常情况下我们希望它应该是Child
>可以使用 `Child.prototype.constructor = Child` 进行修改

##### 原型式继承(为下一步实现寄生组合继承做铺垫)

>ES5中新增了`Object.create()`方法规范化了原型式继承，实际上就是对传入create方法中的对象实现了浅复制

>`Object.create()`方法接受两个参数，一个是新生成对象的原型对象，另一个是新生成对象的额外属性的对象

>`Object.create()`的原理

```
function create(o) {
    function F(){}
    F.prototype = o;
    return new F();
}
```

##### 寄生组合式继承
>完美的继承解决方式

>解决了组合式继承父类调用两次的缺点，保留了以上继承方式的所有优点

```
function Child(){
    Parent.call(this); //借用构造函数继承保持不变
}

Child.prototype = Object.create(Parent.prototype); //使用Obejct.create方法复制Parent构造函数的原型，并赋值给Child的原型，实现原型方法共享，减少了组合式继承在这里第二次调用父类的缺点。

let child1 = new Child("哈哈哈");
child1.sayName() //哈哈哈
child1.constructor //Parent

Child.prototype.constructor = Child; //修改constructor指向

child1.constructor //Child

```

