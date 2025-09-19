<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Language Game</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #fdbb2d);
            min-height: 100vh;
            padding: 20px;
            color: #333;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .container {
            max-width: 1000px;
            width: 100%;
            background: white;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.2);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f);
            color: white;
            padding: 30px;
            text-align: center;
            position: relative;
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
        }

        .header p {
            font-size: 1.2em;
            opacity: 0.9;
        }

        .game-selector {
            display: flex;
            justify-content: center;
            background: #f0f0f0;
            padding: 15px;
            border-bottom: 2px solid #ddd;
        }

        .game-btn {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            margin: 0 10px;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(26, 42, 108, 0.3);
        }

        .game-btn:hover, .game-btn.active {
            background: linear-gradient(135deg, #b21f1f, #1a2a6c);
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(178, 31, 31, 0.4);
        }

        .game-section {
            padding: 30px;
            display: none;
        }

        .game-section.active {
            display: block;
        }

        .word-bank {
            background: linear-gradient(135deg, #f5f7fa, #e8f4f8);
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 25px;
            border: 2px solid #1a2a6c;
            box-shadow: 0 4px 15px rgba(26, 42, 108, 0.2);
        }

        .word-bank h3 {
            color: #1a2a6c;
            margin-bottom: 15px;
            text-align: center;
            font-size: 1.4em;
        }

        .word-options {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            justify-content: center;
        }

        .word-option {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f);
            color: white;
            padding: 10px 18px;
            border-radius: 20px;
            font-weight: bold;
            font-size: 16px;
            box-shadow: 0 3px 10px rgba(26, 42, 108, 0.3);
            transition: all 0.3s ease;
            cursor: default;
        }

        .word-option:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(26, 42, 108, 0.4);
        }

        .question {
            background: #f8f9fa;
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 20px;
            border-left: 5px solid #1a2a6c;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
        }

        .question h3 {
            color: #1a2a6c;
            margin-bottom: 15px;
            font-size: 1.3em;
        }

        .fill-blank {
            background: #fff;
            border: 2px solid #ddd;
            border-radius: 8px;
            padding: 8px 12px;
            font-size: 16px;
            margin: 0 5px;
            min-width: 120px;
            transition: border-color 0.3s ease;
        }

        .fill-blank.correct {
            border-color: #4CAF50;
            background: #e8f5e8;
        }

        .fill-blank.incorrect {
            border-color: #f44336;
            background: #ffeaea;
        }

        .check-btn {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            margin: 20px auto;
            display: block;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(26, 42, 108, 0.3);
        }

        .check-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(26, 42, 108, 0.4);
        }

        .feedback {
            margin-top: 15px;
            padding: 15px;
            border-radius: 10px;
            font-weight: bold;
            text-align: center;
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from { transform: translateX(-20px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        .feedback.correct {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .feedback.incorrect {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .score {
            position: fixed;
            top: 20px;
            right: 20px;
            background: linear-gradient(135deg, #1a2a6c, #b21f1f);
            color: white;
            padding: 15px 20px;
            border-radius: 25px;
            font-weight: bold;
            box-shadow: 0 4px 15px rgba(26, 42, 108, 0.3);
            z-index: 1000;
        }

        .icon {
            font-size: 24px;
            margin-right: 10px;
            vertical-align: middle;
        }

        .shuffle-btn {
            background: linear-gradient(135deg, #fdbb2d, #b21f1f);
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 14px;
            font-weight: bold;
            margin: 10px auto;
            display: block;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(253, 187, 45, 0.3);
        }

        .shuffle-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(253, 187, 45, 0.4);
        }

        .options-container {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            justify-content: center;
            margin-top: 20px;
        }

        .option {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f);
            color: white;
            padding: 15px 25px;
            border-radius: 10px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s ease;
            text-align: center;
            min-width: 150px;
        }

        .option:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(26, 42, 108, 0.4);
        }

        .option.correct {
            background: linear-gradient(135deg, #4CAF50, #2E7D32);
        }

        .option.incorrect {
            background: linear-gradient(135deg, #f44336, #c62828);
        }

        /* Drag and Drop Styles */
        .drag-container {
            display: flex;
            flex-direction: column;
            gap: 15px;
            margin: 20px 0;
        }

        .drag-item {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f);
            color: white;
            padding: 15px;
            border-radius: 10px;
            cursor: grab;
            font-weight: bold;
            transition: all 0.3s ease;
            text-align: center;
        }

        .drag-item:active {
            cursor: grabbing;
        }

        .drag-item.dragging {
            opacity: 0.5;
        }

        .drop-area {
            border: 2px dashed #1a2a6c;
            border-radius: 10px;
            padding: 20px;
            margin: 10px 0;
            min-height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
            background: #f8f9fa;
            transition: all 0.3s ease;
        }

        .drop-area.hover {
            background: #e9ecef;
            border-color: #fdbb2d;
        }

        .drop-area.correct {
            background: #d4edda;
            border-color: #28a745;
        }

        .drop-area.incorrect {
            background: #f8d7da;
            border-color: #dc3545;
        }

        /* Sentence Builder Styles */
        .sentence-builder {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 15px;
            margin: 20px 0;
        }

        .word-pool {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-bottom: 20px;
            justify-content: center;
        }

        .word-piece {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f);
            color: white;
            padding: 10px 15px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s ease;
        }

        .word-piece:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }

        .sentence-area {
            border: 2px solid #1a2a6c;
            border-radius: 10px;
            padding: 20px;
            min-height: 100px;
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            gap: 10px;
            background: white;
        }

        .sentence-word {
            background: linear-gradient(135deg, #fdbb2d, #b21f1f);
            color: white;
            padding: 8px 12px;
            border-radius: 6px;
            font-weight: bold;
            cursor: pointer;
        }

        .sentence-word:hover {
            background: linear-gradient(135deg, #b21f1f, #fdbb2d);
        }

        @media (max-width: 768px) {
            .score {
                position: static;
                margin: 20px auto;
                display: block;
                width: fit-content;
            }
            
            .game-selector {
                flex-wrap: wrap;
                gap: 10px;
            }
            
            .game-btn {
                margin: 5px;
                padding: 10px 15px;
                font-size: 14px;
            }
        }
    </style>
</head>
<body>
    <div class="score">Score: <span id="score">0</span>/<span id="total">0</span></div>
    
    <div class="container">
        <div class="header">
            <h1><i class="fas fa-language icon"></i> Interactive Language Game</h1>
            <p>Practice using these phrases in context!</p>
        </div>

        <div class="game-selector">
            <button class="game-btn active" data-game="fill-blanks">Fill in the Blanks</button>
            <button class="game-btn" data-game="sentence-builder">Sentence Builder</button>
        </div>

        <!-- Fill in the Blanks Game -->
        <div class="game-section active" id="fill-blanks">
            <div class="word-bank">
                <h3><i class="fas fa-list-ul icon"></i> Words to use:</h3>
                <div class="word-options">
                    <span class="word-option">46 years old</span>
                    <span class="word-option">currently</span>
                    <span class="word-option">tasks</span>
                    <span class="word-option">confirming</span>
                    <span class="word-option">because</span>
                    <span class="word-option">moreover</span>
                    <span class="word-option">tired</span>
                </div>
            </div>

            <button class="shuffle-btn" onclick="shuffleQuestions()">
                <i class="fas fa-random"></i> Shuffle Sentences
            </button>

            <div id="questions-container">
                <!-- Questions will be inserted here dynamically -->
            </div>

            <button class="check-btn" onclick="checkFillBlanks()">Check Answers</button>
        </div>

        <!-- Sentence Builder Game -->
        <div class="game-section" id="sentence-builder">
            <div class="word-bank">
                <h3><i class="fas fa-puzzle-piece icon"></i> Build sentences using the words below:</h3>
            </div>

            <div class="sentence-builder">
                <div class="word-pool" id="word-pool">
                    <!-- Word pieces will be inserted here dynamically -->
                </div>
                
                <div class="sentence-area" id="sentence-area">
                    <p>Drag words here to build your sentence</p>
                </div>
                
                <div class="feedback" id="builder-feedback" style="display: none;"></div>
            </div>

            <button class="check-btn" onclick="checkSentence()">Check Sentence</button>
            <button class="shuffle-btn" onclick="resetSentenceBuilder()">
                <i class="fas fa-redo"></i> Reset Builder
            </button>
        </div>
    </div>

    <script>
        let score = 0;
        let totalQuestions = 0;
        let currentGame = "fill-blanks";
        let draggedWord = null;
        
        // Fill in the blanks questions using the provided phrases
        let questions = [
            {
                id: 1,
                question: "I am <input type='text' class='fill-blank' data-answer='46 years old' placeholder='answer'> and I work as an accountant.",
                answer: "46 years old"
            },
            {
                id: 2,
                question: "<input type='text' class='fill-blank' data-answer='Currently' placeholder='answer'>, I'm working on several projects for our clients.",
                answer: "Currently"
            },
            {
                id: 3,
                question: "My main <input type='text' class='fill-blank' data-answer='tasks' placeholder='answer'> include preparing financial reports and analyzing budgets.",
                answer: "tasks"
            },
            {
                id: 4,
                question: "I am responsible for <input type='text' class='fill-blank' data-answer='confirming' placeholder='answer'> all payments before they are processed.",
                answer: "confirming"
            },
            {
                id: 5,
                question: "I need to finish this report by Friday <input type='text' class='fill-blank' data-answer='because' placeholder='answer'> the meeting with the board is scheduled for next week.",
                answer: "because"
            },
            {
                id: 6,
                question: "<input type='text' class='fill-blank' data-answer='Moreover' placeholder='answer'>, I have to prepare a presentation for the annual review.",
                answer: "Moreover"
            },
            {
                id: 7,
                question: "After working for 10 hours straight, I'm feeling extremely <input type='text' class='fill-blank' data-answer='tired' placeholder='answer'>.",
                answer: "tired"
            }
        ];

        // Sentence builder data
        const sentenceTemplates = [
            "I am 46 years old and currently working on important tasks",
            "I am confirming the payments because I need to finish my tasks",
            "I am tired because I have been working on these tasks all day",
            "Moreover, I am currently confirming all the payments for this month",
            "I am 46 years old and tired because of my many tasks",
            "I am currently confirming payments, and moreover, I'm preparing reports"
        ];

        // Initialize the page
        window.onload = function() {
            // Set up game navigation
            document.querySelectorAll('.game-btn').forEach(button => {
                button.addEventListener('click', function() {
                    // Remove active class from all buttons and sections
                    document.querySelectorAll('.game-btn').forEach(btn => btn.classList.remove('active'));
                    document.querySelectorAll('.game-section').forEach(section => section.classList.remove('active'));
                    
                    // Add active class to clicked button and corresponding section
                    this.classList.add('active');
                    currentGame = this.dataset.game;
                    document.getElementById(currentGame).classList.add('active');
                    
                    // Initialize the selected game
                    initGame(currentGame);
                });
            });
            
            // Initialize the first game
            initGame(currentGame);
            
            // Set up drag and drop for sentence builder
            setupDragAndDrop();
        };

        // Initialize the selected game
        function initGame(gameType) {
            switch(gameType) {
                case "fill-blanks":
                    totalQuestions = questions.length;
                    document.getElementById('total').textContent = totalQuestions;
                    shuffleQuestions();
                    break;
                case "sentence-builder":
                    resetSentenceBuilder();
                    break;
            }
            score = 0;
            document.getElementById('score').textContent = score;
        }

        // Fill in the blanks functions
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        function renderQuestions() {
            const container = document.getElementById('questions-container');
            container.innerHTML = '';
            
            questions.forEach((q, index) => {
                const questionDiv = document.createElement('div');
                questionDiv.className = 'question';
                questionDiv.innerHTML = `
                    <h3>Question ${index + 1}:</h3>
                    <p>${q.question}</p>
                    <div class="feedback" id="feedback-${q.id}" style="display: none;"></div>
                `;
                container.appendChild(questionDiv);
            });
        }

        function shuffleQuestions() {
            questions = shuffleArray([...questions]);
            renderQuestions();
            
            // Show confirmation message
            const feedback = document.createElement('div');
            feedback.className = 'feedback correct';
            feedback.textContent = 'Sentences shuffled!';
            feedback.style.marginTop = '10px';
            feedback.style.marginBottom = '20px';
            
            const container = document.getElementById('questions-container');
            container.parentNode.insertBefore(feedback, container);
            
            // Remove message after 2 seconds
            setTimeout(() => {
                feedback.remove();
            }, 2000);
        }

        function checkFillBlanks() {
            const blanks = document.querySelectorAll('.fill-blank');
            let correctCount = 0;
            
            blanks.forEach((blank, index) => {
                const userAnswer = blank.value.toLowerCase().trim();
                const correctAnswer = blank.dataset.answer.toLowerCase();
                
                if (userAnswer === correctAnswer) {
                    blank.classList.remove('incorrect');
                    blank.classList.add('correct');
                    correctCount++;
                } else {
                    blank.classList.remove('correct');
                    blank.classList.add('incorrect');
                }
                
                // Find question ID from input field
                const inputId = blank.closest('.question').querySelector('.feedback').id.split('-')[1];
                const feedback = document.getElementById(`feedback-${inputId}`);
                
                if (userAnswer === correctAnswer) {
                    feedback.textContent = '✅ Correct!';
                    feedback.className = 'feedback correct';
                } else {
                    feedback.textContent = `❌ Incorrect. The correct answer is: "${blank.dataset.answer}"`;
                    feedback.className = 'feedback incorrect';
                }
                feedback.style.display = 'block';
            });
            
            // Update score
            score = correctCount;
            document.getElementById('score').textContent = score;
            
            // Show final score
            showFinalScore(correctCount, questions.length);
        }

        // Sentence Builder functions
        function resetSentenceBuilder() {
            const wordPool = document.getElementById('word-pool');
            const sentenceArea = document.getElementById('sentence-area');
            const feedback = document.getElementById('builder-feedback');
            
            // Clear previous content
            wordPool.innerHTML = '';
            sentenceArea.innerHTML = '<p>Drag words here to build your sentence</p>';
            feedback.style.display = 'none';
            
            // Select a random sentence template
            const randomTemplate = sentenceTemplates[Math.floor(Math.random() * sentenceTemplates.length)];
            const words = randomTemplate.split(' ');
            
            // Shuffle the words
            const shuffledWords = shuffleArray([...words]);
            
            // Add words to the word pool
            shuffledWords.forEach(word => {
                const wordElement = document.createElement('div');
                wordElement.className = 'word-piece';
                wordElement.textContent = word;
                wordElement.setAttribute('draggable', 'true');
                wordPool.appendChild(wordElement);
            });
            
            // Set the correct sentence for validation
            sentenceArea.dataset.correctSentence = randomTemplate;
        }

        function setupDragAndDrop() {
            const sentenceArea = document.getElementById('sentence-area');
            
            // Drag start event
            document.addEventListener('dragstart', function(e) {
                if (e.target.classList.contains('word-piece')) {
                    draggedWord = e.target;
                    e.target.classList.add('dragging');
                    setTimeout(() => {
                        e.target.style.display = 'none';
                    }, 0);
                }
            });
            
            // Drag end event
            document.addEventListener('dragend', function(e) {
                if (e.target.classList.contains('word-piece')) {
                    e.target.classList.remove('dragging');
                    e.target.style.display = 'block';
                    draggedWord = null;
                }
            });
            
            // Drag over event
            sentenceArea.addEventListener('dragover', function(e) {
                e.preventDefault();
                this.classList.add('hover');
            });
            
            // Drag leave event
            sentenceArea.addEventListener('dragleave', function() {
                this.classList.remove('hover');
            });
            
            // Drop event
            sentenceArea.addEventListener('drop', function(e) {
                e.preventDefault();
                this.classList.remove('hover');
                
                if (draggedWord) {
                    // Remove the "Drag words here" message if it exists
                    if (this.querySelector('p')) {
                        this.innerHTML = '';
                    }
                    
                    // Create a new word element in the sentence area
                    const newWord = document.createElement('span');
                    newWord.className = 'sentence-word';
                    newWord.textContent = draggedWord.textContent;
                    
                    // Add click event to remove the word
                    newWord.addEventListener('click', function() {
                        this.remove();
                        // If no words left, show the message again
                        if (sentenceArea.children.length === 0) {
                            sentenceArea.innerHTML = '<p>Drag words here to build your sentence</p>';
                        }
                    });
                    
                    this.appendChild(newWord);
                }
            });
        }

        function checkSentence() {
            const sentenceArea = document.getElementById('sentence-area');
            const feedback = document.getElementById('builder-feedback');
            
            // Get the user's sentence
            let userSentence = '';
            if (sentenceArea.children.length > 0) {
                userSentence = Array.from(sentenceArea.children)
                    .map(word => word.textContent)
                    .join(' ')
                    .toLowerCase();
            }
            
            // Get the correct sentence
            const correctSentence = sentenceArea.dataset.correctSentence.toLowerCase();
            
            // Check if the sentence matches
            if (userSentence === correctSentence) {
                feedback.textContent = '✅ Correct! Well done!';
                feedback.className = 'feedback correct';
                score++;
                document.getElementById('score').textContent = score;
            } else {
                feedback.textContent = `❌ Not quite. Try again! The correct sentence is: "${sentenceArea.dataset.correctSentence}"`;
                feedback.className = 'feedback incorrect';
            }
            
            feedback.style.display = 'block';
        }

        // Utility function to show final score
        function showFinalScore(correct, total) {
            const finalFeedback = document.createElement('div');
            finalFeedback.className = 'feedback ' + (correct === total ? 'correct' : 'incorrect');
            finalFeedback.textContent = correct === total ? 
                '🎉 Congratulations! You answered all questions correctly!' : 
                `You scored ${correct} out of ${total}. Keep practicing!`;
            
            finalFeedback.style.marginTop = '20px';
            
            const checkBtn = document.querySelector('.check-btn');
            checkBtn.parentNode.insertBefore(finalFeedback, checkBtn.nextSibling);
            
            // Remove message after 5 seconds
            setTimeout(() => {
                finalFeedback.remove();
            }, 5000);
        }
    </script>
</body>
</html>
