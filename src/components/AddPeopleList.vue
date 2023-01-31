<template>
<div class="card inline"
    v-for="person in people"
    :key="person.id"
>

<h3>{{ person }}</h3>
<button class="btn danger" @click="$emit('remove', person.id)">Удалить</button>

</div>


<div class="card center">
    <h4 v-if="people.length == 0">Людей пока нет</h4>
    <p v-else>{{ people }}</p>
    <button class="btn" @click="$emit('load')">Загрузить список</button>
</div>
</template>

<script setup>
    const props = defineProps({
        people: Array
    })
</script>

<style scoped>
    .inline{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
</style>