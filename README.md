<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ultimate Portal - Study Edition</title>
    <script src="https://cdn.tailwindcss.com"></script>
    
    <style>
        :root { --primary-color: #3b82f6; }
        .bg-primary { background-color: var(--primary-color); }
        .text-primary { color: var(--primary-color); }
        .border-primary { border-color: var(--primary-color); }
        .hover-bg-primary:hover { background-color: var(--primary-color); }
        
        html, body { height: 100%; overflow: hidden; }
        body { font-family: 'Inter', sans-serif; background-color: #1f2937; color: #f3f4f6; display: flex; flex-direction: column; }
        .card { background-color: #374151; border-radius: 0.75rem; box-shadow: 0 10px 15px rgba(0, 0, 0, 0.2); }
        
        /* Main Content Area */
        #main-content-area { flex-grow: 1; overflow-y: auto; padding-bottom: 2rem; }
        
        /* Game Container */
        .game-container { width: 100%; height: calc(100vh - 220px); max-width: 1200px; margin: 0 auto; overflow: hidden; border-radius: 0.75rem; border: 3px solid var(--primary-color); position: relative; background: #111827;}
        
        /* Inputs */
        .input-dark { width: 100%; padding: 10px; border-radius: 6px; border: 1px solid #4b5563; background: #111827; color: #f3f4f6; margin-top: 5px; }
        .input-dark:focus { outline: none; border-color: var(--primary-color); }

        /* Modal */
        .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background-color: rgba(0, 0, 0, 0.8); z-index: 9999; display: flex; justify-content: center; align-items: center; }
        .modal-content { background-color: #1f2937; padding: 2rem; border-radius: 1rem; max-width: 400px; text-align: center; border: 2px solid var(--primary-color); }
        
        .hidden { display: none !important; }
        .locked { opacity: 0.5; pointer-events: none; }

        /* Game Specific Styles */
        .memory-card { perspective: 1000px; cursor: pointer; }
        .memory-card-inner { position: relative; width: 100%; height: 100%; text-align: center; transition: transform 0.6s; transform-style: preserve-3d; }
        .memory-card.flipped .memory-card-inner { transform: rotateY(180deg); }
        .memory-card-front, .memory-card-back { position: absolute; width: 100%; height: 100%; backface-visibility: hidden; display: flex; align-items: center; justify-content: center; border-radius: 0.5rem; border: 2px solid #4b5563; }
        .memory-card-front { background-color: #374151; color: white; transform: rotateY(180deg); font-size: 2rem; }
        .memory-card-back { background-color: #1f2937; color: transparent; }
    </style>

    <!-- 1. STANDARD SCRIPT for UI -->
    <script>
        // -- Global State --
        window.currentTheme = '#3b82f6';
        window.GLOBAL_KEY = ''; 
        window.ownerUnlocked = false;

        // -- UI Functions --
        function openTab(tabName, button) {
            ['bookmarklets', 'games', 'study', 'settings'].forEach(t => {
                const el = document.getElementById(t);
                if(el) el.classList.add('hidden');
            });
            const target = document.getElementById(tabName);
            if(target) target.classList.remove('hidden');
            document.querySelectorAll('nav button').forEach(b => b.classList.remove('bg-gray-700', 'text-white'));
            if(button) button.classList.add('bg-gray-700', 'text-white');
            if (tabName !== 'settings') document.getElementById('settings-panel').classList.add('hidden');
        }

        function toggleSettings() {
            document.getElementById('settings-panel').classList.toggle('hidden');
        }

        function handleColorChange(e) {
            const color = e.target.value;
            document.documentElement.style.setProperty('--primary-color', color);
            window.currentTheme = color;
            window.dispatchEvent(new CustomEvent('themeChanged', { detail: color }));
        }

        // -- GAME LAUNCHER LOGIC --
        function loadGame(gameId) {
            const container = document.getElementById('game-stage');
            container.innerHTML = ''; // Clear current game

            if (gameId === 'math') startMathGame(container);
            else if (gameId === 'memory') startMemoryGame(container);
            else if (gameId === 'typing') startTypingGame(container);
        }

        // --- 1. MATH GAME ---
        function startMathGame(container) {
            let score = 0;
            let timeLeft = 60;
            let timer;
            
            container.innerHTML = `
                <div class="flex flex-col items-center justify-center h-full">
                    <h2 class="text-4xl font-bold mb-4 text-primary">Math Sprint</h2>
                    <div class="text-xl mb-4">Score: <span id="math-score">0</span> | Time: <span id="math-time">60</span>s</div>
                    <div id="math-problem" class="text-6xl font-bold mb-6 p-6 bg-gray-800 rounded-xl border-2 border-gray-600">Ready?</div>
                    <input type="number" id="math-answer" class="input-dark text-center text-2xl w-48" placeholder="Answer" disabled>
                    <button id="math-start-btn" class="mt-4 px-6 py-2 bg-green-600 rounded font-bold hover:bg-green-500">Start Game</button>
                </div>
            `;

            const problemEl = document.getElementById('math-problem');
            const inputEl = document.getElementById('math-answer');
            const startBtn = document.getElementById('math-start-btn');
            let currentAnswer = 0;

            function generateProblem() {
                const ops = ['+', '-', '*'];
                const op = ops[Math.floor(Math.random() * ops.length)];
                let a = Math.floor(Math.random() * 12) + 1;
                let b = Math.floor(Math.random() * 12) + 1;
                
                if (op === '*') { // Simplify mult
                    a = Math.floor(Math.random() * 9) + 1;
                    b = Math.floor(Math.random() * 9) + 1;
                }
                
                currentAnswer = eval(`${a} ${op} ${b}`);
                problemEl.textContent = `${a} ${op === '*' ? 'x' : op} ${b}`;
                inputEl.value = '';
                inputEl.focus();
            }

            startBtn.onclick = () => {
                startBtn.classList.add('hidden');
                inputEl.disabled = false;
                score = 0;
                timeLeft = 60;
                document.getElementById('math-score').textContent = score;
                generateProblem();
                
                timer = setInterval(() => {
                    timeLeft--;
                    document.getElementById('math-time').textContent = timeLeft;
                    if (timeLeft <= 0) {
                        clearInterval(timer);
                        problemEl.textContent = "Game Over!";
                        inputEl.disabled = true;
                        startBtn.textContent = "Play Again";
                        startBtn.classList.remove('hidden');
                    }
                }, 1000);
            };

            inputEl.addEventListener('keyup', (e) => {
                if (e.key === 'Enter') {
                    if (parseInt(inputEl.value) === currentAnswer) {
                        score += 10;
                        document.getElementById('math-score').textContent = score;
                        generateProblem();
                    } else {
                        // Penalty visual?
                        inputEl.classList.add('border-red-500');
                        setTimeout(()=>inputEl.classList.remove('border-red-500'), 200);
                        inputEl.value = '';
                    }
                }
            });
        }

        // --- 2. MEMORY GAME ---
        function startMemoryGame(container) {
            const icons = ['⚛️', '🧬', '📐', '💻', '🌍', '📚', '🎨', '🔬'];
            let cards = [...icons, ...icons];
            // Shuffle
            cards.sort(() => 0.5 - Math.random());

            let gridHtml = '';
            cards.forEach((icon, index) => {
                gridHtml += `
                    <div class="memory-card h-24 w-full" data-icon="${icon}" onclick="flipCard(this)">
                        <div class="memory-card-inner h-full w-full">
                            <div class="memory-card-front">${icon}</div>
                            <div class="memory-card-back"></div>
                        </div>
                    </div>
                `;
            });

            container.innerHTML = `
                <div class="flex flex-col items-center justify-center h-full p-4">
                    <h2 class="text-3xl font-bold mb-2 text-primary">Memory Match</h2>
                    <p class="mb-4 text-gray-400">Find all pairs to win!</p>
                    <div class="grid grid-cols-4 gap-4 w-full max-w-md">
                        ${gridHtml}
                    </div>
                    <button onclick="loadGame('memory')" class="mt-6 px-4 py-2 bg-gray-600 rounded hover:bg-gray-500">Reset</button>
                </div>
            `;
            
            window.hasFlippedCard = false;
            window.lockBoard = false;
            window.firstCard = null;
            window.secondCard = null;
        }

        window.flipCard = function(card) {
            if (window.lockBoard) return;
            if (card === window.firstCard) return;

            card.classList.add('flipped');

            if (!window.hasFlippedCard) {
                window.hasFlippedCard = true;
                window.firstCard = card;
                return;
            }

            window.secondCard = card;
            checkForMatch();
        }

        function checkForMatch() {
            let isMatch = window.firstCard.dataset.icon === window.secondCard.dataset.icon;
            isMatch ? disableCards() : unflipCards();
        }

        function disableCards() {
            window.firstCard.onclick = null;
            window.secondCard.onclick = null;
            resetBoard();
        }

        function unflipCards() {
            window.lockBoard = true;
            setTimeout(() => {
                window.firstCard.classList.remove('flipped');
                window.secondCard.classList.remove('flipped');
                resetBoard();
            }, 1000);
        }

        function resetBoard() {
            [window.hasFlippedCard, window.lockBoard] = [false, false];
            [window.firstCard, window.secondCard] = [null, null];
        }

        // --- 3. TYPING GAME ---
        function startTypingGame(container) {
             const sentences = [
                "The quick brown fox jumps over the lazy dog.",
                "Mitochondria is the powerhouse of the cell.",
                "To be or not to be, that is the question.",
                "Photosynthesis converts light energy into chemical energy.",
                "The circumference of a circle is pi times the diameter."
            ];
            
            let currentSentence = "";
            let startTime;

            container.innerHTML = `
                <div class="flex flex-col items-center justify-center h-full max-w-2xl mx-auto text-center">
                    <h2 class="text-3xl font-bold mb-4 text-primary">Typing Velocity</h2>
                    <div id="typing-text" class="text-xl mb-6 p-4 bg-gray-800 rounded select-none font-mono text-gray-400">
                        Click Start to begin...
                    </div>
                    <textarea id="typing-input" class="w-full h-24 p-4 bg-gray-900 border border-gray-600 rounded mb-4 text-white font-mono" placeholder="Type here..." disabled></textarea>
                    <div id="typing-result" class="text-lg font-bold h-8 text-green-400"></div>
                    <button id="typing-btn" class="px-6 py-2 bg-blue-600 rounded font-bold hover:bg-blue-500">Start Test</button>
                </div>
            `;

            const textDisplay = document.getElementById('typing-text');
            const inputArea = document.getElementById('typing-input');
            const btn = document.getElementById('typing-btn');
            const result = document.getElementById('typing-result');

            btn.onclick = () => {
                currentSentence = sentences[Math.floor(Math.random() * sentences.length)];
                textDisplay.textContent = currentSentence;
                textDisplay.classList.remove('text-gray-400');
                textDisplay.classList.add('text-white');
                inputArea.value = "";
                inputArea.disabled = false;
                inputArea.focus();
                result.textContent = "";
                startTime = new Date();
                btn.textContent = "Reset";
            };

            inputArea.addEventListener('input', () => {
                const val = inputArea.value;
                if (val === currentSentence) {
                    const endTime = new Date();
                    const timeTaken = (endTime - startTime) / 1000; // seconds
                    const wpm = Math.round((currentSentence.split(' ').length / timeTaken) * 60);
                    result.textContent = `Complete! Speed: ${wpm} WPM`;
                    inputArea.disabled = true;
                    btn.textContent = "New Sentence";
                }
            });
        }


        // -- Owner Settings Logic --
        function unlockOwner() {
            const pass = document.getElementById('owner-pass').value;
            if (pass === 'owner123') {
                window.ownerUnlocked = true;
                document.getElementById('owner-controls').classList.remove('hidden');
                document.getElementById('owner-lock-msg').classList.add('hidden');
                alert('Owner mode unlocked!');
            } else {
                alert('Incorrect password.');
            }
        }

        function saveApiKey() {
            const key = document.getElementById('api-key-input').value.trim();
            if (key) {
                window.GLOBAL_KEY = key;
                if (window.saveGlobalKeyToDB) {
                    window.saveGlobalKeyToDB(key);
                    alert('Hugging Face API Key saved Globally!');
                } else {
                    alert('Database not ready. Key saved locally.');
                }
            }
        }

        // -- AI Logic (Hugging Face) --
        function runAiTool(toolType) {
            const input = document.getElementById(toolType + '-input').value;
            if(!input) return alert("Please enter text.");

            let promptPrefix = "";
            let modelInstructions = ""; // Placeholder for internal model instructions

            // Create specific prompts/instructions based on the tool
            if(toolType === 'teacher') {
                modelInstructions = "Explain the following topic clearly and simply for a student:";
            } else if(toolType === 'quiz') {
                modelInstructions = "Generate a 3-question multiple choice quiz on the following topic. Respond only with the quiz and answers:";
            } else if(toolType === 'flashcard') {
                modelInstructions = "Create 5 term:definition flashcards for the following topic. Format the response as a list of Term: Definition pairs:";
            }
            
            const prompt = `${modelInstructions} ${input}`;
            const targetId = toolType + '-result';
            const resultBox = document.getElementById(targetId);
            
            if(resultBox) {
                 resultBox.innerHTML = '<span class="text-yellow-400">Thinking...</span>';
                 
                 const apiKey = window.GLOBAL_KEY;
                 if(!apiKey) { 
                     resultBox.innerHTML = '<span class="text-red-400">Error: Global API Key not loaded. Please set the Hugging Face API Key in Settings.</span>'; 
                     return; 
                 }

                 // Hugging Face Inference API Configuration
                 const hfModelUrl = "https://api-inference.huggingface.co/models/google/flan-t5-large"; // Placeholder Model URL

                 fetch(hfModelUrl, {
                    method: 'POST',
                    headers: { 
                        'Content-Type': 'application/json',
                        // Use the API Key as a Bearer Token for Hugging Face
                        'Authorization': `Bearer ${apiKey}` 
                    },
                    body: JSON.stringify({
                        inputs: prompt,
                        // Optional parameters for generation
                        parameters: {
                            max_new_tokens: 300,
                            return_full_text: false 
                        }
                    })
                 }).then(r => r.json()).then(data => {
                     let generatedText = "Error: Could not parse response.";
                     
                     // Hugging Face API typically returns an array like [{"generated_text": "..."}]
                     if (Array.isArray(data) && data.length > 0 && data[0].generated_text) {
                         generatedText = data[0].generated_text;
                     } else if (data.error) {
                         // Handle Hugging Face specific errors (e.g., rate limit, model loading)
                         generatedText = `HF Error: ${data.error}. Check your model URL or API Key.`;
                     }

                     resultBox.innerHTML = generatedText.replace(/\n/g, '<br>');

                 }).catch(e => {
                     resultBox.innerHTML = `<span class="text-red-400">Fetch Error: ${e.message}</span>`;
                 });
            }
        }
        
        function runMath() {
            const v = document.getElementById('math-input').value;
            try {
                if (v.includes('window') || v.includes('doc')) throw new Error("Unsafe");
                const res = eval(v);
                document.getElementById('math-result').innerText = "Result: " + res;
            } catch(e) {
                document.getElementById('math-result').innerText = "Error";
            }
        }

        // -- Modal Logic --
        function hideSubscriptionModal() {
            document.getElementById('subscription-modal').classList.add('hidden');
            sessionStorage.setItem('hasSeenModal', 'true');
        }

        function handleSubscribe() {
            window.open('https://www.youtube.com/@cursedgamer2', '_blank');
            hideSubscriptionModal();
        }

        function checkModal() {
            if (!sessionStorage.getItem('hasSeenModal')) {
                document.getElementById('subscription-modal').classList.remove('hidden');
            }
        }

        // -- Initialize UI --
        window.addEventListener('DOMContentLoaded', () => {
            const bookmarklets = [
                { title: "Zoom Text", code: "javascript:(function(){var size=parseFloat(document.body.style.fontSize)||16;size*=1.2;document.body.style.fontSize=size+'px';})();" },
                { title: "Edit Page", code: "javascript:document.body.contentEditable='true';document.designMode='on';void 0" },
                { title: "Dark Mode", code: "javascript:(function(){var s=document.createElement('style');s.innerHTML='body{filter:invert(100%)!important;background:#222!important;}img,video{filter:invert(100%)!important;}';document.head.appendChild(s);})();" },
                { title: "History Back", code: "javascript:history.back()" },
                { title: "Piano", code: "javascript:(function(){var s=document.createElement('script');s.setAttribute('src','https://www.funhtml5games.com/bookmarklets/piano/piano.js');document.body.appendChild(s);})();" }
            ];
            
            const container = document.getElementById('bm-grid');
            if(container) {
                container.innerHTML = bookmarklets.map(b => `
                    <div class="card p-4 hover:ring-2 hover-ring-primary transition">
                        <h3 class="font-bold text-lg mb-2">${b.title}</h3>
                        <a href="${b.code}" class="inline-block px-4 py-2 bg-primary text-white rounded shadow hover:opacity-90">Drag Me</a>
                    </div>
                `).join('');
            }

            checkModal();
            // Load Math game by default
            loadGame('math');
        });
    </script>
</head>
<body class="p-4 sm:p-8">

    <header class="text-center mb-6">
        <h1 class="text-4xl font-extrabold mb-2">Ultimate Portal</h1>
        <p class="text-gray-400">Study Edition</p>
    </header>

    <div id="status-message" class="text-center text-yellow-400 text-xs mb-4 h-4"></div>

    <nav class="flex justify-center card p-1 mb-6 space-x-1 max-w-4xl mx-auto">
        <button onclick="openTab('bookmarklets', this)" class="flex-1 py-2 px-4 rounded-lg font-medium bg-gray-700 text-white transition-all">Bookmarklets</button>
        <button onclick="openTab('games', this)" class="flex-1 py-2 px-4 rounded-lg font-medium text-gray-300 hover:bg-gray-600 transition-all">Study Games</button>
        <button onclick="openTab('study', this)" class="flex-1 py-2 px-4 rounded-lg font-medium text-gray-300 hover:bg-gray-600 transition-all">AI Hub</button>
        <button onclick="toggleSettings()" class="py-2 px-4 rounded-lg font-medium text-gray-300 hover:bg-gray-600 transition-all">Settings</button>
    </nav>
    
    <div id="main-content-area" class="max-w-4xl mx-auto w-full">

        <!-- Settings Panel -->
        <div id="settings-panel" class="card p-6 mb-6 hidden">
            <h2 class="text-2xl font-semibold mb-4">Settings</h2>
            <div class="mb-6">
                <label class="font-medium block mb-2">Theme Color:</label>
                <input type="color" value="#3b82f6" oninput="handleColorChange(event)" class="h-8 w-16 cursor-pointer rounded">
            </div>
            <div class="card p-4 border border-gray-600">
                <h3 class="font-bold text-lg mb-2 text-red-400">Owner Zone</h3>
                <div id="owner-lock-msg">
                    <input type="password" id="owner-pass" placeholder="Enter Password" class="input-dark mb-2">
                    <button onclick="unlockOwner()" class="w-full py-2 bg-gray-600 hover:bg-gray-500 rounded font-bold mt-1">Unlock</button>
                </div>
                <div id="owner-controls" class="hidden">
                    <label class="block text-sm mb-1 text-gray-400">Global AI Key (Hugging Face):</label>
                    <input type="text" id="api-key-input" placeholder="hf_..." class="input-dark mb-2">
                    <button onclick="saveApiKey()" class="w-full py-2 bg-green-600 rounded font-bold">Save Globally</button>
                </div>
            </div>
        </div>

        <!-- Bookmarklets -->
        <div id="bookmarklets" class="tab-content">
            <h2 class="text-3xl font-bold mb-4">Bookmarklets</h2>
            <div id="bm-grid" class="grid grid-cols-1 md:grid-cols-2 gap-4"></div>
        </div>

        <!-- Study Games -->
        <div id="games" class="tab-content hidden">
            <div class="flex justify-between items-center mb-4">
                <h2 class="text-3xl font-bold">Study Games</h2>
                <div class="space-x-2">
                    <button onclick="loadGame('math')" class="text-xs bg-gray-600 px-3 py-1 rounded hover:bg-primary">Math Sprint</button>
                    <button onclick="loadGame('memory')" class="text-xs bg-gray-600 px-3 py-1 rounded hover:bg-primary">Memory Match</button>
                    <button onclick="loadGame('typing')" class="text-xs bg-gray-600 px-3 py-1 rounded hover:bg-primary">Typing</button>
                </div>
            </div>
            
            <!-- Game Stage -->
            <div id="game-stage" class="game-container flex flex-col items-center justify-center">
                <!-- Game content injected here -->
            </div>
        </div>

        <!-- AI Hub -->
        <div id="study" class="tab-content hidden">
            <h2 class="text-3xl font-bold mb-4">AI Hub (Hugging Face)</h2>
            <p class="text-sm text-gray-400 mb-6">Powered by the Global Hugging Face API Key set in Settings.</p>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="card p-4">
                    <h3 class="font-bold text-xl text-blue-400 mb-2">AI Teacher</h3>
                    <textarea id="teacher-input" rows="3" class="input-dark" placeholder="Ask anything..."></textarea>
                    <button onclick="runAiTool('teacher')" class="mt-2 w-full bg-primary py-2 rounded font-bold">Ask</button>
                    <div id="teacher-result" class="mt-3 text-sm p-2 bg-gray-800 rounded h-32 overflow-auto"></div>
                </div>
                <div class="card p-4">
                    <h3 class="font-bold text-xl text-purple-400 mb-2">Quiz Maker</h3>
                    <input id="quiz-input" type="text" class="input-dark" placeholder="Topic">
                    <button onclick="runAiTool('quiz')" class="mt-2 w-full bg-purple-600 py-2 rounded font-bold">Generate</button>
                    <div id="quiz-result" class="mt-3 text-sm p-2 bg-gray-800 rounded h-32 overflow-auto"></div>
                </div>
                <div class="card p-4">
                    <h3 class="font-bold text-xl text-green-400 mb-2">Flashcards</h3>
                    <input id="flashcard-input" type="text" class="input-dark" placeholder="Topic">
                    <button onclick="runAiTool('flashcard')" class="mt-2 w-full bg-green-600 py-2 rounded font-bold">Create</button>
                    <div id="flashcard-result" class="mt-3 text-sm p-2 bg-gray-800 rounded h-32 overflow-auto"></div>
                </div>
                <div class="card p-4">
                    <h3 class="font-bold text-xl text-orange-400 mb-2">Math Solver</h3>
                    <input id="math-input" type="text" class="input-dark" placeholder="e.g. 12 * 12">
                    <button onclick="runMath()" class="mt-2 w-full bg-orange-600 py-2 rounded font-bold">Solve</button>
                    <div id="math-result" class="mt-3 text-sm p-2 bg-gray-800 rounded">Result:</div>
                </div>
            </div>
        </div>

    </div>
    
    <!-- Modal -->
    <div id="subscription-modal" class="modal-overlay hidden">
        <div class="modal-content">
            <h3 class="text-2xl font-bold mb-4 text-primary">👋 Support!</h3>
            <p class="mb-6 text-gray-300">Subscribe to @cursedgamer2</p>
            <button onclick="handleSubscribe()" class="w-full mb-3 px-6 py-3 bg-red-600 text-white font-bold rounded hover:bg-red-700 transition">SUBSCRIBE ▶️</button>
            <button onclick="hideSubscriptionModal()" class="w-full px-6 py-2 bg-gray-600 text-gray-300 font-semibold rounded hover:bg-gray-500">Dismiss</button>
        </div>
    </div>

    <!-- 2. MODULE SCRIPT for Database -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
        
        let db;

        async function initFirebase() {
            if (Object.keys(firebaseConfig).length === 0) return;
            const app = initializeApp(firebaseConfig);
            db = getFirestore(app);
            const auth = getAuth(app);
            await signInAnonymously(auth);
            
            onAuthStateChanged(auth, async (user) => {
                if (user) {
                    document.getElementById('status-message').textContent = "Connected";
                    loadGlobalKey();
                    
                    // Load user theme
                    const tRef = doc(db, 'artifacts', appId, 'users', user.uid, 'settings', 'theme');
                    getDoc(tRef).then(s => {
                        if(s.exists()) {
                             const c = s.data().primaryColor;
                             document.documentElement.style.setProperty('--primary-color', c);
                             const p = document.querySelector('input[type="color"]');
                             if(p) p.value = c;
                        }
                    });
                }
            });
        }

        async function saveGlobalKey(key) {
            if (!db) return;
            // Key is stored in a public, global document
            const ref = doc(db, 'artifacts', appId, 'public', 'data', 'config', 'global_ai');
            await setDoc(ref, { apiKey: key }, { merge: true });
        }

        async function loadGlobalKey() {
            if (!db) return;
            // Key is loaded from the public, global document
            const ref = doc(db, 'artifacts', appId, 'public', 'data', 'config', 'global_ai');
            const snap = await getDoc(ref);
            if (snap.exists()) {
                window.GLOBAL_KEY = snap.data().apiKey;
                console.log("Global Key Loaded");
            }
        }

        window.saveGlobalKeyToDB = saveGlobalKey;
        
        // Listen for theme changes to save them
        window.addEventListener('themeChanged', async (e) => {
            const auth = getAuth();
            if(auth.currentUser) {
                 const ref = doc(db, 'artifacts', appId, 'users', auth.currentUser.uid, 'settings', 'theme');
                 await setDoc(ref, { primaryColor: e.detail }, { merge: true });
            }
        });

        initFirebase();
    </script>
</body>
</html>
