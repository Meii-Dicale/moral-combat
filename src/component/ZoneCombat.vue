<template>
    <div class="d-flex align-center justify-space-around mt-5">
        <v-card v-for="combattant in combattants" :key="combattant.id" 
        class="combattant-card"
        :class="{ 'active': combattant.id === activeCombattantId }"
        >
           <div class="d-flex flex-column align-center justify-space-between">
            <v-card-title>
                <h1>{{combattant.nom}}</h1>
            </v-card-title>
            <v-img :src="combattant.image" alt="Image du combattant" height="200" width="200"></v-img>
        </div> 
            <v-card-text>
                <div class="mb-4">
                    <div class="d-flex justify-space-between mb-2">
                        <span><strong>Moral</strong></span>
                        <span>{{combattant.moral}} / 100</span>
                    </div>
                    <v-progress-linear
                        :model-value="combattant.moral"
                        :max="100"
                        color="success"
                        height="25"
                        rounded
                    >
                        <template v-slot:default="{ value }">
                            <strong>{{ Math.ceil(value) }}%</strong>
                        </template>
                    </v-progress-linear>
                </div>
                <div>
                    <div class="d-flex justify-space-between mb-2">
                        <span><strong>Skill</strong></span>
                        <span>{{combattant.skill}} / 50</span>
                    </div>
                    <v-progress-linear
                        :model-value="combattant.skill"
                        :max="50"
                        color="primary"
                        height="25"
                        rounded
                    >
                        <template v-slot:default="{ value }">
                            <strong>{{ Math.ceil(value) }}%</strong>
                        </template>
                    </v-progress-linear>
                </div>
            </v-card-text>
        </v-card>
    </div>
</template>

<script setup>
import { inject } from 'vue';

const combattants = inject('combattants');
const activeCombattantId = inject('activeCombattantId');
</script>

<style scoped>
.combattant-card {
    min-width: 300px;
    margin: 0 10px;
}

.combattant-card.active {
    border: 4px solid #1976d2;
    box-shadow: 0 4px 8px rgba(25, 118, 210, 0.3);
}
</style>