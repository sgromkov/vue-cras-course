<template>
    <div>
        <h2>Todos page</h2>
        <router-link to="/">Home</router-link>
        <AddTodo
            v-on:add-todo="addTodo"
        />
        <select v-model="filter">
            <option value="all">All</option>
            <option value="completed">Completed</option>
            <option value="not-completed">Not completed</option>
        </select>
        <hr>
        <PreLoader v-if="loading" />
        <TodoList
            v-else-if="filteredTodos.length"
            v-bind:todos="filteredTodos"
            v-on:remove-todo="removeTodo"
            v-on:change-todo="changeTodo"
        />
        <p v-else>No todos</p>
    </div>
</template>

<script>
    import AddTodo from '@/components/AddTodo';
    import TodoList from '@/components/TodoList';
    import PreLoader from '@/components/PreLoader';
    export default {
        name: 'TodosPage',
        data() {
            return {
                todos: [],
                loading: true,
                filter: 'all'
            }
        },
        components: {
            AddTodo,
            TodoList,
            PreLoader,
        },
        methods: {
            removeTodo(id) {
                this.todos = this.todos.filter(el => el.id !== id);
            },
            addTodo(todo) {
                this.todos.push(todo);
            },
            changeTodo(id) {
                this.todos = this.todos.map(el => {
                    const changedTodo = {...el};
                    if (el.id === id) {
                        changedTodo.completed = !changedTodo.completed;
                    }
                    return changedTodo;
                })
            }
        },
        computed: {
            filteredTodos() {
                switch (this.filter) {
                    case 'all':
                        return this.todos
                    case 'completed':
                        return this.todos.filter(el => el.completed)
                    case 'not-completed':
                        return this.todos.filter(el => !el.completed)
                    default:
                        return this.todos
                }
            }
        },
        mounted() {
            fetch('https://jsonplaceholder.typicode.com/todos?_limit=3')
                .then(response => response.json())
                .then(json => {
                    this.todos = json
                    this.loading = false
                })
        }
    };
</script>

<style scoped>
    select {
        margin: 10px auto;
    }
</style>
