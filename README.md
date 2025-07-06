
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>First Grade Fun Quizzes</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Comic+Neue:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Comic Neue', cursive;
            background-color: #f0f9ff; /* Light blue sky background */
        }
        .quiz-container {
            background-color: white;
            border-radius: 20px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.1);
            border: 5px solid #60a5fa; /* A friendly blue border */
        }
        .btn {
            transition: transform 0.2s, box-shadow 0.2s;
            border-radius: 12px;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 1px;
            padding: 1rem 2rem;
        }
        .btn:hover {
            transform: translateY(-4px);
            box-shadow: 0 8px 15px rgba(0,0,0,0.15);
        }
        .btn-green {
            background-color: #4ade80; /* A friendly green */
            color: white;
            border-bottom: 4px solid #22c55e;
        }
        .btn-yellow {
            background-color: #facc15; /* A sunny yellow */
            color: #422006;
            border-bottom: 4px solid #eab308;
        }
        .btn-blue {
            background-color: #60a5fa; /* A calm blue */
            color: white;
            border-bottom: 4px solid #3b82f6;
        }
        .option-btn {
            background-color: #e0f2fe;
            color: #0c4a6e;
            border: 2px solid #7dd3fc;
            width: 100%;
            text-align: center;
            font-size: 1.5rem;
        }
        .option-btn:hover {
            background-color: #bae6fd;
        }
        .option-btn.correct {
            background-color: #4ade80;
            color: white;
            border-color: #22c55e;
        }
        .option-btn.incorrect {
            background-color: #f87171;
            color: white;
            border-color: #ef4444;
        }
        #feedback-message {
            transition: opacity 0.5s;
        }
    </style>
</head>
<body class="flex items-center justify-center min-h-screen p-4">

    <div id="quiz-app" class="quiz-container w-full max-w-2xl mx-auto p-6 md:p-8">

        <!-- Category Selection Screen -->
        <div id="category-screen">
            <h1 class="text-4xl md:text-5xl font-bold text-center text-blue-600 mb-2">Welcome!</h1>
            <p class="text-xl text-center text-gray-600 mb-8">Choose a Quiz!</p>
            <div class="space-y-6">
                <button onclick="startQuiz('math')" class="w-full btn btn-green">
                    <span class="text-3xl mr-4">🔢</span> Math
                </button>
                <button onclick="startQuiz('spelling')" class="w-full btn btn-yellow">
                    <span class="text-3xl mr-4">🔤</span> Spelling
                </button>
                <button onclick="startQuiz('science')" class="w-full btn btn-blue">
                     <span class="text-3xl mr-4">🐾</span> Science
                </button>
            </div>
        </div>

        <!-- Quiz Screen -->
        <div id="quiz-screen" class="hidden">
            <div class="flex justify-between items-center mb-6">
                <div id="quiz-category" class="text-2xl font-bold text-blue-500"></div>
                <div class="text-xl font-bold text-gray-700">Score: <span id="score">0</span></div>
            </div>
            <div id="question-container" class="text-center">
                <div id="image-container" class="mb-4"></div>
                <p id="question-text" class="text-2xl md:text-3xl font-semibold text-gray-800 mb-8"></p>
                <div id="options-container" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <!-- Options will be injected here by JS -->
                </div>
            </div>
            <div id="feedback-container" class="mt-8 text-center">
                <p id="feedback-message" class="text-3xl font-bold opacity-0">&nbsp;</p>
            </div>
            <div class="mt-8 flex justify-end">
                <button id="next-btn" class="hidden btn btn-green">Next</button>
            </div>
        </div>
        
        <!-- Final Score Screen -->
        <div id="score-screen" class="hidden text-center">
            <h2 class="text-5xl font-bold text-yellow-500 mb-4">Great Job!</h2>
            <p class="text-2xl text-gray-700 mb-2">Your final score is:</p>
            <p id="final-score" class="text-6xl font-extrabold text-blue-600 mb-8"></p>
            <button onclick="restartQuiz()" class="btn btn-blue">Play Again!</button>
        </div>

    </div>

    <script>
        // --- Quiz Data ---
        const quizData = {
            math: [
                { question: "What is 2 + 3?", options: ["4", "5", "6"], answer: "5" },
                { question: "What is 5 + 1?", options: ["6", "7", "5"], answer: "6" },
                { question: "What is 4 + 4?", options: ["8", "9", "7"], answer: "8" },
                { question: "What is 7 - 2?", options: ["5", "6", "4"], answer: "5" },
                { question: "What is 3 + 5?", options: ["7", "8", "9"], answer: "8" }
            ],
            spelling: [
                { image: 'https://placehold.co/200x150/fde68a/422006?text=☀️', question: "How do you spell the word for this?", options: ["sun", "son", "sin"], answer: "sun" },
                { image: 'https://placehold.co/200x150/e0f2fe/0c4a6e?text=🐈', question: "How do you spell the word for this?", options: ["cot", "cat", "cut"], answer: "cat" },
                { image: 'https://placehold.co/200x150/fecaca/991b1b?text=🍎', question: "How do you spell the word for this?", options: ["apple", "appel", "aple"], answer: "apple" },
                { image: 'https://placehold.co/200x150/dcfce7/14532d?text=🐸', question: "How do you spell the word for this?", options: ["frog", "flog", "frag"], answer: "frog" },
                { image: 'https://placehold.co/200x150/e9d5ff/581c87?text=🚗', question: "How do you spell the word for this?", options: ["car", "cor", "cur"], answer: "car" }
            ],
            science: [
                { image: 'https://placehold.co/200x150/fca5a5/7f1d1d?text=🦁', question: "What animal is this?", options: ["Tiger", "Lion", "Bear"], answer: "Lion" },
                { image: 'https://placehold.co/200x150/fdba74/854d0e?text=🐘', question: "What animal is this?", options: ["Elephant", "Hippo", "Rhino"], answer: "Elephant" },
                { image: 'https://placehold.co/200x150/a5f3fc/0e7490?text=🐧', question: "What animal is this?", options: ["Duck", "Chicken", "Penguin"], answer: "Penguin" },
                { image: 'https://placehold.co/200x150/fef08a/854d0e?text=🦒', question: "What animal is this?", options: ["Zebra", "Giraffe", "Horse"], answer: "Giraffe" },
                { image: 'https://placehold.co/200x150/d1d5db/1f2937?text=🐵', question: "What animal is this?", options: ["Monkey", "Gorilla", "Chimp"], answer: "Monkey" }
            ]
        };

        // --- State Variables ---
        let currentCategory = '';
        let currentQuestions = [];
        let currentQuestionIndex = 0;
        let score = 0;

        // --- DOM Elements ---
        const categoryScreen = document.getElementById('category-screen');
        const quizScreen = document.getElementById('quiz-screen');
        const scoreScreen = document.getElementById('score-screen');
        
        const quizCategoryEl = document.getElementById('quiz-category');
        const scoreEl = document.getElementById('score');
        const questionTextEl = document.getElementById('question-text');
        const imageContainerEl = document.getElementById('image-container');
        const optionsContainerEl = document.getElementById('options-container');
        const feedbackMessageEl = document.getElementById('feedback-message');
        const nextBtn = document.getElementById('next-btn');
        const finalScoreEl = document.getElementById('final-score');

        // --- Functions ---

        /**
         * Starts the quiz for a selected category.
         * @param {string} category - The selected quiz category ('math', 'spelling', 'science').
         */
        function startQuiz(category) {
            currentCategory = category;
            // Shuffle questions to make it new each time
            currentQuestions = [...quizData[category]].sort(() => Math.random() - 0.5);
            currentQuestionIndex = 0;
            score = 0;

            categoryScreen.classList.add('hidden');
            quizScreen.classList.remove('hidden');
            scoreScreen.classList.add('hidden');
            
            quizCategoryEl.textContent = category.charAt(0).toUpperCase() + category.slice(1);
            scoreEl.textContent = score;

            loadQuestion();
        }

        /**
         * Loads the current question and its options onto the page.
         */
        function loadQuestion() {
            // Reset state from previous question
            optionsContainerEl.innerHTML = '';
            imageContainerEl.innerHTML = '';
            nextBtn.classList.add('hidden');
            feedbackMessageEl.classList.add('opacity-0');
            feedbackMessageEl.innerHTML = '&nbsp;';


            const question = currentQuestions[currentQuestionIndex];
            questionTextEl.textContent = question.question;

            // Display image if it exists for the question
            if (question.image) {
                const img = document.createElement('img');
                img.src = question.image;
                img.alt = "Quiz Image";
                img.className = "mx-auto rounded-lg mb-4";
                img.onerror = () => img.src = 'https://placehold.co/200x150/cccccc/ffffff?text=Image+Error';
                imageContainerEl.appendChild(img);
            }

            // Create and display option buttons
            question.options.forEach(option => {
                const button = document.createElement('button');
                button.textContent = option;
                button.className = "btn option-btn";
                button.onclick = () => selectAnswer(button, option);
                optionsContainerEl.appendChild(button);
            });
        }
        
        /**
         * Handles the logic when a user selects an answer.
         * @param {HTMLElement} buttonEl - The button element that was clicked.
         * @param {string} selectedOption - The text content of the selected option.
         */
        function selectAnswer(buttonEl, selectedOption) {
            // Disable all option buttons after an answer is chosen
            const optionButtons = optionsContainerEl.querySelectorAll('button');
            optionButtons.forEach(btn => btn.disabled = true);
            
            const question = currentQuestions[currentQuestionIndex];
            const isCorrect = selectedOption === question.answer;

            if (isCorrect) {
                score++;
                scoreEl.textContent = score;
                buttonEl.classList.add('correct');
                feedbackMessageEl.textContent = 'Correct! 🎉';
                feedbackMessageEl.style.color = '#22c55e';
            } else {
                buttonEl.classList.add('incorrect');
                feedbackMessageEl.textContent = 'Try again! 😊';
                feedbackMessageEl.style.color = '#ef4444';
                // Highlight the correct answer
                optionButtons.forEach(btn => {
                    if(btn.textContent === question.answer) {
                        btn.classList.add('correct');
                    }
                });
            }
            
            feedbackMessageEl.classList.remove('opacity-0');
            nextBtn.classList.remove('hidden');
        }

        /**
         * Handles moving to the next question or finishing the quiz.
         */
        function goToNextQuestion() {
            currentQuestionIndex++;
            if (currentQuestionIndex < currentQuestions.length) {
                loadQuestion();
            } else {
                showFinalScore();
            }
        }
        
        /**
         * Displays the final score screen.
         */
        function showFinalScore() {
            quizScreen.classList.add('hidden');
            scoreScreen.classList.remove('hidden');
            finalScoreEl.textContent = `${score} / ${currentQuestions.length}`;
        }
        
        /**
         * Resets the quiz and returns to the category selection screen.
         */
        function restartQuiz() {
            scoreScreen.classList.add('hidden');
            categoryScreen.classList.remove('hidden');
        }

        // --- Event Listeners ---
        nextBtn.addEventListener('click', goToNextQuestion);

    </script>
</body>
</html>







# some-important
i build a basic front end 
