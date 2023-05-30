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
  <div class="todo-list">
    <h1 class="green">Todo List</h1>
    <ul style="list-style: none">
      <li v-for="(todo, index) in todoList" :key="index">
        <el-checkbox v-model="todo.done" size="large">
          {{ todo.text }}
        </el-checkbox>
        <el-button type="danger" @click="deleteTodo(index)">Delete</el-button>
      </li>
    </ul>
    <el-input v-model="newTodo" @keyup.enter="addTodo" placeholder="Please input" />
    <el-button type="primary" @click="addTodo">Add</el-button>
  </div>
</template>
