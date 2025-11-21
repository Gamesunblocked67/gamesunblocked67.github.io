<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Interactive Study Hub</title>
<style>
  body { font-family: Arial, sans-serif; margin:0; padding:0; background:#f4f4f4; }
  #hub, #settings, #quizMaker, #flashcards, #studyTool, #musicPlayer, #miniBrowser { display:none; padding:20px; }
  .active { display:block; }
  button { margin:5px; padding:10px; }
  #dashboard { margin-top:10px; background:#eee; padding:10px; }
</style>
</head>
<body>

<div id="loginPanel">
  <h2>Enter Admin Password</h2>
  <input type="password" id="adminPass" placeholder="Password">
  <button onclick="checkPassword()">Login</button>
  <p id="passError" style="color:red;"></p>
</div>

<div id="hub">
  <h1>Study Hub</h1>
  <button onclick="showTool('quizMaker')">Quiz Maker</button>
  <button onclick="showTool('studyTool')">Quiz Study Tool</button>
  <button onclick="showTool('flashcards')">Flashcards</button>
  <button onclick="showTool('musicPlayer')">Music Player</button>
  <button onclick="showTool('miniBrowser')">Mini Browser</button>
  <button onclick="showTool('settings')">Settings</button>
</div>

<div id="settings">
  <h2>Settings</h2>
  <button onclick="hideTool('settings')">Close Settings</button>
  <p>Gemini API Key (hardcoded)</p>
  <p>Owner Password: <strong>owner123</strong></p>
</div>

<div id="quizMaker">
  <h2>Quiz Maker</h2>
  <textarea id="quizInput" placeholder="Enter questions in JSON format"></textarea>
  <button onclick="startQuiz()">Start Quiz</button>
  <div id="quizContainer"></div>
  <button onclick="hideTool('quizMaker')">Back to Hub</button>
</div>

<div id="studyTool">
  <h2>Quiz Study Tool</h2>
  <div id="studyContainer"></div>
  <div id="dashboard"></div>
  <button onclick="hideTool('studyTool')">Back to Hub</button>
</div>

<div id="flashcards">
  <h2>Flashcards</h2>
  <div id="flashcardContainer"></div>
  <button onclick="prevCard()">Previous</button>
  <button onclick="nextCard()">Next</button>
  <button onclick="hideTool('flashcards')">Back to Hub</button>
</div>

<div id="musicPlayer">
  <h2>Music Player</h2>
  <input type="text" id="musicLink" placeholder="YouTube link or local file URL">
  <button onclick="playMusic()">Play</button>
  <audio id="audioPlayer" controls></audio>
  <button onclick="hideTool('musicPlayer')">Back to Hub</button>
</div>

<div id="miniBrowser">
  <h2>Mini Browser</h2>
  <input type="text" id="browserURL" placeholder="Enter URL">
  <button onclick="openBrowser()">Go</button>
  <iframe id="browserFrame" style="width:100%; height:500px;"></iframe>
  <button onclick="hideTool('miniBrowser')">Back to Hub</button>
</div>

<script>
let adminPassword = 'owner123';
let geminiApiKey = 'AIzaSyDbFvQgRDH_D9i7lWsUKBkU28JN1grh-V8';
let currentTool = '';
let quizData = [];
let flashcardsData = [];
let studyProgress = JSON.parse(localStorage.getItem('studyProgress')||'{}');
let currentCard = 0;

function checkPassword(){
  let input = document.getElementById('adminPass').value;
  if(input === adminPassword){
    document.getElementById('loginPanel').style.display = 'none';
    document.getElementById('hub').style.display = 'block';
  } else {
    document.getElementById('passError').innerText = 'Incorrect password';
  }
}

function showTool(tool){
  currentTool = tool;
  document.querySelectorAll('#hub, #settings, #quizMaker, #studyTool, #flashcards, #musicPlayer, #miniBrowser').forEach(el=>el.style.display='none');
  document.getElementById(tool).style.display = 'block';
  if(tool==='studyTool') renderStudy();
  if(tool==='flashcards') renderFlashcard();
}

function hideTool(tool){
  document.getElementById(tool).style.display='none';
  document.getElementById('hub').style.display='block';
}

function startQuiz(){
  try{
    quizData = JSON.parse(document.getElementById('quizInput').value);
    renderStudy();
  }catch(e){
    alert('Invalid JSON format. Example: [{"question":"1+1?","options":["1","2"],"answer":1}]');
  }
}

function renderStudy(){
  let container = document.getElementById('studyContainer');
  container.innerHTML = '';
  quizData.forEach((q,i)=>{
    let div = document.createElement('div');
    div.innerHTML = `<p>${q.question}</p>`+q.options.map((o,j)=>`<button onclick="answerQuestion(${i},${j})">${o}</button>`).join('');
    container.appendChild(div);
  });
  renderDashboard();
}

function answerQuestion(qIndex,optIndex){
  let q = quizData[qIndex];
  if(!studyProgress[qIndex]) studyProgress[qIndex] = {correct:0, wrong:0};
  if(optIndex===q.answer){
    studyProgress[qIndex].correct++;
    alert('Correct!');
  } else {
    studyProgress[qIndex].wrong++;
    alert('Wrong! Correct answer: '+q.options[q.answer]);
  }
  localStorage.setItem('studyProgress', JSON.stringify(studyProgress));
  renderDashboard();
}

function renderDashboard(){
  let dashboard = document.getElementById('dashboard');
  let total = 0, correct = 0;
  for(let k in studyProgress){
    correct += studyProgress[k].correct;
    total += studyProgress[k].correct + studyProgress[k].wrong;
  }
  let accuracy = total?((correct/total)*100).toFixed(1):0;
  dashboard.innerHTML = `<h3>Dashboard</h3>Total Questions Answered: ${total}<br>Correct: ${correct}<br>Accuracy: ${accuracy}%`;
}

function renderFlashcard(){
  if(flashcardsData.length===0){
    flashcardsData = quizData.map(q=>({question:q.question, answer:q.options[q.answer], correct:0, wrong:0}));
  }
  let card = flashcardsData[currentCard];
  document.getElementById('flashcardContainer').innerHTML = `<p>${card.question}</p><p>Answer: ${card.answer}</p><p>Correct: ${card.correct}, Wrong: ${card.wrong}</p>`;
}

function nextCard(){
  currentCard = (currentCard+1)%flashcardsData.length;
  renderFlashcard();
}

function prevCard(){
  currentCard = (currentCard-1+flashcardsData.length)%flashcardsData.length;
  renderFlashcard();
}

function playMusic(){
  let link = document.getElementById('musicLink').value;
  let player = document.getElementById('audioPlayer');
  player.src = link;
  player.play();
}

function openBrowser(){
  let url = document.getElementById('browserURL').value;
  document.getElementById('browserFrame').src = url;
}
</script>
</body>
</html>
