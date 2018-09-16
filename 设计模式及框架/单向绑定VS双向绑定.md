# 单向绑定 VS 双向绑定
+ 对于非UI控件来说，不存在双向，只有单向;只有UI控件才有双向的问题。

+ 单向绑定使得数据流也是单向的，对于复杂应用来说这是实施统一的状态管理（如redux）的前提。双向绑定在一些需要实时反应用户输入的场合会非常方便（比如多级联动菜单）。但通常认为复杂应用中这种便利比不上引入状态管理带来的优势。

## ps:

+ 注意，Vue 虽然通过 v-model 支持双向绑定，但是如果引入了类似redux的vuex，就无法同时使用 v-model

UI-->modal: 绑定UI事件，改变vm中的值
modal-->UI: VM对象上的 访问器属性 数据变动

双向绑定违背了单向数据流，在组件内部修改了prop，然后通过
Vuex 不不能直接修 state
在子组件内部修改prop时 **(单向数据流强调prop是只读的)**

```HTML
<input v-model="searchText">

<input type="text" 
    v-bind:value="searchText" 
    v-on:input="searchText = $event.target.value"
> 
```

```javascript
//指令修饰符  
:prop1.sync="data1"  

//等价于
:prop1="data1"
@update:prop1="data1=value"

//在js使用过程使用
this.prop1 = '修改数据内容'
this.$emit('update:prop1',this.prop1)

/*
.sync修饰符与v-model的差异在于v-model的默认值是：
value 是属性名
input 触发更新的事件名
*/


//实现自定义的 v-model
//创建组件没有语法限制，任意对象即可
//指定 functional:true 创建函数式组件
//默认的 value属性 和 input事件
export default {
    model:{
        prop:'twoWayProp',
        event:'two-way'
    },
    props:{
        value:{
            type:String,
            default:''   
        },
        twoWayProp:{
            type:String,
            default:''
        }
    },
    data(){
        return {
            inputStr:'双向绑定',
            checkedNames:[]
        }
    },
    methods:{
        modifyHandler(evt){
            //违背了单向数据流
            this.twoWayProp = '我修改了内容'
            this.$emit('two-way',this.twoWayProp)
            // this.value = '我修改了内容'
            // this.$emit('input',this.value)
        }
    }
}
```

## 实现双向绑定的方式

1. 发布/订阅模式
2. 属性劫持 Object.defineProperty
3. 脏数据检测 watch方法维护了一个数据


