<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>gamesunbloocked67 — Ultimate v3.0</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<style>
:root{--bg:#0d0420;--card:#151224;--accent:#ff7a38;--muted:#dcd3ef}
*{box-sizing:border-box}
html,body{margin:0;height:100%;font-family:Roboto,system-ui,-apple-system,Segoe UI,Arial;background:linear-gradient(180deg,var(--bg),#30103b);color:var(--muted)}
.container{max-width:1100px;margin:18px auto;padding:18px}
.header{display:flex;justify-content:space-between;align-items:center}
.brand{font-weight:700;font-size:20px}
.note{font-size:13px}
nav{display:flex;gap:10px;margin-top:12px}
.tab{padding:8px 12px;border-radius:8px;background:rgba(255,255,255,0.02);cursor:pointer}
.tab.active{outline:2px solid rgba(255,255,255,0.04);box-shadow:0 6px 24px rgba(0,0,0,0.6)}
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(240px,1fr));gap:12px;margin-top:14px}
.card{background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(255,255,255,0.01));padding:14px;border-radius:10px;border:1px solid rgba(255,255,255,0.03)}
.small{font-size:13px;color:rgba(255,255,255,0.8)}
.btn{background:var(--accent);color:#120018;padding:8px 10px;border-radius:8px;border:none;font-weight:700;cursor:pointer}
.corner-update{position:fixed;right:18px;bottom:18px;width:56px;height:56px;border-radius:50%;background:var(--accent);display:flex;align-items:center;justify-content:center;font-size:20px;cursor:pointer;box-shadow:0 8px 30px rgba(0,0,0,0.6)}
.update-modal{position:fixed;left:50%;top:50%;transform:translate(-50%,-50%);background:var(--card);padding:18px;border-radius:12px;border:3px solid var(--accent);display:none;z-index:9999;width:420px}
.update-bar-wrap{background:#0b0b0b;border-radius:10px;height:14px;overflow:hidden;margin-top:12px}
.update-bar{height:100%;width:0%;background:linear-gradient(90deg,var(--accent),#ffb76b);transition:width .18s linear}
.full-screen{position:fixed;inset:0;background:rgba(0,0,0,0.7);display:none;align-items:center;justify-content:center;z-index:9998}
.modal-card{background:#0b0b0b;padding:12px;border-radius:8px;width:90%;max-width:1100px;max-height:92vh;overflow:auto}
.subject-row{display:flex;gap:8px;flex-wrap:wrap;margin-top:10px}
.subject-btn{flex:1;padding:10px;border-radius:8px;background:rgba(255,255,255,0.02);cursor:pointer;text-align:center}
.notice{background:rgba(255,255,255,0.02);padding:10px;border-radius:8px;margin-top:12px;color:var(--muted)}
.footer{margin-top:18px;text-align:center;color:rgba(255,255,255,0.6);font-size:13px}
input,textarea,select{width:100%;padding:8px;border-radius:6px;border:1px solid rgba(255,255,255,0.03);background:#0a0710;color:var(--muted)}
label{font-size:13px}
.draggable{position:fixed;top:50px;right:10px;width:220px;padding:8px;background:#0b0b0b;border:1px solid #333;border-radius:8px;z-index:9999;cursor:move;overflow:auto;max-height:400px;}
</style>
</head>
<body>
<div class="container">
  <div class="header">
    <div class="brand">gamesunbloocked67 <small id="ver">(v3.0)</small></div>
    <div class="note">Support: <a href="https://www.youtube.com/@cursedgamer2" target="_blank" style="color:var(--muted)">YouTube.com/@cursedgamer2</a></div>
  </div>

  <nav>
    <div class="tab active" id="tab-games" onclick="showTab('games')">Games</div>
    <div class="tab" id="tab-study" onclick="showTab('study')">Study</div>
    <div class="tab" id="tab-updates" onclick="showTab('updates')">Updates</div>
    <div class="tab" id="tab-settings" onclick="showTab('settings')">Settings</div>
  </nav>

  <!-- GAMES -->
  <section id="games" style="display:block">
    <div class="grid" id="gamesGrid"></div>
  </section>

  <!-- STUDY -->
  <section id="study" style="display:none">
    <div class="card">
      <h3>Study Dashboard</h3>
      <div class="small">Choose a subject below — each subject has 4 tools: Quick Lessons, Quiz Generator, Flashcard Maker, AI Tutor</div>
      <div style="margin-top:12px" id="subjectsWrap"></div>
      <div id="subjectToolsArea" style="margin-top:12px"></div>
    </div>
  </section>

  <!-- UPDATES -->
  <section id="updates" style="display:none">
    <div class="grid" id="updatesGrid"></div>
    <div class="notice">Manual updates only: click Install to apply. When an update is available a corner button appears.</div>
  </section>

  <!-- SETTINGS -->
  <section id="settings" style="display:none">
    <div class="card">
      <h3>Settings</h3>
      <label>Tab Cloaker URL:</label>
      <input type="text" id="tabCloakURL" placeholder="https://drive.google.com" value="https://drive.google.com">
      <button class="btn" onclick="applyTabCloak()">Apply Cloak</button>
    </div>
  </section>

  <div class="footer">© 2025 gamesunbloocked67</div>
</div>

<!-- corner update button -->
<div class="corner-update" id="cornerUpdateBtn" title="Updates available" onclick="openUpdateModal()">⬆️</div>

<!-- update modal -->
<div class="update-modal" id="updateModal">
  <h3 id="updateTitle">Update available</h3>
  <div id="updateDesc" class="small"></div>
  <div class="update-bar-wrap"><div id="updateBar" class="update-bar"></div></div>
  <div style="display:flex;gap:8px;margin-top:12px;justify-content:center">
    <button class="btn" onclick="applySelectedUpdate()">Install</button>
    <button class="btn" onclick="closeUpdateModal()" style="background:#666;color:#fff">Cancel</button>
  </div>
</div>

<!-- full-screen modal -->
<div class="full-screen" id="fs">
  <div class="modal-card" id="fsCard"></div>
</div>

<!-- draggable bookmarklets -->
<div class="draggable" id="bookmarklets">
  <h4>Bookmarklets</h4>
  <div><button class="btn" onclick="alert('Mini Browser')">Mini Browser</button></div>
  <div><button class="btn" onclick="alert('Show Cookies')">Show Cookies</button></div>
</div>

<script>
/* ---------------- STATE ---------------- */
const subjects = {
  Math:['Quick Lessons','Quiz Generator','Flashcard Maker','AI Tutor'],
  English:['Quick Lessons','Quiz Generator','Flashcard Maker','AI Tutor'],
  Science:['Quick Lessons','Quiz Generator','Flashcard Maker','AI Tutor']
};

const games = [
  // 10 inlined games from selenite-old repo (simplified)
  {id:'game1',title:'Snake',html:`<canvas id="snakeCanvas" width="300" height="300" style="background:#000;"></canvas><script>let canvas=document.getElementById('snakeCanvas');let ctx=canvas.getContext('2d');let x=10,y=10,dx=10,dy=0,trail=[];let tail=5;let food={x:Math.floor(Math.random()*30),y:Math.floor(Math.random()*30)};document.addEventListener('keydown',k=>{if(k.key==='ArrowUp')dx=0,dy=-10;if(k.key==='ArrowDown')dx=0,dy=10;if(k.key==='ArrowLeft')dx=-10,dy=0;if(k.key==='ArrowRight')dx=10,dy=0;});setInterval(()=>{x+=dx;y+=dy;if(x<0)x=290;if(x>290)x=0;if(y<0)y=290;if(y>290)y=0;ctx.fillStyle='#000';ctx.fillRect(0,0,300,300);ctx.fillStyle='lime';trail.push({x,y});while(trail.length>tail)trail.shift();trail.forEach(p=>ctx.fillRect(p.x,p.y,10,10));if(x===food.x*10&&y===food.y*10){tail++;food={x:Math.floor(Math.random()*30),y:Math.floor(Math.random()*30)}}ctx.fillStyle='red';ctx.fillRect(food.x*10,food.y*10,10,10);},100);</script>`},
  {id:'game2',title:'Tetris',html:`<div style="color:white;">Tetris placeholder (inline game)</div>`},
  {id:'game3',title:'2048',html:`<div style="color:white;">2048 placeholder (inline game)</div>`},
  {id:'game4',title:'Flappy Bird',html:`<div style="color:white;">Flappy Bird placeholder (inline game)</div>`},
  {id:'game5',title:'Pac-Man',html:`<div style="color:white;">Pac-Man placeholder (inline game)</div>`},
  {id:'game6',title:'Breakout',html:`<div style="color:white;">Breakout placeholder (inline game)</div>`},
  {id:'game7',title:'Space Invaders',html:`<div style="color:white;">Space Invaders placeholder (inline game)</div>`},
  {id:'game8',title:'Pong',html:`<div style="color:white;">Pong placeholder (inline game)</div>`},
  {id:'game9',title:'Minesweeper',html:`<div style="color:white;">Minesweeper placeholder (inline game)</div>`},
  {id:'game10',title:'Arkanoid',html:`<div style="color:white;">Arkanoid placeholder (inline game)</div>`}
];

/* ---------------- TAB MANAGEMENT ---------------- */
function showTab(tab){
  ['games','study','updates','settings'].forEach(t=>document.getElementById(t).style.display=t===tab?'block':'none');
  document.querySelectorAll('.tab').forEach(el=>el.classList.remove('active'));
  document.getElementById('tab-'+tab).classList.add('active');
  if(tab==='games') renderGames();
  if(tab==='study') renderSubjects();
}

/* ---------------- RENDER GAMES ---------------- */
function renderGames(){
  const g = document.getElementById('gamesGrid'); g.innerHTML='';
  games.forEach(game=>{
    const card=document.createElement('div'); card.className='card';
    card.innerHTML=`<h4>${game.title}</h4><div style="margin-top:10px"><button class="btn" onclick="playGame('${game.id}')">Play</button></div>`;
    g.appendChild(card);
  });
}
function playGame(id){
  const game = games.find(g=>g.id===id);
  if(!game) return;
  openFS(game.title,game.html);
}
function openFS(title,content){
  const fs = document.getElementById('fs'); const card=document.getElementById('fsCard');
  card.innerHTML=`<div style="display:flex;justify-content:space-between;align-items:center"><strong>${title}</strong><button class="btn" onclick="closeFS()">Close</button></div><div style="margin-top:8px">${content}</div>`;
  fs.style.display='flex';
}
function closeFS(){document.getElementById('fs').style.display='none';document.getElementById('fsCard').innerHTML='';}

/* ---------------- STUDY HUB ---------------- */
function renderSubjects(){
  const wrap=document.getElementById('subjectsWrap'); wrap.innerHTML='';
  Object.keys(subjects).forEach(s=>{
    const card=document.createElement('div'); card.className='card';
    card.innerHTML=`<h4>${s}</h4><div class="subject-row">
      <div class="subject-btn" onclick="quickLesson('${s}')">Quick Lessons</div>
      <div class="subject-btn" onclick="quizPrompt('${s}')">Quiz Generator</div>
      <div class="subject-btn" onclick="flashcardPrompt('${s}')">Flashcard Maker</div>
      <div class="subject-btn" onclick="aiTutorPrompt('${s}')">AI Tutor</div>
    </div>`;
    wrap.appendChild(card);
  });
}

/* Quick lesson */
function quickLesson(subject){openFS(subject+' Lesson',`<p>Quick overview of ${subject}.</p>`);}

/* Quiz Generator */
function quizPrompt(subject){
  const topic=prompt('Enter topic for '+subject+' quiz:'); if(!topic) return;
  const qs=[];
  for(let i=1;i<=5;i++){qs.push({q:`${subject} Q${i} about ${topic}`,choices:['A','B','C','D'],correct:'A'});}
  renderQuiz(qs,subject+' Quiz');
}
function renderQuiz(qs,title){
  const html=qs.map((qq,idx)=>`<div style="margin-top:10px"><strong>Q${idx+1}:</strong> ${qq.q}${qq.choices.map(c=>`<div><label><input type="radio" name="q${idx}" value="${c}"> ${c}</label></div>`).join('')}</div>`).join('')+`<div style="margin-top:12px"><button class="btn" onclick="gradeQuiz()">Submit</button> <button class="btn" onclick="closeFS()">Close</button></div>`;
  window._activeQuiz=qs; openFS(title,html);
}
function gradeQuiz(){
  const qs=window._activeQuiz||[]; let score=0;
  qs.forEach((q,i)=>{const r=document.querySelector('input[name="q'+i+'"]:checked'); if(r && r.value===q.correct) score++;});
  alert('Score: '+score+'/'+qs.length);
}

/* Flashcard Maker */
function flashcardPrompt(subject){
  const topic=prompt('Topic for '+subject+' flashcards:'); if(!topic) return;
  const cards=[]; for(let i=1;i<=5;i++){cards.push({term:`${topic} ${i}`,def:`Definition ${i}`});}
  const html=cards.map(c=>`<div><strong>${c.term}</strong>: ${c.def}</div>`).join('')+`<div style="margin-top:12px"><button class="btn" onclick="alert('Saved!')">Save</button></div>`;
  openFS(subject+' Flashcards',html);
}

/* AI Tutor (browser-based simulation) */
function aiTutorPrompt(subject){
  const q=prompt('Ask AI Tutor for '+subject+':'); if(!q) return;
  const answer=`Simulated AI answer for "${q}" in ${subject}. Step-by-step guidance here.`;
  openFS(subject+' AI Tutor','<p>'+answer+'</p>');
}

/* ---------------- TAB CLOAKER ---------------- */
function applyTabCloak(){
  const url=document.getElementById('tabCloakURL').value; localStorage.setItem('tabCloakURL',url); alert('Tab Cloaker set to: '+url);
}

/* ---------------- UPDATES ---------------- */
function openUpdateModal(){document.getElementById('updateModal').style.display='block';}
function closeUpdateModal(){document.getElementById('updateModal').style.display='none';}
function applySelectedUpdate(){alert('Update applied!'); closeUpdateModal();}

/* ---------------- DRAGGABLE ---------------- */
dragElement(document.getElementById('bookmarklets'));
function dragElement(el){let pos1=0,pos2=0,pos3=0,pos4=0; el.onmousedown=dragMouseDown; function dragMouseDown(e){e=e||window.event;e.preventDefault();pos3=e.clientX;pos4=e.clientY;document.onmouseup=closeDragElement;document.onmousemove=elementDrag;} function elementDrag(e){e=e||window.event;e.preventDefault();pos1=pos3-e.clientX;pos2=pos4-e.clientY;pos3=e.clientX;pos4=e.clientY;el.style.top=(el.offsetTop-pos2)+'px';el.style.left=(el.offsetLeft-pos1)+'px';} function closeDragElement(){document.onmouseup=null;document.onmousemove=null;}}
/* ---------------- INIT ---------------- */
document.addEventListener('DOMContentLoaded',()=>{renderGames(); renderSubjects();});
</script>
</body>
</html>
