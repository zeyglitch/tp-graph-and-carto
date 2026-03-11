<template>
  <div>
    <h2>Carte de la Bretagne – Habitants par département</h2>
    <div id="mapBretagne" class="map-container">
    </div>
  </div>
</template>

<script setup>
import { onMounted } from "vue";

// --- importer Leaflet et le css associé
import L from "leaflet";
import "leaflet/dist/leaflet.css";

// -- Fichier GeoJSON des départements de Bretagne
import geojsonBretagne from "../assets/departementBretagne.json";

// -- nb habitants des depts de Bretagne
const nbHabitantsBretagne = [
  { code: "22", nbHabitants: 598814 },
  { code: "29", nbHabitants: 927912 },
  { code: "35", nbHabitants: 1109232 },
  { code: "56", nbHabitants: 776103 }
]

// -- Fonction pour colorer les départements en fonction du nombre d'habitants
const getCouleur = (count) => {
  return count > 1000000 ? "#0c4a0b" : count > 900000 ? "#287919" :
    count > 700000 ? "#43a926" : "#7bcc5d";
};

// -- affiche la carte de la Bretagne
function afficheCarto() {
  // Initialisation de la carte avec leaflet
  let map = L.map('mapBretagne').setView([48.19107, -2.773595], 8); // Bretagne

  // Ajouter un fond de carte
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '© OpenStreetMap contributors'
  }).addTo(map);

  // Afficher la couche GeoJSON
  L.geoJSON(geojsonBretagne, {
    style:
    // -- fonction qui permet de définir un style différent pour chaque département
    (feature) => {
      const codeDept = feature.properties.code;
      // -- nb d'habitants par département
      //    qu'il faut chercher dans le tableau nbHabitantsBretagne
      const nbHabitants = nbHabitantsBretagne.find((v) => v.code == codeDept)?.nbHabitants || 0
      return {
        fillColor: getCouleur(nbHabitants), // -- couleur en fonction du nb habitants
        weight: 1, opacity: 1, color: "white", fillOpacity: 0.5,
      }
    },
    // -- fonction qui s'exécute pour chaque feature (chaque département)
    //    pour ajouter un tooltip avec le nom du département et le nb d'habitants
    onEachFeature: (feature, layer) => {
      const codeDept = feature.properties.code;
      const nbHabitants = nbHabitantsBretagne.find((v) => v.code == codeDept)?.nbHabitants || 0
      layer.bindTooltip(`${feature.properties.nom}: ${nbHabitants}`)
    }
  }).addTo(map);
}

onMounted(afficheCarto);
</script>

<style scoped>
/* il faut que la div qui contient la map ait une dimension non null */
.map-container {
  width: 100%;
  height: 500px;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}
</style>
