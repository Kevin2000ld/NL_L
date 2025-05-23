<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Diagrama Circular de Procesos</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
      background: #f4f6f9;
    }
    .chart-container {
      position: relative;
      width: 90vmin;
      height: 90vmin;
    }
    #chartLabels {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-wrap: wrap;
      pointer-events: none;
    }
    .label {
      position: absolute;
      width: 80px;
      text-align: center;
      font-size: 0.8rem;
      font-weight: bold;
      color: #fff;
    }
  </style>
</head>
<body>
  <div class="chart-container">
    <canvas id="myChart"></canvas>
    <div id="chartLabels"></div>
  </div>

  <script>
    const labels = [
      "1. Descarga de imágenes",
      "2. Clasificación supervisada",
      "3. Radianza espectral",
      "4. Temperatura del brillo",
      "5. Proporción de vegetación",
      "6. Emisividad",
      "7. LST",
      "8. NDVI",
      "9. NDBI",
      "10. ICU"
    ];

    const colors = [
      "#2E8B57", "#3CB371", "#66CDAA", "#20B2AA", "#5F9EA0",
      "#4682B4", "#6495ED", "#7B68EE", "#9370DB", "#BA55D3"
    ];

    const data = {
      labels,
      datasets: [{
        data: Array(labels.length).fill(1),
        backgroundColor: colors,
        borderWidth: 2,
        borderColor: "#fff"
      }]
    };

    const config = {
      type: "doughnut",
      data,
      options: {
        cutout: "50%",
        plugins: {
          legend: { display: false },
          tooltip: {
            callbacks: {
              label: function(ctx) {
                return ctx.label;
              }
            }
          }
        },
        animation: {
          animateRotate: true,
          animateScale: true
        }
      }
    };

    const myChart = new Chart(
      document.getElementById("myChart"),
      config
    );

    const labelContainer = document.getElementById("chartLabels");
    const radius = 38; // Ajuste de radio interno para ubicar etiquetas dentro
    labels.forEach((label, index) => {
      const angle = (index / labels.length) * 2 * Math.PI - Math.PI / 2;
      const x = 50 + radius * Math.cos(angle);
      const y = 50 + radius * Math.sin(angle);
      const div = document.createElement("div");
      div.className = "label";
      div.style.left = `${x}%`;
      div.style.top = `${y}%`;
      div.innerText = label.split(". ")[0];
      div.title = label;
      labelContainer.appendChild(div);
    });
  </script>
</body>
</html>
