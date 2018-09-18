# TodoList
待办事项列表，增加待办事项和删除待办事项

## 需求
1. 输入待办事项，选择提交，下方出现输入的项
2. 点击待办事项，删除该项

## 实现思路
1. 定义一个列表，每一次提交，就把输入项放入列表，每一次点击该项，就从列表中删除它
2. 输入的值应该和组件双向绑定，从视图流向模型，再流回视图
3. 使用 For of 循环出列表中的每一项
4. 为保证操作与视图分离，尝试引入组件，父组件向子组件传值，使用属性绑定，子组件向父组件传值，使用发布订阅模式

### 具体实现
1. 引入 VUE 脚本
```
<script src="js/vue.js"></script>
```
2. 创建模板，包括输入框，提交按钮，循环列表
```
<div id="root">
    <div>
        <input type="text" />
        <button>提交</button>
    </div>
    <ul>
        <li></li>
    </ul>
</div>
```
3. 实例化一个 VUE 对象  
```
new Vue({
})
```
* el 属性配置挂载点
```
el: "#root"
```
* data 属性定义双向绑定的值 inputValue(初始值设置为空)，和用于存储待办事项的列表 list
```
data: {
    inputValue: "",
    list: []
}
```
* methods 属性定义提交方法 handle，和删除方法 handleDelete
4. 子组件使用类似于 HTML 标签的别名 todo-item，<todo-item></todo-item>
5. 调用子组件
* 子组件循环
```
v-for="(item, index) of list"
```
* 将循环出的项，通过 content 属性接收，便于子组件接收和显示，同时传索引 index
```
:content="item"
:i="index"
```
* 接受父组件传来的用于视图中显示的待办事项 item 值，和明确删除哪一项的索引 index，存入 props 属性中
```
props: ["content", "i"]
```
* 定义模板，template 中使用字符串定义子组件的模板，写入点击方法
```
<li @click='handleClick'>{{content}}</li>
```
* methods 写子组件的方法，发布订阅，将方法使用一个别名传给父组件，同时代入索引
```
this.$emit("delete", this.i);
```
6. 父组件接收到订阅的方法名，
```
@delete="handleDelete"
```
* 根据接收的索引，具体去实现删除功能，也就是从列表中删除点击的某项
```
handleDelete: function(index) {
    this.list.splice(index, 1)
}
```