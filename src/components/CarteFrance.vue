<template>
  <div>
    <h2>E) Densité de médecins par département (2023)</h2>

    <!-- Sélection du type de praticien -->
    <div class="controles">
      <label for="selectType">Type de praticien : </label>
      <select id="selectType" v-model="typePraticien" class="select-praticien">
        <option value="RatioGeneralistes">Généralistes</option>
        <option value="RatioSpecialistes">Spécialistes</option>
        <option value="RatioDentistes">Dentistes</option>
        <option value="RatioPharmaciens">Pharmaciens</option>
      </select>
    </div>

    <!-- Légende -->
    <div class="legende">
      <span class="legende-titre">Densité pour 100 000 hab. :</span>
      <div class="legende-items">
        <div class="legende-item" v-for="item in itemsLegende" :key="item.label">
          <span class="legende-couleur" :style="{ background: item.couleur }"></span>
          <span>{{ item.label }}</span>
        </div>
      </div>
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

// Éléments de la légende
const itemsLegende = [
  { couleur: "#006d2c", label: "> 200" },
  { couleur: "#31a354", label: "161 - 200" },
  { couleur: "#74c476", label: "131 - 160" },
  { couleur: "#a1d99b", label: "101 - 130" },
  { couleur: "#c7e9c0", label: "81 - 100" },
  { couleur: "#e5f5e0", label: "61 - 80" },
  { couleur: "#f7fcf5", label: "≤ 60" },
]

// -- Fonction pour colorer les départements en fonction de la densité
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
.map-container {
  width: 100%;
  height: 600px;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.controles {
  margin: 10px 0 15px 0;
  display: flex;
  align-items: center;
  gap: 10px;
}

.controles label {
  font-weight: 600;
  color: #444;
}

.select-praticien {
  padding: 8px 14px;
  border: 2px solid #ddd;
  border-radius: 8px;
  font-size: 14px;
  font-family: 'Inter', sans-serif;
  background: #fff;
  cursor: pointer;
  transition: border-color 0.2s;
}

.select-praticien:focus {
  outline: none;
  border-color: #667eea;
}

/* Légende de la carte */
.legende {
  margin: 10px 0 15px 0;
  padding: 10px 14px;
  background: #f9f9f9;
  border-radius: 8px;
  border: 1px solid #eee;
}

.legende-titre {
  font-size: 13px;
  font-weight: 600;
  color: #555;
  display: block;
  margin-bottom: 6px;
}

.legende-items {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.legende-item {
  display: flex;
  align-items: center;
  gap: 5px;
  font-size: 12px;
  color: #666;
}

.legende-couleur {
  width: 18px;
  height: 14px;
  border-radius: 3px;
  border: 1px solid #ccc;
  display: inline-block;
}
</style>
