<script setup>
import { ref, computed, onMounted, onUpdated, onUnmounted, watch } from 'vue'

import ChildComp from './ChildComp.vue'
import JSConfetti from 'js-confetti'

const confetti = new JSConfetti()

function showConfetti() {
  confetti.addConfetti()
}

// component logic
// declare some reactive state here.

const message = ref("As-salamu alaykum, world!");

console.log(message.value); // "Hello World!"
message.value = 'Changed';

const titleClass = ref('title');

const count = ref(0)

function increment() {
  // update component state
  count.value++
}

const text = ref('')

function onInput(e) {
  // a v-on handler receives the native DOM event
  // as the argument.
  text.value = e.target.value
}

const awesome = ref(true);

function toggle() {
  if (awesome.value) {
    awesome.value = false;
  } else {
    awesome.value = true;
  }
}





// give each todo a unique id
let id = 0

const newTodo = ref('')
const hideCompleted = ref(false)
const todos = ref([
{ id: id++, text: 'Learn HTML', done: true },
  { id: id++, text: 'Learn JavaScript', done: true },
  { id: id++, text: 'Learn Vue', done: false }
])

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value, done: false })
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}


const filteredTodos = computed(() => {
  return hideCompleted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})

// request a template ref - i.e. a reference to an element in the template
const pElementRef = ref(null)

onMounted(() => {
  // component is now mounted.

  pElementRef.value.textContent = 'mounted!'
})

const todoId = ref(1)
const todoData = ref(null)

const watchCount = ref(0)



async function fetchData() {
  todoData.value = null
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  todoData.value = await res.json()
}



watch(watchCount, (newCount) => {
  // yes, console.log() is a side effect
  console.log(`new count is: ${newCount}`)
})

const greeting = ref('Hello from parent')

const childMsg = ref('No child msg yet')

const msg = ref('from parent')

fetchData()
watch(todoId, fetchData)
showConfetti()

</script>

<template>
  <h1 :class="titleClass">{{ message.split('').reverse().join('') }}</h1>
  <p>Count is: {{ counter.count }}</p>

  <div v-bind:id="dynamicId"></div>
  <div :id="dynamicId"></div>

  <button v-on:click="increment">{{ count }}</button>
  <button @click="increment">{{ count }}</button>

  <input :value="text" @input="onInput" placeholder="Type here">
  <input v-model="text">

  <p>{{ text }}</p>

  <button @click="toggle">Toggle</button>
  <h2 v-if="awesome">Vue is awesome!</h2>
  <h2 v-else>Oh no 😢</h2>

  <form @submit.prevent="addTodo">
    <input v-model="newTodo" required placeholder="new todo">
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      <input type="checkbox" v-model="todo.done">
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : 'Hide completed' }}
  </button>

  <p ref="pElementRef">hello</p>

  <p>Todo id: {{ todoId }}</p>
  <button @click="todoId++" :disabled="!todoData">Fetch next todo</button>
  <p v-if="!todoData">Loading...</p>
  <pre v-else>{{ todoData }}</pre>

  <!-- render child component -->
  <ChildComp :msg="greeting" />
  <p>{{ childMsg }}</p>

  <ChildComp @response="(msg) => childMsg = msg" />

    <ChildComp>
  This is some slot content!
</ChildComp>

<h4 @click="showConfetti">🎉 Congratulations!</h4>
</template>

<style>
.title {
  color: red;
}

.done {
  text-decoration: line-through;
}

h4 {
  text-align: center;
  cursor: pointer;
  margin-top: 3em;
  user-select: none;
}
</style>