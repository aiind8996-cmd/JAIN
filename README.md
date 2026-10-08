# JAIN
AI 
<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>ARCHER AI - Jarvis OS</title>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700;900&family=Rajdhani:wght@500;600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --neon-cyan: #00ffaa;
      --neon-glow: rgba(0, 255, 170, 0.45);
      --bg-dark: #030806;
      --panel-border: rgba(0, 255, 170, 0.25);
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      user-select: none;
      -webkit-tap-highlight-color: transparent;
    }

    body {
      background-color: #000;
      color: var(--neon-cyan);
      font-family: 'Rajdhani', sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      overflow: hidden;
    }

    /* Device Container */
    .archer-hud {
      width: 100%;
      max-width: 430px;
      height: 100vh;
      max-height: 920px;
      background: radial-gradient(circle at center, #061912 0%, #010805 85%);
      border: 2px solid var(--panel-border);
      border-radius: 28px;
      display: flex;
      flex-direction: column;
      position: relative;
      padding: 16px;
      box-shadow: 0 0 35px rgba(0, 255, 170, 0.15);
      overflow: hidden;
    }

    /* Header Bar */
    .top-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 6px 4px 12px;
      border-bottom: 1px solid var(--panel-border);
    }
    .header-title {
      font-family: 'Orbitron', monospace;
      font-size: 17px;
      letter-spacing: 3px;
      font-weight: 700;
      text-shadow: 0 0 10px var(--neon-cyan);
      display: flex;
      align-items: center;
      gap: 8px;
    }
    .online-indicator {
      width: 8px;
      height: 8px;
      background: var(--neon-cyan);
      border-radius: 50%;
      box-shadow: 0 0 10px var(--neon-cyan);
      animation: pulse 1.5s infinite alternate;
    }
    @keyframes pulse {
      0% { opacity: 0.3; transform: scale(0.9); }
      100% { opacity: 1; transform: scale(1.25); }
    }

    /* Dual Transcript Box */
    .transcript-container {
      position: relative;
      margin-top: 14px;
      border: 1px solid var(--panel-border);
      border-radius: 12px;
      background: rgba(0, 255, 170, 0.02);
      padding: 12px;
      backdrop-filter: blur(8px);
    }
    .transcript-tag {
      position: absolute;
      top: -9px;
      left: 18px;
      background: #020b08;
      padding: 0 8px;
      font-family: 'Orbitron', sans-serif;
      font-size: 10px;
      letter-spacing: 2px;
      color: var(--neon-cyan);
    }
    .dialogue-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
      min-height: 70px;
      font-size: 13px;
    }
    .col-header {
      font-family: 'Orbitron', monospace;
      font-size: 10px;
      color: #79ffe1;
      margin-bottom: 5px;
      display: flex;
      align-items: center;
      gap: 4px;
    }
    .col-content {
      line-height: 1.35;
      color: #e0fff4;
      font-weight: 600;
      max-height: 80px;
      overflow-y: auto;
    }

    /* Core Middle Stage */
    .stage {
      flex: 1;
      position: relative;
      display: flex;
      align-items: center;
      margin: 8px 0;
    }

    /* Left Hologram Action Badges */
    .left-controls {
      position: absolute;
      left: 0;
      z-index: 10;
      display: flex;
      flex-direction: column;
      gap: 10px;
    }
    .hud-chip {
      background: rgba(0, 25, 18, 0.6);
      border: 1px solid var(--panel-border);
      color: var(--neon-cyan);
      padding: 6px 14px;
      border-radius: 20px;
      font-size: 11px;
      font-weight: 700;
      letter-spacing: 1.5px;
      display: flex;
      align-items: center;
      gap: 8px;
      cursor: pointer;
      backdrop-filter: blur(5px);
      box-shadow: 0 0 12px rgba(0, 255, 170, 0.08);
      transition: all 0.2s ease;
    }
    .hud-chip:active {
      transform: scale(0.95);
      background: var(--neon-cyan);
      color: #000;
    }

    /* 3D Holo Core Container */
    #holo-core {
      width: 100%;
      height: 100%;
    }

    /* Bottom Control Trigger */
    .action-panel {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 8px;
      margin-bottom: 12px;
    }
    .mic-button {
      background: rgba(0, 255, 170, 0.08);
      border: 1.5px solid var(--neon-cyan);
      color: var(--neon-cyan);
      border-radius: 30px;
      padding: 10px 32px;
      font-family: 'Orbitron', sans-serif;
      font-size: 13px;
      letter-spacing: 2px;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 10px;
      box-shadow: 0 0 20px rgba(0, 255, 170, 0.25);
      transition: all 0.25s ease;
    }
    .mic-button.listening {
      background: var(--neon-cyan);
      color: #030806;
      box-shadow: 0 0 35px var(--neon-cyan);
      animation: listeningPulse 1s infinite alternate;
    }
    @keyframes listeningPulse {
      0% { transform: scale(0.98); }
      100% { transform: scale(1.05); }
    }

    /* Multi-Widget Grid */
    .hud-widgets {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 10px;
      margin-top: 4px;
    }
    .widget {
      border: 1px solid var(--panel-border);
      border-radius: 12px;
      padding: 10px;
      background: rgba(0, 255, 170, 0.02);
      font-size: 11px;
    }
    .widget-head {
      font-family: 'Orbitron', sans-serif;
      font-size: 9.5px;
      letter-spacing: 1px;
      color: #8affd6;
      border-bottom: 1px solid rgba(0, 255, 170, 0.15);
      padding-bottom: 4px;
      margin-bottom: 6px;
      display: flex;
      justify-content: space-between;
    }
    .widget ul {
      list-style: none;
    }
    .widget li {
      margin: 4px 0;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      opacity: 0.9;
    }
  </style>

  <!-- Three.js Library -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
</head>
<body>

  <div class="archer-hud">
    <!-- Top System Header -->
    <div class="top-header">
      <div class="online-indicator"></div>
      <div class="header-title">ARCHER AI</div>
      <span style="font-size: 11px; letter-spacing: 1px;">USER: JACK</span>
    </div>

    <!-- Live Transcript HUD -->
    <div class="transcript-container">
      <span class="transcript-tag">TRANSCRIPT</span>
      <div class="dialogue-grid">
        <div>
          <div class="col-header">● ARCHER</div>
          <div class="col-content" id="ai-output">Hello Jack Sir! Main Archer AI hoon. Batayiye, aaj kya command hai?</div>
        </div>
        <div style="border-left: 1px solid var(--panel-border); padding-left: 10px;">
          <div class="col-header">● JACK</div>
          <div class="col-content" id="user-output">"LISTEN" dabayein aur kuch bhi poochein...</div>
        </div>
      </div>
    </div>

    <!-- Central 3D Holo Realm -->
    <div class="stage">
      <div class="left-controls">
        <button class="hud-chip" onclick="processCommand('open memory')">🧠 MEMORY</button>
        <button class="hud-chip" onclick="processCommand('open chat')">💬 CHAT</button>
        <button class="hud-chip" onclick="processCommand('open soul')">✨ SOUL</button>
        <button class="hud-chip" onclick="processCommand('open settings')">⚙️ SETTINGS</button>
      </div>
      <div id="holo-core"></div>
    </div>

    <!-- Action Mic Trigger -->
    <div class="action-panel">
      <button class="mic-button" id="mic-trigger">
        <span id="mic-icon">🎙️</span> <span id="mic-label">LISTEN</span>
      </button>
    </div>

    <!-- Bottom Data Widgets -->
    <div class="hud-widgets">
      <div class="widget">
        <div class="widget-head"><span>SYSTEM STATUS</span> <span>SECURE</span></div>
        <ul id="status-list">
          <li>• Neural Core: Active</li>
          <li>• Owner: Jack Sir</li>
          <li>• Voice Link: Synced</li>
        </ul>
      </div>
      <div class="widget">
        <div class="widget-head"><span>COMMAND LOGS</span> <span style="color:#00ffaa;">(LIVE)</span></div>
        <ul>
          <li>✓ Voice model loaded</li>
          <li>☐ Ready for queries</li>
          <li>☐ Ready for actions</li>
        </ul>
      </div>
    </div>
  </div>

  <script>
    /* -------------------------------------------------------------
       1. Three.js: Holographic Sphere & Rings Animation
       ------------------------------------------------------------- */
    const stage = document.getElementById('holo-core');
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(45, stage.clientWidth / stage.clientHeight, 0.1, 1000);
    camera.position.z = 3.6;

    const renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true });
    renderer.setSize(stage.clientWidth, stage.clientHeight);
    renderer.setPixelRatio(window.devicePixelRatio);
    stage.appendChild(renderer.domElement);

    const sphereGeometry = new THREE.SphereGeometry(1.05, 36, 36);
    const sphereMaterial = new THREE.PointsMaterial({
      color: 0x00ffaa,
      size: 0.035,
      transparent: true,
      opacity: 0.85
    });
    const holoOrb = new THREE.Points(sphereGeometry, sphereMaterial);
    scene.add(holoOrb);

    function createRing(radius, tube, color) {
      const ringGeo = new THREE.TorusGeometry(radius, tube, 6, 80);
      const ringMat = new THREE.MeshBasicMaterial({ color: color, wireframe: true });
      return new THREE.Mesh(ringGeo, ringMat);
    }
    const ring1 = createRing(1.45, 0.015, 0x00ffaa);
    ring1.rotation.x = Math.PI / 2.5;
    scene.add(ring1);

    const ring2 = createRing(1.7, 0.008, 0x33ffbb);
    ring2.rotation.y = Math.PI / 3;
    scene.add(ring2);

    let baseSpeed = 0.008;
    let currentSpeed = 0.008;

    function renderLoop() {
      requestAnimationFrame(renderLoop);
      holoOrb.rotation.y += currentSpeed;
      holoOrb.rotation.x += currentSpeed * 0.4;
      ring1.rotation.z += currentSpeed * 0.8;
      ring2.rotation.x += currentSpeed * 0.6;
      renderer.render(scene, camera);
    }
    renderLoop();

    window.addEventListener('resize', () => {
      camera.aspect = stage.clientWidth / stage.clientHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(stage.clientWidth, stage.clientHeight);
    });

    /* -------------------------------------------------------------
       2. Voice Recognition & Speech Synthesizer
       ------------------------------------------------------------- */
    const micBtn = document.getElementById('mic-trigger');
    const micLabel = document.getElementById('mic-label');
    const micIcon = document.getElementById('mic-icon');
    const userOutput = document.getElementById('user-output');
    const aiOutput = document.getElementById('ai-output');

    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    let recognition;

    if (SpeechRecognition) {
      recognition = new SpeechRecognition();
      recognition.lang = 'hi-IN'; // Hindi + English mix support
      recognition.interimResults = false;

      recognition.onstart = () => {
        micBtn.classList.add('listening');
        micLabel.innerText = 'LISTENING...';
        micIcon.innerText = '🔴';
        currentSpeed = 0.04;
      };

      recognition.onresult = (e) => {
        const query = e.results[0][0].transcript;
        userOutput.innerText = query;
        processCommand(query);
      };

      recognition.onerror = () => {
        aiOutput.innerText = "Awaz thek se sunayi nahi di, Jack sir dobara bole.";
      };

      recognition.onend = () => {
        micBtn.classList.remove('listening');
        micLabel.innerText = 'LISTEN';
        micIcon.innerText = '🎙️';
        currentSpeed = baseSpeed;
      };

      micBtn.addEventListener('click', () => {
        try {
          recognition.start();
        } catch (err) {
          recognition.stop();
        }
      });
    } else {
      userOutput.innerText = "Speech API supported nahi hai. Chrome browser use karein.";
    }

    function speakOut(message) {
      window.speechSynthesis.cancel();
      const utter = new SpeechSynthesisUtterance(message);
      utter.lang = 'hi-IN';
      utter.rate = 1.05;
      utter.pitch = 1.0;
      utter.onstart = () => { currentSpeed = 0.025; };
      utter.onend = () => { currentSpeed = baseSpeed; };
      window.speechSynthesis.speak(utter);
    }

    /* -------------------------------------------------------------
       3. Advanced Real Intelligence & Action Engine (Smart Replies)
       ------------------------------------------------------------- */
    function processCommand(input) {
      const text = input.toLowerCase();
      let response = "";

      // Q: Who are you / Tum kaun ho?
      if (text.includes("who are you") || text.includes("kaun ho") || text.includes("tumhara naam") || text.includes("what is your name")) {
        response = `Main Archer AI hoon, Jack sir ka personal futuristic digital companion. Main aapke commands execute karne aur system manage karne ke liye banaya gaya hoon.`;
      }
      
      // Q: Who made you / Kisne banaya hai?
      else if (text.includes("made you") || text.includes("created you") || text.includes("kisne banaya")) {
        response = `Mujhe ek advanced developer aur visionary Jack sir ke liye design kiya gaya hai taaki futuristic Jarvis jaisa experience mil sake.`;
      }

      // Action: YouTube Play / Search
      else if (text.includes("youtube")) {
        if (text.includes("play") || text.includes("chalao") || text.includes("gana") || text.includes("bajao")) {
          const query = text.replace(/youtube|play|chalao|gana|bajao/g, "").trim();
          response = `Ji Jack sir, YouTube par ${query || "music"} play kiya ja raha hai.`;
          window.open(`https://www.youtube.com/results?search_query=${encodeURIComponent(query || "lofi songs")}`, '_blank');
        } else {
          response = "YouTube open kar raha hoon Jack sir.";
          window.open("https://www.youtube.com", '_blank');
        }
      }

      // Action: Google Search
      else if (text.includes("search") || text.includes("khojo") || text.includes("dhundho")) {
        const query = text.replace(/search|khojo|dhundho|karo/g, "").trim();
        response = `Google par '${query}' search kiya ja raha hai Jack sir.`;
        window.open(`https://www.google.com/search?q=${encodeURIComponent(query)}`, '_blank');
      }

      // Action: Time & Date
      else if (text.includes("time") || text.includes("samay") || text.includes("baje")) {
        const now = new Date();
        const timeStr = now.toLocaleTimeString('hi-IN', { hour: '2-digit', minute: '2-digit' });
        response = `Jack sir, abhi samay ho raha hai ${timeStr}.`;
      }
      else if (text.includes("date") || text.includes("tarikh")) {
        const now = new Date();
        const dateStr = now.toLocaleDateString('hi-IN', { weekday: 'long', day: 'numeric', month: 'long', year: 'numeric' });
        response = `Aaj ki tarikh hai ${dateStr}.`;
      }

      // Action: Camera
      else if (text.includes("camera") || text.includes("photo")) {
        response = "Camera feed initialize ki ja rahi hai Jack sir.";
        navigator.mediaDevices.getUserMedia({ video: true })
          .then(stream => {
            alert("Camera link active! Permission granted.");
            stream.getTracks().forEach(track => track.stop());
          })
          .catch(() => alert("Camera access deny ho gaya hai."));
      }

      // HUD Buttons
      else if (text.includes("open memory")) {
        response = "Neural Memory bank secure hai Jack sir. Sabhi logs synchronized hain.";
      }
      else if (text.includes("open soul")) {
        response = "Soul core sync status 100% optimal hai Jack sir.";
      }
      else if (text.includes("open settings")) {
        response = "System configuration panel open kiya ja raha hai.";
      }

      // General intelligent fallback
      else {
        response = `Maine aapka command note kar liya hai Jack sir: "${input}". Is par processing shuru kar di gayi hai.`;
      }

      aiOutput.innerText = response;
      speakOut(response);
    }
  </script>
</body>
</html>
