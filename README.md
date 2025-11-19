<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>7th Grade Ultimate Portal</title>
<script src="https://cdn.tailwindcss.com"></script>
<script src="https://unpkg.com/tone@14.9.15/build/Tone.js"></script>
<script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
  import { getAuth, signInAnonymously, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
  import { getFirestore, doc, setDoc, getDoc, onSnapshot } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

  const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : null;
  const appId = typeof __app_id !== 'undefined' ? __app_id : 'study-hub-v7';

  let db, auth;

  window.appSettings = { theme: '#3b82f6', aiProvider: 'Gemini', keys: {}, resources: {} };

  if (firebaseConfig) {
    const app = initializeApp(firebaseConfig);
    db = getFirestore(app);
    auth = getAuth(app);
    await signInAnonymously(auth);

    onAuthStateChanged(auth, async (user) => {
      if(user) {
        const globalConfigRef = doc(db, 'artifacts', appId, 'public_data', 'global_config');
        onSnapshot(globalConfigRef, (doc) => {
          if(doc.exists()){
            const data = doc.data();
            if(data.keys) window.appSettings.keys = data.keys;
          }
        });

        const userConfigRef = doc(db, 'artifacts', appId, 'users', user.uid, 'settings', 'config');
        const snap = await getDoc(userConfigRef);
        if(snap.exists()){
          const data = snap.data();
          if(data.theme) applyTheme(data.theme);
          if(data.provider) window.appSettings.aiProvider = data.provider;
        }
      }
    });

    window.saveSettingsToDb = async (scope) => {
      if(!auth.currentUser) return;
      if(scope === 'local' || scope === 'both'){
        const userDocRef = doc(db, 'artifacts', appId, 'users', auth.currentUser.uid, 'settings', 'config');
        await setDoc(userDocRef, { theme: window.appSettings.theme, provider: window.appSettings.aiProvider }, {merge:true});
      }
      if((scope === 'global' || scope === 'both') && window.ownerUnlocked){
        const globalDocRef = doc(db, 'artifacts', appId, 'public_data', 'global_config');
        await setDoc(globalDocRef, { keys: window.appSettings.keys }, {merge:true});
        alert("Global API Keys Saved for ALL Users!");
      } else if(scope === 'local'){
        alert("User Preferences Saved.");
      }
    };
  }
</script>

<style>
  :root { --primary-color: #3b82f6; }
  .bg-primary { background-color: var(--primary-color); }
  .text-primary { color: var(--primary-color); }
  .border-primary { border-color: var(--primary-color); }
  body { font-family:'Inter',sans-serif; background:#111827; color:#f3f4f6; height:100vh; overflow:hidden; display:flex; flex-direction:column; }
  ::-webkit-scrollbar { width:8px; }
  ::-webkit-scrollbar-thumb { background:#4b5563; border-radius:4px; }
  ::-webkit-scrollbar-track { background:#1f2937; }
  #content-wrapper{flex-grow:1; overflow:hidden; position:relative;}
  .tab-content{height:100%; display:flex; flex-direction:column; overflow-y:auto; padding-bottom:2rem;}
  .hidden{display:none!important;}
  .resource-link:hover{transform:translateX(5px);}
  .youtube-container{position:relative; padding-bottom:56.25%; height:0; overflow:hidden; border-radius:0.5rem;}
  .youtube-container iframe{position:absolute; top:0; left:0; width:100%; height:100%;}
</style>

<script>
  window.ownerUnlocked = false;
  window.adminUnlocked = false;

  function applyTheme(color){document.documentElement.style.setProperty('--primary-color', color); window.appSettings.theme=color; const picker=document.getElementById('color-picker'); if(picker) picker.value=color;}

  function openTab(tabId, btn){
    document.querySelectorAll('.tab-content').forEach(el=>el.classList.add('hidden'));
    document.getElementById(tabId).classList.remove('hidden');
    document.querySelectorAll('nav button').forEach(b=>b.classList.remove('bg-gray-700','text-white'));
    if(btn) btn.classList.add('bg-gray-700','text-white');
    if(tabId==='resources') loadResources('Math');
  }

  function toggleSettings(){document.getElementById('settings-panel').classList.toggle('hidden');}

  function unlockOwner(){
    const pass = document.getElementById('owner-pass').value;
    if(pass==='owner123'){
      window.ownerUnlocked=true;
      document.getElementById('owner-controls').classList.remove('hidden');
      document.getElementById('owner-lock-msg').classList.add('hidden');
      document.getElementById('gemini-key').value=window.appSettings.keys.gemini||'';
      document.getElementById('openai-key').value=window.appSettings.keys.openai||'';
    } else alert('Incorrect password.');
  }

  function unlockAdmin(){
    const pass = document.getElementById('admin-pass').value;
    if(pass==='admin123'){
      window.adminUnlocked=true;
      document.getElementById('admin-controls').classList.remove('hidden');
      document.getElementById('admin-login').classList.add('hidden');
      loadAdminResources();
    } else alert('Incorrect admin password.');
  }

  function saveLocalSettings(){
    if(window.ownerUnlocked){
      window.appSettings.keys.gemini=document.getElementById('gemini-key').value;
      window.appSettings.keys.openai=document.getElementById('openai-key').value;
    }
    window.appSettings.aiProvider=document.getElementById('ai-provider-select-settings').value;
    const scope=window.ownerUnlocked?'both':'local';
    if(window.saveSettingsToDb) window.saveSettingsToDb(scope);
    document.getElementById('ai-provider-select').value=window.appSettings.aiProvider;
    document.getElementById('settings-panel').classList.add('hidden');
  }

  const resources={
    'Math':[
      {title:"Ratios & Proportions Calculator", url:"https://www.calculatorsoup.com/calculators/math/ratios.php", type:"tool"},
      {title:"Integer Operations Game", url:"https://www.mathplayground.com/ASB_IntegerWarp.html", type:"game"},
      {title:"Equation Solver", url:"https://www.mathpapa.com/algebra-calculator.html", type:"tool"},
      {title:"Area & Circumference Visualizer", url:"https://www.geogebra.org/m/j32G5j47", type:"tool"},
      {title:"Probability Spinner", url:"https://wheelofnames.com/", type:"tool"},
      {title:"Khan Academy: 7th Grade Math", url:"https://www.khanacademy.org/math/cc-seventh-grade-math", type:"link"},
      {title:"IXL Math Practice", url:"https://www.ixl.com/math/grade-7", type:"link"},
      {title:"Desmos Graphing Calculator", url:"https://www.desmos.com/calculator", type:"tool"},
      {title:"Prodigy Math Game", url:"https://www.prodigygame.com/", type:"game"}
    ],
    'Science':[
      {title:"Periodic Table Interactive", url:"https://ptable.com/", type:"tool"},
      {title:"Cell Model Visualizer", url:"https://www.cellsalive.com/cells/cell_model.htm", type:"tool"},
      {title:"NASA Eyes on the Solar System", url:"https://eyes.nasa.gov/", type:"tool"},
      {title:"PhET Simulations", url:"https://phet.colorado.edu/", type:"tool"},
      {title:"Dynamic Earth (Plate Tectonics)", url:"https://interactive.learner.org/interactives/dynamicearth/", type:"tool"},
      {title:"National Geographic Kids", url:"https://kids.nationalgeographic.com/", type:"link"},
      {title:"Switch Zoo (Animal Adaptations)", url:"https://www.switchzoo.com/", type:"game"},
      {title:"Food Chain Game", url:"https://www.sheppardsoftware.com/content/animals/kidscorner/games/foodchaingame.htm", type:"game"},
      {title:"InnerBody Anatomy", url:"https://www.innerbody.com/", type:"tool"},
      {title:"Science Bob Experiments", url:"https://sciencebob.com/", type:"link"}
    ],
    'History':[
      {title:"Ancient Civilizations Timeline", url:"https://www.historyforkids.net/ancient-history.html", type:"link"},
      {title:"Google Earth Voyager", url:"https://earth.google.com/web/voyager", type:"tool"},
      {title:"The Democracy Project", url:"https://pbskids.org/democracy", type:"link"},
      {title:"Mission US (History Game)", url:"https://www.mission-us.org/", type:"game"},
      {title:"Interactive World Map", url:"https://geology.com/world/world-map.shtml", type:"tool"},
      {title:"Smithsonian History Explorer", url:"https://historyexplorer.si.edu/", type:"link"},
      {title:"iCivics (Gov Games)", url:"https://www.icivics.org/games", type:"game"},
      {title:"DocsTeach (Primary Sources)", url:"https://www.docsteach.org/", type:"link"},
      {title:"History.com Classroom", url:"https://www.history.com/classroom", type:"link"},
      {title:"Big History Project", url:"https://www.bighistoryproject.com/", type:"link"}
    ],
    'ELA':[
      {title:"Thesaurus.com", url:"https://www.thesaurus.com/", type:"tool"},
      {title:"Hemingway Editor (Writing Tool)", url:"https://hemingwayapp.com/", type:"tool"},
      {title:"Grammarly (Free Checker)", url:"https://www.grammarly.com/", type:"tool"},
      {title:"Vocabulary.com", url:"https://www.vocabulary.com/", type:"link"},
      {title:"Project Gutenberg (Free Books)", url:"https://www.gutenberg.org/", type:"link"},
      {title:"Poetry Foundation", url:"https://www.poetryfoundation.org/", type:"link"},
      {title:"ReadWriteThink", url:"https://www.readwritethink.org/", type:"link"},
      {title:"Quill.org (Grammar Practice)", url:"https://www.quill.org/", type:"tool"},
      {title:"Storyboard That", url:"https://www.storyboardthat.com/", type:"tool"},
      {title:"Citation Machine", url:"https://www.citationmachine.net/", type:"tool"}
    ]
  };

  function loadResources(subject){
    const list=document.getElementById('resource-list'); list.innerHTML='';
    document.querySelectorAll('.subject-btn').forEach(b=>b.classList.remove('bg-primary','text-white'));
    document.getElementById(`btn-${subject}`).classList.add('bg-primary','text-white');
    resources[subject].forEach(res=>{
      const item=document.createElement('div');
      item.className='bg-gray-800 p-4 rounded-lg border border-gray-700 hover:border-primary transition resource-link cursor-pointer';
      item.innerHTML=`<div class="flex justify-between items-center"><div><h4 class="font-bold text-gray-200">${res.title}</h4><span class="text-xs uppercase font-semibold text-gray-500 tracking-wider">${res.type}</span></div><svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" /></svg></div>`;
      item.onclick=()=>window.open(res.url,'_blank');
      list.appendChild(item);
    });
  }

  function loadAdminResources(){
    const container=document.getElementById('admin-resource-list'); container.innerHTML='';
    for(const subject in resources){
      resources[subject].forEach((res,i)=>{
        const item=document.createElement('div');
        item.className='bg-gray-700 p-2 rounded flex justify-between items-center mb-1';
        item.innerHTML=`<span class="text-sm">${subject} - ${res.title}</span>
        <div class="flex space-x-1">
          <button class="bg-green-500 text-xs px-1 rounded" onclick="editResource('${subject}',${i})">Edit</button>
          <button class="bg-red-500 text-xs px-1 rounded" onclick="deleteResource('${subject}',${i})">Delete</button>
        </div>`;
        container.appendChild(item);
      });
    }
  }

  function addResource(){
    const subj=document.getElementById('admin-new-subject').value;
    const title=document.getElementById('admin-new-title').value;
    const url=document.getElementById('admin-new-url').value;
    const type=document.getElementById('admin-new-type').value;
    if(!subj||!title||!url) return alert('Fill all fields!');
    if(!resources[subj]) resources[subj]=[];
    resources[subj].push({title,url,type});
    loadResources(subj);
    loadAdminResources();
  }

  function deleteResource(subj,i){
    if(confirm('Delete this resource?')){
      resources[subj].splice(i,1);
      loadResources(subj);
      loadAdminResources();
    }
  }

  function editResource(subj,i){
    const res=resources[subj][i];
    const newTitle=prompt('New title',res.title);
    const newURL=prompt('New URL',res.url);
    const newType=prompt('Type (link/tool/game)',res.type);
    if(newTitle) res.title=newTitle;
    if(newURL) res.url=newURL;
    if(newType) res.type=newType;
    loadResources(subj);
    loadAdminResources();
  }
</script>
</head>
<body class="p-4 sm:p-6">
<header class="mb-4 flex justify-between items-center">
  <div>
    <h1 class="text-3xl font-bold text-white">7th Grade Ultimate Hub</h1>
    <p class="text-gray-400 text-sm">Study. Focus. Achieve.</p>
  </div>
  <div class="flex space-x-2">
    <button onclick="toggleSettings()" class="p-2 bg-gray-800 rounded-full hover:bg-gray-700 transition">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-300" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z" /><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" /></svg>
    </button>
  </div>
</header>

<div id="settings-panel" class="hidden fixed inset-0 bg-black bg-opacity-90 z-50 flex items-center justify-center">
  <div class="bg-gray-800 p-6 rounded-xl max-w-md w-full border border-gray-700">
    <h2 class="text-xl font-bold mb-4 text-white">Settings</h2>
    <div class="space-y-4">
      <div>
        <label class="block text-sm text-gray-400">Theme Color</label>
        <input type="color" id="color-picker" class="w-full h-10 rounded" onchange="applyTheme(this.value)">
      </div>
      <div>
        <label class="block text-sm text-gray-400">AI Provider</label>
        <select id="ai-provider-select-settings" class="w-full rounded p-1 bg-gray-700 border border-gray-600">
          <option value="Gemini">Gemini</option>
          <option value="OpenAI">OpenAI</option>
        </select>
      </div>

      <div id="owner-lock-msg" class="space-y-2">
        <input type="password" placeholder="Owner Password" id="owner-pass" class="w-full p-1 rounded bg-gray-700 text-white">
        <button onclick="unlockOwner()" class="bg-primary px-3 py-1 rounded text-white w-full">Unlock Owner</button>
      </div>

      <div id="owner-controls" class="hidden space-y-2">
        <h3 class="text-white font-semibold">API Keys</h3>
        <input type="text" placeholder="Gemini API Key" id="gemini-key" class="w-full p-1 rounded bg-gray-700 text-white">
        <input type="text" placeholder="OpenAI API Key" id="openai-key" class="w-full p-1 rounded bg-gray-700 text-white">
      </div>

      <div class="flex justify-between">
        <button onclick="saveLocalSettings()" class="bg-green-500 px-3 py-1 rounded text-white">Save</button>
        <button onclick="toggleSettings()" class="bg-red-500 px-3 py-1 rounded text-white">Close</button>
      </div>
    </div>
  </div>
</div>

<nav class="flex space-x-2 mb-4 overflow-x-auto">
  <button onclick="openTab('resources',this)" class="px-3 py-1 rounded bg-gray-700 text-white">Resources</button>
  <button onclick="openTab('ai-tools',this)" class="px-3 py-1 rounded hover:bg-gray-700 transition">AI Tools</button>
  <button onclick="openTab('games',this)" class="px-3 py-1 rounded hover:bg-gray-700 transition">Games</button>
  <button onclick="openTab('admin',this)" class="px-3 py-1 rounded hover:bg-gray-700 transition">Admin</button>
</nav>

<div id="content-wrapper">
  <div id="resources" class="tab-content hidden">
    <div class="flex space-x-2 mb-2 overflow-x-auto">
      <button class="subject-btn px-2 py-1 rounded" id="btn-Math" onclick="loadResources('Math')">Math</button>
      <button class="subject-btn px-2 py-1 rounded" id="btn-Science" onclick="loadResources('Science')">Science</button>
      <button class="subject-btn px-2 py-1 rounded" id="btn-History" onclick="loadResources('History')">History</button>
      <button class="subject-btn px-2 py-1 rounded" id="btn-ELA" onclick="loadResources('ELA')">ELA</button>
    </div>
    <div id="resource-list" class="space-y-2"></div>
  </div>

  <div id="ai-tools" class="tab-content hidden">
    <p class="text-gray-400 text-sm">AI Tools coming soon...</p>
  </div>

  <div id="games" class="tab-content hidden">
    <p class="text-gray-400 text-sm">Games coming soon...</p>
  </div>

  <div id="admin" class="tab-content hidden">
    <div id="admin-login" class="mb-2">
      <input type="password" placeholder="Admin Password" id="admin-pass" class="w-full p-1 rounded bg-gray-700 text-white mb-2">
      <button onclick="unlockAdmin()" class="bg-primary px-3 py-1 rounded text-white w-full">Unlock Admin</button>
    </div>

    <div id="admin-controls" class="hidden space-y-4">
      <h2 class="text-white font-bold text-lg">Admin Resource Management</h2>
      <div class="flex space-x-2">
        <input type="text" id="admin-new-subject" placeholder="Subject" class="p-1 rounded bg-gray-700 w-1/4">
        <input type="text" id="admin-new-title" placeholder="Title" class="p-1 rounded bg-gray-700 w-1/3">
        <input type="text" id="admin-new-url" placeholder="URL" class="p-1 rounded bg-gray-700 w-1/3">
        <select id="admin-new-type" class="p-1 rounded bg-gray-700 w-1/6">
          <option value="link">Link</option>
          <option value="tool">Tool</option>
          <option value="game">Game</option>
        </select>
        <button onclick="addResource()" class="bg-green-500 px-3 py-1 rounded text-white">Add</button>
      </div>
      <div id="admin-resource-list" class="space-y-1 max-h-64 overflow-y-auto border-t border-gray-700 pt-2"></div>
    </div>
  </div>
</div>
</body>
</html>
