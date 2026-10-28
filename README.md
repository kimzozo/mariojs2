<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>SUPER PIXEL BROS - 5 STAGES EDITION</title>
  <meta name="description" content="HTML5 Canvas와 순수 JavaScript로 제작된 슈퍼 마리오 스타일 5스테이지 2D 횡스크롤 플랫포머 웹 게임" />
  <meta name="theme-color" content="#111827" />
  <meta name="apple-mobile-web-app-capable" content="yes" />
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
  <meta property="og:title" content="SUPER PIXEL BROS - 5 STAGES EDITION" />
  <meta property="og:description" content="외부 에셋 없이 즉시 실행되는 5스테이지 레트로 2D 플랫포머 웹 게임" />
  <meta property="og:type" content="website" />
  <link rel="icon" type="image/svg+xml" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 32 32'%3E%3Crect width='32' height='32' rx='6' fill='%23dc2626'/%3E%3Ccircle cx='16' cy='16' r='10' fill='%23ffffff'/%3E%3Ctext x='16' y='21' font-family='monospace' font-size='15' font-weight='bold' text-anchor='middle' fill='%23dc2626'%3EM%3C/text%3E%3C/svg%3E" />
  <style>
    :root {
      --bg-dark: #0f172a;
      --panel-bg: #1e293b;
      --accent-gold: #fbbf24;
      --accent-red: #ef4444;
      --accent-green: #22c55e;
      --text-light: #f8fafc;
      --retro-font: 'Courier New', 'Consolas', 'Malgun Gothic', monospace;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      user-select: none;
      -webkit-user-select: none;
    }

    body {
      background: radial-gradient(circle at center, #1e293b 0%, #090d16 100%);
      color: var(--text-light);
      font-family: var(--retro-font);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 10px;
      overflow-x: hidden;
    }

    .game-shell {
      width: 100%;
      max-width: 960px;
      background: #111827;
      border: 4px solid #334155;
      border-radius: 14px;
      box-shadow: 0 20px 50px rgba(0, 0, 0, 0.75), inset 0 1px 0 rgba(255, 255, 255, 0.1);
      padding: 12px;
      display: flex;
      flex-direction: column;
      gap: 10px;
    }

    .top-toolbar {
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      justify-content: space-between;
      gap: 8px;
      padding: 6px 10px;
      background: #1e293b;
      border: 2px solid #475569;
      border-radius: 8px;
    }

    .brand-title {
      display: flex;
      align-items: center;
      gap: 8px;
      flex-wrap: wrap;
      font-weight: 900;
      font-size: 1rem;
      letter-spacing: 0.5px;
      color: var(--accent-gold);
      text-shadow: 2px 2px 0 #000;
    }

    .stage-selector {
      display: flex;
      align-items: center;
      gap: 4px;
      flex-wrap: wrap;
    }

    .stage-btn {
      background: #0f172a;
      color: #cbd5e1;
      border: 2px solid #475569;
      border-radius: 5px;
      padding: 4px 8px;
      font-family: var(--retro-font);
      font-size: 0.76rem;
      font-weight: 800;
      cursor: pointer;
      transition: all 0.12s ease;
    }

    .stage-btn:hover {
      background: #334155;
      color: #fff;
    }

    .stage-btn.active {
      background: #dc2626;
      color: #fff;
      border-color: #fca5a5;
      box-shadow: 0 0 8px rgba(239, 68, 68, 0.5);
    }

    .toolbar-actions {
      display: flex;
      align-items: center;
      gap: 6px;
      flex-wrap: wrap;
    }

    .retro-btn {
      background: #334155;
      color: #f8fafc;
      border: 2px solid #64748b;
      border-bottom-width: 4px;
      border-radius: 6px;
      padding: 5px 10px;
      font-family: var(--retro-font);
      font-size: 0.8rem;
      font-weight: 700;
      cursor: pointer;
      transition: transform 0.08s ease, background 0.15s ease;
      display: inline-flex;
      align-items: center;
      gap: 5px;
    }

    .retro-btn:hover {
      background: #475569;
      border-color: #94a3b8;
    }

    .retro-btn:active {
      transform: translateY(2px);
      border-bottom-width: 2px;
    }

    .retro-btn.primary {
      background: #d97706;
      border-color: #fde68a;
      border-bottom-color: #92400e;
      color: #fff;
    }

    .retro-btn.primary:hover {
      background: #f59e0b;
    }

    .retro-btn.success {
      background: #16a34a;
      border-color: #86efac;
      border-bottom-color: #14532d;
      color: #fff;
    }

    .retro-btn.success:hover {
      background: #22c55e;
    }

    .canvas-stage {
      position: relative;
      width: 100%;
      aspect-ratio: 800 / 450;
      background: #5c94fc;
      border: 3px solid #000;
      border-radius: 8px;
      overflow: hidden;
      box-shadow: inset 0 0 12px rgba(0, 0, 0, 0.35);
    }

    canvas {
      display: block;
      width: 100%;
      height: 100%;
      image-rendering: pixelated;
      image-rendering: crisp-edges;
    }

    /* 오버레이 모달 (게임 오버 / 스테이지 클리어) */
    .overlay-modal {
      position: absolute;
      inset: 0;
      background: rgba(9, 13, 22, 0.8);
      backdrop-filter: blur(2px);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 20px;
      z-index: 20;
      opacity: 0;
      pointer-events: none;
      transition: opacity 0.2s ease;
    }

    .overlay-modal.visible {
      opacity: 1;
      pointer-events: auto;
    }

    .modal-card {
      background: #111827;
      border: 4px solid var(--accent-gold);
      border-radius: 12px;
      padding: 22px 28px;
      text-align: center;
      max-width: 460px;
      width: 92%;
      box-shadow: 0 16px 40px rgba(0, 0, 0, 0.85);
    }

    .modal-card.gameover {
      border-color: var(--accent-red);
    }

    .modal-card.clear {
      border-color: var(--accent-green);
    }

    .modal-title {
      font-size: 1.85rem;
      font-weight: 900;
      letter-spacing: 2px;
      margin-bottom: 8px;
      text-shadow: 3px 3px 0 #000;
    }

    .modal-card.gameover .modal-title {
      color: #f87171;
    }

    .modal-card.clear .modal-title {
      color: #4ade80;
    }

    .modal-subtitle {
      font-size: 0.92rem;
      color: #cbd5e1;
      margin-bottom: 16px;
      line-height: 1.45;
    }

    .stats-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 8px;
      background: #1e293b;
      border: 2px solid #334155;
      border-radius: 8px;
      padding: 10px;
      margin-bottom: 18px;
    }

    .stat-box {
      display: flex;
      flex-direction: column;
      gap: 3px;
    }

    .stat-label {
      font-size: 0.7rem;
      color: #94a3b8;
    }

    .stat-val {
      font-size: 1.05rem;
      font-weight: 900;
      color: var(--accent-gold);
    }

    .modal-actions {
      display: flex;
      justify-content: center;
      gap: 10px;
      flex-wrap: wrap;
    }

    .modal-btn {
      font-size: 0.95rem;
      padding: 9px 18px;
    }

    /* 모바일 및 터치 컨트롤러 */
    .controls-footer {
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      justify-content: space-between;
      gap: 10px;
      background: #1e293b;
      border: 2px solid #334155;
      border-radius: 8px;
      padding: 8px 12px;
      font-size: 0.76rem;
      color: #cbd5e1;
    }

    .key-guide {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      align-items: center;
    }

    .key-cap {
      display: inline-block;
      background: #0f172a;
      color: #fbbf24;
      border: 1px solid #64748b;
      border-bottom-width: 3px;
      border-radius: 4px;
      padding: 2px 6px;
      font-weight: 800;
      font-size: 0.72rem;
      margin-right: 3px;
    }

    .touch-controls {
      display: flex;
      width: 100%;
      justify-content: space-between;
      align-items: center;
      gap: 12px;
      padding-top: 4px;
    }

    .dpad-group, .action-group {
      display: flex;
      gap: 8px;
    }

    .touch-btn {
      width: 54px;
      height: 46px;
      background: #334155;
      color: #fff;
      border: 2px solid #94a3b8;
      border-bottom-width: 4px;
      border-radius: 10px;
      font-family: var(--retro-font);
      font-size: 0.92rem;
      font-weight: 900;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      touch-action: manipulation;
    }

    .touch-btn.fire-btn {
      width: 92px;
      background: #ea580c;
      border-color: #fdba74;
      border-bottom-color: #9a3412;
    }

    .touch-btn.jump-btn {
      width: 104px;
      background: #dc2626;
      border-color: #fca5a5;
      border-bottom-color: #7f1d1d;
    }

    .touch-btn:active, .touch-btn.active {
      transform: translateY(2px);
      border-bottom-width: 2px;
      filter: brightness(1.2);
    }

    /* 디바이스 배지 및 게임 설명 요약 바 */
    .device-pill {
      background: #0284c7;
      color: #fff;
      font-size: 0.72rem;
      padding: 3px 8px;
      border-radius: 4px;
      border: 1px solid #7dd3fc;
      font-weight: 800;
      text-shadow: none;
    }

    .game-info-banner {
      background: #1e293b;
      border: 2px solid #334155;
      border-radius: 8px;
      padding: 8px 12px;
      font-size: 0.78rem;
      color: #e2e8f0;
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      justify-content: space-between;
      gap: 8px;
      line-height: 1.45;
    }

    .game-info-highlights {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      align-items: center;
    }

    .info-tag {
      background: #0f172a;
      color: #fde047;
      border: 1px solid #475569;
      border-radius: 4px;
      padding: 1px 6px;
      font-weight: 800;
      font-size: 0.72rem;
      margin-right: 4px;
    }

    /* 모바일 세로 화면일 때 가로 회전 권장 안내 배너 (캔버스를 가리지 않고 상단에 표시) */
    .rotate-notice {
      display: none;
      background: linear-gradient(90deg, #b45309, #d97706);
      color: #fff;
      border: 2px solid #fde68a;
      border-radius: 8px;
      padding: 7px 12px;
      font-size: 0.78rem;
      font-weight: 800;
      align-items: center;
      justify-content: space-between;
      gap: 8px;
    }

    /* 메인 플레이 영역: 캔버스와 좌우/하단 컨트롤러를 분리해 캐릭터를 절대 가리지 않는 구조 */
    .play-viewport {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 8px;
      width: 100%;
    }

    .touch-wing {
      display: none;
    }

    /* 게임 설명(가이드) 모달 스타일 */
    .guide-modal-card {
      background: #111827;
      border: 4px solid #38bdf8;
      border-radius: 12px;
      padding: 18px 22px;
      text-align: left;
      max-width: 580px;
      width: 95%;
      max-height: 92%;
      overflow-y: auto;
      box-shadow: 0 16px 40px rgba(0, 0, 0, 0.9);
      color: #f8fafc;
    }

    .guide-modal-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      border-bottom: 2px solid #334155;
      padding-bottom: 8px;
      margin-bottom: 10px;
    }

    .guide-modal-title {
      font-size: 1.15rem;
      font-weight: 900;
      color: #fde047;
    }

    .guide-section {
      margin-bottom: 10px;
      font-size: 0.78rem;
      line-height: 1.5;
      color: #cbd5e1;
    }

    .guide-section strong {
      color: #38bdf8;
    }

    .guide-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 8px;
      margin-top: 6px;
    }

    .guide-box {
      background: #1e293b;
      border: 1px solid #475569;
      border-radius: 6px;
      padding: 8px 10px;
    }

    /* PC 환경 (마우스/키보드 기준): 터치패드 숨김 */
    @media (min-width: 900px) and (pointer: fine) {
      .touch-controls {
        display: none;
      }
      .touch-controls.force-show {
        display: flex;
      }
    }

    /* 모바일 세로 화면: 가로 회전 안내 배너 표시 & 캔버스 아래 독립 컨트롤러 도크 배치 */
    @media (max-width: 899px) and (orientation: portrait) {
      .rotate-notice {
        display: flex;
      }
      .key-guide {
        display: none;
      }
    }

    /* ★ 핵심: 모바일 가로(Landscape) 모드 — 휴대용 콘솔(Switch/SteamDeck) 스타일 좌·우 베젤 분리 레이아웃!
       터치 버튼이 게임 화면(Canvas) 바깥 좌·우 날개 영역에 배치되어 캐릭터와 지형을 단 1픽셀도 가리지 않음! */
    @media (max-width: 960px) and (orientation: landscape) {
      body {
        padding: 4px;
      }
      .game-shell {
        padding: 6px 8px;
        gap: 6px;
        max-width: 100vw;
      }
      .top-toolbar {
        padding: 4px 8px;
      }
      .game-info-banner {
        display: none;
      }
      .play-viewport {
        flex-direction: row;
        align-items: stretch;
        justify-content: center;
        gap: 8px;
      }
      .canvas-stage {
        flex: 1 1 auto;
        max-height: calc(100vh - 68px);
        width: auto;
      }
      .controls-footer {
        display: none;
      }
      .touch-wing {
        display: flex;
        flex-direction: column;
        justify-content: flex-end;
        align-items: center;
        background: #1e293b;
        border: 2px solid #475569;
        border-radius: 10px;
        padding: 10px 6px;
        min-width: 118px;
        gap: 10px;
      }
      .wing-label {
        font-size: 0.65rem;
        color: #94a3b8;
        font-weight: 800;
        text-align: center;
        margin-bottom: auto;
      }
      .wing-dpad {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        gap: 6px;
        width: 100%;
      }
      .wing-dpad .touch-btn.down-wide {
        grid-column: span 2;
        width: 100%;
        height: 40px;
        background: #0284c7;
        border-color: #7dd3fc;
      }
      .wing-actions {
        display: flex;
        flex-direction: column;
        gap: 8px;
        width: 100%;
      }
      .wing-actions .touch-btn {
        width: 100%;
        height: 54px;
      }
    }

    /* 수동으로 터치패드 강제 표시를 켰을 때 하단 컨트롤러 도크 활성화 */
    .controls-footer.force-show-footer {
      display: flex !important;
    }
    .touch-controls.force-show {
      display: flex !important;
    }
  </style>
</head>
<body>
  <div class="game-shell">
    <!-- 상단 툴바 및 5개 스테이지 선택기 -->
    <div class="top-toolbar">
      <div class="brand-title">
        <span>SUPER PIXEL BROS</span>
        <span id="deviceModeBadge" class="device-pill">💻 PC 키보드 모드</span>
        <div class="stage-selector" role="group" aria-label="스테이지 선택">
          <button id="btnStage1" class="stage-btn active" type="button">1-1 초원</button>
          <button id="btnStage2" class="stage-btn" type="button">1-2 지하동굴</button>
          <button id="btnStage3" class="stage-btn" type="button">1-3 구름고원</button>
          <button id="btnStage4" class="stage-btn" type="button">1-4 사막유적</button>
          <button id="btnStage5" class="stage-btn" type="button">1-5 쿠파성채</button>
        </div>
      </div>
      <div class="toolbar-actions">
        <button id="btnGuide" class="retro-btn" type="button" title="게임 설명 및 공략 보기 (단축키 H)">
          📖 게임 설명
        </button>
        <button id="btnFullscreen" class="retro-btn" type="button" title="전체화면 및 모바일 가로 고정 전환 (단축키 F)">
          ⛶ 전체화면
        </button>
        <button id="btnToggleTouch" class="retro-btn" type="button" title="화면 터치 버튼 표시 전환">
          🎮 터치패드 표시
        </button>
        <button id="btnMute" class="retro-btn" type="button" title="효과음 켜기/끄기">
          🔊 사운드: ON
        </button>
        <button id="btnRestartTop" class="retro-btn primary" type="button" title="처음부터 다시 시작 (단축키 R)">
          🔄 재시작 (R)
        </button>
      </div>
    </div>

    <!-- 모바일 세로 상태일 때 가로 모드를 안내하는 배너 (캐릭터 화면을 가리지 않음) -->
    <div id="rotateNoticeBar" class="rotate-notice">
      <span>📱 모바일에서는 스마트폰을 가로(Landscape)로 돌리거나 [⛶ 전체화면]을 누르면 좌·우 전용 패드로 캐릭터를 가리지 않고 쾌적하게 플레이할 수 있습니다!</span>
    </div>

    <!-- PC & 모바일 공통 한 줄 핵심 게임 설명 바 -->
    <div class="game-info-banner">
      <div class="game-info-highlights">
        <span><span class="info-tag">목표</span>5개 테마 월드(초원·동굴·구름·사막·용암성)를 돌파해 오른쪽 끝 깃대에 도달하세요! (목숨 3개)</span>
        <span><span class="info-tag">아이템</span>[?] 블록 타격 시 슈퍼버섯(성장)·파이어플라워(불꽃탄환)·무적스타·1UP 등장</span>
        <span><span class="info-tag">비밀배관</span>SECRET PIPE 위에서 아래(▼/S)를 누르면 지하 보너스 룸 진입</span>
      </div>
    </div>

    <!-- 메인 플레이 뷰포트: 모바일 가로 모드에서는 좌·우 사이드 독이 활성화되어 게임 화면과 캐릭터를 절대 가리지 않음 -->
    <div class="play-viewport">
      <!-- 모바일 가로 전용 좌측 방향키 베젤 (화면 외곽 배치) -->
      <div class="touch-wing left-wing" aria-label="모바일 가로 좌측 이동 패드">
        <span class="wing-label">MOVE / PIPE</span>
        <div class="wing-dpad">
          <button id="btnWingLeft" class="touch-btn" type="button" aria-label="왼쪽 이동">◀</button>
          <button id="btnWingRight" class="touch-btn" type="button" aria-label="오른쪽 이동">▶</button>
          <button id="btnWingDown" class="touch-btn down-wide" type="button" aria-label="배관 진입">▼ PIPE</button>
        </div>
      </div>

      <!-- 800x450 비율 고정 게임 캔버스 스테이지 -->
      <div class="canvas-stage" id="canvasContainer">
        <canvas id="gameCanvas" width="800" height="450"></canvas>

        <!-- 클리어 / 게임 오버 오버레이 모달 -->
        <div id="overlayModal" class="overlay-modal" role="dialog" aria-modal="true">
          <div id="modalCard" class="modal-card">
            <div id="modalTitle" class="modal-title">STAGE CLEAR!</div>
            <p id="modalSubtitle" class="modal-subtitle">축하합니다! 스테이지를 완벽히 클리어했습니다.</p>
            <div class="stats-grid">
              <div class="stat-box">
                <span class="stat-label">FINAL SCORE</span>
                <span id="modalScore" class="stat-val">000000</span>
              </div>
              <div class="stat-box">
                <span class="stat-label">COINS</span>
                <span id="modalCoins" class="stat-val">00</span>
              </div>
              <div class="stat-box">
                <span class="stat-label">TIME LEFT</span>
                <span id="modalTime" class="stat-val">000</span>
              </div>
            </div>
            <div class="modal-actions">
              <button id="btnModalNext" class="retro-btn success modal-btn" type="button" style="display: none;">
                다음 스테이지 (N)
              </button>
              <button id="btnModalRestart" class="retro-btn primary modal-btn" type="button">
                ▶ 다시 시작하기 (R)
              </button>
            </div>
          </div>
        </div>

        <!-- PC & 모바일 공통 게임 설명(가이드) 오버레이 모달 -->
        <div id="guideModal" class="overlay-modal" role="dialog" aria-modal="true">
          <div class="guide-modal-card">
            <div class="guide-modal-header">
              <span class="guide-modal-title">📖 SUPER PIXEL BROS 게임 설명 (PC & 모바일 공통)</span>
              <button id="btnCloseGuide" class="retro-btn primary" type="button">닫기 ✕</button>
            </div>
            <div class="guide-section">
              <strong>1. 게임 개요 및 승리 조건</strong><br />
              총 <strong>5개의 테마 스테이지(1-1 초원 ~ 1-5 쿠파 성채)</strong>로 구성된 횡스크롤 액션 플랫포머입니다. 총 <strong>3개의 목숨(LIVES)</strong>이 주어지며, 스테이지 중간의 <strong>CHECK 깃발</strong>을 지나면 부활 위치가 저장됩니다. 각 스테이지 오른쪽 끝의 <strong>깃대(Flagpole)</strong>를 잡으면 클리어됩니다!
            </div>
            <div class="guide-grid">
              <div class="guide-box guide-section">
                <strong>2. 조작 방법 (PC 키보드)</strong><br />
                • <b>좌우 이동</b>: 방향키 ◀/▶ 또는 A/D<br />
                • <b>가변 점프</b>: Space / W / ▲ (길게 누를수록 높이 점프)<br />
                • <b>배관 진입</b>: 토관 위에서 ▼ 또는 S<br />
                • <b>불꽃 탄환</b>: Z / Shift / X (파이어 상태)<br />
                • <b>스테이지 선택</b>: 숫자키 1~5 / 재시작 R
              </div>
              <div class="guide-box guide-section">
                <strong>3. 조작 방법 (모바일 가로 기준)</strong><br />
                • <b>화면 가림 제로(0%) 설계</b>: 캐릭터와 지형을 가리지 않도록 게임 화면 바깥(좌·우 베젤 또는 하단 도크)에 전용 패드가 배치됩니다.<br />
                • <b>좌측 패드</b>: ◀/▶ 이동 및 ▼ PIPE(배관 진입)<br />
                • <b>우측 패드</b>: JUMP ▲(점프) 및 FIRE 🔥(불꽃 발사)
              </div>
            </div>
            <div class="guide-section" style="margin-top: 8px; margin-bottom: 0;">
              <strong>4. 아이템 및 몬스터 핵심 공략</strong><br />
              • <b>파워업 아이템</b>: [?] 블록을 치면 <b>슈퍼 버섯</b>(크기 성장·일반 벽돌 파괴), <b>파이어 플라워</b>(통통 튀는 불꽃 탄환 발사), <b>무적 스타</b>(일시 무적), <b>1-UP 버섯</b>(목숨 +1)이 등장합니다.<br />
              • <b>몬스터 상성</b>: 굼바·엉금엉금·날개 엉금엉금은 위에서 밟아서 제압하며, 등껍질을 차면 경로상의 적을 연쇄 처치합니다. 단, <b>1-2 딱정벌레</b>는 파이어볼에 면역이므로 밟아야 하고, <b>1-4·1-5 가시돌이</b>는 등 가시 때문에 밟으면 다치므로 파이어볼이나 등껍질로 처치하세요!
            </div>
          </div>
        </div>
      </div>

      <!-- 모바일 가로 전용 우측 액션 베젤 (화면 외곽 배치) -->
      <div class="touch-wing right-wing" aria-label="모바일 가로 우측 액션 패드">
        <span class="wing-label">ACTION</span>
        <div class="wing-actions">
          <button id="btnWingFire" class="touch-btn fire-btn" type="button" aria-label="불꽃 탄환 발사">FIRE 🔥</button>
          <button id="btnWingJump" class="touch-btn jump-btn" type="button" aria-label="점프">JUMP ▲</button>
        </div>
      </div>
    </div>

    <!-- 하단 조작 가이드 및 터치 패드 -->
    <div class="controls-footer">
      <div class="key-guide">
        <span><kbd class="key-cap">◀</kbd><kbd class="key-cap">▶</kbd>/<kbd class="key-cap">A</kbd><kbd class="key-cap">D</kbd> 이동</span>
        <span><kbd class="key-cap">▼</kbd>/<kbd class="key-cap">S</kbd> 배관 진입</span>
        <span><kbd class="key-cap">Space</kbd>/<kbd class="key-cap">W</kbd>/<kbd class="key-cap">▲</kbd> 가변 점프</span>
        <span><kbd class="key-cap">Z</kbd>/<kbd class="key-cap">Shift</kbd> 파이어볼</span>
        <span><kbd class="key-cap">1</kbd>~<kbd class="key-cap">5</kbd> 스테이지 이동</span>
        <span><kbd class="key-cap">R</kbd> 재시작</span>
      </div>

      <div id="touchControlsBar" class="touch-controls">
        <div class="dpad-group">
          <button id="btnTouchLeft" class="touch-btn" type="button" aria-label="왼쪽 이동">◀</button>
          <button id="btnTouchDown" class="touch-btn" type="button" aria-label="배관 진입 및 숙이기">▼</button>
          <button id="btnTouchRight" class="touch-btn" type="button" aria-label="오른쪽 이동">▶</button>
        </div>
        <div class="action-group">
          <button id="btnTouchFire" class="touch-btn fire-btn" type="button" aria-label="불꽃 탄환 발사">FIRE 🔥</button>
          <button id="btnTouchJump" class="touch-btn jump-btn" type="button" aria-label="점프">JUMP ▲</button>
        </div>
      </div>
    </div>
  </div>

  <script>
    (function () {
      'use strict';

      // ==========================================
      // 1. 캔버스 및 전역 상수 설정 (800 x 450 고정 논리 해상도)
      // ==========================================
      const canvas = document.getElementById('gameCanvas');
      const ctx = canvas.getContext('2d');

      const VIEW_W = 800;
      const VIEW_H = 450;
      const TILE = 32;                    // 기본 타일 크기 32x32px
      const GROUND_ROW = 12;              // 바닥 시작 행 (12 * 32 = 384px)
      const GROUND_Y = GROUND_ROW * TILE; // 384px (384 ~ 450 구간이 바닥)
      const WORLD_WIDTH = 3840;           // 각 스테이지 길이 (120타일 * 32px = 3840px)
      const UNDERGROUND_WIDTH = 800;      // 배관 지하 비밀 보너스 공간 폭 (25타일 * 32px)

      // 물리 엔진 상수
      const PHYSICS = {
        accelGround: 0.42,
        accelAir: 0.28,
        decelSkid: 0.72,
        frictionGround: 0.84,
        frictionAir: 0.94,
        maxSpeedX: 5.4,
        starSpeedBonus: 1.35,
        gravity: 0.58,
        jumpHoldGravity: 0.25,
        maxFallSpeed: 12.0,
        jumpInitialVy: -10.4,
        jumpHoldFramesMax: 17,
        stompBounceVy: -8.6
      };

      // 5개 스테이지 메타데이터 및 테마 구성
      const STAGE_THEMES = {
        1: {
          code: '1-1',
          name: 'GRASSLAND OVERWORLD',
          korName: '초원 월드',
          skyTop: '#4f8df9',
          skyMid: '#68a2ff',
          skyBot: '#93c5fd',
          groundMain: '#c2410c',
          groundTop: '#22c55e',
          groundBorder: '#431407',
          brickMain: '#b45309',
          brickHighlight: '#f59e0b',
          hardMain: '#9a3412',
          hardHighlight: '#fdba74',
          hasCeiling: false,
          pitType: 'pit'
        },
        2: {
          code: '1-2',
          name: 'UNDERGROUND CAVERN',
          korName: '지하 동굴',
          skyTop: '#070b19',
          skyMid: '#0f172a',
          skyBot: '#1e1b4b',
          groundMain: '#1e3a8a',
          groundTop: '#60a5fa',
          groundBorder: '#090d16',
          brickMain: '#1d4ed8',
          brickHighlight: '#60a5fa',
          hardMain: '#3b82f6',
          hardHighlight: '#93c5fd',
          hasCeiling: true,
          pitType: 'pit'
        },
        3: {
          code: '1-3',
          name: 'ATHLETIC SKY HIGHLANDS',
          korName: '구름 고원',
          skyTop: '#0284c7',
          skyMid: '#38bdf8',
          skyBot: '#fde68a',
          groundMain: '#15803d',
          groundTop: '#86efac',
          groundBorder: '#052e16',
          brickMain: '#d97706',
          brickHighlight: '#fde047',
          hardMain: '#0ea5e9',
          hardHighlight: '#bae6fd',
          hasCeiling: false,
          pitType: 'pit'
        },
        4: {
          code: '1-4',
          name: 'DESERT PYRAMID RUINS',
          korName: '사막 유적',
          skyTop: '#c2410c',
          skyMid: '#f97316',
          skyBot: '#fde047',
          groundMain: '#d97706',
          groundTop: '#fde047',
          groundBorder: '#78350f',
          brickMain: '#b45309',
          brickHighlight: '#fcd34d',
          hardMain: '#a16207',
          hardHighlight: '#fef08a',
          hasCeiling: false,
          pitType: 'quicksand'
        },
        5: {
          code: '1-5',
          name: 'BOWSER LAVA CASTLE',
          korName: '쿠파 성채',
          skyTop: '#180505',
          skyMid: '#2a0808',
          skyBot: '#450a0a',
          groundMain: '#475569',
          groundTop: '#94a3b8',
          groundBorder: '#0f172a',
          brickMain: '#334155',
          brickHighlight: '#64748b',
          hardMain: '#64748b',
          hardHighlight: '#cbd5e1',
          hasCeiling: true,
          pitType: 'lava'
        }
      };

      // ==========================================
      // 2. Web Audio API 레트로 사운드 합성기
      // ==========================================
      class RetroAudio {
        constructor() {
          this.ctx = null;
          this.muted = false;
        }

        init() {
          if (!this.ctx) {
            const AudioCtx = window.AudioContext || window.webkitAudioContext;
            if (AudioCtx) {
              this.ctx = new AudioCtx();
            }
          }
          if (this.ctx && this.ctx.state === 'suspended') {
            this.ctx.resume();
          }
        }

        toggleMute() {
          this.muted = !this.muted;
          return this.muted;
        }

        playTone(freqStart, freqEnd, duration, type = 'square', volume = 0.12, delay = 0) {
          if (this.muted) return;
          this.init();
          if (!this.ctx) return;

          const now = this.ctx.currentTime + delay;
          const osc = this.ctx.createOscillator();
          const gain = this.ctx.createGain();

          osc.type = type;
          osc.frequency.setValueAtTime(freqStart, now);
          if (freqEnd !== freqStart) {
            osc.frequency.exponentialRampToValueAtTime(Math.max(20, freqEnd), now + duration);
          }

          gain.gain.setValueAtTime(volume, now);
          gain.gain.exponentialRampToValueAtTime(0.001, now + duration);

          osc.connect(gain);
          gain.connect(this.ctx.destination);

          osc.start(now);
          osc.stop(now + duration);
        }

        jump(isBig = false) {
          const base = isBig ? 160 : 210;
          this.playTone(base, base * 2.8, 0.19, 'square', 0.1);
        }

        spring() {
          this.playTone(180, 760, 0.26, 'triangle', 0.15);
        }

        coin() {
          this.playTone(987.77, 987.77, 0.07, 'square', 0.11, 0);
          this.playTone(1318.51, 1318.51, 0.24, 'square', 0.11, 0.07);
        }

        bump() {
          this.playTone(150, 65, 0.11, 'triangle', 0.18);
        }

        thwompSlam() {
          this.playTone(110, 35, 0.25, 'sawtooth', 0.2, 0);
          this.playTone(80, 28, 0.22, 'square', 0.16, 0.04);
        }

        brickSmash() {
          this.playTone(220, 55, 0.16, 'sawtooth', 0.15, 0);
          this.playTone(140, 42, 0.18, 'square', 0.12, 0.04);
        }

        itemSpawn() {
          const notes = [330, 392, 659, 523, 587, 784];
          notes.forEach((n, idx) => {
            this.playTone(n, n, 0.055, 'square', 0.09, idx * 0.05);
          });
        }

        powerUp() {
          const notes = [330, 392, 659, 523, 587, 784, 659, 784, 1046.5];
          notes.forEach((n, idx) => {
            this.playTone(n, n * 1.02, 0.07, 'square', 0.11, idx * 0.055);
          });
        }

        oneUp() {
          const notes = [330, 392, 659, 523, 587, 784];
          notes.forEach((n, idx) => {
            this.playTone(n * 1.25, n * 1.25, 0.08, 'triangle', 0.14, idx * 0.065);
          });
        }

        powerDown() {
          const notes = [784, 659, 523, 392, 293, 220];
          notes.forEach((n, idx) => {
            this.playTone(n, n * 0.9, 0.065, 'triangle', 0.14, idx * 0.05);
          });
        }

        pipeWarp() {
          this.playTone(440, 220, 0.09, 'square', 0.13, 0.0);
          this.playTone(392, 196, 0.09, 'square', 0.13, 0.11);
          this.playTone(330, 146, 0.12, 'square', 0.13, 0.22);
        }

        fireball() {
          this.playTone(880, 240, 0.09, 'sawtooth', 0.11, 0);
          this.playTone(520, 1100, 0.06, 'square', 0.08, 0.04);
        }

        kick() {
          this.playTone(180, 620, 0.11, 'square', 0.14, 0);
        }

        stomp() {
          this.playTone(420, 130, 0.1, 'square', 0.13);
          this.playTone(280, 560, 0.09, 'triangle', 0.12, 0.04);
        }

        checkpoint() {
          this.playTone(523.25, 659.25, 0.1, 'triangle', 0.13, 0);
          this.playTone(659.25, 1046.5, 0.18, 'triangle', 0.13, 0.1);
        }

        stageClear() {
          const melody = [
            { f: 523.25, d: 0.12, t: 0.00 },
            { f: 659.25, d: 0.12, t: 0.13 },
            { f: 783.99, d: 0.12, t: 0.26 },
            { f: 1046.50, d: 0.22, t: 0.39 },
            { f: 783.99, d: 0.14, t: 0.63 },
            { f: 1046.50, d: 0.45, t: 0.79 }
          ];
          melody.forEach((m) => this.playTone(m.f, m.f, m.d, 'square', 0.12, m.t));
        }

        lifeLost() {
          const melody = [
            { f: 587.33, d: 0.1, t: 0.00 },
            { f: 523.25, d: 0.1, t: 0.12 },
            { f: 440.00, d: 0.14, t: 0.24 },
            { f: 349.23, d: 0.25, t: 0.40 }
          ];
          melody.forEach((m) => this.playTone(m.f, m.f, m.d, 'triangle', 0.13, m.t));
        }

        gameOver() {
          const melody = [
            { f: 493.88, d: 0.13, t: 0.00 },
            { f: 698.46, d: 0.13, t: 0.15 },
            { f: 698.46, d: 0.13, t: 0.30 },
            { f: 659.25, d: 0.16, t: 0.45 },
            { f: 587.33, d: 0.16, t: 0.62 },
            { f: 523.25, d: 0.36, t: 0.79 }
          ];
          melody.forEach((m) => this.playTone(m.f, m.f, m.d, 'sawtooth', 0.11, m.t));
        }
      }

      const sound = new RetroAudio();

      // ==========================================
      // 3. 입력 핸들러 (키보드 + 모바일 터치)
      // ==========================================
      const keys = {
        left: false,
        right: false,
        down: false,
        jump: false,
        jumpJustPressed: false,
        fire: false,
        fireJustPressed: false
      };

      function handleKeyDown(e) {
        if (['ArrowLeft', 'ArrowRight', 'ArrowUp', 'ArrowDown', 'Space', ' '].includes(e.key) || e.code === 'Space') {
          e.preventDefault();
        }
        sound.init();

        const code = e.code;
        if (code === 'ArrowLeft' || code === 'KeyA') keys.left = true;
        if (code === 'ArrowRight' || code === 'KeyD') keys.right = true;
        if (code === 'ArrowDown' || code === 'KeyS') keys.down = true;
        if (code === 'Space' || code === 'KeyW' || code === 'ArrowUp') {
          if (!keys.jump) keys.jumpJustPressed = true;
          keys.jump = true;
        }
        if (code === 'KeyZ' || code === 'ShiftLeft' || code === 'ShiftRight' || code === 'KeyX') {
          if (!keys.fire) keys.fireJustPressed = true;
          keys.fire = true;
        }
        if (code === 'KeyR') {
          startFullNewGame(currentStage);
        }
        if (code === 'KeyN' && gameState === 'STAGE_CLEAR' && currentStage < 5) {
          goToStage(currentStage + 1);
        }
        if (code === 'Digit1') goToStage(1);
        if (code === 'Digit2') goToStage(2);
        if (code === 'Digit3') goToStage(3);
        if (code === 'Digit4') goToStage(4);
        if (code === 'Digit5') goToStage(5);
        if (code === 'KeyF') toggleFullscreen();
        if (code === 'KeyH') toggleGuideModal();
      }

      function handleKeyUp(e) {
        const code = e.code;
        if (code === 'ArrowLeft' || code === 'KeyA') keys.left = false;
        if (code === 'ArrowRight' || code === 'KeyD') keys.right = false;
        if (code === 'ArrowDown' || code === 'KeyS') keys.down = false;
        if (code === 'Space' || code === 'KeyW' || code === 'ArrowUp') {
          keys.jump = false;
          keys.jumpJustPressed = false;
        }
        if (code === 'KeyZ' || code === 'ShiftLeft' || code === 'ShiftRight' || code === 'KeyX') {
          keys.fire = false;
          keys.fireJustPressed = false;
        }
      }

      window.addEventListener('keydown', handleKeyDown, { passive: false });
      window.addEventListener('keyup', handleKeyUp);

      function bindHoldButton(btnId, keyName) {
        const btn = document.getElementById(btnId);
        if (!btn) return;

        const press = (ev) => {
          ev.preventDefault();
          sound.init();
          btn.classList.add('active');
          if (keyName === 'jump' && !keys.jump) keys.jumpJustPressed = true;
          if (keyName === 'fire' && !keys.fire) keys.fireJustPressed = true;
          keys[keyName] = true;
        };

        const release = (ev) => {
          ev.preventDefault();
          btn.classList.remove('active');
          keys[keyName] = false;
          if (keyName === 'jump') keys.jumpJustPressed = false;
          if (keyName === 'fire') keys.fireJustPressed = false;
        };

        btn.addEventListener('mousedown', press);
        btn.addEventListener('mouseup', release);
        btn.addEventListener('mouseleave', release);
        btn.addEventListener('touchstart', press, { passive: false });
        btn.addEventListener('touchend', release, { passive: false });
        btn.addEventListener('touchcancel', release, { passive: false });
      }

      bindHoldButton('btnTouchLeft', 'left');
      bindHoldButton('btnTouchDown', 'down');
      bindHoldButton('btnTouchRight', 'right');
      bindHoldButton('btnTouchFire', 'fire');
      bindHoldButton('btnTouchJump', 'jump');

      // 모바일 가로 베젤 전용 버튼 바인딩 (캐릭터 화면을 전혀 가리지 않는 좌·우 날개 컨트롤러)
      bindHoldButton('btnWingLeft', 'left');
      bindHoldButton('btnWingDown', 'down');
      bindHoldButton('btnWingRight', 'right');
      bindHoldButton('btnWingFire', 'fire');
      bindHoldButton('btnWingJump', 'jump');

      // PC vs 모바일 및 가로/세로 방향 자동 감지 시스템
      const deviceModeBadge = document.getElementById('deviceModeBadge');
      const guideModal = document.getElementById('guideModal');
      const btnGuide = document.getElementById('btnGuide');
      const btnCloseGuide = document.getElementById('btnCloseGuide');
      const controlsFooter = document.querySelector('.controls-footer');

      function detectDeviceAndOrientation() {
        const isTouchDevice = ('ontouchstart' in window) || (navigator.maxTouchPoints > 0) || window.matchMedia('(pointer: coarse)').matches;
        const isLandscape = window.innerWidth > window.innerHeight;
        const isSmallScreen = window.innerWidth <= 960;

        document.body.classList.toggle('mobile-mode', isTouchDevice || isSmallScreen);
        document.body.classList.toggle('landscape-mode', isLandscape && (isTouchDevice || isSmallScreen));

        if (deviceModeBadge) {
          if (isTouchDevice || isSmallScreen) {
            deviceModeBadge.textContent = isLandscape ? '📱 모바일 가로 모드' : '📱 모바일 모드 (가로 권장)';
          } else {
            deviceModeBadge.textContent = '💻 PC 키보드 모드';
          }
        }
      }

      window.addEventListener('resize', detectDeviceAndOrientation);
      window.addEventListener('orientationchange', detectDeviceAndOrientation);
      detectDeviceAndOrientation();

      function toggleGuideModal(forceState) {
        if (!guideModal) return;
        const willShow = typeof forceState === 'boolean' ? forceState : !guideModal.classList.contains('visible');
        guideModal.classList.toggle('visible', willShow);
      }

      if (btnGuide) {
        btnGuide.addEventListener('click', () => {
          sound.init();
          toggleGuideModal();
          btnGuide.blur();
        });
      }

      if (btnCloseGuide) {
        btnCloseGuide.addEventListener('click', () => {
          toggleGuideModal(false);
        });
      }

      // 툴바 버튼 바인딩
      const btnMute = document.getElementById('btnMute');
      const btnRestartTop = document.getElementById('btnRestartTop');
      const btnModalRestart = document.getElementById('btnModalRestart');
      const btnModalNext = document.getElementById('btnModalNext');
      const btnToggleTouch = document.getElementById('btnToggleTouch');
      const btnFullscreen = document.getElementById('btnFullscreen');
      const gameShell = document.querySelector('.game-shell');

      function toggleFullscreen() {
        try {
          if (!document.fullscreenElement) {
            if (gameShell && gameShell.requestFullscreen) {
              gameShell.requestFullscreen().then(() => {
                if (screen.orientation && screen.orientation.lock) {
                  screen.orientation.lock('landscape').catch(() => {});
                }
              }).catch(() => {});
            }
          } else if (document.exitFullscreen) {
            document.exitFullscreen().catch(() => {});
          }
        } catch (err) {}
      }

      if (btnFullscreen) {
        btnFullscreen.addEventListener('click', () => {
          sound.init();
          toggleFullscreen();
          btnFullscreen.blur();
        });
      }
      const touchControlsBar = document.getElementById('touchControlsBar');

      btnMute.addEventListener('click', () => {
        const isMuted = sound.toggleMute();
        btnMute.textContent = isMuted ? '🔇 사운드: OFF' : '🔊 사운드: ON';
        btnMute.blur();
      });

      btnRestartTop.addEventListener('click', () => {
        sound.init();
        startFullNewGame(currentStage);
        btnRestartTop.blur();
      });

      btnModalRestart.addEventListener('click', () => {
        sound.init();
        startFullNewGame(1);
      });

      btnModalNext.addEventListener('click', () => {
        sound.init();
        if (currentStage < 5) {
          goToStage(currentStage + 1);
        } else {
          startFullNewGame(1);
        }
      });

      btnToggleTouch.addEventListener('click', () => {
        touchControlsBar.classList.toggle('force-show');
        if (controlsFooter) controlsFooter.classList.toggle('force-show-footer');
        btnToggleTouch.blur();
      });

      const stageBtnIds = ['btnStage1', 'btnStage2', 'btnStage3', 'btnStage4', 'btnStage5'];
      stageBtnIds.forEach((id, idx) => {
        const sb = document.getElementById(id);
        if (sb) {
          sb.addEventListener('click', () => {
            sound.init();
            goToStage(idx + 1);
            sb.blur();
          });
        }
      });

      function updateStageButtonsUI() {
        stageBtnIds.forEach((id, idx) => {
          const sb = document.getElementById(id);
          if (sb) {
            sb.classList.toggle('active', idx + 1 === currentStage);
          }
        });
      }

      // ==========================================
      // 4. 픽셀 아트 렌더러 및 팔레트
      // ==========================================
      const HERO_PALETTE_NORMAL = {
        R: '#e62822',
        B: '#2563eb',
        S: '#fca560',
        H: '#5c2c06',
        Y: '#facc15',
        W: '#ffffff',
        K: '#111827'
      };

      // 슈퍼 마리오 상태 팔레트 (선명한 레드 & 로열 블루)
      const HERO_PALETTE_SUPER = {
        R: '#ef4444',
        B: '#1d4ed8',
        S: '#fdba74',
        H: '#451a03',
        Y: '#fde047',
        W: '#ffffff',
        K: '#111827'
      };

      // 파이어 마리오 상태 팔레트 (화이트 모자/상의 + 진홍색 멜빵바지)
      const HERO_PALETTE_FIRE = {
        R: '#ffffff',
        B: '#dc2626',
        S: '#fed7aa',
        H: '#7c2d12',
        Y: '#facc15',
        W: '#ffffff',
        K: '#111827'
      };

      // 무적 스타 레인보우 팔레트
      const STAR_PALETTES = [
        { R: '#facc15', B: '#ef4444', S: '#fef08a', H: '#b45309', Y: '#ffffff', W: '#ffffff', K: '#111827' },
        { R: '#4ade80', B: '#2563eb', S: '#dcfce7', H: '#065f46', Y: '#facc15', W: '#ffffff', K: '#111827' },
        { R: '#f472b6', B: '#9333ea', S: '#fce7f3', H: '#581c87', Y: '#fde047', W: '#ffffff', K: '#111827' }
      ];

      const SPRITE_HERO_IDLE = [
        '...RRRRR....',
        '..RRRRRRRRR.',
        '..HHHSSKSS..',
        '.HSHSSSKS...',
        '.HSHHSSSSS..',
        '.HHSSSSHHHH.',
        '...SSSSSSS..',
        '..RRBRRR....',
        '.RRRBRRRBRR.',
        'RRRRBBBBBRRR',
        'SS.RBYBBR.SS',
        'SS.BBBBBB.SS',
        '..BBBBBBBB..',
        '..BBB..BBB..',
        '.HHH....HHH.',
        'HHHH....HHHH'
      ];

      const SPRITE_HERO_RUN1 = [
        '...RRRRR....',
        '..RRRRRRRRR.',
        '..HHHSSKSS..',
        '.HSHSSSKS...',
        '.HSHHSSSSS..',
        '.HHSSSSHHHH.',
        '...SSSSSSS..',
        '...RRBRR....',
        '..RRRBRR....',
        '..RRBBBBSS..',
        '..BBBBBYBS..',
        '..BBBBBBBB..',
        '.BBBB..BBB..',
        'HHHH....HHH.',
        'HHH.....HHHH',
        '............'
      ];

      const SPRITE_HERO_RUN2 = [
        '....RRRRR...',
        '...RRRRRRRRR',
        '...HHHSSKSS.',
        '..HSHSSSKS..',
        '..HSHHSSSSS.',
        '..HHSSSSHHHH',
        '....SSSSSSS.',
        '...RRRBRR...',
        '..SSRRBRRR..',
        '..SSRBBBBBR.',
        '....BBBYBBR.',
        '...BBBBBBBB.',
        '...BBB..BBB.',
        '...HHH...HHH',
        '..HHHH......',
        '............'
      ];

      const SPRITE_HERO_JUMP = [
        '...RRRRR....',
        '..RRRRRRRRR.',
        '..HHHSSKSS..',
        '.HSHSSSKS...',
        '.HSHHSSSSS..',
        '.HHSSSSHHHH.',
        '...SSSSSSS..',
        '..RRRBRR.SS.',
        '.RRRRBRRRSS.',
        'SS.RBBBBBR..',
        '...BBBYBBBB.',
        '..BBBBBBBBB.',
        '.BBBB...BBB.',
        'HHHH.....HHH',
        'HH........HH',
        '............'
      ];

      const GOOMBA_PALETTE_OVERWORLD = {
        M: '#b45309',
        D: '#78350f',
        S: '#fde68a',
        W: '#ffffff',
        K: '#111827'
      };

      const GOOMBA_PALETTE_UNDERGROUND = {
        M: '#1e40af',
        D: '#1e3a8a',
        S: '#93c5fd',
        W: '#ffffff',
        K: '#090d16'
      };

      const SPRITE_GOOMBA_1 = [
        '....MMMM....',
        '..MMMMMMMM..',
        '.MMMMMMMMMM.',
        'MMMWKMMMWKMM',
        'MMMWKMMMWKMM',
        'MMMMKMMMMKMM',
        'DDDDDDDDDDDD',
        '..SSSSSSSS..',
        '..SSSSSSSS..',
        '.KKKKKKKK..K',
        'KKKKKKKK..KK',
        'KKKKK.......'
      ];

      const SPRITE_GOOMBA_2 = [
        '....MMMM....',
        '..MMMMMMMM..',
        '.MMMMMMMMMM.',
        'MMMWKMMMWKMM',
        'MMMWKMMMWKMM',
        'MMMMKMMMMKMM',
        'DDDDDDDDDDDD',
        '..SSSSSSSS..',
        '..SSSSSSSS..',
        'K..KKKKKKKK.',
        'KK..KKKKKKKK',
        '.......KKKKK'
      ];

      function drawPixelMatrix(ctx, matrix, palette, x, y, width, height, flipX = false) {
        const rows = matrix.length;
        const cols = matrix[0].length;
        const pw = width / cols;
        const ph = height / rows;

        ctx.save();
        if (flipX) {
          ctx.translate(x + width, y);
          ctx.scale(-1, 1);
        } else {
          ctx.translate(x, y);
        }

        for (let r = 0; r < rows; r++) {
          const rowStr = matrix[r];
          for (let c = 0; c < cols; c++) {
            const ch = rowStr[c];
            if (ch !== '.' && palette[ch]) {
              ctx.fillStyle = palette[ch];
              ctx.fillRect(
                Math.floor(c * pw),
                Math.floor(r * ph),
                Math.ceil(pw),
                Math.ceil(ph)
              );
            }
          }
        }
        ctx.restore();
      }

      // ==========================================
      // 5. AABB 정밀 충돌 감지 유틸리티
      // ==========================================
      function aabbOverlap(a, b) {
        return (
          a.x < b.x + b.w &&
          a.x + a.w > b.x &&
          a.y < b.y + b.h &&
          a.y + a.h > b.y
        );
      }

      // ==========================================
      // 6. 게임 상태, 목숨(3개), 5개 스테이지 빌더
      // ==========================================
      let gameState = 'PLAYING'; // 'PLAYING' | 'PIPE_WARP' | 'CLEARING' | 'STAGE_CLEAR' | 'DYING' | 'GAME_OVER'
      let currentStage = 1;      // 1 ~ 5 스테이지
      let currentZone = 'OVERWORLD'; // 'OVERWORLD' | 'UNDERGROUND'
      let lives = 3;             // ★ 목숨 3개 시스템
      let score = 0;
      let bestScore = 0;
      try {
        bestScore = parseInt(localStorage.getItem('super_pixel_bros_best') || '0', 10) || 0;
      } catch (e) {
        bestScore = 0;
      }
      let coins = 0;
      let timeLeft = 400;
      let timeAccumulator = 0;
      let frameCount = 0;
      let cameraX = 0;
      let checkpointReached = false;

      // 존(Zone)별 월드 데이터 저장소
      let worlds = {
        OVERWORLD: {
          solids: [],
          platforms: [],   // 이동 리프트 플랫폼
          springs: [],     // 스프링 점프대
          hazards: [],     // 가시 함정, 회전 화염바, 쿵쿵이(Thwomp), 용암 화염구(Podoboo)
          enemies: [],
          items: [],
          fieldCoins: [],
          piranhas: [],
          decors: []
        },
        UNDERGROUND: {
          solids: [],
          platforms: [],
          springs: [],
          hazards: [],
          enemies: [],
          items: [],
          fieldCoins: [],
          piranhas: [],
          decors: []
        }
      };

      // 현재 활성화된 존 참조 포인터
      let solids = [];
      let platforms = [];
      let springs = [];
      let hazards = [];
      let enemies = [];
      let items = [];
      let fieldCoins = [];
      let piranhas = [];
      let decors = [];

      let fireballs = [];
      let particles = [];
      let popups = [];
      let flagpole = null;
      let checkpoint = null;
      let player = null;
      let warpState = null;

      function setActiveZone(zoneName) {
        currentZone = zoneName;
        const w = worlds[zoneName];
        solids = w.solids;
        platforms = w.platforms;
        springs = w.springs;
        hazards = w.hazards;
        enemies = w.enemies;
        items = w.items;
        fieldCoins = w.fieldCoins;
        piranhas = w.piranhas;
        decors = w.decors;
        fireballs = [];
      }

      function createPlayer(spawnAtCheckpoint = false, keepPowerLevel = 0) {
        const startX = spawnAtCheckpoint && checkpoint ? checkpoint.x : 96;
        const h = keepPowerLevel >= 1 ? 48 : 32;
        return {
          x: startX,
          y: GROUND_Y - h,
          w: 26,
          h,
          vx: 0,
          vy: 0,
          facing: 1,
          onGround: false,
          powerLevel: keepPowerLevel, // 0: SMALL, 1: SUPER, 2: FIRE
          isSuper: keepPowerLevel >= 1,
          starTimer: 0,
          fireCooldown: 0,
          // ★ 스폰킬 방지: 시작/부활 시 90프레임(1.5초) 스폰 무적 보호 부여!
          invincibleTimer: 90,
          transformTimer: 0,
          jumpHoldTimer: 0,
          isSkidding: false,
          animFrame: 0,
          animTimer: 0,
          clearPhase: 0
        };
      }

      function addPopup(x, y, text, color = '#ffffff') {
        popups.push({
          x,
          y,
          text,
          color,
          vy: -1.3,
          life: 45
        });
      }

      function spawnDust(x, y, count = 5, color = '#f1f5f9') {
        for (let i = 0; i < count; i++) {
          particles.push({
            type: 'dust',
            x: x + (Math.random() - 0.5) * 14,
            y: y + (Math.random() - 0.5) * 6,
            vx: (Math.random() - 0.5) * 2.6,
            vy: -Math.random() * 2.0 - 0.4,
            size: 4 + Math.random() * 4,
            color,
            gravity: 0.05,
            life: 18 + Math.floor(Math.random() * 10)
          });
        }
      }

      function spawnFireBurst(x, y) {
        const fireColors = ['#ef4444', '#f97316', '#facc15', '#ffffff'];
        for (let i = 0; i < 10; i++) {
          const angle = (Math.PI * 2 * i) / 10;
          const speed = 1.5 + Math.random() * 2.2;
          particles.push({
            type: 'dust',
            x,
            y,
            vx: Math.cos(angle) * speed,
            vy: Math.sin(angle) * speed - 0.5,
            size: 5 + Math.random() * 3,
            color: fireColors[i % fireColors.length],
            gravity: 0.08,
            life: 16 + Math.floor(Math.random() * 8)
          });
        }
      }

      function spawnBrickDebris(bx, by, color = '#c2410c') {
        const offsets = [
          { dx: 4, dy: 4, vx: -2.8, vy: -7.5 },
          { dx: 18, dy: 4, vx: 2.8, vy: -7.5 },
          { dx: 4, dy: 18, vx: -2.2, vy: -5.2 },
          { dx: 18, dy: 18, vx: 2.2, vy: -5.2 }
        ];
        offsets.forEach((o) => {
          particles.push({
            type: 'brick',
            x: bx + o.dx,
            y: by + o.dy,
            vx: o.vx,
            vy: o.vy,
            size: 10,
            color,
            gravity: 0.48,
            angle: 0,
            vAngle: o.vx * 0.08,
            life: 55
          });
        });
      }

      // ==========================================
      // 스테이지 빌더 (Stage 1 ~ Stage 5 테마별 고유 지형·장애물·몬스터)
      // ==========================================
      function buildStage(stageNum) {
        worlds = {
          OVERWORLD: { solids: [], platforms: [], springs: [], hazards: [], enemies: [], items: [], fieldCoins: [], piranhas: [], decors: [] },
          UNDERGROUND: { solids: [], platforms: [], springs: [], hazards: [], enemies: [], items: [], fieldCoins: [], piranhas: [], decors: [] }
        };
        particles = [];
        popups = [];
        fireballs = [];

        const ow = worlds.OVERWORLD;
        const ug = worlds.UNDERGROUND;
        const theme = STAGE_THEMES[stageNum] || STAGE_THEMES[1];

        // 헬퍼 함수들
        function addBlock(targetWorld, col, row, type = 'brick', content = 'coin', extra = {}) {
          targetWorld.solids.push({
            type,          // 'brick' | 'question' | 'empty' | 'hard' | 'ug_brick'
            content,       // 'coin' | 'mushroom' | 'flower' | 'star' | '1up' | 'multicoin' | null
            coinsLeft: content === 'multicoin' ? 5 : 0,
            x: col * TILE,
            y: row * TILE,
            baseY: row * TILE,
            w: TILE,
            h: TILE,
            bumpOffset: 0,
            bumpVy: 0,
            ...extra
          });
        }

        function addPipe(targetWorld, col, heightTiles, extra = {}) {
          const pipeObj = {
            type: 'pipe',
            x: col * TILE,
            y: (GROUND_ROW - heightTiles) * TILE,
            w: TILE * 2,
            h: heightTiles * TILE,
            ...extra
          };
          targetWorld.solids.push(pipeObj);

          if (extra.hasPiranha) {
            targetWorld.piranhas.push({
              x: col * TILE + 16,
              baseY: (GROUND_ROW - heightTiles) * TILE,
              y: (GROUND_ROW - heightTiles) * TILE,
              w: 32,
              h: 38,
              timer: 40,
              alive: true
            });
          }
        }

        function addStairs(targetWorld, startCol, maxSteps, direction = 'up') {
          for (let step = 0; step < maxSteps; step++) {
            const col = startCol + step;
            const height = direction === 'up' ? step + 1 : maxSteps - step;
            for (let h = 1; h <= height; h++) {
              addBlock(targetWorld, col, GROUND_ROW - h, 'hard', null);
            }
          }
        }

        // ★ 몬스터 추가 헬퍼 (스폰킬 방지를 위해 minX 설정 및 스폰 구역 col < 15 침범 완전 차단!)
        function addEnemy(targetWorld, col, row = GROUND_ROW - 1, dir = -1, enemyType = 'goomba', minCol = 13, maxCol = 112) {
          // enemyType: 'goomba' | 'koopa' | 'red_koopa' | 'paratroopa' | 'beetle' | 'spiny'
          const isTall = enemyType === 'koopa' || enemyType === 'red_koopa' || enemyType === 'paratroopa';
          targetWorld.enemies.push({
            type: enemyType,
            x: col * TILE + 2,
            y: row * TILE + (isTall ? -4 : 4),
            w: 28,
            h: isTall ? 36 : 28,
            vx: dir * (enemyType === 'spiny' ? 1.15 : isTall ? 1.35 : 1.2),
            vy: 0,
            minX: minCol * TILE, // 이 좌표보다 왼쪽(플레이어 시작 지점)으로는 절대 넘어오지 않음!
            maxX: maxCol * TILE,
            onGround: false,
            state: 'walk', // 'walk' | 'squished' | 'shell_idle' | 'shell_moving'
            squishTimer: 0,
            animTimer: 0
          });
        }

        function addFieldCoins(targetWorld, cols, row) {
          cols.forEach((c) => {
            targetWorld.fieldCoins.push({
              x: c * TILE + 7,
              y: row * TILE + 5,
              w: 18,
              h: 22,
              collected: false
            });
          });
        }

        function addMovingPlatform(targetWorld, col, row, widthTiles, rangeX, rangeY, speed = 0.03) {
          targetWorld.platforms.push({
            x: col * TILE,
            y: row * TILE,
            startX: col * TILE,
            startY: row * TILE,
            w: widthTiles * TILE,
            h: 16,
            rangeX,
            rangeY,
            speed,
            theta: 0,
            vx: 0,
            vy: 0
          });
        }

        function addSpring(targetWorld, col, row = GROUND_ROW - 1) {
          targetWorld.springs.push({
            x: col * TILE + 4,
            y: row * TILE + 6,
            w: 24,
            h: 26,
            compressTimer: 0
          });
        }

        function addSpikes(targetWorld, startCol, count, row = GROUND_ROW - 1) {
          targetWorld.hazards.push({
            type: 'spikes',
            x: startCol * TILE,
            y: row * TILE + 14,
            w: count * TILE,
            h: 18
          });
        }

        function addFireBar(targetWorld, col, row, lengthBalls = 4, speed = 0.045) {
          addBlock(targetWorld, col, row, 'hard', null);
          targetWorld.hazards.push({
            type: 'firebar',
            cx: col * TILE + 16,
            cy: row * TILE + 16,
            balls: lengthBalls,
            angle: 0,
            speed
          });
        }

        function addThwomp(targetWorld, col, row = 3) {
          targetWorld.hazards.push({
            type: 'thwomp',
            x: col * TILE,
            y: row * TILE,
            homeY: row * TILE,
            w: 48,
            h: 56,
            vy: 0,
            state: 'idle', // 'idle' | 'falling' | 'wait' | 'rising'
            timer: 0
          });
        }

        function addPodoboo(targetWorld, col) {
          targetWorld.hazards.push({
            type: 'podoboo',
            x: col * TILE + 6,
            y: VIEW_H + 10,
            baseY: VIEW_H + 10,
            w: 20,
            h: 24,
            vy: -11.5,
            timer: (col * 17) % 90
          });
        }

        // 배경 장식물 세팅
        for (let cx = 80; cx < WORLD_WIDTH; cx += 280) {
          if (stageNum !== 2 && stageNum !== 5) {
            ow.decors.push({
              type: 'cloud',
              x: cx + (cx % 70),
              y: 44 + ((cx * 3) % 65),
              w: 76 + ((cx % 2) * 28),
              h: 34
            });
          } else if (stageNum === 2) {
            ow.decors.push({
              type: 'crystal',
              x: cx,
              y: TILE * 2,
              w: 28,
              h: 42 + (cx % 24)
            });
          } else if (stageNum === 5) {
            ow.decors.push({
              type: 'chain',
              x: cx,
              y: TILE * 2,
              w: 16,
              h: 72
            });
          }
        }

        const hillPositions = [0, 480, 1020, 1580, 2150, 2720, 3300];
        hillPositions.forEach((hx, idx) => {
          if (stageNum === 1 || stageNum === 3) {
            ow.decors.push({
              type: 'hill',
              x: hx,
              y: GROUND_Y,
              w: idx % 2 === 0 ? 160 : 112,
              h: idx % 2 === 0 ? 74 : 48
            });
          } else if (stageNum === 4) {
            ow.decors.push({
              type: 'pyramid',
              x: hx,
              y: GROUND_Y,
              w: idx % 2 === 0 ? 180 : 130,
              h: idx % 2 === 0 ? 96 : 68
            });
          }
        });

        // 스테이지별 낭떠러지/용암 구간 설정
        let pitRanges = [
          [28, 30],
          [55, 58],
          [85, 87]
        ];
        if (stageNum === 2) {
          pitRanges = [
            [26, 30],
            [52, 57],
            [82, 86]
          ];
        } else if (stageNum === 3) {
          pitRanges = [
            [24, 29],
            [47, 54],
            [78, 85]
          ];
        } else if (stageNum === 4) {
          pitRanges = [
            [27, 30],
            [56, 59],
            [84, 87]
          ];
        } else if (stageNum === 5) {
          pitRanges = [
            [24, 28],
            [50, 55],
            [78, 84]
          ];
        }

        const isPitTile = (col) => pitRanges.some(([s, e]) => col >= s && col <= e);
        const totalCols = Math.floor(WORLD_WIDTH / TILE);

        for (let col = 0; col < totalCols; col++) {
          if (!isPitTile(col)) {
            ow.solids.push({
              type: 'ground',
              x: col * TILE,
              y: GROUND_Y,
              w: TILE,
              h: VIEW_H - GROUND_Y
            });
          }
          // 지하 동굴(Stage 2) 및 쿠파 성채(Stage 5)는 천장 타일 추가
          if (theme.hasCeiling && col >= 2 && col <= 108 && (col < 22 || col > 25)) {
            addBlock(ow, col, 1, 'brick', null);
          }
        }

        // 중간 체크포인트 (모든 스테이지 타일 60 부근)
        checkpoint = {
          x: 60 * TILE + 8,
          y: (GROUND_ROW - 3) * TILE,
          w: 16,
          h: 3 * TILE
        };

        // 도착 지점 깃대 (타일 112)
        addBlock(ow, 112, GROUND_ROW - 1, 'hard', null);
        flagpole = {
          x: 112 * TILE + 12,
          y: (GROUND_ROW - 10) * TILE,
          w: 8,
          h: 9 * TILE,
          flagY: (GROUND_ROW - 10) * TILE + 10,
          targetFlagY: (GROUND_ROW - 2) * TILE - 8,
          castleX: 114 * TILE + 16,
          castleY: GROUND_Y - 128
        };

        // ==========================================
        // 각 스테이지별 세부 레이아웃 & 고유 몬스터·장애물 배치
        // (스폰킬 방지를 위해 시작 구간 타일 0~14에는 몬스터를 절대 배치하지 않고 안전 구역 보장!)
        // ==========================================
        if (stageNum === 1) {
          // [STAGE 1: 초원 지상 월드] - 굼바, 엉금엉금, 뻐끔플라워, 배관 비밀방
          addBlock(ow, 7, 8, 'question', 'coin');
          addBlock(ow, 10, 8, 'brick', 'multicoin');
          addBlock(ow, 11, 8, 'question', 'mushroom');
          addBlock(ow, 12, 8, 'brick', null);
          addBlock(ow, 13, 8, 'question', 'flower');
          addBlock(ow, 14, 8, 'brick', null);
          addBlock(ow, 12, 4, 'question', '1up'); // 1UP 초록 버섯 블록!

          // 토관 16이 시작 구역과 몬스터 구역을 안전하게 분리! (스폰킬 100% 차단)
          addPipe(ow, 16, 2);
          addPipe(ow, 23, 3, {
            canEnterDown: true,
            label: '▼ SECRET PIPE',
            warpTargetZone: 'UNDERGROUND',
            warpSpawnX: 84,
            warpSpawnY: 64
          });

          addEnemy(ow, 20, GROUND_ROW - 1, -1, 'goomba', 18, 22);
          addEnemy(ow, 26, GROUND_ROW - 1, -1, 'koopa', 25, 27);

          // 섹션 2
          addPipe(ow, 33, 3, {
            isWarpExit: true,
            canEnterDown: true,
            label: '▼ BONUS PIPE',
            warpTargetZone: 'UNDERGROUND',
            warpSpawnX: 84,
            warpSpawnY: 64
          });
          addBlock(ow, 38, 8, 'brick', null);
          addBlock(ow, 39, 8, 'question', 'flower');
          addBlock(ow, 40, 8, 'brick', 'star');

          for (let c = 41; c <= 46; c++) {
            addBlock(ow, c, 4, c === 44 ? 'question' : 'brick', 'coin');
          }
          addFieldCoins(ow, [42, 43, 44, 45], 2);

          addPipe(ow, 49, 4, { hasPiranha: true });
          addBlock(ow, 56, 7, 'hard', null);
          addBlock(ow, 57, 7, 'hard', null);

          addEnemy(ow, 37, GROUND_ROW - 1, 1, 'goomba', 35, 48);
          addEnemy(ow, 44, GROUND_ROW - 1, -1, 'koopa', 35, 48);

          // 섹션 3
          addStairs(ow, 62, 4, 'up');
          addStairs(ow, 68, 4, 'down');

          addBlock(ow, 74, 8, 'brick', 'multicoin');
          addBlock(ow, 75, 8, 'question', 'flower');
          addBlock(ow, 76, 8, 'question', 'coin');
          addBlock(ow, 75, 4, 'question', 'star');

          addPipe(ow, 81, 3, { hasPiranha: true });
          addEnemy(ow, 73, GROUND_ROW - 1, -1, 'goomba', 66, 80);
          addEnemy(ow, 78, GROUND_ROW - 1, 1, 'koopa', 66, 80);

          // 섹션 4
          addPipe(ow, 90, 2);
          addBlock(ow, 94, 8, 'question', 'coin');
          addBlock(ow, 95, 8, 'question', 'flower');
          addEnemy(ow, 96, GROUND_ROW - 1, -1, 'goomba', 92, 100);
          addEnemy(ow, 99, GROUND_ROW - 1, -1, 'koopa', 92, 100);

          addStairs(ow, 102, 7, 'up');
        } else if (stageNum === 2) {
          // [STAGE 2: 지하 수정 동굴] - 파이어볼 면역 딱정벌레(Buzzy Beetle), 상하/좌우 이동 리프트, 배관 워프
          addBlock(ow, 7, 8, 'question', 'mushroom');
          addBlock(ow, 9, 8, 'question', 'flower');
          addBlock(ow, 11, 8, 'brick', 'multicoin');
          addPipe(ow, 15, 2);

          addPipe(ow, 21, 3, {
            canEnterDown: true,
            label: '▼ SECRET PIPE',
            warpTargetZone: 'UNDERGROUND',
            warpSpawnX: 84,
            warpSpawnY: 64
          });

          addEnemy(ow, 19, GROUND_ROW - 1, -1, 'beetle', 17, 20); // 딱정벌레 몬스터!

          // 넓은 낭떠러지(26~30) 위 좌우 이동 엘리베이터 플랫폼
          addMovingPlatform(ow, 26, 8, 3, 72, 0, 0.035);
          addFieldCoins(ow, [27, 28, 29], 5);

          addPipe(ow, 33, 3, {
            isWarpExit: true,
            canEnterDown: true,
            label: '▼ BONUS PIPE',
            warpTargetZone: 'UNDERGROUND',
            warpSpawnX: 84,
            warpSpawnY: 64
          });

          for (let c = 37; c <= 44; c++) {
            addBlock(ow, c, 7, c % 2 === 0 ? 'question' : 'brick', c === 40 ? 'flower' : 'coin');
          }
          addEnemy(ow, 39, GROUND_ROW - 1, 1, 'goomba', 35, 47);
          addEnemy(ow, 43, GROUND_ROW - 1, -1, 'beetle', 35, 47);
          addPipe(ow, 48, 3, { hasPiranha: true });

          // 두 번째 낭떠러지(52~57) 위 상하 승강 리프트
          addMovingPlatform(ow, 53, 8, 3, 0, 64, 0.04);
          addFieldCoins(ow, [54, 55], 4);

          addStairs(ow, 62, 4, 'up');
          addBlock(ow, 69, 8, 'question', 'star');
          addBlock(ow, 71, 8, 'question', 'flower');
          addEnemy(ow, 73, GROUND_ROW - 1, -1, 'beetle', 67, 79);
          addEnemy(ow, 77, GROUND_ROW - 1, -1, 'goomba', 67, 79);

          // 세 번째 낭떠러지(82~86) 위 이동 플랫폼
          addMovingPlatform(ow, 82, 7, 3, 64, 0, 0.04);

          addPipe(ow, 90, 2, { hasPiranha: true });
          addEnemy(ow, 95, GROUND_ROW - 1, -1, 'beetle', 92, 100);
          addStairs(ow, 102, 7, 'up');
        } else if (stageNum === 3) {
          // [STAGE 3: 구름 고원 애슬레틱] - 스프링 점프대(Trampoline), 날개 엉금엉금(Paratroopa), 빨간 엉금엉금, 구름 리프트
          addBlock(ow, 7, 8, 'question', 'mushroom');
          addBlock(ow, 10, 8, 'question', 'flower');
          addSpring(ow, 14, GROUND_ROW - 1); // ★ 첫 번째 스프링 점프대!
          addFieldCoins(ow, [14, 15, 16], 4);

          for (let c = 16; c <= 21; c++) {
            addBlock(ow, c, 5, c === 19 ? 'question' : 'hard', '1up');
          }

          addEnemy(ow, 19, GROUND_ROW - 1, -1, 'paratroopa', 16, 23); // ★ 날개 달린 점프 엉금엉금!

          // 첫 번째 넓은 구름 협곡(24~29) + 이동 구름 리프트
          addSpring(ow, 23, GROUND_ROW - 1);
          addMovingPlatform(ow, 25, 7, 3, 80, 0, 0.035);
          addFieldCoins(ow, [26, 27, 28], 4);

          addPipe(ow, 32, 2, {
            canEnterDown: true,
            label: '▼ SKY PIPE',
            warpTargetZone: 'UNDERGROUND',
            warpSpawnX: 84,
            warpSpawnY: 64
          });

          addBlock(ow, 37, 8, 'question', 'flower');
          addBlock(ow, 39, 8, 'question', 'star');
          addEnemy(ow, 38, GROUND_ROW - 1, -1, 'red_koopa', 35, 45); // 절벽에서 안 떨어지는 빨간 엉금엉금!
          addEnemy(ow, 43, GROUND_ROW - 1, -1, 'paratroopa', 35, 45);

          // 두 번째 대협곡(47~54) + 2단 이동 구름 리프트
          addSpring(ow, 46, GROUND_ROW - 1);
          addMovingPlatform(ow, 48, 8, 3, 55, 0, 0.035);
          addMovingPlatform(ow, 52, 6, 3, 0, 50, 0.04);
          addFieldCoins(ow, [49, 50, 52, 53], 3);

          addPipe(ow, 56, 2, { isWarpExit: true });

          addBlock(ow, 65, 8, 'question', 'flower');
          addBlock(ow, 67, 8, 'brick', 'multicoin');
          addEnemy(ow, 70, GROUND_ROW - 1, -1, 'paratroopa', 64, 76);
          addEnemy(ow, 74, GROUND_ROW - 1, 1, 'red_koopa', 64, 76);

          // 세 번째 협곡(78~85)
          addSpring(ow, 77, GROUND_ROW - 1);
          addMovingPlatform(ow, 79, 7, 3, 95, 0, 0.04);

          addEnemy(ow, 92, GROUND_ROW - 1, -1, 'paratroopa', 88, 100);
          addStairs(ow, 102, 7, 'up');
        } else if (stageNum === 4) {
          // [STAGE 4: 사막 피라미드 유적] - 가시돌이(Spiny: 밟기 불가, 파이어볼/등껍질로 처치!), 가시 함정(Spikes), 피라미드 계단
          addBlock(ow, 7, 8, 'question', 'mushroom');
          addBlock(ow, 9, 8, 'question', 'flower'); // 가시돌이 상대용 파이어 플라워 초반 제공!
          addBlock(ow, 11, 8, 'question', 'coin');

          addPipe(ow, 15, 2);
          // 가시 함정 지대 + 위쪽 징검다리 블록
          addSpikes(ow, 19, 3, GROUND_ROW - 1);
          addBlock(ow, 19, 8, 'hard', null);
          addBlock(ow, 20, 8, 'question', 'coin');
          addBlock(ow, 21, 8, 'hard', null);

          addEnemy(ow, 24, GROUND_ROW - 1, -1, 'spiny', 22, 26); // ★ 등 가시 몬스터 가시돌이!

          // 사막 유사(27~30) 위 발판
          addMovingPlatform(ow, 27, 8, 3, 60, 0, 0.035);

          addPipe(ow, 33, 3, {
            canEnterDown: true,
            label: '▼ TOMB PIPE',
            warpTargetZone: 'UNDERGROUND',
            warpSpawnX: 84,
            warpSpawnY: 64
          });

          // 피라미드 지형
          addStairs(ow, 37, 4, 'up');
          addStairs(ow, 41, 4, 'down');
          addBlock(ow, 40, 4, 'question', 'star');

          addEnemy(ow, 47, GROUND_ROW - 1, -1, 'koopa', 45, 54); // 등껍질로 가시돌이 올킬 가능!
          addEnemy(ow, 51, GROUND_ROW - 1, -1, 'spiny', 45, 54);
          addPipe(ow, 53, 3, { hasPiranha: true, isWarpExit: true });

          addMovingPlatform(ow, 56, 7, 3, 50, 0, 0.04);

          addBlock(ow, 64, 8, 'question', 'flower');
          addBlock(ow, 66, 8, 'brick', 'multicoin');
          addSpikes(ow, 69, 3, GROUND_ROW - 1);
          addSpring(ow, 68, GROUND_ROW - 1);
          addFieldCoins(ow, [69, 70, 71], 4);

          addEnemy(ow, 76, GROUND_ROW - 1, -1, 'spiny', 73, 82);
          addEnemy(ow, 80, GROUND_ROW - 1, 1, 'spiny', 73, 82);

          addMovingPlatform(ow, 84, 8, 3, 55, 0, 0.04);

          addEnemy(ow, 93, GROUND_ROW - 1, -1, 'koopa', 89, 100);
          addEnemy(ow, 97, GROUND_ROW - 1, -1, 'spiny', 89, 100);
          addStairs(ow, 102, 7, 'up');
        } else if (stageNum === 5) {
          // [STAGE 5: 쿠파의 용암 성채] - 쿵쿵이(Thwomp), 회전 화염바(Fire Bar), 용암 불덩이(Podoboo), 용암 다리
          addBlock(ow, 7, 8, 'question', 'mushroom');
          addBlock(ow, 9, 8, 'question', 'flower');
          addBlock(ow, 11, 8, 'question', '1up');

          addBlock(ow, 15, 8, 'hard', null);
          // ★ 첫 번째 회전 화염 막대 (Fire Bar)
          addFireBar(ow, 19, 8, 4, 0.042);

          // 첫 번째 용암 구덩이(24~28) + 용암에서 솟구치는 화염구(Podoboo) + 이동 다리
          addPodoboo(ow, 25);
          addPodoboo(ow, 27);
          addMovingPlatform(ow, 25, 8, 3, 64, 0, 0.038);

          // ★ 천장에서 내려찍는 쿵쿵이 (Thwomp #1)
          addThwomp(ow, 34, 3);
          addBlock(ow, 39, 8, 'question', 'flower');
          addBlock(ow, 41, 8, 'question', 'star');

          addEnemy(ow, 43, GROUND_ROW - 1, -1, 'beetle', 37, 48);
          addFireBar(ow, 46, 8, 4, -0.045);

          // 두 번째 용암 구덩이(50~55) + 솟구치는 화염구 2개 + 다리
          addPodoboo(ow, 51);
          addPodoboo(ow, 54);
          addMovingPlatform(ow, 51, 7, 3, 76, 0, 0.04);

          // 섹션 3: 쿵쿵이 회랑 & 화염바
          addThwomp(ow, 65, 3);
          addFireBar(ow, 71, 8, 4, 0.048);
          addBlock(ow, 74, 8, 'question', 'flower');
          addEnemy(ow, 75, GROUND_ROW - 1, -1, 'spiny', 68, 77);

          // 세 번째 용암 구덩이(78~84)
          addPodoboo(ow, 79);
          addPodoboo(ow, 82);
          addMovingPlatform(ow, 79, 8, 3, 88, 0, 0.042);

          // 최종 성채 수비대 & 쿵쿵이 #3
          addThwomp(ow, 90, 3);
          addEnemy(ow, 95, GROUND_ROW - 1, -1, 'red_koopa', 92, 100);
          addEnemy(ow, 98, GROUND_ROW - 1, -1, 'beetle', 92, 100);
          addStairs(ow, 102, 7, 'up');
        }

        // ==========================================
        // [Part 2] 지하 비밀 보너스 공간 (UNDERGROUND BONUS ROOM) 구성
        // ==========================================
        const ugCols = Math.floor(UNDERGROUND_WIDTH / TILE);
        for (let c = 0; c < ugCols; c++) {
          ug.solids.push({
            type: 'ug_ground',
            x: c * TILE,
            y: GROUND_Y,
            w: TILE,
            h: VIEW_H - GROUND_Y
          });
        }
        for (let c = 4; c < ugCols - 1; c++) {
          addBlock(ug, c, 1, 'ug_brick', null);
        }
        for (let r = 1; r < GROUND_ROW; r++) {
          addBlock(ug, 0, r, 'ug_brick', null);
          addBlock(ug, ugCols - 1, r, 'ug_brick', null);
        }
        ug.solids.push({
          type: 'pipe',
          x: 2 * TILE,
          y: 0,
          w: TILE * 2,
          h: TILE * 2,
          isCeilingPipe: true
        });
        for (let c = 6; c <= 16; c++) {
          addBlock(ug, c, 8, 'ug_brick', null);
        }
        addBlock(ug, 8, 5, 'question', 'flower');
        addBlock(ug, 10, 5, 'question', 'star');
        addBlock(ug, 12, 5, 'question', '1up');
        addBlock(ug, 14, 5, 'question', 'mushroom');

        for (let c = 6; c <= 16; c++) {
          ug.fieldCoins.push({ x: c * TILE + 7, y: 7 * TILE + 5, w: 18, h: 22, collected: false });
          ug.fieldCoins.push({ x: c * TILE + 7, y: 10 * TILE + 5, w: 18, h: 22, collected: false });
        }
        for (let c = 7; c <= 15; c += 2) {
          ug.fieldCoins.push({ x: c * TILE + 7, y: 3 * TILE + 5, w: 18, h: 22, collected: false });
        }

        // 지하 출구 배관 찾기
        const exitPipeInOw = ow.solids.find((s) => s.type === 'pipe' && s.isWarpExit) || { x: 33 * TILE, y: (GROUND_ROW - 3) * TILE };
        ug.solids.push({
          type: 'pipe',
          x: 21 * TILE,
          y: (GROUND_ROW - 2) * TILE,
          w: TILE * 2,
          h: TILE * 2,
          canExitRight: true,
          canEnterDown: true,
          label: '▶ EXIT PIPE',
          warpTargetZone: 'OVERWORLD',
          warpSpawnX: exitPipeInOw.x + 19,
          warpSpawnY: exitPipeInOw.y
        });

        setActiveZone('OVERWORLD');
      }

      // ==========================================
      // 7. 배관(토관) 워프 전환 시퀀스
      // ==========================================
      function startPipeWarp(pipe, entryDir) {
        if (gameState !== 'PLAYING') return;
        sound.pipeWarp();
        gameState = 'PIPE_WARP';
        player.vx = 0;
        player.vy = 0;

        if (entryDir === 'DOWN') {
          player.x = pipe.x + (pipe.w - player.w) / 2;
        }

        warpState = {
          phase: 'ENTER',
          dir: entryDir,
          timer: 26,
          targetZone: pipe.warpTargetZone,
          spawnX: pipe.warpSpawnX,
          spawnY: pipe.warpSpawnY
        };
      }

      function updatePipeWarp() {
        if (!warpState) return;

        if (warpState.phase === 'ENTER') {
          if (warpState.dir === 'DOWN') player.y += 1.8;
          else player.x += 1.8;
          warpState.timer--;

          if (warpState.timer <= 0) {
            setActiveZone(warpState.targetZone);

            if (warpState.targetZone === 'UNDERGROUND') {
              cameraX = 0;
              player.x = warpState.spawnX;
              player.y = warpState.spawnY;
              player.vx = 0;
              player.vy = 0;
              player.invincibleTimer = 60;
              addPopup(player.x + 20, player.y + 10, 'SECRET BONUS ROOM!', '#38bdf8');
              warpState = null;
              gameState = 'PLAYING';
            } else {
              player.x = warpState.spawnX;
              player.y = warpState.spawnY + player.h;
              cameraX = Math.max(0, Math.min(WORLD_WIDTH - VIEW_W, player.x - VIEW_W * 0.35));
              sound.pipeWarp();
              warpState.phase = 'EXIT';
              warpState.timer = Math.ceil(player.h / 1.8);
            }
          }
        } else if (warpState.phase === 'EXIT') {
          player.y -= 1.8;
          warpState.timer--;
          if (warpState.timer <= 0) {
            player.y = warpState.spawnY - player.h;
            player.vy = 0;
            player.onGround = true;
            player.invincibleTimer = 75; // 토관 탈출 직후 스폰킬 보호!
            addPopup(player.x, player.y - 12, 'WARP COMPLETE!', '#4ade80');
            warpState = null;
            gameState = 'PLAYING';
          }
        }
      }

      // ==========================================
      // 8. 파이어 마리오 불꽃 탄환 발사 및 물리 업데이트
      // ==========================================
      function tryShootFireball() {
        if (player.powerLevel < 2 || player.fireCooldown > 0 || fireballs.length >= 3) {
          return;
        }
        sound.fireball();
        player.fireCooldown = 14;
        const spawnX = player.facing === 1 ? player.x + player.w + 2 : player.x - 12;
        const spawnY = player.y + Math.floor(player.h * 0.38);

        fireballs.push({
          x: spawnX,
          y: spawnY,
          w: 12,
          h: 12,
          vx: player.facing * 7.2,
          vy: 2.2,
          bounces: 0,
          maxBounces: 5,
          life: 150
        });
      }

      function updateFireballs() {
        const maxW = currentZone === 'UNDERGROUND' ? UNDERGROUND_WIDTH : WORLD_WIDTH;

        for (let i = fireballs.length - 1; i >= 0; i--) {
          const fb = fireballs[i];
          fb.life--;
          if (fb.life <= 0 || fb.x < cameraX - 60 || fb.x > cameraX + VIEW_W + 60 || fb.y > VIEW_H + 40) {
            fireballs.splice(i, 1);
            continue;
          }

          if (frameCount % 2 === 0) {
            particles.push({
              type: 'dust',
              x: fb.x + 4,
              y: fb.y + 4,
              vx: -fb.vx * 0.15,
              vy: -0.4,
              size: 4,
              color: frameCount % 4 === 0 ? '#facc15' : '#f97316',
              gravity: 0,
              life: 8
            });
          }

          fb.x += fb.vx;
          let hitWall = false;
          for (let j = 0; j < solids.length; j++) {
            if (aabbOverlap(fb, solids[j])) {
              hitWall = true;
              break;
            }
          }
          if (hitWall || fb.x <= 0 || fb.x + fb.w >= maxW) {
            sound.bump();
            spawnFireBurst(fb.x + 6, fb.y + 6);
            fireballs.splice(i, 1);
            continue;
          }

          fb.vy += 0.48;
          if (fb.vy > 8.5) fb.vy = 8.5;
          fb.y += fb.vy;

          let destroyedByCeiling = false;
          for (let j = 0; j < solids.length; j++) {
            const s = solids[j];
            if (aabbOverlap(fb, s)) {
              if (fb.vy > 0) {
                fb.y = s.y - fb.h;
                fb.vy = -5.8;
                fb.bounces++;
                if (fb.bounces > fb.maxBounces) destroyedByCeiling = true;
              } else {
                destroyedByCeiling = true;
              }
              break;
            }
          }
          if (destroyedByCeiling) {
            spawnFireBurst(fb.x + 6, fb.y + 6);
            fireballs.splice(i, 1);
            continue;
          }

          // 몬스터 타격 (딱정벌레 beetle은 등껍질이 단단해 파이어볼 면역!)
          let hitTarget = false;
          for (let k = enemies.length - 1; k >= 0; k--) {
            const en = enemies[k];
            if (en.state !== 'squished' && aabbOverlap(fb, en)) {
              if (en.type === 'beetle') {
                sound.bump();
                spawnDust(fb.x, fb.y, 5, '#93c5fd');
                addPopup(en.x, en.y - 10, 'SHELL IMMUNE!', '#93c5fd');
                hitTarget = true;
                break;
              }
              sound.kick();
              score += 200;
              spawnFireBurst(en.x + en.w / 2, en.y + en.h / 2);
              addPopup(en.x, en.y - 10, 'FIRE +200', '#fb923c');
              enemies.splice(k, 1);
              hitTarget = true;
              break;
            }
          }

          if (!hitTarget) {
            for (let p = 0; p < piranhas.length; p++) {
              const pir = piranhas[p];
              if (pir.alive && pir.y < pir.baseY - 6 && aabbOverlap(fb, pir)) {
                pir.alive = false;
                sound.kick();
                score += 300;
                spawnFireBurst(pir.x + pir.w / 2, pir.y + pir.h / 2);
                addPopup(pir.x, pir.y - 10, 'FIRE +300', '#fb923c');
                hitTarget = true;
                break;
              }
            }
          }

          if (hitTarget) {
            fireballs.splice(i, 1);
          }
        }
      }

      // ==========================================
      // 9. 블록 타격 및 아이템 생성
      // ==========================================
      function gainCoin() {
        coins += 1;
        score += 200;
        if (coins >= 100) {
          coins -= 100;
          lives += 1;
          sound.oneUp();
          addPopup(player.x, player.y - 20, '100 COINS! 1-UP!', '#4ade80');
        } else {
          sound.coin();
        }
      }

      function triggerBlockHit(block) {
        if (block.type === 'hard' || block.type === 'pipe' || block.type === 'ground' || block.type === 'ug_ground') {
          sound.bump();
          return;
        }
        if (block.type === 'empty') {
          sound.bump();
          return;
        }

        block.bumpVy = -4.2;

        const topSensor = { x: block.x + 2, y: block.y - 10, w: block.w - 4, h: 12 };
        enemies.forEach((en) => {
          if (en.state !== 'squished' && aabbOverlap(topSensor, en)) {
            en.state = 'squished';
            en.squishTimer = 25;
            score += 100;
            sound.stomp();
            addPopup(en.x, en.y - 10, '+100', '#fde047');
          }
        });

        if (block.content === 'multicoin' && block.coinsLeft > 0) {
          block.coinsLeft--;
          gainCoin();
          items.push({
            type: 'popCoin',
            x: block.x + 8,
            y: block.y - 16,
            w: 16,
            h: 22,
            vy: -7.5,
            life: 28
          });
          addPopup(block.x + 4, block.y - 24, '+200', '#facc15');
          if (block.coinsLeft <= 0) {
            block.type = 'empty';
            block.content = null;
          }
          return;
        }

        if (block.type === 'question' || (block.type === 'brick' && block.content)) {
          block.type = 'empty';

          if (block.content === 'coin') {
            gainCoin();
            items.push({
              type: 'popCoin',
              x: block.x + 8,
              y: block.y - 16,
              w: 16,
              h: 22,
              vy: -7.5,
              life: 28
            });
            addPopup(block.x + 4, block.y - 24, '+200', '#facc15');
          } else if (block.content === 'mushroom') {
            sound.itemSpawn();
            const spawnType = player.powerLevel >= 1 ? 'flower' : 'mushroom';
            items.push({
              type: spawnType,
              x: block.x + 2,
              y: block.y - 4,
              targetRiseY: block.y - 28,
              w: 28,
              h: 28,
              vx: spawnType === 'mushroom' ? 1.85 : 0,
              vy: 0,
              state: 'rising'
            });
          } else if (block.content === '1up') {
            sound.itemSpawn();
            items.push({
              type: '1up',
              x: block.x + 2,
              y: block.y - 4,
              targetRiseY: block.y - 28,
              w: 28,
              h: 28,
              vx: 1.9,
              vy: 0,
              state: 'rising'
            });
          } else if (block.content === 'flower') {
            sound.itemSpawn();
            items.push({
              type: 'flower',
              x: block.x + 2,
              y: block.y - 4,
              targetRiseY: block.y - 28,
              w: 28,
              h: 28,
              vx: 0,
              vy: 0,
              state: 'rising'
            });
          } else if (block.content === 'star') {
            sound.itemSpawn();
            items.push({
              type: 'star',
              x: block.x + 2,
              y: block.y - 4,
              targetRiseY: block.y - 28,
              w: 28,
              h: 28,
              vx: 2.3,
              vy: -5.5,
              state: 'rising'
            });
          }
          block.content = null;
        } else if (block.type === 'brick' || block.type === 'ug_brick') {
          if (player.powerLevel >= 1) {
            sound.brickSmash();
            score += 50;
            const theme = STAGE_THEMES[currentStage] || STAGE_THEMES[1];
            spawnBrickDebris(block.x, block.y, block.type === 'ug_brick' ? '#2563eb' : theme.brickMain);
            const idx = solids.indexOf(block);
            if (idx !== -1) solids.splice(idx, 1);
          } else {
            sound.bump();
          }
        }
      }

      // ==========================================
      // 10. 플레이어 피격, 목숨 3개 차감 및 스테이지 진행 처리
      // ==========================================
      function setPlayerPower(newLevel) {
        const oldBottom = player.y + player.h;
        player.powerLevel = newLevel;
        player.isSuper = newLevel >= 1;
        player.h = newLevel >= 1 ? 48 : 32;
        player.y = oldBottom - player.h;
      }

      function triggerPlayerDamage() {
        if (player.invincibleTimer > 0 || player.transformTimer > 0 || player.starTimer > 0 || gameState !== 'PLAYING') {
          return;
        }

        if (player.powerLevel === 2) {
          sound.powerDown();
          setPlayerPower(1);
          player.invincibleTimer = 110;
          player.transformTimer = 18;
          addPopup(player.x, player.y - 12, 'FIRE LOST!', '#fb923c');
          spawnDust(player.x + player.w / 2, player.y + player.h / 2, 8);
        } else if (player.powerLevel === 1) {
          sound.powerDown();
          setPlayerPower(0);
          player.invincibleTimer = 110;
          player.transformTimer = 18;
          addPopup(player.x, player.y - 12, 'POWER DOWN!', '#f87171');
          spawnDust(player.x + player.w / 2, player.y + player.h / 2, 8);
        } else {
          triggerPlayerDeath();
        }
      }

      function triggerPlayerDeath() {
        if (gameState !== 'PLAYING') return;
        gameState = 'DYING';
        lives = Math.max(0, lives - 1);
        if (lives > 0) {
          sound.lifeLost();
        } else {
          sound.gameOver();
        }
        player.vx = 0;
        player.vy = -10.5;
      }

      function respawnAfterLifeLost() {
        // 목숨이 남아있을 때는 게임 오버 없이 즉시 체크포인트 또는 스테이지 시작점에서 스폰 무적 상태로 부활!
        setActiveZone('OVERWORLD');
        timeLeft = 400;
        timeAccumulator = 0;
        gameState = 'PLAYING';
        warpState = null;
        keys.jumpJustPressed = false;
        keys.fireJustPressed = false;

        player = createPlayer(checkpointReached, 0);
        player.invincibleTimer = 120; // 2초간 부활 스폰 무적 보호!

        // 부활 지점 좌우 180px 이내에 있는 몬스터들을 뒤로 밀어내어 부활 직후 스폰킬 완전 차단!
        enemies.forEach((en) => {
          if (Math.abs(en.x - player.x) < 180) {
            en.x = Math.max(en.minX || 16 * TILE, player.x + 220);
            en.vx = Math.abs(en.vx);
          }
        });

        cameraX = Math.max(0, player.x - VIEW_W * 0.35);
        addPopup(player.x, player.y - 16, `LIVES LEFT: ${lives}`, '#fde047');
      }

      function showModal(mode) {
        const modal = document.getElementById('overlayModal');
        const card = document.getElementById('modalCard');
        const title = document.getElementById('modalTitle');
        const sub = document.getElementById('modalSubtitle');
        const scoreEl = document.getElementById('modalScore');
        const coinsEl = document.getElementById('modalCoins');
        const timeEl = document.getElementById('modalTime');
        const nextBtn = document.getElementById('btnModalNext');

        card.className = 'modal-card ' + (mode === 'CLEAR' ? 'clear' : 'gameover');
        if (mode === 'CLEAR') {
          if (currentStage < 5) {
            title.textContent = `STAGE 1-${currentStage} CLEAR!`;
            sub.textContent = `${STAGE_THEMES[currentStage].korName} 스테이지를 돌파했습니다! 다음 스테이지(${STAGE_THEMES[currentStage + 1].korName})로 진격하세요!`;
            nextBtn.style.display = 'inline-flex';
            nextBtn.textContent = `다음 스테이지 1-${currentStage + 1} (N)`;
          } else {
            title.textContent = 'ALL 5 STAGES CLEAR!';
            sub.textContent = '축하합니다! 초원부터 쿠파의 용암 성채까지 5개 월드를 모두 제패했습니다!';
            nextBtn.style.display = 'none';
          }
        } else {
          title.textContent = 'GAME OVER';
          sub.textContent = '목숨 3개를 모두 소진했습니다. 키보드 R 키 또는 아래 버튼을 눌러 다시 도전하세요!';
          nextBtn.style.display = 'none';
        }

        scoreEl.textContent = String(score).padStart(6, '0');
        coinsEl.textContent = String(coins).padStart(2, '0');
        timeEl.textContent = String(Math.max(0, Math.ceil(timeLeft))).padStart(3, '0');

        modal.classList.add('visible');
      }

      function hideModal() {
        const modal = document.getElementById('overlayModal');
        modal.classList.remove('visible');
      }

      function goToStage(stageNum) {
        hideModal();
        currentStage = Math.max(1, Math.min(5, stageNum));
        updateStageButtonsUI();
        checkpointReached = false;
        timeLeft = 400;
        timeAccumulator = 0;
        gameState = 'PLAYING';
        warpState = null;
        keys.jumpJustPressed = false;
        keys.fireJustPressed = false;
        const prevPower = player ? player.powerLevel : 0;
        buildStage(currentStage);
        player = createPlayer(false, prevPower);
        cameraX = 0;
        addPopup(player.x + 20, player.y - 16, `STAGE 1-${currentStage}: ${STAGE_THEMES[currentStage].name}`, '#fde047');
      }

      function startFullNewGame(startStage = 1) {
        hideModal();
        lives = 3; // 목숨 3개로 초기화
        score = 0;
        coins = 0;
        currentStage = startStage;
        updateStageButtonsUI();
        checkpointReached = false;
        timeLeft = 400;
        timeAccumulator = 0;
        gameState = 'PLAYING';
        warpState = null;
        keys.jumpJustPressed = false;
        keys.fireJustPressed = false;
        buildStage(currentStage);
        player = createPlayer(false, 0);
        cameraX = 0;
      }

      // ==========================================
      // 11. 메인 물리 및 게임 업데이트 루프
      // ==========================================
      function updateGame() {
        frameCount++;

        // 블록 튕김 애니메이션 업데이트
        for (let i = 0; i < solids.length; i++) {
          const b = solids[i];
          if (b.bumpOffset !== undefined && (b.bumpOffset !== 0 || b.bumpVy !== 0)) {
            b.bumpOffset += b.bumpVy;
            b.bumpVy += 0.65;
            if (b.bumpOffset >= 0) {
              b.bumpOffset = 0;
              b.bumpVy = 0;
            }
            b.y = b.baseY + b.bumpOffset;
          }
        }

        // 이동 리프트 플랫폼 업데이트
        for (let i = 0; i < platforms.length; i++) {
          const pf = platforms[i];
          const oldX = pf.x;
          const oldY = pf.y;
          pf.theta += pf.speed;
          pf.x = pf.startX + Math.sin(pf.theta) * pf.rangeX;
          pf.y = pf.startY + Math.sin(pf.theta) * pf.rangeY;
          pf.vx = pf.x - oldX;
          pf.vy = pf.y - oldY;
        }

        // 스프링 점프대 압축 복원 타이머
        for (let i = 0; i < springs.length; i++) {
          if (springs[i].compressTimer > 0) springs[i].compressTimer--;
        }

        // 파티클 및 팝업 업데이트
        for (let i = particles.length - 1; i >= 0; i--) {
          const pt = particles[i];
          pt.x += pt.vx;
          pt.y += pt.vy;
          pt.vy += pt.gravity || 0;
          if (pt.angle !== undefined) pt.angle += pt.vAngle;
          pt.life--;
          if (pt.life <= 0) particles.splice(i, 1);
        }

        for (let i = popups.length - 1; i >= 0; i--) {
          const pp = popups[i];
          pp.y += pp.vy;
          pp.life--;
          if (pp.life <= 0) popups.splice(i, 1);
        }

        if (gameState === 'PIPE_WARP') {
          updatePipeWarp();
          return;
        }

        // 사망 연출 및 목숨(3개) 체크
        if (gameState === 'DYING') {
          player.y += player.vy;
          player.vy += PHYSICS.gravity * 0.85;
          if (player.y > VIEW_H + 120) {
            if (lives > 0) {
              respawnAfterLifeLost();
            } else {
              gameState = 'GAME_OVER';
              showModal('GAME_OVER');
            }
          }
          return;
        }

        // 스테이지 클리어 연출
        if (gameState === 'CLEARING') {
          if (player.clearPhase === 1) {
            const slideSpeed = 4.2;
            const bottomLimit = GROUND_Y - TILE - player.h;
            if (player.y < bottomLimit) {
              player.y = Math.min(bottomLimit, player.y + slideSpeed);
            }
            if (flagpole.flagY < flagpole.targetFlagY) {
              flagpole.flagY = Math.min(flagpole.targetFlagY, flagpole.flagY + slideSpeed);
            }
            if (player.y >= bottomLimit && flagpole.flagY >= flagpole.targetFlagY) {
              player.clearPhase = 2;
              player.facing = 1;
            }
          } else if (player.clearPhase === 2) {
            player.vx = 2.6;
            player.x += player.vx;
            player.vy += PHYSICS.gravity;
            player.y += player.vy;
            if (player.y + player.h >= GROUND_Y) {
              player.y = GROUND_Y - player.h;
              player.vy = 0;
              player.onGround = true;
            }
            player.animTimer++;
            if (player.animTimer % 5 === 0) {
              player.animFrame = (player.animFrame + 1) % 3;
            }
            const castleDoorX = flagpole.castleX + 52;
            if (player.x >= castleDoorX) {
              player.clearPhase = 3;
              const timeBonus = Math.max(0, Math.floor(timeLeft)) * 10;
              score += timeBonus;
              addPopup(castleDoorX, GROUND_Y - 70, `TIME BONUS +${timeBonus}`, '#4ade80');
              gameState = 'STAGE_CLEAR';
              showModal('CLEAR');
            }
          }
          return;
        }

        if (gameState !== 'PLAYING') return;

        // 타이머 감소
        timeAccumulator++;
        if (timeAccumulator >= 60) {
          timeAccumulator = 0;
          timeLeft--;
          if (timeLeft <= 0) {
            timeLeft = 0;
            triggerPlayerDeath();
            return;
          }
        }

        if (player.invincibleTimer > 0) player.invincibleTimer--;
        if (player.transformTimer > 0) player.transformTimer--;
        if (player.fireCooldown > 0) player.fireCooldown--;
        if (player.starTimer > 0) {
          player.starTimer--;
          if (frameCount % 3 === 0) {
            spawnDust(player.x + player.w / 2, player.y + player.h / 2, 1, '#facc15');
          }
        }

        if (keys.fireJustPressed) {
          tryShootFireball();
          keys.fireJustPressed = false;
        }

        // ==========================================
        // A. 플레이어 물리 이동
        // ==========================================
        const accel = player.onGround ? PHYSICS.accelGround : PHYSICS.accelAir;
        const maxSpd = player.starTimer > 0 ? PHYSICS.maxSpeedX + PHYSICS.starSpeedBonus : PHYSICS.maxSpeedX;
        player.isSkidding = false;

        if (keys.left && !keys.right) {
          if (player.vx > 0.6 && player.onGround) {
            player.vx -= PHYSICS.decelSkid;
            player.isSkidding = true;
            if (frameCount % 4 === 0) spawnDust(player.x + player.w / 2, player.y + player.h, 2);
          } else {
            player.vx -= accel;
          }
          player.facing = -1;
        } else if (keys.right && !keys.left) {
          if (player.vx < -0.6 && player.onGround) {
            player.vx += PHYSICS.decelSkid;
            player.isSkidding = true;
            if (frameCount % 4 === 0) spawnDust(player.x + player.w / 2, player.y + player.h, 2);
          } else {
            player.vx += accel;
          }
          player.facing = 1;
        } else {
          const friction = player.onGround ? PHYSICS.frictionGround : PHYSICS.frictionAir;
          player.vx *= friction;
          if (Math.abs(player.vx) < 0.08) player.vx = 0;
        }

        if (player.vx > maxSpd) player.vx = maxSpd;
        if (player.vx < -maxSpd) player.vx = -maxSpd;

        if (keys.jumpJustPressed && player.onGround) {
          player.vy = PHYSICS.jumpInitialVy;
          player.onGround = false;
          player.jumpHoldTimer = PHYSICS.jumpHoldFramesMax;
          keys.jumpJustPressed = false;
          sound.jump(player.isSuper);
          spawnDust(player.x + player.w / 2, player.y + player.h, 5);
        }
        keys.jumpJustPressed = false;

        if (keys.jump && player.jumpHoldTimer > 0 && player.vy < 0) {
          player.vy += PHYSICS.jumpHoldGravity;
          player.jumpHoldTimer--;
        } else {
          if (!keys.jump && player.vy < -3.2 && player.jumpHoldTimer > 0) {
            player.vy *= 0.55;
          }
          player.jumpHoldTimer = 0;
          player.vy += PHYSICS.gravity;
        }

        if (player.vy > PHYSICS.maxFallSpeed) {
          player.vy = PHYSICS.maxFallSpeed;
        }

        // ==========================================
        // B. 플레이어 AABB 충돌 감지 (고정 지형 + 이동 플랫폼 + 스프링 점프대)
        // ==========================================
        const currentMaxW = currentZone === 'UNDERGROUND' ? UNDERGROUND_WIDTH : WORLD_WIDTH;

        player.x += player.vx;
        if (player.x < cameraX) {
          player.x = cameraX;
          if (player.vx < 0) player.vx = 0;
        }
        if (player.x + player.w > currentMaxW) {
          player.x = currentMaxW - player.w;
          player.vx = 0;
        }

        for (let i = 0; i < solids.length; i++) {
          const s = solids[i];
          if (aabbOverlap(player, s)) {
            if (player.vx > 0) {
              player.x = s.x - player.w;
              player.vx = 0;
              if (s.canExitRight && keys.right && player.onGround) {
                startPipeWarp(s, 'RIGHT');
                return;
              }
            } else if (player.vx < 0) {
              player.x = s.x + s.w;
              player.vx = 0;
            }
          }
        }

        const prevBottom = player.y + player.h;
        player.y += player.vy;
        player.onGround = false;
        let standingPipe = null;

        for (let i = 0; i < solids.length; i++) {
          const s = solids[i];
          if (aabbOverlap(player, s)) {
            if (player.vy > 0) {
              player.y = s.y - player.h;
              player.vy = 0;
              player.onGround = true;
              if (s.type === 'pipe' && s.canEnterDown) standingPipe = s;
            } else if (player.vy < 0) {
              player.y = s.y + s.h;
              player.vy = 1.2;
              player.jumpHoldTimer = 0;
              triggerBlockHit(s);
            }
          }
        }

        // 이동 리프트 플랫폼 착지 판정
        for (let i = 0; i < platforms.length; i++) {
          const pf = platforms[i];
          if (
            player.vy >= 0 &&
            player.x + player.w > pf.x + 2 &&
            player.x < pf.x + pf.w - 2 &&
            prevBottom <= pf.y + 10 &&
            player.y + player.h >= pf.y
          ) {
            player.y = pf.y - player.h;
            player.vy = 0;
            player.onGround = true;
            player.x += pf.vx;
          }
        }

        // 스프링 점프대(Trampoline) 충돌 시 초고도 슈퍼 점프!
        for (let i = 0; i < springs.length; i++) {
          const sp = springs[i];
          if (aabbOverlap(player, sp) && player.vy >= 0) {
            player.y = sp.y - player.h;
            player.vy = keys.jump ? -15.2 : -13.2;
            player.onGround = false;
            player.jumpHoldTimer = PHYSICS.jumpHoldFramesMax;
            sp.compressTimer = 14;
            sound.spring();
            spawnDust(sp.x + 12, sp.y + 20, 6, '#fde047');
            addPopup(sp.x, sp.y - 10, 'SPRING BOOST!', '#4ade80');
          }
        }

        // 배관 위에서 ▼ 키로 진입
        if (standingPipe && keys.down) {
          const playerCenter = player.x + player.w / 2;
          if (playerCenter >= standingPipe.x + 8 && playerCenter <= standingPipe.x + standingPipe.w - 8) {
            startPipeWarp(standingPipe, 'DOWN');
            return;
          }
        }

        if (Math.abs(player.vx) > 0.25 && player.onGround) {
          player.animTimer += Math.abs(player.vx) * 0.22;
          if (player.animTimer >= 1) {
            player.animTimer = 0;
            player.animFrame = (player.animFrame + 1) % 2;
          }
        } else {
          player.animFrame = 0;
          player.animTimer = 0;
        }

        if (player.y > VIEW_H + 36) {
          triggerPlayerDeath();
          return;
        }

        // ==========================================
        // C. 카메라 추적, 불꽃 탄환, 코인, 체크포인트
        // ==========================================
        if (currentZone === 'UNDERGROUND') {
          cameraX = 0;
        } else {
          const targetCamX = player.x - VIEW_W * 0.36;
          if (targetCamX > cameraX) {
            cameraX += (targetCamX - cameraX) * 0.14;
          } else if (player.x < cameraX + VIEW_W * 0.18) {
            cameraX += (player.x - VIEW_W * 0.18 - cameraX) * 0.14;
          }
          if (cameraX < 0) cameraX = 0;
          if (cameraX > WORLD_WIDTH - VIEW_W) cameraX = WORLD_WIDTH - VIEW_W;
        }

        updateFireballs();

        for (let i = 0; i < fieldCoins.length; i++) {
          const fc = fieldCoins[i];
          if (!fc.collected && aabbOverlap(player, fc)) {
            fc.collected = true;
            gainCoin();
            addPopup(fc.x, fc.y - 8, '+200', '#facc15');
            spawnDust(fc.x + 9, fc.y + 11, 4, '#fef08a');
          }
        }

        if (currentZone === 'OVERWORLD' && checkpoint && !checkpointReached) {
          if (aabbOverlap(player, checkpoint)) {
            checkpointReached = true;
            sound.checkpoint();
            score += 500;
            addPopup(checkpoint.x, checkpoint.y - 12, 'CHECKPOINT! +500', '#38bdf8');
          }
        }

        // ==========================================
        // D. 스테이지별 특수 장애물(가시, 회전 화염바, 쿵쿵이, 용암 화염구) 업데이트
        // ==========================================
        for (let i = 0; i < hazards.length; i++) {
          const hz = hazards[i];

          if (hz.type === 'spikes') {
            if (aabbOverlap(player, hz)) {
              triggerPlayerDamage();
            }
          } else if (hz.type === 'firebar') {
            hz.angle += hz.speed;
            for (let b = 1; b <= hz.balls; b++) {
              const bx = hz.cx + Math.cos(hz.angle) * (b * 18) - 6;
              const by = hz.cy + Math.sin(hz.angle) * (b * 18) - 6;
              const ballBox = { x: bx, y: by, w: 12, h: 12 };
              if (aabbOverlap(player, ballBox)) {
                triggerPlayerDamage();
                break;
              }
            }
          } else if (hz.type === 'thwomp') {
            // 플레이어가 근처에 오면 쾅! 내려찍는 쿵쿵이(Thwomp)
            if (hz.state === 'idle') {
              if (Math.abs((player.x + player.w / 2) - (hz.x + hz.w / 2)) < 96) {
                hz.state = 'falling';
                hz.vy = 1.5;
              }
            } else if (hz.state === 'falling') {
              hz.vy += 0.85;
              hz.y += hz.vy;
              if (hz.y + hz.h >= GROUND_Y) {
                hz.y = GROUND_Y - hz.h;
                hz.vy = 0;
                hz.state = 'wait';
                hz.timer = 42;
                sound.thwompSlam();
                spawnDust(hz.x + hz.w / 2, GROUND_Y, 8, '#cbd5e1');
              }
            } else if (hz.state === 'wait') {
              hz.timer--;
              if (hz.timer <= 0) hz.state = 'rising';
            } else if (hz.state === 'rising') {
              hz.y -= 1.6;
              if (hz.y <= hz.homeY) {
                hz.y = hz.homeY;
                hz.state = 'idle';
              }
            }

            if (aabbOverlap(player, hz)) {
              triggerPlayerDamage();
            }
          } else if (hz.type === 'podoboo') {
            // 용암에서 주기적으로 솟구치는 화염구
            hz.timer++;
            if (hz.timer === 1) {
              hz.y = VIEW_H;
              hz.vy = -11.4;
            }
            if (hz.y <= VIEW_H + 12) {
              hz.y += hz.vy;
              hz.vy += 0.36;
              if (aabbOverlap(player, hz)) {
                triggerPlayerDamage();
              }
            }
            if (hz.timer >= 125) {
              hz.timer = 0;
            }
          }
        }

        // ==========================================
        // E. 아이템 물리 및 획득 처리 (아이템 획득 시 짧은 무적 보호 추가)
        // ==========================================
        for (let i = items.length - 1; i >= 0; i--) {
          const it = items[i];

          if (it.type === 'popCoin') {
            it.y += it.vy;
            it.vy += 0.48;
            it.life--;
            if (it.life <= 0) items.splice(i, 1);
            continue;
          }

          if (it.state === 'rising') {
            it.y -= 1.25;
            if (it.y <= it.targetRiseY) {
              it.y = it.targetRiseY;
              it.state = 'active';
            }
          } else if (it.type === 'mushroom' || it.type === '1up' || it.type === 'star') {
            it.x += it.vx;
            for (let j = 0; j < solids.length; j++) {
              const s = solids[j];
              if (aabbOverlap(it, s)) {
                if (it.vx > 0) {
                  it.x = s.x - it.w;
                  it.vx = -Math.abs(it.vx);
                } else {
                  it.x = s.x + s.w;
                  it.vx = Math.abs(it.vx);
                }
              }
            }

            it.vy += PHYSICS.gravity * (it.type === 'star' ? 0.65 : 0.85);
            if (it.vy > 9) it.vy = 9;
            it.y += it.vy;
            for (let j = 0; j < solids.length; j++) {
              const s = solids[j];
              if (aabbOverlap(it, s)) {
                if (it.vy > 0) {
                  it.y = s.y - it.h;
                  it.vy = it.type === 'star' ? -7.2 : 0;
                } else if (it.vy < 0) {
                  it.y = s.y + s.h;
                  it.vy = 1;
                }
              }
            }
          }

          if (aabbOverlap(player, it)) {
            if (it.type === '1up') {
              lives += 1;
              score += 1000;
              sound.oneUp();
              addPopup(player.x, player.y - 16, '1-UP! LIFE +1', '#4ade80');
            } else if (it.type === 'mushroom') {
              sound.powerUp();
              score += 1000;
              if (player.powerLevel === 0) {
                setPlayerPower(1);
                player.transformTimer = 18;
                player.invincibleTimer = Math.max(player.invincibleTimer, 45);
                addPopup(player.x, player.y - 16, 'SUPER UP! +1000', '#4ade80');
              } else {
                addPopup(player.x, player.y - 16, '+1000', '#facc15');
              }
            } else if (it.type === 'flower') {
              sound.powerUp();
              score += 1000;
              setPlayerPower(2);
              player.transformTimer = 20;
              player.invincibleTimer = Math.max(player.invincibleTimer, 45);
              addPopup(player.x, player.y - 18, 'FIRE MARIO! (Z/Shift)', '#fb923c');
              spawnFireBurst(player.x + player.w / 2, player.y + player.h / 2);
            } else if (it.type === 'star') {
              sound.powerUp();
              score += 1000;
              player.starTimer = 540;
              addPopup(player.x, player.y - 18, 'STAR INVINCIBLE!', '#fef08a');
            }

            spawnDust(player.x + player.w / 2, player.y + player.h / 2, 10);
            items.splice(i, 1);
            continue;
          }

          if (it.y > VIEW_H + 60) {
            items.splice(i, 1);
          }
        }

        // ==========================================
        // F. 뻐끔플라워(Piranha Plant) 업데이트
        // ==========================================
        for (let i = 0; i < piranhas.length; i++) {
          const pir = piranhas[i];
          if (!pir.alive) continue;

          const nearPlayer = Math.abs((player.x + player.w / 2) - (pir.x + pir.w / 2)) < 56;
          if (!(nearPlayer && pir.y >= pir.baseY - 2)) {
            pir.timer++;
          }

          const cycle = pir.timer % 180;
          if (cycle < 45) pir.y = pir.baseY - (cycle / 45) * pir.h;
          else if (cycle < 95) pir.y = pir.baseY - pir.h;
          else if (cycle < 140) pir.y = pir.baseY - (1 - (cycle - 95) / 45) * pir.h;
          else pir.y = pir.baseY;

          if (pir.y < pir.baseY - 8) {
            const hitbox = {
              x: pir.x + 4,
              y: pir.y + 4,
              w: pir.w - 8,
              h: pir.baseY - pir.y - 4
            };
            if (aabbOverlap(player, hitbox)) {
              if (player.starTimer > 0) {
                pir.alive = false;
                sound.kick();
                score += 300;
                addPopup(pir.x, pir.y, 'STAR +300', '#fef08a');
              } else {
                triggerPlayerDamage();
              }
            }
          }
        }

        // ==========================================
        // G. 몬스터(굼바, 엉금엉금, 날개 엉금엉금, 딱정벌레, 가시돌이) AI 및 충돌 판정
        // ==========================================
        for (let i = enemies.length - 1; i >= 0; i--) {
          const en = enemies[i];

          if (en.state === 'squished') {
            en.squishTimer--;
            if (en.squishTimer <= 0) enemies.splice(i, 1);
            continue;
          }

          if (currentZone === 'OVERWORLD' && en.x > cameraX + VIEW_W + 220) {
            continue;
          }

          en.animTimer++;
          en.x += en.vx;

          // ★ 스폰킬 방지용 순찰 구역 경계 체크 (걷는 몬스터가 시작 스폰 구역으로 넘어오지 못하게 제한!)
          if (en.state === 'walk') {
            if (en.minX !== undefined && en.x < en.minX) {
              en.x = en.minX;
              en.vx = Math.abs(en.vx);
            }
            if (en.maxX !== undefined && en.x + en.w > en.maxX) {
              en.x = en.maxX - en.w;
              en.vx = -Math.abs(en.vx);
            }
          }

          // 벽이나 토관에 부딪히면 방향 반전
          for (let j = 0; j < solids.length; j++) {
            const s = solids[j];
            if (aabbOverlap(en, s)) {
              if (en.vx > 0) {
                en.x = s.x - en.w;
                en.vx = -Math.abs(en.vx);
                if (en.state === 'shell_moving') sound.bump();
              } else if (en.vx < 0) {
                en.x = s.x + s.w;
                en.vx = Math.abs(en.vx);
                if (en.state === 'shell_moving') sound.bump();
              }
            }
          }

          // 고속 회전 등껍질(shell_moving) 연쇄 처치
          for (let k = enemies.length - 1; k >= 0; k--) {
            if (i === k) continue;
            const other = enemies[k];
            if (!other || other.state === 'squished') continue;

            if (aabbOverlap(en, other)) {
              if (en.state === 'shell_moving') {
                other.state = 'squished';
                other.squishTimer = 20;
                score += 400;
                sound.kick();
                spawnFireBurst(other.x + other.w / 2, other.y + other.h / 2);
                addPopup(other.x, other.y - 12, 'SHELL COMBO +400', '#38bdf8');
              } else if (en.state === 'walk' && other.state === 'walk') {
                en.vx = -en.vx;
                en.x += en.vx * 2;
              }
            }
          }

          // 수직 중력 및 바닥 착지
          en.vy += PHYSICS.gravity * (en.type === 'paratroopa' ? 0.55 : 0.85);
          if (en.vy > 9) en.vy = 9;
          en.y += en.vy;
          for (let j = 0; j < solids.length; j++) {
            const s = solids[j];
            if (aabbOverlap(en, s)) {
              if (en.vy > 0) {
                en.y = s.y - en.h;
                en.onGround = true;
                // 날개 엉금엉금(Paratroopa)은 바닥에 닿을 때마다 통통 점프!
                if (en.type === 'paratroopa' && en.state === 'walk') {
                  en.vy = -7.2;
                } else {
                  en.vy = 0;
                }
              }
            }
          }

          if (en.y > VIEW_H + 60) {
            enemies.splice(i, 1);
            continue;
          }

          // 플레이어와 몬스터 충돌 판정
          if (aabbOverlap(player, en)) {
            if (player.starTimer > 0) {
              en.state = 'squished';
              en.squishTimer = 20;
              score += 200;
              sound.kick();
              spawnFireBurst(en.x + en.w / 2, en.y + en.h / 2);
              addPopup(en.x, en.y - 10, 'STAR +200', '#fef08a');
              continue;
            }

            if (en.state === 'shell_idle') {
              sound.kick();
              score += 100;
              const kickDir = (player.x + player.w / 2) < (en.x + en.w / 2) ? 1 : -1;
              en.state = 'shell_moving';
              en.vx = kickDir * 7.6;
              en.x += kickDir * 8;
              if (player.vy > 0) player.vy = PHYSICS.stompBounceVy;
              addPopup(en.x, en.y - 10, 'KICK +100', '#4ade80');
              continue;
            }

            const playerBottom = player.y + player.h;
            const enemyCenterY = en.y + en.h * 0.58;
            const isFallingOnEnemy = player.vy > 0 && (playerBottom - player.vy <= enemyCenterY + 8);

            // ★ 가시돌이(Spiny)는 등 위에 뾰족한 가시가 있어 위에서 밟으면 플레이어가 데미지를 입음!
            if (en.type === 'spiny') {
              triggerPlayerDamage();
              continue;
            }

            if (isFallingOnEnemy) {
              player.y = en.y - player.h;
              player.vy = PHYSICS.stompBounceVy;
              player.onGround = false;
              sound.stomp();

              if (en.type === 'paratroopa') {
                // 날개 엉금엉금을 밟으면 날개가 떨어지고 일반 엉금엉금이 됨!
                en.type = 'koopa';
                en.vy = 0;
                score += 200;
                addPopup(en.x, en.y - 10, 'WINGS OFF +200', '#38bdf8');
              } else if (en.type === 'koopa' || en.type === 'red_koopa' || en.type === 'beetle') {
                if (en.state === 'walk') {
                  en.state = 'shell_idle';
                  en.vx = 0;
                  en.h = 26;
                  en.y += 10;
                  score += 100;
                  addPopup(en.x, en.y - 10, 'SHELL +100', '#4ade80');
                } else if (en.state === 'shell_moving') {
                  en.state = 'shell_idle';
                  en.vx = 0;
                  score += 100;
                  addPopup(en.x, en.y - 10, 'STOP +100', '#fde047');
                }
              } else {
                en.state = 'squished';
                en.squishTimer = 32;
                score += 100;
                addPopup(en.x, en.y - 10, '+100', '#fde047');
              }
              spawnDust(en.x + en.w / 2, en.y + en.h, 6);
            } else {
              triggerPlayerDamage();
            }
          }
        }

        // ==========================================
        // H. 도착 지점 깃대(Flagpole) 접촉 판정
        // ==========================================
        if (currentZone === 'OVERWORLD' && flagpole) {
          const poleHitbox = {
            x: flagpole.x - 4,
            y: flagpole.y,
            w: flagpole.w + 8,
            h: flagpole.h
          };
          if (aabbOverlap(player, poleHitbox)) {
            gameState = 'CLEARING';
            player.clearPhase = 1;
            player.vx = 0;
            player.vy = 0;
            player.x = flagpole.x - player.w + 4;

            const heightRatio = Math.max(0, Math.min(1, 1 - (player.y - flagpole.y) / flagpole.h));
            const polePoints = Math.round((200 + heightRatio * 1800) / 100) * 100;
            score += polePoints;
            sound.stageClear();
            addPopup(flagpole.x - 10, player.y - 14, `FLAG +${polePoints}`, '#facc15');
          }
        }
      }

      // ==========================================
      // 12. Canvas 2D 렌더링 (네모 박스/이모지 깨짐 완전 제거!)
      // ==========================================
      function drawBackground() {
        if (currentZone === 'UNDERGROUND') {
          ctx.fillStyle = '#070b19';
          ctx.fillRect(0, 0, VIEW_W, VIEW_H);

          ctx.strokeStyle = 'rgba(30, 58, 138, 0.28)';
          ctx.lineWidth = 1;
          for (let x = 0; x < VIEW_W; x += 64) {
            ctx.beginPath();
            ctx.moveTo(x, 0);
            ctx.lineTo(x, VIEW_H);
            ctx.stroke();
          }
          for (let y = 0; y < VIEW_H; y += 64) {
            ctx.beginPath();
            ctx.moveTo(0, y);
            ctx.lineTo(VIEW_W, y);
            ctx.stroke();
          }

          ctx.save();
          ctx.font = '900 15px "Courier New", monospace';
          ctx.textAlign = 'center';
          ctx.fillStyle = '#38bdf8';
          ctx.fillText('UNDERGROUND SECRET BONUS ROOM', VIEW_W / 2, 102);
          ctx.font = 'bold 12px "Courier New", monospace';
          ctx.fillStyle = '#93c5fd';
          ctx.fillText('아이템과 코인을 획득한 뒤 오른쪽 초록 배관(EXIT)으로 이동하세요!', VIEW_W / 2, 122);
          ctx.restore();
          return;
        }

        const theme = STAGE_THEMES[currentStage] || STAGE_THEMES[1];
        const skyGrad = ctx.createLinearGradient(0, 0, 0, VIEW_H);
        skyGrad.addColorStop(0, theme.skyTop);
        skyGrad.addColorStop(0.72, theme.skyMid);
        skyGrad.addColorStop(1, theme.skyBot);
        ctx.fillStyle = skyGrad;
        ctx.fillRect(0, 0, VIEW_W, VIEW_H);

        // 쿠파 성채(Stage 5) 또는 사막(Stage 4) 바닥 용암/유사 연출
        if (theme.pitType === 'lava') {
          ctx.fillStyle = '#dc2626';
          ctx.fillRect(0, VIEW_H - 28, VIEW_W, 28);
          ctx.fillStyle = '#facc15';
          for (let lx = 0; lx < VIEW_W; lx += 32) {
            const waveY = Math.sin(frameCount * 0.12 + lx) * 3;
            ctx.fillRect(lx, VIEW_H - 28 + waveY, 20, 6);
          }
        } else if (theme.pitType === 'quicksand') {
          ctx.fillStyle = '#b45309';
          ctx.fillRect(0, VIEW_H - 22, VIEW_W, 22);
        }

        for (let i = 0; i < decors.length; i++) {
          const d = decors[i];
          const parallax = d.type === 'cloud' ? 0.45 : 0.72;
          const sx = Math.floor(d.x - cameraX * parallax);

          if (sx + d.w < -80 || sx > VIEW_W + 80) continue;

          if (d.type === 'cloud') {
            ctx.fillStyle = '#ffffff';
            ctx.beginPath();
            ctx.arc(sx + 22, d.y + 18, 16, 0, Math.PI * 2);
            ctx.arc(sx + d.w * 0.5, d.y + 12, 21, 0, Math.PI * 2);
            ctx.arc(sx + d.w - 22, d.y + 18, 15, 0, Math.PI * 2);
            ctx.fill();
            ctx.fillRect(sx + 20, d.y + 16, d.w - 40, 18);
          } else if (d.type === 'hill') {
            ctx.fillStyle = '#15803d';
            ctx.beginPath();
            ctx.moveTo(sx, d.y);
            ctx.quadraticCurveTo(sx + d.w / 2, d.y - d.h * 1.65, sx + d.w, d.y);
            ctx.closePath();
            ctx.fill();
            ctx.strokeStyle = '#052e16';
            ctx.lineWidth = 2;
            ctx.stroke();
          } else if (d.type === 'pyramid') {
            ctx.fillStyle = '#d97706';
            ctx.beginPath();
            ctx.moveTo(sx, d.y);
            ctx.lineTo(sx + d.w / 2, d.y - d.h);
            ctx.lineTo(sx + d.w, d.y);
            ctx.closePath();
            ctx.fill();
            ctx.strokeStyle = '#78350f';
            ctx.lineWidth = 2;
            ctx.stroke();
          } else if (d.type === 'crystal') {
            ctx.fillStyle = 'rgba(56, 189, 248, 0.35)';
            ctx.beginPath();
            ctx.moveTo(sx, d.y);
            ctx.lineTo(sx + d.w / 2, d.y + d.h);
            ctx.lineTo(sx + d.w, d.y);
            ctx.closePath();
            ctx.fill();
          } else if (d.type === 'chain') {
            ctx.strokeStyle = '#64748b';
            ctx.lineWidth = 3;
            for (let cy = d.y; cy < d.y + d.h; cy += 14) {
              ctx.strokeRect(sx + 4, cy, 8, 10);
            }
          }
        }
      }

      function drawPiranhas() {
        for (let i = 0; i < piranhas.length; i++) {
          const pir = piranhas[i];
          if (!pir.alive || pir.y >= pir.baseY - 2) continue;

          const px = Math.floor(pir.x - cameraX);
          const py = Math.floor(pir.y);
          if (px + pir.w < -20 || px > VIEW_W + 20) continue;

          ctx.save();
          ctx.beginPath();
          ctx.rect(px - 8, py - 10, pir.w + 16, pir.baseY - py + 10);
          ctx.clip();

          ctx.fillStyle = '#16a34a';
          ctx.fillRect(px + 12, py + 20, 8, pir.h - 20);
          ctx.fillStyle = '#4ade80';
          ctx.fillRect(px + 4, py + 26, 8, 5);
          ctx.fillRect(px + 20, py + 26, 8, 5);

          ctx.fillStyle = '#dc2626';
          ctx.beginPath();
          ctx.arc(px + 16, py + 13, 13, 0, Math.PI * 2);
          ctx.fill();
          ctx.strokeStyle = '#450a0a';
          ctx.lineWidth = 1.5;
          ctx.stroke();

          ctx.fillStyle = '#ffffff';
          ctx.fillRect(px + 8, py + 10, 4, 4);
          ctx.fillRect(px + 20, py + 12, 4, 4);
          ctx.fillRect(px + 14, py + 16, 4, 4);

          const mouthOpen = Math.floor(frameCount / 8) % 2 === 0;
          ctx.fillStyle = '#090d16';
          ctx.beginPath();
          ctx.moveTo(px + 16, py + 12);
          ctx.lineTo(px + (mouthOpen ? 6 : 11), py);
          ctx.lineTo(px + (mouthOpen ? 26 : 21), py);
          ctx.closePath();
          ctx.fill();

          ctx.strokeStyle = '#ffffff';
          ctx.lineWidth = 2;
          ctx.beginPath();
          ctx.moveTo(px + (mouthOpen ? 6 : 11), py + 1);
          ctx.lineTo(px + 16, py + 12);
          ctx.lineTo(px + (mouthOpen ? 26 : 21), py + 1);
          ctx.stroke();

          ctx.restore();
        }
      }

      function drawSolidsAndPlatforms() {
        const theme = STAGE_THEMES[currentStage] || STAGE_THEMES[1];
        const qPulse = Math.floor(frameCount / 12) % 3;
        const qColors = ['#facc15', '#f59e0b', '#fbbf24'];

        for (let i = 0; i < solids.length; i++) {
          const s = solids[i];
          const sx = Math.floor(s.x - cameraX);
          const sy = Math.floor(s.y);

          if (sx + s.w < -10 || sx > VIEW_W + 10) continue;

          if (s.type === 'ground' || s.type === 'ug_ground') {
            const isUg = s.type === 'ug_ground';
            ctx.fillStyle = isUg ? '#1e3a8a' : theme.groundMain;
            ctx.fillRect(sx, sy, s.w, s.h);

            ctx.fillStyle = isUg ? '#60a5fa' : theme.groundTop;
            ctx.fillRect(sx, sy, s.w, 5);

            ctx.strokeStyle = isUg ? '#0f172a' : theme.groundBorder;
            ctx.lineWidth = 2;
            ctx.strokeRect(sx + 1, sy + 1, s.w - 2, TILE - 2);
            ctx.strokeRect(sx + 1, sy + TILE + 1, s.w - 2, s.h - TILE - 2);
          } else if (s.type === 'brick' || s.type === 'ug_brick') {
            const isUg = s.type === 'ug_brick';
            ctx.fillStyle = isUg ? '#1d4ed8' : theme.brickMain;
            ctx.fillRect(sx, sy, s.w, s.h);

            ctx.fillStyle = '#111827';
            ctx.fillRect(sx, sy + 7, s.w, 2);
            ctx.fillRect(sx, sy + 15, s.w, 2);
            ctx.fillRect(sx, sy + 23, s.w, 2);
            ctx.fillRect(sx + 15, sy, 2, 7);
            ctx.fillRect(sx + 7, sy + 9, 2, 6);
            ctx.fillRect(sx + 23, sy + 9, 2, 6);
            ctx.fillRect(sx + 15, sy + 17, 2, 6);

            ctx.fillStyle = isUg ? '#60a5fa' : theme.brickHighlight;
            ctx.fillRect(sx + 1, sy + 1, s.w - 2, 2);
            ctx.strokeStyle = '#111827';
            ctx.lineWidth = 1.5;
            ctx.strokeRect(sx + 0.5, sy + 0.5, s.w - 1, s.h - 1);
          } else if (s.type === 'question') {
            ctx.fillStyle = qColors[qPulse];
            ctx.fillRect(sx, sy, s.w, s.h);

            ctx.strokeStyle = '#78350f';
            ctx.lineWidth = 2;
            ctx.strokeRect(sx + 1, sy + 1, s.w - 2, s.h - 2);

            ctx.fillStyle = '#78350f';
            ctx.fillRect(sx + 3, sy + 3, 3, 3);
            ctx.fillRect(sx + s.w - 6, sy + 3, 3, 3);
            ctx.fillRect(sx + 3, sy + s.h - 6, 3, 3);
            ctx.fillRect(sx + s.w - 6, sy + s.h - 6, 3, 3);

            ctx.font = 'bold 20px "Courier New", monospace';
            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';
            ctx.fillStyle = '#78350f';
            ctx.fillText('?', sx + s.w / 2 + 1.5, sy + s.h / 2 + 2);
            ctx.fillStyle = '#ffffff';
            ctx.fillText('?', sx + s.w / 2, sy + s.h / 2 + 0.5);
          } else if (s.type === 'empty') {
            ctx.fillStyle = '#78350f';
            ctx.fillRect(sx, sy, s.w, s.h);
            ctx.strokeStyle = '#292524';
            ctx.lineWidth = 2;
            ctx.strokeRect(sx + 1, sy + 1, s.w - 2, s.h - 2);
          } else if (s.type === 'hard') {
            ctx.fillStyle = theme.hardMain;
            ctx.fillRect(sx, sy, s.w, s.h);

            ctx.fillStyle = theme.hardHighlight;
            ctx.fillRect(sx + 2, sy + 2, s.w - 4, 3);
            ctx.fillRect(sx + 2, sy + 2, 3, s.h - 4);

            ctx.strokeStyle = '#111827';
            ctx.lineWidth = 2;
            ctx.strokeRect(sx + 1, sy + 1, s.w - 2, s.h - 2);
          } else if (s.type === 'pipe') {
            const lipH = 26;
            const bodyX = sx + 4;
            const bodyW = s.w - 8;

            ctx.fillStyle = '#16a34a';
            ctx.fillRect(bodyX, sy + lipH, bodyW, s.h - lipH);
            ctx.fillStyle = '#86efac';
            ctx.fillRect(bodyX + 6, sy + lipH, 8, s.h - lipH);
            ctx.fillStyle = '#14532d';
            ctx.fillRect(bodyX + bodyW - 10, sy + lipH, 6, s.h - lipH);
            ctx.strokeStyle = '#052e16';
            ctx.lineWidth = 2;
            ctx.strokeRect(bodyX, sy + lipH, bodyW, s.h - lipH);

            const lipY = s.isCeilingPipe ? sy + s.h - lipH : sy;
            ctx.fillStyle = '#16a34a';
            ctx.fillRect(sx, lipY, s.w, lipH);
            ctx.fillStyle = '#86efac';
            ctx.fillRect(sx + 8, lipY + 2, 10, lipH - 4);
            ctx.fillStyle = '#14532d';
            ctx.fillRect(sx + s.w - 12, lipY + 2, 8, lipH - 4);
            ctx.strokeRect(sx + 1, lipY + 1, s.w - 2, lipH - 2);

            if (s.label) {
              const floatY = Math.sin(frameCount * 0.1) * 3;
              ctx.save();
              ctx.font = '900 11px "Courier New", monospace';
              ctx.textAlign = 'center';
              ctx.fillStyle = 'rgba(15, 23, 42, 0.8)';
              ctx.fillRect(sx - 18, sy - 26 + floatY, s.w + 36, 18);
              ctx.strokeStyle = '#4ade80';
              ctx.lineWidth = 1;
              ctx.strokeRect(sx - 18, sy - 26 + floatY, s.w + 36, 18);
              ctx.fillStyle = '#fde047';
              ctx.fillText(s.label, sx + s.w / 2, sy - 13 + floatY);
              ctx.restore();
            }
          }
        }

        // 이동 리프트 플랫폼 렌더링
        for (let i = 0; i < platforms.length; i++) {
          const pf = platforms[i];
          const px = Math.floor(pf.x - cameraX);
          const py = Math.floor(pf.y);
          if (px + pf.w < -20 || px > VIEW_W + 20) continue;

          ctx.fillStyle = currentStage === 3 ? '#ffffff' : '#f59e0b';
          ctx.fillRect(px, py, pf.w, pf.h);
          ctx.strokeStyle = '#111827';
          ctx.lineWidth = 2;
          ctx.strokeRect(px, py, pf.w, pf.h);
          ctx.fillStyle = currentStage === 3 ? '#bae6fd' : '#fde68a';
          ctx.fillRect(px + 4, py + 3, pf.w - 8, 4);
        }

        // 스프링 점프대 렌더링
        for (let i = 0; i < springs.length; i++) {
          const sp = springs[i];
          const sx = Math.floor(sp.x - cameraX);
          const sy = Math.floor(sp.y);
          const comp = sp.compressTimer > 0 ? 8 : 0;

          // 바닥 받침
          ctx.fillStyle = '#475569';
          ctx.fillRect(sx, sy + 20, sp.w, 6);
          // 코일 지그재그
          ctx.strokeStyle = '#cbd5e1';
          ctx.lineWidth = 3;
          ctx.beginPath();
          ctx.moveTo(sx + 4, sy + 20);
          ctx.lineTo(sx + sp.w - 4, sy + 12 + comp / 2);
          ctx.lineTo(sx + 4, sy + 6 + comp);
          ctx.stroke();
          // 상단 빨간 쿠션
          ctx.fillStyle = '#ef4444';
          ctx.fillRect(sx - 2, sy + comp, sp.w + 4, 7);
          ctx.strokeStyle = '#7f1d1d';
          ctx.lineWidth = 1.5;
          ctx.strokeRect(sx - 2, sy + comp, sp.w + 4, 7);
        }

        // 특수 위험 장애물(가시, 회전 화염바, 쿵쿵이, 용암 화염구) 렌더링
        for (let i = 0; i < hazards.length; i++) {
          const hz = hazards[i];

          if (hz.type === 'spikes') {
            const sx = Math.floor(hz.x - cameraX);
            const sy = Math.floor(hz.y);
            ctx.fillStyle = '#e2e8f0';
            ctx.strokeStyle = '#334155';
            ctx.lineWidth = 1.5;
            for (let x = 0; x < hz.w; x += 16) {
              ctx.beginPath();
              ctx.moveTo(sx + x, sy + hz.h);
              ctx.lineTo(sx + x + 8, sy);
              ctx.lineTo(sx + x + 16, sy + hz.h);
              ctx.closePath();
              ctx.fill();
              ctx.stroke();
            }
          } else if (hz.type === 'firebar') {
            const cx = Math.floor(hz.cx - cameraX);
            const cy = Math.floor(hz.cy);
            if (cx < -120 || cx > VIEW_W + 120) continue;

            for (let b = 0; b <= hz.balls; b++) {
              const bx = cx + Math.cos(hz.angle) * (b * 18);
              const by = cy + Math.sin(hz.angle) * (b * 18);
              ctx.fillStyle = b % 2 === 0 ? '#ef4444' : '#f97316';
              ctx.beginPath();
              ctx.arc(bx, by, 7, 0, Math.PI * 2);
              ctx.fill();
              ctx.fillStyle = '#fef08a';
              ctx.beginPath();
              ctx.arc(bx, by, 3, 0, Math.PI * 2);
              ctx.fill();
            }
          } else if (hz.type === 'thwomp') {
            const tx = Math.floor(hz.x - cameraX);
            const ty = Math.floor(hz.y);
            if (tx + hz.w < -40 || tx > VIEW_W + 40) continue;

            // 쿵쿵이 바위 몸체
            ctx.fillStyle = '#64748b';
            ctx.fillRect(tx, ty, hz.w, hz.h);
            ctx.strokeStyle = '#0f172a';
            ctx.lineWidth = 3;
            ctx.strokeRect(tx, ty, hz.w, hz.h);

            // 테두리 가시 돌기
            ctx.fillStyle = '#cbd5e1';
            for (let sy = 6; sy < hz.h - 6; sy += 16) {
              ctx.fillRect(tx - 4, ty + sy, 4, 8);
              ctx.fillRect(tx + hz.w, ty + sy, 4, 8);
            }

            // 화난 눈과 이빨
            ctx.fillStyle = '#111827';
            ctx.fillRect(tx + 8, ty + 14, 12, 10);
            ctx.fillRect(tx + 28, ty + 14, 12, 10);
            ctx.fillStyle = '#ef4444';
            ctx.fillRect(tx + 12, ty + 18, 5, 5);
            ctx.fillRect(tx + 31, ty + 18, 5, 5);

            ctx.fillStyle = '#ffffff';
            ctx.fillRect(tx + 10, ty + 36, 28, 10);
            ctx.strokeStyle = '#111827';
            ctx.lineWidth = 1.5;
            ctx.strokeRect(tx + 10, ty + 36, 28, 10);
          } else if (hz.type === 'podoboo') {
            const px = Math.floor(hz.x - cameraX);
            const py = Math.floor(hz.y);
            if (py > VIEW_H + 10) continue;

            ctx.fillStyle = '#ef4444';
            ctx.beginPath();
            ctx.arc(px + 10, py + 12, 10, 0, Math.PI * 2);
            ctx.fill();
            ctx.fillStyle = '#facc15';
            ctx.beginPath();
            ctx.arc(px + 10, py + 12, 5, 0, Math.PI * 2);
            ctx.fill();
          }
        }
      }

      function drawCheckpointAndFlagpole() {
        if (currentZone !== 'OVERWORLD') return;

        if (checkpoint) {
          const cpx = Math.floor(checkpoint.x - cameraX);
          const cpy = Math.floor(checkpoint.y);
          if (cpx > -40 && cpx < VIEW_W + 40) {
            ctx.fillStyle = '#e2e8f0';
            ctx.fillRect(cpx + 6, cpy, 4, checkpoint.h);
            ctx.fillStyle = checkpointReached ? '#22c55e' : '#f97316';
            ctx.beginPath();
            ctx.moveTo(cpx + 10, cpy + 4);
            ctx.lineTo(cpx + 38, cpy + 14);
            ctx.lineTo(cpx + 10, cpy + 24);
            ctx.closePath();
            ctx.fill();
            ctx.font = 'bold 10px "Courier New", monospace';
            ctx.fillStyle = '#ffffff';
            ctx.fillText(checkpointReached ? 'SAVED' : 'CHECK', cpx - 2, cpy - 6);
          }
        }

        if (!flagpole) return;

        const cx = Math.floor(flagpole.castleX - cameraX);
        const cy = Math.floor(flagpole.castleY);

        if (cx + 160 > -20 && cx < VIEW_W + 20) {
          ctx.fillStyle = currentStage === 5 ? '#334155' : '#b45309';
          ctx.fillRect(cx + 16, cy + 48, 112, 80);
          ctx.fillRect(cx + 34, cy + 16, 76, 36);

          for (let i = 0; i < 5; i++) ctx.fillRect(cx + 16 + i * 24, cy + 36, 16, 12);
          for (let i = 0; i < 3; i++) ctx.fillRect(cx + 34 + i * 28, cy + 4, 18, 12);

          ctx.fillStyle = '#111827';
          ctx.beginPath();
          ctx.arc(cx + 72, cy + 98, 18, Math.PI, 0);
          ctx.lineTo(cx + 90, cy + 128);
          ctx.lineTo(cx + 54, cy + 128);
          ctx.closePath();
          ctx.fill();

          ctx.fillRect(cx + 48, cy + 24, 10, 18);
          ctx.fillRect(cx + 86, cy + 24, 10, 18);

          ctx.strokeStyle = '#111827';
          ctx.lineWidth = 2;
          ctx.strokeRect(cx + 16, cy + 48, 112, 80);
        }

        const px = Math.floor(flagpole.x - cameraX);
        const py = Math.floor(flagpole.y);
        if (px < -60 || px > VIEW_W + 60) return;

        ctx.fillStyle = '#d9f99d';
        ctx.fillRect(px, py, flagpole.w, flagpole.h);
        ctx.strokeStyle = '#14532d';
        ctx.lineWidth = 2;
        ctx.strokeRect(px, py, flagpole.w, flagpole.h);

        ctx.fillStyle = '#facc15';
        ctx.beginPath();
        ctx.arc(px + flagpole.w / 2, py, 10, 0, Math.PI * 2);
        ctx.fill();
        ctx.stroke();

        const fy = Math.floor(flagpole.flagY);
        ctx.fillStyle = '#ef4444';
        ctx.beginPath();
        ctx.moveTo(px + flagpole.w, fy);
        ctx.lineTo(px - 44, fy + 16);
        ctx.lineTo(px + flagpole.w, fy + 32);
        ctx.closePath();
        ctx.fill();
        ctx.strokeStyle = '#7f1d1d';
        ctx.stroke();
      }

      function drawFieldCoinsAndItems() {
        const coinScale = Math.abs(Math.cos(frameCount * 0.1));
        for (let i = 0; i < fieldCoins.length; i++) {
          const fc = fieldCoins[i];
          if (fc.collected) continue;
          const cx = Math.floor(fc.x - cameraX);
          const cy = Math.floor(fc.y);
          if (cx < -20 || cx > VIEW_W + 20) continue;

          ctx.fillStyle = '#facc15';
          ctx.beginPath();
          ctx.ellipse(cx + 9, cy + 11, Math.max(3, 8 * coinScale), 11, 0, 0, Math.PI * 2);
          ctx.fill();
          ctx.strokeStyle = '#b45309';
          ctx.lineWidth = 1.5;
          ctx.stroke();
        }

        for (let i = 0; i < items.length; i++) {
          const it = items[i];
          const ix = Math.floor(it.x - cameraX);
          const iy = Math.floor(it.y);

          if (it.type === 'popCoin') {
            ctx.fillStyle = '#facc15';
            ctx.beginPath();
            ctx.ellipse(ix + 8, iy + 11, 7, 11, 0, 0, Math.PI * 2);
            ctx.fill();
            ctx.strokeStyle = '#b45309';
            ctx.lineWidth = 2;
            ctx.stroke();
          } else if (it.type === 'mushroom' || it.type === '1up') {
            const is1Up = it.type === '1up';
            ctx.fillStyle = '#fde68a';
            ctx.fillRect(ix + 6, iy + 15, 16, 12);
            ctx.strokeStyle = '#78350f';
            ctx.lineWidth = 1.5;
            ctx.strokeRect(ix + 6, iy + 15, 16, 12);

            ctx.fillStyle = '#111827';
            ctx.fillRect(ix + 10, iy + 18, 2, 5);
            ctx.fillRect(ix + 16, iy + 18, 2, 5);

            ctx.fillStyle = is1Up ? '#22c55e' : '#ef4444';
            ctx.beginPath();
            ctx.arc(ix + 14, iy + 14, 13, Math.PI, 0);
            ctx.closePath();
            ctx.fill();
            ctx.stroke();

            ctx.fillStyle = '#ffffff';
            ctx.beginPath();
            ctx.arc(ix + 14, iy + 7, 4, 0, Math.PI * 2);
            ctx.arc(ix + 7, iy + 11, 3, 0, Math.PI * 2);
            ctx.arc(ix + 21, iy + 11, 3, 0, Math.PI * 2);
            ctx.fill();
          } else if (it.type === 'flower') {
            const pulse = Math.floor(frameCount / 5) % 2 === 0;
            ctx.fillStyle = '#22c55e';
            ctx.fillRect(ix + 12, iy + 16, 4, 11);
            ctx.fillRect(ix + 6, iy + 21, 6, 4);
            ctx.fillRect(ix + 16, iy + 21, 6, 4);

            ctx.fillStyle = pulse ? '#ef4444' : '#f97316';
            ctx.beginPath();
            ctx.ellipse(ix + 14, iy + 10, 12, 9, 0, 0, Math.PI * 2);
            ctx.fill();
            ctx.strokeStyle = '#7c2d12';
            ctx.lineWidth = 1.5;
            ctx.stroke();

            ctx.fillStyle = pulse ? '#facc15' : '#fde047';
            ctx.beginPath();
            ctx.ellipse(ix + 14, iy + 10, 8, 6, 0, 0, Math.PI * 2);
            ctx.fill();

            ctx.fillStyle = '#ffffff';
            ctx.fillRect(ix + 10, iy + 7, 8, 6);
            ctx.fillStyle = '#111827';
            ctx.fillRect(ix + 11, iy + 8, 2, 4);
            ctx.fillRect(ix + 15, iy + 8, 2, 4);
          } else if (it.type === 'star') {
            const starColor = frameCount % 6 < 3 ? '#facc15' : '#fef08a';
            ctx.save();
            ctx.translate(ix + 14, iy + 14);
            ctx.fillStyle = starColor;
            ctx.strokeStyle = '#b45309';
            ctx.lineWidth = 1.5;
            ctx.beginPath();
            for (let p = 0; p < 5; p++) {
              const outerA = (p * 72 - 90) * (Math.PI / 180);
              const innerA = ((p * 72 + 36) - 90) * (Math.PI / 180);
              const ox = Math.cos(outerA) * 13;
              const oy = Math.sin(outerA) * 13;
              const inx = Math.cos(innerA) * 6;
              const iny = Math.sin(innerA) * 6;
              if (p === 0) ctx.moveTo(ox, oy);
              else ctx.lineTo(ox, oy);
              ctx.lineTo(inx, iny);
            }
            ctx.closePath();
            ctx.fill();
            ctx.stroke();
            ctx.fillStyle = '#111827';
            ctx.fillRect(-3, -2, 2, 5);
            ctx.fillRect(2, -2, 2, 5);
            ctx.restore();
          }
        }
      }

      function drawFireballs() {
        for (let i = 0; i < fireballs.length; i++) {
          const fb = fireballs[i];
          const fx = Math.floor(fb.x - cameraX);
          const fy = Math.floor(fb.y);

          ctx.save();
          ctx.translate(fx + 6, fy + 6);
          ctx.rotate(frameCount * 0.45);
          ctx.fillStyle = '#ef4444';
          ctx.beginPath();
          ctx.arc(0, 0, 6, 0, Math.PI * 2);
          ctx.fill();
          ctx.fillStyle = '#facc15';
          ctx.fillRect(-6, -2, 12, 4);
          ctx.fillRect(-2, -6, 4, 12);
          ctx.fillStyle = '#ffffff';
          ctx.fillRect(-2, -2, 4, 4);
          ctx.restore();
        }
      }

      function drawEnemies() {
        for (let i = 0; i < enemies.length; i++) {
          const en = enemies[i];
          const ex = Math.floor(en.x - cameraX);
          const ey = Math.floor(en.y);

          if (ex + en.w < -20 || ex > VIEW_W + 20) continue;

          if (en.state === 'squished') {
            ctx.fillStyle = '#92400e';
            ctx.fillRect(ex, ey + 16, en.w, 8);
            ctx.fillStyle = '#fde68a';
            ctx.fillRect(ex + 4, ey + 24, en.w - 8, 4);
          } else if (en.type === 'spiny') {
            // ★ 사막/성채 전용 가시돌이(Spiny) 렌더링: 붉은 등껍질 + 날카로운 하얀 가시 3개
            ctx.fillStyle = '#dc2626';
            ctx.beginPath();
            ctx.arc(ex + 14, ey + 18, 12, Math.PI, 0);
            ctx.closePath();
            ctx.fill();
            ctx.strokeStyle = '#450a0a';
            ctx.lineWidth = 1.5;
            ctx.stroke();

            // 등 위의 날카로운 흰 가시
            ctx.fillStyle = '#ffffff';
            const spikesX = [6, 14, 22];
            spikesX.forEach((sx) => {
              ctx.beginPath();
              ctx.moveTo(ex + sx - 4, ey + 10);
              ctx.lineTo(ex + sx, ey + 1);
              ctx.lineTo(ex + sx + 4, ey + 10);
              ctx.closePath();
              ctx.fill();
            });

            // 얼굴 & 다리
            ctx.fillStyle = '#fde68a';
            ctx.fillRect(ex + 4, ey + 18, 20, 6);
            ctx.fillStyle = '#111827';
            ctx.fillRect(ex + (en.vx < 0 ? 6 : 18), ey + 18, 3, 4);
            ctx.fillRect(ex + 5, ey + 24, 6, 4);
            ctx.fillRect(ex + 17, ey + 24, 6, 4);
          } else if (en.type === 'beetle') {
            // ★ 지하 동굴 전용 딱정벌레(Buzzy Beetle): 파이어볼 면역 네이비 금속 등껍질
            ctx.fillStyle = '#1e293b';
            ctx.beginPath();
            ctx.arc(ex + 14, ey + 16, 13, Math.PI, 0);
            ctx.lineTo(ex + 27, ey + 24);
            ctx.lineTo(ex + 1, ey + 24);
            ctx.closePath();
            ctx.fill();
            ctx.strokeStyle = '#60a5fa';
            ctx.lineWidth = 2;
            ctx.stroke();

            ctx.fillStyle = '#ef4444';
            ctx.fillRect(ex + (en.vx < 0 ? 5 : 19), ey + 16, 4, 4);
            ctx.fillStyle = '#94a3b8';
            ctx.fillRect(ex + 6, ey + 24, 5, 4);
            ctx.fillRect(ex + 17, ey + 24, 5, 4);
          } else if (en.type === 'koopa' || en.type === 'red_koopa' || en.type === 'paratroopa') {
            const shellMain = en.type === 'koopa' ? '#16a34a' : '#dc2626';
            const shellLight = en.type === 'koopa' ? '#86efac' : '#fca5a5';

            if (en.state === 'shell_idle' || en.state === 'shell_moving') {
              ctx.fillStyle = shellMain;
              ctx.beginPath();
              ctx.arc(ex + 14, ey + 14, 12, Math.PI, 0);
              ctx.lineTo(ex + 26, ey + 22);
              ctx.lineTo(ex + 2, ey + 22);
              ctx.closePath();
              ctx.fill();
              ctx.strokeStyle = '#111827';
              ctx.lineWidth = 2;
              ctx.stroke();

              ctx.fillStyle = '#fef08a';
              ctx.fillRect(ex + 2, ey + 20, 24, 5);
            } else {
              const flip = en.vx > 0;
              const headX = flip ? ex + 16 : ex + 2;
              const shellX = flip ? ex + 2 : ex + 8;
              const step = Math.floor(en.animTimer / 8) % 2 === 0 ? 0 : 3;

              ctx.fillStyle = '#facc15';
              ctx.fillRect(headX, ey + 2, 10, 12);
              ctx.fillStyle = '#111827';
              ctx.fillRect(flip ? headX + 6 : headX + 2, ey + 4, 3, 4);

              ctx.fillStyle = shellMain;
              ctx.fillRect(shellX, ey + 10, 18, 18);
              ctx.strokeStyle = '#111827';
              ctx.lineWidth = 1.5;
              ctx.strokeRect(shellX, ey + 10, 18, 18);
              ctx.fillStyle = shellLight;
              ctx.fillRect(shellX + 4, ey + 13, 10, 8);

              // 날개 엉금엉금(Paratroopa) 하얀 날개 렌더링
              if (en.type === 'paratroopa') {
                const wingFlap = Math.floor(frameCount / 6) % 2 === 0 ? -4 : 2;
                ctx.fillStyle = '#ffffff';
                ctx.fillRect(shellX + 4, ey + 4 + wingFlap, 10, 8);
                ctx.strokeStyle = '#1e293b';
                ctx.strokeRect(shellX + 4, ey + 4 + wingFlap, 10, 8);
              }

              ctx.fillStyle = '#facc15';
              ctx.fillRect(ex + 5, ey + 28, 6, 8 - step);
              ctx.fillRect(ex + 17, ey + 28, 6, 5 + step);
            }
          } else {
            const sprite = Math.floor(en.animTimer / 10) % 2 === 0 ? SPRITE_GOOMBA_1 : SPRITE_GOOMBA_2;
            const gPalette = (currentStage === 2 || currentZone === 'UNDERGROUND') ? GOOMBA_PALETTE_UNDERGROUND : GOOMBA_PALETTE_OVERWORLD;
            drawPixelMatrix(ctx, sprite, gPalette, ex, ey, en.w, en.h, false);
          }
        }
      }

      // ★ 플레이어 렌더링: 아이템 획득 시 캐릭터 주변에 생기던 큰 네모 박스(strokeRect)를 완벽히 제거!
      function drawPlayer() {
        if (!player || player.clearPhase === 3) return;

        if (player.invincibleTimer > 0 && Math.floor(frameCount / 3) % 2 === 0) {
          return;
        }

        const px = Math.floor(player.x - cameraX);
        const py = Math.floor(player.y);

        let matrix = SPRITE_HERO_IDLE;
        if (!player.onGround && gameState === 'PLAYING') {
          matrix = SPRITE_HERO_JUMP;
        } else if (Math.abs(player.vx) > 0.25) {
          matrix = player.animFrame === 0 ? SPRITE_HERO_RUN1 : SPRITE_HERO_RUN2;
        }

        let drawH = player.h;
        let drawY = py;
        if (player.transformTimer > 0 && player.transformTimer % 4 < 2) {
          drawH = player.isSuper ? 38 : 42;
          drawY = py + (player.h - drawH);
        }

        // 상태에 따른 순수 픽셀 팔레트 변경 (네모 박스 테두리 없이 깔끔하게 캐릭터 자체 색상만 변화!)
        let activePalette = HERO_PALETTE_NORMAL;
        if (player.starTimer > 0) {
          activePalette = STAR_PALETTES[Math.floor(frameCount / 3) % STAR_PALETTES.length];
        } else if (player.powerLevel === 2) {
          activePalette = HERO_PALETTE_FIRE;
        } else if (player.powerLevel === 1) {
          activePalette = HERO_PALETTE_SUPER;
        }

        drawPixelMatrix(ctx, matrix, activePalette, px - 2, drawY, player.w + 4, drawH, player.facing === -1);
      }

      function drawParticlesAndPopups() {
        for (let i = 0; i < particles.length; i++) {
          const pt = particles[i];
          const sx = Math.floor(pt.x - cameraX);
          const sy = Math.floor(pt.y);

          ctx.save();
          ctx.fillStyle = pt.color;
          if (pt.type === 'brick') {
            ctx.translate(sx + pt.size / 2, sy + pt.size / 2);
            ctx.rotate(pt.angle || 0);
            ctx.fillRect(-pt.size / 2, -pt.size / 2, pt.size, pt.size);
            ctx.strokeStyle = '#111827';
            ctx.strokeRect(-pt.size / 2, -pt.size / 2, pt.size, pt.size);
          } else {
            ctx.globalAlpha = Math.max(0.2, pt.life / 24);
            ctx.fillRect(sx, sy, pt.size, pt.size);
          }
          ctx.restore();
        }

        for (let i = 0; i < popups.length; i++) {
          const pp = popups[i];
          const sx = Math.floor(pp.x - cameraX);
          const sy = Math.floor(pp.y);

          ctx.save();
          ctx.font = 'bold 14px "Courier New", monospace';
          ctx.textAlign = 'center';
          ctx.fillStyle = '#000000';
          ctx.fillText(pp.text, sx + 14, sy + 1);
          ctx.fillStyle = pp.color;
          ctx.fillText(pp.text, sx + 13, sy);
          ctx.restore();
        }
      }

      // ★ 상단 HUD: 깨지는 유니코드 이모지(네모 박스)를 완전히 배제하고 순수 Canvas 도형(하트/동전)과 영문/숫자만 사용!
      function drawPixelHeart(ctx, x, y, filled = true) {
        ctx.fillStyle = filled ? '#ef4444' : '#475569';
        ctx.beginPath();
        ctx.arc(x + 4, y + 4, 4, Math.PI, 0);
        ctx.arc(x + 11, y + 4, 4, Math.PI, 0);
        ctx.lineTo(x + 15, y + 6);
        ctx.lineTo(x + 7.5, y + 14);
        ctx.lineTo(x, y + 6);
        ctx.closePath();
        ctx.fill();
      }

      function drawHUD() {
        ctx.save();
        ctx.fillStyle = 'rgba(15, 23, 42, 0.62)';
        ctx.fillRect(12, 8, VIEW_W - 24, 48);
        ctx.strokeStyle = 'rgba(255, 255, 255, 0.22)';
        ctx.lineWidth = 2;
        ctx.strokeRect(12, 8, VIEW_W - 24, 48);

        const drawHudCol = (label, value, x, valueColor = '#ffffff') => {
          ctx.font = '900 12px "Courier New", monospace';
          ctx.textAlign = 'left';
          ctx.fillStyle = '#111827';
          ctx.fillText(label, x + 2, 24);
          ctx.fillStyle = '#fbbf24';
          ctx.fillText(label, x, 22);

          ctx.font = '900 16px "Courier New", monospace';
          ctx.fillStyle = '#111827';
          ctx.fillText(value, x + 2, 45);
          ctx.fillStyle = valueColor;
          ctx.fillText(value, x, 43);
        };

        // 1) LIVES (목숨 3개 픽셀 하트 표시)
        ctx.font = '900 12px "Courier New", monospace';
        ctx.fillStyle = '#111827';
        ctx.fillText('LIVES', 28, 24);
        ctx.fillStyle = '#fbbf24';
        ctx.fillText('LIVES', 26, 22);

        const maxHearts = Math.max(3, Math.min(5, lives));
        for (let h = 0; h < maxHearts; h++) {
          drawPixelHeart(ctx, 26 + h * 19, 31, h < lives);
        }

        // 2) SCORE
        drawHudCol('SCORE', String(score).padStart(6, '0'), 145);

        // 3) COINS
        ctx.fillStyle = '#facc15';
        ctx.beginPath();
        ctx.ellipse(294, 38, 6, 8, 0, 0, Math.PI * 2);
        ctx.fill();
        ctx.strokeStyle = '#78350f';
        ctx.lineWidth = 1.5;
        ctx.stroke();
        drawHudCol('COINS', 'x ' + String(coins).padStart(2, '0'), 306);

        // 4) STAGE
        const zoneStr = currentZone === 'UNDERGROUND' ? `1-${currentStage}B` : `1-${currentStage}`;
        drawHudCol('STAGE', zoneStr, 425, '#38bdf8');

        // 5) POWER STATE (이모지 없이 순수 영문 텍스트만 사용하여 네모 깨짐 현상 원천 차단!)
        let stateName = 'NORMAL';
        let stateColor = '#e2e8f0';
        if (player) {
          if (player.starTimer > 0) {
            stateName = 'STAR';
            stateColor = '#fef08a';
          } else if (player.powerLevel === 2) {
            stateName = 'FIRE';
            stateColor = '#fb923c';
          } else if (player.powerLevel === 1) {
            stateName = 'SUPER';
            stateColor = '#4ade80';
          }
        }
        drawHudCol('POWER', stateName, 540, stateColor);

        // 6) TIME
        const timeStr = String(Math.max(0, Math.ceil(timeLeft))).padStart(3, '0');
        const timeColor = timeLeft <= 60 ? '#f87171' : '#ffffff';
        drawHudCol('TIME', timeStr, 685, timeColor);

        ctx.restore();
      }

      function renderGame() {
        ctx.clearRect(0, 0, VIEW_W, VIEW_H);
        drawBackground();
        drawCheckpointAndFlagpole();
        drawFieldCoinsAndItems();
        drawPiranhas();

        if (gameState === 'PIPE_WARP') {
          drawPlayer();
          drawSolidsAndPlatforms();
        } else {
          drawSolidsAndPlatforms();
          drawPlayer();
        }

        drawEnemies();
        drawFireballs();
        drawParticlesAndPopups();
        drawHUD();
      }

      // ==========================================
      // 13. 게임 루프 및 리사이즈 대응
      // ==========================================
      let rafId = null;

      function gameLoop() {
        updateGame();
        renderGame();
        rafId = window.requestAnimationFrame(gameLoop);
      }

      window.addEventListener('resize', () => {
        renderGame();
      });

      window.onload = function () {
        if (rafId) {
          window.cancelAnimationFrame(rafId);
        }
        startFullNewGame(1);
        rafId = window.requestAnimationFrame(gameLoop);
      };
    })();
  </script>
</body>
</html>
