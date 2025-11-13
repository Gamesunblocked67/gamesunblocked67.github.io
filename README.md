<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Gamesunblocked v1.4-beta</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');
:root {
  --primary-purple: #4a148c;
  --light-purple: #7b1fa2;
  --text-color: #e0e0e0;
  --icon-color: #bbbbbb;
  --icon-hover-color: #ffffff;
  --modal-bg: rgba(0,0,0,0.9);
  --modal-border: #9c27b0;
  --grid-bg: #333333;
  --button-bg: #444444;
  --button-hover-bg: #555555;
  --header-bg: #222222;
  --update-color: #ff5722;
  --downgrade-color: #f44336;
  --new-feature-color: #00bcd4;
  --danger: #f44336;
}
body {
  font-family: 'Roboto', sans-serif;
  margin:0; padding:0;
  min-height:100vh; overflow-x:hidden; overflow-y:auto;
  background: linear-gradient(135deg,var(--primary-purple)0%,var(--light-purple)100%);
  color: var(--text-color); display:flex; flex-direction:column; align-items:center; position:relative;
}
.stars {position:fixed;top:0;left:0;width:100%;min-height:100%;pointer-events:none;overflow:hidden;}
.star{position:absolute;background-color:rgba(255,255,255,0.8);border-radius:50%;animation:twinkle linear infinite;opacity:0;}
@keyframes twinkle {0%{transform:scale(0);opacity:0;}50%{transform:scale(1);opacity:1;}100%{transform:scale(0);opacity:0;}}
#update-button-container{position:fixed;top:15px;right:15px;z-index:50;}
#update-button, #downgrade-button{padding:10px 15px;background-color:var(--update-color);color:white;border:none;border-radius:5px;font-weight:700;cursor:pointer;transition:background-color 0.3s, transform 0.3s;}
#update-button{animation:pulse 1.5s infinite;}
#update-button:hover{background-color:#ff8a65;transform:scale(1.05);}
#downgrade-button{background-color:var(--downgrade-color);}
#downgrade-button:hover{background-color:#e53935;transform:scale(1.01);}
@keyframes pulse{0%{box-shadow:0 0 0 0 rgba(255,87,34,0.7);}70%{box-shadow:0 0 0 10px rgba(255,87,34,0);}100%{box-shadow:0 0 0 0 rgba(255,87,34,0);}}
.container{z-index:10;text-align:center;padding:30px;width:90%;max-width:1000px;background-color:rgba(0,0,0,0.6);border:2px solid var(--modal-border);border-radius:15px;box-shadow:0 0 25px var(--modal-border);margin:50px 0;}
.title{font-size:3em;font-weight:700;margin-bottom:5px;color:var(--text-color);}
.subtitle{font-size:1.2em;color:var(--icon-color);margin-bottom:25px;}
.top-menu{display:flex;gap:25px;justify-content:center;margin-bottom:30px;padding-bottom:10px;border-bottom:1px solid rgba(255,255,255,0.1);}
.menu-link{font-size:1.1em;color:var(--icon-color);cursor:pointer;transition: color 0.3s,transform 0.3s;text-decoration:none;text-transform:uppercase;font-weight:700;}
.menu-link:hover{color:var(--icon-hover-color);transform:translateY(-2px);}
.game-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:20px;background-color:var(--grid-bg);padding:20px;border-radius:10px;margin-bottom:10px;}
.game-button{display:flex;flex-direction:column;align-items:center;justify-content:center;background-color:var(--button-bg);color:var(--text-color);border:2px solid transparent;border-radius:8px;padding:15px 10px;cursor:pointer;transition:background-color 0.2s,border-color 0.2s,transform 0.1s;text-decoration:none;text-align:center;}
.game-button:hover{background-color:var(--button-hover-bg);border-color:var(--modal-border);transform:translateY(-2px);}
.game-icon{font-size:3em;margin-bottom:5px;}
.game-title{font-size:1em;font-weight:700;}
.modal{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background-color:var(--modal-bg);z-index:100;justify-content:center;align-items:center;backdrop-filter:blur(5px);}
.modal.active{display:flex;}
.modal-content-game{position:relative;width:90%;height:90%;max-width:1200px;max-height:800px;background-color:black;border:5px solid var(--modal-border);border-radius:10px;box-shadow:0 0 30px rgba(0,0,0,0.7);overflow:hidden;}
.modal-content-update{position:relative;width:350px;padding:30px;background-color:var(--grid-bg);border:3px solid var(--update-color);border-radius:10px;text-align:center;}
.update-bar-container{width:300px;height:20px;background-color:#444;border-radius:10px;margin-top:15px;overflow:hidden;}
#update-progress-bar{width:0%;height:100%;background-color:var(--update-color);transition:width 0.1s linear;}
.modal-movable{position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);width:400px;height:600px;max-width:90%;max-height:90%;background-color:black;border:5px solid var(--modal-border);border-radius:10px;box-shadow:0 0 30px rgba(0,0,0,0.7);overflow:hidden;display:flex;flex-direction:column;cursor:default;}
.modal-header{padding:10px;background-color:var(--header-bg);color:var(--icon-hover-color);border-bottom:1px solid var(--modal-border);cursor:move;display:flex;justify-content:space-between;align-items:center;}
.modal-title{font-weight:bold;}
.close-button{font-size:1.5em;color:var(--icon-hover-color);cursor:pointer;margin-left:10px;transition:color 0.2s;}
.close-button:hover{color:#ff4d4d;}
.bookmark-item{background-color:var(--button-bg);padding:15px;margin-bottom:15px;border-radius:8px;box-shadow:0 2px 5px rgba(0,0,0,0.3);text-align:left;}
.bookmark-link{display:block;color:#4CAF50;font-family:monospace;word-wrap:break-word;text-decoration:none;cursor:copy;font-size:0.8em;margin-top:5px;}
.link-item a{color:#ff9800;text-decoration:none;word-wrap:break-word;}
.link-item a:hover{text-decoration:underline;}
#history-flood-button{background-color:var(--new-feature-color);color:white;padding:10px 15px;border:none;border-radius:5px;cursor:pointer;font-weight:bold;transition:background-color 0.2s;}
#history-flood-button:hover{background-color:#0097a7;}
@media(max-width:768px){.title{font-size:2.5em;}.top-menu{gap:15px;}.game-grid{grid-template-columns:repeat(2,1fr);gap:15px;padding:15px;}.game-icon{font-size:2.5em;}.modal-movable{width:95%;height:90%;}#update-button-container{top:10px;right:10px;}}
</style>
</head>
<body>
<div class="stars"></div>

<div id="update-button-container">
  <button id="update-button">Update to v1.4-beta</button>
  <button id="downgrade-button">Downgrade to v1.3</button>
</div>

<div class="container">
<h1 class="title">gamesunblocked67</h1>
<p class="subtitle"></p>

<div class="top-menu">
<a href="#" class="menu-link" data-modal="updatesModal">Updates</a>
<a href="#" class="menu-link" data-modal="bookmarkletsModal">Bookmarklets</a>
</div>

<div class="game-grid" id="game-grid">
<a href="#" class="game-button" data-modal="gdLiteModal"><span class="game-icon">🔺</span><span class="game-title">Geometry Dash Lite</span></a>
<a href="#" class="game-button" data-modal="gdSubzeroModal"><span class="game-icon">🧊</span><span class="game-title">GD Subzero</span></a>
<a href="#" class="game-button" data-modal="clickerModal"><span class="game-icon">💰</span><span class="game-title">Clicker Game</span></a>
<a href="#" class="game-button" data-modal="amongUsModal"><span class="game-icon">👽</span><span class="game-title">Among Us</span></a>
<a href="#" class="game-button" data-modal="fnafModal"><span class="game-icon">🐻</span><span class="game-title">FNAF 1</span></a>
<a href="#" class="game-button" data-modal="snakeModal"><span class="game-icon">🐍</span><span class="game-title">Snake Classic</span></a>
<a href="#" class="game-button" data-modal="tetrisModal"><span class="game-icon">🧩</span><span class="game-title">Tetris</span></a>
</div>
</div>

<!-- Update Progress Modal -->
<div id="updateProgressModal" class="modal">
  <div class="modal-content-update">
    <h3 id="progress-title">Installing...</h3>
    <p id="update-message">Preparing...</p>
    <div class="update-bar-container"><div id="update-progress-bar"></div></div>
  </div>
</div>

<!-- Updates Modal -->
<div id="updatesModal" class="modal">
  <div class="modal-content-game">
    <span class="close-button" onclick="closeModal('updatesModal')">&times;</span>
    <div class="page-container">
      <h3>✨ Updates & Changelog</h3>
      <div class="setting-item"><strong>v1.4-beta</strong><span>New beta games, bookmarklets. Must sub to cursedgamer2 to unlock beta.</span></div>
      <div class="setting-item"><strong>v1.3.0</strong><span>History Flooder, Cloaker, and bookmarklets added.</span></div>
      <button id="install-update">Install / Update</button>
      <button id="install-downgrade">Downgrade</button>
    </div>
  </div>
</div>

<!-- Bookmarklets Modal -->
<div id="bookmarkletsModal" class="modal">
  <div id="bookmarkletWindow" class="modal-movable">
    <div id="bookmarkletHeader" class="modal-header">
      <span class="modal-title">🔖 Bookmarklet Collection</span>
      <span class="close-button" onclick="closeModal('bookmarkletsModal')">&times;</span>
    </div>
    <div class="page-container">
      <h3>Bookmarklets</h3>
      <p>Click code to copy.</p>
      <div class="bookmark-item"><strong>Cloaker</strong><code class="bookmark-link">javascript:document.title='About: Blank';</code></div>
      <div class="bookmark-item"><strong>History Flooder</strong><code class="bookmark-link">javascript:for(var i=0;i<100;i++){history.pushState(0,0,'/fake'+i);}</code></div>
      <div class="bookmark-item"><strong>Edit Any Page</strong><code class="bookmark-link">javascript:(function(){document.body.contentEditable='true';document.designMode='on'})()</code></div>
      <div class="bookmark-item"><strong>Mini Browser</strong><code class="bookmark-link">javascript:var i=document.createElement('iframe');i.src='https://www.google.com/';i.style.cssText='position:fixed;top:10px;right:10px;width:400px;height:300px;z-index:9999';document.body.appendChild(i);</code></div>
    </div>
  </div>
</div>

<script>
const CURRENT_VERSION='1.4-beta';
const PREVIOUS_VERSION='1.3.0';
const LOCAL_STORAGE_KEY='gamesunblocked_version';
let storedVersion=localStorage.getItem(LOCAL_STORAGE_KEY)||PREVIOUS_VERSION;
localStorage.setItem(LOCAL_STORAGE_KEY,storedVersion);

const updateModal=document.getElementById('updateProgressModal');
const progressBar=document.getElementById('update-progress-bar');
const progressTitle=document.getElementById('progress-title');
const progressMessage=document.getElementById('update-message');
let simulation={running:false,timerId:null};

function openModal(id){document.getElementById(id)?.classList.add('active');}
function closeModal(id){document.getElementById(id)?.classList.remove('active');}

// Top menu links
document.querySelectorAll('.menu-link').forEach(link=>{
    link.addEventListener('click',e=>{
        e.preventDefault();
        const modalId=link.dataset.modal;
        if(modalId) openModal(modalId);
    });
});

// Game buttons
document.querySelectorAll('.game-button').forEach(btn=>{
    btn.addEventListener('click',e=>{
        const modalId=btn.dataset.modal;
        if(modalId) openModal(modalId);
    });
});

// Bookmarklet copy
document.querySelectorAll('.bookmark-link').forEach(code=>{
    code.addEventListener('click',()=>{
        navigator.clipboard.writeText(code.textContent).then(()=>alert('Bookmarklet copied!'));
    });
});

// Update/Downgrade simulation
function startSimulation(targetVersion,isDowngrade){
  if(simulation.running) return;
  simulation.running=true;
  closeModal('updatesModal'); openModal('updateProgressModal');
  progressBar.style.background=isDowngrade?'var(--danger)':'var(--update-color)';
  progressTitle.textContent=isDowngrade?`Downgrading to ${targetVersion}...`:`Installing ${targetVersion}...`;
  progressMessage.textContent='Preparing...'; progressBar.style.width='0%';
  let progress=0;
  simulation.timerId=setInterval(()=>{
    const step=Math.floor(Math.random()*4)+2;
    progress=Math.min(100,progress+step); progressBar.style.width=progress+'%';
    if(progress<15) progressMessage.textContent=isDowngrade?'Removing new files...':'Downloading core files...';
    else if(progress<35) progressMessage.textContent=isDowngrade?'Restoring previous data...':'Installing games & bookmarklets...';
    else if(progress<60) progressMessage.textContent=isDowngrade?'Cleaning settings...':'Updating UI & features...';
    else if(progress<85) progressMessage.textContent=isDowngrade?'Finishing rollback...':'Finalizing configuration...';
    else if(progress<100) progressMessage.textContent='Almost done...';
    else{
      clearInterval(simulation.timerId); simulation.running=false;
      localStorage.setItem(LOCAL_STORAGE_KEY,targetVersion); storedVersion=targetVersion;
      progressMessage.textContent='Complete! Reloading...';
      setTimeout(()=>{closeModal('updateProgressModal'); location.reload();},1000);
    }
  },150);
}

// Buttons
document.getElementById('install-update').addEventListener('click',()=>startSimulation(CURRENT_VERSION,false));
document.getElementById('install-downgrade').addEventListener('click',()=>startSimulation(PREVIOUS_VERSION,true));
document.getElementById('update-button').addEventListener('click',()=>openModal('updatesModal'));
document.getElementById('downgrade-button').addEventListener('click',()=>openModal('updatesModal'));
</script>
</body>
</html>
