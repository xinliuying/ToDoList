<template>
<body>
    <div id="root">
    <div class="todo-container">
        <div class="todo-wrap">
        <!-- 使用消息订阅与发布进行App和MyHeader中的通信 -->
        <MyHeader/>

        <!-- 使用props使App和MyList进行通信 -->
        <MyList :todos="todos"/>

        <!-- 父子之间的通信:父给子传递参数，子通过props声明接收--CheckAll-->
        <!-- 通过自定义事件，绑定clearAllTodo事件，子$emit触发自定义事件-->
        <MyFooter :todos="todos" :CheckAll="CheckAll" @clearAllTodo="clearAllTodo"/>
        </div>
    </div>
    </div> 
</body>
</template>

<script>
import pubsub from 'pubsub-js';
import MyHeader from './components/MyHeader';
import MyFooter from './components/MyFooter';
import MyList from './components/MyList';

export default {
    name:'App',
    components:{
        MyHeader,
        MyList,
        MyFooter
    },
    data(){
        return{
            todos: JSON.parse(window.localStorage.getItem('todos')) || []
        }       
    },
    methods:{
        // 使用消息订阅与发布进行通信-addTodo 注意：消息订阅回调函数第一个参数是msgName，需要使用_占位
        addTodo(_,todo){
            this.todos.unshift(todo)
        },

        // App要和MyItem进行通信，使用全局事件总线-todoDone，todoDelete
        todoDone(id){
            this.todos.forEach((todo)=>{
                if(todo.id === id) todo.done = !todo.done
            })
        },
        todoDelete(id){
            this.todos = this.todos.filter(todo => todo.id != id)
        },

        CheckAll(value){
            this.todos.forEach((todo) => todo.done = value)
        },
        clearAllTodo(){
            this.todos = this.todos.filter(todo => !todo.done)
        },
        todoEdit(id,value){
            this.todos.forEach((todo)=>{
                if(todo.id === id) todo.title = value
            })
        }
    },
    mounted(){
        this.pid = pubsub.subscribe('addTodo',this.addTodo);
        this.$bus.$on('todoDone',this.todoDone);
        this.$bus.$on('todoDelete',this.todoDelete);
        this.$bus.$on('todoEdit',this.todoEdit);
    },

    // 在销毁前将绑定和订阅的事件取消
    beforeDestroy(){
        this.$bus.$off('todoDone');
        this.$bus.$off('todoDelete');
        this.$bus.$off('todoEdit');
        pubsub.unsubscribe(this.pid)
    },
    // 通过侦听属性将todo数据存放到localStorage中，这样刷新就不会丢失数据了
    watch:{
        todos:{
            deep:true,
            handler(value){
                window.localStorage.setItem('todos',JSON.stringify(value))
            }
        }
    }
}
</script>

<style>
/*base*/
body {
    background: #fff;
}
.btn {
    display: inline-block;
    padding: 4px 12px;
    margin-bottom: 0;
    font-size: 14px;
    line-height: 20px;
    text-align: center;
    vertical-align: middle;
    cursor: pointer;
    box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
    border-radius: 4px;
}
.btn-danger {
    color: #fff;
    background-color: #da4f49;
    border: 1px solid #bd362f;
}
.btn-danger:hover {
    color: #fff;
    background-color: #bd362f;
}
.btn-edit {
    color: #fff;
    background-color: #5a40c4;
    border: 1px solid #2742ab;
    margin-right: 5px;
}
.btn-edit:hover {
    color: #fff;
    background-color: #341880;
    margin-right: 5px;
}
.btn:focus {
    outline: none;
}
.todo-container {
    width: 600px;
    margin: 0 auto;
}
.todo-container .todo-wrap {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
}

</style>