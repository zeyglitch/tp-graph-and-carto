<template>
  <div id="app">
    <h1>TP Graphiques et Cartographie</h1>

    <!-- Navigation entre les différentes étapes -->
    <nav class="navigation">
      <button
        v-for="onglet in onglets"
        :key="onglet.id"
        :class="{ actif: ongletActif === onglet.id }"
        @click="ongletActif = onglet.id"
      >
        {{ onglet.nom }}
      </button>
    </nav>

    <!-- Contenu en fonction de l'onglet sélectionné -->
    <div class="contenu">
      <LineChart v-if="ongletActif === 'ligne'" />
      <ProvenancePatients v-if="ongletActif === 'provenance'" />
      <SortiePatients v-if="ongletActif === 'sortie'" />
      <MotivationPatients v-if="ongletActif === 'motivation'" />
      <DensitePraticiens v-if="ongletActif === 'densite'" />
      <ECGChart v-if="ongletActif === 'ecg'" />
      <CarteCastres v-if="ongletActif === 'castres'" />
      <CarteBretagne v-if="ongletActif === 'bretagne'" />
      <CarteFrance v-if="ongletActif === 'france'" />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// Import des composants
import LineChart from './components/LineChart.vue'
import ProvenancePatients from './components/ProvenancePatients.vue'
import SortiePatients from './components/SortiePatients.vue'
import MotivationPatients from './components/MotivationPatients.vue'
import DensitePraticiens from './components/DensitePraticiens.vue'
import ECGChart from './components/ECGChart.vue'
import CarteCastres from './components/CarteCastres.vue'
import CarteBretagne from './components/CarteBretagne.vue'
import CarteFrance from './components/CarteFrance.vue'

// Liste des onglets
const onglets = [
  { id: 'ligne', nom: 'Étape 1 - Ligne' },
  { id: 'provenance', nom: 'A) Provenance' },
  { id: 'sortie', nom: 'B) Sorties' },
  { id: 'motivation', nom: 'C) Motivations' },
  { id: 'densite', nom: 'D) Densité' },
  { id: 'ecg', nom: 'ECG' },
  { id: 'castres', nom: 'Carte Castres' },
  { id: 'bretagne', nom: 'Carte Bretagne' },
  { id: 'france', nom: 'Carte France' },
]

// Onglet actif par défaut
const ongletActif = ref('ligne')
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
  background-color: #f5f5f5;
}

#app {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  text-align: center;
  margin-bottom: 20px;
  color: #333;
}

/* Navigation par onglets */
.navigation {
  display: flex;
  flex-wrap: wrap;
  gap: 5px;
  margin-bottom: 20px;
  padding: 10px;
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.navigation button {
  padding: 8px 16px;
  border: 1px solid #ddd;
  background: #fff;
  border-radius: 5px;
  cursor: pointer;
  font-size: 14px;
  transition: background 0.2s;
}

.navigation button:hover {
  background: #e8e8e8;
}

.navigation button.actif {
  background: #4CAF50;
  color: white;
  border-color: #4CAF50;
}

/* Zone de contenu */
.contenu {
  background: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h2 {
  color: #333;
  margin-bottom: 15px;
}

h3 {
  color: #555;
  margin: 15px 0 10px 0;
}
</style>
