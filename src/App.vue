<template>
  <div id="app">
<Navbar />
<ZoneCombat />
<InterfaceCompetence />
<Statistiques />
  </div>
</template>

<script>
import InterfaceCompetence from './component/InterfaceCompetence.vue';
import Navbar from './component/Navbar.vue';
import Statistiques from './component/Statistiques.vue';
import ZoneCombat from './component/ZoneCombat.vue';


export default {
  name: 'App',
  components: {
    Navbar,
    ZoneCombat,
    InterfaceCompetence,
    Statistiques
  }
  
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
