<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Verbes Irréguliers - Mode Apprentissage Optimisé</title>
    <style>
        :root {
            --primary: #4f46e5;
            --primary-dark: #4338ca;
            --success: #16a34a;
            --error: #dc2626;
            --bg: #f1f5f9;
            --card-bg: #ffffff;
            --text-main: #1e293b;
            --text-light: #64748b;
        }

        body {
            font-family: 'Inter', 'Segoe UI', system-ui, sans-serif;
            background-color: var(--bg);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            color: var(--text-main);
        }

        .app-container {
            background: var(--card-bg);
            width: 90%;
            max-width: 700px;
            border-radius: 1.5rem;
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            position: relative;
        }

        /* Header & Progress */
        .header {
            background: var(--primary);
            padding: 1.5rem 2rem;
            color: white;
        }

        .stats-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 0.5rem;
            font-size: 0.9rem;
            opacity: 0.9;
        }

        .progress-container {
            background: rgba(255, 255, 255, 0.3);
            height: 8px;
            border-radius: 4px;
            overflow: hidden;
        }

        .progress-bar {
            background: white;
            height: 100%;
            width: 0%;
            transition: width 0.5s ease;
        }

        /* Card Content */
        .content {
            padding: 2rem;
            text-align: center;
        }

        .badge {
            display: inline-block;
            background: #e0e7ff;
            color: var(--primary);
            padding: 0.25rem 0.75rem;
            border-radius: 999px;
            font-size: 0.75rem;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 0.05em;
            margin-bottom: 1rem;
        }

        .french-word {
            font-size: 2rem;
            font-weight: 800;
            color: var(--text-main);
            margin: 0 0 2rem 0;
            line-height: 1.2;
        }

        /* Inputs Grid */
        .inputs-grid {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 1rem;
            margin-bottom: 1.5rem;
        }

        @media (max-width: 600px) {
            .inputs-grid {
                grid-template-columns: 1fr;
            }
        }

        .input-wrapper {
            text-align: left;
        }

        label {
            display: block;
            font-size: 0.75rem;
            font-weight: 600;
            color: var(--text-light);
            margin-bottom: 0.5rem;
            text-transform: uppercase;
        }

        input {
            width: 100%;
            padding: 0.75rem;
            border: 2px solid #e2e8f0;
            border-radius: 0.75rem;
            font-size: 1rem;
            outline: none;
            transition: all 0.2s;
            box-sizing: border-box;
            background: #f8fafc;
        }

        input:focus {
            border-color: var(--primary);
            background: white;
            box-shadow: 0 0 0 4px rgba(79, 70, 229, 0.1);
        }

        input.correct {
            border-color: var(--success);
            background-color: #f0fdf4;
            color: var(--success);
            font-weight: 600;
        }

        input.incorrect {
            border-color: var(--error);
            background-color: #fef2f2;
            color: var(--error);
        }

        /* Correction Display */
        .correction-box {
            background: #fef2f2;
            border: 1px solid #fecaca;
            border-radius: 0.75rem;
            padding: 1rem;
            margin-bottom: 1.5rem;
            text-align: left;
            display: none; /* Hidden by default */
        }
        
        .correction-box.visible {
            display: block;
            animation: fadeIn 0.3s;
        }

        .correction-title {
            color: var(--error);
            font-weight: bold;
            font-size: 0.9rem;
            margin-bottom: 0.5rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .correction-values {
            display: flex;
            gap: 1rem;
            font-family: monospace;
            font-size: 1.1rem;
            color: #7f1d1d;
            flex-wrap: wrap;
        }

        /* Buttons */
        .btn {
            background: var(--primary);
            color: white;
            border: none;
            padding: 1rem 2rem;
            border-radius: 0.75rem;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            width: 100%;
            transition: transform 0.1s, background 0.2s;
        }

        .btn:hover {
            background: var(--primary-dark);
        }

        .btn:active {
            transform: scale(0.98);
        }

        .btn-next {
            background: var(--text-main);
            display: none;
        }

        /* Animations */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Completion Screen */
        .completion-screen {
            display: none;
            text-align: center;
            padding: 3rem 1rem;
        }
        
        .big-check {
            font-size: 4rem;
            color: var(--success);
            margin-bottom: 1rem;
        }

    </style>
</head>
<body>

<div class="app-container">
    <div class="header">
        <div class="stats-row">
            <span id="batchIndicator">Niveau 1</span>
            <span id="progressText">0 / 102 maîtrisés</span>
        </div>
        <div class="progress-container">
            <div class="progress-bar" id="progressBar"></div>
        </div>
    </div>

    <div class="content" id="learningInterface">
        <div class="badge" id="queueCount">Reste : 10</div>
        
        <h2 class="french-word" id="frenchDisplay">...</h2>

        <div class="correction-box" id="correctionBox">
            <div class="correction-title">
                <svg width="20" height="20" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                Oups ! Voici la réponse correcte :
            </div>
            <div class="correction-values" id="correctionText">
                <!-- Values injected via JS -->
            </div>
            <div style="margin-top:0.5rem; font-size:0.85rem; color:#b91c1c;">
                Ce verbe reviendra à la fin de la série.
            </div>
        </div>

        <div class="inputs-grid">
            <div class="input-wrapper">
                <label>Base Verbale</label>
                <input type="text" id="inBase" placeholder="be" autocomplete="off">
            </div>
            <div class="input-wrapper">
                <label>Prétérit</label>
                <input type="text" id="inPast" placeholder="was / were" autocomplete="off">
            </div>
            <div class="input-wrapper">
                <label>Participe Passé</label>
                <input type="text" id="inPart" placeholder="been" autocomplete="off">
            </div>
        </div>

        <button class="btn" id="btnCheck" onclick="checkAnswer()">Valider</button>
        <button class="btn btn-next" id="btnNext" onclick="nextCard()">Continuer</button>
    </div>

    <div class="completion-screen" id="completionScreen">
        <div class="big-check">🎉</div>
        <h2>Série terminée !</h2>
        <p>Tu as maîtrisé ces 10 verbes.</p>
        <button class="btn" onclick="startNextBatch()">Passer au niveau suivant</button>
    </div>
</div>

<script>
    // --- DATA ---
    const allVerbs = [
        {"base": "arise", "past_simple": "arose", "past_participle": "arisen", "french": "survenir, surgir"}, 
        {"base": "awake", "past_simple": "awoke", "past_participle": "awoken", "french": "se réveiller"}, 
        {"base": "be", "past_simple": "was / were", "past_participle": "been", "french": "être"}, 
        {"base": "bear", "past_simple": "bore", "past_participle": "borne", "french": "porter, supporter"}, 
        {"base": "beat", "past_simple": "beat", "past_participle": "beaten", "french": "battre"}, 
        {"base": "become", "past_simple": "became", "past_participle": "become", "french": "devenir"}, 
        {"base": "begin", "past_simple": "began", "past_participle": "begun", "french": "commencer"}, 
        {"base": "bend", "past_simple": "bent", "past_participle": "bent", "french": "plier, (se) courber"}, 
        {"base": "bet", "past_simple": "bet", "past_participle": "bet", "french": "parier"}, 
        {"base": "bite", "past_simple": "bit", "past_participle": "bitten", "french": "mordre"}, 
        {"base": "bleed", "past_simple": "bled", "past_participle": "bled", "french": "saigner"}, 
        {"base": "blend", "past_simple": "blent", "past_participle": "blent", "french": "mélanger"}, 
        {"base": "blow", "past_simple": "blew", "past_participle": "blown", "french": "souffler"}, 
        {"base": "break", "past_simple": "broke", "past_participle": "broken", "french": "casser"}, 
        {"base": "breed", "past_simple": "bred", "past_participle": "bred", "french": "élever (du bétail)"}, 
        {"base": "bring", "past_simple": "brought", "past_participle": "brought", "french": "apporter"}, 
        {"base": "build", "past_simple": "built", "past_participle": "built", "french": "construire"}, 
        {"base": "burn", "past_simple": "burnt", "past_participle": "burnt", "french": "brûler"}, 
        {"base": "burst", "past_simple": "burst", "past_participle": "burst", "french": "éclater"}, 
        {"base": "buy", "past_simple": "bought", "past_participle": "bought", "french": "acheter"}, 
        {"base": "cast", "past_simple": "cast", "past_participle": "cast", "french": "lancer, jeter"}, 
        {"base": "catch", "past_simple": "caught", "past_participle": "caught", "french": "attraper"}, 
        {"base": "choose", "past_simple": "chose", "past_participle": "chosen", "french": "choisir"}, 
        {"base": "come", "past_simple": "came", "past_participle": "come", "french": "venir"}, 
        {"base": "cost", "past_simple": "cost", "past_participle": "cost", "french": "coûter"}, 
        {"base": "creep", "past_simple": "crept", "past_participle": "crept", "french": "ramper"}, 
        {"base": "cut", "past_simple": "cut", "past_participle": "cut", "french": "couper"}, 
        {"base": "deal", "past_simple": "dealt", "past_participle": "dealt", "french": "distribuer/ traiter"}, 
        {"base": "dig", "past_simple": "dug", "past_participle": "dug", "french": "creuser"}, 
        {"base": "do", "past_simple": "did", "past_participle": "done", "french": "faire"}, 
        {"base": "draw", "past_simple": "drew", "past_participle": "drawn", "french": "dessiner, tirer"}, 
        {"base": "dream", "past_simple": "dreamt", "past_participle": "dreamt", "french": "rêver"}, 
        {"base": "drink", "past_simple": "drank", "past_participle": "drunk", "french": "boire"}, 
        {"base": "drive", "past_simple": "drove", "past_participle": "driven", "french": "conduire"}, 
        {"base": "dwell", "past_simple": "dwelt", "past_participle": "dwelt", "french": "demeurer, résider"}, 
        {"base": "eat", "past_simple": "ate", "past_participle": "eaten", "french": "manger"}, 
        {"base": "fall", "past_simple": "fell", "past_participle": "fallen", "french": "tomber"}, 
        {"base": "feed", "past_simple": "fed", "past_participle": "fed", "french": "(se) nourrir"}, 
        {"base": "feel", "past_simple": "felt", "past_participle": "felt", "french": "sentir, ressentir"}, 
        {"base": "fight", "past_simple": "fought", "past_participle": "fought", "french": "(se) battre"}, 
        {"base": "find", "past_simple": "found", "past_participle": "found", "french": "trouver"}, 
        {"base": "flee", "past_simple": "fled", "past_participle": "fled", "french": "s'enfuire, fuir"}, 
        {"base": "fly", "past_simple": "flew", "past_participle": "flown", "french": "voler, prendre l'avion"}, 
        {"base": "forbid", "past_simple": "forbade", "past_participle": "forbidden", "french": "interdire"}, 
        {"base": "forget", "past_simple": "forgot", "past_participle": "forgotten", "french": "oublier"}, 
        {"base": "forgive", "past_simple": "forgave", "past_participle": "forgiven", "french": "pardonner"}, 
        {"base": "forecast", "past_simple": "forecast", "past_participle": "forecast", "french": "prévoir (météo, finance)"}, 
        {"base": "foresee", "past_simple": "foresaw", "past_participle": "foreseen", "french": "prévoir, anticiper"}, 
        {"base": "freeze", "past_simple": "froze", "past_participle": "frozen", "french": "geler"}, 
        {"base": "get", "past_simple": "got", "past_participle": "got", "french": "obtenir, devenir"}, 
        {"base": "give", "past_simple": "gave", "past_participle": "given", "french": "donner"}, 
        {"base": "go", "past_simple": "went", "past_participle": "gone", "french": "aller"}, 
        {"base": "grow", "past_simple": "grew", "past_participle": "grown", "french": "grandir, faire pousser"}, 
        {"base": "hang", "past_simple": "hung", "past_participle": "hung", "french": "accrocher, pendre, suspendre"}, 
        {"base": "have", "past_simple": "had", "past_participle": "had", "french": "avoir"}, 
        {"base": "hear", "past_simple": "heard", "past_participle": "heard", "french": "entendre"}, 
        {"base": "hide", "past_simple": "hid", "past_participle": "hidden", "french": "(se) cacher"}, 
        {"base": "hit", "past_simple": "hit", "past_participle": "hit", "french": "frapper"}, 
        {"base": "hold", "past_simple": "held", "past_participle": "held", "french": "tenir"}, 
        {"base": "hurt", "past_simple": "hurt", "past_participle": "hurt", "french": "blesser, se blesser"}, 
        {"base": "keep", "past_simple": "kept", "past_participle": "kept", "french": "garder"}, 
        {"base": "kneel", "past_simple": "knelt", "past_participle": "knelt", "french": "s'agenouiller"}, 
        {"base": "know", "past_simple": "knew", "past_participle": "known", "french": "connaître, savoir"}, 
        {"base": "lay", "past_simple": "laid", "past_participle": "laid", "french": "mettre, poser, pondre"}, 
        {"base": "lead", "past_simple": "led", "past_participle": "led", "french": "mener, conduire"}, 
        {"base": "learn", "past_simple": "learnt", "past_participle": "learnt", "french": "apprendre"}, 
        {"base": "leave", "past_simple": "left", "past_participle": "left", "french": "laisser, partir, quitter"}, 
        {"base": "lend", "past_simple": "lent", "past_participle": "lent", "french": "prêter"}, 
        {"base": "let", "past_simple": "let", "past_participle": "let", "french": "laisser, louer"}, 
        {"base": "lie", "past_simple": "lay", "past_participle": "lain", "french": "être couché, être étendu"}, 
        {"base": "light", "past_simple": "lit", "past_participle": "lit", "french": "éclairer"}, 
        {"base": "lose", "past_simple": "lost", "past_participle": "lost", "french": "perdre"}, 
        {"base": "make", "past_simple": "made", "past_participle": "made", "french": "faire, fabriquer"}, 
        {"base": "mean", "past_simple": "meant", "past_participle": "meant", "french": "signifier"}, 
        {"base": "meet", "past_simple": "met", "past_participle": "met", "french": "rencontrer"}, 
        {"base": "mistake", "past_simple": "mistook", "past_participle": "mistaken", "french": "se méprendre"}, 
        {"base": "pay", "past_simple": "paid", "past_participle": "paid", "french": "payer"}, 
        {"base": "put", "past_simple": "put", "past_participle": "put", "french": "mettre, poser"}, 
        {"base": "read", "past_simple": "read", "past_participle": "read", "french": "lire"}, 
        {"base": "rid", "past_simple": "rid", "past_participle": "rid", "french": "débarasser"}, 
        {"base": "ride", "past_simple": "rode", "past_participle": "ridden", "french": "monter (à cheval, à bicyclette)"}, 
        {"base": "ring", "past_simple": "rang", "past_participle": "rung", "french": "sonner"}, 
        {"base": "rise", "past_simple": "rose", "past_participle": "risen", "french": "se lever, monter, s'élever"}, 
        {"base": "run", "past_simple": "ran", "past_participle": "run", "french": "courir"}, 
        {"base": "say", "past_simple": "said", "past_participle": "said", "french": "dire"}, 
        {"base": "see", "past_simple": "saw", "past_participle": "seen", "french": "voir"}, 
        {"base": "seek", "past_simple": "sought", "past_participle": "sought", "french": "chercher"}, 
        {"base": "sell", "past_simple": "sold", "past_participle": "sold", "french": "vendre"}, 
        {"base": "send", "past_simple": "sent", "past_participle": "sent", "french": "envoyer"}, 
        {"base": "set", "past_simple": "set", "past_participle": "set", "french": "fixer, mettre"}, 
        {"base": "sew", "past_simple": "sewed", "past_participle": "sewn", "french": "coudre"}, 
        {"base": "shake", "past_simple": "shook", "past_participle": "shaken", "french": "trembler, secouer"}, 
        {"base": "shoot", "past_simple": "shot", "past_participle": "shot", "french": "tirer (arme)"}, 
        {"base": "shine", "past_simple": "shone", "past_participle": "shone", "french": "briller"}, 
        {"base": "show", "past_simple": "showed", "past_participle": "shown", "french": "montrer"}, 
        {"base": "shrink", "past_simple": "shrank", "past_participle": "shrunk", "french": "rétrécir"}, 
        {"base": "shut", "past_simple": "shut", "past_participle": "shut", "french": "fermer"}, 
        {"base": "sing", "past_simple": "sang", "past_participle": "sung", "french": "chanter"}, 
        {"base": "sit", "past_simple": "sat", "past_participle": "sat", "french": "s'assoir"}, 
        {"base": "sleep", "past_simple": "slept", "past_participle": "slept", "french": "dormir"}
    ];

    // --- STATE ---
    let remainingVerbsPool = [...allVerbs]; // Verbs not yet seen
    let currentBatch = [];                  // Active queue (Leitner box 1)
    let masteredCount = 0;                  // Total mastered
    let currentVerb = null;
    let batchSize = 10;
    let batchNumber = 1;

    // --- DOM ---
    const ui = {
        french: document.getElementById('frenchDisplay'),
        base: document.getElementById('inBase'),
        past: document.getElementById('inPast'),
        part: document.getElementById('inPart'),
        btnCheck: document.getElementById('btnCheck'),
        btnNext: document.getElementById('btnNext'),
        correctionBox: document.getElementById('correctionBox'),
        correctionText: document.getElementById('correctionText'),
        queueCount: document.getElementById('queueCount'),
        progressBar: document.getElementById('progressBar'),
        progressText: document.getElementById('progressText'),
        batchInd: document.getElementById('batchIndicator'),
        learningInterface: document.getElementById('learningInterface'),
        completionScreen: document.getElementById('completionScreen')
    };

    // --- LOGIC ---
    

    function init() {
        startNextBatch();
    }

    function startNextBatch() {
        // Reset UI
        ui.learningInterface.style.display = 'block';
        ui.completionScreen.style.display = 'none';
        
        // Load new verbs if pool is not empty
        if (remainingVerbsPool.length > 0) {
            const nextChunk = remainingVerbsPool.slice(0, batchSize);
            remainingVerbsPool = remainingVerbsPool.slice(batchSize);
            currentBatch = nextChunk;
            batchNumber++;
            updateStats();
            nextCard();
        } else {
            // FIN DU JEU GLOBAL
            alert("Félicitations ! Tu as terminé les 102 verbes !");
        }
    }

    function updateStats() {
        ui.queueCount.textContent = `Reste dans cette série : ${currentBatch.length}`;
        const total = allVerbs.length;
        const pct = (masteredCount / total) * 100;
        ui.progressBar.style.width = `${pct}%`;
        ui.progressText.textContent = `${masteredCount} / ${total} maîtrisés`;
        
        // Calculate Batch Number roughly
        const currentLevel = Math.ceil((masteredCount + 1) / 10);
        ui.batchInd.textContent = `Niveau ${currentLevel}`;
    }

    function nextCard() {
        if (currentBatch.length === 0) {
            // Batch finished
            ui.learningInterface.style.display = 'none';
            ui.completionScreen.style.display = 'block';
            return;
        }

        // Clean inputs
        [ui.base, ui.past, ui.part].forEach(i => {
            i.value = '';
            i.className = '';
            i.disabled = false;
        });
        ui.correctionBox.classList.remove('visible');
        ui.btnCheck.style.display = 'block';
        ui.btnNext.style.display = 'none';
        
        // Focus first input
        ui.base.focus();

        // Get first verb in queue (FIFO)
        currentVerb = currentBatch[0];
        ui.french.textContent = currentVerb.french;
        updateStats();
    }

    function cleanStr(s) {
        return s.toLowerCase().trim().replace(/\s+/g, ' ');
    }

    function isCorrect(input, answer) {
        const cleanInput = cleanStr(input);
        const cleanAnswer = cleanStr(answer);
        
        // Handle "was / were" logic
        if (cleanAnswer.includes('/')) {
            const parts = cleanAnswer.split('/').map(p => p.trim());
            return parts.includes(cleanInput) || cleanAnswer === cleanInput || cleanAnswer.replace(/\s/g,'') === cleanInput.replace(/\s/g,'');
        }
        return cleanInput === cleanAnswer;
    }

    function checkAnswer() {
        const b = ui.base.value;
        const p = ui.past.value;
        const pp = ui.part.value;

        const bOk = isCorrect(b, currentVerb.base);
        const pOk = isCorrect(p, currentVerb.past_simple);
        const ppOk = isCorrect(pp, currentVerb.past_participle);

        // Styling inputs
        ui.base.className = bOk ? 'correct' : 'incorrect';
        ui.past.className = pOk ? 'correct' : 'incorrect';
        ui.part.className = ppOk ? 'correct' : 'incorrect';

        if (bOk && pOk && ppOk) {
            // SUCCESS
            // Remove from queue (shift)
            currentBatch.shift();
            masteredCount++;
            
            // Auto continue after short delay
            ui.btnCheck.style.display = 'none';
            setTimeout(nextCard, 1000); // 1s delay to see green
        } else {
            // ERROR
            // Show correction
            ui.correctionText.textContent = `${currentVerb.base} • ${currentVerb.past_simple} • ${currentVerb.past_participle}`;
            ui.correctionBox.classList.add('visible');
            
            // Move to end of queue (Recycle)
            const failed = currentBatch.shift();
            currentBatch.push(failed);

            // Switch buttons
            ui.btnCheck.style.display = 'none';
            ui.btnNext.style.display = 'block';
            ui.btnNext.focus();
            
            // Disable inputs so user looks at correction
            [ui.base, ui.past, ui.part].forEach(i => i.disabled = true);
        }
    }

    // Enter key handling
    document.addEventListener('keydown', (e) => {
        if (e.key === 'Enter') {
            if (ui.btnCheck.style.display !== 'none') {
                checkAnswer();
            } else if (ui.btnNext.style.display !== 'none') {
                nextCard();
            }
        }
    });

    // Start
    init();

</script>

</body>
</html>
