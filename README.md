<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Quiz Game</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: Arial, Helvetica, sans-serif;
    }

    body {
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      background: linear-gradient(135deg, #4f46e5, #0ea5e9);
      padding: 20px;
    }

    .quiz-container {
      width: 100%;
      max-width: 700px;
      background: #ffffff;
      border-radius: 18px;
      box-shadow: 0 20px 50px rgba(0, 0, 0, 0.2);
      overflow: hidden;
    }

    .quiz-header {
      background: #111827;
      color: white;
      padding: 24px;
      text-align: center;
    }

    .quiz-header h1 {
      font-size: 2rem;
      margin-bottom: 8px;
    }

    .quiz-header p {
      color: #cbd5e1;
      font-size: 0.95rem;
    }

    .quiz-body {
      padding: 28px;
    }

    .top-bar {
      display: flex;
      justify-content: space-between;
      gap: 12px;
      margin-bottom: 18px;
      font-size: 0.95rem;
      color: #374151;
      flex-wrap: wrap;
    }

    .progress-box {
      margin-bottom: 18px;
    }

    .progress-label {
      display: flex;
      justify-content: space-between;
      margin-bottom: 8px;
      font-size: 0.9rem;
      color: #4b5563;
    }

    .progress-bar {
      width: 100%;
      height: 10px;
      background: #e5e7eb;
      border-radius: 999px;
      overflow: hidden;
    }

    .progress-fill {
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, #4f46e5, #0ea5e9);
      transition: width 0.3s ease;
    }

    #question {
      font-size: 1.35rem;
      color: #111827;
      margin-bottom: 20px;
      line-height: 1.5;
    }

    .options {
      display: grid;
      gap: 14px;
      margin-bottom: 24px;
    }

    .option-btn {
      width: 100%;
      text-align: left;
      border: 2px solid #d1d5db;
      background: #f9fafb;
      padding: 14px 16px;
      border-radius: 12px;
      cursor: pointer;
      font-size: 1rem;
      transition: all 0.2s ease;
    }

    .option-btn:hover {
      border-color: #4f46e5;
      background: #eef2ff;
    }

    .option-btn.selected {
      border-color: #4f46e5;
      background: #e0e7ff;
      font-weight: bold;
    }

    .option-btn.correct {
      border-color: #16a34a;
      background: #dcfce7;
      color: #14532d;
    }

    .option-btn.wrong {
      border-color: #dc2626;
      background: #fee2e2;
      color: #7f1d1d;
    }

    .controls {
      display: flex;
      justify-content: space-between;
      gap: 12px;
      flex-wrap: wrap;
    }

    .btn {
      border: none;
      padding: 12px 20px;
      border-radius: 12px;
      cursor: pointer;
      font-size: 1rem;
      font-weight: bold;
      transition: transform 0.2s ease, opacity 0.2s ease;
    }

    .btn:hover {
      transform: translateY(-1px);
    }

    .btn-primary {
      background: #4f46e5;
      color: white;
    }

    .btn-secondary {
      background: #111827;
      color: white;
    }

    .btn:disabled {
      opacity: 0.55;
      cursor: not-allowed;
      transform: none;
    }

    .result-box {
      text-align: center;
      padding: 20px 0 0;
      display: none;
    }

    .result-box h2 {
      font-size: 2rem;
      margin-bottom: 12px;
      color: #111827;
    }

    .result-box p {
      font-size: 1.1rem;
      color: #4b5563;
      margin-bottom: 22px;
    }

    .score {
      font-size: 1.4rem;
      color: #4f46e5;
      font-weight: bold;
    }

    @media (max-width: 600px) {
      .quiz-body {
        padding: 20px;
      }

      #question {
        font-size: 1.15rem;
      }

      .controls {
        flex-direction: column;
      }

      .btn {
        width: 100%;
      }
    }
  </style>
</head>
<body>
  <div class="quiz-container">
    <div class="quiz-header">
      <h1>Quiz Game</h1>
      <p>Answer the questions and check your score at the end.</p>
    </div>

    <div class="quiz-body">
      <div class="top-bar">
        <div id="questionCount">Question 1 of 5</div>
        <div id="scoreText">Score: 0</div>
      </div>

      <div class="progress-box">
        <div class="progress-label">
          <span>Progress</span>
          <span id="progressPercent">0%</span>
        </div>
        <div class="progress-bar">
          <div class="progress-fill" id="progressFill"></div>
        </div>
      </div>

      <div id="quizSection">
        <h2 id="question">Question text will appear here</h2>
        <div class="options" id="options"></div>

        <div class="controls">
          <button class="btn btn-secondary" id="prevBtn" onclick="prevQuestion()" disabled>Previous</button>
          <button class="btn btn-primary" id="nextBtn" onclick="nextQuestion()">Next</button>
        </div>
      </div>

      <div class="result-box" id="resultBox">
        <h2>Quiz Completed!</h2>
        <p>Your final score is:</p>
        <div class="score" id="finalScore">0 / 5</div>
        <br />
        <button class="btn btn-primary" onclick="restartQuiz()">Restart Quiz</button>
      </div>
    </div>
  </div>

  <script>
    const quizData = [
      {
        question: "What does HTML stand for?",
        options: [
          "Hyper Text Markup Language",
          "High Tech Modern Language",
          "Home Tool Markup Language",
          "Hyper Transfer Mark Language"
        ],
        answer: 0
      },
      {
        question: "Which tag is used to create a hyperlink in HTML?",
        options: ["<link>", "<a>", "<href>", "<p>"],
        answer: 1
      },
      {
        question: "Which language is used for styling web pages?",
        options: ["JavaScript", "Python", "CSS", "C++"],
        answer: 2
      },
      {
        question: "Which method is used to print output in JavaScript?",
        options: ["console.log()", "print()", "echo()", "show()"],
        answer: 0
      },
      {
        question: "Which symbol is used for comments in JavaScript?",
        options: ["<!-- -->", "//", "**", "##"],
        answer: 1
      }
    ];

    let currentQuestion = 0;
    let score = 0;
    let selectedAnswer = null;
    let answered = Array(quizData.length).fill(false);
    let userAnswers = Array(quizData.length).fill(null);

    const questionEl = document.getElementById("question");
    const optionsEl = document.getElementById("options");
    const questionCountEl = document.getElementById("questionCount");
    const scoreTextEl = document.getElementById("scoreText");
    const progressFillEl = document.getElementById("progressFill");
    const progressPercentEl = document.getElementById("progressPercent");
    const prevBtn = document.getElementById("prevBtn");
    const nextBtn = document.getElementById("nextBtn");
    const quizSection = document.getElementById("quizSection");
    const resultBox = document.getElementById("resultBox");
    const finalScoreEl = document.getElementById("finalScore");

    function loadQuestion() {
      const current = quizData[currentQuestion];
      questionEl.textContent = current.question;
      questionCountEl.textContent = `Question ${currentQuestion + 1} of ${quizData.length}`;
      scoreTextEl.textContent = `Score: ${score}`;
      optionsEl.innerHTML = "";
      selectedAnswer = userAnswers[currentQuestion];

      current.options.forEach((option, index) => {
        const button = document.createElement("button");
        button.className = "option-btn";
        button.textContent = option;
        button.onclick = () => selectAnswer(index);

        if (answered[currentQuestion]) {
          button.disabled = true;
          if (index === current.answer) button.classList.add("correct");
          if (index === userAnswers[currentQuestion] && userAnswers[currentQuestion] !== current.answer) {
            button.classList.add("wrong");
          }
        } else if (selectedAnswer === index) {
          button.classList.add("selected");
        }

        optionsEl.appendChild(button);
      });

      prevBtn.disabled = currentQuestion === 0;
      nextBtn.textContent = currentQuestion === quizData.length - 1 ? "Finish" : "Next";

      const progress = (currentQuestion / quizData.length) * 100;
      progressFillEl.style.width = `${progress}%`;
      progressPercentEl.textContent = `${Math.round(progress)}%`;
    }

    function selectAnswer(index) {
      if (answered[currentQuestion]) return;
      selectedAnswer = index;
      userAnswers[currentQuestion] = index;
      loadQuestion();
    }

    function nextQuestion() {
      if (selectedAnswer === null) {
        alert("Please select an answer before moving on.");
        return;
      }

      if (!answered[currentQuestion]) {
        if (selectedAnswer === quizData[currentQuestion].answer) {
          score++;
        }
        answered[currentQuestion] = true;
      }

      if (currentQuestion < quizData.length - 1) {
        currentQuestion++;
        loadQuestion();
      } else {
        showResult();
      }
    }

    function prevQuestion() {
      if (currentQuestion > 0) {
        currentQuestion--;
        loadQuestion();
      }
    }

    function showResult() {
      quizSection.style.display = "none";
      resultBox.style.display = "block";
      finalScoreEl.textContent = `${score} / ${quizData.length}`;
      progressFillEl.style.width = "100%";
      progressPercentEl.textContent = "100%";
      questionCountEl.textContent = "Quiz Finished";
      scoreTextEl.textContent = `Score: ${score}`;
    }

    function restartQuiz() {
      currentQuestion = 0;
      score = 0;
      selectedAnswer = null;
      answered = Array(quizData.length).fill(false);
      userAnswers = Array(quizData.length).fill(null);

      quizSection.style.display = "block";
      resultBox.style.display = "none";
      loadQuestion();
    }

    loadQuestion();
  </script>
</body>
</html>
