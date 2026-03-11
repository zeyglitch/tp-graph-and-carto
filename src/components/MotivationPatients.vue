<template>
  <div>
    <h2>C) Motivation des patients pour venir aux urgences</h2>
    <apexchart width="100%" height="450" :options="options" :series="donneesSeries"></apexchart>
  </div>
</template>

<script setup>
import apexchart from "vue3-apexcharts";

// Jeu de données motivations
const donneesSeries = [
  { name: 'Motivations',
    data: [
      { x: 'Accident', y: 31.9 },
      { x: 'Conseil médical', y: 26.0 },
      { x: 'État grave patient', y: 21.8 },
      { x: 'Angoisse patient', y: 9.8 },
      { x: 'Besoin régler rapidement', y: 27.3 },
      { x: 'Examen complémentaire', y: 23.1 },
      { x: 'Proximité urgences', y: 21.8 },
      { x: 'Consultation spécialiste', y: 11.9 },
      { x: 'Urgences plus rapides', y: 7.9 },
      { x: 'Médecin traitant absent', y: 5.9 },
      { x: 'Besoin examen sans RDV', y: 5.1 },
      { x: 'Amené par pompiers/SMUR', y: 11.3 },
    ]
  }
];

// Barres horizontales empilées
const options = {
  chart: {
    type: "bar",
    stacked: true, // empilement des barres
  },
  title: {
    text: 'Motivations des patients'
  },
  plotOptions: {
    bar: {
      horizontal: true,
      distributed: true,
    }
  },
  dataLabels: {
    enabled: true,
  },
};
</script>
