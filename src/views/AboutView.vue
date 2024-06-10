<script setup>
import { onMounted, ref } from 'vue'
import axios from 'axios'

let notes = ref([])
let vNote = ref()

const API_URL = 'http://localhost:5066/'

const refreshData = async() => {
  axios.get(API_URL + 'api/ToDoApp/GetNotes').then((response) => {
    notes.value = response.data
    console.log('DATA NEW', notes)
  })
}

const addNewNotes = async() => {
  const formData = new FormData();
  formData.append("newNotes", vNote.value)
  axios.post(API_URL + 'api/ToDoApp/AddNotes', formData).then(
    (response)=>{
      refreshData();
      alert(response.data)
  })
}

const deleteNotes = async(id) => {
  axios.delete(API_URL + 'api/ToDoApp/DeleteNotes?id='+id).then(
    (response)=>{
      refreshData();
      alert(response.data)
  })
}

onMounted(async() => {
  await refreshData()
})
</script>

<template>
  <div class="about">
    <h1>Todo</h1>
    <input id="newNotes" v-model="vNote"/>
    <button @click="addNewNotes()">Add Notes</button>
    <p v-for="item in notes">
      <b>{{ item.description }}</b>
      <button @click="deleteNotes(item.id)">Delete Notes</button>
    </p>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
