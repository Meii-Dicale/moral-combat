<template>
    <div class="d-flex align-center justify-space-around mt-5">
        <v-card v-for="combattant in combattants" :key="combattant.id">
    <v-card-title>
        <h1>{{combattant.nom}}</h1>
    </v-card-title>
    <v-card-text>
        <p>{{combattant.moral}}</p>
    </v-card-text>
</v-card>

    </div>

</template>

<script setup>
import { ref } from 'vue';

const combattants = ref([
    {id: 1, nom: "Lorelei", moral: 100, image: ""},
    {id: 2, nom: "Julien", moral: 100, image: ""}
])
</script>