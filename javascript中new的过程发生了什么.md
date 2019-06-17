### javascript中new的过程发生了什么
>当我们创建一个构造函数实例的时候往往就出现了`new命令`

```
function Parent(){
    this.name = "fanyuanhe";
}

Parent.prototype.getName = function(){
    console.log(this.name);
}

var parent = new Parent();

console.log(parent.name); //fanyuanhe

parent.getName(); //fanyuanhe
```

##### 这个new的过程发生了什么导致出现了上面的结果呢？

>1、new的过程首先创建了一个新的对象
>>`var parent_new = {}`

>2、将新创建的对象（parent_new）的隐式原型（__proto__）指向构造函数（Parent）的原型（prototype）
>>`parent_new.__proto__ = Parent.prototype`

>3、执行（Parent）构造函数，函数中的this指向新建的对象
>>`Parent.call(parent_new)`

>4、若Parent构造函数显示的返回了一个对象，例如
>```
>function Parent(){
>     return { name: 111 } //构造函数直接返回了一个对象
>}
>```
>1、那么`new`操作返回的结果将是这个对象
>2、`否则将会返回初始时候创建的新对象`
