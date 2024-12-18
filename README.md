<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Personal Fitness Drag-and-Drop Game</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background-color: #f4f4f9;
      color: #333;
    }
    .container {
      width: 90%;
      margin: 20px auto;
      text-align: center;
    }
    h1 {
      font-size: 2.5em;
      color: #4caf50;
    }
    .instructions {
      font-size: 1.2em;
      margin: 20px 0;
    }
    .game-section {
      margin: 20px 0;
    }
    .word-bank, .questions {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 15px;
    }
    .word {
      background-color: #e0f7fa;
      padding: 10px 20px;
      border: 2px dashed #00796b;
      border-radius: 5px;
      cursor: grab;
    }
    .question {
      background-color: #fff;
      border: 2px solid #4caf50;
      padding: 10px;
      border-radius: 5px;
      min-width: 300px;
      text-align: left;
      position: relative;
    }
    .dropzone {
      border: 2px dashed #aaa;
      background-color: #f9f9f9;
      padding: 10px;
      min-height: 30px;
      margin-top: 10px;
      border-radius: 5px;
    }
    .score-section {
      margin-top: 30px;
      font-size: 1.5em;
      color: #ff5722;
    }
    .submit-section {
      margin-top: 20px;
    }
    .button {
      background-color: #4caf50;
      color: white;
      padding: 10px 20px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    .button:hover {
      background-color: #45a049;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Personal Fitness Drag-and-Drop Game</h1>
    <div class="instructions">
      <p>Welcome! Drag the correct word from the Word Bank to complete each question. Each question is worth 1 point. At the end, submit your name to see your grade and share it!</p>
    </div>

    <!-- Input Name -->
    <div class="game-section" id="name-section">
      <label for="player-name">Enter Your Full Name:</label>
      <input type="text" id="player-name" placeholder="Full Name">
      <button class="button" id="start-game">Start Game</button>
    </div>

    <!-- Word Bank -->
    <div class="game-section" id="word-bank">
      <h2>Word Bank</h2>
      <div class="word-bank" id="word-bank-words">
        <!-- Words dynamically added here -->
      </div>
    </div>

    <!-- Questions -->
    <div class="game-section" id="questions-section">
      <h2>Questions</h2>
      <div class="questions" id="questions">
        <!-- Questions dynamically added here -->
      </div>
    </div>

    <!-- Score Section -->
    <div class="score-section" id="score-section">
      <p>Score: <span id="score">0</span>/30</p>
    </div>

    <!-- Submit Section -->
    <div class="submit-section" id="submit-section">
      <button class="button" id="submit-button">Submit</button>
      <div id="grade-result"></div>
    </div>
  </div>

  <script>
    // Game Data
    const wordBank = [
      "Pectoralis", "Cholesterol", "Overload, progression, and specificity", "Lunges", "Height & weight", "Inactivity",
      "Frequency, intensity, and time", "Heat stroke", "Fast twitch", "Weight lifting exercises and aerobic exercise", "Pacer/mile", "Lowering one’s heart rate",
      "Cool down", "Carbohydrates", "Cardiovascular fitness", "Muscular strength", "The amount of weight lifted", "Hot dry skin, no sweat, and unsteady walking",
      "Muscular endurance", "Lower resting heart rate", "Stretching exercises", "Where you want your exercise heart rate to receive fitness benefits",
      "Cardiovascular endurance", "The number of repetitions performed", "Push-up", "Protein", "Sit & reach, slow jogging, and yoga", "10-15%", "Decrease",
      "Squats, isotonic exercises, biceps curls", "Oxygen", "Cycling, in-line skating, swimming", "Plaque", "Flexibility", "Zone 3", "Daily", "Sit-n-reach"
    ];

    const questions = [
      "__________ is an exercise that strengthens the glutes/gluteals.",
      "Using _________________________ are the three ways to apply the overload principle.",
      "The __________ are strengthened by performing bench press and push-ups.",
      "Sprinting and powerlifting are exercises that use _________ muscle fibers.",
      "The ______________ measures your cardiovascular system?",
      "High levels of __________ frequently lead to heart disease.",
      "The greatest health risk factor which a person can control is __________.",
      "________________ is NOT a benefit of warming up before beginning to exercise.",
      "The most serious heat-related heat illness is __________.",
      "___________ measures your body composition.",
      "A person's basal metabolic rate (BMR) slows down as they grow older. Exercises that would increase the BMR would include ______________________________.",
      "____________________________ are the three principles of training that should be followed in developing your personal fitness program."
    ];

    let score = 0;

    // DOM Elements
    const wordBankWords = document.getElementById("word-bank-words");
    const questionsContainer = document.getElementById("questions");
    const scoreElement = document.getElementById("score");

    // Populate Word Bank
    wordBank.forEach(word => {
      const wordElement = document.createElement("div");
      wordElement.textContent = word;
      wordElement.classList.add("word");
      wordElement.draggable = true;
      wordElement.addEventListener("dragstart", dragStart);
      wordBankWords.appendChild(wordElement);
    });

    // Populate Questions
    questions.forEach((question, index) => {
      const questionElement = document.createElement("div");
      questionElement.classList.add("question");

      const questionText = document.createElement("p");
      questionText.textContent = question;
      questionElement.appendChild(questionText);

      const dropzone = document.createElement("div");
      dropzone.classList.add("dropzone");
      dropzone.addEventListener("dragover", dragOver);
      dropzone.addEventListener("drop", drop);
      questionElement.appendChild(dropzone);

      questionsContainer.appendChild(questionElement);
    });

    // Drag and Drop Handlers
    function dragStart(event) {
      event.dataTransfer.setData("text", event.target.textContent);
    }

    function dragOver(event) {
      event.preventDefault();
    }

    function drop(event) {
      event.preventDefault();
      const word = event.dataTransfer.getData("text");
      if (!event.target.textContent) {
        event.target.textContent = word;
      }
    }

    // Submit Game
    document.getElementById("submit-button").addEventListener("click", () => {
      const dropzones = document.querySelectorAll(".dropzone");
      dropzones.forEach((zone, index) => {
        // Placeholder logic for scoring, replace with correct answers
        if (zone.textContent.trim()) {
          score += 1; // Increment score for filled dropzones
        }
      });
      scoreElement.text
