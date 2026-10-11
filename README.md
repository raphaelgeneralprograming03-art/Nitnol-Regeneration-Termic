
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulador: Regeneração Térmica da Mola de Nitinol (IA)</title>
    <style>
        :root {
            --bg-color: #030712;
            --panel-bg: rgba(13, 27, 42, 0.85);
            --border-color: #38bdf8;
            --accent-orange: #f97316;
            --accent-red: #ef4444;
            --accent-green: #22c55e;
            --text-main: #e2e8f0;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Courier New', Courier, monospace;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            height: 100vh;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        header {
            background: linear-gradient(90deg, #020617, #0f172a, #020617);
            border-bottom: 2px solid var(--border-color);
            padding: 12px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        header h1 {
            font-size: 1.1rem;
            color: var(--border-color);
            letter-spacing: 2px;
        }

        .badge-protocol {
            background: var(--accent-orange);
            color: #fff;
            padding: 4px 10px;
            font-size: 0.75rem;
            font-weight: bold;
            border-radius: 3px;
            letter-spacing: 1px;
        }

        .main-container {
            display: flex;
            flex: 1;
            gap: 15px;
            padding: 15px;
            overflow: hidden;
        }

        /* Painel de Controle Esquerdo */
        .control-panel {
            width: 340px;
            background: var(--panel-bg);
            border: 1px solid var(--border-color);
            border-radius: 6px;
            padding: 15px;
            display: flex;
            flex-direction: column;
            gap: 15px;
            box-shadow: 0 0 15px rgba(56, 189, 248, 0.1);
        }

        .panel-section h3 {
            font-size: 0.85rem;
            color: var(--border-color);
            margin-bottom: 8px;
            border-bottom: 1px solid rgba(56, 189, 248, 0.3);
            padding-bottom: 4px;
            letter-spacing: 1px;
        }

        .btn-group {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        button {
            background: rgba(56, 189, 248, 0.1);
            border: 1px solid var(--border-color);
            color: var(--text-main);
            padding: 10px 12px;
            text-align: left;
            font-size: 0.8rem;
            cursor: pointer;
            border-radius: 4px;
            transition: all 0.3s ease;
        }

        button:hover {
            background: rgba(56, 189, 248, 0.3);
            color: #fff;
            box-shadow: 0 0 8px var(--border-color);
        }

        button.action-impact {
            border-color: var(--accent-red);
            background: rgba(239, 68, 68, 0.1);
            color: #fca5a5;
        }
        button.action-impact:hover {
            background: rgba(239, 68, 68, 0.3);
        }

        button.action-ai {
            border-color: var(--accent-orange);
            background: rgba(249, 115, 22, 0.1);
            color: #fed7aa;
            font-weight: bold;
        }
        button.action-ai:hover {
            background: rgba(249, 115, 22, 0.3);
        }

        /* Área do Canvas Central */
        .viewport-container {
            flex: 1;
            background: #010409;
            border: 1px solid var(--border-color);
            border-radius: 6px;
            display: flex;
            flex-direction: column;
            position: relative;
            overflow: hidden;
        }

        .viewport-header {
            background: rgba(56, 189, 248, 0.05);
            padding: 8px 15px;
            font-size: 0.75rem;
            display: flex;
            justify-content: space-between;
            border-bottom: 1px solid rgba(56, 189, 248, 0.2);
        }

        canvas {
            flex: 1;
            display: block;
            width: 100%;
            height: 100%;
        }

        /* Painel de Telemetria Direito */
        .telemetry-panel {
            width: 300px;
            background: var(--panel-bg);
            border: 1px solid var(--border-color);
            border-radius: 6px;
            padding: 15px;
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .metric-box {
            background: rgba(2, 6, 23, 0.6);
            border: 1px solid rgba(56, 189, 248, 0.3);
            border-radius: 4px;
            padding: 10px;
        }

        .metric-title {
            font-size: 0.7rem;
            color: var(--border-color);
            margin-bottom: 4px;
        }

        .metric-value {
            font-size: 1.1rem;
            font-weight: bold;
            color: #fff;
        }

        .status-ok { color: var(--accent-green); }
        .status-heat { color: var(--accent-orange); }
        .status-danger { color: var(--accent-red); }
    </style>
</head>
<body>

    <header>
        <h1>PROJETO ESCUDO ESPACIAL: REGENERAÇÃO TÉRMICA DE NITINOL</h1>
        <div class="badge-protocol">EFEITO DE MEMÓRIA DE FORMA (IA PROTOCOL)</div>
    </header>

    <div class="main-container">
        <!-- Painel de Controle -->
        <div class="control-panel">
            <div class="panel-section">
                <h3>1. CENÁRIO DE IMPACTO</h3>
                <div class="btn-group">
                    <button class="action-impact" onclick="triggerImpact()">Simular Impacto (Lixo Espacial / Pouso)</button>
                </div>
            </div>

            <div class="panel-section">
                <h3>2. PROTOCOLO DE RESTAURAÇÃO</h3>
                <div class="btn-group">
                    <button class="action-ai" onclick="startAIRegeneration()">Acionar IA (Injeção de Corrente Joule)</button>
                    <button onclick="resetSimulation()">Resetar Sistema</button>
                </div>
            </div>

            <div class="panel-section">
                <h3>3. ESPECIFICAÇÃO DE MATERIAIS</h3>
                <div style="font-size: 0.75rem; line-height: 1.4; color: #94a3b8;">
                    • Liga: Níquel-Titânio (Nitinol)<br>
                    • Fase Fria: Martensita Deformada<br>
                    • Temp. Transição: 400°C - 500°C<br>
                    • Fase Quente: Austenita (Geometria Original)
                </div>
            </div>
        </div>

        <!-- Visualizador Canvas -->
        <div class="viewport-container">
            <div class="viewport-header">
                <span id="view-status">STATUS: MOLA EM OPERAÇÃO NORMAL (FASE AUSTENITA)</span>
                <span id="temp-display">TEMPERATURA DO NÚCLEO: 25.0 °C</span>
            </div>
            <canvas id="simCanvas"></canvas>
        </div>

        <!-- Telemetria -->
        <div class="telemetry-panel">
            <div class="panel-section">
                <h3>TELEMETRIA TÉRMICA & FASE</h3>
            </div>
            <div class="metric-box">
                <div class="metric-title">ESTADO DA ESTRUTURA</div>
                <div class="metric-value status-ok" id="stat-struct">INTEGRA PERFEITA</div>
            </div>
            <div class="metric-box">
                <div class="metric-title">FASE CRISTALINA</div>
                <div class="metric-value" id="stat-phase">AUSTENITA</div>
            </div>
            <div class="metric-box">
                <div class="metric-title">CIRCUITO ELÉTRICO (EFEITO JOULE)</div>
                <div class="metric-value" id="stat-current">DESLIGADO (0 A)</div>
            </div>
            <div class="metric-box">
                <div class="metric-title">MONITORAMENTO IR / LENTES</div>
                <div class="metric-value status-ok" id="stat-ir">ESTÁVEL</div>
            </div>
        </div>
    </div>

    <script>
        const canvas = document.getElementById('simCanvas');
        const ctx = canvas.getContext('2d');

        // Estados do Sistema de Regeneração
        let state = 'NORMAL'; // NORMAL, CRUSHED, HEATING, RESTORING, COOLING
        let temperature = 25.0; // °C
        let deformationProgress = 0; // 0 = normal, 1 = totalmente amassada
        let restorationProgress = 0; // 0 a 1
        let currentTimer = 0;

        function resizeCanvas() {
            canvas.width = canvas.parentElement.clientWidth;
            canvas.height = canvas.parentElement.clientHeight - 30;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        function triggerImpact() {
            if (state === 'NORMAL') {
                state = 'CRUSHED';
                deformationProgress = 1;
                document.getElementById('view-status').innerText = 'STATUS: DANOS DETECTADOS - MOLA ESMAGADA (MARTENSITA)';
                document.getElementById('view-status').style.color = 'var(--accent-red)';
                document.getElementById('stat-struct').innerText = 'DEFORMAÇÃO PLÁSTICA SEVERA';
                document.getElementById('stat-struct').className = 'metric-value status-danger';
                document.getElementById('stat-phase').innerText = 'MARTENSITA (FRIA)';
                document.getElementById('stat-ir').innerText = 'ANOMALIA GEOMÉTRICA';
                document.getElementById('stat-ir').className = 'metric-value status-danger';
            }
        }

        function startAIRegeneration() {
            if (state === 'CRUSHED') {
                state = 'HEATING';
                currentTimer = 0;
                document.getElementById('view-status').innerText = 'STATUS: IA ATIVADA - INJEÇÃO DE CORRENTE ELÉTRICA (EFEITO JOULE)';
                document.getElementById('view-status').style.color = 'var(--accent-orange)';
                document.getElementById('stat-current').innerText = 'ATIVO (120A - MÁX)';
                document.getElementById('stat-current').className = 'metric-value status-heat';
            }
        }

        function resetSimulation() {
            state = 'NORMAL';
            temperature = 25.0;
            deformationProgress = 0;
            restorationProgress = 0;
            document.getElementById('view-status').innerText = 'STATUS: MOLA EM OPERAÇÃO NORMAL (FASE AUSTENITA)';
            document.getElementById('view-status').style.color = 'var(--text-main)';
            document.getElementById('stat-struct').innerText = 'INTEGRA PERFEITA';
            document.getElementById('stat-struct').className = 'metric-value status-ok';
            document.getElementById('stat-phase').innerText = 'AUSTENITA';
            document.getElementById('stat-current').innerText = 'DESLIGADO (0 A)';
            document.getElementById('stat-current').className = 'metric-value';
            document.getElementById('stat-ir').innerText = 'ESTÁVEL';
            document.getElementById('stat-ir').className = 'metric-value status-ok';
        }

        // Loop de Atualização Lógica e Animação
        function update() {
            if (state === 'HEATING') {
                temperature += 8; // Aumenta temperatura rapidamente
                if (temperature >= 500) {
                    temperature = 500;
                    state = 'RESTORING';
                    document.getElementById('view-status').innerText = 'STATUS: TRANSIÇÃO DE FASE ATINGIDA (500°C) - MEMÓRIA DE FORMA ATIVADA';
                    document.getElementById('stat-phase').innerText = 'AUSTENITA (QUENTE)';
                    document.getElementById('stat-phase').className = 'metric-value status-heat';
                }
            } else if (state === 'RESTORING') {
                restorationProgress += 0.02; // Desamassa progressivamente
                if (restorationProgress >= 1) {
                    restorationProgress = 1;
                    state = 'COOLING';
                    document.getElementById('stat-current').innerText = 'DESLIGADO (CORRENTE CORTADA)';
                    document.getElementById('stat-current').className = 'metric-value';
                    document.getElementById('view-status').innerText = 'STATUS: RESFRIAMENTO CONTROLADO POR LENTES TÉRMICAS IR';
                }
            } else if (state === 'COOLING') {
                temperature -= 4; // Resfria de volta
                if (temperature <= 25) {
                    temperature = 25;
                    state = 'NORMAL';
                    deformationProgress = 0;
                    restorationProgress = 0;
                    document.getElementById('view-status').innerText = 'STATUS: REGENERAÇÃO COMPLETA - MOLA PRONTA PARA OPERAÇÃO';
                    document.getElementById('view-status').style.color = 'var(--accent-green)';
                    document.getElementById('stat-struct').innerText = 'INTEGRA PERFEITA';
                    document.getElementById('stat-struct').className = 'metric-value status-ok';
                    document.getElementById('stat-ir').innerText = 'ESTÁVEL';
                    document.getElementById('stat-ir').className = 'metric-value status-ok';
                }
            }

            document.getElementById('temp-display').innerText = `TEMPERATURA DO NÚCLEO: ${temperature.toFixed(1)} °C`;
        }

        function drawSpring(cx, cy) {
            ctx.save();
            ctx.translate(cx, cy);

            let coils = 12;
            let baseWidth = 300;
            let baseHeight = 50;
            
            // Modificação baseada no estado de esmagamento e restauração
            let currentDef = deformationProgress * (1 - restorationProgress);
            
            ctx.beginPath();
            let points = [];
            let totalSteps = 200;

            for (let i = 0; i <= totalSteps; i++) {
                let p = i / totalSteps;
                let x = (p - 0.5) * baseWidth;
                
                // Cálculo da espira afetada pelo esmagamento
                let squishFactor = 1 - (currentDef * 0.7 * Math.sin(p * Math.PI));
                let y = Math.sin(p * Math.PI * coils * 2) * (baseHeight * squishFactor);
                
                // Adicionar leve deformação lateral se amassada
                if (currentDef > 0) {
                    y += Math.sin(p * Math.PI * 4) * (currentDef * 20);
                }

                points.push({x: x, y: y});
            }

            // Desenhar Linha Externa / Efeito Joule / Brilho Térmico
            ctx.lineWidth = 8;
            if (state === 'HEATING' || state === 'RESTORING' || state === 'COOLING') {
                let heatRatio = (temperature - 25) / 475;
                let red = Math.floor(255);
                let green = Math.floor(100 * (1 - heatRatio));
                let blue = Math.floor(50 * (1 - heatRatio));
                ctx.strokeStyle = `rgb(${red}, ${green}, ${blue})`;
                ctx.shadowBlur = 20 * heatRatio;
                ctx.shadowColor = `rgb(${red}, 0, 0)`;
            } else if (state === 'CRUSHED') {
                ctx.strokeStyle = '#ef4444';
            } else {
                ctx.strokeStyle = '#38bdf8';
            }

            ctx.beginPath();
            for (let i = 0; i < points.length; i++) {
                if (i === 0) ctx.moveTo(points[i].x, points[i].y);
                else ctx.lineTo(points[i].x, points[i].y);
            }
            ctx.stroke();
            ctx.shadowBlur = 0; // Reset sombra

            // Desenhar Terminais de Conexão Elétrica (Efeito Joule)
            ctx.fillStyle = '#64748b';
            ctx.fillRect(-baseWidth/2 - 15, -25, 15, 50);
            ctx.fillRect(baseWidth/2, -25, 15, 50);

            // Se aquecendo, mostrar faíscas ou corrente elétrica
            if (state === 'HEATING' || state === 'RESTORING') {
                ctx.strokeStyle = '#f97316';
                ctx.lineWidth = 2;
                for(let k = 0; k < 5; k++) {
                    ctx.beginPath();
                    ctx.moveTo(-baseWidth/2 - 15 + Math.random()*baseWidth, (Math.random()-0.5)*40);
                    ctx.lineTo(-baseWidth/2 - 15 + Math.random()*baseWidth, (Math.random()-0.5)*40);
                    ctx.stroke();
                }
            }

            ctx.restore();
        }

        function animate() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Atualizar lógica de estados por frame
            update();

            // Desenhar grades de fundo táticas
            ctx.strokeStyle = 'rgba(56, 189, 248, 0.05)';
            ctx.lineWidth = 1;
            let gridSize = 40;
            for (let x = 0; x < canvas.width; x += gridSize) {
                ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, canvas.height); ctx.stroke();
            }
            for (let y = 0; y < canvas.height; y += gridSize) {
                ctx.beginPath(); ctx.moveTo(0, y); ctx.lineTo(canvas.width, y); ctx.stroke();
            }

            // Desenhar a Mola no Centro
            drawSpring(canvas.width / 2, canvas.height / 2);

            requestAnimationFrame(animate);
        }

        animate();
    </script>
</body>
</html>
