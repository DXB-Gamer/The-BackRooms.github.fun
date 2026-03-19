<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>THE BACKROOMS</title>
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body { background: #000; overflow: hidden; font-family: 'Courier New', monospace; }

  #canvas { display: block; width: 100vw; height: 100vh; }

  #overlay {
    position: fixed; inset: 0;
    display: flex; flex-direction: column;
    align-items: center; justify-content: center;
    background: #000;
    z-index: 100;
    transition: opacity 1.5s ease;
  }
  #overlay.hidden { opacity: 0; pointer-events: none; }

  #overlay h1 {
    color: #c8a84b;
    font-size: clamp(2rem, 8vw, 6rem);
    letter-spacing: 0.3em;
    text-transform: uppercase;
    text-align: center;
    text-shadow: 0 0 40px #c8a84b88, 0 0 80px #c8a84b44;
    animation: flicker 3s infinite;
    margin-bottom: 1rem;
  }
  #overlay p {
    color: #888;
    font-size: clamp(0.7rem, 2vw, 1rem);
    letter-spacing: 0.2em;
    margin-bottom: 3rem;
    text-align: center;
    max-width: 500px;
    line-height: 1.8;
  }
  #startBtn {
    background: none;
    border: 1px solid #c8a84b88;
    color: #c8a84b;
    padding: 1rem 3rem;
    font-family: 'Courier New', monospace;
    font-size: 1rem;
    letter-spacing: 0.3em;
    cursor: pointer;
    transition: all 0.3s;
    text-transform: uppercase;
  }
  #startBtn:hover {
    background: #c8a84b11;
    border-color: #c8a84b;
    box-shadow: 0 0 20px #c8a84b44;
  }

  #hud {
    position: fixed;
    bottom: 2rem; left: 50%;
    transform: translateX(-50%);
    color: #c8a84b88;
    font-size: 0.7rem;
    letter-spacing: 0.2em;
    text-align: center;
    pointer-events: none;
    z-index: 10;
    opacity: 0;
    transition: opacity 1s;
  }
  #hud.visible { opacity: 1; }

  #sanity {
    position: fixed;
    top: 2rem; right: 2rem;
    color: #c8a84b66;
    font-size: 0.65rem;
    letter-spacing: 0.15em;
    z-index: 10;
    opacity: 0;
    transition: opacity 1s;
  }
  #sanity.visible { opacity: 1; }

  #vignette {
    position: fixed; inset: 0;
    background: radial-gradient(ellipse at center, transparent 40%, #00000099 100%);
    pointer-events: none;
    z-index: 5;
  }

  #noise {
    position: fixed; inset: 0;
    pointer-events: none;
    z-index: 6;
    opacity: 0.03;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)'/%3E%3C/svg%3E");
  }

  #flicker-overlay {
    position: fixed; inset: 0;
    background: #d4a843;
    pointer-events: none;
    z-index: 7;
    opacity: 0;
  }

  @keyframes flicker {
    0%, 95%, 100% { opacity: 1; }
    96% { opacity: 0.4; }
    97% { opacity: 1; }
    98% { opacity: 0.2; }
    99% { opacity: 0.9; }
  }

  #found-text {
    position: fixed; inset: 0;
    display: flex; align-items: center; justify-content: center;
    color: #c8a84b;
    font-size: clamp(1rem, 4vw, 2.5rem);
    letter-spacing: 0.4em;
    opacity: 0;
    pointer-events: none;
    z-index: 20;
    text-align: center;
    padding: 2rem;
    transition: opacity 2s;
    text-shadow: 0 0 30px #c8a84b;
  }

  #crosshair {
    position: fixed;
    top: 50%; left: 50%;
    transform: translate(-50%, -50%);
    width: 16px; height: 16px;
    pointer-events: none;
    z-index: 10;
    opacity: 0;
    transition: opacity 1s;
  }
  #crosshair.visible { opacity: 0.4; }
  #crosshair::before, #crosshair::after {
    content: '';
    position: absolute;
    background: #c8a84b;
  }
  #crosshair::before { width: 100%; height: 1px; top: 50%; left: 0; transform: translateY(-50%); }
  #crosshair::after { width: 1px; height: 100%; left: 50%; top: 0; transform: translateX(-50%); }
</style>
</head>
<body>

<canvas id="canvas"></canvas>
<div id="vignette"></div>
<div id="noise"></div>
<div id="flicker-overlay"></div>

<div id="overlay">
  <h1>THE BACKROOMS</h1>
  <p>You have noclipped out of reality.<br>There is no exit. There is no map.<br>There is only the hum.</p>
  <button id="startBtn">ENTER</button>
</div>

<div id="hud">WASD / ARROW KEYS — MOVE &nbsp;|&nbsp; MOUSE — LOOK</div>
<div id="sanity">SANITY: <span id="sanityVal">100</span>%</div>
<div id="crosshair"></div>
<div id="found-text" id="found-text"></div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
// ─── SCENE SETUP ─────────────────────────────────────────────────────────────
const canvas = document.getElementById('canvas');
const renderer = new THREE.WebGLRenderer({ canvas, antialias: false });
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.setPixelRatio(Math.min(window.devicePixelRatio, 1.5));
renderer.shadowMap.enabled = false;
renderer.fog = new THREE.Fog(0x1a1206, 2, 28);

const scene = new THREE.Scene();
scene.background = new THREE.Color(0x1a1206);
scene.fog = renderer.fog;

const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 60);
camera.position.set(0, 1.65, 0);

// ─── TEXTURES ─────────────────────────────────────────────────────────────────
function makeWallpaper() {
  const size = 512;
  const c = document.createElement('canvas');
  c.width = c.height = size;
  const ctx = c.getContext('2d');

  // Base yellow-beige
  ctx.fillStyle = '#c8a84b';
  ctx.fillRect(0, 0, size, size);

  // Noise / variation
  for (let i = 0; i < 8000; i++) {
    const x = Math.random() * size;
    const y = Math.random() * size;
    const r = Math.random() * 3;
    const alpha = Math.random() * 0.08;
    ctx.fillStyle = `rgba(${Math.random()>0.5?180:80},${Math.random()>0.5?140:90},20,${alpha})`;
    ctx.fillRect(x, y, r, r);
  }

  // Vertical lines (wallpaper pattern)
  ctx.strokeStyle = 'rgba(160,120,20,0.15)';
  ctx.lineWidth = 1;
  for (let x = 0; x < size; x += 32) {
    ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, size); ctx.stroke();
  }
  // Horizontal lines
  for (let y = 0; y < size; y += 32) {
    ctx.beginPath(); ctx.moveTo(0, y); ctx.lineTo(size, y); ctx.stroke();
  }

  // Stain patches
  for (let i = 0; i < 12; i++) {
    const gx = Math.random() * size, gy = Math.random() * size;
    const gr = ctx.createRadialGradient(gx, gy, 0, gx, gy, 40 + Math.random() * 60);
    gr.addColorStop(0, 'rgba(80,60,10,0.12)');
    gr.addColorStop(1, 'rgba(80,60,10,0)');
    ctx.fillStyle = gr;
    ctx.fillRect(0, 0, size, size);
  }

  const t = new THREE.CanvasTexture(c);
  t.wrapS = t.wrapT = THREE.RepeatWrapping;
  t.repeat.set(4, 2);
  return t;
}

function makeCarpet() {
  const size = 256;
  const c = document.createElement('canvas');
  c.width = c.height = size;
  const ctx = c.getContext('2d');
  ctx.fillStyle = '#4a3820';
  ctx.fillRect(0, 0, size, size);
  for (let i = 0; i < 5000; i++) {
    const x = Math.random() * size, y = Math.random() * size;
    ctx.fillStyle = `rgba(${60+Math.random()*40},${40+Math.random()*30},${10+Math.random()*20},0.5)`;
    ctx.fillRect(x, y, 1, 1);
  }
  // Grid pattern
  ctx.strokeStyle = 'rgba(100,70,30,0.3)';
  ctx.lineWidth = 1;
  for (let x = 0; x < size; x += 16) { ctx.beginPath(); ctx.moveTo(x,0); ctx.lineTo(x,size); ctx.stroke(); }
  for (let y = 0; y < size; y += 16) { ctx.beginPath(); ctx.moveTo(0,y); ctx.lineTo(size,y); ctx.stroke(); }
  const t = new THREE.CanvasTexture(c);
  t.wrapS = t.wrapT = THREE.RepeatWrapping;
  t.repeat.set(8, 8);
  return t;
}

function makeCeiling() {
  const size = 256;
  const c = document.createElement('canvas');
  c.width = c.height = size;
  const ctx = c.getContext('2d');
  ctx.fillStyle = '#d4c89a';
  ctx.fillRect(0, 0, size, size);
  for (let i = 0; i < 3000; i++) {
    const x = Math.random()*size, y = Math.random()*size;
    ctx.fillStyle = `rgba(180,160,100,${Math.random()*0.1})`;
    ctx.fillRect(x,y,Math.random()*3,Math.random()*3);
  }
  // Ceiling tiles
  ctx.strokeStyle = 'rgba(150,130,80,0.4)';
  ctx.lineWidth = 2;
  for (let x = 0; x < size; x += 64) { ctx.beginPath(); ctx.moveTo(x,0); ctx.lineTo(x,size); ctx.stroke(); }
  for (let y = 0; y < size; y += 32) { ctx.beginPath(); ctx.moveTo(0,y); ctx.lineTo(size,y); ctx.stroke(); }
  const t = new THREE.CanvasTexture(c);
  t.wrapS = t.wrapT = THREE.RepeatWrapping;
  t.repeat.set(6, 6);
  return t;
}

const wallTex = makeWallpaper();
const floorTex = makeCarpet();
const ceilTex = makeCeiling();

const wallMat = new THREE.MeshLambertMaterial({ map: wallTex });
const floorMat = new THREE.MeshLambertMaterial({ map: floorTex });
const ceilMat = new THREE.MeshLambertMaterial({ map: ceilTex });

// ─── MAZE GENERATION ─────────────────────────────────────────────────────────
const CELL = 6;   // cell size in world units
const GRID = 12;  // grid dimensions
const ROOM_H = 3.2;

// Simple recursive backtracker
const visited = Array.from({length: GRID}, () => new Array(GRID).fill(false));
const walls = Array.from({length: GRID}, () => Array.from({length: GRID}, () => ({N:true,S:true,E:true,W:true})));

function carve(x, z) {
  visited[x][z] = true;
  const dirs = [{dx:0,dz:-1,d:'N',od:'S'},{dx:0,dz:1,d:'S',od:'N'},{dx:1,dz:0,d:'E',od:'W'},{dx:-1,dz:0,d:'W',od:'E'}];
  dirs.sort(() => Math.random()-0.5);
  for (const {dx,dz,d,od} of dirs) {
    const nx = x+dx, nz = z+dz;
    if (nx>=0&&nx<GRID&&nz>=0&&nz<GRID&&!visited[nx][nz]) {
      walls[x][z][d] = false;
      walls[nx][nz][od] = false;
      carve(nx, nz);
    }
  }
}
carve(0, 0);

// ─── BUILD GEOMETRY ────────────────────────────────────────────────────────────
const wallGeo = new THREE.BoxGeometry(1, 1, 1);

function addPanel(x, y, z, sx, sy, sz, mat) {
  const m = new THREE.Mesh(new THREE.BoxGeometry(sx, sy, sz), mat);
  m.position.set(x, y, z);
  scene.add(m);
}

for (let gx = 0; gx < GRID; gx++) {
  for (let gz = 0; gz < GRID; gz++) {
    const wx = gx * CELL - (GRID * CELL) / 2;
    const wz = gz * CELL - (GRID * CELL) / 2;
    const cx = wx + CELL/2;
    const cz = wz + CELL/2;

    // Floor
    addPanel(cx, 0, cz, CELL, 0.05, CELL, floorMat);
    // Ceiling
    addPanel(cx, ROOM_H, cz, CELL, 0.05, CELL, ceilMat);

    // Walls
    const W = 0.15;
    if (walls[gx][gz].N) addPanel(cx, ROOM_H/2, wz,       CELL, ROOM_H, W, wallMat);
    if (walls[gx][gz].S) addPanel(cx, ROOM_H/2, wz+CELL,  CELL, ROOM_H, W, wallMat);
    if (walls[gx][gz].W) addPanel(wx,       ROOM_H/2, cz,  W, ROOM_H, CELL, wallMat);
    if (walls[gx][gz].E) addPanel(wx+CELL,  ROOM_H/2, cz,  W, ROOM_H, CELL, wallMat);
  }
}

// Outer boundary
const half = (GRID * CELL) / 2;
addPanel(0, ROOM_H/2, -half, GRID*CELL, ROOM_H, 0.15, wallMat);
addPanel(0, ROOM_H/2,  half, GRID*CELL, ROOM_H, 0.15, wallMat);
addPanel(-half, ROOM_H/2, 0, 0.15, ROOM_H, GRID*CELL, wallMat);
addPanel( half, ROOM_H/2, 0, 0.15, ROOM_H, GRID*CELL, wallMat);

// ─── LIGHTS ────────────────────────────────────────────────────────────────────
const ambientLight = new THREE.AmbientLight(0xc8a84b, 0.25);
scene.add(ambientLight);

const lights = [];
for (let gx = 0; gx < GRID; gx++) {
  for (let gz = 0; gz < GRID; gz++) {
    if ((gx + gz) % 2 === 0) {
      const wx = gx * CELL - (GRID * CELL) / 2 + CELL/2;
      const wz = gz * CELL - (GRID * CELL) / 2 + CELL/2;
      const pl = new THREE.PointLight(0xd4c06a, 1.2, 10);
      pl.position.set(wx, ROOM_H - 0.3, wz);
      scene.add(pl);
      lights.push({ light: pl, baseIntensity: 1.2, flicker: Math.random() });

      // Visual tube light
      const tubeGeo = new THREE.BoxGeometry(1.5, 0.08, 0.12);
      const tubeMat = new THREE.MeshBasicMaterial({ color: 0xfffde0 });
      const tube = new THREE.Mesh(tubeGeo, tubeMat);
      tube.position.set(wx, ROOM_H - 0.05, wz);
      scene.add(tube);
    }
  }
}

// ─── PLAYER CONTROLLER ───────────────────────────────────────────────────────
const keys = {};
document.addEventListener('keydown', e => keys[e.code] = true);
document.addEventListener('keyup',   e => keys[e.code] = false);

let yaw = 0, pitch = 0;
let mouseLocked = false;
let gameStarted = false;
let sanity = 100;

const SPEED = 3.5;
const PLAYER_R = 0.4;

document.addEventListener('pointerlockchange', () => {
  mouseLocked = document.pointerLockElement === canvas;
});

canvas.addEventListener('click', () => {
  if (gameStarted) canvas.requestPointerLock();
});

document.addEventListener('mousemove', e => {
  if (!mouseLocked || !gameStarted) return;
  yaw   -= e.movementX * 0.002;
  pitch -= e.movementY * 0.002;
  pitch = Math.max(-Math.PI/3, Math.min(Math.PI/3, pitch));
});

// Simple collision
function getCell(x, z) {
  const off = (GRID * CELL) / 2;
  const gx = Math.floor((x + off) / CELL);
  const gz = Math.floor((z + off) / CELL);
  return { gx: Math.max(0, Math.min(GRID-1, gx)), gz: Math.max(0, Math.min(GRID-1, gz)) };
}

function canMove(x, z) {
  const off = (GRID * CELL) / 2;
  if (Math.abs(x) > half - PLAYER_R || Math.abs(z) > half - PLAYER_R) return false;
  const { gx, gz } = getCell(x, z);
  const localX = (x + off) - gx * CELL;
  const localZ = (z + off) - gz * CELL;
  const margin = PLAYER_R;
  const W = 0.15 + margin;

  if (walls[gx][gz].N && localZ < W) return false;
  if (walls[gx][gz].S && localZ > CELL - W) return false;
  if (walls[gx][gz].W && localX < W) return false;
  if (walls[gx][gz].E && localX > CELL - W) return false;

  return true;
}

// ─── FOOTSTEP AUDIO ──────────────────────────────────────────────────────────
let audioCtx = null;
let lastStep = 0;

function initAudio() {
  audioCtx = new (window.AudioContext || window.webkitAudioContext)();
}

function playStep() {
  if (!audioCtx) return;
  const now = audioCtx.currentTime;
  if (now - lastStep < 0.45) return;
  lastStep = now;

  const buf = audioCtx.createBuffer(1, audioCtx.sampleRate * 0.1, audioCtx.sampleRate);
  const data = buf.getChannelData(0);
  for (let i = 0; i < data.length; i++) {
    data[i] = (Math.random() * 2 - 1) * Math.exp(-i / (data.length * 0.3)) * 0.3;
  }
  const src = audioCtx.createBufferSource();
  src.buffer = buf;
  const gain = audioCtx.createGain();
  gain.gain.value = 0.4;
  const filter = audioCtx.createBiquadFilter();
  filter.type = 'lowpass';
  filter.frequency.value = 400;
  src.connect(filter); filter.connect(gain); gain.connect(audioCtx.destination);
  src.start();
}

function playHum() {
  if (!audioCtx) return;
  const osc = audioCtx.createOscillator();
  const gain = audioCtx.createGain();
  osc.type = 'sawtooth';
  osc.frequency.value = 60;
  gain.gain.value = 0.015;
  const filter = audioCtx.createBiquadFilter();
  filter.type = 'bandpass';
  filter.frequency.value = 120;
  filter.Q.value = 2;
  osc.connect(filter); filter.connect(gain); gain.connect(audioCtx.destination);
  osc.start();
  // Second harmonic
  const osc2 = audioCtx.createOscillator();
  const g2 = audioCtx.createGain();
  osc2.type = 'sine';
  osc2.frequency.value = 180;
  g2.gain.value = 0.006;
  osc2.connect(g2); g2.connect(audioCtx.destination);
  osc2.start();
}

// ─── FLICKER SYSTEM ──────────────────────────────────────────────────────────
const flickerEl = document.getElementById('flicker-overlay');
let nextFlicker = 5 + Math.random() * 10;
let flickerTimer = 0;

function doFlicker(dt) {
  flickerTimer += dt;
  if (flickerTimer > nextFlicker) {
    flickerTimer = 0;
    nextFlicker = 4 + Math.random() * 12;
    triggerFlicker();
  }

  // Per-light shimmer
  lights.forEach(({light, baseIntensity, flicker}, i) => {
    light.intensity = baseIntensity + Math.sin(Date.now() * 0.003 + flicker * 100) * 0.08
      + (Math.random() < 0.005 ? (Math.random() * 0.5 - 0.25) : 0);
  });
}

function triggerFlicker() {
  const seq = [80, 30, 80, 10, 80, 50, 80, 0, 80];
  let t = 0;
  seq.forEach((opacity, i) => {
    setTimeout(() => {
      ambientLight.intensity = opacity > 40 ? 0.25 : 0.0;
      lights.forEach(({light, baseIntensity}) => {
        light.intensity = opacity > 40 ? baseIntensity : 0;
      });
      flickerEl.style.opacity = (100 - opacity) / 100 * 0.15;
    }, t);
    t += 40 + Math.random() * 60;
  });
  setTimeout(() => { flickerEl.style.opacity = 0; }, t + 200);
}

// ─── SANITY SYSTEM ───────────────────────────────────────────────────────────
const sanityEl = document.getElementById('sanityVal');
let sanityDecayTimer = 0;

function updateSanity(dt) {
  sanityDecayTimer += dt;
  if (sanityDecayTimer > 3) {
    sanityDecayTimer = 0;
    sanity = Math.max(0, sanity - 0.5);
    sanityEl.textContent = Math.round(sanity);
  }

  // Low sanity effects
  if (sanity < 50) {
    const fovTarget = 75 + (50 - sanity) * 0.3;
    camera.fov += (fovTarget - camera.fov) * 0.01;
    camera.updateProjectionMatrix();
    scene.fog.near = 2 - (50 - sanity) * 0.02;
  }
}

// ─── START ───────────────────────────────────────────────────────────────────
document.getElementById('startBtn').addEventListener('click', () => {
  const overlay = document.getElementById('overlay');
  overlay.classList.add('hidden');
  setTimeout(() => {
    overlay.style.display = 'none';
    gameStarted = true;
    document.getElementById('hud').classList.add('visible');
    document.getElementById('sanity').classList.add('visible');
    document.getElementById('crosshair').classList.add('visible');
    canvas.requestPointerLock();
    initAudio();
    playHum();
  }, 1500);
});

// ─── CLOCK ───────────────────────────────────────────────────────────────────
const clock = new THREE.Clock();
let moving = false;

// Bob
let bobTime = 0;

function animate() {
  requestAnimationFrame(animate);
  const dt = clock.getDelta();

  if (!gameStarted) { renderer.render(scene, camera); return; }

  // Camera rotation
  const euler = new THREE.Euler(pitch, yaw, 0, 'YXZ');
  camera.quaternion.setFromEuler(euler);

  // Movement
  const dir = new THREE.Vector3();
  const forward = new THREE.Vector3(-Math.sin(yaw), 0, -Math.cos(yaw));
  const right   = new THREE.Vector3( Math.cos(yaw), 0, -Math.sin(yaw));

  if (keys['KeyW'] || keys['ArrowUp'])    dir.addScaledVector(forward,  1);
  if (keys['KeyS'] || keys['ArrowDown'])  dir.addScaledVector(forward, -1);
  if (keys['KeyA'] || keys['ArrowLeft'])  dir.addScaledVector(right,   -1);
  if (keys['KeyD'] || keys['ArrowRight']) dir.addScaledVector(right,    1);

  moving = dir.lengthSq() > 0;
  if (moving) {
    dir.normalize();
    const speed = SPEED * dt;
    const newX = camera.position.x + dir.x * speed;
    const newZ = camera.position.z + dir.z * speed;

    if (canMove(newX, camera.position.z)) camera.position.x = newX;
    if (canMove(camera.position.x, newZ)) camera.position.z = newZ;

    playStep();

    bobTime += dt * 8;
    camera.position.y = 1.65 + Math.sin(bobTime) * 0.04;
  } else {
    camera.position.y += (1.65 - camera.position.y) * 0.1;
    bobTime = 0;
  }

  doFlicker(dt);
  updateSanity(dt);

  renderer.render(scene, camera);
}

animate();

// ─── RESIZE ──────────────────────────────────────────────────────────────────
window.addEventListener('resize', () => {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
});
</script>
</body>
</html>
