<template>
  <div>
    <h2>ECG en temps réel (simulé)</h2>
    <apexchart height="350" width="80%" :options="optionsGraphique" :series="series"></apexchart>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import apexchart from "vue3-apexcharts";

const optionsGraphique = {
  chart: { type: "line", toolbar: { show: false } },
  stroke: { curve: "smooth", width: 2 },
  xaxis: { type: "numeric", tickAmount: 10 }, // nb de divisions sur l'axe X
  yaxis: { min: -2, max: 2 }, // amplitude simulée d'un ECG
  title: { text: "ECG en temps réel" },
  grid: { borderColor: "#e7e7e7", strokeDashArray: 5 },
};

// Donnée du graphique (initialisées vides)
const series = ref([{ name: "ECG", data: [] }]);

// Fonction pour générer un signal ECG simulé
const genererSignalECG = () => {
  return (Math.sin(Date.now() + Math.random() * 0.5) * 1.2 + (Math.random() * 0.2)).toFixed(3);
};

// Mise à jour dynamique du tracé ECG
onMounted(() => {
  let x = 0;
  setInterval(() => {
    const nouveauPoint = { x: x++, y: genererSignalECG() }; // un nouveau point
    if (series.value[0].data.length > 50) {
      series.value[0].data.shift(); // on supprime le point le plus ancien
    }
    series.value[0].data.push(nouveauPoint); // on ajoute le nouveau point
  }, 100); // mise à jour toutes les 100 ms
});
</script>

<style scoped></style>
