<template>
<li>
    <label>
        <!-- 是否勾选看done属性 -->
        <input type="checkbox" :checked="todoObj.done" @change="handleCheck(todoObj.id)"/>
        <!-- 以下代码能完成功能 但是不建议使用 因为修改了props的值 违反了vue的原则 -->
        <!-- <input type="checkbox" v-model="todoObj.done"/> -->
        <span>{{todoObj.title}}</span>
    </label>
    <button class="btn btn-danger" @click="handleDelete(todoObj.id)">删除</button>
</li>
</template>

<script>


export default {
    name:'MyItem',
    // props接收父组件传过来的数据
    props:['todoObj','todoDone','todoDelete'],
    methods:{
        handleCheck(id){
            this.todoDone(id);
        },
        handleDelete(id){
            if (confirm('确认要删除吗')){
                this.todoDelete(id);  
            }
        }
    }
}
</script>

<style scoped>
/*item*/
li {
    list-style: none;
    height: 36px;
    line-height: 36px;
    padding: 0 5px;
    border-bottom: 1px solid #ddd;
}

li label {
    float: left;
    cursor: pointer;
}

li label li input {
    vertical-align: middle;
    margin-right: 6px;
    position: relative;
    top: -1px;
}

li button {
    float: right;
    display: none;
    margin-top: 3px;
}

li:before {
    content: initial;
}

li:last-child {
    border-bottom: none;
}

li:hover {
    background-color: #ddd;
}

li:hover button{
    display: block;
}
</style>