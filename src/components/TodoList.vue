<template>
    <div>
        <input type="text" 
            class='todo-input' 
            placeholder="what need to be done" 
            v-model="newTodo"
            @keyup.enter="addTodo"
            />
        <b>Your Todo List</b>
        <transition-group enter-active-class='animated fadeInUp' leave-active-class="animated fadeOutDown">
        <div v-for="(todo,index) in todosFiltered" :key="todo.id" class="todo-item">
            <div class="todo-item-left">
                <input type="checkbox" v-model="todo.completed">
                <div @dblclick="editTodo(todo)" 
                    v-if="!todo.editing" 
                    class="todo-item-label"
                    :class="{ completed: todo.completed }">{{ todo.title }} 
                </div>
                <input v-focus @keyup.esc="cancelEdit(todo)" @keyup.enter="doneEdit(todo)" @blur="doneEdit(todo)" v-else class="todo-item-edit" type='text' v-model="todo.title" />   
            </div>
            <div class='remove-item' @click="removeTodo(index)">
                &times;
            </div>
        </div>
        </transition-group>
        <div class="extra-container">
            <div>
                <label>
                    <input type="checkbox" 
                        :checked="!anyRemaining"
                        @change="changeAllTodos">
                        Check All
                </label>
            </div>
            <div>{{ remaining}} items left</div>
        </div>

        <div class="extra-container">
            <div>
                <button :class="{active: filter == 'all'}" @click="filter = 'all'">All</button>
                <button :class="{active: filter == 'active'}" @click="filter = 'active'">Active</button>
                <button :class="{active: filter == 'completed'}" @click="filter = 'completed'">Completed</button>
            </div>

            <div>
                <transition name='fade'>
                <button
                    v-if="showClearCompletedButton"
                    @click="clearCompleted"
                >Clear Completed
                </button>
                </transition>
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
            beforeEditCache: '',
            filter: 'all',
            todos:[
                {
                    "id": 1,
                    "title":"Finish Vue Screencast",
                    "completed": false,
                    "editing": false
                },
                {
                    "id": 2,
                    "title":"Take over world",
                    "completed": true,
                    "editing": false
                }
            ]
        }
    },
    directives:{
        focus:{
            inserted: function(el){
                el.focus()
            }
        }
    },
    computed:{
        remaining(){
            return this.todos.filter(todo => !todo.completed).length
        },
        anyRemaining(){
            return this.remaining != 0;
        },
        todosFiltered(){
            if(this.filter == 'all'){
                return this.todos;
            } else if(this.filter == 'active'){
                return this.todos.filter(todo => !todo.completed)
            }else if (this.filter == 'completed'){
                return this.todos.filter(todo => todo.completed)
            }
            return this.todos;
        },
        showClearCompletedButton(){
            return this.todos.filter(todo => todo.completed).length > 0
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
        editTodo(todo){
            this.beforeEditCache = todo.title;
            todo.editing = true;
        },
        doneEdit(todo){
            if(todo.title.trim() == ''){
                todo.title = this.beforeEditCache;
            }
            todo.editing = false;
        },
        cancelEdit(todo){
            todo.title = this.beforeEditCache;
            todo.editing = false;
        },
        removeTodo(index){
            this.todos.splice(index,1)
        },
        changeAllTodos(){
            this.todos.forEach(todo => todo.completed = event.target.checked)
        },
        clearCompleted(){
            this.todos = this.todos.filter(todo => !todo.completed);
        }
    }
}

</script>

<style lang="scss">

    @import url('https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css');

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

    .todo-item-label {
        display:inline;
        margin-left:10px;
    }

    .remove-item{
        cursor:pointer;
        margin-left: 14px;
        &:hover {
            color:red;
        }
    }

    .todo-item-edit {
        display:block;
        width:100%;
        color:#2c3e50;
        padding:10px;
        border: 1px solid #ccc;
        font-size: 24px;
        &:focus{
            outline: none;
        }
    }

    .completed {
        text-decoration: line-through;
        color:grey;
    }

    .extra-container {
        display:flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgrey;
        padding-top: 14px;
        margin-bottom: 14px;
    }

    button {
        font-size: 14px;
        background-color: white;
        appearance: none;
        &:hover {
            background: lightgreen;
        }
        &:focus {
            outline: none;
        }
    }

    .active {
        background-color: lightgreen;
    }

    .fade-enter-active, .fade-leave-active {
        transition:opacity .2s;
    }

    .fade-enter, .fade-leave-to {
        opacity: 0;
    }

</style>
