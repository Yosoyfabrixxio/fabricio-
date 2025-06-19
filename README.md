<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Graficador de Sistemas de Desigualdades</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #f0f2f5;
      margin: 0;
      padding: 20px;
    }

    .container {
      max-width: 750px;
      margin: auto;
      background: white;
      padding: 25px 30px;
      border-radius: 12px;
      box-shadow: 0 8px 16px rgba(0,0,0,0.1);
    }

    h1 {
      text-align: center;
      color: #2c3e50;
    }

    label {
      font-weight: bold;
      margin-top: 10px;
      display: block;
    }

    input {
      width: 100%;
      padding: 10px;
      font-size: 16px;
      margin-top: 5px;
      margin-bottom: 15px;
      border: 1px solid #ccc;
      border-radius: 6px;
    }

    button {
      width: 100%;
      background-color: #3498db;
      color: white;
      padding: 12px;
      font-size: 16px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      transition: background 0.3s;
    }

    button:hover {
      background-color: #2980b9;
    }

    #plot {
      margin-top: 30px;
      height: 600px;
    }
  </style>
</head>
<body>

  <div class="container">
    <h1>Graficador de Desigualdades</h1>

    <label for="ineq1">Primera desigualdad (ej: x + y <= 5):</label>
    <input type="text" id="ineq1" value="x + y <= 5">

    <label for="ineq2">Segunda desigualdad (ej: x - y >= 1):</label>
    <input type="text" id="ineq2" value="x - y >= 1">

    <button onclick="graficar()">Graficar sistema</button>

    <div id="plot"></div>
  </div>

  <script>
    function parseInequality(ineq) {
      const parts = ineq.match(/(.+?)(<=|>=|<|>)(.+)/);
      if (!parts) return null;
      return {
        left: parts[1].trim(),
        operator: parts[2],
        right: parts[3].trim()
      };
    }

    function evalInequality(x, y, parsed) {
      try {
        const exprLeft = parsed.left.replace(/x/g, `(${x})`).replace(/y/g, `(${y})`);
        const exprRight = parsed.right.replace(/x/g, `(${x})`).replace(/y/g, `(${y})`);
        return eval(`${eval(exprLeft)} ${parsed.operator} ${eval(exprRight)}`);
      } catch {
        return false;
      }
    }

    function graficar() {
      const ineq1 = document.getElementById('ineq1').value;
      const ineq2 = document.getElementById('ineq2').value;

      const parsed1 = parseInequality(ineq1);
      const parsed2 = parseInequality(ineq2);

      if (!parsed1 || !parsed2) {
        alert("Por favor ingresa desigualdades válidas.");
        return;
      }

      const xVals = [];
      const yVals = [];
      const xMin = -10, xMax = 10;
      const yMin = -10, yMax = 10;
      const step = 0.3;

      for (let x = xMin; x <= xMax; x += step) {
        for (let y = yMin; y <= yMax; y += step) {
          if (evalInequality(x, y, parsed1) && evalInequality(x, y, parsed2)) {
            xVals.push(x);
            yVals.push(y);
          }
        }
      }

      const trace = {
        x: xVals,
        y: yVals,
        mode: 'markers',
        type: 'scatter',
        name: 'Región solución',
        marker: {
          color: 'rgba(39, 174, 96, 0.5)',
          size: 4
        }
      };

      const layout = {
        title: `Región común del sistema`,
        xaxis: { title: 'x', range: [xMin, xMax], gridcolor: '#ccc' },
        yaxis: { title: 'y', range: [yMin, yMax], gridcolor: '#ccc' },
        plot_bgcolor: '#fdfdfd',
        paper_bgcolor: '#ffffff',
        showlegend: false
      };

      Plotly.newPlot('plot', [trace], layout, { responsive: true });
    }
  </script>

</body>
</html>
