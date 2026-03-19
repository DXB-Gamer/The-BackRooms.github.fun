<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>THE BACKROOMS</title>
<style>
* { margin:0; padding:0; box-sizing:border-box; }
body { background:#000; overflow:hidden; font-family:'Courier New',monospace; }
#canvas { display:block; width:100vw; height:100vh; }

#overlay {
  position:fixed; inset:0; display:flex; flex-direction:column;
  align-items:center; justify-content:center;
  background:#000; z-index:100; transition:opacity 1.5s;
}
#overlay.hidden { opacity:0; pointer-events:none; }
#overlay h1 {
  color:#c8a84b; font-size:clamp(2.5rem,9vw,6.5rem);
  letter-spacing:0.35em; text-transform:uppercase; text-align:center;
  text-shadow:0 0 40px #c8a84b88,0 0 90px #c8a84b33;
  animation:flkr 4s infinite; margin-bottom:.5rem;
}
.sub  { color:#3a2a0a; font-size:.75rem; letter-spacing:.3em; margin-bottom:.4rem; text-align:center; }
.lore { color:#444; font-size:.72rem; letter-spacing:.14em; margin-bottom:2.2rem; text-align:center; max-width:420px; line-height:2.1; }
#startBtn {
  background:none; border:1px solid #c8a84b44; color:#c8a84b;
  padding:.85rem 2.6rem; font-family:'Courier New',monospace;
  font-size:.88rem; letter-spacing:.28em; cursor:pointer;
  transition:all .3s; text-transform:uppercase; margin-bottom:1.1rem;
}
#startBtn:hover { background:#c8a84b0c; border-color:#c8a84b; }
.chint { color:#222; font-size:.56rem; letter-spacing:.17em; text-align:center; line-height:2.3; }

#hud {
  position:fixed; bottom:1.4rem; left:50%; transform:translateX(-50%);
  color:#c8a84b33; font-size:.56rem; letter-spacing:.17em; text-align:center;
  pointer-events:none; z-index:10; opacity:0; transition:opacity 1s;
}
#hud.show { opacity:1; }
#stats {
  position:fixed; top:4rem; left:1.4rem;
  color:#c8a84b66; font-size:.6rem; letter-spacing:.11em;
  z-index:10; opacity:0; transition:opacity 1s; line-height:2.3;
}
#stats.show { opacity:1; }
#stag {
  position:fixed; top:1.4rem; right:1.4rem;
  font-size:.6rem; letter-spacing:.11em; z-index:10;
  opacity:0; transition:opacity 1s;
}
#stag.show { opacity:1; }
#prompt {
  position:fixed; bottom:3.5rem; left:50%; transform:translateX(-50%);
  color:#c8a84b88; font-size:.65rem; letter-spacing:.18em;
  pointer-events:none; z-index:10; opacity:0; transition:opacity .4s;
}

#fsBtn {
  position:fixed; top:1.4rem; left:1.4rem;
  background:none; border:1px solid #c8a84b1a; color:#c8a84b33;
  font-family:'Courier New',monospace; font-size:.56rem;
  letter-spacing:.14em; padding:.38rem .75rem; cursor:pointer; z-index:20; transition:all .3s;
}
#fsBtn:hover { color:#c8a84b; border-color:#c8a84b44; }

#vig    { position:fixed; inset:0; background:radial-gradient(ellipse at center,transparent 28%,#000000cc 100%); pointer-events:none; z-index:5; }
#grain  { position:fixed; inset:0; pointer-events:none; z-index:6; opacity:.038;
  background-image:url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.85' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E"); }
#flk    { position:fixed; inset:0; background:#d4a843; pointer-events:none; z-index:7; opacity:0; }
#bld    { position:fixed; inset:0; background:#5a0000; pointer-events:none; z-index:8; opacity:0; transition:opacity .15s; }
#scare  { position:fixed; inset:0; background:#000; pointer-events:none; z-index:9; opacity:0; }

#xhair {
  position:fixed; top:50%; left:50%; transform:translate(-50%,-50%);
  width:14px; height:14px; pointer-events:none; z-index:10; opacity:0; transition:opacity 1s;
}
#xhair.show { opacity:.28; }
#xhair::before,#xhair::after { content:''; position:absolute; background:#c8a84b; }
#xhair::before { width:100%; height:1px; top:50%; left:0; transform:translateY(-50%); }
#xhair::after  { width:1px; height:100%; left:50%; top:0; transform:translateX(-50%); }

#jscare {
  position:fixed; inset:0; display:flex; align-items:center; justify-content:center;
  pointer-events:none; z-index:50; opacity:0; transition:opacity .06s;
}
#death {
  position:fixed; inset:0; background:#000;
  display:flex; flex-direction:column; align-items:center; justify-content:center;
  z-index:200; opacity:0; pointer-events:none; transition:opacity 2s;
}
#death.show { opacity:1; pointer-events:all; }
#death h2 { color:#5a0000; font-size:clamp(1.8rem,5vw,3.5rem); letter-spacing:.3em; margin-bottom:.8rem; }
#death p  { color:#222; font-size:.7rem; letter-spacing:.18em; margin-bottom:1.8rem; }
#rBtn {
  background:none; border:1px solid #5a000044; color:#5a0000;
  padding:.75rem 1.8rem; font-family:'Courier New',monospace;
  font-size:.8rem; letter-spacing:.22em; cursor:pointer; transition:all .3s;
}
#rBtn:hover { background:#5a000011; border-color:#5a0000; }

@keyframes flkr {
  0%,91%,100%{opacity:1;} 92%{opacity:.25;} 93%{opacity:1;} 95%{opacity:.06;} 96%{opacity:.82;}
}
@keyframes pulse { 0%,100%{opacity:1;} 50%{opacity:.2;} }
</style>
</head>
<body>

<canvas id="canvas"></canvas>
<div id="vig"></div><div id="grain"></div><div id="flk"></div><div id="bld"></div><div id="scare"></div>

<!-- JUMPSCARE SVG -->
<div id="jscare">
  <svg viewBox="0 0 200 320" xmlns="http://www.w3.org/2000/svg"
       style="width:min(70vw,70vh);height:min(70vw,70vh);filter:drop-shadow(0 0 70px #ff000077)">
    <ellipse cx="100" cy="105" rx="58" ry="74" fill="#eaeaea"/>
    <ellipse cx="76"  cy="95"  rx="15" ry="11" fill="#111"/>
    <ellipse cx="124" cy="95"  rx="15" ry="11" fill="#111"/>
    <ellipse cx="76"  cy="95"  rx="7"  ry="7"  fill="#ddd"/>
    <ellipse cx="124" cy="95"  rx="7"  ry="7"  fill="#ddd"/>
    <ellipse cx="76"  cy="96"  rx="5"  ry="6"  fill="#000"/>
    <ellipse cx="124" cy="96"  rx="5"  ry="6"  fill="#000"/>
    <line x1="66" y1="89" x2="60" y2="83" stroke="#900" stroke-width=".8" opacity=".8"/>
    <line x1="86" y1="89" x2="92" y2="83" stroke="#900" stroke-width=".8" opacity=".8"/>
    <path d="M68 130 Q100 158 132 130 Q116 148 100 151 Q84 148 68 130Z" fill="#080808"/>
    <line x1="78"  y1="134" x2="78"  y2="144" stroke="#888" stroke-width="1.5"/>
    <line x1="89"  y1="138" x2="89"  y2="149" stroke="#888" stroke-width="1.5"/>
    <line x1="100" y1="140" x2="100" y2="151" stroke="#888" stroke-width="1.5"/>
    <line x1="111" y1="138" x2="111" y2="149" stroke="#888" stroke-width="1.5"/>
    <line x1="122" y1="134" x2="122" y2="144" stroke="#888" stroke-width="1.5"/>
    <rect x="62" y="170" width="76" height="145" rx="3" fill="#080808"/>
    <polygon points="100,172 93,186 100,244 107,186" fill="#550000"/>
    <rect x="20" y="178" width="44" height="13" rx="5" fill="#090909" transform="rotate(20,62,178)"/>
    <rect x="136" y="178" width="44" height="13" rx="5" fill="#090909" transform="rotate(-20,138,178)"/>
    <path d="M16 212 L8 232 M20 214 L14 235 M24 215 L20 237 M28 214 L26 235" stroke="#111" stroke-width="3" stroke-linecap="round"/>
    <path d="M184 212 L192 232 M180 214 L186 235 M176 215 L180 237 M172 214 L174 235" stroke="#111" stroke-width="3" stroke-linecap="round"/>
    <path d="M65 188 Q28 224 12 300" stroke="#0a0a0a" stroke-width="5" fill="none"/>
    <path d="M135 188 Q172 224 188 300" stroke="#0a0a0a" stroke-width="5" fill="none"/>
    <path d="M76 204 Q44 248 32 318" stroke="#0a0a0a" stroke-width="3" fill="none" opacity=".7"/>
    <path d="M124 204 Q156 248 168 318" stroke="#0a0a0a" stroke-width="3" fill="none" opacity=".7"/>
  </svg>
</div>

<!-- OVERLAY -->
<div id="overlay">
  <h1>THE BACKROOMS</h1>
  <p class="sub">— LEVEL 0 : THE LOBBY —</p>
  <p class="lore">
    You have noclipped out of reality.<br>
    There is no exit. There is no map.<br>
    You are not alone in here.<br>
    <span style="color:#2a0a0a">Do not let them find you.</span>
  </p>
  <button id="startBtn">[ ENTER ]</button>
  <div class="chint">
    WASD — MOVE &nbsp;·&nbsp; MOUSE — LOOK &nbsp;·&nbsp; E/R — SPRINT<br>
    SPACE — JUMP &nbsp;·&nbsp; LSHIFT — CROUCH &nbsp;·&nbsp; LMB — PUNCH &nbsp;·&nbsp; RMB — DOOR &nbsp;·&nbsp; F — FLASHLIGHT &nbsp;·&nbsp; V — CAM
  </div>
</div>

<div id="stats"></div>
<div id="stag"></div>
<div id="prompt"></div>
<div id="hud">WASD/SPRINT(E) &nbsp;·&nbsp; SPACE JUMP &nbsp;·&nbsp; SHIFT CROUCH &nbsp;·&nbsp; LMB PUNCH &nbsp;·&nbsp; RMB DOOR &nbsp;·&nbsp; F FLASHLIGHT &nbsp;·&nbsp; V CAM</div>
<div id="xhair"></div>
<button id="fsBtn">⛶ FULLSCREEN</button>

<div id="death">
  <h2>YOU WERE FOUND</h2>
  <p>There is no escape from the backrooms.</p>
  <button id="rBtn">[ TRY AGAIN ]</button>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
// ════════════════════════════════════════════
// RENDERER
// ════════════════════════════════════════════
const canvas = document.getElementById('canvas');
const renderer = new THREE.WebGLRenderer({ canvas, antialias: false });
renderer.setPixelRatio(1);
renderer.setSize(innerWidth, innerHeight);

const scene = new THREE.Scene();
scene.background = new THREE.Color(0x060400);
scene.fog = new THREE.Fog(0x0a0800, 1.5, 12);

const camera = new THREE.PerspectiveCamera(75, innerWidth / innerHeight, 0.05, 60);

// ════════════════════════════════════════════
// TEXTURES
// ════════════════════════════════════════════
function mkWall() {
  const s=512, c=document.createElement('canvas'); c.width=c.height=s;
  const x=c.getContext('2d');
  x.fillStyle='#b89535'; x.fillRect(0,0,s,s);
  for(let i=0;i<12000;i++){
    x.fillStyle=`rgba(${Math.random()>.5?165:60},${Math.random()>.5?120:70},10,${Math.random()*.07})`;
    x.fillRect(Math.random()*s,Math.random()*s,Math.random()*4,Math.random()*4);
  }
  x.strokeStyle='rgba(125,90,10,.17)'; x.lineWidth=1;
  for(let i=0;i<s;i+=32){x.beginPath();x.moveTo(i,0);x.lineTo(i,s);x.stroke();}
  for(let i=0;i<s;i+=32){x.beginPath();x.moveTo(0,i);x.lineTo(s,i);x.stroke();}
  for(let i=0;i<18;i++){
    const gx=Math.random()*s,gy=Math.random()*s,gr=x.createRadialGradient(gx,gy,0,gx,gy,60+Math.random()*80);
    gr.addColorStop(0,'rgba(45,25,2,.17)'); gr.addColorStop(1,'rgba(45,25,2,0)');
    x.fillStyle=gr; x.fillRect(0,0,s,s);
  }
  const t=new THREE.CanvasTexture(c); t.wrapS=t.wrapT=THREE.RepeatWrapping; t.repeat.set(4,2); return t;
}
function mkFloor() {
  const s=256,c=document.createElement('canvas'); c.width=c.height=s;
  const x=c.getContext('2d');
  x.fillStyle='#352a0e'; x.fillRect(0,0,s,s);
  for(let i=0;i<8000;i++){
    x.fillStyle=`rgba(${48+Math.random()*28},${28+Math.random()*20},${5+Math.random()*12},.65)`;
    x.fillRect(Math.random()*s,Math.random()*s,1,1);
  }
  x.strokeStyle='rgba(75,50,14,.28)'; x.lineWidth=1;
  for(let i=0;i<s;i+=16){x.beginPath();x.moveTo(i,0);x.lineTo(i,s);x.stroke();}
  for(let i=0;i<s;i+=16){x.beginPath();x.moveTo(0,i);x.lineTo(s,i);x.stroke();}
  const t=new THREE.CanvasTexture(c); t.wrapS=t.wrapT=THREE.RepeatWrapping; t.repeat.set(10,10); return t;
}
function mkCeil() {
  const s=256,c=document.createElement('canvas'); c.width=c.height=s;
  const x=c.getContext('2d');
  x.fillStyle='#cbbf88'; x.fillRect(0,0,s,s);
  for(let i=0;i<4000;i++){
    x.fillStyle=`rgba(148,128,68,${Math.random()*.1})`;
    x.fillRect(Math.random()*s,Math.random()*s,Math.random()*4,Math.random()*4);
  }
  x.strokeStyle='rgba(128,108,58,.38)'; x.lineWidth=2;
  for(let i=0;i<s;i+=64){x.beginPath();x.moveTo(i,0);x.lineTo(i,s);x.stroke();}
  for(let i=0;i<s;i+=32){x.beginPath();x.moveTo(0,i);x.lineTo(s,i);x.stroke();}
  const t=new THREE.CanvasTexture(c); t.wrapS=t.wrapT=THREE.RepeatWrapping; t.repeat.set(6,6); return t;
}
function mkDoorTex() {
  const s=256, c=document.createElement('canvas'); c.width=s; c.height=s*2;
  const x=c.getContext('2d');
  x.fillStyle='#5a3a18'; x.fillRect(0,0,s,s*2);
  // Panels
  [[10,10,s-20,s*.45-10],[10,s*.45+10,s-20,s*.95-20]].forEach(([px,py,pw,ph])=>{
    x.strokeStyle='#3a2208'; x.lineWidth=4; x.strokeRect(px,py,pw,ph);
    x.strokeStyle='#7a5228'; x.lineWidth=1.5; x.strokeRect(px+4,py+4,pw-8,ph-8);
  });
  // Grain
  for(let i=0;i<3000;i++){
    x.fillStyle=`rgba(${Math.random()>.5?80:40},${Math.random()>.5?50:25},10,${Math.random()*.08})`;
    x.fillRect(Math.random()*s,Math.random()*s*2,1,Math.random()*8);
  }
  // Handle
  x.fillStyle='#c8a030'; x.beginPath();
  x.arc(s*.85,s*.75,6,0,Math.PI*2); x.fill();
  x.fillStyle='#a88020'; x.fillRect(s*.83,s*.75,14,3);
  const t=new THREE.CanvasTexture(c); return t;
}

const wallMat  = new THREE.MeshPhongMaterial({ map: mkWall(), shininess: 8 });
const floorMat = new THREE.MeshPhongMaterial({ map: mkFloor(), shininess: 2 });
const ceilMat  = new THREE.MeshPhongMaterial({ map: mkCeil(), shininess: 4 });
const doorMat  = new THREE.MeshPhongMaterial({ map: mkDoorTex(), shininess: 12 });
const doorMatO = new THREE.MeshPhongMaterial({ map: mkDoorTex(), transparent: true, opacity: .85, shininess: 12 });

// ════════════════════════════════════════════
// MAZE  (16×16, cell=6)
// ════════════════════════════════════════════
const CELL=6, GRID=16, RH=3.2;
const MHALF=(GRID*CELL)/2;  // 48

// Spawn player in centre of cell [8,8] → world coords
const SPAWN_X = (8*CELL - MHALF) + CELL/2;   //  3
const SPAWN_Z = (8*CELL - MHALF) + CELL/2;   //  3

const vis  = Array.from({length:GRID},()=>new Array(GRID).fill(false));
const wls  = Array.from({length:GRID},()=>Array.from({length:GRID},()=>({N:true,S:true,E:true,W:true})));

function carve(cx,cz){
  vis[cx][cz]=true;
  const dirs=[{dx:0,dz:-1,d:'N',o:'S'},{dx:0,dz:1,d:'S',o:'N'},{dx:1,dz:0,d:'E',o:'W'},{dx:-1,dz:0,d:'W',o:'E'}];
  dirs.sort(()=>Math.random()-.5);
  for(const{dx,dz,d,o} of dirs){
    const nx=cx+dx,nz=cz+dz;
    if(nx>=0&&nx<GRID&&nz>=0&&nz<GRID&&!vis[nx][nz]){
      wls[cx][cz][d]=false; wls[nx][nz][o]=false; carve(nx,nz);
    }
  }
}
carve(8,8); // start carving from spawn cell so player is never trapped

// ── Geometry merge helper — collect then merge into 1 mesh per material ──
// This cuts draw calls from ~1500 down to 3 (walls/floor/ceiling)
const _wallGeos=[], _floorGeos=[], _ceilGeos=[];

function queueBox(x,y,z,sx,sy,sz,bucket){
  const g=new THREE.BoxGeometry(sx,sy,sz);
  const m=new THREE.Matrix4().makeTranslation(x,y,z);
  g.applyMatrix4(m);
  bucket.push(g);
}

// Also keep addBox for doors (they need individual meshes to animate)
function addBox(x,y,z,sx,sy,sz,mat){
  const m=new THREE.Mesh(new THREE.BoxGeometry(sx,sy,sz),mat);
  m.position.set(x,y,z); scene.add(m); return m;
}

// Build rooms — queue into buckets
for(let gx=0;gx<GRID;gx++) for(let gz=0;gz<GRID;gz++){
  const wx=gx*CELL-MHALF, wz=gz*CELL-MHALF;
  const cx=wx+CELL/2, cz=wz+CELL/2, W=0.14;
  queueBox(cx,0,cz,       CELL,.05,CELL, _floorGeos);
  queueBox(cx,RH,cz,      CELL,.05,CELL, _ceilGeos);
  if(wls[gx][gz].N) queueBox(cx, RH/2, wz,      CELL,RH,W, _wallGeos);
  if(wls[gx][gz].S) queueBox(cx, RH/2, wz+CELL, CELL,RH,W, _wallGeos);
  if(wls[gx][gz].W) queueBox(wx,       RH/2, cz, W,RH,CELL, _wallGeos);
  if(wls[gx][gz].E) queueBox(wx+CELL,  RH/2, cz, W,RH,CELL, _wallGeos);
}
// Outer boundary — also walls
queueBox(0,RH/2,-MHALF,  GRID*CELL,RH,.14, _wallGeos);
queueBox(0,RH/2, MHALF,  GRID*CELL,RH,.14, _wallGeos);
queueBox(-MHALF, RH/2,0, .14,RH,GRID*CELL, _wallGeos);
queueBox( MHALF, RH/2,0, .14,RH,GRID*CELL, _wallGeos);

// Merge each bucket → single mesh = 3 draw calls total for the entire maze
function mergeGeos(geos, mat){
  // Manual merge: concatenate all position/normal/uv buffers
  let totalVerts=0, totalIdx=0;
  geos.forEach(g=>{ totalVerts+=g.attributes.position.count; totalIdx+=g.index?g.index.count:0; });
  const positions=new Float32Array(totalVerts*3);
  const normals  =new Float32Array(totalVerts*3);
  const uvs      =new Float32Array(totalVerts*2);
  const indices  =new Uint32Array(totalIdx);
  let vi=0,ni=0,ui=0,ii=0,baseV=0;
  geos.forEach(g=>{
    const p=g.attributes.position.array;
    const n=g.attributes.normal.array;
    const u=g.attributes.uv.array;
    positions.set(p,vi); vi+=p.length;
    normals.set(n,ni);   ni+=n.length;
    uvs.set(u,ui);       ui+=u.length;
    if(g.index){ const idx=g.index.array; for(let i=0;i<idx.length;i++) indices[ii++]=idx[i]+baseV; }
    baseV+=g.attributes.position.count;
    g.dispose();
  });
  const merged=new THREE.BufferGeometry();
  merged.setAttribute('position',new THREE.BufferAttribute(positions,3));
  merged.setAttribute('normal',  new THREE.BufferAttribute(normals,3));
  merged.setAttribute('uv',      new THREE.BufferAttribute(uvs,2));
  merged.setIndex(new THREE.BufferAttribute(indices,1));
  scene.add(new THREE.Mesh(merged,mat));
}

mergeGeos(_wallGeos,  wallMat);
mergeGeos(_floorGeos, floorMat);
mergeGeos(_ceilGeos,  ceilMat);

// ════════════════════════════════════════════
// DOORS — set INTO solid walls with stubs + lintel
// ════════════════════════════════════════════
const doors = [];
const DW = 0.95;  // door width
const DH = 2.35;  // door height
const WT = 0.14;  // wall thickness

function addDoorInWall(cx, cz, rotY, side){
  // Door panel
  const mesh = new THREE.Mesh(new THREE.BoxGeometry(DW, DH, WT+.04), doorMat.clone());
  mesh.position.set(cx, DH/2, cz);
  mesh.rotation.y = rotY;
  scene.add(mesh);
  doors.push({ mesh, open:false, cx, cz, rotY, side, openProgress:0 });

  const sideLen = (CELL - DW) / 2;
  const topH    = RH - DH;
  const isNS    = (rotY === 0);

  if(isNS){
    // Left wall stub
    addBox(cx - DW/2 - sideLen/2, RH/2, cz, sideLen, RH, WT, wallMat);
    // Right wall stub
    addBox(cx + DW/2 + sideLen/2, RH/2, cz, sideLen, RH, WT, wallMat);
    // Top lintel
    addBox(cx, DH + topH/2, cz, DW + .02, topH, WT, wallMat);
  } else {
    addBox(cx, RH/2, cz - DW/2 - sideLen/2, WT, RH, sideLen, wallMat);
    addBox(cx, RH/2, cz + DW/2 + sideLen/2, WT, RH, sideLen, wallMat);
    addBox(cx, DH + topH/2, cz, WT, topH, DW + .02, wallMat);
  }
}

// Place doors in SOLID walls only (wls===true means wall exists)
for(let gx=0;gx<GRID;gx++) for(let gz=0;gz<GRID;gz++){
  const wx=gx*CELL-MHALF, wz=gz*CELL-MHALF;
  const cx=wx+CELL/2, cz=wz+CELL/2;
  if(wls[gx][gz].N && gz>0 && Math.random()<.30)
    addDoorInWall(cx, wz, 0, 'N');
  if(wls[gx][gz].W && gx>0 && Math.random()<.30)
    addDoorInWall(wx, cz, Math.PI/2, 'W');
}

function getDoorInFront(){
  const ray = new THREE.Raycaster();
  ray.setFromCamera(new THREE.Vector2(0,0), camera);
  const hits = ray.intersectObjects(doors.map(d=>d.mesh));
  if(hits.length>0 && hits[0].distance<2.8)
    return doors.find(d=>d.mesh===hits[0].object);
  return null;
}

// ── LEVEL SYSTEM ─────────────────────────────────────────────────────────────
// Doors lead to deeper levels: darker, scarier, more monsters
let currentLevel = 0;
const levelThemes = [
  { fogColor:0x1a1206, fogNear:2, fogFar:22, ambColor:0xc8a84b, ambInt:.18, wallColor:0xb89535, name:'LEVEL 0 — THE LOBBY' },
  { fogColor:0x080804, fogNear:1.5, fogFar:16, ambColor:0x886622, ambInt:.12, wallColor:0x7a6020, name:'LEVEL 1 — HABITABLE ZONE' },
  { fogColor:0x020205, fogNear:1,   fogFar:11, ambColor:0x441122, ambInt:.08, wallColor:0x4a3015, name:'LEVEL 2 — PIPE DREAMS' },
  { fogColor:0x000002, fogNear:.8,  fogFar:8,  ambColor:0x220808, ambInt:.05, wallColor:0x2a1808, name:'LEVEL 3 — THE DARK' },
];

function safeTeleport(){
  // Strategy: scan EVERY cell in the grid, build a list of cells that
  // have at least 2 open sides (passage connections) — these are
  // corridor cells guaranteed to have space. Pick one randomly.
  // Then place player at the exact geometric centre of that cell,
  // which is CELL/2 = 3 units from every wall face — well clear of
  // the 0.14 wall thickness + 0.38 player radius = 0.52 needed.
  const openCells = [];
  for(let gx=0;gx<GRID;gx++){
    for(let gz=0;gz<GRID;gz++){
      // Count how many sides are OPEN (carved passages)
      const w = wls[gx][gz];
      const openSides = (!w.N?1:0)+(!w.S?1:0)+(!w.E?1:0)+(!w.W?1:0);
      // Only use cells with 2+ open sides (proper corridors, not dead ends near borders)
      // Also skip the 2x2 area around spawn cell [8,8]
      if(openSides >= 2 && (Math.abs(gx-8)>3 || Math.abs(gz-8)>3)){
        openCells.push([gx,gz]);
      }
    }
  }

  // Shuffle and pick first valid one
  for(let i=openCells.length-1;i>0;i--){
    const j=Math.floor(Math.random()*(i+1));
    [openCells[i],openCells[j]]=[openCells[j],openCells[i]];
  }

  for(const [gx,gz] of openCells){
    // Exact cell centre in world space
    const wx = (gx * CELL) - MHALF + (CELL * 0.5);
    const wz = (gz * CELL) - MHALF + (CELL * 0.5);
    // Set ALL position state atomically
    camera.position.x = wx;
    camera.position.z = wz;
    camera.position.y = SH;
    playerY  = SH;
    velY     = 0;
    onGround = true;
    pitch    = 0;
    // yaw set by caller (descend)
    return;
  }

  // Absolute fallback — spawn cell centre (guaranteed clear)
  camera.position.x = SPAWN_X;
  camera.position.z = SPAWN_Z;
  camera.position.y = SH;
  playerY=SH; velY=0; onGround=true; pitch=0;
}

function descend(){
  currentLevel = Math.min(currentLevel+1, levelThemes.length-1);
  const theme = levelThemes[currentLevel];

  // Update atmosphere
  scene.fog.color.setHex(theme.fogColor);
  scene.fog.near  = theme.fogNear;
  scene.fog.far   = theme.fogFar;
  scene.background.setHex(theme.fogColor);
  ambL.color.setHex(theme.ambColor);
  ambL.intensity = theme.ambInt;

  // Safe teleport — always lands in open space
  safeTeleport();
  pitch = 0;
  yaw   = Math.random()*Math.PI*2;
  // Force camera to new orientation immediately so mouse delta doesn't fight it
  camera.quaternion.setFromEuler(new THREE.Euler(0, yaw, 0, 'YXZ'));

  // Spawn extra monsters per level
  const extraZombies = currentLevel * 3;
  for(let i=0;i<extraZombies;i++){
    let mx,mz;
    do {
      mx=(Math.random()-.5)*GRID*CELL*.8;
      mz=(Math.random()-.5)*GRID*CELL*.8;
    } while(Math.abs(mx-camera.position.x)<8 && Math.abs(mz-camera.position.z)<8);
    spawnM('zombie', mx, mz);
  }
  if(currentLevel>=2) spawnM('slender', camera.position.x+20, camera.position.z+20);

  // (flash handled by toggleDoor)

  showLevelBanner(theme.name);
  playDoor();
}

function showLevelBanner(name){
  let banner = document.getElementById('lvlBanner');
  if(!banner){
    banner = document.createElement('div');
    banner.id='lvlBanner';
    banner.style.cssText='position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);color:#c8a84b;font-family:Courier New,monospace;font-size:clamp(1rem,3vw,1.8rem);letter-spacing:.3em;text-align:center;pointer-events:none;z-index:30;opacity:0;transition:opacity 1s;text-shadow:0 0 30px #c8a84b88;';
    document.body.appendChild(banner);
  }
  banner.textContent = name;
  banner.style.opacity='1';
  setTimeout(()=>banner.style.opacity='0', 2500);
}

function toggleDoor(door){
  if(!door.open){
    door.open = true;
    playDoor();
    document.getElementById('scare').style.opacity = '1';
    descend();
    setTimeout(()=>{ document.getElementById('scare').style.opacity = '0'; }, 500);
  } else {
    door.open = false;
    playDoor();
  }
}

function updateDoors(dt){
  for(const d of doors){
    d.openProgress += (( d.open?1:0 ) - d.openProgress) * Math.min(1, dt*7);
    const angle = d.openProgress * Math.PI * .48;
    const H = d.cx, V = d.cz;
    const half = DW/2;
    if(d.side==='N'){
      // hinge on left (+X) end, swing into room
      d.mesh.rotation.y = d.rotY - angle;
      d.mesh.position.x = H + half - Math.cos(angle)*half;
      d.mesh.position.z = V     + Math.sin(angle)*half;
    } else {
      // hinge on top (+Z) end, swing into room
      d.mesh.rotation.y = d.rotY + angle;
      d.mesh.position.x = H     - Math.sin(angle)*half;
      d.mesh.position.z = V + half - Math.cos(angle)*half;
    }
  }
}

// ════════════════════════════════════════════
// LIGHTS
// ════════════════════════════════════════════
const ambL=new THREE.AmbientLight(0xc8a84b,.06); scene.add(ambL);

// ── FLASHLIGHT — tight bright circle like a real torch ────────────
// SpotLight: narrow cone, zero penumbra = hard circle edge
// PointLight at same pos adds bright centre "hotspot" bloom
const flashlight = new THREE.SpotLight(
  0xffffff,  // pure white
  0,         // starts off, enabled below
  18,        // range
  Math.PI * 0.07,  // ~8deg cone — tight circle
  0.08,      // almost no penumbra = hard edge
  1.2        // decay
);
const flashTarget = new THREE.Object3D();
scene.add(flashlight);
scene.add(flashTarget);
flashlight.target = flashTarget;

// Inner hotspot — small PointLight for the bright centre disc
const flashHotspot = new THREE.PointLight(0xffffff, 0, 3.5, 2);
scene.add(flashHotspot);

let flashOn = true;
flashlight.intensity   = 4.5;
flashHotspot.intensity = 1.8;

function toggleFlash(){
  flashOn = !flashOn;
  flashlight.intensity   = flashOn ? 4.5 : 0;
  flashHotspot.intensity = flashOn ? 1.8 : 0;
}

// Called every frame — positions SpotLight at the hand, aims along camera look direction
const _flashDir   = new THREE.Vector3();
const _flashRight = new THREE.Vector3();
const _flashUp    = new THREE.Vector3();
const _camQuat    = new THREE.Quaternion();

function updateFlashlight(){
  // Always update positions even when off so no pop on enable
  camera.getWorldQuaternion(_camQuat);
  _flashDir.set(0,0,-1).applyQuaternion(_camQuat);
  _flashRight.set(1,0,0).applyQuaternion(_camQuat);
  _flashUp.set(0,1,0).applyQuaternion(_camQuat);

  // SpotLight origin: right-hand position in world space
  flashlight.position.copy(camera.position)
    .addScaledVector(_flashRight,  0.20)
    .addScaledVector(_flashUp,    -0.16)
    .addScaledVector(_flashDir,    0.10);

  // Aim: exactly where crosshair points — 40 units ahead
  flashTarget.position.copy(flashlight.position)
    .addScaledVector(_flashDir, 40);

  // Hotspot: 2 units ahead of torch mouth along aim direction
  flashHotspot.position.copy(flashlight.position)
    .addScaledVector(_flashDir, 2.0);

  if(!flashOn){
    flashlight.intensity   = 0;
    flashHotspot.intensity = 0;
    return;
  }

  // Slight dying-battery flicker
  const t   = Date.now();
  const flk = Math.sin(t*.022)*0.18
    + (Math.random()<.005 ? Math.random()*1.2-.6 : 0);
  flashlight.intensity   = Math.max(0, 4.5 + flk);
  flashHotspot.intensity = Math.max(0, 1.8 + flk*0.4);
}

document.addEventListener('keydown',e=>{
  if(e.code==='KeyF' && gameStarted) toggleFlash();
});
const lights=[];
// Only 10 ceiling lights spread across the map — each covers a wide area
// This eliminates ~118 PointLights = massive perf win
const lightPositions=[
  [0,0],[-24,-24],[24,-24],[-24,24],[24,24],
  [0,-36],[0,36],[-36,0],[36,0],[0,0]
];
lightPositions.forEach(([lx,lz])=>{
  const pl=new THREE.PointLight(0xd4a030,0.9,28,1);
  pl.position.set(lx,RH-.3,lz); scene.add(pl);
  lights.push({l:pl,b:0.9,p:Math.random()*100});
  // Visual tube marker at each light
  const tube=new THREE.Mesh(new THREE.BoxGeometry(1.55,.07,.13),new THREE.MeshBasicMaterial({color:0xfff6cc}));
  tube.position.set(lx,RH-.04,lz); scene.add(tube);
});

// ════════════════════════════════════════════
// MONSTERS
// ════════════════════════════════════════════
function mkSlender(){
  // Faithful to the real Slenderman image:
  // Very tall (~3.2m), white blank oval head, slim black suit, red tie,
  // extremely long arms hanging past knees, pale hands with long fingers, no face
  const g = new THREE.Group();
  const pale = new THREE.MeshLambertMaterial({color:0xf0efeb});
  const blk  = new THREE.MeshLambertMaterial({color:0x06060a});
  const suit = new THREE.MeshLambertMaterial({color:0x0a0a12});
  const red  = new THREE.MeshLambertMaterial({color:0x8b0000});
  const white= new THREE.MeshLambertMaterial({color:0xe8e8e8});

  // ── HEAD: featureless pale oval, slightly elongated ──
  const headGeo = new THREE.SphereGeometry(.28,16,12);
  const head = new THREE.Mesh(headGeo, pale);
  head.scale.set(.85, 1.25, .78);
  head.position.y = 3.08;
  g.add(head);

  // Very faint, almost invisible eye depressions — no real eyes
  [-0.10,0.10].forEach(ox=>{
    const socket = new THREE.Mesh(new THREE.SphereGeometry(.065,8,6),
      new THREE.MeshLambertMaterial({color:0xd8d7d2}));
    socket.position.set(ox, 3.10, .21);
    socket.scale.set(1,.7,0.3);
    g.add(socket);
  });

  // ── NECK: thin ──
  const neck = new THREE.Mesh(new THREE.CylinderGeometry(.055,.065,.22,8), pale);
  neck.position.y = 2.82; g.add(neck);

  // ── SHOULDERS: wide padded suit shoulders ──
  const shoulderL = new THREE.Mesh(new THREE.BoxGeometry(.18,.12,.2), suit);
  shoulderL.position.set(-.38, 2.68, 0); g.add(shoulderL);
  const shoulderR = new THREE.Mesh(new THREE.BoxGeometry(.18,.12,.2), suit);
  shoulderR.position.set(.38, 2.68, 0); g.add(shoulderR);

  // ── TORSO: slim black suit jacket ──
  const torso = new THREE.Mesh(new THREE.BoxGeometry(.44,1.38,.22), suit);
  torso.position.y = 1.9; g.add(torso);

  // Suit lapels (white shirt beneath)
  const lapelL = new THREE.Mesh(new THREE.BoxGeometry(.07,.5,.03), white);
  lapelL.position.set(-.07,2.1,.12); lapelL.rotation.z=.18; g.add(lapelL);
  const lapelR = new THREE.Mesh(new THREE.BoxGeometry(.07,.5,.03), white);
  lapelR.position.set(.07,2.1,.12); lapelR.rotation.z=-.18; g.add(lapelR);

  // Red tie — narrow, long
  const tie = new THREE.Mesh(new THREE.BoxGeometry(.055,.75,.025), red);
  tie.position.set(0,1.88,.115); g.add(tie);
  // Tie knot
  const knot = new THREE.Mesh(new THREE.BoxGeometry(.07,.06,.04), red);
  knot.position.set(0,2.24,.12); g.add(knot);

  // ── LEGS: long slim trousers — named for animation ──
  const slegL = new THREE.Mesh(new THREE.BoxGeometry(.14,1.22,.18), blk);
  slegL.position.set(-.10,.61,0); g.add(slegL); g.userData.legL=slegL;
  const slegR = new THREE.Mesh(new THREE.BoxGeometry(.14,1.22,.18), blk);
  slegR.position.set( .10,.61,0); g.add(slegR); g.userData.legR=slegR;
  [-0.10,0.10].forEach(ox=>{
    const shoe = new THREE.Mesh(new THREE.BoxGeometry(.15,.08,.22), blk);
    shoe.position.set(ox,.04,.04); g.add(shoe);
  });

  // ── ARMS: extremely long, hanging straight down past knees ──
  [-1,1].forEach(sx=>{
    const upper = new THREE.Mesh(new THREE.BoxGeometry(.09,1.05,.12), suit);
    upper.position.set(sx*.42, 1.96, 0);
    upper.rotation.z = sx * .08;
    g.add(upper);
    if(sx<0) g.userData.armLU=upper; else g.userData.armRU=upper;

    const fore = new THREE.Mesh(new THREE.BoxGeometry(.075,.95,.1), suit);
    fore.position.set(sx*.5, .88, .04);
    fore.rotation.z = sx * .05;
    g.add(fore);
    if(sx<0) g.userData.armLD=fore; else g.userData.armRD=fore;

    // PALE HAND — large, unnaturally long fingers
    const palm = new THREE.Mesh(new THREE.BoxGeometry(.12,.16,.08), pale);
    palm.position.set(sx*.52, .36, .04);
    g.add(palm);

    // 4 long spindly fingers
    const fingerLengths = [.22,.26,.25,.20];
    fingerLengths.forEach((fl,fi)=>{
      const fox = sx*(.42 + fi*.04 - .06);
      const finger = new THREE.Mesh(new THREE.BoxGeometry(.025,fl,.028), pale);
      finger.position.set(fox, .36-fl*.5-.08, .04);
      g.add(finger);
      // Fingertip slightly darker
      const tip = new THREE.Mesh(new THREE.BoxGeometry(.027,.05,.03),
        new THREE.MeshLambertMaterial({color:0xd8c8b8}));
      tip.position.set(fox, .36-fl-.08, .04);
      g.add(tip);
    });

    // Thumb — short, angled
    const thumb = new THREE.Mesh(new THREE.BoxGeometry(.028,.15,.03), pale);
    thumb.position.set(sx*.38, .38, .04);
    thumb.rotation.z = sx * .6;
    g.add(thumb);
  });

  return g;
}

function mkZombie(){
  // Returns group WITH named animation bones stored as userData
  const g = new THREE.Group();

  const skinM  = new THREE.MeshLambertMaterial({color:0x1e2a12, emissive:0x050800, emissiveIntensity:.3});
  const darkM  = new THREE.MeshLambertMaterial({color:0x060604});
  const boneM  = new THREE.MeshLambertMaterial({color:0xb8a870, emissive:0x221800, emissiveIntensity:.2});
  const glowM  = new THREE.MeshLambertMaterial({color:0xff2200, emissive:0xff1100, emissiveIntensity:1.4}); // blood-red glow eyes
  const blackM = new THREE.MeshLambertMaterial({color:0x080608});
  const bloodM = new THREE.MeshLambertMaterial({color:0x6b0000, emissive:0x3a0000, emissiveIntensity:.5});

  // ── SKULL HEAD: sunken, elongated ──────────────────────────────
  const head = new THREE.Mesh(new THREE.SphereGeometry(.24,12,10), skinM);
  head.scale.set(.78,1.45,.7);
  head.position.set(0, 2.62, .06);
  g.add(head);
  g.userData.head = head;

  // Sunken deep eye craters
  [-0.1,0.1].forEach(ox=>{
    const crater = new THREE.Mesh(new THREE.SphereGeometry(.09,8,6), darkM);
    crater.position.set(ox,2.66,.19); crater.scale.set(1,.75,.35);
    g.add(crater);
    // Burning red pupil glow — point of light in the darkness
    const eye = new THREE.Mesh(new THREE.SphereGeometry(.042,7,6), glowM);
    eye.position.set(ox,2.66,.2);
    g.add(eye);
  });

  // Cracked skull top (exposed cranium strips)
  [-.06,0,.07].forEach(ox=>{
    const crack = new THREE.Mesh(new THREE.BoxGeometry(.015,.28,.06), boneM);
    crack.position.set(ox,2.82,.0); crack.rotation.x=.1;
    g.add(crack);
  });

  // Jaw — dislocated, hanging low
  const jaw = new THREE.Mesh(new THREE.BoxGeometry(.2,.08,.14), skinM);
  jaw.position.set(0,2.35,.12); jaw.rotation.x=.55;
  g.add(jaw);
  g.userData.jaw = jaw;

  // Ragged teeth on jaw
  [-0.07,-0.03,0.02,0.06].forEach((ox,i)=>{
    const tl=[.06,.09,.07,.05][i];
    const t=new THREE.Mesh(new THREE.BoxGeometry(.022,tl,.02),boneM);
    t.position.set(ox,2.36-tl*.3,.22); g.add(t);
  });

  // Blood dripping from mouth
  const drip = new THREE.Mesh(new THREE.CylinderGeometry(.008,.004,.18,4), bloodM);
  drip.position.set(.02,2.22,.2); g.add(drip);

  // ── NECK: cracked vertebrae visible ────────────────────────────
  const neck = new THREE.Mesh(new THREE.CylinderGeometry(.032,.048,.22,6), skinM);
  neck.position.set(0,2.24,.04); g.add(neck);
  // Exposed spine knobs
  [0,.08,.16].forEach(dy=>{
    const knob=new THREE.Mesh(new THREE.SphereGeometry(.022,5,4), boneM);
    knob.position.set(0,2.38+dy,-.06); g.add(knob);
  });

  // ── TORSO: skeletal, hunched forward aggressively ───────────────
  const torso = new THREE.Mesh(new THREE.BoxGeometry(.34,1.15,.18), blackM);
  torso.position.set(0,1.58,.04); torso.rotation.x=.22; // hunch forward
  g.add(torso);
  g.userData.torso = torso;

  // Exposed ribcage — both sides visible through torn flesh
  [-1,1].forEach(sx=>{
    [0,.1,.2,.3,.4].forEach((dy,ri)=>{
      const rib=new THREE.Mesh(new THREE.BoxGeometry(.12,.025,.05), boneM);
      rib.position.set(sx*.18,1.85-dy,0);
      rib.rotation.z=sx*.18; rib.rotation.x=.1;
      g.add(rib);
    });
  });

  // Gaping chest wound
  const wound=new THREE.Mesh(new THREE.BoxGeometry(.1,.14,.06), bloodM);
  wound.position.set(.04,1.72,.1); g.add(wound);

  // ── LEGS: long, uneven, bone-exposed ────────────────────────────
  const legL=new THREE.Mesh(new THREE.BoxGeometry(.11,1.18,.14), blackM);
  legL.position.set(-.12,.64,0);
  g.add(legL); g.userData.legL=legL;

  const legR=new THREE.Mesh(new THREE.BoxGeometry(.11,1.18,.14), blackM);
  legR.position.set(.12,.64,0);
  g.add(legR); g.userData.legR=legR;

  // Bone-exposed shins
  [-0.12,0.12].forEach((ox,i)=>{
    const shin=new THREE.Mesh(new THREE.BoxGeometry(.04,.5,.04), boneM);
    shin.position.set(ox,.38,.08); g.add(shin);
  });

  // Bare feet / rotted
  [-0.12,0.12].forEach(ox=>{
    const foot=new THREE.Mesh(new THREE.BoxGeometry(.1,.06,.18), darkM);
    foot.position.set(ox,.04,.06); g.add(foot);
  });

  // ── ARMS: wildly different poses — one dragging, one reaching ───
  // Left arm — dragging low, limp
  const armLU=new THREE.Mesh(new THREE.BoxGeometry(.08,.72,.1), skinM);
  armLU.position.set(-.26,1.72,0); armLU.rotation.z=.35; armLU.rotation.x=.1;
  g.add(armLU); g.userData.armLU=armLU;

  const armLD=new THREE.Mesh(new THREE.BoxGeometry(.07,.62,.09), skinM);
  armLD.position.set(-.4,1.14,.0); armLD.rotation.z=.1;
  g.add(armLD); g.userData.armLD=armLD;

  // Left hand — clawed, bone showing
  const handL=new THREE.Mesh(new THREE.BoxGeometry(.1,.11,.08), skinM);
  handL.position.set(-.46,.76,0); g.add(handL); g.userData.handL=handL;
  [-.05,-.017,.018,.055].forEach((ox,fi)=>{
    const fl=[.1,.13,.12,.09][fi];
    const fing=new THREE.Mesh(new THREE.BoxGeometry(.02,fl,.022), boneM);
    fing.position.set(-.46+ox,.72-fl*.5,0);
    fing.rotation.z=-.08; g.add(fing);
  });

  // Right arm — reaching forward aggressively, higher
  const armRU=new THREE.Mesh(new THREE.BoxGeometry(.08,.7,.1), skinM);
  armRU.position.set(.25,1.8,.04); armRU.rotation.z=-.2; armRU.rotation.x=-.85;
  g.add(armRU); g.userData.armRU=armRU;

  const armRD=new THREE.Mesh(new THREE.BoxGeometry(.07,.6,.09), skinM);
  armRD.position.set(.3,1.38,.5); armRD.rotation.x=-.55;
  g.add(armRD); g.userData.armRD=armRD;

  const handR=new THREE.Mesh(new THREE.BoxGeometry(.1,.11,.08), skinM);
  handR.position.set(.32,1.1,.75); g.add(handR); g.userData.handR=handR;
  [-.05,-.017,.018,.055].forEach((ox,fi)=>{
    const fl=[.1,.13,.12,.09][fi];
    const fing=new THREE.Mesh(new THREE.BoxGeometry(.02,fl,.022), boneM);
    fing.position.set(.32+ox,1.08-.02,(.75+fl*.7));
    fing.rotation.x=-.9; g.add(fing);
  });

  return g;
}

const monsters=[];
function spawnM(type,x,z){
  const mesh=type==='slender'?mkSlender():mkZombie();
  mesh.position.set(x,0,z); scene.add(mesh);
  // Cache each child's base emissive color for hit-flash restore
  mesh.traverse(ch=>{
    if(ch.isMesh && ch.material && ch.material.emissive){
      ch.userData.baseEmissive = ch.material.emissive.getHex();
    }
  });
  monsters.push({mesh,type,
    speed:type==='slender'?2.2:3.4,
    range:type==='slender'?10:8,
    kill:.92,phase:Math.random()*100,angry:false});
}
spawnM('slender',40,40);
[[-30,22],[24,-30],[-22,-22],[34,-12],[-14,34],[16,26],[-34,-6],[28,-28],[10,-16],[-8,22],[20,8],[-6,-30]].forEach(([x,z])=>spawnM('zombie',x,z));

// ════════════════════════════════════════════
// PLAYER HAND (Minecraft-style, attached to camera)
// ════════════════════════════════════════════
const handGroup = new THREE.Group();
camera.add(handGroup);
scene.add(camera); // camera in scene so its children render

// ── FLASHLIGHT HAND MODEL ────────────────────────────────────────
// Built from cylinders + boxes to look like a real torch
// All in camera-local space so it stays bottom-right of screen
const torchGroup = new THREE.Group();

// Grip / handle — dark rubberized
const gripMat  = new THREE.MeshLambertMaterial({color:0x111111});
const bodyMat  = new THREE.MeshLambertMaterial({color:0x1a1a1a});
const lensMat  = new THREE.MeshLambertMaterial({color:0x88aaff, emissive:0x4488ff, emissiveIntensity: flashOn?1.2:0});
const rimMat   = new THREE.MeshLambertMaterial({color:0x333333});
const gripTex  = new THREE.MeshLambertMaterial({color:0x0d0d0d});

// SKIN — right hand holding the torch (visible wrist/thumb)
const skinM2   = new THREE.MeshLambertMaterial({color:0xc8824a});

// Wrist / back of hand
const wrist = new THREE.Mesh(new THREE.BoxGeometry(.09,.07,.12), skinM2);
wrist.position.set(0,0,.02); torchGroup.add(wrist);
// Thumb wrapping
const thumb2 = new THREE.Mesh(new THREE.BoxGeometry(.04,.05,.08), skinM2);
thumb2.position.set(-.058,.01,.02); thumb2.rotation.z=.4; torchGroup.add(thumb2);
// Fingers curled around grip (4 small boxes)
[.02,.006,-.006,-.018].forEach((oy,i)=>{
  const fi=new THREE.Mesh(new THREE.BoxGeometry(.085,.028,.1), skinM2);
  fi.position.set(.0, -.02-i*.022, .03); torchGroup.add(fi);
});

// Torch body — long cylinder along Z axis (pointing forward = -Z in camera space)
const body = new THREE.Mesh(new THREE.CylinderGeometry(.028,.032,.28,10), bodyMat);
body.rotation.x = Math.PI/2; body.position.set(0,0,-.08); torchGroup.add(body);

// Grip knurling rings
[-.05,-.02,.01].forEach(oz=>{
  const ring = new THREE.Mesh(new THREE.CylinderGeometry(.034,.034,.018,10), gripTex);
  ring.rotation.x=Math.PI/2; ring.position.set(0,0,oz+.04); torchGroup.add(ring);
});

// Head / reflector housing — wider flared end
const head2 = new THREE.Mesh(new THREE.CylinderGeometry(.048,.032,.06,12), rimMat);
head2.rotation.x=Math.PI/2; head2.position.set(0,0,-.22); torchGroup.add(head2);

// Lens — glowing circle at front
const lens = new THREE.Mesh(new THREE.CylinderGeometry(.042,.042,.01,12), lensMat);
lens.rotation.x=Math.PI/2; lens.position.set(0,0,-.25); torchGroup.add(lens);

// Bezel rim around lens
const bezel = new THREE.Mesh(new THREE.CylinderGeometry(.05,.05,.015,12), rimMat);
bezel.rotation.x=Math.PI/2; bezel.position.set(0,0,-.248); torchGroup.add(bezel);

// Position group in camera space — bottom right, angled like holding a torch
torchGroup.position.set(.2, -.26, -.32);
torchGroup.rotation.set(.18, -.12, -.06);

handGroup.add(torchGroup);

// Punch animation state
let punchT = 0, punching = false, punchDir = 1;

function updateHand(dt){
  if(punching){
    punchT += dt * 9 * punchDir;
    if(punchT>=1){ punchT=1; punchDir=-1; }
    if(punchT<=0){ punchT=0; punching=false; punchDir=1; }
  }
  const swing = Math.sin(punchT*Math.PI);
  // Torch jabs forward on punch
  torchGroup.position.z = -.32 - swing*.12;
  torchGroup.position.y = -.26 - swing*.03;
  torchGroup.rotation.x =  .18 - swing*.25;

  // Lens glow reflects flashOn state
  lensMat.emissiveIntensity = flashOn ? (0.9+Math.random()*.3) : 0;
  lensMat.color.setHex(flashOn ? 0xaaccff : 0x223344);

  // Walking bob
  if(isMoving){
    const bob=Math.sin(bobTime*.5)*.007;
    torchGroup.position.y = -.26 + bob - swing*.03;
    torchGroup.position.x = .2 + Math.sin(bobTime*.5)*.003;
  }
}

// ════════════════════════════════════════════
// INPUT
// ════════════════════════════════════════════
const keys={};
document.addEventListener('keydown',e=>{
  keys[e.code]=true;
  if(e.code==='Space') e.preventDefault();
  if(e.code==='KeyV') toggleCamMode();
});
document.addEventListener('keyup',  e=>keys[e.code]=false);

let yaw=0,pitch=0,mouseLocked=false,gameStarted=false,isDead=false;
let isTeleporting=false; // freeze movement during door transition

// ── CAMERA MODES ──────────────────────────────────────────────────
// 0 = first person, 1 = third person back
let camMode = 0;
const tpDistance = 3.5;   // how far behind player
const tpHeight   = 1.2;   // how high above player

// Player body mesh (visible in 3rd person) — blocky humanoid
const playerBody = new THREE.Group();
(()=>{
  const sk = new THREE.MeshLambertMaterial({color:0xd4935a});
  const cl = new THREE.MeshLambertMaterial({color:0x2244aa}); // blue shirt
  const pt = new THREE.MeshLambertMaterial({color:0x222233}); // dark pants
  const sh = new THREE.MeshLambertMaterial({color:0x111111}); // shoes

  // Head
  const head=new THREE.Mesh(new THREE.BoxGeometry(.28,.28,.28),sk);
  head.position.y=1.58; playerBody.add(head);
  // Eyes
  [-0.07,0.07].forEach(ox=>{
    const eye=new THREE.Mesh(new THREE.BoxGeometry(.06,.06,.02),new THREE.MeshLambertMaterial({color:0x222288}));
    eye.position.set(ox,1.6,.145); playerBody.add(eye);
  });
  // Torso
  const torso=new THREE.Mesh(new THREE.BoxGeometry(.34,.44,.22),cl);
  torso.position.y=1.14; playerBody.add(torso);
  // Arms
  [-0.22,0.22].forEach(ox=>{
    const arm=new THREE.Mesh(new THREE.BoxGeometry(.12,.38,.14),cl);
    arm.position.set(ox,1.14,0); playerBody.add(arm);
    const hand=new THREE.Mesh(new THREE.BoxGeometry(.1,.1,.1),sk);
    hand.position.set(ox,.88,0); playerBody.add(hand);
  });
  // Legs
  [-0.09,0.09].forEach(ox=>{
    const leg=new THREE.Mesh(new THREE.BoxGeometry(.15,.52,.18),pt);
    leg.position.set(ox,.6,0); playerBody.add(leg);
    const shoe=new THREE.Mesh(new THREE.BoxGeometry(.16,.08,.2),sh);
    shoe.position.set(ox,.28,.02); playerBody.add(shoe);
  });
})();
playerBody.visible = false;
scene.add(playerBody);

// Pivot object that the 3rd-person camera hangs off
const camPivot = new THREE.Object3D();
scene.add(camPivot);

function toggleCamMode(){
  camMode = camMode===0 ? 1 : 0;
  playerBody.visible = camMode===1;
  // Hide/show hand in first person only
  handGroup.visible = camMode===0;
}

// Physics
const SH=1.65, CH=.88;
let playerY=SH, velY=0, onGround=true, isCrouching=false;
const GRAV=-15, JMP=5.8;

let stamina=100, sanity=100, sanTmr=0;
let hp=100, hpTmr=0, dmgFlashTmr=0, regenTmr=0;
const BS=3.5, SS=6.8, CS=1.7;
const PR=.38; // player radius

document.addEventListener('pointerlockchange',()=>{ mouseLocked=document.pointerLockElement===canvas; });
canvas.addEventListener('click',()=>{ if(gameStarted&&!isDead) canvas.requestPointerLock(); });

document.addEventListener('mousemove',e=>{
  if(!mouseLocked||!gameStarted)return;
  yaw   -= e.movementX*.002;
  pitch -= e.movementY*.002;
  pitch=Math.max(-Math.PI/2.1,Math.min(Math.PI/2.1,pitch));
});

// LMB = punch, RMB = door
document.addEventListener('mousedown',e=>{
  if(!gameStarted||!mouseLocked)return;
  if(e.button===0){
    punching=true; punchDir=1; punchT=0;
    // Hit detection — raycast from camera against all monster meshes
    const ray = new THREE.Raycaster();
    ray.setFromCamera(new THREE.Vector2(0,0), camera);
    const meshes = monsters.filter(m=>m.hp>0).map(m=>m.mesh);
    // Also cast against all children of each group
    const allMeshes = [];
    monsters.forEach(m=>{ if(m.hp>0){ allMeshes.push(m.mesh); m.mesh.traverse(ch=>{ if(ch.isMesh) allMeshes.push(ch); }); } });
    const hits = ray.intersectObjects(allMeshes, false);
    if(hits.length>0 && hits[0].distance < 2.5){
      // Find which monster was hit
      const hitObj = hits[0].object;
      const hitMonster = monsters.find(m=>{
        if(!m.hp||m.hp<=0) return false;
        if(m.mesh===hitObj) return true;
        let found=false; m.mesh.traverse(ch=>{ if(ch===hitObj) found=true; }); return found;
      });
      if(hitMonster){
        const dmg = 25 + Math.random()*15;
        hitMonster.hp -= dmg;
        hitMonster.hitFlash = 0.4;
        playPunch();
        // Knockback
        const kdx = hitMonster.mesh.position.x - camera.position.x;
        const kdz = hitMonster.mesh.position.z - camera.position.z;
        const kd = Math.sqrt(kdx*kdx+kdz*kdz)||1;
        hitMonster.mesh.position.x += kdx/kd*0.8;
        hitMonster.mesh.position.z += kdz/kd*0.8;
        // Kill monster
        if(hitMonster.hp<=0){
          scene.remove(hitMonster.mesh);
          hitMonster.hp=0;
        }
      }
    }
  }
  if(e.button===2){ // right click door
    const d=getDoorInFront();
    if(d) toggleDoor(d);
  }
});
document.addEventListener('contextmenu',e=>e.preventDefault());

// ════════════════════════════════════════════
// COLLISION (grid-based, fast)
// ════════════════════════════════════════════
function gc(x,z){
  const gx=Math.floor((x+MHALF)/CELL), gz=Math.floor((z+MHALF)/CELL);
  return{gx:Math.max(0,Math.min(GRID-1,gx)),gz:Math.max(0,Math.min(GRID-1,gz))};
}
function cellBlocked(x,z,radius){
  // Check a single world point with given radius against maze walls
  if(Math.abs(x)>=MHALF-radius||Math.abs(z)>=MHALF-radius) return true;
  const gx=Math.max(0,Math.min(GRID-1,Math.floor((x+MHALF)/CELL)));
  const gz=Math.max(0,Math.min(GRID-1,Math.floor((z+MHALF)/CELL)));
  const lx=(x+MHALF)-gx*CELL, lz=(z+MHALF)-gz*CELL;
  const W=0.14+radius;
  if(wls[gx][gz].N&&lz<W)      return true;
  if(wls[gx][gz].S&&lz>CELL-W) return true;
  if(wls[gx][gz].W&&lx<W)      return true;
  if(wls[gx][gz].E&&lx>CELL-W) return true;
  return false;
}

function canGo(x,z){
  // Sample 4 corners of player circle to catch diagonal wall catches
  if(cellBlocked(x,z,PR)) return false;
  // Extra corner samples prevent sticking in corners
  const s = PR*0.72;
  if(cellBlocked(x+s,z+s,0.01)) return false;
  if(cellBlocked(x-s,z+s,0.01)) return false;
  if(cellBlocked(x+s,z-s,0.01)) return false;
  if(cellBlocked(x-s,z-s,0.01)) return false;
  return true;
}

// ════════════════════════════════════════════
// AUDIO
// ════════════════════════════════════════════
let AC=null, lastStep=0;
function initAudio(){ AC=new(window.AudioContext||window.webkitAudioContext)(); }
function nbuf(dur,vol,ff){
  if(!AC)return;
  const b=AC.createBuffer(1,AC.sampleRate*dur,AC.sampleRate), d=b.getChannelData(0);
  for(let i=0;i<d.length;i++) d[i]=(Math.random()*2-1)*Math.exp(-i/(d.length*.35))*vol;
  const s=AC.createBufferSource(); s.buffer=b;
  const g=AC.createGain(); g.gain.value=1;
  const f=AC.createBiquadFilter(); f.type='lowpass'; f.frequency.value=ff;
  s.connect(f);f.connect(g);g.connect(AC.destination);s.start();
}
function playStep(c){ if(!AC)return; const now=AC.currentTime,r=c?.7:.46; if(now-lastStep<r)return; lastStep=now; nbuf(.1,c?.09:.25,c?180:360); }
function playPunch(){ nbuf(.06,.18,600); }
function playDoor(){ nbuf(.3,.25,300); }
function playGrowl(){
  if(!AC)return;
  const b=AC.createBuffer(1,AC.sampleRate*.85,AC.sampleRate), d=b.getChannelData(0);
  for(let i=0;i<d.length;i++) d[i]=(Math.random()*2-1)*Math.sin(i/d.length*Math.PI)*.42;
  const s=AC.createBufferSource(); s.buffer=b;
  const g=AC.createGain(); g.gain.value=.62;
  const f=AC.createBiquadFilter(); f.type='lowpass'; f.frequency.value=170;
  s.connect(f);f.connect(g);g.connect(AC.destination);s.start();
}
function playHum(){
  if(!AC)return;
  [[60,'sawtooth',.014],[120,'sine',.006],[180,'sine',.003]].forEach(([fr,tp,vol])=>{
    const o=AC.createOscillator(),g=AC.createGain(),f=AC.createBiquadFilter();
    o.type=tp;o.frequency.value=fr;g.gain.value=vol;f.type='bandpass';f.frequency.value=fr;f.Q.value=2;
    o.connect(f);f.connect(g);g.connect(AC.destination);o.start();
  });
}
function playScream(){
  if(!AC)return;
  [260,520,780,1040].forEach(fr=>{
    const o=AC.createOscillator(),g=AC.createGain();
    o.frequency.value=fr+Math.random()*160;o.type='sawtooth';
    g.gain.setValueAtTime(.32,AC.currentTime);
    g.gain.exponentialRampToValueAtTime(.001,AC.currentTime+1.4);
    o.connect(g);g.connect(AC.destination);o.start();o.stop(AC.currentTime+1.4);
  });
}

// ════════════════════════════════════════════
// FLICKER
// ════════════════════════════════════════════
const flkEl=document.getElementById('flk');
let nxtFlk=7+Math.random()*10, flkTmr=0;
function doFlicker(dt){
  flkTmr+=dt;
  if(flkTmr>nxtFlk){flkTmr=0;nxtFlk=5+Math.random()*14;trigFlk();}
  lights.forEach(({l,b,p})=>{
    l.intensity=b+Math.sin(Date.now()*.0037+p)*.1+(Math.random()<.004?Math.random()*.35-.17:0);
  });
}
function trigFlk(){
  const sq=[100,12,100,3,100,30,100,0,100,50,100]; let t=0;
  sq.forEach(op=>{
    setTimeout(()=>{
      ambL.intensity=op>40?.18:0;
      lights.forEach(({l,b})=>l.intensity=op>40?b:0);
      flkEl.style.opacity=(100-op)/100*.22;
    },t); t+=30+Math.random()*50;
  });
  setTimeout(()=>flkEl.style.opacity='0',t+160);
}

// ════════════════════════════════════════════
// JUMPSCARE + DEATH
// ════════════════════════════════════════════
const jscEl=document.getElementById('jscare');
let jscActive=false;
function trigJsc(cb){
  if(jscActive)return; jscActive=true; playScream();
  jscEl.style.opacity='1';
  document.getElementById('scare').style.opacity='.78';
  setTimeout(()=>{ jscEl.style.opacity='0'; document.getElementById('scare').style.opacity='0'; jscActive=false; if(cb)cb(); },1000);
}
function killPl(){
  if(isDead)return; isDead=true;
  trigJsc(()=>{ document.getElementById('death').classList.add('show'); document.exitPointerLock(); });
}
document.getElementById('rBtn').addEventListener('click',()=>location.reload());

// ════════════════════════════════════════════
// MONSTERS UPDATE
// ════════════════════════════════════════════
let growTmr=0;
// ── Stealth: crouching + still = invisible to monsters ──────────────────
// Player is detected if: running, walking, OR not crouching
// Stealth radius shrinks to 0 when crouching & not moving
function playerDetectRange(baseRange){
  if(isCrouching && !isMoving) return 0;         // fully invisible
  if(isCrouching && isMoving)  return baseRange * 0.3;  // very hard to detect
  if(isMoving){
    const sprinting = keys['KeyE'] || keys['KeyR'];
    return sprinting ? baseRange * 1.4 : baseRange; // running = louder
  }
  return baseRange * 0.6; // standing still, not crouching = some detection
}

// Monster wall collision — same grid logic as player
const MR = 0.35; // monster collision radius
function monsterCanGo(x, z){
  if(Math.abs(x) >= MHALF - MR || Math.abs(z) >= MHALF - MR) return false;
  const gx = Math.max(0,Math.min(GRID-1, Math.floor((x+MHALF)/CELL)));
  const gz = Math.max(0,Math.min(GRID-1, Math.floor((z+MHALF)/CELL)));
  const lx = (x+MHALF) - gx*CELL;
  const lz = (z+MHALF) - gz*CELL;
  const W = 0.14 + MR;
  if(wls[gx][gz].N && lz < W)      return false;
  if(wls[gx][gz].S && lz > CELL-W) return false;
  if(wls[gx][gz].W && lx < W)      return false;
  if(wls[gx][gz].E && lx > CELL-W) return false;
  return true;
}

// ── Per-monster wander state ────────────────────────────────────────
// Each monster has a wander target it walks toward, then picks a new one
monsters.forEach(m=>{
  m.wanderTarget = { x: m.mesh.position.x, z: m.mesh.position.z };
  m.wanderTimer  = 0;
  m.hp           = m.type==='slender' ? 150 : 80;   // monster HP for hitting
  m.walkCycle    = 0;  // drives limb animation
  m.hitFlash     = 0;  // red flash timer when hit
});

function pickWanderTarget(m){
  // Pick a random direction and walk up to 8 units
  const angle = Math.random()*Math.PI*2;
  const dist  = 3 + Math.random()*8;
  m.wanderTarget.x = m.mesh.position.x + Math.cos(angle)*dist;
  m.wanderTarget.z = m.mesh.position.z + Math.sin(angle)*dist;
  m.wanderTarget.x = Math.max(-MHALF+1, Math.min(MHALF-1, m.wanderTarget.x));
  m.wanderTarget.z = Math.max(-MHALF+1, Math.min(MHALF-1, m.wanderTarget.z));
  m.wanderTimer = 2 + Math.random()*4;
}

// ── Walking limb animation — uses named userData bones ─────────────
function animateMonsterWalk(m, speedMult){
  m.walkCycle += 0.05 * speedMult;
  const t = m.walkCycle;
  const sw = Math.sin(t);
  const ud = m.mesh.userData;

  if(m.type==='slender'){
    // Slenderman: eerie glide — very slight arm sway, legs nearly invisible under suit
    if(ud.legL) ud.legL.rotation.x =  sw * .22;
    if(ud.legR) ud.legR.rotation.x = -sw * .22;
    // Long arms swing gently — creepy
    if(ud.armLU){ ud.armLU.rotation.x = sw * .14; ud.armLU.rotation.z = .25 + sw*.05; }
    if(ud.armRU){ ud.armRU.rotation.x =-sw * .14; ud.armRU.rotation.z =-.25 - sw*.05; }
    if(ud.armLD){ ud.armLD.rotation.x = sw * .1; }
    if(ud.armRD){ ud.armRD.rotation.x =-sw * .1; }
    // Torso slow sway
    m.mesh.rotation.z = sw * .025;
    m.mesh.position.y = Math.abs(sw) * .04;

  } else {
    // Zombie: violent uneven shamble
    const stagger = Math.sin(t*1.3+1.2); // offset cycle for stagger
    // Legs — heavy uneven limp
    if(ud.legL){ ud.legL.rotation.x = sw * .55; ud.legL.rotation.z = .04; }
    if(ud.legR){ ud.legR.rotation.x =-sw * .45; ud.legR.rotation.z =-.04; }
    // Left arm dragging / swinging up
    if(ud.armLU){ ud.armLU.rotation.x = .1 + sw*.4; ud.armLU.rotation.z = .35+sw*.1; }
    if(ud.armLD){ ud.armLD.rotation.x = sw*.3; }
    if(ud.handL){ ud.handL.rotation.z = sw*.2; }
    // Right arm — stabbing forward reach, cycles with walking
    if(ud.armRU){ ud.armRU.rotation.x = -.85 + sw*.25; }
    if(ud.armRD){ ud.armRD.rotation.x = -.55 + sw*.2; }
    if(ud.handR){ ud.handR.rotation.x = -.2 + sw*.15; }
    // Torso lurch — violent shamble
    if(ud.torso){ ud.torso.rotation.x = .22 + Math.abs(sw)*.12; }
    // Head lolls
    if(ud.head){ ud.head.rotation.z = stagger*.12; ud.head.rotation.x = .1+Math.abs(sw)*.08; }
    // Jaw clacks open and shut
    if(ud.jaw)  ud.jaw.rotation.x = .55 + Math.abs(sw)*.35;
    // Body sway
    m.mesh.rotation.z = stagger * .1;
    m.mesh.position.y = Math.abs(Math.sin(t*2)) * .06;
  }

  // Hit flash
  if(m.hitFlash > 0){
    m.hitFlash -= 0.016;
    m.mesh.traverse(ch=>{ if(ch.isMesh && ch.material.emissive) ch.material.emissive.setHex(0x660000); });
  } else {
    // restore each material's original emissive (stored at spawn)
    m.mesh.traverse(ch=>{ if(ch.isMesh && ch.material.emissive) ch.material.emissive.setHex(ch.userData.baseEmissive||0x000000); });
  }
}

function updMonsters(dt){
  if(isDead)return;
  const px=camera.position.x, pz=camera.position.z;
  growTmr-=dt;

  monsters.forEach(m=>{
    if(m.hp<=0) return;  // dead monster, skip

    const dx=px-m.mesh.position.x, dz=pz-m.mesh.position.z;
    const dist=Math.sqrt(dx*dx+dz*dz);
    const effectiveRange = playerDetectRange(m.range);
    const isChasing = dist < effectiveRange;

    if(isChasing){
      // ── CHASE MODE ───────────────────────────────────────────────
      if(!m.angry){ playGrowl(); }
      m.angry = true;
      if(growTmr<=0 && dist<11){ playGrowl(); growTmr=2.2+Math.random()*3.2; }

      // Face player smoothly
      const targetYaw = Math.atan2(dx, dz);
      m.mesh.rotation.y = targetYaw;

      // Move toward player
      const spd = m.speed * dt;
      if(dist > 0.1){
        const ndx=dx/dist, ndz=dz/dist;
        const nx=m.mesh.position.x+ndx*spd, nz=m.mesh.position.z+ndz*spd;
        if(monsterCanGo(nx,nz)){
          m.mesh.position.x=nx; m.mesh.position.z=nz;
        } else if(monsterCanGo(nx,m.mesh.position.z)){
          m.mesh.position.x=nx;
        } else if(monsterCanGo(m.mesh.position.x,nz)){
          m.mesh.position.z=nz;
        }
      }
      animateMonsterWalk(m, m.type==='slender'?1.2:2.2);

      // Damage player on touch
      // High damage — very easy to die
      const dmgRate = m.type==='slender' ? 45 : 32;
      if(dist < m.kill+0.6){
        hpTmr+=dt;
        if(hpTmr>0.06){ hpTmr=0; takeHit(dmgRate*0.06); }
      }

    } else {
      // ── WANDER MODE ──────────────────────────────────────────────
      // Lose anger when player successfully hides
      if(m.angry && isCrouching && !isMoving){
        m.angry = false;
        m.wanderTimer = 0; // immediately pick new wander target
      }

      // Tick wander timer, pick new target when expired
      m.wanderTimer -= dt;
      if(m.wanderTimer <= 0) pickWanderTarget(m);

      // Walk toward wander target
      const wdx = m.wanderTarget.x - m.mesh.position.x;
      const wdz = m.wanderTarget.z - m.mesh.position.z;
      const wdist = Math.sqrt(wdx*wdx + wdz*wdz);

      if(wdist > 0.3){
        // Face wander direction
        m.mesh.rotation.y = Math.atan2(wdx, wdz);
        const wspd = m.speed * 0.32 * dt;  // wander at 32% of chase speed
        const wx = m.mesh.position.x + (wdx/wdist)*wspd;
        const wz = m.mesh.position.z + (wdz/wdist)*wspd;
        if(monsterCanGo(wx, wz)){
          m.mesh.position.x = wx; m.mesh.position.z = wz;
        } else if(monsterCanGo(wx, m.mesh.position.z)){
          m.mesh.position.x = wx;
          pickWanderTarget(m); // hit wall, pick new target
        } else if(monsterCanGo(m.mesh.position.x, wz)){
          m.mesh.position.z = wz;
          pickWanderTarget(m);
        } else {
          pickWanderTarget(m); // fully stuck, try new direction
        }
        animateMonsterWalk(m, m.type==='slender'?0.5:0.9);
      } else {
        // Reached target — pause then wander again
        m.wanderTimer = Math.max(m.wanderTimer, 0.5);
        m.mesh.rotation.z *= 0.9;  // settle
      }
    }
  });
  // blood overlay handled by updHP()
}

// ════════════════════════════════════════════
// SANITY + HUD
// ════════════════════════════════════════════
// ════════════════════════════════════════════
// HP SYSTEM
// ════════════════════════════════════════════
function takeHit(amount){
  if(isDead) return;
  hp = Math.max(0, hp - amount);
  dmgFlashTmr = 0.25;   // red flash duration
  regenTmr = 0;          // reset regen cooldown
  if(hp <= 0) killPl();
}

function updHP(dt){
  // Red flash on damage
  if(dmgFlashTmr > 0){
    dmgFlashTmr -= dt;
    document.getElementById('bld').style.opacity = String(Math.min(.55, dmgFlashTmr * 2.2));
  }
  // Slow regen after 5 seconds out of danger
  const nearMonster = monsters.some(m=>{
    const dx=camera.position.x-m.mesh.position.x, dz=camera.position.z-m.mesh.position.z;
    return Math.sqrt(dx*dx+dz*dz) < m.range + 1;
  });
  if(!nearMonster && hp < 100){
    regenTmr += dt;
    if(regenTmr > 10){ hp = Math.min(100, hp + 1.2*dt); }
  } else if(nearMonster){
    regenTmr = 0;
  }
}

function updSanity(dt){
  sanTmr+=dt;
  const near=monsters.some(m=>{ const dx=camera.position.x-m.mesh.position.x,dz=camera.position.z-m.mesh.position.z; return Math.sqrt(dx*dx+dz*dz)<12; });
  if(sanTmr>(near?.75:2.4)){ sanTmr=0; sanity=Math.max(0,sanity-(near?2.2:.45)); }
  if(sanity<40){ camera.fov=75+(40-sanity)*.42; camera.updateProjectionMatrix(); }
  if(sanity<20){ scene.fog.near=1+(sanity/20); }
}
const statsEl=document.getElementById('stats'), stagEl=document.getElementById('stag'), promptEl=document.getElementById('prompt');
function updHUD(){
  // Only warn if monster has actually detected us (respects stealth)
  const near = monsters.some(m => m.angry);
  const hidden = isCrouching && !isMoving;
  const sc=sanity<25?'#8b0000':sanity<55?'#aa5500':'#c8a84b66';
  const stc=stamina<20?'#8b4000':stamina<50?'#887700':'#c8a84b66';
  const hc = hp<25?'#cc0000':hp<55?'#cc5500':'#44cc44';
  const hbar = '█'.repeat(Math.round(hp/10)) + '░'.repeat(10-Math.round(hp/10));
  const lvlName = levelThemes[currentLevel]?.name.split('—')[0].trim() || 'LEVEL 0';
  statsEl.innerHTML=`<span style="color:#c8a84b44;font-size:.52rem">${lvlName}</span><br>HP &nbsp;&nbsp;&nbsp;&nbsp; <span style="color:${hc};letter-spacing:.05em">${hbar}</span> <span style="color:${hc}">${Math.round(hp)}</span><br>SANITY &nbsp;<span style="color:${sc}">${Math.round(sanity)}%</span><br>STAMINA <span style="color:${stc}">${Math.round(stamina)}%</span>`;
  if(hidden && near){
    stagEl.innerHTML=`<span style="color:#44aa44">● HIDDEN — stay still</span>`;
  } else if(hidden){
    stagEl.innerHTML=`<span style="color:#44aa44">● CROUCHING — invisible</span>`;
  } else if(near){
    stagEl.innerHTML=`<span style="color:#8b0000;animation:pulse .4s infinite">⚠ THEY ARE NEAR</span>`;
  } else {
    stagEl.innerHTML='';
  }
  // Door prompt
  const d=getDoorInFront();
  promptEl.style.opacity=d?'1':'0';
  promptEl.textContent=d?(d.open?'[ RMB ] CLOSE DOOR':'[ RMB ] OPEN DOOR'):'';
}

// ════════════════════════════════════════════
// FULLSCREEN
// ════════════════════════════════════════════
const fsBtn=document.getElementById('fsBtn');
fsBtn.addEventListener('click',()=>{
  const el=document.documentElement;
  if(!document.fullscreenElement)(el.requestFullscreen||el.webkitRequestFullscreen||el.mozRequestFullScreen).call(el);
  else (document.exitFullscreen||document.webkitExitFullscreen).call(document);
});
document.addEventListener('fullscreenchange',()=>{ fsBtn.textContent=document.fullscreenElement?'✕ EXIT FULLSCREEN':'⛶ FULLSCREEN'; });

// ════════════════════════════════════════════
// START
// ════════════════════════════════════════════
document.getElementById('startBtn').addEventListener('click',()=>{
  const ov=document.getElementById('overlay');
  ov.classList.add('hidden');
  setTimeout(()=>{
    ov.style.display='none'; gameStarted=true;
    statsEl.classList.add('show'); stagEl.classList.add('show');
    document.getElementById('hud').classList.add('show');
    document.getElementById('xhair').classList.add('show');
    canvas.requestPointerLock(); initAudio(); playHum();
  },1500);
});

// Position player in centre of spawn cell
camera.position.set(SPAWN_X, SH, SPAWN_Z);

// ════════════════════════════════════════════
// MAIN LOOP
// ════════════════════════════════════════════
const clock=new THREE.Clock();
let bobTime=0, isMoving=false;

function animate(){
  requestAnimationFrame(animate);
  const dt=Math.min(clock.getDelta(),.05);
  if(!gameStarted||isDead){ renderer.render(scene,camera); return; }

  // ── PLAYER POSITION (shared between camera modes) ────────────────
  isCrouching=!!keys['ShiftLeft'];
  const targetH=isCrouching?CH:SH;

  // Jump + gravity
  if(keys['Space']&&onGround&&!isCrouching){ velY=JMP; onGround=false; }
  if(!onGround){ velY+=GRAV*dt; playerY+=velY*dt; if(playerY<=targetH){playerY=targetH;velY=0;onGround=true;} }
  else { playerY+=(targetH-playerY)*.15; }

  // Sprint
  const spr=(keys['KeyE']||keys['KeyR'])&&stamina>0&&!isCrouching;
  if(spr) stamina=Math.max(0,stamina-19*dt); else stamina=Math.min(100,stamina+8*dt);
  const spd=isCrouching?CS:spr?SS:BS;

  // Movement vectors (always yaw-relative, same in both camera modes)
  const fwd=new THREE.Vector3(-Math.sin(yaw),0,-Math.cos(yaw));
  const rgt=new THREE.Vector3( Math.cos(yaw),0,-Math.sin(yaw));
  const dir=new THREE.Vector3();
  if(keys['KeyW']||keys['ArrowUp'])    dir.addScaledVector(fwd, 1);
  if(keys['KeyS']||keys['ArrowDown'])  dir.addScaledVector(fwd,-1);
  if(keys['KeyA']||keys['ArrowLeft'])  dir.addScaledVector(rgt,-1);
  if(keys['KeyD']||keys['ArrowRight']) dir.addScaledVector(rgt, 1);

  isMoving=dir.lengthSq()>0;
  // Player world position — always tracked as px/pz
  if(isMoving){
    dir.normalize();
    const mv=spd*dt;
    const nx=camera.position.x+dir.x*mv, nz=camera.position.z+dir.z*mv;
    if(canGo(nx,camera.position.z)) camera.position.x=nx;
    if(canGo(camera.position.x,nz)) camera.position.z=nz;
    if(onGround) playStep(isCrouching);
    bobTime+=dt*(spr?15:isCrouching?4:8);
  } else {
    bobTime*=.9;
  }

  // ── CAMERA MODES ───────────────────────────────────────────────
  if(camMode===0){
    // First person
    camera.position.y = playerY + (onGround&&isMoving ? Math.sin(bobTime)*(isCrouching?.012:.038) : 0);
    camera.quaternion.setFromEuler(new THREE.Euler(pitch,yaw,0,'YXZ'));
    playerBody.visible = false;
    handGroup.visible  = true;

  } else {
    // Third person — back view
    // Player body follows movement position
    playerBody.position.set(camera.position.x, playerY - SH, camera.position.z);
    playerBody.rotation.y = yaw + Math.PI; // face forward (away from camera)

    // Bob player body when walking
    if(isMoving && onGround){
      playerBody.position.y += Math.sin(bobTime)*.04;
    }

    // Camera orbits behind player at tpDistance
    const behindX = camera.position.x - Math.sin(yaw)*tpDistance;
    const behindZ = camera.position.z - Math.cos(yaw)*tpDistance;
    const camTPY  = playerY + tpHeight;

    // Smooth camera lerp to behind position
    camera.position.x += (behindX - camera.position.x) * .18;
    camera.position.z += (behindZ - camera.position.z) * .18;
    camera.position.y += (camTPY  - camera.position.y) * .18;

    // Look at player from behind and above
    const lookTarget = new THREE.Vector3(
      playerBody.position.x,
      playerBody.position.y + 1.4,
      playerBody.position.z
    );
    camera.lookAt(lookTarget);
    playerBody.visible = true;
    handGroup.visible  = false;
  }

  updateHand(dt);
  updateDoors(dt);
  updateFlashlight();
  doFlicker(dt);
  updMonsters(dt);
  updHP(dt);
  updSanity(dt);
  updHUD();
  renderer.render(scene,camera);
}
animate();

window.addEventListener('resize',()=>{
  camera.aspect=innerWidth/innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(innerWidth,innerHeight);
});
</script>
</body>
</html>
