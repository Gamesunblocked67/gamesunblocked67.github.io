<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>gamesunbloocked67 — v2.0 (Manual Updates)</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<style>
:root{
  --bg-a:#0f0320; --bg-b:#2f0b3a; --accent:#ff7a38; --accent-2:#c88cff;
  --muted:#e6ddf6; --glass:rgba(255,255,255,0.04);
}
*{box-sizing:border-box}
html,body{height:100%;margin:0;font-family:Roboto,system-ui,-apple-system,Segoe UI,Arial;background:linear-gradient(180deg,var(--bg-a),var(--bg-b));color:var(--muted);-webkit-font-smoothing:antialiased}
.container{max-width:1180px;margin:18px auto;padding:18px}
.header{display:flex;align-items:center;justify-content:space-between}
.brand{font-weight:700;font-size:22px}
.topnote{font-size:13px;color:rgba(255,255,255,0.75)}
.landing{margin-top:18px;padding:18px;border-radius:12px;background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(255,255,255,0.01));border:1px solid rgba(255,255,255,0.03);text-align:center}
.choices{display:flex;gap:18px;justify-content:center;margin-top:12px}
.choice{width:260px;padding:18px;border-radius:12px;background:rgba(255,255,255,0.02);cursor:pointer;border:1px solid rgba(255,255,255,0.03);transition:transform .18s}
.choice:hover{transform:translateY(-6px)}
.topnav{display:flex;gap:10px;margin-top:18px}
.topnav button{background:transparent;border:1px solid rgba(255,255,255,0.05);padding:8px 10px;border-radius:8px;color:var(--muted);cursor:pointer}
.page{display:none;margin-top:18px}
.page.active{display:block}
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:12px}
.card{background:rgba(0,0,0,0.25);padding:12px;border-radius:10px;border:1px solid rgba(255,255,255,0.03)}
.study-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:12px;margin-top:12px}
.tools-wrap{display:grid;grid-template-columns:1fr 340px;gap:12px;margin-top:12px}
@media (max-width:980px){.tools-wrap{grid-template-columns:1fr}}
.small-muted{font-size:13px;color:rgba(255,255,255,0.7)}
.update-modal{position:fixed;left:50%;top:50%;transform:translate(-50%,-50%);background:#242424;border:3px solid var(--accent);border-radius:12px;padding:20px 24px;z-index:9999;display:none;width:420px;box-shadow:0 12px 40px rgba(0,0,0,0.6)}
.update-title{color:#fff;font-weight:700;text-align:center}
.update-desc{color:#ddd;text-align:center;margin-top:8px}
.update-bar-wrap{margin-top:14px;background:#1a1a1a;border-radius:10px;height:14px;overflow:hidden}
.update-bar{height:100%;width:0%;background:linear-gradient(90deg,var(--accent),#ffb76b);transition:width .18s linear}
.full-screen{position:fixed;inset:0;background:rgba(0,0,0,0.85);display:none;align-items:center;justify-content:center;z-index:9998}
.modal-card{background:#0f0f10;border-radius:10px;padding:12px;max-width:92%;width:100%;max-height:90vh;overflow:auto}
.footer{margin-top:18px;text-align:center;color:rgba(255,255,255,0.6);font-size:13px}
input,textarea,select{background:#110611;border:1px solid rgba(255,255,255,0.04);color:var(--muted);padding:8px;border-radius:6px;width:100%}
.btn{background:var(--accent);color:#120018;padding:8px 10px;border-radius:8px;border:none;cursor:pointer;font-weight:700}
.small{padding:6px 8px;font-size:13px}
.bm-board{display:flex;flex-wrap:wrap;gap:12px;margin-top:12px}
.bm-card{min-width:200px;padding:10px;border-radius:8px;background:rgba(0,0,0,0.35);border:1px solid rgba(255,255,255,0.04)}
.notice{padding:8px;border-radius:8px;background:rgba(255,255,255,0.02);margin-top:8px}
</style>
</head>
<body>
<div class="container">
  <div class="header">
    <div class="brand">gamesunbloocked67 <span style="font-size:12px">v2.0</span></div>
    <div class="topnote">Study + Games • Support: <a href="https://www.youtube.com/@cursedgamer2" target="_blank">YouTube.com/@cursedgamer2</a></div>
  </div>

  <div id="landing" class="landing">
    <div style="font-weight:700;font-size:18px">Choose a mode</div>
    <div class="choices">
      <div class="choice" onclick="enterMode('study')"><h3>Study Mode</h3><div class="small-muted">AI Tutor, subjects, flashcards, quizzes</div></div>
      <div class="choice" onclick="enterMode('games')"><h3>Games Mode</h3><div class="small-muted">Play games (avoid school devices)</div></div>
    </div>
    <div style="margin-top:10px" class="small-muted">Tip: Use Updates page to add or remove features manually.</div>
  </div>

  <div id="app" style="display:none">
    <div class="topnav" role="navigation">
      <button onclick="showSection('home')">Home</button>
      <button onclick="showSection('study')">Study</button>
      <button onclick="showSection('games')">Games</button>
      <button onclick="showSection('bookmarklets')">Bookmarklets</button>
      <button onclick="showSection('updates')">Updates</button>
    </div>

    <div id="home" class="page active"><div class="card"><h3>Welcome</h3><div class="small-muted">Choose Study or Games. Use Updates to install features manually.</div></div></div>

    <div id="study" class="page">
      <h2>Study Hub</h2>
      <div class="small-muted">v2.0: manual update system — install/remove updates from the Updates page.</div>

      <div class="study-grid" id="studyGrid"></div>

      <div class="tools-wrap">
        <div class="card">
          <h3>AI Tutor</h3>
          <div class="small-muted">Ask a question — math, science, history, reading, writing.</div>
          <textarea id="aiInput" placeholder="Ask: Solve 3/4 + 1/2 or Explain photosynthesis" style="height:80px"></textarea>
          <div style="display:flex;gap:8px;margin-top:8px"><button class="btn" onclick="runAITutor()">Ask</button><button class="btn" onclick="clearAI()">Clear</button></div>
          <div id="aiResult" style="margin-top:10px;min-height:80px;background:#0b0610;padding:8px;border-radius:6px"></div>

          <hr style="margin:12px 0">
          <h4>Flashcard Generator</h4>
          <input id="fcTopic" placeholder="Topic (e.g. Photosynthesis)">
          <div style="display:flex;gap:8px;margin-top:8px"><input id="fcCount" type="number" value="6" style="width:100px"><button class="btn" onclick="generateFlashcards()">Generate</button></div>
          <div id="generatedFC" style="margin-top:10px"></div>

          <hr style="margin:12px 0">
          <h4>Quiz Maker</h4>
          <div style="display:flex;gap:8px;margin-top:8px"><select id="quizSource"><option value="flashcards">From Flashcards</option><option value="custom">Custom</option></select><input id="quizCount" type="number" value="5" style="width:80px"><button class="btn" onclick="buildQuiz()">Build Quiz</button></div>
          <div id="quizArea" style="margin-top:12px"></div>
        </div>

        <div style="display:flex;flex-direction:column;gap:12px">
          <div class="card">
            <h4>Quick Subject Buttons</h4>
            <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:8px">
              <button class="btn small" onclick="prefill('math')">Math</button>
              <button class="btn small" onclick="prefill('science')">Science</button>
              <button class="btn small" onclick="prefill('history')">History</button>
              <button class="btn small" onclick="prefill('reading')">Reading</button>
              <button class="btn small" onclick="prefill('writing')">Writing</button>
              <button class="btn small" onclick="prefill('vocab')">Vocab</button>
            </div>
          </div>

          <div class="card">
            <h4>Saved Flashcards</h4>
            <div id="fcList" style="max-height:220px;overflow:auto;margin-top:8px"></div>
            <div style="margin-top:8px;display:flex;gap:8px"><button class="btn small" onclick="exportFlashcards()">Export</button><button class="btn small" onclick="importFlashcards()">Import</button></div>
          </div>

          <div class="card">
            <h4>Vocabulary</h4>
            <input id="vWord" placeholder="Word"><input id="vDef" placeholder="Definition" style="margin-top:8px">
            <div style="margin-top:8px;display:flex;gap:8px"><button class="btn small" onclick="addVocab()">Add</button></div>
            <div id="vocabList" style="margin-top:8px;max-height:120px;overflow:auto"></div>
          </div>
        </div>
      </div>
    </div>

    <div id="games" class="page">
      <h2>Games</h2>
      <div class="small-muted">Play games in the embedded viewer.</div>
      <div id="gamesGrid" class="grid" style="margin-top:12px"></div>
      <div class="card" style="margin-top:12px">
        <h4>Installed Games</h4>
        <div id="installedGames" class="small-muted"></div>
      </div>
    </div>

    <div id="bookmarklets" class="page">
      <h2>Bookmarklets</h2>
      <div class="small-muted">Copy the code to your clipboard to save as a bookmarklet.</div>
      <div id="bmBoard" class="bm-board"></div>
    </div>

    <div id="updates" class="page">
      <h2>Updates & Version Manager (Manual)</h2>
      <div class="small-muted">Click Install to apply an update (adds/removes features). Click Remove to uninstall.</div>
      <div id="updatesTiles" class="grid" style="grid-template-columns:repeat(auto-fill,minmax(260px,1fr));gap:12px;margin-top:12px"></div>
      <div class="card" style="margin-top:12px"><h4>Changelog</h4><div id="changelog"></div></div>
    </div>

  </div>

  <div id="updateModal" class="update-modal">
    <div id="updateTitle" class="update-title">Downloading Update...</div>
    <div id="updateDesc" class="update-desc">Installing...</div>
    <div class="update-bar-wrap"><div id="updateBar" class="update-bar"></div></div>
  </div>

  <div id="fs" class="full-screen"><div id="fsCard" class="modal-card"></div></div>
  <div class="footer">© 2025 gamesunbloocked67 — v2.0</div>
</div>

<script>
/* ---------------- Persistent content and updates (Manual) ---------------- */
const BASE = {
  games: [
    {id:'snake', title:'Snake', src:'https://www.google.com/logos/2010/snake_arcade/snake_arcade.html'},
    {id:'dino', title:'T-Rex Dino', src:'https://chromedino.com/'}
  ],
  bookmarklets: [
    {id:'edit', title:'Edit Page', code:"javascript:(function(){document.body.contentEditable='true';})();"}
  ],
  studyTools: ['Flashcards','Basic AI Tutor']
};

/* Updates are manual: user must click Install to apply */
const UPDATES = {
  '1.2': {id:'1.2', title:'v1.2', desc:'Add 2048 & vocab builder', add:{games:[{id:'2048',title:'2048',src:'https://play2048.co/'}], bookmarklets:[], studyTools:['Vocabulary Builder']}, remove:{} },
  '1.4': {id:'1.4', title:'v1.4', desc:'Add Retro Bowl & Quiz Maker', add:{games:[{id:'retrobowl',title:'Retro Bowl',src:'https://retrobowlgame.io/'}], bookmarklets:[], studyTools:['Quiz Maker']}, remove:{} },
  '1.6': {id:'1.6', title:'v1.6', desc:'Add study games & bookmarks', add:{games:[{id:'vocabquest',title:'Vocabulary Quest',src:'https://example.com/vocabquest'},{id:'mathmini',title:'Math Mini-Golf',src:'https://example.com/mathminigolf'}], bookmarklets:[{id:'mini',title:'Mini Browser',code:"javascript:(function(){var i=document.createElement('iframe');i.src='https://www.google.com/';i.style.position='fixed';i.style.right='10px';i.style.top='10px';i.style.width='420px';i.style.height='300px';i.style.zIndex=99999;document.body.appendChild(i);})();"}], studyTools:['Study Games']}, remove:{} },
  '1.8': {id:'1.8', title:'v1.8', desc:'AI Real Tutor & Subject Helpers', add:{studyTools:['AI Real Tutor','Math Solver','Science Explainer','History Helper','Writing Coach']}, remove:{} },
  '1.9': {id:'1.9', title:'v1.9', desc:'Flashcard Generator & Quiz Maker', add:{studyTools:['Flashcard Generator','Quiz Maker']}, remove:{} }
};

let content = JSON.parse(localStorage.getItem('gs_content_v2')) || JSON.parse(JSON.stringify(BASE));
let applied = JSON.parse(localStorage.getItem('gs_applied_v2')) || []; // applied update IDs

/* --------------- UI rendering --------------- */
function enterMode(mode){
  document.getElementById('landing').style.display='none';
  document.getElementById('app').style.display='block';
  showSection(mode==='games'?'games':'study');
  renderAll();
  if(mode==='games') setTimeout(()=>alert('Please do not play these on your school device'),120);
}
function showSection(id){ document.querySelectorAll('.page').forEach(p=>p.classList.remove('active')); const el=document.getElementById(id); if(el) el.classList.add('active'); }

function renderAll(){
  renderStudyGrid();
  renderGames();
  renderBookmarklets();
  renderUpdates();
  renderFlashcards();
  renderVocab();
}

/* Study grid simple buttons */
function renderStudyGrid(){
  const grid = document.getElementById('studyGrid'); grid.innerHTML='';
  const tools = content.studyTools || [];
  if(!tools.length) { grid.innerHTML = '<div class="notice">No study tools installed. Install updates from the Updates page.</div>'; return; }
  tools.forEach(t=>{
    const d = document.createElement('div'); d.className='study-card card';
    d.innerHTML = `<h3>${escapeHtml(t)}</h3><div class="small-muted">Open ${escapeHtml(t)}</div>`;
    d.onclick = ()=>{ openTool(t); };
    grid.appendChild(d);
  });
}
function openTool(t){
  showSection('study');
  const aiInput = document.getElementById('aiInput');
  if(t.toLowerCase().includes('math')) prefill('math');
  else if(t.toLowerCase().includes('photosynthesis')||t.toLowerCase().includes('science')) prefill('science');
  else if(t.toLowerCase().includes('flash')) { document.getElementById('fcTopic').focus(); }
  else { aiInput.value = 'Explain: ' + t; runAITutor(); }
}

/* Games & bookmarklets rendering */
function renderGames(){
  const g = document.getElementById('gamesGrid'); g.innerHTML='';
  (content.games||[]).forEach(game=>{
    const el = document.createElement('div'); el.className='card';
    el.innerHTML = `<h4>${escapeHtml(game.title)}</h4><div class="small-muted">${escapeHtml(game.src)}</div><div style="margin-top:8px"><button class="btn small" onclick="openGame('${encodeURIComponent(game.src)}')">Play</button></div>`;
    g.appendChild(el);
  });
  document.getElementById('installedGames').innerText = (content.games||[]).map(x=>x.title).join(', ');
}
function openGame(urlEnc){ const url = decodeURIComponent(urlEnc); const fs = document.getElementById('fs'); const card = document.getElementById('fsCard'); card.innerHTML = `<div style="display:flex;justify-content:space-between;align-items:center"><strong>Game</strong><button class="btn small" onclick="closeFS()">Close</button></div><div style="height:72vh;margin-top:8px;"><iframe src="${escapeHtml(url)}" style="width:100%;height:100%;border:none"></iframe></div>`; fs.style.display='flex'; }
function closeFS(){ document.getElementById('fs').style.display='none'; document.getElementById('fsCard').innerHTML=''; }

function renderBookmarklets(){
  const board = document.getElementById('bmBoard'); board.innerHTML='';
  (content.bookmarklets||[]).forEach((b,i)=>{ const c = document.createElement('div'); c.className='bm-card card'; c.innerHTML = `<strong>${escapeHtml(b.title)}</strong><div style="font-size:12px;color:#ddd;white-space:pre-wrap;margin-top:8px">${escapeHtml(b.code||'')}</div><div style="margin-top:8px"><button class="btn small" onclick="copyBM(${i})">Copy</button></div>`; board.appendChild(c); });
}
function copyBM(i){ const code = (content.bookmarklets||[])[i]?.code || ''; if(!code){ alert('No code'); return; } navigator.clipboard.writeText(code).then(()=>alert('Copied bookmarklet')); }

/* Updates rendering: manual installs/removals */
function renderUpdates(){
  const tiles = document.getElementById('updatesTiles'); tiles.innerHTML='';
  Object.values(UPDATES).forEach(u=>{
    const appliedFlag = applied.includes(u.id);
    const card = document.createElement('div'); card.className='card';
    card.innerHTML = `<h4>${escapeHtml(u.title)}</h4><div class="small-muted">${escapeHtml(u.desc)}</div>
      <div style="margin-top:8px;display:flex;gap:8px">
        <button class="btn small" onclick="installUpdate('${u.id}')" ${appliedFlag? 'disabled':''}>Install</button>
        <button class="btn small" onclick="uninstallUpdate('${u.id}')" ${appliedFlag? '':'disabled'}>Remove</button>
      </div>
      <div style="margin-top:8px" class="small-muted">Status: ${appliedFlag? 'Installed':'Not installed'}</div>`;
    tiles.appendChild(card);
  });
  const ch = document.getElementById('changelog'); ch.innerHTML = '';
  applied.forEach(id=>{ if(UPDATES[id]){ const d = document.createElement('div'); d.style.marginTop='6px'; d.innerHTML = `<strong>${escapeHtml(UPDATES[id].title)}</strong> — ${escapeHtml(UPDATES[id].desc)}`; ch.appendChild(d); } });
}

/* Install update (manual) with modal progress, then apply changes */
function installUpdate(id){
  const upd = UPDATES[id]; if(!upd) return alert('Update not found');
  if(applied.includes(id)) return alert('Already installed');
  // show modal
  const modal = document.getElementById('updateModal'); const title = document.getElementById('updateTitle'); const desc = document.getElementById('updateDesc'); const bar = document.getElementById('updateBar');
  title.innerText = `Installing ${upd.title}...`; desc.innerText = upd.desc; bar.style.width='0%'; modal.style.display='block';
  let p = 0; const timer = setInterval(()=>{ p += Math.random()*12 + 5; if(p>100) p=100; bar.style.width = p + '%'; if(p>=100){ clearInterval(timer); setTimeout(()=>{ modal.style.display='none'; applyUpdate(upd); alert(upd.title + ' installed'); },600); } },150);
}

/* Uninstall update: remove its additions and revert to base+other applied updates */
function uninstallUpdate(id){
  if(!applied.includes(id)) return alert('Not installed');
  // remove id from applied and rebuild content
  applied = applied.filter(x=> x !== id);
  // rebuild starting from BASE and reapply applied updates in order
  content = JSON.parse(JSON.stringify(BASE));
  applied.forEach(aid=>{ if(UPDATES[aid]) applyUpdateToContent(UPDATES[aid], content); });
  persist();
  renderAll();
  renderUpdates();
  alert('Removed ' + id);
}

/* Apply update: add/remove content and persist */
function applyUpdate(u){
  applyUpdateToContent(u, content);
  applied.push(u.id);
  persist();
  renderAll();
  renderUpdates();
}

/* Core function to modify content object */
function applyUpdateToContent(u, cont){
  if(u.add){
    if(u.add.games) {
      cont.games = cont.games || [];
      u.add.games.forEach(g=>{ if(!cont.games.find(x=>x.id===g.id)) cont.games.push(g); });
    }
    if(u.add.bookmarklets){
      cont.bookmarklets = cont.bookmarklets || [];
      u.add.bookmarklets.forEach(b=>{ if(!cont.bookmarklets.find(x=>x.id===b.id)) cont.bookmarklets.push(b); });
    }
    if(u.add.studyTools){
      cont.studyTools = cont.studyTools || [];
      u.add.studyTools.forEach(t=>{ if(!cont.studyTools.includes(t)) cont.studyTools.push(t); });
    }
  }
  if(u.remove){
    if(u.remove.games) cont.games = (cont.games||[]).filter(g=> !u.remove.games.includes(g.id));
    if(u.remove.bookmarklets) cont.bookmarklets = (cont.bookmarklets||[]).filter(b=> !u.remove.bookmarklets.includes(b.id));
    if(u.remove.studyTools) cont.studyTools = (cont.studyTools||[]).filter(t=> !u.remove.studyTools.includes(t));
  }
}

/* Persist content and applied list */
function persist(){
  localStorage.setItem('gs_content_v2', JSON.stringify(content));
  localStorage.setItem('gs_applied_v2', JSON.stringify(applied));
}

/* -------------- Flashcards and Quiz functions -------------- */
let FLASHCARDS = JSON.parse(localStorage.getItem('gs_flashcards_v2')||'[]');
function renderFlashcards(){ const el = document.getElementById('fcList'); if(!el) return; el.innerHTML=''; FLASHCARDS.forEach((f,i)=>{ const d = document.createElement('div'); d.style.marginTop='8px'; d.innerHTML = `<strong>${escapeHtml(f.term)}</strong><div style="font-size:13px;color:#ddd">${escapeHtml(f.def)}</div><div style="margin-top:6px"><button class="btn small" onclick="removeFlash(${i})">Remove</button></div>`; el.appendChild(d); }); }
function addFlashcardFromGenerated(term,def){ FLASHCARDS.push({term,def}); persistFlashcards(); renderFlashcards(); }
function exportFlashcards(){ prompt('Copy flashcards JSON', JSON.stringify(FLASHCARDS)); }
function importFlashcards(){ const s = prompt('Paste flashcards JSON'); try{ const arr = JSON.parse(s||'[]'); if(Array.isArray(arr)){ FLASHCARDS = FLASHCARDS.concat(arr); persistFlashcards(); renderFlashcards(); alert('Imported'); } }catch(e){alert('Invalid JSON');} }
function removeFlash(i){ FLASHCARDS.splice(i,1); persistFlashcards(); renderFlashcards(); }
function persistFlashcards(){ localStorage.setItem('gs_flashcards_v2', JSON.stringify(FLASHCARDS)); }

function generateFlashcards(){
  const topic = (document.getElementById('fcTopic').value||'').trim(); const count = Math.max(1, Math.min(20, parseInt(document.getElementById('fcCount').value)||6));
  if(!topic) return alert('Enter a topic');
  const cards = makeFlashcardsFromTopic(topic, count);
  const out = document.getElementById('generatedFC'); out.innerHTML=''; cards.forEach(c=>{ const el = document.createElement('div'); el.style.marginTop='8px'; el.innerHTML = `<strong>${escapeHtml(c.term)}</strong><div style="color:#ddd">${escapeHtml(c.def)}</div><div style="margin-top:6px"><button class="btn small" onclick='addFlashcardFromGenerated(${JSON.stringify(c.term)}, ${JSON.stringify(c.def)})'>Save</button></div>`; out.appendChild(el); });
}
function makeFlashcardsFromTopic(topic,n){
  const base = topic.split(/\s+/).slice(0,3).join(' ');
  const cards=[]; for(let i=0;i<n;i++){ cards.push({term:`${base} — key idea #${i+1}`, def:`Explanation or definition for ${base} concept ${i+1}.`}); } return cards;
}

function buildQuiz(){
  const source = document.getElementById('quizSource').value; const count = Math.max(1, Math.min(20, parseInt(document.getElementById('quizCount').value) || 5));
  let questions=[];
  if(source==='flashcards'){
    if(FLASHCARDS.length < 2) return alert('Need at least 2 flashcards for a quiz');
    const pool = FLASHCARDS.slice(); shuffle(pool); const picks = pool.slice(0, Math.min(count,pool.length));
    questions = picks.map(p=> makeMCQFromFlashcard(p, FLASHCARDS));
  } else {
    const raw = prompt('Enter custom Q&A pairs as "Question|Answer" each on a new line (max ' + count + '):'); if(!raw) return;
    const lines = raw.split(/\n/).slice(0,count);
    questions = lines.map(l=>{ const parts = l.split('|'); return {q:parts[0]||'Question', correct:parts[1]||'Answer', choices:generateDistractors(parts[1]||'Answer')}; });
  }
  renderQuiz(questions);
}
function makeMCQFromFlashcard(f,all){ const correct = f.def; const choices=[correct]; const others = all.filter(x=>x.def!==correct).map(x=>x.def).slice(0,3); while(choices.length<4 && others.length) choices.push(others.shift()); while(choices.length<4) choices.push(correct+' (check)'); shuffle(choices); return {q:f.term, choices, correct}; }
function generateDistractors(val){ if(!val) return [val,val+' (no)',val+' (maybe)','None of the above']; const words = val.split(/\s+/); const d1 = words.reverse().join(' '); return [val,d1,val+' (near)','None of the above'].slice(0,4); }
function renderQuiz(questions){ const area = document.getElementById('quizArea'); area.innerHTML=''; if(!questions||!questions.length){ area.innerText='No questions'; return; } const form = document.createElement('div'); questions.forEach((qq,idx)=>{ const qdiv=document.createElement('div'); qdiv.style.marginTop='8px'; qdiv.innerHTML = `<div><strong>Q${idx+1}.</strong> ${escapeHtml(qq.q)}</div>`; qq.choices.forEach((ch,i)=>{ const r=document.createElement('div'); r.style.marginTop='6px'; r.innerHTML = `<label><input type="radio" name="q${idx}" value="${escapeHtml(ch)}"> ${escapeHtml(ch)}</label>`; qdiv.appendChild(r); }); form.appendChild(qdiv); }); const submit = document.createElement('div'); submit.style.marginTop='12px'; submit.innerHTML = `<button class="btn" onclick="gradeQuiz()">Submit</button> <button class="btn" onclick="clearQuiz()">Clear</button>`; form.appendChild(submit); window._currentQuiz = questions; area.appendChild(form); }
function gradeQuiz(){ const qs = window._currentQuiz || []; let score=0; for(let i=0;i<qs.length;i++){ const radios = document.getElementsByName('q'+i); let chosen=null; for(const r of radios){ if(r.checked) chosen = r.value; } if(chosen && chosen === qs[i].correct) score++; } alert('Score: ' + score + ' / ' + qs.length); }
function clearQuiz(){ document.getElementById('quizArea').innerHTML=''; window._currentQuiz=null; }

/* --------------- AI Tutor --------------- */
function runAITutor(){ const q=(document.getElementById('aiInput').value||'').trim(); const out=document.getElementById('aiResult'); if(!q){ out.innerText='Type a question to get help.'; return; } const r = simpleTutor(q); out.innerHTML = `<strong>Hint:</strong> ${escapeHtml(r.hint)}<div style="margin-top:8px"><strong>Example / Steps:</strong><div>${escapeHtml(r.example)}</div></div>`; }
function simpleTutor(q){ const s=q.toLowerCase(); if(s.match(/\d/)){ const m=tryMathSolve(s); if(m.found) return {hint:m.hint, example:m.example}; } if(s.includes('photosynthesis')||s.includes('mitosis')||s.includes('cell')) return {hint:'Identify inputs, processes, and outputs.', example:'Photosynthesis: sunlight + CO2 + water -> glucose + O2'}; if(s.includes('fraction')||s.includes('/')) return {hint:'Find common denominator or convert to decimals.', example:'1/2 + 1/4 = 3/4'}; if(s.includes('essay')||s.includes('paragraph')) return {hint:'Start with topic sentence, add supporting details, end with conclusion.', example:'Intro: State your claim clearly.'}; if(s.includes('summar')||s.includes('reading')) return {hint:'Find the main idea and 2-3 supporting details.', example:'Main idea: X; Details: A, B, C.'}; return {hint:'Break into smaller steps; identify key terms; try an example.', example:'Summarize main idea in one sentence.'}; }
function tryMathSolve(s){ try{ const cleaned = s.replace(/[^0-9+\-*/().\/ ]/g,''); if(cleaned.match(/[0-9]/) && cleaned.match(/[\+\-\*\/]/)){ const val = Function('"use strict";return ('+cleaned+')')(); return {found:true, hint:'Compute step-by-step and check arithmetic.', example:'Result ≈ ' + val}; } }catch(e){} return {found:false}; }

/* --------------- Vocab --------------- */
let VOCAB = JSON.parse(localStorage.getItem('gs_vocab_v2')||'[]');
function addVocab(){ const w=document.getElementById('vWord').value.trim(); const d=document.getElementById('vDef').value.trim(); if(!w||!d){alert('Add word & definition');return;} VOCAB.push({w,d}); localStorage.setItem('gs_vocab_v2',JSON.stringify(VOCAB)); renderVocab(); document.getElementById('vWord').value=''; document.getElementById('vDef').value=''; }
function renderVocab(){ const el=document.getElementById('vocabList'); el.innerHTML=''; VOCAB.forEach((v,i)=>{ const d=document.createElement('div'); d.style.marginTop='6px'; d.innerHTML=`<strong>${escapeHtml(v.w)}</strong><div style="font-size:13px;color:#ddd">${escapeHtml(v.d)}</div><div style="margin-top:6px"><button class="btn small" onclick="removeVocab(${i})">Remove</button></div>`; el.appendChild(d); }); }
function removeVocab(i){ VOCAB.splice(i,1); localStorage.setItem('gs_vocab_v2',JSON.stringify(VOCAB)); renderVocab(); }

/* Utilities */
function escapeHtml(s){ if(!s) return ''; return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;'); }
function shuffle(a){ for(let i=a.length-1;i>0;i--){ const j=Math.floor(Math.random()*(i+1)); [a[i],a[j]]=[a[j],a[i]]; } }
function prefill(subject){ const map={math:'Solve 3/4 + 1/2',science:'Explain photosynthesis',history:'Causes of the American Revolution',reading:'How to summarize a paragraph',writing:'Help me write a strong intro'}; document.getElementById('aiInput').value = map[subject]||''; runAITutor(); }

/* Init */
document.addEventListener('DOMContentLoaded', ()=>{
  // if first time, apply some updates to make app feel full by default (still manual - they are marked as applied so user may remove)
  if(!applied.length){ ['1.2','1.4','1.6','1.8'].forEach(id=>{ if(UPDATES[id]){ applyUpdateToContent(UPDATES[id], content); applied.push(id); } }); persist(); }
  renderAll();
  // render update list options
  const sim = document.getElementById('simGameSelect'); if(sim){ Object.values(UPDATES).forEach(u=>{ const o=document.createElement('option'); o.value=u.id; o.text=u.title; sim.appendChild(o); }); }
});

/* Persist on unload */
window.addEventListener('beforeunload', ()=>{ persist(); persistFlashcards(); localStorage.setItem('gs_vocab_v2', JSON.stringify(VOCAB)); });

</script>
</body>
</html>
