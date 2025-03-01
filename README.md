<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>English Pronunciation Practice</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; margin: 20px; }
        #word, #sentence { font-size: 24px; margin: 20px; }
        #feedback { margin-top: 20px; font-weight: bold; color: green; }
        select { margin: 10px; padding: 5px; }
    </style>
</head>
<body>
    <h1>English Pronunciation Practice (British)</h1>
    <label for="difficulty">Choose difficulty:</label>
    <select id="difficulty" onchange="changeDifficulty()">
        <option value="easy">Easy</option>
        <option value="medium">Medium</option>
        <option value="hard">Hard</option>
    </select>
    <p id="lesson">Today's Lesson: Pronounce the given word and sentence correctly!</p>
    <p id="word">Hello</p>
    <p id="sentence">Hello, how are you today?</p>
    <button onclick="speakWord()">Listen to Word</button>
    <button onclick="speakSentence()">Listen to Sentence</button>
    <button onclick="startRecognition()">Speak</button>
    <p id="feedback"></p>
    
    <script>
        const words = {
            easy: ["Hello", "Apple", "Water", "Dog", "Cat"],
            medium: ["Schedule", "University", "Banana", "Elephant", "Chocolate"],
            hard: ["Entrepreneur", "Phenomenon", "Miscellaneous", "Conscientious", "Architecture"]
        };
        const sentences = {
            easy: ["Hello, how are you today?", "I have an apple.", "The water is cold."],
            medium: ["My university is very big.", "The banana is yellow.", "Chocolate is my favorite treat."],
            hard: ["Being an entrepreneur requires dedication.", "It was a phenomenal experience.", "His conscientious efforts paid off."]
        };
        let currentDifficulty = "easy";
        let currentWord = words[currentDifficulty][0];
        let currentSentence = sentences[currentDifficulty][0];

        function changeDifficulty() {
            currentDifficulty = document.getElementById("difficulty").value;
            currentWord = words[currentDifficulty][Math.floor(Math.random() * words[currentDifficulty].length)];
            currentSentence = sentences[currentDifficulty][Math.floor(Math.random() * sentences[currentDifficulty].length)];
            document.getElementById("word").textContent = currentWord;
            document.getElementById("sentence").textContent = currentSentence;
        }
        
        function speakWord() {
            let utterance = new SpeechSynthesisUtterance(currentWord);
            utterance.lang = "en-GB";
            speechSynthesis.speak(utterance);
        }
        
        function speakSentence() {
            let utterance = new SpeechSynthesisUtterance(currentSentence);
            utterance.lang = "en-GB";
            speechSynthesis.speak(utterance);
        }
        
        function startRecognition() {
            let recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
            recognition.lang = "en-GB";
            recognition.start();
            
            recognition.onresult = function(event) {
                let spokenWord = event.results[0][0].transcript;
                checkPronunciation(spokenWord);
            };
        }
        
        function checkPronunciation(spoken) {
            let feedback = document.getElementById("feedback");
            if (spoken.toLowerCase() === currentWord.toLowerCase() || spoken.toLowerCase() === currentSentence.toLowerCase()) {
                feedback.textContent = "Correct pronunciation! ✅";
            } else {
                feedback.textContent = "Try again! ❌ (You said: " + spoken + ")";
            }
        }

        function setDailyLesson() {
            currentWord = words[currentDifficulty][Math.floor(Math.random() * words[currentDifficulty].length)];
            currentSentence = sentences[currentDifficulty][Math.floor(Math.random() * sentences[currentDifficulty].length)];
            document.getElementById("word").textContent = currentWord;
            document.getElementById("sentence").textContent = currentSentence;
        }

        function setDailyReminder() {
            alert("Don't forget to practice your pronunciation today!");
        }

        setDailyLesson();
        setTimeout(setDailyReminder, 5000); // Reminder after 5 seconds (for testing purposes)
    </script>
</body>
</html>
