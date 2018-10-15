<template>
    <div>
        <input type="text" 
            class='todo-input' 
            placeholder="what need to be done" 
            v-model="newTodo"
            @keyup.enter="addTodo"
            />
        Todo list goes here
        <div v-for="(todo,index) in todos" :key="todo.id" class="todo-item">
            <div>
                {{ todo.title }}
            </div>
            <div class='remove-item' @click="removeTodo(index)">
                &times;
            </div>
        </div>
    </div>
</template>


<script>
export default {
    name:'TodoList',
    data(){
        return{
            newTodo: "Welcome to Your Vue.js App",
            idForTodo:3,
            todos:[
                {
                    "id": 1,
                    "title":"Finish Vue Screencast",
                    "completed": false
                },
                {
                    "id": 2,
                    "title":"Take over world",
                    "completed": true
                }
            ]
        }
    },
    methods:{
        addTodo(){
            if(this.newTodo.trim().length === 0){
                return
            }
            this.todos.push({
                id:this.idForTodo,
                title:this.newTodo,
                completed:false
            })
            this.newTodo = ''
            this.idForTodo++
            
        },
        removeTodo(index){
            this.todos.splice(index,1)
        }
    }
}
</script>

<style lang="scss">
    .todo-input {
        width:100%;
        padding: 10px 18px;
        font-size: 18px;
        margin-bottom: 16px;
        &:focus {
            outline:2px solid darkgreen;
        }
    }

    .todo-item {
        margin-bottom: 12px;
        display:flex;
        align-items: center;
        justify-content: space-between;
    }

    .remove-item{
        cursor:pointer;
        margin-left: 14px;
        &:hover {
            color:red;
        }
    }

</style>
