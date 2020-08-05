#### 1.Vuex是做什么的？
- Vuex是一个专为Vue.js应用程序开发的**状态管理模式**。采用**集中式**存储管理应用的所有组件的状态，并以相应的规则保证以一种可预测的方式发生变化。Vuex最大的优点是**响应式**。
- 主要使用场景：
1. 用户的登录状态、用户名称、头像、地理位置信息等
2. 商品收藏、购物车物品等

#### Vuex的核心概念
1. Vuex有几个核心概念：
- store：每一个vuex应用的核心，store（仓库）基本可以想象为一个容器，它包含着你的应用中大部分的状态。改变store中的状态唯一途径就是显式的提交mutation，这样使得我们可以方便的追踪每一个状态的变化。
- state: 驱动应用的数据源，vuex 管理的状态对象。state是唯一的：


    const state = {
	    xxx: initValue
    }

- mutations：
1. 包含多个直接更新state 的方法(回调函数)的对象
2. 触发: action 中的commit(‘mutation 名称’)
3. **只能包含同步的代码, 不能写异步代码**


    const mutations = {
    	yyy (state, {data1}) {
    		// 更新state 的某个属性
    	}
    }

- actions：
1. 包含多个事件回调函数的对象
2. 通过执行: commit()来触发mutation 的调用, 间接更新state
3. 触发: 组件中: $store.dispatch(‘action 名称’, data1) // ‘zzz’
4. 可以包含异步代码(定时器, ajax)


    const actions = {
    	zzz ({commit, state}, data1) {
    		commit('yyy', {data1})
    	}
    }

- getters：
1. 包含多个计算属性(get)的对象
2. 读取组件中: $store.getters.xxx


    const getters = {
    	mmm (state) {
    		return ...
    	}
    }
    
- modules：
1.包含多个module
2.一个module 是一个store 的配置对象
3.与一个组件(包含有共享数据)对应

#### 使用方法
1. 向外暴露store 对象：

    export default new Vuex.Store({
    	state,
    	mutations,
    	actions,
    	getters
    })

2. 组件中：


    import {mapState, mapGetters, mapActions} from 'vuex'
    export default {
    	computed: {
    		...mapState(['xxx']),
    		...mapGetters(['mmm']),
    	}
    	methods: mapActions(['zzz'])
    }
    {{xxx}} {{mmm}} @click="zzz(data)"
    
3. 映射store:


    import store from './store'
    	new Vue({
    	store
    })
    
4. store 对象:
    1. 所有用vuex 管理的组件中都多了一个属性$store, 它就是一个store 对象
    2. 属性:
    - state: 注册的state 对象
    - getters: 注册的getters 对象
    3. 方法: dispatch(actionName, data): 分发调用action


#### EventBus
1. EventBus 又称为**事件总线**。在Vue中可以使用 EventBus 来作为沟通桥梁的概念，就像是所有组件共用相同的事件中心，可以向该中心注册发送事件或接收事件，所以组件都可以**上下平行**地通知其他组件.

2. 使用方法：
    1. **初始化**：首先需要做的是创建事件总线并将其导出，以便其它模块可以使用或者监听它。有两种方式可以处理：
    - 新创建一个 .js 文件，引入 Vue 并导出它的一个实例
    - 直接在项目中的 main.js 初始化 EventBus ，这种方式初始化的 EventBus 是一个**全局的事件总线**。
    2. 发送事件：在子组件中绑定方法，在方法中，通过EventBus.$emit(channel: 频道名, callback(payload1,…))监听频道。这样，组件就发送出了该频道,如果你只想监听一次事件的发生，可以使用 EventBus.$once(channel: string, callback(payload1,…))
    3. 接收事件：在组件 App.vue 中使用 EventBus.$on(channel: string, callback(payload1,…))监听组件发送出了的频道。
    4. 移除事件监听者:使用 EventBus.$off('频道名') 来移除应用内所有对此事件的监听。或者直接调用EventBus.$off() 来移除所有事件频道， 注意**不需要添加任何参数** 

3. 创建全局EventBus
全局事件总线只是一个简单的 vue 组件，代码如下：


    var EventBus = new Vue();
     
    Object.defineProperties(Vue.prototype, {
        $bus: {
            get: function () {
                return EventBus
            }
        }
    })

这个特定的总线使用两个方法 $on 和 $emit 。一个用于创建发出的事件，它就是$emit ；另一个用于订阅 $on ：

    var EventBus = new Vue();
     
    this.$bus.$emit('nameOfEvent',{ ... pass some event data ...});
     
    this.$bus.$on('nameOfEvent',($event) => {
        // ...
    })