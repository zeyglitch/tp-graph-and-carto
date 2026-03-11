<template>
  <div id="app">
    <!-- En-tête du site -->
    <header class="entete">
      <h1>📊 TP Graphiques et Cartographie</h1>
      <p class="sous-titre">ISIS – Vue.js – ApexCharts & Leaflet</p>
    </header>

    <!-- Navigation entre les différentes étapes -->
    <nav class="navigation">
      <!-- Groupe Graphiques -->
      <div class="groupe-nav">
        <span class="groupe-label">📈 Graphiques</span>
        <div class="groupe-boutons">
          <button
            v-for="onglet in ongletsGraphiques"
            :key="onglet.id"
            :class="{ actif: ongletActif === onglet.id }"
            @click="ongletActif = onglet.id"
          >
            {{ onglet.nom }}
          </button>
        </div>
      </div>

      <!-- Groupe Cartographie -->
      <div class="groupe-nav">
        <span class="groupe-label">🗺️ Cartographie</span>
        <div class="groupe-boutons">
          <button
            v-for="onglet in ongletsCarto"
            :key="onglet.id"
            :class="{ actif: ongletActif === onglet.id }"
            @click="ongletActif = onglet.id"
          >
            {{ onglet.nom }}
          </button>
        </div>
      </div>
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

    <!-- Petit footer -->
    <footer class="pied-de-page">
      TP Informatique – Vue.js – ISIS Castres
    </footer>
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

// Onglets séparés en 2 groupes : graphiques et carto
const ongletsGraphiques = [
  { id: 'ligne', nom: 'Étape 1 – Ligne' },
  { id: 'provenance', nom: 'A) Provenance' },
  { id: 'sortie', nom: 'B) Sorties' },
  { id: 'motivation', nom: 'C) Motivations' },
  { id: 'densite', nom: 'D) Densité' },
  { id: 'ecg', nom: 'ECG temps réel' },
]

const ongletsCarto = [
  { id: 'castres', nom: 'Cabinets Castres' },
  { id: 'bretagne', nom: 'Bretagne' },
  { id: 'france', nom: 'France – Médecins' },
]

// Onglet actif par défaut
const ongletActif = ref('ligne')
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
}

#app {
  max-width: 1300px;
  margin: 0 auto;
  padding: 20px;
}

/* En-tête */
.entete {
  text-align: center;
  margin-bottom: 25px;
  padding: 30px 20px;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 16px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.12);
}

.entete h1 {
  font-size: 28px;
  font-weight: 700;
  color: #1a1a2e;
  margin-bottom: 5px;
}

.sous-titre {
  color: #888;
  font-size: 14px;
  font-weight: 400;
}

/* Navigation par onglets */
.navigation {
  display: flex;
  flex-direction: column;
  gap: 12px;
  margin-bottom: 20px;
}

.groupe-nav {
  background: rgba(255, 255, 255, 0.95);
  border-radius: 12px;
  padding: 12px 16px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.08);
}

.groupe-label {
  font-size: 13px;
  font-weight: 600;
  color: #555;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  display: block;
  margin-bottom: 8px;
}

.groupe-boutons {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
}

.groupe-boutons button {
  padding: 8px 18px;
  border: 2px solid #e0e0e0;
  background: #fff;
  border-radius: 8px;
  cursor: pointer;
  font-size: 13px;
  font-family: 'Inter', sans-serif;
  font-weight: 500;
  color: #444;
  transition: all 0.2s ease;
}

.groupe-boutons button:hover {
  background: #f0f0f0;
  border-color: #bbb;
  transform: translateY(-1px);
}

.groupe-boutons button.actif {
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
  border-color: transparent;
  box-shadow: 0 3px 12px rgba(102, 126, 234, 0.35);
}

/* Zone de contenu */
.contenu {
  background: rgba(255, 255, 255, 0.95);
  padding: 30px;
  border-radius: 16px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.12);
  min-height: 400px;
}

h2 {
  color: #1a1a2e;
  margin-bottom: 20px;
  font-size: 22px;
  font-weight: 600;
  border-bottom: 3px solid #667eea;
  padding-bottom: 10px;
  display: inline-block;
}

h3 {
  color: #555;
  margin: 20px 0 10px 0;
  font-size: 16px;
  font-weight: 600;
}

/* Footer */
.pied-de-page {
  text-align: center;
  margin-top: 25px;
  padding: 15px;
  color: rgba(255, 255, 255, 0.7);
  font-size: 13px;
}
</style>
