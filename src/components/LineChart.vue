<template>
  <div>
    <h2>Graphique en ligne - Ventes / Achats</h2>
    <apexchart width="100%" height="400" :options="options" :series="series"></apexchart>
  </div>
</template>

<script setup>
import apexchart from "vue3-apexcharts";

// les series de données à afficher
const series = [
  {
    name: "Ventes",
    data: [
      { x: "Jan", y: 10 }, { x: "Fév", y: 20 }, { x: "Mar", y: 15 },
      { x: "Avr", y: 25 }, { x: "Mai", y: 30 }, { x: "Juin", y: 40 }
    ]
  },
  {
    name: "Achats",
    data: [
      { x: "Jan", y: 60 }, { x: "Fév", y: 20 }, { x: "Mar", y: 30 },
      { x: "Avr", y: 10 }, { x: "Mai", y: 50 }, { x: "Juin", y: 10 }
    ]
  }
];

// les options du graphique
const options = {
  chart: {
    type: "line", // type de graphique
  },
  colors: ['#667eea', '#f093fb'], // couleurs personnalisées
  stroke: {
    curve: 'smooth', // type de ligne
    width: 3,
  },
  markers: {
    size: 5, // taille des marqueurs à chaque point
  },
  title: {
    text: 'Ventes vs Achats (Janvier - Juin)',
    align: 'center',
    style: { fontSize: '16px', fontFamily: 'Inter, sans-serif' }
  },
};
</script>
