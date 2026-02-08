
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>4G Network Capacity Dimensioning</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://html2canvas.hertzen.com/dist/html2canvas.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        :root {
            --primary-color: #0a0a0a;
            --secondary-color: #1a1a1a;
            --accent-color: #00b4d8;
            --accent-dark: #0077b6;
            --text-color: #f8f9fa;
            --text-secondary: #adb5bd;
            --border-color: #333;
            --card-bg: rgba(25, 25, 35, 0.95);
            --success-color: #2ecc71;
            --warning-color: #f39c12;
            --info-color: #3498db;
            --gradient-start: #0a0a0a;
            --gradient-end: #1a1a2e;
        }
        
        body {
            background: linear-gradient(135deg, var(--gradient-start) 0%, var(--gradient-end) 100%);
            color: var(--text-color);
            line-height: 1.6;
            min-height: 100vh;
            overflow-x: hidden;
        }
        
        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: 
                radial-gradient(circle at 20% 80%, rgba(0, 180, 216, 0.08) 0%, transparent 40%),
                radial-gradient(circle at 80% 20%, rgba(0, 119, 182, 0.08) 0%, transparent 40%),
                radial-gradient(circle at 40% 40%, rgba(46, 204, 113, 0.05) 0%, transparent 30%);
            pointer-events: none;
            z-index: -1;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        header {
            text-align: center;
            padding: 40px 0 30px;
            margin-bottom: 30px;
            position: relative;
            border-bottom: 1px solid rgba(51, 51, 51, 0.5);
        }
        
        .header-content {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 15px;
        }
        
        .logo-container {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-bottom: 10px;
        }
        
        .logo-icon {
            font-size: 2.8rem;
            color: var(--accent-color);
            background: linear-gradient(135deg, var(--accent-color), var(--accent-dark));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            filter: drop-shadow(0 0 15px rgba(0, 180, 216, 0.3));
        }
        
        h1 {
            background: linear-gradient(135deg, var(--accent-color), #90e0ef);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            font-size: 2.8rem;
            font-weight: 800;
            letter-spacing: -0.5px;
        }
        
        .subtitle {
            color: var(--text-secondary);
            font-size: 1.1rem;
            max-width: 600px;
            margin: 0 auto;
            line-height: 1.6;
        }
        
        .quick-actions {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 20px;
            flex-wrap: wrap;
        }
        
        .calculation-steps {
            display: flex;
            flex-direction: column;
            gap: 30px;
            margin-bottom: 40px;
        }
        
        .step-card {
            background: var(--card-bg);
            border-radius: 16px;
            padding: 30px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
            backdrop-filter: blur(10px);
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        
        .step-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 4px;
            height: 100%;
            background: linear-gradient(to bottom, var(--accent-color), var(--accent-dark));
        }
        
        .step-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 12px 40px rgba(0, 0, 0, 0.3);
            border-color: rgba(0, 180, 216, 0.3);
        }
        
        .step-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
        }
        
        .step-title-container {
            display: flex;
            align-items: center;
            gap: 15px;
        }
        
        .step-icon {
            width: 50px;
            height: 50px;
            background: linear-gradient(135deg, rgba(0, 180, 216, 0.2), rgba(0, 119, 182, 0.2));
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            color: var(--accent-color);
        }
        
        .step-title {
            font-size: 1.5rem;
            font-weight: 600;
            color: var(--text-color);
        }
        
        .step-number {
            background: linear-gradient(135deg, var(--accent-color), var(--accent-dark));
            color: white;
            width: 45px;
            height: 45px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 1.2rem;
            box-shadow: 0 4px 15px rgba(0, 180, 216, 0.3);
        }
        
        .input-group {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 25px;
        }
        
        .input-field {
            display: flex;
            flex-direction: column;
        }
        
        label {
            margin-bottom: 8px;
            color: var(--text-secondary);
            font-weight: 500;
            font-size: 0.95rem;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .input-with-icon {
            position: relative;
        }
        
        .input-icon {
            position: absolute;
            left: 15px;
            top: 50%;
            transform: translateY(-50%);
            color: var(--text-secondary);
        }
        
        input, select {
            padding: 14px 15px 14px 45px;
            background: rgba(0, 0, 0, 0.4);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            color: var(--text-color);
            font-size: 1rem;
            transition: all 0.3s ease;
            width: 100%;
        }
        
        input:focus, select:focus {
            outline: none;
            border-color: var(--accent-color);
            box-shadow: 0 0 0 3px rgba(0, 180, 216, 0.1);
        }
        
        .btn-container {
            display: flex;
            gap: 15px;
            margin-top: 10px;
            flex-wrap: wrap;
        }
        
        .btn {
            padding: 14px 28px;
            border-radius: 10px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            border: none;
        }
        
        .btn-primary {
            background: linear-gradient(135deg, var(--accent-color), var(--accent-dark));
            color: white;
        }
        
        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(0, 180, 216, 0.3);
        }
        
        .btn-secondary {
            background: rgba(255, 255, 255, 0.1);
            color: var(--text-color);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .btn-secondary:hover {
            background: rgba(255, 255, 255, 0.15);
            transform: translateY(-2px);
        }
        
        .btn-success {
            background: linear-gradient(135deg, var(--success-color), #27ae60);
            color: white;
        }
        
        .btn-success:hover {
            background: linear-gradient(135deg, #27ae60, #219653);
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(46, 204, 113, 0.3);
        }
        
        .calculate-all-btn {
            display: block;
            margin: 40px auto;
            padding: 18px 50px;
            font-size: 1.2rem;
            background: linear-gradient(135deg, var(--accent-color), var(--accent-dark));
            color: white;
            border-radius: 12px;
            border: none;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 8px 25px rgba(0, 180, 216, 0.3);
        }
        
        .calculate-all-btn:hover {
            transform: translateY(-3px) scale(1.02);
            box-shadow: 0 12px 30px rgba(0, 180, 216, 0.4);
        }
        
        .result-box {
            background: rgba(0, 180, 216, 0.08);
            border: 1px solid rgba(0, 180, 216, 0.3);
            border-radius: 12px;
            padding: 20px;
            margin-top: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            animation: fadeIn 0.5s ease;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .result-label {
            color: var(--text-secondary);
            font-size: 1rem;
        }
        
        .result-value {
            color: var(--accent-color);
            font-size: 1.8rem;
            font-weight: 700;
        }
        
        .final-result {
            background: linear-gradient(135deg, rgba(46, 204, 113, 0.1), rgba(39, 174, 96, 0.1));
            border: 1px solid rgba(46, 204, 113, 0.3);
            border-radius: 20px;
            padding: 40px;
            text-align: center;
            margin: 40px 0;
            backdrop-filter: blur(10px);
            animation: slideUp 0.6s ease;
            position: relative;
        }
        
        .export-overlay {
            position: absolute;
            top: -10px;
            right: -10px;
            background: var(--card-bg);
            padding: 10px 15px;
            border-radius: 10px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            z-index: 10;
        }
        
        @keyframes slideUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .final-result h2 {
            color: var(--success-color);
            margin-bottom: 25px;
            font-size: 2rem;
            font-weight: 700;
        }
        
        .final-number {
            font-size: 4rem;
            color: var(--success-color);
            font-weight: 800;
            margin: 25px 0;
            text-shadow: 0 4px 20px rgba(46, 204, 113, 0.3);
        }
        
        .metrics-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-top: 40px;
        }
        
        .metric-card {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 12px;
            padding: 20px;
            text-align: center;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .metric-label {
            color: var(--text-secondary);
            font-size: 0.9rem;
            margin-bottom: 10px;
        }
        
        .metric-value {
            color: var(--text-color);
            font-size: 1.8rem;
            font-weight: 700;
        }
        
        .reference-table {
            background: var(--card-bg);
            border-radius: 16px;
            padding: 30px;
            margin-top: 40px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
        }
        
        .table-container {
            overflow-x: auto;
            margin-top: 20px;
            border-radius: 12px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            min-width: 800px;
        }
        
        thead {
            background: linear-gradient(135deg, rgba(0, 180, 216, 0.2), rgba(0, 119, 182, 0.2));
        }
        
        th {
            padding: 18px 15px;
            text-align: center;
            color: var(--text-color);
            font-weight: 600;
            border-bottom: 2px solid rgba(0, 180, 216, 0.3);
        }
        
        td {
            padding: 15px;
            text-align: center;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        tbody tr:hover {
            background: rgba(0, 180, 216, 0.05);
        }
        
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            z-index: 1000;
            justify-content: center;
            align-items: center;
            backdrop-filter: blur(5px);
        }
        
        .modal-content {
            background: var(--card-bg);
            border-radius: 20px;
            padding: 30px;
            max-width: 500px;
            width: 90%;
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
            position: relative;
        }
        
        .close-modal {
            position: absolute;
            top: 15px;
            right: 15px;
            background: none;
            border: none;
            color: var(--text-secondary);
            font-size: 1.5rem;
            cursor: pointer;
            transition: color 0.3s;
        }
        
        .close-modal:hover {
            color: var(--accent-color);
        }
        
        .modal-buttons {
            display: flex;
            gap: 15px;
            margin-top: 25px;
            justify-content: center;
        }
        
        footer {
            text-align: center;
            padding: 40px 0 30px;
            margin-top: 60px;
            border-top: 1px solid rgba(51, 51, 51, 0.5);
            color: var(--text-secondary);
        }
        
        .credits {
            font-size: 1.1rem;
            margin-bottom: 15px;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        
        .engineer-name {
            color: var(--accent-color);
            font-weight: 600;
        }
        
        .signature {
            margin-top: 30px;
            padding-top: 20px;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            font-style: italic;
            color: rgba(255, 255, 255, 0.6);
        }
        
        @media (max-width: 768px) {
            .container {
                padding: 15px;
            }
            
            h1 {
                font-size: 2.2rem;
            }
            
            .input-group {
                grid-template-columns: 1fr;
            }
            
            .step-header {
                flex-direction: column;
                align-items: flex-start;
                gap: 15px;
            }
            
            .metrics-grid {
                grid-template-columns: 1fr;
            }
            
            .btn-container {
                flex-direction: column;
            }
            
            .btn {
                width: 100%;
            }
            
            .quick-actions {
                flex-direction: column;
                align-items: center;
            }
            
            .modal-buttons {
                flex-direction: column;
            }
            
            .export-overlay {
                position: static;
                margin-bottom: 20px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <div class="header-content">
                <div class="logo-container">
                    <i class="fas fa-tower-cell logo-icon"></i>
                    <h1>4G Network Capacity Dimensioning</h1>
                </div>
                <p class="subtitle">Calculate the required number of towers based on population, traffic patterns, and network capacity parameters</p>
                
                <div class="quick-actions">
                    <button class="btn btn-primary" onclick="loadExample()">
                        <i class="fas fa-magic"></i> Load Example Values
                    </button>
                    <button class="btn btn-secondary" onclick="clearAll()">
                        <i class="fas fa-eraser"></i> Clear All Fields
                    </button>
                </div>
            </div>
        </header>
        
        <div class="calculation-steps">
            <!-- Step 1: User Demographics -->
            <div class="step-card">
                <div class="step-header">
                    <div class="step-title-container">
                        <div class="step-icon">
                            <i class="fas fa-users"></i>
                        </div>
                        <h3 class="step-title">User Demographics</h3>
                    </div>
                    <div class="step-number">1</div>
                </div>
                
                <div class="input-group">
                    <div class="input-field">
                        <label for="population"><i class="fas fa-city"></i> Population</label>
                        <div class="input-with-icon">
                            <i class="fas fa-user input-icon"></i>
                            <input type="number" id="population" placeholder="Enter population" min="1">
                        </div>
                    </div>
                    
                    <div class="input-field">
                        <label for="mobilePenetration"><i class="fas fa-mobile-alt"></i> Mobile Penetration (%)</label>
                        <div class="input-with-icon">
                            <i class="fas fa-percentage input-icon"></i>
                            <input type="number" id="mobilePenetration" placeholder="e.g., 125" min="0" step="0.1">
                        </div>
                    </div>
                    
                    <div class="input-field">
                        <label for="marketShare"><i class="fas fa-chart-pie"></i> Market Share (%)</label>
                        <div class="input-with-icon">
                            <i class="fas fa-percentage input-icon"></i>
                            <input type="number" id="marketShare" placeholder="e.g., 35" min="0" max="100" step="0.1">
                        </div>
                    </div>
                    
                    <div class="input-field">
                        <label for="busyHourActivity"><i class="fas fa-clock"></i> Busy Hour Activity (%)</label>
                        <div class="input-with-icon">
                            <i class="fas fa-percentage input-icon"></i>
                            <input type="number" id="busyHourActivity" placeholder="e.g., 50" min="0" max="100" step="0.1">
                        </div>
                    </div>
                </div>
                
                <div class="btn-container">
                    <button class="btn btn-primary" onclick="calculateActiveUsers()">
                        <i class="fas fa-calculator"></i> Calculate Active Users
                    </button>
                </div>
                
                <div class="result-box" id="activeUsersResult" style="display: none;">
                    <div class="result-label">Busy Hour Active Users</div>
                    <div class="result-value" id="activeUsersValue">0</div>
                </div>
            </div>
            
            <!-- Step 2: Traffic Distribution -->
            <div class="step-card">
                <div class="step-header">
                    <div class="step-title-container">
                        <div class="step-icon">
                            <i class="fas fa-chart-line"></i>
                        </div>
                        <h3 class="step-title">Traffic Distribution</h3>
                    </div>
                    <div class="step-number">2</div>
                </div>
                
                <div class="input-group">
                    <div class="input-field">
                        <label for="voicePercentage"><i class="fas fa-phone"></i> Voice Calls (%)</label>
                        <div class="input-with-icon">
                            <i class="fas fa-percentage input-icon"></i>
                            <input type="number" id="voicePercentage" placeholder="e.g., 25" min="0" max="100" step="0.1">
                        </div>
                    </div>
                    
                    <div class="input-field">
                        <label for="voiceThroughput"><i class="fas fa-wave-square"></i> Voice Throughput (Kbits/session)</label>
                        <div class="input-with-icon">
                            <i class="fas fa-bolt input-icon"></i>
                            <input type="number" id="voiceThroughput" placeholder="e.g., 2715" min="0">
                        </div>
                    </div>
                    
                    <div class="input-field">
                        <label for="browsingPercentage"><i class="fas fa-globe"></i> Web Browsing (%)</label>
                        <div class="input-with-icon">
                            <i class="fas fa-percentage input-icon"></i>
                            <input type="number" id="browsingPercentage" placeholder="e.g., 45" min="0" max="100" step="0.1">
                        </div>
                    </div>
                    
                    <div class="input-field">
                        <label for="browsingThroughput"><i class="fas fa-wave-square"></i> Browsing Throughput (Kbits/session)</label>
                        <div class="input-with-icon">
                            <i class="fas fa-bolt input-icon"></i>
                            <input type="number" id="browsingThroughput" placeholder="e.g., 46544" min="0">
                        </div>
                    </div>
                </div>
                
                <div class="input-group">
                    <div class="input-field">
                        <label for="gamingPercentage"><i class="fas fa-gamepad"></i> Gaming (%)</label>
                        <div class="input-with-icon">
                            <i class="fas fa-percentage input-icon"></i>
                            <input type="number" id="gamingPercentage" placeholder="e.g., 10" min="0" max="100" step="0.1">
                        </div>
                    </div>
                    
                    <div class="input-field">
                        <label for="gamingThroughput"><i class="fas fa-wave-square"></i> Gaming Throughput (Kbits/session)</label>
                        <div class="input-with-icon">
                            <i class="fas fa-bolt input-icon"></i>
                            <input type="number" id="gamingThroughput" placeholder="e.g., 93091" min="0">
                        </div>
                    </div>
                    
                    <div class="input-field">
                        <label for="streamingPercentage"><i class="fas fa-video"></i> Streaming (%)</label>
                        <div class="input-with-icon">
                            <i class="fas fa-percentage input-icon"></i>
                            <input type="number" id="streamingPercentage" placeholder="e.g., 20" min="0" max="100" step="0.1">
                        </div>
                    </div>
                    
                    <div class="input-field">
                        <label for="streamingThroughput"><i class="fas fa-wave-square"></i> Streaming Throughput (Kbits/session)</label>
                        <div class="input-with-icon">
                            <i class="fas fa-bolt input-icon"></i>
                            <input type="number" id="streamingThroughput" placeholder="e.g., 518182" min="0">
                        </div>
                    </div>
                </div>
                
                <div class="btn-container">
                    <button class="btn btn-primary" onclick="calculateServiceTraffic()">
                        <i class="fas fa-calculator"></i> Calculate Total Traffic
                    </button>
                </div>
                
                <div class="result-box" id="serviceTrafficResult" style="display: none;">
                    <div class="result-label">Total Network Traffic</div>
                    <div class="result-value" id="totalTrafficValue">0 Mbps</div>
                </div>
            </div>
            
            <!-- Step 3: Network Capacity -->
            <div class="step-card">
                <div class="step-header">
                    <div class="step-title-container">
                        <div class="step-icon">
                            <i class="fas fa-satellite-dish"></i>
                        </div>
                        <h3 class="step-title">Network Capacity</h3>
                    </div>
                    <div class="step-number">3</div>
                </div>
                
                <div class="input-group">
                    <div class="input-field">
                        <label for="siteThroughput"><i class="fas fa-tachometer-alt"></i> Site Throughput (T site) in Mbps</label>
                        <div class="input-with-icon">
                            <i class="fas fa-bolt input-icon"></i>
                            <input type="number" id="siteThroughput" placeholder="e.g., 372.4" min="0.1" step="0.1">
                        </div>
                    </div>
                </div>
                
                <div class="btn-container">
                    <button class="btn btn-primary" onclick="calculateRequiredSites()">
                        <i class="fas fa-calculator"></i> Calculate Required Sites
                    </button>
                </div>
                
                <div class="result-box" id="requiredSitesResult" style="display: none;">
                    <div class="result-label">Required Number of Sites</div>
                    <div class="result-value" id="requiredSitesValue">0</div>
                </div>
            </div>
        </div>
        
        <button class="calculate-all-btn" onclick="calculateAll()">
            <i class="fas fa-rocket"></i> Calculate Complete Dimensioning
        </button>
        
        <!-- Final Result -->
        <div class="final-result" id="finalResult" style="display: none;">
            <div class="export-overlay">
                <button class="btn btn-success" onclick="showExportModal()">
                    <i class="fas fa-camera"></i> Export as Image
                </button>
            </div>
            
            <h2>Network Dimensioning Complete</h2>
            <p style="color: var(--text-secondary); margin-bottom: 30px;">Based on your input parameters, here are the results:</p>
            
            <div class="final-number" id="finalSitesRequired">0</div>
            <p style="font-size: 1.2rem; color: var(--text-color);">4G sites required for optimal coverage</p>
            
            <div class="metrics-grid">
                <div class="metric-card">
                    <div class="metric-label">Active Users (Busy Hour)</div>
                    <div class="metric-value" id="finalActiveUsers">0</div>
                </div>
                <div class="metric-card">
                    <div class="metric-label">Total Network Traffic</div>
                    <div class="metric-value" id="finalTotalTraffic">0 Mbps</div>
                </div>
                <div class="metric-card">
                    <div class="metric-label">Site Throughput</div>
                    <div class="metric-value" id="finalSiteThroughput">0 Mbps</div>
                </div>
                <div class="metric-card">
                    <div class="metric-label">Traffic per Site</div>
                    <div class="metric-value" id="trafficPerSite">0 Mbps</div>
                </div>
            </div>
            
            <div style="margin-top: 40px; padding-top: 30px; border-top: 1px solid rgba(255, 255, 255, 0.1);">
                <h3 style="color: var(--text-color); margin-bottom: 20px; font-size: 1.3rem;">Input Parameters</h3>
                <div class="metrics-grid">
                    <div class="metric-card">
                        <div class="metric-label">Population</div>
                        <div class="metric-value" id="exportPopulation">-</div>
                    </div>
                    <div class="metric-card">
                        <div class="metric-label">Mobile Penetration</div>
                        <div class="metric-value" id="exportMobilePenetration">-</div>
                    </div>
                    <div class="metric-card">
                        <div class="metric-label">Market Share</div>
                        <div class="metric-value" id="exportMarketShare">-</div>
                    </div>
                    <div class="metric-card">
                        <div class="metric-label">Busy Hour Activity</div>
                        <div class="metric-value" id="exportBusyHourActivity">-</div>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Reference Table -->
        <div class="reference-table">
            <div class="step-header">
                <div class="step-title-container">
                    <div class="step-icon">
                        <i class="fas fa-table"></i>
                    </div>
                    <h3 class="step-title">Traffic Modeling Reference</h3>
                </div>
            </div>
            <p style="color: var(--text-secondary); margin-bottom: 20px;">Standard throughput values for different services (per session)</p>
            
            <div class="table-container">
                <table>
                    <thead>
                        <tr>
                            <th>Service Type</th>
                            <th>Mean Bit Rate (Kbps)</th>
                            <th>Session Duration</th>
                            <th>Duty Ratio</th>
                            <th>BLER</th>
                            <th>Throughput per Session (Kbit)</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td><i class="fas fa-phone"></i> VoIP</td>
                            <td>32 UL / 32 DL</td>
                            <td>210 sec</td>
                            <td>0.4</td>
                            <td>1%</td>
                            <td>2,715.15</td>
                        </tr>
                        <tr>
                            <td><i class="fas fa-video"></i> Video Phone</td>
                            <td>64 UL / 64 DL</td>
                            <td>54 sec</td>
                            <td>1.0</td>
                            <td>1%</td>
                            <td>3,490.91</td>
                        </tr>
                        <tr>
                            <td><i class="fas fa-users"></i> Video Conference</td>
                            <td>128 UL / 192 DL</td>
                            <td>1,800 sec</td>
                            <td>1.0</td>
                            <td>1%</td>
                            <td>232,727 / 349,091</td>
                        </tr>
                        <tr>
                            <td><i class="fas fa-gamepad"></i> Interactive Gaming</td>
                            <td>50 UL / 64 DL</td>
                            <td>1,800 sec</td>
                            <td>0.2 / 0.4</td>
                            <td>1%</td>
                            <td>18,182 / 46,546</td>
                        </tr>
                        <tr>
                            <td><i class="fas fa-film"></i> Streaming Media</td>
                            <td>10 UL / 300 DL</td>
                            <td>3,600 sec</td>
                            <td>0.05 / 0.95</td>
                            <td>1%</td>
                            <td>1,818 / 1,036,364</td>
                        </tr>
                        <tr>
                            <td><i class="fas fa-comment"></i> Instant Messaging</td>
                            <td>20 UL / 20 DL</td>
                            <td>7 sec</td>
                            <td>0.2</td>
                            <td>1%</td>
                            <td>28.28</td>
                        </tr>
                        <tr style="background: rgba(0, 180, 216, 0.05);">
                            <td><i class="fas fa-globe"></i> Web Browsing</td>
                            <td>8 UL / 256 DL</td>
                            <td>1,800 sec</td>
                            <td>0.05</td>
                            <td>1%</td>
                            <td>727.27 / 23,272.7</td>
                        </tr>
                        <tr style="background: rgba(46, 204, 113, 0.05);">
                            <td><i class="fas fa-envelope"></i> Email</td>
                            <td>256 UL / 256 DL</td>
                            <td>300 sec</td>
                            <td>1.0</td>
                            <td>1%</td>
                            <td>77,575.8</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
        
        <footer>
            <div class="credits">
                <span>Designed and developed by:</span>
                <div>
                    <span class="engineer-name">Eng. Saeed Ragab Baz Mohammed</span> · 
                    <span class="engineer-name">Eng. Ahmed Karam Ali</span>
                    <span class="engineer-name">Supervised by Dr. Abdul Nasser Fawzi</span>
                </div>
                <span>Supervised by <span class="engineer-name">Dr. Abdel Nasser Fawzy</span></span>
            </div>
            <div class="signature">
                4G Network Capacity Dimensioning Tool · Based on 3GPP Standards
            </div>
        </footer>
    </div>

    <!-- Export Modal -->
    <div class="modal" id="exportModal">
        <div class="modal-content">
            <button class="close-modal" onclick="closeExportModal()">&times;</button>
            <h2 style="color: var(--accent-color); margin-bottom: 20px; text-align: center;">
                <i class="fas fa-camera"></i> Export Results
            </h2>
            <p style="color: var(--text-secondary); text-align: center; margin-bottom: 25px;">
                Choose what to include in the exported image:
            </p>
            
            <div style="margin-bottom: 25px;">
                <label style="display: flex; align-items: center; gap: 10px; margin-bottom: 15px; cursor: pointer;">
                    <input type="checkbox" id="includeResults" checked style="width: 18px; height: 18px;">
                    <span>Final Results</span>
                </label>
                <label style="display: flex; align-items: center; gap: 10px; margin-bottom: 15px; cursor: pointer;">
                    <input type="checkbox" id="includeInputs" checked style="width: 18px; height: 18px;">
                    <span>Input Parameters</span>
                </label>
                <label style="display: flex; align-items: center; gap: 10px; margin-bottom: 15px; cursor: pointer;">
                    <input type="checkbox" id="includeReference" style="width: 18px; height: 18px;">
                    <span>Reference Table</span>
                </label>
            </div>
            
            <div class="modal-buttons">
                <button class="btn btn-success" onclick="exportAsImage()">
                    <i class="fas fa-download"></i> Export Now
                </button>
                <button class="btn btn-secondary" onclick="closeExportModal()">
                    <i class="fas fa-times"></i> Cancel
                </button>
            </div>
        </div>
    </div>

    <script>
        // Global variables
        let activeUsers = 0;
        let totalTraffic = 0;
        let requiredSites = 0;
        
        // Load example values
        function loadExample() {
            // Step 1 values
            document.getElementById('population').value = 180000;
            document.getElementById('mobilePenetration').value = 125;
            document.getElementById('marketShare').value = 35;
            document.getElementById('busyHourActivity').value = 50;
            
            // Step 2 values
            document.getElementById('voicePercentage').value = 25;
            document.getElementById('voiceThroughput').value = 2715;
            document.getElementById('browsingPercentage').value = 45;
            document.getElementById('browsingThroughput').value = 46544;
            document.getElementById('gamingPercentage').value = 10;
            document.getElementById('gamingThroughput').value = 93091;
            document.getElementById('streamingPercentage').value = 20;
            document.getElementById('streamingThroughput').value = 518182;
            
            // Step 3 values
            document.getElementById('siteThroughput').value = 372.4;
            
            showNotification("Example values loaded successfully!", "info");
        }
        
        // Clear all fields
        function clearAll() {
            // Clear all input fields
            const inputs = document.querySelectorAll('input[type="number"]');
            inputs.forEach(input => {
                input.value = '';
            });
            
            // Hide all result boxes
            document.querySelectorAll('.result-box').forEach(box => {
                box.style.display = 'none';
            });
            
            // Hide final result
            document.getElementById('finalResult').style.display = 'none';
            
            // Reset global variables
            activeUsers = 0;
            totalTraffic = 0;
            requiredSites = 0;
            
            showNotification("All fields cleared successfully!", "info");
        }
        
        // Show notification
        function showNotification(message, type) {
            // Create notification element
            const notification = document.createElement('div');
            notification.style.cssText = `
                position: fixed;
                top: 20px;
                right: 20px;
                padding: 15px 25px;
                border-radius: 10px;
                color: white;
                font-weight: 600;
                z-index: 1000;
                animation: slideIn 0.3s ease;
                box-shadow: 0 5px 15px rgba(0,0,0,0.3);
                backdrop-filter: blur(10px);
                border: 1px solid rgba(255,255,255,0.1);
            `;
            
            if (type === "success") {
                notification.style.background = "linear-gradient(135deg, rgba(46, 204, 113, 0.9), rgba(39, 174, 96, 0.9))";
            } else if (type === "info") {
                notification.style.background = "linear-gradient(135deg, rgba(52, 152, 219, 0.9), rgba(41, 128, 185, 0.9))";
            } else {
                notification.style.background = "linear-gradient(135deg, rgba(0, 180, 216, 0.9), rgba(0, 119, 182, 0.9))";
            }
            
            notification.innerHTML = `
                <i class="fas fa-${type === "success" ? "check-circle" : "info-circle"}" style="margin-right: 10px;"></i>
                ${message}
            `;
            
            document.body.appendChild(notification);
            
            // Remove after 3 seconds
            setTimeout(() => {
                notification.style.animation = "slideOut 0.3s ease";
                setTimeout(() => notification.remove(), 300);
            }, 3000);
        }
        
        // Calculate active users
        function calculateActiveUsers() {
            const population = document.getElementById('population').value;
            const mobilePenetration = document.getElementById('mobilePenetration').value;
            const marketShare = document.getElementById('marketShare').value;
            const busyHourActivity = document.getElementById('busyHourActivity').value;
            
            // Validate inputs
            if (!population || !mobilePenetration || !marketShare || !busyHourActivity) {
                showNotification("Please fill all fields in Step 1", "info");
                return;
            }
            
            const populationVal = parseInt(population);
            const mobilePenetrationVal = parseFloat(mobilePenetration) / 100;
            const marketShareVal = parseFloat(marketShare) / 100;
            const busyHourActivityVal = parseFloat(busyHourActivity) / 100;
            
            activeUsers = populationVal * (1 + mobilePenetrationVal) * marketShareVal * busyHourActivityVal;
            
            document.getElementById('activeUsersValue').textContent = activeUsers.toLocaleString('en-US', { maximumFractionDigits: 0 });
            document.getElementById('activeUsersResult').style.display = 'flex';
            
            showNotification(`Calculated ${activeUsers.toLocaleString()} active users`, "success");
            return activeUsers;
        }
        
        // Calculate service traffic
        function calculateServiceTraffic() {
            // Get active users first
            if (activeUsers === 0) {
                activeUsers = calculateActiveUsers();
                if (activeUsers === 0) return;
            }
            
            // Get service percentages
            const voicePercentage = document.getElementById('voicePercentage').value;
            const voiceThroughput = document.getElementById('voiceThroughput').value;
            const browsingPercentage = document.getElementById('browsingPercentage').value;
            const browsingThroughput = document.getElementById('browsingThroughput').value;
            const gamingPercentage = document.getElementById('gamingPercentage').value;
            const gamingThroughput = document.getElementById('gamingThroughput').value;
            const streamingPercentage = document.getElementById('streamingPercentage').value;
            const streamingThroughput = document.getElementById('streamingThroughput').value;
            
            // Validate inputs
            if (!voicePercentage || !voiceThroughput || !browsingPercentage || !browsingThroughput || 
                !gamingPercentage || !gamingThroughput || !streamingPercentage || !streamingThroughput) {
                showNotification("Please fill all traffic distribution fields", "info");
                return;
            }
            
            const voicePercentageVal = parseFloat(voicePercentage) / 100;
            const voiceThroughputVal = parseFloat(voiceThroughput);
            const browsingPercentageVal = parseFloat(browsingPercentage) / 100;
            const browsingThroughputVal = parseFloat(browsingThroughput);
            const gamingPercentageVal = parseFloat(gamingPercentage) / 100;
            const gamingThroughputVal = parseFloat(gamingThroughput);
            const streamingPercentageVal = parseFloat(streamingPercentage) / 100;
            const streamingThroughputVal = parseFloat(streamingThroughput);
            
            // Calculate total traffic in Kbps, then convert to Mbps
            const voiceTraffic = voicePercentageVal * activeUsers * voiceThroughputVal;
            const browsingTraffic = browsingPercentageVal * activeUsers * browsingThroughputVal;
            const gamingTraffic = gamingPercentageVal * activeUsers * gamingThroughputVal;
            const streamingTraffic = streamingPercentageVal * activeUsers * streamingThroughputVal;
            
            // Sum all traffic and convert from Kbit to Mbit (divide by 1000)
            // Then divide by 3600 seconds to get Mbps
            totalTraffic = (voiceTraffic + browsingTraffic + gamingTraffic + streamingTraffic) / 3600000;
            
            document.getElementById('totalTrafficValue').textContent = totalTraffic.toFixed(2) + ' Mbps';
            document.getElementById('serviceTrafficResult').style.display = 'flex';
            
            showNotification(`Total traffic: ${totalTraffic.toFixed(2)} Mbps`, "success");
            return totalTraffic;
        }
        
        // Calculate required sites
        function calculateRequiredSites() {
            // Get total traffic first
            if (totalTraffic === 0) {
                totalTraffic = calculateServiceTraffic();
                if (totalTraffic === 0) return;
            }
            
            const siteThroughput = document.getElementById('siteThroughput').value;
            
            if (!siteThroughput) {
                showNotification("Please enter site throughput value", "info");
                return;
            }
            
            const siteThroughputVal = parseFloat(siteThroughput);
            
            if (siteThroughputVal === 0) {
                showNotification("Site throughput cannot be zero", "info");
                return;
            }
            
            requiredSites = totalTraffic / siteThroughputVal;
            
            document.getElementById('requiredSitesValue').textContent = requiredSites.toFixed(2);
            document.getElementById('requiredSitesResult').style.display = 'flex';
            
            showNotification(`${requiredSites.toFixed(2)} sites required`, "success");
            return requiredSites;
        }
        
        // Calculate all steps and show final result
        function calculateAll() {
            // Calculate all steps
            activeUsers = calculateActiveUsers();
            if (activeUsers === 0) return;
            
            totalTraffic = calculateServiceTraffic();
            if (totalTraffic === 0) return;
            
            requiredSites = calculateRequiredSites();
            if (requiredSites === 0) return;
            
            const siteThroughput = parseFloat(document.getElementById('siteThroughput').value);
            const trafficPerSite = totalTraffic / Math.ceil(requiredSites);
            
            // Update final result display
            document.getElementById('finalActiveUsers').textContent = activeUsers.toLocaleString('en-US', { maximumFractionDigits: 0 });
            document.getElementById('finalTotalTraffic').textContent = totalTraffic.toFixed(2) + ' Mbps';
            document.getElementById('finalSiteThroughput').textContent = siteThroughput.toFixed(1) + ' Mbps';
            document.getElementById('trafficPerSite').textContent = trafficPerSite.toFixed(2) + ' Mbps';
            document.getElementById('finalSitesRequired').textContent = Math.ceil(requiredSites);
            
            // Update export parameters
            document.getElementById('exportPopulation').textContent = document.getElementById('population').value;
            document.getElementById('exportMobilePenetration').textContent = document.getElementById('mobilePenetration').value + '%';
            document.getElementById('exportMarketShare').textContent = document.getElementById('marketShare').value + '%';
            document.getElementById('exportBusyHourActivity').textContent = document.getElementById('busyHourActivity').value + '%';
            
            // Show final result with animation
            document.getElementById('finalResult').style.display = 'block';
            
            // Scroll to final result
            document.getElementById('finalResult').scrollIntoView({ behavior: 'smooth', block: 'start' });
            
            showNotification(`Dimensioning complete! ${Math.ceil(requiredSites)} sites required`, "success");
        }
        
        // Show export modal
        function showExportModal() {
            document.getElementById('exportModal').style.display = 'flex';
        }
        
        // Close export modal
        function closeExportModal() {
            document.getElementById('exportModal').style.display = 'none';
        }
        
        // Export as image
        function exportAsImage() {
            const includeResults = document.getElementById('includeResults').checked;
            const includeInputs = document.getElementById('includeInputs').checked;
            const includeReference = document.getElementById('includeReference').checked;
            
            // Create export container
            const exportContainer = document.createElement('div');
            exportContainer.style.cssText = `
                position: fixed;
                top: -10000px;
                left: -10000px;
                width: 800px;
                background: linear-gradient(135deg, #0a0a0a 0%, #1a1a2e 100%);
                color: white;
                padding: 40px;
                border-radius: 20px;
                z-index: 10000;
            `;
            
            // Add header
            exportContainer.innerHTML = `
                <div style="text-align: center; margin-bottom: 30px; border-bottom: 2px solid #00b4d8; padding-bottom: 20px;">
                    <h1 style="color: #00b4d8; font-size: 32px; margin-bottom: 10px;">4G Network Capacity Dimensioning</h1>
                    <p style="color: #adb5bd;">Results exported on ${new Date().toLocaleDateString()} at ${new Date().toLocaleTimeString()}</p>
                    <p style="color: #adb5bd; margin-top: 5px;">Designed by Eng. Saeed Ragab Baz Mohammed & Eng. Ahmed Karam Ali</p>
                    <p style="color: #adb5bd;">Supervised by Dr. Abdel Nasser Fawzy</p>
                </div>
            `;
            
            // Add results if selected
            if (includeResults) {
                exportContainer.innerHTML += `
                    <div style="margin-bottom: 30px;">
                        <h2 style="color: #2ecc71; text-align: center; margin-bottom: 20px; font-size: 28px;">Dimensioning Results</h2>
                        <div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 20px; margin-bottom: 20px;">
                            <div style="background: rgba(0, 0, 0, 0.3); padding: 20px; border-radius: 10px; text-align: center;">
                                <div style="color: #adb5bd; font-size: 14px; margin-bottom: 5px;">Required 4G Sites</div>
                                <div style="color: #2ecc71; font-size: 36px; font-weight: bold;">${Math.ceil(requiredSites)}</div>
                            </div>
                            <div style="background: rgba(0, 0, 0, 0.3); padding: 20px; border-radius: 10px; text-align: center;">
                                <div style="color: #adb5bd; font-size: 14px; margin-bottom: 5px;">Active Users</div>
                                <div style="color: #00b4d8; font-size: 36px; font-weight: bold;">${activeUsers.toLocaleString()}</div>
                            </div>
                            <div style="background: rgba(0, 0, 0, 0.3); padding: 20px; border-radius: 10px; text-align: center;">
                                <div style="color: #adb5bd; font-size: 14px; margin-bottom: 5px;">Total Traffic</div>
                                <div style="color: #00b4d8; font-size: 36px; font-weight: bold;">${totalTraffic.toFixed(2)} Mbps</div>
                            </div>
                            <div style="background: rgba(0, 0, 0, 0.3); padding: 20px; border-radius: 10px; text-align: center;">
                                <div style="color: #adb5bd; font-size: 14px; margin-bottom: 5px;">Site Throughput</div>
                                <div style="color: #00b4d8; font-size: 36px; font-weight: bold;">${document.getElementById('siteThroughput').value} Mbps</div>
                            </div>
                        </div>
                    </div>
                `;
            }
            
            // Add input parameters if selected
            if (includeInputs) {
                exportContainer.innerHTML += `
                    <div style="margin-bottom: 30px;">
                        <h2 style="color: #00b4d8; text-align: center; margin-bottom: 20px; font-size: 24px;">Input Parameters</h2>
                        <div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px;">
                            <div style="background: rgba(0, 180, 216, 0.1); padding: 15px; border-radius: 8px; border: 1px solid rgba(0, 180, 216, 0.3);">
                                <div style="color: #adb5bd; font-size: 13px;">Population</div>
                                <div style="color: white; font-size: 20px; font-weight: bold;">${document.getElementById('population').value}</div>
                            </div>
                            <div style="background: rgba(0, 180, 216, 0.1); padding: 15px; border-radius: 8px; border: 1px solid rgba(0, 180, 216, 0.3);">
                                <div style="color: #adb5bd; font-size: 13px;">Mobile Penetration</div>
                                <div style="color: white; font-size: 20px; font-weight: bold;">${document.getElementById('mobilePenetration').value}%</div>
                            </div>
                            <div style="background: rgba(0, 180, 216, 0.1); padding: 15px; border-radius: 8px; border: 1px solid rgba(0, 180, 216, 0.3);">
                                <div style="color: #adb5bd; font-size: 13px;">Market Share</div>
                                <div style="color: white; font-size: 20px; font-weight: bold;">${document.getElementById('marketShare').value}%</div>
                            </div>
                            <div style="background: rgba(0, 180, 216, 0.1); padding: 15px; border-radius: 8px; border: 1px solid rgba(0, 180, 216, 0.3);">
                                <div style="color: #adb5bd; font-size: 13px;">Busy Hour Activity</div>
                                <div style="color: white; font-size: 20px; font-weight: bold;">${document.getElementById('busyHourActivity').value}%</div>
                            </div>
                        </div>
                    </div>
                `;
            }
            
            // Add reference table if selected
            if (includeReference) {
                exportContainer.innerHTML += `
                    <div>
                        <h2 style="color: #00b4d8; text-align: center; margin-bottom: 15px; font-size: 24px;">Traffic Modeling Reference</h2>
                        <p style="color: #adb5bd; text-align: center; margin-bottom: 15px; font-size: 14px;">Standard throughput values per session</p>
                        <div style="overflow-x: auto;">
                            ${document.querySelector('.reference-table .table-container').outerHTML}
                        </div>
                    </div>
                `;
            }
            
            // Add footer
            exportContainer.innerHTML += `
                <div style="text-align: center; margin-top: 40px; padding-top: 20px; border-top: 1px solid rgba(255, 255, 255, 0.1); color: #adb5bd; font-size: 12px;">
                    <p>4G Network Capacity Dimensioning Tool · Based on 3GPP Standards</p>
                    <p>Generated automatically by 4G Network Dimensioning Calculator</p>
                </div>
            `;
            
            // Add to document
            document.body.appendChild(exportContainer);
            
            // Show loading notification
            showNotification("Generating image... Please wait", "info");
            
            // Use html2canvas to create image
            html2canvas(exportContainer, {
                scale: 2,
                useCORS: true,
                backgroundColor: null,
                logging: false,
                onclone: function(clonedDoc) {
                    // Ensure styles are applied in cloned document
                    const clonedContainer = clonedDoc.querySelector('[style*="top: -10000px"]');
                    clonedContainer.style.width = '800px';
                    clonedContainer.style.padding = '40px';
                }
            }).then(canvas => {
                // Create download link
                const link = document.createElement('a');
                link.download = `4G_Dimensioning_Results_${new Date().toISOString().slice(0,10)}.png`;
                link.href = canvas.toDataURL('image/png', 1.0);
                link.click();
                
                // Remove container
                document.body.removeChild(exportContainer);
                
                // Close modal
                closeExportModal();
                
                // Show success notification
                showNotification("Image exported successfully!", "success");
            }).catch(error => {
                console.error('Error exporting image:', error);
                showNotification("Error exporting image. Please try again.", "info");
                document.body.removeChild(exportContainer);
            });
        }
        
        // Add CSS for notifications
        const style = document.createElement('style');
        style.textContent = `
            @keyframes slideIn {
                from { transform: translateX(100%); opacity: 0; }
                to { transform: translateX(0); opacity: 1; }
            }
            @keyframes slideOut {
                from { transform: translateX(0); opacity: 1; }
                to { transform: translateX(100%); opacity: 0; }
            }
        `;
        document.head.appendChild(style);
        
        // Initialize on page load
        window.onload = function() {
            // Show welcome message
            setTimeout(() => {
                showNotification("Welcome! Enter your values or click 'Load Example Values' to start", "info");
            }, 1000);
        };
    </script>
</body>
</html>
