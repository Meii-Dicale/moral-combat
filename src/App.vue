<template>
  <div id="app">
<Navbar />
<ModalAvantJeu @difficulte-choisie="onDifficulteChoisie" />
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
import ModalAvantJeu from './component/ModalAvantJeu.vue';

const combattants = ref([
  {id: 1, nom: "Lorelei", moral: 100, skill: 50, moralMax: 100, skillMax: 50, image: "./src/assets/lorelei.jpeg"},
  {id: 2, nom: "Julien", moral: 100, skill: 50, moralMax: 100, skillMax: 50, image: "./src/assets/image.jpg"}
]);

const competences = ref([
  {id: 1, nom: "🤭 Lancer une vanne", valuePV: 10, valueSkill: 5 ,sound: ["./src/sounds/rire1.wav", "./src/sounds/rire2.wav", "./src/sounds/rire3.wav"]},
  {id: 2, nom: "🤬 Insulter", valuePV: 15, valueSkill: 10, sound: ["./src/sounds/insulte1.mp3", "./src/sounds/insulte2.wav", "./src/sounds/insulte3.wav"]},
  {id: 3, nom: "🖕 Faire une doigt d'honneur", valuePV: 25, valueSkill: 20, sound: ["./src/sounds/prout1.mp3", "./src/sounds/prout2.mp3"]},
  {id: 4, nom: "🤔 Comparer les notes", valuePV: 20, valueSkill: 0, sound: ["./src/sounds/note1.mp3"]},
  {id: 5, nom: "🧘 Rester calme", valuePV: 0, valueSkill: 0, sound: ["./src/sounds/zen1.mp3", "./src/sounds/zen2.mp3"]},
]);

// Combattant actif (1 = joueur, 2 = ordinateur)
const activeCombattantId = ref(1);
const tour = ref(1);
const combatTermine = ref(false);
const compteurVictoire = ref(0);
const compteurDefaite = ref(0);
const difficulteChoisie = ref(false);

provide('combattants', combattants);
provide('activeCombattantId', activeCombattantId);
provide('combatTermine', combatTermine);
provide('compteurVictoire', compteurVictoire);
provide('compteurDefaite', compteurDefaite);
provide('tour', tour);
provide('difficulteChoisie', difficulteChoisie);

const appliquerCompetence = async (attaquantId, defenseurId, competence) => {
  const attaquant = combattants.value.find(c => c.id === attaquantId);
  const defenseur = combattants.value.find(c => c.id === defenseurId);

  if (attaquant && defenseur) {
    // Mettre à jour le combattant actif
    activeCombattantId.value = attaquantId;
    
    if (attaquant.skill >= competence.valueSkill) {
      // Jouer le son
      const audio = new Audio(competence.sound[Math.floor(Math.random() * competence.sound.length)]);
      audio.play();
      
      // Attendre que le son se joue (délai de 1.5 secondes pour les sons courts)
      await new Promise(resolve => {
        audio.onended = () => resolve();
        // Timeout de sécurité au cas où le son ne se termine pas
        setTimeout(() => resolve(), 2000);
      });
      
      // Appliquer les effets après le délai
      defenseur.moral = Math.max(0, defenseur.moral - competence.valuePV);
      attaquant.skill = Math.max(0, attaquant.skill - competence.valueSkill);
      if(attaquant.skill < 50) {
        attaquant.skill++;
      }
      checkWin();
      return true;
    }
  }
  return false;
};

const tourOrdinateur = async () => {
  if(combatTermine.value === false) {
    activeCombattantId.value = 2;
    
    const competenceAleatoire = competences.value[Math.floor(Math.random() * competences.value.length)];
    
    const succes = await appliquerCompetence(2, 1, competenceAleatoire);
    
    if (succes) {
      await new Promise(resolve => setTimeout(resolve, 500));
    } else {
      const competenceCalme = competences.value.find(c => c.id === 5);
      if (competenceCalme) {
        await appliquerCompetence(2, 1, competenceCalme);
        console.log(`L'ordinateur utilise: ${competenceCalme.nom}`);
        await new Promise(resolve => setTimeout(resolve, 500));
      }
    }
    
    activeCombattantId.value = 1;
    tour.value++;
  }
};

const handleCompetence = async (competence) => {
  if (!difficulteChoisie.value) {
    return; // Empêcher les actions si la difficulté n'est pas choisie
  }
  
  activeCombattantId.value = 1;
  
  const succes = await appliquerCompetence(1, 2, competence);
  
  if (succes) {
    await new Promise(resolve => setTimeout(resolve, 500));
    tourOrdinateur();
  } else {
    alert('Pas assez de skill pour utiliser cette compétence');
  }
};

const checkWin = () => {
  if (combattants.value.find(c => c.id === 1).moral <= 0) {
    alert('Vous avez perdu');
    combatTermine.value = true;
    compteurDefaite.value++;
  }
  if (combattants.value.find(c => c.id === 2).moral <= 0) {
    alert('Vous avez gagné');
    combatTermine.value = true;
    compteurVictoire.value++;
  } 
}

const onDifficulteChoisie = (difficulte) => {
  difficulteChoisie.value = true;
  console.log(`Difficulté choisie: ${difficulte}`);
}

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
