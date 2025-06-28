# My Travel Map 🌍

This is an interactive map of countries I've been to:

<div id="map" style="height: 600px; width: 100%; margin-top: 1em;"></div>

<!-- Leaflet CSS and JS -->
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
  document.addEventListener("DOMContentLoaded", function () {
    const map = L.map('map').setView([20, 0], 2);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '© OpenStreetMap contributors'
    }).addTo(map);

    const highlightCountries = {
      "India": "#16a34a",
      "Japan": "#2563eb",
      "Italy": "#e11d48",
      "Thailand": "#f59e0b",
      "Malaysia": "#a855f7",
      "United States of America": "#f43f5e",
      "France": "#0ea5e9",
      "United Arab Emirates": "#facc15",
      "Switzerland": "#4ade80"
    };

    fetch("https://raw.githubusercontent.com/johan/world.geo.json/master/countries.geo.json")
      .then(res => res.json())
      .then(data => {
        L.geoJSON(data, {
          style: feature => ({
            fillColor: highlightCountries[feature.properties.name] || "#00000000",
            fillOpacity: highlightCountries[feature.properties.name] ? 0.4 : 0,
            color: highlightCountries[feature.properties.name] || "#00000000",
            weight: 1
          }),
          onEachFeature: (feature, layer) => {
            const name = feature.properties.name;
            if (highlightCountries[name]) {
              layer.bindTooltip(`<strong>${name}</strong>`, {sticky: true});
            }
          }
        }).addTo(map);
      });
  });
</script>
