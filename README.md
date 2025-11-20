<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>StudyHub — Interactive (OpenAI / Gemini)</title>
<style>
  :root{
    --bg:#1a0b3a; /* purple-ish background */
    --card:#2a0f57;
    --button:#2563eb; /* blue buttons */
    --muted:#d1d5db;
    --white:#ffffff;
  }
  *{box-sizing:border-box}
  body{margin:0;font-family:Inter,system-ui,Arial,sans-serif;background:var(--bg);color:var(--white);min-height:100vh;display:flex;flex-direction:column}
  header{display:flex;align-items:center;justify-content:space-between;padding:14px 18px;background:linear-gradient(90deg,#240b3f 0%,#2d0f56 100%);border-bottom:1px solid rgba(255,255,255,0.04)}
  header h1{margin:0;font-size:18px}
  nav{display:flex;gap:8px;flex-wrap:wrap}
  button{background:var(--button);color:var(--white);border:none;padding:8px 10px;border-radius:8px;cursor:pointer;font-weight:600}
  button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.08)}
  main{padding:18px;flex:1;display:grid;grid-template-columns:320px 1fr;gap:18px}
  .sidebar{background:var(--card);padding:12px;border-radius:10px;min-height:300px}
  .panel{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); padding:14px;border-radius:10px;border:1px solid rgba(255,255,255,0.04);}
  .hidden{display:none}
  label{display:block;font-size:13px;color:var(--muted);margin-bottom:6px}
  input[type=text],input[type=password],select,textarea{width:100%;padding:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:#120522;color:var(--white);margin-bottom:8px}
  textarea{min-height:90px;resize:vertical}
  .tool-list{display:flex;flex-direction:column;gap:8px}
  .tool-list button{justify-content:flex-start}
  .area{padding:12px;border-radius:10px;background:var(--card);border:1px solid rgba(255,255,255,0.04);min-height:120px}
  .flashgrid{display:flex;flex-wrap:wrap;gap:10px}
  .card{background:#fff;color:#000;padding:12px;border-radius:8px;min-width:140px;min-height:90px;display:flex;align-items:center;justify-content:center;cursor:pointer;user-select:none}
  .mc-option{display:block;margin:6px 0;padding:10px;border-radius:8px;background:rgba(255,255,255,0.04);cursor:pointer}
  .mc-option.correct{background:rgba(16,185,129,0.2);border:1px solid rgba(16,185,129,0.4)}
  .mc-option.wrong{background:rgba(239,68,68,0.15);border:1px solid rgba(239,68,68,0.35)}
  .small{font-size:13px;color:var(--muted)}
  footer{padding:12px;text-align:center;color:var(--muted);font-size:13px;background:transparent}
  .row{display:flex;gap:8px;align-items:center}
  .stack{display:flex;flex-direction:column;gap:8px}
</style>
</head>
<body>

<header>
  <div style="display:flex;gap:12px;align-items:center">
    <h1>StudyHub — Interactive</h1>
    <div class="small">Flashcards • Quizzes • AI Tutor • Music • Timer • Mini Browser</div>
  </div>
  <nav>
    <button id="btn-settings" class="ghost">Settings</button>
    <button id="btn-help" class="ghost">Help</button>
  </nav>
</header>

<main>
  <!-- Left sidebar -->
  <aside class="sidebar panel">
    <h3 style="margin:0 0 8px 0">Tools</h3>
    <div class="tool-list">
      <button data-tool="flashcards">Flashcard Maker</button>
      <button data-tool="quizmaker">Quiz Maker</button>
      <button data-tool="studytutor">AI Tutor</button>
      <button data-tool="quizstudy">Quiz Study (Subjects)</button>
      <button data-tool="music">Music Player</button>
      <button data-tool="timer">Timer</button>
      <button data-tool="minibrowser">Mini Browser</button>
      <button data-tool="resources">Study Resources</button>
    </div>

    <hr style="border:none;margin:12px 0;border-top:1px solid rgba(255,255,255,0.03)">

    <div style="margin-top:8px">
      <div class="small">Owner / Admin</div>
      <div class="row" style="margin-top:8px">
        <button id="open-admin" class="ghost" style="flex:1">Open Admin</button>
      </div>
      <div style="margin-top:12px" class="small">Notes</div>
      <div class="small" style="margin-top:6px;color:var(--muted)">
        Use Settings → API Keys (owner123) to paste your keys, or edit the file locally and set the constants.
      </div>
    </div>
  </aside>

  <!-- Right main area -->
  <section style="display:flex;flex-direction:column;gap:12px">
    <!-- Settings / Admin modal area (hidden panel) -->
    <div id="settings-panel" class="panel hidden">
      <h3>Settings & API Keys (owner only)</h3>
      <div class="stack">
        <label>Enter owner password to unlock:</label>
        <input id="owner-pass" type="password" placeholder="owner password (owner123)">
        <div class="row">
          <button id="unlock-owner">Unlock Settings</button>
          <button id="close-settings" class="ghost">Close</button>
        </div>
        <div id="owner-controls" class="hidden stack">
          <label>Default provider (used to preselect provider dropdowns):</label>
          <select id="default-provider"><option value="openai">OpenAI</option><option value="gemini">Gemini</option></select>

          <label>OpenAI API key (paste here for global use)</label>
          <input id="openai-key" type="text" placeholder="Paste OpenAI key here (will be stored in localStorage)">

          <label>Gemini API key (paste here for global use)</label>
          <input id="gemini-key" type="text" placeholder="Paste Gemini key here (will be stored in localStorage)">

          <div class="row">
            <button id="save-keys">Save Keys</button>
            <button id="clear-keys" class="ghost">Clear Stored Keys</button>
          </div>

          <div class="small" style="color:var(--muted)">
            Warning: storing keys in localStorage exposes them to anyone who can access the machine/browser. For production, use a backend proxy.
          </div>
        </div>
      </div>
    </div>

    <!-- Tool content panels -->
    <div id="tool-area" class="panel">
      <!-- Flashcards -->
      <div id="flashcards-panel" class="">
        <h3>Flashcard Maker</h3>
        <div class="row" style="gap:10px;align-items:flex-start">
          <div style="flex:1">
            <label>Provider</label>
            <select id="fc-provider"><option value="openai">OpenAI</option><option value="gemini">Gemini</option></select>
            <label>Topic / Prompt</label>
            <textarea id="fc-prompt" placeholder="e.g. Photosynthesis; or 'Create 6 flashcards about the water cycle'"></textarea>
            <div class="row">
              <button id="fc-gen">Generate Flashcards</button>
              <button id="fc-local" class="ghost">Use manual input</button>
            </div>

            <div id="fc-controls" class="row small" style="margin-top:8px">
              <div>Known: <span id="fc-known">0</span></div>
              <div>Unknown: <span id="fc-unknown">0</span></div>
              <div style="margin-left:auto"><button id="fc-reset" class="ghost">Reset Session</button></div>
            </div>
          </div>

          <div style="flex:1">
            <label>Flashcards</label>
            <div id="fc-area" class="area" style="min-height:220px">
              <div class="small">No flashcards yet. Generate to begin.</div>
            </div>
          </div>
        </div>
      </div>

      <!-- Quiz Maker -->
      <div id="quizmaker-panel" class="hidden">
        <h3>Quiz Maker — AI-generated multiple choice</h3>
        <label>Provider</label>
        <select id="qm-provider"><option value="openai">OpenAI</option><option value="gemini">Gemini</option></select>
        <label>Topic / Prompt</label>
        <textarea id="qm-prompt" placeholder="e.g. 'American Revolution: generate 5 multiple-choice questions (4 options each) as JSON'"></textarea>
        <div class="row">
          <button id="qm-gen">Generate Quiz</button>
          <button id="qm-import" class="ghost">Import JSON</button>
        </div>

        <div style="margin-top:12px">
          <div id="qm-area" class="area">
            <div class="small">Generated quiz will appear here.</div>
          </div>
          <div style="margin-top:8px" id="qm-score" class="small"></div>
        </div>
      </div>

      <!-- AI Tutor -->
      <div id="tutor-panel" class="hidden">
        <h3>AI Tutor</h3>
        <label>Provider</label>
        <select id="tutor-provider"><option value="openai">OpenAI</option><option value="gemini">Gemini</option></select>
        <label>Question / Topic</label>
        <textarea id="tutor-prompt" placeholder="Ask your question — e.g. 'Explain the greenhouse effect for 7th graders'"></textarea>
        <div class="row">
          <button id="tutor-ask">Ask Tutor</button>
          <button id="tutor-short" class="ghost">Short Answer</button>
          <button id="tutor-step" class="ghost">Step-by-step</button>
        </div>
        <div id="tutor-output" class="area" style="margin-top:12px">AI answers will appear here.</div>
      </div>

      <!-- Quiz Study (Subjects) -->
      <div id="quizstudy-panel" class="hidden">
        <h3>Quiz Study — Pick a subject</h3>
        <div class="row">
          <button class="subject-btn" data-subject="Math">Math</button>
          <button class="subject-btn" data-subject="Science">Science</button>
          <button class="subject-btn" data-subject="History">History</button>
          <button class="subject-btn" data-subject="ELA">ELA</button>
        </div>
        <div style="margin-top:12px">
          <label>Use AI to generate questions?</label>
          <select id="qs-provider"><option value="none">Built-in (faster)</option><option value="openai">OpenAI</option><option value="gemini">Gemini</option></select>
          <div id="qs-area" class="area" style="margin-top:12px">Choose a subject to begin.</div>
        </div>
      </div>

      <!-- Music -->
      <div id="music-panel" class="hidden">
        <h3>Music Player</h3>
        <label>Upload audio file (plays in-page)</label>
        <input id="music-file" type="file" accept="audio/*">
        <div class="row" style="margin-top:8px">
          <button id="music-play">Play Uploaded</button>
          <button id="music-pause" class="ghost">Pause</button>
        </div>
        <label style="margin-top:8px">Or use an audio URL (YouTube links will open in new tab)</label>
        <input id="music-url" type="text" placeholder="https://... or YouTube link">
        <div class="row" style="margin-top:8px">
          <button id="music-play-url">Play URL</button>
        </div>
        <audio id="music-player" controls style="width:100%;margin-top:10px"></audio>
      </div>

      <!-- Timer -->
      <div id="timer-panel" class="hidden">
        <h3>Timer</h3>
        <label>Minutes</label>
        <input id="timer-min" type="number" min="1" placeholder="25">
        <div class="row" style="margin-top:8px">
          <button id="timer-start">Start</button>
          <button id="timer-stop" class="ghost">Stop</button>
        </div>
        <div id="timer-display" class="area small" style="margin-top:12px">00:00</div>
      </div>

      <!-- Mini browser -->
      <div id="browser-panel" class="hidden">
        <h3>Mini Browser</h3>
        <input id="browser-url" placeholder="https://example.com">
        <div class="row" style="margin-top:8px">
          <button id="browser-go">Go</button>
          <button id="browser-open" class="ghost">Open in new tab</button>
        </div>
        <div style="margin-top:12px" class="area">
          <iframe id="browser-frame" style="width:100%;height:420px;border:none;"></iframe>
        </div>
      </div>

      <!-- Resources -->
      <div id="resources-panel" class="hidden">
        <h3>Study Resources</h3>
        <div class="stack small">
          <div><b>Math</b> — Khan Academy, Desmos, IXL</div>
          <div><b>Science</b> — PhET, NASA, CrashCourse</div>
          <div><b>History</b> — History.com, Mission US, CrashCourse</div>
          <div style="margin-top:8px" class="small">Click a subject top-left to open quizzes/flashcards for it quickly.</div>
        </div>
      </div>

    </div>
  </section>
</main>

<footer>
  <div class="small">StudyHub — Local interactive build • Owner: password required for settings</div>
</footer>

<script>
/* =========================
   Initialization & helpers
   ========================= */

// Default keys placeholders (do NOT paste keys here in chat)
const OPENAI_KEY_PLACEHOLDER = ""; // you may paste in Settings panel or localStorage
const GEMINI_KEY_PLACEHOLDER = ""; // you may paste in Settings panel or localStorage

// Setup from localStorage if present
function loadSavedKeys(){
  const o = localStorage.getItem('studyhub_openai');
  const g = localStorage.getItem('studyhub_gemini');
  const def = localStorage.getItem('studyhub_default_provider') || 'openai';
  if(o) document.getElementById('openai-key').value = o;
  else if(OPENAI_KEY_PLACEHOLDER) document.getElementById('openai-key').value = OPENAI_KEY_PLACEHOLDER;
  if(g) document.getElementById('gemini-key').value = g;
  else if(GEMINI_KEY_PLACEHOLDER) document.getElementById('gemini-key').value = GEMINI_KEY_PLACEHOLDER;
  document.getElementById('default-provider').value = def;
}
loadSavedKeys();

// UI wiring
document.querySelectorAll('[data-tool]').forEach(b=>{
  b.addEventListener('click', ()=> openTool(b.getAttribute('data-tool')));
});
document.getElementById('open-admin').addEventListener('click', ()=> showSettings());
document.getElementById('btn-settings').addEventListener('click', ()=> showSettings());
document.getElementById('btn-help').addEventListener('click', ()=> alert('Tools: Flashcards, Quiz Maker, AI Tutor, Quiz Study, Music, Timer, Mini Browser. Owner settings require password (owner123).'));

function hideAllPanels(){
  document.getElementById('settings-panel').classList.add('hidden');
  document.getElementById('flashcards-panel').classList.add('hidden');
  document.getElementById('quizmaker-panel').classList.add('hidden');
  document.getElementById('tutor-panel').classList.add('hidden');
  document.getElementById('quizstudy-panel').classList.add('hidden');
  document.getElementById('music-panel').classList.add('hidden');
  document.getElementById('timer-panel').classList.add('hidden');
  document.getElementById('browser-panel').classList.add('hidden');
  document.getElementById('resources-panel').classList.add('hidden');
}

function openTool(key){
  hideAllPanels();
  // show main tool area always
  document.getElementById('tool-area').classList.remove('hidden');
  switch(key){
    case 'flashcards': document.getElementById('flashcards-panel').classList.remove('hidden'); break;
    case 'quizmaker': document.getElementById('quizmaker-panel').classList.remove('hidden'); break;
    case 'studytutor': document.getElementById('tutor-panel').classList.remove('hidden'); break;
    case 'quizstudy': document.getElementById('quizstudy-panel').classList.remove('hidden'); break;
    case 'music': document.getElementById('music-panel').classList.remove('hidden'); break;
    case 'timer': document.getElementById('timer-panel').classList.remove('hidden'); break;
    case 'minibrowser': document.getElementById('browser-panel').classList.remove('hidden'); break;
    case 'resources': document.getElementById('resources-panel').classList.remove('hidden'); break;
  }
  // hide settings panel whenever a tool opens
  document.getElementById('settings-panel').classList.add('hidden');
}

function showSettings(){
  hideAllPanels();
  document.getElementById('settings-panel').classList.remove('hidden');
  // prefill keys from localStorage
  loadSavedKeys();
}
document.getElementById('close-settings').addEventListener('click', ()=> document.getElementById('settings-panel').classList.add('hidden'));

// Owner unlock
document.getElementById('unlock-owner').addEventListener('click', ()=>{
  const pass = document.getElementById('owner-pass').value;
  if(pass === 'owner123'){
    document.getElementById('owner-controls').classList.remove('hidden');
    alert('Owner unlocked. Paste keys, then click Save Keys.');
  } else {
    alert('Incorrect owner password.');
  }
});

document.getElementById('save-keys').addEventListener('click', ()=>{
  const o = document.getElementById('openai-key').value.trim();
  const g = document.getElementById('gemini-key').value.trim();
  const def = document.getElementById('default-provider').value;
  if(o) localStorage.setItem('studyhub_openai', o); else localStorage.removeItem('studyhub_openai');
  if(g) localStorage.setItem('studyhub_gemini', g); else localStorage.removeItem('studyhub_gemini');
  localStorage.setItem('studyhub_default_provider', def);
  alert('Keys saved locally (in this browser).');
});
document.getElementById('clear-keys').addEventListener('click', ()=>{
  localStorage.removeItem('studyhub_openai');
  localStorage.removeItem('studyhub_gemini');
  localStorage.removeItem('studyhub_default_provider');
  document.getElementById('openai-key').value='';
  document.getElementById('gemini-key').value='';
  alert('Stored keys cleared.');
});

/* =========================
   API wrapper functions
   ========================= */

// returns the selected provider key from localStorage or placeholders
function getOpenAIKey(){ return localStorage.getItem('studyhub_openai') || OPENAI_KEY_PLACEHOLDER; }
function getGeminiKey(){ return localStorage.getItem('studyhub_gemini') || GEMINI_KEY_PLACEHOLDER; }

// OpenAI Chat Completions (gpt-3.5-turbo / gpt-4)
async function callOpenAIChat(prompt, opts = {}) {
  const key = getOpenAIKey();
  if(!key) throw new Error('OpenAI key not set in Settings.');
  const model = opts.model || 'gpt-3.5-turbo';
  const body = {
    model,
    messages: [{ role: 'user', content: prompt }],
    temperature: opts.temperature ?? 0.7,
    max_tokens: opts.max_tokens ?? 800
  };
  const resp = await fetch('https://api.openai.com/v1/chat/completions', {
    method:'POST',
    headers: { 'Content-Type':'application/json', 'Authorization':'Bearer '+key },
    body: JSON.stringify(body)
  });
  if(!resp.ok){
    const txt = await resp.text();
    throw new Error('OpenAI error: '+resp.status+' '+txt);
  }
  const data = await resp.json();
  return data.choices?.[0]?.message?.content ?? JSON.stringify(data);
}

// Gemini / Google Generative Language (example - adjust if your key or endpoint differs)
// Note: Google may require using API key in URL; here we attempt Bearer if provided in key field.
// If your key is an API key, change the endpoint and header usage accordingly.
async function callGemini(prompt, opts = {}) {
  const key = getGeminiKey();
  if(!key) throw new Error('Gemini key not set in Settings.');
  // SAMPLE endpoint — you may need to change to your account's endpoint or include key as query param.
  const url = 'https://generativelanguage.googleapis.com/v1beta2/models/text-bison-001:generate';
  const body = {
    prompt: {
      text: prompt
    },
    temperature: opts.temperature ?? 0.7,
    maxOutputTokens: opts.max_tokens ?? 512
  };
  const resp = await fetch(url + '?key=' + encodeURIComponent(key), {
    method:'POST',
    headers: { 'Content-Type':'application/json' },
    body: JSON.stringify(body)
  });
  if(!resp.ok){
    const txt = await resp.text();
    throw new Error('Gemini error: '+resp.status+' '+txt);
  }
  const data = await resp.json();
  // adapt to response shape
  if(Array.isArray(data?.candidates) && data.candidates[0]?.output) return data.candidates[0].output;
  if(data?.candidates && data.candidates[0]?.content) return data.candidates[0].content;
  return JSON.stringify(data);
}

/* =========================
   Flashcards logic
   ========================= */

let currentFlashcards = []; // array of {q,a,known:false}
function renderFlashcards(){
  const area = document.getElementById('fc-area');
  area.innerHTML = '';
  if(!currentFlashcards.length){ area.innerHTML = '<div class="small">No flashcards — generate or enter some.</div>'; return; }
  const grid = document.createElement('div');
  grid.className = 'flashgrid';
  currentFlashcards.forEach((card, idx)=>{
    const el = document.createElement('div');
    el.className = 'card';
    el.dataset.index = idx;
    el.innerText = card.showAnswer ? card.a : card.q;
    el.onclick = ()=>{
      card.showAnswer = !card.showAnswer;
      renderFlashcards();
    };
    grid.appendChild(el);
  });
  area.appendChild(grid);
  updateFlashcardCounters();
}

function updateFlashcardCounters(){
  const known = currentFlashcards.filter(c=>c.known).length;
  const unknown = currentFlashcards.length - known;
  document.getElementById('fc-known').innerText = known;
  document.getElementById('fc-unknown').innerText = unknown;
}

document.getElementById('fc-reset').addEventListener('click', ()=>{
  currentFlashcards.forEach(c=>{ c.known=false; c.showAnswer=false; });
  renderFlashcards();
});

// manual local input toggle
document.getElementById('fc-local').addEventListener('click', ()=>{
  const raw = prompt('Enter flashcards one per line, format: Question:Answer');
  if(!raw) return;
  const lines = raw.split('\n').map(l=>l.trim()).filter(Boolean);
  currentFlashcards = lines.map(l=>{
    const [q,a] = l.split(':').map(s=>s? s.trim() : '');
    return { q: q||l, a: a||'—', known:false, showAnswer:false };
  });
  renderFlashcards();
});

// generate via AI
document.getElementById('fc-gen').addEventListener('click', async ()=>{
  const provider = document.getElementById('fc-provider').value;
  const promptText = document.getElementById('fc-prompt').value.trim();
  if(!promptText){ alert('Enter a topic or prompt.'); return; }
  const uiArea = document.getElementById('fc-area');
  uiArea.innerHTML = '<div class="small">Generating flashcards...</div>';
  try{
    const instruction = `Generate 6 simple flashcards for the topic: "${promptText}". Output as JSON array of objects in the exact format: [{"q":"Question text","a":"Answer text"}, ...] with no extra commentary. Keep Q & A concise.`;
    let raw;
    if(provider === 'openai') raw = await callOpenAIChat(instruction, {max_tokens:400});
    else raw = await callGemini(instruction, {max_tokens:400});
    // try parse JSON
    let parsed;
    try{ parsed = JSON.parse(raw); }
    catch(e){
      // attempt to extract JSON substring
      const start = raw.indexOf('[');
      const end = raw.lastIndexOf(']');
      if(start!==-1 && end!==-1) parsed = JSON.parse(raw.slice(start, end+1));
      else throw new Error('AI did not return valid JSON. Raw: '+raw.slice(0,400));
    }
    currentFlashcards = parsed.map(p=>({ q: p.q||p.question||'?', a: p.a||p.answer||'', known:false, showAnswer:false }));
    renderFlashcards();
  }catch(err){
    uiArea.innerHTML = '<div class="small">Error generating: '+err.message+'</div>';
    console.error(err);
  }
});

/* =========================
   Quiz Maker logic (AI-driven MCQ)
   ========================= */

let currentQuiz = { questions: [], score: 0 };

async function generateQuizAI(){
  const provider = document.getElementById('qm-provider').value;
  const promptText = document.getElementById('qm-prompt').value.trim();
  if(!promptText){ alert('Enter quiz topic/prompt.'); return; }
  const container = document.getElementById('qm-area');
  container.innerHTML = '<div class="small">Generating quiz (AI)...</div>';
  try{
    const instruction = `Create 5 multiple-choice questions on the topic: "${promptText}". Output EXACTLY a JSON object like:
{
  "questions": [
    {
      "q":"Question text",
      "options":["A text","B text","C text","D text"],
      "answer":1
    },
    ...
  ]
}
Where "answer" is the 0-based index of the correct option. Return ONLY the JSON.`;
    const raw = (provider === 'openai') ? await callOpenAIChat(instruction, {max_tokens:800}) : await callGemini(instruction, {max_tokens:800});
    // parse JSON
    let parsed;
    try{ parsed = JSON.parse(raw); }
    catch(e){
      const start = raw.indexOf('{');
      const end = raw.lastIndexOf('}');
      if(start!==-1 && end!==-1) parsed = JSON.parse(raw.slice(start, end+1));
      else throw new Error('AI did not return valid JSON.');
    }
    if(!Array.isArray(parsed.questions)) throw new Error('Invalid quiz JSON.');
    currentQuiz.questions = parsed.questions;
    currentQuiz.score = 0;
    renderQuiz();
  }catch(err){
    container.innerHTML = '<div class="small">Error: '+err.message+'</div>';
    console.error(err);
  }
}

function renderQuiz(){
  const container = document.getElementById('qm-area');
  container.innerHTML = '';
  if(!currentQuiz.questions.length){ container.innerHTML = '<div class="small">No quiz loaded.</div>'; return; }
  currentQuiz.questions.forEach((qobj, qi)=>{
    const qdiv = document.createElement('div');
    qdiv.className = 'question';
    const qtext = document.createElement('div');
    qtext.innerHTML = `<b>Q${qi+1}.</b> ${qobj.q}`;
    qdiv.appendChild(qtext);
    qobj.options.forEach((opt, oi)=>{
      const btn = document.createElement('div');
      btn.className = 'mc-option';
      btn.innerText = opt;
      btn.onclick = ()=>{
        // disable other options
        Array.from(qdiv.querySelectorAll('.mc-option')).forEach(b=>b.onclick=null);
        if(oi === qobj.answer){
          btn.classList.add('correct'); currentQuiz.score++;
        } else {
          btn.classList.add('wrong');
          // mark correct one
          const opts = qdiv.querySelectorAll('.mc-option');
          if(opts[qobj.answer]) opts[qobj.answer].classList.add('correct');
        }
        // update score display
        const scoreEl = document.getElementById('qm-score');
        scoreEl.innerText = `Score: ${currentQuiz.score} / ${currentQuiz.questions.length}`;
      };
      qdiv.appendChild(btn);
    });
    container.appendChild(qdiv);
  });
  document.getElementById('qm-score').innerText = `Score: ${currentQuiz.score} / ${currentQuiz.questions.length}`;
}

/* wiring QM button */
document.getElementById('qm-gen').addEventListener('click', generateQuizAI);

/* =========================
   Tutor integration
   ========================= */

document.getElementById('tutor-ask').addEventListener('click', async ()=>{
  const provider = document.getElementById('tutor-provider').value;
  const prompt = document.getElementById('tutor-prompt').value.trim();
  if(!prompt){ alert('Type a question.'); return; }
  const area = document.getElementById('tutor-output');
  area.innerHTML = '<div class="small">Thinking...</div>';
  try{
    const fullPrompt = `You are a helpful tutor. Answer this: ${prompt}`;
    const raw = (provider==='openai') ? await callOpenAIChat(fullPrompt, {max_tokens:600}) : await callGemini(fullPrompt, {max_tokens:600});
    area.innerHTML = `<div style="white-space:pre-wrap">${raw}</div>`;
  }catch(err){
    area.innerHTML = '<div class="small">Error: '+err.message+'</div>';
  }
});

/* =========================
   Quiz Study (subject) logic
   ========================= */

const builtInStudy = {
  Math: [
    { q: '2 + 2 = ?', options:['1','2','3','4'], answer:3 },
    { q: '5 × 3 = ?', options:['8','15','10','20'], answer:1 }
  ],
  Science: [
    { q: 'Water chemical formula?', options:['H2O','CO2','O2','N2'], answer:0 }
  ],
  History: [
    { q: 'Year US Declaration signed?', options:['1776','1492','1812','1607'], answer:0 }
  ],
  ELA: [
    { q: 'A synonym for "quick"?', options:['slow','rapid','late','tardy'], answer:1 }
  ]
};

document.querySelectorAll('.subject-btn').forEach(b=>{
  b.addEventListener('click', async (ev)=>{
    const subject = b.dataset.subject || b.innerText;
    const useProvider = document.getElementById('qs-provider').value;
    const area = document.getElementById('qs-area') || document.getElementById('qs-area');
    area.innerHTML = '<div class="small">Preparing questions...</div>';
    if(useProvider === 'none'){
      // use built-in
      const qs = builtInStudy[subject] || [];
      renderStudyQuestions(qs, area);
      return;
    }
    // else use AI
    try{
      const instruction = `Create 5 multiple-choice questions (4 options each) for ${subject} at middle-school level. Output EXACTLY JSON: {"questions":[{"q":"...","options":["..."],"answer":0}, ...]}`;
      let raw = (useProvider === 'openai') ? await callOpenAIChat(instruction, {max_tokens:800}) : await callGemini(instruction, {max_tokens:800});
      let parsed;
      try{ parsed = JSON.parse(raw); }
      catch(e){
        const start = raw.indexOf('{'), end = raw.lastIndexOf('}');
        if(start!==-1 && end!==-1) parsed = JSON.parse(raw.slice(start,end+1));
        else throw new Error('AI did not return valid JSON.');
      }
      if(!Array.isArray(parsed.questions)) throw new Error('Invalid JSON format.');
      renderStudyQuestions(parsed.questions, area);
    }catch(err){
      area.innerHTML = '<div class="small">Error generating questions: '+err.message+'</div>';
    }
  });
});

function renderStudyQuestions(questions, areaEl){
  areaEl.innerHTML = '';
  questions.forEach((qobj, idx)=>{
    const qdiv = document.createElement('div');
    qdiv.className = 'question';
    qdiv.innerHTML = `<b>Q${idx+1}.</b> ${qobj.q}`;
    qobj.options.forEach((opt, oi)=>{
      const op = document.createElement('div');
      op.className = 'mc-option';
      op.innerText = opt;
      op.onclick = ()=>{
        Array.from(qdiv.querySelectorAll('.mc-option')).forEach(x=>x.onclick=null);
        if(oi === qobj.answer) op.classList.add('correct');
        else {
          op.classList.add('wrong');
          const opts = qdiv.querySelectorAll('.mc-option');
          if(opts[qobj.answer]) opts[qobj.answer].classList.add('correct');
        }
      };
      qdiv.appendChild(op);
    });
    areaEl.appendChild(qdiv);
  });
}

/* =========================
   Music player wiring
   ========================= */

document.getElementById('music-play').addEventListener('click', ()=>{
  const file = document.getElementById('music-file').files[0];
  const player = document.getElementById('music-player');
  if(!file){ alert('Select an audio file first.'); return; }
  player.src = URL.createObjectURL(file);
  player.play();
});
document.getElementById('music-pause').addEventListener('click', ()=> document.getElementById('music-player').pause());
document.getElementById('music-play-url').addEventListener('click', ()=>{
  const url = document.getElementById('music-url').value.trim();
  if(!url){ alert('Enter an audio URL'); return; }
  // if youtube, open in new tab (embedding often blocked)
  if(url.includes('youtube.com') || url.includes('youtu.be')){ window.open(url,'_blank'); return; }
  const player = document.getElementById('music-player'); player.src = url; player.play();
});

/* =========================
   Timer wiring
   ========================= */

let timerHandle = null;
document.getElementById('timer-start').addEventListener('click', ()=>{
  const minutes = parseInt(document.getElementById('timer-min').value || '0',10);
  if(!minutes || minutes <= 0){ alert('Enter minutes'); return; }
  clearInterval(timerHandle);
  let seconds = minutes * 60;
  document.getElementById('timer-display').innerText = formatTime(seconds);
  timerHandle = setInterval(()=>{
    seconds--;
    if(seconds < 0){ clearInterval(timerHandle); alert('Time up!'); document.getElementById('timer-display').innerText='00:00'; return; }
    document.getElementById('timer-display').innerText = formatTime(seconds);
  },1000);
});
document.getElementById('timer-stop').addEventListener('click', ()=>{ clearInterval(timerHandle); document.getElementById('timer-display').innerText='00:00'; });
function formatTime(sec){ const m = Math.floor(sec/60); const s = sec%60; return String(m).padStart(2,'0')+':'+String(s).padStart(2,'0'); }

/* =========================
   Mini Browser wiring
   ========================= */

document.getElementById('browser-go').addEventListener('click', ()=>{
  const url = document.getElementById('browser-url').value.trim();
  if(!url){ alert('Enter a URL.'); return; }
  if(!/^https?:\/\//i.test(url)) { alert('Include http:// or https://'); return; }
  try{
    document.getElementById('browser-frame').src = url;
  }catch(e){
    window.open(url,'_blank');
  }
});
document.getElementById('browser-open').addEventListener('click', ()=>{
  const url = document.getElementById('browser-url').value.trim();
  if(!/^https?:\/\//i.test(url)) { alert('Include http:// or https://'); return; }
  window.open(url,'_blank');
});

/* =========================
   Quiz Maker import / JSON paste support
   ========================= */

document.getElementById('qm-import').addEventListener('click', ()=>{
  const raw = prompt('Paste quiz JSON here (format: { "questions":[{ "q":"", "options":[...], "answer":0 }, ...] })');
  if(!raw) return;
  try{
    const parsed = JSON.parse(raw);
    if(!Array.isArray(parsed.questions)) throw new Error('Invalid format');
    currentQuiz.questions = parsed.questions;
    currentQuiz.score = 0;
    renderQuiz();
  }catch(err){
    alert('Invalid JSON: '+err.message);
  }
});

/* =========================
   Init: set provider defaults
   ========================= */

(function init(){
  const def = localStorage.getItem('studyhub_default_provider') || 'openai';
  document.querySelectorAll('select').forEach(s=>{
    if(s.id.includes('provider') || s.id === 'default-provider') s.value = def;
  });
})();
</script>

</body>
</html>
