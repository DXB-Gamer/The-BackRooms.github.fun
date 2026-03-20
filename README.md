<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>The Backrooms — Download</title>
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Creepster&display=swap" rel="stylesheet"/>
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
html{height:100%;background:#0c0a04}
body{
  min-height:100vh;
  background:#0c0a04;
  color:#d4b84a;
  font-family:'Share Tech Mono',monospace;
  overflow-x:hidden;
  cursor:crosshair;
}
.corridor{
  position:fixed;inset:0;z-index:0;
  background:
    repeating-linear-gradient(0deg,transparent,transparent 59px,rgba(180,140,30,0.07) 60px),
    repeating-linear-gradient(90deg,transparent,transparent 59px,rgba(180,140,30,0.07) 60px);
  background-size:60px 60px;
  animation:drift 30s linear infinite;
}
@keyframes drift{from{background-position:0 0}to{background-position:0 60px}}
.vignette{
  position:fixed;inset:0;z-index:1;
  background:radial-gradient(ellipse at center,transparent 30%,rgba(0,0,0,0.92) 100%);
  pointer-events:none;
}
.scan{
  position:fixed;top:0;left:0;right:0;height:2px;z-index:2;
  background:rgba(220,190,60,0.15);
  animation:scanmove 5s linear infinite;
  pointer-events:none;
}
@keyframes scanmove{0%{top:-2px}100%{top:100vh}}
.tape{
  position:fixed;left:0;right:0;height:12px;z-index:10;
  background:repeating-linear-gradient(-45deg,#d4b84a 0,#d4b84a 10px,#0c0a04 10px,#0c0a04 20px);
  animation:flicker 7s ease-in-out infinite;
}
.tape.top{top:0}.tape.bot{bottom:0}
@keyframes flicker{0%,100%{opacity:1}91%{opacity:1}93%{opacity:.3}95%{opacity:1}97%{opacity:.6}99%{opacity:1}}
.page{
  position:relative;z-index:5;
  min-height:100vh;
  display:flex;flex-direction:column;align-items:center;justify-content:center;
  padding:80px 20px;
}
.hero{text-align:center;animation:fadeup .9s ease both}
@keyframes fadeup{from{opacity:0;transform:translateY(28px)}to{opacity:1;transform:translateY(0)}}
.eyebrow{
  font-size:.65rem;letter-spacing:.4em;text-transform:uppercase;
  color:#6b5515;border:1px solid #3a2e0a;
  display:inline-block;padding:5px 18px;margin-bottom:28px;
}
.title{
  font-family:'Creepster',cursive;
  font-size:clamp(4rem,14vw,9rem);
  line-height:.9;color:#d4b84a;
  animation:glow 4s ease-in-out infinite;
}
@keyframes glow{
  0%,100%{text-shadow:0 0 20px rgba(212,184,74,.2),0 0 60px rgba(212,184,74,.08)}
  50%{text-shadow:0 0 50px rgba(212,184,74,.4),0 0 120px rgba(212,184,74,.2)}
}
.sub{
  display:block;
  font-family:'Share Tech Mono',monospace;
  font-size:clamp(.55rem,.9vw,.75rem);
  letter-spacing:.35em;color:#6b5515;
  text-transform:uppercase;margin-top:10px;
}
.hr{
  width:100%;max-width:640px;height:1px;
  background:linear-gradient(90deg,transparent,#3a2e0a,rgba(212,184,74,.5),#3a2e0a,transparent);
  margin:44px 0 36px;
  animation:fadeup .9s .1s ease both;
}
.lore{
  max-width:560px;width:100%;text-align:center;
  font-size:.82rem;line-height:2;color:#8c7530;
  animation:fadeup .9s .15s ease both;
}
.lore em{color:#d4b84a;font-style:normal}
.stats{
  display:flex;gap:2px;max-width:560px;width:100%;
  margin:36px 0 40px;
  animation:fadeup .9s .2s ease both;
  flex-wrap:wrap;
}
.stat{
  flex:1;min-width:100px;
  background:rgba(212,184,74,.04);
  border:1px solid rgba(212,184,74,.12);
  padding:18px 12px;text-align:center;
  transition:background .2s;
}
.stat:hover{background:rgba(212,184,74,.08)}
.sv{display:block;font-family:'Creepster',cursive;font-size:2rem;color:#d4b84a;line-height:1}
.sl{display:block;font-size:.58rem;letter-spacing:.25em;text-transform:uppercase;color:#4a3b10;margin-top:5px}
.dl-wrap{
  display:flex;flex-direction:column;align-items:center;gap:14px;
  animation:fadeup .9s .28s ease both;
}
.dl-label{font-size:.62rem;letter-spacing:.3em;text-transform:uppercase;color:#4a3b10;}
.btn-dl{
  display:flex;align-items:center;gap:16px;
  background:#d4b84a;color:#0c0a04;
  font-family:'Share Tech Mono',monospace;
  font-size:1.1rem;font-weight:700;
  letter-spacing:.18em;text-transform:uppercase;
  text-decoration:none;
  padding:22px 56px;
  border:none;cursor:pointer;
  position:relative;
  transition:transform .15s,background .15s;
}
.btn-dl::after{
  content:'';position:absolute;inset:-5px;
  border:2px solid #d4b84a;opacity:0;
  transition:opacity .2s;
}
.btn-dl:hover{background:#f5d966;transform:translateY(-3px)}
.btn-dl:hover::after{opacity:1}
.btn-dl:active{transform:translateY(0)}
.btn-dl svg{width:22px;height:22px;fill:#0c0a04;flex-shrink:0}
.dl-note{font-size:.62rem;letter-spacing:.15em;color:#4a3b10;text-align:center}
.dl-note strong{color:#8c7530}
.how{
  max-width:560px;width:100%;margin-top:52px;
  animation:fadeup .9s .35s ease both;
}
.how-title{
  font-size:.62rem;letter-spacing:.35em;text-transform:uppercase;color:#6b5515;
  border-bottom:1px solid rgba(212,184,74,.12);padding-bottom:12px;margin-bottom:20px;
}
.steps{display:flex;flex-direction:column;gap:12px}
.step{
  display:flex;align-items:flex-start;gap:16px;
  font-size:.8rem;color:#7a6420;line-height:1.7;
}
.sn{font-family:'Creepster',cursive;font-size:1.1rem;color:#d4b84a;min-width:24px;flex-shrink:0;margin-top:1px}
.keys{
  display:flex;gap:10px;margin-top:28px;flex-wrap:wrap;justify-content:center;
  animation:fadeup .9s .4s ease both;
}
.key{border:1px solid rgba(212,184,74,.2);padding:4px 12px;font-size:.65rem;letter-spacing:.15em;color:#6b5515}
footer{
  margin-top:60px;text-align:center;
  animation:fadeup .9s .45s ease both;
}
.ft-main{font-size:.62rem;letter-spacing:.2em;color:#3a2e0a}
.ft-main a{color:#3a2e0a;text-decoration:underline}
.ft-secure{
  display:inline-block;margin-top:14px;
  font-size:.65rem;letter-spacing:.15em;
  color:#3a6b3a;border:1px solid rgba(60,120,60,.25);
  padding:5px 16px;
}
.ft-dontrust{
  display:block;margin-top:16px;
  font-family:'Creepster',cursive;
  font-size:1.6rem;color:#8b2020;
  letter-spacing:.15em;
  animation:flicker 5s ease-in-out infinite;
}
@media(max-width:500px){
  .btn-dl{padding:18px 28px;font-size:.9rem}
  .stats{gap:6px}
}
</style>
</head>
<body>

<div class="corridor"></div>
<div class="vignette"></div>
<div class="scan"></div>
<div class="tape top"></div>
<div class="tape bot"></div>

<main class="page">

  <div class="hero">
    <div class="eyebrow">⚠ LEVEL 0 — CLASSIFIED</div>
    <h1 class="title">The Backrooms</h1>
    <span class="sub">No-clip into the wrong reality</span>
  </div>

  <div class="hr"></div>

  <p class="lore">
    You blinked. Now you're here.<br/>
    <em>Fluorescent lights. Wet carpet. The hum that never stops.</em><br/>
    Infinite yellow rooms. No exits. No maps. No escape.<br/>
    Something else is here with you — and it knows you're lost.
  </p>

  <div class="stats">
    <div class="stat"><span class="sv">∞</span><span class="sl">Rooms</span></div>
    <div class="stat"><span class="sv">0</span><span class="sl">Known Exits</span></div>
    <div class="stat"><span class="sv">1</span><span class="sl">HTML File</span></div>
    <div class="stat"><span class="sv">FREE</span><span class="sl">Forever</span></div>
  </div>

  <div class="dl-wrap">
    <span class="dl-label">// Save the file. Open it. Play forever offline.</span>

    <a class="btn-dl" href="https://github.com/user-attachments/files/26132553/backrooms.8.html" download="TheBackrooms.html">
      <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
        <path d="M12 15.5l-5-5h3.5V4h3v6.5H17l-5 5zM5 18h14v2H5v-2z"/>
      </svg>
      Download The Game
    </a>

    <span class="dl-note">
      Single <strong>.html</strong> file &nbsp;·&nbsp; No install &nbsp;·&nbsp; Open in any browser &nbsp;·&nbsp; Play offline anytime
    </span>
  </div>

  <div class="how">
    <div class="how-title">// HOW TO PLAY</div>
    <div class="steps">
      <div class="step"><span class="sn">1</span><span>Click <em style="color:#d4b84a">Download The Game</em> above — save the .html file anywhere on your device.</span></div>
      <div class="step"><span class="sn">2</span><span>Open the file in Chrome, Firefox, Safari or any browser. No install needed.</span></div>
      <div class="step"><span class="sn">3</span><span>The game runs fully offline. Keep it on your desktop — play anytime, even without internet.</span></div>
      <div class="step"><span class="sn">4</span><span>Don't run. Don't make noise. Don't look at it. Good luck.</span></div>
    </div>
  </div>

  <div class="keys">
    <span class="key">WASD — MOVE</span>
    <span class="key">MOUSE — LOOK</span>
    <span class="key">SHIFT — RUN</span>
    <span class="key">ESC — PAUSE</span>
  </div>

  <footer>
    <div class="ft-main">
      Built by <a href="https://github.com/dxb-gamer" target="_blank">dxb-gamer</a>
      &nbsp;·&nbsp; The Backrooms &copy; 2024
      &nbsp;·&nbsp; Play at your own risk
    </div>
    <div class="ft-secure">🔒 Served over https:// — the S stands for Secure. Your download is safe.</div>
    <span class="ft-dontrust">⚠ Don't Trust It.</span>
  </footer>

</main>
</body>
</html>
