<template>
<ul class="todo-main">
    <!-- 遍历展示数据 -->
    <!-- 组件间通信 父子间组件通信 这里使用props -->
    <MyItem v-for="todoObj in todos" :key="todoObj.id" :todoObj="todoObj" :todoDone="todoDone" :todoDelete="todoDelete"/>
</ul> 
</template>

<script>
import MyItem from './MyItem';

export default {
    name:'MyList',
    components:{MyItem},
    props:['todos','todoDone','todoDelete']
}
</script scoped>

<style>
/*main*/
.todo-main {
    margin-left: 0px;
    border: 1px solid #ddd;
    border-radius: 2px;
    padding: 0px;
}

.todo-empty {
    height: 40px;
    line-height: 40px;
    border: 1px solid #ddd;
    border-radius: 2px;
    padding-left: 0px;
    margin-top: 10px;
}
</style>