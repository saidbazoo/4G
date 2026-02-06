<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>4G Network Coverage Planning Tool</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: #333;
            line-height: 1.6;
            padding: 20px;
            min-height: 100vh;
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
            background-color: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            padding: 30px;
            backdrop-filter: blur(10px);
        }
        
        header {
            text-align: center;
            margin-bottom: 40px;
            padding-bottom: 20px;
            border-bottom: 3px solid #667eea;
            position: relative;
        }
        
        h1 {
            color: #2c5aa0;
            font-size: 2.5rem;
            margin-bottom: 10px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        h2 {
            color: #2c5aa0;
            margin: 30px 0 20px;
            padding-bottom: 10px;
            border-bottom: 2px solid #e0e8f5;
        }
        
        h3 {
            color: #3a6bc2;
            margin: 20px 0 15px;
        }
        
        .subtitle {
            color: #666;
            font-size: 1.1rem;
            max-width: 800px;
            margin: 0 auto;
        }
        
        .steps-container {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            margin: 40px 0;
            gap: 20px;
        }
        
        .step {
            flex: 1;
            min-width: 300px;
            padding: 25px;
            background: linear-gradient(135deg, #f5f7fa 0%, #e4edf5 100%);
            border-radius: 15px;
            border: none;
            transition: transform 0.3s, box-shadow 0.3s;
            position: relative;
            overflow: hidden;
        }
        
        .step:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
        }
        
        .step-number {
            display: inline-block;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            width: 35px;
            height: 35px;
            border-radius: 50%;
            text-align: center;
            line-height: 35px;
            margin-right: 10px;
            font-weight: bold;
            font-size: 1.2rem;
        }
        
        .form-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
            gap: 30px;
            margin: 40px 0;
        }
        
        .form-group {
            margin-bottom: 25px;
        }
        
        label {
            display: block;
            margin-bottom: 10px;
            font-weight: 600;
            color: #444;
            font-size: 0.95rem;
        }
        
        input, select {
            width: 100%;
            padding: 14px 15px;
            border: 2px solid #c5d2e8;
            border-radius: 10px;
            font-size: 1rem;
            transition: all 0.3s;
            background: white;
        }
        
        input:focus, select:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.2);
            transform: translateY(-2px);
        }
        
        .button-group {
            display: flex;
            gap: 15px;
            margin: 30px 0;
            flex-wrap: wrap;
        }
        
        button {
            padding: 16px 35px;
            font-size: 1.1rem;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: 600;
            border: none;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        #calculateBtn {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            flex: 2;
        }
        
        #calculateBtn:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 20px rgba(102, 126, 234, 0.3);
        }
        
        #resetBtn {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            color: white;
            flex: 1;
        }
        
        #resetBtn:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 20px rgba(245, 87, 108, 0.3);
        }
        
        #compareBtn {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
            flex: 1;
        }
        
        #compareBtn:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 20px rgba(79, 172, 254, 0.3);
        }
        
        #voiceAnalysisBtn {
            background: linear-gradient(135deg, #43e97b 0%, #38f9d7 100%);
            color: white;
            flex: 1;
        }
        
        #voiceAnalysisBtn:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 20px rgba(67, 233, 123, 0.3);
        }
        
        .results-container {
            display: none;
            margin-top: 50px;
            padding: 30px;
            background: linear-gradient(135deg, #f5f7fa 0%, #e4edf5 100%);
            border-radius: 15px;
            border: none;
        }
        
        .results-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 25px;
            margin: 30px 0;
        }
        
        .result-card {
            background: white;
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.08);
            transition: transform 0.3s;
            position: relative;
            overflow: hidden;
            border-left: 5px solid #667eea;
        }
        
        .result-card:hover {
            transform: translateY(-5px);
        }
        
        .result-label {
            font-weight: 600;
            color: #666;
            font-size: 0.9rem;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }
        
        .result-value {
            font-weight: 700;
            color: #2c5aa0;
            font-size: 1.8rem;
            margin: 10px 0;
        }
        
        .result-unit {
            color: #764ba2;
            font-size: 0.9rem;
            font-weight: 600;
        }
        
        .charts-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
            gap: 30px;
            margin-top: 50px;
        }
        
        .chart {
            background: white;
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.08);
            transition: transform 0.3s;
        }
        
        .chart:hover {
            transform: translateY(-5px);
        }
        
        .chart canvas {
            max-width: 100%;
        }
        
        .comparison-container {
            display: none;
            margin-top: 40px;
            padding: 30px;
            background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
            border-radius: 15px;
        }
        
        .voice-analysis-container {
            display: none;
            margin-top: 40px;
            padding: 30px;
            background: linear-gradient(135deg, #f0fff4 0%, #dcffe4 100%);
            border-radius: 15px;
            border-left: 5px solid #43e97b;
        }
        
        .service-comparison {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-top: 30px;
        }
        
        .service-card {
            background: white;
            padding: 25px;
            border-radius: 12px;
            text-align: center;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.08);
            transition: all 0.3s;
            border-top: 5px solid;
            position: relative;
            overflow: hidden;
        }
        
        .service-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.15);
        }
        
        .voice-card {
            border-color: #667eea;
        }
        
        .data-card {
            border-color: #f093fb;
        }
        
        .video-card {
            border-color: #4facfe;
        }
        
        .service-icon {
            font-size: 2.5rem;
            margin-bottom: 15px;
        }
        
        .voice-analysis-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 25px;
            margin-top: 30px;
        }
        
        .voice-metrics {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin: 20px 0;
        }
        
        .voice-metric {
            background: white;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.08);
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin: 20px 0;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.08);
        }
        
        th, td {
            padding: 15px;
            text-align: left;
            border: 1px solid #e0e8f5;
        }
        
        th {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            font-weight: 600;
        }
        
        tr:nth-child(even) {
            background-color: #f8fbff;
        }
        
        tr:hover {
            background-color: #eef5ff;
        }
        
        .highlight {
            background: linear-gradient(135deg, #e3f2fd 0%, #f3e5f5 100%);
            padding: 25px;
            border-radius: 15px;
            margin: 25px 0;
            border-left: 5px solid #667eea;
        }
        
        .site-map-container {
            margin-top: 40px;
            padding: 25px;
            background: white;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.08);
        }
        
        #siteMap {
            width: 100%;
            height: 400px;
            background: #f8fbff;
            border-radius: 10px;
            margin-top: 20px;
            border: 2px solid #e0e8f5;
            position: relative;
            overflow: hidden;
        }
        
        .legend {
            display: flex;
            justify-content: center;
            gap: 30px;
            margin-top: 20px;
            flex-wrap: wrap;
        }
        
        .legend-item {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .legend-color {
            width: 20px;
            height: 20px;
            border-radius: 50%;
        }
        
        .info-icon {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            width: 20px;
            height: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border-radius: 50%;
            margin-left: 5px;
            font-size: 0.8rem;
            cursor: help;
            font-weight: bold;
        }
        
        .tooltip {
            position: relative;
            display: inline-block;
        }
        
        .tooltip .tooltiptext {
            visibility: hidden;
            width: 250px;
            background-color: #333;
            color: #fff;
            text-align: center;
            border-radius: 6px;
            padding: 10px;
            position: absolute;
            z-index: 1000;
            bottom: 125%;
            left: 50%;
            margin-left: -125px;
            opacity: 0;
            transition: opacity 0.3s;
            font-size: 0.9rem;
            font-weight: normal;
        }
        
        .tooltip:hover .tooltiptext {
            visibility: visible;
            opacity: 1;
        }
        
        .status-indicator {
            display: inline-flex;
            align-items: center;
            gap: 5px;
            padding: 5px 10px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 600;
            margin-left: 10px;
        }
        
        .status-good {
            background-color: #d4edda;
            color: #155724;
        }
        
        .status-warning {
            background-color: #fff3cd;
            color: #856404;
        }
        
        .status-poor {
            background-color: #f8d7da;
            color: #721c24;
        }
        
        .progress-bar {
            width: 100%;
            height: 10px;
            background-color: #e0e8f5;
            border-radius: 5px;
            overflow: hidden;
            margin: 10px 0;
        }
        
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
            border-radius: 5px;
            transition: width 0.5s;
        }
        
        .voice-quality-indicator {
            width: 100%;
            height: 30px;
            background: linear-gradient(90deg, #43e97b 0%, #f093fb 50%, #f5576c 100%);
            border-radius: 15px;
            margin: 20px 0;
            position: relative;
        }
        
        .quality-marker {
            position: absolute;
            top: -5px;
            width: 40px;
            height: 40px;
            background: white;
            border: 3px solid #2c5aa0;
            border-radius: 50%;
            transform: translateX(-50%);
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: #2c5aa0;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.2);
        }
        
        .quality-labels {
            display: flex;
            justify-content: space-between;
            margin-top: 5px;
            font-size: 0.8rem;
            color: #666;
        }
        
        .footer {
            margin-top: 50px;
            padding-top: 30px;
            border-top: 2px solid #e0e8f5;
            text-align: center;
            color: #666;
            font-size: 0.9rem;
        }
        
        .credits {
            display: flex;
            justify-content: center;
            gap: 30px;
            margin-top: 20px;
            flex-wrap: wrap;
        }
        
        .credit {
            font-weight: 600;
            color: #2c5aa0;
        }
        
        @media (max-width: 768px) {
            .form-container, .charts-container, .results-grid, .voice-analysis-grid {
                grid-template-columns: 1fr;
            }
            
            .steps-container {
                flex-direction: column;
            }
            
            .button-group {
                flex-direction: column;
            }
            
            button {
                width: 100%;
            }
            
            h1 {
                font-size: 2rem;
            }
        }
    </style>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
    <div class="container">
        <header>
            <h1>4G Network Coverage Planning Tool</h1>
            <p class="subtitle">Advanced tool for calculating required sites, coverage radius, and service quality for 4G networks with Voice Service Analysis</p>
        </header>
        
        <section>
            <h2>Planning Process</h2>
            <div class="steps-container">
                <div class="step">
                    <h3><span class="step-number">1</span> Calculate Maximum Allowable Path Loss (MAPL)</h3>
                    <p>Compute the maximum signal loss the network can tolerate before service interruption, considering all losses and margins.</p>
                </div>
                <div class="step">
                    <h3><span class="step-number">2</span> Determine Propagation Model</h3>
                    <p>Use propagation models (Okumura-Hata, COST-231, or 3GPP 3D-UMA) to calculate the coverage radius for each cell.</p>
                </div>
                <div class="step">
                    <h3><span class="step-number">3</span> Calculate Required Sites</h3>
                    <p>Based on coverage radius and total area, determine the total number of stations required for complete coverage.</p>
                </div>
            </div>
        </section>
        
        <section>
            <h2>Input Parameters</h2>
            <form id="coverageForm">
                <div class="form-container">
                    <div>
                        <h3>Basic Information</h3>
                        <div class="form-group">
                            <label for="area">Total Area (km²) <span class="tooltip"><span class="info-icon">?</span><span class="tooltiptext">Total geographical area to be covered</span></span></label>
                            <input type="number" id="area" name="area" value="50" min="1" max="10000" step="0.1" required>
                        </div>
                        
                        <div class="form-group">
                            <label for="frequency">Frequency (MHz) <span class="tooltip"><span class="info-icon">?</span><span class="tooltiptext">Operating frequency of the base station</span></span></label>
                            <select id="frequency" name="frequency" required>
                                <option value="700">700 MHz</option>
                                <option value="800">800 MHz</option>
                                <option value="900">900 MHz</option>
                                <option value="1800">1800 MHz</option>
                                <option value="2100" selected>2100 MHz</option>
                                <option value="2600">2600 MHz</option>
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label for="environment">Environment Type <span class="tooltip"><span class="info-icon">?</span><span class="tooltiptext">Type of area to be covered</span></span></label>
                            <select id="environment" name="environment" required>
                                <option value="Rural">Rural</option>
                                <option value="Suburban">Suburban</option>
                                <option value="Urban" selected>Urban</option>
                                <option value="DenseUrban">Dense Urban</option>
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label for="propagationModel">Propagation Model <span class="tooltip"><span class="info-icon">?</span><span class="tooltiptext">Wave propagation model for calculation</span></span></label>
                            <select id="propagationModel" name="propagationModel" required>
                                <option value="OkumuraHata">Okumura-Hata</option>
                                <option value="COST231">COST-231 Hata</option>
                                <option value="3GPP_UMA" selected>3GPP 3D-UMA</option>
                            </select>
                        </div>
                    </div>
                    
                    <div>
                        <h3>Base Station Parameters</h3>
                        <div class="form-group">
                            <label for="txPower">Transmit Power (dBm) <span class="tooltip"><span class="info-icon">?</span><span class="tooltiptext">Output power from transmitter</span></span></label>
                            <input type="number" id="txPower" name="txPower" value="43" min="0" max="60" step="0.1" required>
                        </div>
                        
                        <div class="form-group">
                            <label for="txGain">Transmit Antenna Gain (dBi) <span class="tooltip"><span class="info-icon">?</span><span class="tooltiptext">Antenna amplification factor</span></span></label>
                            <input type="number" id="txGain" name="txGain" value="18" min="0" max="30" step="0.1" required>
                        </div>
                        
                        <div class="form-group">
                            <label for="antennaType">Antenna Type <span class="tooltip"><span class="info-icon">?</span><span class="tooltiptext">Base station antenna type</span></span></label>
                            <select id="antennaType" name="antennaType" required>
                                <option value="Directive" selected>Directive</option>
                                <option value="Omni">Omni-directional</option>
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label for="hBS">Base Station Height (m) <span class="tooltip"><span class="info-icon">?</span><span class="tooltiptext">Base station antenna height above ground</span></span></label>
                            <input type="number" id="hBS" name="hBS" value="25" min="10" max="150" step="0.1" required>
                        </div>
                    </div>
                    
                    <div>
                        <h3>User Equipment Parameters</h3>
                        <div class="form-group">
                            <label for="rxSensitivity">Receiver Sensitivity (dBm) <span class="tooltip"><span class="info-icon">?</span><span class="tooltiptext">Minimum signal level receiver can interpret</span></span></label>
                            <input type="number" id="rxSensitivity" name="rxSensitivity" value="-100" min="-120" max="-70" step="0.1" required>
                        </div>
                        
                        <div class="form-group">
                            <label for="rxGain">Receiver Antenna Gain (dBi) <span class="tooltip"><span class="info-icon">?</span><span class="tooltiptext">User equipment antenna gain</span></span></label>
                            <input type="number" id="rxGain" name="rxGain" value="0" min="-5" max="10" step="0.1" required>
                        </div>
                        
                        <div class="form-group">
                            <label for="hUT">User Equipment Height (m) <span class="tooltip"><span class="info-icon">?</span><span class="tooltiptext">User equipment height above ground</span></span></label>
                            <input type="number" id="hUT" name="hUT" value="1.5" min="1" max="22.5" step="0.1" required>
                        </div>
                        
                        <div class="form-group">
                            <label for="voiceUsers">Voice Users per Cell <span class="tooltip"><span class="info-icon">?</span><span class="tooltiptext">Estimated number of simultaneous voice users</span></span></label>
                            <input type="number" id="voiceUsers" name="voiceUsers" value="120" min="10" max="1000" step="10" required>
                        </div>
                    </div>
                </div>
                
                <div class="highlight">
                    <h3>Voice Service Parameters</h3>
                    <div class="form-container">
                        <div class="form-group">
                            <label for="voiceBitrate">Voice Bitrate (kbps) <span class="tooltip"><span class="info-icon">?</span><span class="tooltiptext">Voice codec bit rate</span></span></label>
                            <select id="voiceBitrate" name="voiceBitrate">
                                <option value="12.2">12.2 kbps (AMR-NB)</option>
                                <option value="23.85" selected>23.85 kbps (AMR-WB)</option>
                                <option value="64">64 kbps (G.711)</option>
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label for="voiceMOS">Target MOS Score <span class="tooltip"><span class="info-icon">?</span><span class="tooltiptext">Mean Opinion Score target (1-5)</span></span></label>
                            <select id="voiceMOS" name="voiceMOS">
                                <option value="4.0">4.0 (Excellent)</option>
                                <option value="3.5" selected>3.5 (Good)</option>
                                <option value="3.0">3.0 (Fair)</option>
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label for="coverageProbability">Coverage Probability (%)</label>
                            <input type="number" id="coverageProbability" name="coverageProbability" value="95" min="50" max="99" step="1">
                        </div>
                        
                        <div class="form-group">
                            <label for="streetWidth">Street Width (m)</label>
                            <input type="number" id="streetWidth" name="streetWidth" value="20" min="5" max="50" step="1">
                        </div>
                        
                        <div class="form-group">
                            <label for="buildingHeight">Average Building Height (m)</label>
                            <input type="number" id="buildingHeight" name="buildingHeight" value="20" min="5" max="50" step="1">
                        </div>
                    </div>
                </div>
                
                <div class="button-group">
                    <button type="button" id="calculateBtn">
                        <span>📊</span> Calculate Coverage
                    </button>
                    <button type="button" id="voiceAnalysisBtn">
                        <span>📞</span> Voice Analysis
                    </button>
                    <button type="button" id="compareBtn">
                        <span>⚖️</span> Compare Services
                    </button>
                    <button type="button" id="resetBtn">
                        <span>🔄</span> Reset
                    </button>
                </div>
            </form>
        </section>
        
        <section id="resultsSection" class="results-container">
            <h2>Calculation Results</h2>
            
            <div class="results-grid">
                <div class="result-card">
                    <div class="result-label">Maximum Allowable Path Loss (MAPL)</div>
                    <div class="result-value" id="resultMAPL">--</div>
                    <div class="result-unit">dB</div>
                    <div class="progress-bar">
                        <div class="progress-fill" id="maplProgress"></div>
                    </div>
                </div>
                
                <div class="result-card">
                    <div class="result-label">Coverage Radius (R)</div>
                    <div class="result-value" id="resultRadius">--</div>
                    <div class="result-unit">km</div>
                    <div class="progress-bar">
                        <div class="progress-fill" id="radiusProgress"></div>
                    </div>
                </div>
                
                <div class="result-card">
                    <div class="result-label">Inter-site Distance (D)</div>
                    <div class="result-value" id="resultDistance">--</div>
                    <div class="result-unit">km</div>
                    <div class="progress-bar">
                        <div class="progress-fill" id="distanceProgress"></div>
                    </div>
                </div>
                
                <div class="result-card">
                    <div class="result-label">Sites Required (N)</div>
                    <div class="result-value" id="resultSites">--</div>
                    <div class="result-unit">sites</div>
                    <div class="progress-bar">
                        <div class="progress-fill" id="sitesProgress"></div>
                    </div>
                </div>
            </div>
            
            <div class="highlight">
                <h3>Link Budget Components</h3>
                <table>
                    <thead>
                        <tr>
                            <th>Component</th>
                            <th>Value (dB)</th>
                            <th>Status</th>
                        </tr>
                    </thead>
                    <tbody id="lossesTable">
                        <!-- Populated by JavaScript -->
                    </tbody>
                </table>
            </div>
            
            <div class="charts-container">
                <div class="chart">
                    <h4>Coverage Analysis</h4>
                    <canvas id="coverageChart"></canvas>
                </div>
                
                <div class="chart">
                    <h4>Losses Distribution</h4>
                    <canvas id="lossesChart"></canvas>
                </div>
            </div>
            
            <div class="site-map-container">
                <h4>Site Deployment Visualization</h4>
                <div id="siteMap">
                    <!-- Site map will be drawn here -->
                </div>
                <div class="legend">
                    <div class="legend-item">
                        <div class="legend-color" style="background-color: #667eea;"></div>
                        <span>Base Station</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-color" style="background-color: #4facfe;"></div>
                        <span>Coverage Area</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-color" style="background-color: #f093fb;"></div>
                        <span>Overlap Area</span>
                    </div>
                </div>
            </div>
        </section>
        
        <section id="voiceAnalysisSection" class="voice-analysis-container">
            <h2>Voice Service Analysis</h2>
            
            <div class="voice-analysis-grid">
                <div class="result-card">
                    <div class="result-label">Voice Coverage Radius</div>
                    <div class="result-value" id="voiceRadius">--</div>
                    <div class="result-unit">km</div>
                    <div class="status-indicator status-good" id="voiceCoverageStatus">Good</div>
                </div>
                
                <div class="result-card">
                    <div class="result-label">Voice Quality (MOS)</div>
                    <div class="result-value" id="voiceMOSScore">--</div>
                    <div class="result-unit">Score</div>
                    <div class="status-indicator status-good" id="voiceQualityStatus">Excellent</div>
                </div>
                
                <div class="result-card">
                    <div class="result-label">Voice Capacity</div>
                    <div class="result-value" id="voiceCapacity">--</div>
                    <div class="result-unit">Users/Cell</div>
                    <div class="status-indicator status-good" id="voiceCapacityStatus">Good</div>
                </div>
            </div>
            
            <div class="voice-metrics">
                <div class="voice-metric">
                    <div class="result-label">Body Loss</div>
                    <div class="result-value">3 dB</div>
                    <div class="result-unit">Fixed</div>
                </div>
                
                <div class="voice-metric">
                    <div class="result-label">Required SINR</div>
                    <div class="result-value" id="voiceSINR">-- dB</div>
                    <div class="result-unit">Signal Quality</div>
                </div>
                
                <div class="voice-metric">
                    <div class="result-label">Bit Error Rate</div>
                    <div class="result-value" id="voiceBER">--</div>
                    <div class="result-unit">BER</div>
                </div>
                
                <div class="voice-metric">
                    <div class="result-label">Jitter</div>
                    <div class="result-value" id="voiceJitter">-- ms</div>
                    <div class="result-unit">Delay Variation</div>
                </div>
            </div>
            
            <div class="highlight">
                <h4>Voice Quality Indicator</h4>
                <div class="voice-quality-indicator" id="voiceQualityIndicator">
                    <div class="quality-marker" id="mosMarker" style="left: 80%;">4.0</div>
                </div>
                <div class="quality-labels">
                    <span>Poor (1.0)</span>
                    <span>Fair (2.0)</span>
                    <span>Good (3.0)</span>
                    <span>Very Good (4.0)</span>
                    <span>Excellent (5.0)</span>
                </div>
                
                <h4 style="margin-top: 30px;">Voice Service Parameters</h4>
                <table>
                    <thead>
                        <tr>
                            <th>Parameter</th>
                            <th>Value</th>
                            <th>Target</th>
                            <th>Status</th>
                        </tr>
                    </thead>
                    <tbody id="voiceParametersTable">
                        <!-- Populated by JavaScript -->
                    </tbody>
                </table>
            </div>
            
            <div class="chart">
                <h4>Voice Coverage vs Quality</h4>
                <canvas id="voiceChart"></canvas>
            </div>
        </section>
        
        <section id="comparisonSection" class="comparison-container">
            <h2>Service Coverage Comparison</h2>
            <div class="service-comparison">
                <div class="service-card voice-card">
                    <div class="service-icon">📞</div>
                    <h3>Voice Service</h3>
                    <div class="result-value" id="voiceCoverage">--</div>
                    <div class="result-unit">km coverage</div>
                    <div class="status-indicator status-good" id="voiceStatus">Good</div>
                    <p>Body Loss: 3 dB<br>Data Rate: 23.85 kbps<br>MOS: 4.0</p>
                </div>
                
                <div class="service-card data-card">
                    <div class="service-icon">📱</div>
                    <h3>Data Service</h3>
                    <div class="result-value" id="dataCoverage">--</div>
                    <div class="result-unit">km coverage</div>
                    <div class="status-indicator status-good" id="dataStatus">Good</div>
                    <p>Body Loss: 0 dB<br>Data Rate: 50 Mbps<br>SINR: 10 dB</p>
                </div>
                
                <div class="service-card video-card">
                    <div class="service-icon">🎥</div>
                    <h3>Video Streaming</h3>
                    <div class="result-value" id="videoCoverage">--</div>
                    <div class="result-unit">km coverage</div>
                    <div class="status-indicator status-good" id="videoStatus">Good</div>
                    <p>SINR Required: 15 dB<br>Data Rate: 10 Mbps<br>Buffer: 5 sec</p>
                </div>
            </div>
            
            <div class="highlight" style="margin-top: 30px;">
                <h4>Service Quality Comparison</h4>
                <table>
                    <thead>
                        <tr>
                            <th>Service Type</th>
                            <th>Coverage Radius (km)</th>
                            <th>Required SINR (dB)</th>
                            <th>User Capacity</th>
                            <th>Quality Level</th>
                        </tr>
                    </thead>
                    <tbody id="serviceTable">
                        <!-- Populated by JavaScript -->
                    </tbody>
                </table>
            </div>
        </section>
        
        <footer class="footer">
            <p>4G Network Coverage Planning Tool v2.1 with Voice Service Analysis</p>
            <p>Supports Okumura-Hata, COST-231, and 3GPP 3D-UMA propagation models</p>
            <div class="credits">
                <div class="credit">Designed by Eng. Said Ragab Baz</div>
                <div class="credit">Developed by Eng. Ahmed Karam Ali</div>
            </div>
            <p style="margin-top: 15px;">© 2024 - Advanced Telecommunications Research Group</p>
        </footer>
    </div>

    <script>
        // Data tables and information
        const feederLossTable = {
            "1/2 inch": {700: 8.5, 800: 8.8, 900: 9, 2100: 10.8, 2600: 11},
            "7/8 inch": {700: 4.2, 800: 4.5, 900: 4.9, 2100: 6, 2600: 6.3},
            "1.25 inch": {700: 2.8, 800: 3.0, 900: 3.2, 2100: 4.5, 2600: 4.6},
            "1.625 inch": {700: 2.2, 800: 2.4, 900: 2.6, 2100: 3.5, 2600: 3.8}
        };
        
        const penetrationLossTable = {
            "Rural": 9,
            "Suburban": 13.5,
            "Urban": 19,
            "DenseUrban": 22.5
        };
        
        const foliageLossTable = {
            "Rural": {low: 7.5, high: 8},
            "Suburban": {low: 8.5, high: 15},
            "Urban": {low: 11, high: 17},
            "DenseUrban": {low: 11, high: 17}
        };
        
        const bodyLossTable = {
            "Voice": 3,
            "Data": 0,
            "Video": 0
        };
        
        const fastFadingMarginTable = {
            "Rural": 0,
            "Suburban": 1,
            "Urban": 2,
            "DenseUrban": 2
        };
        
        const slowFadingMarginTable = {
            "Rural": {98: 5.5, 95: 2.9, 90: 0.5, 85: -1.2},
            "Suburban": {98: 5.5, 95: 2.9, 90: 0.5, 85: -1.2},
            "Urban": {98: 8.1, 95: 4.9, 90: 1.8, 85: 0.2},
            "DenseUrban": {98: 10.6, 95: 6.7, 90: 3.1, 85: 0.6}
        };
        
        const rainIceMarginTable = {
            low: 0,    // for frequencies up to 6 GHz
            high: 3    // for frequencies above 6 GHz
        };
        
        // Voice service parameters
        const voiceCodecs = {
            "12.2": {name: "AMR-NB", mosMax: 4.2, sinrReq: 5, ber: "1e-3"},
            "23.85": {name: "AMR-WB", mosMax: 4.5, sinrReq: 7, ber: "1e-4"},
            "64": {name: "G.711", mosMax: 4.8, sinrReq: 10, ber: "1e-5"}
        };
        
        // Service requirements
        const serviceRequirements = {
            "Voice": {
                sinr: 7,
                dataRate: "23.85 kbps",
                usersPerCell: 120,
                color: "#667eea",
                bodyLoss: 3
            },
            "Data": {
                sinr: 10,
                dataRate: "50 Mbps",
                usersPerCell: 50,
                color: "#f093fb",
                bodyLoss: 0
            },
            "Video": {
                sinr: 15,
                dataRate: "10 Mbps",
                usersPerCell: 30,
                color: "#4facfe",
                bodyLoss: 0
            }
        };
        
        // Chart instances
        let coverageChart = null;
        let lossesChart = null;
        let voiceChart = null;
        
        // Calculate feeder loss
        function getFeederLoss(freq) {
            const feederType = "7/8 inch";
            const feederLength = 50; // meters
            
            let lossPer100m = 6; // default value
            
            if (feederLossTable[feederType]) {
                // Find the closest frequency in the table
                const frequencies = Object.keys(feederLossTable[feederType]).map(Number);
                let closestFreq = frequencies[0];
                
                for (const f of frequencies) {
                    if (Math.abs(f - freq) < Math.abs(closestFreq - freq)) {
                        closestFreq = f;
                    }
                }
                
                lossPer100m = feederLossTable[feederType][closestFreq];
            }
            
            return lossPer100m * (feederLength / 100);
        }
        
        // Calculate penetration loss
        function getPenetrationLoss(envType) {
            return penetrationLossTable[envType] || 15;
        }
        
        // Calculate foliage loss
        function getFoliageLoss(freq, envType) {
            const fc_GHz = freq / 1000;
            const lossInfo = foliageLossTable[envType] || foliageLossTable["Urban"];
            
            if (fc_GHz <= 3.5) {
                return lossInfo.low;
            } else {
                return lossInfo.high;
            }
        }
        
        // Calculate body loss
        function getBodyLoss(serviceType) {
            return bodyLossTable[serviceType] || 0;
        }
        
        // Calculate fast fading margin
        function getFastFadingMargin(envType) {
            return fastFadingMarginTable[envType] || 1;
        }
        
        // Calculate slow fading margin
        function getSlowFadingMargin(envType, coverageProb) {
            const table = slowFadingMarginTable[envType] || slowFadingMarginTable["Urban"];
            
            // Round to nearest value in table
            const probabilities = Object.keys(table).map(Number);
            let closestProb = 95; // default value
            
            for (const prob of probabilities) {
                if (Math.abs(prob - coverageProb) < Math.abs(closestProb - coverageProb)) {
                    closestProb = prob;
                }
            }
            
            return table[closestProb];
        }
        
        // Calculate rain/ice margin
        function getRainIceMargin(freq) {
            const fc_GHz = freq / 1000;
            if (fc_GHz <= 6) {
                return rainIceMarginTable.low;
            } else {
                return rainIceMarginTable.high;
            }
        }
        
        // Calculate MAPL for voice service
        function calculateVoiceMAPL(inputs, voiceCodec) {
            const freq = parseInt(inputs.frequency);
            
            // Get losses
            const feederLoss_db = getFeederLoss(freq);
            const pen_loss_db = getPenetrationLoss(inputs.environment);
            const foliage_loss_db = getFoliageLoss(freq, inputs.environment);
            const body_loss_db = 3; // Fixed body loss for voice
            const fast_fade_db = getFastFadingMargin(inputs.environment);
            const slow_fade_db = getSlowFadingMargin(inputs.environment, inputs.coverageProbability);
            const rain_margin_db = getRainIceMargin(freq);
            
            // Additional values for voice
            const interf_margin_db = 5; // Higher for voice due to real-time requirements
            const thermal_noise_dBm = -174 + 10 * Math.log10(5e6); // For 5 MHz bandwidth (voice optimized)
            const noise_figure_db = 7;
            const sinr_req_db = voiceCodec.sinrReq; // From codec requirements
            
            // Calculate MAPL for voice
            const MAPL = inputs.txPower + inputs.txGain - feederLoss_db - pen_loss_db -
                        foliage_loss_db - body_loss_db - interf_margin_db - rain_margin_db -
                        slow_fade_db + inputs.rxGain - thermal_noise_dBm - noise_figure_db -
                        sinr_req_db;
            
            return {
                MAPL: MAPL,
                losses: {
                    feederLoss: feederLoss_db,
                    penetrationLoss: pen_loss_db,
                    foliageLoss: foliage_loss_db,
                    bodyLoss: body_loss_db,
                    interferenceMargin: interf_margin_db,
                    rainIceMargin: rain_margin_db,
                    slowFadingMargin: slow_fade_db,
                    fastFadingMargin: fast_fade_db,
                    thermalNoise: thermal_noise_dBm,
                    noiseFigure: noise_figure_db,
                    sinrRequired: sinr_req_db,
                    txPower: inputs.txPower,
                    txGain: inputs.txGain,
                    rxGain: inputs.rxGain
                },
                voiceSpecific: {
                    codecName: voiceCodec.name,
                    requiredSINR: sinr_req_db,
                    maxMOS: voiceCodec.mosMax,
                    targetMOS: parseFloat(inputs.voiceMOS)
                }
            };
        }
        
        // Calculate radius using Okumura-Hata
        function okumuraHataRadius(MAPL, freq, hBS, hUT, envType) {
            // Calculate a(hUT)
            let a_hUT = 0;
            if (envType === "Urban") {
                a_hUT = 3.2 * Math.pow(Math.log10(11.75 * hUT), 2) - 4.97;
            } else if (envType === "Suburban" || envType === "Rural") {
                a_hUT = (1.1 * Math.log10(freq) - 0.7) * hUT - (1.56 * Math.log10(freq) - 0.8);
            }
            
            // Path loss at 1 km
            const PL_1km = 69.55 + 26.16 * Math.log10(freq) - 13.82 * Math.log10(hBS) - a_hUT;
            
            // Calculate radius
            const R_km = Math.pow(10, (MAPL - PL_1km) / (44.9 - 6.55 * Math.log10(hBS)));
            
            // Limit to reasonable value
            return Math.min(R_km, 50);
        }
        
        // Calculate radius using COST-231
        function cost231HataRadius(MAPL, freq, hBS, hUT, envType) {
            // Calculate a(hUT)
            const a_hUT = (1.1 * Math.log10(freq) - 0.7) * hUT - (1.56 * Math.log10(freq) - 0.8);
            
            // Path loss at 1 km
            let PL_1km = 46.3 + 33.9 * Math.log10(freq) - 13.82 * Math.log10(hBS) - a_hUT;
            
            // Environment correction
            if (envType === "Urban") {
                PL_1km += 3;
            }
            
            // Calculate radius
            const R_km = Math.pow(10, (MAPL - PL_1km) / (44.9 - 6.55 * Math.log10(hBS)));
            
            // Limit to reasonable value
            return Math.min(R_km, 50);
        }
        
        // Calculate radius using 3GPP 3D-UMA
        function threegppUmaRadius(MAPL, freq, hBS, hUT, W, h_avg) {
            const fc_GHz = freq / 1000;
            
            // Model constants
            const PL_const = 161.04 - 7.1 * Math.log10(W) + 7.5 * Math.log10(h_avg) -
                           (24.37 - 3.7 * Math.pow(h_avg / hBS, 2)) * Math.log10(hBS) +
                           20 * Math.log10(fc_GHz) -
                           (3.2 * Math.pow(Math.log10(17.625), 2) - 4.97) - 0.6 * (hUT - 1.5);
            
            // Distance coefficient
            const coeff_d = 43.42 - 3.1 * Math.log10(hBS);
            
            // Calculate 3D distance (in meters)
            const d_3D_m = Math.pow(10, (MAPL - PL_const) / coeff_d + 3);
            
            // Convert to 2D distance (approximation)
            const R_km = Math.sqrt(Math.pow(d_3D_m, 2) - Math.pow(hBS - hUT, 2)) / 1000;
            
            // Limit to reasonable value
            return Math.min(R_km, 5); // UMA model valid up to 5 km
        }
        
        // Calculate number of sites
        function calculateSites(R_km, area_km2, antennaType) {
            // Calculate site area
            let A_site_km2;
            if (antennaType === "Directive") {
                A_site_km2 = 1.94 * Math.pow(R_km, 2);
            } else { // Omni
                A_site_km2 = 2.5 * Math.pow(R_km, 2);
            }
            
            // Number of sites
            const N_sites = area_km2 / A_site_km2;
            const N_sites_ceil = Math.ceil(N_sites);
            
            // Inter-site distance
            const D_km = 1.5 * R_km;
            
            return {
                siteArea: A_site_km2,
                sitesRequired: N_sites,
                sitesRequiredCeil: N_sites_ceil,
                interSiteDistance: D_km
            };
        }
        
        // Calculate voice quality metrics
        function calculateVoiceQuality(inputs, radius, maplResult) {
            const voiceBitrate = parseFloat(inputs.voiceBitrate);
            const targetMOS = parseFloat(inputs.voiceMOS);
            const voiceUsers = parseInt(inputs.voiceUsers);
            
            // Get codec information
            const codec = voiceCodecs[voiceBitrate.toString()] || voiceCodecs["23.85"];
            
            // Calculate actual MOS based on SINR margin
            const sinrMargin = maplResult.losses.sinrRequired - 5; // Margin over minimum
            const mosScore = Math.min(codec.mosMax, targetMOS + (sinrMargin * 0.1));
            
            // Calculate voice coverage radius (typically 90% of data coverage)
            const voiceRadius = radius * 0.9;
            
            // Calculate voice capacity
            const voiceCapacity = Math.min(voiceUsers, 180); // Max practical limit
            
            // Calculate additional voice metrics
            const ber = codec.ber;
            const jitter = 10 + (100 - (sinrMargin * 10)); // ms, lower SINR = higher jitter
            const packetLoss = Math.max(0, 1 - (sinrMargin / 10));
            
            return {
                radius: voiceRadius,
                mosScore: mosScore,
                capacity: voiceCapacity,
                sinrRequired: codec.sinrReq,
                ber: ber,
                jitter: jitter.toFixed(1),
                packetLoss: (packetLoss * 100).toFixed(2),
                codecName: codec.name,
                targetMOS: targetMOS,
                status: getVoiceStatus(mosScore, voiceRadius)
            };
        }
        
        // Get voice service status
        function getVoiceStatus(mosScore, radius) {
            if (mosScore >= 4.0 && radius >= 1.0) return "Excellent";
            if (mosScore >= 3.5 && radius >= 0.8) return "Good";
            if (mosScore >= 3.0 && radius >= 0.5) return "Fair";
            return "Poor";
        }
        
        // Draw site map
        function drawSiteMap(radius_km, numSites, area_km2) {
            const siteMap = document.getElementById('siteMap');
            siteMap.innerHTML = '';
            
            const width = siteMap.clientWidth;
            const height = siteMap.clientHeight;
            
            const canvas = document.createElement('canvas');
            canvas.width = width;
            canvas.height = height;
            siteMap.appendChild(canvas);
            
            const ctx = canvas.getContext('2d');
            
            // Clear canvas
            ctx.clearRect(0, 0, width, height);
            
            // Draw background
            ctx.fillStyle = '#f8fbff';
            ctx.fillRect(0, 0, width, height);
            
            // Calculate number of rows and columns
            const side_km = Math.sqrt(area_km2);
            const radius_px = (radius_km / side_km) * Math.min(width, height) * 0.4;
            
            const rows = Math.ceil(Math.sqrt(numSites));
            const cols = Math.ceil(numSites / rows);
            
            const cellWidth = width / cols;
            const cellHeight = height / rows;
            
            // Draw coverage areas
            for (let i = 0; i < rows; i++) {
                for (let j = 0; j < cols; j++) {
                    const x = j * cellWidth + cellWidth / 2;
                    const y = i * cellHeight + cellHeight / 2;
                    
                    // Draw coverage area (hexagon)
                    ctx.beginPath();
                    for (let k = 0; k < 6; k++) {
                        const angle = (Math.PI / 3) * k;
                        const hexX = x + radius_px * Math.cos(angle);
                        const hexY = y + radius_px * Math.sin(angle);
                        
                        if (k === 0) {
                            ctx.moveTo(hexX, hexY);
                        } else {
                            ctx.lineTo(hexX, hexY);
                        }
                    }
                    ctx.closePath();
                    ctx.fillStyle = 'rgba(79, 172, 254, 0.2)';
                    ctx.fill();
                    ctx.strokeStyle = '#4facfe';
                    ctx.lineWidth = 2;
                    ctx.stroke();
                    
                    // Draw overlap areas
                    if (i < rows - 1 && j < cols - 1) {
                        ctx.beginPath();
                        ctx.arc(x + cellWidth / 2, y + cellHeight / 2, radius_px * 0.3, 0, Math.PI * 2);
                        ctx.fillStyle = 'rgba(240, 147, 251, 0.3)';
                        ctx.fill();
                    }
                }
            }
            
            // Draw base stations
            let siteCount = 0;
            for (let i = 0; i < rows; i++) {
                for (let j = 0; j < cols; j++) {
                    if (siteCount >= numSites) break;
                    
                    const x = j * cellWidth + cellWidth / 2;
                    const y = i * cellHeight + cellHeight / 2;
                    
                    // Draw base station
                    ctx.beginPath();
                    ctx.arc(x, y, 8, 0, Math.PI * 2);
                    ctx.fillStyle = '#667eea';
                    ctx.fill();
                    ctx.strokeStyle = '#2c5aa0';
                    ctx.lineWidth = 2;
                    ctx.stroke();
                    
                    // Draw tower
                    ctx.beginPath();
                    ctx.moveTo(x, y + 8);
                    ctx.lineTo(x, y + 20);
                    ctx.strokeStyle = '#2c5aa0';
                    ctx.lineWidth = 3;
                    ctx.stroke();
                    
                    siteCount++;
                }
            }
            
            // Draw legend on canvas
            ctx.font = '14px Arial';
            ctx.fillStyle = '#333';
            ctx.fillText(`Coverage Radius: ${radius_km.toFixed(2)} km`, 10, 30);
            ctx.fillText(`Sites: ${numSites}`, 10, 50);
            ctx.fillText(`Inter-site Distance: ${(1.5 * radius_km).toFixed(2)} km`, 10, 70);
        }
        
        // Create charts
        function createCharts(results, inputs, voiceResults) {
            // Coverage Chart
            const coverageCtx = document.getElementById('coverageChart').getContext('2d');
            
            if (coverageChart) {
                coverageChart.destroy();
            }
            
            coverageChart = new Chart(coverageCtx, {
                type: 'radar',
                data: {
                    labels: ['Coverage Radius', 'Site Capacity', 'Signal Quality', 'Service Area', 'Network Density', 'Voice Quality'],
                    datasets: [{
                        label: 'Coverage Performance',
                        data: [
                            Math.min(results.radius * 10, 100),
                            Math.min((results.sitesRequired / inputs.area) * 100, 100),
                            85,
                            Math.min((results.siteArea / inputs.area) * 500, 100),
                            Math.min(results.sitesRequired * 2, 100),
                            Math.min(voiceResults.mosScore * 20, 100)
                        ],
                        backgroundColor: 'rgba(102, 126, 234, 0.2)',
                        borderColor: 'rgba(102, 126, 234, 1)',
                        borderWidth: 2,
                        pointBackgroundColor: 'rgba(102, 126, 234, 1)',
                        pointBorderColor: '#fff',
                        pointBorderWidth: 2
                    }]
                },
                options: {
                    scales: {
                        r: {
                            beginAtZero: true,
                            max: 100,
                            ticks: {
                                stepSize: 20
                            }
                        }
                    },
                    plugins: {
                        legend: {
                            display: false
                        }
                    }
                }
            });
            
            // Losses Chart
            const lossesCtx = document.getElementById('lossesChart').getContext('2d');
            
            if (lossesChart) {
                lossesChart.destroy();
            }
            
            lossesChart = new Chart(lossesCtx, {
                type: 'doughnut',
                data: {
                    labels: ['Feeder Loss', 'Penetration Loss', 'Foliage Loss', 'Body Loss', 'Fading Margins', 'Other Losses'],
                    datasets: [{
                        data: [
                            results.losses.feederLoss,
                            results.losses.penetrationLoss,
                            results.losses.foliageLoss,
                            results.losses.bodyLoss,
                            results.losses.slowFadingMargin + results.losses.fastFadingMargin,
                            results.losses.interferenceMargin + results.losses.rainIceMargin
                        ],
                        backgroundColor: [
                            '#667eea',
                            '#764ba2',
                            '#f093fb',
                            '#4facfe',
                            '#00f2fe',
                            '#f5576c'
                        ],
                        borderWidth: 1,
                        borderColor: '#fff'
                    }]
                },
                options: {
                    plugins: {
                        legend: {
                            position: 'right'
                        }
                    }
                }
            });
            
            // Voice Chart
            const voiceCtx = document.getElementById('voiceChart').getContext('2d');
            
            if (voiceChart) {
                voiceChart.destroy();
            }
            
            voiceChart = new Chart(voiceCtx, {
                type: 'bar',
                data: {
                    labels: ['Coverage Radius', 'MOS Score', 'User Capacity', 'SINR Margin', 'Jitter', 'Packet Loss'],
                    datasets: [{
                        label: 'Voice Service Metrics',
                        data: [
                            voiceResults.radius * 10,
                            voiceResults.mosScore * 20,
                            voiceResults.capacity / 2,
                            (results.losses.sinrRequired - 5) * 10,
                            100 - voiceResults.jitter,
                            100 - (voiceResults.packetLoss * 10)
                        ],
                        backgroundColor: [
                            'rgba(102, 126, 234, 0.7)',
                            'rgba(67, 233, 123, 0.7)',
                            'rgba(240, 147, 251, 0.7)',
                            'rgba(79, 172, 254, 0.7)',
                            'rgba(0, 242, 254, 0.7)',
                            'rgba(245, 87, 108, 0.7)'
                        ],
                        borderColor: [
                            '#667eea',
                            '#43e97b',
                            '#f093fb',
                            '#4facfe',
                            '#00f2fe',
                            '#f5576c'
                        ],
                        borderWidth: 1
                    }]
                },
                options: {
                    scales: {
                        y: {
                            beginAtZero: true,
                            max: 100,
                            title: {
                                display: true,
                                text: 'Score (%)'
                            }
                        }
                    },
                    plugins: {
                        legend: {
                            display: false
                        }
                    }
                }
            });
        }
        
        // Update voice analysis section
        function updateVoiceAnalysis(voiceResults) {
            document.getElementById('voiceRadius').textContent = voiceResults.radius.toFixed(2);
            document.getElementById('voiceMOSScore').textContent = voiceResults.mosScore.toFixed(1);
            document.getElementById('voiceCapacity').textContent = voiceResults.capacity;
            document.getElementById('voiceSINR').textContent = voiceResults.sinrRequired + " dB";
            document.getElementById('voiceBER').textContent = voiceResults.ber;
            document.getElementById('voiceJitter').textContent = voiceResults.jitter;
            
            // Update status indicators
            updateVoiceStatusIndicators(voiceResults);
            
            // Update MOS marker position
            const mosPercentage = ((voiceResults.mosScore - 1) / 4) * 100;
            document.getElementById('mosMarker').style.left = `${mosPercentage}%`;
            document.getElementById('mosMarker').textContent = voiceResults.mosScore.toFixed(1);
            
            // Update voice parameters table
            const voiceTable = document.getElementById('voiceParametersTable');
            voiceTable.innerHTML = '';
            
            const parameters = [
                {name: "Codec", value: voiceResults.codecName, target: "AMR-WB", status: "Good"},
                {name: "Bit Rate", value: document.getElementById('voiceBitrate').value + " kbps", target: "≥12.2 kbps", status: "Good"},
                {name: "MOS Score", value: voiceResults.mosScore.toFixed(1), target: "≥" + voiceResults.targetMOS, status: voiceResults.mosScore >= voiceResults.targetMOS ? "Good" : "Warning"},
                {name: "Coverage Radius", value: voiceResults.radius.toFixed(2) + " km", target: "≥0.8 km", status: voiceResults.radius >= 0.8 ? "Good" : "Warning"},
                {name: "User Capacity", value: voiceResults.capacity + " users", target: "≥100 users", status: voiceResults.capacity >= 100 ? "Good" : "Warning"},
                {name: "Body Loss", value: "3 dB", target: "3 dB", status: "Good"},
                {name: "Jitter", value: voiceResults.jitter + " ms", target: "≤30 ms", status: parseFloat(voiceResults.jitter) <= 30 ? "Good" : "Warning"},
                {name: "Packet Loss", value: voiceResults.packetLoss + "%", target: "≤1%", status: parseFloat(voiceResults.packetLoss) <= 1 ? "Good" : "Warning"}
            ];
            
            parameters.forEach(param => {
                const row = document.createElement('tr');
                const statusClass = param.status === "Good" ? "status-good" : "status-warning";
                
                row.innerHTML = `
                    <td>${param.name}</td>
                    <td>${param.value}</td>
                    <td>${param.target}</td>
                    <td><span class="status-indicator ${statusClass}">${param.status}</span></td>
                `;
                voiceTable.appendChild(row);
            });
        }
        
        // Update voice status indicators
        function updateVoiceStatusIndicators(voiceResults) {
            const status = voiceResults.status;
            let statusClass = "status-good";
            
            if (status === "Excellent") statusClass = "status-good";
            else if (status === "Good") statusClass = "status-good";
            else if (status === "Fair") statusClass = "status-warning";
            else statusClass = "status-poor";
            
            document.getElementById('voiceCoverageStatus').className = `status-indicator ${statusClass}`;
            document.getElementById('voiceCoverageStatus').textContent = status;
            
            document.getElementById('voiceQualityStatus').className = `status-indicator ${statusClass}`;
            document.getElementById('voiceQualityStatus').textContent = status;
            
            const capacityStatus = voiceResults.capacity >= 100 ? "Good" : "Warning";
            const capacityClass = capacityStatus === "Good" ? "status-good" : "status-warning";
            document.getElementById('voiceCapacityStatus').className = `status-indicator ${capacityClass}`;
            document.getElementById('voiceCapacityStatus').textContent = capacityStatus;
        }
        
        // Update comparison section
        function updateComparison(radius, voiceResults) {
            const voiceCoverage = voiceResults.radius;
            const dataCoverage = radius * 0.95;
            const videoCoverage = radius * 0.8;
            
            document.getElementById('voiceCoverage').textContent = voiceCoverage.toFixed(2);
            document.getElementById('dataCoverage').textContent = dataCoverage.toFixed(2);
            document.getElementById('videoCoverage').textContent = videoCoverage.toFixed(2);
            
            // Update status indicators
            updateServiceStatus('voiceStatus', voiceCoverage, voiceResults.mosScore);
            updateServiceStatus('dataStatus', dataCoverage, 0);
            updateServiceStatus('videoStatus', videoCoverage, 0);
            
            // Update service table
            const serviceTable = document.getElementById('serviceTable');
            serviceTable.innerHTML = '';
            
            const services = [
                {
                    type: 'Voice', 
                    coverage: voiceCoverage,
                    sinr: voiceResults.sinrRequired,
                    capacity: voiceResults.capacity,
                    quality: voiceResults.status
                },
                {
                    type: 'Data', 
                    coverage: dataCoverage,
                    sinr: 10,
                    capacity: 50,
                    quality: dataCoverage >= 1.0 ? "Good" : "Fair"
                },
                {
                    type: 'Video', 
                    coverage: videoCoverage,
                    sinr: 15,
                    capacity: 30,
                    quality: videoCoverage >= 0.8 ? "Good" : "Fair"
                }
            ];
            
            services.forEach(service => {
                const row = document.createElement('tr');
                const qualityClass = service.quality === "Good" || service.quality === "Excellent" ? "status-good" : 
                                   service.quality === "Fair" ? "status-warning" : "status-poor";
                
                row.innerHTML = `
                    <td>${service.type}</td>
                    <td>${service.coverage.toFixed(2)} km</td>
                    <td>${service.sinr} dB</td>
                    <td>${service.capacity} users</td>
                    <td><span class="status-indicator ${qualityClass}">${service.quality}</span></td>
                `;
                serviceTable.appendChild(row);
            });
        }
        
        // Update service status
        function updateServiceStatus(elementId, coverage, mosScore) {
            const element = document.getElementById(elementId);
            let quality = "Good";
            let qualityClass = "status-good";
            
            if (elementId === 'voiceStatus') {
                quality = mosScore >= 3.5 ? "Good" : mosScore >= 3.0 ? "Fair" : "Poor";
            } else {
                quality = coverage >= 1.0 ? "Good" : coverage >= 0.5 ? "Fair" : "Poor";
            }
            
            qualityClass = quality === "Good" ? "status-good" : 
                          quality === "Fair" ? "status-warning" : "status-poor";
            
            element.className = `status-indicator ${qualityClass}`;
            element.textContent = quality;
        }
        
        // Update progress bars
        function updateProgressBars(results, inputs) {
            const maplProgress = Math.min((results.MAPL + 50) / 2, 100);
            const radiusProgress = Math.min(results.radius * 20, 100);
            const distanceProgress = Math.min(results.interSiteDistance * 15, 100);
            const sitesProgress = Math.min((results.sitesRequiredCeil / 50) * 100, 100);
            
            document.getElementById('maplProgress').style.width = `${maplProgress}%`;
            document.getElementById('radiusProgress').style.width = `${radiusProgress}%`;
            document.getElementById('distanceProgress').style.width = `${distanceProgress}%`;
            document.getElementById('sitesProgress').style.width = `${sitesProgress}%`;
        }
        
        // Update UI with results
        function updateResults(results, voiceResults) {
            document.getElementById('resultMAPL').textContent = results.MAPL.toFixed(2);
            document.getElementById('resultRadius').textContent = results.radius.toFixed(3);
            document.getElementById('resultDistance').textContent = results.interSiteDistance.toFixed(3);
            document.getElementById('resultSites').textContent = `${results.sitesRequiredCeil} (${results.sitesRequired.toFixed(2)})`;
            
            // Update losses table
            const lossesTable = document.getElementById('lossesTable');
            lossesTable.innerHTML = '';
            
            const losses = [
                {name: 'Transmit Power', value: results.losses.txPower, threshold: 40, good: '>40 dBm'},
                {name: 'Transmit Gain', value: results.losses.txGain, threshold: 15, good: '>15 dBi'},
                {name: 'Feeder Loss', value: -results.losses.feederLoss, threshold: -5, good: '<5 dB'},
                {name: 'Penetration Loss', value: -results.losses.penetrationLoss, threshold: -20, good: '<20 dB'},
                {name: 'Foliage Loss', value: -results.losses.foliageLoss, threshold: -15, good: '<15 dB'},
                {name: 'Body Loss', value: -results.losses.bodyLoss, threshold: -3, good: '<3 dB'},
                {name: 'Slow Fading Margin', value: -results.losses.slowFadingMargin, threshold: -10, good: '<10 dB'},
                {name: 'Fast Fading Margin', value: -results.losses.fastFadingMargin, threshold: -5, good: '<5 dB'},
                {name: 'Receiver Gain', value: results.losses.rxGain, threshold: 0, good: '≥0 dBi'},
                {name: 'Thermal Noise', value: -results.losses.thermalNoise, threshold: -100, good: '<-100 dBm'}
            ];
            
            losses.forEach(loss => {
                const row = document.createElement('tr');
                const status = loss.value >= loss.threshold ? 'Good' : 'Check';
                const statusClass = loss.value >= loss.threshold ? 'status-good' : 'status-warning';
                
                row.innerHTML = `
                    <td>${loss.name}</td>
                    <td>${loss.value.toFixed(2)}</td>
                    <td><span class="status-indicator ${statusClass}">${status}</span></td>
                `;
                lossesTable.appendChild(row);
            });
            
            // Update progress bars
            updateProgressBars(results, results.inputs);
            
            // Draw site map
            drawSiteMap(results.radius, results.sitesRequiredCeil, results.inputs.area);
            
            // Create charts
            createCharts(results, results.inputs, voiceResults);
            
            // Update voice analysis
            updateVoiceAnalysis(voiceResults);
            
            // Update comparison
            updateComparison(results.radius, voiceResults);
            
            // Show results section
            document.getElementById('resultsSection').style.display = 'block';
            
            // Hide other sections
            document.getElementById('voiceAnalysisSection').style.display = 'none';
            document.getElementById('comparisonSection').style.display = 'none';
            
            // Scroll to results
            document.getElementById('resultsSection').scrollIntoView({behavior: 'smooth'});
        }
        
        // Gather inputs from form
        function gatherInputs() {
            return {
                area: parseFloat(document.getElementById('area').value),
                frequency: parseInt(document.getElementById('frequency').value),
                environment: document.getElementById('environment').value,
                propagationModel: document.getElementById('propagationModel').value,
                txPower: parseFloat(document.getElementById('txPower').value),
                txGain: parseFloat(document.getElementById('txGain').value),
                antennaType: document.getElementById('antennaType').value,
                hBS: parseFloat(document.getElementById('hBS').value),
                rxSensitivity: parseFloat(document.getElementById('rxSensitivity').value),
                rxGain: parseFloat(document.getElementById('rxGain').value),
                hUT: parseFloat(document.getElementById('hUT').value),
                voiceUsers: parseInt(document.getElementById('voiceUsers').value),
                voiceBitrate: document.getElementById('voiceBitrate').value,
                voiceMOS: document.getElementById('voiceMOS').value,
                coverageProbability: parseInt(document.getElementById('coverageProbability').value),
                streetWidth: parseFloat(document.getElementById('streetWidth').value),
                buildingHeight: parseFloat(document.getElementById('buildingHeight').value)
            };
        }
        
        // Handle calculate button click
        document.getElementById('calculateBtn').addEventListener('click', function() {
            const inputs = gatherInputs();
            
            // Get voice codec information
            const voiceCodec = voiceCodecs[inputs.voiceBitrate] || voiceCodecs["23.85"];
            
            // Calculate MAPL for voice service
            const maplResult = calculateVoiceMAPL(inputs, voiceCodec);
            
            // Calculate radius using selected propagation model
            let radius;
            switch(inputs.propagationModel) {
                case 'OkumuraHata':
                    radius = okumuraHataRadius(maplResult.MAPL, inputs.frequency, inputs.hBS, inputs.hUT, inputs.environment);
                    break;
                case 'COST231':
                    radius = cost231HataRadius(maplResult.MAPL, inputs.frequency, inputs.hBS, inputs.hUT, inputs.environment);
                    break;
                case '3GPP_UMA':
                    radius = threegppUmaRadius(maplResult.MAPL, inputs.frequency, inputs.hBS, inputs.hUT, inputs.streetWidth, inputs.buildingHeight);
                    break;
                default:
                    radius = cost231HataRadius(maplResult.MAPL, inputs.frequency, inputs.hBS, inputs.hUT, inputs.environment);
            }
            
            // Calculate number of sites
            const sitesResult = calculateSites(radius, inputs.area, inputs.antennaType);
            
            // Calculate voice quality metrics
            const voiceResults = calculateVoiceQuality(inputs, radius, maplResult);
            
            // Compile results
            const results = {
                MAPL: maplResult.MAPL,
                radius: radius,
                siteArea: sitesResult.siteArea,
                sitesRequired: sitesResult.sitesRequired,
                sitesRequiredCeil: sitesResult.sitesRequiredCeil,
                interSiteDistance: sitesResult.interSiteDistance,
                losses: maplResult.losses,
                inputs: inputs
            };
            
            // Update UI
            updateResults(results, voiceResults);
        });
        
        // Handle voice analysis button click
        document.getElementById('voiceAnalysisBtn').addEventListener('click', function() {
            document.getElementById('voiceAnalysisSection').style.display = 'block';
            document.getElementById('comparisonSection').style.display = 'none';
            document.getElementById('voiceAnalysisSection').scrollIntoView({behavior: 'smooth'});
        });
        
        // Handle compare button click
        document.getElementById('compareBtn').addEventListener('click', function() {
            document.getElementById('comparisonSection').style.display = 'block';
            document.getElementById('voiceAnalysisSection').style.display = 'none';
            document.getElementById('comparisonSection').scrollIntoView({behavior: 'smooth'});
        });
        
        // Handle reset button click
        document.getElementById('resetBtn').addEventListener('click', function() {
            document.getElementById('coverageForm').reset();
            document.getElementById('resultsSection').style.display = 'none';
            document.getElementById('voiceAnalysisSection').style.display = 'none';
            document.getElementById('comparisonSection').style.display = 'none';
        });
        
        // Initialize page
        document.addEventListener('DOMContentLoaded', function() {
            // Set default values
            document.getElementById('frequency').value = '2100';
            document.getElementById('environment').value = 'Urban';
            document.getElementById('propagationModel').value = '3GPP_UMA';
            document.getElementById('antennaType').value = 'Directive';
            document.getElementById('voiceBitrate').value = '23.85';
            document.getElementById('voiceMOS').value = '3.5';
            
            // Initial example calculation
            setTimeout(() => {
                document.getElementById('calculateBtn').click();
            }, 1000);
        });
    </script>
</body>
</html>
