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
            --update-color: #ff5722; /* Deep Orange for update button */
            --downgrade-color: #f44336; /* Red for downgrade button */
            --new-feature-color: #00bcd4; /* Cyan for new features */
            
            /* iOS Style Colors */
            --ios-bg: #1c1c1e;
            --ios-card-bg: #2c2c2e;
            --ios-separator: #48484a;
            --ios-blue: #0a84ff; 
            --ios-red: #ff3b30;
        }

        body {
            font-family: 'Roboto', sans-serif;
            margin: 0;
            padding: 0;
            min-height: 100vh; 
            overflow-x: hidden;
            overflow-y: auto; 
            
            background: linear-gradient(135deg, var(--primary-purple) 0%, var(--light-purple) 100%);
            color: var(--text-color);
            display: flex;
            flex-direction: column;
            align-items: center;
            position: relative;
        }

        /* Particle Effect */
        .stars {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            min-height: 100%;
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

        /* Update Button Styling */
        #update-button-container {
            position: fixed;
            top: 15px;
            right: 15px;
            z-index: 50;
        }

        #update-button {
            display: none;
            padding: 10px 15px;
            background-color: var(--update-color);
            color: white;
            border: none;
            border-radius: 5px;
            font-weight: 700;
            cursor: pointer;
            transition: background-color 0.3s, transform 0.3s;
            animation: pulse 1.5s infinite;
        }
        
        #update-button:hover {
            background-color: #ff8a65;
            transform: scale(1.05);
        }

        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 rgba(255, 87, 34, 0.7); }
            70% { box-shadow: 0 0 0 10px rgba(255, 87, 34, 0); }
            100% { box-shadow: 0 0 0 0 rgba(255, 87, 34, 0); }
        }

        /* Main Container */
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
            margin: 50px 0;
        }

        .title {
            font-size: 3em;
            font-weight: 700;
            margin-bottom: 5px;
            color: var(--text-color);
        }

        .subtitle {
            font-size: 1.2em;
            color: var(--icon-color);
            margin-bottom: 25px;
        }

        /* Top Menu Styling */
        .top-menu {
            display: flex;
            gap: 25px;
            justify-content: center;
            margin-bottom: 30px;
            padding-bottom: 10px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .menu-link {
            font-size: 1.1em;
            color: var(--icon-color);
            cursor: pointer;
            transition: color 0.3s, transform 0.3s;
            text-decoration: none;
            text-transform: uppercase;
            font-weight: 700;
        }

        .menu-link:hover {
            color: var(--icon-hover-color);
            transform: translateY(-2px);
        }

        /* Game Grid Styling */
        .game-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 20px;
            background-color: var(--grid-bg);
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 10px; 
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

        /* Modals (Game, Bookmarklet, Updates, Settings, Links) */
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
        
        /* Modal Content Styling */
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

        /* Update/Downgrade Simulation Modal (smaller, fixed size) */
        .modal-content-update {
            position: relative;
            width: 350px;
            padding: 30px;
            background-color: var(--grid-bg);
            border: 3px solid var(--update-color);
            border-radius: 10px;
            text-align: center;
        }

        .update-bar-container {
            width: 300px;
            height: 20px;
            background-color: #444;
            border-radius: 10px;
            margin-top: 15px;
            overflow: hidden;
        }
        
        #update-progress-bar {
            width: 0%;
            height: 100%;
            background-color: var(--update-color);
            transition: width 0.1s linear;
        }
        
        /* Change color for downgrade simulation */
        #updateProgressModal.downgrading .modal-content-update {
            border-color: var(--downgrade-color);
        }
        
        #updateProgressModal.downgrading #update-progress-bar {
            background-color: var(--downgrade-color);
        }

        .game-iframe {
            width: 100%;
            height: 100%;
            border: none;
            display: block; 
        }

        /* Content Areas for Updates/Settings/Links */
        .page-container {
            flex-grow: 1;
            padding: 20px;
            color: var(--text-color);
            overflow-y: auto;
            background-color: var(--grid-bg);
            text-align: left;
        }
        
        /* ---------------------------------------------------------------------- */
        /* iOS Update Page Style */
        /* ---------------------------------------------------------------------- */
        #updatesModal .modal-content-game {
            max-width: 600px;
            max-height: 90%;
            border: none;
            box-shadow: none;
            background-color: transparent;
        }
        
        #updatesModal .page-container {
            background-color: var(--ios-bg);
            padding: 0;
            border-radius: 10px;
            /* Ensure inner content is styled for iOS look */
        }
        
        .ios-header {
            padding: 20px;
            text-align: center;
        }
        
        .ios-header h2 {
            font-size: 2.5em;
            margin-bottom: 5px;
            color: white;
        }
        
        .ios-header p {
            color: var(--icon-color);
            font-size: 1.1em;
        }

        .ios-section {
            background-color: var(--ios-card-bg);
            margin: 15px 0;
            padding: 15px;
            border-radius: 10px;
        }
        
        .ios-section h3 {
            color: white;
            font-size: 1.2em;
            border-bottom: 1px solid var(--ios-separator);
            padding-bottom: 8px;
            margin-top: 0;
            margin-bottom: 15px;
        }
        
        .ios-changelog-item {
            padding: 10px 0;
            border-bottom: 1px solid var(--ios-separator);
        }
        
        .ios-changelog-item:last-child {
            border-bottom: none;
        }
        
        .ios-version-title {
            color: var(--ios-blue);
            font-weight: bold;
            display: block;
            margin-bottom: 5px;
        }
        
        .ios-description {
            color: var(--text-color);
            font-size: 0.95em;
        }

        #downgrade-in-updates {
            display: block;
            background-color: var(--ios-red);
            color: white;
            padding: 15px;
            border: none;
            border-radius: 8px;
            font-weight: 700;
            cursor: pointer;
            width: 90%;
            max-width: 400px;
            margin: 20px auto;
            transition: background-color 0.2s;
        }
        
        #downgrade-in-updates:hover {
            background-color: #cc0000;
        }
        /* ---------------------------------------------------------------------- */


        /* Draggable/Movable Modal (Bookmarklet only) */
        .modal-movable {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 400px;
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
            cursor: default;
        }

        .modal-header {
            padding: 10px;
            background-color: var(--header-bg);
            color: var(--icon-hover-color);
            border-bottom: 1px solid var(--modal-border);
            cursor: move;
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

        /* Settings Flooder Button */
        #history-flood-button {
            background-color: var(--new-feature-color);
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            transition: background-color 0.2s;
        }
        
        #history-flood-button:hover {
            background-color: #0097a7;
        }
        
        /* Links Page Styling */
        .link-item a {
            color: #ff9800;
            text-decoration: none;
            word-wrap: break-word;
        }
        .link-item a:hover {
            text-decoration: underline;
        }

        /* Responsive Design */
        @media (max-width: 768px) {
            .title { font-size: 2.5em; }
            .top-menu { gap: 15px; }
            .game-grid { grid-template-columns: repeat(2, 1fr); gap: 15px; padding: 15px; }
            .game-icon { font-size: 2.5em; }
            .modal-movable { width: 95%; height: 90%; }
            #update-button-container { top: 10px; right: 10px; }
            
            .ios-header h2 { font-size: 2em; }
        }
    </style>
</head>
<body>

    <div class="stars"></div>

    <div id="update-button-container">
        <button id="update-button" onclick="initiateUpdate()">
            Update to v1.3.0
        </button>
    </div>

    <div class="container">
        <h1 class="title">gamesunblocked67</h1>
        <p class="subtitle"></p>

        <div class="top-menu">
            <a href="#" class="menu-link" data-modal="updatesModal">Updates</a>
            <a href="#" class="menu-link" data-modal="bookmarkletsModal">Bookmarklets</a>
            <a href="#" class="menu-link" data-modal="linksModal">Links</a>
            <a href="#" class="menu-link" data-modal="settingsModal">Settings</a>
        </div>
        
        <div class="game-grid" id="game-grid">
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
    </div>

    <div id="updateProgressModal" class="modal">
        <div class="modal-content-update">
            <h3 id="progress-title">Downloading Update v1.3.0...</h3>
            <p id="update-message">Preparing files...</p>
            <div class="update-bar-container">
                <div id="update-progress-bar"></div>
            </div>
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
    <div id="clickerModal" class="modal">
        <div class="modal-content-game">
            <span class="close-button" onclick="closeModal('clickerModal')">&times;</span>
            <iframe class="game-iframe" src="https://selenite.cc/resources/semag/clicker/index.html" allowfullscreen></iframe>
        </div>
    </div>


    <div id="bookmarkletsModal" class="modal">
        <div id="bookmarkletWindow" class="modal-movable">
            <div id="bookmarkletHeader" class="modal-header">
                <span class="modal-title">🔖 Bookmarklet Collection</span>
                <span class="close-button" onclick="closeModal('bookmarkletsModal')">&times;</span>
            </div>
            <div class="page-container">
                <h3>Bookmarklets</h3>
                <p>Click the code below to **copy** it. Then, manually create a new bookmark in your browser and paste the code into the URL field.</p>
                
                <div class="bookmark-item">
                    <strong>Cloaker (Change Title/Icon) <span style="color: var(--new-feature-color);">(v1.3.0)</span></strong>
                    <code class="bookmark-link">javascript:document.title='About: Blank';var link=document.querySelector("link[rel*='icon']");if(link){link.href='data:image/x-icon;base64,AAABAAEAEBAAAAEACABoBQAAFgAAACgAAAAQAAAAIAAAAAEACAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA////AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=';}else{var link=document.createElement('link');link.rel='shortcut icon';link.href='data:image/x-icon;base64,AAABAAEAEBAAAAEACABoBQAAFgAAACgAAAAQAAAAIAAAAAEACAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA////AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=';document.getElementsByTagName('head')[0].appendChild(link);}void(0)</code>
                </div>

                <div class="bookmark-item">
                    <strong>History Flooder (Adds 100 fake entries) <span style="color: var(--new-feature-color);">(v1.3.0)</span></strong>
                    <code class="bookmark-link">javascript:for(var i=0;i<100;i++){history.pushState(0,0,'/fake-history-entry-'+i);}alert('100 fake history entries added. Your real history is now harder to find.');void(0)</code>
                </div>

                <div class="bookmark-item">
                    <strong>Edit Any Page (Design Mode)</strong>
                    <code class="bookmark-link">javascript:(function(){document.body.contentEditable='true'; document.designMode='on'; void(0);})();</code>
                </div>

                <div class="bookmark-item">
                    <strong>Dev Console (Simple Alert)</strong>
                    <code class="bookmark-link">javascript:alert('Developer console access is typically browser-specific (F12). This is a simple cookie alert.');alert(document.cookie);</code>
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

    <div id="updatesModal" class="modal">
        <div class="modal-content-game">
            <span class="close-button" onclick="closeModal('updatesModal')">&times;</span>
            <div class="page-container">
                <div class="ios-header">
                    <h2>Software Update</h2>
                    <p>gamesunblocked67</p>
                </div>
                
                <div class="ios-section">
                    <h3>Current Version</h3>
                    <div class="ios-changelog-item">
                        <span class="ios-version-title" id="ios-current-version">v1.3.0</span>
                        <span class="ios-description">The latest version provides new utility tools and fixes for the core framework.</span>
                    </div>
                </div>

                <div class="ios-section">
                    <h3>Version History</h3>
                    
                    <div class="ios-changelog-item">
                        <span class="ios-version-title">v1.3.0</span>
                        <span class="ios-description">**MAJOR!** Added History Flooder tool in settings. New Cloaker and History Flooder bookmarklets added. iOS-style updates page.</span>
                    </div>
                    
                    <div class="ios-changelog-item">
                        <span class="ios-version-title">v1.2.0</span>
                        <span class="ios-description">Added a Clicker Game. Implemented full update/downgrade system. Fixed scrolling and grid issues.</span>
                    </div>

                    <div class="ios-changelog-item" style="border-bottom: none;">
                        <span class="ios-version-title">v1.1.1 (Initial)</span>
                        <span class="ios-description">Title changed to "gamesunblocked67". Initial game library and base functions.</span>
                    </div>
                </div>
                
                <button id="downgrade-in-updates" onclick="initiateDowngrade()">
                    <span id="downgrade-text">Downgrade to v1.2.0</span>
                </button>

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
                    <label>History Flooder (Instant)</label>
                    <button id="history-flood-button" onclick="floodHistory(100)">
                        Flood History (100 Entries)
                    </button>
                </div>
                
                <div class="setting-item">
                    <label for="theme">Switch to Light Mode (Conceptual)</label>
                    <input type="checkbox" id="theme" onchange="alert('Light mode feature coming soon! (Conceptual only)');">
                </div>
                
                <div class="setting-item">
                    <label for="version">Current Version</label>
                    <span id="current-version-display">1.3.0</span>
                </div>
                
                <div class="setting-item" style="color: var(--update-color);">
                    <label>Update System Check</label>
                    <span>Target: 1.3.0</span>
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
        
        // --- CONFIGURATION & INITIALIZATION ---
        const CURRENT_VERSION = "1.3.0"; 
        const V120 = "1.2.0";
        const V111 = "1.1.1";
        const LOCAL_STORAGE_KEY = 'gamesunblocked_version';

        // 1. Check/Set Local Version
        let storedVersion = localStorage.getItem(LOCAL_STORAGE_KEY);
        if (!storedVersion) {
            // If first time loading this code base, start at v1.2.0 to prompt update to v1.3.0
            storedVersion = V120; 
            localStorage.setItem(LOCAL_STORAGE_KEY, storedVersion);
        }
        
        // Helper function for version comparison (handles X.Y.Z)
        function compareVersions(v1, v2) {
            const parts1 = v1.split('.').map(Number);
            const parts2 = v2.split('.').map(Number);
            for (let i = 0; i < Math.max(parts1.length, parts2.length); i++) {
                const num1 = parts1[i] || 0;
                const num2 = parts2[i] || 0;
                if (num1 > num2) return 1;
                if (num1 < num2) return -1;
            }
            return 0;
        }

        const isUpdateAvailable = (compareVersions(CURRENT_VERSION, storedVersion) === 1);
        
        // --- FEATURE LOGIC ---
        
        function floodHistory(count) {
            if (compareVersions(storedVersion, "1.3.0") < 0) {
                alert('History Flooder is only available in v1.3.0 and newer. Please update.');
                return;
            }
            
            for (let i = 0; i < count; i++) {
                history.pushState(null, '', `/temp-history-item-${i}`);
            }
            alert(`${count} fake history entries have been added. Your real history is now harder to find.`);
            
            // Push one final state to revert the current URL back to the base if desired
            history.pushState(null, '', window.location.pathname);
        }


        // --- UPDATE/DOWNGRADE SYSTEM LOGIC ---
        
        function runSimulation(targetVersion, isDowngrade) {
            const updateModal = document.getElementById('updateProgressModal');
            const progressBar = document.getElementById('update-progress-bar');
            const updateMessage = document.getElementById('update-message');
            const progressTitle = document.getElementById('progress-title');
            
            updateModal.classList.toggle('downgrading', isDowngrade);
            
            progressTitle.textContent = isDowngrade ? 
                `Reverting to v${targetVersion}...` : 
                `Downloading Update v${targetVersion}...`;
            
            // Reset progress bar
            progressBar.style.width = '0%';
            openModal('updateProgressModal');
            
            let progress = 0;
            const intervalTime = 100; 

            const updateInterval = setInterval(() => {
                progress += 5;
                progressBar.style.width = `${progress}%`;

                if (progress <= 30) {
                    updateMessage.textContent = isDowngrade ? 'Reversing changes...' : 'Downloading new files...';
                } else if (progress <= 70) {
                    updateMessage.textContent = isDowngrade ? 'Restoring previous version state...' : 'Installing core features...';
                } else if (progress <= 95) {
                    updateMessage.textContent = isDowngrade ? 'Cleaning up new version files...' : 'Finalizing configuration...';
                } else {
                    clearInterval(updateInterval);
                    const action = isDowngrade ? 'Downgrade' : 'Installation';
                    updateMessage.textContent = `${action} complete! Reloading...`;
                    
                    setTimeout(() => {
                        localStorage.setItem(LOCAL_STORAGE_KEY, targetVersion);
                        window.location.reload();
                    }, 1000);
                }
            }, intervalTime);
        }

        function initiateUpdate() {
            document.getElementById('update-button').style.display = 'none';
            runSimulation(CURRENT_VERSION, false);
        }
        
        function initiateDowngrade() {
             const currentStoredVersion = localStorage.getItem(LOCAL_STORAGE_KEY);
             let targetVersion;

             if (compareVersions(currentStoredVersion, CURRENT_VERSION) === 0) {
                // If on v1.3.0, downgrade to v1.2.0
                targetVersion = V120;
             } else if (compareVersions(currentStoredVersion, V120) === 0) {
                // If on v1.2.0, downgrade to v1.1.1
                targetVersion = V111;
             } else {
                 // Already at v1.1.1 or lower, stop
                 alert('Cannot downgrade further than v1.1.1.');
                 return;
             }
             
             document.getElementById('downgrade-in-updates').style.display = 'none';
             runSimulation(targetVersion, true);
        }


        function displayContentByVersion(version) {
            const gameGrid = document.getElementById('game-grid');
            
            // Game V1.2.0: Clicker Game
            const clickerHTML = `
                <a href="#" class="game-button" data-modal="clickerModal">
                    <span class="game-icon">💰</span>
                    <span class="game-title">Clicker Game</span>
                </a>
            `;
            const clickerExists = gameGrid.querySelector('[data-modal="clickerModal"]');

            if (compareVersions(version, V120) >= 0) {
                if (!clickerExists) gameGrid.insertAdjacentHTML('beforeend', clickerHTML);
            } else {
                if (clickerExists) clickerExists.remove();
            }
            
            // Update Downgrade Button State and Text
            const downgradeButton = document.getElementById('downgrade-in-updates');
            const downgradeText = document.getElementById('downgrade-text');
            
            if (downgradeButton) {
                 if (compareVersions(version, CURRENT_VERSION) === 0) {
                    // Current version is v1.3.0
                    downgradeButton.style.display = 'block';
                    downgradeText.textContent = `Downgrade to v${V120}`;
                    document.getElementById('ios-current-version').textContent = `v${CURRENT_VERSION}`;
                } else if (compareVersions(version, V120) === 0) {
                    // Current version is v1.2.0
                    downgradeButton.style.display = 'block';
                    downgradeText.textContent = `Downgrade to v${V111}`;
                    document.getElementById('ios-current-version').textContent = `v${V120}`;
                } else if (compareVersions(version, V111) === 0) {
                    // Current version is v1.1.1
                    downgradeButton.style.display = 'block';
                    downgradeText.textContent = `v${V111} (Cannot Downgrade Further)`;
                    downgradeButton.disabled = true;
                    downgradeButton.style.backgroundColor = '#48484a'; // Gray out
                    document.getElementById('ios-current-version').textContent = `v${V111}`;
                }
            }


            // Update the version display in settings
            document.getElementById('current-version-display').textContent = storedVersion;

            // Re-attach event listeners to all buttons
            attachEventListeners();
        }

        // --- CORE PAGE LOGIC ---

        // 1. Check for Update and Display Button
        if (isUpdateAvailable) {
            document.getElementById('update-button').style.display = 'block';
            document.getElementById('current-version-display').textContent = storedVersion;
        } else {
            // If up to date, display the content based on the stored version
            displayContentByVersion(storedVersion);
        }
        
        // 2. Random Subtitle Message
        const SUBTITLES = [
            "hi", 
            "6 7", 
            "your gateway to fun", 
            "sub to my channel (shahir2tuff) and make some of your own"
        ];
        const subtitleElement = document.querySelector('.subtitle');
        const randomIndex = Math.floor(Math.random() * SUBTITLES.length);
        subtitleElement.textContent = SUBTITLES[randomIndex];


        // 3. Particle Effect 
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

        // 4. Modal Logic (Open/Close)
        function openModal(modalId) {
            document.getElementById(modalId).classList.add('active');
            document.body.style.overflow = 'hidden';
        }

        function closeModal(modalId) {
            document.getElementById(modalId).classList.remove('active');
            document.body.style.overflow = 'auto';
        }

        // 5. Attach event listeners
        function attachEventListeners() {
            document.querySelectorAll('.game-button, .menu-link').forEach(button => {
                button.removeEventListener('click', handleButtonClick);
                button.addEventListener('click', handleButtonClick);
            });
            
            document.querySelectorAll('.bookmark-link').forEach(codeElement => {
                codeElement.removeEventListener('click', handleBookmarkCopy);
                codeElement.addEventListener('click', handleBookmarkCopy);
            });
        }
        
        function handleButtonClick(e) {
            if (this.classList.contains('external')) {
                return; 
            }
            e.preventDefault();
            const modalId = this.getAttribute('data-modal');
            if (modalId) {
                openModal(modalId);
            }
        }
        
        function handleBookmarkCopy() {
            let codeToCopy = this.textContent.trim();
                
            if (!codeToCopy.startsWith('javascript:')) {
                codeToCopy = 'javascript:' + codeToCopy;
            }
            
            navigator.clipboard.writeText(codeToCopy).then(() => {
                const notification = document.createElement('div');
                notification.style.cssText = `
                    position: fixed; top: 20px; right: 20px; background-color: #4CAF50; 
                    color: white; padding: 10px 20px; border-radius: 8px; z-index: 1000;
                    box-shadow: 0 4px 6px rgba(0,0,0,0.1); transition: opacity 0.5s;
                `;
                notification.textContent = 'Code Copied!';
                document.body.appendChild(notification);
                setTimeout(() => {
                    notification.style.opacity = '0';
                    setTimeout(() => document.body.removeChild(notification), 500);
                }, 2000);
                
            }).catch(err => {
                console.error('Could not copy text: ', err);
                const message = 'Failed to copy code. Please manually select and copy the text.';
                console.log(message);
            });
        }
        
        // Initial call to attach listeners
        attachEventListeners();


        // 6. Settings Toggle
        function toggleParticles(enabled) {
            starsContainer.style.display = enabled ? 'block' : 'none';
        }

        // 7. Draggable Modal (Bookmarklet only)
        var dragItem = document.getElementById("bookmarkletWindow");
        var container = document.getElementById("bookmarkletHeader");

        var active = false;
        var currentX;
        var currentY;
        var initialX;
        var initialY;
        var xOffset = 0;
        var yOffset = 0;

        if (dragItem && container) {
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
        }
        
    </script>
</body>
</html>
