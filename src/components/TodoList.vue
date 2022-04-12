<template>
    <div>
        <ul>
            <TodoItem
                v-for="(todo, index) in todos"
                v-bind:todo="todo"
                v-bind:key="todo.id"
                v-bind:index="index"
                v-on:remove-todo="removeTodo"
                v-on:change-todo="changeTodo"
            />
        </ul>
    </div>
</template>

<script>
    import TodoItem from '@/components/TodoItem';
    export default {
        props: ['todos'],
        components: {
            TodoItem
        },
        methods: {
            removeTodo(id) {
                this.$emit('remove-todo', id)
            },
            changeTodo(id) {
                this.$emit('change-todo', id)
            }
        }
    }
</script>

<style scoped>
    ul {
        list-style: none;
        margin: 0;
        padding: 0;
    }
</style>
