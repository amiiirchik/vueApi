<template>
<div class="card center"
    v-if="people.lenght !== 0"
    v-for="person in people"
    :key="person"
>

<h3>{{ person }}</h3>

</div>


<div class="card center">
    <h4>Людей пока нет</h4>
    <button class="btn" @click="$emit('load')">Загрузить список</button>
</div>
</template>

<script setup>
    const props = defineProps({
        people: Array
    })
</script>