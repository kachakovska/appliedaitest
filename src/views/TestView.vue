<script setup>
/* eslint-disable */
import { reactive, ref, computed } from "vue";

const counter = reactive({ count: 0 });
const message = ref("Hello World!");
const counterRef = ref(4);
const text = ref("");
const hideCompleted = ref(false);

function increment() {
  counter.count++;
  counterRef.value++;
}
function onInput(e) {
  text.value = e.target.value;
}

let id = 0;

const newTodo = ref("");
const todos = ref([
  { id: id++, text: "Learn HTML" },
  { id: id++, text: "Learn JavaScript" },
  { id: id++, text: "Learn Vue" },
]);

function addTodo() {
  if (newTodo.value) {
    todos.value.push({ id: id++, text: newTodo.value });
    newTodo.value = "";
  }
}

function removeTodo(todo) {
  console.log("JJ: ", todo);
  todos.value = todos.value.filter((t) => t.id !== todo.id);
}

const filteredTodos = computed(() => {
  return hideCompleted.value ? todos.value.filter((t) => !t.done) : todos.value;
});
</script>

<template>
  <h1>{{ message }}</h1>
  <p>Count is: {{ counter.count }}</p>
  <p>counterRef is: {{ counterRef }}</p>

  <input v-model="text" placeholder="Type here" />
  <br />
  <input :value="text" @input="onInput" />
  <p>{{ text }}</p>

  <button @click="increment">Click me</button>
  <br />
  <br />
  <input v-model="newTodo" @keyup.enter="addTodo" />
  <button @click="addTodo">Add Todo</button>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      {{ todo.text }}
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>

  <input v-model="newTodo" @keyup.enter="addTodo" />
  <button @click="addTodo">Add Todo</button>
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done" />
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? "Show all" : "Hide completed" }}
  </button>
</template>
