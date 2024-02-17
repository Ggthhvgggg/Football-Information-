<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>معلومات كرة القدم</title>
<style>
  body {
    font-family: Arial, sans-serif;
    text-align: center;
    background-color: #f5f5f5;
    margin: 0;
    padding: 0;
  }

  h1 {
    margin-top: 20px;
    color: #333;
  }

  button {
    background-color: #4CAF50;
    color: white;
    padding: 15px 32px;
    text-align: center;
    font-size: 16px;
    margin: 10px;
    cursor: pointer;
    border: none;
    border-radius: 10px;
  }

  button:hover {
    background-color: #45a049;
  }

  #question-container {
    background-color: #fff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    margin: 20px auto;
    max-width: 600px;
    display: none;
  }

  #question {
    font-size: 20px;
    margin-bottom: 20px;
    color: #333;
  }

  #answer-input {
    width: 80%;
    padding: 10px;
    font-size: 16px;
    margin-bottom: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
    box-sizing: border-box;
  }

  #timer {
    font-size: 18px;
    color: #333;
    margin-bottom: 20px;
  }

  #points {
    font-size: 18px;
    color: #333;
    margin-bottom: 20px;
  }

  #hints {
    font-size: 18px;
    color: #333;
    margin-bottom: 20px;
  }

  #settings-container,
  #game-maker-info,
  #support-container {
    background-color: #fff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    margin: 20px auto;
    max-width: 600px;
    display: none;
  }

  #feedback {
    width: 80%;
    padding: 10px;
    font-size: 16px;
    margin-bottom: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
    box-sizing: border-box;
  }
</style>
</head>
<body>
<h1>معلومات كرة القدم</h1>
<button onclick="showEasyQuestions()">سهل</button>
<button onclick="showHardQuestions()">صعب</button>
<button onclick="showLearningMode()">طور تعليمي</button>
<button onclick="showSettings()">اعدادات</button>
<button onclick="showGameMakerInfo()">صانع اللعبة</button>
<button onclick="showSupport()">الدعم الفني</button>

<div id="question-container">
  <p id="question"></p>
  <input type="text" id="answer-input" placeholder="أدخل إجابتك">
  <button onclick="checkAnswer()">تحقق من الإجابة</button>
  <p id="timer"></p>
  <p id="points"></p>
  <p id="hints"></p>
</div>

<div id="settings-container">
  <button onclick="changeLanguage('arabic')">اللغة: عربي</button>
  <button onclick="changeLanguage('english')">اللغة: انجليزي</button>
</div>

<div id="game-maker-info">
  <p>صانع اللعبة هو عمر حمدي الزملي</p>
  <p>صنعت بتاريخ: 10/2/2024</p>
</div>

<div id="support-container">
  <p>مرحبا بكم في لعبتنا. إذا واجهتم أي مشاكل، يرجى إرسال ملاحظتكم.</p>
  <textarea id="feedback" rows="4" cols="50" placeholder="أكتب ملاحظتك هنا..."></textarea>
  <button onclick="sendFeedback()">ارسال ملاحظات</button>
</div>

<script>
  var easyQuestions = [
    { question: "ما هو اسم اول من فاز بجائزة الكرة الذهبية؟", answer: "لويس سواريز", hint: "لاعب أوروغوياني" },
    { question: "ما هو عدد اللاعبين في كل فريق في كرة القدم؟", answer: "11 لاعباً", hint: "عدد معين من اللاعبين" },
    { question: "ما هو اسم أول نادي فاز بدوري أبطال أوروبا؟", answer: "ريال مدريد", hint: "نادي إسباني" }
  ];

  var hardQuestions = [
    { question: "ما هي الدولة التي فازت بكأس العالم لكرة القدم أكثر من أي دولة أخرى؟", answer: "البرازيل", hint: "في أمريكا الجنوبية" },
    { question: "ما هو اسم النادي الذي فاز بدوري أبطال أوروبا في عام 2020؟", answer: "باريس سان جيرمان", hint: "نادي فرنسي" },
    { question: "من هو أكثر لاعب فاز بجائزة الكرة الذهبية؟", answer: "ليونيل ميسي", hint: "لاعب أرجنتيني" }
  ];

  var currentQuestion;
  var currentAnswer;
  var currentHint;
  var points = 0;
  var timer;
  var secondsRemaining = 10;

  function showQuestion(question, answer, hint) {
    currentQuestion = question;
    currentAnswer = answer;
    currentHint = hint;
    document.getElementById("question").textContent = question;
    document.getElementById("answer-input").value = "";
    document.getElementById("question-container").style.display = "block";
    startTimer();
    document.getElementById("hints").textContent = "تلميح: " + hint;
  }

  function showEasyQuestions() {
    var randomIndex = Math.floor(Math.random() * easyQuestions.length);
    var question = easyQuestions[randomIndex].question;
    var answer = easyQuestions[randomIndex].answer;
    var hint = easyQuestions[randomIndex].hint;
    showQuestion(question, answer, hint);
  }

  function showHardQuestions() {
    var randomIndex = Math.floor(Math.random() * hardQuestions.length);
    var question = hardQuestions[randomIndex].question;
    var answer = hardQuestions[randomIndex].answer;
    var hint = hardQuestions[randomIndex].hint;
    showQuestion(question, answer, hint);
  }

  function startTimer() {
    secondsRemaining = 10;
    updateTimer();
    timer = setInterval(updateTimer, 1000);
  }

  function updateTimer() {
    document.getElementById("timer").textContent = "الوقت المتبقي: " + secondsRemaining + " ثانية";
    secondsRemaining--;
    if (secondsRemaining < 0) {
      clearInterval(timer);
      document.getElementById("timer").textContent = "انتهى الوقت!";
      document.getElementById("question-container").style.display = "none";
    }
  }

  function checkAnswer() {
    var userAnswer = document.getElementById("answer-input").value.trim();
    if (userAnswer === currentAnswer) {
      points++;
      document.getElementById("points").textContent = "عدد النقاط: " + points;
      clearInterval(timer);
      document.getElementById("timer").textContent = "إجابة صحيحة!";
    } else {
      document.getElementById("timer").textContent = "إجابة خاطئة! حاول مرة أخرى.";
    }
  }

  function showSettings() {
    hideAllContainers();
    document.getElementById("settings-container").style.display = "block";
  }

  function changeLanguage(language) {
    if (language === "english") {
      document.body.dir = "ltr";
    } else {
      document.body.dir = "rtl";
    }
  }

  function showGameMakerInfo() {
    hideAllContainers();
    document.getElementById("game-maker-info").style.display = "block";
  }

  function showSupport() {
    hideAllContainers();
    document.getElementById("support-container").style.display = "block";
  }

  function sendFeedback() {
    var feedback = document.getElementById("feedback").value;
    alert("تم استلام ملاحظاتكم: " + feedback);
    document.getElementById("feedback").value = "";
  }

  function hideAllContainers() {
    document.getElementById("question-container").style.display = "none";
    document.getElementById("settings-container").style.display = "none";
    document.getElementById("game-maker-info").style.display = "none";
    document.getElementById("support-container").style.display = "none";
  }

  function showLearningMode() {
    hideAllContainers();
    var learningQuestions = easyQuestions.concat(hardQuestions);
    var randomIndex = Math.floor(Math.random() * learningQuestions.length);
    var question = learningQuestions[randomIndex].question;
    var answer = learningQuestions[randomIndex].answer;
    var hint = learningQuestions[randomIndex].hint;
    showQuestion(question, answer, hint);
  }
</script>
</body>
</html>
