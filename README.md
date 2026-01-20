<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ultimate EMI Calculator</title>
    
    <!-- Google Fonts & Icons -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.2.1/css/all.min.css">

    <!-- Firebase SDKs -->
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>

    <style>
        :root {
            --primary-color: #4A90E2; /* A modern, calm blue */
            --gradient-start: #f4f7fa;
            --gradient-end: #e2e8f0;
            --text-dark: #333;
            --text-light: #666;
            --card-bg: #ffffff;
            --shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
            --border-color: #d1d9e6;
        }

        body {
            font-family: 'Poppins', sans-serif;
            background-image: linear-gradient(135deg, var(--gradient-start) 0%, var(--gradient-end) 100%);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 2rem 1rem;
            box-sizing: border-box;
        }

        .calculator-container {
            background: var(--card-bg);
            padding: 2.5rem;
            border-radius: 20px;
            box-shadow: var(--shadow);
            width: 100%;
            max-width: 500px;
        }

        .header {
            text-align: center;
            margin-bottom: 2.5rem;
        }
        .header h1 {
            color: var(--text-dark);
            font-weight: 700;
            margin: 0;
        }
        .header p {
            color: var(--text-light);
            font-size: 0.9rem;
        }

        .input-group {
            position: relative;
            margin-bottom: 2rem;
        }
        
        .input-field {
            width: 100%;
            padding: 1rem 0.5rem;
            border: none;
            border-bottom: 2px solid var(--border-color);
            background: transparent;
            font-size: 1.1rem;
            color: var(--text-dark);
            font-family: 'Poppins', sans-serif;
            transition: border-color 0.3s;
            box-sizing: border-box;
        }

        .input-field:focus {
            outline: none;
            border-color: var(--primary-color);
        }

        .input-label {
            position: absolute;
            left: 0.5rem;
            top: 1rem;
            color: var(--text-light);
            pointer-events: none;
            transition: all 0.3s ease;
        }
        
        .input-field:focus ~ .input-label,
        .input-field:not(:placeholder-shown) ~ .input-label {
            top: -0.5rem;
            left: 0;
            font-size: 0.8rem;
            color: var(--primary-color);
        }

        .calc-button {
            width: 100%;
            padding: 1rem;
            border: none;
            border-radius: 12px;
            background-color: var(--primary-color);
            color: white;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
        }
        
        .calc-button:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(74, 144, 226, 0.3);
        }

        .result-container {
            margin-top: 2rem;
            padding: 1.5rem;
            background-image: linear-gradient(135deg, #4A90E2 0%, #357ABD 100%);
            border-radius: 15px;
            color: white;
            text-align: center;
            display: none;
            animation: fadeIn 0.5s ease-in-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .result-container p {
            margin: 0;
            font-size: 1rem;
            opacity: 0.9;
        }
        .result-container h2 {
            margin: 0.5rem 0 0;
            font-size: 2.5rem;
            font-weight: 700;
            letter-spacing: 1px;
        }
        
        .divider {
            margin: 2.5rem 0;
            border: 0;
            border-top: 1px solid var(--border-color);
        }
        
        h3 {
            text-align: center;
            color: var(--text-dark);
            margin-bottom: 1.5rem;
        }

        #history-list {
            list-style: none;
            padding: 0;
            max-height: 250px;
            overflow-y: auto;
        }
        #history-list li {
            background-color: #f8f9fa;
            padding: 1rem 1.5rem;
            border-radius: 10px;
            margin-bottom: 0.75rem;
            font-size: 0.9rem;
            display: flex;
            flex-direction: column;
            border: 1px solid #e9ecef;
        }
        #history-list li .emi-value {
            font-size: 1.2rem;
            font-weight: 600;
            color: var(--primary-color);
        }
        #history-list li .details {
            font-size: 0.8rem;
            color: var(--text-light);
        }

        .ad-container {
            margin-top: 2rem;
            display: flex;
            justify-content: center;
        }
    </style>
</head>
<body>
    <div class="calculator-container">
        <div class="header">
            <h1>EMI Calculator</h1>
            <p>सरलता से अपनी मासिक किस्त की गणना करें।</p>
        </div>
        
        <form id="emi-form">
            <div class="input-group">
                <input type="number" id="loan-amount" class="input-field" placeholder=" " required>
                <label for="loan-amount" class="input-label">Loan Amount (मूलधन राशि)</label>
            </div>
            <div class="input-group">
                <input type="number" id="interest-rate" step="0.01" class="input-field" placeholder=" " required>
                <label for="interest-rate" class="input-label">Annual Interest Rate (%)</label>
            </div>
            <div class="input-group">
                <input type="number" id="loan-tenure" class="input-field" placeholder=" " required>
                <label for="loan-tenure" class="input-label">Loan Tenure (in Years)</label>
            </div>
            <button type="submit" class="calc-button">
                <i class="fas fa-calculator"></i> Calculate EMI
            </button>
        </form>
        
        <div id="result" class="result-container">
            <p>Your Monthly EMI</p>
            <h2 id="emi-output"></h2>
        </div>
        
        <hr class="divider">

        <h3><i class="fas fa-history"></i> Calculation History</h3>
        <ul id="history-list">
            <li>No history yet. Perform a calculation.</li>
        </ul>

        <!-- Your Ad Goes Here -->
        <div class="ad-container">
            <script>
                atOptions = {
                    'key' : 'ac7411ad1c4de0e2a8b62b4fac4711d7',
                    'format' : 'iframe',
                    'height' : 60,
                    'width' : 468,
                    'params' : {}
                };
            </script>
            <script src="https://www.highperformanceformat.com/ac7411ad1c4de0e2a8b62b4fac4711d7/invoke.js"></script>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // Your Firebase Configuration
            const firebaseConfig = {
                apiKey: "AIzaSyAAJ6Rj7CrVy3jdfopOjGEUcnTPczjyJT8",
                authDomain: "gen-lang-client-0088239422.firebaseapp.com",
                databaseURL: "https://gen-lang-client-0088239422-default-rtdb.firebaseio.com",
                projectId: "gen-lang-client-0088239422",
                storageBucket: "gen-lang-client-0088239422.firebasestorage.app",
                messagingSenderId: "351972056292",
                appId: "1:351972056292:web:8a06a1718454e8a6bee0f1",
                measurementId: "G-T4FSMTPZKJ"
            };
            
            // Initialize Firebase
            firebase.initializeApp(firebaseConfig);
            const database = firebase.database();

            // DOM Elements
            const emiForm = document.getElementById('emi-form');
            const resultDiv = document.getElementById('result');
            const emiOutput = document.getElementById('emi-output');
            const historyList = document.getElementById('history-list');

            // Save calculation to Firebase
            const saveCalculation = (data) => {
                database.ref('emi-calculations').push(data);
            };

            // Fetch and display history from Firebase
            const fetchHistory = () => {
                const calculationsRef = database.ref('emi-calculations').limitToLast(5);
                calculationsRef.on('value', (snapshot) => {
                    historyList.innerHTML = '';
                    if (snapshot.exists()) {
                        const data = snapshot.val();
                        const historyItems = Object.values(data).reverse(); // Show latest first
                        historyItems.forEach(item => {
                            const li = document.createElement('li');
                            li.innerHTML = `
                                <span class="emi-value">₹ ${item.emi.toLocaleString('en-IN', { minimumFractionDigits: 2, maximumFractionDigits: 2 })}</span>
                                <span class="details">Loan: ₹${item.principal.toLocaleString('en-IN')} | Rate: ${item.rate}% | Tenure: ${item.tenure} Yrs</span>
                            `;
                            historyList.appendChild(li);
                        });
                    } else {
                        historyList.innerHTML = '<li>No history yet. Perform a calculation.</li>';
                    }
                });
            };
            
            // Calculate EMI
            const calculateEMI = (e) => {
                e.preventDefault();
                const principal = parseFloat(document.getElementById('loan-amount').value);
                const annualInterestRate = parseFloat(document.getElementById('interest-rate').value);
                const tenureInYears = parseFloat(document.getElementById('loan-tenure').value);

                if ([principal, annualInterestRate, tenureInYears].some(v => isNaN(v) || v <= 0)) {
                    alert("Please enter valid positive numbers in all fields.");
                    return;
                }

                const monthlyInterestRate = annualInterestRate / 12 / 100;
                const tenureInMonths = tenureInYears * 12;
                const emi = principal * monthlyInterestRate * Math.pow(1 + monthlyInterestRate, tenureInMonths) / (Math.pow(1 + monthlyInterestRate, tenureInMonths) - 1);
                
                // Display result with animation
                resultDiv.style.display = 'block';
                emiOutput.textContent = `₹ ${emi.toLocaleString('en-IN', { minimumFractionDigits: 2, maximumFractionDigits: 2 })}`;
                
                // Save to Firebase
                saveCalculation({
                    principal: principal,
                    rate: annualInterestRate,
                    tenure: tenureInYears,
                    emi: emi,
                    timestamp: new Date().toISOString()
                });
            };
            
            // Event Listeners
            emiForm.addEventListener('submit', calculateEMI);

            // Initial call to fetch history
            fetchHistory();
        });
    </script>
</body>
</html>