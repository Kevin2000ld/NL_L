<div id="grafico-flujo"></div>

<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
<script>
  var data = [{
    type: "sunburst",
    labels: [
      "1. Descarga imágenes", 
      "2. Clasificación supervisada", 
      "3. Cálculo de radianza", 
      "4. Temperatura del brillo",
      "5. Proporción de vegetación", 
      "6. Emisividad",
      "7. LST", 
      "8. NDVI", 
      "9. NDBI", 
      "10. ICU"
    ],
    parents: ["", "", "", "", "", "", "", "", "", ""],
    values: [10,10,10,10,10,10,10,10,10,10],
    textinfo: "label",
    hoverinfo: "label+text",
    text: [
      "Imágenes Landsat 8 desde USGS Earth Explorer",
      "Combinación bandas 6,5,2",
      "Cálculo de radianza espectral TOA",
      "Cálculo de temperatura de brillo (Tb)",
      "Fórmula NDVI y PV",
      "Fórmula emisividad (ε)",
      "Fórmula LST usando Tb, ε y constantes",
      "NDVI clásico para vegetación",
      "NDBI para áreas construidas (banda 6 y 5)",
      "ICU = LST - LSTmedia"
    ]
  }];

  var layout = {
    margin: { t: 0, l: 0, r: 0, b: 0 },
    sunburstcolorway:["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],
    extendsunburstcolorway: true
  };

  Plotly.newPlot('grafico-flujo', data, layout);
</script>
