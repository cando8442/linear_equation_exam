<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>일차함수 레벨업 진단기 v4.0</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Noto Sans KR', sans-serif;
        }
        .option-btn {
            transition: all 0.2s ease-in-out;
        }
        .option-btn.selected {
            background-color: #3b82f6;
            color: white;
            border-color: #3b82f6;
            transform: scale(1.05);
        }
        .progress-bar-fill {
            transition: width 0.5s ease-in-out;
        }
        .feedback-popup {
            animation: fadeInOut 1.5s forwards;
        }
        @keyframes fadeInOut {
            0% { opacity: 0; transform: translate(-50%, -50%) scale(0.8); }
            20% { opacity: 1; transform: translate(-50%, -50%) scale(1); }
            80% { opacity: 1; transform: translate(-50%, -50%) scale(1); }
            100% { opacity: 0; transform: translate(-50%, -50%) scale(0.8); }
        }
        /* Ensure responsive text sizes */
        h1 { font-size: 2.25rem; } /* 36px */
        h2 { font-size: 1.5rem; } /* 24px */
        p, button, input { font-size: 1rem; } /* 16px */
        @media (min-width: 640px) {
            h1 { font-size: 2.5rem; } /* 40px */
            h2 { font-size: 1.75rem; } /* 28px */
            p, button, input { font-size: 1.125rem; } /* 18px */
        }
    </style>
</head>
<body class="bg-gray-100 flex items-center justify-center min-h-screen p-2 sm:p-4">
    <div id="app" class="w-full max-w-2xl mx-auto">
        <!-- App content will be rendered here by JavaScript -->
    </div>

    <script type="module">
        // ==================================================================
        // 교사 설정: 제공된 URL로 교체 완료되었습니다.
        // ==================================================================
        const SCRIPT_URL = 'https://script.google.com/macros/s/AKfycbwbhiswOJkrzvZY0DIf01R1ARabHB1hMYO_VBbQc1X3hSBx3q0Pk9ZYnZByK1J4sypn/exec';
        // ==================================================================

        const quizData = [
            // STAGE 1
            {
                stage: 1, title: "1단계: 준비 운동 💪", description: "계산과 좌표의 기본을 확인해봐요.", passThreshold: 3,
                questions: [
                    { type: 'mcq', question: '다음을 계산하면 얼마일까요? (-2) + 7 = ?', options: ['5', '9', '-5', '-9'], answer: '5' },
                    { type: 'input', question: '네모 안에 들어갈 숫자는 무엇일까요? 3 × □ = 12', answer: '4' },
                    { type: 'mcq-img', question: '아래 그림에서 A 점의 위치를 나타내는 순서쌍은 무엇일까요?', image: `<svg viewBox="0 0 200 200" class="w-48 h-48 mx-auto border rounded-md mt-2"><line x1="100" y1="0" x2="100" y2="200" stroke="#9ca3af"/><line x1="0" y1="100" x2="200" y2="100" stroke="#9ca3af"/><circle cx="140" cy="40" r="5" fill="#ef4444"/><text x="145" y="40" font-size="12" fill="#ef4444">A(2,3)</text><text x="102" y="12" font-size="10">y</text><text x="190" y="98" font-size="10">x</text></svg>`, options: ['(2, 3)', '(3, 2)', '(-2, 3)'], answer: '(2, 3)' },
                    { type: 'mcq', question: '순서쌍 (-1, 4)를 점으로 찍으려면 어떻게 움직여야 할까요?', options: ['왼쪽 1칸, 위로 4칸', '오른쪽 1칸, 위로 4칸', '위로 1칸, 왼쪽 4칸'], answer: '왼쪽 1칸, 위로 4칸' }
                ]
            },
            // STAGE 2
            {
                stage: 2, title: "2단계: 규칙과 친해지기 🤝", description: "숫자를 넣으면 새로운 숫자가 나오는 규칙 상자를 이해해봐요.", passThreshold: 2,
                questions: [
                    { type: 'input', question: '규칙 상자 y = 3x 가 있어요. x에 2를 넣으면 y는 어떤 숫자가 나올까요?', answer: '6' },
                    { type: 'mcq', question: '다음 점들 중에서, 직선 y = -2x 위에 있지 않은 점은 무엇일까요?', options: ['(1, -2)', '(3, -6)', '(0, 0)', '(-2, -4)'], answer: '(-2, -4)' },
                    { type: 'mcq', question: 'y = ax 형태의 그래프는 항상 어떤 특별한 점을 지나갈까요?', options: ['(0, 0) 원점', '(1, 1)', '(0, 1)'], answer: '(0, 0) 원점' }
                ]
            },
            // STAGE 3
            {
                stage: 3, title: "3단계: 기울기와 시작점 🎯", description: "직선 그래프의 비밀, 기울기와 y절편을 알아봐요.", passThreshold: 2,
                questions: [
                    { type: 'mcq-img', question: '이 직선의 ‘y축 시작점(y절편)’은 얼마일까요?', image: `<svg viewBox="0 0 200 200" class="w-48 h-48 mx-auto border rounded-md mt-2"><line x1="100" y1="0" x2="100" y2="200" stroke="#9ca3af"/><line x1="0" y1="100" x2="200" y2="100" stroke="#9ca3af"/><line x1="0" y1="60" x2="200" y2="100" stroke="#3b82f6" stroke-width="2"/><circle cx="100" cy="80" r="5" fill="#ef4444"/><text x="105" y="78" font-size="12" fill="#ef4444">+1</text></svg>`, options: ['1', '2', '-1'], answer: '1' },
                    { type: 'mcq', question: 'y = 3x - 4 라는 식에서, ‘기울기’를 알려주는 숫자는 무엇일까요?', options: ['3', '-4', 'x'], answer: '3' },
                    { type: 'mcq-img', question: '그래프의 점 A에서 오른쪽으로 1칸 갔을 때, 위/아래로 몇 칸 움직여야 직선과 다시 만나나요?', image: `<svg viewBox="0 0 200 200" class="w-48 h-48 mx-auto border rounded-md mt-2"><line x1="100" y1="0" x2="100" y2="200" stroke="#9ca3af"/><line x1="0" y1="100" x2="200" y2="100" stroke="#9ca3af"/><line x1="50" y1="200" x2="150" y2="0" stroke="#3b82f6" stroke-width="2"/><circle cx="100" cy="100" r="5" fill="#ef4444"/><text x="90" y="95" font-size="12" fill="#ef4444">A</text><path d="M 100 100 L 120 100 L 120 60" stroke-dasharray="2 2" stroke="#16a34a" fill="none"/><text x="102" y="115" font-size="10" fill="#16a34a">오른쪽 1칸</text><text x="122" y="85" font-size="10" fill="#16a34a">아래로 2칸</text></svg>`, options: ['위로 2칸', '아래로 1칸', '아래로 2칸'], answer: '아래로 2칸' }
                ]
            },
            // STAGE 4
            {
                stage: 4, title: "4단계: 식으로 완성하기 ✨", description: "그래프를 보고, 또 다른 형태의 식을 이해해봐요!", passThreshold: 3,
                questions: [
                    { type: 'mcq-img', question: '아래 그래프를 나타내는 일차함수 식은 무엇일까요?', image: `<svg viewBox="0 0 200 200" class="w-48 h-48 mx-auto border rounded-md mt-2"><line x1="100" y1="0" x2="100" y2="200" stroke="#9ca3af"/><line x1="0" y1="100" x2="200" y2="100" stroke="#9ca3af"/><line x1="0" y1="100" x2="200" y2="60" stroke="#3b82f6" stroke-width="2"/><circle cx="100" cy="80" r="5" fill="#ef4444"/><text x="105" y="78" font-size="12" fill="#ef4444">+1</text></svg>`, options: ['y = 2x + 1', 'y = 0.2x + 1', 'y = x + 1'], answer: 'y = 0.2x + 1' },
                    { type: 'mcq', question: '직선 2x + y - 3 = 0 을 y = ax + b 꼴로 바꾸면 어떻게 될까요?', options: ['y = 2x - 3', 'y = -2x + 3', 'y = 2x + 3'], answer: 'y = -2x + 3' },
                    { type: 'mcq', question: '직선 x - 2y + 6 = 0 의 y절편은 무엇일까요?', options: ['6', '-2', '3'], answer: '3' },
                    { type: 'mcq', question: '직선 3x + 2y - 1 = 0 의 기울기는 무엇일까요?', options: ['-3/2', '3', '2/3'], answer: '-3/2' }
                ]
            },
            // STAGE 5
            {
                stage: 5, title: "5단계: 직선의 방정식 활용 🚀", description: "주어진 조건으로 직선의 방정식을 직접 구해봐요!", passThreshold: 7,
                questions: [
                    { type: 'mcq', question: '점 (2, 5)를 지나고 기울기가 3인 직선의 방정식은?', options: ['y = 3x - 1', 'y = 3x + 5', 'y = 5x + 3'], answer: 'y = 3x - 1' },
                    { type: 'mcq', question: '점 (-1, 0)을 지나고 기울기가 -2인 직선의 방정식은?', options: ['y = -2x + 1', 'y = -2x - 2', 'y = -2x'], answer: 'y = -2x - 2' },
                    { type: 'mcq', question: '두 점 (1, 3)과 (3, 7)을 지나는 직선의 방정식은?', options: ['y = 2x + 1', 'y = 3x', 'y = 4x - 1'], answer: 'y = 2x + 1' },
                    { type: 'mcq', question: '두 점 (0, 5)와 (2, 1)을 지나는 직선의 방정식은?', options: ['y = -2x + 5', 'y = 2x + 5', 'y = -x + 5'], answer: 'y = -2x + 5' },
                    { type: 'mcq', question: '점 (2, 1)을 지나고 직선 y = 3x - 5 에 평행한 직선은?', options: ['y = 3x + 1', 'y = 3x - 5', 'y = x + 3'], answer: 'y = 3x - 5' },
                    { type: 'mcq', question: '점 (-3, 4)를 지나고 직선 2x + y - 1 = 0 에 평행한 직선은?', options: ['y = -2x + 4', 'y = 2x + 10', 'y = -2x - 2'], answer: 'y = -2x - 2' },
                    { type: 'mcq', question: '점 (4, 1)을 지나고 직선 y = 2x + 3 에 수직인 직선은?', options: ['y = (-1/2)x + 3', 'y = 2x - 7', 'y = (-1/2)x + 1'], answer: 'y = (-1/2)x + 3' },
                    { type: 'mcq', question: '점 (1, -2)를 지나고 직선 x - 3y + 6 = 0 에 수직인 직선은?', options: ['y = -3x + 1', 'y = (1/3)x - 7/3', 'y = 3x - 5'], answer: 'y = -3x + 1' }
                ]
            }
        ];

        const app = document.getElementById('app');
        let studentId = '';
        let studentName = '';
        let currentStageIndex = 0;
        let currentQuestionIndex = 0;
        let scores = Array(quizData.length).fill(0);
        let selectedOption = null;
        let consecutiveIncorrectAnswers = 0;
        let isAnswering = false;
        let finalResultData = {};

        function render() {
            if (isAnswering) return;
            const stageData = quizData[currentStageIndex];
            const questionData = stageData.questions[currentQuestionIndex];
            const totalStages = quizData.length;
            const progress = ((currentStageIndex * 100) / totalStages);

            app.innerHTML = `
                <div class="bg-white rounded-2xl shadow-lg p-6 sm:p-8">
                    <div class="mb-4">
                        <div class="flex justify-between mb-1">
                            <span class="text-base font-medium text-blue-700">${stageData.title}</span>
                            <span class="text-sm font-medium text-blue-700">단계 ${stageData.stage} / ${totalStages}</span>
                        </div>
                        <div class="w-full bg-gray-200 rounded-full h-2.5">
                            <div class="bg-blue-600 h-2.5 rounded-full progress-bar-fill" style="width: ${progress}%"></div>
                        </div>
                    </div>
                    <h2 class="font-bold text-gray-800 mb-2">${questionData.question}</h2>
                    <p class="text-gray-600 mb-6">${stageData.description}</p>
                    <div id="options-container" class="space-y-3 mb-8">${renderOptions(questionData)}</div>
                    <div class="flex flex-col sm:flex-row gap-4">
                        <button id="next-btn" class="w-full bg-blue-600 text-white font-bold py-3 px-4 rounded-lg hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-opacity-50 transition-transform transform hover:scale-105">제출하기</button>
                        <button id="stop-btn" class="w-full bg-red-500 text-white font-bold py-3 px-4 rounded-lg hover:bg-red-600 focus:outline-none focus:ring-2 focus:ring-red-400 focus:ring-opacity-50 transition-transform transform hover:scale-105">어려워요, 여기까지 할래요</button>
                    </div>
                </div>`;
            addEventListeners();
        }

        function renderOptions(questionData) {
            if (questionData.type.includes('mcq')) {
                let optionsHtml = questionData.options.map(option => `<button class="option-btn w-full text-left p-4 border-2 border-gray-300 rounded-lg text-gray-700 font-semibold hover:bg-blue-100 hover:border-blue-400" data-value="${option}">${option}</button>`).join('');
                return questionData.image ? `${questionData.image}<div class="mt-4">${optionsHtml}</div>` : optionsHtml;
            }
            return `<input type="text" id="answer-input" class="w-full p-4 border-2 border-gray-300 rounded-lg text-lg focus:border-blue-500 focus:ring-blue-500" placeholder="정답을 입력하세요">`;
        }
        
        function addEventListeners() {
            document.getElementById('stop-btn').addEventListener('click', () => showResult(currentStageIndex));
            document.getElementById('next-btn').addEventListener('click', handleNext);
            const optionsContainer = document.getElementById('options-container');
            if (optionsContainer.querySelector('.option-btn')) {
                optionsContainer.querySelectorAll('.option-btn').forEach(btn => {
                    btn.addEventListener('click', () => {
                        optionsContainer.querySelectorAll('.option-btn').forEach(b => b.classList.remove('selected'));
                        btn.classList.add('selected');
                        selectedOption = btn.dataset.value;
                    });
                });
            }
        }

        function handleNext() {
            if (isAnswering) return;
            const questionData = quizData[currentStageIndex].questions[currentQuestionIndex];
            let userAnswer = questionData.type.includes('mcq') ? selectedOption : document.getElementById('answer-input').value.trim();

            if (!userAnswer) {
                alert('답을 선택하거나 입력해주세요!');
                return;
            }
            isAnswering = true;
            const isCorrect = userAnswer === questionData.answer;
            showFeedback(isCorrect);
        }
        
        function showFeedback(isCorrect) {
            const feedbackEl = document.createElement('div');
            feedbackEl.className = `feedback-popup fixed top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 p-6 rounded-lg shadow-xl text-white text-2xl font-bold ${isCorrect ? 'bg-green-500' : 'bg-red-500'}`;
            feedbackEl.textContent = isCorrect ? '정답입니다! 🎉' : '아쉬워요... 🤔';
            document.body.appendChild(feedbackEl);

            if (isCorrect) {
                scores[currentStageIndex]++;
                consecutiveIncorrectAnswers = 0;
            } else {
                consecutiveIncorrectAnswers++;
            }

            setTimeout(() => {
                feedbackEl.remove();
                moveToNext();
            }, 1500);
        }

        function moveToNext() {
            if (consecutiveIncorrectAnswers >= 3) {
                showResult(currentStageIndex, true);
                return;
            }

            const stageData = quizData[currentStageIndex];
            if (currentQuestionIndex < stageData.questions.length - 1) {
                currentQuestionIndex++;
            } else {
                if (scores[currentStageIndex] >= stageData.passThreshold) {
                    if (currentStageIndex < quizData.length - 1) {
                        currentStageIndex++;
                        currentQuestionIndex = 0;
                    } else {
                        showResult(quizData.length);
                        return;
                    }
                } else {
                    showResult(currentStageIndex);
                    return;
                }
            }
            selectedOption = null;
            isAnswering = false;
            render();
        }

        function showResult(stoppedStageIndex, byError = false) {
            let title, message, icon, resultStage;
            if (stoppedStageIndex >= quizData.length) {
                title = "모든 단계를 완료했어요! 🏆";
                message = `${studentName} 학생, 정말 대단해요! 모든 문제를 해결하고 일차함수 마스터가 되었네요. 스스로의 힘으로 여기까지 온 것이 정말 자랑스러워요!`;
                icon = '🎉';
                resultStage = '모두 통과';
            } else {
                const stageData = quizData[stoppedStageIndex];
                title = `${stageData.stage}단계에서 멈췄어요!`;
                resultStage = `${stageData.stage}단계`;
                if (byError) {
                    message = `${studentName} 학생, 괜찮아요! 3번 연속으로 어려움을 겪어서 잠시 쉬어가는 거예요. 우리에겐 새로운 목표, **'${stageData.title.split(' ')[1]}'**가 생겼어요. 이 부분을 집중적으로 연습하면 금방 다음 단계로 나아갈 수 있을 거예요!`;
                } else {
                    message = `${studentName} 학생, 여기까지 온 것도 정말 잘했어요. 👍 우리에겐 새로운 목표, **'${stageData.title.split(' ')[1]}'**가 생겼어요. 이 부분을 집중적으로 연습하면 금방 다음 단계로 나아갈 수 있을 거예요!`;
                }
                icon = '🚀';
            }

            finalResultData = { studentId, studentName, resultStage };
            
            app.innerHTML = `
                <div class="bg-white rounded-2xl shadow-lg p-8 text-center">
                    <div class="text-6xl mb-4">${icon}</div>
                    <h2 class="font-bold text-gray-800 mb-4">${title}</h2>
                    <p class="text-gray-600 mb-8">${message.replace(/\*\*/g, '')}</p>
                    <div class="flex flex-col gap-4">
                        <button id="submit-btn" class="w-full bg-green-600 text-white font-bold py-3 px-4 rounded-lg hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-opacity-50 transition-transform transform hover:scale-105">결과 제출하기</button>
                        <button id="restart-btn" class="w-full bg-blue-600 text-white font-bold py-3 px-4 rounded-lg hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-opacity-50 transition-transform transform hover:scale-105">처음부터 다시하기</button>
                    </div>
                </div>`;
            document.getElementById('restart-btn').addEventListener('click', renderWelcome);
            document.getElementById('submit-btn').addEventListener('click', submitResult);
        }
        
        function submitResult() {
            const submitBtn = document.getElementById('submit-btn');
            if (!SCRIPT_URL || SCRIPT_URL === '여기에_선생님의_웹_앱_URL을_붙여넣으세요') {
                alert('교사 설정이 필요합니다. HTML 파일 상단의 SCRIPT_URL 변수에 선생님의 웹 앱 URL을 입력해주세요.');
                return;
            }

            submitBtn.disabled = true;
            submitBtn.textContent = '제출 중...';

            fetch(SCRIPT_URL, {
                method: 'POST',
                mode: 'cors',
                headers: {
                    "Content-Type": "text/plain;charset=utf-8",
                },
                body: JSON.stringify(finalResultData)
            })
            .then(res => res.json())
            .then(data => {
                if (data.status === 'success') {
                    submitBtn.textContent = '제출 완료!';
                    alert('결과가 성공적으로 제출되었습니다.');
                } else {
                    throw new Error(data.message || '알 수 없는 오류');
                }
            })
            .catch(error => {
                submitBtn.textContent = '제출 실패';
                submitBtn.disabled = false;
                alert('제출 중 오류가 발생했습니다: ' + error.message);
            });
        }

        function renderWelcome() {
            currentStageIndex = 0;
            currentQuestionIndex = 0;
            scores.fill(0);
            selectedOption = null;
            consecutiveIncorrectAnswers = 0;
            isAnswering = false;

            app.innerHTML = `
                <div class="bg-white rounded-2xl shadow-lg p-8 text-center">
                    <h1 class="font-bold text-gray-800 mb-4">일차함수 레벨업 진단기</h1>
                    <p class="text-gray-600 mb-6">'규칙 찾기' 게임을 통해 너의 실력을 확인해봐! <br>어렵다고 느껴지면 언제든 멈춰도 괜찮아.</p>
                    <div class="space-y-4 mb-8">
                        <input type="text" id="student-id-input" class="w-full p-3 border-2 border-gray-300 rounded-lg focus:border-blue-500 focus:ring-blue-500" placeholder="학번">
                        <input type="text" id="student-name-input" class="w-full p-3 border-2 border-gray-300 rounded-lg focus:border-blue-500 focus:ring-blue-500" placeholder="이름">
                    </div>
                    <button id="start-btn" class="w-full sm:w-auto bg-blue-600 text-white font-bold py-4 px-8 rounded-lg hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-opacity-50 transition-transform transform hover:scale-105">진단 시작하기!</button>
                </div>`;
            
            document.getElementById('start-btn').addEventListener('click', () => {
                studentId = document.getElementById('student-id-input').value.trim();
                studentName = document.getElementById('student-name-input').value.trim();
                if (!studentId || !studentName) {
                    alert('학번과 이름을 모두 입력해주세요!');
                    return;
                }
                render();
            });
        }

        renderWelcome();
    </script>
</body>
</html>
