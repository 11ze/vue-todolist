<script setup lang="ts">
import { ref, watch } from 'vue'

export interface Todo {
  id: number
  text: string
  done: boolean
}

const getTodoList = (): Todo[] => {
  const todoList = localStorage.getItem('vue-todoList')
  if (todoList) {
    return JSON.parse(todoList)
  }
  return []
}

const setTodoList = (todoList: Todo[]) => {
  localStorage.setItem('vue-todoList', JSON.stringify(todoList))
}

const todoList = ref(getTodoList())

const newTodo = ref('')

const maxId = () => {
  if (todoList.value.length === 0) return 0
  return todoList.value.reduce((prev, current) => {
    return prev.id > current.id ? prev : current
  }).id
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
  if (index === -1) return
  todoList.value.splice(index, 1)
}

const watchTodoList = () => {
  setTodoList(todoList.value)
}
watch(todoList, watchTodoList, { deep: true })
</script>

<template>
  <div class="todo-list">
    <h1 class="green">Todo List</h1>
    <ul style="list-style: none">
      <li v-for="todo in todoList" :key="todo.id">
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
