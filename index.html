<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ping Pong</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&display=swap');
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body {
    background: #050a14;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    font-family: 'Orbitron', monospace;
    overflow: hidden;
    user-select: none;
  }
  #gameWrapper { position: relative; display: flex; flex-direction: column; align-items: center; gap: 12px; }
  canvas { display: block; border-radius: 12px; border: 1px solid rgba(255,255,255,0.07); }
  #controls { font-size: 10px; letter-spacing: 2px; color: rgba(255,255,255,0.22); text-align: center; font-family: 'Orbitron', monospace; }
  #overlay {
    position: absolute; inset: 0;
    display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 20px;
    pointer-events: none;
  }
  #overlayTitle { font-size: clamp(24px,5vw,40px); font-weight: 900; color: #fff; letter-spacing: 5px; text-align: center; text-shadow: 0 0 30px rgba(0,229,255,0.7); font-family: 'Orbitron', monospace; }
  #overlaySubtitle { font-size: 12px; letter-spacing: 3px; color: rgba(255,255,255,0.45); text-align: center; font-family: 'Orbitron', monospace; }
  #startBtn { font-family: 'Orbitron', monospace; font-size: 12px; font-weight: 700; letter-spacing: 3px; padding: 13px 38px; background: transparent; border: 1.5px solid rgba(0,229,255,0.65); color: #00e5ff; border-radius: 6px; cursor: pointer; pointer-events: all; text-transform: uppercase; transition: background 0.2s, box-shadow 0.2s; }
  #startBtn:hover { background: rgba(0,229,255,0.12); box-shadow: 0 0 22px rgba(0,229,255,0.35); }
  #difficultyRow { display: flex; gap: 10px; pointer-events: all; }
  .diff-btn { font-family: 'Orbitron', monospace; font-size: 10px; font-weight: 700; letter-spacing: 2px; padding: 8px 16px; background: transparent; border: 1px solid rgba(255,255,255,0.18); color: rgba(255,255,255,0.35); border-radius: 4px; cursor: pointer; text-transform: uppercase; transition: all 0.2s; }
  .diff-btn.active { border-color: rgba(0,229,255,0.65); color: #00e5ff; background: rgba(0,229,255,0.09); }
</style>
</head>
<body>
<div id="gameWrapper">
  <div style="position:relative;">
    <canvas id="c"></canvas>
    <div id="overlay">
      <div id="overlayTitle">PING PONG</div>
      <div id="overlaySubtitle">Select difficulty</div>
      <div id="difficultyRow">
        <button class="diff-btn" data-d="easy">Easy</button>
        <button class="diff-btn active" data-d="medium">Medium</button>
        <button class="diff-btn" data-d="hard">Hard</button>
      </div>
      <button id="startBtn">Play</button>
    </div>
  </div>
  <div id="controls">W / S &nbsp;|&nbsp; Mouse / Touch &nbsp;— move paddle &nbsp;|&nbsp; First to 7 wins</div>
</div>
<script>
const canvas = document.getElementById('c');
const ctx = canvas.getContext('2d');
const overlay = document.getElementById('overlay');
const overlayTitle = document.getElementById('overlayTitle');
const overlaySubtitle = document.getElementById('overlaySubtitle');
const startBtn = document.getElementById('startBtn');

const W = Math.min(window.innerWidth - 32, 820);
const H = Math.round(W * 0.575);
canvas.width = W; canvas.height = H;

const PAD_W = Math.round(W * 0.016);
const PAD_H = Math.round(H * 0.17);
const PAD_MARGIN = Math.round(W * 0.024);
const BALL_R = Math.round(W * 0.014);
const NET_DASH = Math.round(H * 0.032);
const SZ = Math.round(H * 0.175); // score zone height
const WIN_SCORE = 7;
const GAME_TOP = SZ;

let state = 'menu';
let playerY, aiY, ball, pScore = 0, aScore = 0, particles = [], trail = [];
let pScoreAnim = 1, aScoreAnim = 1;
let keys = {}, mouseY = H/2, mouseActive = false;
let currentDiff = 'medium';

const DIFF = {
  easy:   { speed: W*0.0055, ai: 0.055, aiMaxSpd: H*0.0075, aiErr: H*0.15,  aiPredict: false },
  medium: { speed: W*0.0068, ai: 0.085, aiMaxSpd: H*0.013,  aiErr: H*0.055, aiPredict: true  },
  hard:   { speed: W*0.0082, ai: 0.14,  aiMaxSpd: H*0.019,  aiErr: H*0.016, aiPredict: true  }
};

document.querySelectorAll('.diff-btn').forEach(btn => {
  btn.addEventListener('click', () => {
    document.querySelectorAll('.diff-btn').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    currentDiff = btn.dataset.d;
  });
});

// Audio
const AudioCtx = window.AudioContext || window.webkitAudioContext;
let audioCtx;
function beep(freq, dur, vol, type) {
  if (!audioCtx) { try { audioCtx = new AudioCtx(); } catch(e){return;} }
  const o = audioCtx.createOscillator(), g = audioCtx.createGain();
  o.connect(g); g.connect(audioCtx.destination);
  o.type = type; o.frequency.value = freq;
  g.gain.setValueAtTime(vol, audioCtx.currentTime);
  g.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + dur);
  o.start(); o.stop(audioCtx.currentTime + dur);
}
const sndPaddle = () => beep(460, 0.07, 0.22, 'square');
const sndWall   = () => beep(260, 0.05, 0.15, 'sine');
const sndScore  = (w) => beep(w ? 680 : 190, 0.38, 0.28, w ? 'sine' : 'sawtooth');

// Predict Y where ball will reach targetX (accounts for wall bounces)
function predictBallY(bx, by, vx, vy, targetX) {
  if (Math.abs(vx) < 0.001) return by;
  let x = bx, y = by, dx = vx, dy = vy;
  const steps = Math.ceil(Math.abs(targetX - x) / Math.abs(dx));
  const maxSteps = Math.min(steps, 600);
  for (let i = 0; i < maxSteps; i++) {
    x += dx; y += dy;
    if (y - BALL_R < GAME_TOP) { y = GAME_TOP + BALL_R; dy = Math.abs(dy); }
    if (y + BALL_R > H)        { y = H - BALL_R;         dy = -Math.abs(dy); }
  }
  return y;
}

function initGame() {
  playerY = GAME_TOP + (H - GAME_TOP) / 2;
  aiY     = GAME_TOP + (H - GAME_TOP) / 2;
  pScore = 0; aScore = 0;
  particles = []; trail = [];
  pScoreAnim = 1; aScoreAnim = 1;
  spawnBall();
}

function spawnBall() {
  const d = DIFF[currentDiff];
  const angle = (Math.random() * 0.5 + 0.2) * (Math.random() < 0.5 ? 1 : -1);
  ball = {
    x: W/2, y: GAME_TOP + (H - GAME_TOP)/2,
    vx: d.speed * Math.cos(angle) * (Math.random() < 0.5 ? 1 : -1),
    vy: d.speed * Math.sin(angle),
    speed: d.speed, _err: 0, _errSet: false
  };
  trail = [];
}

function burst(x, y, color, n) {
  n = n || 16;
  for (let i = 0; i < n; i++) {
    const a = Math.random()*Math.PI*2, s = Math.random()*4.5+1.2;
    particles.push({ x, y, vx: Math.cos(a)*s, vy: Math.sin(a)*s, life: 1, decay: 0.035+Math.random()*0.04, r: Math.random()*4+2, color });
  }
}

let aiTargetY = H/2;
const TRAIL_LEN = 20;

function update() {
  if (state !== 'playing') return;
  const d = DIFF[currentDiff];

  // Player movement
  const padSpd = H * 0.013;
  if (keys['w'] || keys['W'] || keys['ArrowUp'])   playerY -= padSpd;
  if (keys['s'] || keys['S'] || keys['ArrowDown']) playerY += padSpd;
  if (mouseActive) playerY += (mouseY - playerY) * 0.24;
  playerY = Math.max(GAME_TOP + PAD_H/2, Math.min(H - PAD_H/2, playerY));

  // AI logic
  const aiRight = W - PAD_MARGIN - PAD_W;
  if (ball.vx > 0) {
    if (!ball._errSet) {
      if (d.aiPredict) {
        aiTargetY = predictBallY(ball.x, ball.y, ball.vx, ball.vy, aiRight);
      } else {
        aiTargetY = ball.y;
      }
      ball._err = (Math.random() - 0.5) * d.aiErr;
      ball._errSet = true;
    }
  } else {
    aiTargetY = GAME_TOP + (H - GAME_TOP) / 2;
    ball._errSet = false; ball._err = 0;
  }
  const desired = aiTargetY + ball._err;
  const diff = desired - aiY;
  aiY += Math.sign(diff) * Math.min(Math.abs(diff) * d.ai + 0.4, d.aiMaxSpd);
  aiY = Math.max(GAME_TOP + PAD_H/2, Math.min(H - PAD_H/2, aiY));

  // Trail
  trail.push({ x: ball.x, y: ball.y });
  if (trail.length > TRAIL_LEN) trail.shift();

  // Ball
  ball.x += ball.vx; ball.y += ball.vy;

  if (ball.y - BALL_R < GAME_TOP) { ball.y = GAME_TOP + BALL_R; ball.vy = Math.abs(ball.vy);  sndWall(); }
  if (ball.y + BALL_R > H)        { ball.y = H - BALL_R;         ball.vy = -Math.abs(ball.vy); sndWall(); }

  const pLeft = PAD_MARGIN + PAD_W;
  if (ball.vx < 0 && ball.x - BALL_R < pLeft && ball.x > PAD_MARGIN &&
      ball.y > playerY - PAD_H/2 - BALL_R && ball.y < playerY + PAD_H/2 + BALL_R) {
    ball.vx = Math.abs(ball.vx) * 1.045;
    ball.vy = ((ball.y - playerY)/(PAD_H/2)) * ball.speed * 1.25;
    ball.speed = Math.min(ball.speed * 1.045, W * 0.015);
    ball.x = pLeft + BALL_R + 1;
    ball._errSet = false;
    sndPaddle(); burst(ball.x, ball.y, '#00e5ff');
  }

  if (ball.vx > 0 && ball.x + BALL_R > aiRight && ball.x < W - PAD_MARGIN &&
      ball.y > aiY - PAD_H/2 - BALL_R && ball.y < aiY + PAD_H/2 + BALL_R) {
    ball.vx = -Math.abs(ball.vx) * 1.045;
    ball.vy = ((ball.y - aiY)/(PAD_H/2)) * ball.speed * 1.25;
    ball.speed = Math.min(ball.speed * 1.045, W * 0.015);
    ball.x = aiRight - BALL_R - 1;
    sndPaddle(); burst(ball.x, ball.y, '#ff4081');
  }

  if (ball.x < -BALL_R) {
    pScore++; pScoreAnim = 1.5;
    sndScore(true); burst(8, ball.y, '#00e5ff', 22);
    if (!checkWin()) resetRound();
  } else if (ball.x > W + BALL_R) {
    aScore++; aScoreAnim = 1.5;
    sndScore(false); burst(W - 8, ball.y, '#ff4081', 22);
    if (!checkWin()) resetRound();
  }

  pScoreAnim += (1 - pScoreAnim) * 0.16;
  aScoreAnim += (1 - aScoreAnim) * 0.16;

  for (let i = particles.length - 1; i >= 0; i--) {
    const p = particles[i];
    p.x += p.vx; p.y += p.vy; p.vy += 0.1; p.life -= p.decay;
    if (p.life <= 0) particles.splice(i, 1);
  }
}

function checkWin() {
  if (pScore >= WIN_SCORE || aScore >= WIN_SCORE) {
    state = 'gameover';
    const won = pScore >= WIN_SCORE;
    overlayTitle.textContent = won ? 'YOU WIN!' : 'CPU WINS';
    overlayTitle.style.color = won ? '#00e5ff' : '#ff4081';
    overlayTitle.style.textShadow = won ? '0 0 32px rgba(0,229,255,0.8)' : '0 0 32px rgba(255,64,129,0.8)';
    overlaySubtitle.textContent = 'Score: ' + pScore + ' — ' + aScore;
    startBtn.textContent = 'Play Again';
    document.getElementById('difficultyRow').style.display = 'flex';
    overlay.style.display = 'flex';
    return true;
  }
  return false;
}

function resetRound() {
  ball.x = -300;
  setTimeout(() => { if (state === 'playing') spawnBall(); }, 750);
}

// Input
canvas.addEventListener('mousemove', e => {
  const r = canvas.getBoundingClientRect();
  mouseY = (e.clientY - r.top) * (H / r.height);
  mouseActive = true;
});
canvas.addEventListener('mouseleave', () => { mouseActive = false; });
canvas.addEventListener('touchmove', e => {
  e.preventDefault();
  const r = canvas.getBoundingClientRect();
  mouseY = (e.touches[0].clientY - r.top) * (H / r.height);
  mouseActive = true;
}, { passive: false });
document.addEventListener('keydown', e => {
  keys[e.key] = true;
  if (['ArrowUp','ArrowDown',' '].includes(e.key)) e.preventDefault();
});
document.addEventListener('keyup', e => { keys[e.key] = false; });

// Draw helpers
function rRect(x, y, w, h, r) {
  ctx.beginPath();
  ctx.moveTo(x+r,y); ctx.arcTo(x+w,y,x+w,y+h,r); ctx.arcTo(x+w,y+h,x,y+h,r);
  ctx.arcTo(x,y+h,x,y,r); ctx.arcTo(x,y,x+w,y,r); ctx.closePath();
}

function draw() {
  ctx.fillStyle = '#070d1a';
  ctx.fillRect(0, 0, W, H);

  // Score zone
  ctx.fillStyle = 'rgba(0,0,0,0.32)';
  ctx.fillRect(0, 0, W, SZ);
  ctx.strokeStyle = 'rgba(255,255,255,0.08)';
  ctx.lineWidth = 1;
  ctx.beginPath(); ctx.moveTo(0, SZ); ctx.lineTo(W, SZ); ctx.stroke();

  // Scores on canvas
  const midX = W / 2;
  const sz = Math.round(SZ * 0.58);
  ctx.font = '900 ' + sz + 'px Orbitron, monospace';
  ctx.textBaseline = 'middle';

  ctx.save();
  ctx.translate(midX * 0.46, SZ * 0.54);
  ctx.scale(pScoreAnim, pScoreAnim);
  ctx.textAlign = 'center';
  ctx.fillStyle = '#00e5ff';
  ctx.shadowBlur = 20; ctx.shadowColor = '#00e5ff';
  ctx.fillText(pScore, 0, 0);
  ctx.restore();

  ctx.save();
  ctx.translate(midX + midX * 0.46, SZ * 0.54);
  ctx.scale(aScoreAnim, aScoreAnim);
  ctx.textAlign = 'center';
  ctx.fillStyle = '#ff4081';
  ctx.shadowBlur = 20; ctx.shadowColor = '#ff4081';
  ctx.fillText(aScore, 0, 0);
  ctx.restore();

  ctx.shadowBlur = 0;
  const lsz = Math.round(SZ * 0.2);
  ctx.font = '700 ' + lsz + 'px Orbitron, monospace';
  ctx.textBaseline = 'top';
  ctx.textAlign = 'center';
  ctx.fillStyle = 'rgba(255,255,255,0.25)';
  ctx.fillText('YOU', midX * 0.46, SZ * 0.06);
  ctx.fillText('CPU', midX + midX * 0.46, SZ * 0.06);

  ctx.font = '400 ' + Math.round(SZ*0.38) + 'px Orbitron, monospace';
  ctx.textBaseline = 'middle';
  ctx.fillStyle = 'rgba(255,255,255,0.13)';
  ctx.fillText(':', midX, SZ * 0.54);

  // Court tints
  const g1 = ctx.createLinearGradient(0, GAME_TOP, 0, H);
  g1.addColorStop(0,'rgba(0,229,255,0.045)'); g1.addColorStop(1,'rgba(0,229,255,0)');
  ctx.fillStyle = g1; ctx.fillRect(0, GAME_TOP, midX-1, H-GAME_TOP);
  const g2 = ctx.createLinearGradient(0, GAME_TOP, 0, H);
  g2.addColorStop(0,'rgba(255,64,129,0.045)'); g2.addColorStop(1,'rgba(255,64,129,0)');
  ctx.fillStyle = g2; ctx.fillRect(midX+1, GAME_TOP, midX, H-GAME_TOP);

  // Net
  ctx.save();
  ctx.strokeStyle = 'rgba(255,255,255,0.07)';
  ctx.lineWidth = 1.5;
  ctx.setLineDash([NET_DASH, NET_DASH*0.75]);
  ctx.beginPath(); ctx.moveTo(midX, GAME_TOP); ctx.lineTo(midX, H); ctx.stroke();
  ctx.setLineDash([]);
  ctx.restore();

  if (state !== 'playing') return;

  // Trail
  for (let i = 0; i < trail.length; i++) {
    const t = trail[i], frac = i / trail.length;
    ctx.beginPath();
    ctx.arc(t.x, t.y, Math.max(BALL_R * frac * 0.85, 1), 0, Math.PI*2);
    ctx.fillStyle = 'rgba(0,229,255,' + (frac*0.42) + ')';
    ctx.fill();
  }

  // Ball
  ctx.save();
  ctx.shadowBlur = 26; ctx.shadowColor = '#00e5ff';
  ctx.beginPath(); ctx.arc(ball.x, ball.y, BALL_R, 0, Math.PI*2);
  ctx.fillStyle = '#fff'; ctx.fill();
  ctx.restore();

  // Player paddle
  ctx.save();
  ctx.shadowBlur = 22; ctx.shadowColor = '#00e5ff';
  ctx.fillStyle = '#00e5ff';
  rRect(PAD_MARGIN, playerY - PAD_H/2, PAD_W, PAD_H, PAD_W/2);
  ctx.fill();
  ctx.restore();

  // AI paddle
  ctx.save();
  ctx.shadowBlur = 22; ctx.shadowColor = '#ff4081';
  ctx.fillStyle = '#ff4081';
  rRect(W - PAD_MARGIN - PAD_W, aiY - PAD_H/2, PAD_W, PAD_H, PAD_W/2);
  ctx.fill();
  ctx.restore();

  // Particles
  for (const p of particles) {
    ctx.save();
    ctx.globalAlpha = Math.max(p.life, 0);
    ctx.beginPath(); ctx.arc(p.x, p.y, p.r * p.life, 0, Math.PI*2);
    ctx.fillStyle = p.color; ctx.fill();
    ctx.restore();
  }
}

function loop() { update(); draw(); requestAnimationFrame(loop); }

startBtn.addEventListener('click', () => {
  if (!audioCtx) { try { audioCtx = new AudioCtx(); } catch(e){} }
  initGame(); state = 'playing';
  overlay.style.display = 'none';
  overlayTitle.style.color = '#fff';
  overlayTitle.style.textShadow = '0 0 30px rgba(0,229,255,0.7)';
  overlayTitle.textContent = 'PING PONG';
  overlaySubtitle.textContent = 'Select difficulty';
  startBtn.textContent = 'Play';
});

loop();
</script>
</body>
</html>
