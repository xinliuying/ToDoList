<template>
<li>
    <label>
        <!-- 是否勾选看done属性 -->
        <input type="checkbox" :checked="todoObj.done" @change="handleCheck(todoObj.id)"/>

        <!-- 以下代码能完成功能 但是不建议使用 因为修改了props的值 违反了vue的原则 -->
        <!-- <input type="checkbox" v-model="todoObj.done"/> -->

        <!-- 是否呈现输入框取决于isEdit的值 -->
        <span v-show="!todoObj.isEdit">{{todoObj.title}}</span>
        <input type="text" v-show="todoObj.isEdit" @blur="handleBlur(todoObj,$event)" ref="inputBox"/>
    </label>
    <!-- 删除和编辑按钮 -->
    <button class="btn btn-danger" @click="handleDelete(todoObj.id)">删除</button>
    <button v-show="!todoObj.isEdit" class="btn btn-edit" @click="handleEdit(todoObj)">编辑</button>
</li>
</template>

<script>


export default {
    name:'MyItem',
    // props接收父组件传过来的数据
    props:['todoObj'],
    methods:{
        handleCheck(id){
            this.$bus.$emit('todoDone',id);
        },
        handleDelete(id){
            if (confirm('确认要删除吗')){
                this.$bus.$emit('todoDelete',id); 
            }
        },
        handleEdit(todoObj){
            // 对象中后添加的属性 vue默认不做响应式处理；如需给后添加的属性做响应式，应使用vue.set或vm.$set
            if(todoObj.hasOwnProperty("isEdit")){
                todoObj.isEdit = true;              
            }else{
                this.$set(todoObj,"isEdit",true);  
            } 
            // 在下一次DOM更新结束后执行其指定的回调 nextTick
            this.$nextTick(function(){
                this.$refs.inputBox.focus()
            })
        },
        handleBlur(todoObj,e){
            if(!e.target.value.trim()){
                return alert ('请输入有效的内容')
            }else{
                this.$bus.$emit('todoEdit',todoObj.id,e.target.value);
                todoObj.isEdit = false;
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