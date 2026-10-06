<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<meta name="theme-color" content="#07131f">
<title>Solar Home Calculator — S A Chamika Gayashan</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Inter,system-ui,-apple-system,Segoe UI,sans-serif;color:#eef8ff;background:#06111c;min-height:100vh;overflow-x:hidden}
body:before,body:after{content:"";position:fixed;width:280px;height:280px;border-radius:50%;filter:blur(70px);opacity:.28;z-index:-1;animation:float 9s ease-in-out infinite}
body:before{background:#00d9a6;top:-90px;left:-100px}
body:after{background:#1684ff;right:-110px;bottom:-80px;animation-delay:-4s}
@keyframes float{50%{transform:translate(30px,25px) scale(1.12)}}
@keyframes up{from{opacity:0;transform:translateY(16px)}to{opacity:1;transform:none}}
.wrap{width:min(760px,100%);margin:auto;padding:18px 14px 32px}
.glass{background:linear-gradient(135deg,#ffffff13,#ffffff08);border:1px solid #ffffff1c;box-shadow:0 18px 50px #0005,inset 0 1px #ffffff12;backdrop-filter:blur(18px);-webkit-backdrop-filter:blur(18px);border-radius:24px}
.hero{padding:25px 20px;margin-bottom:14px;animation:up .55s ease}
.badge{display:inline-flex;padding:7px 11px;border-radius:99px;background:#00d9a61c;border:1px solid #00d9a64a;color:#72ffd9;font-size:12px;font-weight:700}
h1{font-size:clamp(27px,8vw,42px);line-height:1.03;margin:13px 0 8px;letter-spacing:-1.5px}
.hero p{margin:0;color:#a8bac7;line-height:1.5}
.solar{font-size:46px;float:right;filter:drop-shadow(0 0 18px #ffe06b)}
.section{padding:17px;margin-bottom:14px;animation:up .6s ease}
.section h2{font-size:17px;margin:0 0 12px;display:flex;justify-content:space-between;align-items:center}
.grid{display:grid;grid-template-columns:1fr 1fr;gap:10px}
label{display:block;color:#9fb2bf;font-size:12px;font-weight:600}
input, select{margin-top:6px;width:100%;padding:12px 11px;border-radius:13px;border:1px solid #ffffff1c;background:#071521;color:white;outline:none;font-size:15px;font-family:inherit;}
input:focus, select:focus{border-color:#00d9a6;box-shadow:0 0 0 3px #00d9a615}
select { cursor: pointer; appearance: auto; }
.appliance{display:grid;grid-template-columns:1.5fr .7fr .55fr .7fr 40px;gap:7px;align-items:end;padding:10px 0;border-bottom:1px solid #ffffff0d}
.appliance:last-child{border:0}
.name-col { display:flex; flex-direction:column; gap:5px; }
.small{font-size:10px;color:#718594}
.btn{border:0;border-radius:13px;padding:12px 14px;font-weight:800;cursor:pointer;color:#041016;transition:.2s}
.btn:hover{transform:translateY(-2px)}
.add{background:#65f6cf;margin-top:12px;width:100%}
.remove{background:#ff61701a;color:#ff8994;border:1px solid #ff61702b;height:43px;display:flex;align-items:center;justify-content:center;font-size:18px;padding:0;}
.save-btn{background:linear-gradient(135deg, #00d9a6, #1684ff);color:white;width:100%;margin-top:14px;font-size:15px;box-shadow:0 4px 15px #00000044;}
.clear-btn{background:#ff61701a;color:#ff8994;border:1px solid #ff61702b;padding:5px 10px;font-size:11px;border-radius:8px;}
.results{display:grid;grid-template-columns:1fr 1fr;gap:9px}
.metric{padding:14px;border-radius:17px;background:#ffffff09;border:1px solid #ffffff12}
.metric span{display:block;color:#91a7b5;font-size:11px}
.metric b{display:block;margin-top:5px;font-size:23px}
.recommend{margin-top:11px;padding:16px;border-radius:18px;background:linear-gradient(135deg,#00d9a61b,#1684ff15);border:1px solid #00d9a638}
.recommend strong{color:#74ffda}
.history-item{padding:14px;border-radius:15px;background:#ffffff09;border:1px solid #ffffff12;margin-bottom:10px;display:flex;justify-content:space-between;align-items:center;}
.history-item span{color:#91a7b5;font-size:11px;display:block;margin-bottom:4px;}
.history-item b{color:#65f6cf;font-size:14px;}
.history-item .hist-right{text-align:right;}
.history-item .hist-right b{color:white;font-size:18px;}
.footer{text-align:center;color:#8296a3;font-size:12px;padding:8px}
.footer b{color:#d7e9f3}
@media(max-width:560px){
  .grid,.results{grid-template-columns:1fr 1fr}
  .appliance{grid-template-columns:1fr 1fr 1fr}
  .appliance .name-col{grid-column:1/-1}
  .appliance .remove{grid-column:3}
  .solar{font-size:38px}
  .hero{padding:22px 17px}
}
</style>
</head>
<body>
<div class="wrap">
<header class="glass hero">
  <div class="solar">☀️</div>
  <span class="badge">OFF-GRID SOLAR CALCULATOR</span>
  <h1>Power your home.<br>Know your solar size.</h1>
  <p>Add your home appliances and daily usage hours. Get an instant estimated solar, battery and inverter requirement.</p>
</header>

<section class="glass section">
<h2>🏠 Home Appliances</h2>
<div id="list"></div>
<button class="btn add" onclick="addAppliance()">＋ Add appliance</button>
</section>

<section class="glass section">
<h2>☀️ Calculation Settings</h2>
<div class="grid">
<label>Sun hours / day<input id="sun" type="number" value="4.5" step=".1" oninput="calculate()"></label>
<label>System efficiency %<input id="eff" type="number" value="80" oninput="calculate()"></label>
</div>
</section>

<section class="glass section">
<h2>📊 Estimated Requirement</h2>
<div class="results" id="out"></div>
<div class="recommend" id="recommend"></div>
<button class="btn save-btn" onclick="saveToHistory()">💾 Save This Calculation</button>
</section>

<section class="glass section">
<h2>
  <span>📜 Saved History</span>
  <button class="btn clear-btn" onclick="clearHistory()">Clear All</button>
</h2>
<div id="history-list"></div>
</section>

<div class="footer">Designed for <b>S A Chamika Gayashan</b> · 0740222175</div>
</div>

<script>
// Appliance database with default watts
const applianceData = {
  "Refrigerator": 150, 
  "TV": 100, 
  "Fan": 60, 
  "LED Light": 10,
  "Air Conditioner": 1200, 
  "Washing Machine": 500, 
  "Water Pump": 750,
  "Iron": 1000, 
  "Laptop": 65, 
  "Rice Cooker": 400, 
  "Custom": ""
};

// Default appliances when the page loads
const presets = [
  { name: "Refrigerator", w: 150, q: 1, h: 12 },
  { name: "TV", w: 100, q: 1, h: 5 },
  { name: "Fan", w: 60, q: 2, h: 8 },
  { name: "LED Light", w: 10, q: 6, h: 6 }
];

// Automatically update watts when a new appliance is selected
function updateWatt(selectElem) {
  let parent = selectElem.parentElement.parentElement;
  let customInput = parent.querySelector(".custom-name");
  let wattInput = parent.querySelector(".w");
  
  if (selectElem.value === "Custom") {
    customInput.style.display = "block";
    wattInput.value = ""; // Clear watts for custom entry
  } else {
    customInput.style.display = "none";
    wattInput.value = applianceData[selectElem.value]; // Auto-fill watts
  }
  calculate();
}

// Add a new appliance row
function addAppliance(v = { name: "Custom", w: 100, q: 1, h: 1 }) {
  let d = document.createElement("div");
  d.className = "appliance";
  
  let isCustom = !applianceData.hasOwnProperty(v.name) || v.name === "Custom";
  
  // Create dropdown options
  let optionsHTML = Object.keys(applianceData).map(k => 
    `<option value="${k}" ${(v.name === k || (isCustom && k === 'Custom')) ? 'selected' : ''}>${k}</option>`
  ).join('');

  d.innerHTML = `
    <div class="name-col">
      <label>Appliance</label>
      <select class="n-select" onchange="updateWatt(this)">${optionsHTML}</select>
      <input class="custom-name" placeholder="Type custom name..." value="${isCustom && v.name !== 'Custom' ? v.name : ''}" style="display:${isCustom ? 'block' : 'none'}" oninput="calculate()">
    </div>
    <label>Watts<input class="w" type="number" min="0" value="${v.w}" oninput="calculate()"></label>
    <label>Qty<input class="q" type="number" min="1" value="${v.q}" oninput="calculate()"></label>
    <label>Hours/day<input class="h" type="number" min="0" max="24" step=".1" value="${v.h}" oninput="calculate()"></label>
    <button class="btn remove" onclick="this.parentElement.remove();calculate()">×</button>
  `;
  document.getElementById("list").appendChild(d);
  calculate();
}

let currentCalcData = {};

// Main calculation logic
function calculate() {
  let kwh = 0, peak = 0;
  
  document.querySelectorAll(".appliance").forEach(x => {
    let w = +x.querySelector(".w").value || 0;
    let q = +x.querySelector(".q").value || 0;
    let h = +x.querySelector(".h").value || 0;
    kwh += (w * q * h) / 1000;
    peak += w * q;
  });
  
  let sun = +document.getElementById("sun").value || 4.5;
  let eff = (+document.getElementById("eff").value || 80) / 100;
  
  let solar = kwh / (sun * eff);
  let battery = kwh / 0.8;
  let inv = peak * 1.25;
  let p640 = Math.ceil((solar * 1000) / 640);
  
  // Save current state for history
  currentCalcData = { 
    kwh: kwh.toFixed(2), 
    solar: solar.toFixed(2), 
    battery: battery.toFixed(2),
    inv: (inv / 1000).toFixed(2),
    date: new Date().toLocaleDateString() + ' ' + new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})
  };

  document.getElementById("out").innerHTML = `
    <div class="metric"><span>Daily energy</span><b>${kwh.toFixed(2)} kWh</b></div>
    <div class="metric"><span>Peak load</span><b>${(peak / 1000).toFixed(2)} kW</b></div>
    <div class="metric"><span>Solar array</span><b>${solar.toFixed(2)} kW</b></div>
    <div class="metric"><span>Battery bank</span><b>${battery.toFixed(2)} kWh</b></div>
    <div class="metric"><span>Inverter estimate</span><b>${(inv / 1000).toFixed(2)} kW</b></div>
    <div class="metric"><span>640W panels</span><b>${p640} panels</b></div>`;
    
  document.getElementById("recommend").innerHTML = `<strong>Recommended starting point</strong><br>
    ${solar.toFixed(1)} kW solar · ${battery.toFixed(1)} kWh battery · ${(inv / 1000).toFixed(1)} kW inverter<br>
    <span class="small">Panel count shown is an estimate. Final design should account for surge loads, battery limits and local solar conditions.</span>`;
}

// History Functions
function saveToHistory() {
  let hist = JSON.parse(localStorage.getItem('solarCalcHistory') || '[]');
  hist.unshift(currentCalcData); // Add to beginning
  if(hist.length > 5) hist.pop(); // Keep only the last 5 records
  localStorage.setItem('solarCalcHistory', JSON.stringify(hist));
  loadHistory();
}

function loadHistory() {
  let hist = JSON.parse(localStorage.getItem('solarCalcHistory') || '[]');
  let hDiv = document.getElementById("history-list");
  if(hist.length === 0) { 
    hDiv.innerHTML = "<div class='small' style='text-align:center; padding: 10px;'>No saved calculations yet.</div>"; 
    return; 
  }
  hDiv.innerHTML = hist.map(h => `
    <div class="history-item">
      <div>
        <span>${h.date}</span>
        Solar: <b>${h.solar} kW</b><br>
        Battery: <b>${h.battery} kWh</b>
      </div>
      <div class="hist-right">
        <span>Daily Usage</span>
        <b>${h.kwh} kWh</b>
      </div>
    </div>
  `).join('');
}

function clearHistory() {
  localStorage.removeItem('solarCalcHistory');
  loadHistory();
}

// Initialize the app
presets.forEach(p => addAppliance(p));
loadHistory();
</script>
</body>
</html>
