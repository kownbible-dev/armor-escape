# armor-escape
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <title>전신갑주 방탈출 퀘스트</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    :root {
      --bg: #050816;
      --card: #111827;
      --accent: #fbbf24;
      --accent-soft: rgba(251, 191, 36, 0.14);
      --text: #f9fafb;
      --muted: #9ca3af;
      --danger: #f97373;
      --success: #4ade80;
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI",
        sans-serif;
      background: radial-gradient(circle at top, #111827 0, #020617 55%);
      color: var(--text);
      display: flex;
      justify-content: center;
      min-height: 100vh;
      padding: 16px;
    }

    .app {
      width: 100%;
      max-width: 840px;
      background: linear-gradient(145deg, #020617, #020617);
      border-radius: 24px;
      padding: 20px 18px 24px;
      box-shadow: 0 30px 80px rgba(0, 0, 0, 0.7);
      border: 1px solid rgba(148, 163, 184, 0.2);
      position: relative;
      overflow: hidden;
    }

    @media (min-width: 720px) {
      .app { padding: 28px 28px 30px; }
    }

    .header {
      display: flex;
      align-items: center;
      gap: 12px;
      margin-bottom: 16px;
    }

    .logo-circle {
      width: 38px;
      height: 38px;
      border-radius: 999px;
      border: 2px solid var(--accent);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 18px;
      color: var(--accent);
      box-shadow: 0 0 18px rgba(251, 191, 36, 0.35);
    }

    .header-text h1 {
      margin: 0;
      font-size: 18px;
      letter-spacing: 0.06em;
      text-transform: uppercase;
      color: var(--accent);
    }

    .header-text p {
      margin: 2px 0 0;
      font-size: 13px;
      color: var(--muted);
    }

    .status-bar {
      display: flex;
      align-items: center;
      gap: 12px;
      margin-bottom: 18px;
      flex-wrap: wrap;
    }

    .progress {
      flex: 1;
      height: 8px;
      background: #020617;
      border-radius: 999px;
      overflow: hidden;
      border: 1px solid rgba(148, 163, 184, 0.4);
    }

    .progress-fill {
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, var(--accent), #f97316);
      transition: width 0.35s ease-out;
    }

    .step-indicator {
      font-size: 12px;
      color: var(--muted);
      min-width: 105px;
      text-align: right;
    }

    .inventory {
      background: rgba(15, 23, 42, 0.9);
      border-radius: 16px;
      padding: 10px 12px;
      border: 1px solid rgba(148, 163, 184, 0.25);
      margin-bottom: 12px;
    }

    .inventory-title {
      font-size: 12px;
      text-transform: uppercase;
      letter-spacing: 0.14em;
      color: var(--muted);
      margin-bottom: 6px;
    }

    .badge-row {
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
    }

    .badge {
      font-size: 11px;
      padding: 4px 9px;
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.3);
      background: radial-gradient(circle at top left, #1f2937 0, #020617 72%);
      color: var(--muted);
      display: flex;
      align-items: center;
      gap: 5px;
    }

    .badge.collected {
      border-color: rgba(251, 191, 36, 0.8);
      color: var(--accent);
      background: radial-gradient(circle at top left, #451a03 0, #020617 72%);
      box-shadow: 0 0 14px rgba(251, 191, 36, 0.4);
    }

    .badge-dot {
      width: 7px;
      height: 7px;
      border-radius: 50%;
      background: rgba(148, 163, 184, 0.6);
    }

    .badge.collected .badge-dot {
      background: var(--accent);
    }

    /* 캐릭터 패널 */
    .character-panel {
      margin-bottom: 16px;
      background: rgba(15,23,42,0.95);
      border-radius: 18px;
      border: 1px solid rgba(148,163,184,0.35);
      padding: 8px 10px 10px;
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .character-figure {
      width: 70px;
      height: 70px;
      border-radius: 18px;
      background: radial-gradient(circle at top, #1f2937 0, #020617 80%);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 34px;
      position: relative;
      flex-shrink: 0;
    }

    .character-gear-icon {
      position: absolute;
      font-size: 20px;
    }

    .gear-belt      { bottom: 10px; }
    .gear-breast    { top: 16px; }
    .gear-shoes     { bottom: 4px; right: 10px; }
    .gear-shield    { left: 6px; top: 24px; }
    .gear-helmet    { top: 2px; }
    .gear-sword     { right: 4px; top: 18px; }

    .character-info {
      flex: 1;
    }

    .character-info-title {
      font-size: 12px;
      color: var(--muted);
      margin-bottom: 4px;
    }

    .character-gear-row {
      display: flex;
      flex-wrap: wrap;
      gap: 4px;
    }

    .gear-tag {
      font-size: 11px;
      padding: 2px 7px;
      border-radius: 999px;
      border: 1px dashed rgba(148,163,184,0.5);
      color: var(--muted);
      background: rgba(15,23,42,0.9);
    }

    .gear-tag.on {
      border-style: solid;
      border-color: rgba(251,191,36,0.9);
      color: var(--accent);
      background: rgba(120,53,15,0.7);
      box-shadow: 0 0 12px rgba(251,191,36,0.6);
    }

    .card {
      background: radial-gradient(circle at top, #1f2937 0, #020617 58%);
      border-radius: 22px;
      padding: 18px 16px 18px;
      border: 1px solid rgba(148, 163, 184, 0.35);
      position: relative;
      overflow: hidden;
    }

    @media (min-width: 720px) {
      .card { padding: 22px 22px 22px; }
    }

    .card::before {
      content: "";
      position: absolute;
      inset: -80px;
      background: radial-gradient(
        circle at top right,
        rgba(251, 191, 36, 0.18),
        transparent 62%
      );
      opacity: 0.75;
      pointer-events: none;
    }

    .card-inner {
      position: relative;
      z-index: 1;
    }

    .room-label {
      font-size: 11px;
      text-transform: uppercase;
      letter-spacing: 0.2em;
      color: var(--muted);
      margin-bottom: 6px;
    }

    .room-title {
      margin: 0;
      font-size: 17px;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .room-title span.badge-hard,
    .room-title span.badge-easy {
      font-size: 11px;
      text-transform: uppercase;
      letter-spacing: 0.12em;
      border-radius: 999px;
      padding: 3px 7px;
    }

    .room-title span.badge-hard {
      border: 1px solid rgba(248, 113, 113, 0.8);
      color: #fecaca;
      background: rgba(127, 29, 29, 0.4);
    }

    .room-title span.badge-easy {
      border: 1px solid rgba(45, 212, 191, 0.8);
      color: #a5f3fc;
      background: rgba(6, 78, 59, 0.55);
    }

    .room-subtitle {
      margin: 6px 0 14px;
      font-size: 14px;
      color: var(--muted);
      line-height: 1.5;
    }

    .section-label {
      font-size: 12px;
      font-weight: 600;
      text-transform: uppercase;
      letter-spacing: 0.18em;
      margin: 10px 0 6px;
      color: var(--muted);
    }

    .clue-box {
      background: rgba(15, 23, 42, 0.92);
      border-radius: 16px;
      padding: 10px 11px;
      border: 1px solid rgba(148, 163, 184, 0.25);
      font-size: 13px;
      line-height: 1.6;
    }

    .clue-box ul {
      padding-left: 18px;
      margin: 4px 0 0;
    }

    .clue-box li { margin-bottom: 4px; }

    .question-text {
      margin: 10px 0 8px;
      font-size: 14px;
      line-height: 1.6;
    }

    .choices {
      margin-top: 6px;
      display: flex;
      flex-direction: column;
      gap: 6px;
    }

    .choice-btn {
      width: 100%;
      text-align: left;
      border-radius: 999px;
      padding: 8px 12px;
      font-size: 13px;
      border: 1px solid rgba(148, 163, 184, 0.35);
      background: rgba(15, 23, 42, 0.96);
      color: var(--text);
      cursor: pointer;
      display: flex;
      gap: 8px;
      align-items: center;
      transition: all 0.16s ease-out;
    }

    .choice-btn span.label {
      font-size: 11px;
      width: 22px;
      height: 22px;
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.65);
      display: inline-flex;
      align-items: center;
      justify-content: center;
      flex-shrink: 0;
    }

    .choice-btn:hover {
      border-color: rgba(251, 191, 36, 0.85);
      box-shadow: 0 0 0 1px rgba(251, 191, 36, 0.25);
      transform: translateY(-1px);
    }

    .choice-btn.correct {
      border-color: rgba(74, 222, 128, 0.85);
      background: rgba(22, 101, 52, 0.8);
    }

    .choice-btn.wrong {
      border-color: rgba(248, 113, 113, 0.85);
      background: rgba(127, 29, 29, 0.7);
    }

    .hint-row {
      margin-top: 10px;
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
      align-items: center;
    }

    .hint-btn {
      border-radius: 999px;
      padding: 6px 12px;
      font-size: 12px;
      background: rgba(15, 23, 42, 0.9);
      color: var(--accent);
      border: 1px dashed rgba(251, 191, 36, 0.6);
      cursor: pointer;
      display: none; /* 처음엔 안 보이게 */
    }

    .hint-text {
      font-size: 12px;
      color: var(--muted);
      flex: 1;
      min-width: 180px;
    }

    .feedback {
      margin-top: 10px;
      font-size: 13px;
      line-height: 1.6;
    }

    .feedback.ok { color: var(--success); }
    .feedback.no { color: var(--danger); }

    .footer-row {
      margin-top: 16px;
      display: flex;
      justify-content: space-between;
      gap: 10px;
      flex-wrap: wrap;
      align-items: center;
    }

    .nav-btn {
      border-radius: 999px;
      padding: 8px 16px;
      font-size: 13px;
      border: none;
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      gap: 6px;
      background: var(--accent-soft);
      color: var(--accent);
    }

    .nav-btn.primary {
      background: linear-gradient(135deg, #f97316, #fbbf24);
      color: #111827;
      font-weight: 600;
      box-shadow: 0 12px 30px rgba(248, 181, 55, 0.5);
    }

    .nav-btn:disabled {
      opacity: 0.4;
      cursor: default;
      box-shadow: none;
    }

    .completion {
      text-align: center;
      padding: 16px 8px 4px;
      font-size: 14px;
      color: var(--muted);
    }

    .completion strong { color: var(--accent); }

    /* 시작 화면 & 장소 오버레이 */
    .start-overlay,
    .location-overlay {
      position: absolute;
      inset: 0;
      background: radial-gradient(circle at top, rgba(15,23,42,0.98), #020617);
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      padding: 24px 18px;
      z-index: 10;
    }

    .start-card,
    .location-card {
      max-width: 420px;
      width: 100%;
      background: rgba(15,23,42,0.96);
      border-radius: 22px;
      padding: 20px 18px 22px;
      border: 1px solid rgba(148,163,184,0.35);
      box-shadow: 0 24px 60px rgba(0,0,0,0.7);
      text-align: center;
    }

    .start-title,
    .location-title {
      font-size: 18px;
      margin: 0 0 6px;
      color: var(--accent);
    }

    .start-sub,
    .location-sub {
      font-size: 13px;
      color: var(--muted);
      margin: 0 0 14px;
      line-height: 1.6;
    }

    .start-label {
      font-size: 12px;
      color: var(--muted);
      text-align: left;
      margin-bottom: 4px;
    }

    .start-input {
      width: 100%;
      border-radius: 999px;
      border: 1px solid rgba(148,163,184,0.5);
      padding: 8px 12px;
      font-size: 13px;
      background: #020617;
      color: var(--text);
      outline: none;
    }

    .start-input::placeholder {
      color: rgba(148,163,184,0.7);
    }

    .start-btn,
    .location-btn {
      margin-top: 14px;
      width: 100%;
      border-radius: 999px;
      border: none;
      padding: 9px 12px;
      font-size: 14px;
      background: linear-gradient(135deg, #f97316, #fbbf24);
      color: #111827;
      font-weight: 600;
      cursor: pointer;
      box-shadow: 0 14px 36px rgba(248, 181, 55, 0.6);
    }

    .start-hint,
    .location-hint {
      margin-top: 8px;
      font-size: 11px;
      color: var(--muted);
      text-align: left;
    }

    .location-overlay { display: none; }

    .location-file {
      width: 100%;
      margin-top: 6px;
      font-size: 12px;
      color: var(--muted);
      text-align: left;
    }

    .location-file input {
      margin-top: 4px;
      width: 100%;
      font-size: 12px;
      color: var(--muted);
    }

    .location-btn:disabled {
      opacity: 0.4;
      cursor: default;
      box-shadow: none;
    }
  </style>
</head>
<body>
  <div class="app">
    <div class="header">
      <div class="logo-circle">🛡</div>
      <div class="header-text">
        <h1>Armor Escape</h1>
        <p id="headerSub">바울의 전도여행과 전신갑주로 떠나는 방탈출 퀘스트</p>
      </div>
    </div>

    <div class="status-bar">
      <div class="progress">
        <div class="progress-fill" id="progressFill"></div>
      </div>
      <div class="step-indicator" id="stepIndicator">1 / 6 단계</div>
    </div>

    <div class="inventory">
      <div class="inventory-title">획득한 전신갑주</div>
      <div class="badge-row" id="inventoryBadges"></div>
    </div>

    <!-- 캐릭터 패널 -->
    <div class="character-panel">
      <div class="character-figure" id="characterFigure">
        🧍
      </div>
      <div class="character-info">
        <div class="character-info-title">내 캐릭터 장비 상태</div>
        <div class="character-gear-row" id="characterGearRow"></div>
      </div>
    </div>

    <div class="card">
      <div class="card-inner">
        <div id="roomContent"></div>
      </div>
    </div>

    <div class="completion" id="completionText">
      전신갑주 6개를 모두 모으면 <strong>엔딩 메시지</strong>가 열립니다.
    </div>

    <!-- 시작 화면 -->
    <div class="start-overlay" id="startOverlay">
      <div class="start-card">
        <h2 class="start-title">전신갑주 방탈출 퀘스트</h2>
        <p class="start-sub">
          바울의 전도여행을 따라가며,<br />
          각 방에서 전신갑주 한 가지씩을 획득해 보세요.
        </p>
        <div style="text-align:left; margin-bottom:8px;">
          <div class="start-label">이름 또는 팀 이름 (선택)</div>
          <input id="playerName" class="start-input" type="text" placeholder="예: 청년부 1팀, OO셀" />
        </div>
        <button class="start-btn" id="startBtn">퀘스트 시작하기 →</button>
        <div class="start-hint">
          성경(종이 또는 앱)을 옆에 두고, 단서를 따라가며 정답을 찾아보세요.
        </div>
      </div>
    </div>

    <!-- 장소 이동 + 사진 업로드 게이트 -->
    <div class="location-overlay" id="locationOverlay">
      <div class="location-card">
        <h2 class="location-title">다음 장소로 이동!</h2>
        <p class="location-sub" id="locationText">
          장소 안내 문구가 여기에 표시됩니다.
        </p>
        <div class="location-file">
          <div>📷 해당 장소에서 사진을 찍고 업로드하세요.</div>
          <input type="file" id="photoInput" accept="image/*" />
        </div>
        <button class="location-btn" id="locationNextBtn" disabled>다음 방 열기 →</button>
        <div class="location-hint">
          * 사진 파일 내용은 자동으로 저장되지는 않지만,<br />
          &nbsp;실제 모임에서는 리더가 업로드 여부만 확인하면 됩니다.
        </div>
      </div>
    </div>
  </div>

  <script>
    const rooms = [
      {
        id: 1,
        type: "easy",
        label: "ROOM 1 · 진리의 허리띠",
        title: "안디옥 기록실에서 탈출하라",
        subtitle:
          "안디옥 교회에 거짓 가르침이 들어왔습니다. 참된 복음을 분명히 붙잡아야만 이 방을 나갈 수 있습니다.",
        clues: [
          "후보 구절: 갈 2:16 / 갈 3:11 / 갈 3:2",
          "키워드: ‘믿음으로 의롭다’ vs ‘율법의 행위’",
          "“예수님 + 아무것도 더하지 않는다”가 복음의 핵심"
        ],
        question:
          "세 단서를 모두 만족하는, ‘복음의 진리’를 가장 분명하게 말하는 구절은 어느 것일까요?",
        choices: [
          {
            id: "A",
            text: "갈라디아서 3:11",
            correct: false,
            feedback:
              "갈 3:11도 믿음에 대해 말하지만, ‘율법의 행위가 아니라 예수 그리스도를 믿음으로 의롭다 하심을 얻는다’는 구조가 완전히 드러나지 않아요."
          },
          {
            id: "B",
            text: "갈라디아서 3:2",
            correct: false,
            feedback:
              "갈 3:2는 시작은 좋지만, ‘의롭다’와 ‘행위/믿음 대비’가 세밀하게 정리되어 있지 않아요. 더 정확한 요약이 한 곳에 있습니다."
          },
          {
            id: "C",
            text: "갈라디아서 2:16",
            correct: true,
            feedback:
              "정답입니다! 갈 2:16은 ‘율법의 행위가 아닌 예수 그리스도를 믿음으로 의롭다 하심을 얻는다’는 복음을 가장 명확하게 선포합니다."
          }
        ],
        hint: "갈라디아서 2–3장을 빠르게 훑어보면서, ‘율법의 행위’와 ‘믿음으로 의롭다’가 동시에 나오는 절을 찾아보세요.",
        armorKey: "belt"
      },
      {
        id: 2,
        type: "easy",
        label: "ROOM 2 · 의의 흉배",
        title: "루스드라 광장에서 다시 일어서는 이유",
        subtitle:
          "바울은 루스드라에서 돌에 맞아 거의 죽을 뻔했지만, 다시 그 도시로 들어갑니다. 그는 왜 위험을 감수하고 돌아갔을까요?",
        clues: [
          "“우리가 하나님 나라에 들어가려면 많은 환난을 겪어야 할 것이라”(사도행전 14장 내용)",
          "바울의 관심은 ‘자기 안전’보다 ‘하나님 나라와 복음’",
          "환난조차도 의로운 목적을 막지 못한다"
        ],
        question:
          "바울이 죽을 뻔한 상황에서도 다시 도시로 돌아간 가장 성경적인 이유는 무엇일까요?",
        choices: [
          {
            id: "A",
            text: "돌에 맞았지만, 사람들의 인정을 다시 받고 싶어서",
            correct: false,
            feedback:
              "바울의 동기는 사람들의 인정이 아니라 하나님 나라와 복음이었어요."
          },
          {
            id: "B",
            text: "그곳 음식을 더 먹고 싶어서 (농담)",
            correct: false,
            feedback: "이건 그냥 농담 같은 상상일 뿐, 본문과는 거리가 멀어요!"
          },
          {
            id: "C",
            text: "하나님 나라에 들어가기까지 환난을 감수하는 것이 의로운 길이기 때문에",
            correct: true,
            feedback:
              "맞아요! 행 14:22에 따르면, 바울은 환난을 ‘하나님 나라로 가는 길에서 마땅히 감수해야 할 것’으로 이해했습니다."
          }
        ],
        hint: "사도행전 14:19–22를 읽어 보세요. 바울이 제자들을 어떻게 격려했는지에 주목해 보세요.",
        armorKey: "breastplate"
      },
      {
        id: 3,
        type: "easy",
        label: "ROOM 3 · 평안의 복음의 신",
        title: "열린 감옥에서 도망가지 않은 선택",
        subtitle:
          "지진으로 문이 열리고 쇠사슬이 풀렸는데도, 바울과 실라는 감옥에서 도망가지 않습니다. 이 선택은 한 사람의 생명을 살리고, 한 가정을 복음 안으로 이끌죠.",
        clues: [
          "간수: “내가 죽었구나!”라고 두려워함 (행 16:27)",
          "바울: “우리가 다 여기 있노라” (행 16:28)",
          "이 한 마디가 간수의 절망을 멈추고, 이후 복음의 대화로 이어짐"
        ],
        question:
          "바울이 도망가지 않았다는 사실을 가장 분명하게 드러내는 구절은 어느 절일까요?",
        choices: [
          {
            id: "A",
            text: "사도행전 16:27",
            correct: false,
            feedback:
              "16:27은 간수의 두려운 반응을 보여주지만, 바울의 선택은 아직 나타나지 않았습니다."
          },
          {
            id: "B",
            text: "사도행전 16:28",
            correct: true,
            feedback:
              "정답입니다! “우리가 다 여기 있노라”는 바울이 도망가지 않았다는 사실을 직접적으로 보여줍니다."
          },
          {
            id: "C",
            text: "사도행전 16:31",
            correct: false,
            feedback:
              "16:31은 구원의 길을 설명하는 중요한 말씀입니다. 그러나 ‘도망가지 않은 행동’ 자체를 보여주는 구절은 아닙니다."
          }
        ],
        hint: "행 16:25–34 전체를 읽어 본 뒤, ‘행동’과 ‘설명’을 구분해 보세요.",
        armorKey: "shoes"
      },
      {
        id: 4,
        type: "hard",
        label: "ROOM 4 · 믿음의 방패",
        title: "두 편지로 푸는 믿음의 비밀",
        subtitle:
          "바울은 계속되는 박해 속에서도 낙심하지 않습니다. 그 이유는 단순한 ‘성격 문제’가 아니라, 하나님께서 주신 사명과 영원한 것에 대한 시선 때문입니다.",
        clues: [
          "데살로니가전서 2:4 – “하나님께 옳게 여기심을 입어 복음을 맡겼으니…”",
          "고린도후서 4:16–18 – “보이는 것은 잠깐이요 보이지 않는 것은 영원함이라”",
          "믿음의 방패 = 사명 + 영원한 것을 바라보는 시선"
        ],
        question:
          "바울이 박해 가운데서도 낙심하지 않은 이유를 가장 잘 설명하는 선택지는 어느 것일까요?",
        choices: [
          {
            id: "A",
            text: "성격이 강해서 웬만한 일에는 별로 신경 쓰지 않았기 때문에",
            correct: false,
            feedback:
              "성경은 바울의 인격보다 ‘하나님께서 맡기신 사명’과 ‘영원한 것에 대한 믿음’을 강조합니다."
          },
          {
            id: "B",
            text: "하나님께 복음을 맡겨진 자로서, 환난 가운데서도 영원한 영광을 바라보았기 때문에",
            correct: true,
            feedback:
              "정답입니다! 살전 2:4와 고후 4:16–18을 함께 보면, 바울의 믿음의 방패는 ‘사명’과 ‘영원한 것에 대한 소망’ 위에 서 있음을 알 수 있습니다."
          },
          {
            id: "C",
            text: "고난이 지나면 반드시 물질적인 축복이 온다고 생각했기 때문에",
            correct: false,
            feedback:
              "바울은 물질적 축복을 약속으로 붙잡은 것이 아니라, 영원한 영광과 하나님의 승인(옳게 여기심)을 붙잡았습니다."
          }
        ],
        hint: "데살로니가전서 2:1–4와 고린도후서 4장을 함께 읽으면서 ‘무엇 때문에’ 낙심하지 않는지 찾아보세요.",
        armorKey: "shield"
      },
      {
        id: 5,
        type: "hard",
        label: "ROOM 5 · 구원의 투구",
        title: "우상 도시 에베소에서 지켜야 할 확신",
        subtitle:
          "에베소는 우상과 마술이 가득한 도시였습니다. 그 한가운데서 바울이 붙잡은 것은 ‘구원의 확신’이었습니다. 구원의 투구는 생각과 정체성을 지키는 방어구죠.",
        clues: [
          "에베소서 2:8–9 – 은혜로, 믿음으로, 행위가 아니요, 자랑치 못하게 하려 함이라.",
          "에베소서 6:17 – “구원의 투구와 성령의 검 곧 하나님의 말씀을 가지라”",
          "구원의 투구 = 구원의 근거가 ‘나의 행위’가 아니라 ‘하나님의 은혜’라는 확신"
        ],
        question:
          "‘구원의 투구’가 말하는 구원의 본질(구원이 어떻게 주어지는가)을 가장 정확히 설명하는 구절은 무엇일까요?",
        choices: [
          {
            id: "A",
            text: "에베소서 6:17 – 구원의 투구를 가지라",
            correct: false,
            feedback:
              "전신갑주 목록이지만, ‘구원이 어떻게 주어지는가’를 설명하는 본문은 아닙니다."
          },
          {
            id: "B",
            text: "에베소서 2:8–9 – 은혜로, 믿음으로, 행위가 아니요",
            correct: true,
            feedback:
              "정답입니다! 엡 2:8–9는 구원의 본질을 가장 명확하게 정리한 구절입니다."
          },
          {
            id: "C",
            text: "에베소서 4:1 – 부르심에 합당하게 행하라",
            correct: false,
            feedback:
              "중요한 권면이지만, 구원의 ‘근거’보다 구원받은 자의 ‘삶의 방식’을 다룹니다."
          }
        ],
        hint: "에베소서 2장을 먼저 읽고, 6장의 전신갑주 본문을 다시 보면 구조가 보일 거예요.",
        armorKey: "helmet"
      },
      {
        id: 6,
        type: "hard",
        label: "ROOM 6 · 성령의 검",
        title: "로마 황제 앞에서 휘두르는 마지막 한 검",
        subtitle:
          "황제가 묻습니다. “네가 믿는 예수가 너를 어디까지 구원할 수 있지?” 이제 마지막 방탈출은 ‘말씀 한 구절’에 달려 있습니다.",
        clues: [
          "로마서 8장은 성령, 고난, 영광, 하나님의 사랑을 다룹니다.",
          "후보 구절: 8:26 / 8:28 / 8:36 / 8:38–39",
          "황제의 질문은 ‘예수의 구원 능력과 범위’에 대한 도전입니다."
        ],
        question:
          "예수님의 구원을 의심하는 황제의 질문에 가장 논리적으로 반박할 수 있는 구절은 무엇일까요?",
        choices: [
          {
            id: "A",
            text: "로마서 8:26 – 성령이 우리의 연약함을 도우신다",
            correct: false,
            feedback:
              "위로가 되는 말씀입니다. 그러나 황제의 ‘구원이 어디까지 미치는가’에 대한 질문에 직접 반박하지는 못합니다."
          },
          {
            id: "B",
            text: "로마서 8:28 – 하나님을 사랑하는 자에게 모든 것이 합력하여 선을 이룬다",
            correct: false,
            feedback:
              "중요한 약속이지만, ‘끊을 수 없는 구원’에 대한 직접적인 대답은 아닙니다."
          },
          {
            id: "C",
            text: "로마서 8:36 – 우리가 도살당할 양과 같이 여김을 받았나이다",
            correct: false,
            feedback:
              "고난의 현실을 보여주지만, 구원의 능력에 대한 승리 선언은 아닙니다."
          },
          {
            id: "D",
            text: "로마서 8:38–39 – 어떤 것도 우리를 하나님의 사랑에서 끊을 수 없다",
            correct: true,
            feedback:
              "정답입니다! 이 구절은 그리스도 예수 안에 있는 하나님의 사랑에서 우리를 끊을 수 있는 것이 아무것도 없다고 선언합니다."
          }
        ],
        hint: "로마서 8장 전체를 훑어보면서, ‘어떤 것도 끊을 수 없다’는 표현을 찾아보세요.",
        armorKey: "sword"
      }
    ];

    const locations = [
      null,
      "장소 1: 교회 입구(또는 지정한 장소 1)에 이동해서 팀 전체가 나온 사진을 찍고 업로드하세요.",
      "장소 2: 본당 안(또는 지정한 장소 2)에서 말씀을 상징하는 곳 앞에서 사진을 찍고 업로드하세요.",
      "장소 3: 교육관 또는 교실(장소 3) 앞에서 팀이 함께 서 있는 사진을 업로드하세요.",
      "장소 4: 교회 십자가가 보이는 지점(또는 장소 4)에서 사진을 찍어 업로드하세요.",
      "장소 5: 친교 공간(또는 지정한 장소 5)에서 서로 웃는 모습의 사진을 업로드하세요.",
      "장소 6: 마지막 모임 장소(또는 지정한 장소 6)에서 마무리 사진을 찍고 업로드하세요."
    ];

    const armorNames = {
      belt: "진리의 허리띠",
      breastplate: "의의 흉배",
      shoes: "평안의 신",
      shield: "믿음의 방패",
      helmet: "구원의 투구",
      sword: "성령의 검"
    };

    let currentIndex = 0;
    let playerName = "";
    const collected = new Set();
    const answered = new Set();
    let wrongAttempts = 0;
    let noMoreHints = false;
    let pendingNextIndex = null;

    const roomContentEl = document.getElementById("roomContent");
    const progressFill = document.getElementById("progressFill");
    const stepIndicator = document.getElementById("stepIndicator");
    const inventoryBadges = document.getElementById("inventoryBadges");
    const completionText = document.getElementById("completionText");
    const headerSub = document.getElementById("headerSub");

    const startOverlay = document.getElementById("startOverlay");
    const startBtn = document.getElementById("startBtn");
    const playerNameInput = document.getElementById("playerName");

    const locationOverlay = document.getElementById("locationOverlay");
    const locationText = document.getElementById("locationText");
    const photoInput = document.getElementById("photoInput");
    const locationNextBtn = document.getElementById("locationNextBtn");

    const characterFigure = document.getElementById("characterFigure");
    const characterGearRow = document.getElementById("characterGearRow");

    startBtn.addEventListener("click", () => {
      playerName = playerNameInput.value.trim();
      if (playerName) {
        headerSub.textContent = `${playerName}의 전신갑주 방탈출 퀘스트`;
      }
      startOverlay.style.display = "none";
      renderRoom();
    });

    function renderInventory() {
      inventoryBadges.innerHTML = "";
      Object.keys(armorNames).forEach((key) => {
        const badge = document.createElement("div");
        const has = collected.has(key);
        badge.className = "badge" + (has ? " collected" : "");
        badge.innerHTML = `
          <div class="badge-dot"></div>
          <span>${armorNames[key]}</span>
        `;
        inventoryBadges.appendChild(badge);
      });
    }

    function renderCharacter() {
      // 중앙 캐릭터 그림
      characterFigure.innerHTML = "🧍";

      // 이미 올려둔 장비 아이콘 제거
      const oldIcons = characterFigure.querySelectorAll(".character-gear-icon");
      oldIcons.forEach((el) => el.remove());

      function addIcon(emoji, extraClass) {
        const span = document.createElement("span");
        span.className = "character-gear-icon " + extraClass;
        span.textContent = emoji;
        characterFigure.appendChild(span);
      }

      if (collected.has("belt"))      addIcon("🧵", "gear-belt");
      if (collected.has("breastplate")) addIcon("🛡️", "gear-breast");
      if (collected.has("shoes"))     addIcon("👢", "gear-shoes");
      if (collected.has("shield"))    addIcon("🛡️", "gear-shield");
      if (collected.has("helmet"))    addIcon("🪖", "gear-helmet");
      if (collected.has("sword"))     addIcon("⚔️", "gear-sword");

      // 아래 장비 목록 태그
      characterGearRow.innerHTML = "";
      Object.entries(armorNames).forEach(([key, label]) => {
        const div = document.createElement("div");
        div.className = "gear-tag" + (collected.has(key) ? " on" : "");
        div.textContent = label;
        characterGearRow.appendChild(div);
      });
    }

    function updateProgress() {
      const step = currentIndex + 1;
      const total = rooms.length;
      const pct = (step / total) * 100;
      progressFill.style.width = pct + "%";
      stepIndicator.textContent = `${step} / ${total} 단계`;
      if (collected.size === Object.keys(armorNames).length) {
        completionText.innerHTML =
          "🎉 전신갑주 <strong>6개를 모두 모았습니다!</strong> 이제 바울과 함께 복음을 따라 담대히 서 있을 수 있습니다.";
      } else {
        completionText.innerHTML =
          "전신갑주 6개를 모두 모으면 <strong>엔딩 메시지</strong>가 열립니다.";
      }
    }

    function renderRoom() {
      const room = rooms[currentIndex];
      updateProgress();
      renderInventory();
      renderCharacter();

      let difficultyBadge =
        room.type === "easy"
          ? '<span class="badge-easy">EASY</span>'
          : '<span class="badge-hard">HARD</span>';

      roomContentEl.innerHTML = `
        <div class="room-label">${room.label}</div>
        <h2 class="room-title">
          ${room.title}
          ${difficultyBadge}
        </h2>
        <p class="room-subtitle">${room.subtitle}</p>

        <div class="section-label">단서</div>
        <div class="clue-box">
          <ul>
            ${room.clues.map((c) => `<li>${c}</li>`).join("")}
          </ul>
        </div>

        <div class="section-label">퀘스트</div>
        <p class="question-text">${room.question}</p>

        <div class="choices">
          ${room.choices
            .map(
              (choice) => `
            <button class="choice-btn" data-id="${choice.id}">
              <span class="label">${choice.id}</span>
              <span>${choice.text}</span>
            </button>
          `
            )
            .join("")}
        </div>

        <div class="hint-row">
          <button class="hint-btn" id="hintBtn">힌트 보기</button>
          <div class="hint-text" id="hintText"></div>
        </div>

        <div class="feedback" id="feedback"></div>

        <div class="footer-row">
          <button class="nav-btn" id="prevBtn" ${
            currentIndex === 0 ? "disabled" : ""
          }>← 이전</button>
          <button class="nav-btn primary" id="nextBtn" disabled>
            ${currentIndex === rooms.length - 1 ? "엔딩 보기 →" : "다음 방으로 →"}
          </button>
        </div>
      `;

      const choiceButtons = roomContentEl.querySelectorAll(".choice-btn");
      const feedbackEl = document.getElementById("feedback");
      const hintBtn = document.getElementById("hintBtn");
      const hintText = document.getElementById("hintText");
      const prevBtn = document.getElementById("prevBtn");
      const nextBtn = document.getElementById("nextBtn");

      // 힌트: 처음에는 숨김
      hintBtn.style.display = "none";
      if (noMoreHints) {
        hintText.textContent = "힌트 사용 가능 횟수를 모두 소진했습니다.";
      }

      hintBtn.addEventListener("click", () => {
        if (!noMoreHints) {
          hintText.textContent = room.hint;
        }
      });

      choiceButtons.forEach((btn) => {
        btn.addEventListener("click", () => {
          if (answered.has(room.id)) return;
          const id = btn.getAttribute("data-id");
          const choice = room.choices.find((c) => c.id === id);
          choiceButtons.forEach((b) => b.classList.remove("correct", "wrong"));

          if (choice.correct) {
            btn.classList.add("correct");
            feedbackEl.className = "feedback ok";
            feedbackEl.textContent = choice.feedback;
            answered.add(room.id);
            collected.add(room.armorKey);
            renderInventory();
            renderCharacter();
            nextBtn.disabled = false;
          } else {
            btn.classList.add("wrong");
            feedbackEl.className = "feedback no";
            feedbackEl.textContent = choice.feedback;

            wrongAttempts++;
            if (wrongAttempts >= 3) {
              noMoreHints = true;
              hintBtn.style.display = "none";
              hintText.textContent = "힌트 사용 가능 횟수를 모두 소진했습니다.";
            } else {
              if (!noMoreHints) {
                hintBtn.style.display = "inline-flex";
              }
            }
          }
        });
      });

      prevBtn.addEventListener("click", () => {
        if (currentIndex > 0) {
          currentIndex -= 1;
          renderRoom();
        }
      });

      nextBtn.addEventListener("click", () => {
        if (currentIndex < rooms.length - 1) {
          const targetIndex = currentIndex + 1;
          pendingNextIndex = targetIndex;
          const desc = locations[targetIndex] || "다음 장소로 이동한 뒤 사진을 업로드하세요.";
          locationText.textContent = desc;
          photoInput.value = "";
          locationNextBtn.disabled = true;
          locationOverlay.style.display = "flex";
        } else {
          // 엔딩
          roomContentEl.innerHTML = `
            <div class="room-label">ENDING · 전신갑주 완성</div>
            <h2 class="room-title">
              모든 방을 탈출했습니다!
            </h2>
            <p class="room-subtitle">
              ${
                playerName
                  ? `${playerName}은(는) 바울과 함께 전신갑주 6개를 모두 모았습니다.`
                  : "당신은 바울과 함께 전신갑주 6개를 모두 모았습니다."
              }
              이제 진리와 의, 평안, 믿음, 구원, 말씀으로 무장한
              하나님의 전사로 설 준비가 되었습니다.
            </p>

            <div class="section-label">획득한 전신갑주</div>
            <div class="clue-box">
              <ul>
                ${Object.keys(armorNames)
                  .map((k) => `<li>${armorNames[k]}</li>`)
                  .join("")}
              </ul>
            </div>

            <p class="question-text" style="margin-top:14px;">
              이 방탈출을 함께 한 청년들과, 각 전신갑주가 <strong>자기 삶의 어떤 영역</strong>에
              필요할지 나누어 보세요. 오늘 내게 가장 필요한 전신갑주는 무엇인가요?
            </p>

            <div class="footer-row">
              <button class="nav-btn primary" id="restartBtn">다시 하기 ↺</button>
            </div>
          `;

          const restartBtn = document.getElementById("restartBtn");
          restartBtn.addEventListener("click", () => {
            currentIndex = 0;
            collected.clear();
            answered.clear();
            wrongAttempts = 0;
            noMoreHints = false;
            startOverlay.style.display = "flex";
            playerNameInput.value = "";
            headerSub.textContent =
              "바울의 전도여행과 전신갑주로 떠나는 방탈출 퀘스트";
            renderCharacter();
          });
          renderInventory();
          renderCharacter();
          updateProgress();
        }
      });

      photoInput.addEventListener("change", () => {
        if (photoInput.files && photoInput.files.length > 0) {
          locationNextBtn.disabled = false;
        } else {
          locationNextBtn.disabled = true;
        }
      });

      locationNextBtn.onclick = () => {
        if (pendingNextIndex != null) {
          currentIndex = pendingNextIndex;
          pendingNextIndex = null;
          locationOverlay.style.display = "none";
          renderRoom();
        }
      };
    }
  </script>
</body>
</html>
