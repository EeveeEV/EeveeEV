<!DOCTYPE html>
<html lang="en" dir="ltr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TypeSprinter - World Challenge</title>
    <style>
        /* ==========================================================================
           MODERN CARBON DECOR & LAYOUT (MONKEYTYPE STYLE)
           ========================================================================== */
        :root {
            --bg-color: #1e1e24;
            --panel-bg: #16161a;
            --border-color: #2c2e3e;
            --text-muted: #4f5263;
            --text-main: #75798c;
            --text-bright: #d1d0c5;
            --pink-neon: #ff007f;
            --pink-dark: #d8006c;
            --error-red: #ca4754;
        }

        body {
            font-family: 'Courier New', Courier, monospace;
            background-color: var(--bg-color);
            color: var(--text-main);
            margin: 0;
            padding: 0;
            overflow-x: hidden;
        }

        /* 1. ADVERTENTIE ZONE BOVENIN (De Geldmaker) */
        .top-ad-zone {
            background-color: var(--panel-bg);
            padding: 10px 0;
            text-align: center;
            border-bottom: 1px solid var(--border-color);
        }
        .ad-label {
            font-size: 10px;
            text-transform: uppercase;
            color: var(--text-muted);
            display: block;
            margin-bottom: 4px;
            letter-spacing: 1px;
        }
        .ad-banner-placeholder {
            width: 320px;
            height: 50px;
            background-color: var(--bg-color);
            border: 1px dashed var(--border-color);
            margin: 0 auto;
            display: flex;
            justify-content: center;
            align-items: center;
            color: var(--text-muted);
            font-size: 11px;
        }

        /* 2. HOOFDLAYOUT & INTERFACE */
        .app-content {
            padding: 20px;
            max-width: 650px;
            margin: 0 auto;
        }

        .header-controls {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
        }
        .app-logo {
            font-size: 22px;
            font-weight: bold;
            color: var(--pink-neon);
            text-transform: uppercase;
            letter-spacing: 2px;
        }
        .language-selector select {
            background: var(--panel-bg);
            color: var(--text-bright);
            border: 1px solid var(--border-color);
            padding: 6px 12px;
            font-family: inherit;
            border-radius: 4px;
            outline: none;
        }

        /* STATS (LIVE MOTOR) */
        .stats-container {
            display: flex;
            justify-content: space-between;
            margin-bottom: 30px;
            font-size: 24px;
            font-weight: bold;
            background: var(--panel-bg);
            padding: 15px 25px;
            border-radius: 8px;
            border: 1px solid var(--border-color);
        }
        .stat-box { color: var(--pink-neon); }
        .stat-label { font-size: 11px; color: var(--text-muted); display: block; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 2px;}

        /* TYPE VLAK (VLOEIEND & MEESLEPEND) */
        .words-wrapper {
            font-size: 26px;
            line-height: 1.6;
            text-align: left;
            letter-spacing: 1px;
            word-break: break-word;
            min-height: 100px;
            cursor: text;
            margin-bottom: 40px;
            user-select: none;
        }
        .char { position: relative; border-bottom: 2px solid transparent; color: var(--text-main); }
        .char.correct { color: var(--text-bright); }
        .char.incorrect { color: var(--error-red); background-color: rgba(202, 71, 84, 0.1); }
        .char.current { color: var(--text-bright); border-bottom: 2px solid var(--pink-neon); }

        #hidden-input { position: absolute; opacity: 0; z-index: -1; }

        /* 3. MODERN SCOREBORD INTERFACE */
        .leaderboard-section {
            background-color: var(--panel-bg);
            border: 1px solid var(--border-color);
            border-radius: 8px;
            padding: 20px;
            margin-top: 20px;
        }
        .tabs {
            display: flex;
            justify-content: space-around;
            border-bottom: 1px solid var(--border-color);
            margin-bottom: 15px;
        }
        .tab-btn {
            background: none;
            border: none;
            color: var(--text-main);
            font-family: inherit;
            font-size: 14px;
            padding: 10px;
            cursor: pointer;
            text-transform: uppercase;
            font-weight: bold;
        }
        .tab-btn.active {
            color: var(--pink-neon);
            border-bottom: 2px solid var(--pink-neon);
        }
        .score-list {
            text-align: left;
            font-size: 16px;
            line-height: 2;
        }
        .score-item {
            display: flex;
            justify-content: space-between;
            border-bottom: 1px dashed rgba(44, 46, 62, 0.5);
        }
        .score-item:last-child { border: none; }
        .rank { color: var(--pink-neon); font-weight: bold; margin-right: 10px; }
        .score-val { color: var(--text-bright); font-weight: bold; }

        /* RESULTATEN PANEL & VOLGENDE KNOP */
        .results-panel { display: none; margin-bottom: 30px; }
        .completion-text { font-size: 22px; color: var(--pink-neon); margin-bottom: 15px; font-weight: bold; }
        .next-btn {
            background-color: var(--pink-neon);
            color: #ffffff;
            padding: 16px 32px;
            font-size: 18px;
            font-weight: bold;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-family: inherit;
            box-shadow: 0 4px 15px rgba(255, 0, 127, 0.4);
            transition: background 0.2s;
        }
        .next-btn:hover { background-color: var(--pink-dark); }
    </style>
</head>
<body>

    <!-- ADVERTENTIE BOVENAN -->
    <div class="top-ad-zone">
        <span class="ad-label">Advertisement</span>
        <div class="ad-banner-placeholder">[Sticky Banner - Cash Generator]</div>
    </div>

    <div class="app-content">
        <div class="header-controls">
            <div class="app-logo">TypeSprinter</div>
            <div class="language-selector">
                <select id="langSelect" onchange="changeLanguage()">
                    <option value="nl">Nederlands</option>
                    <option value="en" selected>English</option>
                    <option value="zh">中文 (Chinees)</option>
                    <option value="ja">日本語 (Japans)</option>
                    <option value="ko">한국어 (Koreaans)</option>
                    <option value="ru">Русский (Russisch)</option>
                </select>
            </div>
        </div>

        <!-- LIVE INTERFACE -->
        <div class="stats-container">
            <div><span class="stat-label">Time</span><span id="timer" class="stat-box">15</span></div>
            <div><span class="stat-label">WPM</span><span id="wpm" class="stat-box">0</span></div>
            <div><span class="stat-label">Accuracy</span><span id="accuracy" class="stat-box">100%</span></div>
        </div>
        
        <div class="words-wrapper" id="words" onclick="focusInput()"></div>
        <input type="text" id="hidden-input" autocomplete="off" autofocus>

        <!-- SPRINT COMPLETED & NEXT ACTION -->
        <div class="results-panel" id="resultBox">
            <div class="completion-text">🔥 SPEED SPRINT COMPLETE! 🔥</div>
            <button class="next-btn" onclick="resetTest()">Sprint Again &rarr;</button>
        </div>

        <!-- COMPETITIE SCOREBORD (DE HYPE MOTOR) -->
        <div class="leaderboard-section">
            <div class="tabs">
                <button class="tab-btn active" onclick="switchTab('school')">Schools</button>
                <button class="tab-btn" onclick="switchTab('company')">Companies</button>
                <button class="tab-btn" onclick="switchTab('world')">World 100</button>
            </div>
            <div class="score-list" id="leaderboardContent">
                <!-- Wordt realtime geladen door JavaScript -->
            </div>
        </div>
    </div>

    <!-- ==========================================================================
       REALTIME MULTI-LANGUAGE TYPING ENGINE & LOCAL SCORE SYSTEM
       ========================================================================== */ -->
    <script>
        const languageDatabase = {
            nl: ["succes komt naar degenen die elke dag blijven typen und nooit stoppen", "bouw een kwalitatieve database en geniet van mooie momenten met je gezin"],
            en: ["success comes to those who keep typing every single day and never stop", "build a high quality database and enjoy beautiful moments with your family"],
            zh: ["成功属于那些每天坚持打字且永不停止的人", "建立高质量的数据库并与家人一起享受美好时光"],
            ja: ["毎日タイピングを続け決して諦めない人に成功は訪れます", "高品質なデータベースを構築して家族との素晴らしい時間を楽しんでください"],
            ko: ["매일 멈추지 않고 타자를 계속 치는 사람에게 성공이 찾아옵니다", "고품질 데이터베이스를 구축하고 가족과 함께 행복한 시간을 보내세요"],
            ru: ["успех приходит к тем кто продолжает печатать каждый день без остановки", "создайте качественную базу данных и наслаждайтесь прекрасными моментами с семьей"]
        };

        // Dummy data voor de directe hype-ervaring vanavond
        const mockScores = {
            school: [
                { name: "Erasmus University", score: "112 WPM" },
                { name: "Dalton Den Haag", score: "98 WPM" },
                { name: "University of Oxford", score: "95 WPM" }
            ],
            company: [
{ name: "Google Inc.", score: "124 WPM" },{ name: "ASML Technology", score: "115 WPM" },{ name: "Albert Heijn HQ", score: "89 WPM" }],world: [{ name: "TypeGod_99", score: "142 WPM" },{ name: "SpeedyFingers", score: "138 WPM" },{ name: "CodeSprinter", score: "131 WPM" }]};const wordsContainer = document.getElementById('words');const hiddenInput = document.getElementById('hidden-input');const timerElement = document.getElementById('timer');const wpmElement = document.getElementById('wpm');const accuracyElement = document.getElementById('accuracy');const resultBoxElement = document.getElementById('resultBox');const langSelect = document.getElementById('langSelect');const leaderboardContent = document.getElementById('leaderboardContent');let textToType = "";let timeLeft = 15;let timerRunning = false;let intervalId = null;let charElements = [];let errors = 0;function initTest() {clearInterval(intervalId);timeLeft = 15;timerRunning = false;errors = 0;timerElement.innerText = timeLeft;wpmElement.innerText = "0";accuracyElement.innerText = "100%";resultBoxElement.style.display = "none";hiddenInput.disabled = false;hiddenInput.value = "";const currentLang = langSelect.value;const langTexts = languageDatabase[currentLang];textToType = langTexts[Math.floor(Math.random() * langTexts.length)];wordsContainer.innerHTML = '';charElements = [];textToType.split('').forEach((char, index) => {const span = document.createElement('span');span.innerText = char;span.classList.add('char');if (index === 0) span.classList.add('current');wordsContainer.appendChild(span);charElements.push(span);});const isMobile = window.innerWidth <= 900;hiddenInput.setAttribute('autocorrect', isMobile ? 'on' : 'off');hiddenInput.setAttribute('spellcheck', isMobile ? 'true' : 'false');setTimeout(() => { hiddenInput.focus(); }, 100);}function focusInput() { if (!hiddenInput.disabled) hiddenInput.focus(); }hiddenInput.addEventListener('input', () => {if (!timerRunning && hiddenInput.value.length > 0) {timerRunning = true;intervalId = setInterval(() => {if (timeLeft > 0) {timeLeft--;timerElement.innerText = timeLeft;calculateStats();} else { endTest(); }}, 1000);}const inputVal = hiddenInput.value;errors = 0;charElements.forEach((span, index) => {span.classList.remove('current', 'correct', 'incorrect');if (index < inputVal.length) {if (inputVal[index] === textToType[index]) { span.classList.add('correct'); }else { span.classList.add('incorrect'); errors++; }} else if (index === inputVal.length) { span.classList.add('current'); }});if (inputVal.length >= textToType.length) { endTest(); }calculateStats();});function calculateStats() {const inputLength = hiddenInput.value.length;if (inputLength === 0) return;const timeSpentMinutes = (15 - timeLeft) / 60 || 0.01;const correctChars = inputLength - errors;const netWpm = Math.round((correctChars / 5) / timeSpentMinutes);wpmElement.innerText = netWpm < 0 ? 0 : netWpm;const accuracy = Math.round(((inputLength - errors) / inputLength) * 100);accuracyElement.innerText = (accuracy < 0 ? 0 : accuracy) + '%';}function endTest() {clearInterval(intervalId);hiddenInput.disabled = true;resultBoxElement.style.display = "block";}function switchTab(type) {document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));event.target.classList.add('active');let html = "";mockScores[type].forEach((item, index) => {html += <div class="score-item"> <div><span class="rank">#${index + 1}</span>${item.name}</div> <div class="score-val">${item.score}</div> </div>;});leaderboardContent.innerHTML = html;}function changeLanguage() { initTest(); }function resetTest() { initTest(); }document.addEventListener("DOMContentLoaded", () => {initTest();switchTab('school'); // Start met scholen tab geladen});
