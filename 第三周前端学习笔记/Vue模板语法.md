### 1.MVVM
MVVM 是一种简化用户界面的事件驱动编程方式。

- View是视图层，也是用户交互层，主要是由 html 和 css 构建。
- Model是视图层，泛指后端进行的各种逻辑处理和数据操控。
- ViewModel是指视图数据层，在这一层，前端开发者对从后端获取到的 Model 数据进行转换处理，做二次封装，以生成符合 View 层使用预期的视图数据模型。

与 MVM 相比，MVVM 主要是解决 MVM 的反馈不即时的问题。

### 2.生命周期函数
##### beforeCreate
在实例初始化之后，这个时候数据还没有挂载，无法访问数据和真实的DOM（此时data属性和$el都是undefined）。一般不作操作。

##### created
组件实例创建完成调用，属性绑定，此时已经可以使用数据(data)了,但DOM还未生成，$el属性还不存在。**一般可以做初始化数据的获取**。

##### beforeMount
在挂载开始之前被调用，这个时候虚拟DOM已经创建完成。马上要渲染，这里可以更改数据，不会触发updated,渲染前最后一个更改数据的机会，不会触发其他钩子函数，**一般可以在这里做初始化数据的获取**。

##### mounted
挂载到实例渲染出真实的DOM，数据真实DOM都处理好了，事件已经挂载好了，**可以在这里操作真实DOM**。

##### beforeUpdate
数据更新(组件更新之前调用)时调用，发生在虚拟DOM重新渲染和补丁之前，当组件或实例的数据更改之后，会立即执行beforeUpdate，然后vue的虚拟dom机制会重新构建虚拟dom与上一次的虚拟dom树利用diff算法进行对比之后重新渲染，一般不做什么事儿。

##### updated
由于数据更改导致的虚拟DOM重新渲染和打补丁，在这之后会调用该钩子(组件更新之后调用)，当组件或实例的数据更改之后，会立即执行beforeUpdate，然后vue的虚拟dom机制会重新构建虚拟dom与上一次的虚拟dom树利用diff算法进行对比之后重新渲染，一般不做什么事儿。

##### beforeDestory
实例销毁之前调用，一般在这里做一些善后工作，例如**清除计时器、清除非指令绑定的事件等等**。

##### destroyed
实例销毁之后调用，组件数据绑定、监听等去掉之后只剩下DOM空壳，这时执行destroyed，也可以在这里做善后工作

完整的生命周期按照上述**由上而下**执行。

### 3.模板语法
#### 插值语法：
##### Mustache语法(双大括号)
数据绑定最常见的形式就是使用Mustache语法进行**文本插值**。

##### Html插值
使用 v-html 指令用于输出 html 代码。
**适用于解析data属性里是html形式的message**。

##### v-once指令
被定义了 v-once指令的元素或组件（包括元素或组件内的所有子孙节点）只能被渲染一次。首次渲染后，即使数据发生变化，也不会被重新渲染。**一般用于静态内容展示**。

##### v-cloak指令
经常和display:none;配合使用。这个指令保持在元素上直到关联实例结束编译。经常和display:none;配合使用。用于解决HTML绑定Vue实例，在页面加载时会闪烁的问题。

#### 样式绑定
v-bind用于绑定数据和元素属性。官方提供了一个v-bind的语法糖：

    <!-- 完整语法 -->
    <a v-bind:href="url"></a>
    <!-- 语法糖 -->
    <a :href="url"></a>
    
###### 使用方法：
- 绑定HTML Class
1. 对象语法：给v-bind:class 一个对象，以动态地切换class。注意：v-bind:class指令可以与普通的class特性共存
2. 数组语法：把一个数组传给v-bind:class，以应用一个class列表

- 绑定内联样式
1. 对象语法：v-bind:style 的对象语法十分直观--非常像CSS，其实它是一个Javascript对象，CSS属性名**必须用驼峰命名法**。
2. 数组语法：可将多个样式对象应用到一个元素上

- 绑定其他html属性。例如url，src

#### 列表渲染
v-for 指令基于一个数组来渲染一个列表。v-for 指令需要使用 item in items 形式的特殊语法，其中 items 是源数据数组，而 item 则是被迭代的数组元素的别名。
v-for还支持一个可选的第二个参数，即当前项的索引(index)。

###### 使用方法：
- 在v-for里遍历数组
- 在v-for里遍历对象
- 使用v-for处理数组中的过滤和排序等处理结果

**注意：**
在使用v-for时，最好给对应元素或组件加上一个key。
详情见：https://www.jianshu.com/p/4bd5e745ce95

#### 条件渲染
##### v-if
v-if 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回 truthy 值的时候被渲染。

##### v-else
用来表示v-if 的“else 块”

##### v-else-if
充当 v-if 的“else-if 块”，可以连续使用

##### v-if和v-show的区别：
- v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。


- v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。


- v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。

##### key
因为Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。而key可以管理可复用的元素，给标签添加一个具有唯一值的key，可以使整个标签在切换时都会被重新渲染，不可复用。

#### 事件处理
##### 监听事件
可以用 v-on 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。
官方提供了一个语法糖：用@代替v-on。



##### 事件处理方法
由于许多事件处理逻辑会更为复杂，直接把 JavaScript 代码写在 v-on 指令中是不可行的，因此 v-on 还可以接收一个需要调用的方法名称。

##### v-on参数
在methods中定义方法，以供@click调用时，需要注意参数问题：

- 如果该方法不需要额外参数，那么方法后的()可以不添加，如果方法本身有一个参数，会默认将原生事件event参数传入事件。
- 如果需要同时传入某个参数，需要event时，需要通过 **$event**传入事件。

##### v-on修饰符
Vue提供了修饰符来帮助我们方便处理一些事件：

###### 事件修饰符：

- .stop-调用event.stopPropagation()，阻止事件冒泡。
- .prevent-调用event.preventDefault()。阻止元素发生的默认行为。
- .capture-添加事件监听器时使用事件捕获模式，也就是从外向内执行。.capture修饰符要写在外层才能生效。
- .self-当前元素自身时触发处理函数时才会触发函数,根据event.target确定是否当前元素本身，来决定是否触发的事件/函数。
- .once-加上此修饰符之后相应的函数只能触发一次。
- .passive-执行默认方法,一般用在滚动监听，@scoll，@touchmove 。

注意：passive和prevent冲突，不能同时绑定在一个监听器上。

###### 按键修饰符
在监听键盘事件时，我们经常需要检查详细的按键。Vue 允许为 v-on 在监听键盘事件时添加按键修饰符。

**按键码**：

- .enter
- .tab
- .delete
- .esc
- .space
- .up
- .down
- .left
- .right

###### 系统修饰键
可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器：

- .ctrl
- .alt
- .shift
- .meta(Windows的徽标键)

注意：饰键与常规按键不同，在和 keyup 事件一起用时，事件触发时修饰键必须**处于按下状态**。

- .exact 修饰符允许你控制由精确的系统修饰符组合触发的事件。

###### 鼠标按钮修饰符

- .left
- .right
- .middle

#### 表单输入绑定
1. v-model在表单 <input>、<textarea> 及 <select> 元素上创建双向数据绑定。负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。


2. v-model 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：

- text 和 textarea 元素使用 value property 和 input 事件；
- checkbox 和 radio 使用 checked property 和 change 事件；
- select 字段将 value 作为 prop 并将 change 作为事件。

3. 修饰符
- .lazy: 在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步,lazy 修饰符可以转为在 change 事件之后进行同步。
- .number: 自动将用户的输入值转为数值类型,如果这个值无法被 parseFloat()解析，则会返回原始的值。
- .trim: 自动过滤用户输入的**首尾**(字符串中间的空白字符不会处理)空白字符。

