
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Neon Snake Adventure</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
            height: 100vh;
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            color: #fff;
            position: relative;
        }

        body::before {
            content: "";
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: 
                radial-gradient(circle at 20% 30%, rgba(255, 105, 180, 0.15) 0%, transparent 40%),
                radial-gradient(circle at 80% 70%, rgba(65, 105, 225, 0.15) 0%, transparent 40%);
            z-index: -1;
        }

        .game-container {
            width: 95%;
            max-width: 1200px;
            height: 90vh;
            display: flex;
            flex-direction: column;
            background: rgba(15, 15, 35, 0.85);
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.5);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            position: relative;
        }

        .header {
            padding: 20px 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(25, 25, 55, 0.7);
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .logo i {
            font-size: 2.5rem;
            color: #ff7e5f;
            text-shadow: 0 0 15px rgba(255, 126, 95, 0.7);
        }

        .logo h1 {
            font-size: 2.2rem;
            background: linear-gradient(45deg, #ff7e5f, #feb47b, #86a8e7);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            letter-spacing: 1px;
        }

        .stats {
            display: flex;
            gap: 30px;
        }

        .stat-box {
            display: flex;
            flex-direction: column;
            align-items: center;
            min-width: 120px;
            padding: 10px 20px;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 15px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .stat-value {
            font-size: 2.2rem;
            font-weight: bold;
            color: #feb47b;
        }

        .stat-label {
            font-size: 0.9rem;
            color: #a0d2eb;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .level-progress {
            width: 200px;
            height: 12px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            overflow: hidden;
            position: relative;
        }

        .level-progress-bar {
            height: 100%;
            background: linear-gradient(90deg, #ff7e5f, #feb47b);
            border-radius: 10px;
            transition: width 0.3s ease;
        }

        .level-indicator {
            position: absolute;
            top: -25px;
            left: 0;
            font-size: 0.9rem;
            color: #86a8e7;
        }

        .game-area {
            flex: 1;
            display: flex;
            position: relative;
            overflow: hidden;
        }

        #game-board {
            flex: 1;
            position: relative;
        }

        canvas {
            display: block;
            width: 100%;
            height: 100%;
        }

        .controls {
            padding: 20px;
            display: flex;
            justify-content: center;
            gap: 20px;
            background: rgba(25, 25, 55, 0.7);
            border-top: 1px solid rgba(255, 255, 255, 0.1);
        }

        .btn {
            padding: 15px 30px;
            font-size: 1.1rem;
            font-weight: bold;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 10px;
            background: linear-gradient(45deg, #ff7e5f, #feb47b);
            color: white;
            box-shadow: 0 5px 15px rgba(255, 126, 95, 0.4);
        }

        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(255, 126, 95, 0.6);
        }

        .btn:active {
            transform: translateY(1px);
        }

        .btn-secondary {
            background: rgba(255, 255, 255, 0.15);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        .btn-secondary:hover {
            background: rgba(255, 255, 255, 0.25);
        }

        .overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background: rgba(15, 15, 35, 0.9);
            z-index: 10;
            padding: 30px;
            text-align: center;
        }

        .overlay h2 {
            font-size: 4rem;
            margin-bottom: 20px;
            background: linear-gradient(45deg, #ff7e5f, #feb47b);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 5px 15px rgba(255, 126, 95, 0.5);
        }

        .overlay p {
            font-size: 1.5rem;
            max-width: 600px;
            margin-bottom: 30px;
            color: #a0d2eb;
            line-height: 1.6;
        }

        .hidden {
            display: none;
        }

        .instructions {
            margin-top: 30px;
            padding: 20px;
            background: rgba(0, 0, 0, 0.2);
            border-radius: 15px;
            max-width: 600px;
        }

        .instructions h3 {
            color: #feb47b;
            margin-bottom: 15px;
            font-size: 1.3rem;
        }

        .instructions ul {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 20px;
            list-style: none;
        }

        .instructions li {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 1rem;
            color: #86a8e7;
        }

        .instructions i {
            color: #ff7e5f;
        }

        .level-up-animation {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            pointer-events: none;
            opacity: 0;
            z-index: 5;
        }

        .level-text {
            font-size: 8rem;
            font-weight: bold;
            color: rgba(255, 255, 255, 0.1);
            text-transform: uppercase;
            letter-spacing: 10px;
        }

        .mobile-controls {
            display: none;
            position: absolute;
            bottom: 100px;
            width: 100%;
            justify-content: center;
            gap: 30px;
            z-index: 4;
        }

        .dpad {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            grid-template-rows: repeat(3, 1fr);
            gap: 10px;
            width: 150px;
            height: 150px;
        }

        .dpad-btn {
            background: rgba(255, 255, 255, 0.15);
            border: none;
            border-radius: 50%;
            color: white;
            font-size: 1.5rem;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            transition: all 0.2s ease;
            backdrop-filter: blur(5px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .dpad-btn:active {
            background: rgba(255, 255, 255, 0.3);
            transform: scale(0.95);
        }

        .dpad-up { grid-column: 2; grid-row: 1; }
        .dpad-left { grid-column: 1; grid-row: 2; }
        .dpad-right { grid-column: 3; grid-row: 2; }
        .dpad-down { grid-column: 2; grid-row: 3; }
        .dpad-center { 
            grid-column: 2; 
            grid-row: 2; 
            background: rgba(255, _localStorage255, 255, 0.05);
            cursor: default;
        }

        @media (max-width: 768px) {
            .header {
                flex-direction: column;
                gap: 15px;
                padding: 15px;
            }
            
            .stats {
                width: 100%;
                justify-content: center;
            }
            
            .stat-box {
                min-width: auto;
                padding: 8px 15px;
            }
            
            .stat-value {
                font-size: 1.8rem;
            }
            
            .level-progress {
                width: 150px;
            }
            
            .overlay h2 {
                font-size: 2.5rem;
            }
            
            .overlay p {
                font-size: 1.2rem;
            }
            
            .mobile-controls {
                display: flex;
            }
            
            .controls {
                flex-wrap: wrap;
            }
        }

        @media (max-width: 480px) {
            .stats {
                flex-wrap: wrap;
                justify-content: center;
            }
            
            .logo h1 {
                font-size: 1.8rem;
            }
            
            .btn {
                padding: 12px 20px;
                font-size: 1rem;
            }
            
            .dpad {
                width: 120px;
                height: 120px;
            }
        }
    </style>
</head>
<body>
    <div class="game-container">
        <div class="header">
            <div class="logo">
                <i class="fas fa-snake"></i>
                <h1>Neon Snake Adventure</h1>
            </div>
            <div class="stats">
                <div class="stat-box">
                    <div class="stat-value" id="score">0</div>
                    <div class="stat-label">Score</div>
                </div>
                <div class="stat-box">
                    <div class="stat-value" id="level">1</div>
                    <div class="stat-label">Level</div>
                </div>
                <div class="stat-box">
                    <div class="stat-value" id="speed">1</div>
                    <div class="stat-label">Speed</div>
                </div>
                <div class="level-progress">
                    <div class="level-indicator">Progress to checkpoint</div>
                    <div class="level-progress-bar" id="level-progress" style="width: 0%"></div>
                </div>
            </div>
        </div>
        
        <div class="game-area">
            <div id="game-board">
                <canvas id="game-canvas"></canvas>
                
                <div class="level-up-animation" id="level-up-animation">
                    <div class="level-text">Checkpoint!</div>
                </div>
                
                <div id="start-screen" class="overlay">
                    <h2>Neon Snake Adventure</h2>
                    <p>Navigate through the level, eat food, avoid walls and your own tail. Reach checkpoints to continue playing!</p>
                    <button class="btn" id="start-btn">
                        <i class="fas fa-play"></i> Start Game
                    </button>
                    <div class="instructions">
                        <h3>How to Play</h3>
                        <ul>
                            <li><i class="fas fa-arrow-up"></i><i class="fas fa-arrow-down"></i><i class="fas fa-arrow-left"></i><i class="fas fa-arrow-right"></i> Direction keys to move</li>
                            <li><i class="fas fa-arrow-up"></i><i class="fas fa-arrow-down"></i><i class="fas fa-arrow-left"></i><i class="fas fa-arrow-right"></i> Hold any arrow key to speed up</li>
                            <li><i class="fas fa-space-shuttle"></i> SPACE to pause</li>
                            <li><i class="fas fa-redo"></i> R to restart</li>
                            <li><i class="fas fa-play"></i> ENTER to restart after game over or continue at checkpoint</li>
                        </ul>
                    </div>
                </div>
                
                <div id="game-over" class="overlay hidden">
                    <h2>Game Over!</h2>
                    <p id="final-score">Your score: 0</p>
                    <button class="btn" id="restart-btn">
                        <i class="fas fa-redo"></i> Play Again
                    </button>
                    <p>Press Enter to Restart</p>
                </div>
                
                <div id="level-up" class="overlay hidden">
                    <h2>Continue?</h2>
                    <button class="btn" id="next-level-btn">
                        <i class="fas fa-arrow-right"></i> Continue
                    </button>
                    <p>Press Enter to Continue</p>
                </div>
            </div>
        </div>
        
        <div class="mobile-controls">
            <div class="dpad">
                <button class="dpad-btn dpad-up" id="up-btn"><i class="fas fa-arrow-up"></i></button>
                <div class="dpad-center"></div>
                <button class="dpad-btn dpad-left" id="left-btn"><i class="fas fa-arrow-left"></i></button>
                <button class="dpad-btn dpad-right" id="right-btn"><i class="fas fa-arrow-right"></i></button>
                <button class="dpad-btn dpad-down" id="down-btn"><i class="fas fa-arrow-down"></i></button>
            </div>
        </div>
        
        <div class="controls">
            <button class="btn" id="pause-btn">
                <i class="fas fa-pause"></i> Pause
            </button>
            <button class="btn btn-secondary" id="speed-up-btn">
                <i class="fas fa-plus"></i> Speed Up
            </button>
            <button class="btn btn-secondary" id="speed-down-btn">
                <i class="fas fa-minus"></i> Speed Down
            </button>
            <button class="btn btn-secondary" id="reset-btn">
                <i class="fas fa-sync-alt"></i> Reset
            </button>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // Game elements
            const canvas = document.getElementById('game-canvas');
            const ctx = canvas.getContext('2d');
            const scoreDisplay = document.getElementById('score');
            const levelDisplay = document.getElementById('level');
            const speedDisplay = document.getElementById('speed');
            const levelProgress = document.getElementById('level-progress');
            const startScreen = document.getElementById('start-screen');
            const gameOverScreen = document.getElementById('game-over');
            const levelUpScreen = document.getElementById('level-up');
            const levelUpAnimation = document.getElementById('level-up-animation');
            const startBtn = document.getElementById('start-btn');
            const restartBtn = document.getElementById('restart-btn');
            const nextLevelBtn = document.getElementById('next-level-btn');
            const pauseBtn = document.getElementById('pause-btn');
            const resetBtn = document.getElementById('reset-btn');
            const speedUpBtn = document.getElementById('speed-up-btn');
            const speedDownBtn = document.getElementById('speed-down-btn');
            const finalScore = document.getElementById('final-score');
            
            // Direction buttons
            const upBtn = document.getElementById('up-btn');
            const downBtn = document.getElementById('down-btn');
            const leftBtn = document.getElementById('left-btn');
            const rightBtn = document.getElementById('right-btn');
            
            // Game settings
            const gridSize = 25;
            let gridWidth, gridHeight;
            
            // Game state
            let snake = [];
            let food = {};
            let direction = 'right';
            let nextDirection = 'right';
            let score = 0;
            let level = 1;
            let speed = 1;
            let baseSpeed = speed; // Store the base speed (user-set speed)
            let gameRunning = false;
            let gamePaused = false;
            let foodEaten = 0;
            let foodsToNextLevel = 5; // Now represents foods needed to reach a checkpoint
            let animationFrameId;
            let lastRenderTime = 0;
            let particles = [];
            let arrowKeysHeld = 0; // Track how many arrow keys are currently held
            
            // Snake colors for each level
            const snakeColors = [
                '#FF7E5F', // Coral
                '#86A8E7', // Light blue
                '#91EAE4', // Turquoise
                '#FEB47B', // Peach
                '#D9A7C7'  // Lavender
            ];
            
            // Set canvas to full size and initialize grid dimensions
            function resizeCanvas() {
                canvas.width = canvas.parentElement.clientWidth;
                canvas.height = canvas.parentElement.clientHeight;
                gridWidth = Math.floor(canvas.width / gridSize);
                gridHeight = Math.floor(canvas.height / gridSize);
            }
            
            window.addEventListener('resize', resizeCanvas);
            resizeCanvas();
            
            // Initialize snake with default values
            snake = [
                {x: 5, y: Math.floor(gridHeight / 2)},
                {x: 4, y: Math.floor(gridHeight / 2)},
                {x: 3, y: Math.floor(gridHeight / 2)}
            ];
            
            // Create food at random position
            function createFood() {
                food = {
                    x: Math.floor(Math.random() * gridWidth),
                    y: Math.floor(Math.random() * gridHeight),
                    color: getFoodColor()
                };
                
                // Make sure food doesn't appear on snake
                for (let segment of snake) {
                    if (segment.x === food.x && segment.y === food.y) {
                        return createFood();
                    }
                }
            }
            
            // Get food color based on level
            function getFoodColor() {
                const colors = [
                    '#FF6B6B', // Red
                    '#4ECDC4', // Teal
                    '#FFD166', // Yellow
                    '#6A0572', // Purple
                    '#06D6A0'  // Green
                ];
                return colors[level - 1];
            }
            
            // Initialize food before first draw
            createFood();
            
            // Draw game elements
            function draw() {
                // Clear canvas
                ctx.clearRect(0, 0, canvas.width, canvas.height);
                
                // Draw grid background
                ctx.fillStyle = 'rgba(255, 255, 255, 0.03)';
                for (let x = 0; x < gridWidth; x++) {
                    for (let y = 0; y < gridHeight; y++) {
                        if ((x + y) % 2 === 0) {
                            ctx.fillRect(x * gridSize, y * gridSize, gridSize, gridSize);
                        }
                    }
                }
                
                // Draw snake
                for (let i = 0; i < snake.length; i++) {
                    // Snake head
                    if (i === 0) {
                        ctx.fillStyle = snakeColors[level - 1];
                    } 
                    // Snake body
                    else {
                        // Create gradient effect for body
                        const colorValue = 200 - Math.min(50, i * 2);
                        ctx.fillStyle = `rgb(${colorValue}, ${200 - i * 3}, ${180 - i * 4})`;
                    }
                    
                    ctx.fillRect(snake[i].x * gridSize, snake[i].y * gridSize, gridSize, gridSize);
                    
                    // Add border to segments
                    ctx.strokeStyle = 'rgba(0, 0, 0, 0.2)';
                    ctx.strokeRect(snake[i].x * gridSize, snake[i].y * gridSize, gridSize, gridSize);
                    
                    // Draw rounded corners
                    ctx.beginPath();
                    ctx.arc(
                        snake[i].x * gridSize + gridSize/2,
                        snake[i].y * gridSize + gridSize/2,
                        gridSize/2,
                        0,
                        Math.PI * 2
                    );
                    ctx.fill();
                }
                
                // Draw eyes on head
                if (snake.length > 0) {
                    ctx.fillStyle = '#000';
                    let eyeOffsetX = 0;
                    let eyeOffsetY = 0;
                    
                    if (direction === 'right') eyeOffsetX = 3;
                    if (direction === 'left') eyeOffsetX = -3;
                    if (direction === 'up') eyeOffsetY = -3;
                    if (direction === 'down') eyeOffsetY = 3;
                    
                    ctx.beginPath();
                    ctx.arc(
                        snake[0].x * gridSize + gridSize/2 + eyeOffsetX/2, 
                        snake[0].y * gridSize + gridSize/3 + eyeOffsetY/2, 
                        3, 0, Math.PI * 2
                    );
                    ctx.fill();
                    
                    ctx.beginPath();
                    ctx.arc(
                        snake[0].x * gridSize + gridSize/2 + eyeOffsetX/2, 
                        snake[0].y * gridSize + 2*gridSize/3 + eyeOffsetY/2, 
                        3, 0, Math.PI * 2
                    );
                    ctx.fill();
                }
                
                // Draw food with pulsing effect, but only if food is properly initialized
                if (food && typeof food.x === 'number' && typeof food.y === 'number' && food.color) {
                    const pulse = Math.sin(Date.now() / 200) * 3;
                    ctx.fillStyle = food.color;
                    ctx.beginPath();
                    ctx.arc(
                        food.x * gridSize + gridSize/2,
                        food.y * gridSize + gridSize/2,
                        gridSize/2 - 2 + pulse,
                        0, Math.PI * 2
                    );
                    ctx.fill();
                    
                    // Draw food shine
                    ctx.fillStyle = 'rgba(255, 255, 255, 0.7)';
                    ctx.beginPath();
                    ctx.arc(
                        food.x * gridSize + gridSize/3,
                        food.y * gridSize + gridSize/3,
                        gridSize/6,
                        0, Math.PI * 2
                    );
                    ctx.fill();
                }
                
                // Draw particles
                drawParticles();
            }
            
            // Draw particles for effects
            function drawParticles() {
                for (let i = particles.length - 1; i >= 0; i--) {
                    const p = particles[i];
                    p.x += p.vx;
                    p.y += p.vy;
                    p.alpha -= 0.02;
                    
                    if (p.alpha <= 0) {
                        particles.splice(i, 1);
                        continue;
                    }
                    
                    ctx.fillStyle = `rgba(${p.r}, ${p.g}, ${p.b}, ${p.alpha})`;
                    ctx.beginPath();
                    ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
                    ctx.fill();
                }
            }
            
            // Create particles
            function createParticles(x, y, color) {
                const rgb = color.match(/\d+/g);
                const r = parseInt(rgb[0]);
                const g = parseInt(rgb[1]);
                const b = parseInt(rgb[2]);
                
                for (let i = 0; i < 15; i++) {
                    particles.push({
                        x: x,
                        y: y,
                        size: Math.random() * 3 + 1,
                        vx: (Math.random() - 0.5) * 4,
                        vy: (Math.random() - 0.5) * 4,
                        alpha: Math.random() * 0.5 + 0.5,
                        r: r,
                        g: g,
                        b: b
                    });
                }
            }
            
            // Initialize game
            function initGame() {
                // Reset game state
                snake = [
                    {x: 5, y: Math.floor(gridHeight / 2)},
                    {x: 4, y: Math.floor(gridHeight / 2)},
                    {x: 3, y: Math.floor(gridHeight / 2)}
                ];
                
                direction = 'right';
                nextDirection = 'right';
                score = 0;
                foodEaten = 0;
                level = 1;
                speed = 1;
                baseSpeed = speed;
                arrowKeysHeld = 0; // Reset arrow key counter
                
                // Create first food
                createFood();
                
                // Update displays
                scoreDisplay.textContent = score;
                levelDisplay.textContent = level;
                speedDisplay.textContent = speed;
                levelProgress.style.width = '0%';
                
                // Hide screens
                gameOverScreen.classList.add('hidden');
                levelUpScreen.classList.add('hidden');
                startScreen.classList.add('hidden');
                
                gameRunning = true;
                gamePaused = false;
                pauseBtn.innerHTML = '<i class="fas fa-pause"></i> Pause';
                
                // Start game loop
                if (animationFrameId) {
                    cancelAnimationFrame(animationFrameId);
                }
                animationFrameId = requestAnimationFrame(gameLoop);
            }
            
            // Move snake
            function moveSnake() {
                if (!gameRunning || gamePaused) return;
                
                // Update direction
                direction = nextDirection;
                
                // Create new head based on direction
                const head = {x: snake[0].x, y: snake[0].y};
                
                switch (direction) {
                    case 'up': head.y--; break;
                    case 'down': head.y++; break;
                    case 'left': head.x--; break;
                    case 'right': head.x++; break;
                }
                
                // Check for collision with walls
                if (head.x < 0 || head.x >= gridWidth || head.y < 0 || head.y >= gridHeight) {
                    gameOver();
                    return;
                }
                
                // Check for collision with self
                for (let i = 0; i < snake.length; i++) {
                    if (head.x === snake[i].x && head.y === snake[i].y) {
                        gameOver();
                        return;
                    }
                }
                
                // Add new head to snake
                snake.unshift(head);
                
                // Check if food was eaten
                if (head.x === food.x && head.y === food.y) {
                    // Create particles
                    createParticles(
                        food.x * gridSize + gridSize/2,
                        food.y * gridSize + gridSize/2,
                        food.color
                    );
                    
                    // Increase score
                    score += 10 * level;
                    scoreDisplay.textContent = score;
                    
                    // Increase food eaten count
                    foodEaten++;
                    
                    // Update level progress
                    const progress = (foodEaten / foodsToNextLevel) * 100;
                    levelProgress.style.width = `${progress}%`;
                    
                    // Check if checkpoint reached
                    if (foodEaten >= foodsToNextLevel) {
                        checkpointReached();
                        return;
                    }
                    
                    // Create new food
                    createFood();
                } else {
                    // Remove tail if no food was eaten
                    snake.pop();
                }
            }
            
            // Game over function
            function gameOver() {
                gameRunning = false;
                finalScore.textContent = `Your score: ${score}`;
                gameOverScreen.classList.remove('hidden');
                cancelAnimationFrame(animationFrameId);
            }
            
            // Checkpoint reached function
            function checkpointReached() {
                gameRunning = false;
                
                // Show checkpoint animation
                levelUpAnimation.style.opacity = '1';
                setTimeout(() => {
                    levelUpAnimation.style.opacity = '0';
                }, 1500);
                
                levelUpScreen.classList.remove('hidden');
            }
            
            // Continue game at checkpoint
            function continueGame() {
                // Reset food count
                foodEaten = 0;
                levelProgress.style.width = '0%';
                
                // Reset snake position
                snake = [
                    {x: 5, y: Math.floor(gridHeight / 2)},
                    {x: 4, y: Math.floor(gridHeight / 2)},
                    {x: 3, y: Math.floor(gridHeight / 2)}
                ];
                
                direction = 'right';
                nextDirection = 'right';
                
                // Create new food
                createFood();
                
                // Hide checkpoint screen
                levelUpScreen.classList.add('hidden');
                
                gameRunning = true;
                animationFrameId = requestAnimationFrame(gameLoop);
            }
            
            // Game loop
            function gameLoop(timestamp) {
                if (lastRenderTime === 0) {
                    lastRenderTime = timestamp;
                }
                
                const progress = timestamp - lastRenderTime;
                
                if (progress > 1000 / (5 + speed * 2)) {
                    moveSnake();
                    draw();
                    lastRenderTime = timestamp;
                }
                
                if (gameRunning) {
                    animationFrameId = requestAnimationFrame(gameLoop);
                }
            }
            
            // Speed adjustment functions
            function increaseSpeed() {
                if (speed < 5) {
                    speed++;
                    baseSpeed = speed; // Update base speed
                    speedDisplay.textContent = speed;
                }
            }

            function decreaseSpeed() {
                if (speed > 1) {
                    speed--;
                    baseSpeed = speed; // Update base speed
                    speedDisplay.textContent = speed;
                }
            }
            
            // Event listeners for buttons
            startBtn.addEventListener('click', () => {
                initGame();
            });
            
            restartBtn.addEventListener('click', () => {
                initGame();
            });
            
            nextLevelBtn.addEventListener('click', () => {
                continueGame();
            });
            
            pauseBtn.addEventListener('click', () => {
                if (gameRunning) {
                    gamePaused = !gamePaused;
                    pauseBtn.innerHTML = gamePaused ? 
                        '<i class="fas fa-play"></i> Resume' : 
                        '<i class="fas fa-pause"></i> Pause';
                }
            });
            
            resetBtn.addEventListener('click', () => {
                initGame();
            });
            
            speedUpBtn.addEventListener('click', () => {
                if (gameRunning) {
                    increaseSpeed();
                }
            });
            
            speedDownBtn.addEventListener('click', () => {
                if (gameRunning) {
                    decreaseSpeed();
                }
            });
            
            // Keyboard controls for direction and other actions
            document.addEventListener('keydown', (e) => {
                // Handle direction and speed increase
                switch (e.key) {
                    case 'ArrowUp':
                        if (direction !== 'down') nextDirection = 'up';
                        if (!e.repeat) { // Only increment on initial press, not on repeat
                            arrowKeysHeld++;
                            speed = baseSpeed + 2; // Temporarily increase speed
                            speedDisplay.textContent = speed;
                        }
                        e.preventDefault(); // Prevent scrolling
                        break;
                    case 'ArrowDown':
                        if (direction !== 'up') nextDirection = 'down';
                        if (!e.repeat) {
                            arrowKeysHeld++;
                            speed = baseSpeed + 2;
                            speedDisplay.textContent = speed;
                        }
                        e.preventDefault();
                        break;
                    case 'ArrowLeft':
                        if (direction !== 'right') nextDirection = 'left';
                        if (!e.repeat) {
                            arrowKeysHeld++;
                            speed = baseSpeed + 2;
                            speedDisplay.textContent = speed;
                        }
                        e.preventDefault();
                        break;
                    case 'ArrowRight':
                        if (direction !== 'left') nextDirection = 'right';
                        if (!e.repeat) {
                            arrowKeysHeld++;
                            speed = baseSpeed + 2;
                            speedDisplay.textContent = speed;
                        }
                        e.preventDefault();
                        break;
                    case ' ':
                        if (gameRunning) {
                            gamePaused = !gamePaused;
                            pauseBtn.innerHTML = gamePaused ? 
                                '<i class="fas fa-play"></i> Resume' : 
                                '<i class="fas fa-pause"></i> Pause';
                        }
                        e.preventDefault();
                        break;
                    case 'r':
                    case 'R':
                        initGame();
                        e.preventDefault();
                        break;
                    case 'Enter':
                        if (!gameRunning) {
                            if (!gameOverScreen.classList.contains('hidden')) {
                                initGame();
                            } else if (!levelUpScreen.classList.contains('hidden')) {
                                continueGame();
                            }
                        }
                        e.preventDefault();
                        break;
                }
            });

            // Revert speed when arrow keys are released
            document.addEventListener('keyup', (e) => {
                switch (e.key) {
                    case 'ArrowUp':
                    case 'ArrowDown':
                    case 'ArrowLeft':
                    case 'ArrowRight':
                        arrowKeysHeld--;
                        if (arrowKeysHeld <= 0) {
                            arrowKeysHeld = 0; // Prevent negative count
                            speed = baseSpeed; // Revert to base speed
                            speedDisplay.textContent = speed;
                        }
                        e.preventDefault();
                        break;
                }
            });
            
            // Direction button controls for mobile
            upBtn.addEventListener('click', () => { if (direction !== 'down') nextDirection = 'up'; });
            downBtn.addEventListener('click', () => { if (direction !== 'up') nextDirection = 'down'; });
            leftBtn.addEventListener('click', () => { if (direction !== 'right') nextDirection = 'left'; });
            rightBtn.addEventListener('click', () => { if (direction !== 'left') nextDirection = 'right'; });
            
            // Initial draw
            draw();
        });
    </script>
</body>
</html>
