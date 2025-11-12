# Games-
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gamesunblocked</title>
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');
        
        :root {
            --primary-purple: #4a148c; 
            --light-purple: #7b1fa2;  
            --text-color: #e0e0e0;
            --icon-color: #bbbbbb;
            --icon-hover-color: #ffffff;
            --modal-bg: rgba(0, 0, 0, 0.9);
            --modal-border: #9c27b0; 
            --grid-bg: #333333;
            --button-bg: #444444;
            --button-hover-bg: #555555;
            --header-bg: #222222; /* Header for draggable modals */
        }

        body {
            font-family: 'Roboto', sans-serif;
            margin: 0;
            padding: 0;
            background: linear-gradient(135deg, var(--primary-purple) 0%, var(--light-purple) 100%);
            color: var(--text-color);
            overflow: hidden; 
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            position: relative;
        }

        /* Particle Effect */
        .stars {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            overflow: hidden;
        }

        .star {
            position: absolute;
            background-color: rgba(255, 255, 255, 0.8);
            border-radius: 50%;
            animation: twinkle linear infinite;
            opacity: 0;
        }

        @keyframes twinkle {
            0% { transform: scale(0); opacity: 0; }
            50% { transform: scale(1); opacity: 1; }
            100% { transform: scale(0); opacity: 0; }
        }

        /* Main Container - The Selenite Window */
        .container {
            z-index: 10;
            text-align: center;
            padding: 20px;
            width: 90%;
            max-width: 1000px;
            background-color: rgba(0, 0, 0, 0.6);
            border: 2px solid var(--modal-border);
            border-radius: 15px;
            box-shadow: 0 0 25px var(--modal-border);
            padding: 30px;
        }

        .title {
            font-size: 3em;
            font-weight: 700;
            margin-bottom: 5px;
            color: var(--text-color);
        }

        .subtitle {
            font-size: 1em;
            color: var(--icon-color);
            margin-bottom: 30px;
        }

        /* Game Grid Styling */
        .game-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 20px;
            background-color: var(--grid-bg);
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 30px;
        }

        .game-button {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            background-color: var(--button-bg);
            color: var(--text-color);
            border: 2px solid transparent;
            border-radius: 8px;
            padding: 15px 10px;
            cursor: pointer;
            transition: background-color 0.2s, border-color 0.2s, transform 0.1s;
            text-decoration: none;
            text-align: center;
        }
        
        /* Ensure retro bowl buttons open in new tab if embed fails */
        .game-button.external {
            /* No special style, but used for JS to identify external links */
        }

        .game-button:hover {
            background-color: var(--button-hover-bg);
            border-color: var(--modal-border);
            transform: translateY(-2px);
        }

        .game-icon {
            font-size: 3em;
            margin-bottom: 5px;
        }

        .game-title {
            font-size: 1em;
            font-weight: 700;
        }

        /* Bottom Menu Icons */
        .bottom-menu {
            display: flex;
            gap: 25px;
            justify-content: center;
            margin-top: 20px;
        }

        .menu-icon {
            font-size: 4em;
            color: var(--icon-color);
            cursor: pointer;
            transition: color 0.3s, transform 0.3s;
            text-decoration: none;
        }

        .menu-icon:hover {
            color: var(--icon-hover-color);
            transform: scale(1.1);
        }
        
        /* Icon content using Unicode */
        .menu-icon.gamepad::before { content: '🎮'; }
        .menu-icon.bookmark::before { content: '🔖'; } 
        .menu-icon.news::before { content: '📰'; } 
        .menu-icon.settings::before { content: '⚙️'; } 
        .menu-icon.link::before { content: '🔗'; } 

        /* Modals (Game, Bookmarklet, News, Settings, Links) */
        .modal {
            display: none; 
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: var(--modal-bg);
            z-index: 100;
            justify-content: center;
            align-items: center;
            backdrop-filter: blur(5px);
        }

        .modal.active {
            display: flex;
        }

        /* Draggable/Movable Modal (Bookmarklet only) */
        .modal-movable {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 400px; /* Smaller for bookmarklets */
            height: 600px;
            max-width: 90%;
            max-height: 90%;
            background-color: black; 
            border: 5px solid var(--modal-border);
            border-radius: 10px;
            box-shadow: 0 0 30px rgba(0, 0, 0, 0.7);
            overflow: hidden;
            display: flex;
            flex-direction: column;
            cursor: default; /* Default cursor for content */
        }

        .modal-header {
            padding: 10px;
            background-color: var(--header-bg);
            color: var(--icon-hover-color);
            border-bottom: 1px solid var(--modal-border);
            cursor: move; /* Move cursor on header */
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .modal-title {
            font-weight: bold;
        }

        .close-button {
            font-size: 1.5em;
            color: var(--icon-hover-color);
            cursor: pointer;
            margin-left: 10px;
            transition: color 0.2s;
        }

        .close-button:hover {
            color: #ff4d4d;
        }

        /* Standard Game Modal */
        .modal-content-game {
            position: relative;
            width: 90%;
            height: 90%;
            max-width: 1200px;
            max-height: 800px;
            background-color: black;
            border: 5px solid var(--modal-border);
            border-radius: 10px;
            box-shadow: 0 0 30px rgba(0, 0, 0, 0.7);
            overflow: hidden;
        }

        .game-iframe {
            width: 100%;
            height: 100%;
            border: none;
            display: block; 
        }

        /* Content Areas for News/Settings/Links */
        .page-container {
            flex-grow: 1;
            padding: 20px;
            color: var(--text-color);
            overflow-y: auto;
            background-color: var(--grid-bg);
            text-align: left;
        }
        
        .page-container h3 {
            color: var(--modal-border);
            border-bottom: 2px solid var(--modal-border);
            padding-bottom: 10px;
            margin-top: 0;
            margin-bottom: 20px;
        }

        /* Bookmark Specific Styling */
        .bookmark-item {
            background-color: var(--button-bg);
            padding: 15px;
            margin-bottom: 15px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
            text-align: left;
        }

        .bookmark-link {
            display: block;
            color: #4CAF50; 
            font-family: monospace;
            word-wrap: break-word;
            text-decoration: none;
            cursor: copy;
            font-size: 0.8em;
            margin-top: 5px;
        }

        /* Settings Specific Styling */
        .setting-item {
            margin-bottom: 15px;
            padding: 10px;
            border-bottom: 1px solid var(--button-hover-bg);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .setting-item label {
            font-weight: bold;
        }

        /* Links Page Styling */
        .link-item a {
            color: #ff9800; /* Orange link color */
            text-decoration: none;
            word-wrap: break-word;
        }
        .link-item a:hover {
            text-decoration: underline;
        }

        /* Responsive Design */
        @media (max-width: 768px) {
            .title { font-size: 2.5em; }
            .game-grid { grid-template-columns: repeat(2, 1fr); gap: 15px; padding: 15px; }
            .game-icon { font-size: 2.5em; }
            .menu-icon { font-size: 3em; }
            .modal-movable { width: 95%; height: 90%; }
        }
    </style>
</head>
<body>

    <div class="stars"></div>

    <div class="container">
        <h1 class="title">selenite.</h1>
        <p class="subtitle">your gateway to the web.</p>

        <div class="game-grid">
            <a href="#" class="game-button" data-modal="gdLiteModal">
                <span class="game-icon">🔺</span>
                <span class="game-title">Geometry Dash Lite</span>
            </a>

            <a href="https://retrobowlgame.io/" target="_blank" class="game-button external">
                <span class="game-icon">🏈</span>
                <span class="game-title">Retro Bowl (Link)</span>
            </a>

            <a href="#" class="game-button" data-modal="2048Modal">
                <span class="game-icon">🔢</span>
                <span class="game-title">2048</span>
            </a>

            <a href="#" class="game-button" data-modal="gdSubzeroModal">
                <span class="game-icon">🧊</span>
                <span class="game-title">GD Subzero</span>
            </a>

            <a href="#" class="game-button" data-modal="amongUsModal">
                <span class="game-icon">👽</span>
                <span class="game-title">Among Us (Embed)</span>
            </a>
            
            <a href="#" class="game-button" data-modal="fnafModal">
                <span class="game-icon">🐻</span>
                <span class="game-title">FNAF 1 (Embed)</span>
            </a>
        </div>

        <div class="bottom-menu">
            <a href="#" class="menu-icon news" data-modal="newsModal"></a>
            <a href="#" class="menu-icon bookmark" data-modal="bookmarkletsModal"></a>
            <a href="#" class="menu-icon link" data-modal="linksModal"></a>
            <a href="#" class="menu-icon settings" data-modal="settingsModal"></a>
        </div>
    </div>

    <div id="gdLiteModal" class="modal">
        <div class="modal-content-game">
            <span class="close-button" onclick="closeModal('gdLiteModal')">&times;</span>
            <iframe class="game-iframe" src="https://selenite.cc/resources/semag/gdlite/index.html" allowfullscreen></iframe>
        </div>
    </div>

    <div id="2048Modal" class="modal">
        <div class="modal-content-game">
            <span class="close-button" onclick="closeModal('2048Modal')">&times;</span>
            <iframe class="game-iframe" src="https://selenite.cc/resources/semag/2048/index.html" allowfullscreen></iframe>
        </div>
    </div>

    <div id="gdSubzeroModal" class="modal">
        <div class="modal-content-game">
            <span class="close-button" onclick="closeModal('gdSubzeroModal')">&times;</span>
            <iframe class="game-iframe" src="https://geometry-lessons.github.io/geometry-dash-subzero.html" allowfullscreen></iframe>
        </div>
    </div>
    
    <div id="amongUsModal" class="modal">
        <div class="modal-content-game">
            <span class="close-button" onclick="closeModal('amongUsModal')">&times;</span>
            <iframe class="game-iframe" src="https://selenite.cc/resources/semag/amongus/index.html" allowfullscreen></iframe>
        </div>
    </div>
    
    <div id="fnafModal" class="modal">
        <div class="modal-content-game">
            <span class="close-button" onclick="closeModal('fnafModal')">&times;</span>
            <iframe class="game-iframe" src="https://selenite.cc/resources/semag/fnaf/index.html" allowfullscreen></iframe>
        </div>
    </div>


    <div id="bookmarkletsModal" class="modal">
        <div id="bookmarkletWindow" class="modal-movable">
            <div id="bookmarkletHeader" class="modal-header">
                <span class="modal-title">🔖 Bookmarklet Collection</span>
                <span class="close-button" onclick="closeModal('bookmarkletsModal')">&times;</span>
            </div>
            <div class="page-container">
                <p>Click the code below to **copy** it. Then, manually create a new bookmark in your browser and paste the code into the URL field.</p>
                
                <div class="bookmark-item">
                    <strong>Edit Any Page (Design Mode)</strong>
                    <code class="bookmark-link">javascript:(function(){document.body.contentEditable='true'; document.designMode='on'; void(0);})();</code>
                </div>

                <div class="bookmark-item">
                    <strong>Dev Console (Simple Alert)</strong>
                    <code class="bookmark-link">javascript:alert('Developer console access is typically browser-specific (F12). This is a simple cookie alert.');alert(document.cookie);</code>
                </div>

                <div class="bookmark-item">
                    <strong>Tampermonkey (Search Prompt)</strong>
                    <code class="bookmark-link">javascript:window.open('https://www.google.com/search?q=tampermonkey+bookmarklet+installer', '_blank');</code>
                </div>

                <div class="bookmark-item">
                    <strong>Mini Browser (iFrame)</strong>
                    <code class="bookmark-link">javascript:var i=document.createElement("iframe");i.src="https://www.google.com/";i.style.position="fixed";i.style.top="10px";i.style.right="10px";i.style.width="400px";i.style.height="300px";i.style.zIndex="9999";document.body.appendChild(i);void(0);</code>
                </div>

                <div class="bookmark-item">
                    <strong>Website Crash Simulation (Loop)</strong>
                    <p style="color: #FF5722;">⚠️ WARNING: This will intentionally hang the browser tab it is run on. Use with caution!</p>
                    <code class="bookmark-link">javascript:(function(){while(true){}})();</code>
                </div>
            </div>
        </div>
    </div>

    <div id="newsModal" class="modal">
        <div class="modal-content-game">
            <span class="close-button" onclick="closeModal('newsModal')">&times;</span>
            <div class="page-container">
                <h3>📰 Site News & Updates</h3>
                
                <div class="setting-item">
                    <span>**November 12, 2025:**</span>
                    <span>Site redesign completed! Added News, Settings, and Links pages. Bookmarklets are now draggable!</span>
                </div>
                
                <div class="setting-item">
                    <span>**Important Note:**</span>
                    <span>Games like Retro Bowl must be opened in a new tab because their owners block embedding.</span>
                </div>
            </div>
        </div>
    </div>
    
    <div id="settingsModal" class="modal">
        <div class="modal-content-game">
            <span class="close-button" onclick="closeModal('settingsModal')">&times;</span>
            <div class="page-container">
                <h3>⚙️ Website Settings</h3>
                
                <div class="setting-item">
                    <label for="particles">Enable Star Particles</label>
                    <input type="checkbox" id="particles" checked onchange="toggleParticles(this.checked)">
                </div>
                
                <div class="setting-item">
                    <label for="theme">Switch to Light Mode (Conceptual)</label>
                    <input type="checkbox" id="theme" onchange="alert('Light mode feature coming soon! (Conceptual only)');">
                </div>
                
                <div class="setting-item">
                    <label for="version">Current Version</label>
                    <span>1.1.0</span>
                </div>
            </div>
        </div>
    </div>
    
    <div id="linksModal" class="modal">
        <div class="modal-content-game">
            <span class="close-button" onclick="closeModal('linksModal')">&times;</span>
            <div class="page-container">
                <h3>🔗 "Unblocked" Links</h3>
                
                <div class="link-item">
                    <strong>999 Aura</strong>
                    <a href="https://999aura.carrd.co/" target="_blank">https://999aura.carrd.co/</a>
                </div>
                
                <div class="link-item" style="margin-top: 20px;">
                    <strong>Google Search</strong>
                    <a href="https://www.google.com/" target="_blank">https://www.google.com/</a>
                </div>
            </div>
        </div>
    </div>


    <script>
        // JavaScript for all dynamic functionality
        
        // ----------------------------------------------------------------------
        // 1. Particle Effect
        // ----------------------------------------------------------------------
        const starsContainer = document.querySelector('.stars');
        const numStars = 70; 

        for (let i = 0; i < numStars; i++) {
            const star = document.createElement('div');
            star.className = 'star';
            star.style.width = star.style.height = `${Math.random() * 3 + 1}px`;
            star.style.left = `${Math.random() * 100}%`;
            star.style.top = `${Math.random() * 100}%`;
            star.style.animationDuration = `${Math.random() * 5 + 3}s`;
            star.style.animationDelay = `${Math.random() * 5}s`;
            starsContainer.appendChild(star);
        }

        // ----------------------------------------------------------------------
        // 2. Modal Logic (Open/Close)
        // ----------------------------------------------------------------------
        function openModal(modalId) {
            document.getElementById(modalId).classList.add('active');
            document.body.style.overflow = 'hidden';
        }

        function closeModal(modalId) {
            document.getElementById(modalId).classList.remove('active');
            document.body.style.overflow = '';
        }

        // Attach event listeners to Game Buttons and Menu Icons
        document.querySelectorAll('.game-button').forEach(button => {
            button.addEventListener('click', function(e) {
                // If the button has an external link, let the browser handle it (open in new tab)
                if (this.classList.contains('external')) {
                    return; 
                }
                // Otherwise, open the modal
                e.preventDefault();
                const modalId = this.getAttribute('data-modal');
                if (modalId) {
                    openModal(modalId);
                }
            });
        });

        document.querySelectorAll('.menu-icon').forEach(icon => {
            icon.addEventListener('click', function(e) {
                e.preventDefault();
                const modalId = icon.getAttribute('data-modal');
                if (modalId) {
                    openModal(modalId);
                }
            });
        });
        
        // Settings Toggle
        function toggleParticles(enabled) {
            starsContainer.style.display = enabled ? 'block' : 'none';
        }

        // ----------------------------------------------------------------------
        // 3. Draggable Modal (Bookmarklet only)
        // ----------------------------------------------------------------------
        var dragItem = document.getElementById("bookmarkletWindow");
        var container = document.getElementById("bookmarkletHeader");

        var active = false;
        var currentX;
        var currentY;
        var initialX;
        var initialY;
        var xOffset = 0;
        var yOffset = 0;

        container.addEventListener("touchstart", dragStart, false);
        container.addEventListener("touchend", dragEnd, false);
        container.addEventListener("touchmove", drag, false);

        container.addEventListener("mousedown", dragStart, false);
        container.addEventListener("mouseup", dragEnd, false);
        container.addEventListener("mousemove", drag, false);

        function dragStart(e) {
            if (e.type === "touchstart") {
                initialX = e.touches[0].clientX - xOffset;
                initialY = e.touches[0].clientY - yOffset;
            } else {
                initialX = e.clientX - xOffset;
                initialY = e.clientY - yOffset;
            }

            if (e.target === container || e.target.closest('#bookmarkletHeader') === container) {
                active = true;
            }
        }

        function dragEnd(e) {
            initialX = currentX;
            initialY = currentY;

            active = false;
        }

        function drag(e) {
            if (active) {
                e.preventDefault();
                
                if (e.type === "touchmove") {
                    currentX = e.touches[0].clientX - initialX;
                    currentY = e.touches[0].clientY - initialY;
                } else {
                    currentX = e.clientX - initialX;
                    currentY = e.clientY - initialY;
                }

                xOffset = currentX;
                yOffset = currentY;

                setTranslate(currentX, currentY, dragItem);
            }
        }

        function setTranslate(xPos, yPos, el) {
            el.style.transform = "translate3d(" + xPos + "px, " + yPos + "px, 0)";
        }
        
        // ----------------------------------------------------------------------
        // 4. Bookmarklet Copy Functionality
        // ----------------------------------------------------------------------
        document.querySelectorAll('.bookmark-link').forEach(codeElement => {
            codeElement.addEventListener('click', function() {
                // Ensure the javascript: prefix is included for the user to copy
                let codeToCopy = this.textContent.trim();
                
                if (!codeToCopy.startsWith('javascript:')) {
                    codeToCopy = 'javascript:' + codeToCopy;
                }
                
                navigator.clipboard.writeText(codeToCopy).then(() => {
                    alert('Bookmarklet code copied! Now create a new bookmark and paste the code into the URL field.');
                }).catch(err => {
                    console.error('Could not copy text: ', err);
                    alert('Failed to copy code. Please manually select and copy the text.');
                });
            });
        });

    </script>
</body>
</html>
