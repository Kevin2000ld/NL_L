<div id="graficoFlujo" style="width: 100%; max-width: 720px; margin: auto;"></div>

<!-- Modal mejorado -->
<div id="modalInfo" style="
  display: none;
  position: fixed;
  top: 5%;
  left: 5%;
  width: 90%;
  max-width: 800px;
  height: 80%;
  overflow-y: auto;
  background: #ffffff;
  border-radius: 12px;
  padding: 30px;
  box-shadow: 0 0 20px rgba(0,0,0,0.3);
  z-index: 9999;
  font-family: 'Arial', sans-serif;
">
  <h2 id="modalTitulo" style="margin-top: 0;"></h2>
  <div id="modalContenido" style="white-space: pre-wrap; line-height: 1.6;"></div>
  <button onclick="document.getElementById('modalInfo').style.display='none'"
    style="margin-top: 20px; padding: 10px 20px; background: #007acc; color: white; border: none; border-radius: 8px;">
    Cerrar
  </button>
</div>

<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
<script>
  const pasos = [
    {
      titulo: "1. Descarga de Imágenes Satelitales",
      detalle: `• Fuente: USGS Earth Explorer\n• Satélite: Landsat 8 OLI/TIRS\n• Recomendación: Seleccionar escenas sin nubosidad (<10%) y temporada seca.`
    },
    {
      titulo: "2. Clasificación Supervisada",
      detalle: `• Combinación de bandas: 6 (SWIR1), 5 (NIR), 2 (Blue)\n• Método: Clasificación supervisada con entrenamiento manual\n• Resultado: Mapa de coberturas (urbana, vegetación, agua, etc.).`
    },
    {
      titulo: "3. Cálculo de Radianza Espectral",
      detalle: `• Fórmula: Lλ = ML * Qcal + AL\n• ML: multiplicador de ganancia radiométrica\n• AL: valor aditivo de sesgo\n• Qcal: valor digital (DN).`
    },
    {
      titulo: "4. Temperatura de Brillo",
      detalle: `• Fórmula: Tb = K2 / ln(K1 / Lλ + 1)\n• Constantes K1 y K2 proporcionadas por USGS\n• Unidad: Kelvin.`
    },
    {
      titulo: "5. Proporción de Vegetación (PV)",
      detalle: `• A partir del NDVI:\nNDVI = (NIR - RED) / (NIR + RED)\nPV = ((NDVI - NDVImin) / (NDVImax - NDVImin))²`
    },
    {
      titulo: "6. Emisividad (ε)",
      detalle: `• ε = 0.004 * PV + 0.986\n• Depende de la cobertura vegetal del píxel.\n• Rango típico: 0.97 – 0.99`
    },
    {
      titulo: "7. Temperatura Superficial (LST)",
      detalle: `• Fórmula:\nLST = Tb / [1 + (λ * Tb / ρ) * ln(ε)]\n• λ: longitud de onda central\n• ρ: constante de Planck ≈ 1.438 * 10⁻² m K`
    },
    {
      titulo: "8. NDVI",
      detalle: `• Fórmula: (NIR - RED) / (NIR + RED)\n• Valores entre -1 y +1\n• Permite diferenciar vegetación densa, escasa y áreas artificiales.`
    },
    {
      titulo: "9. NDBI",
      detalle: `• Fórmula: (SWIR - NIR) / (SWIR + NIR)\n• Áreas construidas tienden a valores positivos\n• Complementario del NDVI para análisis urbano.`
    },
    {
      titulo: "10. ICU (Isla de Calor Urbana)",
      detalle: `• ICU = LST_píxel - LST_promedio_vegetación\n• Mide la intensidad térmica urbana en contraste con áreas vegetadas\n• Aplicación: planeamiento urbano sostenible.`
    }
  ];

  const labels = pasos.map(p => p.titulo.split(". ")[0]); // Solo números
  const names = pasos.map(p => p.titulo.split(". ")[1]); // Nombres cortos

  const data = [{
    type: "pie",
    values: Array(pasos.length).fill(1),
    labels: labels.map((num, i) => `${num}. ${names[i]}`),
    textinfo: "label",
    textposition: "inside",
    hoverinfo: "label",
    marker: {
      colors: [
        "#66c2a5", "#fc8d62", "#8da0cb", "#e78ac3", "#a6d854",
        "#ffd92f", "#e5c494", "#b3b3b3", "#80b1d3", "#fdb462"
      ]
    }
  }];

  const layout = {
    title: "Flujo de Procesamiento para el Cálculo de LST e ICU",
    showlegend: false
  };

  Plotly.newPlot("graficoFlujo", data, layout);

  document.getElementById("graficoFlujo").on("plotly_click", function(data) {
    const punto = data.points[0].pointIndex;
    document.getElementById("modalTitulo").innerText = pasos[punto].titulo;
    document.getElementById("modalContenido").innerText = pasos[punto].detalle;
    document.getElementById("modalInfo").style.display = "block";
  });
</script>
