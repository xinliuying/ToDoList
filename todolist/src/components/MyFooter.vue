<template>
<div class="todo-footer" v-if="todos.length">
    <label>
    <input type="checkbox" v-model="isAll" />
    </label>
    <span>
    <span>已完成{{doneNum}}</span> / 全部{{todos.length}}
    </span>
    <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
</div>
</template>

<script>
export default {
    name:'MyFooter',
    props:['todos','CheckAll','clearAllTodo'],
    computed:{
        doneNum(){
            // 数组函数reduce的使用，对该数组进行统计
            return this.todos.reduce((pre,todo) => pre + (todo.done ? 1 : 0),0)
        },
        // 使用计算属性的完整写法 因为涉及到了数据的改变
        isAll:{
            get(){
                return this.doneNum === this.todos.length && this.todos.length > 0
            },
            set(value){
                this.CheckAll(value)
            }    
        }
    },
    methods:{
        clearAll(){
            this.clearAllTodo();
        }
    }
}
</script>

<style scoped>
/*footer*/
.todo-footer {
    height: 40px;
    line-height: 40px;
    padding-left: 6px;
    margin-top: 5px;
}

.todo-footer label {
    display: inline-block;
    margin-right: 20px;
    cursor: pointer;
}

.todo-footer label input {
    position: relative;
    top: -1px;
    vertical-align: middle;
    margin-right: 5px;
}

.todo-footer button {
    float: right;
    margin-top: 5px;
}

</style>