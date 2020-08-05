#### 1.认识路由
路由就是通过互联的网络把信息从原地址传输到目的地址的活动。

i. 路由器提供了两种机制：**路由和转送**：
- 路由是决定数据包从来源到目的地的路径
- 转送将输入端的数据转移到合适的输出端

ii. 路由中有一个非常重要的概念：路由表。
- 路由表本质上就是一个映射表，决定了数据包的指向。

#### 2.前端路由
前端路由阶段是基于前后端分离后出现的一个新阶段，也叫**单页面富应用**(SPA)阶段，主要是在前后端分离的基础上加了一层前端路由。前端路由的概念和路由基本一样，也带有一个映射表，**根据不同的 url 地址展示不同的内容或页面**，但页面不进行整体的刷新。

**优点**：
1. 从性能和用户体验的层面来比较的话，后端路由每次访问一个新页面的时候都要向服务器发送请求，然后服务器再响应请求，这个过程肯定会有延迟。而前端路由在访问一个新页面的时候仅仅是变换了一下路径而已，没有了网络延迟，对于用户体验来说会有相当大的提升。
2. 在某些场合中，用ajax请求，可以让页面无刷新，页面变了但Url没有变化，用户就不能复制到想要的地址，用前端路由做单页面网页就很好的解决了这个问题

**缺点**：使用浏览器的前进，后退键的时候会重新发送请求，没有合理地利用缓存。

#### 3.URL的hash模式和HTML5的history模式
##### i. URL的hash
- URL的hash也就是锚点(#)，本质上是改变window.location的href(超文本链接)属性。
- 通过直接赋值location.hash来改变href，页面不会发生刷新。
- 使用方法：location.hash='path'。

##### ii. HTML5的history模式：pushState
- 使用方法：history.pushState({},'','path')。
- pushState遵循栈的规则（先进先出），越先加入的锚点越后取出，每次按返回键都是返回上一次访问的path。
- history.back()是返回上一次访问的path；

##### iii. HTML5的history模式：replaceState
- 使用方法：history.replaceState({},'','path')。
- 每次使用都会替换上一个path ，无法返回。目的是减少缓存的数据。

##### iv. HTML5的history模式：go
- 使用方法：history.go(n),n代表返回的页面的数目。
- history.go(-1)与history.back()等价；history.go(1)与history.forward()等价。

#### 4.vue-router
##### i. 概念：
- vue-router是Vue.js的官方路由插件，它和vue.js是深度集成的，适合用于构建单页面应用。

- vue-router是基于路由和组件的，路由用于设定访问路径。将组件和路径映射起来。

- 在vue-router的单页面应用中，页面的路径的改变就是组件的切换。

##### ii. 使用插件方式：
1. 通过Vue.use(插件名称)，安装插件
2. 创建路由实例，传入路由映射配置
3. 在Vue实例中挂载创建的路由实例

##### iii. 使用vue-router的步骤：
1. 创建路由组件
2. 配置路由映射：组件和路径映射关系
3. 使用路由：通过<router-link>和<router-view>

##### iv. <router-link>和<router-view>
1. <router-link>是一个vue-router中已经内置的组件，会被渲染成一个<a>标签；
2. <router-view>会根据当前的路径，动态渲染出不同的组件；
3. 网页其他内容回合<router-view>处于同一等级；
4. 路由切换时，切换的是<router-view>挂载的组件，其他内容不会发生改变。

###### <router-link>属性
1. to：用于指定跳转的路径
2. tag：用于指定<router-link>渲染成什么组件
3. replace：指定replace切换成history.replaceState，使浏览器后退键失效
4. active-class：当<router-link>对应路由匹配成功时，会自动给当前元素设置一个router-link-active的class，设置active-class可以修改默认名称。


##### v. 路由的默认路径
让路径默认跳转到一个path，可以使用重定向(redirect),将根路径重定向到一个path下，每次刷新会默认跳转到该path

##### vi. 路由使用的默认模式
若不想使用hash模式，可以在router实例中加上一句"mode: 'history'"切换到HTML5的history模式。

##### vii. 动态路由的使用
-  使用场景：点击链接之后，将用户相关信息显示在新的页面上，或者需要显示信息在url地址栏之中。
-  使用方式：
1. 修改index.js文件中的routes属性，对path属性进行修改，添加要在地址栏中显示的属性，在显示属性前加入字符':'。例：


    {
        // 只有获取到userId时才会显示该组件
        path: '/user/:userId',
        component: User
    }

2. 修改App.vue中的相关属性，在router-link标签中对to属性进行修改，使用v-bind来绑定to属性，在export default中添加data属性，添加要显示的属性：


    <!--    动态获取userId属性-->
    <router-link :to="'/user/' + userId" tag="button"     replace>用户</router-link>
     
    export default {
      name: 'App',
      data() {
        return {
          userId: 'admin'
        }
      }
    }

3. 当需要在页面中显示的时候，进入相应的组件文件中，添加computed属性，返回this.$route.params.userId，其中userId与在index.js文件中输入的属性要一致，在template模板中调用该计算属性：


    <template>
      <div>
        <h2>这是用户标题</h2>
        <p>这是用户信息</p>
        <p>{{userId}}</p>
      </div>
    </template>
     
    // 动态获取到userId，并将其显示在界面上
    computed: {
      userId() {
        return this.$route.params.userId
      }
    }
    
##### viii. 路由的懒加载
- 概念：直接打包构建应用时，JavaScript包会变得很大，影响页面的加载。路由懒加载的主要作用就是将路由对应的组件打包成一个个js代码块，这样这有在这个路由被访问到的时候，才加载对应的组件。
- 使用方式：const Home= () => import('组件路径')。

##### ix. 路由的嵌套使用
- 使用场景：整个页面是一个路由，为了点击选项卡来展示不同的子组件，这个时候就需要路由中的嵌套路由。
- 实现步骤：
1. 创建对应子组件，并且在路由映射中配置对应的子路由；
2. 在组件内部使用<router-view>和<router-link>标签。

##### x. vue-router的参数传递
-  参数传递主要有两种类型：params和query。
1. params类型：

    - 配置路由格式：/router/:id
    - 传递的方式：在path后面跟上对应的值
    - 传递后形成的路径：/router/name

2. query类型：

    - 配置路由格式：/router(普通配置)
    - 传递的方式：对象中使用query的key作为传递方式
    - 传递后形成的路径：/router?id=xxx

##### xi. 全局导航守卫
- 作用：监听路由的进入和离开。
- 导航守卫主要通过钩子函数beforeEach(在路由即将改变前触发)和afterEach(在路由改变后触发)。函数主要参数有to,from,next(afterEach没有):

    - to：即将要进入的目标的路由对象。
    - from：当前导航即将离开的路由对象。
    - next：调用该方法后，才能进入下一个对象。
    
- 补充：
1. afterEach不主动调用next函数；
2. 解析守卫（router.beforeResolve）：在导航被确认之前，同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被调用。
3. 路由独享的守卫：在路由配置上直接定义守卫，与全局前置守卫的方法参数一样。
4. 组件内的守卫：在路由组件内直接定义以下路由导航守卫：
- beforeRouteEnter：在渲染该组件的对应路由被 confirm 前调用(**不能**获取组件实例 'this',因为当守卫执行前，组件实例还没被创建)
- beforeRouteUpdate：在当前路由改变，但是该组件被复用时调用(可以访问组件实例 'this')
- beforeRouteLeave：导航离开该组件的对应路由时调用(可以访问组件实例 'this')

##### xii. vue-router的keep-alive
- keep-alive是Vue内置的一个组件，可以使被包含的组件保留状态，避免被重新渲染。
    - 属性：
    1. include：只有匹配的组件会被缓存；
    2. exclude：任何匹配的组件都不会被缓存。
- 使用方法：将<router-view>组件包裹在<keep-alive>标签内。
- 生命周期函数activated()和deactivated()只有在keep-alive状态下才能够被调用。