<template>
    <div class="d-flex align-center justify-space-around mt-5">
        <v-card v-for="combattant in combattants" :key="combattant.id">
    <v-card-title>
        <h1>{{combattant.nom}}</h1>
    </v-card-title>
    <v-card-text>
        <p>Moral : {{combattant.moral}}</p>
    </v-card-text>
    <v-card-text>
        <p>Skill restant :{{combattant.skill}}</p>
    </v-card-text>
</v-card>

    </div>

</template>

<script setup>
import { inject } from 'vue';

// Récupérer les combattants depuis le parent (App.vue)
const combattants = inject('combattants');
</script>