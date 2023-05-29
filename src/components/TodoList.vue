<script setup lang="ts">
import { ref } from 'vue'

export interface Todo {
  text: string
  done: boolean
}

const todoList = ref<Todo[]>([
  { text: 'Learn Vue', done: false },
  { text: 'Learn Go', done: false },
  { text: 'Learn Rust', done: false }
])
const newTodo = ref('')

const addTodo = () => {
  if (newTodo.value.trim() === '') return
  todoList.value.push({
    text: newTodo.value,
    done: false
  })
  newTodo.value = ''
}

const deleteTodo = (index: number) => {
  todoList.value.splice(index, 1)
}
</script>

<template>
  <div class="todolist">
    <h1 class="green">Todo List</h1>
    <ul>
      <li v-for="(todo, index) in todoList" :key="index">
        <input type="checkbox" v-model="todo.done" />
        {{ todo.text }}
        <button @click="deleteTodo(index)">Delete</button>
      </li>
    </ul>
    <input type="text" v-model="newTodo" @keyup.enter="addTodo" />
    <button @click="addTodo">Add</button>
  </div>
</template>
