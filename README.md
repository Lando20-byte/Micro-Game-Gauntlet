# Micro-Game-Gauntlet
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Micro-Game Gauntlet: So Many Games!</title>
    <style>
        :root {
            --primary: #ff0055;
            --secondary: #00e5ff;
            --accent: #ffcc00;
            --dark: #1a1a2e;
            --darker: #16213e;
            --light: #ffffff;
            --font-main: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        * {
            box-sizing: border-box;
            user-select: none;
            -webkit-user-select: none;
            touch-action: manipulation;
        }

        body {
            margin: 0;
            padding: 0;
            background-color: var(--dark);
            color: var(--light);
            font-family: var(--font-main);
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        /* Container for the game console */
        #game-container {
            width: 100%;
            max-width: 600px;
            height: 100%;
            max-height: 800px;
            background-color: var(--darker);
            position: relative;
            display: flex;
            flex-direction: column;
            box-shadow: 0 0 20px rgba(0,0,0,0.5);
            border: 4px solid var(--secondary);
            border-radius: 8px;
            overflow: hidden;
        }

        /* Header / HUD */
        header {
            background-color: rgba(0, 0, 0, 0.8);
            padding: 10px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            height: 60px;
            border-bottom: 2px solid var(--secondary);
            z-index: 10;
        }

        .hud-item {
            font-size: 1.2rem;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 5px;
        }

        #score-display { color: var(--accent); }
        #lives-display { color: var(--primary); }
        #timer-bar-container {
            position: absolute;
            top: 60px;
            left: 0;
            width: 100%;
            height: 6px;
            background: #333;
        }
        #timer-bar {
            height: 100%;
            background: var(--primary);
            width: 100%;
            transition: width 0.1s linear;
        }

        /* Main Game Area */
        #stage {
            flex: 1;
            position: relative;
            background: #222;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            overflow: hidden;
        }

        #instruction-text {
            position: absolute;
            top: 20px;
            font-size: 1.5rem;
            font-weight: 900;
            text-transform: uppercase;
            text-align: center;
            width: 100%;
            text-shadow: 2px 2px 0px #000;
            z-index: 5;
            padding: 0 10px;
        }

        /* Overlay Screens (Menu, Game Over) */
        .overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(26, 26, 46, 0.95);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 100;
            text-align: center;
            padding: 20px;
        }

        .hidden { display: none !important; }

        h1 {
            font-size: 3rem;
            margin: 0 0 20px 0;
            color: var(--secondary);
            text-shadow: 4px 4px 0 var(--primary);
            animation: bounce 2s infinite;
        }

        p { font-size: 1.2rem; margin-bottom: 30px; line-height: 1.5; }

        button.btn-primary {
            background: var(--primary);
            color: white;
            border: none;
            padding: 15px 40px;
            font-size: 1.5rem;
            font-weight: bold;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 6px 0 #990033;
            transition: transform 0.1s, box-shadow 0.1s;
        }

        button.btn-primary:active {
            transform: translateY(4px);
            box-shadow: 0 2px 0 #990033;
        }

        button.btn-primary:hover {
            filter: brightness(1.1);
        }

        /* Mini-Game Common Elements */
        .game-element {
            position: absolute;
            transition: all 0.2s;
            cursor: pointer;
        }

        /* --- SPECIFIC MINI-GAME STYLES --- */

        /* Game 1: Click Red */
        #game-click-target {
            width: 80px;
            height: 80px;
            border-radius: 50%;
            background: var(--primary);
            box-shadow: 0 0 15px var(--primary);
        }
        .target-blue { background: var(--secondary) !important; box-shadow: 0 0 15px var(--secondary) !important; }

        /* Game 2: Maze/Wall */
        #game-player {
            width: 30px;
            height: 30px;
            background: var(--accent);
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
        }
        #game-obstacle {
            position: absolute;
            top: -50px;
            width: 100px;
            height: 100px;
            background: red;
            border-radius: 10px;
        }

        /* Game 3: Math */
        #math-question { font-size: 2.5rem; margin-bottom: 20px; font-weight: bold; }
        .math-options { display: flex; gap: 20px; }
        .math-btn {
            background: #444; border: 2px solid #666; color: white; padding: 10px 20px; font-size: 1.5rem; cursor: pointer; border-radius: 8px;
        }
        .math-btn:hover { background: #666; }

        /* Game 4: Catch */
        #catch-object {
            width: 40px; height: 40px; background: var(--accent); border-radius: 50%; position: absolute;
        }
        #catch-player {
            width: 80px; height: 20px; background: var(--secondary); position: absolute; bottom: 20px; left: 50%; transform: translateX(-50%);
        }

        /* Game 5: Type */
        #type-area {
            width: 80%; padding: 10px; font-size: 1.5rem; text-align: center; border: 2px solid #555; background: #000; color: white; outline: none;
        }

        /* Game 6: Stop in Zone */
        #slider-track { width: 80%; height: 20px; background: #333; border-radius: 10px; position: relative; margin-top: 50px; }
        #slider-zone { height: 100%; background: #0f0; position: absolute; left: 40%; width: 20%; }
        #slider-cursor { height: 40px; width: 10px; background: white; position: absolute; top: -10px; left: 0; }

        /* Game 7: Memory */
        .mem-card { width: 60px; height: 60px; background: #444; border: 2px solid #fff; cursor: pointer; display: inline-block; margin: 5px; line-height: 60px; font-size: 2rem; }
        .mem-card.revealed { background: var(--accent); color: #000; transform: rotateY(180deg); }

        /* Game 8: Shake */
        #shake-meter { width: 200px; height: 20px; background: #333; border: 2px solid white; margin-top: 20px; }
        #shake-fill { width: 0%; height: 100%; background: var(--primary); transition: width 0.1s; }

        /* Game 9: Unscramble */
        #scramble-word { font-size: 2rem; letter-spacing: 5px; margin-bottom: 20px; font-weight: bold; color: var(--secondary); }
        #scramble-input { padding: 10px; font-size: 1.5rem; text-align: center; background: #222; color: white; border: 1px solid #555; width: 200px; }

        /* Game 10: Reverse */
        #reverse-text { font-size: 2rem; font-weight: bold; }

        /* Animations */
        @keyframes bounce { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-10px); } }
        @keyframes shake-anim { 0% { transform: translate(1px, 1px) rotate(0deg); } 10% { transform: translate(-1px, -2px) rotate(-1deg); } 20% { transform: translate(-3px, 0px) rotate(1deg); } 30% { transform: translate(3px, 2px) rotate(0deg); } 40% { transform: translate(1px, -1px) rotate(1deg); } 50% { transform: translate(-1px, 2px) rotate(-1deg); } 60% { transform: translate(-3px, 1px) rotate(0deg); } 70% { transform: translate(3px, 1px) rotate(-1deg); } 80% { transform: translate(-1px, -1px) rotate(1deg); } 90% { transform: translate(1px, 2px) rotate(0deg); } 100% { transform: translate(1px, -2px) rotate(-1deg); } }
        .shaking { animation: shake-anim 0.5s; animation-iteration-count: infinite; }

        /* Feedback Text */
        #feedback {
            position: absolute;
            top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            font-size: 4rem;
            font-weight: 900;
            opacity: 0;
            pointer-events: none;
            z-index: 50;
        }
        .win-feedback { color: #0f0; animation: popIn 0.5s forwards; }
        .lose-feedback { color: #f00; animation: popIn 0.5s forwards; }

        @keyframes popIn {
            0% { transform: translate(-50%, -50%) scale(0); opacity: 0; }
            50% { transform: translate(-50%, -50%) scale(1.2); opacity: 1; }
            100% { transform: translate(-50%, -50%) scale(1); opacity: 0; }
        }

    </style>
</head>
<body>

<div id="game-container">
    <header>
        <div class="hud-item">SCORE: <span id="score-display">0</span></div>
        <div class="hud-item">LIVES: <span id="lives-display">❤❤❤</span></div>
    </header>
    <div id="timer-bar-container"><div id="timer-bar"></div></div>

    <div id="stage">
        <div id="instruction-text">GET READY</div>
        
        <!-- Dynamic Game Content Container -->
        <div id="game-layer" style="width: 100%; height: 100%; position: relative;"></div>
        
        <!-- Feedback Overlay -->
        <div id="feedback"></div>
    </div>

    <!-- Start Menu -->
    <div id="start-menu" class="overlay">
        <h1>MICRO<br>GAUNTLET</h1>
        <p>10 Different Mini-Games.<br>Speed increases every round.<br>Don't run out of time!</p>
        <button class="btn-primary" onclick="startGame()">START MISSION</button>
    </div>

    <!-- Game Over Screen -->
    <div id="game-over-menu" class="overlay hidden">
        <h1 style="color: var(--primary)">GAME OVER</h1>
        <p>You survived <span id="final-score" style="color: var(--accent); font-weight: bold;">0</span> rounds!</p>
        <button class="btn-primary" onclick="resetGame()">TRY AGAIN</button>
    </div>
</div>

<script>
    /* --- GAME ENGINE --- */
    const gameContainer = document.getElementById('game-layer');
    const instructionText = document.getElementById('instruction-text');
    const timerBar = document.getElementById('timer-bar');
    const scoreDisplay = document.getElementById('score-display');
    const livesDisplay = document.getElementById('lives-display');
    const startMenu = document.getElementById('start-menu');
    const gameOverMenu = document.getElementById('game-over-menu');
    const finalScoreSpan = document.getElementById('final-score');
    const feedbackEl = document.getElementById('feedback');

    let score = 0;
    let lives = 3;
    let timeLimit = 5000; // Starts at 5 seconds
    let currentTime = 0;
    let gameLoopId;
    let isGameRunning = false;
    let currentGameIndex = -1;
    let difficultyMultiplier = 1;

    // List of all mini-games
    const games = [
        { id: 'clickRed', name: 'Click the Red!', init: initClickRed, cleanup: cleanupGeneric },
        { id: 'dodge', name: 'Dodge!', init: initDodge, cleanup: cleanupDodge },
        { id: 'math', name: 'Solve It!', init: initMath, cleanup: cleanupGeneric },
        { id: 'catch', name: 'Catch!', init: initCatch, cleanup: cleanupCatch },
        { id: 'type', name: 'Type "GO"', init: initType, cleanup: cleanupGeneric },
        { id: 'stopZone', name: 'Stop in Green!', init: initStopZone, cleanup: cleanupGeneric },
        { id: 'memory', name: 'Find Match', init: initMemory, cleanup: cleanupGeneric },
        { id: 'shake', name: 'Shake Device!', init: initShake, cleanup: cleanupGeneric },
        { id: 'unscramble', name: 'Unscramble!', init: initUnscramble, cleanup: cleanupGeneric },
        { id: 'reverse', name: 'Don\'t Click!', init: initReverse, cleanup: cleanupGeneric }
    ];

    function startGame() {
        score = 0;
        lives = 3;
        difficultyMultiplier = 1;
        updateHUD();
        startMenu.classList.add('hidden');
        gameOverMenu.classList.add('hidden');
        isGameRunning = true;
        nextRound();
    }

    function resetGame() {
        gameOverMenu.classList.add('hidden');
        startMenu.classList.remove('hidden');
    }

    function updateHUD() {
        scoreDisplay.textContent = score;
        livesDisplay.textContent = '❤'.repeat(Math.max(0, lives));
    }

    function showFeedback(type) {
        feedbackEl.className = type === 'win' ? 'win-feedback' : 'lose-feedback';
        feedbackEl.textContent = type === 'win' ? 'NICE!' : 'FAIL!';
        feedbackEl.style.animation = 'none';
        feedbackEl.offsetHeight; /* trigger reflow */
        feedbackEl.style.animation = null; 
    }

    function nextRound() {
        if (!isGameRunning) return;

        // Increase difficulty
        timeLimit = Math.max(1500, 5000 - (score * 100)); 
        difficultyMultiplier = 1 + (score * 0.1);

        // Pick random game
        const nextGameIdx = Math.floor(Math.random() * games.length);
        currentGameIndex = nextGameIdx;
        const game = games[currentGameIndex];

        // Setup UI
        instructionText.textContent = game.name;
        instructionText.style.color = '#fff';
        gameContainer.innerHTML = ''; // Clear previous game
        timerBar.style.width = '100%';
        timerBar.style.backgroundColor = 'var(--primary)';

        // Start Timer
        let startTime = Date.now();
        
        if (gameLoopId) cancelAnimationFrame(gameLoopId);

        function loop() {
            if (!isGameRunning) return;
            let elapsed = Date.now() - startTime;
            let remaining = timeLimit - elapsed;
            
            let pct = (remaining / timeLimit) * 100;
            timerBar.style.width = pct + '%';

            // Color change on urgency
            if (pct < 30) timerBar.style.backgroundColor = 'red';

            if (remaining <= 0) {
                loseGame("Time's Up!");
            } else {
                gameLoopId = requestAnimationFrame(loop);
            }
        }
        gameLoopId = requestAnimationFrame(loop);

        // Init Specific Game Logic
        game.init();
    }

    function winGame() {
        if (!isGameRunning) return;
        showFeedback('win');
        score++;
        updateHUD();
        // Cleanup current game before moving to next
        if (games[currentGameIndex].cleanup) games[currentGameIndex].cleanup();
        
        // Small delay before next round
        isGameRunning = false; // Pause loop temporarily
        setTimeout(() => {
            isGameRunning = true;
            nextRound();
        }, 1000);
    }

    function loseGame(reason) {
        if (!isGameRunning) return;
        showFeedback('lose');
        lives--;
        updateHUD();
        
        if (games[currentGameIndex].cleanup) games[currentGameIndex].cleanup();

        if (lives <= 0) {
            gameOver();
        } else {
            isGameRunning = false;
            instructionText.textContent = reason || "Ouch!";
            setTimeout(() => {
                isGameRunning = true;
                nextRound();
            }, 1500);
        }
    }

    function gameOver() {
        isGameRunning = false;
        cancelAnimationFrame(gameLoopId);
        finalScoreSpan.textContent = score;
        gameOverMenu.classList.remove('hidden');
    }

    /* --- CLEANUP UTILS --- */
    function cleanupGeneric() {
        gameContainer.innerHTML = '';
        document.removeEventListener('keydown', globalKeyHandler);
    }

    /* --- GAME 1: CLICK RED --- */
    function initClickRed() {
        const target = document.createElement('div');
        target.id = 'game-click-target';
        
        // Random position
        const x = Math.random() * (gameContainer.clientWidth - 80);
        const y = Math.random() * (gameContainer.clientHeight - 150) + 50; // avoid header/instruction
        
        target.style.left = x + 'px';
        target.style.top = y + 'px';

        // Sometimes make it blue (trap)
        const isTrap = Math.random() > 0.8;
        if (isTrap) target.classList.add('target-blue');

        target.onmousedown = (e) => {
            e.stopPropagation();
            if (isTrap) loseGame("Wrong Color!");
            else winGame();
        };
        
        // Add decoys
        for(let i=0; i<3; i++) {
            let decoy = document.createElement('div');
            decoy.className = 'game-element target-blue';
            decoy.style.width = '60px'; decoy.style.height = '60px'; decoy.style.borderRadius = '50%';
            decoy.style.left = (Math.random() * (gameContainer.clientWidth - 60)) + 'px';
            decoy.style.top = (Math.random() * (gameContainer.clientHeight - 150) + 50) + 'px';
            decoy.onmousedown = (e) => { e.stopPropagation(); loseGame("Wrong Color!"); };
            gameContainer.appendChild(decoy);
        }

        gameContainer.appendChild(target);
    }

    /* --- GAME 2: DODGE --- */
    function cleanupDodge() {
        document.removeEventListener('mousemove', dodgeMoveHandler);
        document.removeEventListener('touchmove', dodgeTouchHandler);
    }

    let dodgePlayer;
    function initDodge() {
        dodgePlayer = document.createElement('div');
        dodgePlayer.id = 'game-player';
        gameContainer.appendChild(dodgePlayer);

        document.addEventListener('mousemove', dodgeMoveHandler);
        document.addEventListener('touchmove', dodgeTouchHandler);

        let obstacleInterval = setInterval(() => {
            if(!isGameRunning) { clearInterval(obstacleInterval); return; }
            spawnObstacle();
        }, 400 / difficultyMultiplier);
        
        dodgePlayer.obstacleInterval = obstacleInterval; // Attach to clear later
    }

    function dodgeMoveHandler(e) {
        if(!isGameRunning) return;
        const rect = gameContainer.getBoundingClientRect();
        let x = e.clientX - rect.left - 15;
        // Keep inside bounds
        if(x < 0) x = 0;
        if(x > rect.width - 30) x = rect.width - 30;
        dodgePlayer.style.left = x + 'px';
    }

    function dodgeTouchHandler(e) {
        if(!isGameRunning) return;
        e.preventDefault(); // prevent scroll
        const rect = gameContainer.getBoundingClientRect();
        let x = e.touches[0].clientX - rect.left - 15;
        if(x < 0) x = 0;
        if(x > rect.width - 30) x = rect.width - 30;
        dodgePlayer.style.left = x + 'px';
    }

    function spawnObstacle() {
        let obs = document.createElement('div');
        obs.id = 'game-obstacle';
        obs.style.left = Math.random() * (gameContainer.clientWidth - 100) + 'px';
        gameContainer.appendChild(obs);

        let pos = -50;
        let speed = 5 * difficultyMultiplier;

        function fall() {
            if(!isGameRunning || !obs.parentNode) return;
            pos += speed;
            obs.style.top = pos + 'px';

            // Collision
            let pRect = dodgePlayer.getBoundingClientRect();
            let oRect = obs.getBoundingClientRect();

            if (!(pRect.right < oRect.left || 
                  pRect.left > oRect.right || 
                  pRect.bottom < oRect.top || 
                  pRect.top > oRect.bottom)) {
                loseGame("Crashed!");
            }

            if (pos > gameContainer.clientHeight) {
                winGame(); // Survived one wave logic (simplified: win immediately on first dodge in this micro version)
            } else {
                requestAnimationFrame(fall);
            }
        }
        requestAnimationFrame(fall);
    }

    /* --- GAME 3: MATH --- */
    function initMath() {
        const a = Math.floor(Math.random() * 10) + 1;
        const b = Math.floor(Math.random() * 10) + 1;
        const isAddition = Math.random() > 0.5;
        const answer = isAddition ? a + b : a * b;
        const symbol = isAddition ? '+' : '×';

        const qDiv = document.createElement('div');
        qDiv.id = 'math-question';
        qDiv.textContent = `${a} ${symbol} ${b} = ?`;
        
        const btnContainer = document.createElement('div');
        btnContainer.className = 'math-options';

        // Generate wrong answers
        let options = [answer];
        while(options.length < 3) {
            let wrong = answer + Math.floor(Math.random() * 10) - 5;
            if(wrong !== answer && wrong > 0 && !options.includes(wrong)) options.push(wrong);
        }
        // Shuffle
        options.sort(() => Math.random() - 0.5);

        options.forEach(opt => {
            let btn = document.createElement('button');
            btn.className = 'math-btn';
            btn.textContent = opt;
            btn.onclick = () => {
                if(opt === answer) winGame();
                else loseGame("Wrong Answer!");
            };
            btnContainer.appendChild(btn);
        });

        // Center everything
        const wrapper = document.createElement('div');
        wrapper.style.display = 'flex';
        wrapper.style.flexDirection = 'column';
        wrapper.style.alignItems = 'center';
        wrapper.appendChild(qDiv);
        wrapper.appendChild(btnContainer);
        gameContainer.appendChild(wrapper);
    }

    /* --- GAME 4: CATCH --- */
    function cleanupCatch() {
        document.removeEventListener('mousemove', catchMoveHandler);
        document.removeEventListener('touchmove', catchTouchHandler);
    }

    let catchPlayer;
    function initCatch() {
        catchPlayer = document.createElement('div');
        catchPlayer.id = 'catch-player';
        gameContainer.appendChild(catchPlayer);

        let obj = document.createElement('div');
        obj.id = 'catch-object';
        obj.style.left = '50%';
        obj.style.top = '0';
        gameContainer.appendChild(obj);

        document.addEventListener('mousemove', catchMoveHandler);
        document.addEventListener('touchmove', catchTouchHandler);

        let objX = 50; // percentage
        let objY = 0;
        let speed = 2 * difficultyMultiplier;

        function fall() {
            if(!isGameRunning) return;
            objY += speed;
            obj.style.top = objY + 'px';

            // Get player X (pixel)
            let pRect = catchPlayer.getBoundingClientRect();
            let oRect = obj.getBoundingClientRect();

            // Collision
            if (!(pRect.right < oRect.left || 
                  pRect.left > oRect.right || 
                  pRect.bottom < oRect.top || 
                  pRect.top > oRect.bottom)) {
                winGame();
                return;
            }

            if (objY > gameContainer.clientHeight) {
                loseGame("Missed!");
            } else {
                requestAnimationFrame(fall);
            }
        }
        requestAnimationFrame(fall);
    }

    function catchMoveHandler(e) {
        if(!isGameRunning) return;
        const rect = gameContainer.getBoundingClientRect();
        let x = e.clientX - rect.left - 40; // center 80px width
        if(x < 0) x = 0;
        if(x > rect.width - 80) x = rect.width - 80;
        catchPlayer.style.left = x + 'px';
    }
    
    function catchTouchHandler(e) {
        if(!isGameRunning) return;
        e.preventDefault();
        const rect = gameContainer.getBoundingClientRect();
        let x = e.touches[0].clientX - rect.left - 40;
        if(x < 0) x = 0;
        if(x > rect.width - 80) x = rect.width - 80;
        catchPlayer.style.left = x + 'px';
    }


    /* --- GAME 5: TYPE "GO" --- */
    function initType() {
        const input = document.createElement('input');
        input.type = 'text';
        input.id = 'type-area';
        input.placeholder = 'Type GO here';
        input.autofocus = true;
        
        input.addEventListener('input', (e) => {
            if(e.target.value.toUpperCase() === 'GO') {
                winGame();
            }
        });

        // Ensure focus
        setTimeout(() => input.focus(), 100);

        gameContainer.appendChild(input);
    }

    /* --- GAME 6: STOP IN ZONE --- */
    function initStopZone() {
        const track = document.createElement('div');
        track.id = 'slider-track';
        
        const zone = document.createElement('div');
        zone.id = 'slider-zone';
        // Randomize zone position
        let zoneLeft = 20 + Math.random() * 60;
        zone.style.left = zoneLeft + '%';
        
        const cursor = document.createElement('div');
        cursor.id = 'slider-cursor';

        track.appendChild(zone);
        track.appendChild(cursor);
        gameContainer.appendChild(track);

        const btn = document.createElement('button');
        btn.className = 'btn-primary';
        btn.style.marginTop = '20px';
        btn.style.fontSize = '1rem';
        btn.style.padding = '10px 20px';
        btn.textContent = 'STOP!';
        
        let pos = 0;
        let direction = 1;
        let speed = 1.5 * difficultyMultiplier;
        let animId;

        function move() {
            if(!isGameRunning) return;
            pos += speed * direction;
            if(pos >= 100 || pos <= 0) direction *= -1;
            cursor.style.left = pos + '%';
            animId = requestAnimationFrame(move);
        }
        move();

        btn.onclick = () => {
            cancelAnimationFrame(animId);
            // Check collision
            if (pos >= zoneLeft && pos <= (zoneLeft + 20)) {
                winGame();
            } else {
                loseGame("Missed Zone!");
            }
        };

        gameContainer.appendChild(btn);
    }

    /* --- GAME 7: MEMORY (Simple) --- */
    function initMemory() {
        const symbols = ['A', 'B', 'C', 'D'];
        // Pick one pair
        let pick = symbols[Math.floor(Math.random() * symbols.length)];
        let deck = [pick, pick];
        
        // Add 2 decoys
        while(deck.length < 4) {
            let d = symbols[Math.floor(Math.random() * symbols.length)];
            if(d !== pick) deck.push(d);
        }
        deck.sort(() => Math.random() - 0.5);

        let flipped = 0;
        let found = 0;

        deck.forEach((sym, index) => {
            let card = document.createElement('div');
            card.className = 'mem-card';
            card.dataset.sym = sym;
            card.textContent = '?';
            
            card.onclick = () => {
                if(card.classList.contains('revealed') || flipped >= 2) return;
                
                flipped++;
                card.textContent = sym;
                card.classList.add('revealed');

                if(flipped === 2) {
                    // Check match
                    let revealed = document.querySelectorAll('.mem-card.revealed');
                    let s1 = revealed[0].dataset.sym;
                    let s2 = revealed[1].dataset.sym;

                    if(s1 === s2) {
                        winGame();
                    } else {
                        setTimeout(() => loseGame("No Match!"), 500);
                    }
                }
            };
            gameContainer.appendChild(card);
        });
    }

    /* --- GAME 8: SHAKE (Mobile mostly, fallback click for desktop) --- */
    let shakeCount = 0;
    let lastX = 0;
    let lastY = 0;
    
    function initShake() {
        shakeCount = 0;
        const meter = document.createElement('div');
        meter.id = 'shake-meter';
        const fill = document.createElement('div');
        fill.id = 'shake-fill';
        meter.appendChild(fill);
        
        const hint = document.createElement('div');
        hint.textContent = "Shake device or Click Rapidly!";
        hint.style.marginTop = "10px";
        hint.style.fontSize = "0.9rem";

        gameContainer.appendChild(meter);
        gameContainer.appendChild(hint);

        // Shake logic
        window.addEventListener('devicemotion', handleMotion);
        
        // Click fallback for desktop
        gameContainer.onclick = () => {
            if(!isGameRunning) return;
            shakeCount += 5;
            updateShake(fill);
        };
    }

    function handleMotion(event) {
        if(!isGameRunning) return;
        let x = event.accelerationIncludingGravity.x;
        let y = event.accelerationIncludingGravity.y;
        
        if (Math.abs(x - lastX) > 5 || Math.abs(y - lastY) > 5) {
            shakeCount++;
            updateShake(document.getElementById('shake-fill'));
        }
        lastX = x;
        lastY = y;
    }

    function updateShake(el) {
        if(!el) return;
        let pct = Math.min(100, shakeCount * 2);
        el.style.width = pct + '%';
        if(pct >= 100) {
            window.removeEventListener('devicemotion', handleMotion);
            winGame();
        }
    }

    /* --- GAME 9: UNSCRAMBLE --- */
    function initUnscramble() {
        const words = ['GAME', 'PLAY', 'CODE', 'FAST', 'WEB', 'HTML', 'JAVA'];
        let word = words[Math.floor(Math.random() * words.length)];
        let scrambled = word.split('').sort(() => Math.random() - 0.5).join('');
        
        const display = document.createElement('div');
        display.id = 'scramble-word';
        display.textContent = scrambled;

        const input = document.createElement('input');
        input.type = 'text';
        input.id = 'scramble-input';
        input.placeholder = 'Answer?';
        input.autofocus = true;
        input.maxLength = word.length;

        input.addEventListener('input', (e) => {
            if(e.target.value.toUpperCase() === word) {
                winGame();
            }
        });

        setTimeout(() => input.focus(), 100);

        gameContainer.appendChild(display);
        gameContainer.appendChild(input);
    }

    /* --- GAME 10: REVERSE PSYCHOLOGY --- */
    function initReverse() {
        instructionText.textContent = "DO NOT CLICK!";
        instructionText.style.color = "var(--primary)";
        
        gameContainer.onclick = () => {
            loseGame("I said DON'T!");
        };

        // Auto win after 2 seconds if they don't click
        setTimeout(() => {
            if(isGameRunning) winGame();
        }, 2000);
    }

</script>
</body>
</html>
