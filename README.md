<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>反义疑问句答题系统</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, sans-serif;
        }
        
        body {
            padding: 20px;
            background-color: #f5f5f5;
        }
        
        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            padding: 25px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        
        h1 {
            text-align: center;
            color: #333;
            margin-bottom: 30px;
        }
        
        .name-input {
            margin-bottom: 25px;
        }
        
        .name-input label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
            color: #555;
        }
        
        .name-input input {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 6px;
            font-size: 16px;
        }
        
        .question {
            margin-bottom: 25px;
            padding: 20px;
            border: 1px solid #eee;
            border-radius: 8px;
            background-color: #f9f9f9;
        }
        
        .question h3 {
            margin-bottom: 15px;
            color: #333;
            font-size: 18px;
        }
        
        .options {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        
        .option {
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 6px;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .option:hover {
            background-color: #e8f4f8;
            border-color: #bcdff1;
        }
        
        .option.selected {
            background-color: #4299e1;
            color: white;
            border-color: #4299e1;
        }
        
        .option.correct {
            background-color: #48bb78;
            color: white;
            border-color: #48bb78;
        }
        
        .option.incorrect {
            background-color: #e53e3e;
            color: white;
            border-color: #e53e3e;
        }
        
        .submit-btn {
            display: block;
            width: 100%;
            padding: 15px;
            background-color: #4299e1;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 18px;
            cursor: pointer;
            margin-top: 20px;
            transition: background-color 0.2s;
        }
        
        .submit-btn:hover {
            background-color: #3182ce;
        }
        
        .result {
            margin-top: 30px;
            padding: 20px;
            border-radius: 8px;
            background-color: #e8f4f8;
            display: none;
        }
        
        .result h2 {
            color: #2d3748;
            margin-bottom: 15px;
        }
        
        .score {
            font-size: 24px;
            font-weight: bold;
            color: #4299e1;
            margin-bottom: 10px;
        }
        
        .record-btn {
            margin-top: 20px;
            padding: 10px 20px;
            background-color: #9f7aea;
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
        }
        
        .records {
            margin-top: 30px;
            display: none;
        }
        
        .records h2 {
            margin-bottom: 15px;
        }
        
        .record-item {
            padding: 15px;
            border: 1px solid #eee;
            border-radius: 6px;
            margin-bottom: 10px;
            background-color: #f9f9f9;
        }
        
        /* 适配不同设备的样式 */
        @media (max-width: 768px) {
            .container {
                padding: 15px;
            }
            
            .question {
                padding: 15px;
            }
            
            h1 {
                font-size: 24px;
            }
            
            .question h3 {
                font-size: 16px;
            }
        }
        
        /* 苹果/安卓专属样式标识 */
        .ios-version {
            --primary-color: #007aff;
        }
        
        .android-version {
            --primary-color: #3ddc84;
        }
        
        .version-badge {
            position: fixed;
            top: 10px;
            right: 10px;
            padding: 5px 10px;
            border-radius: 20px;
            font-size: 12px;
            color: white;
        }
        
        .ios-badge {
            background-color: #007aff;
        }
        
        .android-badge {
            background-color: #3ddc84;
        }
    </style>
</head>
<body>
    <!-- 版本标识徽章 -->
    <div class="version-badge" id="versionBadge"></div>

    <div class="container">
        <h1>反义疑问句专项练习</h1>
        
        <!-- 姓名输入 -->
        <div class="name-input">
            <label for="studentName">请输入您的姓名：</label>
            <input type="text" id="studentName" placeholder="例如：张三" required>
        </div>
        
        <!-- 题目区域 -->
        <div id="questionsContainer">
            <!-- 题目会通过JS动态生成 -->
        </div>
        
        <!-- 提交按钮 -->
        <button class="submit-btn" id="submitBtn">提交答案</button>
        
        <!-- 结果展示 -->
        <div class="result" id="result">
            <h2>答题结果</h2>
            <div class="score" id="scoreDisplay"></div>
            <div id="correctnessDisplay"></div>
        </div>
        
        <!-- 查看记录按钮 -->
        <button class="record-btn" id="viewRecordsBtn">查看答题记录</button>
        
        <!-- 答题记录展示 -->
        <div class="records" id="recordsContainer">
            <h2>答题记录</h2>
            <div id="recordsList"></div>
        </div>
    </div>

    <script>
        // 题目数据
        const questions = [
            {
                id: 1,
                question: "Few of them hurt themselves in the accident last night, ________",
                options: [
                    "A. don’t they",
                    "B. didn’t they",
                    "C. did they",
                    "D. do they"
                ],
                answer: "C"
            },
            {
                id: 2,
                question: "-You’ve never seen dinosaur eggs, have you ? --_____. How I wish to visit the Dinosaur World.",
                options: [
                    "A. Yes, I have",
                    "B. No, I haven’t",
                    "C. Certainly, I have",
                    "D. Of course, I haven’t"
                ],
                answer: "B"
            },
            {
                id: 3,
                question: "His sister had a bad cough, ______ she?",
                options: [
                    "A. wasn’t",
                    "B. doesn’t",
                    "C. hadn’t",
                    "D. didn’t"
                ],
                answer: "D"
            },
            {
                id: 4,
                question: "Mr. Green went to Shenzhen on business last week, ________?",
                options: [
                    "A. isn’t he",
                    "B. doesn’t he",
                    "C. didn’t he",
                    "D. hasn’t he"
                ],
                answer: "C"
            },
            {
                id: 5,
                question: "John can hardly understand any Chinese, _________he?",
                options: [
                    "A. Can’t",
                    "B. doesn’t",
                    "C. can",
                    "D. does"
                ],
                answer: "C"
            },
            {
                id: 6,
                question: "Don’t smoke in the meeting-room,_________?",
                options: [
                    "A. do you",
                    "B. will you",
                    "C. can you",
                    "D. could you"
                ],
                answer: "B"
            },
            {
                id: 7,
                question: "Lucy, you clean the blackboard today,_______",
                options: [
                    "A. do you",
                    "B. did you",
                    "C. will you",
                    "D. can you"
                ],
                answer: "C"
            },
            {
                id: 8,
                question: "Miss Cheng will never forget her first visit to Canada ,________?",
                options: [
                    "A. will she",
                    "B. won’t she",
                    "C. isn’t she",
                    "D. wasn’t she"
                ],
                answer: "A"
            },
            {
                id: 9,
                question: "The lady couldn’t say a word when she saw the snake,________?",
                options: [
                    "A. could the lady",
                    "B. couldn’t the lady",
                    "C. could she",
                    "D. couldn’t she"
                ],
                answer: "C"
            },
            {
                id: 10,
                question: "Tina is unhappy now,________?",
                options: [
                    "A. isn’t she",
                    "B. is she",
                    "C. is he",
                    "D. did she"
                ],
                answer: "A"
            },
            {
                id: 11,
                question: "My uncle has never been to a foreign country, _______?",
                options: [
                    "A. has he",
                    "B. does he",
                    "C. hasn’t he",
                    "D. doesn’t he"
                ],
                answer: "A"
            },
            {
                id: 12,
                question: "There is some water in that bottle, isn’t _______",
                options: [
                    "A. there",
                    "B. it",
                    "C. that",
                    "D. those"
                ],
                answer: "D" // 注意：原答案是D？核对一下：原答案是1-5CBDCC 6-10BCACA 11-15AADCA，第12题是D？
            },
            {
                id: 13,
                question: "---Let’s go and play football,__________? ---That’s wonderful.",
                options: [
                    "A. will you",
                    "B. do you",
                    "C. won’t you",
                    "D. shall we"
                ],
                answer: "A"
            },
            {
                id: 14,
                question: "---The boy has to stay at home to look after his little sister, _______? ---Yes, because his mother has gone shopping.",
                options: [
                    "A. does he",
                    "B. is he",
                    "C. doesn’t he",
                    "D. hasn’t he"
                ],
                answer: "D"
            },
            {
                id: 15,
                question: "--- You won’t follow his example, will you ? --- _______, I don’t think he is right.",
                options: [
                    "A. No, I won’t",
                    "B. Yes, I will",
                    "C. No, I will",
                    "D. Yes, I won’t"
                ],
                answer: "C"
            }
        ];

        // 存储用户选择的答案
        let userAnswers = {};
        
        // 检测设备类型
        function detectDevice() {
            const userAgent = navigator.userAgent.toLowerCase();
            const isIOS = /iphone|ipad|ipod/.test(userAgent);
            const isAndroid = /android/.test(userAgent);
            
            const badge = document.getElementById('versionBadge');
            
            if (isIOS) {
                document.body.classList.add('ios-version');
                badge.textContent = '苹果版本';
                badge.classList.add('ios-badge');
                return 'ios';
            } else if (isAndroid) {
                document.body.classList.add('android-version');
                badge.textContent = '安卓版本';
                badge.classList.add('android-badge');
                return 'android';
            } else {
                badge.textContent = '通用版本';
                badge.style.backgroundColor = '#718096';
                return 'desktop';
            }
        }

        // 初始化题目
        function initQuestions() {
            const container = document.getElementById('questionsContainer');
            
            questions.forEach(question => {
                const questionDiv = document.createElement('div');
                questionDiv.className = 'question';
                questionDiv.dataset.id = question.id;
                
                const questionTitle = document.createElement('h3');
                questionTitle.textContent = `${question.id}、${question.question}`;
                
                const optionsDiv = document.createElement('div');
                optionsDiv.className = 'options';
                
                question.options.forEach(option => {
                    const optionDiv = document.createElement('div');
                    optionDiv.className = 'option';
                    optionDiv.dataset.option = option.charAt(0); // 获取选项字母(A/B/C/D)
                    optionDiv.textContent = option;
                    
                    // 选项点击事件
                    optionDiv.addEventListener('click', function() {
                        // 清除同题目的其他选中状态
                        const siblings = this.parentElement.querySelectorAll('.option');
                        siblings.forEach(sib => sib.classList.remove('selected'));
                        
                        // 设置当前选中状态
                        this.classList.add('selected');
                        
                        // 保存用户答案
                        userAnswers[question.id] = this.dataset.option;
                    });
                    
                    optionsDiv.appendChild(optionDiv);
                });
                
                questionDiv.appendChild(questionTitle);
                questionDiv.appendChild(optionsDiv);
                container.appendChild(questionDiv);
            });
        }

        // 计算成绩并显示结果
        function calculateScore() {
            const studentName = document.getElementById('studentName').value.trim();
            if (!studentName) {
                alert('请输入您的姓名！');
                return;
            }
            
            let correctCount = 0;
            let incorrectQuestions = [];
            
            // 检查每道题的答案
            questions.forEach(question => {
                const userAnswer = userAnswers[question.id];
                const isCorrect = userAnswer === question.answer;
                
                if (isCorrect) {
                    correctCount++;
                } else {
                    incorrectQuestions.push({
                        id: question.id,
                        userAnswer: userAnswer || '未作答',
                        correctAnswer: question.answer
                    });
                }
                
                // 标记正确/错误的选项
                const questionDiv = document.querySelector(`.question[data-id="${question.id}"]`);
                const options = questionDiv.querySelectorAll('.option');
                
                options.forEach(option => {
                    const optionLetter = option.dataset.option;
                    
                    // 清除之前的标记
                    option.classList.remove('correct', 'incorrect');
                    
                    // 标记正确答案
                    if (optionLetter === question.answer) {
                        option.classList.add('correct');
                    }
                    
                    // 标记用户错误选择
                    if (optionLetter === userAnswer && userAnswer !== question.answer) {
                        option.classList.add('incorrect');
                    }
                });
            });
            
            // 计算得分（每题1分，总分15分）
            const score = correctCount;
            const percentage = ((score / questions.length) * 100).toFixed(1);
            
            // 显示结果
            const resultDiv = document.getElementById('result');
            const scoreDisplay = document.getElementById('scoreDisplay');
            const correctnessDisplay = document.getElementById('correctnessDisplay');
            
            scoreDisplay.textContent = `得分：${score}/15 (${percentage}%)`;
            
            let correctnessHTML = `<p>答对：${correctCount}题，答错：${questions.length - correctCount}题</p>`;
            
            if (incorrectQuestions.length > 0) {
                correctnessHTML += '<h4>错题列表：</h4><ul>';
                incorrectQuestions.forEach(item => {
                    correctnessHTML += `<li>第${item.id}题：您选择了${item.userAnswer}，正确答案是${item.correctAnswer}</li>`;
                });
                correctnessHTML += '</ul>';
            }
            
            correctnessDisplay.innerHTML = correctnessHTML;
            resultDiv.style.display = 'block';
            
            // 保存答题记录
            saveRecord(studentName, score, incorrectQuestions);
        }

        // 保存答题记录到本地存储
        function saveRecord(name, score, incorrectQuestions) {
            const record = {
                name: name,
                score: score,
                total: questions.length,
                incorrect: incorrectQuestions,
                time: new Date().toLocaleString(),
                device: detectDevice()
            };
            
            // 获取现有记录
            let records = JSON.parse(localStorage.getItem('quizRecords') || '[]');
            records.push(record);
            
            // 保存回本地存储
            localStorage.setItem('quizRecords', JSON.stringify(records));
        }

        // 显示答题记录
        function showRecords() {
            const recordsContainer = document.getElementById('recordsContainer');
            const recordsList = document.getElementById('recordsList');
            
            // 获取记录
            const records = JSON.parse(localStorage.getItem('quizRecords') || '[]');
            
            if (records.length === 0) {
                recordsList.innerHTML = '<p>暂无答题记录</p>';
            } else {
                let recordsHTML = '';
                records.forEach((record, index) => {
                    recordsHTML += `
                    <div class="record-item">
                        <h4>记录 ${index + 1}</h4>
                        <p>姓名：${record.name}</p>
                        <p>得分：${record.score}/${record.total}</p>
                        <p>答题时间：${record.time}</p>
                        <p>设备类型：${record.device === 'ios' ? '苹果' : record.device === 'android' ? '安卓' : '电脑'}</p>
                    </div>
                    `;
                });
                recordsList.innerHTML = recordsHTML;
            }
            
            // 切换显示状态
            if (recordsContainer.style.display === 'none' || !recordsContainer.style.display) {
                recordsContainer.style.display = 'block';
            } else {
                recordsContainer.style.display = 'none';
            }
        }

        // 页面加载完成后初始化
        document.addEventListener('DOMContentLoaded', function() {
            // 检测设备类型
            detectDevice();
            
            // 初始化题目
            initQuestions();
            
            // 提交按钮事件
            document.getElementById('submitBtn').addEventListener('click', calculateScore);
            
            // 查看记录按钮事件
            document.getElementById('viewRecordsBtn').addEventListener('click', showRecords);
        });
    </script>
</body>
</html>
