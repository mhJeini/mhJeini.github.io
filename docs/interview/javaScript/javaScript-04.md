# 1. 匿名函数和命名函数有什么区别？
匿名函数通常是某一个事件触发后进行触发的。

命名函数可以进行预先的封装，在需要使用的地方通过调用函数名运行。
```js
var niming = function() { // 赋给变量niming的匿名函数
    // ..
};
var mingming = function bar(){ // 赋给变量mingming的命名函数bar
    // ..
};
niming(); // 实际执行函数
mingming();
```

# 2. JavaScript 中的闭包是什么？举个例子？
闭包是在另一个函数（称为父函数）中定义的函数，并且可以访问在父函数作用域中声明和定义的变量。

闭包可以访问三个作用域中的变量：

- 在自己作用域中声明的变量；
- 在父函数中声明的变量；
- 在全局作用域中声明的变量。

```js
var globalVar = "abc";
// 自调用函数
(function outerFunction (outerArg) { // outerFunction 作用域开始
// 在 outerFunction 函数作用域中声明的变量
var outerFuncVar = 'x';
// 闭包自调用函数
(function innerFunction (innerArg) { // innerFunction 作用域开始
// 在 innerFunction 函数作用域中声明的变量
var innerFuncVar = "y";
console.log(
"outerArg = " + outerArg + "
" +
"outerFuncVar = " + outerFuncVar + "
" +
"innerArg = " + innerArg + "
" +
"innerFuncVar = " + innerFuncVar + "
" +
"globalVar = " + globalVar);
// innerFunction 作用域结束
})(5); // 将 5 作为参数
// outerFunction 作用域结束
})(7); // 将 7 作为参数
```
innerFunction是在outerFunction中定义的闭包，可以访问在outerFunction 作用域内声明和定义的所有变量。除此之外，闭包还可以访问在全局命名空间中声明的变量。

上述代码的输出结果：
```js
outerArg = 7
outerFuncVar = x
innerArg = 5
innerFuncVar = y
globalVar = abc
```
# 3. 如何在 JavaScript 中创建私有变量？
在JavaScript中创建无法被修改的私有变量，需要将其创建为函数中的局部变量。即使这个函数被调用，也无法在函数之外访问这个变量。例如：
```js
function func() {
    var priv = "secret code";
}
console.log(priv); // throws error
```
要访问这个变量，需要创建一个返回私有变量的辅助函数。
```js
function func() {
    var priv = "secret code";
    return function() {
        return priv;
    }
}
var getPriv = func();
console.log(getPriv()); // => secret code
```
# 4. JavaScript 中解释原型设计模式？
原型模式可用于创建新对象，但它创建的不是非初始化的对象，而是使用原型对象（或样本对象）的值进行初始化的对象。原型模式也称为属性模式。

原型模式在初始化业务对象时非常有用，业务对象的值与数据库中的默认值相匹配。原型对象中的默认值被复制到新创建的业务对象中。

经典的编程语言很少使用原型模式，但作为原型语言的JavaScript在构造新对象及其原型时使用了这个模式。

# 5. “this”关键字的原理是什么？请提供一些代码示例。
在JavaScript中，this是指正在执行的函数的“所有者”，或者更确切地说，指将当前函数作为方法的对象。
```js
function foo() {
    console.log( this.bar );
}
var bar = "global";
var obj1 = {
    bar: "obj1",
    foo: foo
};
var obj2 = {
    bar: "obj2"
};
foo(); // "global"
obj1.foo(); // "obj1"
foo.call( obj2 ); // "obj2"
new foo(); // undefined
```
# 6. 如何向 Array 对象添加自定义方法，让下面的代码可以运行？
```js
var arr = [1, 2, 3, 4, 5];
var avg = arr.average();
console.log(avg);
```
JavaScript不是基于类的，但它是基于原型的语言。这意味着每个对象都链接到另一个对象（也就是对象的原型），并继承原型对象的方法。你可以跟踪每个对象的原型链，直到到达没有原型的null对象。我们需要通过修改Array原型来向全局Array对象添加方法。
```js
Array.prototype.average = function() {
    // 计算 sum 的值
    var sum = this.reduce(function(prev, cur) { return prev + cur; });
    // 将 sum 除以元素个数并返回
    return sum / this.length;
}
var arr = [1, 2, 3, 4, 5];
var avg = arr.average();
console.log(avg); // => 3
```
# 7. 什么是 JavaScript 中的提升操作？
提升（hoisting）是JavaScript解释器将所有变量和函数声明移动到当前作用域顶部的操作。

有两种类型的提升：

1）变量提升——非常少见 2）函数提升——常见

无论var（或函数声明）出现在作用域的什么地方，它都属于整个作用域，并且可以在该作用域内的任何地方访问它。
```js
var a = 2;
foo(); // 因为`foo()`声明被"提升"，所以可调用
function foo() {
    a = 3;
    console.log( a ); // 3
    var a; // 声明被"提升"到 foo() 的顶部
}
console.log( a ); // 2
```
# 8. 0.1 + 0.2 === 0.3 输出的结果是什么？
0.1 + 0.2 === 0.3
这段代码的输出是false，这是由浮点数内部表示导致的。

0.1 + 0.2 并不刚好等于0.3，实际结果是 0.30000000000000004。

解决这个问题的一个办法是在对小数进行算术运算时对结果进行舍入。

# 9. 描述一下 Revealing Module Pattern 设计模式？
暴露模块模式（Revealing Module Pattern）是模块模式的一个变体，目的是维护封装性并暴露在对象中返回的某些变量和方法。如下所示：
```js
var Exposer = (function() {
    var privateVariable = 10;
    var privateMethod = function() {
        console.log('Inside a private method!');
        privateVariable++;
    }
    var methodToExpose = function() {
        console.log('This is a method I want to expose!');
    }
    var otherMethodIWantToExpose = function() {
        privateMethod();
    }
    return {
        first: methodToExpose,
        second: otherMethodIWantToExpose
    };
})();
Exposer.first(); // 输出: This is a method I want to expose!
Exposer.second(); // 输出: Inside a private method!
Exposer.methodToExpose; // undefined
```
它的一个明显的缺点是无法引用私有方法。