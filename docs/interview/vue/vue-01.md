# 1. 什么是 Vue？
Vue（读音 /vjuː/，类似于view）是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，Vue被设计为可以自底向上逐层应用。

Vue的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。

另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue也完全能够为复杂的单页应用提供驱动。

# 2. 什么是 MVVM？
MVVM是Model-View-ViewModel的缩写。MVVM是一种设计思想。Model层代表数据模型，也可以在Model中定义数据修改和操作的业务逻辑；View代表UI组件，它负责将数据模型转化成UI展现出来，ViewModel是一个同步View和Model的对象。

在MVVM架构下，View和Model之间并没有直接的联系，而是通过ViewModel进行交互，Model和ViewModel之间的交互是双向的，因此View数据的变化会同步到Model中，而Model数据的变化也会立即反应到View上。

ViewModel通过双向数据绑定把View层和Model层连接起来，而View和Model之间的同步工作完全是自动的，无需人为干涉，因此开发者只需关注业务逻辑，不需要手动操作DOM,不需要关注数据状态的同步问题，复杂的数据状态维护完全由MVVM来统一管理。

# 3. Vue 中 v-show 和 v-if 有什么区别？
v-if和v-show两个都是让元素不可见。

1、v-if在条件切换时，会对标签进行适当的创建和销毁，而v-show则仅在初始化时加载一次，因此v-if的开销比v-show大；

2、v-show控制的时元素的display属性，无论初始条件是否成立，都会渲染标签，而v-if是惰性的，只有在条件成立时才渲染为真实的标签，条件为假，不会去渲染标签。

# 4. Vue 中 v-show 和 v-if 应用场景？
v-if：此元素进入页面后，此元素只会显示或隐藏不会被再次改变显示状态，此时用v-if更加合适，如请求后台接口通过后台数据控制某块内容是否显示或隐藏，且这个数据在当前页不会被修改。

作用：当用v-if来隐藏元素时，初次加载时就不用渲染此dom节点，提升页面加载速度。

v-show：此元素进入页面后，此元素会频繁的改变显示状态，此时用v-show更加合适，如页面中有一个toggle按钮,点击按钮来控制某块区域的显示隐藏。

作用：当用v-show来隐藏元素时，只会在初次加载时渲染此dom节点，之后都是通用display来控制显隐，如果此时使用v-if，那会频繁的操作dom，会极大的影响性能，但用display则不会。

# 5. Vue 中 Class 与 Style 如何动态绑定？
Class 可以通过对象语法和数组语法进行动态绑定：

对象语法：
```html
<div v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>
```
数组语法：
```html
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```
Style 也可以通过对象语法和数组语法进行动态绑定：

对象语法：
```html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
数组语法：
```html
<div v-bind:style="[styleColor, styleSize]"></div>
```
# 6. 说说你对 SPA 单页面理解，它有什么优缺点？
SPA（single-page application）仅在Web页面初始化时加载相应的HTML、JavaScript和CSS。一旦页面加载完成，SPA 不会因为用户的操作而进行页面的重新加载或跳转；取而代之的是利用路由机制实现HTML内容的变换，UI与用户的交互，避免页面的重新加载。

优点：

用户体验好、快，内容的改变不需要重新加载整个页面，避免了不必要的跳转和重复渲染；

基于上面一点，SPA相对对服务器压力小；

前后端职责分离，架构清晰，前端进行交互逻辑，后端负责数据处理；

缺点：

初次加载耗时多：为实现单页Web应用功能及显示效果，需要在加载页面的时候将JavaScript、CSS 统一加载，部分页面按需加载；

前进后退路由管理：由于单页应用在一个页面中显示所有的内容，所以不能使用浏览器的前进后退功能，所有的页面切换需要自己建立堆栈管理；

SEO难度较大：由于所有的内容都在一个页面中动态替换显示，所以在SEO上其有着天然的弱势。

# 7. Vue 中 computed 和 watch 有什么区别和运用场景？
computed： 是计算属性，依赖其它属性值，并且computed的值有缓存，只有它依赖的属性值发生改变，下一次获取computed的值时才会重新计算computed的值；

watch： 更多的是「观察」的作用，类似于某些数据的监听回调 ，每当监听的数据变化时都会执行回调进行后续操作；

运用场景：

当需要进行数值计算，并且依赖于其它数据时，应该使用computed，因为可以利用computed的缓存特性，避免每次获取值时，都要重新计算；

当需要在数据变化时执行异步或开销较大的操作时，应该使用watch，使用watch选项允许执行异步操作( 访问一个API)，限制我们执行该操作的频率，并在得到最终结果前，设置中间状态。这些都是计算属性无法做到的。

# 8. 列举出 3 个 Vue 中常用的生命周期钩子函数？
created： 实例创建完成之后调用，在这一步，实例已经完成数据观测、 属性和方法的运算、watch/event事件回调。然而，挂载阶段还没有开始，$el属性目前还不可见。

mounted： el被新创建的vm.el替换，并挂载到实例上去之后调用该钩子。如果root实例挂载了一个文档内元素，当mounted被调用时vm.el替换，并挂载到实例上去之后调用该钩子。如果root实例挂载了一个文档内元素，当mounted被调用时vm.el替换，并挂载到实例上去之后调用该钩子。如果root实例挂载了一个文档内元素，当mounted被调用时vm.el也在文档内。

activated： keep-alive组件激活时调用。

# 9. Vue 中 active-class 是哪个组件的属性？
vue-router模块的router-link组件。

# 10. Vue 中 vue-router 组件有哪些？
路由声明式跳转，active-class是标签被点击时的样式：
```html
<router-link :to='' class='active-class'>
```
渲染路由的容器：
```html
<router-view>
```
缓存组件：
```html
<keep-alive>
```
# 11. Vue 中生命周期是什么？各生命周期的作用？
生命周期是指Vue实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模版、挂载 Dom -> 渲染、更新 -> 渲染、卸载等一系列过程，称这是Vue的生命周期。

各个生命周期的作用

| 生命周期	| 描述|
| ------------------	| ------------------|
| beforeCreate	| 组件实例被创建之初，组件的属性生效之前|
| created	| 组件实例已经完全创建，属性也绑定，但真实dom还没有生成，$el还不可用|
| beforeMount	| 在挂载开始之前被调用：相关的render函数首次被调用|
| mounted	| el被新创建的vm.$el替换，并挂载到实例上去之后调用该钩子|
| beforeUpdate	| 组件数据更新之前调用，发生在虚拟DOM打补丁之前|
| update	| 组件数据更新之后|
| activited | 	keep-alive专属，组件被激活时调用|
| deactivated	| keep-alive专属，组件被销毁时调用|
| beforeDestory| 	组件销毁前调用|
| destoryed	| 组件销毁后调用|
# 12. Vue 中父组件和子组件生命周期钩子函数执行顺序？
Vue的父组件和子组件生命周期钩子函数执行顺序可以归类为以下4部分：

- 加载渲染过程

    父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mounted

- 子组件更新过程

    父beforeUpdate->子beforeUpdate->子updated->父updated

- 父组件更新过程

    父beforeUpdate->父updated

- 销毁过程

    父beforeDestroy->子beforeDestroy->子destroyed->父destroyed

# 13. Vue 中 created 和 mounted 有什么区别？
在created阶段，实例已经被初始化，但是还没有挂载至$el上，所以无法获取到对应的节点，但是此时可以获取到vue中data与methods中的数据；而在mounted阶段，vue的template成功挂载在$el中，此时一个完整的页面已经能够显示在浏览器中，所以在这个阶段，可以调用节点。

以下为测试vue部分生命函数，便于理解
```html
beforeCreate(){  //创建前
    console.log('beforecreate:',document.getElementById('first'))//null
    console.log('data:',this.text);//undefined
    this.sayHello();//error:not a function
},
created(){  //创建后
    console.log('create:',document.getElementById('first'))//null
    console.log('data:',this.text);//this.text
    this.sayHello();//this.sayHello()
},
beforeMount(){ //挂载前
    console.log('beforeMount:',document.getElementById('first'))//null
    console.log('data:',this.text);//this.text
    this.sayHello();//this.sayHello()
},
mounted(){  //挂载后
    console.log('mounted:',document.getElementById('first'))//<p></p>
    console.log('data:',this.text);//this.text
    this.sayHello();//this.sayHello()
}
```
# 14. Vue 中 computed 和 method 有什么区别？
**computed和method相同点：**

如果作为模板的数据显示，二者能实现响应的功能，唯一不同的是methods定义的方法需要执行。

**computed和method不同点：**

1、computed 会基于响应数据缓存，methods不会缓存；

2、diff之前先看data里的数据是否发生变化，如果没有变化computed的方法不会执行，但methods里的方法会执行；

3、computed是属性调用，而methods是函数调用。

# 15. Vue 中虚拟 DOM 的 key 有什么作用？
简单的说：key是虚拟DOM对象的标识，在更新显示时key起着极其重要的作用。

复杂的说：当状态中的数据发生了变化时，react会根据【新数据】生成【新的虚拟DOM】，随后React进行【新虚拟DOM】与【旧虚拟DOM】的diff比较，比较规则如下：

旧虚拟DOM中找到了与新虚拟DOM相同的key。
1）若虚拟DOM中的内容没有变，直接使用之前的真是DOM；

2）若虚拟DOM中内容变了，则生成新的真实DOM，随后替换掉页面中之前的真实DOM。

旧虚拟DOM中未找到与新虚拟DOM相同的key。
1）根据数据创建新的真实DOM，随后渲染到页面。

# 16. Vue 中异步更新机制是如何实现的？
Vue的异步更新机制的核心是利用了浏览器的异步任务队列来实现的，首选微任务队列，宏任务队列次之。

当响应式数据更新后，会调用dep.notify方法，通知dep中收集的watcher去执行update方法，watcher.update将watcher自己放入一个watcher队列（全局的queue数组）。

然后通过nextTick方法将一个刷新watcher队列的方法（flushSchedulerQueue）放入一个全局的callbacks数组中。

如果此时浏览器的异步任务队列中没有一个叫flushCallbacks的函数，则执行timerFunc函数，将flushCallbacks函数放入异步任务队列。如果异步任务队列中已经存在flushCallbacks函数，等待其执行完成以后再放入下一个flushCallbacks函数。

flushCallbacks函数负责执行callbacks数组中的所有flushSchedulerQueue函数。

flushSchedulerQueue函数负责刷新watcher队列，即执行queue数组中每一个watcher的run方法，从而进入更新阶段，比如执行组件更新函数或者执行用户watch的回调函数。

# 17. Vue 中常用指令都有哪些？
vue常用指令有：v-once指令、v-show指令、v-if指令、v-else指令、v-else-if指令、v-for指令、v-html指令、v-text指令、v-bind指令、v-on指令、v-model指令等等。

1、v-model指令：用于表单输入，实现表单控件和数据的双向绑定。

2、v-on：简写为@，基础事件绑定。

3、v-bind：简写为:，动态绑定一些元素的属性，类型可以是：字符串、对象或数组。

4、v-if指令：取值为true/false，控制元素是否需要被渲染。

5、v-else指令：和v-if指令搭配使用，没有对应的值。当v-if的值false，v-else才会被渲染出来。

6、v-show指令：指令的取值为true/false，分别对应着显示/隐藏。

7、v-for指令：遍历data中存放的数组数据，实现列表的渲染。

8、v-once：通过使用v-once指令，也能执行一次性地插值，当数据改变时，插值处的内容不会更新。

# 18. Vue 中组件通信有哪些方式？
1、父传子：props

父组件通过props向下传递数据给子组件。注：组件中的数据共有三种形式：data、props、computed

2、父传子孙：provide和inject

父组件定义provide方法return需要分享给子孙组件的属性，子孙组件使用inject选项来接收指定的我们想要添加在这个实例上的 属性；

3、子传父：通过事件形式

子组件通过$emit()给父组件发送消息，父组件通过v-on绑定事件接收数据。

4、父子、兄弟、跨级：eventBus.js

这种方法通过一个空的Vue实例作为中央事件总线（事件中心），用它来（emit） 触发事件和（ emit）触发事件和（emit）触发事件和（on）监听事件，巧妙而轻量地实现了任何组件间的通信。

5、通信插件：PubSub.js

6、vuex

vuex 是 vue 的状态管理器，存储的数据是响应式的。只需要把共享的值放到vuex中，其他需要的组件直接获取使用即可。

# 19. Vue 中 router 和 route 有什么区别？
router为VueRouter的实例，相当于一个全局的路由器对象，里面含有很多属性和子对象，例如history对象。经常用的跳转链接就可以用this.$router.push，和router-link跳转一样。

route相当于当前正在跳转的路由对象。。可以从里面获取name、path、params、query等。

# 20. Vuex 是什么？如何使用？
Vuex是实现组件全局状态（数据）管理的一种机制，可以方便实现组件数据之间的共享；Vuex集中管理共享的数据，易于开发和后期维护；能够高效的实现组件之间的数据共享，提高开发效率；存储在Vuex的数据是响应式的，能够实时保持页面和数据的同步；

Vuex重要核心属性包括：state、mutations、action、getters、modules。

state

Vuex 使用单一状态树,即每个应用将仅仅包含一个store 实例，但单一状态树和模块化并不冲突。存放的数据状态，不可以直接修改里面的数据。

mutations

mutations定义的方法动态修改Vuex 的 store 中的状态或数据。

action

actions可以理解为通过将mutations里面处里数据的方法变成可异步的处理数据的方法，简单的说就是异步操作数据。view 层通过 store.dispath 来分发 action。

getters

类似vue的计算属性，主要用来过滤一些数据。

modules

项目特别复杂的时候，可以让每一个模块拥有自己的state、mutation、action、getters，使得结构非常清晰，方便管理。