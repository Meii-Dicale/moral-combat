<template>
    <div v-if="afficherModal" class="modal-avant-jeu">
        <v-card class="modal-content">
            <v-card-title class="text-h4 mb-4">Choisissez la difficulté</v-card-title>
            <v-card-text>
                <div class="d-flex flex-column gap-4">
                    <v-btn 
                        color="success" 
                        size="large" 
                        @click="choisirDifficulte('facile')"
                        class="difficulte-btn"
                    >
                        <div class="d-flex flex-column align-center">
                            <span class="text-h5">Facile</span>
                            <span class="text-caption">Joueur: 150 PV / 70 Skill | Ordinateur: 80 PV / 30 Skill</span>
                        </div>
                    </v-btn>
                    
                    <v-btn 
                        color="primary" 
                        size="large" 
                        @click="choisirDifficulte('moyen')"
                        class="difficulte-btn"
                    >
                        <div class="d-flex flex-column align-center">
                            <span class="text-h5"> Moyen</span>
                            <span class="text-caption">Joueur: 100 PV / 50 Skill | Ordinateur: 100 PV / 50 Skill</span>
                        </div>
                    </v-btn>
                    
                    <v-btn 
                        color="error" 
                        size="large" 
                        @click="choisirDifficulte('difficile')"
                        class="difficulte-btn"
                    >
                        <div class="d-flex flex-column align-center">
                            <span class="text-h5">Difficile</span>
                            <span class="text-caption">Joueur: 80 PV / 30 Skill | Ordinateur: 150 PV / 70 Skill</span>
                        </div>
                    </v-btn>
                </div>
            </v-card-text>
        </v-card>
    </div>
</template>

<script setup>
import { ref, inject, watch } from 'vue';

const emit = defineEmits(['difficulte-choisie']);

const afficherModal = ref(true);
const combattants = inject('combattants');
const difficulteChoisie = inject('difficulteChoisie');

// Surveiller difficulteChoisie pour réafficher la modale si elle redevient false
watch(difficulteChoisie, (nouvelleValeur) => {
    if (!nouvelleValeur) {
        afficherModal.value = true;
    }
});

const difficulteConfig = {
    facile: {
        joueur: { moral: 150, skill: 70 },
        ordinateur: { moral: 80, skill: 30 }
    },
    moyen: {
        joueur: { moral: 100, skill: 50 },
        ordinateur: { moral: 100, skill: 50 }
    },
    difficile: {
        joueur: { moral: 80, skill: 30 },
        ordinateur: { moral: 150, skill: 70 }
    }
};

const choisirDifficulte = (difficulte) => {
    const config = difficulteConfig[difficulte];
    
    // Appliquer les valeurs au joueur (id: 1)
    const joueur = combattants.value.find(c => c.id === 1);
    if (joueur) {
        joueur.moral = config.joueur.moral;
        joueur.skill = config.joueur.skill;
        joueur.moralMax = config.joueur.moral;
        joueur.skillMax = config.joueur.skill;
    }
    
    // Appliquer les valeurs à l'ordinateur (id: 2)
    const ordinateur = combattants.value.find(c => c.id === 2);
    if (ordinateur) {
        ordinateur.moral = config.ordinateur.moral;
        ordinateur.skill = config.ordinateur.skill;
        ordinateur.moralMax = config.ordinateur.moral;
        ordinateur.skillMax = config.ordinateur.skill;
    }
    
    afficherModal.value = false;
    emit('difficulte-choisie', difficulte);
};
</script>

<style scoped>
.modal-avant-jeu {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.7);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1000;
}

.modal-content {
    min-width: 500px;
    max-width: 600px;
    padding: 24px;
}

.difficulte-btn {
    width: 100%;
    min-height: 80px;
    padding: 16px !important;
}

.difficulte-btn :deep(.v-btn__content) {
    width: 100%;
}
</style>