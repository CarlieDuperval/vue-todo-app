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

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime(), // we can create by createdAt
  });
   input_content.value = "";
  input_category.value = null;

  }

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};
// if the variable "todos" changes on submit, we will store the newVal in the localstorage
 
// if the variable "todos" changes on submit, we will store the newVal in the localstorage
watch(
  todos,
  (newVal) => {
    localStorage.setItem("todos", JSON.stringify(newVal)); //because it is an object we need to stringfy it
  },
  { deep: true } // because todos is an array, we need "deep" to look through the array
);
// if the variable "name" changes with the input, we will store the newVal in the localstorage
watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

 // we bring the value on the localStorage to the input. whenever the screen is refreshed we have it
onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});

</script>

<template>
<main class="app">
  <section class="greeting">
    <h2 class="title">
      What's up , <input type="text" placeholder="Name here" v-model="name" />
    </h2>

  </section>
  <section class="create-todo">
    <h3>Ceate a Todo</h3>
    <!-- Funtion Todo to handle the submit event -->
    <form @submit.prevent="addTodo">
    <h4>What's on your list?</h4>
    <input 
    type="text" 
    placeholder="e.g. make a video"  
    v-model="input_content" />
 <!-- {{input_content }} -->

    <h4>Pick a category</h4>

    <div class="options">
      <label>
        <input 
        type="radio" 
        name="category"  
        value="business" 
        v-model="input_category" />

 <!-- set the class bubble to business  --> 
        <span class="bubble business"></span>
        <div>Business</div>
      </label>

      <label>
        <input 
        type="radio" 
        name="category"  
        value="personal" 
        v-model="input_category" />
 <!-- set the class bubble to business  --> 

        <span class="bubble personal"></span>
        <div>Personal</div>
      </label>
      {{ input_category}}
    </div>
    <input type="submit" value="Add todo" />
    </form>
  </section>
  <section class="todo-list">
    <h4>Todo List</h4>
    <div class="list"> <!-- create a class that display every item  -->

     <!-- v-for = vue for loop / for every todo inside todos_ascOrder  -->
     <div 
      v-for="todo in todos_ascending"  
      :class="`todo-item ${todo.done && 'done'}`">
      <label>
            <input type="checkbox" v-model="todo.done" /> <!-- input will display the item and allow to be update -->
            <span :class="`bubble ${todo.category}`"></span>
          </label>
          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>
           <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Delete</button>   <!-- the delete button will call function removeTodo to delete the item from localstorage -->
          </div>
          </div>
    </div>
  </section>
</main>
</template>


