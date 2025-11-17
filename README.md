<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>gamesunbloocked67 — WebLLM (Medium) Ready</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#0d0420;--card:#151224;--accent:#ff7a38;--muted:#dcd3ef;
}
*{box-sizing:border-box}
html,body{margin:0;height:100%;font-family:Roboto,system-ui,-apple-system,Segoe UI,Arial;background:linear-gradient(180deg,var(--bg),#30103b);color:var(--muted);-webkit-font-smoothing:antialiased}
.container{max-width:1100px;margin:14px auto;padding:14px}
.header{display:flex;justify-content:space-between;align-items:center;gap:12px;flex-wrap:wrap}
.brand{font-weight:700;font-size:20px}
.note{font-size:13px}
nav{display:flex;gap:8px;margin-top:12px;flex-wrap:wrap}
.tab{padding:8px 12px;border-radius:10px;background:rgba(255,255,255,0.02);cursor:pointer}
.tab.active{outline:2px solid rgba(255,255,255,0.04);box-shadow:0 6px 24px rgba(0,0,0,0.6)}
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:12px;margin-top:14px}
.card{background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(255,255,255,0.01));padding:12px;border-radius:10px;border:1px solid rgba(255,255,255,0.03)}
.small{font-size:13px;color:rgba(255,255,255,0.8)}
.btn{background:var(--accent);color:#120018;padding:8px 10px;border-radius:8px;border:none;font-weight:700;cursor:pointer}
.full-screen{position:fixed;inset:0;background:rgba(0,0,0,0.75);display:none;align-items:center;justify-content:center;z-index:9998;padding:24px}
.modal-card{background:#0b0b0b;padding:14px;border-radius:12px;width:92%;max-width:1100px;max-height:88vh;overflow:auto}
.input,textarea,select{width:100%;padding:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.03);background:#0a0710;color:var(--muted);margin-top:8px}
.panel{position:fixed;background:var(--card);border:2px solid var(--accent);padding:10px;border-radius:10px;z-index:9999;cursor:move;display:none;max-height:80vh;overflow:auto;width:300px}
.panel h4{margin:0 0 8px 0;color:var(--accent)}
.small-muted{font-size:12px;color:rgba(255,255,255,0.65)}
.q-card{background:#0e0b13;padding:10px;border-radius:8px;margin-top:8px}
.choice{background:#120418;padding:8px;border-radius:8px;margin-top:6px;cursor:pointer}
.choice.selected{outline:2px solid rgba(255,200,0,0.22)}
.footer{margin-top:18px;text-align:center;color:rgba(255,255,255,0.6);font-size:13px}
@media (max-width:520px){
  .grid{grid-template-columns:repeat(auto-fill,minmax(140px,1fr))}
}
</style>
</head>
<body>
<div class="container">
  <div class="header">
    <div class="brand">gamesunbloocked67 <small id="ver">(WebLLM-ready)</small></div>
    <div class="note small-muted">Study Hub + 100 games • Browser AI (medium) supported — no server required</div>
  </div>

  <nav>
    <div class="tab active" id="tab-games" onclick="showTab('games')">Games</div>
    <div class="tab" id="tab-study" onclick="showTab('study')">Study</div>
    <div class="tab" id="tab-bookmarklets" onclick="showTab('bookmarklets')">Bookmarklets</div>
    <div class="tab" id="tab-settings" onclick="showTab('settings')">Settings</div>
    <div class="tab" id="tab-updates" onclick="showTab('updates')">Updates</div>
  </nav>

  <!-- GAMES -->
  <section id="games" style="display:block">
    <div class="grid" id="gamesGrid"></div>
    <div class="small" style="margin-top:10px">Tip: Click Play. If embedding is blocked, use "Open in new tab".</div>
  </section>

  <!-- STUDY -->
  <section id="study" style="display:none">
    <div class="card">
      <h3>Study Hub</h3>
      <div class="small-muted">AI Tutor · Quiz Maker · Flashcards (WebLLM medium model recommended)</div>

      <label class="small-muted" style="margin-top:10px">Select subject</label>
      <select id="subjectSelect" class="input"><option>Math</option><option>Science</option><option>English</option></select>

      <label class="small-muted" style="margin-top:8px">AI Tutor — type your question</label>
      <textarea id="aiQuestion" rows="2" class="input" placeholder="e.g. Solve 2x+5=17 step-by-step or explain photosynthesis"></textarea>
      <div style="display:flex;gap:8px;margin-top:8px">
        <button class="btn" onclick="askWebLLM()">Ask WebLLM</button>
        <button class="btn" onclick="askLocal()">Ask Local (fallback)</button>
        <button class="btn" onclick="startStudyMode()">Start Study Mode</button>
      </div>

      <div id="aiTutorArea" style="margin-top:12px"></div>

      <hr style="border:none;border-top:1px solid rgba(255,255,255,0.04);margin:12px 0">

      <label class="small-muted">Quiz Maker</label>
      <input id="quizPrompt" class="input" placeholder="e.g. photosynthesis basics" />
      <div style="display:flex;gap:8px;margin-top:8px">
        <input id="quizCount" type="number" value="5" min="1" max="20" style="width:80px;padding:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.03);background:#0a0710;color:var(--muted)">
        <button class="btn" onclick="generateQuiz()">Generate Quiz</button>
      </div>
      <div id="quizArea" style="margin-top:12px"></div>

      <hr style="border:none;border-top:1px solid rgba(255,255,255,0.04);margin:12px 0">

      <label class="small-muted">Flashcard Maker</label>
      <input id="flashPrompt" class="input" placeholder="e.g. photosynthesis" />
      <div style="display:flex;gap:8px;margin-top:8px">
        <input id="flashCount" type="number" value="8" min="1" max="50" style="width:80px;padding:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.03);background:#0a0710;color:var(--muted)">
        <button class="btn" onclick="createFlashcards()">Create Flashcards</button>
      </div>
      <div id="flashArea" style="margin-top:12px"></div>
    </div>
  </section>

  <!-- BOOKMARKLETS -->
  <section id="bookmarklets" style="display:none">
    <div class="card">
      <h3>Bookmarklets</h3>
      <div class="small-muted">Draggable panel available — copy or run bookmarklets.</div>
      <div style="margin-top:10px;display:flex;gap:8px">
        <button class="btn" onclick="toggleBMPanel()">Toggle Panel</button>
        <button class="btn" onclick="addCustomBookmarklet()">Add custom</button>
      </div>
    </div>
  </section>

  <!-- SETTINGS -->
  <section id="settings" style="display:none">
    <div class="card">
      <h3>Settings</h3>
      <div class="small-muted">WebLLM model (medium: 4–8B) — supply a direct model bundle / endpoint URL if you have one.</div>
      <label class="small-muted" style="margin-top:8px">Model bundle / endpoint URL (optional)</label>
      <input id="modelUrl" class="input" placeholder="Paste a WebLLM-compatible model bundle URL or endpoint here (leave blank to use local fallback)" />
      <div style="margin-top:8px;display:flex;gap:8px">
        <button class="btn" onclick="loadWebLLMModel()">Load model</button>
        <button class="btn" onclick="unloadWebLLMModel()" style="background:#666;color:#fff">Unload model</button>
      </div>
      <div style="margin-top:12px" class="small-muted">If you don't have a hosted bundle, leave empty — the local fallback tutor will work.</div>
    </div>
  </section>

  <!-- UPDATES -->
  <section id="updates" style="display:none">
    <div class="card">
      <h3>Updates</h3>
      <div class="small-muted">Manual updates system (none pending)</div>
    </div>
  </section>

  <div class="footer">© 2025 gamesunbloocked67</div>
</div>

<!-- Draggable bookmarklet panel & tab cloaker -->
<div id="bmPanel" class="panel" style="left:18px;top:110px">
  <h4>Bookmarklets</h4>
  <div id="bmList" class="small-muted"></div>
  <div style="margin-top:8px" class="small-muted">Drag this panel — click Run to open bookmarklet.</div>
</div>

<div id="cloaker" class="panel" style="left:340px;top:110px">
  <h4>Tab Cloaker</h4>
  <a href="https://drive.google.com" target="_blank" class="btn" style="display:block;text-align:center">Open Google Drive</a>
  <div class="small-muted" style="margin-top:8px">Cloaker bookmarklet (copy below)</div>
  <textarea id="cloakCode" class="input" rows="3">javascript:(function(){window.location='https://drive.google.com';})();</textarea>
  <button class="btn" style="margin-top:8px" onclick="copyText(document.getElementById('cloakCode').value)">Copy Cloaker</button>
</div>

<!-- full-screen modal -->
<div class="full-screen" id="fs" style="display:none">
  <div class="modal-card" id="fsCard"></div>
</div>

<script>
/* ============================
   App-wide state & helpers
   ============================ */

const STATE = {
  games: [],            // will hold 100 games
  bm : [],              // bookmarklets
  webllm: null,         // web-llm model runtime object (if loaded)
  webllmLoaded: false,  // flag
  modelUrl: ''          // user-provided model URL
};

function escapeHtml(s){ if(!s) return ''; return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); }
function escapeAttr(s){ if(!s) return (s||''); return String(s).replace(/"/g,'%22').replace(/'/g,"%27"); }

/* ----------------------------
   Populate 100 games (geometry-lessons .html)
   ---------------------------- */
(function populateGames(){
  for(let i=1;i<=100;i++){
    STATE.games.push({
      id: 'g'+i,
      title: 'Geometry Game ' + i,
      src: `https://geometry-lessons.github.io/game${i}.html`
    });
  }
})();

function renderGames(){
  const grid = document.getElementById('gamesGrid');
  grid.innerHTML = '';
  STATE.games.forEach(g=>{
    const card = document.createElement('div'); card.className='card';
    card.innerHTML = `<div style="font-weight:700">${escapeHtml(g.title)}</div>
      <div class="small" style="margin-top:6px">${escapeHtml(g.src)}</div>
      <div style="margin-top:10px;display:flex;gap:8px">
        <button class="btn" onclick="playGame('${escapeAttr(g.src)}','${escapeAttr(g.title)}')">Play</button>
        <button class="btn" style="background:#666;color:#fff" onclick="window.open('${escapeAttr(g.src)}','_blank')">Open in new tab</button>
      </div>`;
    grid.appendChild(card);
  });
}

/* ----------------------------
   Play game in fullscreen iframe
   ---------------------------- */
function playGame(src,title){
  const html = `<div style="display:flex;justify-content:space-between;align-items:center">
    <strong>${escapeHtml(title)}</strong><div style="display:flex;gap:8px"><button class="btn" onclick="openInNew()">Open in new tab</button><button class="btn" onclick="closeFS()">Close</button></div></div>
    <div style="height:72vh;margin-top:8px"><iframe id="gameFrame" src="${src}" style="width:100%;height:100%;border:none" sandbox="allow-scripts allow-forms allow-same-origin"></iframe></div>`;
  openFS(html);
  // helper open in new tab
  window.openInNew = ()=>window.open(src,'_blank');
}

/* ----------------------------
   Fullscreen/modal helpers
   ---------------------------- */
function openFS(html){
  document.getElementById('fsCard').innerHTML = html;
  document.getElementById('fs').style.display = 'flex';
}
function closeFS(){
  document.getElementById('fs').style.display = 'none';
  document.getElementById('fsCard').innerHTML = '';
}

/* ----------------------------
   Tabs
   ---------------------------- */
function showTab(id){
  ['games','study','bookmarklets','settings','updates'].forEach(s=>{
    document.getElementById(s).style.display = (s===id?'block':'none');
    const tab = document.getElementById('tab-'+s);
    if(tab) tab.classList.toggle('active', s===id);
  });
}

/* ----------------------------
   Bookmarklets
   ---------------------------- */
(function defaultBookmarklets(){
  // 20 bookmarklets: titles + code
  const list = [
    ['Mini Browser',"javascript:(function(){var u=prompt('Open URL','https://'); if(u) window.open(u,'_blank');})();"],
    ['Show Cookies',"javascript:(function(){alert(document.cookie);})();"],
    ['Toggle Invert',"javascript:(function(){document.documentElement.style.filter=document.documentElement.style.filter?'':'invert(1) hue-rotate(180deg)';})();"],
    ['Highlight Links',"javascript:(function(){document.querySelectorAll('a').forEach(a=>a.style.background='yellow');})();"],
    ['Remove Images',"javascript:(function(){document.querySelectorAll('img').forEach(i=>i.remove());})();"],
    ['Increase Font',"javascript:(function(){document.body.style.fontSize='18px';})();"],
    ['Decrease Font',"javascript:(function(){document.body.style.fontSize='12px';})();"],
    ['Auto Scroll',"javascript:(function(){window._autoScroll=setInterval(()=>window.scrollBy(0,1),20);})();"],
    ['Stop Auto Scroll',"javascript:(function(){clearInterval(window._autoScroll);})();"],
    ['View H1s',"javascript:(function(){alert(Array.from(document.querySelectorAll('h1')).map(h=>h.innerText).join('\\n'));})();"],
    ['Download Images',"javascript:(function(){Array.from(document.images).forEach((img,i)=>{const a=document.createElement('a');a.href=img.src;a.download='img'+i;document.body.appendChild(a);a.click();a.remove();});})();"],
    ['Translate Page (Google)',"javascript:(function(){window.open('https://translate.google.com/translate?sl=auto&tl=en&u='+encodeURIComponent(location.href));})();"],
    ['Outline Blocks',"javascript:(function(){document.querySelectorAll('*').forEach(e=>e.style.outline='1px solid rgba(255,0,0,0.12)');})();"],
    ['Read Aloud',"javascript:(function(){var t=document.body.innerText;var s=new SpeechSynthesisUtterance(t);speechSynthesis.speak(s);})();"],
    ['Print Page',"javascript:(function(){window.print();})();"],
    ['Toggle Images Visibility',"javascript:(function(){document.querySelectorAll('img').forEach(i=>i.style.visibility=i.style.visibility==='hidden'?'visible':'hidden');})();"],
    ['Alert Time',"javascript:(function(){alert(new Date());})();"],
    ['Find H2s',"javascript:(function(){alert(Array.from(document.querySelectorAll('h2')).map(h=>h.innerText).join('\\n'));})();"],
    ['Remove Ads (basic)',"javascript:(function(){document.querySelectorAll('iframe,ins,[id^=ad],.ad').forEach(e=>e.remove());})();"],
    ['Bookmarklet Info',"javascript:(function(){alert('gamesunbloocked67 bookmarklets');})();"]
  ];
  STATE.bm = list.map((x,i)=>({id:'bm'+(i+1), title:x[0], code:x[1]}));
})();

function renderBMPanel(){
  const el = document.getElementById('bmList'); el.innerHTML = '';
  STATE.bm.forEach(b=>{
    const d = document.createElement('div'); d.style.marginBottom='10px';
    d.innerHTML = `<div style="display:flex;gap:8px;align-items:center">
      <div style="flex:1;font-weight:700">${escapeHtml(b.title)}</div>
      <div><button class="btn" onclick="copyText('${b.code.replace(/'/g,"\\'")}')">Copy</button></div>
      <div><a class="btn" href="${b.code}" target="_blank">Run</a></div>
      </div><div class="small-muted" style="margin-top:6px;font-family:monospace;word-break:break-all">${escapeHtml(b.code)}</div>`;
    el.appendChild(d);
  });
}

function toggleBMPanel(){ const p = document.getElementById('bmPanel'); p.style.display = p.style.display==='block'?'none':'block'; }

function addCustomBookmarklet(){
  const title = prompt('Bookmarklet title');
  const code = prompt('Bookmarklet code (start with javascript:)');
  if(title && code){ STATE.bm.push({id:'bm'+(STATE.bm.length+1), title, code}); renderBMPanel(); alert('Added'); }
}
function copyText(t){ navigator.clipboard.writeText(t).then(()=>alert('Copied')); }

/* Draggable helper for panels */
function makeDraggable(el){
  let dragging=false, ox=0, oy=0;
  el.addEventListener('mousedown', e=>{
    dragging = true;
    ox = e.clientX - (el.offsetLeft || 0);
    oy = e.clientY - (el.offsetTop || 0);
    el.style.cursor = 'grabbing';
  });
  document.addEventListener('mouseup', ()=>{ dragging=false; el.style.cursor='grab'; });
  document.addEventListener('mousemove', e=>{
    if(!dragging) return;
    el.style.left = (e.clientX - ox) + 'px';
    el.style.top = (e.clientY - oy) + 'px';
  });
  el.style.left = el.style.left || (20 + Math.random()*80) + 'px';
  el.style.top = el.style.top || (110 + Math.random()*60) + 'px';
  el.style.cursor='grab';
}

/* ----------------------------
   Tab Cloaker copy helper
   ---------------------------- */
function copyCloaker(){ copyText(document.getElementById('cloakCode').value); }

/* ----------------------------
   Local fallback AI (fast, no model)
   ---------------------------- */
function askLocal(){
  const subj = document.getElementById('subjectSelect').value;
  const q = document.getElementById('aiQuestion').value.trim();
  if(!q) return alert('Please enter a question');
  const outEl = document.getElementById('aiTutorArea');
  outEl.innerHTML = '<div class="card"><div class="small-muted">Thinking (local) …</div></div>';
  setTimeout(()=>{
    const resp = localTutor(subj,q);
    outEl.innerHTML = `<div class="card"><pre style="white-space:pre-wrap">${escapeHtml(resp)}</pre></div>`;
  }, 150);
}

function localTutor(subject, q){
  // improved local heuristics: math solver for linear equations and arithmetic, plus structured text for others
  const lower = q.toLowerCase();
  if(subject==='Math' && /[0-9x\+\-\*\/\(\)=]/.test(q)){
    // try simple linear eq: ax+b=c
    const eq = q.replace(/\s+/g,'');
    const lin = eq.match(/^([+-]?\d*)x([+-]\d+)?=([+-]?\d+)$/i);
    if(lin){
      let a = lin[1]; if(a===''||a==='+') a='1'; if(a==='-') a='-1'; a = Number(a);
      const b = lin[2] ? Number(lin[2]) : 0;
      const c = Number(lin[3]);
      const x = (c - b) / a;
      return `Solve ${a}x ${b>=0?'+':'-'} ${Math.abs(b)} = ${c}\n1) Subtract ${b}: ${a}x = ${c-b}\n2) Divide by ${a}: x = ${x}\nAnswer: x = ${x}\n\nIf you want step-by-step hints, ask "next step".`;
    }
    // arithmetic expression
    try{
      const cleaned = q.replace(/[^0-9+\-*/(). ]/g,'');
      const val = Function('"use strict";return ('+cleaned+')')();
      return `Compute: ${cleaned}\nResult: ${val}\nBreakdown: follow parentheses, then * /, then + -`;
    }catch(e){}
  }
  // non-math: structured guidance
  if(subject==='Science'){
    if(lower.includes('photosynthesis')){
      return `Photosynthesis (short):\n1) Inputs: CO2 + H2O + sunlight\n2) Light reactions convert sunlight to chemical energy (ATP/NADPH)\n3) Calvin cycle builds glucose from CO2\n4) Output: glucose + O2\n\nAsk for "light reactions details" or "calvin cycle steps" to drill down.`;
    }
  }
  if(subject==='English'){
    if(lower.includes('essay')){
      return `Essay writing guide:\n1) Hook + thesis statement\n2) 3 body paragraphs each with claim + evidence\n3) Conclusion restates thesis and adds perspective\n\nI can help write outlines or full paragraphs — ask for "outline" or "paragraph 1".`;
    }
  }
  // fallback generic tutor
  return `Here's how to approach "${q}":\n1) Restate the question in your own words\n2) Break into smaller tasks\n3) Solve each task with an example\n4) Check your results\n\nAsk for "an example" or "simplify step 1" to continue.`;
}

/* ----------------------------
   WebLLM integration (browser-based)
   ---------------------------- */

/*
  Implementation notes:
  - We don't bundle a model here. Instead the page attempts to load a WebLLM runtime if a model URL is supplied in Settings.
  - If you have a WebLLM-compatible model bundle URL or hosted endpoint that provides a runnable inference (a .json + weight files or a web-service), put that URL into Settings -> Model bundle / endpoint URL and click "Load model".
  - For iPad (medium 4-8B), pick a model bundle labeled for WebLLM or MLC Web. After loading the runtime, the page will call the model for tutor/quiz/flashcard tasks.
  - If model fails to load, the page falls back to the local JS tutor.
*/

async function loadWebLLMModel(){
  const url = document.getElementById('modelUrl').value.trim();
  if(!url) return alert('Paste a WebLLM-compatible model bundle URL or endpoint in Settings first.');
  STATE.modelUrl = url;
  // Try to import web-llm runtime from CDN (best-effort). If the CDN isn't reachable or the runtime changes, fallback to local.
  openFS(`<div style="display:flex;justify-content:space-between;align-items:center"><strong>Loading model</strong><button class="btn" onclick="closeFS()">Cancel</button></div><div style="margin-top:12px">Attempting to load WebLLM runtime and model. This may take a while depending on model size and your device.</div>`);
  try{
    // dynamic import from mlc web-llm (best-effort). We don't hard-code a single CDN to avoid breaking; try a common URL.
    // The code below attempts to load the runtime in one of two ways. If neither succeeds, we show instructions.
    try{
      // attempt to load @mlc-ai/web-llm via unpkg/esm
      if(!window.WebLLM){
        const s = document.createElement('script');
        s.src = 'https://unpkg.com/@mlc-ai/web-llm/dist/webllm.min.js';
        document.head.appendChild(s);
        await new Promise((res,rej)=>{ s.onload=res; s.onerror=rej; setTimeout(()=>{},200); });
      }
    }catch(e){
      console.warn('web-llm CDN load failed',e);
    }

    if(!window.WebLLM){
      // fallback: maybe mlc is available as global mlc? try another CDN
      try{
        const s2 = document.createElement('script');
        s2.src = 'https://cdn.jsdelivr.net/npm/@mlc-ai/web-llm/dist/webllm.min.js';
        document.head.appendChild(s2);
        await new Promise((res,rej)=>{ s2.onload=res; s2.onerror=rej; });
      }catch(e){
        console.warn('web-llm second CDN failed',e);
      }
    }

    if(!window.WebLLM){
      throw new Error('WebLLM runtime failed to load from CDN. This environment may not support dynamic runtime loading. The app will fall back to the local tutor.');
    }

    // instantiate model from URL; how this is done depends on the runtime's API.
    // We'll try a safe / common call pattern used by WebLLM: WebLLM.load({model: modelUrl, ...})
    const modelUrl = STATE.modelUrl;
    // eslint-disable-next-line no-undef
    const runtime = await WebLLM.load({ model: modelUrl, backend: 'webgpu' }).catch(err=>{
      console.warn('error loading model via WebLLM.load',err);
      throw err;
    });

    // Keep runtime in state for future queries
    STATE.webllm = runtime;
    STATE.webllmLoaded = true;
    closeFS();
    alert('Model loaded successfully. WebLLM is ready.');
  }catch(err){
    console.error('Model load failed',err);
    closeFS();
    alert('Could not load WebLLM model: ' + (err.message||String(err)) + '\nFalling back to local AI.');
    STATE.webllm = null;
    STATE.webllmLoaded = false;
  }
}

function unloadWebLLMModel(){
  if(STATE.webllm && STATE.webllm.release){
    try{ STATE.webllm.release(); }catch(e){console.warn('release error',e);}
  }
  STATE.webllm = null; STATE.webllmLoaded = false; STATE.modelUrl = '';
  document.getElementById('modelUrl').value = '';
  alert('Unloaded WebLLM model. Local fallback is active.');
}

/* Query WebLLM (if loaded) with a prompt and return raw text.
   This function MUST be adapted to the specific runtime API used by your model bundle.
   The example below uses a common pattern (runtime.generate) — adjust to the actual WebLLM implementation you use.
*/
async function queryWebLLM(prompt, opts = {maxNewTokens:256, temperature:0.2}){
  if(!STATE.webllmLoaded || !STATE.webllm) throw new Error('WebLLM not loaded');
  // Example pattern — you will likely need to adapt according to the WebLLM runtime you host
  // Some runtimes expose runtime.generate() that returns text/stream. We'll attempt a promise style call:
  try{
    // The API differs between bundles. Try a few common names.
    if(typeof STATE.webllm.generate === 'function'){
      const out = await STATE.webllm.generate(prompt, opts);
      // assume out is string or {text:...}
      if(typeof out === 'string') return out;
      if(out && out.text) return out.text;
      if(out && out[0] && out[0].generated_text) return out[0].generated_text;
      return JSON.stringify(out);
    }
    if(typeof STATE.webllm.run === 'function'){
      const out = await STATE.webllm.run(prompt, opts);
      if(typeof out === 'string') return out;
      return JSON.stringify(out);
    }
    // fallback: if runtime provides .call
    if(typeof STATE.webllm.call === 'function'){
      const out = await STATE.webllm.call(prompt, opts);
      if(typeof out === 'string') return out;
      if(out && out.output) return out.output;
      return JSON.stringify(out);
    }
    throw new Error('No known generate/run API on the loaded WebLLM object. Consult the runtime docs and adapt queryWebLLM.');
  }catch(err){
    throw err;
  }
}

/* ----------------------------
   "Ask WebLLM" flow for AI Tutor
   - if WebLLM loaded => use it
   - else => fallback to local tutor
   ---------------------------- */
async function askWebLLM(){
  const subj = document.getElementById('subjectSelect').value;
  const q = document.getElementById('aiQuestion').value.trim();
  if(!q) return alert('Enter a question');
  const outEl = document.getElementById('aiTutorArea');
  outEl.innerHTML = `<div class="card"><div class="small-muted">Sending prompt to model…</div></div>`;
  // Compose a tutor prompt instructing to return structured steps, hints, example, practice question
  const prompt = `You are a helpful ${subj} tutor. Provide:
1) One-line concise answer.
2) Numbered step-by-step explanation or solution (if applicable).
3) Two hints for the student.
4) One short practice problem with answer (label as 'practice:').
Keep output plain text and clearly separated by headings. Question: ${q}`;
  try{
    if(STATE.webllmLoaded && STATE.webllm){
      const text = await queryWebLLM(prompt, { maxNewTokens: 512, temperature: 0.15 });
      outEl.innerHTML = `<div class="card"><pre style="white-space:pre-wrap">${escapeHtml(text)}</pre></div>`;
    } else {
      // fallback
      const local = localTutor(subj, q);
      outEl.innerHTML = `<div class="card"><pre style="white-space:pre-wrap">${escapeHtml('(local fallback)\n\n' + local)}</pre></div>`;
    }
  }catch(err){
    console.error('WebLLM query error',err);
    outEl.innerHTML = `<div class="card"><pre style="white-space:pre-wrap">Model error: ${escapeHtml(String(err))}\n\nFalling back to local tutor.</pre></div>`;
    const local = localTutor(subj, q);
    outEl.innerHTML += `<div class="card" style="margin-top:8px"><pre style="white-space:pre-wrap">${escapeHtml(local)}</pre></div>`;
  }
}

/* ----------------------------
   Quiz generation (local + WebLLM)
   ---------------------------- */
function generateQuiz(){
  const prompt = document.getElementById('quizPrompt').value.trim();
  const count = Math.max(1, Math.min(20, Number(document.getElementById('quizCount').value || 5)));
  if(!prompt) return alert('Enter a prompt for quiz');
  // Use WebLLM if present, else local generator
  if(STATE.webllmLoaded){
    (async ()=>{
      openFS(`<div style="display:flex;justify-content:space-between;align-items:center"><strong>Generating quiz</strong><button class="btn" onclick="closeFS()">Cancel</button></div><div style="margin-top:12px">Please wait, model is generating ${count} MCQs…</div>`);
      try{
        const req = `Create ${count} multiple-choice questions about "${prompt}". Return JSON only like {"questions":[{"q":"...","choices":["A","B","C","D"],"answer":0},...]} `;
        const text = await queryWebLLM(req, { maxNewTokens: 600, temperature:0.25 });
        // try to extract JSON
        const first = text.indexOf('{'), last = text.lastIndexOf('}');
        let json;
        if(first!==-1 && last!==-1) {
          json = JSON.parse(text.substring(first,last+1));
        } else {
          json = JSON.parse(text);
        }
        renderQuiz(json.questions);
      }catch(e){
        console.warn('Quiz generation failed, using local generator',e);
        const qlocal = localQuiz(prompt, count);
        renderQuiz(qlocal);
      } finally { closeFS(); }
    })();
  } else {
    const qlocal = localQuiz(prompt, count);
    renderQuiz(qlocal);
  }
}

function renderQuiz(questions){
  const out = document.getElementById('quizArea');
  out.innerHTML = '';
  questions.forEach((q,qi)=>{
    const card = document.createElement('div'); card.className='card';
    let inner = `<div style="font-weight:700">Q${qi+1}: ${escapeHtml(q.q)}</div>`;
    q.choices.forEach((c,ci)=>{
      inner += `<div style="margin-top:6px"><label><input type="radio" name="q${qi}" value="${ci}"> ${escapeHtml(c)}</label></div>`;
    });
    inner += `</div>`;
    card.innerHTML = inner;
    out.appendChild(card);
  });
  out.innerHTML += `<div style="margin-top:8px"><button class="btn" onclick="gradeQuiz(${questions.length})">Submit</button></div>`;
  window._currentQuiz = questions;
}

function gradeQuiz(total){
  const qs = window._currentQuiz || [];
  let score = 0;
  for(let i=0;i<qs.length;i++){
    const sel = document.querySelector(`input[name="q${i}"]:checked`);
    if(sel && Number(sel.value) === Number(qs[i].answer)) score++;
  }
  openFS(`<div style="display:flex;justify-content:space-between;align-items:center"><strong>Quiz result</strong><button class="btn" onclick="closeFS()">Close</button></div><div style="margin-top:12px">Score: <span class="kv">${score}</span> / ${qs.length}</div>`);
}

/* Local quiz generator (fallback) */
function localQuiz(prompt, count){
  const words = prompt.split(/\s+/).slice(0,3).join(' ');
  const out = [];
  for(let i=1;i<=count;i++){
    const correct = `Correct about ${words} ${i}`;
    const choices = shuffle([correct, `Wrong A ${i}`, `Wrong B ${i}`, `Wrong C ${i}`]);
    out.push({ q:`Which statement is true about ${prompt}? (item ${i})`, choices, answer: choices.indexOf(correct) });
  }
  return out;
}

/* ----------------------------
   Flashcards generator (local + WebLLM)
   ---------------------------- */
function createFlashcards(){
  const prompt = document.getElementById('flashPrompt').value.trim();
  const count = Math.max(1, Math.min(50, Number(document.getElementById('flashCount').value || 8)));
  if(!prompt) return alert('Enter a topic');
  // use webllm if available
  if(STATE.webllmLoaded){
    (async ()=>{
      openFS(`<div style="display:flex;justify-content:space-between;align-items:center"><strong>Generating flashcards</strong><button class="btn" onclick="closeFS()">Cancel</button></div><div style="margin-top:12px">Generating ${count} flashcards…</div>`);
      try{
        const full = `Generate ${count} concise flashcards for "${prompt}" as JSON like {"cards":[{"term":"...","def":"..."}]}`;
        const text = await queryWebLLM(full, { maxNewTokens: 400, temperature:0.2 });
        const first = text.indexOf('{'), last = text.lastIndexOf('}');
        let json;
        if(first!==-1 && last!==-1) json = JSON.parse(text.substring(first,last+1));
        else json = JSON.parse(text);
        saveAndRenderFlashcards(json.cards, prompt);
      }catch(e){
        console.warn('HF flash parse failed',e);
        const cards = localFlashcards(prompt,count);
        saveAndRenderFlashcards(cards, prompt);
      } finally { closeFS(); }
    })();
  } else {
    const cards = localFlashcards(prompt,count);
    saveAndRenderFlashcards(cards,prompt);
  }
}

function saveAndRenderFlashcards(cards, prompt){
  const key = `flash:${prompt}`;
  localStorage.setItem(key, JSON.stringify(cards));
  renderFlashPreview(cards, prompt);
}

function renderFlashPreview(cards,prompt){
  const out = document.getElementById('flashArea'); out.innerHTML = `<h4>Flashcards — ${escapeHtml(prompt)}</h4>`;
  cards.forEach((c,idx)=>{
    const d = document.createElement('div'); d.className='card';
    d.innerHTML = `<div style="font-weight:700">${escapeHtml(c.term)}</div><div class="small-muted" style="margin-top:6px">${escapeHtml(c.def)}</div>
      <div style="margin-top:8px"><button class="btn" onclick="startFlashStudy('${encodeURIComponent(prompt)}',${idx})">Study</button></div>`;
    out.appendChild(d);
  });
}

/* Local flashcards generator (fallback) */
function localFlashcards(topic,n){
  const base = topic.split(/\s+/).slice(0,3).join(' ');
  const cards = [];
  for(let i=1;i<=n;i++){
    cards.push({ term:`${base} — Key ${i}`, def:`Short explanation of ${base} key ${i}.` });
  }
  return cards;
}

/* Flash study UI */
function startFlashStudy(topicEnc,startIndex=0){
  const topic = decodeURIComponent(topicEnc);
  const key = `flash:${topic}`;
  const raw = localStorage.getItem(key);
  if(!raw) return alert('No flashcards for this topic (save first).');
  const cards = JSON.parse(raw);
  let i = startIndex;
  function showCard(){
    const c = cards[i];
    openFS(`<div style="display:flex;justify-content:space-between;align-items:center"><strong>${escapeHtml(c.term)}</strong><button class="btn" onclick="closeFS()">Close</button></div>
      <div style="margin-top:12px">${escapeHtml(c.def)}</div>
      <div style="display:flex;gap:8px;margin-top:12px"><button class="btn" onclick="markFlash('${encodeURIComponent(topic)}',${i},true)">I knew this</button><button class="btn" onclick="markFlash('${encodeURIComponent(topic)}',${i},false)" style="background:#666;color:#fff">I didn't</button><button class="btn" onclick="nextCard()">Next</button></div>`);
  }
  function nextCard(){ i = (i+1) % cards.length; showCard(); }
  showCard();
}

function markFlash(topicEnc,index,known){
  const topic = decodeURIComponent(topicEnc);
  const key = `flash:${topic}`;
  const cards = JSON.parse(localStorage.getItem(key));
  if(!cards) return;
  if(known) cards[index].interval = (cards[index].interval||1)*2;
  else cards[index].interval = 1;
  cards[index].due = Date.now() + (cards[index].interval||1)*24*3600*1000;
  localStorage.setItem(key, JSON.stringify(cards));
  closeFS();
  alert('Saved progress');
}

/* ----------------------------
   Utilities
   ---------------------------- */
function shuffle(a){ for(let i=a.length-1;i>0;i--){ const j=Math.floor(Math.random()*(i+1)); [a[i],a[j]]=[a[j],a[i]] } return a; }

/* ----------------------------
   Init on load
   ---------------------------- */
document.addEventListener('DOMContentLoaded', ()=>{
  renderGames();
  renderBMPanel();
  makeDraggable(document.getElementById('bmPanel'));
  makeDraggable(document.getElementById('cloaker'));
  // panels hidden initially
  document.getElementById('bmPanel').style.display='none';
  document.getElementById('cloaker').style.display='block';
});

/* ----------------------------
   Study Mode quick flow starter
   ---------------------------- */
function startStudyMode(){
  const subj = document.getElementById('subjectSelect').value;
  const q = document.getElementById('aiQuestion').value.trim();
  if(!q) return alert('Enter a question to start Study Mode');
  // Launch a study flow: AI Tutor -> Quiz -> Flashcards (best effort)
  askWebLLM(); // will put an answer in aiTutorArea (either WebLLM or local)
  // Show buttons to run quiz/flashcards for same query
  openFS(`<div style="display:flex;justify-content:space-between;align-items:center"><strong>Study Mode</strong><button class="btn" onclick="closeFS()">Close</button></div>
    <div style="margin-top:12px">
      <div style="margin-bottom:8px">1) Tutor answer shown on the Study page (close this modal)</div>
      <div style="margin-bottom:8px"><button class="btn" onclick="document.getElementById('quizPrompt').value='${escapeAttr(q)}';generateQuiz()">Generate Quiz</button></div>
      <div style="margin-bottom:8px"><button class="btn" onclick="document.getElementById('flashPrompt').value='${escapeAttr(q)}';createFlashcards()">Create Flashcards</button></div>
    </div>`);
}

</script>
</body>
</html>
