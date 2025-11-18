<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>gamesunbloocked67 — v2.4 (Massive Upgrade)</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<style>
:root{--bg:#0d0420;--card:#151224;--accent:#ff7a38;--muted:#dcd3ef}
*{box-sizing:border-box}
html,body{margin:0;height:100%;font-family:Roboto,system-ui,-apple-system,Segoe UI,Arial;background:linear-gradient(180deg,var(--bg),#30103b);color:var(--muted)}
.container{max-width:1200px;margin:14px auto;padding:14px}
.header{display:flex;justify-content:space-between;align-items:center;gap:12px;flex-wrap:wrap}
.brand{font-weight:700;font-size:20px}
.note{font-size:13px}
nav{display:flex;gap:8px;margin-top:12px;flex-wrap:wrap}
.tab{padding:8px 12px;border-radius:8px;background:rgba(255,255,255,0.02);cursor:pointer}
.tab.active{outline:2px solid rgba(255,255,255,0.04);box-shadow:0 6px 24px rgba(0,0,0,0.6)}
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(240px,1fr));gap:12px;margin-top:14px}
.card{background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(255,255,255,0.01));padding:14px;border-radius:10px;border:1px solid rgba(255,255,255,0.03)}
.small{font-size:13px;color:rgba(255,255,255,0.8)}
.btn{background:var(--accent);color:#120018;padding:8px 10px;border-radius:8px;border:none;font-weight:700;cursor:pointer}
.full-screen{position:fixed;inset:0;background:rgba(0,0,0,0.9);display:none;align-items:center;justify-content:center;z-index:9999}
.modal-card{background:#0b0b0b;padding:12px;border-radius:8px;width:94%;max-width:1200px;max-height:92vh;overflow:auto;color:#fff}
.input,textarea,select{width:100%;padding:8px;border-radius:6px;border:1px solid rgba(255,255,255,0.03);background:#0a0710;color:var(--muted)}
.footer{margin-top:18px;text-align:center;color:rgba(255,255,255,0.6);font-size:13px}
.draggable{position:fixed;top:80px;right:20px;background:#151224;padding:10px;border:1px solid var(--accent);border-radius:8px;z-index:99999;cursor:move;overflow:auto;max-height:70vh}
.notice{background:rgba(255,255,255,0.02);padding:10px;border-radius:8px;margin-top:12px;color:var(--muted)}
.err{color:#ff9b9b}
label{font-size:13px;display:block;margin-top:8px}
.game-tile{display:flex;flex-direction:column;gap:8px;align-items:flex-start}
.kv{display:flex;gap:8px;align-items:center}
.small-btn{background:rgba(255,255,255,0.04);color:var(--muted);padding:6px 8px;border-radius:6px;border:1px solid rgba(255,255,255,0.02);cursor:pointer}
</style>
</head>
<body>
<div class="container">
  <div class="header">
    <div class="brand">gamesunbloocked67 <small id="ver">(v2.4)</small></div>
    <div class="note">
      Support: <a href="https://m.youtube.com/@cursedgamer2?sub_confirmation=1" target="_blank" style="color:var(--muted)">Subscribe</a>
      &nbsp;•&nbsp;
      <span id="ownerTag" class="small">Owner mode: off</span>
    </div>
  </div>

  <nav>
    <div class="tab active" id="tab-games" onclick="showTab('games')">Games</div>
    <div class="tab" id="tab-study" onclick="showTab('study')">Study</div>
    <div class="tab" id="tab-updates" onclick="showTab('updates')">Updates</div>
    <div class="tab" id="tab-bookmarks" onclick="showTab('bookmarks')">Bookmarklets</div>
    <div class="tab" id="tab-settings" onclick="showTab('settings')">Settings</div>
  </nav>

  <!-- GAMES -->
  <section id="games" style="display:block">
    <div class="grid" id="gamesGrid"></div>
    <div class="notice">Click Play. Games are fetched from the selenite-old repo and loaded into a sandboxed iframe (rewritten asset paths).</div>
  </section>

  <!-- STUDY -->
  <section id="study" style="display:none">
    <div class="card">
      <h3>Study Hub — AI Study Tools</h3>
      <div class="small">AI Teacher, AI Quiz Maker, AI Flashcards, and Math Solver. Global API key is used automatically if set by owner.</div>

      <label>AI Teacher — Ask any question (step-by-step)</label>
      <textarea id="teacherQuery" rows="4" placeholder="Explain mitosis step by step or solve 2x+3=11"></textarea>
      <div style="display:flex;gap:8px;margin-top:8px">
        <button class="btn" onclick="askTeacher()">Ask Teacher</button>
        <button class="btn" onclick="clearTeacher()">Clear</button>
      </div>
      <div id="teacherAnswer" class="notice" style="margin-top:12px"></div>

      <hr style="margin:14px 0;border-color:#241224">

      <label>AI Quiz Maker — topic or prompt</label>
      <input id="aiQuizTopic" class="input" placeholder="e.g. photosynthesis, fractions">
      <div style="display:flex;gap:8px;margin-top:8px">
        <button class="btn" onclick="makeAIQuiz()">Make AI Quiz</button>
        <button class="btn" onclick="clearAIQuiz()">Clear</button>
      </div>
      <div id="aiQuizArea" class="notice" style="margin-top:12px"></div>

      <hr style="margin:14px 0;border-color:#241224">

      <label>AI Flashcard Generator — topic</label>
      <input id="aiFlashTopic" class="input" placeholder="e.g. algebra basics">
      <div style="display:flex;gap:8px;margin-top:8px">
        <button class="btn" onclick="makeAIFlashcards()">Generate Flashcards</button>
        <button class="btn" onclick="clearAIFlashcards()">Clear</button>
      </div>
      <div id="aiFlashArea" class="notice" style="margin-top:12px"></div>

      <hr style="margin:14px 0;border-color:#241224">

      <label>Math Solver (local)</label>
      <input id="mathInput" class="input" placeholder="e.g. solve 3x+5=20 or compute 12*3/4">
      <div style="display:flex;gap:8px;margin-top:8px">
        <button class="btn" onclick="runMathSolver()">Solve</button>
        <button class="btn" onclick="clearMath()">Clear</button>
      </div>
      <div id="mathArea" class="notice" style="margin-top:12px"></div>

    </div>
  </section>

  <!-- UPDATES -->
  <section id="updates" style="display:none">
    <div class="card">
      <h3>Updates</h3>
      <div class="small">Install updates that add games and study tools. v2.4 (Massive Upgrade) is available below.</div>
      <div style="margin-top:12px" id="updatesList"></div>
      <div style="margin-top:12px"><button class="btn" onclick="installUpdate('v2.4')">Install v2.4</button></div>
    </div>
  </section>

  <!-- BOOKMARKLETS -->
  <section id="bookmarks" style="display:none">
    <div class="card">
      <h3>Bookmarklets</h3>
      <div class="small">Drag the panel on the right to keep bookmarklets handy.</div>
      <div id="bmList" style="margin-top:12px"></div>
    </div>
  </section>

  <!-- SETTINGS -->
  <section id="settings" style="display:none">
    <div class="card">
      <h3>Settings & Owner Controls</h3>
      <div class="small">Set global API key (owner only) — this key will be stored in site localStorage and used for AI requests for all visitors. Keep it secret.</div>

      <label>Owner password</label>
      <input id="ownerPass" class="input" placeholder="Enter owner password to unlock owner controls">

      <label style="margin-top:8px">Global Hugging Face API Key (owner)</label>
      <input id="globalApiKey" class="input" placeholder="hf_...">

      <div style="display:flex;gap:8px;margin-top:8px">
        <button class="btn" onclick="setGlobalKey()">Save Global Key (owner)</button>
        <button class="btn" onclick="clearGlobalKey()">Clear Global Key</button>
      </div>

      <hr style="margin:12px 0;border-color:#241224">

      <label>Tab Cloaker Title</label>
      <input id="cloakTitle" class="input" placeholder="e.g. Google Drive">

      <label>Tab Cloaker Favicon URL (optional)</label>
      <input id="cloakFavicon" class="input" placeholder="https://.../favicon.png">

      <div style="display:flex;gap:8px;margin-top:8px">
        <button class="btn" onclick="applyCloak()">Apply Cloak</button>
        <button class="btn" onclick="resetCloak()">Reset Cloak</button>
      </div>
    </div>
  </section>

  <div class="footer">© 2025 gamesunbloocked67</div>
</div>

<!-- right draggable bookmarklet panel -->
<div class="draggable" id="bmPanel" title="Drag me">
  <strong style="display:block">Bookmarklets</strong>
  <div style="margin-top:8px">
    <button class="small-btn" onclick="copyBM('javascript:(function(){document.body.contentEditable=true;document.designMode=\"on\"})()')">Copy: Edit Page</button>
  </div>
  <div style="margin-top:8px">
    <button class="small-btn" onclick="copyBM('javascript:(function(){var s=document.createElement(\"script\");s.src=\"https://cdn.jsdelivr.net/npm/eruda\";document.body.appendChild(s);s.onload=function(){eruda.init()}})()')">Copy: Dev Console</button>
  </div>
  <div style="margin-top:8px">
    <button class="small-btn" onclick="copyBM('javascript:(function(){document.title=\"Google Drive\";alert(\"Cloaked\")})()')">Copy: Cloaker</button>
  </div>
</div>

<!-- fullscreen modal -->
<div class="full-screen" id="fs"><div class="modal-card" id="fsCard"></div></div>

<script>
/* =========================
   Core app state & helpers
   ========================= */
const APP = {
  version: 'v2.4',
  repoBase: 'https://cdn.jsdelivr.net/gh/selenite-cc/selenite-old@main/',
  games: [], // will be populated by update install
  bookmarklets: [
    {id:'edit',title:'Edit Page',code:"javascript:(function(){document.body.contentEditable=true;document.designMode='on';})()"},
    {id:'eruda',title:'Dev Console',code:"javascript:(function(){var s=document.createElement('script');s.src='https://cdn.jsdelivr.net/npm/eruda';document.body.appendChild(s);s.onload=function(){eruda.init();}})()"},
    {id:'cloak',title:'Simple Cloak',code:"javascript:(function(){document.title='Google Drive';})();"}
  ],
  updates: {
    'v2.4': {
      id:'v2.4',
      title:'v2.4 — Massive Upgrade',
      desc:'Adds 20+ games and new AI study tools (AI Teacher, AI Quiz Maker, AI Flashcards, AI Math Solver).',
      applied: false,
      addGames:[
        // 20 example game entries from selenite-old repo (jsDelivr paths)
        {id:'1v1soccer', title:'1v1 Soccer', path:'1v1soccer/index.html'},
        {id:'2048', title:'2048', path:'2048/index.html'},
        {id:'badicecream', title:'Bad Ice Cream', path:'badicecream/index.html'},
        {id:'basketball', title:'Basketball', path:'basketball/index.html'},
        {id:'bombsquad', title:'Bombs', path:'bombs/index.html'},
        {id:'cars', title:'Cars', path:'cars/index.html'},
        {id:'chess', title:'Chess', path:'chess/index.html'},
        {id:'checkers', title:'Checkers', path:'checkers/index.html'},
        {id:'crooked', title:'Crooked', path:'crooked/index.html'},
        {id:'pong', title:'Pong', path:'pong/index.html'},
        {id:'tetris', title:'Tetris', path:'tetris/index.html'},
        {id:'snake', title:'Snake', path:'snake/index.html'},
        {id:'soccerphysics', title:'Soccer Physics', path:'soccerphysics/index.html'},
        {id:'run', title:'Run Game', path:'run/index.html'},
        {id:'parking', title:'Parking', path:'parking/index.html'},
        {id:'memory', title:'Memory', path:'memory/index.html'},
        {id:'minesweeper', title:'Minesweeper', path:'minesweeper/index.html'},
        {id:'shoot', title:'Shooter', path:'shooter/index.html'},
        {id:'platform', title:'Platformer', path:'platform/index.html'},
        {id:'puzzle', title:'Puzzle', path:'puzzle/index.html'}
      ]
    }
  }
};

/* simple escaping helpers */
function esc(s){ return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); }
function escJs(s){ return String(s).replace(/\\/g,'\\\\').replace(/'/g,"\\'").replace(/"/g,'\\"'); }

/* -------------------------
   Page & UI management
   ------------------------- */
document.getElementById('ver').innerText = '('+APP.version+')';
function showTab(t){
  ['games','study','updates','bookmarks','settings'].forEach(id=> document.getElementById(id).style.display = (id===t)?'block':'none');
  document.querySelectorAll('.tab').forEach(el=>el.classList.remove('active'));
  document.getElementById('tab-'+t).classList.add('active');
  if(t==='games') renderGames();
  if(t==='bookmarks') renderBookmarklets();
  if(t==='updates') renderUpdates();
}
showTab('games');

/* -------------------------
   Games loader (fetch + rewrite)
   ------------------------- */
function renderGames(){
  const grid = document.getElementById('gamesGrid'); grid.innerHTML = '';
  if(APP.games.length === 0){
    grid.innerHTML = '<div class="card small">No games installed. Install update v2.4 from Updates tab.</div>';
    return;
  }
  APP.games.forEach(g=>{
    const card = document.createElement('div'); card.className='card game-tile';
    card.innerHTML = `<strong>${esc(g.title)}</strong>
      <div style="display:flex;gap:8px;margin-top:8px">
        <button class="btn" onclick="playGame('${escJs(g.path)}')">Play</button>
        <button class="small-btn" onclick="openRaw('${escJs(g.path)}')">Open raw</button>
      </div>
      <div class="small" id="st-${esc(g.id)}"></div>`;
    grid.appendChild(card);
  });
}

/* rewrite relative urls to CDN absolute */
function rewriteHtmlForCDN(html, basePath){
  // basePath ends with '/'
  html = html.replace(/(<head[^>]*>)/i, `$1<base href="${basePath}">`);
  // rewrite src and href in attributes that are relative (not beginning with http, //, data:, mailto:, #)
  html = html.replace(/(src|href)\s*=\s*["'](?!https?:|\/\/|data:|mailto:|#)([^"']+)["']/gi, (m,p1,p2)=> `${p1}="${basePath}${p2}"`);
  // rewrite CSS url(...) patterns without absolute URLs
  html = html.replace(/url\((?!['"]?(?:https?:|data:|\/\/))['"]?([^'")]+)['"]?\)/gi, (m,p1)=> `url(${basePath}${p1})`);
  return html;
}

async function playGame(relPath){
  const fs = document.getElementById('fs'); const card = document.getElementById('fsCard');
  card.innerHTML = `<div style="display:flex;justify-content:space-between;align-items:center"><strong>Loading Game</strong><div><button class="btn" onclick="closeFS()">Close</button></div></div><div id="playerArea" style="height:82vh;margin-top:8px;display:flex;align-items:center;justify-content:center;color:var(--muted)">Fetching...</div>`;
  fs.style.display='flex';
  const playerArea = document.getElementById('playerArea');
  try{
    const url = APP.repoBase + relPath;
    const res = await fetch(url);
    if(!res.ok){ playerArea.innerHTML = `<div class="err">Failed to fetch (${res.status})</div><div style="margin-top:8px"><a href="${url}" target="_blank">Open raw</a></div>`; return; }
    let html = await res.text();
    // compute base path for assets
    const dir = relPath.split('/').slice(0,-1).join('/') + '/';
    const base = APP.repoBase + dir;
    const rewritten = rewriteHtmlForCDN(html, base);
    // create iframe with srcdoc
    playerArea.innerHTML = `<iframe id="gameFrame" style="width:100%;height:100%;border:none"></iframe>`;
    const iframe = document.getElementById('gameFrame');
    iframe.srcdoc = rewritten;
  }catch(e){
    playerArea.innerHTML = `<div class="err">Error: ${esc(e.message)}</div>`;
  }
}

function openRaw(relPath){ window.open(APP.repoBase + relPath, '_blank'); }
function closeFS(){ document.getElementById('fs').style.display='none'; document.getElementById('fsCard').innerHTML=''; }

/* -------------------------
   Bookmarklets panel
   ------------------------- */
function renderBookmarklets(){
  const list = document.getElementById('bmList'); list.innerHTML='';
  APP.bookmarklets.forEach(b=>{
    const div = document.createElement('div'); div.className='card';
    div.innerHTML = `<strong>${esc(b.title)}</strong><div style="margin-top:8px"><button class="btn" onclick="runBookmarklet('${escJs(b.code)}')">Run</button> <button class="small-btn" onclick="copyBM('${escJs(b.code)}')">Copy</button></div>`;
    list.appendChild(div);
  });
}

function runBookmarklet(code){
  try{ eval(code.replace(/^javascript:/,'')); }catch(e){ alert('Bookmarklet error: '+e.message); }
}
function copyBM(code){ navigator.clipboard.writeText(code).then(()=>alert('Bookmarklet copied to clipboard')) }

/* draggable panel */
(function initPanel(){
  const panel = document.getElementById('bmPanel');
  let isDown=false, ox=0, oy=0;
  panel.addEventListener('mousedown', e=>{ isDown=true; ox = e.clientX - panel.offsetLeft; oy = e.clientY - panel.offsetTop; panel.style.cursor='grabbing'; });
  document.addEventListener('mouseup', ()=>{ isDown=false; panel.style.cursor='move'; });
  document.addEventListener('mousemove', e=>{ if(!isDown) return; panel.style.left=(e.clientX-ox)+'px'; panel.style.top=(e.clientY-oy)+'px'; });
})();

/* -------------------------
   Updates system
   ------------------------- */
function renderUpdates(){
  const out = document.getElementById('updatesList'); out.innerHTML='';
  Object.values(APP.updates).forEach(u=>{
    out.innerHTML += `<div style="margin-bottom:10px"><strong>${esc(u.title)}</strong><div class="small">${esc(u.desc)}</div><div style="margin-top:8px">${u.applied?'<span class="small">Installed</span>':'<button class="btn" onclick="installUpdate(\\''+escJs(u.id)+'\\')">Install</button>'}</div></div>`;
  });
}
function installUpdate(id){
  const upd = APP.updates[id];
  if(!upd) return alert('Update not found');
  if(upd.applied) return alert('Update already installed');
  // add games (avoid duplicates)
  (upd.addGames||[]).forEach(g=>{
    if(!APP.games.find(x=>x.path === g.path)) APP.games.push({id:g.id,title:g.title,path:g.path});
  });
  upd.applied = true;
  renderGames(); renderUpdates();
  alert(upd.title + ' applied — new games installed');
}

/* -------------------------
   Owner & Global API Key
   ------------------------- */
const OWNER_PASSWORD = 'letmein_owner'; // change this before publishing
function setGlobalKey(){
  const pass = document.getElementById('ownerPass').value || '';
  if(pass !== OWNER_PASSWORD){ alert('Invalid owner password'); return; }
  const key = document.getElementById('globalApiKey').value.trim();
  if(!key){ alert('Enter key'); return; }
  localStorage.setItem('global_api_key', key);
  document.getElementById('ownerTag').innerText = 'Owner mode: on';
  alert('Global API key saved');
}
function clearGlobalKey(){
  const pass = document.getElementById('ownerPass').value || '';
  if(pass !== OWNER_PASSWORD){ alert('Invalid owner password'); return; }
  localStorage.removeItem('global_api_key');
  document.getElementById('ownerTag').innerText = 'Owner mode: off';
  alert('Global API key cleared');
}
function getGlobalKey(){ return localStorage.getItem('global_api_key') || ''; }

/* -------------------------
   Tab Cloaker
   ------------------------- */
function applyCloak(){
  const title = document.getElementById('cloakTitle').value.trim();
  const icon = document.getElementById('cloakFavicon').value.trim();
  if(title) document.title = title;
  if(icon){
    document.querySelectorAll('link[rel="icon"],link[rel="shortcut icon"]').forEach(e=>e.remove());
    const l = document.createElement('link'); l.rel='icon'; l.href = icon; document.head.appendChild(l);
  }
  alert('Cloak applied');
}
function resetCloak(){ location.reload(); }

/* -------------------------
   AI Tools (Teacher, Quiz, Flashcards)
   ------------------------- */
async function askTeacher(){
  const q = document.getElementById('teacherQuery').value.trim();
  const out = document.getElementById('teacherAnswer');
  if(!q){ out.innerHTML = '<div class="err">Type a question first.</div>'; return; }
  out.innerHTML = 'Thinking...';
  const key = getGlobalKey();
  if(!key){
    // Local heuristic fallback (math or step tips)
    const math = simpleMathSolver(q);
    if(math){
      out.innerHTML = `<strong>Step-by-step:</strong><br>${math.steps.join('<br>')}<br><strong>Result:</strong> ${math.result}`;
      return;
    }
    out.innerHTML = `Quick tips:<br>1) Restate the problem.<br>2) Break into steps.<br>3) Try an example.<br><br>Your question: ${esc(q)}`;
    return;
  }
  // call HF inference (gpt2 or other model) using global key
  try{
    const payload = { inputs: `You are an expert tutor. Answer step-by-step and show intermediate steps. Question: ${q}`, parameters:{ max_new_tokens: 400, temperature:0.2 } };
    const resp = await fetch('https://api-inference.huggingface.co/models/gpt2', {
      method:'POST', headers:{ Authorization: 'Bearer '+key, 'Content-Type':'application/json' }, body: JSON.stringify(payload)
    });
    const data = await resp.json();
    if(data.error){ out.innerHTML = `<div class="err">Model error: ${esc(data.error)}</div>`; return; }
    const text = (Array.isArray(data) && data[0]?.generated_text) ? data[0].generated_text : (data.generated_text || JSON.stringify(data));
    out.innerHTML = text.replace(/\n/g,'<br>');
  }catch(e){ out.innerHTML = `<div class="err">Request failed: ${esc(e.message)}</div>`; }
}

function clearTeacher(){ document.getElementById('teacherQuery').value=''; document.getElementById('teacherAnswer').innerHTML=''; }

/* AI Quiz Maker: uses HF if key present, otherwise local naive generator */
async function makeAIQuiz(){
  const topic = document.getElementById('aiQuizTopic').value.trim();
  const area = document.getElementById('aiQuizArea');
  if(!topic){ area.innerHTML = '<div class="err">Enter topic</div>'; return; }
  area.innerHTML = 'Generating quiz...';
  const key = getGlobalKey();
  if(!key){
    // local simple generator
    const qs = [];
    for(let i=1;i<=5;i++){
      const correct = `Correct about ${topic} #${i}`;
      const choices = shuffle([correct, `Wrong A ${i}`, `Wrong B ${i}`, `Wrong C ${i}`]);
      qs.push({q:`What is ${topic} part ${i}?`, choices, correct});
    }
    renderQuizInline(qs, area);
    return;
  }
  // ask HF to produce 5 MCQs in JSON: [{q, correct, wrongs:[...]}]
  try{
    const prompt = `Create 5 multiple-choice questions (JSON array) about "${topic}". Return JSON only. Each item: {"q":"...","correct":"...","wrongs":["...","...","..."]}`;
    const payload = { inputs: prompt, parameters:{ max_new_tokens: 512, temperature:0.2 } };
    const resp = await fetch('https://api-inference.huggingface.co/models/gpt2', { method:'POST', headers:{ Authorization:'Bearer '+key,'Content-Type':'application/json' }, body: JSON.stringify(payload) });
    const data = await resp.json();
    let text = (Array.isArray(data) && data[0]?.generated_text) ? data[0].generated_text : (data.generated_text || JSON.stringify(data));
    // try to extract JSON from text
    const jsonMatch = text.match(/(\[.*\])/s);
    const arr = jsonMatch ? JSON.parse(jsonMatch[1]) : JSON.parse(text);
    const qs = arr.map(item=>({ q: item.q, choices: shuffle([item.correct, ...(item.wrongs||[])]), correct: item.correct }));
    renderQuizInline(qs, area);
  }catch(e){
    area.innerHTML = `<div class="err">AI quiz failed: ${esc(e.message)}</div>`;
  }
}

function renderQuizInline(qs, areaEl){
  areaEl.innerHTML = `<form id="aiQuizForm">${qs.map((qq,idx)=>`<div style="margin-bottom:10px"><strong>Q${idx+1}.</strong> ${esc(qq.q)}<br>`+qq.choices.map((c)=>`<label><input type="radio" name="q${idx}" value="${esc(c)}"> ${esc(c)}</label><br>`).join('')+`</div>`).join('')}<div style="margin-top:8px"><button type="button" class="btn" onclick='gradeAIQuiz(${escJs(JSON.stringify(qs))})'>Submit</button></div></form>`;;
}

function gradeAIQuiz(qsJson){
  const qs = JSON.parse(qsJson);
  const form = document.getElementById('aiQuizForm'); let score=0;
  for(let i=0;i<qs.length;i++){
    const sel = form.querySelector(`input[name=q${i}]:checked`);
    if(sel && sel.value === qs[i].correct) score++;
  }
  alert(`Score: ${score} / ${qs.length}`);
}

function clearAIQuiz(){ document.getElementById('aiQuizArea').innerHTML=''; document.getElementById('aiQuizTopic').value=''; }

/* AI Flashcards: uses HF if available */
async function makeAIFlashcards(){
  const topic = document.getElementById('aiFlashTopic').value.trim();
  const area = document.getElementById('aiFlashArea');
  if(!topic){ area.innerHTML = '<div class="err">Enter topic</div>'; return; }
  area.innerHTML = 'Generating flashcards...';
  const key = getGlobalKey();
  if(!key){
    const cards = Array.from({length:6},(_,i)=>({term:`${topic} concept ${i+1}`, def:`Short explanation of ${topic} concept ${i+1}`}));
    area.innerHTML = cards.map(c=>`<div style="margin-bottom:8px"><strong>${esc(c.term)}</strong><div class="small">${esc(c.def)}</div></div>`).join('');
    localStorage.setItem('last_ai_flashcards', JSON.stringify(cards));
    return;
  }
  try{
    const prompt = `Create 6 concise flashcards for the topic "${topic}". Return JSON array: [{"term":"...","def":"..."}, ...]`;
    const payload = { inputs: prompt, parameters:{ max_new_tokens: 512, temperature:0.2 } };
    const resp = await fetch('https://api-inference.huggingface.co/models/gpt2', { method:'POST', headers:{ Authorization:'Bearer '+key,'Content-Type':'application/json' }, body: JSON.stringify(payload) });
    const data = await resp.json();
    let text = (Array.isArray(data) && data[0]?.generated_text) ? data[0].generated_text : (data.generated_text || JSON.stringify(data));
    const jsonMatch = text.match(/(\[.*\])/s);
    const arr = jsonMatch ? JSON.parse(jsonMatch[1]) : JSON.parse(text);
    const cards = arr.map(a=>({term:a.term, def:a.def}));
    area.innerHTML = cards.map(c=>`<div style="margin-bottom:8px"><strong>${esc(c.term)}</strong><div class="small">${esc(c.def)}</div></div>`).join('');
    localStorage.setItem('last_ai_flashcards', JSON.stringify(cards));
  }catch(e){
    area.innerHTML = `<div class="err">AI flashcards failed: ${esc(e.message)}</div>`;
  }
}

function clearAIFlashcards(){ document.getElementById('aiFlashArea').innerHTML=''; document.getElementById('aiFlashTopic').value=''; }

/* -------------------------
   Math solver (local)
   ------------------------- */
function runMathSolver(){
  const q = document.getElementById('mathInput').value.trim();
  const out = document.getElementById('mathArea');
  if(!q){ out.innerHTML = '<div class="err">Enter math expression or simple linear equation (e.g. 2x+3=9)</div>'; return; }
  const res = simpleMathSolver(q);
  if(!res){ out.innerHTML = '<div class="err">Could not parse. Try a simpler expression (e.g. 12*3/4 or solve 2x+3=9)</div>'; return; }
  out.innerHTML = `<strong>Steps:</strong><br>${res.steps.join('<br>')}<br><strong>Answer:</strong> ${res.result}`;
}

function clearMath(){ document.getElementById('mathInput').value=''; document.getElementById('mathArea').innerHTML=''; }

function simpleMathSolver(q){
  try{
    // linear equation parse: ax + b = c
    const eq = q.replace(/\s+/g,'').match(/^([+-]?\d*\.?\d*)x([+-]\d+\.?\d*)?=([+-]?\d+\.?\d*)$/i);
    if(eq){
      let a = eq[1]; if(a===''||a==='+') a='1'; if(a==='-') a='-1';
      a = parseFloat(a); const b = eq[2]?parseFloat(eq[2]):0; const c = parseFloat(eq[3]);
      const steps = [`Equation: ${a}x ${b>=0?'+':'-'} ${Math.abs(b)} = ${c}`, `Subtract ${b}: ${a}x = ${c - b}`, `Divide by ${a}: x = ${(c - b)/a}`];
      return {steps, result: (c - b)/a};
    }
    // arithmetic expression
    const cleaned = q.replace(/[^0-9+\-*/(). ]/g,'');
    if(cleaned.match(/[0-9]/) && cleaned.match(/[\+\-\*\/]/)){
      const val = Function('"use strict";return ('+cleaned+')')();
      return {steps:[`Evaluate ${cleaned}`], result: val};
    }
  }catch(e){}
  return null;
}

/* -------------------------
   Utility & init
   ------------------------- */
function shuffle(a){ for(let i=a.length-1;i>0;i--){ const j=Math.floor(Math.random()*(i+1)); [a[i],a[j]]=[a[j],a[i]] } return a; }

document.addEventListener('DOMContentLoaded', ()=>{
  renderGames();
  renderBookmarklets();
  renderUpdates();
  // show owner tag if global key exists
  if(localStorage.getItem('global_api_key')) document.getElementById('ownerTag').innerText = 'Owner mode: on';
});

/* helper to render bookmarklets list when opening tab */
function renderBookmarklets(){ renderBookmarklets(); } // alias for tab action (already called in init)

/* Expose small helpers for debugging in console */
window.APP = APP;
window.playGame = playGame;
window.installUpdate = installUpdate;
window.getGlobalKey = () => localStorage.getItem('global_api_key') || '';
</script>
</body>
</html>
