<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Study Hub — Quiz • Flashcards • AI Tutor</title>

<!-- Chart.js for charts -->
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>

<style>
  :root{
    --nav-bg:#3b6fb5;
    --nav-text:#ffffff;
    --card-bg:#ffffff;
    --muted:#6b7280;
    --accent:#57b96a;
    --danger:#f56565;
    --soft:#eef2f7;
    --container-max:1100px;
  }
  html,body{height:100%;margin:0;font-family:Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;}
  body{background:linear-gradient(180deg,var(--soft),#f7fafc); color:#111827; -webkit-font-smoothing:antialiased;}
  .wrap{max-width:var(--container-max);margin:18px auto;padding:0 14px;}
  header.topbar{background:var(--nav-bg);color:var(--nav-text);border-radius:8px;padding:10px 14px;display:flex;align-items:center;gap:12px;box-shadow:0 4px 14px rgba(12,26,50,0.08);}
  header .brand{font-weight:700;font-size:18px;margin-right:8px;display:flex;align-items:center;gap:12px}
  header .brand img{height:36px;border-radius:6px;object-fit:cover}
  header nav{display:flex;gap:12px;align-items:center;flex:1}
  header nav button, header nav .api-key-display{background:transparent;border:none;color:var(--nav-text);font-size:15px;padding:6px 8px;border-radius:6px;cursor:pointer}
  header nav button:hover{background:rgba(255,255,255,0.06)}
  header .api-key-display{background:#f3f6fb;color:#123; padding:6px 10px;border-radius:6px; font-family:monospace; font-size:13px; color:#0b2f5a}
  .grid{display:grid;grid-template-columns:1fr 420px;gap:18px;margin-top:18px;}
  .card{background:var(--card-bg);border-radius:10px;padding:18px;box-shadow:0 2px 6px rgba(12,26,50,0.04);border:1px solid rgba(15,23,42,0.04);}
  .card h2{margin:0 0 12px;font-size:22px}
  .muted{color:var(--muted)}
  .quiz-card .question{font-size:18px;margin-bottom:12px}
  .choices{display:flex;flex-direction:column;gap:10px}
  .choice{background:#fbfdff;border-radius:8px;padding:12px 14px;border:1px solid rgba(15,23,42,0.04);cursor:pointer;font-size:16px}
  .choice:hover{background:#f1f7ff}
  .choice.correct{background:var(--accent);color:#fff;border-color:rgba(0,0,0,0.06)}
  .choice.wrong{background:var(--danger);color:#fff;border-color:rgba(0,0,0,0.06)}
  .timer-badge{background:#2f5fa6;color:#fff;padding:6px 10px;border-radius:8px;font-weight:600;float:right}
  .performance-row{display:flex;gap:18px;margin-top:14px;align-items:center}
  .small{font-size:13px;color:var(--muted)}
  .flashcard-box{display:flex;flex-direction:column;gap:8px}
  .flashcard-big{border-radius:8px;padding:20px;border:1px solid rgba(15,23,42,0.04);background:#fff;text-align:center}
  .flashcard-big h3{margin:0;font-size:28px}
  .flashcard-sub{color:var(--muted);margin-top:8px}
  .stats-list{margin-top:12px}
  .stats-list div{margin:6px 0}
  .focus-card .focus-pill{background:#e7f7ec;color:#064;display:inline-block;padding:10px;border-radius:8px;margin-bottom:8px}
  .suggestion{background:#fcfbff;border-radius:8px;padding:10px;border:1px solid rgba(15,23,42,0.04)}
  .toolbar{display:flex;gap:8px;align-items:center;margin-top:12px}
  .small-btn{padding:8px 12px;border-radius:8px;border:1px solid rgba(15,23,42,0.06);background:#fff;cursor:pointer}
  .tabs{display:flex;gap:8px;margin-bottom:12px}
  .tab{padding:8px 12px;border-radius:8px;background:transparent;border:1px solid transparent;cursor:pointer;color:var(--muted)}
  .tab.active{background:#fff;border-color:rgba(15,23,42,0.04);color:#0b2f5a}
  @media (max-width:980px){ .grid{grid-template-columns:1fr;} header nav{overflow:auto} }
  .modal{position:fixed;inset:0;background:rgba(2,6,23,0.5);display:flex;align-items:center;justify-content:center;padding:20px;z-index:60}
  .modal .modal-card{width:100%;max-width:1000px;height:80vh;background:white;border-radius:10px;overflow:hidden;display:flex;flex-direction:column}
  .modal .modal-card .toolbar{padding:10px;border-bottom:1px solid rgba(12,26,50,0.04)}
  .modal iframe{flex:1;border:0}
  .label{font-weight:600;color:#1f2937}
</style>
</head>
<body>
  <div class="wrap">
    <header class="topbar" role="banner">
      <div class="brand">
        <!-- Header image — uploaded file path included for reference/transform -->
        <img src="/mnt/data/ChatGPT Image Nov 21, 2025 at 08_24_03 AM.png" alt="logo" onerror="this.style.display='none'"/>
        Study Hub
      </div>

      <nav role="navigation" aria-label="Main navigation">
        <button onclick="openSettings()">⚙️ Settings</button>
        <button onclick="showTab('aiTutor')">AI Tutor</button>
        <button onclick="showTab('quizMaker')">Quiz Maker</button>
        <button onclick="showTab('games')">Games</button>
        <button onclick="showTab('timers')">Timers</button>
        <button onclick="showTab('resources')">Resources</button>

        <div style="margin-left:auto"></div>

        <!-- Hardcoded API key preview -->
        <div class="api-key-display" title="Gemini API key (hardcoded)">API Key Settings</div>
        <div id="apiKeyTiny" class="api-key-display" style="margin-left:10px"></div>
      </nav>
    </header>

    <div class="grid" id="dashboardGrid" aria-live="polite">
      <div class="card quiz-card" id="quizCard">
        <div style="display:flex;align-items:center;justify-content:space-between">
          <h2>Quiz Study</h2>
          <div class="timer-badge" id="questionTimer">0:30</div>
        </div>

        <div class="question" id="currentQuestion">What is the powerhouse of the cell?</div>

        <div class="choices" id="choicesContainer"></div>

        <div class="performance-row">
          <div>
            <div class="small">Recent performance</div>
            <div class="label" id="recentPerf">—</div>
          </div>

          <div>
            <div class="small">Performance</div>
            <div class="label">Weak areas: <span id="weakAreas">—</span></div>
          </div>
        </div>

        <div style="height:12px"></div>
        <div class="card suggestion" id="suggestionBox">
          <div class="small">Suggestions based on recent performance</div>
          <div style="margin-top:8px">Suggest: <a href="#" id="suggestTopic">—</a></div>
        </div>
      </div>

      <div style="display:flex;flex-direction:column;gap:14px;">
        <div class="card flashcard-box">
          <h2>Flashcards</h2>
          <div class="flashcard-big" id="flashcardBig">
            <h3 id="flashFront">Biotic factor</h3>
            <div class="flashcard-sub" id="flashBack">...a living component of an ecosystem</div>
          </div>

          <div class="stats-list">
            <div><span class="small">Performance</span></div>
            <div> <strong id="flashCorrect">0</strong> correct </div>
            <div> <strong id="flashIncorrect">0</strong> incorrect </div>
          </div>

          <canvas id="flashDonut" width="140" height="140" style="align-self:flex-end;margin-top:-18px"></canvas>
        </div>

        <div class="card focus-card">
          <h2>Focus Area</h2>
          <div class="focus-pill">⚑ Focus: —</div>
          <p class="small" id="focusDesc">—</p>
          <div style="height:10px"></div>
          <div class="small">Next step</div>
          <div id="focusNext">Use study materials from your resources or AI tutor</div>
        </div>
      </div>
    </div>

    <!-- Tabs + panels -->
    <div style="margin-top:18px">
      <div class="card">
        <div class="tabs" role="tablist" aria-label="Tool tabs">
          <div class="tab active" data-tab="dashboardTab">Dashboard</div>
          <div class="tab" data-tab="aiTutorTab">AI Tutor</div>
          <div class="tab" data-tab="quizMakerTab">Quiz Maker</div>
          <div class="tab" data-tab="musicTab">Music</div>
          <div class="tab" data-tab="miniBrowserTab">Mini Browser</div>
          <div class="tab" data-tab="resourcesTab">Resources</div>
          <div class="tab" data-tab="weakAreasTab">Weak Areas</div>
          <div class="tab" data-tab="leaderboardTab">Leaderboard</div>
        </div>

        <div id="dashboardTab" class="tab-panel">
          <div class="small muted">Overview of your study progress, performance and suggestions.</div>
        </div>

        <div id="aiTutorTab" class="tab-panel" style="display:none">
          <h3>AI Tutor</h3>
          <textarea id="aiQuestion" rows="3" style="width:100%;padding:10px;border-radius:8px;border:1px solid rgba(15,23,42,0.06)"></textarea>
          <div style="margin-top:8px;display:flex;gap:8px;align-items:center">
            <select id="aiModelSelect" style="padding:8px;border-radius:8px">
              <option value="text-bison-001">Gemini / Text Bison</option>
            </select>
            <button class="small-btn" onclick="askTutor()">Ask Tutor</button>
            <div id="aiLoading" class="small muted"></div>
          </div>
          <div id="aiAnswer" style="margin-top:12px;padding:12px;border-radius:8px;background:#fbfdff;border:1px solid rgba(15,23,42,0.04)"></div>
        </div>

        <div id="quizMakerTab" class="tab-panel" style="display:none">
          <h3>Quiz Maker</h3>
          <input id="quizTopicInput" placeholder="Topic (e.g. Cells)" style="width:60%;padding:8px;border-radius:8px;border:1px solid rgba(15,23,42,0.06)">
          <button class="small-btn" onclick="generateQuiz()">Generate Quiz</button>
          <div id="quizMakerResult" style="margin-top:12px"></div>
        </div>

        <div id="musicTab" class="tab-panel" style="display:none">
          <h3>Music Player</h3>
          <input id="musicURL" placeholder="YouTube link or file URL" style="width:70%;padding:8px;border-radius:8px;border:1px solid rgba(15,23,42,0.06)">
          <button class="small-btn" onclick="loadMusic()">Load</button>
          <div style="margin-top:10px"><iframe id="ytFrame" width="100%" height="166" src="" frameborder="0" allow="autoplay; encrypted-media"></iframe></div>
        </div>

        <div id="miniBrowserTab" class="tab-panel" style="display:none">
          <h3>Mini Browser</h3>
          <input id="miniURL" placeholder="https://example.com" style="width:70%;padding:8px;border-radius:8px;border:1px solid rgba(15,23,42,0.06)">
          <button class="small-btn" onclick="openMini()">Open</button>
        </div>

        <div id="resourcesTab" class="tab-panel" style="display:none">
          <h3>Resources</h3>
          <div style="display:flex;gap:8px;flex-wrap:wrap">
            <a class="small-btn" href="https://www.khanacademy.org" target="_blank">Khan Academy</a>
            <a class="small-btn" href="https://www.desmos.com" target="_blank">Desmos</a>
            <a class="small-btn" href="https://phet.colorado.edu" target="_blank">PhET</a>
          </div>
        </div>

        <div id="weakAreasTab" class="tab-panel" style="display:none">
          <h3>Weak Areas</h3>
          <div id="weakOutput"></div>
        </div>

        <div id="leaderboardTab" class="tab-panel" style="display:none">
          <h3>Leaderboard & Streaks</h3>
          <div id="leaderboardOutput"></div>
        </div>

      </div>
    </div>

  </div>

  <!-- Mini Browser modal -->
  <div id="miniModal" class="modal" style="display:none">
    <div class="modal-card">
      <div class="toolbar">
        <input id="modalURL" placeholder="Enter URL" style="flex:1;padding:8px;border-radius:6px;border:1px solid rgba(15,23,42,0.06)">
        <button onclick="modalLoad()" class="small-btn">Go</button>
        <button onclick="closeMini()" class="small-btn">Close</button>
      </div>
      <iframe id="modalFrame" src="about:blank"></iframe>
    </div>
  </div>

<script>
/* === HARD-CODED GEMINI KEY (as provided) === */
const GEMINI_API_KEY = 'AIzaSyDbFvQgRDH_D9i7lWsUKBkU28JN1grh-V8';
document.getElementById('apiKeyTiny').textContent = GEMINI_API_KEY.slice(0,24) + '...';

/* === Storage & initial state === */
const STORAGE_PREFIX = 'studyhub_v1_';
let state = {
  quizzes: JSON.parse(localStorage.getItem(STORAGE_PREFIX + 'quizzes') || '[]'),
  flashcards: JSON.parse(localStorage.getItem(STORAGE_PREFIX + 'flashcards') || '[]'),
  progress: JSON.parse(localStorage.getItem(STORAGE_PREFIX + 'progress') || '{}'),
  leaderboard: JSON.parse(localStorage.getItem(STORAGE_PREFIX + 'leaderboard') || '{}')
};

if(state.flashcards.length === 0){
  state.flashcards.push({front:'Biotic factor', back:'a living component of an ecosystem', correct:5, wrong:3, topic:'Biology'});
  state.flashcards.push({front:'Mitochondrion', back:'Powerhouse of the cell', correct:4, wrong:2, topic:'Biology'});
  localStorage.setItem(STORAGE_PREFIX + 'flashcards', JSON.stringify(state.flashcards));
}
if(state.quizzes.length === 0){
  state.quizzes.push({topic:'Biology', questions:[
    {q:'What is the powerhouse of the cell?', options:['Chloroplast','Nucleus','Mitochondrion','Ribosome'], correct:'C'}
  ]});
  localStorage.setItem(STORAGE_PREFIX + 'quizzes', JSON.stringify(state.quizzes));
}

/* save function */
function saveState(){ localStorage.setItem(STORAGE_PREFIX + 'quizzes', JSON.stringify(state.quizzes)); localStorage.setItem(STORAGE_PREFIX + 'flashcards', JSON.stringify(state.flashcards)); localStorage.setItem(STORAGE_PREFIX + 'progress', JSON.stringify(state.progress)); localStorage.setItem(STORAGE_PREFIX + 'leaderboard', JSON.stringify(state.leaderboard)); }

/* ---------- UI wiring for tabs ---------- */
document.querySelectorAll('.tab').forEach(t=> t.addEventListener('click', ()=> {
  document.querySelectorAll('.tab').forEach(x=>x.classList.remove('active'));
  t.classList.add('active');
  document.querySelectorAll('.tab-panel').forEach(p=>p.style.display='none');
  document.getElementById(t.dataset.tab).style.display = 'block';
}));

/* ---------- Quiz UI ---------- */
let currentQuizIndex = 0;
let currentQuestionIndex = 0;
let questionTimerId = null;
let questionSeconds = 30;

function renderQuizUI(){
  const quiz = state.quizzes[currentQuizIndex];
  if(!quiz) return;
  const qObj = quiz.questions[currentQuestionIndex];
  document.getElementById('currentQuestion').textContent = qObj.q;
  const choicesEl = document.getElementById('choicesContainer');
  choicesEl.innerHTML = '';
  qObj.options.forEach((opt,idx) => {
    const code = String.fromCharCode(65 + idx);
    const div = document.createElement('div');
    div.className = 'choice';
    div.textContent = opt;
    div.dataset.choice = code;
    div.addEventListener('click', ()=> selectChoice(div, code));
    choicesEl.appendChild(div);
  });
  startQuestionTimer(30);
  updatePerformanceSummary();
}

function selectChoice(div, code){
  const quiz = state.quizzes[currentQuizIndex];
  const qObj = quiz.questions[currentQuestionIndex];
  const correctCode = qObj.correct;
  if(code === correctCode){
    div.classList.add('correct');
  } else {
    div.classList.add('wrong');
    const correctEl = Array.from(document.querySelectorAll('.choice')).find(c=>c.dataset.choice === correctCode);
    if(correctEl) correctEl.classList.add('correct');
  }
  const key = quiz.topic;
  state.progress[key] = state.progress[key] || [];
  state.progress[key].push({question:qObj.q, selected:code, correct:correctCode, ts:Date.now()});
  // update leaderboard & stats
  updateLeaderboardsForTopic(key);
  saveState();
  stopQuestionTimer();
  setTimeout(()=> {
    currentQuestionIndex++;
    if(currentQuestionIndex >= quiz.questions.length){
      currentQuestionIndex = 0;
      currentQuizIndex = (currentQuizIndex + 1) % state.quizzes.length;
    }
    renderQuizUI();
  }, 700);
}

/* timer */
function startQuestionTimer(seconds){
  questionSeconds = seconds;
  document.getElementById('questionTimer').textContent = formatTime(questionSeconds);
  stopQuestionTimer();
  questionTimerId = setInterval(()=>{
    questionSeconds--;
    if(questionSeconds <= 0){
      stopQuestionTimer();
      const quiz = state.quizzes[currentQuizIndex];
      const qObj = quiz.questions[currentQuestionIndex];
      state.progress[quiz.topic] = state.progress[quiz.topic] || [];
      state.progress[quiz.topic].push({question:qObj.q, selected:'', correct:qObj.correct, ts:Date.now(), timedOut:true});
      updateLeaderboardsForTopic(quiz.topic);
      saveState();
      const correctCode = qObj.correct;
      Array.from(document.querySelectorAll('.choice')).forEach(c=>{
        if(c.dataset.choice === correctCode) c.classList.add('correct'); else c.classList.add('wrong');
      });
      setTimeout(()=>{ currentQuestionIndex = (currentQuestionIndex+1) % quiz.questions.length; renderQuizUI(); }, 900);
    }
    document.getElementById('questionTimer').textContent = formatTime(questionSeconds);
  }, 1000);
}
function stopQuestionTimer(){ if(questionTimerId) clearInterval(questionTimerId); questionTimerId = null; }
function formatTime(sec){ const m = Math.floor(sec/60); const s = sec%60; return `${m}:${s.toString().padStart(2,'0')}`; }

/* ---------- Flashcard UI & donut ---------- */
let currentFlashIndex = 0;
let donutChart = null;
function renderFlashcardBig(){
  const fc = state.flashcards[currentFlashIndex];
  if(!fc) return;
  document.getElementById('flashFront').textContent = fc.front;
  document.getElementById('flashBack').textContent = fc.back;
  document.getElementById('flashCorrect').textContent = fc.correct || 0;
  document.getElementById('flashIncorrect').textContent = fc.wrong || 0;
  renderDonutChart();
}
function nextFlashcard(){ currentFlashIndex = (currentFlashIndex + 1) % state.flashcards.length; renderFlashcardBig(); }
function prevFlashcard(){ currentFlashIndex = (currentFlashIndex - 1 + state.flashcards.length) % state.flashcards.length; renderFlashcardBig(); }
function renderDonutChart(){
  const fc = state.flashcards[currentFlashIndex];
  const correct = fc.correct || 0;
  const wrong = fc.wrong || 0;
  const ctx = document.getElementById('flashDonut').getContext('2d');
  if(donutChart) donutChart.destroy();
  donutChart = new Chart(ctx, {
    type:'doughnut',
    data:{labels:['Correct','Incorrect'], datasets:[{data:[correct, wrong], backgroundColor:['#57b96a','#f56565']}]},
    options:{cutout:70, plugins:{legend:{display:false}, tooltip:{enabled:false}}, responsive:false}
  });
}

/* ---------- Generate quiz with Gemini and parse ---------- */
async function generateQuiz(){
  const topic = document.getElementById('quizTopicInput').value.trim();
  if(!topic){ alert('Enter a topic'); return; }
  const prompt = `Create a 5-question multiple choice quiz about ${topic}. Format:
1. Question?
A) ...
B) ...
C) ...
D) ...
Answer: B

Provide 5 such questions.`;
  document.getElementById('quizMakerResult').innerText = 'Generating quiz...';
  try{
    const raw = await callGemini(prompt, {maxOutputTokens:800});
    const text = raw.trim();
    const questions = parseMCQFromText(text);
    if(questions.length === 0){ document.getElementById('quizMakerResult').innerText = 'Could not parse response.'; return; }
    state.quizzes.push({topic, questions});
    saveState();
    document.getElementById('quizMakerResult').innerHTML = `<div style="padding:10px;border-radius:8px;background:#fbfdff">Quiz '${topic}' added — ${questions.length} questions.</div>`;
    currentQuizIndex = state.quizzes.length - 1;
    currentQuestionIndex = 0;
    renderQuizUI();
  }catch(e){
    console.error(e);
    document.getElementById('quizMakerResult').innerText = 'Error generating quiz: ' + (e.message || e);
  }
}

function parseMCQFromText(text){
  const lines = text.split(/\r?\n/).map(l=>l.trim()).filter(l=>l);
  const questions = [];
  let i=0;
  while(i < lines.length){
    const qMatch = lines[i].match(/^\d+\.\s*(.*)/);
    if(qMatch){
      const qText = qMatch[1].trim();
      const opts = [];
      i++;
      for(let k=0;k<4 && i < lines.length; i++,k++){
        const optMatch = lines[i].match(/^[A-D]\)\s*(.*)/i);
        if(optMatch) opts.push(optMatch[1].trim());
        else break;
      }
      const ansLine = lines.slice(i, i+2).find(l => /^Answer:/i.test(l));
      if(ansLine){
        const ans = ansLine.split(':')[1].trim().toUpperCase();
        i = i + 1;
        if(opts.length === 4){
          questions.push({q:qText, options:opts, correct:ans});
        }
      } else {
        i++;
      }
    } else {
      i++;
    }
  }
  return questions;
}

/* ---------- Gemini call (client-side) ---------- */
async function callGemini(prompt, opts = {}){
  const url = 'https://generativelanguage.googleapis.com/v1beta2/models/text-bison-001:generate';
  const body = { prompt:{text: prompt}, temperature:0.2, maxOutputTokens: opts.maxOutputTokens || 400 };
  const res = await fetch(url + '?key=' + encodeURIComponent(GEMINI_API_KEY), {
    method:'POST',
    headers:{'Content-Type':'application/json'},
    body: JSON.stringify(body)
  });
  const data = await res.json();
  const candidate = (data?.candidates && data.candidates[0]) || data;
  const content = candidate?.content || candidate?.output || candidate?.text || JSON.stringify(data);
  return content;
}

/* ---------- AI Tutor ---------- */
async function askTutor(){
  const q = document.getElementById('aiQuestion').value.trim();
  if(!q) return;
  document.getElementById('aiLoading').textContent = 'Thinking...';
  document.getElementById('aiAnswer').textContent = '';
  try{
    const out = await callGemini(q, {maxOutputTokens:300});
    document.getElementById('aiAnswer').textContent = out;
  }catch(e){
    document.getElementById('aiAnswer').textContent = 'Error: ' + (e.message || e);
  } finally {
    document.getElementById('aiLoading').textContent = '';
  }
}

/* ---------- Music / Mini browser ---------- */
function loadMusic(){
  const u = document.getElementById('musicURL').value.trim();
  if(!u) return alert('Enter a URL');
  const yt = u.match(/(?:v=|youtu\.be\/|embed\/)([A-Za-z0-9_-]{11})/);
  if(yt){
    document.getElementById('ytFrame').src = 'https://www.youtube.com/embed/' + yt[1] + '?autoplay=1&rel=0';
  } else {
    document.getElementById('ytFrame').src = u;
  }
}
function openMini(){
  document.getElementById('miniModal').style.display = 'flex';
  const url = document.getElementById('miniURL').value.trim();
  if(url) document.getElementById('modalFrame').src = url;
}
function closeMini(){ document.getElementById('miniModal').style.display = 'none'; document.getElementById('modalFrame').src = 'about:blank'; }
function modalLoad(){ const u = document.getElementById('modalURL').value.trim(); if(u) document.getElementById('modalFrame').src = u; }

/* ---------- Settings editing (password protected) ---------- */
function openSettings(){
  const pw = prompt('Enter settings password to edit (admin only):');
  if(pw !== 'owner123'){ alert('Settings are protected. Enter correct password to edit.'); return; }
  const newKey = prompt('Enter Gemini API key (or cancel to keep):', GEMINI_API_KEY);
  if(newKey){
    // override in-session (note: GEMINI_API_KEY constant remains for the embedded key, but we set window override)
    window.GEMINI_API_KEY_OVERRIDE = newKey;
    alert('API key updated for this session.');
    document.getElementById('apiKeyTiny').textContent = newKey.slice(0,24) + '...';
  }
}

/* ---------- Performance summary / weak areas ---------- */
function updatePerformanceSummary(){
  const quiz = state.quizzes[currentQuizIndex];
  const topic = quiz.topic;
  const rec = state.progress[topic] || [];
  const lastN = rec.slice(-10);
  const correct = lastN.filter(r=>r.selected && r.selected === r.correct).length;
  const wrong = lastN.filter(r=>!(r.selected && r.selected === r.correct)).length;
  document.getElementById('recentPerf').textContent = `${correct} correct / ${wrong} incorrect`;
  const weak = [];
  for(const t in state.progress){
    const arr = state.progress[t];
    const total = arr.length;
    const wrongCount = arr.filter(r=>!(r.selected && r.selected === r.correct)).length;
    if(total > 0 && (wrongCount/total) > 0.3) weak.push(t);
  }
  document.getElementById('weakAreas').textContent = weak.length ? weak.join(', ') : 'None';
  let worst = null; let worstScore = 0;
  for(const t in state.progress){
    const arr = state.progress[t];
    const total = arr.length; if(total===0) continue;
    const wrongCount = arr.filter(r=>!(r.selected && r.selected === r.correct)).length;
    const ratio = wrongCount/total;
    if(ratio > worstScore){ worstScore = ratio; worst = t; }
  }
  document.getElementById('suggestTopic').textContent = worst || 'Keep practicing';
  // focus card
  document.querySelector('.focus-pill').textContent = worst ? '⚑ Focus: ' + worst : '⚑ Focus: None';
  document.getElementById('focusDesc').textContent = worst ? `Revise key concepts in ${worst}` : 'No immediate weak area.';
}

/* ---------- Leaderboard & streaks ---------- */
function calculateStreak(arr){
  let maxStreak=0, current=0;
  for(const p of arr){
    if(p.selected===p.correct){ current++; if(current>maxStreak) maxStreak=current; } else current=0;
  }
  return maxStreak;
}
function updateLeaderboardsForTopic(topic){
  const arr = state.progress[topic] || [];
  const totalCorrect = arr.filter(p=>p.selected===p.correct).length;
  const streak = calculateStreak(arr);
  state.leaderboard[topic] = state.leaderboard[topic] || {topScore:0, streak:0};
  if(totalCorrect > state.leaderboard[topic].topScore) state.leaderboard[topic].topScore = totalCorrect;
  if(streak > state.leaderboard[topic].streak) state.leaderboard[topic].streak = streak;
  saveState();
  renderLeaderboard();
}
function renderLeaderboard(){
  const out = document.getElementById('leaderboardOutput');
  out.innerHTML = '';
  for(const t in state.leaderboard){
    const v = state.leaderboard[t];
    out.innerHTML += `<div style="padding:8px;border-radius:8px;background:#fbfdff;margin-bottom:6px"><strong>${t}</strong><div>Top correct: ${v.topScore}</div><div>Best streak: ${v.streak}</div></div>`;
  }
}

/* ---------- Weak areas panel ---------- */
function updateWeakAreasPanel(){
  const out = document.getElementById('weakOutput');
  out.innerHTML = '';
  for(const t in state.progress){
    const arr = state.progress[t];
    const total = arr.length;
    const wrong = arr.filter(p=>p.selected!==p.correct).length;
    if(total>0){
      out.innerHTML += `<div style="padding:8px;border-radius:8px;background:#fff;margin-bottom:8px;border:1px solid rgba(15,23,42,0.04)"><strong>${t}</strong><div class="small">${wrong}/${total} incorrect</div></div>`;
    }
  }
}

/* ---------- Init ---------- */
function init(){
  renderQuizUI();
  renderFlashcardBig();
  updatePerformanceSummary();
  renderLeaderboard();
  updateWeakAreasPanel();
  document.addEventListener('keydown',(e)=>{ if(e.key==='ArrowRight') nextFlashcard(); if(e.key==='ArrowLeft') prevFlashcard(); });
}
init();

</script>
</body>
</html>
