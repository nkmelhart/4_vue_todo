<template>
  <div class="min-h-screen flex flex-col justify-center items-center">
    <header class="flex flex-col w-full container">
      <h1 class="text-6xl font-medium text-gray-800 text-center">Nolan's To Do App</h1>
      <input class="text-3xl py-2 px-4 rounded-xl mt-8 text-gray-800" type="text" placeholder="New Todo"
      v-model="newTodo" 
      @change="addTodo"
      />
    </header>
    <main class="container w-full mt-8 space-y-8">
      <h4 class="text-center" v-if="todos.length==0">Enter an task to get the list started!</h4>
      <section class="space-y-4" v-if="pendingTodos.length>0">
        <h3 class="text-3xl text-green-800">Pending Items : {{pendingTodos.length}}</h3>
        <ul class="space-y-4">
          <li v-for="todo in pendingTodos" :key="todo.id" 
          class="bg-white rounded-xl text-2xl py-2 px-4 font-light text-center hover:text-white hover:bg-green-800 transition transform ease-in-out hover:scale-110 duration-500 cursor-pointer" 
          @click="changeStatus(todo.id)"
          >
          {{todo.text}}
          </li>
        </ul>
      </section>
      <section class="space-y-4" v-if="completedTodos.length>0">
        <h3 class="text-3xl text-red-800">Completed Items : {{completedTodos.length}}</h3>
        <ul class="space-y-4">
          <li v-for="todo in completedTodos" :key="todo.id" 
          class="bg-white rounded-xl text-2xl py-2 px-4 font-light text-center hover:text-white hover:bg-red-800 transition transform ease-in-out hover:scale-110 duration-500 cursor-pointer"
          @click="changeStatus(todo.id)"
          >
          {{todo.text}}
          </li>
        </ul>
      </section>
    </main>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTodo = ref("")

const todos = ref([])

const pendingTodos = computed(()=> todos.value.filter(todo=>todo.status === 'pending'))

const completedTodos = computed(()=> todos.value.filter(todo=>todo.status === 'done'))

const changeStatus = id => {
  todos.value.map(todo => {
    if(todo.id === id){
      todo.status = todo.status === "pending" ? 'done' : 'pending'
    }
  })
}

const addTodo = () => {
  if(newTodo.value.length > 0) {
    todos.value.push({
      id: Date.now(),
      text: newTodo.value,
      status: "pending",
    })
  }
  newTodo.value = '';
}
</script>
