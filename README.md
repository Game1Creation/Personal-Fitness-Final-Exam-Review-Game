<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Personal Fitness Final Exam Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f3f3f3;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        header {
            background-color: #28a745;
            color: white;
            padding: 10px 0;
            width: 100%;
            text-align: center;
            font-size: 24px;
        }

        .game-container {
            width: 80%;
            max-width: 1200px;
            margin: 20px auto;
            background: white;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
            border-radius: 8px;
            padding: 20px;
        }

        .question {
            margin-bottom: 20px;
        }

        .question h3 {
            margin: 0 0 10px 0;
        }

        .answers {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }

        .answers div {
            border: 2px dashed #ccc;
            padding: 10px;
            background-color: #f9f9f9;
            cursor: pointer;
            text-align: center;
            border-radius: 4px;
        }

        .answers div.dragging {
            opacity: 0.5;
        }

        .drop-zone {
            min-height: 40px;
            border: 2px dashed #007bff;
            padding: 10px;
            border-radius: 4px;
            text-align: center;
            background-color: #e9f7fe;
        }

        .completed {
            background-color: #28a745;
            color: white;
            font-weight: bold;
        }

        .submit-button {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            border-radius: 4px;
        }

        .submit-button:hover {
            background-color: #0056b3;
        }

        footer {
            margin-top: 20px;
            font-size: 14px;
        }

        .grade-display {
            display: none;
            margin-top: 20px;
            font-size: 18px;
            background-color: #f8f9fa;
            padding: 10px;
            border-radius: 8px;
            text-align: center;
        }
    </style>
</head>
<body>
    <header>
        Personal Fitness Final Exam Game
    </header>

    <div class="game-container">
        <div id="name-entry">
            <h2>Enter Your Full Name:</h2>
            <input type="text" id="player-name" placeholder="First and Last Name">
            <button class="submit-button" onclick="startGame()">Start</button>
        </div>

        <div id="game-content" style="display: none;">
            <h2>Match the Questions with the Correct Answers!</h2>

            <div id="questions"></div>

            <button class="submit-button" onclick="submitAnswers()">Submit Answers</button>

            <div id="grade-display" class="grade-display"></div>
        </div>
    </div>

    <footer>
        Designed for educational purposes.
    </footer>

    <script>
        const part1 = {
            questions: [
                "__________ is an exercise that strengthens the glutes/gluteals.",
                "Using _________________________ are the three ways to apply the overload principle",
                "The __________ are strengthened by performing bench press and push-ups",
                "Sprinting and powerlifting are exercises that use _________ muscle fibers",
                "The ______________ measures your cardiovascular system?",
                "High levels of __________ frequently lead to heart disease",
                "The greatest health risk factor which a person can control is __________.",
                "________________ is NOT a benefit of warming up before beginning to exercise",
                "The most serious heat-related heat illness is __________.",
                "___________ measures your body composition.",
                "A person's basal metabolic rate (BMR) slows down as they grow older. Exercises that would increase the BMR would include ______________________________.",
                "____________________________ are the three principles of training that should be followed in developing your personal fitness program."
            ],
            answers: [
                "Lunges",
                "Overload, progression, and specificity",
                "Pectoralis",
                "Fast twitch",
                "Pacer/mile",
                "Cholesterol",
                "Inactivity",
                "Lowering one’s heart rate",
                "Heat stroke",
                "Height & weight",
                "Weight lifting exercises and aerobic exercise",
                "Frequency, intensity, and time"
            ]
        };

        const part2 = {
            questions: [
                "________________ in the body’s primary source of energy",
                "Target heart rate zone is _______________________________________",
                "The primary difference between muscular strength is __________________ and muscular endurance training is _________________________",
                "________________ is the ability of muscles to exert a force one time",
                "_________________ is developed by using less weight and more repetitions is called.",
                "Jogging benefits ____________________ the most.",
                "_________________ will increase your flexibility.",
                "Performing a _____________ routine after exercise prevents muscles from being sore, lightheadedness, and blood from pooling in your muscles.",
                "Symptoms of heat stroke are ___________________________.",
                "The ability of the heart, blood, blood vessels, and the respiratory system to supply oxygen and necessary fuel to the muscles during exercise is called________________________.",
                "Participating in cardiovascular exercise in the target heart rate zone will result in a ____________________."
            ],
            answers: [
                "Carbohydrates",
                "Where you want your exercise heart rate to receive fitness benefits",
                "The amount of weight lifted",
                "Muscular strength",
                "Muscular endurance",
                "Cardiovascular fitness",
                "Stretching exercises",
                "Cool down",
                "Hot dry skin, no sweat, and unsteady walking",
                "Cardiovascular endurance",
                "Lower resting heart rate"
            ]
        };

        const parts = [part1, part2];

        let playerName = "";
        
        function startGame() {
            playerName = document.getElementById('player-name').value;
            if (!playerName.trim()) {
                alert("Please enter your name to start the game.");
                return;
            }
            document.getElementById('name-entry').style.display = 'none';
            document.getElementById('game-content').style.display = 'block';
            loadQuestions();
        }

        function loadQuestions() {
            const questionContainer = document.getElementById('questions');
            questionContainer.innerHTML = '';
            
            parts.forEach((part, index) => {
                const partDiv = document.createElement('div');
                partDiv.classList.add('part-section');
                partDiv.innerHTML = `<h3>Part ${index + 1}</h3>`;

                part.questions.forEach((question, qIndex) => {
                    const questionDiv = document.createElement('div');
                    questionDiv.classList.add('question');
                    questionDiv.innerHTML = `<h3>${question}</h3><div class="drop-zone" data-answer="${part.answers[qIndex]}"></div>`;
                    partDiv.appendChild(questionDiv);
                });

                const answersDiv = document.createElement('div');
                answersDiv.classList.add('answers');
                part.answers.forEach(answer => {
                    const answerDiv = document.createElement('div');
                    answerDiv.textContent = answer;
                    answerDiv.draggable = true;
                    answerDiv.addEventListener('dragstart', handleDragStart);
                    answerDiv.addEventListener('dragend', handleDragEnd);
                    answersDiv.appendChild(answerDiv);
                });

                partDiv.appendChild
