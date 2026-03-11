<template>
  <div>
    <h2>D) Évolution densité généralistes / spécialistes / pharmaciens / dentistes</h2>
    <apexchart width="700" :options="options" :series="series"></apexchart>
  </div>
</template>

<script setup>
import apexchart from "vue3-apexcharts";

// Données d'évolution sur 12 ans (2012-2023)
const series = [
  {
    name: "Densité des médecins généralistes",
    data: [
      { x: 2012, y: 161 }, { x: 2013, y: 155 }, { x: 2014, y: 155 },
      { x: 2015, y: 154 }, { x: 2016, y: 153 }, { x: 2017, y: 152 },
      { x: 2018, y: 152 }, { x: 2019, y: 152 }, { x: 2020, y: 151 },
      { x: 2021, y: 149 }, { x: 2022, y: 148 }, { x: 2023, y: 147 }
    ]
  },
  {
    name: "Densité des pharmaciens",
    data: [
      { x: 2012, y: 115 }, { x: 2013, y: 112 }, { x: 2014, y: 112 },
      { x: 2015, y: 113 }, { x: 2016, y: 112 }, { x: 2017, y: 111 },
      { x: 2018, y: 110 }, { x: 2019, y: 110 }, { x: 2020, y: 110 },
      { x: 2021, y: 109 }, { x: 2022, y: 109 }, { x: 2023, y: 108 }
    ]
  },
  {
    name: "Densité des médecins spécialistes",
    data: [
      { x: 2012, y: 182 }, { x: 2013, y: 178 }, { x: 2014, y: 179 },
      { x: 2015, y: 180 }, { x: 2016, y: 182 }, { x: 2017, y: 183 },
      { x: 2018, y: 184 }, { x: 2019, y: 185 }, { x: 2020, y: 187 },
      { x: 2021, y: 189 }, { x: 2022, y: 191 }, { x: 2023, y: 193 }
    ]
  },
  {
    name: "Densité des dentistes",
    data: [
      { x: 2012, y: 64 }, { x: 2013, y: 62 }, { x: 2014, y: 63 },
      { x: 2015, y: 63 }, { x: 2016, y: 63 }, { x: 2017, y: 63 },
      { x: 2018, y: 63 }, { x: 2019, y: 63 }, { x: 2020, y: 64 },
      { x: 2021, y: 64 }, { x: 2022, y: 65 }, { x: 2023, y: 67 }
    ]
  }
];

// Options du graphique en ligne
const options = {
  chart: {
    type: "line",
  },
  stroke: {
    curve: 'smooth',
    width: 3,
  },
  markers: {
    size: 4,
  },
  title: {
    text: 'Évolution de la densité de praticiens (pour 100 000 hab.)'
  },
  xaxis: {
    title: { text: 'Année' }
  },
  yaxis: {
    title: { text: 'Densité pour 100 000 habitants' }
  },
};
</script>
