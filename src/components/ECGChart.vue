<template>
  <div>
    <h2>ECG en temps réel (simulé)</h2>
    <div class="ecg-conteneur">
      <apexchart height="350" width="100%" :options="optionsGraphique" :series="series"></apexchart>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import apexchart from "vue3-apexcharts";

const optionsGraphique = {
  chart: {
    type: "line",
    toolbar: { show: false },
    animations: { enabled: true, easing: 'linear', dynamicAnimation: { speed: 100 } },
    background: '#1a1a2e',
  },
  colors: ['#00ff88'],
  stroke: { curve: "smooth", width: 2 },
  xaxis: {
    type: "numeric",
    tickAmount: 10,
    labels: { style: { colors: '#888' } }
  },
  yaxis: {
    min: -2, max: 2,
    labels: { style: { colors: '#888' } }
  },
  title: {
    text: "ECG en temps réel",
    style: { color: '#ccc', fontSize: '16px', fontFamily: 'Inter, sans-serif' }
  },
  grid: { borderColor: "#333", strokeDashArray: 3 },
  tooltip: { theme: 'dark' },
};

// Donnée du graphique (initialisées vides)
const series = ref([{ name: "ECG", data: [] }]);

// On garde l'id de l'intervalle pour le nettoyer quand on quitte
let intervalId = null

// Fonction pour générer un signal ECG simulé
const genererSignalECG = () => {
  return (Math.sin(Date.now() + Math.random() * 0.5) * 1.2 + (Math.random() * 0.2)).toFixed(3);
};

// Mise à jour dynamique du tracé ECG
onMounted(() => {
  let x = 0;
  intervalId = setInterval(() => {
    const nouveauPoint = { x: x++, y: genererSignalECG() }; // un nouveau point
    if (series.value[0].data.length > 50) {
      series.value[0].data.shift(); // on supprime le point le plus ancien
    }
    series.value[0].data.push(nouveauPoint); // on ajoute le nouveau point
  }, 100); // mise à jour toutes les 100 ms
});

// On arrête l'intervalle quand on change d'onglet
onUnmounted(() => {
  if (intervalId) clearInterval(intervalId)
})
</script>

<style scoped>
.ecg-conteneur {
  background: #1a1a2e;
  border-radius: 12px;
  padding: 15px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
}
</style>
