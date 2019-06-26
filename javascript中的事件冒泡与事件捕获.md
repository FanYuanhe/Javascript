### javascript中的事件冒泡与事件捕获
#### 首先来看一个html文件的结构
```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <div id="open-event"></div>
</body>
</html>
```
##### 什么是事件冒泡？
>事件冒泡是`IE`提出的一种事件流模式，这种模式指出，当事件发生的时候，`应该是由最为具体的元素逐级向上传递到较为不具体的元素`
>在上面例子中可以理解为，首先是`<div id="open-event"></div>`这个最为具体的元素接收到事件

```
<div id="open-event"></div>

<body></body>

<html></html>

document对象
```

##### 什么是事件捕获？
>事件捕获是由`Netscape`公司提出的事件流模式，这种模式指出，当事件发生的时候，`应该是由较为不具体的节点接受到，最为具体的节点最后接受到这个事件`

```
document对象

<html></html>

<body></body>

<div id="open-event"></div>
```

### w3c制定的DOM事件级别
##### 1、DOM0级别事件
>`针对某个特定元素为它添加点击事件，一般事件为on开头的`，这个时候事件的处理程序是在当前元素的作用域下面的，所以this指向了当前元素

```
var openEvent = document.getElementById("open-event");
openEvent.onclick = function(){
    console.log(this)
}
```
>`也可以删除DOM0级别绑定的事件处理程序`

```
openEvent.onclick = null
```

##### 2、DOM2级别的事件处理程序
>**DOM2级别事件，规定事件发生分为三个阶段**
>>1、`事件捕获阶段`
>>2、`处于目标元素阶段`
>>3、`事件冒泡阶段`

>**提供了两个方法分别事件绑定以及事件移除**
>一、`addEventListener()`, 该方法用来绑定事件处理程序，接受三个参数。
>>1、一个字符串，所绑定事件的字符串，如`click`
>>2、一个函数，绑定事件发生的时候的操作,可接受一个参数，代表具体发生事件的元素
>>3、可选参数，是一个布尔值类型（`true/false`）,默认为false，代表绑定的事件发生在事件冒泡阶段，当设置为true的时候，代表事件发生在事件捕获的阶段，一般都设置为false，因为这这样可以最最大限度的兼容各个浏览器
>>```
>>var openEvent = document.getElementById("open-event");
>>openEvent.addEventListener("click", function(e){
>>    console.log(e)
>>}, false(默认的))
>>```
>>
>二、`removeEventListener()`该方法用来移除DOM2级别绑定的事件处理程序,接受三个参数，与`addEventListener`方法相同
>>`第二个参数需要注意`
>>>若当初绑定DOM2级别事件处理程序的时候，第二个参数使用的是`匿名函数`,如上面例子
>>>那么`在移除事件绑定程序的时候，也使用匿名函数是不起作用的`
>>>```
>>>openEvent.removeEventListener("click", function(e){ //匿名函数用作移除事件处理程序不起作用
>>>    console.log(e)
>>>})
>>>```
>>>###### 那我应该怎么处理呢？
>>>你可以像下面这样来处理，将事件发生时所执行的函数提出来
>>>`绑定DOM2级别事件`
>>>```
>>>function handler(e){ console.log(e) }
>>>openEvent.addEventListener("click", handler, true);
>>>```
>>>`移除DOM2级别事件`
>>>```
>>>openEvent.removeEventListener("click", handler, true);
>>>```

### IE制定的事件处理程序
>IE公司实现了类似于DOM的事件绑定机制，同样也提供了两种方法，分别绑定和移除事件
>`attachEvent` 用于绑定，`detachEvent`用于移除

>`由于IE8及其更早版本只支持事件冒泡，以用`attachEvent`绑定的事件`只发生在事件冒泡阶段`


>`attachEvent方法`，接受两个参数一个
>1、一个字符串，所绑定的事件，与DOM2同点在于是，以`on`开头的字符串，例如`onclick`
>2、一个事件处理函数

>##### attachEvent事件绑定与DOM0级别事件绑定的不同之处。
>**this指向不同**
>>`DOM0级别中事件处理函数的this指向当前元素，因为是在当前元素的作用域中执行`
>>`IE事件规则中，this指向window，这点要尤为注意`
>
>**事件执行顺序不同**
>>```
>>var openEvent = document.getElementById("open-event");
>>openEvent.attachEvent("onclick", function(){
>>    console.log(1)
>>})
>>openEvent.attachEvent("onclick", function(){
>>    console.log(2)
>>})
>>```
>>`先执行打印出2，后执行打印出1`

### 事件冒泡及事件捕获有啥用呢
>我们在实际开发过程中常会遇到给一个列表中的每一个元素都添加相同的点击事件

>原始方法，循环所有的元素，添加点击事件

>这种方法是给，每个元素都添加了点击事件，当元素过多的时候会占用过多的内存，导致页面性能下降
```
<div id="open-event">
<ul>
  <li>1</li>
  <li>2</li>
  <li>3</li>
</ul>
</div>
<script>
let lis = document.getElementsByTagName("li");
for(let i=0; i<lis.length; i++){
  lis[i].onclick = function(e){
    console.log(e)
  }
}
</script>
```

>`DOM2级别事件委托添加事件（推荐）`

>利用事件冒泡的原理，我们找到某个外层的具体元素，为它添加一个事件，这时候当我们点击列表中的某个特定元素时，它们的事件会出现冒泡，被外层的父元素接受到并处理

>`只为一个元素添加了点击事件，利用了事件冒泡，将子元素的事件委托给当前元素处理，提高了页面性能`
```
let openEvent = document.getElementById("open-event");
openEvent.addEventListener("click", function(e){
  console.log(e);
})
```

### 怎么阻止事件冒泡
##### 为什么要阻止呢？
```
let openEvent = document.getElementById("open-event");
let d1 = document.getElementById("d1");
let d2 = document.getElementById("d2");
openEvent.onclick = function(){
  console.log('openEvent')
}
d2.onclick = function(){
  console.log('d2')
}
d1.onclick = function(){
  console.log('d1')
}
```

>如上面的例子我们为三个元素分别添加了DOM0级别的点击事件

>我的本意是点击`d1元素`的时候弹出`d1`

>但我发现并不是这样的，打印效果如下
>```
>d1
>d2
>openEvent
>```
>这是咋回事呢？
>`这个就是事件冒泡原理在作怪，我三个元素是互相嵌套的，这个时候当我点击d1的时候。`
>1、`它本身的事件发生了`
>2、`它还会向上继续冒泡，所以包裹他的元素上面绑定的事件也发生了`

##### 怎么阻止呢？
###### **阻止事件冒泡**
>非IE情况下`e.stopPropagation()`方法
>IE情况下`window.event.cancelBubble = true`
```
openEvent.onclick = function(){
  console.log('openEvent')
}
d2.onclick = function(e){
  console.log('d2')
}
d1.onclick = function(e){
  if(e.stopPropagation){
    e.stopPropagation() 
  } else {
    window.event.cancelBubble = true
  }
  console.log('d1')
}
```

###### **阻止元素的默认行为**
>非IE情况下`e.preventDefault()`方法，
>IE情况下`window.event.returnValue = true`
>>**默认行为指的是什么？**
>>`a`标签默认就是会跳转到`href`中的网址，这个就是默认行为。
```
<a id="a-jump" href="https://github.com/FanYuanhe/Javascript"></a>

var aJump = document.getElementById("a-jump");
aJump.onclick = function(e){
    if (e.preventDefault) {
        e.preventDefault()
    } else {
        window.event.returnValue = true;
    }
}
```
`阻止了a标签的跳转`
