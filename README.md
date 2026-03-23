<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>스피드 영어 퀴즈 (Week 4 최종 완성)</title>
    <style>
        body {
            font-family: 'Pretendard', -apple-system, BlinkMacSystemFont, system-ui, Roboto, sans-serif;
            background-color: #f0f4f8;
            color: #333;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }
        .container {
            background: white;
            padding: 30px;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.08);
            max-width: 650px;
            width: 100%;
        }
        h1 { text-align: center; margin-bottom: 20px; color: #1e293b; font-size: 26px; }
        
        /* Week 탭 스타일 */
        .week-tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 25px;
            border-bottom: 2px solid #e2e8f0;
            padding-bottom: 10px;
            justify-content: center;
            flex-wrap: wrap;
        }
        .week-tab {
            padding: 10px 16px;
            border: none;
            background: none;
            font-size: 15px;
            font-weight: bold;
            color: #94a3b8;
            cursor: pointer;
            border-radius: 8px;
            transition: all 0.2s;
        }
        .week-tab.active {
            background-color: #e0f2fe;
            color: #0284c7;
        }
        .week-tab:hover:not(.active) { background-color: #f1f5f9; }

        /* 카테고리 영역 */
        .category-section {
            margin-bottom: 25px;
            background: #f8fafc;
            padding: 20px;
            border-radius: 12px;
            border: 1px solid #e2e8f0;
        }
        .category-title {
            font-size: 18px; font-weight: bold; color: #334155;
            margin-top: 0; margin-bottom: 15px; display: flex; align-items: center; gap: 8px;
        }
        
        /* 순한맛 / 매운맛 버튼 */
        .flavor-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
        @media (max-width: 480px) { .flavor-grid { grid-template-columns: 1fr; } }
        .flavor-btn {
            background: white; border: 2px solid #cbd5e1; border-radius: 10px;
            padding: 15px; text-align: left; cursor: pointer; transition: all 0.2s ease;
        }
        .flavor-btn:hover { border-color: #3b82f6; box-shadow: 0 4px 12px rgba(59, 130, 246, 0.15); transform: translateY(-2px); }
        .flavor-title { font-size: 16px; font-weight: bold; display: block; margin-bottom: 6px; }
        .flavor-desc { font-size: 13px; color: #64748b; line-height: 1.4; word-break: keep-all; }
        .t-easy { color: #10b981; } .t-hard { color: #ef4444; }
        .hidden { display: none !important; }
        
        /* 퀴즈 영역 스타일 */
        #quiz-area { display: flex; flex-direction: column; gap: 20px; }
        #question-counter { font-size: 15px; color: #64748b; font-weight: bold; text-align: center;}
        
        .question-box { 
            font-size: 22px; font-weight: bold; word-break: keep-all; line-height: 1.5; 
            background: #f8fafc; padding: 30px 20px; border-radius: 12px;
            border: 2px dashed #cbd5e1; cursor: pointer; transition: background 0.2s; text-align: center;
        }
        .question-box:hover { background: #e2e8f0; }
        .question-box::after { content: "(클릭하여 정답 확인)"; font-size: 13px; color: #94a3b8; display: block; margin-top: 10px; font-weight: normal; }

        .answer-box { 
            background: #ecfdf5; padding: 25px 20px; border-radius: 12px; border: 1px solid #a7f3d0; text-align: left;
        }
        .en-text { font-size: 22px; color: #059669; font-weight: bold; margin-bottom: 15px; line-height: 1.4; word-break: keep-all;}
        .meta-info { font-size: 14px; color: #475569; margin-bottom: 5px; background: #fff; display: inline-block; padding: 4px 10px; border-radius: 20px; border: 1px solid #e2e8f0; }
        .meaning-info { font-size: 15px; color: #b45309; margin-bottom: 15px; background: #fef3c7; padding: 8px 12px; border-radius: 8px; font-weight: 500;}
        
        .controls { display: flex; justify-content: space-between; align-items: center; margin-top: 10px; flex-wrap: wrap; gap: 10px;}
        .left-controls { display: flex; gap: 10px; }
        
        /* 버튼 스타일 */
        .btn { padding: 10px 20px; border: none; border-radius: 8px; cursor: pointer; font-weight: bold; font-size: 15px; transition: 0.2s;}
        .btn-tts { background-color: #8b5cf6; color: white; display: flex; align-items: center; gap: 5px; }
        .btn-tts:hover { background-color: #7c3aed; }
        .btn-star { background-color: #e2e8f0; color: #475569; }
        .btn-star.active { background-color: #fef08a; color: #ca8a04; border: 1px solid #fde047; }
        .btn-next { background-color: #10b981; color: white; }
        .btn-next:hover { background-color: #059669; }
        .btn-finish { background-color: #3b82f6; color: white; width: 100%; padding: 15px; font-size: 16px; margin-top: 10px; }
        .btn-finish:hover { background-color: #2563eb; }
        .btn-home { background-color: #64748b; color: white; width: 100%; padding: 15px; font-size: 16px; margin-top: 20px;}
        .btn-home:hover { background-color: #475569; }

        /* 복습(결과) 영역 스타일 */
        #review-area { display: flex; flex-direction: column; gap: 15px; }
        .review-card { background: #fff; border: 1px solid #e2e8f0; border-radius: 10px; padding: 15px; box-shadow: 0 2px 5px rgba(0,0,0,0.05); text-align: left;}
        .review-ko { font-size: 16px; font-weight: bold; color: #334155; margin-bottom: 8px; }
        .review-en { font-size: 18px; font-weight: bold; color: #059669; margin-bottom: 8px; }
        .review-empty { text-align: center; padding: 40px 20px; font-size: 18px; color: #10b981; font-weight: bold; background: #ecfdf5; border-radius: 12px;}
    </style>
</head>
<body>

<div class="container">
    <h1 id="main-title">🚀 스피드 영어 퀴즈</h1>
    
    <div id="mode-selection">
        <div class="week-tabs">
            <button class="week-tab active" onclick="setWeek(1, this)">Week 1</button>
            <button class="week-tab" onclick="setWeek(2, this)">Week 2</button>
            <button class="week-tab" onclick="setWeek(3, this)">Week 3</button>
            <button class="week-tab" onclick="setWeek(4, this)">Week 4</button>
            <button class="week-tab" onclick="setWeek('all', this)">Week 1~4 누적</button>
        </div>

        <div class="category-section">
            <h3 class="category-title">🧩 구동사 (Phrasal Verbs)</h3>
            <div class="flavor-grid">
                <button class="flavor-btn" onclick="startQuiz('phrasal-easy')">
                    <span class="flavor-title t-easy">🟢 순한맛</span>
                    <span class="flavor-desc">구동사 의미별로 짧고 쉬운 문장들이 선별해서 담겨있음</span>
                </button>
                <button class="flavor-btn" onclick="startQuiz('phrasal-hard')">
                    <span class="flavor-title t-hard">🔴 매운맛</span>
                    <span class="flavor-desc">전체 예문에서 선별됨</span>
                </button>
            </div>
        </div>

        <div class="category-section">
            <h3 class="category-title">🗣️ 영어회화 (Conversation)</h3>
            <div class="flavor-grid">
                <button class="flavor-btn" onclick="startQuiz('conv-easy')">
                    <span class="flavor-title t-easy">💬 순한맛</span>
                    <span class="flavor-desc">'대표문장'과 '교재1'만 담고 있음</span>
                </button>
                <button class="flavor-btn" onclick="startQuiz('conv-hard')">
                    <span class="flavor-title t-hard">🔥 매운맛</span>
                    <span class="flavor-desc">전체 예문에서 선별됨</span>
                </button>
            </div>
        </div>
    </div>

    <div id="quiz-area" class="hidden">
        <div id="question-counter">문제 1 / 7</div>
        
        <div class="question-box" id="ko-box" onclick="showAnswer()">
            <span id="ko-text">한국어 문장이 여기에 표시됩니다.</span>
        </div>

        <div class="answer-box hidden" id="answer-section">
            <div class="meta-info" id="source-info">출처: Day001 교재1</div>
            <div class="en-text" id="en-text">English Answer</div>
            <div class="meaning-info hidden" id="meaning-info">의미: </div>
            
            <div class="controls">
                <div class="left-controls">
                    <button class="btn btn-tts" onclick="playTTS()">🔊 듣기</button>
                    <button class="btn btn-star" id="btn-star" onclick="toggleStar()">⭐ 어려워요</button>
                </div>
                <button class="btn btn-next" id="btn-next" onclick="nextQuestion()">다음 문제 ➡</button>
            </div>
        </div>

        <button class="btn btn-finish hidden" id="btn-finish" onclick="showReview()">결과 보기 (오답 노트) 📝</button>
    </div>

    <div id="review-area" class="hidden">
        <h2 style="text-align: center; color: #1e293b; margin-top:0;">⭐ 나의 오답 노트</h2>
        <p style="text-align: center; color: #64748b; font-size: 14px; margin-top:-10px;">어려웠던 문장들을 다시 확인해 보세요!</p>
        
        <div id="review-list"></div>

        <button class="btn btn-home" onclick="resetToHome()">🏠 처음으로 돌아가기</button>
    </div>
</div>

<script>
    let currentWeekFilter = 1;

    function setWeek(week, btn) {
        currentWeekFilter = week;
        document.querySelectorAll('.week-tab').forEach(el => el.classList.remove('active'));
        btn.classList.add('active');
    }

    // 📌 구동사 전체 의미 매핑 (Week 1 ~ Week 4)
    const meanings = {
        "add up_1": "add up: 1. (수 등을) 하나하나 더하다",
        "add up_2": "add up: 2. (점진적으로 쌓여) 합계가 결국 ~가 되다",
        "add up_3": "add up: 3. (주로 부정문에서) 앞뒤가 맞다, 논리적으로 말이 되다",
        "blow away_1": "blow away: 1. (바람 등이 사물을) 멀리 날려 보내다",
        "blow away_2": "blow away: 2. (사람을) 대단히 놀라게 하거나 깊은 감명을 주다",
        "break down_1": "break down: 1. (기계나 차량이) 고장이 나다",
        "break down_2": "break down: 2. (협상이나 시스템 등이) 결렬되다, 실패로 끝나다",
        "break down_3": "break down: 3. (슬픔 등을 못 참고) 울며 무너지다, 자제력을 잃다",
        "break down_4": "break down: 4. (이해를 돕기 위해) 세부 사항을 조목조목 나누어 설명하다",
        "break up_1": "break up: 1. (연인 등이 관계를 끝내고) 헤어지다",
        "break up_2": "break up: 2. (여러 조각이나 작은 그룹으로) 분해하다, 분리하다",
        "break up_3": "break up: 3. (전화 신호 등이) 잡음과 함께 뚝뚝 끊기다",
        "brush up on_1": "brush up on: 1. (오래전 배운 지식이나 기술을) 다시 복습하다, 다듬다",
        "care for_1": "care for: 1. ~을 좋아하다 (주로 부정문/의문문에서 쓰임)",
        "care for_2": "care for: 2. ~을 보살피다, 돌보다",
        "catch on_1": "catch on: 1. (패션, 트렌드 등이) 유행하다, 인기를 끌다",
        "catch on_2": "catch on: 2. (상황, 농담, 힌트 등을) 눈치채다, 파악하다",
        "catch up on_1": "catch up on: 1. (밀린 일, 잠 등을) 보충하다, 만회하다",
        "catch up on_2": "catch up on: 2. (밀린 소식, 정보 등을) 듣다, 알아내다",
        "check on_1": "check on / check in on: 1. (상태, 안부, 건강 등을) 확인하다",
        "check out_1": "check out: 1. (흥미로운 곳, 물건 등을) 살펴보다, 확인하다",
        "check out_2": "check out: 2. (지루함 등으로) 집중력이 흐트러져 딴생각을 하다",
        "check out_3": "check out: 3. (관계 등에서) 이미 마음이 떠나 신경을 끈 상태가 되다",
        "check out_4": "check out: 4. (정보나 사실의) 진위 여부를 대조하여 검증하다",
        "come across_1": "come across: (물건이나 사람을) 우연히 발견하다, 뜻밖에 마주치다",
        "come along_1": "come along: (제안을 받고 상대와) 동행하여 따라가다",
        "come along_2": "come along: (일이 계획대로) 순조롭게 진행되어 가다",
        "come along_3": "come along: (필요한 기회나 사람이) 예상치 못하게 나타나다",
        "come around_1": "come around: (기절 상태에서) 다시 의식이 돌아오다",
        "come around_2": "come around: (계절이나 기념일 등이) 때가 되어 다시 돌아오다",
        "come around_3": "come around: (의견 차이가 있다가) 결국 상대의 입장에 동의하거나 마음을 바꾸다",
        "come around_4": "come around: (누군가 있는) 특정한 장소에 방문하다, 오다",
        "come off_1": "come off: (붙어 있던 라벨 등이) 자연스럽게 떨어지거나 벗겨지다",
        "fall off_1": "fall off: (높은 곳이나 가장자리에서) 아래로 추락하다",
        "break off_1": "break off: (충격 등으로 부서지거나 꺾여서) 떨어져 나가다",
        "come off as_1": "come off as: (남들에게) 특정한 태도나 성격처럼 비치다",
        "come across as_1": "come across as: (의도와 상관없이) 어떠한 인상을 주다",
        
        // Week 4 추가 의미 매핑
        "come up with_1": "come up with: (아이디어 등을) 자연스럽게 생각해내다",
        "come up with_2": "come up with: (급한 상황에서) 아쉬운 대로 해결책이나 돈을 마련해내다",
        "cut off_1": "cut off: (상대방의 말을) 중간에 가로막아 끊다",
        "cut off_2": "cut off: (전기, 물 공급이나 인터넷 접속을) 완전히 차단하다",
        "cut off_3": "cut off: (도로 주행 중) 다른 차 앞으로 위험하게 끼어들다",
        "cut off_4": "cut off: (외부와 담을 쌓고) 자신을 고립시키거나 단절시키다",
        "cut off_5": "cut off: (화면이나 지면 끝에서) 내용이 잘려서 보이다",
        "figure out_1": "figure out: (어려운 수학 문제나 퀴즈 등을) 논리적으로 풀다",
        "figure out_2": "figure out: (복잡한 기계나 시스템의) 작동법을 알아내다",
        "figure out_3": "figure out: (벌어진 일의) 근본적인 원인을 파악하다",
        "figure out_4": "figure out: (결정이나 타이밍 등을) 심사숙고하여 정하다",
        "figure out_5": "figure out: (특정 인물의) 본모습이나 성격을 파악하다",
        "figure out_6": "figure out: (뒤늦게 깨달음을 통해) 어떤 사실을 인지하다",
        "figure out_7": "figure out: (이유나 현상을) 도무지 이해하지 못하다",
        "figure out_8": "figure out: (비밀이나 거짓을) 결국 알아채다, 눈치채다",
        "fill in for_1": "fill in for: (부재중이거나 아픈 사람을 위해) 잠시 업무를 대신하다"
    };

    // 1. 구동사 순한맛 (Week 1 ~ Week 4 통합)
    const phrasalEasy = [
        // Week 1
        { week: 1, ko: "자, 이 숫자들을 더해 보자.", en: "Let’s add up these numbers now.", source: "Day 001 순한맛", meaning: meanings["add up_1"] },
        { week: 1, ko: "별것 아니게 보일 수 있어도, 하루 10분의 연습도 쌓이면 정말 큽니다.", en: "It might not seem like much, but 10 minutes of practice every day really adds up.", source: "Day 001 순한맛", meaning: meanings["add up_2"] },
        { week: 1, ko: "뭔가 앞뒤가 안 맞잖아.", en: "Something doesn’t add up.", source: "Day 001 순한맛", meaning: meanings["add up_3"] },
        { week: 1, ko: "모자가 강풍에 날아가 버렸다.", en: "My hat blew away in the strong wind.", source: "Day 002 순한맛", meaning: meanings["blow away_1"] },
        { week: 1, ko: "정말 놀랐어요!", en: "You really blew me away!", source: "Day 002 순한맛", meaning: meanings["blow away_2"] },
        { week: 1, ko: "제 차가 고속 도로에서 고장이 났습니다.", en: "My car broke down on the highway.", source: "Day 003 순한맛", meaning: meanings["break down_1"] },
        { week: 1, ko: "근무시간 관련 협상이 결렬된 것은 불가피했습니다.", en: "It was inevitable that the negotiation over working hours broke down.", source: "Day 003 순한맛", meaning: meanings["break down_2"] },
        { week: 1, ko: "의사 선생님이 제가 다시는 프로 선수로 뛸 수 없다고 했을 때 저는 무너졌습니다.", en: "When the doctor said I could never play professionally again, I broke down.", source: "Day 003 순한맛", meaning: meanings["break down_3"] },
        { week: 1, ko: "스케줄을 세부적으로 말씀드릴게요.", en: "Let me break down the schedule.", source: "Day 003 순한맛", meaning: meanings["break down_4"] },
        { week: 1, ko: "사실 Susie가 헤어지자고 해서 헤어진 거야.", en: "She broke up with me, actually.", source: "Day 004 순한맛", meaning: meanings["break up_1"] },
        { week: 1, ko: "3명씩 조를 나누어라.", en: "I need you to break up into groups of three.", source: "Day 004 순한맛", meaning: meanings["break up_2"] },
        { week: 1, ko: "네 말이 끊겨서 들려.", en: "You are breaking up.", source: "Day 004 순한맛", meaning: meanings["break up_3"] },
        { week: 1, ko: "지난 학기에 배운 내용을 다시 한번 복습해 보겠습니다.", en: "I’d like us to brush up on what we learned last semester.", source: "Day 005 순한맛", meaning: meanings["brush up on_1"] },
        
        // Week 2
        { week: 2, ko: "이 시계 자세히 봐봐.", en: "Check out this watch.", source: "Day 010 순한맛", meaning: meanings["check out_4"] },
        { week: 2, ko: "불과 몇 분 만에 수업 듣는 학생 대부분이 딴생각을 하기 시작했다.", en: "It only took a few minutes before most of the class had mentally checked out.", source: "Day 010 순한맛", meaning: meanings["check out_2"] },
        { week: 2, ko: "Samantha는 저랑 헤어지기 한참 전부터 이미 마음이 떠났다고 하더라고요.", en: "Samantha said she had already checked out of the relationship long before she broke up with me.", source: "Day 010 순한맛", meaning: meanings["check out_3"] },
        
        // Week 3
        { week: 3, ko: "걸어서 집에 오다가 자두나무를 봤어요.", en: "I came across a plum tree while walking home.", source: "Day 011 순한맛", meaning: meanings["come across_1"] },
        { week: 3, ko: "혹시 파티에 따라가도 될까?", en: "Do you mind if I come along with you to the party?", source: "Day 012 순한맛", meaning: meanings["come along_1"] },
        { week: 3, ko: "공부는 잘되어 가니?", en: "How are your studies coming along?", source: "Day 012 순한맛", meaning: meanings["come along_2"] },
        { week: 3, ko: "당신이 나타날 때까지 나는 방황했어.", en: "I was lost until you came along.", source: "Day 012 순한맛", meaning: meanings["come along_3"] },
        { week: 3, ko: "의식이 돌아왔을 때 괜찮았는데, 그 후 20분 정도 지나니 마취가 완전히 풀리더군요.", en: "I felt okay when I came around, but the anesthesia fully wore off about 20 minutes after that.", source: "Day 013 순한맛", meaning: meanings["come around_1"] },
        { week: 3, ko: "여름이 될 때마다 다시 태어난 기분이에요.", en: "Every time summer comes around, I feel like a brand-new person.", source: "Day 013 순한맛", meaning: meanings["come around_2"] },
        { week: 3, ko: "이번에도 틀림없이 풀릴 거야.", en: "I’m sure he’ll come around this time, too.", source: "Day 013 순한맛", meaning: meanings["come around_3"] },
        { week: 3, ko: "먼 친척들이 올 때면 집이 좀 북적이고 분주한 느낌이 든답니다.", en: "Whenever our distant relatives come around, the house always feels a bit more crowded and hectic.", source: "Day 013 순한맛", meaning: meanings["come around_4"] },
        { week: 3, ko: "그 병에 붙은 라벨은 물에 들어가면 쉽게 벗겨진다.", en: "The labels on the bottles come off so easily in water.", source: "Day 014 순한맛", meaning: meanings["come off_1"] },
        { week: 3, ko: "아들에게 절벽 가장자리로 너무 가까이 가지 말라고 경고했다, 혹시 떨어질 수도 있어서.", en: "I warned my son not to get too close to the cliff’s edge, or else he might fall off.", source: "Day 014 순한맛", meaning: meanings["fall off_1"] },
        { week: 3, ko: "충돌 사고로 우측 사이드 미러가 부서져 떨어져 나갔다.", en: "The right side-view mirror broke off the car in the collision.", source: "Day 014 순한맛", meaning: meanings["break off_1"] },
        { week: 3, ko: "거친 외모와는 달리 그는 실제로는 온화하고 따뜻한 사람처럼 보였다.", en: "Despite his rough looks, he actually came off as gentle and caring.", source: "Day 015 순한맛", meaning: meanings["come off as_1"] },
        { week: 3, ko: "면접에서는 자신감 있고 똑똑하다는 인상을 주는 것이 중요하다.", en: "In interviews, it’s important to come across as confident and knowledgeable.", source: "Day 015 순한맛", meaning: meanings["come across as_1"] },

        // Week 4
        { week: 4, ko: "나는 항상 샤워를 할 때 좋은 생각이 떠온다.", en: "I always come up with good ideas in the shower.", source: "Day 016 순한맛", meaning: meanings["come up with_1"] },
        { week: 4, ko: "이것이 생각나는 최선의 번역입니다.", en: "This is the best translation I could come up with.", source: "Day 016 순한맛", meaning: meanings["come up with_2"] },
        { week: 4, ko: "내가 이야기를 하고 있는데 사람들이 말을 끊으면 정말 짜증난다.", en: "My pet peeve is when people cut me off when I’m in the middle of a story.", source: "Day 017 순한맛", meaning: meanings["cut off_1"] },
        { week: 4, ko: "나 인스타 접속이 막혔어.", en: "I’ve just been cut off, is all.", source: "Day 017 순한맛", meaning: meanings["cut off_2"] },
        { week: 4, ko: "제 남편은 도로에서 차들이 끼어드는 걸 너무 싫어합니다.", en: "My husband hates when people cut him off in the middle of traffic.", source: "Day 017 순한맛", meaning: meanings["cut off_3"] },
        { week: 4, ko: "최근에 왜 연락도 없고 담을 쌓고 지내는 거야?", en: "Why have you been cutting yourself off from us lately?", source: "Day 017 순한맛", meaning: meanings["cut off_4"] },
        { week: 4, ko: "페이지 설정이 잘못된 듯하네, 각 줄마다 마지막 단어가 다 잘렸어.", en: "It seems like the page formatting was off, because the last word in every line got cut off.", source: "Day 017 순한맛", meaning: meanings["cut off_5"] },
        { week: 4, ko: "드디어 루빅큐브 맞추는 방법 알아냈다.", en: "I finally figured out how to solve the Rubik’s Cube.", source: "Day 018 순한맛", meaning: meanings["figure out_1"] },
        { week: 4, ko: "새로 들인 복사기 작동법을 도무지 모르겠어.", en: "I can’t seem to figure out how to work this new photocopier.", source: "Day 018 순한맛", meaning: meanings["figure out_2"] },
        { week: 4, ko: "어디서 나는 소리인지 알 수가 없네.", en: "I can’t figure out where it’s coming from.", source: "Day 018 순한맛", meaning: meanings["figure out_3"] },
        { week: 4, ko: "뭘 먹을지 고민되시나요?", en: "Can't figure out what to eat?", source: "Day 019 순한맛", meaning: meanings["figure out_4"] },
        { week: 4, ko: "아직 그녀가 어떤 사람인지를 모르겠습니다.", en: "I can’t figure her out.", source: "Day 019 순한맛", meaning: meanings["figure out_5"] },
        { week: 4, ko: "40대가 되어서야 그 사실을 깨달았네요.", en: "It just took me until my forties to figure it out.", source: "Day 019 순한맛", meaning: meanings["figure out_6"] },
        { week: 4, ko: "사람들이 아이폰에 충성심이 높은 이유를 잘 모르겠어.", en: "I can’t quite figure out why people are so loyal to iPhones.", source: "Day 019 순한맛", meaning: meanings["figure out_7"] },
        { week: 4, ko: "거짓말하는 걸 알게 되면, 너희 관계가 끝날지도 몰라.", en: "If he figures it out, it could ruin your relationship.", source: "Day 019 순한맛", meaning: meanings["figure out_8"] },
        { week: 4, ko: "내일 제 일 좀 맡아 줄 수 있을까요?", en: "Could you fill in for me tomorrow?", source: "Day 020 순한맛", meaning: meanings["fill in for_1"] }
    ];

    // 2. 구동사 매운맛 (Week 1 ~ Week 4 통합)
    const phrasalHard = [
        // Week 1 
        { week: 1, ko: "별것 아니게 보일 수 있어도, 하루 10분의 연습도 쌓이면 정말 큽니다.", en: "It might not seem like much, but 10 minutes of practice every day really adds up.", source: "Day001 교재1", meaning: meanings["add up_2"]},
        { week: 1, ko: "어제 집에 있었다고 했는데, 내 친구가 당신을 술집에서 봤다고 했어. 뭔가 앞뒤가 안 맞잖아.", en: "You told me you were at home, but my friend mentioned seeing you at a bar. Something doesn’t add up.", source: "Day001 교재1", meaning: meanings["add up_3"]},
        { week: 1, ko: "자, 이 숫자들을 더해 보자. 5 더하기 3은 뭘까?", en: "Let’s add up these numbers now. What’s five plus three?", source: "Day001 교재1", meaning: meanings["add up_1"]},
        { week: 1, ko: "월 10만 원도 쌓이면 4년 후에 거의 5백만 원이 된다.", en: "Just 100,000 won a month will add up to almost 5 million won in four years.", source: "Day001 교재1", meaning: meanings["add up_2"]},
        { week: 1, ko: "그의 이야기에는 앞뒤가 맞지 않는 것이 있어요. 그가 거짓말을 하고 있는 게 틀림없어요.", en: "There’s something about his story that doesn’t add up. He must be telling a lie.", source: "Day001 교재1", meaning: meanings["add up_3"]},
        { week: 1, ko: "한 달에 커피값으로 백 달러 정도를 지출해. 나의 길티 플레저거든.", en: "I spend around a hundred bucks a month on coffee. It’s my guilty pleasure.", source: "Day001 교재2", meaning: meanings["add up_2"]},
        { week: 1, ko: "한 달에 백 달러도 5년이면 6천 달러야. 집에서 만들어 먹는 게 어때?", en: "A hundred bucks a month will add up to $6,000 in five years. Why don’t you make coffee at home?", source: "Day001 교재2", meaning: meanings["add up_2"]},
        { week: 1, ko: "지난 분기 매출 수치를 합해서 목표치와 비교해 주시겠어요?", en: "Can you add up the sales figures from last quarter and compare them to our targets?", source: "Day001 교재2", meaning: meanings["add up_1"]},
        { week: 1, ko: "물론입니다. 계산해서 오늘 오후까지 세부 보고서를 준비해 두겠습니다.", en: "Sure thing. I’ll do the math and have a detailed report ready by this afternoon.", source: "Day001 교재2", meaning: meanings["add up_1"]},
        { week: 1, ko: "Brian은 너무 완벽해. 우리 다음 달에 오프라인에서 만날 거야.", en: "Brian is so perfect. We’re going to meet offline next month.", source: "Day001 교재2", meaning: meanings["add up_3"]},
        { week: 1, ko: "그러니까 의사이자 파일럿이고, 외모는 모델 같은데, 아직 사진을 안 보여 줬다? 이건 말이 안 돼.", en: "So he’s a doctor, a pilot, and he looks like a model, yet he hasn’t shown you a photo? This doesn’t add up, Gina.", source: "Day001 교재2", meaning: meanings["add up_3"]},
        { week: 1, ko: "이게 쌓이면 정말 큽니다.", en: "it really adds up.", source: "Day001 교재3", meaning: meanings["add up_2"]},
        { week: 1, ko: "어젯밤에 눈이 많이 와서 미끄러우면 어쩌나 했지만, 바람이 워낙 강해서 눈이 다 날아가 버렸더군.", en: "I was worried that sidewalks would be slippery, but the wind was so strong that it blew the snow away.", source: "Day002 교재1", meaning: meanings["blow away_1"]},
        { week: 1, ko: "그냥 압축공기를 이용해서 먼지를 날려 버린답니다.", en: "I just use compressed air to blow the dust away.", source: "Day002 교재1", meaning: meanings["blow away_1"]},
        { week: 1, ko: "조심해! 자칫 날아갈 수도 있어!", en: "Be careful! You might get blown away!", source: "Day002 교재1", meaning: meanings["blow away_1"]},
        { week: 1, ko: "이번에 새로 나온 태블릿을 처음 봤을 때 매우 인상적이었어.", en: "When I first saw their new tablet, I was blown away.", source: "Day002 교재1", meaning: meanings["blow away_2"]},
        { week: 1, ko: "우와, 프레젠테이션 정말 유익했어요. 정말 놀랐어요!", en: "Wow, your presentation was so informative. You really blew me away!", source: "Day002 교재1", meaning: meanings["blow away_2"]},
        { week: 1, ko: "바람이 많이 부는 날씨는 불쾌하지만, 적어도 미세 먼지를 날려 버리긴 하지.", en: "Windy weather is unpleasant, but at least it blows away all the microdust.", source: "Day002 교재1", meaning: meanings["blow away_1"]},
        { week: 1, ko: "모자가 강풍에 날아가 버렸다.", en: "My hat blew away in the strong wind.", source: "Day002 교재1", meaning: meanings["blow away_1"]},
        { week: 1, ko: "제 동생이 영어를 너무 잘해서 정말 놀랐어요.", en: "I was really blown away by his English.", source: "Day002 교재1", meaning: meanings["blow away_2"]},
        { week: 1, ko: "바람이 너무 세서 자동차도 날아갔나 보더라고.", en: "Apparently, the wind was so strong that it even blew away cars.", source: "Day002 교재2", meaning: meanings["blow away_1"]},
        { week: 1, ko: "주연 배우의 연기가 정말 인상적이더라고.", en: "I was absolutely blown away by the main actor’s performance.", source: "Day002 교재2", meaning: meanings["blow away_2"]},
        { week: 1, ko: "김종국이 외국인들과 대화하는 거 봤는데 정말 대단하더라.", en: "I was completely blown away when I saw him chatting with foreigners.", source: "Day002 교재2", meaning: meanings["blow away_2"]},
        { week: 1, ko: "메뉴 볼 때마다 깜짝 놀라. 만 오천 원 이하가 없다니까.", en: "I’m blown away every time I look at a menu. You can’t eat for less than 15,000 won these days.", source: "Day002 교재3", meaning: meanings["blow away_2"]},
        { week: 1, ko: "몇 년을 매일 썼더니 컴퓨터가 결국 고장이 났다.", en: "The computer finally broke down after using it daily for years.", source: "Day003 교재1", meaning: meanings["break down_1"]},
        { week: 1, ko: "근무시간 관련 협상이 결렬된 것은 불가피했습니다.", en: "It was inevitable that the negotiation over working hours broke down.", source: "Day003 교재1", meaning: meanings["break down_2"]},
        { week: 1, ko: "의사 선생님이 제가 다시는 프로 선수로 뛸 수 없다고 했을 때 저는 무너졌습니다.", en: "When the doctor said I could never play professionally again, I broke down.", source: "Day003 교재1", meaning: meanings["break down_3"]},
        { week: 1, ko: "스케줄을 세부적으로 말씀드릴게요.", en: "Let me break down the schedule.", source: "Day003 교재1", meaning: meanings["break down_4"]},
        { week: 1, ko: "수치가 잘 이해되지 않네요. 미안한데 다시 한번 자세히 설명해 주시겠어요?", en: "Would you mind going back and breaking those down?", source: "Day003 교재1", meaning: meanings["break down_4"]},
        { week: 1, ko: "담배꽁초가 분해되는 데 18개월에서 10년이 걸리는 거 알았어?", en: "Did you know that cigarette butts take between 18 months and 10 years to break down?", source: "Day003 교재1", meaning: meanings["break down_2"]},
        { week: 1, ko: "제가 고속 도로에서 차가 고장 났습니다.", en: "My car broke down on the highway.", source: "Day003 교재1", meaning: meanings["break down_1"]},
        { week: 1, ko: "항상 돈 이야기가 나오면 이런 대화가 깨집니다.", en: "but those talks always break down once money comes up.", source: "Day003 교재1", meaning: meanings["break down_2"]},
        { week: 1, ko: "회사에서 계속 압박에 시달린 후에 결국 감정적으로 무너졌고 사무실에서 울었어요.", en: "After weeks of constant pressure at work, I finally broke down and cried in my office.", source: "Day003 교재1", meaning: meanings["break down_3"]},
        { week: 1, ko: "문장을 좀 더 잘 이해하기 위해서, 여러 단위로 나눠서 표현 하나하나를 살펴봤습니다.", en: "In order to understand the sentence better, I broke it down into parts and looked at the phrases one by one.", source: "Day003 교재1", meaning: meanings["break down_4"]},
        { week: 1, ko: "동물성 단백질은 분해되는 데 더 많은 에너지를 필요로 한다.", en: "Protein from meat requires more energy for your body to break down.", source: "Day003 교재1", meaning: meanings["break down_2"]},
        { week: 1, ko: "사용하던 컴퓨터가 결국 고장이 났거든요.", en: "My old computer finally broke down.", source: "Day003 교재2", meaning: meanings["break down_1"]},
        { week: 1, ko: "정신적으로 완전히 무너져서 아무 말도 안 나왔어.", en: "I just broke down completely and couldn’t even get words out.", source: "Day003 교재2", meaning: meanings["break down_3"]},
        { week: 1, ko: "아파트 청약 제도를 잘 모르겠습니다. 자세히 좀 설명해 주시겠어요?", en: "I still don’t understand how apartment lotteries work. Could you break it down for me?", source: "Day003 교재2", meaning: meanings["break down_4"]},
        { week: 1, ko: "버려진 후에 분해되는 데 정말 오래 걸려.", en: "They take forever to break down once they’re thrown away.", source: "Day003 교재3", meaning: meanings["break down_2"]},
        { week: 1, ko: "너 Susie랑 헤어졌다는 게 사실이야?", en: "Is it true you broke up with Susie?", source: "Day004 교재1", meaning: meanings["break up_1"]},
        { week: 1, ko: "사실 Susie가 헤어지자고 해서 헤어진 거야.", en: "She broke up with me, actually.", source: "Day004 교재1", meaning: meanings["break up_1"]},
        { week: 1, ko: "구름이 서서히 걷히고 해가 더 밝아졌다.", en: "The clouds gradually broke up and the sun got brighter.", source: "Day004 교재1", meaning: meanings["break up_2"]},
        { week: 1, ko: "닭고기를 한 입 크기로 찢은 다음, 샐러드에 넣어 섞으세요.", en: "First, use two forks to break up the chicken into bite-size pieces.", source: "Day004 교재1", meaning: meanings["break up_2"]},
        { week: 1, ko: "여기 (지하라서) 신호가 끊겨.", en: "My signal is breaking up down here.", source: "Day004 교재1", meaning: meanings["break up_3"]},
        { week: 1, ko: "사람들은 매일 같이 헤어지잖아. 너무 힘들게 받아들이지 마!", en: "People break up every day. Don’t take it so hard!", source: "Day004 교재1", meaning: meanings["break up_1"]},
        { week: 1, ko: "자, 얘들아. 3명씩 조를 나누어라.", en: "OK, class. I need you to break up into groups of three.", source: "Day004 교재1", meaning: meanings["break up_2"]},
        { week: 1, ko: "오노 요코 때문에 비틀즈가 해체됐다고 한다.", en: "My dad says Yoko Ono broke up The Beatles.", source: "Day004 교재1", meaning: meanings["break up_1"]},
        { week: 1, ko: "저는 하루 일과를 다양한 종류의 업무로 쪼갭니다.", en: "I like to break up my day with various kinds of tasks.", source: "Day004 교재1", meaning: meanings["break up_2"]},
        { week: 1, ko: "네 말이 끊겨서 들려.", en: "You are breaking up.", source: "Day004 교재1", meaning: meanings["break up_3"]},
        { week: 1, ko: "아무도 안 볼 텐데. 좀 더 짧은 클립으로 쪼개야 해.", en: "No one is gonna watch that. You need to break it up into smaller clips.", source: "Day004 교재2", meaning: meanings["break up_2"]},
        { week: 1, ko: "잠시만, 잘 안 들려. 신호가 계속 끊기네.", en: "Hold on, I can’t hear you. Your signal keeps breaking up.", source: "Day004 교재2", meaning: meanings["break up_3"]},
        { week: 1, ko: "그러니까 밴드가 해산하는 건 미나 잘못이야.", en: "so it’s her fault the band is breaking up.", source: "Day004 교재2", meaning: meanings["break up_1"]},
        { week: 1, ko: "하지만 급기야 둘 간의 관계가 흔들렸고 최근에 헤어지게 되었다.", en: "However, their relationship eventually got rocky, and they recently broke up.", source: "Day004 교재3", meaning: meanings["break up_1"]},
        { week: 1, ko: "“민호랑 헤어진 마당에 서울에 계속 있어야 할 이유가 없어.”", en: "She told me, “Now that Minho and I broke up, I don’t have any reason to stay in Seoul.”", source: "Day004 교재3", meaning: meanings["break up_1"]},
        
        // Week 2 
        { week: 2, ko: "다음 달에 파리로 여행 가는데, 프랑스어 복습 좀 해야겠어.", en: "I am travelling to Paris next month, so I think I need to brush up on my French.", source: "Day 005 교재1", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "지난 수업에서 배운 내용을 복습해 보겠습니다.", en: "I’d like us to review what we learned last class.", source: "Day 005 교재1", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "지난 학기에 배운 내용을 다시 한번 복습해 보겠습니다.", en: "I’d like us to brush up on what we learned last semester.", source: "Day 005 교재1", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "나 케이크 만들어야 해. 빵 굽는 것을 연습하려면 요리책 읽어 봐야겠다.", en: "I’ve got to make a cake. I’m going to read a cookbook so that I can brush up on my baking skills.", source: "Day 005 교재1", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "이 책은 ‘한 번 읽고 끝내는’ 책이 아닙니다. 잊어버릴 만하면 (이 책에 담긴) 구동사를 복습하셔야 실제 상황에서 자신 있게 쓸 수 있습니다.", en: "This book is not a “one-and-done” read. You should brush up on these phrasal verbs from time to time in order to feel confident using them in real-life situations.", source: "Day 005 교재1", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "A: 은퇴 생활은 어떠신가요? 무료하신가요?", en: "A: So, how’s retired life treating you? Are you bored yet?", source: "Day 005 교재1", meaning: null },
        { week: 2, ko: "B: 전혀요. 한동안 안 치던 기타도 연습하고 있어요. 내가 기타 연주를 얼마나 좋아했는지를 잊고 있었어요.", en: "B: Not at all. I’ve been brushing up on my guitar playing. I forgot how much I loved playing.", source: "Day 005 교재1", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "내일 경복궁 가는 거 정말 기대돼.", en: "I’m excited to visit Gyeongbokgung Palace tomorrow.", source: "Day 005 교재2", meaning: null },
        { week: 2, ko: "나도. 가기 전에 한국 역사 복습 좀 해야겠어. 그래야 경복궁에 가서 구경하게 될 것을 좀 더 잘 감상할 수 있을 테니.", en: "Me, too. I want to brush up on my Korean history before we go, so I can better appreciate what I’m seeing while we’re there.", source: "Day 005 교재2", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "Janet, 내일 회의 때 제안서 발표하는 거죠?", en: "So, Janet, you’re going to present our proposal at the meeting tomorrow, right?", source: "Day 005 교재2", meaning: null },
        { week: 2, ko: "맞아요. 이번 주는 집에서 거울 보고 연습하면서 발표 스킬을 좀 가다듬고 있어요.", en: "Right. I’ve been brushing up on my presentation skills this week by practicing in the mirror at home.", source: "Day 005 교재2", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "친구들한테 제가 만든 자장면이 정말 맛있다고 자랑을 했더니, 이번 주 토요일에 점심 먹으러 오겠대요. 그래서 요리 연습을 좀 해야 해요. (자장면) 안 만들어 본 지 10년도 넘었거든요.", en: "I was bragging about how good my homemade jajangmyeon is to my friends, and they invited themselves over for lunch this Saturday. I need to brush up on my cooking skills, because I haven’t made it in over 10 years.", source: "Day 005 교재2", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "고등학교 때 만난 여자 친구와 10년을 사귀었는데 최근에 헤어졌다.", en: "After dating my high school girlfriend for 10 years, we recently broke up.", source: "Day 005 교재3", meaning: meanings["break up_1"] },
        { week: 2, ko: "“지도 있으세요? 당신의 눈에서 길을 잃어서요.”와 같은 (소개팅에서 쓸 수 있는) 작업 멘트를 연습해 보는 것이 내가 생각할 수 있는 전부였다.", en: "All I could think to do was brush up on some pickup lines like “Do you have a map? Because I just got lost in your eyes.” I hope it works.", source: "Day 005 교재3", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "커피나 차 좀 드시겠어요?", en: "Would you care for some coffee or tea?", source: "Day 006 교재1", meaning: meanings["care for_1"] },
        { week: 2, ko: "죄송하지만 Jones 상무님은 아직 회의 중이시며, 마치려면 좀 걸릴 수 있습니다. 기다리시는 동안 마실 것 좀 가져다 드릴까요?", en: "I’m afraid Mr. Jones is still in a meeting, and it may take a while before he’s available. Would you care for something to drink while you wait?", source: "Day 006 교재1", meaning: meanings["care for_1"] },
        { week: 2, ko: "저는 고수를 별로 좋아하지 않습니다.", en: "I don’t really care for cilantro.", source: "Day 006 교재1", meaning: meanings["care for_1"] },
        { week: 2, ko: "우리 아빠 매운 음식 안 좋아하시는 거 알잖아.", en: "You know my dad doesn’t care much for spicy food.", source: "Day 006 교재1", meaning: meanings["care for_1"] },
        { week: 2, ko: "제가 휴가 가는 동안 우리 집 식물 좀 돌봐 주실 수 있으세요?", en: "Could you care for my plants while I’m on vacation?", source: "Day 006 교재1", meaning: meanings["care for_2"] },
        { week: 2, ko: "Sally, 너 정신 건강을 좀 챙겨야겠어.", en: "You need to care for your mental health, Sally.", source: "Day 006 교재1", meaning: meanings["care for_2"] },
        { week: 2, ko: "쿠션 드릴까요? 그 의자 조금 불편할 거예요.", en: "Would you care for a seat cushion? I know that chair can be a little uncomfortable.", source: "Day 006 교재1", meaning: meanings["care for_1"] },
        { week: 2, ko: "저녁 먹고 산책 가실래요?", en: "Would you care for a walk after dinner?", source: "Day 006 교재1", meaning: meanings["care for_1"] },
        { week: 2, ko: "괜찮아요. 전 단 것을 별로 안 좋아해서요.", en: "No thanks. I don’t care much for sweets.", source: "Day 006 교재1", meaning: meanings["care for_1"] },
        { week: 2, ko: "저는 무서운 영화를 별로 좋아하지 않아요.", en: "I don’t really care for scary movies.", source: "Day 006 교재1", meaning: meanings["care for_1"] },
        { week: 2, ko: "최근에 연로하신 부모님을 돌보기 시작했는데, 저를 키우느라 얼마나 많은 희생을 하셨는지 생각하니 정말 감사한 마음이 들었답니다.", en: "I recently started caring for my elderly parents, and it’s made me really grateful for how much they sacrificed to raise me.", source: "Day 006 교재1", meaning: meanings["care for_2"] },
        { week: 2, ko: "사실, 저 친구(화장실에 간 친구)는 해산물 별로 안 좋아해요.", en: "Actually, he doesn’t care for seafood.", source: "Day 006 교재2", meaning: meanings["care for_1"] },
        { week: 2, ko: "식물 기르는 거 보기보다 쉬워. 흙이 마르면 물만 주면 되거든.", en: "Caring for plants is easier than it looks, actually. All you have to do is water them when the soil is dry.", source: "Day 006 교재2", meaning: meanings["care for_2"] },
        { week: 2, ko: "게다가 Sam이랑 Frank는 남부 출신인데, 남부 사람들은 뉴요커를 별로 안 좋아해.", en: "Besides, Sam and Frank are from the South, and Southerners don’t care for New Yorkers...", source: "Day 006 교재2", meaning: meanings["care for_1"] },
        { week: 2, ko: "보살핌을 받는다는 느낌을 받을 수 없었습니다.", en: "I didn’t really feel cared for.", source: "Day 006 교재3", meaning: meanings["care for_2"] },
        { week: 2, ko: "헐렁한 셔츠와 바지가 다시 유행할 줄은 몰랐다.", en: "I didn’t expect baggy shirts and pants to catch on again.", source: "Day 007 교재1", meaning: meanings["catch on_1"] },
        { week: 2, ko: "최근 들어 (돈 문제로) 남편이랑 다투고 있다. 하지만 아이들이 우리 돈 문제를 눈치 못 채도록 노력하고 있다.", en: "My husband and I have been fighting recently, but I’m trying not to let my children catch on to our money problems.", source: "Day 007 교재1", meaning: meanings["catch on_2"] },
        { week: 2, ko: "우리 회사는 트렌드에 많이 뒤처져 있습니다. 재택근무의 수많은 장점을 아직 이해하지 못하고 있어요.", en: "My company is so behind the times. They still haven’t caught on to all the benefits that come with working from home.", source: "Day 007 교재1", meaning: meanings["catch on_2"] },
        { week: 2, ko: "스포츠 도박이 북미에서 선풍적인 인기를 끌고 있습니다.", en: "Sports betting is really catching on in North America.", source: "Day 007 교재1", meaning: meanings["catch on_1"] },
        { week: 2, ko: "Dave의 셔츠 뒷면에 ‘나는 바보다’라고 적혀 있었다. 그는 선생님이 그게 뭐냐고 물을 때까지 눈치를 못 챘다.", en: "Dave had “I’m an idiot” written on the back of his shirt. He didn’t catch on until the teacher asked him about it.", source: "Day 007 교재1", meaning: meanings["catch on_2"] },
        { week: 2, ko: "너희들 왜 계속 웃는 거야? 나는 너희들 유머가 전혀 이해가 안 되는데….", en: "Why do you guys keep laughing? I can never catch on to your jokes…", source: "Day 007 교재1", meaning: meanings["catch on_2"] },
        { week: 2, ko: "제 고객은 새로운 운동을 가르쳐 줄 때 이해하는 속도가 더딥니다.", en: "My client is slow to catch on when I teach him new exercises.", source: "Day 007 교재1", meaning: meanings["catch on_2"] },
        { week: 2, ko: "그랬더니? 정장 입고 수업에 온 거야, 아니면 그 전에 (장난으로 한 말이라는 걸) 눈치챈 거야?", en: "And what happened? Did he come to class dressed up, or did he catch on before that?", source: "Day 007 교재2", meaning: meanings["catch on_2"] },
        { week: 2, ko: "내 남편은 눈치가 너무 없어. 생일 선물 갖고 싶은 게 있으면 남편 쇼핑 리스트에 적어 줘야 한다니까.", en: "My husband doesn’t catch on to my hints.", source: "Day 007 교재2", meaning: meanings["catch on_2"] },
        { week: 2, ko: "보통 신입 서빙 직원을 교육하는 데 최대 일주일 정도 걸리는데, 당신은 상당히 빨리 익히는군요.", en: "Normally, it can take up to a week to train new servers, but you’re catching on pretty quick.", source: "Day 007 교재2", meaning: meanings["catch on_2"] },
        { week: 2, ko: "2~3년 전부터 전기차가 인기를 끌기 시작했습니다.", en: "Electric vehicles started to catch on a couple of years ago.", source: "Day 007 교재3", meaning: meanings["catch on_1"] },
        { week: 2, ko: "주말을 이용해 밀린 집안일을 해야겠다. 빨래가 계속 쌓이고 있다.", en: "I’ll use the weekend to catch up on household chores; the laundry has been piling up.", source: "Day 008 교재1", meaning: meanings["catch up on_1"] },
        { week: 2, ko: "아직이요. 점심 식사 후에 처리할게요. 오전에 일이 너무 많네요.", en: "Not yet. I’ll catch up on them after lunch; I’ve been swamped all morning.", source: "Day 008 교재1", meaning: meanings["catch up on_1"] },
        { week: 2, ko: "다음 주 언제 만나서 점심이나 먹으면 좋은데. 그동안 밀린 이야기가 정말 많잖아.", en: "Maybe we could meet up for lunch sometime next week. We have a lot to catch up on.", source: "Day 008 교재1", meaning: meanings["catch up on_2"] },
        { week: 2, ko: "이번 주말엔 밀린 잠을 좀 자야겠어. 요즘 계속 너무 피곤해.", en: "I’m going to catch up on sleep this weekend; I’ve been feeling exhausted lately.", source: "Day 008 교재1", meaning: meanings["catch up on_1"] },
        { week: 2, ko: "인스타그램을 하면 최신 패션 트렌드를 파악하는 데 도움이 된다.", en: "Instagram helps me to catch up on the latest fashion trends.", source: "Day 008 교재1", meaning: meanings["catch up on_2"] },
        { week: 2, ko: "읽어야 할 것들이 많이 밀렸어. 만회하는 데만 몇 주는 걸릴 거야.", en: "I am really behind on the reading. It will take me weeks just to catch up.", source: "Day 008 교재1", meaning: meanings["catch up on_1"] },
        { week: 2, ko: "난 그 정도로 힘든 건 없었지. 그냥 밀린 이메일 처리했지.", en: "Nothing as stressful as that; I just caught up on e-mails.", source: "Day 008 교재2", meaning: meanings["catch up on_1"] },
        { week: 2, ko: "음, 토요일에는 그동안 못 쉰 거 몰아서 쉴까 했어.", en: "Well, I was really hoping to catch up on some much-needed rest on Saturday.", source: "Day 008 교재2", meaning: meanings["catch up on_1"] },
        { week: 2, ko: "밀린 강좌 들어야 해. 최근에 좀 해이해졌거든.", en: "I need to catch up on them; I’ve been slacking off lately.", source: "Day 008 교재2", meaning: meanings["catch up on_1"] },
        { week: 2, ko: "첫 월급 받은 것으로 밀린 공과금도 내고 드디어 재정 상태도 다시 안정을 찾아서 기분이 정말 좋았다.", en: "I felt really good catching up on bills with my first paycheck...", source: "Day 008 교재3", meaning: meanings["catch up on_1"] },
        { week: 2, ko: "(할머니 댁에 가서) 할머니 안부는 확인해 본 거야? 고관절 골절이신데 집에 혼자 계시는 게 걱정이 되네.", en: "Have you checked in on Grandma?", source: "Day 009 교재1", meaning: meanings["check on_1"] },
        { week: 2, ko: "식당에서 서빙을 하게 되면 담당 테이블의 손님에게 필요한 것이 있는지 확인해 보아야 한다.", en: "If you are a server at a restaurant, you need to check on diners at your tables to see if they need anything.", source: "Day 009 교재1", meaning: meanings["check on_1"] },
        { week: 2, ko: "오븐에 닭고기 익히고 있는데, (잘 익고 있는지) 확인 좀 해 줄 수 있어?", en: "Could you check on the chicken I’m cooking in the oven?", source: "Day 009 교재1", meaning: meanings["check on_1"] },
        { week: 2, ko: "제 아들이 몸 상태가 안 좋아서 내일 가서 어떤지 확인하고, 식료품이나 약이 필요한지 알아봐야겠습니다.", en: "Since my son has been feeling under the weather, I’m going to check in on him tomorrow...", source: "Day 009 교재1", meaning: meanings["check on_1"] },
        { week: 2, ko: "지난번에 얼굴 본 뒤로 시간이 제법 흘렀네. (자네 사무실에 들러서) 어떻게 지내는지, 그리고 새 직장은 어떤지 한번 볼까 싶어서 연락했어.", en: "It’s been a while since we last spoke, so I thought I’d check in on you and see how you’re doing with the new job.", source: "Day 009 교재1", meaning: meanings["check on_1"] },
        { week: 2, ko: "왜 거기 그냥 서 있는 거예요? 손님에게 음료나 뭐 필요한 게 있는지 어서 테이블을 확인해 보세요.", en: "Why are you just standing there? Go check on your tables and see if they need drinks or anything.", source: "Day 009 교재1", meaning: meanings["check on_1"] },
        { week: 2, ko: "제 상사는 하루에 열두 번씩 (메시지, 통화 등으로) 저를 통제하는 데 이 때문에 미치겠습니다.", en: "My boss checks on me like twelve times a day and it drives me crazy.", source: "Day 009 교재1", meaning: meanings["check on_1"] },
        { week: 2, ko: "며칠 전에 장염 걸렸다고 했잖아요. 그래서 잘 회복하고 있는지 물어보고 싶었어요.", en: "You mentioned that you caught a stomach bug a few days ago, so I wanted to check in and ask how you’re holding up.", source: "Day 009 교재2", meaning: meanings["check on_1"] },
        { week: 2, ko: "챙겨 주셔서 고마워요! 괜찮아요. 아직 침대에 누워 있지만 좋아지고 있어요.", en: "Thanks for checking in on me! I’ve been okay. I’m still stuck in bed but starting to get better.", source: "Day 009 교재2", meaning: meanings["check on_1"] },
        { week: 2, ko: "출발하기 전에 식물들을 확인해야 해. 곧 물을 줘야 할 거야.", en: "I need to check on the plants before we leave. I think they’ll be needing water soon.", source: "Day 009 교재2", meaning: meanings["check on_1"] },
        { week: 2, ko: "303호에 새로 들어온 환자 상태 확인했나요? 차트 보니까 이 환자 주기적인 모니터링이 필요하다고 적혀 있군요.", en: "Have you checked on the new patient in room 303?", source: "Day 009 교재2", meaning: meanings["check on_1"] },
        { week: 2, ko: "동료애를 발휘해서 이들의 안부를 확인하는 것만으로도 큰 변화가 생깁니다.", en: "Giving simple companionship by checking in on them makes a huge difference.", source: "Day 009 교재3", meaning: meanings["check on_1"] },
        { week: 2, ko: "제가 사는 곳의 엘리베이터는 아주 느려요. 제가 15층에 삽니다. 엘리베이터가 내려가는 시간을 이용해서 거울로 외모를 확인하고 보풀이나 고양이 털을 뗍니다.", en: "I use the time coming down to check out my appearance in the mirror and pull off any lint or cat fur.", source: "Day 010 교재1", meaning: meanings["check out_1"] },
        { week: 2, ko: "이 그림 속에 있는 작은 디테일들을 꼼꼼히 봐봐. 이 화가의 독특한 붓 터치가 보일거야.", en: "Check out the tiny details in this painting. You can see the artist’s individual brushstrokes.", source: "Day 010 교재1", meaning: meanings["check out_1"] },
        { week: 2, ko: "〈오징어 게임〉과 〈기생충〉과 같은 한국 미디어에 등장한 유명한 곳을 방문해 보고싶습니다.", en: "We want to check out some of the famous locations from Korean media, like Squid Game and Parasite.", source: "Day 010 교재1", meaning: meanings["check out_1"] },
        { week: 2, ko: "와, 저 여자 옷 좀 봐! 저렇게 입고 이 날씨를 어떻게 견디지?", en: "Wow, check out what she’s wearing!", source: "Day 010 교재1", meaning: meanings["check out_1"] },
        { week: 2, ko: "나랑 손잡고 있으면서 저 여자를 훑어본 거야?", en: "Were you just checking her out while holding my hand?", source: "Day 010 교재1", meaning: meanings["check out_1"] },
        { week: 2, ko: "잠시만. 안에 들어가서 먹을 만한 것 있는지 메뉴 좀 보고 올게.", en: "Hold on. I am gonna go in and check out their menu to see if they have anything good.", source: "Day 010 교재1", meaning: meanings["check out_1"] },
        { week: 2, ko: "용리단길에는 정말 가볼 만한 멋진 곳이 많아.", en: "Yongnidangil has so many cool places to check out.", source: "Day 010 교재1", meaning: meanings["check out_1"] },
        { week: 2, ko: "영업시간과 특별 메뉴와 관련된 최신 정보는 저희 인스타그램을 방문해서 확인해 주시기 바랍니다.", en: "Please check out our Instagram for the latest information on hours and special dishes.", source: "Day 010 교재1", meaning: meanings["check out_1"] },
        { week: 2, ko: "이 시계 자세히 봐봐. 진품이야, 아니면 짝퉁이야?", en: "Check out this watch. Is it genuine or a knock-off?", source: "Day 010 교재1", meaning: meanings["check out_4"] },
        { week: 2, ko: "강연이 너무 지루하고 어려웠다. 불과 몇 분 만에 수업 듣는 학생 대부분이 딴생각을 하기 시작했다.", en: "The lecture was so tedious and complicated. It only took a few minutes before most of the class had mentally checked out.", source: "Day 010 교재1", meaning: meanings["check out_2"] },
        { week: 2, ko: "Samantha는 저랑 헤어지기 한참 전부터 이미 마음이 떠났다고 하더라고요.", en: "Samantha said she had already checked out of the relationship long before she broke up with me.", source: "Day 010 교재1", meaning: meanings["check out_3"] },
        { week: 2, ko: "네, 정말 괜찮은 것 같네요. 그런데 제가 스피커에 대해 아는 게 거의 없어서, 다른 브랜드도 살펴보고 결정해야 할 것 같습니다.", en: "Yeah, that sounds like a real bargain, but I know almost nothing about speakers, so I think I need to check out other brands before I make a decision.", source: "Day 010 교재2", meaning: meanings["check out_1"] },
        { week: 2, ko: "고객들이 저희 매장에서 옷을 입어만 보고는 온라인에서 구매를 하네요.", en: "Customers just use the store to check out clothes before buying them online.", source: "Day 010 교재2", meaning: meanings["check out_1"] },
        { week: 2, ko: "보니까 Jackson이 회의에 집중을 안 하는 듯하네. 이메일 답장한다고 정신이 없는 듯.", en: "It looks like Jackson has checked out of our meeting. Maybe he’s caught up in some e-mails.", source: "Day 010 교재2", meaning: meanings["check out_2"] },
        { week: 2, ko: "어쩌면 내가 이미 아주 오랫동안 감정이 떠난 상태인 걸 수도 있다. 하지만 이런 이야기를 어떻게 꺼내야 할지 모르겠다.", en: "Maybe I’ve been emotionally checked out for too long, but I don’t know how to bring this up with him.", source: "Day 010 교재3", meaning: meanings["check out_3"] },
        
        // Week 3
        { week: 3, ko: "걸어서 집에 오다가 자두나무를 봤어요.", en: "I came across a plum tree while walking home.", source: "Day 011 교재1", meaning: meanings["come across_1"] },
        { week: 3, ko: "부모님 댁에 있는 옷장 정리를 하다가 우연히 고등학교 때 찍은 이 사진을 발견했어.", en: "I came across this photo from high school while I was cleaning out a closet at my parents’ place.", source: "Day 011 교재1", meaning: meanings["come across_1"] },
        { week: 3, ko: "명동 시장을 걷고 있는데 이 점집을 발견했어. 점을 봐 주는 데 만 오천 원밖에 안 받아. 그래서 ‘그럼 한번 봐 보지 뭐!’라고 생각했어.", en: "I came across this fortune teller while walking through the Myeongdong Market. He only charges 15,000 won to predict your future, so I thought, “Why not?”", source: "Day 011 교재1", meaning: meanings["come across_1"] },
        { week: 3, ko: "네이버에서 실내 암벽 등반 센터의 무료 수업 광고를 봤어. 한번 알아볼까?", en: "I came across this ad for a free session at a climbing gym on Naver. Why don’t we go check it out?", source: "Day 011 교재1", meaning: meanings["come across_1"] },
        { week: 3, ko: "오늘 아침에 너희들이 늦잠을 자는 동안 산책하러 나갔다가 바닷가에서 작고 예쁜 카페를 발견했어. 내 휴대폰으로 한번 봐 봐.", en: "While you were sleeping in this morning, I went off on a walk and came across this cute little beachside café. Take a look on my phone.", source: "Day 011 교재1", meaning: meanings["come across_1"] },
        { week: 3, ko: "우와! 리뷰가 500개가 넘고 평점이 4.8점이네! 떠나기 전에 한번 가봐야겠다.", en: "Oh wow! Over 500 reviews, and the average score is 4.8! We’ll have to try it out before we leave.", source: "Day 011 교재1", meaning: null },
        { week: 3, ko: "북촌에 있는 어느 골목길을 걷고 있다가 문신하는 분을 발견했어. 문신이 상당히 독특하더라고. 그래서 그 자리에서 바로 문신을 해 버렸어.", en: "I was walking down an alley in Bukchon, and I came across a tattoo artist. Her work was so unique, I ended up getting a tattoo on the spot.", source: "Day 011 교재1", meaning: meanings["come across_1"] },
        { week: 3, ko: "여기 오는 길에 역 근처에서 작은 쿠키 가게를 발견했어. 가게가 너무 귀여워서, 쿠키를 안 사 올 수가 없더라고.", en: "On my way here, I came across a little cookie shop near the station. It looked so cute, and I had to pick some up for us.", source: "Day 011 교재2", meaning: meanings["come across_1"] },
        { week: 3, ko: "아, 나 거기 어딘지 알 것 같아. 문 위에 크고 화려한 간판 있었지?", en: "Oh, I think I know the place you’re talking about. Did they have a big, colorful sign over the door?", source: "Day 011 교재2", meaning: null },
        { week: 3, ko: "집에 둘 만한 것은 좀 찾은 거야?", en: "Have you come across anything that you want to get for the house?", source: "Day 011 교재2", meaning: meanings["come across_1"] },
        { week: 3, ko: "아직 눈에 띄는 게 없네. 근데 아직 안 본 것들이 많아. 괜찮은 의자나 침대 테이블을 열심히 찾고 있어.", en: "Nothing has really caught my eye yet. There’s so much we haven’t seen yet, though. I’m keeping an eye out for a nice chair or bedside table.", source: "Day 011 교재2", meaning: null },
        { week: 3, ko: "안녕하세요. 어제 우연히 이 청소 세제를 발견했는데, 욕실 타일이 잘 닦이네요. 이미 알고 있었어요?", en: "Hey, I came across this cleaner yesterday, and it works really well on my bathroom tiles. Did you already know about it?", source: "Day 011 교재2", meaning: meanings["come across_1"] },
        { week: 3, ko: "아, 네. 그 제품 쓴 지 오래됐어요. 어디에 가나 파는데, 처음 써 본다고 하시니 의외군요.", en: "Oh, yeah. I’ve been using that for ages. You can get it pretty much anywhere, so I’m surprised you hadn’t tried it before.", source: "Day 011 교재2", meaning: null },
        { week: 3, ko: "지난주 서점에서 이 아동용 도서를 우연히 발견했고, 표지가 화려해서 아주 마음에 들었습니다.", en: "I came across this children’s book at the bookstore last week, and I really liked its colorful cover.", source: "Day 011 교재3", meaning: meanings["come across_1"] },
        { week: 3, ko: "우리 아이들이 읽으면 재미있을 이야기겠다 싶어서 사가지고 왔습니다.", en: "I thought it would be a fun story for my kids(4 & 7 years old), so I decided to bring it home.", source: "Day 011 교재3", meaning: null },
        { week: 3, ko: "하지만 아이들이 읽기에는 너무 무서운 이야기라는 걸 금방 알게 되었습니다.", en: "However, we quickly found out that the story was too scary for them.", source: "Day 011 교재3", meaning: null },
        { week: 3, ko: "쿠키 감옥이라는 주제는 생각보다 훨씬 어두웠고, 이 때문에 아이들이 며칠 밤을 잠을 못 잤습니다.", en: "The themes of a cookie prison were darker than I expected, and it kept my kids awake for several nights.", source: "Day 011 교재3", meaning: null },
        { week: 3, ko: "삽화는 정말 멋졌지만, 내용은 저희 아이들에게 적합하지 않았습니다.", en: "While the illustrations were beautiful, the content just wasn’t suitable for my little guys.", source: "Day 011 교재3", meaning: null },
        { week: 3, ko: "가볍고 재미있는 이야기를 찾는 어린 자녀를 둔 부모님에게는 추천하지 않습니다.", en: "I wouldn’t recommend it to parents with young children looking for a light and enjoyable story.", source: "Day 011 교재3", meaning: null },
        { week: 3, ko: "나랑 같이 피크닉 갈 생각 있는지 궁금하네.", en: "I was wondering if you would like to come along to the picnic.", source: "Day 012 교재1", meaning: meanings["come along_1"] },
        { week: 3, ko: "공부는 잘되어 가니?", en: "How are your studies coming along?", source: "Day 012 교재1", meaning: meanings["come along_2"] },
        { week: 3, ko: "책 쓰는 건 잘 진행되고 있나요?", en: "How is your book coming along?", source: "Day 012 교재1", meaning: meanings["come along_2"] },
        { week: 3, ko: "당신이 나타날 때까지 나는 방황했어.", en: "I was lost until you came along.", source: "Day 012 교재1", meaning: meanings["come along_3"] },
        { week: 3, ko: "내가 그 직장에 합격하지 못하다니 믿기지가 않아. 딱 나한테 맞는 일 같았는데.", en: "I can’t believe I didn’t get that job. It seemed perfect for me.", source: "Day 012 교재1", meaning: null },
        { week: 3, ko: "걱정 마. 조만간 더 좋은 기회가 생길 거야.", en: "Don’t worry. I’m sure a better opportunity will come along before you know it.", source: "Day 012 교재1", meaning: meanings["come along_3"] },
        { week: 3, ko: "혹시 파티에 따라가도 될까?", en: "Do you mind if I come along with you to the party?", source: "Day 012 교재1", meaning: meanings["come along_1"] },
        { week: 3, ko: "일은 잘 진행되고 있어? 지난주에 작업하던 프로젝트는 다 마쳤고?", en: "How’s your work coming along? Did you finish that project you were working on last week?", source: "Day 012 교재1", meaning: meanings["come along_2"] },
        { week: 3, ko: "응, 거의. 근데 아직 해결해야 할 버그가 많아. 그래서 지금 수정하고 있어.", en: "Yeah, pretty much, but there are a lot of bugs to work out, so I’m trying to fix them now.", source: "Day 012 교재1", meaning: null },
        { week: 3, ko: "안녕, Samantha. 프로젝트가 잘 되고 있기를 바랍니다. 바쁜 건 알지만, 가능하면 오늘 저녁 우리 집에서 같이 저녁 먹고 싶어요.", en: "Hello, Samantha. I hope your project is coming along well. I know you’re busy, but if you can make it, we’d love to have you over for dinner tonight at our place.", source: "Day 012 교재1", meaning: meanings["come along_2"] },
        { week: 3, ko: "둘째 아들이 태어나자 급여가 좀 더 괜찮은 직장을 찾아야 했습니다.", en: "I had to finally find a better-paying job when my second son came along.", source: "Day 012 교재1", meaning: meanings["come along_3"] },
        { week: 3, ko: "거의 포기하고 걷기 시작했는데 그때 낯선 분이 나타나서는 타이어 교체하는 걸 도와주겠다고 했다.", en: "I almost gave up and started walking, but then, a stranger came along and offered to help change the tire.", source: "Day 012 교재1", meaning: meanings["come along_3"] },
        { week: 3, ko: "마지막 학기는 좀 어때?", en: "How is your last semester coming along?", source: "Day 012 교재2", meaning: meanings["come along_2"] },
        { week: 3, ko: "기말고사와 구직 활동으로 좀 스트레스를 받긴 하지만, 긍정적으로 생활하려고 노력 중이야.", en: "It’s a bit stressful with finals and job hunting, but I’m trying to stay optimistic.", source: "Day 012 교재2", meaning: null },
        { week: 3, ko: "Nick이 나를 찼다는 게 믿기지가 않아. 우린 서로 너무 잘 어울린다고 생각했기 때문에 정말 예상 못 했어.", en: "I can’t believe Nick broke up with me. I didn’t see it coming because we seemed so perfect for each other.", source: "Day 012 교재2", meaning: null },
        { week: 3, ko: "걱정 마. 너를 진정 아껴 주는 더 나은 사람이 나타날 거야.", en: "Don’t worry. Someone better will come along who truly appreciates you.", source: "Day 012 교재2", meaning: meanings["come along_3"] },
        { week: 3, ko: "어떻게 해야 할지 막막해. 아무도 내게 기회를 주지 않을 거야. 난 이제 취업은 글렀어.", en: "I don’t know what else to do. No one will give me a chance, and I’m out of employment options.", source: "Day 012 교재2", meaning: null },
        { week: 3, ko: "내 말 들어 봐. 네가 할 일은 일자리를 찾는 거야. 그러니까 자책 좀 그만해. 스스로가 준비가 되어 있어야 기회가 오면 잡을 수 있게 된단 말이야.", en: "Listen to me, your job is to find a job, so stop feeling so sorry for yourself. You need to stay ready so that when an opportunity comes along, you’ll be prepared to grab it.", source: "Day 012 교재2", meaning: meanings["come along_3"] },
        { week: 3, ko: "소선 씨, 영어 공부는 잘되어 가나요? 지금쯤이면 원어민이 다 되었겠어요.", en: "Hey, Sosun, how are your English studies coming along? I bet you’re practically a native speaker by now.", source: "Day 012 교재3", meaning: meanings["come along_2"] },
        { week: 3, ko: "교수님, 안녕하세요! 사실 주위 사람들이 전부 한국말을 해서 외국에 사는 것 같지가 않아요.", en: "Hi, Professor! Actually, everyone around me speaks Korean, so I don’t even feel like I’m living abroad.", source: "Day 012 교재3", meaning: null },
        { week: 3, ko: "오, 무슨 말인지 알겠어요. 휴스턴에는 한인 사회가 꽤 크게 형성되어 있다고 들었어요. 그래도 새로운 환경을 즐기고는 있죠?", en: "Oh, I understand. I heard Houston has a big Korean community. Are you at least enjoying the change of scenery?", source: "Day 012 교재3", meaning: null },
        { week: 3, ko: "그렇게 많이 나가지도 않아요. 그냥 공부 좀 하고 여기서 만난 한국인 몇 명이랑 어울리는 게 전부예요. 돈과 시간을 버리고 있는 기분이에요. 이러려고 여기 온 게 아닌데.", en: "I don’t get out much. I just study and then hang out with some of the Korean people I’ve met out here. I kind of feel like I’m wasting my money and time. This isn’t how I expected it to be.", source: "Day 012 교재3", meaning: null },
        { week: 3, ko: "좀 더 적극적으로 현지인 친구를 사귀어 보는 게 좋을 듯한데. 소선 씨는 재미있는 사람이니 어렵지 않을 거예요.", en: "Maybe you should put yourself out there more to make some local friends. That shouldn’t be hard for you. You’re a fun person.", source: "Day 012 교재3", meaning: null },
        { week: 3, ko: "고맙습니다, 교수님. 그 말씀이 필요했어요. 노력해 볼게요!", en: "Thanks, Professor. I needed to hear that. I’ll try!", source: "Day 012 교재3", meaning: null },
        { week: 3, ko: "버스가 코너에 너무 바짝 붙어서 돌거든, 그러니까 (도로에서) 좀 물러서 있는 게 가장 좋아.", en: "The bus comes around the corner sharply, so it’s best to stand back a bit.", source: "Day 013 교재1", meaning: meanings["come around_4"] },
        { week: 3, ko: "내 남자 친구는 화가 금방 풀리는 스타일이야. 이번에도 틀림없이 풀릴 거야.", en: "My boyfriend never stays mad at me for long. I’m sure he’ll come around this time, too.", source: "Day 013 교재1", meaning: meanings["come around_3"] },
        { week: 3, ko: "Sarah랑 John이 지난주에 헤어졌잖아. Sarah가 오늘 여기 올 거라고 생각해?", en: "Hey, Sarah and John broke up last week. Do you think she’s going to show up here?", source: "Day 013 교재1", meaning: null },
        { week: 3, ko: "잘 모르겠어. 근데 여기 나타나면 아주 난장판이 되겠지.", en: "I don’t know, but if she comes around there is definitely going to be drama.", source: "Day 013 교재1", meaning: meanings["come around_4"] },
        { week: 3, ko: "의식이 돌아왔을 때 괜찮았는데, 그 후 20분 정도 지나니 마취가 완전히 풀리더군요.", en: "I felt okay when I came around, but the anesthesia fully wore off about 20 minutes after that.", source: "Day 013 교재1", meaning: meanings["come around_1"] },
        { week: 3, ko: "여름이 될 때마다 다시 태어난 기분이에요.", en: "Every time summer comes around, I feel like a brand-new person.", source: "Day 013 교재1", meaning: meanings["come around_2"] },
        { week: 3, ko: "(매일 저녁) 7시가 되면 술이 확 당깁니다.", en: "When 7 o’clock comes around, I get the urge to drink.", source: "Day 013 교재1", meaning: meanings["come around_2"] },
        { week: 3, ko: "네 상사가 사실 너를 싫어하는 건 아닐 거라 확신해. 출근만 제시간에 하면 마음이 풀릴 거야.", en: "I’m sure your boss doesn’t actually hate you. If you just get to the office on time, I’m sure he’ll come around.", source: "Day 013 교재1", meaning: meanings["come around_3"] },
        { week: 3, ko: "그녀의 마음을 바꿔 볼 테니 걱정 마세요. 조금 더 나은 혜택을 제안 하면 틀림없이 계약 조건에 동의할 겁니다.", en: "We’re going to get her to come around; don’t you worry. If we offer slightly better benefits, I’m sure she’ll agree to the terms.", source: "Day 013 교재1", meaning: meanings["come around_3"] },
        { week: 3, ko: "먼 친척들이 올 때면 집이 좀 북적이고 분주한 느낌이 든답니다.", en: "Whenever our distant relatives come around, the house always feels a bit more crowded and hectic.", source: "Day 013 교재1", meaning: meanings["come around_4"] },
        { week: 3, ko: "서울 생활에 전반적으로 만족해. 그런데 하나 적응이 안 되는 게 있어. 겨울만 되면 서울 생활이 후회가 돼. 너무 춥잖아.", en: "I am happy living in Seoul, overall. There is one thing I can’t get used to, though. When winter comes around, I often wish I didn’t live here. It’s freezing.", source: "Day 013 교재2", meaning: meanings["come around_2"] },
        { week: 3, ko: "정말? 너 헝가리 출신 아니야? 난 부다페스트가 훨씬 더 추운 줄 알았는데.", en: "Seriously? You are from Hungary, aren’t you? I thought Budapest was much colder than Seoul.", source: "Day 013 교재2", meaning: null },
        { week: 3, ko: "여자 친구한테 사과를 했는데도 여전히 화가 안 풀린 듯해. 생각했던 것보다 마음이 더 상했나 봐.", en: "I apologized to her, but she still seems distant. I guess I really hurt her feelings more than I realized.", source: "Day 013 교재2", meaning: null },
        { week: 3, ko: "그냥 시간을 좀 줘. 틀림없이 풀릴 테니까. 용서하기 전에 그냥 좀 내버려둬야 하는 경우가 많잖아.", en: "Just give her some time. I’m sure she will come around. People often need space before they can forgive.", source: "Day 013 교재2", meaning: meanings["come around_3"] },
        { week: 3, ko: "너무 기대돼! 집들이를 빨리 했으면 좋겠다. Sally도 초대했다고 이야기했었나?", en: "I’m so excited! My housewarming party just can’t come soon enough. Did I mention I also invited Sally?", source: "Day 013 교재2", meaning: null },
        { week: 3, ko: "안 돼! 너 진짜로 Sally를 초대한 거야? Sally가 오면 Jane이 굉장히 화낼 텐데.", en: "Oh no! You seriously invited her? If she comes around, Jane will definitely be upset.", source: "Day 013 교재2", meaning: meanings["come around_4"] },
        { week: 3, ko: "내가 한 말과 너에게 상처 준 점 진심으로 미안해.", en: "I’m truly sorry for what I said and for hurting you.", source: "Day 013 교재3", meaning: null },
        { week: 3, ko: "네 험담을 한 점 인정해.", en: "I admit that I’ve been talking behind your back.", source: "Day 013 교재3", meaning: null },
        { week: 3, ko: "그렇지만 나에게 우리 우정은 정말 중요해. 심한 말 한 점 후회하고 있어.", en: "Our friendship means a lot to me, though, and I regret saying those awful things.", source: "Day 013 교재3", meaning: null },
        { week: 3, ko: "네가 화가 난 것 이해해. 하지만 용서해 주면 좋겠어.", en: "I understand you’re upset, but I hope you can forgive me.", source: "Day 013 교재3", meaning: null },
        { week: 3, ko: "네가 다시 나를 믿어 줄 수만 있다면 뭐든 할 생각이야.", en: "I’m willing to do whatever it takes to get your trust back.", source: "Day 013 교재3", meaning: null },
        { week: 3, ko: "진심으로 마음 풀기를 바란다.", en: "I really hope you’ll come around.", source: "Day 013 교재3", meaning: meanings["come around_3"] },
        { week: 3, ko: "벽에 붙은 페인트가 벗겨지기 시작하네. 결국 다시 칠해야 할 것 같아.", en: "The paint on the walls is starting to come off. We’ll have to repaint them eventually.", source: "Day 014 교재1", meaning: meanings["come off_1"] },
        { week: 3, ko: "내 책상 의자 다리 하나가 빠졌어. 드라이버를 찾아서 고쳐야 해.", en: "One of the legs of my desk chair came off and I need to find a screwdriver to repair it.", source: "Day 014 교재1", meaning: meanings["come off_1"] },
        { week: 3, ko: "그가 선반에 책을 너무 많이 올려놓는 바람에 책 하나가 떨어져 버렸다.", en: "He put too many books on the shelf, so one fell off.", source: "Day 014 교재1", meaning: meanings["fall off_1"] },
        { week: 3, ko: "나무에서 나뭇잎이 떨어지기 시작하네. 가을은 내가 가장 좋아하는 계절이지.", en: "The leaves are beginning to fall off the trees. Fall is my favorite season.", source: "Day 014 교재1", meaning: meanings["fall off_1"] },
        { week: 3, ko: "강력한 천둥 번개로 인해 큰 나뭇가지가 부러져 (나무에서) 떨어졌고 우리 지붕이 파손됐다.", en: "During an intense thunderstorm, a heavy branch broke off and damaged our roof.", source: "Day 014 교재1", meaning: meanings["break off_1"] },
        { week: 3, ko: "나는 내 베이컨 조각을 떼서 우리 집 강아지에게 먹이는 걸 즐긴다.", en: "I like to break off pieces of my bacon and feed them to my dog.", source: "Day 014 교재1", meaning: meanings["break off_1"] },
        { week: 3, ko: "네 신발이 벗겨지고 있어. 끈을 다시 묶어.", en: "Your shoe is coming off. You should retie your laces.", source: "Day 014 교재1", meaning: meanings["come off_1"] },
        { week: 3, ko: "그 병에 붙은 라벨은 물에 들어가면 쉽게 벗겨진다.", en: "The labels on the bottles come off so easily in water.", source: "Day 014 교재1", meaning: meanings["come off_1"] },
        { week: 3, ko: "아들에게 절벽 가장자리로 너무 가까이 가지 말라고 경고했다. 혹시 떨어질 수도 있어서.", en: "I warned my son not to get too close to the cliff’s edge, or else he might fall off.", source: "Day 014 교재1", meaning: meanings["fall off_1"] },
        { week: 3, ko: "내가 살이 빠져서 결혼반지가 너무 크다. 계속 손가락에서 빠져서 떨어진다. 잃어버리기 전에 크기를 조정해야겠다.", en: "My wedding ring is too big now that I’ve lost weight. It falls off my finger all the time. I need to get it resized before I lose it.", source: "Day 014 교재1", meaning: meanings["fall off_1"] },
        { week: 3, ko: "충돌 사고로 우측 사이드 미러가 부서져 (차에서) 떨어져 나갔다.", en: "The right side-view mirror broke off the car in the collision.", source: "Day 014 교재1", meaning: meanings["break off_1"] },
        { week: 3, ko: "자물쇠 비번을 잊어버려서 자전거 자물쇠를 큰 망치로 부숴 버렸다.", en: "I broke off the combination lock to my bike with a sledge hammer because I couldn’t remember my combination.", source: "Day 014 교재1", meaning: meanings["break off_1"] },
        { week: 3, ko: "Suzy와 Pete가 약혼을 깨 버렸는데 아무도 이유는 모른다.", en: "Suzy and Pete broke off their engagement, and no one knows why.", source: "Day 014 교재1", meaning: meanings["break off_1"] },
        { week: 3, ko: "안녕, Daniel, 주말은 어땠어?", en: "Hey, Daniel, how was your weekend?", source: "Day 014 교재2", meaning: null },
        { week: 3, ko: "나쁘지 않았어. 근데 토요일에 새 가방을 하나 샀는데, 매장에서 무료로 내 이니셜을 새겨 주겠다고 하더라고. 당연히 좋다고 했지. (그런데) 일요일쯤 이니셜이 떨어져 나가 버렸어! 싼 게 비지떡인 것 같아.", en: "Not bad, but Saturday, I bought a new bag, and the store offered to have it customized with my initials for free. Of course, I took them up on it. By Sunday, my initials had already come off! I guess you get what you pay for.", source: "Day 014 교재2", meaning: meanings["come off_1"] },
        { week: 3, ko: "진짜 이 집에서 이사 나가야 해. 욕실 문손잡이가 또 부서져서 떨어져 나갔어.", en: "We really need to move out of this house. The door knob to the bathroom broke off again.", source: "Day 014 교재2", meaning: meanings["break off_1"] },
        { week: 3, ko: "새 집으로 이사 갈 형편이 안 돼. 손잡이를 고쳐 쓰는 게 훨씬 돈이 적게 들잖아.", en: "We can’t afford a new house. Fixing a door knob is way cheaper.", source: "Day 014 교재2", meaning: null },
        { week: 3, ko: "Brandon, 볼 때마다 선글라스가 바뀌는구나.", en: "Brandon, you have a new pair of sunglasses on every time I see you.", source: "Day 014 교재2", meaning: null },
        { week: 3, ko: "알아. 근데 다 저렴한 거야. 호수에서 세 번이나 선글라스를 잃어버리고는 비싼 건 안 사. 보트에 타고 있는 동안 머리 위에 선글라스를 끼고 있다는 걸 깜박하거든. 그러다 보면 너무 쉽게 떨어져 버려.", en: "I know. They’re all cheap, though. I stopped buying expensive sunglasses after I lost my third pair in the lake. When I’m out on the boat, I forget I have them on top of my head, and they fall off so easily.", source: "Day 014 교재2", meaning: meanings["fall off_1"] },
        { week: 3, ko: "요즘은 연애가 참 쉽지 않다.", en: "Dating these days is so tricky.", source: "Day 014 교재3", meaning: null },
        { week: 3, ko: "작년에 소개팅을 50번이나 했다.", en: "Last year, I went on fifty blind dates.", source: "Day 014 교재3", meaning: null },
        { week: 3, ko: "아르바이트하는 느낌이었지만, 정말 좋은 사람을 만나서 정착하고 싶었다.", en: "It felt like a part-time job, but I was really serious about meeting the right person for me and settling down.", source: "Day 014 교재3", meaning: null },
        { week: 3, ko: "그나마 거의 사귈 뻔했던 사람이 Mina라는 여성이었다.", en: "The closest I got was with this girl named Mina.", source: "Day 014 교재3", meaning: null },
        { week: 3, ko: "어렵게 다섯 번을 만나고 난 뒤에 Mina는 나와의 관계를 정리해 버렸다.", en: "We made it to the fifth date before she broke it off with me.", source: "Day 014 교재3", meaning: meanings["break off_1"] },
        { week: 3, ko: "자신의 어머니를 돌봐 드려야 한다는 게 변명이었다.", en: "Her only explanation was that she needed to take care of her mother.", source: "Day 014 교재3", meaning: null },
        { week: 3, ko: "내가 저녁 식사로 스테이크를 사 주기 전에 나와의 관계를 정리해 버렸으면 좋았을 텐데.", en: "I wish she would’ve broken it off before I paid for that steak dinner.", source: "Day 014 교재3", meaning: meanings["break off_1"] },
        { week: 3, ko: "거친 외모와는 달리 그는 실제로는 온화하고 따뜻한 사람처럼 보였다.", en: "Despite his rough looks, he actually came off as gentle and caring.", source: "Day 015 교재1", meaning: meanings["come off as_1"] },
        { week: 3, ko: "회의 때 제가 너무 심하게 보이지 않았기를 바랍니다. 기분을 상하게 할 뜻은 없었어요.", en: "I hope I didn’t come off as too harsh during the meeting; I didn’t mean to upset anyone.", source: "Day 015 교재1", meaning: meanings["come off as_1"] },
        { week: 3, ko: "가끔씩 그가 던지는 농담이 상대방 기분을 배려하지 않는다는 느낌이 들어. 물론 악의가 없다는 건 알지만 말이야.", en: "Sometimes, his jokes can come off as insensitive, even though I know he doesn’t mean any harm.", source: "Day 015 교재1", meaning: meanings["come off as_1"] },
        { week: 3, ko: "나 그 배우 좋아하는데, 그 영화에서는 잘난 척하는 머저리 같더라.", en: "I like the actor, but he came off as a condescending jerk in the movie.", source: "Day 015 교재1", meaning: meanings["come off as_1"] },
        { week: 3, ko: "A: 회의 때 나 긴장돼 보였어?", en: "A: Did I come across as nervous during the meeting?", source: "Day 015 교재1", meaning: meanings["come across as_1"] },
        { week: 3, ko: "B: 전혀! 자신감 있고 침착해 보이던데.", en: "B: Not at all! You seemed confident and composed.", source: "Day 015 교재1", meaning: null },
        { week: 3, ko: "면접에서는 자신감 있고 똑똑하다는 인상을 주는 것이 중요하다.", en: "In interviews, it’s important to come across as confident and knowledgeable.", source: "Day 015 교재1", meaning: meanings["come across as_1"] },
        { week: 3, ko: "너한테는 Hailey가 거만해 보이니?", en: "Does Hailey really come off as arrogant to you?", source: "Day 015 교재2", meaning: meanings["come off as_1"] },
        { week: 3, ko: "나한테는 안 그래. 근데 다른 사람 눈에는 그 친구 자신감이 거만하다고 오해를 살 수 있어.", en: "Not to me, but sometimes her confidence can be mistaken for arrogance to others.", source: "Day 015 교재2", meaning: null },
        { week: 3, ko: "이메일에서 제가 너무 무례하게 느껴지진 않았나 모르겠네요.", en: "I hope I didn’t come off as rude in the e-mail.", source: "Day 015 교재2", meaning: meanings["come off as_1"] },
        { week: 3, ko: "아니요, 분명하고 프로다웠습니다. 걱정 마세요.", en: "No, you were clear and professional. Don’t worry about it.", source: "Day 015 교재2", meaning: null },
        { week: 3, ko: "우와! 민수야, 너 이제 영어 정말 잘하는구나. 원어민 같은 느낌이야. 무슨 일이 있었던 거야?", en: "Wow! Minsu, your English is so good now. You come across like a native speaker. What happened?", source: "Day 015 교재2", meaning: meanings["come across as_1"] },
        { week: 3, ko: "호주에 있을 때 호주 친구들이 다들 느긋하고 편했거든. 그 덕분에 좀 더 수월하게 영어를 배울 수 있었어.", en: "During my time in Australia, nearly all my Aussie friends were so laid-back and easygoing. That made it easier for me to learn English with them.", source: "Day 015 교재2", meaning: null },
        { week: 3, ko: "나는 취업 면접을 볼 때 안 좋은 인상을 줄까 봐 늘 걱정했다.", en: "I always used to worry about making a negative impression during job interviews.", source: "Day 015 교재3", meaning: null },
        { week: 3, ko: "내가 하는 답에 대해 너무 많이 생각하는 편이었다. 준비가 안 되었다거나 확신이 없다는 인상을 줄까봐 겁이 났기 때문이다.", en: "I tended to overthink my answers, fearing I might appear unprepared or unsure.", source: "Day 015 교재3", meaning: null },
        { week: 3, ko: "하지만 최근 면접에서는 나의 본능을 믿고 솔직히 답하기로 결심했다.", en: "However, during my latest interview, I decided to trust my instincts and answer questions honestly.", source: "Day 015 교재3", meaning: null },
        { week: 3, ko: "놀랍게도 나중에 팀장님한테 들은 이야기인데 내가 자신감이 넘치고 능력 있게 보였다는 것이다.", en: "To my surprise, I later found out from my manager that I came off as confident and capable.", source: "Day 015 교재3", meaning: meanings["come off as_1"] },
        { week: 3, ko: "면접관들이 내 자신감에 깊은 인상을 받은 것이었다.", en: "They were impressed with my confidence.", source: "Day 015 교재3", meaning: null },
        { week: 3, ko: "이런 경험을 통해 진솔한 것이 좋은 인상을 주는 열쇠라는 점을 깨달았다.", en: "This experience taught me that being genuine was the key to making a positive impression.", source: "Day 015 교재3", meaning: null },

        // Week 4 추가 분량
        { week: 4, ko: "나는 항상 샤워를 할 때 좋은 생각이 떠오른다.", en: "I always come up with good ideas in the shower.", source: "Day 016 교재1", meaning: meanings["come up with_1"] },
        { week: 4, ko: "해당 구동사 관련해서 당장 생각나는 예문은 이게 다입니다. 나중에 더 생각나면 알려 드릴게요.", en: "I’m afraid those are all the examples I can come up with on the spot, using this phrasal verb. If I think of any more later, I’ll let you know.", source: "Day 016 교재1", meaning: meanings["come up with_1"] },
        { week: 4, ko: "있는 재료로 이걸 만들었다는 말이야? 대단하다!", en: "You mean you just came up with this with ingredients you had on hand? That’s amazing!", source: "Day 016 교재1", meaning: meanings["come up with_1"] },
        { week: 4, ko: "어디서 그런 말도 안 되는 생각을 해낸 거니? 대체 왜 그렇게 생각하는 건데?", en: "Where did you come up with that idea? Why would you think that?", source: "Day 016 교재1", meaning: meanings["come up with_1"] },
        { week: 4, ko: "이것이 (당장, 즉석에서) 생각나는 최선의 번역입니다.", en: "This is the best translation I could come up with.", source: "Day 016 교재1", meaning: meanings["come up with_2"] },
        { week: 4, ko: "공원에서 개를 산책시키다가 제 소설에 대한 완벽한 제목이 떠올랐어요.", en: "I came up with the perfect title for my novel while walking my dog in the park.", source: "Day 016 교재1", meaning: meanings["come up with_1"] },
        { week: 4, ko: "요즘 학생들은 숙제를 안 한 것에 대한 점점 더 기발한 변명거리를 만들어 낸다.", en: "Students are coming up with more and more creative excuses for not doing their homework these days.", source: "Day 016 교재1", meaning: meanings["come up with_1"] },
        { week: 4, ko: "Sarah가 자기에게 소개해 줄 만한 괜찮은 미혼 친구가 있는지 물었는데, 마땅히 생각나는 사람이 없었다.", en: "Sarah asked me if I had any nice single friends I could set her up with, but I couldn’t come up with anyone.", source: "Day 016 교재1", meaning: meanings["come up with_1"] },
        { week: 4, ko: "돈을 마련하는 대로 갚을게.", en: "I will pay you back as soon as I come up with the money.", source: "Day 016 교재1", meaning: meanings["come up with_2"] },
        { week: 4, ko: "정부가 주택 부족을 해결하기 위해 어설픈 정책을 마련하는 바람에 상황을 엉망으로 만든 것 같다.", en: "I think the government messed up when they came up with this stopgap measure for the housing shortage.", source: "Day 016 교재1", meaning: meanings["come up with_1"] },
        { week: 4, ko: "(아드님에게) 숙제를 안 한 이유를 물으면 매번 다른 변명거리를 생각해 냅니다.", en: "When I ask him why he hasn’t done his homework, he just comes up with a different excuse each time.", source: "Day 016 교재2", meaning: meanings["come up with_1"] },
        { week: 4, ko: "저도 당황스럽습니다. 집에 오면 바로 자기 방으로 가서 문을 걸어 잠급니다. 대화를 시도해 봤지만 제 말을 들은 척도 안 하는 것 같네요.", en: "I’m at a loss, too. He locks himself in his room as soon as he gets home. I’ve tried reaching out, but he doesn’t seem to hear what I’m saying.", source: "Day 016 교재2", meaning: null },
        { week: 4, ko: "매 학기마다 자기소개하는 게 가장 힘든 부분이야. 재미있는 말이 생각이 안 나거든.", en: "Doing self-introductions is the worst part of every semester. I can never come up with anything interesting to say.", source: "Day 016 교재2", meaning: meanings["come up with_1"] },
        { week: 4, ko: "샤워를 할 때 최고의 아이디어가 떠오른다는 기사를 얼마 전에 읽었어. 근데 너 하루에 다섯 번이나 샤워하잖아. 대체 좋은 아이디어는 다 어디 간 거야?", en: "I just read an article that said you can get your best ideas in the shower. Then again, you take, like, five showers a day. Where are all your good ideas?", source: "Day 016 교재2", meaning: null },
        { week: 4, ko: "이 국 너무 맛있다. 속이 편해지네. 모퉁이에 있는 반찬가게에서 산 거야?", en: "This soup tastes good and it settles my stomach. Did you get it at the side dish shop around the corner?", source: "Day 016 교재2", meaning: null },
        { week: 4, ko: "아니, 숙취가 너무 심한 날이 있었는데, 그때 내가 직접 생각해 낸 레시피야.", en: "Nah, I actually came up with this recipe myself one day when I was really hungover.", source: "Day 016 교재2", meaning: meanings["come up with_1"] },
        { week: 4, ko: "머리가 꽉 막힌 채 참신한 해결책이 떠오르지 않을 때면, 전혀 상관없는 무언가를 하기 위해 한 발 물러서 보는 것이 많은 도움이 될 수 있습니다.", en: "When you’re stuck and can’t come up with a creative solution, stepping away to do something completely unrelated could really help.", source: "Day 016 교재3", meaning: meanings["come up with_1"] },
        { week: 4, ko: "더 나은 아이디어를 떠올리기 위해서 우리의 마음은 휴식이 필요할 때가 있습니다.", en: "Sometimes our minds need a break in order to come up with even better ideas.", source: "Day 016 교재3", meaning: meanings["come up with_1"] },
        { week: 4, ko: "따뜻한 물로 샤워를 하거나, 공원에서 산책을 하면 창의적인 생각이 흐르게 됩니다.", en: "Enjoying a hot shower or a walk in the park would be a good way to get your creative juices flowing.", source: "Day 016 교재3", meaning: null },
        { week: 4, ko: "개인적으로 저는 생각이 막힐 때면 항상 뜨거운 물로 샤워를 하거나 목욕을 합니다.", en: "Personally, when I’m stuck I always take a hot shower or bath.", source: "Day 016 교재3", meaning: null },
        { week: 4, ko: "따뜻하게 흐르는 물은 심신을 편안하게 해 주고, 그러고 나면 멋진 아이디어가 생각납니다.", en: "Something about the warm, flowing water relaxes me, and then I suddenly come up with a great idea.", source: "Day 016 교재3", meaning: meanings["come up with_1"] },
        { week: 4, ko: "얼마 전에 줌 회의용 AI 소프트웨어를 다운로드했다. 내가 상대방 말을 몇 번 끊는지를 기록해 준다.", en: "I just downloaded this AI software for my Zoom meetings. It counts how many times I cut someone off.", source: "Day 017 교재1", meaning: meanings["cut off_1"] },
        { week: 4, ko: "A: Samantha, 한동안 인스타에 아무것도 안 올렸네. 괜찮은 거야?", en: "A: Hey, Samantha, you haven’t posted anything on Instagram for a while. Are you OK?", source: "Day 017 교재1", meaning: null },
        { week: 4, ko: "B: 응. 다른 건 아니고 나 (인스타 접속이) 막혔어. 성적 올릴 때까지 엄마가 전화기 안 돌려주실 거야.", en: "B: Yeah. I’ve just been cut off, is all. My mom won’t give me my phone back until I get my grades up.", source: "Day 017 교재1", meaning: meanings["cut off_2"] },
        { week: 4, ko: "제 남편은 도로에서 차들이 끼어드는 걸 너무 싫어합니다. 그때가 유일하게 남편이 욕을 하는 때입니다.", en: "My husband hates when people cut him off in the middle of traffic. That’s the only time he ever swears.", source: "Day 017 교재1", meaning: meanings["cut off_3"] },
        { week: 4, ko: "스캔들이 확산되자 미나는 자신의 모든 SNS 계정에서 자취를 감추었다.", en: "After the scandal went viral, Mina cut herself off from all her social media accounts.", source: "Day 017 교재1", meaning: meanings["cut off_4"] },
        { week: 4, ko: "이 서류들 다시 프린트해야겠어. 페이지 설정이 잘못된 듯하네. 각 줄마다 마지막 단어가 다 잘렸어.", en: "These documents need to be reprinted. It seems like the page formatting was off, because the last word in every line got cut off.", source: "Day 017 교재1", meaning: meanings["cut off_5"] },
        { week: 4, ko: "아, 맞네. 설정을 조정하고 바로 다시 프린트할게.", en: "Ah, you’re right. I’ll adjust the formatting and print them out again right away.", source: "Day 017 교재1", meaning: null },
        { week: 4, ko: "내가 이야기를 하고 있는데 사람들이 말을 끊으면 정말 짜증난다.", en: "My pet peeve is when people cut me off when I’m in the middle of a story.", source: "Day 017 교재1", meaning: meanings["cut off_1"] },
        { week: 4, ko: "Sam이랑 마지막으로 이야기한 게 언제야?", en: "When was the last time you talked to Sam?", source: "Day 017 교재1", meaning: null },
        { week: 4, ko: "몇 달 됐지. 내 험담을 하고 다닌다는 걸 알고는 (Sam을) 차단해 버렸어.", en: "It’s been months. I cut him off when I found out he had been talking behind my back.", source: "Day 017 교재1", meaning: meanings["cut off_4"] },
        { week: 4, ko: "나 다시는 민수 차 안 탈래. 휴대폰으로 ‘포켓몬 고’ 게임을 하면서 세 명이나 끼어들더라고!", en: "I’ll never let Minsu give me a ride again. That guy cut off three people while playing Pokémon GO on his phone!", source: "Day 017 교재1", meaning: meanings["cut off_3"] },
        { week: 4, ko: "최근에 왜 연락도 없고 담을 쌓고 지내는 거야? 소식 못 들은 지 정말 오래됐어.", en: "Why have you been cutting yourself off from us lately? We haven’t heard from you in ages.", source: "Day 017 교재1", meaning: meanings["cut off_4"] },
        { week: 4, ko: "엑셀로 매출 수치를 보는데, 왼쪽 열의 주문 번호들이 잘려 나간 걸 알아차렸다.", en: "While I was looking at the sales figures on Excel, I couldn’t help but notice the order numbers in the left column were cut off.", source: "Day 017 교재1", meaning: meanings["cut off_5"] },
        { week: 4, ko: "네가 신이 나서 이야기하고 싶은 건 알지만 상대방이 이야기하고 있는데 중간에 자르는 건 예의가 아니야. 네 차례를 기다려야지.", en: "I know you’re excited and you want to talk, but cutting someone off while they’re in the middle of talking is bad manners. You need to wait your turn.", source: "Day 017 교재2", meaning: meanings["cut off_1"] },
        { week: 4, ko: "알겠어요, 엄마. 좀 더 인내심을 갖도록 해 볼게요.", en: "OK, Mom. I’ll try to be more patient.", source: "Day 017 교재2", meaning: null },
        { week: 4, ko: "이런! 저 사람이 갑자기 끼어들었어! 뭐 저런 인간이 다 있어!", en: "Oh my gosh! That guy just cut me off! How dare that jerk!", source: "Day 017 교재2", meaning: meanings["cut off_3"] },
        { week: 4, ko: "Melinda, 나 매일 너랑 차 타고 다니잖아. 대부분은 네가 끼어드는 쪽이거든!", en: "Melinda, I drive with you every day. Most of the time you’re the jerk cutting people off on the road!", source: "Day 017 교재2", meaning: meanings["cut off_3"] },
        { week: 4, ko: "전 여자 친구랑 완전히 연락을 끊거나, 아님 우리 헤어지자. 너희 둘이 아직 친구 관계라는 걸 참을 수가 없어. 너무 힘들어.", en: "Either you completely cut off your ex-girlfriend, or we’re breaking up. I just can’t handle you guys being friends anymore. It’s too much for me.", source: "Day 017 교재2", meaning: meanings["cut off_4"] },
        { week: 4, ko: "우리 타협점을 찾자. 그 친구와의 우정도 지키고 싶고, 너를 잃고 싶지도 않단 말이야.", en: "Let’s make a compromise. I don’t want to lose my friendship with her, and I don’t want to lose you, either.", source: "Day 017 교재2", meaning: null },
        { week: 4, ko: "비트코인으로 돈을 전부 잃고 난 뒤 나는 절망에 빠졌다.", en: "After losing all of my money to Bitcoin, I was hopeless.", source: "Day 017 교재3", meaning: null },
        { week: 4, ko: "가족과 친구를 볼 면목도 없고, 나 스스로를 마주할 자신도 없었다. 그래서 나는 세상과 단절했다.", en: "I couldn’t face myself, let alone my family and friends, so I cut myself off from the world.", source: "Day 017 교재3", meaning: meanings["cut off_4"] },
        { week: 4, ko: "약 6개월간 이런 상황이 지속되었다.", en: "Things went on like that for around six months.", source: "Day 017 교재3", meaning: null },
        { week: 4, ko: "그러던 어느 날 《실버 라이닝》이라는 책을 알게 되었다.", en: "One day I came across this book, Silver Lining.", source: "Day 017 교재3", meaning: meanings["come across_1"] },
        { week: 4, ko: "서점을 지나가는데, 글쎄... 그냥 이 책에 끌렸다.", en: "I was walking past a bookstore, and... I don’t know, it just called to me.", source: "Day 017 교재3", meaning: null },
        { week: 4, ko: "이 책을 읽고는 내가 빠져 있던 구멍에서(이런 악순환에서) 나올 힘이 생겼다.", en: "Anyway, after reading it, I finally found the strength to dig myself out of the hole I was in.", source: "Day 017 교재3", meaning: null },
        { week: 4, ko: "책에 이런 힘이 있을 줄은 몰랐다. 하지만 내 인생을 바꾸어 놓았다.", en: "I never thought a book would have the power to do that, but it was a huge life-changer for me.", source: "Day 017 교재3", meaning: null },
        { week: 4, ko: "서울 지하철은 너무 복잡해서 헷갈려.", en: "Seoul’s subway system is too complex to figure out.", source: "Day 018 교재1", meaning: meanings["figure out_2"] },
        { week: 4, ko: "드디어 루빅큐브 맞추는 방법 알아냈다.", en: "I finally figured out how to solve the Rubik’s Cube.", source: "Day 018 교재1", meaning: meanings["figure out_1"] },
        { week: 4, ko: "이 분수 곱하는 법을 도무지 모르겠어. 좀 도와줄래?", en: "I can’t figure out how to multiply these fractions. Can you help me?", source: "Day 018 교재1", meaning: meanings["figure out_1"] },
        { week: 4, ko: "우리 아파트에 뭔가 두드리는 소리가 계속 들려. 근데 어디서 나는 소리인지 알 수가 없네.", en: "I keep hearing a tapping sound in my apartment, and I can’t figure out where it’s coming from.", source: "Day 018 교재1", meaning: meanings["figure out_3"] },
        { week: 4, ko: "우리 엄마는 크로스워드 퍼즐을 엄청 빨리 풀 수 있어요.", en: "My mom can figure out crossword puzzles very quickly.", source: "Day 018 교재1", meaning: meanings["figure out_1"] },
        { week: 4, ko: "새로 들인 복사기 작동법을 도무지 모르겠어.", en: "I can’t seem to figure out how to work this new photocopier.", source: "Day 018 교재1", meaning: meanings["figure out_2"] },
        { week: 4, ko: "벌써 몇 주째 허리가 아픈데 원인을 모르겠어. 스트레칭을 하고 쉬어도 도움이 안 돼. 아무래도 병원에 가 봐야 할 것 같아.", en: "I’ve had this backache for weeks now and I can’t figure out what’s causing it. Stretching and rest don’t help. I think I’ll have to go see a doctor.", source: "Day 018 교재1", meaning: meanings["figure out_3"] },
        { week: 4, ko: "막힌 거야? 과제 이리 가져와 봐. 같이 풀어 보자.", en: "Are you stuck? Bring your homework over here. We can figure it out together.", source: "Day 018 교재2", meaning: meanings["figure out_1"] },
        { week: 4, ko: "고마워요, 엄마. 혼자서는 이 문제 답을 도저히 모르겠어요.", en: "Thanks, Mom. I can’t seem to get the answer to this on my own.", source: "Day 018 교재2", meaning: null },
        { week: 4, ko: "차가 계속 고장이 나는데 이유를 모르겠더라고. 정비사랑 통화하고 나서 엔진 오일을 갈아야 한다는 걸 알았지 뭐야.", en: "My car kept breaking down, and I couldn’t figure out why. After calling my mechanic, I finally found out that my car needs an oil change.", source: "Day 018 교재2", meaning: meanings["figure out_3"] },
        { week: 4, ko: "아, 그래서 그렇구나. 경험으로 보아 엔진 오일은 대충 5천에서 7천5백 마일마다 갈면 돼.", en: "Oh, that explains it. Just so you know, the general rule of thumb is to get an oil change every 5,000 to 7,500 miles.", source: "Day 018 교재2", meaning: null },
        { week: 4, ko: "도쿄 여행은 정말 엉망이었어. 지하철이 너무 복잡해서 헷갈리더라고. 그래서 공항에 늦을 뻔했어.", en: "My trip to Tokyo was a total mess. The subway system was way too complex to figure out, so I was almost late to the airport.", source: "Day 018 교재2", meaning: meanings["figure out_2"] },
        { week: 4, ko: "지난번 갔을 때 나도 똑같이 느꼈는데. 지도만 복잡한 것이 아니라 일부 표지판은 전부 일본어로만 되어 있어서 더 어렵더라고.", en: "I felt the same way on my last trip to Japan. On top of the complicated maps, some of the signs were only written in Japanese, which made it even more difficult.", source: "Day 018 교재2", meaning: null },
        { week: 4, ko: "형이 자신의 운동 루틴을 보여 줬을 때 왜 형이 몸이 좋아지지 않는지 이해가 되었다.", en: "When my brother showed me his exercise routine, I understood why he wasn’t seeing any results.", source: "Day 018 교재3", meaning: null },
        { week: 4, ko: "형은 폼롤러로 스트레칭을 좀 하고, 러닝 머신을 3분 걷고는 사우나 가서 좀 쉬다 오겠다고 했다.", en: "After he did some stretching with a foam roller and walked on a treadmill for just three minutes, he said he was gonna go relax in the sauna for a while.", source: "Day 018 교재3", meaning: null },
        { week: 4, ko: "형이 가고 나서 나는 혼자 운동을 해야만 했다.", en: "After he left, I had to exercise on my own.", source: "Day 018 교재3", meaning: null },
        { week: 4, ko: "사실 나는 기구들을 어떻게 사용하는지 잘 몰랐다.", en: "The thing is, I didn’t really know how to use any of the machines.", source: "Day 018 교재3", meaning: null },
        { week: 4, ko: "그 기구들은 모두 너무 복잡했지만 어떻게든 내 스스로 해결하려고 노력해야만 했다.", en: "They were all so complicated, but I had to try and figure things out for myself.", source: "Day 018 교재3", meaning: meanings["figure out_2"] },
        { week: 4, ko: "내가 제대로 사용하고 있었는지 아직도 100퍼센트 확신이 없다.", en: "I’m still not a hundred percent sure I was using the equipment the right way.", source: "Day 018 교재3", meaning: null },
        { week: 4, ko: "Suzy와 저 둘 다 아이를 갖고 싶긴 하지만 언제 가져야 할지가 고민입니다. Suzy는 직장을 그만둬야 하나 어쩌나 아직 결정을 못 하고 있어요.", en: "Suzy and I do want to start a family, but we need to figure out the timing. She’s still on the fence about whether she should quit her job or not.", source: "Day 019 교재1", meaning: meanings["figure out_4"] },
        { week: 4, ko: "아직 그녀가 어떤 사람인지를 모르겠습니다.", en: "I can’t figure her out.", source: "Day 019 교재1", meaning: meanings["figure out_5"] },
        { week: 4, ko: "A: 이번 주에 계속 우울하네. 그런데 이유를 모르겠어.", en: "A: I’ve been feeling down this week, but I can’t figure out why.", source: "Day 019 교재1", meaning: meanings["figure out_3"] },
        { week: 4, ko: "B: 계속 비가 와서 그런지도.", en: "B: Maybe it’s the rainy weather we’ve been having.", source: "Day 019 교재1", meaning: null },
        { week: 4, ko: "아직도 제 셔츠 사이즈를 잘 모르겠어요. 어떤 브랜드는 스몰이고 또 어떤 브랜드는 미디엄이에요.", en: "I still can’t figure out my shirt size. I’m a small in one brand, but a medium in another.", source: "Day 019 교재1", meaning: meanings["figure out_6"] },
        { week: 4, ko: "사람들이 아이폰에 충성심이 높은 이유를 잘 모르겠어.", en: "I can’t quite figure out why people are so loyal to iPhones.", source: "Day 019 교재1", meaning: meanings["figure out_7"] },
        { week: 4, ko: "눈치가 빠르시네요.", en: "You figured that out right away.", source: "Day 019 교재1", meaning: meanings["figure out_8"] },
        { week: 4, ko: "당신을 사랑하지만, 아직 결혼할 준비가 안 되었어. 아직 내 삶에 대해서도 잘 모르겠거든.", en: "I love you, but I’m not ready to get married because I haven’t figured out my life yet.", source: "Day 019 교재1", meaning: meanings["figure out_4"] },
        { week: 4, ko: "뭘 먹을지 고민되시나요? 걱정 마세요! 저희 식당에는 여러분이 먹고 싶어 할 만한 모든 종류의 요리가 있습니다.", en: "Can’t figure out what to eat? No problem! We have every type of cuisine you might be craving.", source: "Day 019 교재1", meaning: meanings["figure out_4"] },
        { week: 4, ko: "결혼한 지 5년이 되었는데 남편에 대해 잘 모르겠어요.", en: "I’ve been married for five years and I can’t figure out my husband.", source: "Day 019 교재1", meaning: meanings["figure out_5"] },
        { week: 4, ko: "제 짝이 오랫동안 제 옆에 있었던 것 같습니다. 40대가 되어서야 그 사실을 깨달았네요.", en: "I guess the right person was there all along. It just took me until my forties to figure it out.", source: "Day 019 교재1", meaning: meanings["figure out_6"] },
        { week: 4, ko: "삿포로가 왜 이렇게 인기 있는 휴양지인지 도무지 이해가 안 되네. 그렇게 아름답지도, 그렇다고 가격이 합리적이지도 않은데 말이지.", en: "I can’t figure out why Sapporo is such a popular vacation spot. It’s not all that beautiful or reasonably-priced.", source: "Day 019 교재1", meaning: meanings["figure out_7"] },
        { week: 4, ko: "남자 친구에게 거짓말을 하는 건 위험해. 거짓말하는 걸 알게 되면, 너희 관계가 끝날지도 몰라.", en: "Lying to your boyfriend is risky. If he figures it out, it could ruin your relationship.", source: "Day 019 교재1", meaning: meanings["figure out_8"] },
        { week: 4, ko: "안녕, 수진아. 잘 지냈어? 어디로 이사 갈지는 이제 정한 거지?", en: "Hey, Sujin. What’s up? Have you figured out where to move yet?", source: "Day 019 교재2", meaning: meanings["figure out_4"] },
        { week: 4, ko: "아니. 어떻게 해야 할지 아직 잘 모르겠어. 서초구를 생각하고 있었는데, (거기로 이사 갈) 형편이 안 될 것 같아서….", en: "Not really. I’m stuck. I was considering Seocho-gu, but I don’t think I can afford anything there….", source: "Day 019 교재2", meaning: null },
        { week: 4, ko: "철호야, 안녕. 다시 한번 확인차 연락했어. 우리 7시에 보는 거 맞지?", en: "Hi, Cheolho. I just wanted to double-check. We’re meeting at 7, right?", source: "Day 019 교재2", meaning: null },
        { week: 4, ko: "맞아. 산 초입 쪽에 있는 식당에서 만나서 뭐 먹을지 정하고 든든하게 (등산을) 시작하자. 서두를 것 없으니.", en: "Yep. Let’s meet at the restaurant at the foot of the mountain, figure out what to eat, and then start off with full bellies. There’s no need to rush.", source: "Day 019 교재2", meaning: meanings["figure out_4"] },
        { week: 4, ko: "대학에서 원래는 법을 공부하셨는데요. 어떤 계기로 전공을 바꾸어서 의학을 공부하게 되셨나요?", en: "You started off college studying law. What actually made you change your major and study medicine?", source: "Day 019 교재2", meaning: null },
        { week: 4, ko: "2학년이 되어서야 법 공부가 안 맞는다는 걸 깨달았어요. 그래서 결심을 하고 의과대로 바꾸었습니다.", en: "Well, it wasn’t till my sophomore year that I figured out studying law just wasn’t for me. That’s when I made up my mind and switched to medicine.", source: "Day 019 교재2", meaning: meanings["figure out_6"] },
        { week: 4, ko: "나는 지금 5개월째 쉬고 있다.", en: "I’ve been out of work for five months now.", source: "Day 019 교재3", meaning: null },
        { week: 4, ko: "그런데 다음으로 뭘 해야 할지 모르겠다.", en: "I can’t figure out what to do next.", source: "Day 019 교재3", meaning: meanings["figure out_4"] },
        { week: 4, ko: "대학원을 갈까 싶기도 하지만 돈이 좀 여유가 없다.", en: "I guess I could go to graduate school, but money has been a bit tight.", source: "Day 019 교재3", meaning: null },
        { week: 4, ko: "그래서 조만간 직장을 찾아야 할 것 같다.", en: "So, I think I’ll have to find work soon.", source: "Day 019 교재3", meaning: null },
        { week: 4, ko: "사실 캘리포니아에서 사업을 시작할 것을 고민 중인 친구로부터 제안을 받았다.", en: "I actually got an offer from my friend who is considering setting up a business in California.", source: "Day 019 교재3", meaning: null },
        { week: 4, ko: "그 친구 말이 내가 영어를 잘하니 내 도움이 필요하다는 것이다.", en: "He said he could use my help since I am quite fluent in English.", source: "Day 019 교재3", meaning: null },
        { week: 4, ko: "다른 나라에 가서 산다는 건 큰 변화처럼 느껴지지만, 그런 게 바로 내게 필요한 것일지도 모른다.", en: "Moving across the world seems like a big change, but maybe something like that is exactly what I need.", source: "Day 019 교재3", meaning: null },
        { week: 4, ko: "Jane이 출산휴가 중이라 사장님이 그녀를 대신할 임시 직원을 찾고 있어요.", en: "Jane is on maternity leave, so the boss is looking for a temp to fill in for her.", source: "Day 020 교재1", meaning: meanings["fill in for_1"] },
        { week: 4, ko: "지수 씨, 내일 제 일 좀 맡아 줄 수 있을까요? 아들이 야구 경기를 하는데 꼭 가 봐야해서요.", en: "Hey, Jisoo, could you fill in for me tomorrow? My son’s having a baseball game that I don’t want to miss.", source: "Day 020 교재1", meaning: meanings["fill in for_1"] },
        { week: 4, ko: "저희 제빵사가 다음 주 해외 워크숍에 참석합니다. 이 친구를 대신할 오전 근무 인원이 몇 명 필요할 것 같네요. 저희 제빵사 실력이 워낙 좋아서 말이죠.", en: "Our pastry chef is attending a workshop abroad next week. It’ll take a few morning people to fill in for her. She’s really good.", source: "Day 020 교재1", meaning: meanings["fill in for_1"] },
        { week: 4, ko: "포수는 대타를 구하기 쉽지 않은 포지션입니다. 다른 포지션과는 매우 다릅니다.", en: "It’s hard to find someone to fill in for the catcher. That position is so different from the others.", source: "Day 020 교재1", meaning: meanings["fill in for_1"] },
        { week: 4, ko: "내일 휴가라고 들었어. 업무를 대신 처리해 줄 사람은 찾았어?", en: "I heard you’re taking tomorrow off. Have you found anyone to fill in for you yet?", source: "Day 020 교재2", meaning: meanings["fill in for_1"] },
        { week: 4, ko: "응, Joe가 대신 맡아 주기로 했어. 넌 언제 휴가 갈 계획이야?", en: "Yeah, Joe said he’d take over for me. When are you planning to take your vacation?", source: "Day 020 교재2", meaning: null },
        { week: 4, ko: "John, 어려운 부탁 하나 할게요. 아들이 아파서 가서 챙겨 줘야 해요. 당신이 이 프로젝트를 속속들이 알고 있잖아요. 이따가 나 대신 콘퍼런스 콜을 좀 해 줄 수 있을까요?", en: "John, I need to ask you a huge favor. My son is sick and I need to go check in on him. You know this project backwards and forwards. Could you fill in for me on the conference call later?", source: "Day 020 교재2", meaning: meanings["fill in for_1"] },
        { week: 4, ko: "물론이죠! 제가 할게요. 세부 내용을 문자로 주시면 제가 콘퍼러스 콜을 처리하겠습니다. 아드님이 무탈하길 바랍니다.", en: "Of course! I’ve got your back. Just text me the details and I’ll handle the call. Hope everything’s okay with your son.", source: "Day 020 교재2", meaning: null },
        { week: 4, ko: "지난 몇 주 동안 엄청 바빠 보이는구나. 평소보다 더. 무슨 일이야?", en: "You’ve seemed so busy these past few weeks. Busier than usual. What’s been going on?", source: "Day 020 교재2", meaning: null },
        { week: 4, ko: "음, 팀장님에게 갑작스레 가족 문제가 생겨서 몇 주간 자리를 비워야만 했어. 그래서 내가 팀장님 일까지 대신 해야 했어. 너무 힘들었어.", en: "Well, my supervisor had a family emergency and had to leave for a few weeks, so I’ve had to fill in for her. It’s been exhausting.", source: "Day 020 교재2", meaning: meanings["fill in for_1"] },
        { week: 4, ko: "지난 몇 년간 내 소유의 작은 카페를 운영해 오고 있다.", en: "Over the past few years, I’ve been managing a little café that I own.", source: "Day 020 교재3", meaning: null },
        { week: 4, ko: "오후에만 문을 여는데 나름 단골도 많다.", en: "It opens in the afternoons only and attracts a good number of regulars.", source: "Day 020 교재3", meaning: null },
        { week: 4, ko: "카페가 작아서 그동안은 혼자 운영하기에 충분했다. 이 덕분에 비용을 절감할 수 있었다.", en: "The café is small enough that I’ve managed it alone, which helps to keep my expenses down.", source: "Day 020 교재3", meaning: null },
        { week: 4, ko: "하지만 나 대신 일을 봐줄 사람이 없었다는 건 그동안 하루도 쉬지 못했다는 얘기다.", en: "However, not having anyone to fill in for me means I haven’t been able to take a single day off.", source: "Day 020 교재3", meaning: meanings["fill in for_1"] },
        { week: 4, ko: "가족 모임이나 여행에도 빠져야 했다.", en: "I’ve had to skip family gatherings and trips.", source: "Day 020 교재3", meaning: null },
        { week: 4, ko: "(그래서) 비용은 추가로 들겠지만 파트 타임 직원을 채용하는 걸 심각하게 고려하고 있다.", en: "Despite the extra cost, I’m seriously considering hiring a part-time helper.", source: "Day 020 교재3", meaning: null },
        { week: 4, ko: "그래야 내가 좀 쉴 수가 있을 테니 말이다.", en: "That way, I could finally enjoy some breaks.", source: "Day 020 교재3", meaning: null }
    ];

    // 3. 영어회화 데이터 (Week 1 ~ Week 4 통합)
    const rawConvData = [
        // Week 1
        { week: 1, source: "Day001 교재1", ko: "저는 재택근무 체질이 아니에요. 늘 딴짓하게 되거든요", en: "Working from home isn’t for me. I always get distracted." },
        { week: 1, source: "Day001 교재1", ko: "소개팅은 저랑 안 맞아요.", en: "Going on blind dates isn’t for me." },
        { week: 1, source: "Day001 교재1", ko: "노트북은 저랑 좀 안 맞아요. 키보드가 뭔가 엄청 불편하거든요.", en: "Laptops aren’t really for me. Something about the keyboards is super uncomfortable." },
        { week: 1, source: "Day001 교재1", ko: "전기차는 좀 별로예요. 충전소는 요즘 늘었지만, 여전히 엄청 귀찮게 느껴져요.", en: "Electric cars aren’t for me. We have more charging stations around now, but it still feels like too much of a hassle." },
        { week: 1, source: "Day001 교재1", ko: "그 사람 직업이 좋은 건 아는데, 그런 남자는 나는 별로야.", en: "I know he has a decent job, but guys like him aren’t really for me." },
        { week: 1, source: "Day001 교재2", ko: "우리 나가서 맛난 회 먹을까? 내가 살 게.", en: "Why don’t we go out and get some nice sashimi? My treat!" },
        { week: 1, source: "Day001 교재2", ko: "너무 고맙긴 한데. 난 회를 별로 안 좋아해. 식감이 적응이 안 돼.", en: "It’s kind of you to offer, but raw fish just isn’t for me. I can’t get used to the texture." },
        { week: 1, source: "Day001 교재2", ko: "청취 연습을 위해 <기묘한 이야기>를 시청할 것을 추천합니다.", en: "I recommend watching Stranger Things to practice listening." },
        { week: 1, source: "Day001 교재2", ko: "좋은 생각이긴 한데, 저는 미국 프로그램이 체질에 안 맞아요. 스토리에 재미가 안 붙어요.", en: "It’s a good idea, but American shows aren’t for me. I can’t really get into the stories." },
        { week: 1, source: "Day001 교재2", ko: "애들하고 정말 잘 노는군요. 선생님 할 생각은 해 보셨나요?", en: "You’re really great around kids. Have you ever thought of being a teacher?" },
        { week: 1, source: "Day001 교재2", ko: "아니요. 저는 가르치는 거랑 잘 안 맞아요. 애들이랑 노는 건 좋은데, 공부시키는 게 너무 힘들 듯해요", en: "No, no. Teaching isn’t really for me. I like to play with them but trying to make them study seems like hard work." },
        { week: 1, source: "Day001 교재3", ko: "안녕, Greg. 내가 생일 선물로 받은 로잉 머신 기억하지? 혹시 관심 있어? 나랑은 별로 안 맞더라고", en: "Hey, Greg. Do you remember that rowing machine I got for my birthday? Are you interested in it? Turns out it’s not really for me." },
        { week: 1, source: "Day001 교재4", ko: "좋아 보인다. 편안해 보이고. 학과장을 안 하는 게 너랑 맞는 거야", en: "You look good. Relaxed. Not being chair suits you." },
        { week: 1, source: "Day001 교재4", ko: "혼자 일하는 건 나랑 안 맞는다는 걸 느꼈어. 사람들이랑 같이 있어야 진짜로 능률이 오르거든.", en: "I’ve found that working on my own doesn’t really suit me. I need to be around other people to really be productive." },
        { week: 1, source: "Day001 교재4", ko: "있잖아. 재택근무는 내 체질이 아니야.", en: "You know what? Working from home doesn’t really work for me." },
        { week: 1, source: "Day001 대표", ko: "재택근무는 저랑 안 맞아요.", en: "Working from home isn’t for me." },
        { week: 1, source: "Day002 교재1", ko: "다음 에피소드는 어떤 내용일지 궁금해 미치겠어.", en: "I can’t wait to see what the next episode will bring." },
        { week: 1, source: "Day002 교재1", ko: "아내가 제 선물을 개봉할 때 어떤 표정일지 궁금해 죽겠습니다.", en: "I can’t wait to see the look on my wife’s face when she opens my gift." },
        { week: 1, source: "Day002 교재1", ko: "이 프로젝트가 빨리 끝났으면 좋겠어요. 너무 오래 걸립니다.", en: "I can’t wait to be done with this project. It’s taking forever." },
        { week: 1, source: "Day002 교재1", ko: "여보, 저녁 식사가 너무 맛있는 냄새가 나네. 어서 먹고 싶어.", en: "That dinner smells delicious, honey. I can’t wait." },
        { week: 1, source: "Day002 교재1", ko: "<베이비 드라이버>가 미국에서는 몇 달 전에 개봉했어. 이곳에서도 어서 개봉했으면 좋겠다.", en: "Baby Driver was released months ago in the United States. I can’t wait for it to come out here." },
        { week: 1, source: "Day002 교재2", ko: "그 책 드디어 영화로 만들었다며?", en: "Did you hear they finally made that book into a movie?" },
        { week: 1, source: "Day002 교재2", ko: "응! 어서 보고 싶어. 내가 제일 좋아하는 장면들이 다 포함되어 있기를.", en: "Yes! I can’t wait to see it. I hope they included all my favorite scenes." },
        { week: 1, source: "Day002 교재2", ko: "프로젝트는 잘 되어 가나요? 한동안 매달려 있으신 것 같은데.", en: "How’s that project going? It seems like you’ve been working on it for a while." },
        { week: 1, source: "Day002 교재2", ko: "네, 일주일 내내 이것을 하고 있습니다. 어서 끝내고 뭔가 다른 걸로 넘어가고 싶어요.", en: "Yeah, I’ve been working on it all week. I can’t wait to finish it and finally move on to something else." },
        { week: 1, source: "Day002 교재2", ko: "네가 뜨개질할 수 있는 걸 몰랐네. 뭐 만들고 있니?", en: "I didn’t know you could knit. What are you making?" },
        { week: 1, source: "Day002 교재2", ko: "여동생에게 줄 스카프를 만들고 있어. 내가 자기 주려고 이걸 만든 걸 알면 어떤 표정일까 궁금해 죽겠어.", en: "I’m making a scarf for my little sister. I can’t wait to see the look on her face when she realizes I made this for her." },
        { week: 1, source: "Day002 교재3", ko: "신형 그랜저를 어서 보고 싶네요. 위장막 때문에 스파이 샷이 의미가 없어서 너무 속상하네요.", en: "I can’t wait to get a glimpse of the new Grandeur. Hyundai uses these car camouflage wraps. They make spy shots useless and it’s really frustrating." },
        { week: 1, source: "Day002 교재4", ko: "(소개팅 상황에서) 그 여성분 어서 만나 보고 싶어.", en: "I’m really anxious to meet her." },
        { week: 1, source: "Day002 교재4", ko: "어서 집에 가서 선물을 개봉해 보고 싶다.", en: "I’m anxious to get home to open my presents." },
        { week: 1, source: "Day002 교재4", ko: "(이메일에서) 말씀 많이 들었습니다. 하루빨리 함께 일하고 싶습니다.", en: "I’ve heard a great deal about you. I look forward to working with you." },
        { week: 1, source: "Day002 교재4", ko: "안 그래도 그 콘서트 너무 기대됐는데, 아이유도 나온다고 하니까 더 기대된다.", en: "I was looking forward to the concert, but now even more so, since I heard IU will be there." },
        { week: 1, source: "Day002 대표", ko: "하루빨리 새 집으로 이사 가고 싶어요.", en: "I can’t wait to move into the new house." },
        { week: 1, source: "Day003 교재1", ko: "제가 마지막 남은 피자 한 조각 먹어도 될까요?", en: "Do you mind if I finish off the last piece of pizza?" },
        { week: 1, source: "Day003 교재1", ko: "미안한데, 오는 길에 커피 좀 사다 줄 수 있나요?", en: "Do you mind grabbing me some coffee on your way?" },
        { week: 1, source: "Day003 교재1", ko: "제가 여유 시간이 겨우 5분 있어요. 짧게 해 주실 수 있을까요?", en: "I’ve got only five minutes to spare. Do you mind keeping it short?" },
        { week: 1, source: "Day003 교재1", ko: "에어컨 좀 약하게 하면 안 될까요? 좀 추워서요.", en: "Do you mind turning down the air-conditioning? I feel a bit cold." },
        { week: 1, source: "Day003 교재1", ko: "개인적인 질문 하나 해도 될까요?", en: "Do you mind if I ask you a personal question?" },
        { week: 1, source: "Day003 교재2", ko: "죄송한데, 회의를 금요일로 옮겨도 될까요?", en: "Do you mind if we move the meeting to Friday?" },
        { week: 1, source: "Day003 교재2", ko: "네, 괜찮습니다. 사실 저희에겐 금요일이 더 좋아요.", en: "Sure. Friday works better for us, actually." },
        { week: 1, source: "Day003 교재2", ko: "죄송한데, 꼭대기 선반에 있는 저 시리얼 상자들 중 하나를 내려 줄 수 있을까요?", en: "Excuse me, do you mind grabbing me one of those cereal boxes on the top shelf?" },
        { week: 1, source: "Day003 교재2", ko: "당연하죠. 얼마든지요", en: "Sure. Always happy to help!" },
        { week: 1, source: "Day003 교재2", ko: "어디서 만나면 될까요? (비즈니스적)", en: "Where would you like to meet?" },
        { week: 1, source: "Day003 교재2", ko: "제가 그쪽 사무실로 가도 상관없습니다.", en: "I don’t mind coming over to your office." },
        { week: 1, source: "Day003 교재3", ko: "괜찮으시면 혹시 모르니까 이번에는 2시 30분에 시작해도 될는지요?", en: "If you don’t mind, could we start at 2:30 this time, just to be safe?" },
        { week: 1, source: "Day003 교재4", ko: "제가 이번 주 금요일에 이사 나가요. 미안하지만 좀 도와주실 수 있을까요?", en: "I’m moving out this Friday. Would you mind giving me a hand?" },
        { week: 1, source: "Day003 교재4", ko: "슬슬 배가 고프네요. 잠깐 나가서 간단히 뭐 좀 먹고 와도 될까요?", en: "I’m starting to get a bit hungry. Would you mind if I stepped out for a moment and grabbed a bite to eat?" },
        { week: 1, source: "Day003 대표", ko: "죄송한데 조금 짧게 해 주시겠어요?", en: "Do you mind keeping it a bit short?" },
        { week: 1, source: "Day004 교재1", ko: "그 여자분 키 엄청 커요.", en: "She is super tall." },
        { week: 1, source: "Day004 교재1", ko: "그 사람이 무지 바쁘거나, 아니면 저에 대한 관심이 식고 있는 거겠죠.", en: "Either he has been super busy, or he is losing interest in me." },
        { week: 1, source: "Day004 교재1", ko: "제가 요즘 이사 준비 때문에 엄청 바빴어요.", en: "I’ve been super busy with my upcoming move." },
        { week: 1, source: "Day004 교재1", ko: "우와. 연세 있으신 분치고는 몸매가 너무 좋으시네요.", en: "Wow. You’re in super good shape for an old guy." },
        { week: 1, source: "Day004 교재1", ko: "서울은 어디라도 다 너무 비싸. 근데 후암동은 상대적으로 저렴한 편이지", en: "All the neighborhoods in Seoul are super expensive, but Huam-dong is relatively cheap" },
        { week: 1, source: "Day004 교재2", ko: "무슨 점심값이 만 원이 넘는 거야.", en: "I never thought I’d have to pay over 10,000 won for lunch." },
        { week: 1, source: "Day004 교재2", ko: "그러게. 요새 물가가 너무너무 비싸.", en: "Yeah. Everything is getting super expensive." },
        { week: 1, source: "Day004 교재2", ko: "오! 그럼 귤 한 박스 사다 줄래? 지금 제철이니 엄청 쌀 거야.", en: "Oh! How about a box of tangerines? They should be super cheap since they’re in-season." },
        { week: 1, source: "Day004 교재2", ko: "11월 말치고는 너무 따뜻하다. 지금쯤이면 보통은 훨씬 더 추운데.", en: "It’s unusually warm for late November. It’s usually much colder by now." },
        { week: 1, source: "Day004 교재2", ko: "맞아. 가을이 점점 짧아지고는 있는데 올해는 엄청 길다.", en: "Right. Autumn has been getting shorter, but this year, it’s been super long." },
        { week: 1, source: "Day004 교재3", ko: "이번 침대 프레임 조립은 정말 쉽더군요. 조립하는 데 한 시간도 안 걸렸습니다.", en: "this bed frame was super easy to put together. It took me less than an hour." },
        { week: 1, source: "Day004 교재4", ko: "그 삼겹살집은 맛은 괜찮은 편인데 가격이 상당히 비싸다.", en: "The pork belly place is pretty good, but it’s quite expensive." },
        { week: 1, source: "Day004 교재4", ko: "당신은 영어를 상당히 잘하는군요.", en: "Your English is quite good." },
        { week: 1, source: "Day004 대표", ko: "물가가 올라도 너무 올라요.", en: "Everything is getting super expensive." },
        { week: 1, source: "Day005 교재1", ko: "중매업체에 등록해 보는 게 어때요?", en: "How do you feel about signing up for a matchmaking service?" },
        { week: 1, source: "Day005 교재1", ko: "교회에 가 보는 게 어때요?", en: "How do you feel about going to church?" },
        { week: 1, source: "Day005 교재1", ko: "등산 모임에 가입해 보는 게 어떨까요?", en: "How do you feel about joining a hiking club?" },
        { week: 1, source: "Day005 교재1", ko: "성형 수술 하는 거 어떻게 생각하세요?", en: "How do you feel about plastic surgery?" },
        { week: 1, source: "Day005 교재2", ko: "코치가 전 동료였는데, 그런 팀에 합류하는 기분이 어떠신가요?", en: "How do you feel about joining a team when the coach is your ex-teammate?" },
        { week: 1, source: "Day005 교재2", ko: "저녁 먹고 우리 집에 가서 <컨저링> 볼까 하는데. 공포 영화 어때?", en: "After dinner, I was thinking we could go to my place and watch The Conjuring. How do you feel about horror movies?" },
        { week: 1, source: "Day005 교재2", ko: "싫어, 공포 영화는 못 보겠어. 무서운 거 보는 게 뭐가 재미있다고.", en: "No, I can’t stand horror movies! Watching something scary isn’t my idea of fun." },
        { week: 1, source: "Day005 교재3", ko: "Frank 팀장님 밑에서 일하니까 어떤가요? 이동 제안을 진지하게 고민해보기 전에 우선 당신의 경험을 듣고 싶습니다.", en: "How do you feel about working under Frank? I want to hear about your experience before I really consider his transfer offer." },
        { week: 1, source: "Day005 교재4", ko: "모든 게 완전 비싸네.", en: "Everything is totally overpriced." },
        { week: 1, source: "Day005 교재4", ko: "완전 깜박했어.", en: "It totally slipped my mind." },
        { week: 1, source: "Day005 교재4", ko: "양복 입으니까 완전 딴 사람 같네.", en: "You look totally different in a suit." },
        { week: 1, source: "Day005 교재4", ko: "완전 괜찮아.", en: "That’s totally fine." },
        { week: 1, source: "Day005 교재4", ko: "괜히 고치느라 애쓰지 마. 완전히 고장이 났으니까.", en: "Don’t bother trying to fix it. It’s totally broken." },
        { week: 1, source: "Day005 교재4", ko: "그 영화는 무조건 아이맥스로 봐야 해.", en: "You should totally go see the movie in IMAX." },
        { week: 1, source: "Day005 교재4", ko: "난 초밥이 너무 땡겨.", en: "I could totally go for some sushi." },
        
        // Week 2
        { week: 2, source: "Day006 교재1", ko: "안 좋았던 한 주를 날려 버리려면 친구들과 맛있는 식사를 하는 게 최고지.", en: "There’s nothing like a nice meal with friends to turn a bad week around." },
        { week: 2, source: "Day006 교재1", ko: "주말 내내 넷플릭스 드라마 보는 게 최고야.", en: "There’s nothing like binging a show on Netflix all weekend." },
        { week: 2, source: "Day006 교재1", ko: "크리스마스에는 칠면조 저녁 식사와 풍미 좋은 와인만 한 게 없지.", en: "There is nothing like a turkey dinner and spiced wine for Christmas." },
        { week: 2, source: "Day006 교재1", ko: "다시 콘서트에 갈 수 있어서 너무 좋아. 가장 좋아하는 밴드의 라이브 공연을 보는 것만큼 좋은 것은 없지.", en: "I’m so glad we can go to concerts again. There’s nothing like seeing your favorite band live." },
        { week: 2, source: "Day006 교재1", ko: "팬케이크에는 진짜 메이플 시럽을 얹어 먹어야 제맛이야.", en: "There’s nothing like real maple syrup on pancakes." },
        { week: 2, source: "Day006 교재2", ko: "회사에서 힘든 하루를 보내고 나면 시원한 맥주가 최고지. 퇴근하고 한잔하러 갈까?", en: "There’s nothing like a cold beer after a long day of work. How about we go grab one when we get off?" },
        { week: 2, source: "Day006 교재2", ko: "좋지! 네가 산다면.", en: "Sure! As long as you’re buying." },
        { week: 2, source: "Day006 교재2", ko: "내가 좀 쳐져 보인다면 미안. 남자 친구랑 잠시 안 보기로 했거든. 근데 정말 보고 싶어.", en: "Sorry if I seem a little depressed. My boyfriend and I decided to take a little break. I really miss him." },
        { week: 2, source: "Day006 교재2", ko: "아, 그랬구나. 그럼 쇼핑하러 가자. 내가 널 알잖아. 기분 전환에는 옷 사는 게 최고야.", en: "Aww, I’m sorry. Come on, let’s go shopping. I know you. There’s nothing like buying clothes to cheer you up." },
        { week: 2, source: "Day006 교재2", ko: "어서 집에 가고 싶어. 남편이 특별한 음식을 해 준다고 했거든", en: "I can’t wait to get home. My husband said he would cook me something special." },
        { week: 2, source: "Day006 교재2", ko: "오, 멋지다! 힘든 하루를 보낸 후에는 기운을 차리는 데 집밥만 한 게 없지.", en: "Oh, that’s perfect then! There’s nothing like a home-cooked meal to lift your spirits after a long day." },
        { week: 2, source: "Day006 교재3", ko: "바디프렌드 매장에 오셔서 새로 출시된 프로 마사지 X7 의자를 체험해 보세요. 일주일에 세 번 전문 마사지사를 집에 부른다고 해도 저희 의자로 받는 안마가 최고라는 점을 인정할 수밖에 없을 겁니다.", en: "Come to a Bodyfriend store and try out our new line of Pro Massage X7 chairs. Even if you have a professional masseur come to your house three times a week, you’ll have to admit there’s nothing like a massage from one of our chairs." },
        { week: 2, source: "Day006 교재4", ko: "서울에서는 이 집 빵이 최고야.", en: "You can’t find any better bread in Seoul." },
        { week: 2, source: "Day006 교재4", ko: "서핑하기에는 오늘이 딱이다.", en: "I can’t imagine a better day for surfing." },
        { week: 2, source: "Day006 교재4", ko: "요즘같이 장사가 안 된 적도 없어요.", en: "Business has never been any worse." },
        { week: 2, source: "Day006 교재4", ko: "이건 Sarah가 가장 잘 알지.", en: "No one knows this better than Sarah." },
        { week: 2, source: "Day006 교재4", ko: "지난 분기에 가장 많은 차를 판매했다.", en: "They sold more vehicles last quarter than they ever have." },
        { week: 2, source: "Day006 교재4", ko: "카카오톡이 한국에서 가장 성공한 플랫폼인 것 같습니다.", en: "No other platform in Korea seems more successful than KakaoTalk." },
        { week: 2, source: "Day006 대표", ko: "재충전에는 캠핑만 한 게 없죠.", en: "There is nothing like camping to recharge your batteries." },
        { week: 2, source: "Day007 교재1", ko: "미슐랭 스타를 받은 음식이라면 뭐든 좋아.", en: "I’m up for anything with a Michelin star." },
        { week: 2, source: "Day007 교재1", ko: "뭐 하고 싶어? 난 뭐든 다 좋아.", en: "What do you feel like doing? I’d be up for just about anything." },
        { week: 2, source: "Day007 교재1", ko: "나 비어 퐁 파트너 찾고 있는데. 관심 있어?", en: "I’m looking for a beer pong partner. Are you down?" },
        { week: 2, source: "Day007 교재1", ko: "나 프라이드치킨이 무지 먹고 싶어. 오늘 밤에 같이 먹을 해. 관심 있을까?", en: "I’ve been craving fried chicken. Is anyone down for some tonight?" },
        { week: 2, source: "Day007 교재1", ko: "토요일 아침에 북한산 등산 갈까 하는데 같이 갈 사람이 필요해. 관심 있을까?", en: "I was thinking of hiking Bukhan Mountain on Saturday morning, and I need a buddy. Do you think you’d be down?" },
        { week: 2, source: "Day007 교재2", ko: "오늘 저녁에 뭐 먹고 싶어?", en: "What do you want to have tonight?" },
        { week: 2, source: "Day007 교재2", ko: "뭐라도 좋아. 너무 매운 것만 아니면.", en: "I am up for anything, as long as it’s not too spicy." },
        { week: 2, source: "Day007 교재2", ko: "안녕 얘들아, 나랑 고든 램지 버거 먹으러 갈 사람 있을까?", en: "Hey guys‒anyone want to go with me to try Gordon Ramsey’s burger place?" },
        { week: 2, source: "Day007 교재2", ko: "나 갈게! 네가 산다면 말이야.", en: "I’m down, as long as you’re paying." },
        { week: 2, source: "Day007 교재2", ko: "얘들아, 오늘 밤에 애들 집에 불러서 게임할까 하는데. 같이 할 사람?", en: "Guys, I was thinking about having people over for a game night. Who’s in?" },
        { week: 2, source: "Day007 교재2", ko: "음, 이따가 약속이 있긴 한데, 얼굴 정도는 비출 수 있어.", en: "Well, I have plans later, but I am down for saying hello, at least." },
        { week: 2, source: "Day007 교재3", ko: "일생일대의 모험을 즐기고 싶으신가요? 에베레스트 등반을 원하신다면 네팔 어드벤처를 선택해 주세요. 이번 한 달 동안만 신규 고객을 위한 특별 행사가 준비되어 있습니다. 선착순 50인은 무료로 여행 보험에 가입할 수 있습니다. 다른 고객이 채 가기 전에 지금 예약하세요.", en: "Are you down for the adventure of a lifetime? Choose Nepal Adventures for your Everest climb. We have a special offer for new customers available only this month. The first fifty hikers who sign up will receive free travel insurance. Act now, before someone else takes your spot" },
        { week: 2, source: "Day007 교재4", ko: "아무거나 좋아요.", en: "Anything sounds great." },
        { week: 2, source: "Day007 교재4", ko: "아무거나 괜찮아요.", en: "Anything would be fine." },
        { week: 2, source: "Day007 교재4", ko: "(너무) 좋습니다.", en: "That would be (so) nice." },
        { week: 2, source: "Day007 교재4", ko: "나랑 같이 가는 거 어때?", en: "Are you interested in coming with me?" },
        { week: 2, source: "Day007 교재4", ko: "너무 좋지.", en: "That would be nice." },
        { week: 2, source: "Day007 교재4", ko: "좋은 생각이야.", en: "That sounds like a good idea." },
        { week: 2, source: "Day007 대표", ko: "너무 매운 것만 아니면 뭐든 다 좋아요", en: "I am up for anything, as long as it’s not too spicy" },
        { week: 2, source: "Day008 교재1", ko: "나도 가고는 싶은데, 오늘 몸이 좀 안 좋아.", en: "I wish I could come, but I don’t feel quite right today." },
        { week: 2, source: "Day008 교재1", ko: "저녁을 같이 못 할 것 같습니다. 오늘 몸이 좀 안 좋네요.", en: "I’m afraid I can’t join you for dinner. I don’t feel quite right today." },
        { week: 2, source: "Day008 교재1", ko: "여보, 나 오늘 몸이 좀 안 좋아. 수진이 학교에서 좀 데려와 줄래?", en: "Honey, I don’t feel quite right today. Can you pick up Sujin from school?" },
        { week: 2, source: "Day008 교재1", ko: "오늘은 저녁 안 먹을래. 오늘 속이 좀 안 좋아서.", en: "I think I’ll skip dinner. My stomach doesn’t feel quite right today." },
        { week: 2, source: "Day008 교재1", ko: "너희 딸 파티에 나는 안 가는 게 좋을 것 같아. 오늘 몸이 좀 안 좋네.", en: "I don’t think it’s a good idea for me to come to your daughter’s party. I don’t feel quite right today" },
        { week: 2, source: "Day008 교재2", ko: "뭔지는 모르겠는데, 오늘 몸이 좀 안 좋아.", en: "I’m not sure what it is, but I don’t feel quite right today." },
        { week: 2, source: "Day008 교재2", ko: "아침에 먹은 국에 문제가 있었을지도. 좀 상한 냄새가 났거든.", en: "Maybe there was something wrong with that soup you had for breakfast. It smelled a little funny." },
        { week: 2, source: "Day008 교재2", ko: "Sam, 우리 점심 약속 유효한 거지?", en: "Sam, are you still able to meet for lunch?" },
        { week: 2, source: "Day008 교재2", ko: "미안해, 사실 오늘 몸이 좀 안 좋아. 집에 있어야 할 것 같아. 다음에 먹어도 될까?", en: "Sorry, actually I don’t feel quite right today. I think I need to stay home. Can we take a rain check?" },
        { week: 2, source: "Day008 교재2", ko: "오늘 밤에 우리 밖에 나가 놀기로 한 건 아는데, 오늘 뭔가 몸이 좀 이상해.", en: "I know we’re supposed to go out tonight, but I don’t feel quite right today." },
        { week: 2, source: "Day008 교재2", ko: "이런. 괜찮은 거야? 그냥 음식 포장해 와서 집에서 영화 보면서 쉬는 건 어때?", en: "Oh, no. Are you okay? How about we get takeout and rest at home with a movie instead?" },
        { week: 2, source: "Day008 교재3", ko: "안녕하세요, Brian. 샌프란시스코 사무실은 요즘 어때요? 지난주에 코로나 걸렸다고 들었는데, 안부 확인차 연락드렸어요. 사실 저도 어제 몸이 안 좋아서 검사를 했는데 다행히 음성으로 나왔습니다", en: "Hi, Brian. How are things going over there in the San Francisco office? I heard that you caught COVID last week, and so I wanted to check in and ask how you’re doing. Actually, I wasn’t feeling quite right myself yesterday. I got tested and thankfully, it turned out negative." },
        { week: 2, source: "Day008 교재4", ko: "몸이 조금 아프네.", en: "I feel a little bit sick." },
        { week: 2, source: "Day008 교재4", ko: "몸이 너무 안 좋아.", en: "I am really not feeling well." },
        { week: 2, source: "Day008 교재4", ko: "몸이 좀 안 좋아.", en: "I am not really feeling well." },
        { week: 2, source: "Day008 교재4", ko: "다행히 많이 나아졌어.", en: "I am feeling much better thankfully." },
        { week: 2, source: "Day008 교재4", ko: "계속 기운이 좀 없네.", en: "I have been feeling low." },
        { week: 2, source: "Day008 교재4", ko: "감기 기운이 있다.", en: "I feel like I’m coming down with a cold." },
        { week: 2, source: "Day008 교재4", ko: "감기인지 뭔지는 모르지만 뭔가 몸이 안 좋다.", en: "I feel like I’m coming down with something." },
        { week: 2, source: "Day008 대표", ko: "오늘 몸이 좀 안 좋아요.", en: "I don’t feel quite right today." },
        { week: 2, source: "Day009 교재1", ko: "제가 첫 번째 문장을 읽을까요?", en: "Would you like me to read the first sentence?" },
        { week: 2, source: "Day009 교재1", ko: "제가 일어난 김에 물 좀 가져다드릴까요?", en: "Would you like me to get you some water while I’m up?" },
        { week: 2, source: "Day009 교재1", ko: "수정본 보내드릴까요?", en: "Would you like me to send you the revised version?" },
        { week: 2, source: "Day009 교재1", ko: "내가 따라가 줄까?", en: "Do you want me to come along with you?" },
        { week: 2, source: "Day009 교재1", ko: "내가 너희 둘 자리 마련해 줄까?", en: "Do you want me to set you two up on a date?" },
        { week: 2, source: "Day009 교재2", ko: "지금 역에서 걸어가고 있는데요. 커피 사다 드릴까요?", en: "I’m walking from the station. Would you like me to pick up any coffee?" },
        { week: 2, source: "Day009 교재2", ko: "좋습니다. 고마워요.", en: "That would be great. Thanks." },
        { week: 2, source: "Day009 교재2", ko: "아직 다 하지는 못했는데, 지금까지 작업한 거 보내 드릴까요?", en: "I’m not done yet, but would you like me to send you what I have so far?" },
        { week: 2, source: "Day009 교재2", ko: "고마워요. 그러면 언제까지 완성할 수 있을 것 같나요?", en: "Thanks. So, when do you think you will be able to complete the materials?" },
        { week: 2, source: "Day009 교재2", ko: "방금 인천에 도착했어요! 그나저나 면세점에 들를까 하는데요, 아빠랑 엄마 뭐 사다 드릴까요?", en: "I just landed at Incheon! By the way, I think I’ll go by a duty-free shop. Do you want me to get you or Mom anything?" },
        { week: 2, source: "Day009 교재2", ko: "말만이라도 고맙다, 내 딸.", en: "Thanks for offering, Sweetie." },
        { week: 2, source: "Day009 교재3", ko: "안녕하세요. 다름이 아니라 저희 수요일에 회의하는 거 맞는지 확인차 연락드립니다. 그리고 용산 본사에 회의실 잡을까요? (잠시 후) 근데 보니까 본사 회의실은 예약이 꽉 찼네요. 장소를 변경하든지, 날짜를 바꾸든지 해야 할 것 같습니다.", en: "I just wanted to make sure the meeting is still on for Wed. And would you like me to arrange a room at the headquarters in Yongsan? (pause) I just found out that headquarters is all booked up. We’ll have to change something, either the location or date." },
        { week: 2, source: "Day009 교재4", ko: "공항에 마중 나와 줘.", en: "I want you to pick me up at the airport." },
        { week: 2, source: "Day009 교재4", ko: "냄새 심해지기 전에 쓰레기 밖에 좀 내다버려 줘.", en: "I want you to take out the trash before it gets too smelly." },
        { week: 2, source: "Day009 교재4", ko: "Jeff와 회의 잡아 주십시오.", en: "I’d like you to set up a meeting with Jeff." },
        { week: 2, source: "Day009 교재4", ko: "우리 같이 회의록 살펴볼까요?", en: "I’d like us to go over the minutes." },
        { week: 2, source: "Day009 교재4", ko: "이런 가족 식사 자리를 좀 더 자주 가졌으면 해요.", en: "I’d like us to have more family dinners together like this." },
        { week: 2, source: "Day009 대표", ko: "저 지금 스타벅스인데 커피 사다 드릴까요?", en: "Would you like me to grab you some coffee while I’m at Starbucks?" },
        { week: 2, source: "Day010 교재1", ko: "주연 배우로 생각하고 있는 분 있으신가요?", en: "Do you have any actor in mind for the lead role?" },
        { week: 2, source: "Day010 교재1", ko: "괜찮은 소고깃집 생각해 둔 데 있어?", en: "Do you have any good beef place in mind?" },
        { week: 2, source: "Day010 교재1", ko: "딱히 염두에 둔 차는 없습니다. 상태만 좋으면 뭐라도 사겠습니다.", en: "I don’t really have any car in mind. I will go with pretty much anything as long as it’s in good shape." },
        { week: 2, source: "Day010 교재1", ko: "특별히 염두에 둔 건 없습니다.", en: "I have nothing particular in mind." },
        { week: 2, source: "Day010 교재1", ko: "다른 안이 없으시면 제가 제안을 하고 싶습니다.", en: "I’d like to make a suggestion, unless you have something in mind." },
        { week: 2, source: "Day010 교재2", ko: "가격대는 어느 정도 생각하세요?", en: "What price range do you have in mind?" },
        { week: 2, source: "Day010 교재2", ko: "십만 원 미만이면 다 괜찮아요.", en: "Anything under 100,000 won would be fine." },
        { week: 2, source: "Day010 교재2", ko: "포르쉐를 받으려면 2년을 기다리셔야 합니다. 그것도 보증금으로 오백만 원을 걸 때 이야기고요.", en: "You will have to wait two years to get a Porsche. And that’s only if you put down five million won as a deposit." },
        { week: 2, source: "Day010 교재2", ko: "그건 문제가 되지 않아요. 전 포르쉐 외에는 살 생각이 없거든요.", en: "It really doesn’t matter. A Porsche is the only car I have in mind." },
        { week: 2, source: "Day010 교재2", ko: "올해 여름휴가는 어디로 가고 싶어?", en: "Where do you want to go for our summer vacation this year?" },
        { week: 2, source: "Day010 교재2", ko: "몇 군데 생각하고 있는 곳이 있는데, 장시간 비행기 타도 괜찮아?", en: "I have a few places in mind. Are you okay taking a long flight?" },
        { week: 2, source: "Day010 교재3", ko: "워크숍 장소로 다음 두 곳을 생각하고 있습니다. 하지만 더 나은 곳이 있으면 알려 주십시오. 꼭 고려해 보겠습니다. 그나저나 조 인사팀장이 기조연설을 못 하게 되었다면서요. 팀 내에서 대신할 분 누구 염두에 두고 계신가요?", en: "We have the following two places in mind as possible sites for the workshop. However, if you have any suggestions for places that would be more suitable, please let us know. We’ll definitely take them into consideration. By the way, I heard that your HR manager, Ms. Cho, is no longer available to deliver the keynote speech. Does your team have anyone in mind to replace her?" },
        { week: 2, source: "Day010 교재4", ko: "연필 놓고 앞쪽으로 시험지를 제출하세요.", en: "Put your pencils down and hand your papers to the front." },
        { week: 2, source: "Day010 교재4", ko: "보증금으로 최소 50%는 걸어 두셔야 합니다.", en: "You have to put down at least 50%." },
        { week: 2, source: "Day010 교재4", ko: "내가 보기엔 네 이력서에 쓰기에 알콘에서의 인턴 경력이 너무 짧은 것 같아.", en: "I think your internship experience at Alcon is too short to put down on your résumé." },
        { week: 2, source: "Day010 교재4", ko: "건강보험 칸은 그냥 비워 두셨군요. 건강보험 미가입자인가요?", en: "You didn’t put down anything in the health insurance section. Does that mean you’re uninsured?" },
        { week: 2, source: "Day010 대표", ko: "가격대는 어느 정도 생각하세요?", en: "What price range do you have in mind?" },

        // Week 3
        { week: 3, source: "Day011 교재1", ko: "연휴 때 호주로 여행을 갈까 생각 중입니다.", en: "I was thinking of traveling to Australia for the holiday." },
        { week: 3, source: "Day011 교재1", ko: "오늘 저녁 약속 있어? 동료가 추천해 준 피자 가게 가 볼까 하는데.", en: "Do you already have dinner plans? I was thinking of trying a pizza place that my coworker recommended." },
        { week: 3, source: "Day011 교재1", ko: "다음 여행은 몽골을 생각 중이었는데, 안 가기로 했습니다.", en: "I was thinking of Mongolia for my next trip, but I decided not to go there." },
        { week: 3, source: "Day011 교재1", ko: "제가 생각하던 가격대보다 조금 비싸네요. 게다가 저는 좀 더 기본형인 것을 생각하고 있었거든요.", en: "It’s a little out of my price range. Besides, I was thinking of going with something more basic." },
        { week: 3, source: "Day011 교재2", ko: "지난달에 내가 소개팅했다고 이야기했었나? 완전히 내 스타일이야. 그녀 생일에 시계를 선물할까 고민 중이야.", en: "Did I mention that I had a blind date last month? She is really my cup of tea. I was thinking of getting her a watch for her birthday." },
        { week: 3, source: "Day011 교재2", ko: "너무 과하지 않아? 앞으로 눈만 더 높아질거야.", en: "Isn’t that a bit much? You are going to spoil her." },
        { week: 3, source: "Day011 교재2", ko: "여보, 벌써 요리 시작했어? 저녁으로 피자를 먹을까 하는데", en: "Honey, have you already started cooking? I was thinking of pizza for dinner." },
        { week: 3, source: "Day011 교재2", ko: "안 돼, 또 정크푸드 먹으면 안 된다고. 내가 살찌는 거 보고 싶어?", en: "No, but we shouldn’t have junk food again. You don’t want me getting fat." },
        { week: 3, source: "Day011 교재2", ko: "토요일 밤에 약속 있어? 노원에 새로 생긴 초밥집에 가 볼까 하는데, 같이 갈 사람이 없네.", en: "Do you have any plans for Saturday night? I was thinking of trying a new sushi place in Nowon and I need someone to go with me." },
        { week: 3, source: "Day011 교재2", ko: "좋아. 원래는 집에서 좀 쉴까 했는데. 초밥 먹는 게 훨씬 더 좋지.", en: "Oh, sure. I was just planning on chilling at home. Getting sushi sounds more fun." },
        { week: 3, source: "Day011 교재3", ko: "너무 늦게 연락드려서 죄송합니다. 1월 첫째 주에 있을 한국 출장 일정을 검토하다가 본사 부사장님과 회의를 잡을 수 있을까 고민 중이었습니다. 혹시 부사장님 그 주 수요일에 시간이 되실까요?", en: "I’m sorry for contacting you so late. We were going over the itinerary for our Korea trip the first week of January, and we were thinking of setting up a meeting with your vice-president at headquarters. By any chance, is she available that Wednesday?" },
        { week: 3, source: "Day011 교재4", ko: "진우 님, 안녕하세요. 평소대로 해 드릴까요?", en: "Hi, Jinwoo! Do you want the usual?" },
        { week: 3, source: "Day011 교재4", ko: "사실 오늘은 조금 다른 스타일로 할까 하는데요. 곱슬한 파마를 한번 해 보면 어떨까요?", en: "Actually, I was hoping to try something different today. Could we go with a tight perm?" },
        { week: 3, source: "Day011 교재4", ko: "그쪽 부사장님과 회의를 잡았으면 합니다.", en: "I was hoping to set up a meeting with your vice-president." },
        { week: 3, source: "Day011 대표", ko: "통번역대학원 진학을 고민하고 있어요.", en: "I was thinking of going to translation grad school." },
        { week: 3, source: "Day012 교재1", ko: "너의 자신감이 참 부럽다.", en: "I wish I had your confidence." },
        { week: 3, source: "Day012 교재1", ko: "저도 같이 가고 싶긴 한데, 시간이 안 나네요.", en: "I wish I could go with you, but I can’t find the time." },
        { week: 3, source: "Day012 교재1", ko: "제가 해산물을 못 먹어서 너무 아쉽네요.", en: "I wish I could eat seafood." },
        { week: 3, source: "Day012 교재1", ko: "항상 가족들이랑 시간을 좀 더 많이 보내고 싶은데 그러질 못하네요.", en: "I always wish I could spend more time with my family." },
        { week: 3, source: "Day012 교재1", ko: "내가 한 말을 주워 담을 수도 없고.", en: "I wish I could take back what I said." },
        { week: 3, source: "Day012 교재2", ko: "이번 주에 Jessica 뭐 하는지 들었니? 콘서트도 두 군데 가고 일본 여행도 간대!", en: "Did you hear what Jessica is doing this week? She’s going to two concerts and taking a trip to Japan!" },
        { week: 3, source: "Day012 교재2", ko: "와우, 나도 그럴 시간이 있었으면. 풀타임으로 일하니 너무 힘들고, 내가 좋아하는 걸 할 시간이 없어.", en: "Wow, I wish I had time for that. It’s so hard having a full-time job, and there’s never enough time to do things I really enjoy." },
        { week: 3, source: "Day012 교재2", ko: "이백만 원짜리 이 의자 상당히 실망스러워. 반품도 안 되니, 원.", en: "I am actually quite disappointed with this two million won chair. I wish I could take it back." },
        { week: 3, source: "Day012 교재2", ko: "언제라도 당근마켓에 되팔면 되지 뭐.", en: "You can always resell it on Daangn Market." },
        { week: 3, source: "Day012 교재2", ko: "날도 추워지고 하니 어릴 때 생각이 많이 나네. 혹시 후회되는 거 있어?", en: "The weather’s getting cold and that always makes me look back on my youth. Do you have any regrets?" },
        { week: 3, source: "Day012 교재2", ko: "응, 어릴 때 1년을 더 기다렸다 대학에 갔어야 했어.", en: "Yeah, when I was younger, I wish I had waited a year before going to college." },
        { week: 3, source: "Day012 교재3", ko: "이번 출장에 팀원들과 같이 갈 수 있으면 좋겠지만, 지금은 제가 업무가 너무 많습니다. 다음에 꼭 귀사의 시설에 방문하겠습니다. 그건 그렇고 Holtz 씨는 좀 어때요? 독감 걸리셨다는 이야기 들었어요.", en: "I wish I could join the team on the trip over there, but I’m afraid I have too much on my plate at the moment. I will definitely come and visit your facilities next time. By the way, how is Mr. Holtz holding up? I heard that he came down with the flu." },
        { week: 3, source: "Day012 교재4", ko: "실력이 크게 늘지 않는 기분입니다.", en: "I feel like I am not making much progress." },
        { week: 3, source: "Day012 교재4", ko: "올해 겨울은 평소보다 훨씬 더 추운 것 같아.", en: "It feels like winter this year is much colder than usual." },
        { week: 3, source: "Day012 교재4", ko: "아주 오래전 일처럼 느껴지세요?", en: "Does it feel like a long time ago?" },
        { week: 3, source: "Day012 교재4", ko: "제가 직접 못 가서 아쉽습니다.", en: "It’s a shame that I can’t be there in person." },
        { week: 3, source: "Day012 교재4", ko: "너무 일찍 일어나게 돼서 아쉬워.", en: "It’s a shame we have to leave so early." },
        { week: 3, source: "Day012 대표", ko: "나도 그렇게 돈이 많으면 좋으련만.", en: "I wish I had that much money." },
        { week: 3, source: "Day013 교재1", ko: "요리하고 싶지 않아. 프라이드치킨 먹는 건 어때?", en: "I don’t feel like cooking. How does fried chicken sound?" },
        { week: 3, source: "Day013 교재1", ko: "오늘 저녁에는 인도 음식 먹을까 싶은데, 어때?", en: "I was thinking about having Indian food tonight. How does that sound?" },
        { week: 3, source: "Day013 교재1", ko: "월요일 월차 못 내면 그냥 경기도 가서 휴가 보내야 할 듯해. 어때?", en: "If I can’t take Monday off, maybe we could just vacation in Gyeonggi-do. How does that sound?" },
        { week: 3, source: "Day013 교재1", ko: "다음 주는 줌에서 만나면 어떨까 하는데요. 어떠세요?", en: "I thought maybe we could meet on Zoom next week. How does that sound?" },
        { week: 3, source: "Day013 교재1", ko: "다음 주 크리스마스 때는 가게 문 닫을까 하는 데. 당신 생각은 어때", en: "I was thinking of closing the store next week for Christmas. How does that sound to you?" },
        { week: 3, source: "Day013 교재2", ko: "죄송한데 조개가 다 떨어졌어요. 그래도 주방장님이 특별 홍합요리를 만들어 드릴 수 있습니다. 어떠세요?", en: "I’m afraid we’re out of clams, but the chef can cook a special mussel dish, instead. How does that sound?" },
        { week: 3, source: "Day013 교재2", ko: "좋죠! 사실 홍합이 더 좋아요.", en: "That would be great! I actually prefer mussels." },
        { week: 3, source: "Day013 교재2", ko: "야간 비행기는 이백 달러 정도 싸대. 어때?", en: "The overnight flight is about $200 cheaper. How does that sound?" },
        { week: 3, source: "Day013 교재2", ko: "하아. 난 밤 비행기 못 타. 더 이상 이삼십 대가 아니잖아.", en: "Ugh. I can’t stand overnight flights. I’m not in my 20s or 30s anymore." },
        { week: 3, source: "Day013 교재2", ko: "그 호텔이 우리 예약을 혼동했어. 도심이 보이는 방밖에 안 남았다는데, 그 방 선택하면 저녁은 공짜로 주겠대. 어떻게 생각해?", en: "The hotel mixed up our reservation. They only have city view rooms left, but if we take one, they’re willing to throw in a free dinner. How does that sound?" },
        { week: 3, source: "Day013 교재2", ko: "좋아. 난 어차피 전망은 크게 상관없거든.", en: "That sounds nice. I don’t care much about the view anyway." },
        { week: 3, source: "Day013 교재3", ko: "안녕하세요. 당근마켓에 올리신 TV에 관심 있습니다. 삼십만 원을 원하시는 것 같은데, 혹시 이십오만 원은 어때요? 제가 그쪽으로 가서 제 차로 직접 픽업해 올 수 있습니다.", en: "Hello. I’m interested in the TV you put up for sale on Daangn Market. I see you would like 300,000 won, but how does 250,000 won sound? I can come over and pick it up myself with my van." },
        { week: 3, source: "Day013 교재4", ko: "그거 좋은 생각이다.", en: "That sounds good." },
        { week: 3, source: "Day013 교재4", ko: "그 사람 엄청 들뜬 것 같더구요.", en: "He sounded very excited." },
        { week: 3, source: "Day013 교재4", ko: "별로 좋은 생각 같지는 않은데요.", en: "That dosen't sound like a good idea." },
        { week: 3, source: "Day013 교재4", ko: "발리로 5일간 여행 가는데 30만 원밖에 안 한다고? 내가 보기엔 사기 같은데.", en: "Only 300,000 won for five days in Bali? That sounds like a scam to me." },
        { week: 3, source: "Day013 교재4", ko: "저 여성분 호주 사람 같나요?", en: "Does she sound like she’s Australian?" },
        { week: 3, source: "Day013 교재4", ko: "정말로 삼촌 집에 3주나 있었던 거야? 삼촌이랑 사이가 엄청 좋은가 보네.", en: "I can’t believe you stayed at his place for three weeks. It sounds like you’ve got a really great relationship with your uncle." },
        { week: 3, source: "Day013 대표", ko: "2시 30분 어때요?", en: "How does 2:30 sound?" },
        { week: 3, source: "Day014 교재1", ko: "이번에 면접 봤는데 뭔가 이상했어요.", en: "There was something weird about the interview." },
        { week: 3, source: "Day014 교재1", ko: "이 브랜드에 사람들이 열광하는 이유가 있지.", en: "There is something about this brand people are crazy about." },
        { week: 3, source: "Day014 교재1", ko: "그 사람에게는 뭔가 끌리는 점이 있어요.", en: "There is something about him that I am attracted to." },
        { week: 3, source: "Day014 교재1", ko: "유재석은 뭔가 사람을 편하게 해 주는 게 있어.", en: "There is something about Yu Jae-seok that puts people at ease." },
        { week: 3, source: "Day014 교재1", ko: "그 코치에게는 선수들의 잠재력을 이끌어내는 뭔가가 있어.", en: "There is something about the coach that brings out the best in players." },
        { week: 3, source: "Day014 교재1", ko: "그 식당에는 뭔가가 있길래 이렇게 인기가 많겠지.", en: "There must be something about that restaurant that makes it so popular." },
        { week: 3, source: "Day014 교재1", ko: "그 사람에게는 뭔가 차별 포인트가 있을 거야.", en: "There must be something special about him that sets him apart." },
        { week: 3, source: "Day014 교재2", ko: "요즘 차 알아보고 있는데, 포르쉐에는 거부할 수 없는 뭔가가 있어.", en: "I’ve been shopping around for a car, and there’s something about Porsches that I can’t resist." },
        { week: 3, source: "Day014 교재2", ko: "스포츠카에 그렇게 큰돈을 쓸 생각을 하다니. 돈 모으고 투자해서 빨리 은퇴해야지.", en: "I can’t believe you’re thinking about spending so much money on a sports car. You should just save, invest, and retire early." },
        { week: 3, source: "Day014 교재2", ko: "터치스크린이 딸린 기기에는 익숙하지 않은 것 같네, 그렇지?", en: "It looks like you’re not used to devices with a touch screen, right?" },
        { week: 3, source: "Day014 교재2", ko: "응. 실물 마우스랑 키보드가 뭔가 더 편해.", en: "Yes. There’s just something about a real mouse and keyboard that makes me more comfortable." },
        { week: 3, source: "Day014 교재2", ko: "그 사람은 뭔가 달라. 내가 만난 다른 남자들이랑 달라.", en: "There is something different about him. He is not like other guys I have met." },
        { week: 3, source: "Day014 교재2", ko: "그렇긴 하지만 이제 겨우 한 번 만난 거잖아. 좀 천천히 시간을 가진 다음에 공식적으로 만나.", en: "Sure, but it was only one date. Give it more time before you become official." },
        { week: 3, source: "Day014 교재3", ko: "남성분들은 이해를 못 합니다. 저희 차가 너무 비싸다거나 너무 작다거나 장거리 운행에는 별로라는 말을 합니다. 미니 쿠퍼에는 여성들이 거부할 수 없는 무언가가 있습니다. 누가 봐도 귀여운 건 당연하고, 운전의 재미도 있으며 좁은 공간에서 주차하기도 더 편합니다.", en: "Guys just don’t get it. They say our cars are overpriced, or too small, or impractical for long distances. There is something about Mini Coopers that women find irresistible. And besides obviously looking cute, our cars are fun to drive, and also easier to park in cramped spaces." },
        { week: 3, source: "Day014 교재4", ko: "저는 중고 물건 구매하는 데 거부감이 없어요.", en: "I don’t mind buying something second-hand." },
        { week: 3, source: "Day014 교재4", ko: "나한테 비싼 거 안 사 줘도 돼.", en: "You don’t have to get me anything expensive." },
        { week: 3, source: "Day014 교재4", ko: "이 TV에는 최신 기능이 다 들어 있습니다. 가격은 오백만 원이고요.", en: "This TV has all the latest features. It costs five million won." },
        { week: 3, source: "Day014 교재4", ko: "음, 저는 딱 기본적인 기능만 있으면 됩니다. 조금 더 저렴 거 있을까요?", en: "Well, I really just want something basic. Do you have anything a bit cheaper?" },
        { week: 3, source: "Day014 대표", ko: "BTS는 뭔가 좀 달라.", en: "There is something different about BTS." },
        { week: 3, source: "Day015 교재1", ko: "이 스쾃 기구 다 쓰신 거죠? 제가 써도 될까요?", en: "Are you done with this squat rack? Is it alright if I use it?" },
        { week: 3, source: "Day015 교재1", ko: "샌드위치 그만 먹을래. 너무 커.", en: "I think I’m done with my sandwich. It’s just way too big." },
        { week: 3, source: "Day015 교재1", ko: "제가 빌려준 책 다 읽은 거죠? 그럼 돌려주세요.", en: "Are you done with the book I lent you? I’d like to have it back." },
        { week: 3, source: "Day015 교재1", ko: "차량 점검 마쳤습니다. 어디가 고장인지 말씀드릴게요.", en: "I’m done taking a look at your car. I’ll tell you what you’ve got here." },
        { week: 3, source: "Day015 교재1", ko: "들어오지 마! 나 아직 옷 덜 갈아입었다고.", en: "Don’t come in! I’m not done changing." },
        { week: 3, source: "Day015 교재2", ko: "컴퓨터는 다 쓴 거야? 내 회사 이메일 확인해야 하는데.", en: "Are you done with the computer? I need to check my work emails." },
        { week: 3, source: "Day015 교재2", ko: "근데 이 게임 너무 재미있어. 당신 전화기로 확인하면 안 돼?", en: "I’m really into this game, though. Can’t you just check them on your phone?" },
        { week: 3, source: "Day015 교재2", ko: "옷은 다 입어 본 거야? 여기 너한테 어울리는 옷이 없는 것 같아.", en: "Are you done trying on clothes? I don’t think anything here really suits you." },
        { week: 3, source: "Day015 교재2", ko: "응, 거의 다 입어 봤어. 잠깐만! 이 스커트 너무 귀엽다!", en: "Yeah, just about. Wait! Look at this cute skirt!" },
        { week: 3, source: "Day015 교재2", ko: "신규 환자분들이 작성해야 하는 양식 세 장 여기 있습니다. 다 작성하시면 말해 주세요.", en: "Here are three forms we ask all new patients to fill out. Please let me know when you are done." },
        { week: 3, source: "Day015 교재2", ko: "네. 근데 펜이 있을까요?", en: "Alright. Oh, do you have a pen I could use?" },
        { week: 3, source: "Day015 교재3", ko: "기구를 다 쓴 뒤에는 꼭 제자리에 놔두실 것을 당부드립니다. 아무렇게나 놔두면 다른 사람들이 찾을 수가 없습니다. 1회 위반자는 경고 조치하며 2회 위반자는 일주일간 헬스장 출입이 금지됩니다. 모두가 즐길 수 있는 헬스장을 만드는 데 협조해 주시면 감사하겠습니다.", en: "Please make sure to put away the weights when you’re done. If you leave them out, others can’t find what they need. Violators will be warned the first time, and then suspended for a week the second time. We appreciate your cooperation in making the gym a place everyone can enjoy." },
        { week: 3, source: "Day015 교재4", ko: "Jack한테 문제가 있는 건 아니야. 내가 연애하는 게 지겨워진 것 같아.", en: "There’s not anything really wrong with Jack. Maybe I’m just done with dating." },
        { week: 3, source: "Day015 교재4", ko: "두 번째 폭력 장면이 나왔을 때, 더 이상 영화를 못 보겠더라고. 나와서 환불 요구했잖아.", en: "When a second fight scene came up, I was done with the movie. I walked out and asked for a refund." },
        { week: 3, source: "Day015 교재4", ko: "더 이상은 카카오톡 안 쓰려고. 라인이나 왓츠앱으로 갈아타야 할 때야.", en: "I think I’m done with KakaoTalk. It’s time for me to switch to Line or WhatsApp." },
        { week: 3, source: "Day015 교재4", ko: "서울 생활이 이젠 너무 지친다. 물가도 비싸고, 사람도 너무 많고, 경쟁도 너무 심해.", en: "I’m done with living in Seoul. It’s too expensive, too crowded and there’s too much competition." },
        { week: 3, source: "Day015 대표", ko: "다 먹은 거니?", en: "Are you done with your plate?" },

        // Week 4
        { week: 4, source: "Day016 교재1", ko: "이 옷 너한테 잘 어울린다.", en: "This outfit looks good on you." },
        { week: 4, source: "Day016 교재1", ko: "제가 목이 짧아서 터틀넥은 저한테 안 어울려요.", en: "Turtlenecks never look good on me because my neck is too short." },
        { week: 4, source: "Day016 교재1", ko: "안경이 비싸다고 잘 어울리는 건 아닙니다.", en: "Just because glasses are expensive, that doesn’t mean they would look good on you." },
        { week: 4, source: "Day016 교재1", ko: "처음 이 모자를 봤을 때 남자 친구가 쓰면 잘 어울리겠다고 생각했어요.", en: "When I first looked at this hat, I thought ‘That would look good on my boyfriend.’" },
        { week: 4, source: "Day016 교재1", ko: "저 여자는 어떻게 저 옷을 소화할까? 저걸 내가 입으면 어울릴까?", en: "How does she pull that off? Would it look good on me?" },
        { week: 4, source: "Day016 교재2", ko: "안녕하세요. 모자를 보다가 이게 눈에 띄어서요. 할인 중인가요?", en: "Hi, there. I’m looking for a new hat, and this one caught my eye. Is it on sale?" },
        { week: 4, source: "Day016 교재2", ko: "아닙니다, 손님. 할인하는 제품은 아닌데, 잘 어울리시네요!", en: "No, sir. I’m afraid it isn’t, but it looks very good on you!" },
        { week: 4, source: "Day016 교재2", ko: "어떤 게 더 나아? 회색 카디건 아님 파란색?", en: "Which fits me better, the grey cardigan or the blue one?" },
        { week: 4, source: "Day016 교재2", ko: "둘 다 잘 어울려. 그냥 싼 걸로 사.", en: "They both look good on you. I say just go with whatever’s cheaper." },
        { week: 4, source: "Day016 교재2", ko: "이 옷 어때? 사람들이 그러는데 내 피부색이 너무 어두워서 이런 핑크색은 안 어울린대.", en: "How does this dress look? I’ve been told that my skin is too dark for pink stuff like this." },
        { week: 4, source: "Day016 교재2", ko: "누가 그래? 너 핑크 엄청 잘 어울려.", en: "Who told you that? You look great in pink." },
        { week: 4, source: "Day016 교재3", ko: "저희 울 스카프에 관심 가져 주셔서 감사합니다. 하지만 제가 프로필 사진을 봤는데, 고객님 피부 톤이랑은 안맞을 것 같습니다. 좀 더 잘 어울릴 만한 스카프가 하나 더 있습니다. 6월 15일에 포스팅한 것 확인해 보시면 됩니다.", en: "I appreciate your interest in my wool scarf. However, I looked at your profile picture, and I’m afraid it probably wouldn’t look good on you with your skin tone. I have another scarf that might suit you better. You can check it out in my post from June 15th." },
        { week: 4, source: "Day016 교재4", ko: "검은색은 이 갈색 바지랑 안 어울릴 텐데.", en: "Black wouldn’t work with these brown pants." },
        { week: 4, source: "Day016 교재4", ko: "손이 예쁘셔서, 클러치 백이 더 잘 어울릴 듯합니다.", en: "With your cute hands, I think a clutch would suit you better." },
        { week: 4, source: "Day016 교재4", ko: "그 셔츠 입으니 네 어깨가 사는구나.", en: "That shirt really compliments your shoulders." },
        { week: 4, source: "Day016 대표", ko: "이 티셔츠 너한테 잘 어울려.", en: "This t-shirt looks good on you." },
        { week: 4, source: "Day017 교재1", ko: "사실 화요일이 더 좋습니다.", en: "Tuesday works better for me, actually." },
        { week: 4, source: "Day017 교재1", ko: "수요일 괜찮은가요?", en: "Does Wed work for you?" },
        { week: 4, source: "Day017 교재1", ko: "제안 주신 날짜가 저희랑은 하나도 안 맞습니다.", en: "None of the dates you proposed work for us." },
        { week: 4, source: "Day017 교재1", ko: "1시 이후에는 다 좋습니다.", en: "Anytime after 1 would work for me." },
        { week: 4, source: "Day017 교재1", ko: "일요일은 안 되지만, 토요일은 하루 종일 가능합니다.", en: "Sunday doesn’t work for me, but I am available all day Saturday." },
        { week: 4, source: "Day017 교재2", ko: "안녕, Mark. 반가워. 나는 월요일은 저녁 식사 무조건 가능해. 어때?", en: "Hey, Mark. It’s nice to hear from you. I’m down for dinner on Monday. What do you think?" },
        { week: 4, source: "Day017 교재2", ko: "아, 월요일은 약속이 있어. 화요일이 더 나은데. 괜찮아?", en: "Ah, I already have plans for Monday. Tuesday works better for me. Would that be alright?" },
        { week: 4, source: "Day017 교재2", ko: "최 선생님, 일요일 수업에 참석 못 할 것 같아요. 다른 날에 해도 될까요?", en: "Mr. Choi, I’m afraid I won’t be able to attend our class on Sunday. Could we meet another day?" },
        { week: 4, source: "Day017 교재2", ko: "저는 월요일부터 수요일까지 오후 시간은 다 괜찮아요. 어떤 요일이 제일 좋으세요?", en: "I’m actually free every afternoon from Monday to Wednesday. What day works best for you?" },
        { week: 4, source: "Day017 교재3", ko: "친애하는 Johnson 씨에게,\n계약 갱신 논의를 위해 제안하신 시간들을 쭉 한번 봤습니다. 안타깝게도 제안하신 날짜에는 저희가 안 됩니다. 저희가 다른 가능한 날짜들을 취합해서 첨부했습니다. 적어도 이 중 하루가 가능하기를 바랍니다.", en: "Dear Mr. Johnson,\nWe’ve looked over the proposed times for discussing contract renewal, and I’m afraid that none of the dates you suggested would work for us. Attached is a list of alternative dates we have put together. I hope that at least one of them is acceptable." },
        { week: 4, source: "Day017 교재4", ko: "스케일링 예약을 하려고 합니다.", en: "I’d like to schedule a teeth cleaning." },
        { week: 4, source: "Day017 교재4", ko: "수요일에 회의가 다섯 개나 잡혀 있습니다.", en: "I have five meetings scheduled for Wednesday." },
        { week: 4, source: "Day017 교재4", ko: "다음 주로 일정을 변경하는 건 어떨까요?", en: "How about if we reschedule for next week?" },
        { week: 4, source: "Day017 교재4", ko: "오전 10시 이전에는 시간이 다 됩니다.", en: "I am available any time before 10 a.m." },
        { week: 4, source: "Day017 교재4", ko: "5시에 회의하면 다른 일정과 겹칠 것 같습니다.", en: "I’m afraid that having the meeting at 5 would conflict with another appointment." },
        { week: 4, source: "Day017 대표", ko: "화요일 시간 괜찮으세요?", en: "Does Tuesday work for you?" },
        { week: 4, source: "Day018 교재1", ko: "돈 이야기가 나왔으니 말인데, 너한테 십만 원 갚을 거 있어.", en: "Speaking of money, I owe you 100,000 won." },
        { week: 4, source: "Day018 교재1", ko: "날씨 이야기가 나왔으니 말인데, 이번 가을은 유난히 따뜻했어. 그렇지?", en: "Speaking of the weather, this autumn was unusually warm, wasn’t it?" },
        { week: 4, source: "Day018 교재1", ko: "장보는 이야기가 나왔으니 말인데, 신촌역 부근에 대형 마트가 생겼다는 거 들었어요?", en: "Speaking of grocery shopping, did you hear that there’s this new megastore near Shinchon Station?" },
        { week: 4, source: "Day018 교재1", ko: "아, Teri 이야기가 나왔으니 말인데, 어떻게 지냈대? 새 아파트는 구했대?", en: "Oh, speaking of Teri, how has she been? Has she found a new apartment?" },
        { week: 4, source: "Day018 교재2", ko: "내가 교수님에게 상황을 다 설명했더니, 기말시험 하루 늦게 보게 해 주시는 데 동의하셨어.", en: "So, after explaining everything to the professor, he agreed to let me take the final a day late." },
        { week: 4, source: "Day018 교재2", ko: "이야기가 잘 돼서 다행이다. 근데, 스케줄 이야기가 나왔으니 말인데, 오늘 저녁 이탈리아 음식점 예약은 한 거야?", en: "I’m glad things worked out for you. By the way, speaking of scheduling, did you make a reservation at the Italian place for dinner tonight?" },
        { week: 4, source: "Day018 교재2", ko: "오늘 Karen 옷 입은 거 봤어? 회사에서 입기엔 좀 그렇지 않아?", en: "Did you see what Karen is wearing today? Is it really appropriate for the office?" },
        { week: 4, source: "Day018 교재2", ko: "맞아. 너무 야해. 근데 Karen 이야기가 나왔으니 말인데, 다음 주 목요일에 그 친구 생일이야.", en: "Right. It’s a little too revealing. By the way, speaking of Karen, her birthday is coming up next Thursday." },
        { week: 4, source: "Day018 교재3", ko: "나는 영화 보러 못 가. 야근해야 해.", en: "I can’t make it to the movie. I have to work late." },
        { week: 4, source: "Day018 교재3", ko: "괜찮아. 이해해.", en: "Alright. We understand." },
        { week: 4, source: "Day018 교재3", ko: "일 이야기가 나왔으니 말인데, Kate라는 그 여자분과 여전히 같이 일하고 있어? 그 분이랑 다시 얼굴 보면 좋겠다고 생각했거든.", en: "Speaking of work, though, do you still work with that woman named Kate? I was just thinking that it would be nice to see her again, too." },
        { week: 4, source: "Day018 교재4", ko: "그 여성분이 마스크를 안 쓰고 있었어요.", en: "She didn’t have a mask on." },
        { week: 4, source: "Day018 교재4", ko: "Jeff가 헤드폰을 끼고 걸어가고 있는 걸 봤어.", en: "I saw Jeff walking down the street with headphones on." },
        { week: 4, source: "Day018 교재4", ko: "Derek이 핼러윈 복장으로 나타났더군.", en: "Derek showed up in a Halloween costume." },
        { week: 4, source: "Day018 대표", ko: "말이 나왔으니 말인데, 어젯밤에 너랑 Nicole 사이에 무슨 일이 있었던 거야?", en: "Speaking of which, what happened with you and Nicole last night?" },
        { week: 4, source: "Day019 교재1", ko: "진짜 당분간 일 좀 쉬어야 해.", en: "I think you really need to take some time off from work." },
        { week: 4, source: "Day019 교재1", ko: "John, 제가 오후 반차를 좀 내도 될까요? 편두통이 오는 것 같아서요.", en: "John, do you mind if I take the afternoon off? I think I’m getting a migraine." },
        { week: 4, source: "Day019 교재1", ko: "너 올해는 단 하루도 안 쉬었구나.", en: "You haven’t even taken a single day off this year." },
        { week: 4, source: "Day019 교재1", ko: "Shawna가 다음 주 초에는 출근을 안 합니다. 간단한 수술 후에 3일 휴가를 쓸 예정이라서요.", en: "Shawna won’t be here at the beginning of next week. She’s taking three days off to recover after minor surgery." },
        { week: 4, source: "Day019 교재1", ko: "아산까지 가서 면접을 봅니다. 오후를 통째로 휴가를 내야 할 것 같아요.", en: "The interview is all the way in Asan. I’ll have to take the whole afternoon off" },
        { week: 4, source: "Day019 교재2", ko: "Hutchinson 씨, 제가 어제부터 기침이 좀 나고 미열이 있습니다. 코로나 자가 진단 검사를 해 보니 음성이 나오긴 했는데요. 그래도 내일은 하루 쉴까 합니다.", en: "Ms. Hutchinson, I’ve had a bit of a cough since yesterday and a slight fever. I took an at-home Covid test, which turned out negative, but I was still thinking of taking tomorrow off." },
        { week: 4, source: "Day019 교재2", ko: "안녕하세요, Steve. 괜찮습니다. 미리 알려줘서 고마워요.", en: "Hi, Steve. That will be fine. Thank you for letting me know in advance." },
        { week: 4, source: "Day019 교재2", ko: "나 커피 좀 더 마셔야 할 것 같아. 자꾸 졸려서. 아기 본다고 계속 바빴거든.", en: "I think I’ll need even more coffee. I can barely stay awake. I’ve been so busy taking care of the baby." },
        { week: 4, source: "Day019 교재2", ko: "그럴 만도 하지. 당분간 좀 쉬어야겠다.", en: "That makes sense. Maybe you should take some time off." },
        { week: 4, source: "Day019 교재2", ko: "우리 회사에서는 매년 한 달여간 유급휴가를 쓸 수 있어.", en: "I can take over a month of paid time off each year in my job." },
        { week: 4, source: "Day019 교재2", ko: "우와. 혹시 너희 회사에 자리 있을까?", en: "Wow. Does your company have any openings?" },
        { week: 4, source: "Day019 교재3", ko: "단골손님에게 알림 - 해럴드 카페가 다음 주 문을 닫습니다. 갑작스레 개인사가 생겨서 쉬게 되었습니다. 불편하게 해 드려 죄송합니다. 6일 재오픈 예정이며, 저희 인스타에서 확인해 주세요. 추가 업데이트는 인스타에서 공지하겠습니다.", en: "Attention loyal customers ‒ Harold’s Cafe will be closed next week. We are taking some time off to deal with a family emergency. We apologize for the inconvenience. We plan to reopen on the 6th, but please keep an eye on our Instagram account. More updates will be posted there." },
        { week: 4, source: "Day019 교재4", ko: "한동안 계속 쉬지 못했어.", en: "It’s been a while since I took some time off." },
        { week: 4, source: "Day019 교재4", ko: "나 당분간 진짜 좀 쉬어야겠어.", en: "I feel like I could really use some time off." },
        { week: 4, source: "Day019 대표", ko: "나 내일 쉬어.", en: "I’m taking tomorrow off." },
        { week: 4, source: "Day020 교재1", ko: "워크숍 준비하느라 바쁩니다.", en: "I am busy getting ready for the workshop." },
        { week: 4, source: "Day020 교재1", ko: "공부하느라 요새 무척 바쁩니다.", en: "I’ve been busy with my studies." },
        { week: 4, source: "Day020 교재1", ko: "제가 곧 이사해서 요즘 엄청 바쁩니다.", en: "I’ve been super busy with my upcoming move." },
        { week: 4, source: "Day020 교재1", ko: "나 행정 업무 하느라 무지 바빠!", en: "I am busy with all this admin work!" },
        { week: 4, source: "Day020 교재1", ko: "학교 다닐 때 과제다, 학원이다, 방과 후 활동이다 해서 잠시도 저희를 가만두지 않았죠.", en: "When I was in school, they would always keep us busy with homework, academies, and after-school activities." },
        { week: 4, source: "Day020 교재2", ko: "Daniel은 늘 피곤하다고 해.", en: "Daniel is always saying that he is tired." },
        { week: 4, source: "Day020 교재2", ko: "음, 이해돼. 동시에 책 두 권을 작업하느라 많이 바쁘니까.", en: "Well, that makes sense. He’s so busy working on two books at the same time." },
        { week: 4, source: "Day020 교재2", ko: "안녕하세요, Samantha. 어떻게 지내는지 안부 궁금해서 연락드려요.", en: "Hi, Samantha. I’m just checking in to see if you’re doing okay." },
        { week: 4, source: "Day020 교재2", ko: "안녕하세요. 다음 달 있을 워크숍 준비 때문에 정신이 없네요.", en: "Good afternoon. I’ve actually been really busy working on arrangements for next month’s workshop." },
        { week: 4, source: "Day020 교재2", ko: "어떻게 지냈어, Julie? 문자 보냈는데 답도 없더라", en: "How are you, Julie? You know, you never texted me back." },
        { week: 4, source: "Day020 교재2", ko: "미안해. 일 때문에 너무 바빴어.", en: "I’m sorry. I’ve just been so busy with work." },
        { week: 4, source: "Day020 교재3", ko: "안녕하세요, Steve 씨. 출장 준비로 바쁘신 줄 압니다. 금요일에 요청한 인보이스를 아직 안 보내 주셨다는 걸 알려드리려고요. 최대한 빨리 처리 부탁드려도 될까요?", en: "Hi, Steve. How are you? I understand that you must be busy getting ready for the business trip. I just wanted to remind you, though, that you still haven’t sent us the invoice that we requested on Friday. Could you please take care of it at your earliest convenience?" },
        { week: 4, source: "Day020 교재4", ko: "제 아들은 주말 동안 숙제로 바빠요.", en: "My son is busy with his homework during the weekend." },
        { week: 4, source: "Day020 교재4", ko: "오늘 아침에 무슨 일로 바쁘세요?", en: "What are you busy with this morning?" },
        { week: 4, source: "Day020 교재4", ko: "이번 연휴 동안 바빴어.", en: "I had a busy holiday." },
        { week: 4, source: "Day020 교재4", ko: "요새 뭐 때문에 그렇게 바쁜 거야?", en: "What’s keeping you so busy these days?" },
        { week: 4, source: "Day020 대표", ko: "제가 논문 쓰느라 바쁩니다.", en: "I’m busy working on my dissertation." }
    ];

    // 영어회화 난이도 분리
    const convEasy = rawConvData.filter(item => item.source.includes('대표') || item.source.includes('교재1'));
    const convHard = rawConvData.filter(item => !(item.source.includes('대표') || item.source.includes('교재1')));

    // 퀴즈 진행 상태 변수
    let currentQuestions = [];
    let currentIndex = 0;
    let isPhrasalMode = false;
    let starredQuestions = []; 

    function shuffleArray(array) {
        let shuffled = [...array];
        for (let i = shuffled.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
        }
        return shuffled;
    }

    // 선택된 주차에 맞게 필터링
    function filterByWeek(dataArray) {
        if (currentWeekFilter === 'all') {
            return dataArray;
        }
        return dataArray.filter(item => item.week === currentWeekFilter);
    }

    function startQuiz(mode) {
        let sourceArray = [];
        let titleMode = "";

        if (mode === 'phrasal-easy') { 
            sourceArray = phrasalEasy; isPhrasalMode = true; 
            titleMode = "🟢 구동사 순한맛 퀴즈"; 
        }
        else if (mode === 'phrasal-hard') { 
            sourceArray = phrasalHard; isPhrasalMode = true; 
            titleMode = "🔴 구동사 매운맛 퀴즈"; 
        }
        else if (mode === 'conv-easy') { 
            sourceArray = convEasy; isPhrasalMode = false; 
            titleMode = "💬 영어회화 순한맛 퀴즈"; 
        }
        else if (mode === 'conv-hard') { 
            sourceArray = convHard; isPhrasalMode = false; 
            titleMode = "🔥 영어회화 매운맛 퀴즈"; 
        }

        const filteredData = filterByWeek(sourceArray);

        if (filteredData.length === 0) {
            alert(`선택하신 주차(${currentWeekFilter === 'all' ? '누적' : 'Week ' + currentWeekFilter})에 해당하는 데이터가 아직 없습니다.`);
            return;
        }

        document.getElementById('mode-selection').classList.add('hidden');
        document.getElementById('quiz-area').classList.remove('hidden');
        
        let weekText = currentWeekFilter === 'all' ? " (1~4주차 누적)" : ` (Week ${currentWeekFilter})`;
        document.getElementById('main-title').innerText = titleMode + weekText; 
        
        starredQuestions = []; 
        // 7문제만 추출 (데이터가 7개 미만이면 전체 추출)
        const qCount = Math.min(filteredData.length, 7);
        currentQuestions = shuffleArray(filteredData).slice(0, qCount);
        currentIndex = 0;
        
        loadQuestion();
    }

    function loadQuestion() {
        document.getElementById('answer-section').classList.add('hidden');
        document.getElementById('btn-next').classList.add('hidden');
        document.getElementById('btn-finish').classList.add('hidden');
        document.getElementById('meaning-info').classList.add('hidden');
        
        const koBox = document.getElementById('ko-box');
        koBox.style.cursor = 'pointer';
        koBox.style.pointerEvents = 'auto';

        const q = currentQuestions[currentIndex];
        document.getElementById('question-counter').innerText = `문제 ${currentIndex + 1} / ${currentQuestions.length}`;
        document.getElementById('ko-text').innerText = q.ko;
        document.getElementById('en-text').innerText = q.en;
        document.getElementById('source-info').innerText = `출처: ${q.source}`;
        
        if(isPhrasalMode && q.meaning) {
            document.getElementById('meaning-info').innerText = q.meaning;
        }

        const starBtn = document.getElementById('btn-star');
        if (starredQuestions.includes(q)) {
            starBtn.classList.add('active');
            starBtn.innerText = "⭐ 어려워요 (저장됨)";
        } else {
            starBtn.classList.remove('active');
            starBtn.innerText = "⭐ 어려워요";
        }
    }

    function showAnswer() {
        const koBox = document.getElementById('ko-box');
        koBox.style.cursor = 'default';
        koBox.style.pointerEvents = 'none';

        document.getElementById('answer-section').classList.remove('hidden');
        
        if(isPhrasalMode && currentQuestions[currentIndex].meaning) {
            document.getElementById('meaning-info').classList.remove('hidden');
        }
        
        if (currentIndex < currentQuestions.length - 1) {
            document.getElementById('btn-next').classList.remove('hidden');
        } else {
            document.getElementById('btn-finish').classList.remove('hidden');
        }
    }

    function toggleStar() {
        const q = currentQuestions[currentIndex];
        const starBtn = document.getElementById('btn-star');
        const index = starredQuestions.indexOf(q);
        
        if (index > -1) {
            starredQuestions.splice(index, 1);
            starBtn.classList.remove('active');
            starBtn.innerText = "⭐ 어려워요";
        } else {
            starredQuestions.push(q);
            starBtn.classList.add('active');
            starBtn.innerText = "⭐ 어려워요 (저장됨)";
        }
    }

    function playTTS() {
        const textToSpeak = currentQuestions[currentIndex].en;
        if ('speechSynthesis' in window) {
            window.speechSynthesis.cancel();
            const utterance = new SpeechSynthesisUtterance(textToSpeak);
            utterance.lang = 'en-US';
            utterance.rate = 0.9; 
            window.speechSynthesis.speak(utterance);
        } else {
            alert('이 브라우저에서는 음성 듣기 기능을 지원하지 않습니다.');
        }
    }

    function nextQuestion() {
        currentIndex++;
        loadQuestion();
    }

    function showReview() {
        document.getElementById('quiz-area').classList.add('hidden');
        document.getElementById('review-area').classList.remove('hidden');
        document.getElementById('main-title').innerText = "결과 및 오답 노트";

        const reviewList = document.getElementById('review-list');
        reviewList.innerHTML = '';

        if (starredQuestions.length === 0) {
            reviewList.innerHTML = `<div class="review-empty">🎉 어려운 문장이 없습니다! 완벽해요! 🎉</div>`;
        } else {
            starredQuestions.forEach((q, idx) => {
                let meaningHtml = (isPhrasalMode && q.meaning) ? `<div style="font-size: 13px; color: #b45309; background: #fef3c7; padding: 4px 8px; border-radius: 4px; display: inline-block; margin-bottom: 5px;">${q.meaning}</div>` : '';
                
                reviewList.innerHTML += `
                    <div class="review-card">
                        <div style="font-size: 12px; color: #94a3b8; margin-bottom: 5px;">${idx + 1}. 출처: ${q.source}</div>
                        ${meaningHtml}
                        <div class="review-ko">${q.ko}</div>
                        <div class="review-en">${q.en}</div>
                    </div>
                `;
            });
        }
    }

    function resetToHome() {
        document.getElementById('review-area').classList.add('hidden');
        document.getElementById('mode-selection').classList.remove('hidden');
        document.getElementById('main-title').innerText = "🚀 스피드 영어 퀴즈";
    }
</script>

</body>
</html>
