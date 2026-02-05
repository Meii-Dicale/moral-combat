<template>
  <div id="app">
<Navbar />
<ZoneCombat />
<InterfaceCompetence @competence-utilisee="handleCompetence" />
<Statistiques />
  </div>
</template>

<script setup>
import { ref, provide } from 'vue';
import InterfaceCompetence from './component/InterfaceCompetence.vue';
import Navbar from './component/Navbar.vue';
import Statistiques from './component/Statistiques.vue';
import ZoneCombat from './component/ZoneCombat.vue';

const combattants = ref([
  {id: 1, nom: "Lorelei", moral: 100, skill: 50, image: "./src/assets/lorelei.jpeg"},
  {id: 2, nom: "Julien", moral: 100, skill: 50, image: "./src/assets/image.jpg"}
]);

const competences = ref([
  {id: 1, nom: "🤭 Lancer une vanne", valuePV: 10, valueSkill: 10},
  {id: 2, nom: "🤬 Insulter", valuePV: 15, valueSkill: 15},
  {id: 3, nom: "🖕 Faire une doigt d'honneur", valuePV: 20, valueSkill: 20},
  {id: 4, nom: "🤔 Comparer les notes", valuePV: 20, valueSkill: 20},
  {id: 5, nom: "🧘 Rester calme", valuePV: 0, valueSkill: 0},
]);

// Combattant actif (1 = joueur, 2 = ordinateur)
const activeCombattantId = ref(1);

provide('combattants', combattants);
provide('activeCombattantId', activeCombattantId);

const appliquerCompetence = (attaquantId, defenseurId, competence) => {
  const attaquant = combattants.value.find(c => c.id === attaquantId);
  const defenseur = combattants.value.find(c => c.id === defenseurId);

  if (attaquant && defenseur) {
    // Mettre à jour le combattant actif
    activeCombattantId.value = attaquantId;
    
    if (attaquant.skill >= competence.valueSkill) {
      defenseur.moral = Math.max(0, defenseur.moral - competence.valuePV);
      attaquant.skill = Math.max(0, attaquant.skill - competence.valueSkill);
      attaquant.skill++
      return true;
    }
  }
  return false;
};

const tourOrdinateur = () => {
  // Mettre à jour le combattant actif pour l'ordinateur
  activeCombattantId.value = 2;
  
  const competenceAleatoire = competences.value[Math.floor(Math.random() * competences.value.length)];
  
  const succes = appliquerCompetence(2, 1, competenceAleatoire);
  
  if (succes) {
  } else {
    const competenceCalme = competences.value.find(c => c.id === 5);
    if (competenceCalme) {
      appliquerCompetence(2, 1, competenceCalme);
      console.log(`L'ordinateur utilise: ${competenceCalme.nom}`);
    }
  }
  
  setTimeout(() => {
    activeCombattantId.value = 1;
  }, 1000);
};

const handleCompetence = (competence) => {
  activeCombattantId.value = 1;
  
  const succes = appliquerCompetence(1, 2, competence);
  
  if (succes) {

      tourOrdinateur();

  } else {
    alert('Pas assez de skill pour utiliser cette compétence');
  }
};
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}
</style>
