<template>
  <div>
    <h2>Carte des cabinets médicaux de Castres</h2>
    <!-- il faut prévoir une div pour afficher la carte -->
    <!-- et il faut qu'elle ait une dimension non null ==> style -->
    <div id="mapcarto" class="map-container">
    </div>
  </div>
</template>

<script setup>
import { onMounted } from "vue";

// --- importer Leaflet et le css associé
import L from "leaflet";
import "leaflet/dist/leaflet.css";

// -- datas à afficher sur la carte
const cabinetsMedical = [
  { "nom": "Maison de Santé Carré Gambetta", "lat": 43.60411, "lng": 2.23862 },
  { "nom": "Cabinet Médical Gaillac et Luguet", "lat": 43.60883, "lng": 2.23078 },
  { "nom": "Cabinet Médical de Lameilhé", "lat": 43.59259, "lng": 2.25492 },
  { "nom": "Cabinet Médical du Saillenc", "lat": 43.60928, "lng": 2.25946 }
]

// -- Facultatif : définir une icône personnalisée pour les centres médicaux
const monIcone = L.icon({
  iconUrl: 'https://cdn-icons-png.flaticon.com/512/387/387561.png',
  iconSize: [48, 48],
});

// ------------- Fonction pour afficher la carte
function afficheCarto() {
  // Initialisation de la carte avec leaflet
  // - la placer dans la div d'id 'mapcarto'
  // - la centrer sur Castres avec un zoom de 14
  let mapcarto = L.map('mapcarto').setView([43.60548, 2.24167], 14);

  // Ajouter un fond de carte (OpenStreetMap)
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '© OpenStreetMap contributors'
  }).addTo(mapcarto);

  // Ajouter les markers : 1 pour chaque centre médical
  cabinetsMedical.forEach((cabinet) => {
    L.marker([cabinet.lat, cabinet.lng], { icon: monIcone }).addTo(mapcarto)
      .bindPopup(cabinet.nom); // Ajouter une popup au marker
  });

  // Ajouter un cercle sur la carte
  L.circle([43.60548, 2.24167], {
    color: 'blue', fillOpacity: 0.1, radius: 2000
  }).addTo(mapcarto);
}

// -- afficher la carte à la création du composant
onMounted(afficheCarto);
</script>

<style scoped>
/* il faut que la div ait une dimension non null */
.map-container {
  width: 100%;
  height: 600px;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}
</style>
