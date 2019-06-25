<template>
<div>
    <input type="text" class = "todo-input" placeholder = "What needs to be done?" v-model = "newTodo"
           v-on:keyup.enter = "addTodo">
    <button class = "button1" :class = "{disabled: newTodo.trim().length === 0, testhover: newTodo.trim().length > 0}" @click = "addTodo">Add Todo</button>
    <transition-group enter-active-class = "animated fadeInRight" leave-active-class = "animated fadeOutRight">
    <div v-for = "(todo, index) in todosFiltered" :key = "todo.id" class = "todo-item">
        <div class = "todo-item-left">
            <input type="checkbox" v-model = "todo.completed" class = "input1">
            <div class = "todo-item-label" :class = "{completed: todo.completed}" v-if = "!todo.editing" @dblclick="editTodo(todo)">{{todo.title}}</div>
            <input type="text" v-model = "todo.title" class = "todo-item-edit" v-else @blur = "doneEdit(todo)"
            @keyup.enter = "doneEdit(todo)" v-focus @keyup.esc = "cancelEdit(todo)">
        </div>
        <div class="remove-item" v-on:click = "removeTodo(index)">
            &times;
        </div>
    </div>
    </transition-group>

    <div class="extra-container">
        <div>
            <label><input type="checkbox" v-on:change = "checkAll" :checked = "!anyRemaining">Check All</label>
        </div>
        <div>
            {{remaining}} items left
        </div>
    </div>

    <div class="extra-container">
        <div>
            <button class = "button2" v-bind:class = "{active: filter === 'all'}" @click = "filter = 'all'">All</button>
            <button class = "button2 buttonPosition" v-bind:class = "{active: filter === 'active'}" @click = "filter = 'active'">Active</button>
            <button class = "button2 buttonPosition" v-bind:class = "{active: filter === 'completed'}" @click = "filter = 'completed'">Completed</button>
        </div>

        <div>
            <button class = "button3" @click = "clearCompleted" v-bind:class = "{disabled:todos.filter(todo => todo.completed).length === 0, testhover2:todos.filter(todo => todo.completed).length > 0 }">Clear Completed</button>
        </div>
    </div>
</div>
</template>

<script>
    export default {
        name: "TodoList",
        data() {
            return {
                newTodo: "",
                idForTodo: 3,
                beforeEditing: "",
                filter: "all",
                todos: [
                    {
                        'id': 1,
                        'title': 'Finish vue screencast!',
                        completed: false,
                        editing: false
                    },
                    {
                        'id': 2,
                        'title': 'Take over world!',
                        completed: false,
                        editing: false
                    },
                ]
            }
        },
        directives: {
            focus: {
                inserted: function (el) {
                    el.focus()
                }
            }
        },
        computed: {
            remaining() {
                return this.todos.filter(todo => !todo.completed).length
            },
            anyRemaining() {
                return this.remaining !== 0
            },
            todosFiltered() {
                if(this.filter === "all") {
                    return this.todos
                }
                else if(this.filter === "active") {
                    return this.todos.filter(todo => !todo.completed)
                }
                else if(this.filter === "completed") {
                    return this.todos.filter(todo => todo.completed)
                }
            }
        },

        methods: {
            addTodo() {
                if(this.newTodo.trim().length === 0) {
                    return
                }

                this.todos.push({
                    id: this.idForTodo,
                    title: this.newTodo,
                    completed: false

                })
                this.idForTodo++;
                this.newTodo = "";
            },
            removeTodo(index) {
                this.todos.splice(index, 1);
            },
            editTodo(todo) {
                this.beforeEditing = todo.title
                todo.editing = true
            },
            doneEdit(todo) {
                if(todo.title.trim().length === 0) {
                    todo.title = this.beforeEditing
                }

                todo.editing = false
            },
            cancelEdit(todo) {
                todo.title = this.beforeEditing;
                todo.editing = false
            },
            checkAll() {
                this.todos.forEach((todo) => todo.completed = event.target.checked)
            },
            clearCompleted() {
                this.todos = this.todos.filter(todo => !todo.completed)
            }
        }
    }
</script>

<style>
    @import "../../node_modules/animate.css/animate.css";

    button {
        cursor: pointer;
    }

    .todo-input {
        width: 100%;
        padding: 10px 18px;
        font-size: 18px;
        margin-bottom: 16px;
        border-radius: 5px;
    }

    &:focus {
        outline: 0;
     }

    .todo-ite
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        margin-top: 35px;
        animation-duration: 0.5s;
    }

    .remove-item {
        cursor:pointer;
        margin-left: 14px;
        animation-duration: 0.3s
    }

    .remove-item:hover {
        color: black;
    }

    .todo-item-left {
        display: flex;
        align-items: center;
    }

    .todo-item-label {
        padding: 10px;
        border: 1px solid white;
        margin-left: 12px;
    }

    .todo-item-edit {
        font-size: 24px;
        color: #2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc;
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
    }

    @keyframes completed {
        from { text-decoration-color: transparent; }
        to { text-decoration-color: auto; }
    }

    .completed {
        -webkit-transition-duration: 0.4s; /* Safari */
        transition-duration: 0.4s;
        text-decoration: line-through;
        color:#9b9595;
        animation: completed 0.4s linear;
    }

    .button1 {
        width: 100%;
        background-color: #4CAF50;
        color: white;
        padding: 15px 32px;
        text-align: center;
        text-decoration: none;
        display: inline-block;
        font-size: 16px;
        border-radius: 5px;
    }

    .button1 {
        -webkit-transition-duration: 0.4s; /* Safari */
        transition-duration: 0.4s;
    }

    .testhover:hover {
        background-color: #f9f7f7; /* Green */
        color: black;
        border-color: #4CAF50;
        box-shadow: 0 12px 16px 0 rgba(0,0,0,0.24), 0 17px 50px 0 rgba(0,0,0,0.19);

    }

    .disabled {
        opacity: 0.6;
        cursor: not-allowed;
    }

    .extra-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgrey;
        padding-top: 14px;
        margin-bottom: 14px;
    }
    .button2 {
        background-color: #fcf4f4;
        color: black;
        padding: 7px 22px;
        text-align: center;
        text-decoration: none;
        display: inline-block;
        font-size: 15px;
        border-radius: 5px;
    }

    .button3 {
        -webkit-transition-duration: 0.4s; /* Safari */
        transition-duration: 0.4s;
        background-color: black;
        color: white;
        padding: 10px 27px;
        text-align: center;
        text-decoration: none;
        display: inline-block;
        font-size: 16px;
        border-radius: 5px;
    }

    .buttonPosition {
        margin-left: 4px;
    }
    .button2:hover {
        background: lightblue;
    }

    .active {
        background: lightblue;
    }

    .input1 {
        height: 25px;
        width: 25px;
    }

    .testhover2:hover {
        background-color: #f9f7f7;
        color: black;
        border-color: black;
        box-shadow: 0 12px 16px 0 rgba(0,0,0,0.24), 0 17px 50px 0 rgba(0,0,0,0.19);

    }





</style>