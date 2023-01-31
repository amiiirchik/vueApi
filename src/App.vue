<template>
<div class="container">
  <AppAlert :alert="alert" />
  <form class="card" @submit.prevent="createPerson">
    <h2>Работа с базой данных</h2>
    <div class="card">
      <label for="name">Введите имя</label>
      <input name="" id="name" v-model.trim="name">
    </div>
    <button class="btn primary" :disabled="name.length === 0">Создать</button>
    <AppLoader
      v-if="loading"

    >
    </AppLoader>
  </form>
  <AddPeopleList :people="people" @load="loadPeople" @remove="removePerson"/>
</div>
</template>

<script setup>
import { ref, reactive } from 'vue'
import axios from 'axios'
import AddPeopleList from './components/AddPeopleList.vue'
import AppAlert from './components/AppAlert.vue'
import AppLoader from './components/AppLoader.vue'

let name = ref('')
let people = reactive([])
let loading = ref()

async function createPerson(){
  const response = await fetch('https://database-a2210-default-rtdb.firebaseio.com/people.json', {
    method: 'POST',
    headers:{
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      firstName: name,
    })
  })

  const fireBaseData = await response.json()

  people.push({
    firstName: name,
    id: fireBaseData.name
  })
  
  name.value = ''
}

async function loadPeople(){
  try{
    loading = true

    const { response } = await axios.get('https://database-a2210-default-rtdb.firebaseio.com/people.json')
    
    if(!response) throw new Error('Список пуст')

    people = Object.keys(response).map(key => {
      id: key
      firstName: response[key].firstName
    })
    loading = false
  } 
  catch (e){
    alert = {
      type: 'danger',
      title: 'Ошибка',
      text: e.message,
    }
    console.log(e)
  }
  
  
  console.log(people)
}

async function removePerson(id){
  try {
    let name = people.find(person => person.id == id).firstName
    await axios.delete(`https://database-a2210-default-rtdb.firebaseio.com/people/${id}.json`)
    people = people.filter(person => person.id != id)

    alert = {
      type: 'primary',
      title: 'Успешно',
      text: `Пользователь с именем ${name}`
    }
  } catch(e) {
    alert('Alert')
  }




}

</script>