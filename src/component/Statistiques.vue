<template>
    <div class="d-flex flex-row align-center justify-space-around mt-5">
        <v-card>
            <v-card-title class="d-flex flex-row align-center justify-space-between">
                <h1 class="mr-5">Statistiques</h1>
                <p class="mr-5">Tour : {{ tour }}</p>
                <p class="mr-5">Victoires : {{ compteurVictoire }}</p>
                <p class="mr-5">Défaites : {{ compteurDefaite }}</p>
                <v-btn v-if="combatTermine" @click="resetCombat">Recommencer</v-btn>
            </v-card-title>
        </v-card>


    </div>
</template>

<script setup>
import { ref, inject } from 'vue';
const combatTermine = inject('combatTermine');
const activeCombattantId = inject('activeCombattantId');
const combattants = inject('combattants');
const tour = inject('tour');
const compteurVictoire = inject('compteurVictoire');
const compteurDefaite = inject('compteurDefaite');


const resetCombat = () => {
    combattants.value.forEach(combattant => {
        combattant.moral = 100;
        combattant.skill = 50;
    });
    activeCombattantId.value = 1;
    combatTermine.value = false;
}
</script>