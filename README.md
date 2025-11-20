<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI Study Hub</title>
<style>
body { font-family: Arial, sans-serif; margin:0; padding:0; background:#f4f4f4; }
header { background:#333; color:#fff; padding:10px; text-align:center; }
nav { display:flex; justify-content:center; margin:10px 0; flex-wrap:wrap; }
nav button { margin:5px; padding:10px; cursor:pointer; }
section { display:none; padding:20px; }
#settingsPanel { display:none; background:#eee; padding:20px; }
.flashcard, .quiz, .quiz-study, .ai-tutor, .bookmarks { margin-top:20px; }
input[type=text], input[type=password], select { padding:5px; margin:5px 0; width:100%; }
.music-player { margin-top:20px; }
.timer { margin-top:20px; font-size:20px; }
.bookmark-item { padding:5px; background:#ccc; margin:5px 0; cursor:move; }
</style>
</head>
<body>

<header><h1>AI Study Hub</h1></header>

<nav>
<button onclick="showSection('flashcards')">Flashcards</button>
<button onclick="showSection('quizMaker')">Quiz Maker</button>
<button onclick="showSection('quizStudy')">Quiz Study Tool</button>
<button onclick="showSection('aiTutor')">AI Tutor</button>
<button onclick="showSection('music')">Music Player</button>
<button onclick="showSection('timer')">Timer</button>
<button onclick="showSection('browser')">Mini Browser</button>
<button onclick="showSection('bookmarksSection')">Bookmarks</button>
<button onclick="showSettings()">Settings</button>
</nav>

<!-- Flashcards -->
<section id="flashcards">
<h2>Flashcard Maker</h2>
<div class="flashcard">
<input type="text" id="flashQuestion" placeholder="Enter question">
<input type="text" id="flashAnswer" placeholder="Enter answer">
<button onclick="addFlashcard()">Add Flashcard</button>
<div id="flashcardList"></div>
</div>
</section>

<!-- Quiz Maker -->
<section id="quizMaker">
<h2>Quiz Maker</h2>
<div class="quiz">
<input type="text" id="quizQuestion" placeholder="Enter question">
<input type="text" id="quizOption1" placeholder="Option 1">
<input type="text" id="quizOption2" placeholder="Option 2">
<input type="text" id="quizOption3" placeholder="Option 3">
<input type="text" id="quizOption4" placeholder="Option 4">
<input type="text" id="quizAnswer" placeholder="Correct option (1-4)">
<button onclick="addQuiz()">Add Question</button>
<div id="quizList"></div>
</div>
</section>

<!-- Quiz Study Tool -->
<section id="quizStudy">
<h2>Quiz Study Tool</h2>
<div class="quiz-study">
<button onclick="startQuiz()">Start Quiz</button>
<div id="quizQuestions"></div>
</div>
</section>

<!-- AI Tutor -->
<section id="aiTutor">
<h2>AI Tutor</h2>
<div class="ai-tutor">
<input type="text" id="tutorQuestion" placeholder="Ask a question">
<select id="aiProvider">
<option value="openai">OpenAI</option>
<option value="gemini">Gemini</option>
</select>
<button onclick="askAI()">Ask AI</button>
<div id="aiAnswer"></div>
</div>
</section>

<!-- Music Player -->
<section id="music">
<h2>Music Player</h2>
<div class="music-player">
<input type="text" id="musicLink" placeholder="YouTube link or local file URL">
<button onclick="playMusic()">Play</button>
<audio id="audioPlayer" controls></audio>
</div>
</section>

<!-- Timer -->
<section id="timer">
<h2>Timer</h2>
<div class="timer">
<input type="number" id="timerMinutes" placeholder="Minutes">
<button onclick="startTimer()">Start</button>
<div id="timerDisplay">00:00</div>
</div>
</section>

<!-- Mini Browser -->
<section id="browser">
<h2>Mini Browser</h2>
<input type="text" id="miniUrl" placeholder="Enter URL">
<button onclick="loadBrowser()">Go</button>
<iframe id="browserFrame" style="width:100%;height:400px;"></iframe>
</section>

<!-- Bookmarks -->
<section id="bookmarksSection">
<h2>Bookmarks</h2>
<select id="bookmarkSubject">
<option value="Math">Math</option>
<option value="Science">Science</option>
<option value="History">History</option>
<option value="English">English</option>
</select>
<input type="text" id="bookmarkLink" placeholder="Enter link">
<button onclick="addBookmark()">Add Bookmark</button>
<div id="bookmarkList" class="bookmarks"></div>
</section>

<!-- Settings -->
<section id="settingsPanel">
<h2>Settings (Admin)</h2>
<input type="password" id="adminPassword" placeholder="Enter Password">
<button onclick="checkPassword()">Unlock</button>
<div id="settingsContent" style="display:none;">
<h3>API Keys (Stored Globally)</h3>
<p>OpenAI: <span id="openAIKey">sk-abcdef1234567890abcdef1234567890abcdef12</span></p>
<p>Gemini: <span id="geminiKey">AIzaSyDbFvQgRDH_D9i7lWsUKBkU28JN1grh-V8</span></p>
</div>
</section>

<script>
// Sections
function showSection(id) {
  document.querySelectorAll('section').forEach(s=>s.style.display='none');
  document.getElementById(id).style.display='block';
}

// Settings
function showSettings() {
  document.querySelectorAll('section').forEach(s=>s.style.display='none');
  document.getElementById('settingsPanel').style.display='block';
}
function checkPassword() {
  const pass = document.getElementById('adminPassword').value;
  if(pass==='owner123') document.getElementById('settingsContent').style.display='block';
  else alert('Incorrect password');
}

// Flashcards
let flashcards=[];
function addFlashcard() {
  const q=document.getElementById('flashQuestion').value;
  const a=document.getElementById('flashAnswer').value;
  flashcards.push({q,a});
  renderFlashcards();
}
function renderFlashcards() {
  const list=document.getElementById('flashcardList'); list.innerHTML='';
  flashcards.forEach(f=>list.innerHTML+=`<p><b>${f.q}</b> - ${f.a}</p>`);
}

// Quiz Maker
let quizzes=[];
function addQuiz() {
  const q=document.getElementById('quizQuestion').value;
  const options=[document.getElementById('quizOption1').value,document.getElementById('quizOption2').value,document.getElementById('quizOption3').value,document.getElementById('quizOption4').value];
  const a=parseInt(document.getElementById('quizAnswer').value)-1;
  quizzes.push({q,options,a});
  renderQuizzes();
}
function renderQuizzes() {
  const list=document.getElementById('quizList'); list.innerHTML='';
  quizzes.forEach(q=>list.innerHTML+=`<p>${q.q} (Answer: ${q.options[q.a]})</p>`);
}

// Quiz Study
function startQuiz() {
  const container=document.getElementById('quizQuestions'); container.innerHTML='';
  quizzes.forEach((q,i)=>{
    container.innerHTML+=`<p>${q.q}<br>${q.options.map((o,j)=>`<button onclick="checkAnswer(${i},${j})">${o}</button>`).join(' ')}</p>`;
  });
}
function checkAnswer(qIndex,optIndex){
  alert(quizzes[qIndex].a===optIndex?'Correct!':'Incorrect');
}

// AI Tutor
function askAI() {
  const question=document.getElementById('tutorQuestion').value;
  const provider=document.getElementById('aiProvider').value;
  const key = provider==='openai'?openAIKey:geminiKey;
  document.getElementById('aiAnswer').textContent=`Answer from ${provider} (mock): ${question}`;
}

// Music
function playMusic() {
  const link=document.getElementById('musicLink').value;
  const player=document.getElementById('audioPlayer'); player.src=link; player.play();
}

// Timer
let timerInterval;
function startTimer() {
  clearInterval(timerInterval);
  let seconds=parseInt(document.getElementById('timerMinutes').value)*60;
  timerInterval=setInterval(()=>{
    const m=Math.floor(seconds/60); const s=seconds%60;
    document.getElementById('timerDisplay').textContent=`${m.toString().padStart(2,'0')}:${s.toString().padStart(2,'0')}`;
    if(seconds>0) seconds--; else clearInterval(timerInterval);
  },1000);
}

// Mini Browser
function loadBrowser() {
  const url=document.getElementById('miniUrl').value;
  document.getElementById('browserFrame').src=url;
}

// Bookmarks
let bookmarks={Math:[],Science:[],History:[],English:[]};
function addBookmark() {
  const subject=document.getElementById('bookmarkSubject').value;
  const link=document.getElementById('bookmarkLink').value;
  bookmarks[subject].push(link);
  renderBookmarks();
}
function renderBookmarks() {
  const list=document.getElementById('bookmarkList'); list.innerHTML='';
  const subject=document.getElementById('bookmarkSubject').value;
  bookmarks[subject].forEach((b,i)=>{
    const div=document.createElement('div'); div.className='bookmark-item'; div.draggable=true;
    div.textContent=b;
    div.ondragstart=e=>e.dataTransfer.setData('text/plain',i);
    div.ondrop=e=>{
      e.preventDefault();
      const fromIndex=e.dataTransfer.getData('text/plain');
      const temp=bookmarks[subject][fromIndex];
      bookmarks[subject][fromIndex]=bookmarks[subject][i];
      bookmarks[subject][i]=temp;
      renderBookmarks();
    };
    div.ondragover=e=>e.preventDefault();
    list.appendChild(div);
  });
}

// API keys (global inside code)
const openAIKey="sk-abcdef1234567890abcdef1234567890abcdef12";
const geminiKey="AIzaSyDbFvQgRDH_D9i7lWsUKBkU28JN1grh-V8";
</script>

</body>
</html>
