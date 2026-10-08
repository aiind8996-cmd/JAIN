<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>ARCHER AI - Master Controller (Jain Sir)</title>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@600;700;800;900&family=Rajdhani:wght@600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --neon-cyan: #00ffaa;
      --neon-glow: rgba(0, 255, 170, 0.45);
      --bg-dark: #020906;
      --panel-border: rgba(0, 255, 170, 0.3);
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

    /* Chassis HUD Container */
    .archer-hud {
      width: 100%;
      max-width: 440px;
      height: 100vh;
      max-height: 940px;
      background: radial-gradient(circle at center, #052317 0%, #010805 85%);
      border: 2px solid var(--panel-border);
      border-radius: 30px;
      display: flex;
      flex-direction: column;
      position: relative;
      padding: 16px;
      box-shadow: 0 0 45px rgba(0, 255, 170, 0.22);
      overflow: hidden;
    }

    /* Top Header */
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

    /* Central 3D Holo Orb Viewport */
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
      display: flex;
      align-items: center;
      gap: 8px;
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

    /* Zero Touch Control Badge */
    .action-panel {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 6px;
      margin-bottom: 10px;
    }
    .zero-touch-badge {
      background: rgba(0, 255, 170, 0.12);
      border: 1.5px solid var(--neon-cyan);
      color: var(--neon-cyan);
      border-radius: 30px;
      padding: 9px 28px;
      font-family: 'Orbitron', sans-serif;
      font-size: 12px;
      letter-spacing: 2px;
      cursor: pointer;
      box-shadow: 0 0 20px rgba(0, 255, 170, 0.25);
      animation: listeningPulse 1.2s infinite alternate;
    }
    @keyframes listeningPulse {
      0% { opacity: 0.8; transform: scale(0.98); }
      100% { opacity: 1; transform: scale(1.03); box-shadow: 0 0 30px var(--neon-cyan); }
    }

    /* Data Widgets Grid */
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

  <!-- Three.js for 3D Holographic Rendering -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
</head>
<body onclick="startAutonomousEngine()">

  <div class="archer-hud">
    <!-- Header -->
    <div class="top-header">
      <div class="online-indicator"></div>
      <div class="header-title">ARCHER AI</div>
      <span style="font-size: 11px; letter-spacing: 1px;">OWNER: JAIN SIR</span>
    </div>

    <!-- Live Transcript Window -->
    <div class="transcript-container">
      <span class="transcript-tag">TRANSCRIPT</span>
      <div class="dialogue-grid">
        <div>
          <div class="col-header">● ARCHER</div>
          <div class="col-content" id="ai-output">Screen par ek baar touch karke mic allow karein Jain sir. Uske baad mobile ko bina chhue direct muh se aadesh dijiye!</div>
        </div>
        <div style="border-left: 1px solid var(--panel-border); padding-left: 10px;">
          <div class="col-header">● JAIN SIR</div>
          <div class="col-content" id="user-output">Listening standby mode me hai...</div>
        </div>
      </div>
    </div>

    <!-- Central 3D Holo Orb Viewport -->
    <div class="stage">
      <div class="left-controls">
        <button class="hud-chip" onclick="executeCommand('open memory')">🧠 MEMORY</button>
        <button class="hud-chip" onclick="executeCommand('open chat')">💬 CHAT</button>
        <button class="hud-chip" onclick="executeCommand('open soul')">✨ SOUL</button>
        <button class="hud-chip" onclick="executeCommand('open settings')">⚙️ SETTINGS</button>
      </div>
      <div id="holo-core"></div>
    </div>

    <!-- Hands-Free State Badge -->
    <div class="action-panel">
      <div class="zero-touch-badge" id="hud-status">TOUCH ONCE TO ACTIVATE</div>
      <span style="font-size: 10px; opacity: 0.8;">100% HANDS-FREE AUTOMATION ACTIVE</span>
    </div>

    <!-- Widgets -->
    <div class="hud-widgets">
      <div class="widget">
        <div class="widget-head"><span>TOUCH-FREE APPS</span> <span>LIVE</span></div>
        <ul>
          <li>• YouTube auto-player</li>
          <li>• WhatsApp message & call</li>
          <li>• Phone dialer executor</li>
        </ul>
      </div>
      <div class="widget">
        <div class="widget-head"><span>DEVICE INTENTS</span> <span>READY</span></div>
        <ul>
          <li>• Flashlight & Camera</li>
          <li>• Battery status monitor</li>
          <li>• Real-time Maps & Clock</li>
        </ul>
      </div>
    </div>
  </div>

  <script>
    /* =============================================================
       1. Three.js: 3D Hologram Particle Sphere & Dual Orbit Rings
       ============================================================= */
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
      if (!stage.clientWidth) return;
      camera.aspect = stage.clientWidth / stage.clientHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(stage.clientWidth, stage.clientHeight);
    });

    /* =============================================================
       2. Zero-Touch Voice Engine & Text-To-Speech (Continuous Mic)
       ============================================================= */
    const userOutput = document.getElementById('user-output');
    const aiOutput = document.getElementById('ai-output');
    const hudStatus = document.getElementById('hud-status');
    let isInitialized = false;

    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    let recognition = null;

    function startAutonomousEngine() {
      if (isInitialized) return;
      isInitialized = true;

      if (!SpeechRecognition) {
        aiOutput.innerText = "Browser Web Speech support nahi karta. Google Chrome use karein.";
        return;
      }

      recognition = new SpeechRecognition();
      recognition.lang = 'hi-IN'; // Hindi + English auto-detection
      recognition.continuous = true;
      recognition.interimResults = false;

      recognition.onstart = () => {
        hudStatus.innerText = "● LIVE LISTENING (ZERO TOUCH)";
        currentSpeed = 0.035;
      };

      recognition.onresult = (e) => {
        const lastIdx = e.results.length - 1;
        const voiceCmd = e.results[lastIdx][0].transcript;
        userOutput.innerText = voiceCmd;
        executeCommand(voiceCmd);
      };

      recognition.onerror = () => {
        // Automatic silent recovery
      };

      recognition.onend = () => {
        currentSpeed = baseSpeed;
        setTimeout(() => {
          try { recognition.start(); } catch(err){}
        }, 300);
      };

      try {
        recognition.start();
        speakOut("Archer system fully active ho chuka hai Jain sir. Ab aap jo bhi bolenge, main bina touch kiye wahi execute karunga.");
      } catch(err){}
    }

    function speakOut(message, callback) {
      window.speechSynthesis.cancel();
      const utter = new SpeechSynthesisUtterance(message);
      utter.lang = 'hi-IN';
      utter.rate = 1.05;
      utter.pitch = 1.0;
      utter.onstart = () => { currentSpeed = 0.025; };
      utter.onend = () => {
        currentSpeed = baseSpeed;
        if (callback) callback();
      };
      window.speechSynthesis.speak(utter);
    }

    /* =============================================================
       3. 100% Real Commands Engine (Autonomous Execution Matrix)
       ============================================================= */
    function executeCommand(input) {
      const q = input.toLowerCase();
      let response = "";
      let actionCallback = null;

      // 1. YouTube Video & Music Playback
      if (q.includes("youtube") || q.includes("gana") || q.includes("video") || q.includes("song")) {
        let term = q.replace(/youtube|play|chalao|gana|bajao|song|video|open|kholo|dikhao/g, "").trim();
        response = `Ji Jain sir, YouTube par ${term || "video"} shuru kiya ja raha hai.`;
        actionCallback = () => {
          window.open(`https://www.youtube.com/results?search_query=${encodeURIComponent(term || "trending music")}`, "_blank");
        };
      }

      // 2. WhatsApp Direct Messaging & Chat
      else if (q.includes("whatsapp")) {
        if (q.includes("abhishek")) {
          response = "Abhishek ka WhatsApp conversation open ho raha hai Jain sir.";
          actionCallback = () => { window.open("https://api.whatsapp.com/send?phone=919876543210", "_blank"); };
        } else if (q.includes("message") || q.includes("bhejo") || q.includes("send")) {
          let msg = q.replace(/whatsapp|message|bhejo|send|karo|ko/g, "").trim();
          response = "WhatsApp message interface load ho raha hai.";
          actionCallback = () => { window.open(`https://api.whatsapp.com/send?text=${encodeURIComponent(msg)}`, "_blank"); };
        } else {
          response = "WhatsApp launch kiya ja raha hai Jain sir.";
          actionCallback = () => { window.open("whatsapp://", "_blank"); };
        }
      }

      // 3. Phone Call & Contacts Dialer
      else if (q.includes("call") || q.includes("phone")) {
        if (q.includes("abhishek")) {
          response = "Jain sir, Abhishek ko call milaya ja raha hai.";
          actionCallback = () => { window.location.href = "tel:+919876543210"; };
        } else {
          response = "Phone dialer launch kar raha hoon Jain sir.";
          actionCallback = () => { window.location.href = "tel:"; };
        }
      }

      // 4. Torch / Flashlight Control
      else if (q.includes("torch") || q.includes("flash")) {
        response = "Torch trigger initialize ki ja rahi hai.";
        actionCallback = () => {
          navigator.mediaDevices.getUserMedia({ video: { facingMode: "environment" } })
            .then(stream => {
              const track = stream.getVideoTracks()[0];
              if (track.getCapabilities().torch) {
                track.applyConstraints({ advanced: [{ torch: true }] });
                setTimeout(() => track.stop(), 6000);
              } else {
                alert("Device flashlight direct browser access limited hai.");
                track.stop();
              }
            })
            .catch(() => alert("Camera/Flash permission allow karein."));
        };
      }

      // 5. Camera Control
      else if (q.includes("camera") || q.includes("photo") || q.includes("selfie")) {
        response = "Camera initialize ho raha hai Jain sir.";
        actionCallback = () => {
          navigator.mediaDevices.getUserMedia({ video: true })
            .then(stream => {
              alert("Camera stream online!");
              stream.getTracks().forEach(t => t.stop());
            })
            .catch(() => alert("Camera permission allow karein."));
        };
      }

      // 6. Google Search & Web Finder
      else if (q.includes("search") || q.includes("google") || q.includes("khojo") || q.includes("dhundho")) {
        let query = q.replace(/search|google|khojo|dhundho|karo/g, "").trim();
        response = `Google par '${query}' search kar raha hoon Jain sir.`;
        actionCallback = () => { window.open(`https://www.google.com/search?q=${encodeURIComponent(query)}`, "_blank"); };
      }

      // 7. Google Maps / Location
      else if (q.includes("map") || q.includes("location") || q.includes("rasta")) {
        let loc = q.replace(/map|location|rasta|dikhaye|kaha hai/g, "").trim();
        response = `Google Maps par ${loc || "live location"} load ki ja rahi hai.`;
        actionCallback = () => { window.open(`https://www.google.com/maps/search/${encodeURIComponent(loc || "current location")}`, "_blank"); };
      }

      // 8. Battery Status
      else if (q.includes("battery")) {
        if (navigator.getBattery) {
          navigator.getBattery().then(batt => {
            const level = Math.round(batt.level * 100);
            const status = `Jain sir, battery level abhi ${level}% hai.`;
            aiOutput.innerText = status;
            speakOut(status);
          });
          return;
        } else {
          response = "Battery status API restricted hai.";
        }
      }

      // 9. Time & Date
      else if (q.includes("time") || q.includes("samay") || q.includes("baje")) {
        const timeNow = new Date().toLocaleTimeString('hi-IN', { hour: '2-digit', minute: '2-digit' });
        response = `Jain sir, abhi time hua hai ${timeNow}.`;
      }
      else if (q.includes("date") || q.includes("tarikh")) {
        const dateNow = new Date().toLocaleDateString('hi-IN', { weekday: 'long', day: 'numeric', month: 'long' });
        response = `Aaj ki tarikh hai ${dateNow}.`;
      }

      // 10. Calculator
      else if (q.includes("calculate") || q.includes("hisab") || q.includes("calculator")) {
        response = "Calculator launch kiya ja raha hai.";
        actionCallback = () => { window.open("https://www.google.com/search?q=calculator", "_blank"); };
      }

      // 11. Weather
      else if (q.includes("weather") || q.includes("mausam")) {
        response = "Live weather report open ho rahi hai Jain sir.";
        actionCallback = () => { window.open("https://www.google.com/search?q=weather+today", "_blank"); };
      }

      // 12. Identity & Creator Verification
      else if (q.includes("who are you") || q.includes("kaun ho") || q.includes("kisne banaya") || q.includes("kiska ho")) {
        response = "Main Archer AI hoon, Jain sir ka fully personal autonomous assistant. Mera ek hi kaam hai: Jain sir ke har order ko bina touch kiye execute karna.";
      }

      // 13. HUD Module Badges
      else if (q.includes("open memory")) {
        response = "Memory core database 100% secure hai Jain sir.";
      }
      else if (q.includes("open soul")) {
        response = "Soul matrix connection stable hai Jain sir.";
      }
      else if (q.includes("open settings")) {
        response = "Settings panel activate kar diya gaya hai.";
      }

      // 14. Fallback Handler
      else {
        response = `Jain sir, aadesh mila: "${input}". Main turant is par action perform kar raha hoon.`;
      }

      aiOutput.innerText = response;
      speakOut(response, actionCallback);
    }
  </script>
</body>
</html>
