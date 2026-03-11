<template>
  <div>
    <h2>B) Sortie des patients aux urgences</h2>
    <apexchart width="600" :options="options" :series="donneesSeries"></apexchart>
  </div>
</template>

<script setup>
import apexchart from "vue3-apexcharts";

// Jeu de données sorties
const donneesSeries = [
  { name: 'Sorties',
    data: [
      { x: 'Retour à domicile', y: 75.7 },
      { x: 'Retour en HAD', y: 0.6 },
      { x: 'Hospitalisation', y: 19.9 },
      { x: 'Retour en EHPAD', y: 0.9 },
      { x: 'Décès', y: 0.1 },
      { x: 'Sortie contre avis médical', y: 0.5 },
      { x: 'Vers une maison médicale de garde (MMG)', y: 0.2 },
      { x: 'Parti sans attendre', y: 2.0 },
    ]
  }
];

const options = {
  chart: {
    type: "bar",
  },
  title: {
    text: 'Sortie des patients aux urgences'
  },
  plotOptions: {
    bar: {
      distributed: true,
    }
  },
  dataLabels: {
    enabled: true,
  },
};
</script>
