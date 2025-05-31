<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Ulang Tahun</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      background-color: black;
      color: white;
      font-family: 'Segoe UI', sans-serif;
      overflow: hidden;
      touch-action: pan-y;
    }

    canvas {
      position: fixed;
      top: 0;
      left: 0;
      z-index: -1;
      width: 100vw;
      height: 100vh;
    }

    .slides-container {
      display: flex;
      width: 300vw;
      height: 100vh;
      transition: transform 0.5s ease-in-out;
    }

    .slide {
      width: 100vw;
      height: 100vh;
      flex-shrink: 0;
      text-align: center;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      display: flex;
      padding: 20px;
    }

    #typing-text {
      font-size: 32px;
      white-space: nowrap;
      overflow: hidden;
      border-right: 2px solid white;
      width: 0;
      animation: typing 5s steps(30, end) forwards, blink 0.7s step-end infinite;
    }

    @keyframes typing {
      from { width: 0 }
      to { width: 100% }
    }

    @keyframes blink {
      50% { border-color: transparent }
    }

    .cake {
      width: 100px;
      height: 120px;
      background: #ff4081;
      border-radius: 10px;
      position: relative;
      margin: 30px auto;
      animation: float 2s ease-in-out infinite;
    }

    .candle {
      width: 10px;
      height: 30px;
      background: yellow;
      position: absolute;
      top: -30px;
      left: 45px;
      border-radius: 5px;
    }

    @keyframes float {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-10px); }
    }

    .btn {
      margin-top: 20px;
      padding: 10px 20px;
      font-size: 18px;
      border: none;
      border-radius: 10px;
      background: #ff4081;
      color: white;
      cursor: pointer;
      transition: 0.3s;
    }

    .btn:hover { background: #e91e63; }

    .photo-slide img {
      max-width: 80%;
      border: 4px solid white;
      border-radius: 15px;
      box-shadow: 0 0 20px rgba(255,255,255,0.3);
    }

    .gallery {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 15px;
      margin-top: 20px;
      max-width: 90%;
    }

    .gallery img {
      width: 120px;
      height: 120px;
      object-fit: cover;
      border-radius: 10px;
      border: 2px solid white;
      transition: transform 0.3s;
      opacity: 0;
      transform: translateY(20px);
      animation: fadeIn 0.5s forwards;
    }

    @keyframes fadeIn {
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    .lightbox {
      display: none;
      position: fixed;
      top: 0; left: 0;
      width: 100vw; height: 100vh;
      background-color: rgba(0, 0, 0, 0.8);
      justify-content: center;
      align-items: center;
      z-index: 1000;
    }

    .lightbox img {
      max-width: 90%;
      max-height: 90%;
      border: 4px solid white;
      border-radius: 15px;
      box-shadow: 0 0 20px rgba(255, 255, 255, 0.3);
    }

    /* Amplop & Kertas */
.envelope-container {
  position: relative;
  width: min(90vw, 300px);
  height: min(60vw, 200px);
  margin: 40px auto;
  perspective: 1200px;
  cursor: pointer;
}

.envelope {
  width: 100%;
  height: 100%;
  position: absolute;
  transform-style: preserve-3d;
  transition: transform 1.2s ease;
}

.envelope.open {
  transform: rotateX(-180deg);
}

.envelope .front,
.envelope .back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  background: linear-gradient(135deg, #ff4081, #f06292);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: clamp(14px, 4vw, 20px);
  color: white;
  padding: 10px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.4);
  text-align: center;
}

.envelope .front {
  z-index: 2;
}

.envelope .back {
  transform: rotateX(180deg);
}

.paper {
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translate(-50%, 20px);
  background: #fff;
  color: #333;
  width: 250%;
  max-width: 500px;
  padding: 25px;
  border-radius: 100px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.3);
  transition: transform 0.8s ease-out, opacity 0.8s ease;
  transform-origin:top center;
  opacity: 0;
  z-index: 1;
  font-size: clamp(16px, 3vw, 20px);
  line-height: 1.8;
  text-align: center;
}

.paper.show {
  transform: translate(-50%, -100%);
  opacity: 1;
}
    .harapan {
      font-size: 20px;
      line-height: 1.6;
    }

  </style>
</head>
<body>

  <canvas></canvas>

  <div id="slider" class="slides-container">
    <!-- SLIDE 1 -->
    <div class="slide">
      <h1 id="typing-text">Selamat Ulang Tahun Sayang! 🎉</h1>
      <div class="cake"><div class="candle"></div></div>
      <button class="btn" onclick="playMusic()">🔊 Mainkan Musik</button>
      <button class="btn" onclick="nextSlide()">Lanjut ➡️</button>
    </div>

    <!-- SLIDE 2 (Amplop) -->
    <div class="slide">
  <h2>🎂 Harapan untukmu 🎂</h2>
  <div class="envelope-container" onclick="openEnvelope()">
    <div class="envelope" id="envelope">
      <div class="front">📩 Open</div>
      <div class="back">📨 Terbuka</div>
    </div>
    <div class="paper" id="paper">
      <p class="harapan">
        Di hari yang penuh kebahagiaan ini, semoga kamu diberikan panjang umur, sehat selalu, rejeki yang lancar, dan semua yang kamu impikan cita cita kamu tercapai. 
        Selamat bertambah usia pacarku, semoga kedepannya kita selalu sama sama sampe takdir berkata.<br><br>
        Terima kasih sudah menjadi bagian terindah dalam hidupku. Aku sayang kamu, selalu. ❤️
      </p>
    </div>
  </div>

  <div>
    <button class="btn" onclick="prevSlide()">⬅️ Kembali</button>
    <button class="btn" onclick="nextSlide()">Lanjut ➡️</button>
  </div>
</div>

    <!-- SLIDE 3 (Galeri Foto) -->
    <div class="slide photo-slide">
      <h2>📸 MEMORY TIME 📸</h2>
      <div class="gallery">
        <img src="poto/WhatsApp Image 2025-05-25 at 22.31.06.jpeg" alt="Kenangan 1" />
        <img src="poto/WhatsApp Image 2025-05-25 at 22.31.09.jpeg" alt="Kenangan 2" />
        <img src="poto/WhatsApp Image 2025-05-25 at 22.31.10.jpeg" alt="Kenangan 3" />
        <img src="poto/WhatsApp Image 2025-05-25 at 22.31.11.jpeg" alt="Kenangan 4" />
        <img src="poto/WhatsApp Image 2025-05-25 at 22.31.11 (1).jpeg" alt="Kenangan 5" />
      </div>
      <div>
        <P>Moment dimana kita main dan jajan keliling serang </P>
        <button class="btn" onclick="prevSlide()">⬅️ Kembali</button>
      </div>
    </div>
  </div>

  <!-- Lightbox -->
  <div class="lightbox" id="lightbox">
    <img src="" id="lightbox-img" alt="preview">
  </div>

  <!-- Musik -->
  <audio id="bg-music" loop>
    <source src="music/Ed Sheeran - Perfect (lyrics) Mix Playlist.mp3" type="audio/mpeg" />
    Browser Anda tidak mendukung audio.
  </audio>

  <script>
    // Slider logic
    let currentSlide = 0;
    const totalSlides = 3;
    const slider = document.getElementById('slider');

    function updateSlide() {
      slider.style.transform = `translateX(-${currentSlide * 100}vw)`;

      // Reset typing animation saat kembali ke slide 0
      if (currentSlide === 0) {
        const typing = document.getElementById('typing-text');
        typing.style.animation = 'none';
        typing.offsetHeight;
        typing.style.animation = 'typing 5s steps(30, end) forwards, blink 0.7s step-end infinite';
      }

      // Trigger animasi fade-in galeri
      if (currentSlide === 2) {
        const imgs = document.querySelectorAll('.gallery img');
        imgs.forEach((img, index) => {
          img.style.animationDelay = `${index * 0.2}s`;
        });
      }
    }

    function nextSlide() {
      if (currentSlide < totalSlides - 1) {
        currentSlide++;
        updateSlide();
      }
    }

    function prevSlide() {
      if (currentSlide > 0) {
        currentSlide--;
        updateSlide();
      }
    }

    let startX = 0;
    slider.addEventListener('touchstart', e => startX = e.touches[0].clientX);
    slider.addEventListener('touchend', e => {
      let diff = startX - e.changedTouches[0].clientX;
      if (diff > 50) nextSlide();
      else if (diff < -50) prevSlide();
    });

    function playMusic() {
      const music = document.getElementById('bg-music');
      if (music.paused) music.play();
    }

    function openEnvelope() {
      const envelope = document.querySelector('.envelope');
      envelope.classList.add('open');
    }

    // Lightbox
    const galleryImgs = document.querySelectorAll('.gallery img');
    const lightbox = document.getElementById('lightbox');
    const lightboxImg = document.getElementById('lightbox-img');

    galleryImgs.forEach(img => {
      img.addEventListener('click', () => {
        lightboxImg.src = img.src;
        lightbox.style.display = 'flex';
      });
    });

    lightbox.addEventListener('click', () => {
      lightbox.style.display = 'none';
    });

    document.addEventListener('keydown', e => {
      if (e.key === 'Escape') lightbox.style.display = 'none';
    });

    // Fireworks animation
    const canvas = document.querySelector('canvas');
    const ctx = canvas.getContext('2d');
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
    let fireworks = [], gravity = 0.2;
    const rnd = Math.random;

    function vector(x, y) { this.x = x; this.y = y; this.add = v => { this.x += v.x; this.y += v.y; }; }
    function particle(pos, vel) {
      this.pos = new vector(pos.x, pos.y); this.vel = vel;
      this.finish = false; this.start = 0;
      this.update = time => {
        if (time - this.start > 500) this.finish = true;
        if (!this.finish) { this.pos.add(this.vel); this.vel.y += gravity; }
      };
      this.draw = () => !this.finish && drawDot(this.pos.x, this.pos.y, 1);
    }
    function firework(x, y) {
      this.pos = new vector(x, y); this.vel = new vector(0, -rnd() * 10 - 3);
      this.color = `hsl(${rnd() * 360}, 100%, 50%)`; this.size = 4;
      this.finish = false; this.start = 0;
      let exploded = false, particles = [];
      this.update = time => {
        if (this.finish) return;
        if (!exploded) {
          this.pos.add(this.vel); this.vel.y += gravity;
          if (this.vel.y >= 0) {
            exploded = true;
            for (let i = 0; i < 100; i++) {
              let p = new particle(this.pos, new vector((rnd() - 0.5) * 10, (rnd() - 0.5) * 10));
              p.start = time; particles.push(p);
            }
          }
        } else {
          let done = 0;
          particles.forEach(p => { p.update(time); if (p.finish) done++; });
          if (done === particles.length) this.finish = true;
        }
      };
      this.draw = () => {
        ctx.fillStyle = this.color;
        if (!exploded) drawDot(this.pos.x, this.pos.y, this.size);
        else particles.forEach(p => p.draw());
      };
    }

    function drawDot(x, y, size) {
      ctx.beginPath();
      ctx.arc(x, y, size, 0, Math.PI * 2);
      ctx.fill();
    }

    function initFireworks() {
      for (let i = 0; i < 20; i++) {
        let f = new firework(rnd() * canvas.width, canvas.height);
        f.start = performance.now();
        fireworks.push(f);
      }
    }

    function animateFireworks(time) {
      ctx.fillStyle = 'rgba(0,0,0,0.2)';
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      fireworks.forEach((f, i) => {
        if (f.finish) {
          fireworks[i] = new firework(rnd() * canvas.width, canvas.height);
          fireworks[i].start = time;
        }
        f.update(time);
        f.draw();
      });

      requestAnimationFrame(animateFireworks);
    }

    window.addEventListener('resize', () => {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    });

    initFireworks();
    animateFireworks();

  function openEnvelope() {
  const envelope = document.getElementById('envelope');
  const paper = document.getElementById('paper');
  const sound = document.getElementById('click-sound');

  if (!envelope.classList.contains('open')) {
    envelope.classList.add('open');
    sound.currentTime = 0;
    sound.play();

    setTimeout(() => {
      paper.classList.add('show');
    }, 700); // tunggu amplop terbuka dulu
  }
}
  </script>
  <audio id="click-sound" src="sounds/click.mp3" preload="auto"></audio>
</body>
</html>
