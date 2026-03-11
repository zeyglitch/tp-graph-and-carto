<template>
  <div>
    <h2>E) Densité de médecins généralistes par département (2023)</h2>
    <!-- on peut changer le type de praticien affiché -->
    <div style="margin: 10px 20px;">
      <label for="selectType">Type de praticien : </label>
      <select id="selectType" v-model="typePraticien">
        <option value="RatioGeneralistes">Généralistes</option>
        <option value="RatioSpecialistes">Spécialistes</option>
        <option value="RatioDentistes">Dentistes</option>
        <option value="RatioPharmaciens">Pharmaciens</option>
      </select>
    </div>
    <div id="mapFrance" class="map-container">
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";

// --- importer Leaflet et le css associé
import L from "leaflet";
import "leaflet/dist/leaflet.css";

// -- Fichier GeoJSON de tous les départements de France
import geojsonFrance from "../assets/departementsFrance.json";

// -- Données de densité des médecins par département
import { dataMedecins } from "../data/dataMedecins.js";

// Type de praticien sélectionné
const typePraticien = ref("RatioGeneralistes")

// Référence vers la carte et la couche GeoJSON
let map = null
let coucheGeoJSON = null

// -- Fonction pour colorer les départements en fonction de la densité
// l'échelle de couleurs dépend du type de praticien
const getCouleur = (ratio) => {
  return ratio > 200 ? "#006d2c" :
    ratio > 160 ? "#31a354" :
    ratio > 130 ? "#74c476" :
    ratio > 100 ? "#a1d99b" :
    ratio > 80 ? "#c7e9c0" :
    ratio > 60 ? "#e5f5e0" :
    "#f7fcf5";
};

// Met à jour la couche GeoJSON sur la carte
function mettreAJourCarte() {
  // On enlève l'ancienne couche si elle existe
  if (coucheGeoJSON) {
    map.removeLayer(coucheGeoJSON)
  }

  coucheGeoJSON = L.geoJSON(geojsonFrance, {
    style: (feature) => {
      const codeDept = feature.properties.code
      // On cherche les données du département dans le tableau
      const deptData = dataMedecins.find((v) => v.code == codeDept)
      const ratio = deptData ? parseInt(deptData[typePraticien.value]) : 0
      return {
        fillColor: getCouleur(ratio),
        weight: 1, opacity: 1, color: "white", fillOpacity: 0.7,
      }
    },
    onEachFeature: (feature, layer) => {
      const codeDept = feature.properties.code
      const deptData = dataMedecins.find((v) => v.code == codeDept)
      const ratio = deptData ? deptData[typePraticien.value] : "N/A"
      const nomDept = deptData ? deptData.nomdept : feature.properties.nom
      layer.bindTooltip(`${nomDept} (${codeDept}): ${ratio} pour 100 000 hab.`)
    }
  }).addTo(map);
}

// -- affiche la carte de la France
function afficheCarto() {
  // Initialisation de la carte avec leaflet
  map = L.map('mapFrance').setView([46.603354, 2.3], 6); // France

  // Ajouter un fond de carte
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '© OpenStreetMap contributors'
  }).addTo(map);

  mettreAJourCarte()
}

// Quand on change le type de praticien on met à jour la carte
watch(typePraticien, () => {
  mettreAJourCarte()
})

onMounted(afficheCarto);
</script>

<style scoped>
/* il faut que la div qui contient la map ait une dimension non null */
.map-container { width: 80%; height: 600px; margin: 20px; }
</style>
