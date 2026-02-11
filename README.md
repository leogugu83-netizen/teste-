<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora de Ocupação de Cateter</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: white;
            padding: 40px;
            border-radius: 15px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
            max-width: 500px;
            width: 100%;
        }

        h1 {
            color: #333;
            margin-bottom: 30px;
            text-align: center;
            font-size: 24px;
        }

        .form-group {
            margin-bottom: 25px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            color: #555;
            font-weight: 600;
            font-size: 14px;
        }

        input {
            width: 100%;
            padding: 12px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
            transition: border-color 0.3s;
        }

        input:focus {
            outline: none;
            border-color: #667eea;
        }

        .unit {
            font-size: 13px;
            color: #999;
            margin-top: 5px;
        }

        button {
            width: 100%;
            padding: 14px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
        }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 20px rgba(102, 126, 234, 0.4);
        }

        .result-section {
            margin-top: 30px;
            padding: 20px;
            background: #f8f9ff;
            border-radius: 10px;
            display: none;
        }

        .result-section.show {
            display: block;
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .result-item {
            margin-bottom: 15px;
            padding: 12px;
            background: white;
            border-left: 4px solid #667eea;
            border-radius: 5px;
        }

        .result-label {
            color: #666;
            font-size: 13px;
            margin-bottom: 5px;
        }

        .result-value {
            font-size: 24px;
            font-weight: bold;
            color: #667eea;
        }

        .percentage-bar {
            width: 100%;
            height: 20px;
            background: #e0e0e0;
            border-radius: 10px;
            margin-top: 8px;
            overflow: hidden;
        }

        .percentage-fill {
            height: 100%;
            background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
            transition: width 0.5s ease;
            display: flex;
            align-items: center;
            justify-content: flex-end;
            padding-right: 8px;
            color: white;
            font-size: 11px;
            font-weight: bold;
        }

        .info-box {
            background: #e3f2fd;
            border-left: 4px solid #2196F3;
            padding: 12px;
            border-radius: 5px;
            margin-top: 20px;
            font-size: 13px;
            color: #1976d2;
            line-height: 1.6;
        }

        .error {
            color: #d32f2f;
            font-size: 13px;
            margin-top: 5px;
            display: none;
        }

        .error.show {
            display: block;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>📊 Calculadora de Ocupação de Cateter</h1>
        
        <form id="calculatorForm">
            <div class="form-group">
                <label for="catheterFrench">Diâmetro do Cateter</label>
                <input 
                    type="number" 
                    id="catheterFrench" 
                    placeholder="Digite o diâmetro em French" 
                    step="0.1"
                    required
                >
                <div class="unit">Medida em French (Fr)</div>
                <div class="error" id="catheterError">Por favor, insira um valor válido</div>
            </div>

            <div class="form-group">
                <label for="vesselDiameter">Diâmetro do Vaso Sanguíneo</label>
                <input 
                    type="number" 
                    id="vesselDiameter" 
                    placeholder="Digite o diâmetro em centímetros" 
                    step="0.1"
                    required
                >
                <div class="unit">Medida em centímetros (cm)</div>
                <div class="error" id="vesselError">Por favor, insira um valor válido</div>
            </div>

            <button type="submit">Calcular Ocupação</button>
        </form>

        <div class="result-section" id="resultSection">
            <div class="result-item">
                <div class="result-label">Diâmetro do Cateter (cm)</div>
                <div class="result-value" id="catheterDiameterCm">-</div>
            </div>

            <div class="result-item">
                <div class="result-label">Diâmetro do Vaso Sanguíneo (cm)</div>
                <div class="result-value" id="vesselDiameterValue">-</div>
            </div>

            <div class="result-item">
                <div class="result-label">Percentual de Ocupação</div>
                <div class="result-value" id="occupationPercentage">-</div>
                <div class="percentage-bar">
                    <div class="percentage-fill" id="percentageFill" style="width: 0%"></div>
                </div>
            </div>

            <div class="result-item">
                <div class="result-label">Área Ocupada (cm²)</div>
                <div class="result-value" id="occupiedArea">-</div>
            </div>

            <div class="result-item">
                <div class="result-label">Área Disponível (cm²)</div>
                <div class="result-value" id="availableArea">-</div>
            </div>

            <div class="info-box">
                <strong>ℹ️ Informações:</strong><br>
                • 1 French (Fr) = 0,333 mm = 0,0333 cm<br>
                • Ocupação recomendada: até 50% para fluxo adequado<br>
                • Cálculo: (Área do cateter / Área do vaso) × 100
            </div>
        </div>
    </div>

    <script>
        const form = document.getElementById('calculatorForm');
        const catheterFrenchInput = document.getElementById('catheterFrench');
        const vesselDiameterInput = document.getElementById('vesselDiameter');
        const resultSection = document.getElementById('resultSection');

        // Constante: 1 French = 0.0333 cm
        const FRENCH_TO_CM = 1 / 30;

        form.addEventListener('submit', (e) => {
            e.preventDefault();
            
            // Limpar mensagens de erro
            document.getElementById('catheterError').classList.remove('show');
            document.getElementById('vesselError').classList.remove('show');

            const catheterFrench = parseFloat(catheterFrenchInput.value);
            const vesselDiameter = parseFloat(vesselDiameterInput.value);

            // Validação
            if (!catheterFrench || catheterFrench <= 0) {
                document.getElementById('catheterError').classList.add('show');
                return;
            }

            if (!vesselDiameter || vesselDiameter <= 0) {
                document.getElementById('vesselError').classList.add('show');
                return;
            }

            // Converter French para cm
            const catheterDiameterCm = catheterFrench * FRENCH_TO_CM;

            // Validação: cateter não pode ser maior que o vaso
            if (catheterDiameterCm >= vesselDiameter) {
                document.getElementById('catheterError').classList.add('show');
                document.getElementById('catheterError').textContent = 
                    'O cateter não pode ter diâmetro maior ou igual ao vaso!';
                return;
            }

            // Cálculos geométricos
            const catheterRadius = catheterDiameterCm / 2;
            const vesselRadius = vesselDiameter / 2;

            const catheterArea = Math.PI * Math.pow(catheterRadius, 2);
            const vesselArea = Math.PI * Math.pow(vesselRadius, 2);

            const occupationPercentage = (catheterArea / vesselArea) * 100;
            const availableArea = vesselArea - catheterArea;

            // Atualizar resultados
            document.getElementById('catheterDiameterCm').textContent = 
                catheterDiameterCm.toFixed(4) + ' cm';
            
            document.getElementById('vesselDiameterValue').textContent = 
                vesselDiameter.toFixed(2) + ' cm';
            
            document.getElementById('occupationPercentage').textContent = 
                occupationPercentage.toFixed(2) + '%';
            
            document.getElementById('occupiedArea').textContent = 
                catheterArea.toFixed(4) + ' cm²';
            
            document.getElementById('availableArea').textContent = 
                availableArea.toFixed(4) + ' cm²';

            // Atualizar barra de progresso
            const fillWidth = Math.min(occupationPercentage, 100);
            const percentageFill = document.getElementById('percentageFill');
            percentageFill.style.width = fillWidth + '%';
            percentageFill.textContent = occupationPercentage.toFixed(1) + '%';

            // Mostrar seção de resultados
            resultSection.classList.add('show');
        });

        // Permitir cálculo ao pressionar Enter
        [catheterFrenchInput, vesselDiameterInput].forEach(input => {
            input.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') {
                    form.dispatchEvent(new Event('submit'));
                }
            });
        });
    </script>
</body>
</html>
