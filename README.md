<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Stochastic Process Calculator</title>
<style>
    body {
        font-family: 'Segoe UI', sans-serif;
        background: linear-gradient(135deg, #0f172a, #020617);
        color: #e2e8f0;
        margin: 0;
    }

    header {
        text-align: center;
        padding: 20px;
        font-size: 28px;
        font-weight: bold;
        background: #1e293b;
        color: #38bdf8;
    }

    .container {
        width: 90%;
        max-width: 900px;
        margin: auto;
        margin-top: 20px;
    }

    .card {
        background: rgba(30,41,59,0.8);
        padding: 20px;
        margin-bottom: 20px;
        border-radius: 15px;
        backdrop-filter: blur(10px);
        box-shadow: 0 0 15px rgba(0,0,0,0.6);
    }

    h2 {
        color: #38bdf8;
    }

    input {
        width: 100%;
        padding: 10px;
        margin-top: 10px;
        border-radius: 8px;
        border: none;
        outline: none;
    }

    button {
        margin-top: 12px;
        padding: 10px 15px;
        border: none;
        border-radius: 8px;
        background: #38bdf8;
        color: black;
        font-weight: bold;
        cursor: pointer;
    }

    button:hover {
        background: #0ea5e9;
    }

    .output {
        margin-top: 12px;
        padding: 12px;
        background: #020617;
        border-radius: 8px;
        color: #22c55e;
        white-space: pre-wrap;
    }
</style>
</head>

<body>

<header>📊 Stochastic Calculator (Markov + AutoCorr + PSD)</header>

<div class="container">

    <!-- MARKOV -->
    <div class="card">
        <h2>🔁 Markov Chain</h2>
        <input id="matrix" placeholder="Matrix: 0.7,0.3;0.4,0.6">
        <input id="state" placeholder="State: 1,0">
        <button onclick="computeMarkov()">Compute</button>
        <div class="output" id="markovResult"></div>
    </div>

    <!-- AUTO CORRELATION -->
    <div class="card">
        <h2>📈 Auto-Correlation</h2>
        <input id="autoInput" placeholder="Sequence: 1,2,3,4">
        <button onclick="autoCorrelation()">Calculate</button>
        <div class="output" id="autoResult"></div>
    </div>

    <!-- PSD -->
    <div class="card">
        <h2>⚡ Power Spectral Density (PSD)</h2>
        <input id="psdInput" placeholder="Sequence: 1,2,3,4">
        <button onclick="computePSD()">Compute PSD</button>
        <div class="output" id="psdResult"></div>
    </div>

</div>

<script>

// ---------------- MARKOV ----------------
function computeMarkov() {
    let matrix = document.getElementById("matrix").value
        .split(";")
        .map(r => r.split(",").map(Number));

    let state = document.getElementById("state").value
        .split(",")
        .map(Number);

    let result = [];

    for (let i = 0; i < matrix.length; i++) {
        let sum = 0;
        for (let j = 0; j < state.length; j++) {
            sum += matrix[i][j] * state[j];
        }
        result.push(sum.toFixed(3));
    }

    document.getElementById("markovResult").innerText =
        "Next State:\n[" + result.join(", ") + "]";
}

// ---------------- AUTO CORRELATION ----------------
function autoCorrelation() {
    let x = document.getElementById("autoInput").value
        .split(",").map(Number);

    let n = x.length;
    let result = [];

    for (let lag = 0; lag < n; lag++) {
        let sum = 0;
        for (let i = 0; i < n - lag; i++) {
            sum += x[i] * x[i + lag];
        }
        result.push(sum.toFixed(3));
    }

    document.getElementById("autoResult").innerText =
        "R_xx(k):\n[" + result.join(", ") + "]";
}

// ---------------- PSD USING DFT ----------------
function computePSD() {
    let x = document.getElementById("psdInput").value
        .split(",").map(Number);

    let N = x.length;
    let psd = [];

    for (let k = 0; k < N; k++) {
        let real = 0;
        let imag = 0;

        for (let n = 0; n < N; n++) {
            let angle = (-2 * Math.PI * k * n) / N;
            real += x[n] * Math.cos(angle);
            imag += x[n] * Math.sin(angle);
        }

        let power = real*real + imag*imag;
        psd.push(power.toFixed(3));
    }

    document.getElementById("psdResult").innerText =
        "PSD:\n[" + psd.join(", ") + "]";
}

</script>

</body>
</html>
