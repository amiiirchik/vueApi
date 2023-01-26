<template>
<div class="container">
  <form class="card" @submit.prevent="createPerson">
    <h2>Работа с базой данных</h2>
    <div class="card">
      <label for="name">Введите имя</label>
      <input name="" id="name" v-model.trim="name">
    </div>
    <button class="btn primary" :disabled="name.length === 0">Создать</button>
  </form>
  <AddPeopleList :people="people" />
</div>
</template>

<script setup>
import { ref } from 'vue'
import AddPeopleList from './components/AddPeopleList.vue'
let name = ref('')
console.log(name.value)

async function createPerson(){
  const response = await fetch('https://database-a2210-default-rtdb.firebaseio.com/people.json', {
    method: 'POST',
    headers:{
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      firstName: name
    })
  })

  const firebaseData = await response.json()
  console.log(firebaseData)
  name.value = ''

}


</script>

<style>

</style>
