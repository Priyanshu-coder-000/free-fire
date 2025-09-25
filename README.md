
[quiz-game.html](https://github.com/user-attachments/files/22532446/quiz-game.html)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz Game</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            overflow-x: hidden;
        }

        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><defs><pattern id="grain" width="100" height="100" patternUnits="userSpaceOnUse"><circle cx="50" cy="50" r="0.5" fill="%23ffffff" opacity="0.1"/></pattern></defs><rect width="100" height="100" fill="url(%23grain)"/></svg>') repeat;
            pointer-events: none;
            z-index: -1;
        }

        .quiz-container {
            background: linear-gradient(145deg, #2d3748, #1a202c);
            color: #e2e8f0;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.4), inset 0 1px 0 rgba(255,255,255,0.1);
            padding: 40px;
            max-width: 650px;
            width: 100%;
            border: 1px solid rgba(255,255,255,0.1);
            backdrop-filter: blur(10px);
            position: relative;
            overflow: hidden;
        }

        .quiz-container::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 2px;
            background: linear-gradient(90deg, #667eea, #764ba2, #667eea);
            animation: shimmer 3s ease-in-out infinite;
        }

        @keyframes shimmer {
            0%, 100% { opacity: 0.5; }
            50% { opacity: 1; }
        }

        .quiz-header {
            text-align: center;
            margin-bottom: 30px;
        }

        .quiz-header h1 {
            color: #e2e8f0;
            margin-bottom: 15px;
            font-size: 2.2em;
            font-weight: 700;
            text-shadow: 0 2px 10px rgba(102, 126, 234, 0.3);
            background: linear-gradient(135deg, #667eea, #764ba2);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .stats-bar {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
            padding: 10px 15px;
            background: rgba(255,255,255,0.05);
            border-radius: 10px;
            border: 1px solid rgba(255,255,255,0.1);
        }

        .stat-item {
            text-align: center;
            font-size: 0.9em;
        }

        .stat-value {
            font-weight: bold;
            color: #81c784;
            font-size: 1.2em;
        }

        .timer {
            position: absolute;
            top: 20px;
            right: 20px;
            background: rgba(220, 53, 69, 0.2);
            color: #ff6b6b;
            padding: 8px 15px;
            border-radius: 20px;
            font-weight: bold;
            border: 1px solid rgba(220, 53, 69, 0.3);
            display: none;
        }

        .mode-selection {
            text-align: center;
        }

        .mode-buttons {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
            gap: 15px;
            margin: 20px 0;
        }

        .mode-btn {
            background: linear-gradient(145deg, #4a5568, #2d3748);
            border: 2px solid #718096;
            color: #e2e8f0;
            border-radius: 12px;
            padding: 20px 15px;
            cursor: pointer;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            font-size: 0.95em;
            font-weight: 600;
            position: relative;
            overflow: hidden;
        }

        .mode-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.1), transparent);
            transition: left 0.5s;
        }

        .mode-btn:hover {
            background: linear-gradient(145deg, #667eea, #5a6fd8);
            color: white;
            border-color: #667eea;
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(102, 126, 234, 0.3);
        }

        .mode-btn:hover::before {
            left: 100%;
        }

        .difficulty-badge {
            display: inline-block;
            padding: 3px 8px;
            border-radius: 12px;
            font-size: 0.8em;
            font-weight: bold;
            margin-bottom: 10px;
        }

        .easy { background: #28a745; color: white; }
        .medium { background: #ffc107; color: black; }
        .hard { background: #fd7e14; color: white; }
        .extreme { background: #dc3545; color: white; }
        .old-player { background: #6f42c1; color: white; }



        .progress-bar {
            background: #4a5568;
            height: 12px;
            border-radius: 6px;
            overflow: hidden;
            margin-bottom: 25px;
            position: relative;
            box-shadow: inset 0 2px 4px rgba(0,0,0,0.3);
        }

        .progress {
            background: linear-gradient(90deg, #667eea, #764ba2);
            height: 100%;
            transition: width 0.6s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            overflow: hidden;
        }

        .progress::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent);
            animation: progressShine 2s ease-in-out infinite;
        }

        @keyframes progressShine {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }

        .question {
            font-size: 1.2em;
            margin-bottom: 25px;
            color: #e2e8f0;
            line-height: 1.5;
        }

        .options {
            display: grid;
            gap: 15px;
            margin-bottom: 30px;
        }

        .option {
            background: linear-gradient(145deg, #4a5568, #2d3748);
            border: 2px solid #718096;
            color: #e2e8f0;
            border-radius: 12px;
            padding: 18px;
            cursor: pointer;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            overflow: hidden;
        }

        .option::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(45deg, transparent, rgba(255,255,255,0.05), transparent);
            transform: translateX(-100%);
            transition: transform 0.6s;
        }

        .option:hover {
            background: linear-gradient(145deg, #718096, #4a5568);
            border-color: #667eea;
            transform: translateX(5px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.2);
        }

        .option:hover::before {
            transform: translateX(100%);
        }

        .option.selected {
            background: #667eea;
            color: white;
            border-color: #667eea;
        }

        .option.correct {
            background: #28a745;
            border-color: #28a745;
            color: white;
        }

        .option.incorrect {
            background: #dc3545;
            border-color: #dc3545;
            color: white;
        }

        .quiz-footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .score {
            font-weight: bold;
            color: #81c784;
        }

        .btn {
            background: linear-gradient(145deg, #667eea, #5a6fd8);
            color: white;
            border: none;
            padding: 14px 30px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 1em;
            font-weight: 600;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            box-shadow: 0 4px 15px rgba(102, 126, 234, 0.3);
            position: relative;
            overflow: hidden;
        }

        .btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
            transition: left 0.5s;
        }

        .btn:hover {
            background: linear-gradient(145deg, #5a6fd8, #4c63d2);
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(102, 126, 234, 0.4);
        }

        .btn:hover::before {
            left: 100%;
        }

        .btn:disabled {
            background: #ccc;
            cursor: not-allowed;
        }

        .result {
            text-align: center;
            display: none;
        }

        .result h2 {
            color: #e2e8f0;
            margin-bottom: 20px;
        }

        .final-score {
            font-size: 2em;
            color: #81c784;
            margin-bottom: 20px;
        }

        @media (max-width: 768px) {
            .quiz-container {
                padding: 20px;
                margin: 10px;
            }
            
            .question {
                font-size: 1.1em;
            }
            
            .quiz-footer {
                flex-direction: column;
                gap: 15px;
            }
            

        }
    </style>
</head>
<body>
    <div class="quiz-container">
        <div class="quiz-header">
            <h1>🔥 Free Fire Quiz Pro 🔥</h1>
            <div class="stats-bar" id="statsBar" style="display: none;">
                <div class="stat-item">
                    <div class="stat-value" id="questionNum">1</div>
                    <div>Question</div>
                </div>
                <div class="stat-item">
                    <div class="stat-value" id="accuracy">0%</div>
                    <div>Accuracy</div>
                </div>
                <div class="stat-item">
                    <div class="stat-value" id="streak">0</div>
                    <div>Streak</div>
                </div>
            </div>
            <div class="progress-bar">
                <div class="progress" id="progress"></div>
            </div>
            <div class="timer" id="timer">⏱️ 30s</div>
        </div>

        <div id="modeSelection" class="mode-selection">
            <h2>Select Difficulty Mode</h2>
            <div class="mode-buttons">
                <div class="mode-btn" onclick="startQuiz('easy')">
                    <div class="difficulty-badge easy">EASY</div>
                    <div>Basic Questions</div>
                </div>
                <div class="mode-btn" onclick="startQuiz('medium')">
                    <div class="difficulty-badge medium">MEDIUM</div>
                    <div>Moderate Level</div>
                </div>
                <div class="mode-btn" onclick="startQuiz('hard')">
                    <div class="difficulty-badge hard">HARD</div>
                    <div>Challenging</div>
                </div>
                <div class="mode-btn" onclick="startQuiz('extreme')">
                    <div class="difficulty-badge extreme">EXTREME</div>
                    <div>Pro Level</div>
                </div>
                <div class="mode-btn" onclick="startQuiz('old-player')">
                    <div class="difficulty-badge old-player">OLD PLAYER</div>
                    <div>Nostalgia Mode</div>
                </div>
            </div>
        </div>

        <div id="quiz" class="quiz-content" style="display: none;">
            <div class="difficulty-badge" id="currentMode"></div>
            <div class="question" id="question"></div>
            <div class="options" id="options"></div>
            <div class="quiz-footer">
                <div class="score">Score: <span id="score">0</span></div>
                <button class="btn" id="nextBtn" onclick="nextQuestion()" disabled>Next Question</button>
            </div>
        </div>

        <div id="result" class="result">
            <h2>Quiz Complete!</h2>
            <div class="final-score" id="finalScore"></div>
            <button class="btn" onclick="restartQuiz()">Play Again</button>
        </div>
    </div>

    <script>
        const questionBank = {
            easy: [
                {
                    question: "Free Fire mein booyah ka matlab kya hai?",
                    options: ["Game start", "Game over", "Winner winner", "New match"],
                    correct: 2
                },
                {
                    question: "Gloo wall ka kya use hai?",
                    options: ["Attack", "Defense", "Jump", "Speed"],
                    correct: 1
                },
                {
                    question: "Free Fire mein kitne players maximum ho sakte hain?",
                    options: ["40", "50", "60", "100"],
                    correct: 1
                },
                {
                    question: "Landing ke baad sabse pehle kya karna chahiye?",
                    options: ["Loot karna", "Fight karna", "Hide karna", "Run karna"],
                    correct: 0
                },
                {
                    question: "Free Fire ka developer kaun hai?",
                    options: ["Tencent", "Garena", "PUBG Corp", "Supercell"],
                    correct: 1
                }
            ],
            medium: [
                {
                    question: "AWM sniper rifle mein kitni damage hoti hai headshot mein?",
                    options: ["295", "298", "300", "310"],
                    correct: 0
                },
                {
                    question: "Clash Squad mode mein kitne rounds hote hain?",
                    options: ["5", "7", "9", "11"],
                    correct: 1
                },
                {
                    question: "Kalahari map ka size kitna hai?",
                    options: ["2x2 km", "4x4 km", "6x6 km", "8x8 km"],
                    correct: 1
                },
                {
                    question: "DJ Alok ka ability kya hai?",
                    options: ["Speed boost", "Heal + Speed", "Damage boost", "Invisible"],
                    correct: 1
                },
                {
                    question: "M1014 shotgun mein kitni bullets hoti hain?",
                    options: ["6", "8", "10", "12"],
                    correct: 0
                }
            ],
            hard: [
                {
                    question: "Chrono character ka ability duration kitna hai max level par?",
                    options: ["6 seconds", "8 seconds", "9 seconds", "12 seconds"],
                    correct: 2
                },
                {
                    question: "Purgatory map mein kitne spawn points hain?",
                    options: ["12", "14", "16", "18"],
                    correct: 1
                },
                {
                    question: "Hayato character ka passive ability kya hai?",
                    options: ["Armor penetration", "Damage reduction", "Speed boost", "Health regen"],
                    correct: 0
                },
                {
                    question: "Treatment gun se kitni HP heal hoti hai per second?",
                    options: ["50", "60", "75", "100"],
                    correct: 2
                },
                {
                    question: "Ranked mode mein Heroic rank ke liye kitne points chahiye?",
                    options: ["4000", "4500", "5000", "5500"],
                    correct: 0
                }
            ],
            extreme: [
                {
                    question: "Wukong character ka ability cooldown kitna hai max level par?",
                    options: ["85 seconds", "100 seconds", "120 seconds", "150 seconds"],
                    correct: 0
                },
                {
                    question: "Bermuda Remastered mein Clock Tower ke exact coordinates kya hain?",
                    options: ["(512, 512)", "(256, 256)", "(400, 400)", "(300, 300)"],
                    correct: 0
                },
                {
                    question: "Groza assault rifle ka exact damage per bullet kitna hai?",
                    options: ["61", "64", "67", "70"],
                    correct: 1
                },
                {
                    question: "Clash Squad mein bomb defuse time kitna hai?",
                    options: ["5 seconds", "7 seconds", "10 seconds", "12 seconds"],
                    correct: 1
                },
                {
                    question: "Maxim character ka ability range kitna hai max level par?",
                    options: ["85m", "100m", "120m", "150m"],
                    correct: 2
                }
            ],
            'old-player': [
                {
                    question: "Free Fire ka pehla map kaun sa tha?",
                    options: ["Bermuda", "Kalahari", "Purgatory", "Alpine"],
                    correct: 0
                },
                {
                    question: "Old Bermuda mein Mill area kahan tha?",
                    options: ["Center mein", "North mein", "South mein", "East mein"],
                    correct: 1
                },
                {
                    question: "Pehle Free Fire mein kitne characters the launch ke time?",
                    options: ["6", "8", "10", "12"],
                    correct: 1
                },
                {
                    question: "Old Factory area ka naam kya tha originally?",
                    options: ["Factory", "Warehouse", "Industrial", "Mill"],
                    correct: 3
                },
                {
                    question: "2018 mein Free Fire ka tagline kya tha?",
                    options: ["Survive Till The End", "Battle Royale", "Last Man Standing", "Winner Winner"],
                    correct: 0
                }
            ]
        };

        let currentQuestion = 0;
        let score = 0;
        let selectedOption = null;
        let currentMode = '';
        let questions = [];
        let correctStreak = 0;
        let totalAnswered = 0;
        let timeLeft = 30;
        let timer = null;
        
        function startQuiz(mode) {
            currentMode = mode;
            questions = questionBank[mode];
            currentQuestion = 0;
            score = 0;
            selectedOption = null;
            correctStreak = 0;
            totalAnswered = 0;
            
            document.getElementById('modeSelection').style.display = 'none';
            document.getElementById('quiz').style.display = 'block';
            document.getElementById('statsBar').style.display = 'flex';
            document.getElementById('score').textContent = '0';
            
            const modeDisplay = document.getElementById('currentMode');
            modeDisplay.textContent = mode.toUpperCase().replace('-', ' ');
            modeDisplay.className = `difficulty-badge ${mode}`;
            
            loadQuestion();
            updateStats();
        }

        function loadQuestion() {
            const question = questions[currentQuestion];
            document.getElementById('question').textContent = question.question;
            
            const optionsContainer = document.getElementById('options');
            optionsContainer.innerHTML = '';
            
            question.options.forEach((option, index) => {
                const optionElement = document.createElement('div');
                optionElement.className = 'option';
                optionElement.textContent = option;
                optionElement.onclick = () => selectOption(index);
                optionsContainer.appendChild(optionElement);
            });

            updateProgress();
            updateStats();
            document.getElementById('nextBtn').disabled = true;
            selectedOption = null;
            
            // Add entrance animation
            optionsContainer.style.opacity = '0';
            optionsContainer.style.transform = 'translateY(20px)';
            setTimeout(() => {
                optionsContainer.style.transition = 'all 0.5s ease';
                optionsContainer.style.opacity = '1';
                optionsContainer.style.transform = 'translateY(0)';
            }, 100);
        }

        function selectOption(index) {
            const options = document.querySelectorAll('.option');
            options.forEach(option => option.classList.remove('selected'));
            options[index].classList.add('selected');
            selectedOption = index;
            document.getElementById('nextBtn').disabled = false;
        }

        function nextQuestion() {
            if (selectedOption === null) return;

            const question = questions[currentQuestion];
            const options = document.querySelectorAll('.option');
            
            totalAnswered++;
            options[question.correct].classList.add('correct');
            
            if (selectedOption !== question.correct) {
                options[selectedOption].classList.add('incorrect');
                correctStreak = 0;
            } else {
                score++;
                correctStreak++;
                document.getElementById('score').textContent = score;
            }

            updateStats();

            setTimeout(() => {
                currentQuestion++;
                if (currentQuestion < questions.length) {
                    loadQuestion();
                } else {
                    showResult();
                }
            }, 1500);
        }

        function updateProgress() {
            const progress = ((currentQuestion + 1) / questions.length) * 100;
            document.getElementById('progress').style.width = progress + '%';
        }

        function showResult() {
            document.getElementById('quiz').style.display = 'none';
            document.getElementById('result').style.display = 'block';
            document.getElementById('finalScore').textContent = `${score}/${questions.length}`;
        }

        function updateStats() {
            document.getElementById('questionNum').textContent = currentQuestion + 1;
            const accuracy = totalAnswered > 0 ? Math.round((score / totalAnswered) * 100) : 0;
            document.getElementById('accuracy').textContent = accuracy + '%';
            document.getElementById('streak').textContent = correctStreak;
        }

        function restartQuiz() {
            document.getElementById('modeSelection').style.display = 'block';
            document.getElementById('quiz').style.display = 'none';
            document.getElementById('result').style.display = 'none';
            document.getElementById('statsBar').style.display = 'none';
            correctStreak = 0;
            totalAnswered = 0;
        }


    </script>
</body>
</html>
