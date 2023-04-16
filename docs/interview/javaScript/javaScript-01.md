# 1. 什么是 JavaScript？
JavaScript（简称“JS”）是一种具有函数优先的轻量级，解释型或即时编译型的编程语言。虽然它是作为开发Web页面的脚本语言而出名，但是它也被用到了很多非浏览器环境中，JavaScript基于原型编程、多范式的动态脚本语言，并且支持面向对象、命令式、声明式、函数式编程范式。

JavaScript是客户端和服务器端脚本语言，可以插入到HTML页面中，并且是目前较热门的Web开发语言。同时，JavaScript也是面向对象编程语言。

# 2. 列举 Java 和 JavaScript 之间的区别？
Java是一门十分完整、成熟的编程语言。相比之下，JavaScript是一个可以被引入HTML页面的编程语言。这两种语言并不完全相互依赖，而是针对不同的意图而设计的。

Java是一种面向对象编程（OOPS）或结构化编程语言，类似的如C ++或C，而JavaScript是客户端脚本语言，它被称为非结构化编程。

# 3. JavaScript 和 ASP 脚本相比，哪个更快？
JavaScript更快。

JavaScript是一种客户端语言，因此它不需要Web服务器的协助来执行。另一方面，ASP是服务器端语言，因此总是比JavaScript慢。值得注意的是，Javascript现在也可用于服务器端语言（nodejs）。

# 4. 什么是负无穷大？
负无穷大是JavaScript中的一个数字，可以通过将负数除以零来得到。

# 5. 如何将 JavaScript 代码分解成几行吗？
在字符串语句中可以通过在第一行末尾使用反斜杠“\”来完成

例：
```js
document.write("This is \a program");
```

如果不是在字符串语句中更改为新行，那么javaScript会忽略行中的断点。

例：
```js
var x=1, y=2,
z=
x+y;
```
上面的代码是完美的，但并不建议这样做，因为阻碍了调试。

# 6. 什么是未声明和未定义的变量？
未声明的变量是程序中不存在且未声明的变量。如果程序尝试读取未声明变量的值，则会遇到运行时错误。

未定义的变量是在程序中声明但尚未给出任何值的变量。如果程序尝试读取未定义变量的值，则返回未定义的值。

# 7. 什么是全局变量？这些变量如何声明，使用全局变量有哪些问题？
全局变量是整个代码长度可用的变量，也就是说这些变量没有任何作用域。var关键字用于声明局部变量或对象。如果省略var关键字，则声明一个全局变量。

例：
```js
Declare a global globalVariable = "Test";
```
使用全局变量所面临的问题是本地和全局变量名称的冲突。此外，很难调试和测试依赖于全局变量的代码。

# 8. 解释 JavaScript 中定时器的工作？如果有，也可以说明使用定时器的缺点？
定时器用于在设定的时间执行一段代码，或者在给定的时间间隔内重复该代码。这通过使用函数setTimeout，setInterval和clearInterval来完成。

1、setTimeout(function，delay) 函数用于启动在所述延迟之后调用特定功能的定时器。

2、setInterval(function，delay) 函数用于在提到的延迟中重复执行给定的功能，只有在取消时才停止。

3、clearInterval(id) 函数指示定时器停止。

定时器在一个线程内运行，因此事件可能需要排队等待执行。

# 9. ViewState 和 SessionState 有什么区别？
ViewState特定于会话中的页面。

SessionState特定于可在Web应用程序中的所有页面上访问的用户特定数据。

# 10. 什么是 === 运算符？
===被称为严格等式运算符，当两个操作数具有相同的值而没有任何类型转换时，该运算符返回true。

# 11. 如何使用 JavaScript 提交表单？
要使用JavaScript提交表单，请使用
```js
document.form [0].submit();
```
# 12. 如何改变元素的样式或者类？
可以通过以下方式完成：
```js
document.getElementById("myText").style.fontSize = "20px";
```
或者
```js
document.getElementById(“myText”).className = "anyclass";
```
# 13. JavaScript 中的循环结构都有什么？
For、While、do-while loops

# 14. 如何在 JavaScript 中将 base 字符串转换为 integer？
parseInt() 函数解析一个字符串参数，并返回一个指定基数的整数。

parseInt()将要转换的字符串作为其第一个参数，第二个参数是给定字符串的基础。

为了将4F（基数16）转换为整数，所使用的代码是：parseInt("4F", 16);

# 15. 说一说 == 和 === 之间有什么区别？
“==”仅检查值相等，而“===”是一个更严格的等式判定，如果两个变量的值或类型不同，则返回false。

# 16. 3 + 2 +"7" 的结果是什么？
由于3和2是整数，它们将直接相加。由于7是一个字符串，它将会被直接连接，所以结果将是57。

# 17. 如何检测客户端机器上的操作系统？
为了检测客户端机器上的操作系统，应使用navigator.appVersion字符串（属性）。

# 18. JavaScript 中的 NULL 是什么意思？
NULL用于表示无值或无对象。它意味着没有对象或空字符串，没有有效的布尔值，没有数值和数组对象。

# 19. delete 操作符的功能是什么？
delete操作符用于删除程序中的所有变量或对象，但不能删除使用VAR关键字声明的变量。

# 20. JavaScript 中有哪些类型的弹出框？
Alert、Confirm、Prompt