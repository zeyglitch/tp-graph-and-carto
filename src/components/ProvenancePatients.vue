<template>
  <div>
    <h2>A) Provenance des patients aux urgences</h2>

    <h3>Diagramme en barres</h3>
    <apexchart width="600" :options="optionsBarres" :series="donneesSeries"></apexchart>

    <h3>Diagramme en barres horizontales</h3>
    <apexchart width="600" :options="optionsBarresHorizontales" :series="donneesSeries"></apexchart>

    <h3>Camembert</h3>
    <apexchart width="500" :options="optionsCamembert" :series="valeursCamembert"></apexchart>
  </div>
</template>

<script setup>
import apexchart from "vue3-apexcharts";

// Jeu de données provenance
const donneesSeries = [
  { name: 'Provenance',
    data: [
      { x: 'Domicile', y: 65 },
      { x: 'Voie publique, lieu de travail ou école', y: 19 },
      { x: 'Établissement de santé', y: 1 },
      { x: 'Structure médico-sociale', y: 2 },
      { x: 'Maison médicale de garde (MMG)', y: 0 },
      { x: 'Autre provenance', y: 3 },
      { x: 'Non-réponse et autre', y: 9 },
    ]
  }
];

// Options pour le graphique en barres verticales
const optionsBarres = {
  chart: {
    type: "bar",
  },
  title: {
    text: 'Provenance des patients aux urgences'
  },
  plotOptions: {
    bar: {
      distributed: true, // une couleur par barre
    }
  },
  dataLabels: {
    enabled: true,
  },
};

// Options pour le graphique en barres horizontales
const optionsBarresHorizontales = {
  chart: {
    type: "bar",
  },
  title: {
    text: 'Provenance des patients aux urgences'
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

// Pour le camembert il faut structurer les données autrement
// un tableau qui contient les valeurs et un autre les étiquettes
const valeursCamembert = donneesSeries[0].data.map(item => item.y)
const labelsCamembert = donneesSeries[0].data.map(item => item.x)

const optionsCamembert = {
  chart: {
    type: "pie", // type camembert
  },
  labels: labelsCamembert,
  title: {
    text: 'Provenance des patients aux urgences'
  },
};
</script>
