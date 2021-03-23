# IIFE

## 定义

> IIFE: Immediately Invoked Function Expression，意为立即调用的函数表达式，也就是说，声明函数的同时立即调用这个函数。

​		对比一下，这是不采用IIFE时的函数声明和函数调用：

```javascript
function foo(){
  var a = 10;
  console.log(a);
}
foo();
```

​		下面是IIFE形式的函数调用：

```javascript
(function foo(){
  var a = 10;
  console.log(a);
})();
```

​		函数的声明和IIFE的区别在于，在函数的声明中，我们首先看到的是function关键字，而IIFE我们首先看到的是左边的“（”。也就是说，使用一对"（）"将函数的声明括起来，使得JS编译器不再认为这是一个函数声明，而是一个IIFE，即需要立刻执行声明的函数。两者达到的目的是相同的，都是声明了一个函数foo并且随后调用函数foo。


## 为什么需要IIFE

​		如果只是为了立即执行一个函数，显然IIFE所带来的好处有限。实际上，<span style="color:orange">IIFE的出现是为了弥补JS在scope方面的缺陷</span>：JS只有全局作用域（global scope）、函数作用域（function scope），从ES6开始才有块级作用域（block scope）。对比现在流行的其他面向对象的语言可以看出，JS在访问控制这方面是多么的脆弱！那么如何实现作用域的隔离呢？<span style="color:orange">在JS中，只有function，只有function，只有function才能实现作用域隔离</span>，因此如果要将一段代码中的变量、函数等的定义隔离出来，只能将这段代码封装到一个函数中。
​		在我们通常的理解中，将代码封装到函数中的目的是为了复用。在JS中，当然声明函数的目的在大多数情况下也是为了复用，但是JS迫于作用域控制手段的贫乏，<span style="color:orange">我们也经常看到只使用一次的函数：这通常的目的是为了隔离作用域了</span>！既然只使用一次，那么立即执行好了！既然只使用一次，函数的名字也省掉了！这就是IIFE的由来。


## IIFE的常见形式

​		根据最后表示函数执行的一对（）位置的不同，常见的IIFE写法有两种，示例如下：

- 列表1:IIFE写法一

```javascript
(function foo(){
  var a = 10;
  console.log(a);
})();
```

- 列表2:IIFE写法二

```javascript
(function foo(){
  var a = 10;
  console.log(a);
}());
```

​		这两种写法效果完全一样，使用哪种写法取决于你的风格，貌似第一种写法比较常见。其实，IIFE不限于（）的表现形式[1]，但是还是遵守约定俗成的习惯比较好。


## IIFE的函数名和参数

​		根据《You Don’t Know JS:Scope & Clouses》[2]的说法，尽量避免使用匿名函数（没有函数名就是匿名函数）。但是IIFE确实只执行一次，给IIFE起个名字有些画蛇添足了。如果非要给IIFE起个名字，干脆就叫IIFE好了。

​		IIFE可以带（多个）参数，比如下面的形式：

```javascript
var a = 2;
(function IIFE(global){
    var a = 3;
    console.log(a); // 3
    console.log(global.a); // 2
})(window);

console.log(a); // 2
```


## IIFE构造单例模式

JS的模块就是函数，最常见的模块定义如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```javascript
function myModule(){
  var someThing = "123";
  var otherThing = [1,2,3];

  function doSomeThing(){
    console.log(someThing);
  }

  function doOtherThing(){
    console.log(otherThing);
  }

  return {
    doSomeThing:doSomeThing,
    doOtherThing:doOtherThing
  }
}

var foo = myModule();
foo.doSomeThing();
foo.doOtherThing();

var foo1 = myModule();
foo1.doSomeThing();
```


如果需要一个单例模式的模块，那么可以利用IIFE：

```javascript
var myModule = (function module(){
  var someThing = "123";
  var otherThing = [1,2,3];

  function doSomeThing(){
    console.log(someThing);
  }

  function doOtherThing(){
    console.log(otherThing);
  }

  return {
    doSomeThing:doSomeThing,
    doOtherThing:doOtherThing
  }
})();

myModule.doSomeThing();
myModule.doOtherThing();
```

 

## 小结

IIFE的目的是为了隔离作用域，防止污染全局命名空间。
ES6以后也许有更好的访问控制手段（模块？类？），有待研究。