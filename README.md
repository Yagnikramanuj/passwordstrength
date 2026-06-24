<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Password & URL Security Toolkit</title>

    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: Arial, sans-serif; }
        body { background: #0f172a; color: white; scroll-behavior: smooth; }
        header { background: #1e293b; padding: 20px; text-align: center; position: sticky; top: 0; z-index: 100; border-bottom: 1px solid #334155; }
        nav { margin-top: 10px; }
        nav a { color: #94a3b8; text-decoration: none; margin: 0 10px; font-weight: bold; font-size: 16px; cursor: pointer; padding-bottom: 5px; transition: color 0.3s, border-bottom 0.3s; }
        nav a:hover { color: white; }
        nav a.active-link { color: white; border-bottom: 3px solid #38bdf8; }
        .container { width: 90%; max-width: 900px; margin: auto; padding: 40px 20px; }
        .page { display: none; animation: fadeIn 0.4s ease-in-out; }
        .page.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .card { background: #1e293b; padding: 25px; margin-bottom: 30px; border-radius: 10px; box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1); }
        h2 { margin-bottom: 15px; color: #38bdf8; }
        label { display: block; margin-top: 15px; margin-bottom: 5px; color: #94a3b8; }
        input { width: 100%; padding: 12px; margin-top: 10px; border: 2px solid #334155; border-radius: 8px; background: #0f172a; color: white; font-size: 16px; }
        input:focus { outline: none; border-color: #2563eb; }
        .input-group { position: relative; margin-top: 10px; }
        .input-group input { margin-top: 0; padding-right: 60px; }
        .toggle-password { position: absolute; right: 15px; top: 50%; transform: translateY(-50%); color: #94a3b8; cursor: pointer; font-weight: bold; font-size: 14px; user-select: none; }
        button { width: 100%; padding: 12px; margin-top: 15px; border: none; border-radius: 8px; background: #2563eb; color: white; cursor: pointer; font-weight: bold; font-size: 16px; transition: background 0.3s; }
        button:hover { background: #1d4ed8; }
        .progress { width: 100%; height: 12px; background: #334155; border-radius: 20px; overflow: hidden; margin-top: 15px; }
        .progress-bar { height: 100%; width: 0%; background: #ef4444; transition: width 0.4s ease; }
        .result { margin-top: 15px; font-weight: bold; font-size: 18px; }
        .about p { margin-top: 12px; line-height: 1.6; color: #cbd5e1; }
    </style>
</head>

<body>

<header>
    <h1>🔐 Security Toolkit</h1>
    <nav>
        <a onclick="showPage('page1', this)" class="active-link">Tools</a>
        <a onclick="showPage('page2', this)">URL Check</a>
        <a onclick="showPage('page3', this)">Info</a>
    </nav>
</header>

<div class="container">

    <!-- PAGE 1: TOOLS -->
    <div id="page1" class="page active">
        <div class="card" id="checker">
            <h2>Password Strength Checker</h2>
            <div class="input-group">
                <input type="password" id="password" maxlength="32" placeholder="Enter Password" onkeyup="checkPassword()">
                <span class="toggle-password" id="toggleBtn" onclick="toggleVisibility()">Show</span>
            </div>
            <div class="progress"><div class="progress-bar" id="bar"></div></div>
            <div class="result" id="strengthText">Strength: -</div>
        </div>

        <div class="card" id="generator">
            <h2>Password Generator</h2>
            <label for="length">Password Length (8-32)</label>
            <input type="number" id="length" min="8" max="32" value="12">
            <button onclick="generatePassword()">Generate Password</button>
            <input type="text" id="generatedPassword" readonly placeholder="Result">
            <button onclick="copyPassword()" style="background: #059669;">Copy Password</button>
        </div>
    </div>

    <!-- PAGE 2: URL CHECK -->
    <div id="page2" class="page">
        <div class="card">
            <h2>URL Safety Checker</h2>
            <input type="text" id="urlInput" placeholder="https://example.com">
            <button onclick="checkURL()">Check URL</button>
            <div id="result" class="result" style="color: #cbd5e1; margin-top:20px;">Result will appear here</div>
        </div>
    </div>

    <!-- PAGE 3: INFO -->
    <div id="page3" class="page">
        <div class="card" id="estimator">
            <h2>Crack Time Estimator</h2>
            <p id="crackTime">Enter a password in Page 1 to estimate its crack time.</p>
        </div>
        <div class="card about" id="about">
            <h2>About Developer</h2>
            <p><strong>Name:</strong> Yagnik Ramanuj</p>
            <p><strong>Email:</strong> <a href="mailto:yagnikramanuj2@gmail.com" style="color: #38bdf8;">yagnikramanuj2@gmail.com</a></p>
            <p><strong>College:</strong> Sir Bhavsinhji Polytechnic Institute, Bhavnagar</p>
            <p><strong>Branch:</strong> Electronics & Communication (EC)</p>
            <p><strong>En.no:</strong> 246490311028</p>
            <p><strong>Internship:</strong> CSRBOX Summer Internship</p>
            <p><strong>Project Duration:</strong> 15 Days</p>
            <p>This project helps users analyze password strength, generate secure passwords, and estimate password cracking time to improve cybersecurity awareness.</p>
        </div>
    </div>

</div>

<script>
function showPage(pageId, element) {
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.querySelectorAll('nav a').forEach(a => a.classList.remove('active-link'));
    document.getElementById(pageId).classList.add('active');
    element.classList.add('active-link');
}

function toggleVisibility() {
    let p = document.getElementById("password"), t = document.getElementById("toggleBtn");
    p.type = (p.type === "password") ? "text" : "password";
    t.innerText = (p.type === "password") ? "Show" : "Hide";
}

function checkPassword() {
    let password = document.getElementById("password").value;
    let score = 0;
    if(password.length >= 8) score += 20;
    if(/[A-Z]/.test(password)) score += 20;
    if(/[a-z]/.test(password)) score += 20;
    if(/[0-9]/.test(password)) score += 20;
    if(/[^A-Za-z0-9]/.test(password)) score += 20;

    let strength = "Very Weak", color = "#ef4444";
    if(score <= 20) { strength = "Very Weak"; color = "#ef4444"; }
    else if(score <= 40) { strength = "Weak / Medium"; color = "#f97316"; }
    else if(score <= 60) { strength = "Strong"; color = "#eab308"; }
    else if(score <= 80) { strength = "Very Strong"; color = "#22c55e"; }
    else { strength = "Excellent"; color = "#15803d"; }

    document.getElementById("bar").style.width = score + "%";
    document.getElementById("bar").style.background = color;
    document.getElementById("strengthText").innerHTML = "Strength: " + strength;

    let time = (score <= 20) ? "Less than 1 Second" : (score <= 40) ? "Few Minutes" : (score <= 60) ? "Few Days" : (score <= 80) ? "Few Years" : "Millions of Years";
    document.getElementById("crackTime").innerHTML = "<b>Estimated Crack Time:</b> " + time;
}

function generatePassword() {
    let len = Math.max(8, Math.min(32, document.getElementById("length").value));
    let chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()_+";
    let pass = "";
    for(let i = 0; i < len; i++) pass += chars.charAt(Math.floor(Math.random() * chars.length));
    document.getElementById("generatedPassword").value = pass;
}

function copyPassword() {
    let copyText = document.getElementById("generatedPassword");
    if(!copyText.value) return;
    navigator.clipboard.writeText(copyText.value).then(() => alert("Copied!"));
}

function checkURL() {
    let url = document.getElementById("urlInput").value.trim();
    let result = document.getElementById("result");
    if (!url) { result.innerHTML = "Please enter a URL."; return; }
    try {
        let parsed = new URL(url);
        result.innerHTML = (parsed.protocol !== "https:") ? "⚠ Warning: Site does not use HTTPS." : "✅ URL format looks safe (basic check only).";
    } catch (e) {
        result.innerHTML = "❌ Invalid URL.";
    }
}
</script>
</body>
</html>
