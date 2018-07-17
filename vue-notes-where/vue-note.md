Presenter层是View层以及Model层的中转站


1. MVP与MVVM设计模式
MVVM:  View - ViewModel - Model

Model:负责存储数据
View:视图层，负责显示数据
ViewModel:vue自带的一层(内置))，不需要我们去关心怎么实现的。
当我们使用MVVM设计模式的时候，我们只需要关心view和model。

在mvp设计模式开发的时候是面向DOM，mvvm设计模式面向数据进行编程，大大的简化了DOM的操作，可以节约代码量

2. 前端组件化

使用Vue.component创建全局组件
全局注册的行为必须在根 Vue 实例 (通过 new Vue) 创建之前发生

```javascript
Vue.component("TodoItem",{
    props:["content"],
    template:"<li>{{content}}</li>"
});

```
注册局部组件

```javascript
 var TodoItem = {
    props:["content"],
    template:"<li>{{content}}</li>"
}
//在根部注册
var app = new Vue({
    el:"#root",
    components:{
        TodoItem
    }
})
```

3. 子组件向父组件传值
在子组件的模板中定义事件`deleteSelf`，并在methods中使用$emit()向外触发事件，并且可以传值;
```javascript
//在子组件中的methods中
 var TodoItem = {
    props:["content",'index'],
    template:"<li @click='deleteSelf'>{{content}}</li>",
    methods:{
        deleteSelf:function(){
            this.$emit('delete',this.index);
        }
    }
}
```
在局部组件中，动态的监听刚刚在子组件中定义的事件`delete`，并将其动态的绑定到父组件的事件中 `deleteSon`,此时需要父组件改变数据，则DOM树会相应改变。
```javascript
  <todo-item v-for="(item,index) in list " 
            v-bind:index="index"  
            v-bind:content="item"   
            v-on:delete = "deleteSon">
  </todo-item>

     var app = new Vue({
        el:"#root",
        components:{
            TodoItem
        },
        data:{
            todoValue:"",
            list:[]
        },
        methods:{
            deleteSon:function(index){
                this.list.splice(index,1)
            }
        }
    })
```
4. 生命周期函数

生命周期函数就是vue实例在某一个时间点会自动执行的函数

