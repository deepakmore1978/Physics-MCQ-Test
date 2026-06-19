<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Online MCQ Test</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 20px;
        }
        .container {
            max-width: 800px;
            margin: auto;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        h1 {
            text-align: center;
            color: #333;
        }
        #timer {
            text-align: center;
            font-size: 24px;
            font-weight: bold;
            color: red;
            margin: 15px 0;
        }
        .question {
            font-size: 18px;
            margin: 20px 0;
            font-weight: bold;
        }
        .option {
            padding: 12px;
            margin: 8px 0;
            border: 1px solid #ccc;
            border-radius: 5px;
            cursor: pointer;
        }
        .option:hover {
            background-color: #f0f0f0;
        }
        button {
            padding: 10px 20px;
            margin: 10px 5px;
            font-size: 16px;
        }
        .nav {
            text-align: center;
            margin-top: 20px;
        }
        #result {
            text-align: center;
            font-size: 22px;
            margin-top: 30px;
            display: none;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>Online MCQ Test - 25 Questions</h1>
    <p style="text-align:center;">Time Left: <span id="timer">45:00</span></p>

    <div id="test">
        <p class="question" id="question-text">1. Question text here...</p>
        
        <div id="options"></div>

        <div class="nav">
            <button onclick="prevQuestion()">Previous</button>
            <button onclick="nextQuestion()">Next</button>
            <button onclick="submitTest()" style="background-color:green;color:white;">Submit Test</button>
        </div>
    </div>

    <div id="result"></div>
</div>

<script>
// 25 Sample Questions
const questions = [
    { q: "What is the capital of France?", options: ["London", "Berlin", "Paris", "Madrid"], answer: 2 },
    { q: "Which planet is known as the Red Planet?", options: ["Venus", "Mars", "Jupiter", "Saturn"], answer: 1 },
    { q: "Who painted the Mona Lisa?", options: ["Van Gogh", "Picasso", "Leonardo da Vinci", "Michelangelo"], answer: 2 },
    { q: "What is the largest ocean?", options: ["Atlantic", "Indian", "Arctic", "Pacific"], answer: 3 },
    { q: "In which year did man first land on the Moon?", options: ["1965", "1969", "1972", "1959"], answer: 1 },
    { q: "What is the chemical symbol for Gold?", options: ["Go", "Gd", "Au", "Ag"], answer: 2 },
    { q: "Who wrote Romeo and Juliet?", options: ["Dickens", "Shakespeare", "Austen", "Twain"], answer: 1 },
    { q: "What is the square root of 144?", options: ["10", "11", "12", "14"], answer: 2 },
    { q: "Which gas do plants absorb?", options: ["Oxygen", "Nitrogen", "Carbon Dioxide", "Helium"], answer: 2 },
    { q: "How many continents are there?", options: ["5", "6", "7", "8"], answer: 2 },
    // Add more questions up to 25 (I kept 10 for brevity, you can extend)
    { q: "What is the national animal of India?", options: ["Lion", "Tiger", "Elephant", "Peacock"], answer: 1 },
    { q: "What is the currency of Japan?", options: ["Yuan", "Yen", "Won", "Ringgit"], answer: 1 },
    { q: "Who is known as Father of Computers?", options: ["Turing", "Babbage", "Gates", "Jobs"], answer: 1 },
    { q: "What is the tallest mountain?", options: ["K2", "Kangchenjunga", "Everest", "Lhotse"], answer: 2 },
    { q: "What is the hardest natural substance?", options: ["Gold", "Iron", "Diamond", "Platinum"], answer: 2 },
    { q: "How many players in a cricket team?", options: ["9", "10", "11", "12"], answer: 2 },
    { q: "Which is the largest mammal?", options: ["Elephant", "Blue Whale", "Giraffe", "Hippo"], answer: 1 },
    { q: "What is the longest river?", options: ["Amazon", "Nile", "Yangtze", "Mississippi"], answer: 1 },
    { q: "What is the capital of Australia?", options: ["Sydney", "Melbourne", "Canberra", "Perth"], answer: 2 },
    { q: "Who invented the telephone?", options: ["Edison", "Bell", "Tesla", "Marconi"], answer: 1 },
    { q: "Which vitamin comes from sunlight?", options: ["A", "B", "C", "D"], answer: 3 },
    { q: "What is the speed of light?", options: ["3×10⁶", "3×10⁸", "3×10⁵", "3×10¹⁰"], answer: 1 },
    { q: "What is the atomic number of Hydrogen?", options: ["1", "2", "3", "4"], answer: 0 },
    { q: "Where is the Great Pyramid of Giza?", options: ["India", "Egypt", "Mexico", "Peru"], answer: 1 },
    { q: "Which element is Gold?", options: ["Ag", "Fe", "Au", "Cu"], answer: 2 }
];

let currentQuestion = 0;
let answers = new Array(25).fill(null);
let timeLeft = 45 * 60; // 45 minutes
let timer;

function startTimer() {
    timer = setInterval(() => {
        timeLeft--;
        let min = Math.floor(timeLeft / 60);
        let sec = timeLeft % 60;
        document.getElementById("timer").innerHTML = `${min}:${sec < 10 ? '0' : ''}${sec}`;
        if (timeLeft <= 0) submitTest();
    }, 1000);
}

function loadQuestion() {
    const q = questions[currentQuestion];
    document.getElementById("question-text").innerHTML = `${currentQuestion + 1}. ${q.q}`;
    
    let optionsHTML = "";
    q.options.forEach((opt, i) => {
        optionsHTML += `
            <div class="option" onclick="selectAnswer(${i})">
                <input type="radio" name="answer" ${answers[currentQuestion] === i ? 'checked' : ''}> ${opt}
            </div>`;
    });
    document.getElementById("options").innerHTML = optionsHTML;
}

function selectAnswer(index) {
    answers[currentQuestion] = index;
    loadQuestion();
}

function nextQuestion() {
    if (currentQuestion < questions.length - 1) {
        currentQuestion++;
        loadQuestion();
    }
}

function prevQuestion() {
    if (currentQuestion > 0) {
        currentQuestion--;
        loadQuestion();
    }
}

function submitTest() {
    clearInterval(timer);
    let score = 0;
    for (let i = 0; i < questions.length; i++) {
        if (answers[i] === questions[i].answer) score++;
    }
    
    document.getElementById("test").style.display = "none";
    const resultDiv = document.getElementById("result");
    resultDiv.style.display = "block";
    resultDiv.innerHTML = `
        <h2>Test Completed!</h2>
        <p>Your Score: <b>${score} / ${questions.length}</b></p>
        <p>Percentage: <b>${Math.round((score / questions.length) * 100)}%</b></p>
        <button onclick="location.reload()" style="padding:10px 20px; font-size:16px;">Restart Test</button>
    `;
}

// Initialize
window.onload = function() {
    loadQuestion();
    startTimer();
};
</script>

</body>
</html>
