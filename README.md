<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Programación Lineal - Método Gráfico</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/10.6.4/math.min.js"></script>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; }
    input { margin: 5px; padding: 5px; width: 150px; }
    button { padding: 10px; margin: 10px 0; }
    table { border-collapse: collapse; margin-top: 20px; }
    td, th { border: 1px solid #aaa; padding: 6px 10px; }
  </style>
</head>
<body>

<h2>Resolver Programación Lineal (Método Gráfico)</h2>

<label><strong>Función Objetivo:</strong></label><br>
Z = <input type="number" id="coefX" value="60"> x +
    <input type="number" id="coefY" value="80"> y<br><br>

<label><strong>Restricciones (formato: 4x + 2y <= 100):</strong></label><br>
<textarea id="restricciones" rows="5" cols="50">4x + 2y <= 100
2x + 3y <= 90
x >= 0
y >= 0</textarea><br>

<button onclick="resolver()">Resolver</button>

<div id="resultado"></div>
<div id="grafica" style="width: 100%; height: 500px;"></div>

<script>
function resolver() {
  const coefX = parseFloat(document.getElementById('coefX').value);
  const coefY = parseFloat(document.getElementById('coefY').value);
  const restricciones = document.getElementById('restricciones').value.trim().split('\n').map(r => r.trim());

  // Extraer todos los pares de restricciones para encontrar intersecciones
  let puntos = [];
  for (let i = 0; i < restricciones.length; i++) {
    for (let j = i + 1; j < restricciones.length; j++) {
      const r1 = restricciones[i];
      const r2 = restricciones[j];
      const punto = resolverInterseccion(r1, r2);
      if (punto && esFactible(punto, restricciones)) {
        puntos.push(punto);
      }
    }
  }

  // Filtrar duplicados
  puntos = puntos.filter((p, index, self) =>
    index === self.findIndex(o => Math.abs(o.x - p.x) < 1e-6 && Math.abs(o.y - p.y) < 1e-6)
  );

  if (puntos.length === 0) {
    document.getElementById('resultado').innerHTML = "<b>No hay región factible.</b>";
    Plotly.newPlot('grafica', []);
    return;
  }

  // Evaluar Z
  puntos.forEach(p => p.z = coefX * p.x + coefY * p.y);
  let optimo = puntos.reduce((max, p) => (p.z > max.z ? p : max), puntos[0]);

  // Mostrar resultados
  let html = `<h3>Resultado</h3><p><b>Máximo Z = ${optimo.z}</b> en (x = ${optimo.x}, y = ${optimo.y})</p>`;
  html += `<table><tr><th>x</th><th>y</th><th>Z</th></tr>`;
  puntos.forEach(p => {
    html += `<tr><td>${p.x.toFixed(2)}</td><td>${p.y.toFixed(2)}</td><td>${p.z.toFixed(2)}</td></tr>`;
  });
  html += `</table>`;
  document.getElementById('resultado').innerHTML = html;

  // Graficar
  const traceFactible = {
    x: puntos.map(p => p.x),
    y: puntos.map(p => p.y),
    mode: 'markers',
    type: 'scatter',
    name: 'Vértices factibles',
    marker: { size: 6, color: 'blue' }
  };

  const traceOptimo = {
    x: [optimo.x],
    y: [optimo.y],
    mode: 'markers',
    type: 'scatter',
    name: 'Óptimo',
    marker: { size: 10, color: 'red', symbol: 'star' }
  };

  Plotly.newPlot('grafica', [traceFactible, traceOptimo], {
    title: 'Región Factible y Óptimo',
    xaxis: { title: 'x', range: [0, 100] },
    yaxis: { title: 'y', range: [0, 100] }
  });
}

// Evalúa si un punto cumple con todas las restricciones
function esFactible(punto, restricciones) {
  const scope = { x: punto.x, y: punto.y };
  return restricciones.every(r => {
    const [lhs, op, rhs] = parseRestriccion(r);
    const izq = math.evaluate(lhs, scope);
    const der = parseFloat(rhs);
    if (op === "<=") return izq <= der + 1e-6;
    if (op === ">=") return izq >= der - 1e-6;
    if (op === "=") return Math.abs(izq - der) < 1e-6;
    return false;
  });
}

// Extrae intersección entre dos restricciones lineales
function resolverInterseccion(r1, r2) {
  const [lhs1, , rhs1] = parseRestriccion(r1);
  const [lhs2, , rhs2] = parseRestriccion(r2);
  try {
    const res = math.lusolve([
      [math.derivative(lhs1, 'x').evaluate({x:1,y:0}), math.derivative(lhs1, 'y').evaluate({x:0,y:1})],
      [math.derivative(lhs2, 'x').evaluate({x:1,y:0}), math.derivative(lhs2, 'y').evaluate({x:0,y:1})]
    ], [parseFloat(rhs1), parseFloat(rhs2)]);
    return { x: res[0][0], y: res[1][0] };
  } catch {
    return null; // No hay solución o son paralelas
  }
}

// Divide restricción tipo "2x + 3y <= 10" en partes [lhs, operador, rhs]
function parseRestriccion(r) {
  const match = r.match(/^(.+)(<=|>=|=)(.+)$/);
  return match ? [match[1].trim(), match[2], match[3].trim()] : [null, null, null];
}
</script>

</body>
</html>
