# 单向绑定 VS 双向绑定

+ 对于非UI控件来说，不存在双向，只有单向;只有UI控件才有双向的问题。
+ 单向绑定使得数据流也是单向的，对于复杂应用来说这是实施统一的状态管理（如redux）的前提。双向绑定在一些需要实时反应用户输入的场合会非常方便（比如多级联动菜单）。但通常认为复杂应用中这种便利比不上引入状态管理带来的优势。
## ps:
+ 注意，Vue 虽然通过 v-model 支持双向绑定，但是如果引入了类似redux的vuex，就无法同时使用 v-model

UI-->modal: 绑定UI事件，改变vm中的值
modal-->UI: VM对象上的 访问器属性 数据变动

`
<input type="text" v-bind:value="msg" v-on:input="msg = $event.target.value"> 
`

