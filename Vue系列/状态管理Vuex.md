# 理解Vuex
Vuex使用**单一状态树** 集中式状态管理,单向数据流。一个应用只有一个全局单例的store对象
各个组件内部使用 this.$store访问该对象
## Vuex编码规则
1. 应用层级的状态应该集中到单个 store 对象中。
2. 提交 mutation 是更改状态的唯一方法，并且这个过程是同步的。
3. 异步逻辑都应该封装到 action 里面。

## 状态state
>Vuex 通过 **store 选项**，提供了一种机制将状态从根组件“注入”到每一个子组件中（需调用 Vue.use(Vuex))
1. Vuex 状态存储**响应式**,从store实例中读取状态的方法，在**计算属性**中返回某个状态
```javascript
    const Counter = {
        template:'<div>{{count}}</div>',
        computed:{
            count(){
                return store.state.count
            }
        }
    }
```
2. mapState 帮助生成计算属性,结合**扩展运算符** 使用 
函数接收的参数 Array | Object
mapState(['count']);
mapState({

})

## 状态的计算属性 Getter
1. 会暴露为 store.getters.doneTodos,函数接收的参数 state
```javascript
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {  //state 的计算属性函数
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }，
    getTodoById: (state) => (id) => { //返回函数 实现传递参数
        return state.todos.find(todo => todo.id === id)
   }
  }
})
```
2. mapGetters 将一个getter属性映射到本地**计算属性**ß当中

## 改变状态 mutation
该函数 必须是同步执行的，只有它才能改变state 这便是约定的
函数接收的参数是 state


## 异步改变状态 action
出于架构的目的，因为action可以包含异步操作
函数接受一个 与store对象相同的副本,{state,commit}
this.$store.dispatch() 分发 action
mapActions() 将方法 映射到 分发action
dispatch() 返回Promise·,等所有的Action对应的

## Module
命名空间
this.$store.[name]  这个name优先访问的是 模块的状态，如果全局的state中与这个[name]重名，那么 这个被 模块覆盖