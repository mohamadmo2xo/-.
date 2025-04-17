<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>اختبار موسوعي</title>
    <style>
        :root {
            --primary: #2c3e50;
            --secondary: #3498db;
            --accent: #e74c3c;
            --light: #ecf0f1;
        }

        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            min-height: 100vh;
        }

        .start-screen {
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            color: white;
            text-align: center;
        }

        .designer-text {
            font-size: 2.5em;
            margin-bottom: 10px;
            background: linear-gradient(45deg, #ff6b6b, #4ecdc4, #45b7d1);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            animation: gradient 3s ease infinite;
        }

        .title-text {
            font-size: 3.5em;
            margin: 15px 0;
            background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            animation: gradient 2s ease infinite;
        }

        .version-text {
            font-size: 1.2em;
            color: #bdc3c7;
            margin-bottom: 30px;
        }

        .start-btn {
            padding: 15px 40px;
            font-size: 1.2em;
            background: #e74c3c;
            border: none;
            border-radius: 30px;
            color: white;
            cursor: pointer;
            transition: all 0.3s;
        }

        .start-btn:hover {
            transform: scale(1.1);
            box-shadow: 0 0 20px rgba(231, 76, 60, 0.5);
        }

        .container {
            max-width: 800px;
            margin: 20px auto;
            background: white;
            border-radius: 15px;
            box-shadow: 0 0 20px rgba(0,0,0,0.1);
            padding: 30px;
            display: none;
        }

        .question-card {
            display: none;
            animation: fadeIn 0.5s;
        }

        .question-card.active {
            display: block;
        }

        .options {
            margin: 20px 0;
        }

        .option {
            background: var(--light);
            padding: 15px;
            margin: 10px 0;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
        }

        .option:hover {
            transform: translateX(10px);
            background: var(--secondary);
            color: white;
        }

        .navigation {
            display: flex;
            justify-content: flex-end;
            margin-top: 30px;
        }

        button {
            background: var(--secondary);
            color: white;
            border: none;
            padding: 10px 25px;
            border-radius: 25px;
            cursor: pointer;
            transition: all 0.3s;
        }

        button:hover {
            opacity: 0.9;
            transform: scale(1.05);
        }

        .progress-bar {
            height: 8px;
            background: #eee;
            border-radius: 4px;
            margin-bottom: 20px;
        }

        .progress {
            height: 100%;
            background: var(--secondary);
            transition: width 0.5s;
        }

        .timer {
            position: fixed;
            top: 20px;
            left: 20px;
            background: rgba(0,0,0,0.5);
            color: white;
            padding: 10px 20px;
            border-radius: 20px;
            font-size: 1.2em;
            display: none;
        }

        .results {
            text-align: center;
            padding: 30px;
        }

        .score {
            font-size: 2.5em;
            color: var(--secondary);
            margin: 20px 0;
        }

        .final-rating {
            font-size: 2em;
            color: #27ae60;
            margin: 15px 0;
        }

        .question-result {
            margin: 15px 0;
            padding: 15px;
            border-radius: 8px;
        }

        .correct {
            background: #e8f5e9;
            border: 2px solid #4caf50;
        }

        .wrong {
            background: #ffebee;
            border: 2px solid #f44336;
        }

        .correct-answer {
            color: #2e7d32;
            font-weight: bold;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @keyframes gradient {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        @media (max-width: 768px) {
            .container {
                margin: 10px;
                padding: 15px;
            }
            
            .option {
                padding: 10px;
                font-size: 0.9em;
            }
            
            .title-text {
                font-size: 2.5em;
            }
        }
    </style>
</head>
<body>
    <div class="start-screen" id="startScreen">
        <div class="designer-text">mo2xo</div>
        <div class="title-text">اشلفها</div>
        <div class="version-text">نسخة تجريبية 1.0</div>
        <button class="start-btn" onclick="startTest()">ابدأ الاختبار</button>
    </div>

    <div class="container" id="mainContainer">
        <div class="progress-bar">
            <div class="progress" style="width: 0%"></div>
        </div>
        <div id="questions-container"></div>
    </div>

    <div class="timer" id="timer">الوقت المتبقي: 30 ثانية</div>

    <script>
        const originalQuestions = [
            {
                question: "ما هو الكوكب الأقرب إلى الشمس؟",
                options: ["الزهرة", "المريخ", "عطارد"],
                correct: 2
            },
            {
                question: "ما هي عاصمة اليابان؟",
                options: ["أوساكا", "طوكيو", "كيوطو"],
                correct: 1
            },
            {
                question: "كم عدد أضلاع المثلث؟",
                options: ["2", "3", "4"],
                correct: 1
            },
            {
                question: "أي من هذه الحيوانات من الثدييات؟",
                options: ["التمساح", "الخفاش", "الأفعى"],
                correct: 1
            },
            {
                question: "ما هي العملة الرسمية لألمانيا؟",
                options: ["اليورو", "الدولار", "الجنيه"],
                correct: 0
            },
            {
                question: "في أي عام هبط الإنسان على القمر؟",
                options: ["1965", "1969", "1972"],
                correct: 1
            },
            {
                question: "ما هو الرمز الكيميائي للذهب؟",
                options: ["Ag", "Fe", "Au"],
                correct: 2
            },
            {
                question: "أي من هذه اللغات تنتمي إلى عائلة اللغات الرومانسية؟",
                options: ["الألمانية", "الفرنسية", "العربية"],
                correct: 1
            },
            {
                question: "ما هو أطول نهر في العالم؟",
                options: ["النيل", "الأمازون", "الميسيسيبي"],
                correct: 0
            },
            {
                question: "من رسم لوحة الموناليزا؟",
                options: ["فان جوخ", "بيكاسو", "ليوناردو دافنشي"],
                correct: 2
            },
            {
                question: "من أول قائل لكلمة 'طرم' بالأرضي السورية؟",
                options: ["بحبوح", "جهجوه", "عبود"],
                correct: 0
            },
            {
                question: "ما العنصر الكيميائي الذي رمزه 'Fe'؟",
                options: ["الحديد", "الفلور", "الذهب"],
                correct: 0
            },
            {
                question: "ما وحدة قياس القوة في الفيزياء؟",
                options: ["نيوتن", "جول", "وات"],
                correct: 0
            },
            {
                question: "أي من هذه العناصر غاز نبيل؟",
                options: ["الهيدروجين", "الهيليوم", "الأكسجين"],
                correct: 1
            },
            {
                question: "ما اسم العالم الذي اكتشف الجاذبية؟",
                options: ["أينشتاين", "نيوتن", "غاليليو"],
                correct: 1
            },
            {
                question: "ما هو الرقم الهيدروجيني للماء النقي؟",
                options: ["5", "7", "9"],
                correct: 1
            },
            {
                question: "أي من هذه المواد موصل جيد للكهرباء؟",
                options: ["المطاط", "النحاس", "الخشب"],
                correct: 1
            },
            {
                question: "ما هو أسرع حيوان بري؟",
                options: ["الفهد", "النمر", "الأسد"],
                correct: 0
            },
            {
                question: "ما اسم العملية التي تنتج الطاقة في الشمس؟",
                options: ["الانشطار النووي", "الاندماج النووي", "التأين"],
                correct: 1
            },
            {
                question: "ما هو العدد الذري للكربون؟",
                options: ["6", "12", "14"],
                correct: 0
            }
        ];

        let questions = [];
        let currentQuestion = 0;
        let userAnswers = [];
        let timer;
        let timeLeft = 30;

        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
        }

        function prepareQuestions() {
            questions = JSON.parse(JSON.stringify(originalQuestions));
            shuffleArray(questions);
            
            questions.forEach(q => {
                const correctAnswer = q.options[q.correct];
                shuffleArray(q.options);
                q.correct = q.options.indexOf(correctAnswer);
            });
        }

        function startTest() {
            prepareQuestions();
            document.getElementById('startScreen').style.display = 'none';
            document.getElementById('mainContainer').style.display = 'block';
            document.getElementById('timer').style.display = 'block';
            initTest();
            startTimer();
        }

        function initTest() {
            const container = document.getElementById('questions-container');
            container.innerHTML = '';
            
            questions.forEach((q, index) => {
                const card = document.createElement('div');
                card.className = 'question-card';
                card.innerHTML = `
                    <h2>السؤال ${index + 1}</h2>
                    <p>${q.question}</p>
                    <div class="options">
                        ${q.options.map((opt, i) => `
                            <div class="option" onclick="selectAnswer(${index}, ${i})">${opt}</div>
                        `).join('')}
                    </div>
                    <div class="navigation">
                        ${index < questions.length - 1 ? 
                            `<button onclick="nextQuestion()">التالي</button>` : 
                            `<button onclick="submitTest()">إنهاء الاختبار</button>`}
                    </div>
                `;
                container.appendChild(card);
            });

            showQuestion(currentQuestion);
            updateProgress();
        }

        function showQuestion(index) {
            document.querySelectorAll('.question-card').forEach(card => {
                card.classList.remove('active');
            });
            document.querySelectorAll('.question-card')[index].classList.add('active');
        }

        function selectAnswer(questionIndex, optionIndex) {
            userAnswers[questionIndex] = optionIndex;
            document.querySelectorAll(`#questions-container .question-card:nth-child(${questionIndex + 1}) .option`).forEach(opt => {
                opt.style.background = 'var(--light)';
            });
            event.target.style.background = 'var(--secondary)';
        }

        function nextQuestion() {
            if (currentQuestion < questions.length - 1) {
                currentQuestion++;
                resetTimer();
                showQuestion(currentQuestion);
                updateProgress();
            }
        }

        function updateProgress() {
            const progress = (currentQuestion + 1) / questions.length * 100;
            document.querySelector('.progress').style.width = `${progress}%`;
        }

        function startTimer() {
            clearInterval(timer);
            timeLeft = 30;
            document.getElementById('timer').textContent = `الوقت المتبقي: ${timeLeft} ثانية`;
            
            timer = setInterval(() => {
                timeLeft--;
                document.getElementById('timer').textContent = `الوقت المتبقي: ${timeLeft} ثانية`;
                
                if(timeLeft <= 0) {
                    clearInterval(timer);
                    if(currentQuestion < questions.length - 1) {
                        nextQuestion();
                    } else {
                        submitTest();
                    }
                }
            }, 1000);
        }

        function resetTimer() {
            startTimer();
        }

        function submitTest() {
            clearInterval(timer);
            const correctAnswers = questions.filter((q, i) => userAnswers[i] === q.correct).length;
            const score = (correctAnswers / questions.length * 100).toFixed(2);
            const rating = getRating(score);

            document.getElementById('mainContainer').innerHTML = `
                <div class="results">
                    <h2>نتيجة الاختبار</h2>
                    <div class="score">${score}%</div>
                    <div class="final-rating">${rating}</div>
                    
                    <div id="detailed-results">
                        ${questions.map((q, i) => {
                            const userAnswer = userAnswers[i];
                            const isCorrect = userAnswer === q.correct;
                            return `
                                <div class="question-result ${isCorrect ? 'correct' : 'wrong'}">
                                    <p>السؤال ${i+1}: ${q.question}</p>
                                    <p>إجابتك: ${userAnswer !== undefined ? q.options[userAnswer] : 'لم تُجب'} ${isCorrect ? '✅' : '❌'}</p>
                                    <p class="correct-answer">الإجابة الصحيحة: ${q.options[q.correct]}</p>
                                </div>
                            `;
                        }).join('')}
                    </div>
                    
                    <button onclick="restartTest()">إعادة الاختبار</button>
                </div>
            `;
        }

        function getRating(score) {
            if(score >= 90) return "ممتاز 🎉";
            if(score >= 80) return "جيد جدًا 👍";
            if(score >= 70) return "جيد 👌";
            return "يحتاج تحسين 💪";
        }

        function restartTest() {
            currentQuestion = 0;
            userAnswers = [];
            document.getElementById('mainContainer').style.display = 'none';
            document.getElementById('timer').style.display = 'none';
            document.getElementById('startScreen').style.display = 'flex';
            prepareQuestions();
        }

        // تهيئة الأسئلة عند التحميل
        prepareQuestions();
    </script>
</body>
</html>
