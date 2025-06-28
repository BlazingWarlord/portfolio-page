---
layout: default
title: My Travel Map
---

<style>
  body {
    background-color: #111;
    color: #eee;
    font-family: sans-serif;
  }
  h1, h2, p {
    color: #eee;
  }
  #map {
    height: 600px;
    width: 100%;
    margin-top: 1em;
    border: 1px solid #333;
    border-radius: 8px;
  }
  .leaflet-control-attribution {
    background: transparent;
    color: #aaa;
  }
  .leaflet-tooltip {
    background-color: #1f2937 !important;
    color: white !important;
    border-radius: 4px;
    padding: 4px 8px;
    font-size: 14px;
    border: none;
  }
</style>

# 🌍 My Travel Map

This interactive map highlights some of the countries I've visited.

<div id="map"></div>

<!-- Leaflet CSS and JS -->
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
  document.addEventListener("DOMContentLoaded", function () {
    const map = L.map('map', {
      scrollWheelZoom: true,
      zoomControl: true
    }).setView([20, 0], 2);

    L.tileLayer('https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png', {
      attribution: '© OpenStreetMap contributors',
      subdomains: 'abcd',
      maxZoom: 19
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
            fillColor: highlightCountries[feature.properties.name] || "#ffffff00", // transparent default
            fillOpacity: highlightCountries[feature.properties.name] ? 0.6 : 0,
            color: highlightCountries[feature.properties.name] || "#ffffff00",
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
