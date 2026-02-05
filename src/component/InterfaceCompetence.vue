<template>
    <div class="d-flex flex-row align-stretch justify-space-around mt-5">
        <v-btn  v-for="competence in competences" :key="competence.id" @click="utiliserCompetence(competence.id)" 
                :disabled="activeCombattantId === 2 || combatTermine || !difficulteChoisie" 
                class="competence-btn"
                :class="{ 'disabled-btn': activeCombattantId === 2 || combatTermine || !difficulteChoisie }">
            <div class="d-flex flex-column align-center justify-center">
                <h3>{{competence.nom}}</h3>
                <p>PV : {{competence.valuePV}}</p>
                <p>Skill : {{competence.valueSkill}}</p>
            </div>
        </v-btn>
    </div>
</template>

<script setup >
import { ref, inject } from 'vue';

const emit = defineEmits(['competence-utilisee']);
const activeCombattantId = inject('activeCombattantId');
const combatTermine = inject('combatTermine');
const difficulteChoisie = inject('difficulteChoisie');

const competences = ref([
  {id: 1, nom: "🤭 Lancer une vanne", valuePV: 10, valueSkill: 5 ,sound: ["./src/sounds/rire1.wav", "./src/sounds/rire2.wav", "./src/sounds/rire3.wav"]},
  {id: 2, nom: "🤬 Insulter", valuePV: 15, valueSkill: 10, sound: ["./src/sounds/insulte1.mp3", "./src/sounds/insulte2.wav", "./src/sounds/insulte3.wav"]},
  {id: 3, nom: "🖕 Faire une doigt d'honneur", valuePV: 25, valueSkill: 20, sound: ["./src/sounds/prout1.mp3", "./src/sounds/prout2.mp3"]},
  {id: 4, nom: "🤔 Comparer les notes", valuePV: 20, valueSkill: 0, sound: ["./src/sounds/note1.mp3"]},
  {id: 5, nom: "🧘 Rester calme", valuePV: 0, valueSkill: 0, sound: ["./src/sounds/zen1.mp3", "./src/sounds/zen2.mp3"]},
]);

const utiliserCompetence = (id) => {
    if (activeCombattantId.value === 2 || combatTermine.value || !difficulteChoisie.value) {
        return; 
    }
    const competence = competences.value.find(c => c.id === id);
    if (competence) {
        emit('competence-utilisee', competence);
    }
}
</script>

<style scoped>
.competence-btn {
    height: 100% !important;
    min-height: 150px;
    padding: 16px !important;
}

.competence-btn :deep(.v-btn__content) {
    height: 100%;
    width: 100%;
}

.disabled-btn {
    opacity: 0.5 !important;
    cursor: not-allowed !important;
    filter: grayscale(100%) !important;
}

.disabled-btn :deep(.v-btn__content) {
    pointer-events: none !important;
}
</style>