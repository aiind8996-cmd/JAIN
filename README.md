<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>ARCHER AI - Master Controller</title>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@600;700;800;900&family=Rajdhani:wght@600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --neon-cyan: #00ffaa;
      --neon-glow: rgba(0, 255, 170, 0.45);
      --bg-dark: #020906;
      --panel-border: rgba(0, 255, 170, 0.35);
      --text-muted: #84d9ba;
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

    /* Outer Mobile Frame */
    .archer-hud {
      width: 100%;
      max-width: 440px;
      height: 100vh;
      max-height: 940px;
      background: radial-gradient(circle at center, #052317 0%, #010805 85%);
      border: 2px solid var(--panel-border);
      border-radius: 28px;
      display: flex;
      flex-direction: column;
      position: relative;
      padding: 16px;
      box-shadow: 0 0 45px rgba(0, 255, 170, 0.25);
      overflow: hidden;
    }

    /* Header Bar */
    .top-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 4px 6px 12px;
      border-bottom: 1px solid var(--panel-border);
    }
    .header-title {
      font-family: 'Orbitron', monospace;
      font-size: 18px;
      letter-spacing: 3px;
      font-weight: 800;
      text-shadow: 0 0 10px var(--neon-cyan);
      display: flex;
      align-items: center;
      gap: 8px;
    }
    .online-indicator {
      width: 9px;
      height: 9px;
      background: var(--neon-cyan);
      border-radius: 50%;
      box-shadow: 0 0 12px var(--neon-cyan);
      animation: pulse 1.4s infinite alternate;
    }
    @keyframes pulse {
      0% { opacity: 0.3; transform: scale(0.9); }
      100% { opacity: 1; transform: scale(1.3); }
    }

    /* Dual Transcript Box */
    .transcript-container {
      position: relative;
      margin-top: 12px;
      border: 1px solid var(--panel-border);
      border-radius: 12px;
      background: rgba(0, 255, 170, 0.03);
      padding: 12px;
      backdrop-filter: blur(8px);
    }
    .transcript-tag {
      position: absolute;
      top: -9px;
      left: 18px;
      background: #020906;
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
      min-height: 75px;
      font-size: 13px;
    }
    .col-header {
      font-family: 'Orbitron', monospace;
      font-size: 10px;
      color: var(--text-muted);
      margin-bottom: 5px;
      display: flex;
      align-items: center;
      gap: 4px;
    }
    .col-content {
      line-height: 1.35;
      color: #e5fff5;
      font-weight: 600;
      max-height: 82px;
      overflow-y: auto;
    }

    /* Center Stage (3D Holo Orb + Left Chips) */
    .stage {
      flex: 1;
      position: relative;
      display: flex;
      align-items: center;
      margin: 8px 0;
    }
    .left-controls {
      position: absolute;
      left: 0;
      z-index: 10;
      display: flex;
      flex-direction: column;
      gap: 10px;
    }
    .hud-chip {
      background: rgba(0, 28, 19, 0.65);
      border: 1px solid var(--panel-border);
      color: var(--neon-cyan);
      padding: 7px 14px;
      border-radius: 20px;
      font-size: 11px;
      font-weight: 700;
      letter-spacing: 1.5px;
      cursor: pointer;
      backdrop-filter: blur(6px);
      box-shadow: 0 0 12px rgba(0, 255, 170, 0.08);
      transition: all 0.2s ease;
    }
    .hud-chip:active {
      transform: scale(0.95);
      background: var(--neon-cyan);
      color: #000;
    }
    #holo-core {
      width: 100%;
      height: 100%;
    }

    /* Action Trigger Button */
    .action-panel {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 6px;
      margin-bottom: 10px;
    }
    .mic-button {
      background: rgba(0, 255, 170, 0.12);
      border: 1.5px solid var(--neon-cyan);
      color: var(--neon-cyan);
      border-radius: 30px;
      padding: 10px 32px;
      font-family: 'Orbitron', sans-serif;
      font-size: 13px;
      letter-spacing: 2px;
      cursor: pointer;
      box-shadow: 0 0 20px rgba(0, 255, 170, 0.25);
      transition: all 0.2s ease;
    }
    .mic-button.active {
      background: var(--neon-cyan);
      color: #010805;
      box-shadow: 0 0 35px var(--neon-cyan);
    }

    /* Bottom Widgets */
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

  <!-- Three.js for 3D Holographic Sphere -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
</head>
<body>

  <div class="archer-hud">
    <!-- Header -->
    <div class="top-header">
      <div class="online-indicator"></div>
      <div class="header-title">ARCHER AI</div>
      <span style="font-size: 11px; letter-spacing: 1px;">OWNER: JAIN SIR</span>
    </div>

    <!-- Live Transcript Box -->
    <div class="transcript-container">
      <span class="transcript-tag">TRANSCRIPT</span>
      <div class="dialogue-grid">
        <div>
          <div class="col-header">● ARCHER</div>
          <div class="col-content" id="ai-output">Namaste Jain Sir! Niche 'LISTEN' dabayein aur jo aadesh denge wahi hoga.</div>
        </div>
        <div style="border-left: 1px solid var(--panel-border); padding-left: 10px;">
          <div class="col-header">● JAIN SIR</div>
          <div class="col-content" id="user-output">Mic ka intazar hai...</div>
        </div>
      </div>
    </div>

    <!-- Middle Stage: 3D Hologram Orb -->
    <div class="stage">
      <div class="left-controls">
        <button class="hud-chip" onclick="executeCommand('open memory')">🧠 MEMORY</button>
        <button class="hud-chip" onclick="executeCommand('open chat')">💬 CHAT</button>
        <button class="hud-chip" onclick="executeCommand('open soul')">✨ SOUL</button>
        <button class="hud-chip" onclick="executeCommand('open settings')">⚙️ SETTINGS</button>
      </div>
      <div id="holo-core"></div>
    </div>

    <!-- Action Mic Button -->
    <div class="action-panel">
      <button class="mic-button" id="mic-btn">🎙️ LISTEN</button>
      <span style="font-size: 10px; opacity: 0.8; margin-top: 4px;">TOUCH BUTTON & SPEAK COMMAND</span>
    </div>

    <!-- Widgets -->
    <div class="hud-widgets">
      <div class="widget">
        <div class="widget-head"><span>ACTIVE CONTROLS</span> <span>LIVE</span></div>
        <ul>
          <li>• YouTube Instant Play</li>
          <li>• WhatsApp Direct Launch</li>
          <li>• Phone Calling Dialer</li>
        </ul>
      </div>
      <div class="widget">
        <div class="widget-head"><span>SYSTEM STATUS</span> <span>READY</span></div>
        <ul>
          <li>• Camera & Flashlight</li>
          <li>• Battery & Clock Readout</li>
          <li>• Google Web Search</li>
        </ul>
      </div>
    </div>
  </div>

  <script>
    /* 1. Three.js 3D Visualizer Setup */
    const stage = document.getElementById('holo-core');
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(45, stage.clientWidth / stage.clientHeight, 0.1, 1000);
    camera.position.z = 3.6;

    const renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true });
    renderer.setSize(stage.clientWidth, stage.clientHeight);
    stage.appendChild(renderer.domElement);

    const sphereGeometry = new THREE.SphereGeometry(1.05, 36, 36);
    const sphereMaterial = new THREE.PointsMaterial({ color: 0x00ffaa, size: 0.035, transparent: true, opacity: 0.85 });
    const holoOrb = new THREE.Points(sphereGeometry, sphereMaterial);
    scene.add(holoOrb);

    function createRing(radius, tube, color) {
      const ringGeo = new THREE.TorusGeometry(radius, tube, 6, 80);
      return new THREE.Mesh(ringGeo, new THREE.MeshBasicMaterial({ color: color, wireframe: true }));
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
      if (!stage.clientWidth) return;
      camera.aspect = stage.clientWidth / stage.clientHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(stage.clientWidth, stage.clientHeight);
    });

    /* 2. Direct Voice Recognition Engine */
    const userOutput = document.getElementById('user-output');
    const aiOutput = document.getElementById('ai-output');
    const micBtn = document.getElementById('mic-btn');

    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;

    if (SpeechRecognition) {
      const recognition = new SpeechRecognition();
      recognition.lang = 'hi-IN';
      recognition.continuous = false;
      recognition.interimResults = false;

      micBtn.addEventListener('click', () => {
        try {
          recognition.start();
          micBtn.classList.add('active');
          micBtn.innerText = "● LISTENING...";
          currentSpeed = 0.04;
        } catch(err) {
          recognition.stop();
        }
      });

      recognition.onresult = (e) => {
        const cmd = e.results[0][0].transcript;
        userOutput.innerText = cmd;
        executeCommand(cmd);
      };

      recognition.onend = () => {
        micBtn.classList.remove('active');
        micBtn.innerText = "🎙️ LISTEN";
        currentSpeed = baseSpeed;
      };

      recognition.onerror = () => {
        micBtn.classList.remove('active');
        micBtn.innerText = "🎙️ LISTEN";
        currentSpeed = baseSpeed;
      };
    } else {
      userOutput.innerText = "Chrome me open karein (Speech API required).";
    }

    function speakOut(message) {
      window.speechSynthesis.cancel();
      const utter = new SpeechSynthesisUtterance(message);
      utter.lang = 'hi-IN';
      utter.rate = 1.05;
      utter.onstart = () => { currentSpeed = 0.025; };
      utter.onend = () => { currentSpeed = baseSpeed; };
      window.speechSynthesis.speak(utter);
    }

    /* 3. Real Instant Action Trigger */
    function executeCommand(input) {
      const q = input.toLowerCase();
      let response = "";

      // 1. YouTube Playback
      if (q.includes("youtube") || q.includes("gana") || q.includes("video") || q.includes("song")) {
        let term = q.replace(/youtube|play|chalao|gana|bajao|song|video|open|kholo/g, "").trim();
        response = `Jain sir, YouTube par ${term || "video"} shuru kar raha hoon.`;
        aiOutput.innerText = response;
        speakOut(response);
        setTimeout(() => {
          window.location.assign(`https://www.youtube.com/results?search_query=${encodeURIComponent(term || "new music")}`);
        }, 600);
        return;
      }

      // 2. WhatsApp
      else if (q.includes("whatsapp")) {
        response = "WhatsApp open kar raha hoon Jain sir.";
        aiOutput.innerText = response;
        speakOut(response);
        setTimeout(() => {
          window.location.assign("whatsapp://");
        }, 600);
        return;
      }

      // 3. Direct Phone Call
      else if (q.includes("call") || q.includes("phone")) {
        response = "Calling dialer open ho raha hai Jain sir.";
        aiOutput.innerText = response;
        speakOut(response);
        setTimeout(() => {
          window.location.assign("tel:9876543210");
        }, 600);
        return;
      }

      // 4. Torch / Flashlight
      else if (q.includes("torch") || q.includes("flash")) {
        response = "Torch trigger ho rahi hai.";
        aiOutput.innerText = response;
        speakOut(response);
        navigator.mediaDevices.getUserMedia({ video: { facingMode: "environment" } })
          .then(stream => {
            const track = stream.getVideoTracks()[0];
            if (track.getCapabilities().torch) {
              track.applyConstraints({ advanced: [{ torch: true }] });
              setTimeout(() => track.stop(), 5000);
            }
          });
        return;
      }

      // 5. Camera
      else if (q.includes("camera") || q.includes("photo")) {
        response = "Camera initialize kiya ja raha hai.";
        aiOutput.innerText = response;
        speakOut(response);
        navigator.mediaDevices.getUserMedia({ video: true });
        return;
      }

      // 6. Google Search
      else if (q.includes("search") || q.includes("google") || q.includes("khojo")) {
        let query = q.replace(/search|google|khojo|dhundho|karo/g, "").trim();
        response = `Google par '${query}' dhoond raha hoon.`;
        aiOutput.innerText = response;
        speakOut(response);
        setTimeout(() => {
          window.location.assign(`https://www.google.com/search?q=${encodeURIComponent(query)}`);
        }, 600);
        return;
      }

      // 7. Battery
      else if (q.includes("battery")) {
        if (navigator.getBattery) {
          navigator.getBattery().then(batt => {
            const level = Math.round(batt.level * 100);
            response = `Jain sir, battery abhi ${level}% hai.`;
            aiOutput.innerText = response;
            speakOut(response);
          });
        }
        return;
      }

      // 8. Time & Date
      else if (q.includes("time") || q.includes("samay")) {
        response = `Jain sir, samay hua hai: ${new Date().toLocaleTimeString('hi-IN', { hour: '2-digit', minute: '2-digit' })}`;
      }
      else if (q.includes("date") || q.includes("tarikh")) {
        response = `Aaj ki tarikh hai: ${new Date().toLocaleDateString('hi-IN', { weekday: 'long', day: 'numeric', month: 'long' })}`;
      }

      // 9. Identity
      else if (q.includes("who are you") || q.includes("kaun ho") || q.includes("kisne banaya")) {
        response = "Main Archer AI hoon, Jain sir ka personal system companion!";
      }

      // 10. General Answer
      else {
        response = `Aadesh mila Jain sir: "${input}". Main ise turant execute kar raha hoon.`;
      }

      aiOutput.innerText = response;
      speakOut(response);
    }
  </script>
</body>
</html>
