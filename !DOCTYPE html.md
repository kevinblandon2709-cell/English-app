<!DOCTYPE html>  
<html>  
<head>  
  <title>English</title>  
  <meta name="viewport" content="width=device-width, initial-scale=1">  
  
  <style>  
    body {  
      font-family: Arial;  
      text-align: center;  
      padding: 20px;  
      background: #eef2f3;  
    }  
  
    .card {  
      background: white;  
      padding: 20px;  
      border-radius: 15px;  
      margin: 15px auto;  
      max-width: 400px;  
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);  
    }  
  
    button {  
      padding: 10px 15px;  
      margin: 5px;  
      font-size: 16px;  
      border: none;  
      border-radius: 8px;  
      background: #4CAF50;  
      color: white;  
    }  
  
    input {  
      padding: 10px;  
      width: 80%;  
      font-size: 16px;  
      margin-top: 10px;  
    }  
  </style>  
</head>  
  
<body>  
  
<h2>English</h2>  
  
<div class="card">  
  <h3 id="level">Level: 1</h3>  
  <h3 id="score">Score: 0</h3>  
  
  <p id="sentence"></p>  
  
  <button onclick="speak(1)">🔊 Normal</button>  
  <button onclick="speak(0.6)">🐢 Slow</button>  
  <br>  
  <button onclick="startRecognition()">🎤 Speak</button>  
  
  <p id="speechResult"></p>  
</div>  
  
<div class="card">  
  <h3>Type what you hear:</h3>  
  <input id="typingInput" type="text">  
  <br>  
  <button onclick="checkTyping()">Check</button>  
  
  <p id="typingResult"></p>  
</div>  
  
<script>  
let level = 1;  
let score = 0;  
let currentSentence = "";  
  
// Sentences  
const sentences = {  
  1: ["Hello", "Good morning", "Thank you"],  
  2: ["How are you?", "I am fine", "Nice to meet you"],  
  3: ["I would like some water", "Can you help me?", "Where is the store?"]  
};  
  
function newSentence() {  
  const list = sentences[level] || sentences[3];  
  currentSentence = list[Math.floor(Math.random() * list.length)];  
  
  document.getElementById("sentence").innerText = currentSentence;  
  document.getElementById("speechResult").innerText = "";  
  document.getElementById("typingResult").innerText = "";  
  document.getElementById("typingInput").value = "";  
}  
  
function speak(rate = 1) {  
  const speech = new SpeechSynthesisUtterance(currentSentence);  
  speech.lang = "en-US";  
  speech.rate = rate;  
  speechSynthesis.speak(speech);  
}  
  
function similarity(a, b) {  
  a = a.toLowerCase();  
  b = b.toLowerCase();  
  
  let matches = 0;  
  const wordsA = a.split(" ");  
  const wordsB = b.split(" ");  
  
  wordsA.forEach(word => {  
    if (wordsB.includes(word)) matches++;  
  });  
  
  return matches / wordsA.length;  
}  
  
function startRecognition() {  
  const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();  
  recognition.lang = "en-US";  
  
  recognition.onresult = function(event) {  
    const spoken = event.results[0][0].transcript;  
    const sim = similarity(currentSentence, spoken);  
  
    if (sim > 0.8) {  
      document.getElementById("speechResult").innerText = "✅ Great pronunciation!";  
      score++;  
      updateProgress();  
    }   
    else if (sim > 0.5) {  
      document.getElementById("speechResult").innerText = "👍 Almost! Try again slowly.";  
    }   
    else {  
      document.getElementById("speechResult").innerText = "❌ Listen and try again.";  
    }  
  };  
  
  recognition.start();  
}  
  
function checkTyping() {  
  const typed = document.getElementById("typingInput").value;  
  const sim = similarity(currentSentence, typed);  
  
  if (sim === 1) {  
    document.getElementById("typingResult").innerText = "✅ Perfect!";  
    score++;  
    updateProgress();  
  }   
  else if (sim > 0.6) {  
    document.getElementById("typingResult").innerText = "👍 Almost correct!";  
  }   
  else {  
    document.getElementById("typingResult").innerText = "❌ Try again.";  
  }  
}  
  
function updateProgress() {  
  document.getElementById("score").innerText = "Score: " + score;  
  
  if (score >= 5) {  
    level++;  
    score = 0;  
    document.getElementById("level").innerText = "Level: " + level;  
    alert("Level up! 🎉");  
  }  
  
  newSentence();  
}  
  
newSentence();  
</script>  
  
</body>  
</html>  
