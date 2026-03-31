Aicalculator 
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Al-Harithiya Calculator</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/decimal.js/10.4.3/decimal.min.js"></script>
<style>
body {
  margin:0;
  padding:0;
  font-family: Arial;
  background: linear-gradient(135deg,#0f0c29,#302b63,#24243e);
  color:white;
  display:flex;
  justify-content:center;
  align-items:center;
  min-height:100vh;
  text-align:center;
}

/* صفحة الترحيب */
#welcome {
  display:flex;
  flex-direction:column;
  justify-content:center;
  align-items:center;
}
#welcome h1 {
  font-size:28px;
  margin-bottom:20px;
}
#welcome .desc {
  font-size:14px;
  margin:5px 0;
}
#welcome button {
  background: rgba(255,255,255,0.1);
  border:2px solid #888;
  padding:20px 40px;
  font-size:18px;
  border-radius:20px;
  color:white;
  cursor:pointer;
  transition:0.3s;
  max-width:500px;
  white-space:normal;
  line-height:1.4;
}
#welcome button:hover { transform: scale(1.05); }

/* صفحة الحاسبة */
#calculator {
  display:none;
  padding:20px;
  width:100%;
  max-width:600px;
  text-align:center;
  overflow:auto;
  max-height:100vh;
}
input {
  padding:12px;
  width:90%;
  font-size:18px;
  border-radius:10px;
  border:2px solid #888;
  text-align:center;
  background: rgba(0,0,0,0.3);
  color:#fff;
  backdrop-filter: blur(5px);
  margin-bottom:20px;
}

/* مربع الأزرار الرمزية */
#buttons-box {
  background: rgba(0,0,0,0.3);
  padding:15px;
  border-radius:15px;
  display:inline-block;
  margin-bottom:20px;
  border:2px solid #888;
}

/* مربع النتائج والشرح */
#result-box {
  margin:10px auto;
  max-width:90%;
  text-align:center;
  border:2px solid #888;
  border-radius:12px;
  padding:10px;
  background: rgba(0,0,0,0.3);
}
#result, #explain {
  background: rgba(0,0,0,0.3);
  padding:10px;
  border-radius:12px;
  font-size:16px;
  min-height:40px;
  margin-bottom:5px;
  overflow-wrap:break-word;
  border:2px solid #888;
}

/* مربع القائمة الفيزيائية */
#physics-box {
  background: rgba(0,0,0,0.3);
  padding:15px;
  border-radius:15px;
  display:inline-block;
  margin-bottom:20px;
  max-width:90%;
  border:2px solid #888;
}

/* الأزرار */
.button-container, #physics-list {
  display:flex;
  justify-content:center;
  flex-wrap:wrap;
  gap:10px;
}
button.calc {
  padding:12px 18px;
  margin:5px;
  border:2px solid #888;
  cursor:pointer;
  border-radius:10px;
  color:white;
  background: rgba(0,0,0,0.3);
  backdrop-filter: blur(5px);
  transition:0.2s;
  font-size:18px;
}
button.calc:hover { transform: scale(1.05);}
#physics-list { max-height:0; overflow:hidden; transition:max-height 0.4s ease; margin-top:10px;}
#physics-list.open { max-height:1000px; }

/* المربع العائم للقوانين */
#overlay {
  display:none;
  position:fixed;
  top:0; left:0;
  width:100%;
  height:100%;
  background: rgba(0,0,0,0.7);
  justify-content:center;
  align-items:center;
  z-index:999;
}
#overlay .popup {
  background: rgba(0,0,0,0.3);
  border:2px solid #888;
  padding:20px;
  border-radius:15px;
  display:flex;
  flex-direction:column;
  gap:10px;
  width:300px;
}
#overlay .popup input {
  padding:10px;
  border-radius:10px;
  border:2px solid #888;
  background: rgba(0,0,0,0.2);
  color:white;
  text-align:center;
}
#overlay .popup button {
  padding:10px;
  border-radius:10px;
  border:2px solid #888;
  background: rgba(0,0,0,0.3);
  color:white;
  cursor:pointer;
}
</style>
</head>
<body>

<!-- صفحة الترحيب -->
<div id="welcome">
  <h1>Hi! Welcome to Al-Harithiya AI Calculator</h1>
  <button onclick="startCalculator()">Start</button>
  <div class="desc">This is a smart calculator powered by AI, containing the most famous physics and math formulas.</div>
  <div class="desc">Designed by: Mustafa Ahmed Shaheed, Ali Saif, and Amir Ghaith Shawqi with assistance from Mustafa Ahmed Khurshid</div>
</div>

<!-- صفحة الحاسبة -->
<div id="calculator">
<input id="input" placeholder="Type operation or use buttons">

<!-- مربع الأزرار الرمزية -->
<div id="buttons-box" class="button-container">
  <button class="calc" onclick="addWord('+')">+</button>
  <button class="calc" onclick="addWord('-')">-</button>
  <button class="calc" onclick="addWord('*')">×</button>
  <button class="calc" onclick="addWord('/')">÷</button>
  <button class="calc" onclick="addWord('**')">^</button>
  <button class="calc" onclick="addWord('/100')">%</button>
  <button class="calc" onclick="clearInput()">C</button>
</div>

<!-- مربع القائمة الفيزيائية -->
<div id="physics-box">
  <button class="calc" onclick="togglePhysics()">Physics Formulas ⬇️</button>
  <div id="physics-list">
    <button class="calc" onclick="showPopup('Force','kg m/s²')">Force</button>
    <button class="calc" onclick="showPopup('Pressure','N m²')">Pressure</button>
    <button class="calc" onclick="showPopup('Energy','kg m/s²')">Energy</button>
    <button class="calc" onclick="showPopup('Kinetic','kg m/s²')">Kinetic</button>
    <button class="calc" onclick="showPopup('Gravity','kg m')">Gravity</button>
    <button class="calc" onclick="showPopup('Ohm','V Ω')">Ohm</button>
    <button class="calc" onclick="showPopup('Work','N m')">Work</button>
    <button class="calc" onclick="showPopup('Power','W s')">Power</button>
    <button class="calc" onclick="showPopup('Density','kg m³')">Density</button>
    <button class="calc" onclick="showPopup('Momentum','kg m/s')">Momentum</button>
    <button class="calc" onclick="showPopup('Acceleration','m/s² s')">Acceleration</button>
    <button class="calc" onclick="showPopup('Velocity','m s')">Velocity</button>
    <button class="calc" onclick="showPopup('Frequency','s Hz')">Frequency</button>
  </div>
</div>

<!-- زر الحساب -->
<button class="calc" onclick="calculate()">Confirm</button>

<!-- الناتج والشرح -->
<div id="result-box">
  <div id="result">Result</div>
  <div id="explain">Explanation</div>
</div>
</div>

<!-- مربع القوانين العائم -->
<div id="overlay">
  <div class="popup">
    <div id="popup-title"></div>
    <input id="val1" placeholder="">
    <input id="val2" placeholder="">
    <button onclick="applyFormula()">Apply</button>
  </div>
</div>

<script>
function togglePhysics(){ document.getElementById("physics-list").classList.toggle("open"); }
function addWord(w){ document.getElementById("input").value += " "+w+" "; }
function clearInput(){ 
  document.getElementById("input").value=""; 
  document.getElementById("result").innerText="Result"; 
  document.getElementById("explain").innerText="Explanation"; 
}
function startCalculator(){
  document.getElementById("welcome").style.display="none";
  document.getElementById("calculator").style.display="block";
}

/* المربع العائم */
let currentFormula = '';
let currentUnits = [];
function showPopup(formula, units){
  currentFormula = formula;
  currentUnits = units.split(' ');
  document.getElementById("popup-title").innerText = formula;
  document.getElementById("val1").placeholder = "First value (" + currentUnits[0] + ")";
  document.getElementById("val2").placeholder = "Second value (" + currentUnits[1] + ")";
  document.getElementById("val1").value = '';
  document.getElementById("val2").value = '';
  document.getElementById("overlay").style.display='flex';
}
function applyFormula(){
  let val1 = document.getElementById("val1").value;
  let val2 = document.getElementById("val2").value;
  let inputStr = '';
  switch(currentFormula){
    case 'Force': inputStr = val1+' * '+val2; break;
    case 'Pressure': inputStr = val1+' / '+val2; break;
    case 'Energy': inputStr = val1+' * '+val2+' ** 2'; break;
    case 'Kinetic': inputStr = '0.5 * '+val1+' * '+val2+' ** 2'; break;
    case 'Gravity': inputStr = val1+' * '+val2+' / 1'; break;
    case 'Ohm': inputStr = val1+' / '+val2; break;
    case 'Work': inputStr = val1+' * '+val2; break;
    case 'Power': inputStr = val1+' / '+val2; break;
    case 'Density': inputStr = val1+' / '+val2; break;
    case 'Momentum': inputStr = val1+' * '+val2; break;
    case 'Acceleration': inputStr = val1+' / '+val2; break;
    case 'Velocity': inputStr = val1+' / '+val2; break;
    case 'Frequency': inputStr = val1+' / '+val2; break;
  }
  document.getElementById("input").value = inputStr;
  document.getElementById("overlay").style.display='none';
}

/* الحساب */
function interpretInput(inputVal){
  return inputVal.toLowerCase()
    .replace(/plus/g,'+')
    .replace(/minus/g,'-')
    .replace(/multiply/g,'*')
    .replace(/divide/g,'/')
    .replace(/percent/g,'/100')
    .replace(/power/g,'**')
    .replace(/\b(n|newton|n)\b/g,''); 
}
function calculate(){
  try{
    let p=interpretInput(document.getElementById("input").value);
    let resultVal = eval(p);
    document.getElementById("result").innerText = resultVal;
    document.getElementById("explain").innerText = document.getElementById("input").value + " = " + resultVal;
  }catch{
    document.getElementById("result").innerText = "Error";
    document.getElementById("explain").innerText = "Check your input";
  }
}
</script>
</body>
</html>
