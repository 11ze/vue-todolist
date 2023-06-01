<script setup lang="ts">
import { ref } from 'vue'

export interface Todo {
  id: number
  text: string
  done: boolean
}

const todoList = ref<Todo[]>([
  { id: 1, text: 'Learn Vue', done: false },
  { id: 2, text: 'Learn Go', done: false },
  { id: 3, text: 'Learn Rust', done: false }
])
const newTodo = ref('')

const maxId = () => {
  return todoList.value.reduce((acc, cur) => {
    return acc > cur.id ? acc : cur.id
  }, 0)
}

const addTodo = () => {
  const value = newTodo.value.trim()
  if (value === '') return
  todoList.value.push({
    id: maxId() + 1,
    text: value,
    done: false
  })
  newTodo.value = ''
}

const deleteTodo = (id: number) => {
  const index = todoList.value.findIndex((todo) => todo.id === id)
  todoList.value.splice(index, 1)
}
</script>

<template>
  <div class="todo-list">
    <h1 class="green">Todo List</h1>
    <ul style="list-style: none">
      <li v-for="(todo) in todoList" :key="todo.id">
        <el-checkbox v-model="todo.done" size="large">
          {{ todo.text }}
        </el-checkbox>
        <el-button type="danger" @click="deleteTodo(todo.id)">Delete</el-button>
      </li>
    </ul>
    <el-input v-model="newTodo" @keyup.enter="addTodo" placeholder="Please input" />
    <el-button type="primary" @click="addTodo">Add</el-button>
  </div>
</template>
