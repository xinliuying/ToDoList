<template>
<div class="todo-header">
    <input type="text" placeholder="请输入你的任务名称，按回车键确认" @keyup.enter="add"/>
</div> 
</template>

<script>
import pubsub from 'pubsub-js';
import {nanoid} from 'nanoid'
export default {
    name:'MyHeader',
    // props:['addTodo'],
    methods:{
        add(e){
            // 判断输入是否合法
            if(!e.target.value.trim()) return alert ('请输入有效的内容');
            // ID生成用nanoid
            const todoThing = {id:nanoid(),title:e.target.value,done:false};
            // 将包装好的对象传递给App组件
            pubsub.publish('addTodo',todoThing);
            // 清空输入框
            e.target.value = ''
        }
    }
}
</script>

<style scoped>
/*header*/
.todo-header input {
    width: 560px;
    height: 28px;
    font-size: 14px;
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 4px 7px;
}

.todo-header input:focus {
    outline: none;
    border-color: rgba(82, 168, 236, 0.8);
    box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
}
</style>