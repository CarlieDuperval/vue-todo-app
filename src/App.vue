<script setup>
// import: ref to handle the state
// onMounted: use like a local storage when we first render tha page
// computed :to  have the data in order , will order the list by created date
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([]) // creat an empty array to store todo items
const name = ref('') // user name
const input_content = ref('') // is the field for the content of todo
const input_category = ref (null) // is the feild for the category of todo 

const todos_ascending = computed(
  () => 
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  }) // create at date
  )

  const addTodo = () => {
    if(input_content.value.trim() === "" || input_category === null){
      return
    }
  }

  watch( name, (newVal) => {
    localStorage.setItem('name' , newVal)
  })

  onMounted(() => {
    name.value = localStorage.getItem('name') || ''
  })

</script>

<template>
<main class="app">
  <section class="greeting">
    <h2 class="title">
      What's up , <input type="text" placeholder="Name here" v-model="name" />
    </h2>

  </section>
  <section class="create-todo">
    <h3>CREATE A TODO</h3>
    <form @submit.prevent="addTodo">
    <h4>What's on your list?</h4>
    <input type="text" placeholder="e.g. make a video"  v-model="input_content">

    {{input_content }}
    </form>
  </section>

</main>
</template>

