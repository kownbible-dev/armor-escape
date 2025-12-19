<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <title>전신갑주를 취하라</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    :root{
      --bg:#050816;
      --card:#111827;
      --accent:#fbbf24;
      --accent-soft:rgba(251,191,36,.14);
      --text:#f9fafb;
      --muted:#9ca3af;
      --danger:#f97373;
      --success:#4ade80;

      /* ✅ 큰(장착/엔딩) 캐릭터 크기 */
      --big-box: min(320px, 82vw);
      --big-font: min(150px, 50vw);
      --big-radius: 50px;

      /* ✅ 큰 화면 장비(이모지) 크기 */
      --sz-helmet: 56px;
      --sz-breast: 64px;
      --sz-shield: 78px;
      --sz-shoes: 58px;
      --sz-sword: 64px;
      --sz-belt: 180px;

      /* ✅ 큰 화면 장비 위치(%) */
      --y-helmet: 6%;
      --y-breast: 34%;
      --y-shield: 52%;
      --y-belt: 55%;
      --y-shoes: 82%;
      --y-sword: 46%;
    }

    *{box-sizing:border-box;}
    body{
      margin:0;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: radial-gradient(circle at top, #111827 0, #020617 55%);
      color: var(--text);
      display:flex;
      justify-content:center;
      min-height:100vh;
      padding:16px;
    }

    .app{
      width:100%;
      max-width:840px;
      background: linear-gradient(145deg,#020617,#020617);
      border-radius:24px;
      padding:20px 18px 24px;
      box-shadow:0 30px 80px rgba(0,0,0,.7);
      border:1px solid rgba(148,163,184,.2);
      position:relative;
      overflow:hidden;
    }
    @media (min-width:720px){ .app{padding:28px 28px 30px;} }

    .header{display:flex;align-items:center;gap:12px;margin-bottom:16px;}
    .logo-circle{
      width:38px;height:38px;border-radius:999px;border:2px solid var(--accent);
      display:flex;align-items:center;justify-content:center;font-size:18px;color:var(--accent);
      box-shadow:0 0 18px rgba(251,191,36,.35);
    }
    .header-text h1{margin:0;font-size:18px;letter-spacing:.06em;text-transform:uppercase;color:var(--accent);}
    .header-text p{margin:2px 0 0;font-size:13px;color:var(--muted);}

    .status-bar{display:flex;align-items:center;gap:12px;margin-bottom:18px;flex-wrap:wrap;}
    .progress{
      flex:1;height:8px;background:#020617;border-radius:999px;overflow:hidden;
      border:1px solid rgba(148,163,184,.4);
    }
    .progress-fill{height:100%;width:0%;background:linear-gradient(90deg,var(--accent),#f97316);transition:width .35s ease-out;}
    .step-indicator{font-size:12px;color:var(--muted);min-width:105px;text-align:right;}

    .inventory{
      background:rgba(15,23,42,.9);
      border-radius:16px;padding:10px 12px;
      border:1px solid rgba(148,163,184,.25);
      margin-bottom:12px;
    }
    .inventory-title{font-size:12px;text-transform:uppercase;letter-spacing:.14em;color:var(--muted);margin-bottom:6px;}
    .badge-row{display:flex;flex-wrap:wrap;gap:6px;}
    .badge{
      font-size:11px;padding:4px 9px;border-radius:999px;
      border:1px solid rgba(148,163,184,.3);
      background: radial-gradient(circle at top left,#1f2937 0,#020617 72%);
      color:var(--muted);
      display:flex;align-items:center;gap:5px;
    }
    .badge.collected{
      border-color:rgba(251,191,36,.8);
      color:var(--accent);
      background: radial-gradient(circle at top left,#451a03 0,#020617 72%);
      box-shadow:0 0 14px rgba(251,191,36,.4);
    }
    .badge-dot{width:7px;height:7px;border-radius:50%;background:rgba(148,163,184,.6);}
    .badge.collected .badge-dot{background:var(--accent);}

    /* ✅ 문제 위 작은 캐릭터(패널) 완전 숨김 (DOM은 남겨둬야 gear-overlay/엔딩 복사가 가능) */
    .character-panel{display:none !important;}
    .character-figure{
      width:64px;height:64px;border-radius:16px;font-size:30px;
      position:relative;flex-shrink:0;overflow:hidden;
    }

    .base-emoji{display:block;line-height:1;position:relative;z-index:1;}

    /* 장비 아이콘 공통 */
    .character-gear-icon{position:absolute;pointer-events:none;}
    .character-gear-icon img{width:26px;height:26px;object-fit:contain;display:block;}

    /* ===== 카드(문제 UI) ===== */
    .card{
      background: radial-gradient(circle at top,#1f2937 0,#020617 58%);
      border-radius:22px;padding:18px 16px 18px;
      border:1px solid rgba(148,163,184,.35);
      position:relative;overflow:hidden;
    }
    @media(min-width:720px){ .card{padding:22px 22px 22px;} }
    .card::before{
      content:"";position:absolute;inset:-80px;
      background: radial-gradient(circle at top right, rgba(251,191,36,.18), transparent 62%);
      opacity:.75;pointer-events:none;
    }
    .card-inner{position:relative;z-index:1;}

    .room-label{font-size:11px;text-transform:uppercase;letter-spacing:.2em;color:var(--muted);margin-bottom:6px;}
    .room-title{margin:0;font-size:17px;display:flex;align-items:center;gap:8px;}
    .room-title span.badge-hard,.room-title span.badge-easy{
      font-size:11px;text-transform:uppercase;letter-spacing:.12em;border-radius:999px;padding:3px 7px;
    }
    .room-title span.badge-hard{border:1px solid rgba(248,113,113,.8);color:#fecaca;background:rgba(127,29,29,.4);}
    .room-title span.badge-easy{border:1px solid rgba(45,212,191,.8);color:#a5f3fc;background:rgba(6,78,59,.55);}
    .room-subtitle{margin:6px 0 14px;font-size:14px;color:var(--muted);line-height:1.5;}

    .section-label{font-size:12px;font-weight:600;text-transform:uppercase;letter-spacing:.18em;margin:10px 0 6px;color:var(--muted);}
    .clue-box{
      background: rgba(15,23,42,.92);
      border-radius:16px;padding:10px 11px;
      border:1px solid rgba(148,163,184,.25);
      font-size:13px;line-height:1.6;
    }
    .clue-box ul{padding-left:18px;margin:4px 0 0;}
    .clue-box li{margin-bottom:4px;}
    .question-text{margin:10px 0 8px;font-size:14px;line-height:1.6;}

    .choices{margin-top:6px;display:flex;flex-direction:column;gap:6px;}
    .choice-btn{
      width:100%;text-align:left;border-radius:999px;padding:8px 12px;
      font-size:13px;border:1px solid rgba(148,163,184,.35);
      background: rgba(15,23,42,.96);color:var(--text);
      cursor:pointer;display:flex;gap:8px;align-items:center;
      transition: all .16s ease-out;
    }
    .choice-btn span.label{
      font-size:11px;width:22px;height:22px;border-radius:999px;
      border:1px solid rgba(148,163,184,.65);
      display:inline-flex;align-items:center;justify-content:center;flex-shrink:0;
    }
    .choice-btn:hover{border-color:rgba(251,191,36,.85);box-shadow:0 0 0 1px rgba(251,191,36,.25);transform:translateY(-1px);}
    .choice-btn.correct{border-color:rgba(74,222,128,.85);background:rgba(22,101,52,.8);}

    .hint-row{margin-top:10px;display:flex;gap:8px;flex-wrap:wrap;align-items:center;}
    .hint-btn{
      border-radius:999px;padding:6px 12px;font-size:12px;
      background: rgba(15,23,42,.9);color:var(--accent);
      border:1px dashed rgba(251,191,36,.6);cursor:pointer;display:none;
    }
    .hint-text{font-size:12px;color:var(--muted);flex:1;min-width:180px;}

    .feedback{margin-top:10px;font-size:13px;line-height:1.6;}
    .feedback.ok{color:var(--success);}
    .feedback.no{color:var(--danger);}

    .footer-row{margin-top:16px;display:flex;justify-content:space-between;gap:10px;flex-wrap:wrap;align-items:center;}
    .nav-btn{
      border-radius:999px;padding:8px 16px;font-size:13px;border:none;cursor:pointer;
      display:inline-flex;align-items:center;gap:6px;background:var(--accent-soft);color:var(--accent);
    }
    .nav-btn.primary{
      background: linear-gradient(135deg,#f97316,#fbbf24);
      color:#111827;font-weight:600;box-shadow:0 12px 30px rgba(248,181,55,.5);
    }
    .nav-btn:disabled{opacity:.4;cursor:default;box-shadow:none;}

    .completion{text-align:center;padding:16px 8px 4px;font-size:14px;color:var(--muted);}
    .completion strong{color:var(--accent);}

    /* ===== 오버레이 공통 ===== */
    .start-overlay,.location-overlay,.gear-overlay{
      position:absolute;inset:0;
      background: radial-gradient(circle at top, rgba(15,23,42,.98), #020617);
      display:flex;flex-direction:column;justify-content:center;align-items:center;
      padding:24px 18px;z-index:10;
    }

    .start-card,.location-card,.gear-card{
      max-width:420px;width:100%;
      background: rgba(15,23,42,.96);
      border-radius:22px;padding:20px 18px 22px;
      border:1px solid rgba(148,163,184,.35);
      box-shadow:0 24px 60px rgba(0,0,0,.7);
      text-align:center;
    }

    /* ✅ 시작 화면도 "문제 카드" 느낌으로 크게 */
    .start-card{
      max-width:840px;
      background: radial-gradient(circle at top,#1f2937 0,#020617 58%);
    }

    .start-title,.location-title,.gear-title{font-size:18px;margin:0 0 6px;color:var(--accent);}
    .start-sub,.location-sub,.gear-sub{font-size:13px;color:var(--muted);margin:0 0 14px;line-height:1.6;}

    .start-label{font-size:12px;color:var(--muted);text-align:left;margin-bottom:4px;}
    .start-input{
      width:100%;border-radius:999px;border:1px solid rgba(148,163,184,.5);
      padding:8px 12px;font-size:13px;background:#020617;color:var(--text);outline:none;
    }
    .start-input::placeholder{color:rgba(148,163,184,.7);}

    .start-btn,.location-btn,.gear-btn{
      margin-top:14px;width:100%;
      border-radius:999px;border:none;padding:9px 12px;font-size:14px;
      background: linear-gradient(135deg,#f97316,#fbbf24);
      color:#111827;font-weight:600;cursor:pointer;
      box-shadow:0 14px 36px rgba(248,181,55,.6);
    }
    .start-hint,.location-hint{margin-top:8px;font-size:11px;color:var(--muted);text-align:left;}

    .location-overlay{display:none;}
    .gear-overlay{display:none;}

    .location-file{width:100%;margin-top:6px;font-size:12px;color:var(--muted);text-align:left;}
    .location-file input{margin-top:4px;width:100%;font-size:12px;color:var(--muted);}
    .location-btn:disabled{opacity:.4;cursor:default;box-shadow:none;}

    /* ===== 장비 장착(큰) 화면 ===== */
    .gear-card{
      max-width:560px;
      padding:30px 24px 30px;
      border-radius:28px;
      box-shadow:0 32px 80px rgba(0,0,0,.85);
    }

    .gear-figure{
      margin:18px auto 12px;
      width: var(--big-box);
      height: var(--big-box);
      font-size: var(--big-font);
      border-radius: var(--big-radius);
      background: radial-gradient(circle at top,#1f2937 0,#020617 80%);
      display:flex;align-items:center;justify-content:center;
      position:relative;overflow:hidden;
      border:3px solid rgba(251,191,36,.9);
      box-shadow:0 0 42px rgba(251,191,36,.6);
    }

    /* ✅ 큰 화면 베이스 이모지(키/비율) */
    .gear-figure .base-emoji{
      font-size:1em;       /* gear-figure font-size 그대로 */
      line-height:1;
      transform: scaleY(1.25); /* 인형 비율 */
      transform-origin:center;
    }

    /* 큰 화면: 이미지 아이콘(벨트) 기본 */
    .gear-figure .character-gear-icon img{
      width:78px;height:78px;object-fit:contain;
    }

    /* ===== 큰 화면 장비 위치/크기 ===== */
    .gear-figure .gear-helmet{
      left:50%; top: 12%;
      transform: translateX(-50%) rotate(-4deg);
      z-index:6; font-size: 46px;
    }
    .gear-figure .gear-breast{
      left:50%; top:50%;
      transform: translate(-50%,-50%);
      z-index:4; font-size: 62px;
    }
    .gear-figure .gear-shield{
      left:50%; top:46%;
      transform: translate(-50%,-50%);
      z-index:9; font-size: 70px;
    }
    .gear-figure .gear-shoes{
      left:50%; top: 85%;
      transform: translate(-50%,-50%);
      z-index:3; font-size: 34px;
    }
    .gear-figure .gear-sword{
      right:16%; top: 25%;
      z-index:8; font-size: 58px;
    }

    /* ✅ 벨트(큰 화면): 투명 크롭 PNG이므로 “잘라내기” 불필요 */
    .gear-figure .gear-belt{
      left:50%; top: 57%;
      transform: translate(-50%,-50%);
      z-index:5;
      fontsize:20px;
    }
    .gear-figure .gear-belt img{
      width: 52px;
      height:auto;
      object-fit:contain;
      filter: drop-shadow(0 6px 10px rgba(0,0,0,.55));
    }

    /* ===== 숨겨진 작은 캐릭터용(복사용) 위치 ===== */
    .character-figure .gear-helmet{left:50%;top:2%;transform:translateX(-50%);z-index:6;}
    .character-figure .gear-breast{left:50%;top:38%;transform:translate(-50%,-50%);z-index:4;}
    .character-figure .gear-belt{left:50%;top:56%;transform:translate(-50%,-50%);z-index:5;}
    .character-figure .gear-shoes{left:50%;top:88%;transform:translate(-50%,-50%);z-index:3;}
    .character-figure .gear-shield{left:50%;top:52%;transform:translate(-50%,-50%);z-index:7;}
    .character-figure .gear-sword{right:4px;top:42%;z-index:6;}

    .character-figure .gear-belt img{
      width:56px;height:auto;object-fit:contain;
      filter: drop-shadow(0 6px 10px rgba(0,0,0,.55));
    }

    /* ===== 엔딩: 풀착장 큰 캐릭터 ===== */
    .ending-figure-wrap{margin:14px auto 10px;display:flex;justify-content:center;}
    .ending-figure{
      width: var(--big-box);
      height: var(--big-box);
      font-size: var(--big-font);
      border-radius: var(--big-radius);
      background: radial-gradient(circle at top,#1f2937 0,#020617 80%);
      display:flex;align-items:center;justify-content:center;
      position:relative;overflow:hidden;
      border:3px solid rgba(251,191,36,.9);
      box-shadow:0 0 42px rgba(251,191,36,.6);
    }
    .ending-figure .base-emoji{
      font-size:1em;line-height:1;
      transform: scaleY(1.22);
      transform-origin:center;
    }

    /* ===== ✅ 시작 버튼 팝 연출(전역: @media 밖) ===== */
    .start-card{transform:translateZ(0);will-change:transform,opacity;}
    .start-card.start-pop{animation:startPop 420ms cubic-bezier(.2,.9,.2,1) both;}
    @keyframes startPop{
      0%{transform:scale(1);opacity:1;}
      55%{transform:scale(1.04);opacity:1;}
      100%{transform:scale(1);opacity:1;}
    }

    /* 모바일 최적화(필요한 것만) */
    @media (max-width:480px){
      :root{
        --big-box: min(340px, 86vw);
        --big-font: min(150px, 48vw);
        --sz-belt: min(200px, 62vw);
      }
      .gear-card{padding:26px 18px 26px;}
      .start-card{max-width:840px;}
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
      <div class="progress"><div class="progress-fill" id="progressFill"></div></div>
      <div class="step-indicator" id="stepIndicator">1 / 6 단계</div>
    </div>

    <div class="inventory">
      <div class="inventory-title">획득한 전신갑주</div>
      <div class="badge-row" id="inventoryBadges"></div>
    </div>

    <!-- ✅ 숨김(복사용) 캐릭터 패널 -->
    <div class="character-panel">
      <div class="character-figure" id="characterFigure"><span class="base-emoji">🧍</span></div>
      <div class="character-info">
        <div class="character-info-title">내 캐릭터 장비 상태</div>
        <div class="character-gear-row" id="characterGearRow"></div>
      </div>
    </div>

    <div class="card">
      <div class="card-inner"><div id="roomContent"></div></div>
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
        <button class="start-btn" id="startBtn" type="button">퀘스트 시작하기 →</button>
        <div class="start-hint">
          성경(종이 또는 앱)을 옆에 두고, 단서를 따라가며 정답을 찾아보세요.
        </div>
      </div>
    </div>

    <!-- 장소 이동 + 사진/영상 업로드 게이트 -->
    <div class="location-overlay" id="locationOverlay">
      <div class="location-card">
        <h2 class="location-title" id="locationTitle">다음 장소로 이동!</h2>
        <p class="location-sub" id="locationText">장소 안내 문구가 여기에 표시됩니다.</p>
        <div class="location-file">
          <div>📷 위 지시를 따라 <strong>사진 또는 영상</strong>을 준비하고 업로드하세요.</div>
          <input type="file" id="photoInput" accept="image/*,video/*" />
        </div>
        <button class="location-btn" id="locationNextBtn" disabled type="button">이제 문제 풀기 →</button>
        <div class="location-hint">
          * 업로드한 파일은 이 브라우저 안에서만 사용되고,<br />
          &nbsp;실제 서버에는 저장되지 않습니다. (리더가 현장에서만 확인해 주세요)
        </div>
      </div>
    </div>

    <!-- 장비 장착 연출 화면 -->
    <div class="gear-overlay" id="gearOverlay">
      <div class="gear-card">
        <h2 class="gear-title" id="gearTitle">새 장비를 획득했습니다!</h2>
        <p class="gear-sub" id="gearSub">전신갑주 조각이 장착되었습니다.</p>
        <div class="gear-figure" id="gearFigure">
          <span class="base-emoji">🧍</span>
        </div>
        <button class="gear-btn" id="gearNextBtn" type="button">다음 장소로 이동 →</button>
      </div>
    </div>
  </div>

<script>
  /* ✅ 업로드 링크(구글 드라이브 / 네이버 클라우드 등) 여기만 바꾸면 됨 */
  const uploadUrl = "https://drive.google.com/drive/folders/13xmf4DbCG1tfWdAWf4lDQbHTLjq6UEPp?usp=drive_link";

  const rooms = [
    {
      id: 1, type: "easy",
      label: "ROOM 1 · 진리의 허리띠",
      title: "안디옥 기록실에서 탈출하라",
      subtitle: "안디옥 교회에 거짓 가르침이 들어왔습니다. 참된 복음을 분명히 붙잡아야만 이 방을 나갈 수 있습니다.",
      clues: [
        "후보 구절: 갈 2:16 / 갈 3:11 / 갈 3:2",
        "키워드: ‘믿음으로 의롭다’ vs ‘율법의 행위’",
        "“예수님 + 아무것도 더하지 않는다”가 복음의 핵심"
      ],
      question: "세 단서를 모두 만족하는, ‘복음의 진리’를 가장 분명하게 말하는 구절은 어느 것일까요?",
      choices: [
        { id:"A", text:"갈라디아서 3:11", correct:false, feedback:"갈 3:11도 믿음에 대해 말하지만, 구조가 완전히 드러나지 않아요." },
        { id:"B", text:"갈라디아서 3:2",  correct:false, feedback:"갈 3:2는 시작은 좋지만 더 정확한 요약이 한 곳에 있습니다." },
        { id:"C", text:"갈라디아서 2:16", correct:true,  feedback:"정답입니다! 갈 2:16은 복음을 가장 명확하게 선포합니다." }
      ],
      hint:"갈라디아서 2–3장을 훑어보면서 ‘율법의 행위’와 ‘믿음으로 의롭다’가 동시에 나오는 절을 찾아보세요.",
      armorKey:"belt"
    },
    {
      id: 2, type: "easy",
      label: "ROOM 2 · 의의 흉배",
      title: "루스드라 광장에서 다시 일어서는 이유",
      subtitle: "바울은 루스드라에서 돌에 맞아 거의 죽을 뻔했지만, 다시 그 도시로 들어갑니다. 그는 왜 위험을 감수하고 돌아갔을까요?",
      clues: [
        "“우리가 하나님 나라에 들어가려면 많은 환난을 겪어야 할 것이라”(사도행전 14장 내용)",
        "바울의 관심은 ‘자기 안전’보다 ‘하나님 나라와 복음’",
        "환난조차도 의로운 목적을 막지 못한다"
      ],
      question: "바울이 죽을 뻔한 상황에서도 다시 도시로 돌아간 가장 성경적인 이유는 무엇일까요?",
      choices: [
        { id:"A", text:"돌에 맞았지만, 사람들의 인정을 다시 받고 싶어서", correct:false, feedback:"바울의 동기는 사람들의 인정이 아니라 하나님 나라와 복음이었어요." },
        { id:"B", text:"그곳 음식을 더 먹고 싶어서 (농담)", correct:false, feedback:"이건 농담 같은 상상일 뿐, 본문과는 거리가 멀어요!" },
        { id:"C", text:"하나님 나라에 들어가기까지 환난을 감수하는 것이 의로운 길이기 때문에", correct:true, feedback:"맞아요! 행 14:22에 따르면 바울은 환난을 감수했습니다." }
      ],
      hint:"사도행전 14:19–22를 읽어 보세요.",
      armorKey:"breastplate"
    },
    {
      id: 3, type: "easy",
      label: "ROOM 3 · 평안의 복음의 신",
      title: "열린 감옥에서 도망가지 않은 선택",
      subtitle: "지진으로 문이 열리고 쇠사슬이 풀렸는데도, 바울과 실라는 감옥에서 도망가지 않습니다.",
      clues: [
        "간수: “내가 죽었구나!” (행 16:27)",
        "바울: “우리가 다 여기 있노라” (행 16:28)",
        "이 한 마디가 복음의 대화로 이어짐"
      ],
      question:"바울이 도망가지 않았다는 사실을 가장 분명하게 드러내는 구절은 어느 절일까요?",
      choices: [
        { id:"A", text:"사도행전 16:27", correct:false, feedback:"16:27은 간수의 반응입니다." },
        { id:"B", text:"사도행전 16:28", correct:true,  feedback:"정답입니다! “우리가 다 여기 있노라”는 직접 증거입니다." },
        { id:"C", text:"사도행전 16:31", correct:false, feedback:"구원의 길이지만 행동을 보여주는 절은 아닙니다." }
      ],
      hint:"행 16:25–34 전체를 읽어 보세요.",
      armorKey:"shoes"
    },
    {
      id: 4, type: "hard",
      label: "ROOM 4 · 믿음의 방패",
      title: "두 편지로 푸는 믿음의 비밀",
      subtitle: "바울은 박해 속에서도 낙심하지 않습니다. 그 이유는 사명과 영원한 것에 대한 시선 때문입니다.",
      clues: [
        "살전 2:4 – 복음을 맡겼으니…",
        "고후 4:16–18 – 보이는 것은 잠깐, 보이지 않는 것은 영원",
        "믿음의 방패 = 사명 + 영원한 것"
      ],
      question:"바울이 박해 가운데서도 낙심하지 않은 이유를 가장 잘 설명하는 선택지는?",
      choices: [
        { id:"A", text:"성격이 강해서 신경을 안 써서", correct:false, feedback:"성경은 사명과 영원한 소망을 강조합니다." },
        { id:"B", text:"복음을 맡겨진 자로서 영원한 영광을 바라보았기 때문에", correct:true, feedback:"정답입니다! 살전 2:4 + 고후 4:16–18의 결합입니다." },
        { id:"C", text:"고난 후 물질적 축복을 기대해서", correct:false, feedback:"바울은 영원한 영광을 붙잡았습니다." }
      ],
      hint:"살전 2:1–4와 고후 4장을 함께 읽어 보세요.",
      armorKey:"shield"
    },
    {
      id: 5, type: "hard",
      label: "ROOM 5 · 구원의 투구",
      title: "우상 도시 에베소에서 지켜야 할 확신",
      subtitle: "구원의 투구는 생각과 정체성을 지키는 방어구입니다.",
      clues: [
        "엡 2:8–9 – 은혜로, 믿음으로, 행위가 아님",
        "엡 6:17 – 구원의 투구",
        "구원의 근거는 은혜"
      ],
      question:"‘구원의 투구’가 말하는 구원의 본질을 가장 정확히 설명하는 구절은?",
      choices: [
        { id:"A", text:"에베소서 6:17", correct:false, feedback:"목록이지만 구원의 근거 설명은 아닙니다." },
        { id:"B", text:"에베소서 2:8–9", correct:true,  feedback:"정답입니다! 구원의 본질을 가장 명확히 정리합니다." },
        { id:"C", text:"에베소서 4:1", correct:false, feedback:"삶의 방식 권면입니다." }
      ],
      hint:"엡 2장을 먼저 읽고 6장을 다시 보세요.",
      armorKey:"helmet"
    },
    {
      id: 6, type: "hard",
      label: "ROOM 6 · 성령의 검",
      title: "로마 황제 앞에서 휘두르는 마지막 한 검",
      subtitle: "황제의 도전에 ‘끊을 수 없는 구원’을 선포해야 합니다.",
      clues: [
        "로마서 8장",
        "후보: 8:26 / 8:28 / 8:36 / 8:38–39",
        "구원의 범위"
      ],
      question:"황제의 질문에 가장 논리적으로 반박할 수 있는 구절은?",
      choices: [
        { id:"A", text:"로마서 8:26", correct:false, feedback:"위로지만 범위 반박은 약합니다." },
        { id:"B", text:"로마서 8:28", correct:false, feedback:"약속이지만 끊을 수 없음 선언은 아닙니다." },
        { id:"C", text:"로마서 8:36", correct:false, feedback:"현실이지만 승리 선언은 아닙니다." },
        { id:"D", text:"로마서 8:38–39", correct:true,  feedback:"정답입니다! 어떤 것도 끊을 수 없다고 선언합니다." }
      ],
      hint:"로마서 8장에서 ‘끊을 수 없다’를 찾아보세요.",
      armorKey:"sword"
    }
  ];

  const locationTitles = [
    "첫번째 문제",
    "성가1연습실로 이동",
    "1층 로비",
    "3층 유초등부",
    "청년부실로 이동",
    "비밀의 장소로 이동"
  ];
  const locations = [
    "갈라디아서 2장 16절 말씀 퍼즐 완성하세요.",
    "도미노를 완성하시오.",
    "간수와 바울 역할극을 촬영하여 영상을 업로드 하시오.",
    "사명, 소망 선언문을 작성하시오.",
    "안대 착용 후 에베소서 2장 8~9절을 조원들이 나누어서 암송하시오.",
    "하트 전등을 찾아 단체 사진을 찍으세요."
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
  let lastArmorKey = null;

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
  const locationTitleEl = document.getElementById("locationTitle");
  const photoInput = document.getElementById("photoInput");
  const locationNextBtn = document.getElementById("locationNextBtn");

  const gearOverlay = document.getElementById("gearOverlay");
  const gearTitle = document.getElementById("gearTitle");
  const gearSub = document.getElementById("gearSub");
  const gearFigure = document.getElementById("gearFigure");
  const gearNextBtn = document.getElementById("gearNextBtn");

  const characterFigure = document.getElementById("characterFigure");
  const characterGearRow = document.getElementById("characterGearRow");

  startBtn.addEventListener("click", () => {
    playerName = playerNameInput.value.trim();
    if (playerName) headerSub.textContent = `${playerName}의 전신갑주 방탈출 퀘스트`;

    const startCard = document.querySelector(".start-card");
    startBtn.disabled = true;

    const goNext = () => {
      startOverlay.style.display = "none";
      pendingNextIndex = 0;
      showLocationGate(pendingNextIndex);
      startBtn.disabled = false;
      if (startCard) startCard.classList.remove("start-pop");
    };

    if (!startCard) { goNext(); return; }

    startCard.classList.add("start-pop");
    startCard.addEventListener("animationend", goNext, { once:true });

    // ✅ fallback (animationend가 안 와도 시작되게)
    setTimeout(goNext, 480);
  });

  function renderInventory(){
    inventoryBadges.innerHTML = "";
    Object.keys(armorNames).forEach((key) => {
      const badge = document.createElement("div");
      const has = collected.has(key);
      badge.className = "badge" + (has ? " collected" : "");
      badge.innerHTML = `<div class="badge-dot"></div><span>${armorNames[key]}</span>`;
      inventoryBadges.appendChild(badge);
    });
  }

  function renderCharacter(){
    // 숨겨진 캐릭터 DOM(복사용)
    characterFigure.innerHTML = '<span class="base-emoji">🧍</span>';

    function addIcon(content, extraClass, isImage=false){
      const span = document.createElement("span");
      span.className = "character-gear-icon " + extraClass;
      if (isImage){
        const img = document.createElement("img");
        img.src = content;
        img.alt = "";
        span.appendChild(img);
      }else{
        span.textContent = content;
      }
      characterFigure.appendChild(span);
    }

    // ✅ 벨트(투명 크롭 PNG)
    if (collected.has("belt")){
      addIcon("https://i.postimg.cc/HWbS3KXs/belt-transparent-cropped.png", "gear-belt", true);
    }

    if (collected.has("breastplate")) addIcon("🦺", "gear-breast");
    if (collected.has("shoes"))       addIcon("🥾", "gear-shoes");
    if (collected.has("shield"))      addIcon("🛡️", "gear-shield");
    if (collected.has("helmet"))      addIcon("⛑️", "gear-helmet");
    if (collected.has("sword"))       addIcon("⚔️", "gear-sword");

    characterGearRow.innerHTML = "";
    Object.entries(armorNames).forEach(([key,label])=>{
      const div = document.createElement("div");
      div.className = "gear-tag" + (collected.has(key) ? " on" : "");
      div.textContent = label;
      characterGearRow.appendChild(div);
    });
  }

  function updateProgress(){
    const step = currentIndex + 1;
    const total = rooms.length;
    progressFill.style.width = ((step / total) * 100) + "%";
    stepIndicator.textContent = `${step} / ${total} 단계`;

    if (collected.size === Object.keys(armorNames).length){
      completionText.innerHTML = "🎉 전신갑주 <strong>6개를 모두 모았습니다!</strong> 이제 바울과 함께 복음을 따라 담대히 서 있을 수 있습니다.";
    }else{
      completionText.innerHTML = "전신갑주 6개를 모두 모으면 <strong>엔딩 메시지</strong>가 열립니다.";
    }
  }

  function renderRoom(){
    const room = rooms[currentIndex];
    updateProgress();
    renderInventory();
    renderCharacter(); // ✅ gear-overlay/엔딩 복사용 DOM 항상 최신 유지

    const difficultyBadge = room.type==="easy"
      ? '<span class="badge-easy">EASY</span>'
      : '<span class="badge-hard">HARD</span>';

    roomContentEl.innerHTML = `
      <div class="room-label">${room.label}</div>
      <h2 class="room-title">${room.title} ${difficultyBadge}</h2>
      <p class="room-subtitle">${room.subtitle}</p>

      <div class="section-label">단서</div>
      <div class="clue-box">
        <ul>${room.clues.map(c=>`<li>${c}</li>`).join("")}</ul>
      </div>

      <div class="section-label">퀘스트</div>
      <p class="question-text">${room.question}</p>

      <div class="choices">
        ${room.choices.map(choice=>`
          <button class="choice-btn" type="button" data-id="${choice.id}">
            <span class="label">${choice.id}</span>
            <span>${choice.text}</span>
          </button>
        `).join("")}
      </div>

      <div class="hint-row">
        <button class="hint-btn" type="button" id="hintBtn">힌트 보기</button>
        <div class="hint-text" id="hintText"></div>
      </div>

      <div class="feedback" id="feedback"></div>

      <div class="footer-row">
        <button class="nav-btn" type="button" id="prevBtn" ${currentIndex===0?"disabled":""}>← 이전</button>
        <button class="nav-btn primary" type="button" id="nextBtn" disabled>
          ${currentIndex===rooms.length-1?"엔딩 보기 →":"다음 방으로 →"}
        </button>
      </div>
    `;

    const choiceButtons = roomContentEl.querySelectorAll(".choice-btn");
    const feedbackEl = document.getElementById("feedback");
    const hintBtn = document.getElementById("hintBtn");
    const hintText = document.getElementById("hintText");
    const prevBtn = document.getElementById("prevBtn");
    const nextBtn = document.getElementById("nextBtn");

    hintBtn.style.display = "none";
    hintText.textContent = noMoreHints ? "힌트 사용 가능 횟수를 모두 소진했습니다." : "";

    hintBtn.addEventListener("click", () => {
      if (!noMoreHints) hintText.textContent = room.hint;
    });

    choiceButtons.forEach(btn=>{
      btn.addEventListener("click", ()=>{
        if (answered.has(room.id)) return;

        // 초기화
        feedbackEl.textContent = "";
        feedbackEl.className = "feedback";
        if (!noMoreHints){
          hintText.textContent = "";
          hintBtn.style.display = "none";
        }

        const id = btn.getAttribute("data-id");
        const choice = room.choices.find(c=>c.id===id);
        choiceButtons.forEach(b=>b.classList.remove("correct"));

        if (choice.correct){
          btn.classList.add("correct");
          feedbackEl.className = "feedback ok";
          feedbackEl.textContent = choice.feedback;

          answered.add(room.id);
          collected.add(room.armorKey);
          lastArmorKey = room.armorKey;

          renderInventory();
          renderCharacter();
          nextBtn.disabled = false;

        }else{
          // 오답: 색/해설 표시 안 함
          wrongAttempts++;
          if (wrongAttempts >= 3){
            noMoreHints = true;
            hintBtn.style.display = "none";
            hintText.textContent = "힌트 사용 가능 횟수를 모두 소진했습니다.";
          }else{
            if (!noMoreHints) hintBtn.style.display = "inline-flex";
          }
        }
      });
    });

    prevBtn.addEventListener("click", ()=>{
      if (currentIndex > 0){
        currentIndex -= 1;
        renderRoom();
      }
    });

    nextBtn.addEventListener("click", ()=>{
      if (currentIndex < rooms.length - 1){
        pendingNextIndex = currentIndex + 1;
        showGearOverlay(lastArmorKey);
      }else{
        showEnding();
      }
    });
  }

  function showEnding(){
    // ✅ 엔딩에서는 풀착장 강제
    Object.keys(armorNames).forEach(k=>collected.add(k));
    renderInventory();
    renderCharacter();
    updateProgress();

    roomContentEl.innerHTML = `
      <div class="room-label">ENDING · 전신갑주 완성</div>
      <h2 class="room-title">모든 방을 탈출했습니다!</h2>
      <p class="room-subtitle">
        ${playerName ? `${playerName}은(는)` : "당신은"} 바울과 함께 전신갑주 6개를 모두 모았습니다.
        이제 진리와 의, 평안, 믿음, 구원, 말씀으로 무장한 하나님의 전사로 설 준비가 되었습니다.
      </p>

      <div class="ending-figure-wrap">
        <div class="ending-figure gear-figure" id="endingFigure">
          <span class="base-emoji">🧍</span>
        </div>
      </div>

      <div class="section-label">획득한 전신갑주</div>
      <div class="clue-box">
        <ul>${Object.keys(armorNames).map(k=>`<li>${armorNames[k]}</li>`).join("")}</ul>
      </div>

      <p class="question-text" style="margin-top:14px;">
        마지막 미션! 오늘 찍은 사진/영상 중 베스트 1개를 <strong>업로드 링크</strong>에 등록해 주세요.
      </p>

      <div class="footer-row">
        <button class="nav-btn" type="button" id="restartBtn">다시 하기 ↺</button>
        <button class="nav-btn primary" type="button" id="uploadBtn">업로드 링크 열기 📤</button>
      </div>
    `;

    // ✅ 엔딩 큰 캐릭터에 장비 아이콘 복사
    const endingFigure = document.getElementById("endingFigure");
    if (endingFigure){
      const iconsOnly = characterFigure.innerHTML.replace(/<span class="base-emoji">🧍<\/span>/g, "");
      endingFigure.innerHTML = `<span class="base-emoji">🧍</span>` + iconsOnly;
    }

    document.getElementById("restartBtn").addEventListener("click", ()=>{
      currentIndex = 0;
      collected.clear();
      answered.clear();
      wrongAttempts = 0;
      noMoreHints = false;
      playerNameInput.value = "";
      headerSub.textContent = "바울의 전도여행과 전신갑주로 떠나는 방탈출 퀘스트";
      startOverlay.style.display = "flex";
      renderInventory();
      renderCharacter();
      updateProgress();
    });

    document.getElementById("uploadBtn").addEventListener("click", ()=>{
      if (uploadUrl && uploadUrl.startsWith("http")){
        window.open(uploadUrl, "_blank");
      }else{
        alert("업로드 URL이 아직 설정되지 않았습니다.");
      }
    });
  }

  function showLocationGate(index){
    locationTitleEl.textContent = locationTitles[index] || "다음 장소로 이동";
    locationText.textContent = locations[index] || "다음 지시에 따라 진행하세요.";
    photoInput.value = "";
    locationNextBtn.disabled = true;
    locationOverlay.style.display = "flex";
  }

  function showGearOverlay(armorKey){
    if (!armorKey){
      if (pendingNextIndex != null) showLocationGate(pendingNextIndex);
      return;
    }

    gearTitle.textContent = `${armorNames[armorKey]} 획득!`;
    gearSub.textContent = `새로운 전신갑주 조각, '${armorNames[armorKey]}'를 장착했습니다.`;

    // ✅ 큰 화면 캐릭터: base-emoji + 장비 아이콘(숨겨진 characterFigure에서 복사)
    const iconsOnly = characterFigure.innerHTML.replace(/<span class="base-emoji">🧍<\/span>/g, "");
    gearFigure.innerHTML = `<span class="base-emoji">🧍</span>` + iconsOnly;

    gearOverlay.style.display = "flex";
  }

  photoInput.addEventListener("change", ()=>{
    locationNextBtn.disabled = !(photoInput.files && photoInput.files.length > 0);
  });

  locationNextBtn.addEventListener("click", ()=>{
    if (pendingNextIndex != null){
      currentIndex = pendingNextIndex;
      pendingNextIndex = null;
      locationOverlay.style.display = "none";
      renderRoom();
    }
  });

  gearNextBtn.addEventListener("click", ()=>{
    gearOverlay.style.display = "none";
    if (pendingNextIndex != null) showLocationGate(pendingNextIndex);
  });

  // 초기 표시(시작 전에도 뱃지 렌더)
  renderInventory();
  renderCharacter();
  updateProgress();
</script>
</body>
</html>
