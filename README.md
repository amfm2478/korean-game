# korean-game[korean_conversation_game (1).html](https://github.com/user-attachments/files/26162848/korean_conversation_game.1.html)
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>한국어 대화 연습 게임</title>
<link href="https://fonts.googleapis.com/css2?family=Jua&family=Nanum+Gothic:wght@400;700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --sky: #e0f4ff;
    --blue: #3b9edd;
    --deep: #1a5c8a;
    --yellow: #ffd94a;
    --green: #4ecb71;
    --orange: #ff8c42;
    --pink: #ff6b9d;
    --purple: #9b72cf;
    --red: #ff4d4d;
    --white: #ffffff;
    --card: #ffffffcc;
    --shadow: 0 4px 20px rgba(0,0,0,0.12);
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'Nanum Gothic', sans-serif;
    background: linear-gradient(160deg, #cce9ff 0%, #e8f8e8 50%, #fff3cc 100%);
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 8px 12px;
  }

  h1 {
    font-family: 'Jua', cursive;
    font-size: 1.5rem;
    color: var(--deep);
    text-align: center;
    margin-bottom: 2px;
    text-shadow: 1px 1px 0 #fff;
  }

  .subtitle {
    font-size: 0.8rem;
    color: #558;
    margin-bottom: 8px;
    text-align: center;
  }

  #situation-screen { width: 100%; max-width: 700px; }

  .situation-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 8px;
    margin-top: 4px;
  }

  .sit-card {
    background: var(--white);
    border-radius: 14px;
    padding: 10px 8px;
    text-align: center;
    cursor: pointer;
    box-shadow: var(--shadow);
    transition: transform 0.15s, box-shadow 0.15s;
    border: 2px solid transparent;
  }
  .sit-card:hover { transform: translateY(-3px); box-shadow: 0 6px 18px rgba(0,0,0,0.15); }
  .sit-card .icon { font-size: 1.8rem; display: block; margin-bottom: 4px; }
  .sit-card .label { font-family: 'Jua', cursive; font-size: 0.95rem; color: #333; }
  .sit-card .desc { font-size: 0.7rem; color: #777; margin-top: 2px; }

  .sit-card[data-color="blue"] { border-color: var(--blue); }
  .sit-card[data-color="green"] { border-color: var(--green); }
  .sit-card[data-color="orange"] { border-color: var(--orange); }
  .sit-card[data-color="purple"] { border-color: var(--purple); }
  .sit-card[data-color="pink"] { border-color: var(--pink); }
  .sit-card[data-color="yellow"] { border-color: #e5bb00; }

  #game-screen { display: none; width: 100%; max-width: 700px; }

  .game-header { display: flex; align-items: center; gap: 8px; margin-bottom: 8px; }

  .back-btn {
    background: var(--white); border: none; border-radius: 50%;
    width: 32px; height: 32px; font-size: 1rem; cursor: pointer;
    box-shadow: var(--shadow); display: flex; align-items: center; justify-content: center;
  }
  .back-btn:hover { background: #eee; }

  .game-title { font-family: 'Jua', cursive; font-size: 1.1rem; color: var(--deep); }

  .progress-bar-wrap { background: #ddd; border-radius: 999px; height: 7px; flex: 1; overflow: hidden; }
  .progress-bar { height: 100%; background: linear-gradient(90deg, var(--blue), var(--green)); border-radius: 999px; transition: width 0.5s; }

  .scene-box {
    background: var(--white); border-radius: 16px;
    padding: 8px 12px 8px; box-shadow: var(--shadow); margin-bottom: 8px;
  }

  .scene-illustration { font-size: 2.2rem; text-align: center; display: block; margin-bottom: 4px; animation: bounce 1.2s infinite alternate; }
  @keyframes bounce { from { transform: translateY(0); } to { transform: translateY(-5px); } }

  .npc-row { display: flex; align-items: flex-end; gap: 8px; margin-bottom: 0; }
  .npc-avatar { font-size: 1.8rem; flex-shrink: 0; animation: wiggle 2s infinite; }
  @keyframes wiggle { 0%,100%{transform:rotate(0deg)} 25%{transform:rotate(-5deg)} 75%{transform:rotate(5deg)} }

  .speech-bubble {
    background: var(--deep); color: #fff;
    border-radius: 12px 12px 12px 3px;
    padding: 8px 12px; font-size: 0.95rem; font-weight: 800; line-height: 1.4;
    box-shadow: 0 2px 8px rgba(27,92,138,0.3); max-width: 480px;
  }
  .speech-bubble .hint { font-size: 0.75rem; font-weight: 400; opacity: 0.8; margin-top: 2px; }

  .step-label { text-align: center; font-size: 0.75rem; color: #888; margin-bottom: 5px; font-weight: 700; }

  .choice-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 6px; margin-bottom: 8px; }

  .choice-btn {
    background: var(--white); border: 2px solid #ddd; border-radius: 10px;
    padding: 8px 4px; cursor: pointer; font-family: 'Nanum Gothic', sans-serif;
    font-size: 0.82rem; font-weight: 700; color: #333; transition: all 0.15s;
    display: flex; flex-direction: column; align-items: center; gap: 3px;
    box-shadow: 0 1px 5px rgba(0,0,0,0.07); text-align: center; width: 100%;
  }
  .choice-btn .choice-icon { font-size: 1.4rem; }
  .choice-btn .choice-label { font-size: 0.82rem; line-height: 1.2; }
  .choice-btn .choice-price { color: #e5aa00; font-size: 0.75rem; font-weight: 800; }
  .choice-btn:hover { transform: translateY(-2px); border-color: var(--blue); box-shadow: 0 4px 12px rgba(59,158,221,0.2); }
  .choice-btn.selected { border-color: var(--blue); background: #e8f4ff; }

  /* Model answer */
  .model-box {
    background: linear-gradient(135deg, #fff8e1, #fffde7);
    border: 2px dashed #e5bb00;
    border-radius: 12px;
    padding: 8px 14px;
    margin-bottom: 8px;
    display: none;
  }
  .model-label { font-size: 0.75rem; color: #a08000; font-weight: 700; margin-bottom: 3px; }
  .model-text { font-size: 1rem; font-weight: 800; color: #5a4200; line-height: 1.3; }

  /* Receipt box */
  .receipt-box { background: #fff9f0; border: 2px solid #e5bb00; border-radius: 12px; padding: 8px 12px; margin-bottom: 8px; display: none; font-size: 0.85rem; }
  .receipt-title { font-family: 'Jua', cursive; font-size: 0.9rem; color: #5a4200; text-align: center; margin-bottom: 6px; border-bottom: 1px dashed #e5bb00; padding-bottom: 5px; }
  .receipt-row { display: flex; justify-content: space-between; color: #5a4200; margin-bottom: 3px; font-weight: 700; }
  .receipt-total { border-top: 2px solid #e5bb00; margin-top: 5px; padding-top: 5px; font-size: 0.95rem; font-weight: 800; color: #c85000; }

  /* Voice repeat area */
  .repeat-area { display: none; flex-direction: column; align-items: center; gap: 8px; margin-bottom: 8px; }
  .repeat-label { font-size: 0.85rem; color: #555; font-weight: 700; text-align: center; }

  /* Mic button */
  .mic-btn {
    width: 64px; height: 64px; border-radius: 50%; border: none;
    background: linear-gradient(135deg, var(--blue), #1a7fc4);
    color: #fff; font-size: 1.8rem; cursor: pointer;
    box-shadow: 0 4px 16px rgba(59,158,221,0.45);
    transition: transform 0.15s; display: flex; align-items: center; justify-content: center; position: relative;
  }
  .mic-btn:hover { transform: scale(1.08); }
  .mic-btn.listening { background: linear-gradient(135deg, #ff4d4d, #cc1100); box-shadow: 0 4px 20px rgba(255,77,77,0.55); animation: pulse-mic 0.9s infinite; }
  @keyframes pulse-mic { 0%,100%{transform:scale(1)} 50%{transform:scale(1.1)} }
  .mic-btn.listening::before, .mic-btn.listening::after { content:''; position:absolute; border-radius:50%; border:2px solid rgba(255,77,77,0.5); animation:ring-expand 1.2s infinite; }
  .mic-btn.listening::before { width:80px; height:80px; animation-delay:0s; }
  .mic-btn.listening::after  { width:100px; height:100px; animation-delay:0.4s; }
  @keyframes ring-expand { 0%{transform:scale(0.8);opacity:1} 100%{transform:scale(1.4);opacity:0} }

  .voice-result-box {
    width: 100%; min-height: 40px; background: #f5f5ff; border: 2px solid #ccc;
    border-radius: 10px; padding: 8px 12px; font-size: 0.95rem; font-weight: 700;
    color: #333; text-align: center; transition: border-color 0.3s, background 0.3s;
  }
  .voice-result-box.listening { border-color: var(--red); background: #fff8f8; color: #999; }
  .voice-result-box.correct   { border-color: var(--green); background: #f0fff4; color: #1a7a40; }
  .voice-result-box.wrong     { border-color: var(--red); background: #fff0f0; color: #cc2200; }

  .mic-status { font-size: 0.78rem; color: #888; text-align: center; min-height: 18px; }
  .mic-status.active { color: var(--red); font-weight: 700; }

  /* Buttons */
  .btn-row { display: flex; gap: 8px; justify-content: center; margin-bottom: 6px; }
  .btn { padding: 8px 20px; border: none; border-radius: 10px; font-family: 'Jua', cursive; font-size: 0.95rem; cursor: pointer; transition: transform 0.1s, box-shadow 0.1s; box-shadow: 0 2px 8px rgba(0,0,0,0.12); }
  .btn:hover { transform: translateY(-2px); }
  .btn-primary { background: var(--blue); color: #fff; }
  .btn-success { background: var(--green); color: #fff; }
  .btn-secondary { background: #eee; color: #555; }
  .btn:disabled { opacity: 0.4; cursor: not-allowed; transform: none; }

  /* Feedback */
  .feedback { text-align: center; font-family: 'Jua', cursive; font-size: 1rem; padding: 6px; border-radius: 10px; display: none; animation: pop 0.3s ease; }
  @keyframes pop { from{transform:scale(0.8);opacity:0} to{transform:scale(1);opacity:1} }
  .feedback.ok { background: #e8fff0; color: #1a8a40; }
  .feedback.err { background: #fff0f0; color: #cc2200; }

  /* Completion screen */
  #complete-screen { display: none; text-align: center; width: 100%; max-width: 700px; }
  .complete-card { background: var(--white); border-radius: 20px; padding: 24px 20px; box-shadow: var(--shadow); }
  .star-row { font-size: 2rem; margin-bottom: 8px; }
  .complete-title { font-family: 'Jua', cursive; font-size: 1.5rem; color: var(--deep); margin-bottom: 6px; }
  .complete-msg { font-size: 0.95rem; color: #555; margin-bottom: 14px; }
  .score-badge { display: inline-block; background: linear-gradient(135deg, var(--yellow), #ffb300); color: #5a3800; font-family: 'Jua', cursive; font-size: 1.2rem; padding: 8px 24px; border-radius: 50px; margin-bottom: 16px; }

  /* TTS button */
  .tts-btn { background: none; border: none; font-size: 1rem; cursor: pointer; margin-left: 4px; vertical-align: middle; }
  .tts-btn:hover { transform: scale(1.2); }

  /* Confetti */
  .confetti-wrap { position: fixed; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 9999; }
  .confetti-piece { position: absolute; width: 10px; height: 10px; border-radius: 2px; animation: fall linear forwards; }
  @keyframes fall { 0%{transform:translateY(-20px) rotate(0deg);opacity:1} 100%{transform:translateY(100vh) rotate(720deg);opacity:0} }

  @media (max-width: 480px) {
    .situation-grid { grid-template-columns: repeat(2, 1fr); }
  }
</style>
</head>
<body>

<h1>🗣️ 한국어 대화 게임</h1>
<p class="subtitle">상황에 맞게 대화 연습을 해봐요! 💪</p>

<!-- SITUATION SELECTION -->
<div id="situation-screen">
  <div class="situation-grid">
    <div class="sit-card" data-color="blue" onclick="startGame('restaurant')">
      <span class="icon">🍽️</span>
      <div class="label">식당 주문</div>
      <div class="desc">음식을 주문해봐요</div>
    </div>
    <div class="sit-card" data-color="green" onclick="startGame('store')">
      <span class="icon">🛒</span>
      <div class="label">편의점 쇼핑</div>
      <div class="desc">물건을 사봐요</div>
    </div>
    <div class="sit-card" data-color="orange" onclick="startGame('introduce')">
      <span class="icon">👋</span>
      <div class="label">자기소개</div>
      <div class="desc">나를 소개해봐요</div>
    </div>
    <div class="sit-card" data-color="purple" onclick="startGame('bus')">
      <span class="icon">🚌</span>
      <div class="label">버스 / 지하철</div>
      <div class="desc">대중교통을 이용해봐요</div>
    </div>
    <div class="sit-card" data-color="pink" onclick="startGame('doctor')">
      <span class="icon">🏥</span>
      <div class="label">병원 접수</div>
      <div class="desc">병원에 가봐요</div>
    </div>
    <div class="sit-card" data-color="yellow" onclick="startGame('phone')">
      <span class="icon">📞</span>
      <div class="label">전화 통화</div>
      <div class="desc">전화를 해봐요</div>
    </div>
  </div>
</div>

<!-- GAME SCREEN -->
<div id="game-screen">
  <div class="game-header">
    <button class="back-btn" onclick="goBack()">←</button>
    <span class="game-title" id="game-title"></span>
    <div class="progress-bar-wrap">
      <div class="progress-bar" id="progress-bar" style="width:0%"></div>
    </div>
  </div>

  <div class="scene-box">
    <div class="npc-row">
      <span class="npc-avatar" id="npc-avatar"></span>
      <div class="speech-bubble">
        <span id="npc-text"></span>
        <button class="tts-btn" onclick="speakText(document.getElementById('npc-text').innerText)" title="듣기">🔊</button>
        <div class="hint" id="npc-hint"></div>
      </div>
    </div>
  </div>

  <!-- STEP 1: Choose -->
  <div id="step-choose">
    <div class="step-label" id="step-choose-label">👆 하나를 골라봐요!</div>
    <div class="choice-grid" id="choice-grid"></div>
  </div>

  <!-- STEP 2: Model answer -->
  <div class="model-box" id="model-box">
    <div class="model-label">📣 이렇게 말해봐요:</div>
    <div class="model-text" id="model-text"></div>
    <button class="tts-btn" onclick="speakText(document.getElementById('model-text').innerText)" title="듣기">🔊 들어보기</button>
  </div>

  <!-- Receipt -->
  <div class="receipt-box" id="receipt-box">
    <div class="receipt-title">🧾 영수증 <button class="tts-btn" onclick="speakText(buildReceiptSpeech())" title="영수증 내역 듣기">🔊</button></div>
    <div id="receipt-rows"></div>
  </div>

  <!-- STEP 3: Voice Repeat -->
  <div class="repeat-area" id="repeat-area">
    <div class="repeat-label">🎤 마이크를 누르고 큰 소리로 말해봐요!</div>
    <div class="voice-result-box" id="voice-result-box">여기에 말한 내용이 나타나요</div>
    <button class="mic-btn" id="mic-btn" onclick="toggleMic()">🎤</button>
    <div class="mic-status" id="mic-status">마이크 버튼을 눌러봐요 👆</div>
    <div class="btn-row">
      <button class="btn btn-secondary" onclick="skipStep()">건너뛰기</button>
    </div>
  </div>

  <div class="feedback" id="feedback"></div>

  <div class="btn-row" id="next-row" style="display:none; margin-top:10px;">
    <button class="btn btn-success" onclick="nextStep()">다음 ▶</button>
  </div>
</div>

<!-- COMPLETE SCREEN -->
<div id="complete-screen">
  <div class="complete-card">
    <div class="star-row">⭐⭐⭐</div>
    <div class="complete-title">완료! 정말 잘했어요! 🎉</div>
    <div class="complete-msg" id="complete-msg"></div>
    <div class="score-badge" id="score-badge"></div>
    <div class="btn-row">
      <button class="btn btn-primary" onclick="goBack()">다른 상황 도전하기 🚀</button>
    </div>
  </div>
</div>

<div class="confetti-wrap" id="confetti"></div>

<script>
// ─── SITUATION DATA ────────────────────────────────────────────────────────
const SITUATIONS = {
  restaurant: {
    title: "🍽️ 식당 주문",
    icon: "🍜",
    npcAvatar: "👨‍🍳",
    menuBoard: {
      title: "🍽️ 오늘의 메뉴",
      items: [
        { icon: "🍜", label: "김치찌개", price: 8000 },
        { icon: "🍱", label: "비빔밥", price: 9000 },
        { icon: "🍗", label: "치킨", price: 15000 },
        { icon: "🍕", label: "피자", price: 12000 },
      ]
    },
    drinkMenu: [
      { icon: "🧃", label: "주스", price: 2000 },
      { icon: "🥤", label: "콜라", price: 1500 },
      { icon: "🍵", label: "녹차", price: 1000 },
      { icon: "💧", label: "물", price: 500 },
    ],
    steps: [
      {
        npcText: "어서 오세요! 메뉴판 보시고 주문해 주세요!",
        hint: "가격표를 보고 먹고 싶은 음식을 골라봐요",
        choiceLabel: "👆 먹고 싶은 음식을 골라봐요!",
        choices: [
          { icon: "🍜", label: "김치찌개", price: 8000 },
          { icon: "🍱", label: "비빔밥", price: 9000 },
          { icon: "🍗", label: "치킨", price: 15000 },
          { icon: "🍕", label: "피자", price: 12000 },
        ],
        modelFn: (c) => `${c.label} 1개 주세요.`,
        onSelect: (c, state) => { state.foodOrder = c; }
      },
      {
        npcText: "음료는 무엇으로 드릴까요?",
        hint: "마시고 싶은 음료를 골라봐요",
        choiceLabel: "👆 마시고 싶은 음료를 골라봐요!",
        choices: [
          { icon: "🧃", label: "주스", price: 2000 },
          { icon: "🥤", label: "콜라", price: 1500 },
          { icon: "🍵", label: "녹차", price: 1000 },
          { icon: "💧", label: "물", price: 500 },
        ],
        modelFn: (c) => `${c.label} 주세요.`,
        onSelect: (c, state) => { state.drinkOrder = c; }
      },
      {
        npcText: "잠시만요, 주문 확인할게요!",
        hint: "주문 내역이 맞는지 확인해봐요",
        choiceLabel: "👆 주문 내역이 맞나요?",
        showReceipt: true,
        choices: [
          { icon: "✅", label: "네, 맞아요" },
          { icon: "❌", label: "아니요, 달라요" },
        ],
        modelFn: (c) => c.label === "네, 맞아요"
          ? "네, 맞아요."
          : "아니요, 제가 주문한 것과 달라요."
      },
      {
        npcText: "드시고 가시나요? 포장인가요?",
        hint: "먹을 방법을 골라봐요",
        choiceLabel: "👆 어떻게 드실 건가요?",
        choices: [
          { icon: "🍽️", label: "먹고 갈게요" },
          { icon: "🛍️", label: "포장해 주세요" },
        ],
        modelFn: (c) => c.label === "먹고 갈게요"
          ? "먹고 갈게요."
          : "포장해 주세요."
      },
      {
        npcText: "결제는 어떻게 해드릴까요?",
        hint: "계산 방법을 골라봐요",
        choiceLabel: "👆 결제 방법을 골라봐요!",
        choices: [
          { icon: "💳", label: "카드로 낼게요" },
          { icon: "💵", label: "현금으로 낼게요" },
          { icon: "📱", label: "휴대폰으로 낼게요" },
          { icon: "🎁", label: "상품권으로 낼게요" },
        ],
        modelFn: (c) => `${c.label}.`
      },
      {
        npcText: "",
        hint: "거스름돈이 얼마인지 말해봐요",
        choiceLabel: "👆 낼 돈을 골라봐요!",
        isPaymentStep: true,
        choices: [
          { icon: "💵", label: "10,000원", amount: 10000 },
          { icon: "💵", label: "20,000원", amount: 20000 },
          { icon: "💵", label: "50,000원", amount: 50000 },
          { icon: "💵", label: "딱 맞게", amount: 0 },
        ],
        modelFn: (c, state) => {
          const total = (state.foodOrder?.price || 0) + (state.drinkOrder?.price || 0);
          if (c.amount === 0) return `딱 맞게 ${total.toLocaleString()}원 드릴게요.`;
          const change = c.amount - total;
          if (change < 0) return `돈이 부족해요. 다른 걸로 낼게요.`;
          return `${c.label} 드릴게요. 거스름돈은 ${change.toLocaleString()}원이에요.`;
        }
      }
    ]
  },

  store: {
    title: "🛒 편의점 쇼핑",
    icon: "🏪",
    npcAvatar: "🧑‍💼",
    menuBoard: {
      title: "🏪 편의점 가격표",
      items: [
        { icon: "🍫", label: "초콜릿", price: 1500 },
        { icon: "🥤", label: "음료수", price: 2000 },
        { icon: "🍜", label: "컵라면", price: 1200 },
        { icon: "🍞", label: "빵", price: 1800 },
      ]
    },
    steps: [
      {
        npcText: "어서 오세요! 가격표 보시고 골라봐요!",
        hint: "사고 싶은 것을 골라봐요",
        choiceLabel: "👆 사고 싶은 것을 골라봐요!",
        showMenu: 'store',
        choices: [
          { icon: "🍫", label: "초콜릿", price: 1500 },
          { icon: "🥤", label: "음료수", price: 2000 },
          { icon: "🍜", label: "컵라면", price: 1200 },
          { icon: "🍞", label: "빵", price: 1800 },
        ],
        modelFn: (c) => `${c.label} 있어요?`,
        onSelect: (c, state) => { state.storeItem = c; }
      },
      {
        npcText: "몇 개 드릴까요?",
        hint: "몇 개 살지 골라봐요",
        choiceLabel: "👆 몇 개 살지 골라봐요!",
        choices: [
          { icon: "1️⃣", label: "1개", count: 1 },
          { icon: "2️⃣", label: "2개", count: 2 },
          { icon: "3️⃣", label: "3개", count: 3 },
          { icon: "🔢", label: "5개", count: 5 },
        ],
        modelFn: (c) => `${c.label} 주세요.`,
        onSelect: (c, state) => { state.storeCount = c; }
      },
      {
        npcText: "잠시만요, 계산할게요!",
        hint: "총 금액을 확인해봐요",
        choiceLabel: "👆 계산 방법을 골라봐요!",
        showReceipt: true,
        choices: [
          { icon: "💳", label: "카드로 낼게요" },
          { icon: "💵", label: "현금으로 낼게요" },
          { icon: "📱", label: "페이로 낼게요" },
          { icon: "🚫", label: "봉투 필요 없어요" },
        ],
        modelFn: (c) => `${c.label}.`
      },
      {
        npcText: "",
        hint: "거스름돈이 얼마인지 말해봐요",
        choiceLabel: "👆 낼 돈을 골라봐요!",
        showPayment: true,
        isPaymentStep: true,
        choices: [
          { icon: "💵", label: "1,000원", amount: 1000 },
          { icon: "💵", label: "5,000원", amount: 5000 },
          { icon: "💵", label: "10,000원", amount: 10000 },
          { icon: "💵", label: "딱 맞게", amount: 0 },
        ],
        modelFn: (c, state) => {
          const item = state.storeItem;
          const cnt = state.storeCount?.count || 1;
          const total = (item?.price || 0) * cnt;
          if (c.amount === 0) return `딱 맞게 ${total.toLocaleString()}원 드릴게요.`;
          const change = c.amount - total;
          if (change < 0) return `돈이 부족해요. 다른 걸로 낼게요.`;
          return `${c.label} 드릴게요. 거스름돈은 ${change.toLocaleString()}원이에요.`;
        }
      }
    ]
  },

  introduce: {
    title: "👋 자기소개",
    icon: "🙋",
    npcAvatar: "👩‍🏫",
    steps: [
      {
        npcText: "안녕하세요! 이름이 뭐예요?",
        hint: "인사를 골라봐요",
        choiceLabel: "👆 인사 방법을 골라봐요!",
        choices: [
          { icon: "😊", label: "안녕하세요" },
          { icon: "👋", label: "반가워요" },
          { icon: "🌟", label: "처음 만나요" },
          { icon: "😄", label: "저도 반가워요" }
        ],
        modelFn: (c) => `${c}! 제 이름은 민지예요.`
      },
      {
        npcText: "몇 학년이에요?",
        hint: "학년을 골라봐요",
        choiceLabel: "👆 학년을 골라봐요!",
        choices: [
          { icon: "🏫", label: "고등학교 1학년" },
          { icon: "📚", label: "고등학교 2학년" },
          { icon: "🎓", label: "고등학교 3학년" },
          { icon: "📖", label: "중학교 3학년" }
        ],
        modelFn: (c) => `저는 ${c}이에요.`
      },
      {
        npcText: "취미가 뭐예요?",
        hint: "좋아하는 것을 골라봐요",
        choiceLabel: "👆 좋아하는 것을 골라봐요!",
        choices: [
          { icon: "🎵", label: "음악 듣기" },
          { icon: "📺", label: "영상 보기" },
          { icon: "🎨", label: "그림 그리기" },
          { icon: "🎮", label: "게임 하기" }
        ],
        modelFn: (c) => `제 취미는 ${c}예요.`
      }
    ]
  },

  bus: {
    title: "🚌 버스 / 지하철",
    icon: "🚉",
    npcAvatar: "👷",
    steps: [
      {
        npcText: "어디 가세요?",
        hint: "가고 싶은 곳을 골라봐요",
        choiceLabel: "👆 가고 싶은 곳을 골라봐요!",
        choices: [
          { icon: "🏫", label: "학교" },
          { icon: "🏥", label: "병원" },
          { icon: "🛒", label: "마트" },
          { icon: "🏠", label: "집" }
        ],
        modelFn: (c) => `${c}에 가요.`
      },
      {
        npcText: "몇 번 버스를 타실 건가요?",
        hint: "버스 번호를 골라봐요",
        choiceLabel: "👆 버스 번호를 골라봐요!",
        choices: [
          { icon: "🔢", label: "5번" },
          { icon: "🔢", label: "17번" },
          { icon: "🔢", label: "32번" },
          { icon: "🚇", label: "지하철" }
        ],
        modelFn: (c) => c === "지하철" ? "지하철을 탈 거예요." : `${c} 버스를 탈 거예요.`
      },
      {
        npcText: "교통카드 있어요?",
        hint: "교통카드에 대해 말해봐요",
        choiceLabel: "👆 맞는 것을 골라봐요!",
        choices: [
          { icon: "💳", label: "네, 있어요" },
          { icon: "🚫", label: "없어요" },
          { icon: "💵", label: "현금 낼게요" },
          { icon: "❓", label: "얼마예요?" }
        ],
        modelFn: (c) => {
          if (c === "네, 있어요") return "네, 교통카드 있어요.";
          if (c === "없어요") return "교통카드가 없어요.";
          if (c === "현금 낼게요") return "현금으로 낼게요.";
          return "요금이 얼마예요?";
        }
      }
    ]
  },

  doctor: {
    title: "🏥 병원 접수",
    icon: "🩺",
    npcAvatar: "👩‍⚕️",
    steps: [
      {
        npcText: "어떻게 오셨어요?",
        hint: "아픈 곳을 골라봐요",
        choiceLabel: "👆 아픈 곳을 골라봐요!",
        choices: [
          { icon: "🤒", label: "열이 나요" },
          { icon: "🤧", label: "콧물이 나요" },
          { icon: "😣", label: "배가 아파요" },
          { icon: "🤕", label: "머리가 아파요" }
        ],
        modelFn: (c) => `${c}.`
      },
      {
        npcText: "언제부터 아팠어요?",
        hint: "언제부터인지 골라봐요",
        choiceLabel: "👆 언제부터인지 골라봐요!",
        choices: [
          { icon: "📅", label: "오늘부터요" },
          { icon: "📅", label: "어제부터요" },
          { icon: "📅", label: "2일 전부터요" },
          { icon: "📅", label: "일주일 전부터요" }
        ],
        modelFn: (c) => `${c}.`
      },
      {
        npcText: "어디에 사세요?",
        hint: "주소를 어떻게 말하는지 골라봐요",
        choiceLabel: "👆 어떻게 말할지 골라봐요!",
        choices: [
          { icon: "🏠", label: "수원에 살아요" },
          { icon: "📋", label: "주소가 뭐예요?" },
          { icon: "📝", label: "적어드릴게요" },
          { icon: "🆔", label: "신분증 있어요" }
        ],
        modelFn: (c) => {
          if (c === "수원에 살아요") return "저는 수원에 살아요.";
          if (c === "주소가 뭐예요?") return "주소가 어디예요?";
          if (c === "적어드릴게요") return "여기 적어드릴게요.";
          return "신분증 있어요.";
        }
      }
    ]
  },

  phone: {
    title: "📞 전화 통화",
    icon: "📱",
    npcAvatar: "🤳",
    steps: [
      {
        npcText: "여보세요? 누구세요?",
        hint: "전화로 나를 소개해봐요",
        choiceLabel: "👆 전화로 나를 어떻게 소개할지 골라봐요!",
        choices: [
          { icon: "👋", label: "안녕하세요, 민지예요" },
          { icon: "🏫", label: "학교 친구예요" },
          { icon: "👨‍👩‍👧", label: "딸이에요" },
          { icon: "📞", label: "전화 잘못 거셨어요" }
        ],
        modelFn: (c) => `${c}.`
      },
      {
        npcText: "무슨 일이에요?",
        hint: "전화한 이유를 말해봐요",
        choiceLabel: "👆 전화한 이유를 골라봐요!",
                choices: [
          { icon: "📚", label: "숙제 물어보려고요" },
          { icon: "🤝", label: "약속 정하려고요" },
          { icon: "❓", label: "확인하려고요" },
          { icon: "💌", label: "안부 전하려고요" }
        ],
        modelFn: (c) => `${c}.`
      },
      {
        npcText: "나중에 다시 전화해도 될까요?",
        hint: "대답을 골라봐요",
        choiceLabel: "👆 어떻게 대답할지 골라봐요!",
        choices: [
          { icon: "✅", label: "네, 괜찮아요" },
          { icon: "⏰", label: "저녁에 전화하세요" },
          { icon: "📱", label: "문자 보내주세요" },
          { icon: "❌", label: "지금 얘기해요" }
        ],
        modelFn: (c) => {
          if (c === "네, 괜찮아요") return "네, 괜찮아요. 전화 주세요.";
          if (c === "저녁에 전화하세요") return "저녁에 다시 전화해 주세요.";
          if (c === "문자 보내주세요") return "문자 메시지로 보내주세요.";
          return "지금 얘기해요.";
        }
      }
    ]
  }
};

// ─── STATE ─────────────────────────────────────────────────────────────────
let currentSituation = null;
let currentStep = 0;
let totalSteps = 0;
let selectedChoice = null;
let modelAnswer = '';
let correctCount = 0;
let gamePhase = 'choose';
let gameState = {}; // 주문 내역 저장 (음식, 음료, 수량 등)

// ─── MAIN FUNCTIONS ────────────────────────────────────────────────────────
function startGame(situationKey) {
  currentSituation = SITUATIONS[situationKey];
  currentStep = 0;
  totalSteps = currentSituation.steps.length;
  correctCount = 0;
  gameState = {};

  document.getElementById('situation-screen').style.display = 'none';
  document.getElementById('complete-screen').style.display = 'none';
  document.getElementById('game-screen').style.display = 'block';
  document.getElementById('game-title').textContent = currentSituation.title;
  document.getElementById('npc-avatar').textContent = currentSituation.npcAvatar;

  loadStep();
}

function getTotal() {
  if (currentSituation.title.includes('식당')) {
    return (gameState.foodOrder?.price || 0) + (gameState.drinkOrder?.price || 0);
  } else {
    return (gameState.storeItem?.price || 0) * (gameState.storeCount?.count || 1);
  }
}

function showReceiptBox(step) {
  const box = document.getElementById('receipt-box');
  // 영수증 단계 이후에도 계속 표시 (showReceipt 이후 단계 포함)
  const receiptShown = currentStep >= currentSituation.steps.findIndex(s => s.showReceipt);
  if (!step.showReceipt && !step.isPaymentStep && !receiptShown) { box.style.display = 'none'; return; }
  if (receiptShown && (gameState.foodOrder || gameState.storeItem)) {
    box.style.display = 'block';
  } else if (!step.showReceipt && !step.isPaymentStep) {
    box.style.display = 'none'; return;
  } else {
    box.style.display = 'block';
  }
  const rows = document.getElementById('receipt-rows');
  let html = '';
  const total = getTotal();
  if (currentSituation.title.includes('식당')) {
    if (gameState.foodOrder) html += `<div class="receipt-row"><span>${gameState.foodOrder.icon} ${gameState.foodOrder.label} 1개</span><span>${gameState.foodOrder.price.toLocaleString()}원</span></div>`;
    if (gameState.drinkOrder) html += `<div class="receipt-row"><span>${gameState.drinkOrder.icon} ${gameState.drinkOrder.label} 1개</span><span>${gameState.drinkOrder.price.toLocaleString()}원</span></div>`;
  } else {
    if (gameState.storeItem && gameState.storeCount) {
      html += `<div class="receipt-row"><span>${gameState.storeItem.icon} ${gameState.storeItem.label} ${gameState.storeCount.label}</span><span>${(gameState.storeItem.price * gameState.storeCount.count).toLocaleString()}원</span></div>`;
    }
  }
  html += `<div class="receipt-row receipt-total"><span>합계</span><span>${total.toLocaleString()}원</span></div>`;
  rows.innerHTML = html;
}

function loadStep() {
  const step = currentSituation.steps[currentStep];
  gamePhase = 'choose';
  selectedChoice = null;

  const pct = (currentStep / totalSteps) * 100;
  document.getElementById('progress-bar').style.width = pct + '%';

  // NPC 말 — 계산 단계면 동적으로 생성
  let npcText = step.npcText;
  if (step.isPaymentStep) {
    const total = getTotal();
    npcText = `총 ${total.toLocaleString()}원이에요. 얼마 내실 건가요?`;
  }
  document.getElementById('npc-text').textContent = npcText;
  document.getElementById('npc-hint').textContent = step.hint || '';

  // 영수증 표시
  showReceiptBox(step);

  // 보기 버튼
  document.getElementById('step-choose-label').textContent = step.choiceLabel;
  const grid = document.getElementById('choice-grid');
  grid.innerHTML = '';
  step.choices.forEach((ch) => {
    const btn = document.createElement('button');
    btn.className = 'choice-btn';
    // 가격 있으면 가격도 표시
    btn.innerHTML = `<span class="choice-icon">${ch.icon}</span><span class="choice-label">${ch.label}</span>${ch.price ? `<span class="choice-price">${ch.price.toLocaleString()}원</span>` : `<span class="choice-label" style="opacity:0"> </span>`}`;
    btn.onclick = () => selectChoice(ch, btn);
    grid.appendChild(btn);
  });

  // UI 초기화
  document.getElementById('step-choose').style.display = 'block';
  document.getElementById('model-box').style.display = 'none';
  document.getElementById('repeat-area').style.display = 'none';
  document.getElementById('feedback').style.display = 'none';
  document.getElementById('next-row').style.display = 'none';

  stopMic();
  const vrb = document.getElementById('voice-result-box');
  vrb.textContent = '여기에 말한 내용이 나타나요';
  vrb.className = 'voice-result-box';
  document.getElementById('mic-status').textContent = '마이크 버튼을 눌러봐요 👆';
  document.getElementById('mic-status').className = 'mic-status';
  document.getElementById('mic-btn').className = 'mic-btn';

  // NPC 음성 자동 재생 (0.4초 후) — 영수증 단계면 이어서 내역도 읽어줌
  setTimeout(() => {
    if (step.showReceipt) {
      speakTextThen(npcText, buildReceiptSpeech());
    } else {
      speakText(npcText);
    }
  }, 400);
}

// 영수증 내역을 음성 문자열로 변환
function buildReceiptSpeech() {
  let parts = [];
  if (currentSituation.title.includes('식당')) {
    if (gameState.foodOrder) parts.push(`${gameState.foodOrder.label} 한 개, ${gameState.foodOrder.price.toLocaleString()}원`);
    if (gameState.drinkOrder) parts.push(`${gameState.drinkOrder.label} 한 개, ${gameState.drinkOrder.price.toLocaleString()}원`);
  } else {
    if (gameState.storeItem && gameState.storeCount) {
      const total = gameState.storeItem.price * gameState.storeCount.count;
      parts.push(`${gameState.storeItem.label} ${gameState.storeCount.label}, ${total.toLocaleString()}원`);
    }
  }
  const total = getTotal();
  parts.push(`합계 ${total.toLocaleString()}원입니다.`);
  return parts.join('. ');
}

// 첫 번째 문장 재생 후 이어서 두 번째 문장 재생
function speakTextThen(first, second) {
  if (!window.speechSynthesis) return;
  stopSpeech();
  const u1 = new SpeechSynthesisUtterance(first);
  u1.lang = 'ko-KR'; u1.rate = 0.85; u1.pitch = 1.05;
  const voices = window.speechSynthesis.getVoices();
  const koVoice = voices.find(v => v.lang.startsWith('ko'));
  if (koVoice) u1.voice = koVoice;

  u1.onend = () => {
    const u2 = new SpeechSynthesisUtterance(second);
    u2.lang = 'ko-KR'; u2.rate = 0.85; u2.pitch = 1.05;
    if (koVoice) u2.voice = koVoice;
    window.speechSynthesis.speak(u2);
  };
  window.speechSynthesis.speak(u1);
}

function selectChoice(ch, btn) {
  if (gamePhase !== 'choose') return;

  document.querySelectorAll('.choice-btn').forEach(b => b.classList.remove('selected'));
  btn.classList.add('selected');
  selectedChoice = ch;

  const step = currentSituation.steps[currentStep];

  // gameState에 선택 저장
  if (step.onSelect) step.onSelect(ch, gameState);

  // 모범 답변 계산
  modelAnswer = step.modelFn(ch, gameState);

  setTimeout(() => {
    gamePhase = 'model';
    document.getElementById('model-text').textContent = modelAnswer;
    document.getElementById('model-box').style.display = 'block';
    // 영수증 단계면 영수증 업데이트
    if (step.showReceipt || step.isPaymentStep) showReceiptBox(step);
    speakText(modelAnswer);

    setTimeout(() => {
      gamePhase = 'repeat';
      document.getElementById('repeat-area').style.display = 'flex';
    }, 1400);
  }, 400);
}

// ─── VOICE RECOGNITION ─────────────────────────────────────────────────────
let recognition = null;
let isListening = false;

function initRecognition() {
  const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
  if (!SpeechRecognition) {
    alert('이 브라우저는 음성 인식을 지원하지 않아요.\nChrome 브라우저를 사용해주세요! 🙏');
    return null;
  }
  const rec = new SpeechRecognition();
  rec.lang = 'ko-KR';
  rec.interimResults = true;
  rec.maxAlternatives = 5;
  rec.continuous = false;

  rec.onstart = () => {
    isListening = true;
    document.getElementById('mic-btn').className = 'mic-btn listening';
    const vrb = document.getElementById('voice-result-box');
    vrb.textContent = '🎤 듣고 있어요...';
    vrb.className = 'voice-result-box listening';
    document.getElementById('mic-status').textContent = '말해봐요! 🎙️';
    document.getElementById('mic-status').className = 'mic-status active';
  };

  rec.onresult = (e) => {
    let interim = '';
    let final = '';
    for (let i = e.resultIndex; i < e.results.length; i++) {
      const t = e.results[i][0].transcript;
      if (e.results[i].isFinal) final += t;
      else interim += t;
    }
    const display = final || interim;
    document.getElementById('voice-result-box').textContent = display || '🎤 듣고 있어요...';
    if (final) {
      // Collect all alternatives
      const alts = [];
      for (let i = e.resultIndex; i < e.results.length; i++) {
        if (e.results[i].isFinal) {
          for (let j = 0; j < e.results[i].length; j++) {
            alts.push(e.results[i][j].transcript);
          }
        }
      }
      evaluateVoice(alts.length ? alts : [final]);
    }
  };

  rec.onerror = (e) => {
    isListening = false;
    document.getElementById('mic-btn').className = 'mic-btn';
    if (e.error === 'no-speech') {
      document.getElementById('mic-status').textContent = '소리가 안 들려요. 다시 눌러봐요!';
    } else if (e.error === 'not-allowed') {
      document.getElementById('mic-status').textContent = '마이크 권한이 필요해요! 브라우저 설정을 확인해주세요.';
    } else {
      document.getElementById('mic-status').textContent = '다시 시도해봐요!';
    }
    document.getElementById('mic-status').className = 'mic-status';
    document.getElementById('voice-result-box').className = 'voice-result-box';
  };

  rec.onend = () => {
    isListening = false;
    document.getElementById('mic-btn').className = 'mic-btn';
    if (document.getElementById('mic-status').className.includes('active')) {
      document.getElementById('mic-status').textContent = '다시 눌러봐요 👆';
      document.getElementById('mic-status').className = 'mic-status';
    }
  };

  return rec;
}

function toggleMic() {
  if (gamePhase !== 'repeat') return;
  if (isListening) { stopMic(); return; }
  stopSpeech();
  if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
    navigator.mediaDevices.getUserMedia({ audio: true })
      .then(stream => {
        stream.getTracks().forEach(t => t.stop());
        recognition = initRecognition();
        if (recognition) recognition.start();
      })
      .catch(() => {
        document.getElementById('mic-status').textContent = '마이크 권한을 허용해주세요! 주소창 옆 자물쇠 아이콘을 눌러봐요.';
        document.getElementById('mic-status').className = 'mic-status active';
      });
  } else {
    recognition = initRecognition();
    if (recognition) recognition.start();
  }
}

function stopMic() {
  if (recognition) {
    try { recognition.stop(); } catch(e) {}
  }
  isListening = false;
  document.getElementById('mic-btn').className = 'mic-btn';
}

function evaluateVoice(alts) {
  const normalize = s => s
    .replace(/[.,!?~\s·]/g, '')
    .replace(/[0-9]/g, d => ['영','일','이','삼','사','오','육','칠','팔','구'][+d] || d)
    .toLowerCase();

  const target = normalize(modelAnswer);
  const matched = alts.some(a => normalize(a) === target);

  // 핵심 단어 추출 (숫자 포함 2글자 이상 단어)
  const keywords = modelAnswer.match(/[가-힣0-9]{2,}/g) || [];
  const normalizedKeywords = keywords.map(k => normalize(k));

  // 85% 이상 + 핵심단어 모두 포함해야 정답
  const partialMatch = alts.some(a => {
    const na = normalize(a);
    if (!target.length) return false;

    // 글자 유사도 85% 체크
    let hits = 0;
    for (let ch of na) { if (target.includes(ch)) hits++; }
    const similarity = hits / target.length;
    const lengthOk = na.length >= target.length * 0.8;

    // 핵심 단어가 모두 포함돼 있는지 체크
    const keywordsOk = normalizedKeywords.every(kw => na.includes(kw));

    return similarity >= 0.95 && lengthOk && keywordsOk;
  });

  const vrb = document.getElementById('voice-result-box');
  vrb.textContent = alts[0];

  if (matched || partialMatch) {
    vrb.className = 'voice-result-box correct';
    correctCount++;
    showFeedback('🎉 완벽해요! 정말 잘했어요! 👏', 'ok');
    document.getElementById('next-row').style.display = 'flex';
    document.getElementById('mic-status').textContent = '훌륭해요! ✅';
    document.getElementById('mic-status').className = 'mic-status';
    speakText('정말 잘했어요!');
    if (currentStep === totalSteps - 1) {
      setTimeout(showComplete, 1500);
    }
  } else {
    vrb.className = 'voice-result-box wrong';
    showFeedback('❌ 다시 해봐요! 위의 문장을 보고 따라 말해봐요 💪', 'err');
    document.getElementById('mic-status').textContent = '다시 눌러서 말해봐요! 🎤';
    document.getElementById('mic-status').className = 'mic-status';
  }
}

function skipStep() {
  stopMic();
  showFeedback('💡 다음엔 꼭 말해봐요!', 'err');
  document.getElementById('next-row').style.display = 'flex';
  if (currentStep === totalSteps - 1) {
    setTimeout(showComplete, 1000);
  }
}

function nextStep() {
  currentStep++;
  if (currentStep >= totalSteps) {
    showComplete();
  } else {
    loadStep();
  }
}

function showComplete() {
  document.getElementById('game-screen').style.display = 'none';
  document.getElementById('complete-screen').style.display = 'block';

  const pct = Math.round((correctCount / totalSteps) * 100);
  let msg = '';
  let stars = '';
  if (pct === 100) { msg = '모든 문제를 완벽하게 맞혔어요! 최고예요! 🥇'; stars = '⭐⭐⭐'; }
  else if (pct >= 66) { msg = '잘 하고 있어요! 조금 더 연습하면 완벽해질 거예요! 💪'; stars = '⭐⭐'; }
  else { msg = '잘했어요! 다시 한번 도전해 보아요! 🌱'; stars = '⭐'; }

  document.querySelector('.star-row').textContent = stars;
  document.getElementById('complete-msg').textContent = msg;
  document.getElementById('score-badge').textContent = `${correctCount} / ${totalSteps} 정답 (${pct}%)`;
  document.getElementById('progress-bar').style.width = '100%';

  launchConfetti();
  speakText(msg);
}

function showFeedback(text, type) {
  const fb = document.getElementById('feedback');
  fb.textContent = text;
  fb.className = 'feedback ' + type;
  fb.style.display = 'block';
}

function goBack() {
  stopMic();
  document.getElementById('game-screen').style.display = 'none';
  document.getElementById('complete-screen').style.display = 'none';
  document.getElementById('situation-screen').style.display = 'block';
  stopSpeech();
}

// ─── TTS ───────────────────────────────────────────────────────────────────
let currentUtterance = null;
function speakText(text) {
  if (!window.speechSynthesis) return;
  stopSpeech();
  const utter = new SpeechSynthesisUtterance(text);
  utter.lang = 'ko-KR';
  utter.rate = 0.85;
  utter.pitch = 1.05;
  // Try to find Korean voice
  const voices = window.speechSynthesis.getVoices();
  const koVoice = voices.find(v => v.lang.startsWith('ko'));
  if (koVoice) utter.voice = koVoice;
  currentUtterance = utter;
  window.speechSynthesis.speak(utter);
}
function stopSpeech() {
  if (window.speechSynthesis) window.speechSynthesis.cancel();
}
window.speechSynthesis && window.speechSynthesis.onvoiceschanged !== undefined
  && (window.speechSynthesis.onvoiceschanged = () => {});

// ─── CONFETTI ──────────────────────────────────────────────────────────────
function launchConfetti() {
  const wrap = document.getElementById('confetti');
  wrap.innerHTML = '';
  const colors = ['#ffd94a','#4ecb71','#3b9edd','#ff6b9d','#ff8c42','#9b72cf'];
  for (let i = 0; i < 60; i++) {
    const div = document.createElement('div');
    div.className = 'confetti-piece';
    div.style.left = Math.random() * 100 + 'vw';
    div.style.top = '-20px';
    div.style.background = colors[Math.floor(Math.random() * colors.length)];
    div.style.animationDuration = (1.5 + Math.random() * 2) + 's';
    div.style.animationDelay = (Math.random() * 1) + 's';
    div.style.transform = `rotate(${Math.random()*360}deg)`;
    wrap.appendChild(div);
    setTimeout(() => div.remove(), 4000);
  }
}
</script>
</body>
</html>
