<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>行至朝雾里，坠入暮云间。</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Noto+Serif+SC:wght@600&family=Orbitron:wght@700&display=swap');

    body {
      margin: 0;
      padding: 0;
      height: 100vh;
      background: radial-gradient(circle at center, #050505, #000000 90%);
      display: flex;
      justify-content: center;
      align-items: center;
      overflow: hidden;
      font-family: 'Noto Serif SC', serif;
    }

    .fog {
      position: absolute;
      top: 0;
      left: 0;
      width: 200%;
      height: 200%;
      background: radial-gradient(circle, rgba(255,255,255,0.05) 10%, transparent 70%);
      filter: blur(90px);
      animation: fogMove 30s infinite linear;
      opacity: 0.25;
      z-index: 0;
    }

    @keyframes fogMove {
      from { transform: translate(-50%, -50%); }
      to { transform: translate(50%, 50%); }
    }

    a {
      text-decoration: none;
      color: #ffcc00;
      text-align: center;
      z-index: 10;
      position: relative;
      transition: 0.3s;
    }

    .quote-chinese {
      display: block;
      font-size: 3rem;
      font-weight: bold;
      text-shadow: 0 0 15px #ffcc00, 0 0 30px #ff6600;
      transition: transform 0.15s, text-shadow 0.15s;
    }

    .translation {
      display: block;
      font-family: 'Orbitron', sans-serif;
      margin-top: 20px;
      font-size: 1.1rem;
      color: #ccc;
      text-transform: uppercase;
      letter-spacing: 2px;
      text-shadow: 0 0 10px #ffcc00;
      transition: text-shadow 0.3s;
    }

    .author {
      display: block;
      margin-top: 30px;
      font-size: 1rem;
      color: #999;
      font-style: italic;
      letter-spacing: 1px;
    }

    a:hover {
      color: #fff;
    }

    /* Musik */
    audio {
      position: fixed;
      bottom: 10px;
      right: 10px;
      opacity: 0.5;
      transition: 0.3s;
      z-index: 20;
    }
    audio:hover { opacity: 1; }
  </style>
</head>
<body>
  <div class="fog"></div>

  <!-- Seluruh teks jadi link -->
  <a href="https://x.com/YP1555" target="_blank">
    <span class="quote-chinese" id="mainText">行至朝雾里，坠入暮云间。</span>
    <span class="translation" id="subText">Melangkah di kabut pagi, terjatuh di antara awan senja.</span>
    <span class="author">— by Aphen a.k.a @YP1555</span>
  </a>

  <!-- Musik Breakbeat -->
  <audio id="audio" autoplay loop controls>
    <source src="https://files.freemusicarchive.org/storage-freemusicarchive-org/music/no_curator/Breakbeat_Heaven/Breakbeat_Heaven_-_01_-_Neon_Trip.mp3" type="audio/mpeg">
    Browser Anda tidak mendukung pemutar audio.
  </audio>

  <script>
    const audio = document.getElementById("audio");
    const mainText = document.getElementById("mainText");
    const subText = document.getElementById("subText");

    // Web Audio API setup
    const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    const source = audioCtx.createMediaElementSource(audio);
    const analyser = audioCtx.createAnalyser();
    analyser.fftSize = 256;
    const bufferLength = analyser.frequencyBinCount;
    const dataArray = new Uint8Array(bufferLength);

    source.connect(analyser);
    analyser.connect(audioCtx.destination);

    // Aktifkan context ketika user berinteraksi
    document.body.addEventListener("click", () => {
      if (audioCtx.state === "suspended") {
        audioCtx.resume();
      }
    });

    function animate() {
      requestAnimationFrame(animate);
      analyser.getByteFrequencyData(dataArray);

      // Ambil kekuatan bass dari 0-30 Hz
      const bass = dataArray.slice(0, 30).reduce((a, b) => a + b, 0) / 30;

      const scale = 1 + bass / 300;
      const glow = Math.min(255, bass * 2);

      // Efek jedag jedug sinkron musik
      mainText.style.transform = `scale(${scale})`;
      mainText.style.textShadow = `0 0 ${glow / 5}px #ffcc00, 0 0 ${glow / 3}px #ff6600`;
      subText.style.textShadow = `0 0 ${glow / 10}px #ffcc00`;
    }

    animate();
  </script>
</body>
</html>
