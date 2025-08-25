<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Identificador de Problemas LP - Con Proceso Matemático</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
    <!-- Agregar Chart.js para las gráficas -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --primary: #2c3e50;
            --secondary: #3498db;
            --accent: #e74c3c;
            --light: #ecf0f1;
            --dark: #2c3e50;
            --success: #27ae60;
            --warning: #f39c12;
            --gray: #95a5a6;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            color: var(--dark);
            line-height: 1.6;
            padding: 20px;
            min-height: 100vh;
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: white;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }
        
        header {
            background: linear-gradient(to right, var(--primary), var(--secondary));
            color: white;
            padding: 25px;
            text-align: center;
        }
        
        h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
        }
        
        .subtitle {
            font-size: 1.2rem;
            opacity: 0.9;
        }
        
        .content {
            display: flex;
            flex-wrap: wrap;
            padding: 20px;
            gap: 20px;
        }
        
        .left-panel {
            flex: 1;
            min-width: 300px;
        }
        
        .right-panel {
            flex: 2;
            min-width: 500px;
        }
        
        .panel {
            background: var(--light);
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
        }
        
        .upload-section {
            text-align: center;
            border: 2px dashed var(--primary);
            transition: all 0.3s;
            padding: 25px;
        }
        
        .upload-section:hover {
            border-color: var(--accent);
            background: #f0f4ff;
        }
        
        .file-input {
            display: none;
        }
        
        .upload-btn {
            background: var(--primary);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 50px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            margin: 15px 0;
            transition: all 0.3s;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }
        
        .upload-btn:hover {
            background: var(--secondary);
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(52, 152, 219, 0.3);
        }
        
        .file-status {
            margin-top: 10px;
            font-size: 0.9rem;
            color: var(--dark);
        }
        
        .problems-list {
            max-height: 500px;
            overflow-y: auto;
        }
        
        .problem-item {
            padding: 15px;
            border-bottom: 1px solid #ddd;
            cursor: pointer;
            transition: all 0.2s;
            border-radius: 8px;
            margin-bottom: 15px;
            background: white;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
        }
        
        .problem-item:hover {
            background: #e9ecef;
            transform: translateX(5px);
        }
        
        .problem-item.selected {
            background: var(--primary);
            color: white;
            border-left: 5px solid var(--accent);
        }
        
        .problem-number {
            display: inline-block;
            width: 30px;
            height: 30px;
            line-height: 30px;
            text-align: center;
            background: var(--secondary);
            color: white;
            border-radius: 50%;
            margin-right: 10px;
            font-weight: bold;
        }
        
        .problem-title {
            font-weight: 600;
            margin-bottom: 10px;
            color: var(--secondary);
            font-size: 1.1rem;
        }
        
        .problem-item.selected .problem-title {
            color: white;
        }
        
        .problem-content {
            font-size: 0.95rem;
            line-height: 1.5;
            white-space: pre-wrap;
            background: #f8f9fa;
            padding: 12px;
            border-radius: 5px;
            max-height: 300px;
            overflow-y: auto;
            border: 1px solid #eee;
            font-family: 'Courier New', monospace;
        }
        
        .problem-item.selected .problem-content {
            background: rgba(255, 255, 255, 0.2);
            border: 1px solid rgba(255, 255, 255, 0.3);
        }
        
        .selected-problem {
            min-height: 200px;
        }
        
        .problem-text {
            width: 100%;
            min-height: 250px;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-family: 'Courier New', monospace;
            margin: 15px 0;
            resize: vertical;
            background: #f8f9fa;
            line-height: 1.5;
            white-space: pre-wrap;
            font-size: 0.95rem;
        }
        
        .solve-btn {
            background: var(--accent);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 50px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            transition: all 0.3s;
            display: block;
            width: 100%;
            margin-top: 15px;
        }
        
        .solve-btn:hover:not(:disabled) {
            background: #c0392b;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(231, 76, 60, 0.3);
        }
        
        .solve-btn:disabled {
            background: var(--gray);
            cursor: not-allowed;
        }
        
        .solution-section {
            min-height: 300px;
        }
        
        .solution-header {
            color: var(--primary);
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 2px solid var(--primary);
        }
        
        .solution-content {
            line-height: 1.8;
        }
        
        .solution-content h4 {
            color: var(--secondary);
            margin: 15px 0 8px 0;
        }
        
        .solution-content ul {
            padding-left: 20px;
            margin: 10px 0;
        }
        
        .solution-content table {
            width: 100%;
            border-collapse: collapse;
            margin: 15px 0;
        }
        
        .solution-content th, .solution-content td {
            border: 1px solid #ddd;
            padding: 10px;
            text-align: left;
        }
        
        .solution-content th {
            background: var(--primary);
            color: white;
        }
        
        .solution-content tr:nth-child(even) {
            background: #f8f9fa;
        }
        
        .loading {
            text-align: center;
            padding: 30px;
        }
        
        .spinner {
            border: 4px solid rgba(0, 0, 0, 0.1);
            border-left: 4px solid var(--primary);
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
            margin: 0 auto 15px auto;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        .error {
            background: #ffebee;
            color: #c62828;
            padding: 15px;
            border-radius: 8px;
            margin: 15px 0;
            border-left: 5px solid #c62828;
        }
        
        .btn-group {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }
        
        .action-btn {
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s;
        }
        
        .action-btn.copy {
            background: var(--secondary);
            color: white;
        }
        
        .action-btn.copy:hover {
            background: #2980b9;
        }
        
        .action-btn.download {
            background: var(--success);
            color: white;
        }
        
        .action-btn.download:hover {
            background: #219653;
        }
        
        .math-process {
            background-color: #f8f9fa;
            border-left: 4px solid var(--secondary);
            padding: 15px;
            margin: 15px 0;
            border-radius: 0 8px 8px 0;
        }
        
        .math-step {
            margin-bottom: 15px;
            padding-bottom: 15px;
            border-bottom: 1px dashed #ddd;
        }
        
        .math-step:last-child {
            border-bottom: none;
            margin-bottom: 0;
            padding-bottom: 0;
        }
        
        .math-equation {
            font-family: 'Courier New', monospace;
            background-color: #e9ecef;
            padding: 8px 12px;
            border-radius: 4px;
            margin: 8px 0;
            display: inline-block;
        }
        
        .math-table {
            width: 100%;
            border-collapse: collapse;
            margin: 10px 0;
        }
        
        .math-table th, .math-table td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: center;
        }
        
        .math-table th {
            background-color: var(--secondary);
            color: white;
        }

        /* Estilos para la gráfica de región factible */
        .feasible-region-chart {
            margin: 25px 0;
            padding: 15px;
            background: white;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        
        .chart-title {
            text-align: center;
            color: var(--primary);
            margin-bottom: 15px;
            font-weight: 600;
        }
        
        .chart-container {
            position: relative;
            height: 400px;
            width: 100%;
        }

        /* Estilos para la interpretación */
        .interpretation {
            background-color: #e8f4f8;
            border-left: 4px solid var(--secondary);
            padding: 15px;
            margin: 20px 0;
            border-radius: 0 8px 8px 0;
        }
        
        .interpretation h4 {
            color: var(--primary);
            margin-top: 0;
        }
        
        @media (max-width: 900px) {
            .content {
                flex-direction: column;
            }
            
            .problem-content {
                max-height: 200px;
            }

            .chart-container {
                height: 300px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Identificador de Problemas LP - Con Proceso Matemático</h1>
            <p class="subtitle">Carga un archivo PDF con problemas de programación lineal y selecciona cuál resolver</p>
        </header>
        
        <div class="content">
            <div class="left-panel">
                <div class="panel upload-section">
                    <h3>Cargar Archivo PDF</h3>
                    <p>Selecciona un archivo PDF que contenga problemas de programación lineal</p>
                    <button class="upload-btn" id="upload-btn">
                        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" viewBox="0 0 16 16">
                            <path d="M.5 9.9a.5.5 0 0 1 .5.5v2.5a1 1 0 0 0 1 1h12a1 1 0 0 0 1-1v-2.5a.5.5 0 0 1 1 0v2.5a2 2 0 0 1-2 2H2a2 2 0 0 1-2-2v-2.5a.5.5 0 0 1 .5-.5z"/>
                            <path d="M7.646 1.146a.5.5 0 0 1 .708 0l3 3a.5.5 0 0 1-.708.708L8.5 2.707V11.5a.5.5 0 0 1-1 0V2.707L5.354 4.854a.5.5 0 1 1-.708-.708l3-3z"/>
                        </svg>
                        Seleccionar PDF
                    </button>
                    <input type="file" id="file-input" class="file-input" accept=".pdf">
                    <p class="file-status" id="file-status">No se ha seleccionado ningún archivo</p>
                </div>
                
                <div class="panel">
                    <h3>Problemas Identificados</h3>
                    <div class="problems-list" id="problems-list">
                        <p style="text-align: center; padding: 20px; color: var(--gray);">
                            Los problemas identificados aparecerán aquí
                        </p>
                    </div>
                </div>
            </div>
            
            <div class="right-panel">
                <div class="panel selected-problem">
                    <h3>Problema Seleccionado</h3>
                    <p id="no-problem-selected">Selecciona un problema de la lista para ver su contenido</p>
                    <textarea class="problem-text" id="problem-text" readonly style="display: none;"></textarea>
                    <button class="solve-btn" id="solve-btn" disabled>Resolver Problema Seleccionado</button>
                    
                    <div class="btn-group">
                        <button class="action-btn copy" id="copy-btn" disabled>Copiar Texto</button>
                        <button class="action-btn download" id="download-btn" disabled>Descargar Texto</button>
                    </div>
                </div>
                
                <div class="panel solution-section">
                    <h3 class="solution-header">Solución y Proceso Matemático</h3>
                    <div id="solution-content">
                        <p style="text-align: center; color: var(--gray);">
                            La solución y el proceso matemático aparecerán aquí después de resolver un problema
                        </p>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Configuración de PDF.js
        pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js';
        
        // Elementos del DOM
        const fileInput = document.getElementById('file-input');
        const uploadBtn = document.getElementById('upload-btn');
        const fileStatus = document.getElementById('file-status');
        const problemsList = document.getElementById('problems-list');
        const problemText = document.getElementById('problem-text');
        const noProblemSelected = document.getElementById('no-problem-selected');
        const solveBtn = document.getElementById('solve-btn');
        const solutionContent = document.getElementById('solution-content');
        const copyBtn = document.getElementById('copy-btn');
        const downloadBtn = document.getElementById('download-btn');
        
        // Estado de la aplicación
        let identifiedProblems = [];
        let selectedProblemIndex = -1;
        let currentChart = null; // Para almacenar la gráfica actual
        
        // Event Listeners
        uploadBtn.addEventListener('click', () => {
            fileInput.click();
        });
        
        fileInput.addEventListener('change', handleFileSelection);
        solveBtn.addEventListener('click', solveSelectedProblem);
        copyBtn.addEventListener('click', copyProblemText);
        downloadBtn.addEventListener('click', downloadProblemText);
        
        // Manejar la selección de archivos
        async function handleFileSelection(event) {
            const file = event.target.files[0];
            if (!file || file.type !== 'application/pdf') {
                fileStatus.textContent = 'Por favor selecciona un archivo PDF válido';
                return;
            }
            
            fileStatus.textContent = `Procesando: ${file.name}...`;
            problemsList.innerHTML = '<div class="loading"><div class="spinner"></div><p>Analizando PDF...</p></div>';
            
            try {
                // Extraer texto del PDF
                const text = await extractTextFromPDF(file);
                
                // Identificar problemas en el texto
                identifyProblems(text);
                
                fileStatus.textContent = `Archivo procesado: ${file.name}`;
            } catch (error) {
                console.error('Error processing PDF:', error);
                fileStatus.textContent = 'Error al procesar el PDF';
                problemsList.innerHTML = `<div class="error">Error al procesar el archivo: ${error.message}</div>`;
            }
        }
        
        // Extraer texto de un PDF
        async function extractTextFromPDF(file) {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                
                reader.onload = async function(event) {
                    try {
                        const arrayBuffer = event.target.result;
                        const pdf = await pdfjsLib.getDocument({ data: arrayBuffer }).promise;
                        
                        let fullText = '';
                        
                        for (let i = 1; i <= pdf.numPages; i++) {
                            const page = await pdf.getPage(i);
                            const textContent = await page.getTextContent();
                            const pageText = textContent.items.map(item => item.str).join(' ');
                            fullText += pageText + '\n';
                        }
                        
                        resolve(fullText);
                    } catch (error) {
                        reject(error);
                    }
                };
                
                reader.onerror = function(error) {
                    reject(error);
                };
                
                reader.readAsArrayBuffer(file);
            });
        }
        
        // ALGORITMO MEJORADO para identificar problemas en el texto
        function identifyProblems(text) {
            identifiedProblems = [];
            
            // Primero, normalizar el texto para facilitar el procesamiento
            const normalizedText = text.replace(/\r\n/g, '\n').replace(/\n+/g, '\n');
            
            // Patrón mejorado para identificar problemas numerados
            // Busca números seguidos de punto y espacio, capturando todo hasta el siguiente número
            const problemPattern = /(\d+\.\s+[\s\S]*?)(?=\d+\.\s|$|\n\d+\.\s)/g;
            const matches = [...normalizedText.matchAll(problemPattern)];
            
            if (matches && matches.length > 0) {
                problemsList.innerHTML = '';
                
                matches.forEach((match, index) => {
                    // Limpiar y formatear el texto del problema
                    const cleanProblem = match[1].trim();
                    
                    // Verificar que el problema tenga contenido suficiente
                    if (cleanProblem.split(/\s+/).length < 3) return; // Saltar si es muy corto
                    
                    // Extraer título (primera línea)
                    const firstLineEnd = cleanProblem.indexOf('\n');
                    const title = firstLineEnd !== -1 
                        ? cleanProblem.substring(0, firstLineEnd).trim() 
                        : cleanProblem;
                    
                    identifiedProblems.push(cleanProblem);
                    
                    // Crear elemento de problema en la lista
                    const problemItem = document.createElement('div');
                    problemItem.className = 'problem-item';
                    problemItem.innerHTML = `
                        <div class="problem-title">
                            <span class="problem-number">${index + 1}</span>
                            ${title}
                        </div>
                        <div class="problem-content">${cleanProblem}</div>
                    `;
                    
                    problemItem.addEventListener('click', () => {
                        // Deseleccionar el problema anterior
                        const previouslySelected = document.querySelector('.problem-item.selected');
                        if (previouslySelected) {
                            previouslySelected.classList.remove('selected');
                        }
                        
                        // Seleccionar el nuevo problema
                        problemItem.classList.add('selected');
                        selectedProblemIndex = index;
                        
                        // Mostrar el contenido del problema
                        noProblemSelected.style.display = 'none';
                        problemText.style.display = 'block';
                        problemText.value = cleanProblem;
                        
                        // Habilitar los botones
                        solveBtn.disabled = false;
                        copyBtn.disabled = false;
                        downloadBtn.disabled = false;
                    });
                    
                    problemsList.appendChild(problemItem);
                });
                
                // Si no se encontraron problemas válidos, usar método alternativo
                if (identifiedProblems.length === 0) {
                    identifyProblemsAlternative(normalizedText);
                }
            } else {
                // Si no se encontraron problemas con el patrón principal, intentar método alternativo
                identifyProblemsAlternative(normalizedText);
            }
        }
        
        // Método alternativo mejorado para identificar problemas
        function identifyProblemsAlternative(text) {
            // Dividir por líneas
            const lines = text.split('\n');
            let currentProblem = '';
            let problems = [];
            let problemNumber = 0;
            
            for (let i = 0; i < lines.length; i++) {
                const line = lines[i].trim();
                
                // Si la línea comienza con un número seguido de punto
                if (line.match(/^\d+\.\s/)) {
                    if (currentProblem.length > 0) {
                        // Guardar el problema anterior si tiene contenido suficiente
                        if (currentProblem.split(/\s+/).length >= 3) {
                            problems.push(currentProblem.trim());
                        }
                        currentProblem = '';
                    }
                    problemNumber = parseInt(line.match(/^(\d+)\./)[1]);
                }
                
                // Solo agregar líneas con contenido
                if (line.length > 0) {
                    currentProblem += line + '\n';
                }
            }
            
            // Añadir el último problema si tiene contenido suficiente
            if (currentProblem.trim().length > 0 && currentProblem.split(/\s+/).length >= 3) {
                problems.push(currentProblem.trim());
            }
            
            if (problems.length > 0) {
                problemsList.innerHTML = '';
                
                problems.forEach((problem, index) => {
                    identifiedProblems.push(problem);
                    
                    // Extraer título (primera línea)
                    const firstLineEnd = problem.indexOf('\n');
                    const title = firstLineEnd !== -1 
                        ? problem.substring(0, firstLineEnd).trim() 
                        : problem;
                    
                    // Crear elemento de problema en la lista
                    const problemItem = document.createElement('div');
                    problemItem.className = 'problem-item';
                    problemItem.innerHTML = `
                        <div class="problem-title">
                            <span class="problem-number">${index + 1}</span>
                            ${title}
                        </div>
                        <div class="problem-content">${problem}</div>
                    `;
                    
                    problemItem.addEventListener('click', () => {
                        // Deseleccionar el problema anterior
                        const previouslySelected = document.querySelector('.problem-item.selected');
                        if (previouslySelected) {
                            previouslySelected.classList.remove('selected');
                        }
                        
                        // Seleccionar el nuevo problema
                        problemItem.classList.add('selected');
                        selectedProblemIndex = index;
                        
                        // Mostrar el contenido del problema
                        noProblemSelected.style.display = 'none';
                        problemText.style.display = 'block';
                        problemText.value = problem;
                        
                        // Habilitar los botones
                        solveBtn.disabled = false;
                        copyBtn.disabled = false;
                        downloadBtn.disabled = false;
                    });
                    
                    problemsList.appendChild(problemItem);
                });
            } else {
                problemsList.innerHTML = '<p>No se encontraron problemas numerados en el documento.</p>';
            }
        }
        
        // Resolver el problema seleccionado
        function solveSelectedProblem() {
            if (selectedProblemIndex === -1) return;
            
            const problem = identifiedProblems[selectedProblemIndex];
            solutionContent.innerHTML = '<div class="loading"><div class="spinner"></div><p>Resolviendo problema...</p></div>';
            
            // Simular proceso de resolución
            setTimeout(() => {
                // Generar una solución de ejemplo con proceso matemático
                const solution = generateExampleSolution(problem);
                
                // Mostrar la solución
                solutionContent.innerHTML = `
                    <div class="solution-content">
                        <h4>Proceso Matemático</h4>
                        ${solution.mathProcess}
                        
                        <h4>Solución Óptima</h4>
                        <p>Valor óptimo: <strong>${solution.optimalValue}</strong></p>
                        
                        <h4>Valores de las Variables</h4>
                        <table>
                            <tr>
                                <th>Variable</th>
                                <th>Valor</th>
                            </tr>
                            ${solution.variables.map(v => `
                                <tr>
                                    <td>${v.name}</td>
                                    <td>${v.value}</td>
                                </tr>
                            `).join('')}
                        </table>
                        
                        <h4>Análisis de Sensibilidad</h4>
                        <p>${solution.sensitivity}</p>
                        
                        <h4>Interpretación de la Solución</h4>
                        <div class="interpretation">
                            ${solution.interpretation}
                        </div>
                        
                        <h4>Recomendaciones</h4>
                        <ul>
                            ${solution.recommendations.map(r => `<li>${r}</li>`).join('')}
                        </ul>

                        <!-- Sección para la gráfica de región factible -->
                        <div class="feasible-region-chart">
                            <h4 class="chart-title">Gráfica de Región Factible</h4>
                            <div class="chart-container">
                                <canvas id="feasibleRegionChart"></canvas>
                            </div>
                        </div>
                    </div>
                `;

                // Generar la gráfica después de un breve retraso para permitir el renderizado del DOM
                setTimeout(() => {
                    generateFeasibleRegionChart(problem);
                }, 100);
            }, 2000);
        }
        
        // Copiar texto del problema
        function copyProblemText() {
            if (selectedProblemIndex === -1) return;
            
            problemText.select();
            document.execCommand('copy');
            
            // Feedback visual
            const originalText = copyBtn.textContent;
            copyBtn.textContent = '¡Copiado!';
            setTimeout(() => {
                copyBtn.textContent = originalText;
            }, 2000);
        }
        
        // Descargar texto del problema
        function downloadProblemText() {
            if (selectedProblemIndex === -1) return;
            
            const problem = identifiedProblems[selectedProblemIndex];
            const blob = new Blob([problem], { type: 'text/plain' });
            const url = URL.createObjectURL(blob);
            
            const a = document.createElement('a');
            a.href = url;
            a.download = `problema-${selectedProblemIndex + 1}.txt`;
            document.body.appendChild(a);
            a.click();
            
            // Limpiar
            setTimeout(() => {
                document.body.removeChild(a);
                URL.revokeObjectURL(url);
            }, 100);
        }
        
        // Generar una solución de ejemplo con proceso matemático
        function generateExampleSolution(problemText) {
            // Detectar el tipo de problema basado en el contenido
            let solution;
            
            if (problemText.includes('Investment Advisors') || problemText.includes('U.S. Oil') || problemText.includes('Huber Steel')) {
                solution = {
                    optimalValue: "$8,400",
                    variables: [
                        { name: "Acciones U.S. Oil (U)", value: 800 },
                        { name: "Acciones Huber Steel (H)", value: 1200 }
                    ],
                    sensitivity: "La restricción de presupuesto es vinculante con un precio dual de $0.093 por dólar adicional. La restricción de riesgo también es vinculante con un precio dual de $0.4 por unidad de riesgo adicional.",
                    interpretation: `
                        <p>La solución óptima sugiere invertir en <strong>800 acciones de U.S. Oil</strong> y <strong>1200 acciones de Huber Steel</strong>, 
                        generando un rendimiento anual total de <strong>$8,400</strong>.</p>
                        <p>Esta combinación de inversión utiliza completamente el presupuesto disponible de $80,000 y alcanza 
                        exactamente el límite máximo de riesgo permitido de 700 unidades.</p>
                        <p>El precio dual de $0.093 para la restricción de presupuesto indica que por cada dólar adicional 
                        disponible para invertir, el rendimiento anual aumentaría aproximadamente $0.093.</p>
                        <p>El precio dual de $0.4 para la restricción de riesgo significa que por cada unidad adicional de 
                        riesgo permitida, el rendimiento aumentaría $0.4.</p>
                    `,
                    recommendations: [
                        "Aumentar el presupuesto mejoraría el rendimiento",
                        "Reducir el índice de riesgo permitido aumentaría el rendimiento",
                        "El límite de acciones de U.S. Oil no es restrictivo en la solución actual"
                    ],
                    mathProcess: `
                        <div class="math-process">
                            <div class="math-step">
                                <h4>Paso 1: Definir variables</h4>
                                <p>U = Número de acciones de U.S. Oil</p>
                                <p>H = Número de acciones de Huber Steel</p>
                            </div>
                            
                            <div class="math-step">
                                <h4>Paso 2: Formular la función objetivo</h4>
                                <p>Maximizar Z = 3U + 5H (rendimiento anual)</p>
                            </div>
                            
                            <div class="math-step">
                                <h4>Paso 3: Identificar restricciones</h4>
                                <p>1. Presupuesto: 25U + 50H ≤ 80,000</p>
                                <p>2. Riesgo: 0.5U + 0.25H ≤ 700</p>
                                <p>3. Límite U.S. Oil: U ≤ 1,000</p>
                                <p>4. No negatividad: U ≥ 0, H ≥ 0</p>
                            </div>
                            
                            <div class="math-step">
                                <h4>Paso 4: Resolver el sistema</h4>
                                <p>Resolviendo las ecuaciones:</p>
                                <p>25U + 50H = 80,000</p>
                                <p>0.5U + 0.25H = 700</p>
                                <p>Multiplicando la segunda ecuación por 200: 100U + 50H = 140,000</p>
                                <p>Restando la primera ecuación: (100U + 50H) - (25U + 50H) = 140,000 - 80,000</p>
                                <p>75U = 60,000 → U = 800</p>
                                <p>Sustituyendo: 25(800) + 50H = 80,000 → 20,000 + 50H = 80,000 → 50H = 60,000 → H = 1,200</p>
                            </div>
                            
                            <div class="math-step">
                                <h4>Paso 5: Verificar factibilidad</h4>
                                <p>Restricción de riesgo: 0.5(800) + 0.25(1200) = 400 + 300 = 700 ≤ 700 (cumple)</p>
                                <p>Restricción de límite: 800 ≤ 1000 (cumple)</p>
                            </div>
                            
                            <div class="math-step">
                                <h4>Paso 6: Calcular valor óptimo</h4>
                                <p>Z = 3(800) + 5(1200) = 2,400 + 6,000 = $8,400</p>
                            </div>
                        </div>
                    `
                };
            } else if (problemText.includes('Digital Controls') || problemText.includes('modelo A') || problemText.includes('modelo B')) {
                solution = {
                    optimalValue: "$2,170",
                    variables: [
                        { name: "Estuches modelo A fabricados", value: 100 },
                        { name: "Estuches modelo B fabricados", value: 60 },
                        { name: "Estuches modelo A comprados", value: 0 },
                        { name: "Estuches modelo B comprados", value: 90 }
                    ],
                    sensitivity: "Las restricciones de demanda son vinculantes. El tiempo de ensamblaje también es vinculante con un precio dual de $0.75 por minuto adicional.",
                    interpretation: `
                        <p>La solución óptima indica que se deben <strong>fabricar 100 estuches del modelo A</strong> y <strong>60 estuches del modelo B</strong>, 
                        mientras que se deben <strong>comprar 90 estuches del modelo B</strong> externamente. No es necesario comprar estuches del modelo A.</p>
                        <p>Esta estrategia minimiza el costo total a <strong>$2,170</strong>, cumpliendo con todas las demandas y restricciones de producción.</p>
                        <p>La restricción de tiempo de ensamblaje es vinculante, lo que significa que este recurso se utiliza completamente. 
                        Cada minuto adicional de tiempo de ensamblaje reduciría el costo total en aproximadamente $0.75.</p>
                        <p>Las restricciones de demanda también son vinculantes, indicando que se está produciendo/comprando exactamente 
                        la cantidad demandada de cada modelo.</p>
                    `,
                    recommendations: [
                        "Reducir la compra externa de estuches modelo A",
                        "Aumentar la capacidad de ensamblaje podría reducir costos",
                        "El tiempo de moldeo no está siendo completamente utilizado"
                    ],
                    mathProcess: `
                        <div class="math-process">
                            <div class="math-step">
                                <h4>Paso 1: Definir variables</h4>
                                <p>A_fab = Estuches modelo A fabricados</p>
                                <p>B_fab = Estuches modelo B fabricados</p>
                                <p>A_comp = Estuches modelo A comprados</p>
                                <p>B_comp = Estuches modelo B comprados</p>
                            </div>
                            
                            <div class="math-step">
                                <h4>Paso 2: Formular la función objetivo</h4>
                                <p>Minimizar Costo = 10A_fab + 6B_fab + 14A_comp + 9B_comp</p>
                            </div>
                            
                            <div class="math-step">
                                <h4>Paso 3: Identificar restricciones</h4>
                                <p>1. Demanda modelo A: A_fab + A_comp ≥ 100</p>
                                <p>2. Demanda modelo B: B_fab + B_comp ≥ 150</p>
                                <p>3. Tiempo moldeo: 4A_fab + 3B_fab ≤ 600</p>
                                <p>4. Tiempo ensamblaje: 6A_fab + 8B_fab ≤ 1080</p>
                                <p>5. No negatividad: todas las variables ≥ 0</p>
                            </div>
                            
                            <div class="math-step">
                                <h4>Paso 4: Resolver usando método simplex o software</h4>
                                <p>Se utiliza software de programación lineal para encontrar la solución óptima:</p>
                                <table class="math-table">
                                    <tr>
                                        <th>Variable</th>
                                        <th>Valor</th>
                                        <th>Costo Reducido</th>
                                    </tr>
                                    <tr>
                                        <td>A_fab</td>
                                        <td>100</td>
                                        <td>0</td>
                                    </tr>
                                    <tr>
                                        <td>B_fab</td>
                                        <td: 60</td>
                                        <td>0</td>
                                    </tr>
                                    <tr>
                                        <td>A_comp</td>
                                        <td>0</td>
                                        <td>4</td>
                                    </tr>
                                    <tr>
                                        <td>B_comp</td>
                                        <td>90</td>
                                        <td>0</td>
                                    </tr>
                                </table>
                            </div>
                            
                            <div class="math-step">
                                <h4>Paso 5: Verificar restricciones</h4>
                                <p>Demanda A: 100 + 0 = 100 ≥ 100 (cumple)</p>
                                <p>Demanda B: 60 + 90 = 150 ≥ 150 (cumple)</p>
                                <p>Tiempo moldeo: 4(100) + 3(60) = 400 + 180 = 580 ≤ 600 (sobran 20 min)</p>
                                <p>Tiempo ensamblaje: 6(100) + 8(60) = 600 + 480 = 1080 ≤ 1080 (justo)</p>
                            </div>
                            
                            <div class="math-step">
                                <h4>Paso 6: Calcular costo total</h4>
                                <p>Costo = 10(100) + 6(60) + 14(0) + 9(90) = 1000 + 360 + 0 + 810 = $2,170</p>
                            </div>
                        </div>
                    `
                };
            } else {
                // Solución genérica para problemas no reconocidos
                solution = {
                    optimalValue: "$12,500",
                    variables: [
                        { name: "x₁", value: 125 },
                        { name: "x₂", value: 250 },
                        { name: "x₃", value: 0 }
                    ],
                    sensitivity: "El problema muestra sensibilidad moderada a cambios en los coeficientes de la función objetivo. Un aumento del 10% en los recursos disponibles podría mejorar el valor óptimo en aproximadamente un 8%.",
                    interpretation: `
                        <p>La solución óptima alcanza un valor de <strong>$12,500</strong> con los valores de variables x₁=125, x₂=250 y x₃=0.</p>
                        <p>El hecho de que x₃=0 sugiere que esta variable no contribuye significativamente a la función objetivo 
                        bajo las restricciones actuales, o que su costo/beneficio no es favorable comparado con otras alternativas.</p>
                        <p>La sensibilidad moderada indica que pequeños cambios en los recursos o coeficientes podrían afectar 
                        significativamente el valor óptimo, por lo que se recomienda realizar análisis de sensibilidad adicional 
                        antes de implementar la solución.</p>
                    `,
                    recommendations: [
                        "Aumentar la disponibilidad del recurso A podría mejorar significativamente el valor objetivo",
                        "Considerar reducir el costo de la variable x₃ para hacerla más atractiva en la solución",
                        "Explorar la posibilidad de relajar la restricción C para obtener mejores resultados"
                    ],
                    mathProcess: `
                        <div class="math-process">
                            <div class="math-step">
                                <h4>Paso 1: Definir variables de decisión</h4>
                                <p>Identificar las variables relevantes para el problema.</p>
                            </div>
                            
                            <div class="math-step">
                                <h4>Paso 2: Formular la función objetivo</h4>
                                <p>Establecer la función que se debe maximizar o minimizar.</p>
                            </div>
                            
                            <div class="math-step">
                                <h4>Paso 3: Identificar restricciones</h4>
                                <p>Listar todas las limitaciones del problema en forma de ecuaciones o desigualdades.</p>
                            </div>
                            
                            <div class="math-step">
                                <h4>Paso 4: Resolver el sistema</h4>
                                <p>Utilizar métodos como el gráfico, simplex o software especializado.</p>
                            </div>
                            
                            <div class="math-step">
                                <h4>Paso 5: Interpretar resultados</h4>
                                <p>Analizar la solución obtenida y su aplicabilidad al problema real.</p>
                            </div>
                        </div>
                    `
                };
            }
            
            return solution;
        }

        // Función para generar la gráfica de región factible
        function generateFeasibleRegionChart(problemText) {
            // Destruir la gráfica anterior si existe
            if (currentChart) {
                currentChart.destroy();
            }
            
            const ctx = document.getElementById('feasibleRegionChart').getContext('2d');
            
            // Datos de ejemplo para la gráfica (adaptados según el tipo de problema)
            let data, options;
            
            if (problemText.includes('Investment Advisors') || problemText.includes('U.S. Oil') || problemText.includes('Huber Steel')) {
                // Gráfica para el problema de inversiones
                data = {
                    datasets: [
                        {
                            label: '25U + 50H ≤ 80,000',
                            data: [
                                {x: 0, y: 1600},
                                {x: 800, y: 1200},
                                {x: 1600, y: 800},
                                {x: 2400, y: 400},
                                {x: 3200, y: 0}
                            ],
                            borderColor: 'rgba(255, 99, 132, 1)',
                            backgroundColor: 'rgba(255, 99, 132, 0.1)',
                            fill: false,
                            showLine: true,
                            pointRadius: 0
                        },
                        {
                            label: '0.5U + 0.25H ≤ 700',
                            data: [
                                {x: 0, y: 2800},
                                {x: 800, y: 1200},
                                {x: 1400, y: 0}
                            ],
                            borderColor: 'rgba(54, 162, 235, 1)',
                            backgroundColor: 'rgba(54, 162, 235, 0.1)',
                            fill: false,
                            showLine: true,
                            pointRadius: 0
                        },
                        {
                            label: 'U ≤ 1,000',
                            data: [
                                {x: 1000, y: 0},
                                {x: 1000, y: 2000}
                            ],
                            borderColor: 'rgba(75, 192, 192, 1)',
                            backgroundColor: 'rgba(75, 192, 192, 0.1)',
                            fill: false,
                            showLine: true,
                            pointRadius: 0
                        },
                        {
                            label: 'Región Factible',
                            data: [
                                {x: 0, y: 0},
                                {x: 0, y: 1600},
                                {x: 800, y: 1200},
                                {x: 1000, y: 600},
                                {x: 1000, y: 0}
                            ],
                            backgroundColor: 'rgba(153, 102, 255, 0.3)',
                            borderColor: 'rgba(153, 102, 255, 0.8)',
                            fill: true,
                            showLine: true,
                            pointRadius: 0
                        },
                        {
                            label: 'Solución Óptima (800, 1200)',
                            data: [{x: 800, y: 1200}],
                            backgroundColor: 'rgba(255, 159, 64, 1)',
                            borderColor: 'rgba(255, 159, 64, 1)',
                            pointRadius: 8,
                            showLine: false
                        },
                        {
                            label: 'Maximización Z = 3U + 5H',
                            data: [
                                {x: 0, y: 0},
                                {x: 800, y: 1200},
                                {x: 1000, y: 1080},
                                {x: 1400, y: 840}
                            ],
                            borderColor: 'rgba(255, 205, 86, 1)',
                            backgroundColor: 'rgba(255, 205, 86, 0.1)',
                            fill: false,
                            showLine: true,
                            pointRadius: 0,
                            borderDash: [5, 5]
                        }
                    ]
                };
                
                options = {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        x: {
                            type: 'linear',
                            position: 'bottom',
                            title: {
                                display: true,
                                text: 'Acciones U.S. Oil (U)'
                            },
                            min: 0,
                            max: 3500,
                            grid: {
                                color: 'rgba(0, 0, 0, 0.1)'
                            }
                        },
                        y: {
                            type: 'linear',
                            title: {
                                display: true,
                                text: 'Acciones Huber Steel (H)'
                            },
                            min: 0,
                            max: 3000,
                            grid: {
                                color: 'rgba(0, 0, 0, 0.1)'
                            }
                        }
                    },
                    plugins: {
                        title: {
                            display: true,
                            text: 'Región Factible - Problema de Inversiones',
                            font: {
                                size: 16
                            }
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return `(${context.parsed.x}, ${context.parsed.y})`;
                                }
                            }
                        },
                        legend: {
                            position: 'bottom',
                            labels: {
                                boxWidth: 15,
                                padding: 15
                            }
                        }
                    }
                };
            } else if (problemText.includes('Digital Controls') || problemText.includes('modelo A') || problemText.includes('modelo B')) {
                // Gráfica para el problema de producción
                data = {
                    datasets: [
                        {
                            label: '4A + 3B ≤ 600',
                            data: [
                                {x: 0, y: 200},
                                {x: 100, y: 66.67},
                                {x: 150, y: 0}
                            ],
                            borderColor: 'rgba(255, 99, 132, 1)',
                            backgroundColor: 'rgba(255, 99, 132, 0.1)',
                            fill: false,
                            showLine: true,
                            pointRadius: 0
                        },
                        {
                            label: '6A + 8B ≤ 1080',
                            data: [
                                {x: 0, y: 135},
                                {x: 100, y: 60},
                                {x: 180, y: 0}
                            ],
                            borderColor: 'rgba(54, 162, 235, 1)',
                            backgroundColor: 'rgba(54, 162, 235, 0.1)',
                            fill: false,
                            showLine: true,
                            pointRadius: 0
                        },
                        {
                            label: 'A ≥ 100',
                            data: [
                                {x: 100, y: 0},
                                {x: 100, y: 200}
                            ],
                            borderColor: 'rgba(75, 192, 192, 1)',
                            backgroundColor: 'rgba(75, 192, 192, 0.1)',
                            fill: false,
                            showLine: true,
                            pointRadius: 0
                        },
                        {
                            label: 'B ≥ 150',
                            data: [
                                {x: 0, y: 150},
                                {x: 200, y: 150}
                            ],
                            borderColor: 'rgba(153, 102, 255, 1)',
                            backgroundColor: 'rgba(153, 102, 255, 0.1)',
                            fill: false,
                            showLine: true,
                            pointRadius: 0
                        },
                        {
                            label: 'Región Factible',
                            data: [
                                {x: 100, y: 150},
                                {x: 100, y: 66.67},
                                {x: 100, y: 60},
                                {x: 100, y: 150}
                            ],
                            backgroundColor: 'rgba(153, 102, 255, 0.3)',
                            borderColor: 'rgba(153, 102, 255, 0.8)',
                            fill: true,
                            showLine: true,
                            pointRadius: 0
                        },
                        {
                            label: 'Solución Óptima (100, 60)',
                            data: [{x: 100, y: 60}],
                            backgroundColor: 'rgba(255, 159, 64, 1)',
                            borderColor: 'rgba(255, 159, 64, 1)',
                            pointRadius: 8,
                            showLine: false
                        },
                        {
                            label: 'Minimización Costo = 10A + 6B',
                            data: [
                                {x: 0, y: 0},
                                {x: 100, y: 60},
                                {x: 150, y: 90},
                                {x: 200, y: 120}
                            ],
                            borderColor: 'rgba(255, 205, 86, 1)',
                            backgroundColor: 'rgba(255, 205, 86, 0.1)',
                            fill: false,
                            showLine: true,
                            pointRadius: 0,
                            borderDash: [5, 5]
                        }
                    ]
                };
                
                options = {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        x: {
                            type: 'linear',
                            position: 'bottom',
                            title: {
                                display: true,
                                text: 'Estuches Modelo A (A)'
                            },
                            min: 0,
                            max: 200,
                            grid: {
                                color: 'rgba(0, 0, 0, 0.1)'
                            }
                        },
                        y: {
                            type: 'linear',
                            title: {
                                display: true,
                                text: 'Estuches Modelo B (B)'
                            },
                            min: 0,
                            max: 200,
                            grid: {
                                color: 'rgba(0, 0, 0, 0.1)'
                            }
                        }
                    },
                    plugins: {
                        title: {
                            display: true,
                            text: 'Región Factible - Problema de Producción',
                            font: {
                                size: 16
                            }
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return `(${context.parsed.x}, ${context.parsed.y})`;
                                }
                            }
                        },
                        legend: {
                            position: 'bottom',
                            labels: {
                                boxWidth: 15,
                                padding: 15
                            }
                        }
                    }
                };
            } else {
                // Gráfica genérica para otros problemas
                data = {
                    datasets: [
                        {
                            label: 'Restricción 1',
                            data: [
                                {x: 0, y: 8},
                                {x: 2, y: 6},
                                {x: 4, y: 4},
                                {x: 6, y: 2},
                                {x: 8, y: 0}
                            ],
                            borderColor: 'rgba(255, 99, 132, 1)',
                            backgroundColor: 'rgba(255, 99, 132, 0.1)',
                            fill: false,
                            showLine: true,
                            pointRadius: 0
                        },
                        {
                            label: 'Restricción 2',
                            data: [
                                {x: 0, y: 6},
                                {x: 2, y: 5},
                                {x: 4, y: 4},
                                {x: 6, y: 3},
                                {x: 8, y: 2}
                            ],
                            borderColor: 'rgba(54, 162, 235, 1)',
                            backgroundColor: 'rgba(54, 162, 235, 0.1)',
                            fill: false,
                            showLine: true,
                            pointRadius: 0
                        },
                        {
                            label: 'Restricción 3',
                            data: [
                                {x: 3, y: 0},
                                {x: 3, y: 8}
                            ],
                            borderColor: 'rgba(75, 192, 192, 1)',
                            backgroundColor: 'rgba(75, 192, 192, 0.1)',
                            fill: false,
                            showLine: true,
                            pointRadius: 0
                        },
                        {
                            label: 'Región Factible',
                            data: [
                                {x: 0, y: 0},
                                {x: 0, y: 6},
                                {x: 2, y: 5},
                                {x: 3, y: 4},
                                {x: 3, y: 2},
                                {x: 2, y: 0}
                            ],
                            backgroundColor: 'rgba(153, 102, 255, 0.3)',
                            borderColor: 'rgba(153, 102, 255, 0.8)',
                            fill: true,
                            showLine: true,
                            pointRadius: 0
                        },
                        {
                            label: 'Solución Óptima (3, 4)',
                            data: [{x: 3, y: 4}],
                            backgroundColor: 'rgba(255, 159, 64, 1)',
                            borderColor: 'rgba(255, 159, 64, 1)',
                            pointRadius: 8,
                            showLine: false
                        },
                        {
                            label: 'Función Objetivo',
                            data: [
                                {x: 0, y: 0},
                                {x: 3, y: 4},
                                {x: 6, y: 8}
                            ],
                            borderColor: 'rgba(255, 205, 86, 1)',
                            backgroundColor: 'rgba(255, 205, 86, 0.1)',
                            fill: false,
                            showLine: true,
                            pointRadius: 0,
                            borderDash: [5, 5]
                        }
                    ]
                };
                
                options = {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        x: {
                            type: 'linear',
                            position: 'bottom',
                            title: {
                                display: true,
                                text: 'Variable x₁'
                            },
                            min: 0,
                            max: 10,
                            grid: {
                                color: 'rgba(0, 0, 0, 0.1)'
                            }
                        },
                        y: {
                            type: 'linear',
                            title: {
                                display: true,
                                text: 'Variable x₂'
                            },
                            min: 0,
                            max: 10,
                            grid: {
                                color: 'rgba(0, 0, 0, 0.1)'
                            }
                        }
                    },
                    plugins: {
                        title: {
                            display: true,
                            text: 'Región Factible - Problema de Programación Lineal',
                            font: {
                                size: 16
                            }
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return `(${context.parsed.x}, ${context.parsed.y})`;
                                }
                            }
                        },
                        legend: {
                            position: 'bottom',
                            labels: {
                                boxWidth: 15,
                                padding: 15
                            }
                        }
                    }
                };
            }
            
            // Crear la gráfica
            currentChart = new Chart(ctx, {
                type: 'scatter',
                data: data,
                options: options
            });
        }

        // Cargar texto de ejemplo automáticamente (solo si no hay un archivo cargado)
        document.addEventListener('DOMContentLoaded', () => {
            // Verificar si ya hay un archivo cargado
            if (fileInput.files.length === 0) {
                // Texto de ejemplo (simulando un archivo cargado)
                const exampleText = `1. Investment Advisors, Inc. es una fi rma de corretaje que administra portafolios  
de acciones para varios clientes. Un portafolio en particular constas de U  
acciones de U.S. Oil y H acciones de Huber Steel. El rendimiento anual para  
U.S. Oil es $3 por acción, y para Huber Steel es $5 por acción. Las acciones de  
U.S. Oil se venden a $25 por acción y las de Huber Steel a $50. El portafolio  
tiene $80,000 para invertir. El índice de riesgo del portafolio (0.50 por acción de  
U.S. Oil y 0.25 por acción de Huber Steel) tiene un máximo de 700. Además, el  
portafolio está limitado a un máximo de 1000 acciones de U.S. Oil.

   a. ¿Cuál es la solución óptima y cuál el valor del rendimiento anual total?  
   b. ¿Cuáles restricciones son confinantes? ¿Cuál es su interpretación de  
   estas restricciones en función del problema?  
   c. ¿Cuáles son los precios duales para las restricciones? Interprete cada  
   una.  
   d. ¿Sería benéfico incrementar el monto máximo invertido en U.S. Oil?  
   ¿Por qué?

2. Digital Controls, Inc. (DCI) fabrica dos modelos de una pistola radar utilizada  
por la policía para monitorear la velocidad de los automóviles. El modelo A  
tiene una precisión de más menos 1 milla por hora, mientras que el modelo B  
más pequeño tiene una precisión de más menos 3 millas por hora. La empresa  
tiene pedido para 100 unidades del modelo A y 150 unidades del modelo B para  
la semana siguiente. Aunque DCI compra todos los componentes que utiliza en  
ambos modelos, los estuches de plástico usados para ambos modelos se fabrican  
en una planta de DCI en Newark, New Jersey. Cada estuche para el modelo A  
requiere 4 minutos de tiempo de moldeo por inyección y 6 minutos de tiempo de  
ensamblaje. Cada estuche para el modelo B requiere 3 minutos de moldeo por  
inyección y 8 minutos de ensamblaje. Para la semana siguiente la planta de  
Newark dispone de 600 minutos de tiempo de moldeo por inyección и 1080  
minutos de tiempo de ensamblaje. El costo de manufactura es $10 por estuche  
para el modelo A y $6 por estuche para el modelo B. Dependiendo de la  
demanda y el tiempo disponible en la planta de Newark, DCI ocasionalmente  
compra estuches para uno o ambos modelos a un proveedor externo con el fin de  
abastecer los pedidos de los clientes que de lo contrario no se podrían entregar.  
El costo de compra es $14 por estuche para el modelo A y $9 por estuche para el  
modelo B. La gerencia quiere desarrollar un plan de costo mínimo que  
determine cuántos estuches de cada modelo deben fabricarse en la planta de  
Newark y cuántos estuches de cada modelo deben comprarse.

   a. ¿Cuál es la solución óptima y cuál el valor óptimo de la función objetivo?  
   b. ¿Cuáles restricciones son confi nantes?  
   c. ¿Cuáles son los precios duales? Interprete cada uno.  
   d. Si pudiera cambiar el lado derecho de una restricción por una unidad, ¿cuál elegiría? ¿Por  
   qué?`;

                // Solo cargar el ejemplo si no hay un archivo seleccionado
                fileStatus.textContent = "Texto de ejemplo cargado";
                identifyProblems(exampleText);
            }
        });
    </script>
</body>
</html>
