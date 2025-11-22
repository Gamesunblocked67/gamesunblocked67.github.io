<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI-Powered Study Hub</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Load Lucide Icons for aesthetic components -->
    <script src="https://unpkg.com/lucide@latest"></script>
    
    <!-- Load MathJax for LaTeX/Mathematical rendering -->
    <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
    <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

    <!-- Configure Font -->
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6;
        }
        /* Styles for 3D flip effect */
        .card-container {
            perspective: 1000px;
        }
        .card-inner {
            transition: transform 0.6s;
            transform-style: preserve-3d;
        }
        .flipped .card-inner {
            transform: rotateY(180deg);
        }
        .card-front, .card-back {
            backface-visibility: hidden;
            position: absolute;
            width: 100%;
            height: 100%;
        }
        .card-back {
            transform: rotateY(180deg);
        }
        .sidebar-menu {
            transition: transform 0.3s ease-in-out;
        }
        /* For markdown rendering, especially for the Study Guide */
        .markdown-content h1 { font-size: 2.25rem; font-weight: 800; margin-top: 1.5rem; margin-bottom: 0.75rem; border-bottom: 2px solid #e5e7eb; padding-bottom: 0.5rem; }
        .markdown-content h2 { font-size: 1.875rem; font-weight: 700; margin-top: 1.5rem; margin-bottom: 0.75rem; color: #4338ca; }
        .markdown-content h3 { font-size: 1.5rem; font-weight: 600; margin-top: 1rem; margin-bottom: 0.5rem; }
        .markdown-content ul { list-style-type: disc; margin-left: 1.5rem; padding-left: 0.5rem; }
        .markdown-content ol { list-style-type: decimal; margin-left: 1.5rem; padding-left: 0.5rem; }
        .markdown-content p, .markdown-content li { margin-bottom: 0.5rem; line-height: 1.6; }
        .markdown-content strong { font-weight: 700; color: #1f2937; }
        .markdown-content pre { background-color: #f4f4f5; padding: 1rem; border-radius: 0.5rem; overflow-x: auto; margin: 1rem 0; }
        /* Style for displayed MathJax equations */
        .markdown-content .MJX-Container { 
            margin: 1rem 0;
            padding: 0.75rem;
            background-color: #f1f5f9; /* Slate 100 */
            border-left: 4px solid #6366f1; /* Indigo 500 */
            border-radius: 0.5rem;
        }
    </style>
</head>
<body class="min-h-screen flex">

    <!-- Sidebar / Dropdown Menu (Now Scrollable) -->
    <div id="sidebar" class="sidebar-menu fixed top-0 left-0 h-full w-64 bg-indigo-800 text-white p-6 z-40 shadow-2xl transform -translate-x-full md:translate-x-0 overflow-y-auto">
        <div class="flex justify-between items-center mb-10">
            <h1 class="text-2xl font-bold">AI Study Hub</h1>
            <button onclick="toggleSidebar()" class="md:hidden p-2">
                <i data-lucide="x" class="w-6 h-6"></i>
            </button>
        </div>

        <!-- User Status (Login/Sign Up Placeholder) -->
        <div class="mb-8 p-3 bg-indigo-700 rounded-lg shadow-inner">
            <div class="flex items-center mb-2">
                <i data-lucide="user" class="w-5 h-5 mr-3"></i>
                <span class="font-semibold">User ID</span>
            </div>
            <p id="user-id-display" class="text-sm break-all font-mono text-gray-200">Authenticating...</p>
        </div>

        <!-- Navigation Buttons -->
        <nav class="space-y-2 pt-4 border-t border-indigo-700">
            <h3 class="text-lg font-semibold mb-2">1. Select Tool & Enter Prompt</h3>
            
            <button onclick="setView('ai_quiz')" class="w-full flex items-center p-3 rounded-lg text-left font-medium bg-indigo-600 hover:bg-indigo-700 transition duration-150">
                <i data-lucide="check-circle" class="w-6 h-6 mr-3"></i> AI Quiz Generator
            </button>
            <button onclick="setView('ai_flashcards')" class="w-full flex items-center p-3 rounded-lg text-left font-medium bg-indigo-600 hover:bg-indigo-700 transition duration-150">
                <i data-lucide="rotate-cw" class="w-6 h-6 mr-3"></i> AI Flashcards
            </button>
            <button onclick="setView('ai_guide')" class="w-full flex items-center p-3 rounded-lg text-left font-medium bg-indigo-600 hover:bg-indigo-700 transition duration-150">
                <i data-lucide="book-open" class="w-6 h-6 mr-3"></i> AI Study Guide Maker
            </button>
        </nav>

        <!-- Other Views -->
        <nav class="space-y-2 pt-4 mt-4 border-t border-indigo-700">
            <h3 class="text-lg font-semibold mb-2">Review</h3>
            <button onclick="setView('dashboard')" class="w-full flex items-center p-3 rounded-lg text-left font-medium hover:bg-indigo-700 transition duration-150">
                <i data-lucide="bar-chart-2" class="w-6 h-6 mr-3"></i> Dashboard & Saved Items
            </button>
        </nav>
    </div>

    <!-- Content Area -->
    <div id="content-wrapper" class="flex-1 transition-all duration-300 md:ml-64 p-4 sm:p-8">
        <!-- Header/Toggle Button for Mobile -->
        <div class="md:hidden mb-6 flex items-center justify-between">
            <button onclick="toggleSidebar()" class="p-3 bg-indigo-600 text-white rounded-lg shadow-md hover:bg-indigo-700">
                <i data-lucide="menu" class="w-6 h-6"></i>
            </button>
            <h2 class="text-2xl font-extrabold text-gray-900">AI Study Hub</h2>
        </div>

        <div class="pb-8">
            <h2 id="view-title" class="text-4xl font-extrabold text-gray-900">Welcome to the AI Study Hub</h2>
            <p id="view-subtitle" class="text-gray-500 mt-1">Select a tool from the left menu to begin your prompt-driven study session.</p>
        </div>

        <!-- Main View Container -->
        <main id="app-content" class="w-full">
            <div class="flex items-center justify-center h-full text-xl text-gray-500 min-h-[50vh]">
                <svg class="animate-spin -ml-1 mr-3 h-8 w-8 text-indigo-500" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                  <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                  <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                </svg>
                Loading application and authenticating...
            </div>
        </main>
    </div>

    <!-- Custom Modal for Alerts/Confirms -->
    <div id="custom-modal" class="fixed inset-0 bg-black bg-opacity-50 z-50 hidden items-center justify-center">
        <div class="bg-white p-6 rounded-xl shadow-2xl max-w-sm w-full space-y-4">
            <h3 id="modal-title" class="text-xl font-bold text-gray-900">Notification</h3>
            <p id="modal-message" class="text-gray-700">Message content.</p>
            <div class="flex justify-end space-x-3">
                <button id="modal-ok" class="px-4 py-2 bg-indigo-600 text-white rounded-lg hover:bg-indigo-700 transition">OK</button>
            </div>
        </div>
    </div>


    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, onSnapshot, collection, query, addDoc, updateDoc, serverTimestamp, setLogLevel, getDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // --- GLOBAL STATE & CONFIG ---
        const API_URL_BASE = 'https://generativelanguage.googleapis.com/v1beta/models';
        const MODEL_NAME = 'gemini-2.5-flash-preview-09-2025';
        const apiKey = ""; // Leave as-is for Canvas environment

        let db;
        let auth;
        let userId = null;
        let isAuthReady = false;
        let activeView = 'welcome'; 

        // AI Generation States
        let currentPrompt = localStorage.getItem('currentPrompt') || "";
        let currentTopic = localStorage.getItem('currentTopic') || "";
        let currentCount = localStorage.getItem('currentCount') || 5; 
        let generatedContent = null; 
        let isGenerating = false;
        let activeTool = null; 

        // Study Data States (Synced with Firestore)
        let weakAreas = {};
        let totalScore = 0;
        let totalQuizzes = 0;
        let savedItems = [];

        // --- PROGRESS/SESSION STATE (New) ---
        let quizSessionId = null; 
        let isQuizSessionLoading = false;
        let currentQuestionIndex = 0;
        let quizScore = 0;
        let showQuizResult = false;
        let selectedOption = null;
        let isAnswerChecked = false;
        
        let flashcardSessionId = null;
        let isFlashcardSessionLoading = false;
        let currentCardIndex = 0;
        let isFlipped = false;


        // Fallback UUID for non-authenticated/non-Firebase scenarios
        const generateUUID = () => {
            return crypto.randomUUID();
        };

        // Global variables provided by the environment
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {};
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

        const getUserBasePath = (uid) => `/artifacts/${appId}/users/${uid}`;
        
        // Firestore Path Constants
        const QUIZ_SESSION_DOC = 'quiz_session';
        const FLASHCARD_SESSION_DOC = 'flashcard_session';
        const WEAK_AREAS_DOC = 'weak_areas_summary';
        const SAVED_ITEMS_COL = 'saved_items';


        // --- UTILITY FUNCTIONS ---

        const showModal = (title, message) => {
            document.getElementById('modal-title').textContent = title;
            document.getElementById('modal-message').textContent = message;
            document.getElementById('custom-modal').classList.remove('hidden');
            document.getElementById('custom-modal').classList.add('flex');
            document.getElementById('modal-ok').onclick = () => {
                document.getElementById('custom-modal').classList.add('hidden');
                document.getElementById('custom-modal').classList.remove('flex');
            };
        };

        const markdownToHtml = (markdown) => {
            // Simple markdown parser for headings, bold, and lists.
            let html = markdown
                .replace(/^### (.*$)/gim, '<h3>$1</h3>') // H3
                .replace(/^## (.*$)/gim, '<h2>$1</h2>') // H2
                .replace(/^# (.*$)/gim, '<h1>$1</h1>') // H1
                .replace(/\*\*(.*?)\*\*/gim, '<strong>$1</strong>') // Bold
                
                // Handle lists: convert new lines starting with * or 1. to <li>
                .replace(/\n\* (.*)/gim, '\n<li>$1</li>') 
                .replace(/<\/li>\n<li>/gim, '</li><li>') // Merge consecutive list items
                .replace(/(\n<li>[\s\S]*?<\/li>)/g, '<ul>$1</ul>') // Wrap in <ul>

                .replace(/\n\d+\. (.*)/gim, '\n<li>$1</li>')
                .replace(/<\/li>\n<li>/gim, '</li><li>') 
                .replace(/(\n<li>[\s\S]*?<\/li>)/g, '<ol>$1</ol>') // Wrap in <ol>
                
                // Clean up list wrappers that end up adjacent
                .replace(/<\/ul><ul>/gim, '') 
                .replace(/<\/ol><ol>/gim, '')

                // Convert double newlines (paragraph breaks) to HTML paragraphs
                .replace(/\n\n/g, '</p><p>')
                .replace(/\n/g, '<br>') // Single newline is a soft break
                .replace(/<br><br>/g, '<p>') // Convert double soft breaks back to one paragraph break
                .replace(/^/,'<p>') // Wrap start
                .replace(/$/,'</p>') // Wrap end
                .replace(/<p><\/p>/g, ''); // Remove empty paragraphs

            return `<div class="markdown-content">${html}</div>`;
        };
        
        const parseQuizMarkdown = (markdown) => {
            // ... (Parsing logic remains the same) ...
            const questions = [];
            const questionSections = markdown.split(/##\s*Question\s*\d+[:.]\s*/).filter(s => s.trim() !== '');

            questionSections.forEach(section => {
                try {
                    const lines = section.trim().split('\n').map(line => line.trim()).filter(line => line.length > 0);
                    
                    if (lines.length < 6) return;

                    const questionText = lines[0].trim();
                    const optionLines = lines.slice(1, 5).filter(line => /^[A-D]\.\s*/i.test(line));
                    const options = optionLines.map(opt => opt.replace(/^[A-D]\.\s*/, '').trim());
                    
                    if (options.length !== 4) return;

                    const answerLine = lines.find(line => line.startsWith('**Answer:**'));
                    const topicLine = lines.find(line => line.startsWith('**Topic:**'));

                    let answer = '';
                    if (answerLine) {
                        let answerText = answerLine.replace('**Answer:**', '').trim();
                        const matchLetter = answerText.match(/^[A-D]\.\s*(.*)/i);
                        answer = matchLetter ? matchLetter[1].trim() : answerText;
                        
                        if (answer.length === 1 && /^[A-D]$/i.test(answer)) {
                            const answerLetter = answer.toUpperCase();
                            const optionIndex = answerLetter.charCodeAt(0) - 'A'.charCodeAt(0);
                            answer = options[optionIndex] || `ERROR: Letter ${answerLetter} not found`;
                        }
                    }
                    
                    const topic = topicLine ? topicLine.replace('**Topic:**', '').trim() : 'General';
                    
                    if (questionText && options.length === 4 && answer && !answer.startsWith('ERROR:')) {
                        questions.push({ question: questionText, options, answer, topic });
                    }

                } catch (e) {
                    console.error("Error parsing question section:", section, e);
                }
            });

            return questions;
        };

        const parseFlashcardMarkdown = (markdown) => {
            // ... (Parsing logic remains the same) ...
            const flashcards = [];
            const cardSections = markdown.split('---').filter(s => s.trim() !== '');

            cardSections.forEach(section => {
                try {
                    const termMatch = section.match(/\*\*TERM:\*\*\s*([\s\S]*?)(?=\*\*DEFINITION:|\s*$)/i);
                    const definitionMatch = section.match(/\*\*DEFINITION:\*\*\s*([\s\S]*?)(?=\*\*TOPIC:|\s*$)/i);
                    const topicMatch = section.match(/\*\*TOPIC:\*\*\s*([\s\S]*)/i);

                    const term = termMatch ? termMatch[1].trim() : '';
                    const definition = definitionMatch ? definitionMatch[1].trim() : '';
                    const topic = topicMatch ? topicMatch[1].trim() : 'General';

                    if (term && definition) {
                        flashcards.push({ term, definition, topic });
                    }

                } catch (e) {
                    console.error("Error parsing flashcard section:", section, e);
                }
            });

            return flashcards.filter(card => card.term && card.definition && card.term !== 'N/A' && card.definition !== 'N/A');
        };

        // --- GEMINI API CALLER ---

        const callGeminiAPI = async (userQuery, systemPrompt) => {
            const apiUrl = `${API_URL_BASE}/${MODEL_NAME}:generateContent?key=${apiKey}`;
            const headers = { 'Content-Type': 'application/json' };

            const payload = {
                contents: [{ parts: [{ text: userQuery }] }],
                systemInstruction: { parts: [{ text: systemPrompt }] },
                tools: [{ "google_search": {} }],
            };

            const maxRetries = 5;
            let lastError = null;

            for (let attempt = 0; attempt < maxRetries; attempt++) {
                try {
                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: headers,
                        body: JSON.stringify(payload)
                    });

                    if (response.ok) {
                        const result = await response.json();
                        const candidate = result.candidates?.[0];

                        if (candidate && candidate.content?.parts?.[0]?.text) {
                            return candidate.content.parts[0].text;
                        } else {
                            throw new Error("API response content was empty or malformed.");
                        }
                    } else if (response.status === 429) {
                        const delay = Math.pow(2, attempt) * 1000 + Math.random() * 1000;
                        lastError = `Rate limit exceeded. Retrying in ${Math.ceil(delay/1000)}s.`;
                        await new Promise(resolve => setTimeout(resolve, delay));
                        continue;
                    } else {
                        const errorBody = await response.json();
                        throw new Error(`API Error: ${response.status} - ${errorBody.error.message}`);
                    }
                } catch (error) {
                    lastError = error;
                    if (attempt === maxRetries - 1) {
                         throw lastError;
                    }
                    const delay = Math.pow(2, attempt) * 1000 + Math.random() * 500;
                    await new Promise(resolve => setTimeout(resolve, delay));
                }
            }
             throw new Error("API failed after all retries.");
        };

        // --- PROGRESS & SESSION MANAGEMENT (New & Updated) ---
        
        const saveQuizProgress = async () => {
             if (!db || !userId || !quizSessionId) return;

             const quizRef = doc(db, getUserBasePath(userId), QUIZ_SESSION_DOC);
             try {
                 await setDoc(quizRef, {
                    sessionId: quizSessionId,
                    type: 'quiz',
                    topic: currentTopic,
                    fullContent: JSON.stringify(generatedContent), // Store array as JSON string
                    currentQuestionIndex: currentQuestionIndex,
                    quizScore: quizScore,
                    selectedOption: selectedOption,
                    isAnswerChecked: isAnswerChecked,
                    updatedAt: serverTimestamp()
                 }, { merge: true });
             } catch (e) {
                 console.error("Error saving quiz progress:", e);
             }
        };
        
        const loadActiveQuizSession = async () => {
            if (!db || !userId) return;
            isQuizSessionLoading = true;

            try {
                const quizRef = doc(db, getUserBasePath(userId), QUIZ_SESSION_DOC);
                const docSnap = await getDoc(quizRef);

                if (docSnap.exists() && docSnap.data().fullContent) {
                    const data = docSnap.data();
                    
                    quizSessionId = data.sessionId;
                    currentTopic = data.topic;
                    generatedContent = JSON.parse(data.fullContent); // Parse back to array
                    currentCount = generatedContent.length;
                    
                    // Restore state
                    currentQuestionIndex = data.currentQuestionIndex || 0;
                    quizScore = data.quizScore || 0;
                    selectedOption = data.selectedOption || null;
                    isAnswerChecked = data.isAnswerChecked || false;
                    showQuizResult = false; // Reset result display on load
                    
                    activeTool = 'ai_quiz';

                    console.log(`Quiz session loaded for ${currentTopic}. Resuming at Q${currentQuestionIndex + 1}.`);
                } else {
                    quizSessionId = null;
                    generatedContent = null;
                    activeTool = null;
                }
            } catch (e) {
                console.error("Error loading quiz session:", e);
                quizSessionId = null;
                generatedContent = null;
                activeTool = null;
            } finally {
                isQuizSessionLoading = false;
                updateUI();
            }
        };

        const saveFlashcardProgress = async () => {
             if (!db || !userId || !flashcardSessionId) return;

             const cardRef = doc(db, getUserBasePath(userId), FLASHCARD_SESSION_DOC);
             try {
                 await setDoc(cardRef, {
                    sessionId: flashcardSessionId,
                    type: 'flashcards',
                    topic: currentPrompt,
                    fullContent: JSON.stringify(generatedContent), // Store array as JSON string
                    currentCardIndex: currentCardIndex,
                    isFlipped: isFlipped,
                    updatedAt: serverTimestamp()
                 }, { merge: true });
             } catch (e) {
                 console.error("Error saving flashcard progress:", e);
             }
        };
        
        const loadActiveFlashcardSession = async () => {
            if (!db || !userId) return;
            isFlashcardSessionLoading = true;

            try {
                const cardRef = doc(db, getUserBasePath(userId), FLASHCARD_SESSION_DOC);
                const docSnap = await getDoc(cardRef);

                if (docSnap.exists() && docSnap.data().fullContent) {
                    const data = docSnap.data();
                    
                    flashcardSessionId = data.sessionId;
                    currentPrompt = data.topic;
                    generatedContent = JSON.parse(data.fullContent); // Parse back to array
                    
                    // Restore state
                    currentCardIndex = data.currentCardIndex || 0;
                    isFlipped = data.isFlipped || false;
                    
                    activeTool = 'ai_flashcards';
                    console.log(`Flashcard session loaded. Resuming at card ${currentCardIndex + 1}.`);
                } else {
                    flashcardSessionId = null;
                    generatedContent = null;
                    activeTool = null;
                }
            } catch (e) {
                console.error("Error loading flashcard session:", e);
                flashcardSessionId = null;
                generatedContent = null;
                activeTool = null;
            } finally {
                isFlashcardSessionLoading = false;
                updateUI();
            }
        };


        // --- GENERATION HANDLER (Updated to initialize new sessions) ---

        window.generateContent = async (tool) => {
            if (isGenerating) return;

            const promptInput = document.getElementById(`prompt-input-${tool}`);
            let prompt = promptInput?.value.trim();

            if (!prompt) {
                showModal('Input Required', 'Please enter a prompt describing the content you want to generate.');
                return;
            }
            
            currentPrompt = prompt;
            localStorage.setItem('currentPrompt', currentPrompt);

            isGenerating = true;
            setView(tool, true); 

            try {
                let systemPrompt = "You are a world-class educational tool. Your goal is to generate study content based on the user's provided prompt. You must use the Google Search tool to first gather information on the topic before generating the final structured output. For any mathematical content, use LaTeX syntax enclosed in $...$ for inline formulas and $$...$$ for display formulas (e.g., $E=mc^2$). ONLY output the requested format and nothing else. DO NOT include any preamble or postamble text.";
                let userQuery = `First, research the topic. Then, based on the research, `;
                let rawText = null;
                let parsedResult = null;
                
                if (tool === 'ai_quiz') {
                    let countMatch = prompt.match(/^(\d+)\s+(.+)/);
                    if (countMatch && parseInt(countMatch[1], 10) >= 1 && parseInt(countMatch[1], 10) <= 15) {
                        currentCount = parseInt(countMatch[1], 10);
                        currentTopic = countMatch[2].trim();
                    } else {
                        currentCount = 5; 
                        currentTopic = prompt;
                    }
                    if (currentCount > 15 || currentCount < 1) currentCount = 5;
                    localStorage.setItem('currentTopic', currentTopic);
                    localStorage.setItem('currentCount', currentCount);
                    
                    systemPrompt += ` Generate exactly ${currentCount} multiple-choice questions. Format each question strictly as follows, using Markdown, and ONLY output the questions and answers:
                    ## Question 1: [Question Text]
                    A. [Option A]
                    B. [Option B]
                    C. [Option C]
                    D. [Option D]
                    **Answer:** [Full text of the correct option]
                    **Topic:** [Concise Sub-Topic]
                    Repeat the structure for Question 2, Question 3, etc.`;
                    userQuery += `generate exactly ${currentCount} multiple-choice questions about: "${currentTopic}"`;
                    
                    rawText = await callGeminiAPI(userQuery, systemPrompt);
                    parsedResult = parseQuizMarkdown(rawText);

                    if (parsedResult.length === 0) {
                        throw new Error("Failed to parse any questions. Check the raw output for unexpected formatting.");
                    }
                    
                    // INITIALIZE NEW QUIZ SESSION (Crucial for persistence)
                    quizSessionId = generateUUID();
                    currentQuestionIndex = 0;
                    quizScore = 0;
                    selectedOption = null;
                    isAnswerChecked = false;
                    await saveQuizProgress(); // Save initial state
                    
                } else if (tool === 'ai_flashcards') {
                    currentTopic = prompt; 
                    currentCount = 10;
                    systemPrompt += " Generate 10 key terms and their concise, clear definitions. Format each flashcard strictly as follows, using Markdown, and ONLY output this format:\n---\n**TERM:** [The Key Term]\n**DEFINITION:** [The Definition]\n**TOPIC:** [Concise Sub-Topic]\n---\nRepeat the structure for all 10 cards.";
                    userQuery += `generate 10 flashcards (term and definition) for the prompt: "${currentPrompt}"`;
                    
                    rawText = await callGeminiAPI(userQuery, systemPrompt);
                    parsedResult = parseFlashcardMarkdown(rawText);

                    if (parsedResult.length === 0) {
                        throw new Error("Failed to parse any flashcards. Check the raw output for unexpected formatting.");
                    }
                    
                    // INITIALIZE NEW FLASHCARD SESSION
                    flashcardSessionId = generateUUID();
                    currentCardIndex = 0;
                    isFlipped = false;
                    await saveFlashcardProgress(); // Save initial state

                } else if (tool === 'ai_guide') {
                    systemPrompt += " Create a comprehensive, well-structured, and easy-to-read study guide that fully addresses the user's prompt. Use clean, clear Markdown formatting including headings (#, ##, ###), bold text (**), and lists (-, 1.). If the guide includes formulas or math symbols, use LaTeX syntax enclosed in $...$ or $$...$$";
                    userQuery += `create a detailed study guide for the prompt: "${currentPrompt}"`;
                    
                    parsedResult = await callGeminiAPI(userQuery, systemPrompt);
                }

                generatedContent = parsedResult;
                activeTool = tool;

            } catch (error) {
                console.error(`AI Generation failed for ${tool}:`, error);
                let message = error.message;
                if (rawText) {
                    message += `\n\n(Raw output start: ${rawText.substring(0, 150)}...)`;
                }
                showModal('Generation/Parsing Error', `The AI tool encountered an error: ${message}. The model may have failed to follow the required format. Please try again with a simpler prompt.`);
                generatedContent = null;
                activeTool = null;
                quizSessionId = null;
                flashcardSessionId = null;
            } finally {
                isGenerating = false;
                updateUI();
            }
        };


        // --- AUTHENTICATION & INITIALIZATION ---

        const initFirebase = async () => {
            try {
                if (Object.keys(firebaseConfig).length > 0) {
                    setLogLevel('debug');
                    const app = initializeApp(firebaseConfig);
                    db = getFirestore(app);
                    auth = getAuth(app);
                } else {
                    console.error("Firebase config not found. Running in mock mode.");
                    userId = generateUUID();
                    isAuthReady = true;
                    updateUI();
                    return;
                }
            } catch (e) {
                console.error("Firebase initialization failed:", e);
                userId = generateUUID();
                isAuthReady = true;
                updateUI();
                return;
            }

            const performAuth = async () => {
                try {
                    if (initialAuthToken) {
                        await signInWithCustomToken(auth, initialAuthToken);
                    } else {
                        await signInAnonymously(auth);
                    }
                } catch (error) {
                    console.error("Authentication failed, trying anonymous sign-in fallback:", error);
                    try {
                         await signInAnonymously(auth);
                    } catch(anonError) {
                         console.error("Anonymous sign-in failed:", anonError);
                         userId = generateUUID();
                    }
                }
            };

            onAuthStateChanged(auth, (user) => {
                if (user) {
                    userId = user.uid;
                } else if (auth.currentUser) {
                    userId = auth.currentUser.uid;
                } else {
                    performAuth();
                }

                isAuthReady = true;
                if (userId) {
                    setupFirestoreListeners(userId);
                }
                updateUI();
            });

            if (!auth.currentUser) {
                performAuth();
            }
        };

        const setupFirestoreListeners = (uid) => {
            if (!db || !uid) return;

            const userBasePath = getUserBasePath(uid);

            // 1. Dashboard Metrics Listener
            const weakAreasRef = doc(db, userBasePath, 'study_data', WEAK_AREAS_DOC);
            onSnapshot(weakAreasRef, (docSnap) => {
                if (docSnap.exists()) {
                    const data = docSnap.data();
                    weakAreas = data.topics || {};
                    totalScore = data.totalScore || 0;
                    totalQuizzes = data.totalQuizzes || 0;
                } else {
                    setDoc(weakAreasRef, { topics: {}, totalScore: 0, totalQuizzes: 0, initializedAt: serverTimestamp() }, { merge: true })
                        .catch(e => console.error("Error initializing weak areas doc:", e));
                }
                if (activeView === 'dashboard') updateUI();
            }, (error) => console.error("Error fetching weak areas:", error));

            // 2. Saved Items Listener
            const savedItemsColRef = collection(db, userBasePath, 'study_data', SAVED_ITEMS_COL);
            onSnapshot(savedItemsColRef, (snapshot) => {
                savedItems = snapshot.docs.map(doc => ({
                    firestoreId: doc.id,
                    ...doc.data()
                }));
                if (activeView === 'dashboard') updateUI();
            }, (error) => console.error("Error fetching saved items:", error));
        };

        // --- UI UTILITIES ---

        let isSidebarOpen = false;

        window.toggleSidebar = () => {
            isSidebarOpen = !isSidebarOpen;
            const sidebar = document.getElementById('sidebar');

            if (isSidebarOpen) {
                sidebar.classList.remove('-translate-x-full');
            } else {
                sidebar.classList.add('-translate-x-full');
            }
            if (window.innerWidth < 768) {
                 const overlay = document.getElementById('sidebar-overlay');
                 if (isSidebarOpen && !overlay) {
                     const newOverlay = document.createElement('div');
                     newOverlay.id = 'sidebar-overlay';
                     newOverlay.className = 'fixed inset-0 bg-black bg-opacity-50 z-30 transition-opacity';
                     newOverlay.onclick = toggleSidebar;
                     document.body.appendChild(newOverlay);
                 } else if (!isSidebarOpen && overlay) {
                     overlay.remove();
                 }
            }
        };

        window.setView = (view, isGeneratingState = false) => {
            // Check if switching to quiz or flashcards when content is not yet generated
            if (view === 'ai_quiz' && !isGeneratingState && !generatedContent) {
                loadActiveQuizSession();
            } else if (view === 'ai_flashcards' && !isGeneratingState && !generatedContent) {
                loadActiveFlashcardSession();
            } else if (view !== activeView && view !== 'dashboard' && view !== 'welcome' && !isGeneratingState) {
                // Clear state if switching tools or starting fresh
                generatedContent = null;
                activeTool = null;
                quizSessionId = null;
                flashcardSessionId = null;
            }
            activeView = view;
            if (window.innerWidth < 768) {
                toggleSidebar();
            }
            if (!isGeneratingState) {
                updateUI();
            }
        };
        
        const renderLoading = (container, toolName) => {
            container.innerHTML = `
                <div class="flex flex-col items-center justify-center h-full text-xl text-gray-500 min-h-[50vh] bg-white p-8 rounded-xl shadow-xl">
                    <svg class="animate-spin -ml-1 mr-3 h-10 w-10 text-indigo-500" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                      <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                      <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                    </svg>
                    <p class="mt-4 font-semibold text-gray-800">Researching and Generating ${toolName}...</p>
                    <p class="text-base text-gray-500">The AI is currently researching your prompt using Google Search to generate content.</p>
                </div>
            `;
        }
        
        const renderSessionLoading = (container, toolName) => {
            container.innerHTML = `
                <div class="flex flex-col items-center justify-center h-full text-xl text-gray-500 min-h-[50vh] bg-white p-8 rounded-xl shadow-xl">
                    <svg class="animate-spin -ml-1 mr-3 h-10 w-10 text-indigo-500" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                      <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                      <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                    </svg>
                    <p class="mt-4 font-semibold text-gray-800">Loading active ${toolName} session...</p>
                </div>
            `;
        }

        const updateUI = () => {
            const appContent = document.getElementById('app-content');
            const viewTitle = document.getElementById('view-title');
            const viewSubtitle = document.getElementById('view-subtitle');
            const userIdDisplay = document.getElementById('user-id-display');

            if (!isAuthReady) {
                appContent.innerHTML = `<p class="text-center py-10">Authenticating...</p>`;
                return;
            }

            // Update Header
            let titleText = "Welcome to the AI Study Hub";
            let subtitleText = "Select a tool from the left menu to begin your prompt-driven study session.";

            if (activeView === 'welcome') {
                titleText = 'Welcome to the AI Study Hub';
                subtitleText = 'Select a tool from the left menu to begin your prompt-driven study session.';
                renderWelcome(appContent);
            } else if (activeView === 'ai_quiz') {
                titleText = 'AI Quiz Generator';
                subtitleText = (generatedContent && activeTool === 'ai_quiz') ? `Quiz generated for topic: ${currentTopic} (${currentCount} questions)` : 'Enter a prompt (e.g., "5 questions about the history of the Roman Republic").';
                renderAIQuiz(appContent);
            } else if (activeView === 'ai_flashcards') {
                titleText = 'AI Flashcards';
                subtitleText = (generatedContent && activeTool === 'ai_flashcards') ? `Flashcards generated for prompt: ${currentPrompt}` : 'Enter a prompt to generate interactive flashcards.';
                renderAIFlashcards(appContent);
            } else if (activeView === 'ai_guide') {
                titleText = 'AI Study Guide Maker';
                subtitleText = (generatedContent && activeTool === 'ai_guide') ? `Study Guide generated for prompt: ${currentPrompt}` : 'Enter a prompt to generate a comprehensive study guide.';
                renderAIGuide(appContent);
            } else if (activeView === 'dashboard') {
                titleText = 'Dashboard & Saved Items';
                subtitleText = 'Review your performance and saved materials.';
                renderDashboard(appContent);
            }

            viewTitle.textContent = titleText;
            viewSubtitle.textContent = subtitleText;
            userIdDisplay.textContent = userId ? userId : 'Auth Failed/Mock ID';
            
            lucide.createIcons();
        };
        
        const renderWelcome = (container) => {
            container.innerHTML = `
                <div class="space-y-6 max-w-4xl mx-auto py-10">
                    <div class="p-8 bg-indigo-50 border-4 border-indigo-200 text-indigo-800 rounded-2xl shadow-xl text-center">
                        <i data-lucide="brain" class="w-12 h-12 mx-auto text-indigo-600 mb-4"></i>
                        <p class="text-xl font-semibold mb-2">Welcome!</p>
                        <p class="text-lg">This hub uses advanced AI and real-time Google Search grounding to create custom study tools.</p>
                        <p class="mt-4 font-bold">Simply select a tool on the left and enter a detailed prompt to begin.</p>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6 pt-4">
                        <div class="p-4 bg-white rounded-xl shadow-md border-t-4 border-indigo-500">
                            <h3 class="font-bold text-lg mb-1">Quiz Generator</h3>
                            <p class="text-sm text-gray-600">Enter: "5 questions on World War II battles".</p>
                        </div>
                        <div class="p-4 bg-white rounded-xl shadow-md border-t-4 border-indigo-500">
                            <h3 class="font-bold text-lg mb-1">Flashcards</h3>
                            <p class="text-sm text-gray-600">Enter: "Generate 10 flashcards for common computer science data structures."</p>
                        </div>
                        <div class="p-4 bg-white rounded-xl shadow-md border-t-4 border-indigo-500">
                            <h3 class="font-bold text-lg mb-1">Study Guide</h3>
                            <p class="text-sm text-gray-600">Enter: "Write a comprehensive guide on the process and products of photosynthesis."</p>
                        </div>
                    </div>
                </div>
            `;
            lucide.createIcons();
        };

        // RENDER TOOL INPUT
        const renderToolInput = (container, toolName, toolKey, placeholder) => {
            const iconMap = {
                'ai_quiz': 'check-circle',
                'ai_flashcards': 'rotate-cw',
                'ai_guide': 'book-open'
            };
            const icon = iconMap[toolKey];
            const inputId = `prompt-input-${toolKey}`;
            let generateButtonText = `Generate ${toolName}`;

            container.innerHTML = `
                <div class="space-y-6 max-w-4xl mx-auto">
                    <!-- Hint Card -->
                    <div class="p-4 mb-4 bg-indigo-100 border-l-4 border-indigo-500 text-indigo-800 rounded-lg shadow-md flex items-start">
                        <i data-lucide="lightbulb" class="w-6 h-6 mr-3 flex-shrink-0 mt-1"></i>
                        <div>
                            <p class="font-bold">Enter Your Prompt:</p>
                            <p class="text-sm">Be specific. For Quiz, try starting with the number of questions, e.g., "7 questions on the Roman Empire."</p>
                        </div>
                    </div>
                    
                    <label for="${inputId}" class="block text-sm font-medium text-gray-700 mb-1">Detailed Prompt</label>
                    <textarea
                        id="${inputId}"
                        rows="5"
                        class="w-full p-6 text-lg border-2 border-indigo-300 rounded-xl shadow-inner focus:ring-indigo-500 focus:border-indigo-500 transition duration-150"
                        placeholder="${placeholder}"
                    >${currentPrompt}</textarea>

                    <div class="flex justify-center md:justify-end">
                        <button onclick="generateContent('${toolKey}')" class="px-6 py-3 bg-indigo-600 text-white font-bold rounded-lg shadow-lg hover:bg-indigo-700 transition duration-300 transform hover:scale-[1.02] flex items-center">
                            <i data-lucide="${icon}" class="w-5 h-5 mr-2"></i> ${generateButtonText}
                        </button>
                    </div>
                </div>
            `;
            lucide.createIcons();
        };
        
        const renderAIGuide = (container) => {
             if (isGenerating) {
                renderLoading(container, "Study Guide");
                return;
            }
            if (activeTool !== 'ai_guide' || !generatedContent) {
                 renderToolInput(container, 'Study Guide', 'ai_guide', 'e.g., Explain the concept of blockchain technology and its applications in financial services.');
                 return;
            }
            
            container.innerHTML = `
                <div class="bg-white p-6 sm:p-10 rounded-xl shadow-2xl max-w-4xl mx-auto space-y-6">
                    <div class="flex justify-between items-center border-b pb-4 mb-4">
                        <h3 class="text-2xl font-bold text-gray-800">Study Guide (Prompt: ${currentPrompt.substring(0, 40)}...)</h3>
                        <button
                            onclick="saveItem('study_guide', 'guide_${Date.now()}', '${currentPrompt.replace(/'/g, "\\'")}', 'General')"
                            class="flex items-center px-4 py-2 bg-green-500 text-white font-semibold rounded-lg shadow-md hover:bg-green-600 transition duration-300"
                        >
                            <i data-lucide="save" class="w-5 h-5 mr-2"></i> Save Guide
                        </button>
                    </div>
                    ${markdownToHtml(generatedContent)}
                    <div class="pt-6 border-t text-right">
                         <button onclick="setView('ai_guide')" class="px-4 py-2 bg-gray-200 text-gray-700 font-semibold rounded-lg hover:bg-gray-300 transition">
                            <i data-lucide="edit-3" class="w-4 h-4 inline mr-1"></i> New Prompt
                        </button>
                    </div>
                </div>
            `;
            if (window.MathJax) {
                 MathJax.typesetPromise([container]);
            }
            lucide.createIcons();
        };

        const renderAIQuiz = (container) => {
            if (isGenerating) {
                renderLoading(container, "Quiz");
                return;
            }
            if (isQuizSessionLoading) {
                renderSessionLoading(container, "Quiz");
                return;
            }
            const quizData = generatedContent;
            
            if (activeTool !== 'ai_quiz' || !quizData || quizData.length === 0) {
                 renderToolInput(container, 'Quiz', 'ai_quiz', 'e.g., 8 questions about the history of the Roman Republic.');
                 return;
            }

            const currentQuestion = quizData[currentQuestionIndex];
            
            if (showQuizResult) {
                container.innerHTML = `
                    <div class="p-8 bg-white rounded-xl shadow-2xl space-y-6 text-center max-w-xl mx-auto">
                        <h2 class="text-3xl font-extrabold text-indigo-700">Quiz Completed!</h2>
                        <p class="text-xl text-gray-700">
                            You scored <span class="text-4xl font-black text-green-600">${quizScore}</span> out of <span class="text-2xl font-bold">${quizData.length}</span>.
                        </p>
                        <p class="text-lg text-gray-500">Check your weak areas in the Dashboard to improve!</p>
                        <div class="flex justify-center space-x-4">
                            <button onclick="restartQuiz()" class="px-6 py-3 bg-indigo-600 text-white font-semibold rounded-lg shadow-lg hover:bg-indigo-700 transition duration-300 transform hover:scale-105">
                                <i data-lucide="rotate-cw" class="w-5 h-5 inline mr-2"></i> Try Again
                            </button>
                             <button onclick="setView('ai_quiz')" class="px-6 py-3 bg-gray-200 text-gray-700 font-semibold rounded-lg shadow-lg hover:bg-gray-300 transition duration-300">
                                <i data-lucide="edit-3" class="w-5 h-5 inline mr-2"></i> New Prompt
                            </button>
                        </div>
                    </div>
                `;
                return;
            }

            const optionsHtml = currentQuestion.options.map((option, index) => {
                let optionClass = "w-full text-left px-5 py-3 border-2 rounded-lg transition duration-200 shadow-sm flex justify-between items-center text-lg ";
                let icon = '';

                if (isAnswerChecked) {
                    if (option.trim() === currentQuestion.answer.trim()) {
                        optionClass += "border-green-500 bg-green-50 text-green-700 font-bold";
                        icon = '<i data-lucide="check-circle" class="w-5 h-5 ml-2"></i>';
                    } else if (option.trim() === selectedOption) {
                        optionClass += "border-red-500 bg-red-50 text-red-700 font-bold";
                        icon = '<i data-lucide="x-circle" class="w-5 h-5 ml-2"></i>';
                    } else {
                        optionClass += "border-gray-200 bg-gray-50 text-gray-600 cursor-default";
                    }
                } else {
                    optionClass += "border-indigo-100 bg-indigo-50 text-indigo-800 hover:border-indigo-400 hover:bg-indigo-100 cursor-pointer";
                    if (selectedOption === option.trim()) {
                        optionClass += " border-indigo-500 ring-4 ring-indigo-200";
                    }
                }

                return `
                    <button
                        key="${index}"
                        onclick="handleQuizOptionClick('${option.replace(/'/g, "\\'")}')"
                        ${isAnswerChecked ? 'disabled' : ''}
                        class="${optionClass}"
                    >
                        <span>${option}</span>
                        ${icon}
                    </button>
                `;
            }).join('');
            
            const actionButton = isAnswerChecked ?
                `<button onclick="goToNextQuestion()" class="px-6 py-3 bg-indigo-600 text-white font-semibold rounded-lg shadow-md hover:bg-indigo-700 transition duration-300 flex items-center">
                    ${currentQuestionIndex === quizData.length - 1 ? 'Finish Quiz' : 'Next Question'}
                    <i data-lucide="arrow-right" class="w-5 h-5 ml-2"></i>
                </button>` :
                `<button onclick="checkAnswer()" ${selectedOption ? '' : 'disabled'} class="px-6 py-3 ${selectedOption ? 'bg-green-600 hover:bg-green-700' : 'bg-gray-300 text-gray-600 cursor-not-allowed'} text-white font-semibold rounded-lg shadow-md transition duration-300 flex items-center">
                    Check Answer
                    <i data-lucide="check" class="w-5 h-5 ml-2"></i>
                </button>`;

            container.innerHTML = `
                <div class="bg-white p-6 sm:p-8 rounded-xl shadow-2xl max-w-xl mx-auto space-y-6">
                    <div class="flex justify-between items-center border-b pb-4 mb-4">
                        <h3 class="text-xl font-bold text-gray-800">Question ${currentQuestionIndex + 1} / ${quizData.length} (Topic: ${currentTopic.substring(0, 20)}...)</h3>
                        <div class="flex items-center space-x-3">
                            <span class="text-sm font-medium text-gray-500">Sub-Topic: ${currentQuestion.topic || 'General'}</span>
                            <button
                                onclick="saveItem('quiz_question', 'q_${quizSessionId}_${currentQuestionIndex}', '${currentQuestion.question.replace(/'/g, "\\'")}', '${currentQuestion.topic || 'General'}')"
                                class="flex items-center text-sm font-medium text-indigo-600 hover:text-indigo-800 transition duration-150"
                                title="Save this question for later review"
                            >
                                <i data-lucide="bookmark" class="w-5 h-5 mr-1"></i> Save
                            </button>
                        </div>
                    </div>
                    <p class="text-2xl font-semibold text-gray-900 mb-6">${currentQuestion.question}</p>

                    <div class="space-y-4">
                        ${optionsHtml}
                    </div>

                    <div class="pt-6 border-t mt-6 flex justify-end">
                        ${actionButton}
                    </div>
                </div>
            `;
            lucide.createIcons();
            if (window.MathJax) {
                 MathJax.typesetPromise([container]);
            }
        };

        // --- QUIZ FUNCTIONS (Updated to use saveQuizProgress) ---

        window.handleQuizOptionClick = (option) => {
            if (isAnswerChecked) return;
            selectedOption = option.trim();
            saveQuizProgress(); // Save selected option
            renderAIQuiz(document.getElementById('app-content'));
        };

        window.checkAnswer = () => {
            const quizData = generatedContent || [];
            if (isAnswerChecked || !selectedOption || quizData.length === 0) return;

            isAnswerChecked = true;
            const currentQuestion = quizData[currentQuestionIndex];
            const isCorrect = selectedOption === currentQuestion.answer.trim();

            if (isCorrect) {
                quizScore += 1;
            } else {
                updateWeakArea(currentQuestion.topic || 'General');
            }
            
            saveQuizProgress(); // Save score and checked state
            renderAIQuiz(document.getElementById('app-content'));
        };

        window.goToNextQuestion = () => {
            const quizData = generatedContent || [];
            if (!isAnswerChecked) {
                checkAnswer(); 
                if (!isAnswerChecked) return;
            }

            const nextQuestionIndex = currentQuestionIndex + 1;
            if (nextQuestionIndex < quizData.length) {
                currentQuestionIndex = nextQuestionIndex;
                selectedOption = null;
                isAnswerChecked = false;
                saveQuizProgress(); // Save new index
                renderAIQuiz(document.getElementById('app-content'));
            } else {
                showQuizResult = true;
                updateQuizSummary(quizScore, quizData.length);
                quizSessionId = null; // Clear session ID as quiz is finished
                saveQuizProgress(); // Save final state
                renderAIQuiz(document.getElementById('app-content'));
            }
        };

        window.restartQuiz = () => {
            currentQuestionIndex = 0;
            quizScore = 0;
            showQuizResult = false;
            selectedOption = null;
            isAnswerChecked = false;
            quizSessionId = null; // Clear ID to prompt for new quiz next time
            saveQuizProgress(); 
            renderAIQuiz(document.getElementById('app-content'));
        };


        // --- FLASHCARDS FUNCTIONS (Updated to use saveFlashcardProgress) ---

        window.handleCardFlip = () => {
            isFlipped = !isFlipped;
            saveFlashcardProgress(); // Save flipped state
            renderAIFlashcards(document.getElementById('app-content'));
        };

        window.handleCardNav = (direction) => {
            const flashcardData = generatedContent || [];
            const totalCards = flashcardData.length;
            if (direction === 'prev') {
                currentCardIndex = (currentCardIndex > 0 ? currentCardIndex - 1 : totalCards - 1);
            } else {
                currentCardIndex = (currentCardIndex < totalCards - 1 ? currentCardIndex + 1 : 0);
            }
            isFlipped = false;
            saveFlashcardProgress(); // Save new index
            renderAIFlashcards(document.getElementById('app-content'));
        };

        const renderAIFlashcards = (container) => {
            if (isGenerating) {
                renderLoading(container, "Flashcards");
                return;
            }
            if (isFlashcardSessionLoading) {
                renderSessionLoading(container, "Flashcards");
                return;
            }

            const flashcardData = generatedContent;
            
            if (activeTool !== 'ai_flashcards' || !flashcardData || flashcardData.length === 0) {
                 renderToolInput(container, 'Flashcards', 'ai_flashcards', 'e.g., Give me 10 key terms and definitions from the novel "1984" by George Orwell.');
                 return;
            }
            
            const currentCard = flashcardData[currentCardIndex];
            const totalCards = flashcardData.length;

            container.innerHTML = `
                <div class="space-y-6 max-w-lg mx-auto">
                    <div class="flex justify-between items-center text-gray-600">
                        <p class="text-lg font-medium">Card ${currentCardIndex + 1} / ${totalCards} (Prompt Topic: ${currentPrompt.substring(0, 20)}...)</p>
                        <div class="flex items-center space-x-3">
                            <span class="text-sm font-medium text-gray-500">Sub-Topic: ${currentCard.topic || 'General'}</span>
                            <button
                                onclick="saveItem('flashcard', 'f_${flashcardSessionId}_${currentCardIndex}', '${currentCard.term.replace(/'/g, "\\'")}', '${currentCard.topic || 'General'}')"
                                class="flex items-center text-sm font-medium text-indigo-600 hover:text-indigo-800 transition duration-150"
                                title="Save this flashcard"
                            >
                                <i data-lucide="bookmark" class="w-5 h-5 mr-1"></i> Save Card
                            </button>
                        </div>
                    </div>

                    <!-- Flashcard Box -->
                    <div onclick="handleCardFlip()" class="card-container relative w-full h-80 bg-white rounded-xl shadow-2xl flex items-center justify-center p-8 cursor-pointer ${isFlipped ? 'flipped' : ''}">
                        <div class="card-inner w-full h-full absolute">
                            <!-- Front (Term) -->
                            <div class="card-front flex flex-col items-center justify-center text-center p-8 bg-white rounded-xl border border-indigo-200">
                                <p class="text-sm text-indigo-500 font-medium uppercase mb-2">Term</p>
                                <h2 class="text-3xl font-extrabold text-gray-900">${currentCard.term}</h2>
                                <div class="absolute bottom-4 right-4 text-gray-400 text-xs flex items-center">
                                    Click to see Definition <i data-lucide="rotate-cw" class="w-3 h-3 ml-1"></i>
                                </div>
                            </div>

                            <!-- Back (Definition) -->
                            <div class="card-back flex flex-col items-center justify-center text-center p-8 bg-white rounded-xl border border-green-200">
                                <p class="text-sm text-green-500 font-medium uppercase mb-2">Definition</p>
                                <p class="text-xl text-gray-700">${currentCard.definition}</p>
                                <div class="absolute bottom-4 right-4 text-gray-400 text-xs flex items-center">
                                    Click to see Term <i data-lucide="rotate-cw" class="w-3 h-3 ml-1"></i>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Navigation Buttons -->
                    <div class="flex justify-between pt-4">
                        <button
                            onclick="handleCardNav('prev')"
                            class="flex items-center px-4 py-2 bg-gray-100 text-gray-700 font-semibold rounded-full shadow-md hover:bg-gray-200 transition duration-150"
                        >
                            <i data-lucide="arrow-left" class="w-5 h-5 mr-2"></i> Previous
                        </button>
                        <div class="flex space-x-2">
                             <button onclick="setView('ai_flashcards')" class="px-4 py-2 bg-gray-200 text-gray-700 font-semibold rounded-full hover:bg-gray-300 transition">
                                <i data-lucide="edit-3" class="w-4 h-4 inline"></i> New Prompt
                            </button>
                            <button
                                onclick="handleCardNav('next')"
                                class="flex items-center px-4 py-2 bg-indigo-600 text-white font-semibold rounded-full shadow-md hover:bg-indigo-700 transition duration-150"
                            >
                                Next <i data-lucide="arrow-right" class="w-5 h-5 ml-2"></i>
                            </button>
                        </div>
                    </div>
                </div>
            `;
            lucide.createIcons();
            if (window.MathJax) {
                 MathJax.typesetPromise([container]);
            }
        };


        // --- DASHBOARD FUNCTIONS ---

        window.saveItem = async (type, id, content, topic) => {
            if (!db || !userId) {
                showModal('Save Failed', 'Authentication is not ready. Cannot save item.');
                return;
            }

            const userBasePath = getUserBasePath(userId);
            const savedItemsColRef = collection(db, userBasePath, 'study_data', SAVED_ITEMS_COL);
            
            let displayContent = content;
            let fullContent = null;
            
            // Determine the full content to save
            if (type === 'study_guide') {
                fullContent = generatedContent;
            } else if (type === 'quiz_question' && generatedContent && generatedContent[currentQuestionIndex]) {
                 fullContent = generatedContent[currentQuestionIndex];
            } else if (type === 'flashcard' && generatedContent && generatedContent[currentCardIndex]) {
                 fullContent = generatedContent[currentCardIndex];
            }

            // Simple check to prevent saving the same item twice based on content string
            const isAlreadySaved = savedItems.some(saved => 
                saved.type === type && 
                saved.content === displayContent
            );
            
            if (isAlreadySaved) {
                showModal('Already Saved', `This ${type.replace('_', ' ')} seems to be already saved.`);
                return;
            }

            try {
                await addDoc(savedItemsColRef, {
                    type,
                    id: id,
                    content: displayContent,
                    fullContent: JSON.stringify(fullContent), // Store complex objects as JSON strings
                    topic: topic,
                    userId: userId,
                    savedAt: serverTimestamp()
                });
                showModal('Success!', 'Item saved successfully to your dashboard!');
            } catch (e) {
                console.error("Error saving item:", e);
                showModal('Save Failed', `Error saving item: ${e.message}`);
            }
        };

        window.updateWeakArea = async (topic) => {
            if (!db || !userId) return;

            const userBasePath = getUserBasePath(userId);
            const weakAreasRef = doc(db, userBasePath, 'study_data', WEAK_AREAS_DOC);

            const newWeakAreas = {
                ...weakAreas,
                [topic]: (weakAreas[topic] || 0) + 1
            };

            try {
                await updateDoc(weakAreasRef, { topics: newWeakAreas });
                weakAreas = newWeakAreas;
            } catch (e) {
                console.error("Error updating weak areas:", e);
            }
        };
        
        const updateQuizSummary = async (score, total) => {
            if (!db || !userId) return;

            const userBasePath = getUserBasePath(userId);
            const weakAreasRef = doc(db, userBasePath, 'study_data', WEAK_AREAS_DOC);

            const newTotalScore = totalScore + score;
            const newTotalQuizzes = totalQuizzes + total;

            try {
                await updateDoc(weakAreasRef, {
                    totalScore: newTotalScore,
                    totalQuizzes: newTotalQuizzes
                });
                totalScore = newTotalScore;
                totalQuizzes = newTotalQuizzes;
            } catch (e) {
                console.error("Error updating quiz summary:", e);
            }
        };


        const renderDashboard = (container) => {
            const percentage = totalQuizzes > 0 ? ((totalScore / totalQuizzes) * 100).toFixed(1) : 0;
            const uniqueWeakAreas = Object.keys(weakAreas).filter(topic => weakAreas[topic] > 0).sort((a, b) => weakAreas[b] - weakAreas[a]); // Sort by most misses

            const getTopicColor = (index) => {
                const colors = ['bg-red-100 text-red-800', 'bg-orange-100 text-orange-800', 'bg-yellow-100 text-yellow-800', 'bg-pink-100 text-pink-800'];
                return colors[index % colors.length];
            };

            const weakAreasHtml = uniqueWeakAreas.length > 0 ?
                `<div class="flex flex-wrap gap-2">
                    ${uniqueWeakAreas.map((topic, index) => `
                        <span class="px-3 py-1 text-sm font-medium rounded-full ${getTopicColor(index)}">
                            ${topic} (${weakAreas[topic]} misses)
                        </span>
                    `).join('')}
                </div>` :
                '<p class="text-gray-600">Great job! No weak areas recorded yet. Keep quizzing!</p>';

            const savedItemsHtml = savedItems.length > 0 ?
                `<div class="space-y-3">
                    ${savedItems.map(item => {
                        const typeClass = item.type.includes('quiz') ? 'bg-purple-100 text-purple-800' : item.type.includes('flashcard') ? 'bg-blue-100 text-blue-800' : 'bg-green-100 text-green-800';
                        const displayContent = item.type === 'study_guide' ? `Guide for: ${item.content.substring(0, 50)}...` : item.content;

                        return `
                        <div class="p-4 border border-gray-100 bg-gray-50 rounded-lg flex justify-between items-center shadow-sm">
                            <div>
                                <p class="text-lg font-medium text-gray-800">${displayContent}</p>
                                <p class="text-sm text-gray-500">
                                    <span class="px-2 py-0.5 text-xs rounded-full ${typeClass}">
                                        ${item.type.charAt(0).toUpperCase() + item.type.slice(1).replace('_', ' ')}
                                    </span> - Sub-Topic: ${item.topic}
                                </p>
                            </div>
                        </div>
                    `;
                    }).join('')}
                </div>` :
                `<div class="p-4 bg-yellow-50 text-yellow-700 rounded-lg flex items-center">
                    <i data-lucide="info" class="w-5 h-5 mr-3"></i>
                    <p>You haven't saved any items yet. Use the 'Save' button on the generated tools!</p>
                </div>`;
            
            // Hint Card for Mobile Users
            const hintCard = `
                <div class="md:hidden p-4 mb-8 bg-yellow-100 border-l-4 border-yellow-500 text-yellow-800 rounded-lg shadow-md flex items-center">
                    <i data-lucide="hand-pointing" class="w-6 h-6 mr-3 flex-shrink-0"></i>
                    <p class="font-medium">
                        Click the <span class="font-extrabold text-yellow-900">Menu button</span> (<i data-lucide="menu" class="w-5 h-5 inline"></i>) to access the AI study tools!
                    </p>
                </div>
            `;


            container.innerHTML = `
                ${hintCard}
                <div class="space-y-8 p-4 sm:p-8 bg-white rounded-xl shadow-2xl">
                    <h2 class="text-3xl font-bold text-gray-900 border-b pb-4">Study Overview</h2>

                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <!-- Quiz Performance -->
                        <div class="bg-indigo-50 p-6 rounded-xl shadow-md space-y-4">
                            <div class="flex items-center text-indigo-700">
                                <i data-lucide="bar-chart-2" class="w-6 h-6 mr-2"></i>
                                <h3 class="text-xl font-semibold">Overall Quiz Performance</h3>
                            </div>
                            <p class="text-4xl font-extrabold text-indigo-900">${percentage}%</p>
                            <p class="text-gray-600">
                                Based on ${totalScore} correct answers out of ${totalQuizzes} total questions.
                            </p>
                        </div>

                        <!-- Weak Areas/Growth -->
                        <div class="bg-red-50 p-6 rounded-xl shadow-md space-y-4">
                            <div class="flex items-center text-red-700">
                                <i data-lucide="trending-up" class="w-6 h-6 mr-2"></i>
                                <h3 class="text-xl font-semibold">Weak Areas (Topics to Review)</h3>
                            </div>
                            ${weakAreasHtml}
                        </div>
                    </div>

                    <!-- Saved Items -->
                    <div class="space-y-4 pt-4 border-t">
                        <div class="flex items-center text-green-700">
                            <i data-lucide="save" class="w-6 h-6 mr-2"></i>
                            <h3 class="text-2xl font-semibold">Saved Study Items (${savedItems.length})</h3>
                        </div>
                        ${savedItemsHtml}
                    </div>
                </div>
            `;
            lucide.createIcons();
        };


        // --- ENTRY POINT ---
        window.onload = () => {
            initFirebase();
        };
    </script>
</body>
</html>
