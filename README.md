[samurai-frog.html](https://github.com/user-attachments/files/26449072/samurai-frog.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Samurai Frog — Unsheathe</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Zen+Antique+Soft&family=Noto+Sans+JP:wght@300;700&display=swap');

:root {
  --teal: #2a7a7a;
  --salmon: #e8846a;
  --gold: #c8a84b;
  --dark: #0d0f0e;
  --ink: #1a1f1e;
}

* { margin: 0; padding: 0; box-sizing: border-box; }

body {
  background: var(--dark);
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: 'Noto Sans JP', sans-serif;
  overflow: hidden;
  position: relative;
}

/* Paper texture bg */
body::before {
  content: '';
  position: fixed;
  inset: 0;
  background:
    radial-gradient(ellipse 60% 80% at 50% 50%, #1a2a28 0%, #0d0f0e 100%);
  z-index: 0;
}

/* Vertical stripe decoration */
.bg-stripes {
  position: fixed;
  inset: 0;
  background: repeating-linear-gradient(
    90deg,
    transparent 0px,
    transparent 60px,
    rgba(42,122,122,0.03) 60px,
    rgba(42,122,122,0.03) 61px
  );
  z-index: 0;
  pointer-events: none;
}

.stage {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
}

/* Title */
.title-block {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-bottom: 18px;
  opacity: 0;
  animation: fade-in 0.8s 0.2s forwards;
}

.kanji {
  font-family: 'Zen Antique Soft', serif;
  font-size: 28px;
  color: var(--salmon);
  letter-spacing: 8px;
  text-shadow: 0 0 20px rgba(232,132,106,0.4);
}

.title-line {
  width: 1px;
  height: 32px;
  background: linear-gradient(to bottom, transparent, var(--gold), transparent);
}

.en-title {
  font-size: 10px;
  color: var(--gold);
  letter-spacing: 4px;
  text-transform: uppercase;
  line-height: 1.8;
}

/* Frame */
.frame-wrap {
  position: relative;
  width: 360px;
}

.frame-border {
  position: absolute;
  inset: -6px;
  border: 1px solid rgba(200,168,75,0.3);
  pointer-events: none;
  z-index: 20;
}
.frame-border::before {
  content: '';
  position: absolute;
  inset: 4px;
  border: 1px solid rgba(200,168,75,0.12);
}

/* Corner ornaments */
.corner-orn {
  position: absolute;
  width: 20px;
  height: 20px;
  border-color: var(--gold);
  border-style: solid;
  z-index: 21;
}
.corner-orn.tl { top: -6px; left: -6px; border-width: 2px 0 0 2px; }
.corner-orn.tr { top: -6px; right: -6px; border-width: 2px 2px 0 0; }
.corner-orn.bl { bottom: -6px; left: -6px; border-width: 0 0 2px 2px; }
.corner-orn.br { bottom: -6px; right: -6px; border-width: 0 2px 2px 0; }

.frame {
  position: relative;
  overflow: hidden;
  border-radius: 2px;
  box-shadow:
    0 0 60px rgba(42,122,122,0.2),
    0 0 120px rgba(42,122,122,0.08),
    inset 0 0 30px rgba(0,0,0,0.5);
}

.nft-img {
  width: 100%;
  display: block;
  transform-origin: center center;
  transition: transform 0.05s;
}

/* =====================
   SWORD OVERLAY
   ===================== */

/* The sword drawn from the scabbard — starts tucked, flies out */
.sword-layer {
  position: absolute;
  inset: 0;
  pointer-events: none;
  z-index: 15;
}

/* Blade — a long diagonal rectangle */
.blade {
  position: absolute;
  width: 6px;
  height: 220px;
  background: linear-gradient(
    to right,
    rgba(180,200,210,0.2) 0%,
    rgba(230,240,245,0.95) 35%,
    rgba(255,255,255,1) 50%,
    rgba(210,225,235,0.9) 65%,
    rgba(160,185,200,0.3) 100%
  );
  border-radius: 0 0 2px 2px;
  /* Position near the character's hand area — upper right */
  top: 18%;
  left: 52%;
  transform-origin: bottom center;
  /* Starts at 0 length (scabbard position), angled diagonally */
  transform: rotate(-42deg) scaleY(0) translateY(60px);
  opacity: 0;
  filter: drop-shadow(0 0 8px rgba(180,230,255,0.9)) drop-shadow(0 0 20px rgba(120,200,255,0.5));
  transition: none;
}

.blade::before {
  /* Tip */
  content: '';
  position: absolute;
  top: -12px;
  left: 50%;
  transform: translateX(-50%);
  width: 0;
  height: 0;
  border-left: 3px solid transparent;
  border-right: 3px solid transparent;
  border-bottom: 12px solid rgba(230,240,245,0.95);
}

.blade::after {
  /* Edge shimmer */
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(
    170deg,
    rgba(255,255,255,0) 30%,
    rgba(255,255,255,0.8) 50%,
    rgba(255,255,255,0) 70%
  );
  animation: shimmer-idle 3s infinite;
  opacity: 0;
}

@keyframes shimmer-idle {
  0%, 100% { opacity: 0; transform: translateY(-100%); }
  50% { opacity: 1; transform: translateY(100%); }
}

/* Sword drawn state */
.blade.drawn {
  animation: draw-sword 0.35s cubic-bezier(0.2, 0, 0.1, 1) forwards;
}

@keyframes draw-sword {
  0%   { transform: rotate(-42deg) scaleY(0) translateY(60px); opacity: 0; }
  15%  { opacity: 1; }
  60%  { transform: rotate(-42deg) scaleY(1.1) translateY(-10px); }
  80%  { transform: rotate(-55deg) scaleY(1) translateY(0px); }
  100% { transform: rotate(-48deg) scaleY(1) translateY(0px); opacity: 1; }
}

.blade.drawn::after { opacity: 1; }

/* Slash arc effect */
.slash-arc {
  position: absolute;
  top: 10%;
  left: -5%;
  width: 120%;
  height: 100%;
  pointer-events: none;
  opacity: 0;
  z-index: 16;
}

.slash-line {
  position: absolute;
  top: 20%;
  left: 5%;
  width: 88%;
  height: 3px;
  background: linear-gradient(
    to right,
    transparent 0%,
    rgba(180,230,255,0.8) 20%,
    rgba(255,255,255,1) 50%,
    rgba(180,230,255,0.8) 80%,
    transparent 100%
  );
  transform: rotate(-15deg);
  filter: blur(1px);
  box-shadow: 0 0 12px rgba(180,230,255,0.8);
}

.slash-line-2 {
  position: absolute;
  top: 23%;
  left: 8%;
  width: 70%;
  height: 1px;
  background: linear-gradient(
    to right,
    transparent,
    rgba(255,255,255,0.6),
    transparent
  );
  transform: rotate(-15deg);
  filter: blur(0.5px);
}

.slash-arc.active {
  animation: slash-flash 0.4s ease-out forwards;
}

@keyframes slash-flash {
  0%   { opacity: 0; transform: translateX(-20px); }
  20%  { opacity: 1; transform: translateX(0); }
  60%  { opacity: 0.8; }
  100% { opacity: 0; transform: translateX(10px); }
}

/* Impact flash */
.flash {
  position: absolute;
  inset: 0;
  background: white;
  opacity: 0;
  z-index: 17;
  pointer-events: none;
  border-radius: 2px;
}

.flash.active {
  animation: impact-flash 0.3s ease-out forwards;
}

@keyframes impact-flash {
  0%  { opacity: 0.6; }
  100% { opacity: 0; }
}

/* Screen shake */
@keyframes shake {
  0%   { transform: translateX(0) translateY(0); }
  15%  { transform: translateX(-4px) translateY(-2px); }
  30%  { transform: translateX(4px) translateY(2px); }
  45%  { transform: translateX(-3px) translateY(1px); }
  60%  { transform: translateX(2px) translateY(-1px); }
  75%  { transform: translateX(-1px) translateY(1px); }
  100% { transform: translateX(0) translateY(0); }
}

.frame-wrap.shaking {
  animation: shake 0.35s ease-out;
}

/* Idle bob animation on image */
@keyframes idle-bob {
  0%, 100% { transform: translateY(0px); }
  50%       { transform: translateY(-4px); }
}

.nft-img.idle {
  animation: idle-bob 3s ease-in-out infinite;
}

/* Particle sparks */
.sparks {
  position: absolute;
  inset: 0;
  pointer-events: none;
  z-index: 18;
}

.spark {
  position: absolute;
  width: 3px;
  height: 3px;
  background: white;
  border-radius: 50%;
  opacity: 0;
}

/* =====================
   BOTTOM UI
   ===================== */
.ui-block {
  margin-top: 20px;
  width: 360px;
  display: flex;
  flex-direction: column;
  gap: 10px;
  opacity: 0;
  animation: fade-in 0.8s 0.5s forwards;
}

.ui-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
}

.nft-id {
  font-family: 'Zen Antique Soft', serif;
  font-size: 13px;
  color: var(--gold);
  letter-spacing: 3px;
}

.status-pill {
  font-size: 9px;
  letter-spacing: 2px;
  padding: 4px 10px;
  border: 1px solid rgba(42,122,122,0.5);
  color: var(--teal);
  text-transform: uppercase;
  border-radius: 1px;
  transition: all 0.3s;
}

.status-pill.alert {
  border-color: var(--salmon);
  color: var(--salmon);
  box-shadow: 0 0 12px rgba(232,132,106,0.3);
}

/* THE BUTTON */
.draw-btn {
  width: 100%;
  background: transparent;
  border: 1px solid var(--gold);
  color: var(--gold);
  font-family: 'Zen Antique Soft', serif;
  font-size: 15px;
  letter-spacing: 6px;
  padding: 14px 0;
  cursor: pointer;
  position: relative;
  overflow: hidden;
  text-transform: uppercase;
  transition: color 0.3s;
}

.draw-btn::before {
  content: '';
  position: absolute;
  inset: 0;
  background: var(--gold);
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  z-index: -1;
}

.draw-btn:hover::before { transform: scaleX(1); }
.draw-btn:hover { color: var(--dark); }

.draw-btn:disabled {
  opacity: 0.4;
  cursor: default;
}
.draw-btn:disabled::before { transform: scaleX(0); }
.draw-btn:disabled { color: var(--gold); }

/* Sheathe btn */
.sheathe-btn {
  width: 100%;
  background: transparent;
  border: 1px solid rgba(42,122,122,0.5);
  color: rgba(42,122,122,0.7);
  font-family: 'Zen Antique Soft', serif;
  font-size: 12px;
  letter-spacing: 4px;
  padding: 10px 0;
  cursor: pointer;
  text-transform: uppercase;
  transition: all 0.3s;
  display: none;
}
.sheathe-btn:hover {
  border-color: var(--teal);
  color: var(--teal);
}
.sheathe-btn.visible { display: block; }

/* Divider */
.divider {
  width: 100%;
  height: 1px;
  background: linear-gradient(to right, transparent, rgba(200,168,75,0.2), transparent);
}

@keyframes fade-in {
  from { opacity: 0; transform: translateY(8px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* Katana stats bar */
.stats-bar {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 8px 0;
}

.stat {
  display: flex;
  flex-direction: column;
  gap: 3px;
  flex: 1;
}

.stat-label {
  font-size: 8px;
  color: rgba(200,168,75,0.5);
  letter-spacing: 2px;
  text-transform: uppercase;
}

.stat-bar-wrap {
  height: 2px;
  background: rgba(255,255,255,0.06);
  border-radius: 1px;
  overflow: hidden;
}

.stat-bar-fill {
  height: 100%;
  background: var(--teal);
  border-radius: 1px;
  width: 0;
  transition: width 0.8s cubic-bezier(0.4, 0, 0.2, 1);
}

.stat-bar-fill.gold { background: var(--gold); }
.stat-bar-fill.salmon { background: var(--salmon); }

.stat-val {
  font-size: 9px;
  color: rgba(200,168,75,0.7);
  letter-spacing: 1px;
  align-self: flex-end;
}
</style>
</head>
<body>
<div class="bg-stripes"></div>

<div class="stage">

  <!-- Title -->
  <div class="title-block">
    <span class="kanji">武士蛙</span>
    <div class="title-line"></div>
    <div class="en-title">
      SAMURAI FROG<br>
      <span style="color:rgba(200,168,75,0.5); font-size:8px;">WARRIOR CLASS · NFT</span>
    </div>
  </div>

  <!-- Card Frame -->
  <div class="frame-wrap" id="frameWrap">
    <div class="frame-border"></div>
    <div class="corner-orn tl"></div>
    <div class="corner-orn tr"></div>
    <div class="corner-orn bl"></div>
    <div class="corner-orn br"></div>

    <div class="frame" id="frame">
      <img class="nft-img idle" id="nftImg" src="1000083445.jpg" alt="Samurai Frog NFT">

      <!-- Sword overlay -->
      <div class="sword-layer" id="swordLayer">
        <div class="blade" id="blade"></div>
        <div class="slash-arc" id="slashArc">
          <div class="slash-line"></div>
          <div class="slash-line-2"></div>
        </div>
        <div class="sparks" id="sparks"></div>
        <div class="flash" id="flash"></div>
      </div>
    </div>
  </div>

  <!-- UI -->
  <div class="ui-block">
    <div class="ui-row">
      <span class="nft-id">KATANA · #4471</span>
      <span class="status-pill" id="statusPill">SHEATHED</span>
    </div>

    <div class="stats-bar" id="statsBar">
      <div class="stat">
        <span class="stat-label">ATK</span>
        <div class="stat-bar-wrap">
          <div class="stat-bar-fill salmon" id="atkBar"></div>
        </div>
      </div>
      <div class="stat">
        <span class="stat-label">SPD</span>
        <div class="stat-bar-wrap">
          <div class="stat-bar-fill gold" id="spdBar"></div>
        </div>
      </div>
      <div class="stat">
        <span class="stat-label">HONOR</span>
        <div class="stat-bar-wrap">
          <div class="stat-bar-fill" id="honBar"></div>
        </div>
      </div>
    </div>

    <div class="divider"></div>

    <button class="draw-btn" id="drawBtn" onclick="drawSword()">⚔ DRAW SWORD</button>
    <button class="sheathe-btn" id="sheatheBtn" onclick="sheatheSword()">↩ SHEATHE</button>
  </div>

</div>

<script>
  let drawn = false;
  let busy = false;

  function spawnSparks() {
    const sparks = document.getElementById('sparks');
    sparks.innerHTML = '';
    // Sparks around hand/sword area
    const positions = [
      { x: 55, y: 35 }, { x: 62, y: 28 }, { x: 48, y: 32 },
      { x: 70, y: 40 }, { x: 44, y: 44 }, { x: 66, y: 22 },
      { x: 58, y: 50 }, { x: 75, y: 33 }, { x: 40, y: 38 },
    ];

    positions.forEach((p, i) => {
      const el = document.createElement('div');
      el.className = 'spark';
      el.style.cssText = `
        left: ${p.x}%;
        top: ${p.y}%;
        background: ${i % 3 === 0 ? '#e8c97a' : i % 3 === 1 ? '#fff' : '#7adcff'};
        width: ${2 + Math.random() * 3}px;
        height: ${2 + Math.random() * 3}px;
        animation: spark-fly-${i} 0.5s ${i * 0.03}s ease-out forwards;
      `;
      sparks.appendChild(el);

      // Animate with JS since keyframes are dynamic
      const dx = (Math.random() - 0.5) * 60;
      const dy = (Math.random() - 0.7) * 60;
      el.animate([
        { opacity: 1, transform: `translate(0,0) scale(1)` },
        { opacity: 0, transform: `translate(${dx}px, ${dy}px) scale(0)` }
      ], { duration: 500, delay: i * 30, fill: 'forwards', easing: 'ease-out' });
    });
  }

  function drawSword() {
    if (busy || drawn) return;
    busy = true;

    const blade    = document.getElementById('blade');
    const slashArc = document.getElementById('slashArc');
    const flash    = document.getElementById('flash');
    const frameWrap = document.getElementById('frameWrap');
    const drawBtn  = document.getElementById('drawBtn');
    const sheatheBtn = document.getElementById('sheatheBtn');
    const statusPill = document.getElementById('statusPill');

    drawBtn.disabled = true;

    // 1. Draw the blade
    blade.classList.add('drawn');

    // 2. Slash arc + flash 250ms later
    setTimeout(() => {
      slashArc.classList.add('active');
      flash.classList.add('active');
      frameWrap.classList.add('shaking');
      spawnSparks();
      statusPill.textContent = 'DRAWN';
      statusPill.classList.add('alert');

      // Animate stats
      document.getElementById('atkBar').style.width = '88%';
      document.getElementById('spdBar').style.width = '94%';
      document.getElementById('honBar').style.width = '72%';
    }, 250);

    // 3. Cleanup
    setTimeout(() => {
      slashArc.classList.remove('active');
      flash.classList.remove('active');
      frameWrap.classList.remove('shaking');
      drawn = true;
      busy = false;
      sheatheBtn.classList.add('visible');
    }, 700);
  }

  function sheatheSword() {
    if (busy || !drawn) return;
    busy = true;

    const blade      = document.getElementById('blade');
    const drawBtn    = document.getElementById('drawBtn');
    const sheatheBtn = document.getElementById('sheatheBtn');
    const statusPill = document.getElementById('statusPill');

    // Reverse: slide blade back
    blade.style.transition = 'transform 0.4s cubic-bezier(0.4,0,1,1), opacity 0.4s';
    blade.style.transform  = 'rotate(-42deg) scaleY(0) translateY(60px)';
    blade.style.opacity    = '0';

    statusPill.textContent = 'SHEATHED';
    statusPill.classList.remove('alert');

    document.getElementById('atkBar').style.width = '0';
    document.getElementById('spdBar').style.width = '0';
    document.getElementById('honBar').style.width = '0';

    setTimeout(() => {
      blade.classList.remove('drawn');
      blade.style.transition = '';
      blade.style.transform  = '';
      blade.style.opacity    = '';
      drawn = false;
      busy = false;
      drawBtn.disabled = false;
      sheatheBtn.classList.remove('visible');
    }, 500);
  }
</script>
</body>
</html>
