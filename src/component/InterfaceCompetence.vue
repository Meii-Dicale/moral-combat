<template>
    <div class="d-flex flex-row align-center justify-space-around mt-5">
        <v-btn v-for="competence in competences" :key="competence.id">
            {{competence.nom}}
        </v-btn>
    </div>
</template>

<script setup >
import { ref } from 'vue';
const competences = ref([
    {id: 1, nom: "🤭 Lancer une vanne", value: 10},
    {id: 2, nom: "🤬 Insulter", value: 15},
    {id: 3, nom: "🖕 Faire une doigt d'honneur", value: 20},
    {id: 4, nom: "🤔 Comparer les notes", value: 20},
])
</script>