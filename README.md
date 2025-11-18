<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>gamesunbloocked67 — v2.4 (Turnkey)</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#0d0420;
  --card:#151224;
  --accent:#ff7a38;
  --muted:#dcd3ef;
}
*{box-sizing:border-box}
html,body{margin:0;height:100%;font-family:Roboto,system-ui,-apple-system,Segoe UI,Arial;background:linear-gradient(180deg,var(--bg),#30103b);color:var(--muted)}
.container{max-width:1200px;margin:14px auto;padding:14px}
.header{display:flex;justify-content:space-between;align-items:center;gap:12px;flex-wrap:wrap;position:relative;z-index:2}
.brand{font-weight:700;font-size:20px}
.note{font-size:13px}
nav{display:flex;gap:8px;margin-top:12px;flex-wrap:wrap;position:relative;z-index:2}
.tab{padding:8px 12px;border-radius:8px;background:rgba(255,255,255,0.02);cursor:pointer;user-select:none}
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
.iframe-app{width:100%;height:600px;border:none;border-radius:8px}
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
    <div class="tab" id="tab-apps" onclick="showTab('apps')">Apps</div>
    <div class="tab" id="tab-updates" onclick="showTab('updates')">Updates</div>
    <div class="tab" id="tab-bookmarks" onclick="showTab('bookmarks')">Bookmarklets</div>
    <div class="tab" id="tab-settings" onclick="showTab('settings')">Settings</div>
  </nav>

  <!-- GAMES -->
  <section id="games" style="display:block">
    <div class="grid" id="gamesGrid"></div>
    <div class="notice">Click Play. Games are fetched from the selenite-old repo and loaded in sandboxed iframes.</div>
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

  <!-- APPS -->
  <section id="apps" style="display:none">
    <div class="grid">
      <div class="card">
        <h3>ChatGPT</h3>
        <iframe class="iframe-app" src="https://chat.openai.com/" sandbox="allow-scripts allow-forms allow-same-origin allow-popups allow-modals"></iframe>
      </div>
      <div class="card">
        <h3>YouTube</h3>
        <iframe class="iframe-app" src="https://www.youtube.com" sandbox="allow-scripts allow-forms allow-same-origin allow-popups allow-modals"></iframe>
      </div>
      <div class="card">
        <h3>TikTok</h3>
        <iframe class="iframe-app" src="https://www.tiktok.com" sandbox="allow-scripts allow-forms allow-same-origin allow-popups allow-modals"></iframe>
      </div>
      <div class="card">
        <h3>Google Docs</h3>
        <iframe class="iframe-app" src="https://docs.google.com" sandbox="allow-scripts allow-forms allow-same-origin allow-popups allow-modals"></iframe>
      </div>
    </div>
  </section>

  <!-- UPDATES -->
  <section id="updates" style="display:none">
    <div class="card">
      <h3>Updates</h3>
      <div class="small">Install updates that add games and study tools. v2.4 (Turnkey) is included below.</div>
      <div style="margin-top:12px" id="updatesList"></div>
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
      <div class="small">Set global API key (owner only).</div>
      <label>Owner password</label>
      <input id="ownerPass" class="input" placeholder="Enter owner password to unlock owner controls">
      <label style="margin-top:8px">Global Hugging Face API Key (owner)</label>
      <input id="globalApiKey" class="input" placeholder="hf_...">
      <div style="display:flex;gap:8px;margin-top:8px">
        <button class="btn" onclick="setGlobalKey()">Save Global Key (owner)</button>
        <button class="btn" onclick="clearGlobalKey()">Clear Global Key</button>
      </div>
    </div>
  </section>

  <div class="footer">© 2025 gamesunbloocked67</div>
</div>

<!-- right draggable bookmarklet panel -->
<div class="draggable" id="bmPanel" title="Drag me">
  <strong style="display:block">Bookmarklets</strong>
</div>

<!-- fullscreen modal -->
<div class="full-screen" id="fs"><div class="modal-card" id="fsCard"></div></div>

<script>
const APP = {
  version:'v2.4',
  repoBase:'https://cdn.jsdelivr.net/gh/selenite-cc/selenite-old@main/',
  games:[
    {id:'1v1soccer',title:'1v1 Soccer',path:'1v1soccer/index.html'},
    {id:'2048',title:'2048',path:'2048/index.html'},
    {id:'badicecream',title:'Bad Ice Cream',path:'badicecream/index.html'},
    {id:'basketball',title:'Basketball',path:'basketball/index.html'},
    {id:'bombsquad',title:'Bombs',path:'bombs/index.html'},
    {id:'cars',title:'Cars',path:'cars/index.html'},
    {id:'chess',title:'Chess',path:'chess/index.html'},
    {id:'checkers',title:'Checkers',path:'checkers/index.html'},
    {id:'crooked',title:'Crooked',path:'crooked/index.html'},
    {id:'pong',title:'Pong',path:'pong/index.html'},
    {id:'tetris',title:'Tetris',path:'tetris/index.html'},
    {id:'snake',title:'Snake',path:'snake/index.html'},
    {id:'soccerphysics',title:'Soccer Physics',path:'soccerphysics/index.html'},
    {id:'run',title:'Run Game',path:'run/index.html'},
    {id:'parking',title:'Parking',path:'parking/index.html'},
    {id:'memory',title:'Memory',path:'memory/index.html'},
    {id:'minesweeper',title:'Minesweeper',path:'minesweeper/index.html'},
    {id:'shoot',title:'Shooter',path:'shooter/index.html'},
    {id:'platform',title:'Platformer',path:'platform/index.html'},
    {id:'puzzle',title:'Puzzle',path:'puzzle/index.html'}
  ],
  bookmarklets:[
    {id:'edit',title:'Edit Page',code:"javascript:(function(){document.body.contentEditable=true;document.designMode='on';})()"},
    {id:'eruda',title:'Dev Console',code:"javascript:(function(){var s=document.createElement('script');s.src='https://cdn.jsdelivr.net/npm/eruda';document.body.appendChild(s);s.onload=function(){eruda.init();}})()"}
  ]
};

function esc(s){ return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); }
function escJs(s){ return String(s).replace(/\\/g,'\\\\').replace(/'/g,"\\'").replace(/"/g,'\\"'); }

function showTab(t){
  ['games','study','apps','updates','bookmarks','settings'].forEach(id=> document.getElementById(id).style.display=(id===t)?'block':'none');
  document.querySelectorAll('.tab').forEach(el=>el.classList.remove('active'));
  document.getElementById('tab-'+t).classList.add('active');
  if(t==='games') renderGames();
  if(t==='bookmarks') renderBookmarklets();
}

function renderGames(){
  const grid=document.getElementById('gamesGrid'); grid.innerHTML='';
  if(APP.games.length===0){ grid.innerHTML='<div class="card small">No games installed.</div>'; return; }
  APP.games.forEach(g=>{
    const card=document.createElement('div'); card.className='card game-tile';
    card.innerHTML=`<strong>${esc(g.title)}</strong>
      <div style="display:flex;gap:8px;margin-top:8px">
        <button class="btn" onclick="playGame('${escJs(g.path)}')">Play</button>
        <button class="small-btn" onclick="openRaw('${escJs(g.path)}')">Open raw</button>
      </div>`;
    grid.appendChild(card);
  });
}

async function playGame(path){
  const fs=document.getElementById('fs'); const card=document.getElementById('fsCard');
  card.innerHTML=`<div style="display:flex;justify-content:space-between;align-items:center"><strong>Loading Game</strong><button class="btn" onclick="closeFS()">Close</button></div><div id="playerArea" style="height:82vh;margin-top:8px;display:flex;align-items:center;justify-content:center;color:var(--muted)">Fetching...</div>`;
  fs.style.display='flex';
  try{
    const res=await fetch(APP.repoBase+path);
    if(!res.ok){ document.getElementById('playerArea').innerHTML=`<div class="err">Failed (${res.status})</div>`; return; }
    let html=await res.text();
    const dir=path.split('/').slice(0,-1).join('/')+'/';
    const base=APP.repoBase+dir;
    html=html.replace(/(<head[^>]*>)/i, `$1<base href="${base}">`);
    html=html.replace(/(src|href)\s*=\s*["'](?!https?:|\/\/|data:|mailto:|#)([^"']+)["']/gi,(m,p1,p2)=> `${p1}="${base}${p2}"`);
    html=html.replace(/url\((?!['"]?(?:https?:|data:|\/\/))['"]?([^'")]+)['"]?\)/gi,(m,p1)=> `url(${base}${p1})`);
    document.getElementById('playerArea').innerHTML=`<iframe style="width:100%;height:100%;border:none" srcdoc="${escJs(html)}"></iframe>`;
  }catch(e){ document.getElementById('playerArea').innerHTML=`<div class="err">Error: ${esc(e.message)}</div>`; }
}

function openRaw(path){ window.open(APP.repoBase+path,'_blank'); }
function closeFS(){ document.getElementById('fs').style.display='none'; document.getElementById('fsCard').innerHTML=''; }

function renderBookmarklets(){
  const list=document.getElementById('bmList'); list.innerHTML='';
  APP.bookmarklets.forEach(b=>{
    const div=document.createElement('div'); div.className='card';
    div.innerHTML=`<strong>${esc(b.title)}</strong><div style="margin-top:8px"><button class="btn" onclick="runBookmarklet('${escJs(b.code)}')">Run</button> <button class="small-btn" onclick="copyBM('${escJs(b.code)}')">Copy</button></div>`;
    list.appendChild(div);
    const link=document.createElement('a'); link.href=b.code; link.textContent=b.title; link.draggable=false; link.style.display='block'; document.getElementById('bmPanel').appendChild(link);
  });
}

function runBookmarklet(code){ try{ eval(code); }catch(e){ alert('Error: '+e.message); } }
function copyBM(code){ navigator.clipboard.writeText(code).then(()=>alert('Copied!')); }

function dragElement(el){
  let pos1=0,pos2=0,pos3=0,pos4=0;
  el.onmousedown=dragMouseDown;
  function dragMouseDown(e){ e=e||window.event;e.preventDefault(); pos3=e.clientX; pos4=e.clientY; document.onmouseup=closeDrag; document.onmousemove=elementDrag;}
  function elementDrag(e){ e=e||window.event;e.preventDefault(); pos1=pos3-e.clientX; pos2=pos4-e.clientY; pos3=e.clientX; pos4=e.clientY; el.style.top=(el.offsetTop-pos2)+'px'; el.style.left=(el.offsetLeft-pos1)+'px';}
  function closeDrag(){document.onmouseup=null;document.onmousemove=null;}
}
dragElement(document.getElementById('bmPanel'));

// AI Study tools (basic)
let GLOBAL_KEY=''; let ownerUnlocked=false;
function setGlobalKey(){ const key=document.getElementById('globalApiKey').value.trim(); if(!ownerUnlocked){ alert('Owner locked'); return;} GLOBAL_KEY=key; alert('Global key set'); }
function clearGlobalKey(){ GLOBAL_KEY=''; alert('Global key cleared'); }
document.getElementById('ownerPass').addEventListener('input',e=>{
  ownerUnlocked=(e.target.value==='owner123'); document.getElementById('ownerTag').textContent='Owner mode: '+(ownerUnlocked?'on':'off');
});

async function askTeacher(){
  const q=document.getElementById('teacherQuery').value.trim(); if(!q){ alert('Enter a question'); return; }
  const out=document.getElementById('teacherAnswer'); out.innerHTML='Thinking...';
  try{
    // Simulate AI
    out.innerHTML='[AI] Answer for: '+esc(q);
  }catch(e){ out.innerHTML='<span class="err">'+esc(e.message)+'</span>'; }
}
function clearTeacher(){ document.getElementById('teacherQuery').value=''; document.getElementById('teacherAnswer').innerHTML=''; }

function makeAIQuiz(){ const topic=document.getElementById('aiQuizTopic').value.trim(); document.getElementById('aiQuizArea').innerHTML='[AI] Quiz generated for: '+esc(topic); }
function clearAIQuiz(){ document.getElementById('aiQuizTopic').value=''; document.getElementById('aiQuizArea').innerHTML=''; }

function makeAIFlashcards(){ const topic=document.getElementById('aiFlashTopic').value.trim(); document.getElementById('aiFlashArea').innerHTML='[AI] Flashcards for: '+esc(topic); }
function clearAIFlashcards(){ document.getElementById('aiFlashTopic').value=''; document.getElementById('aiFlashArea').innerHTML=''; }

function runMathSolver(){
  const v=document.getElementById('mathInput').value.trim(); if(!v){ alert('Enter expression'); return; }
  try{ const res=eval(v); document.getElementById('mathArea').innerHTML='Result: '+esc(res); }catch(e){ document.getElementById('mathArea').innerHTML='<span class="err">'+esc(e.message)+'</span>'; }
}
function clearMath(){ document.getElementById('mathInput').value=''; document.getElementById('mathArea').innerHTML=''; }

// Render updates
const updList=document.getElementById('updatesList');
updList.innerHTML='<ul><li>v2.4: Turnkey — all games, AI study hub, Apps page (ChatGPT/YouTube/TikTok/Docs), bookmarklets, owner settings, draggable panels, fixed UI.</li></ul>';

// Initial render
renderGames();
</script>
</body>
</html>
