# 1. JavaScript 中 void(0) 如何使用？
void(0)用于防止页面刷新，并在调用时传递参数“zero”。

void(0)用于调用另一种方法而不刷新页面。

# 2. escape 字符是用来做什么的？
escape方法返回一个包含charstring内容的字符串值（Unicode格式）。所有空格、标点、重音符号以及其他非ASCII字符都用%xx 编码代替，其中xx等于表示该字符的十六进制数。例如，空格返回的是"%20" 。

字符值大于255的以%uxxxx格式存储。

# 3. 什么是 JavaScript Cookie？
Cookie是用来存储计算机中的小型测试文件，当用户访问网站以存储他们需要的信息时，它将被创建。

# 4. 解释 JavaScript 中的 pop() 方法？
pop()方法与shift()方法类似，但不同之处在于Shift方法在数组的开头工作。此外，pop()方法将最后一个元素从给定的数组中取出并返回。然后改变被调用的数组。

例：
```js
var cloths = ["Shirt", "Pant", "TShirt"];
cloths.pop();
//Now cloth becomes Shirt,Pant
```
# 5. JavaScript 中使用 innerHTML 的缺点是什么？
如果不重新解析整个innerHTML，就没有附加支持。这使得直接更改innerHTML非常慢。

例如，要附加到html标签，需要执行以下操作：
```js
let myDiv = document.querySelector('#myDiv')
//重新解析整个myDiv标签。
myDiv.innerHTML += '<p>Added new tag</p>'
```
innerHTML不提供验证，因此我们可以潜在地在文档中插入有效和损坏的HTML并将其破坏。

# 6. JavaScript 中 break 和 continue 语句的作用？
break语句从当前循环中退出。

continue语句继续下一个循环语句。

# 7. JavaScript 中 dataypes 的两个基本组是什么？
Primitive

Reference types

原始类型是数字和布尔数据类型。引用类型是更复杂的类型，如字符串和日期。

# 8. JavaScript 中如何创建通用对象？
通用对象可以创建为：
```js
var obj = new object();
```
# 9. JavaScript 中哪些关键字用于处理异常？
try... catch-finally用于处理JavaScript中的异常。

# 10. JavaScript 中不同类型的错误有几种？
三种类型的错误：

- Load time errors：该错误发生于加载网页时，例如出现语法错误等状况，称为加载时间错误，并且会动态生成错误。

- Run time errors：由于在HTML语言中滥用命令而导致的错误。

- Logical Errors：这是由于在具有不同操作的函数上执行了错误逻辑而发生的错误。

# 11. JavaScript 中使用的 push 方法是什么？
push方法用于将一个或多个元素添加或附加到数组的末尾。使用这种方法，可以通过传递多个参数来附加多个元素。

# 12. 什么是 JavaScript 中的 unshift 方法？
unshift方法就像在数组开头工作的push方法。该方法用于将一个或多个元素添加到数组的开头。

# 13. JavaScript 中获取 CheckBox 状态的方式是什么？
```js
alert(document.getElementById('checkbox1').checked);
```
如果CheckBox被检查，此警报将返回TRUE。

# 14. 解释 window.onload 和 onDocumentReady？
在载入页面的所有信息之前，不运行onload函数。这导致在执行任何代码之前会出现延迟。

onDocumentReady在加载DOM之后加载代码。这允许早期的代码操纵。

# 15. JavaScript 中 .call() 和.apply() 之间有什么区别？
函数.call()和.apply()在使用上非常相似，只是有一点区别。当程序员知道函数参数的编号时，使用.call()，因为它们必须在调用语句中被提及为参数。另一方面，当不知道数字时使用.apply(),函数.apply()期望参数为数组。

.call()和.apply()之间的基本区别在于将参数传递给函数。它们的用法可以通过给定的例子进行说明。

# 16. 什么样的布尔运算符可以在 JavaScript 中使用？
“And”运算符（&&），'Or'运算符（||）和'Not'运算符（！）可以在JavaScript中使用。

*运算符没有括号。

# 17. 一个特定的框架如何使用 JavaScript 中的超链接定位？
可以通过使用“target”属性在超链接中包含所需帧的名称来实现。
```html
<a href="newpage.htm" target="newframe">New Page</a>
```
# 18. web-garden 和 web-farm 之间有何不同？
web-garden和web-farm都是网络托管系统。

唯一的区别是web-garden是在单个服务器中包含许多处理器的设置，而web-farm是使用多个服务器的较大设置。

# 19. JavaScript 中读取和写入文件的方法是什么？
可以通过使用JavaScript扩展（从JavaScript编辑器运行），打开文件的示例来完成：
```js
fh = fopen(getScriptPath(), 0);
```
# 20. JavaScript 中如何使用 DOM？
DOM代表文档对象模型，并且负责文档中各种对象的相互交互。DOM是开发网页所必需的，其中包括诸如段落，链接等对象。可以操作这些对象以包括添加或删除等操作，DOM还需要向网页添加额外的功能。除此之外，API的使用比其他更有优势。