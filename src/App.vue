<template>
  <div class="todo-container">
    <div class="todo-app">
      <form @submit.prevent="addTodo" class="todo-form">
        <input v-model="newTodo" required placeholder="New todo" class="todo-input">
        <button type="submit" class="todo-button">Add Todo</button>
      </form>

      <ul class="todo-list">
        <li v-for="todo in filteredTodos" :key="todo.id" class="todo-item">
          <input
            v-if="editingTodo !== todo"
            type="checkbox"
            v-model="todo.done"
            class="todo-checkbox"
          >
          <span
            v-if="editingTodo !== todo"
            :class="{ done: todo.done }"
            class="todo-text"
            @dblclick="editTodoStart(todo)"
          >
            {{ todo.text }}
          </span>
          <input
            v-if="editingTodo === todo"
            v-model="todo.text"
            @keyup.enter="editTodoEnd(todo)"
            @blur="editTodoEnd(todo)"
            class="todo-edit-input"
          >
          <button @click="editTodoStart(todo)" class="edit-button">Edit</button>
          <button @click="removeTodo(todo)" class="delete-button">X</button>
        </li>
      </ul>

      <button @click="hideCompleted = !hideCompleted" class="toggle-completed">
        {{ hideCompleted ? 'Show all' : 'Hide completed' }}
      </button>
      <p>{{ pesan }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'

let id = 0

const newTodo = ref('')
const hideCompleted = ref(false)
const todos = ref([
  { id: id++, text: 'Watch Netflix', done: true },
  { id: id++, text: 'Playing Game', done: true },
  { id: id++, text: 'Learn Python', done: false }
])

const filteredTodos = computed(() => {
  return hideCompleted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})

const editingTodo = ref(null)

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value, done: false })
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t.id !== todo.id)
}

function editTodoStart(todo) {
  editingTodo.value = todo
}

function editTodoEnd(todo) {
  if (editingTodo.value === todo) {
    editingTodo.value = null
  }
}

const pesan = ref("")
watch(newTodo, (newTodoValue) => {
  if (newTodoValue.includes("?")) {
    pesan.value = "Tidak boleh mengandung ?";
  } else {
    pesan.value = "";
  }
})

</script>

<style>
.todo-container {
  display: flex;
  justify-content: center; 
  align-items: center; 
  min-height: 100vh; 
}

.todo-app {
  font-family: 'Arial', sans-serif;
  width: 100%;
  margin: 0 auto;
  padding: 20px;
  background-color: #f8f8f8;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.todo-form {
  display: flex;
  margin-bottom: 20px;
}

.todo-input {
  flex: 1;
  padding: 10px;
  border: 2px solid #62bd69;
  border-radius: 5px 0 0 5px;
  background-color: #f9f9f9;
}

.todo-button, .delete-button, .toggle-completed {
  padding: 10px 20px;
  border: none;
  cursor: pointer;
  transition: all 0.3s ease;
}

.todo-button {
  background: linear-gradient(to right, #6a11cb, #2575fc);
  color: white;
  border-radius: 0 5px 5px 0;
}

.todo-button:hover {
  background-color: #4ea955;
}

.delete-button {
  background: linear-gradient(to right, #6a11cb, #2575fc);
  color: white;
  border-radius: 50%;
  padding: 5px;
}

.delete-button:hover {
  background-color: #f24c3d;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
}

.toggle-completed {
  background: linear-gradient(to right, #6a11cb, #2575fc);
  color: white;
  border-radius: 5px;
  padding: 10px 20px;
}

.toggle-completed:hover {
  background-color: #4781d3;
}

.todo-list {
  list-style: none;
  padding: 0;
}

.todo-item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  background-color: #fff;
  border-radius: 10px;
  padding: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.todo-checkbox {
  margin-right: 10px;
}

.todo-text {
  flex: 1;
}

.todo-item:hover {
  transform: translateY(-2px);
}

.todo-checkbox:checked + .todo-text {
  transform: translateX(5px);
}

.done {
  text-decoration: line-through;
  color: #777;
}

.pesan {
  color: red;
  font-family: 'Courier New', monospace;
}

.todo-edit-input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.edit-button {
  padding: 8px 16px;
  background-color: #4CAF50; 
  color: white; 
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease; 
  margin-right: 5px;
}

.edit-button:hover {
  background-color: #45a049; 
}

</style>
