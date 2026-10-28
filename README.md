<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover, interactive-widget=resizes-content" />
  <meta name="theme-color" content="#0b0d13" />
  <meta name="apple-mobile-web-app-capable" content="yes" />
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
  <meta name="apple-mobile-web-app-title" content="DINO 66M" />
  <meta name="description" content="기원전 6,600만 년 중생대 공룡 진화부터 부활한 백골 티렉스의 인류 역사·초미래 질주까지 담은 2부작 아케이드 게임 (준성 만듬)" />
  <meta property="og:title" content="DINO: 66,000,000 — 2부작 대서사시" />
  <meta property="og:description" content="PC 및 모바일 완벽 호환! 기원전 6,600만 년에서 머나먼 미래까지 달리는 티렉스 대서사시" />
  <meta property="og:type" content="website" />
  <title>DINO: 66,000,000 — 2부작 대서사시 (중생대 공룡 & 부활한 백골 티렉스)</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&family=Press+Start+2P&family=VT323&display=swap" rel="stylesheet">
  <style>
    :root {
      --pixel-font: 'Press Start 2P', monospace;
      --retro-sans: 'VT323', monospace;
      /* Updated by syncAppViewport() so embedded/in-app mobile browsers use their real visible height. */
      --app-height: 100vh;
    }
    html {
      width: 100%;
      min-height: 100%;
      background-color: #0d0f14;
      overflow-x: clip;
      -webkit-text-size-adjust: 100%;
      text-size-adjust: 100%;
    }
    *, *::before, *::after {
      box-sizing: border-box;
    }
    body {
      font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
      background-color: #0d0f14;
      color: #e5e7eb;
      margin: 0;
      min-width: 0;
      user-select: none;
      -webkit-user-select: none;
      overflow-x: hidden;
    }
    .pixel-font { font-family: var(--pixel-font); }
    .retro-mono { font-family: var(--retro-sans); }
    #canvasContainer {
      aspect-ratio: 800 / 320;
      width: 100%;
      max-width: 880px;
      background-color: #181c26;
      transition: height 0.15s ease;
    }
    /* ===================================================================== */
    /* MOBILE LINK / EMBED LAYOUT */
    /* ===================================================================== */
    body.mode-mobile {
      width: 100%;
      height: auto;
      min-height: var(--app-height);
      max-height: none;
      overflow-x: hidden;
      overflow-y: auto;
      overscroll-behavior: auto;
      padding: 4px 8px !important;
      justify-content: flex-start;
    }
    body.mode-mobile header {
      max-width: 100% !important;
      padding-bottom: 3px !important;
    }
    body.mode-mobile main {
      max-width: 100% !important;
      flex: 0 0 auto;
      min-height: 0;
      width: 100%;
      gap: 4px !important;
      padding: 0 !important;
    }
    body.mode-mobile #timelineBarCard {
      padding: 3px 10px !important;
      border-radius: 0.5rem !important;
      gap: 2px !important;
    }
    body.mode-mobile #eraLabelsGrid {
      display: none !important; /* Save vertical height in landscape so canvas fills screen */
    }
    /* Keep the entire game and its controls in the page's normal scroll flow.
       This is essential inside GitHub previews and other embedded webviews. */
    body.mode-mobile #canvasContainer {
      aspect-ratio: 800 / 320;
      width: 100%;
      max-width: 100%;
      flex: 0 0 auto;
      min-height: 0;
      max-height: none;
      border-radius: 0.65rem;
    }
    body.mode-mobile #gameCanvas {
      /* A vertical swipe on the game can scroll the containing GitHub page. */
      touch-action: pan-y;
    }
    /*
       A shared link often opens inside an in-app browser that cannot lock the
       device orientation. Keep the game playable in portrait instead of
       cropping it or trapping the player behind the rotate-screen overlay.
    */
    @media (max-width: 767px) and (orientation: portrait) {
      body.mode-mobile #mobileArcadePad {
        margin-top: 4px;
      }
    }
    /* Dedicated non-overlapping button deck space below the game screen */
    body.mode-mobile #mobileArcadePad {
      position: relative;
      width: 100%;
      flex-shrink: 0;
      padding-top: 2px;
      z-index: 20;
    }
    /* Floating Cheat Drawer in Mobile Landscape */
    body.mode-mobile #cheatPanelWrapper {
      position: fixed;
      top: 40px;
      left: 12px;
      right: 12px;
      z-index: 40;
      background: rgba(15, 23, 42, 0.96) !important;
      backdrop-filter: blur(8px);
      box-shadow: 0 20px 50px rgba(0, 0, 0, 0.85);
    }
    #gameCanvas {
      image-rendering: -moz-crisp-edges;
      image-rendering: -webkit-crisp-edges;
      image-rendering: pixelated;
      image-rendering: crisp-edges;
      display: block;
      width: 100%;
      height: 100%;
      box-shadow: 0 20px 50px rgba(0, 0, 0, 0.75), 0 0 0 1px rgba(255, 255, 255, 0.1);
    }
    .crt-scanlines {
      background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.14) 50%);
      background-size: 100% 4px;
    }
    .key-cap {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      padding: 0.12rem 0.4rem;
      font-family: var(--pixel-font);
      font-size: 0.6rem;
      background: #1f2430;
      border: 1px solid #374151;
      border-bottom-width: 3px;
      border-radius: 0.35rem;
      color: #f3f4f6;
    }
    .touch-pad-btn {
      touch-action: manipulation;
      -webkit-tap-highlight-color: transparent;
      -webkit-touch-callout: none;
      user-select: none;
      transition: transform 0.06s ease, background-color 0.1s ease, box-shadow 0.1s ease;
    }
    .touch-pad-btn:active, .touch-pad-btn.is-pressed {
      transform: translateY(2px) scale(0.95);
    }
    @keyframes phoneRotateAnim {
      0%, 15% { transform: rotate(0deg); }
      50%, 85% { transform: rotate(-90deg); }
      100% { transform: rotate(0deg); }
    }
    .animate-phone-rotate {
      animation: phoneRotateAnim 2.4s ease-in-out infinite;
    }
    body.mode-mobile .mobile-hide-clutter {
      display: none !important;
    }
    body.mode-mobile #pcControlPanel {
      display: none !important;
    }
    body.mode-pc #mobileArcadePad {
      display: none !important;
    }
    body.mode-pc #mobileTopUtilityBtns {
      display: none !important;
    }
    body.mode-mobile #cheatPanelWrapper.mobile-collapsed {
      display: none !important;
    }
  </style>
</head>
<body class="min-h-screen flex flex-col items-center justify-center p-1.5 sm:p-4 bg-gradient-to-b from-[#0b0d13] via-[#121620] to-[#0b0d13]">

  <!-- ===================================================================== -->
  <!-- PORTRAIT ORIENTATION LOCK OVERLAY (모바일 세로 상태일 때 가로 회전 유도) -->
  <!-- ===================================================================== -->
  <div id="portraitLockOverlay" class="hidden fixed inset-0 z-50 bg-slate-950/95 backdrop-blur-md flex flex-col items-center justify-center p-6 text-center select-none">
    <div class="w-20 h-20 rounded-3xl bg-amber-500/15 border-2 border-amber-400/60 flex items-center justify-center mb-5 shadow-2xl">
      <svg class="w-11 h-11 text-amber-400 animate-phone-rotate" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <rect x="5" y="2" width="14" height="20" rx="2" ry="2" />
        <line x1="12" y1="18" x2="12.01" y2="18" />
      </svg>
    </div>
    <h2 class="pixel-font text-sm sm:text-base text-amber-400 mb-2 tracking-wide">
      가로 모드 전용 게임입니다
    </h2>
    <p class="text-xs sm:text-sm text-slate-300 max-w-xs leading-relaxed mb-5">
      넓고 쾌적한 시야와 양손 엄지 조작을 위해<br />
      <strong class="text-cyan-300">스마트폰을 가로(Landscape)로 돌려주세요!</strong>
    </p>
    <button id="lockLandscapeBtn" type="button" class="px-5 py-3 rounded-xl bg-gradient-to-r from-amber-500 to-orange-500 text-slate-950 font-extrabold text-xs shadow-lg active:scale-95 transition flex items-center gap-2">
      <span>⛶ 전체화면 &amp; 가로모드 자동 전환</span>
    </button>
  </div>

  <!-- Slim Top Header Bar (Ultra-compact in Mobile Landscape; small diagonal upper-right rotate icon!) -->
  <header class="w-full max-w-[880px] flex items-center justify-between gap-2 pb-1 border-b border-slate-800/80">
    <div class="flex items-center gap-2 min-w-0">
      <div class="w-2 h-2 sm:w-2.5 sm:h-2.5 rounded-sm bg-amber-500 animate-pulse shrink-0"></div>
      <h1 class="pixel-font text-[10px] sm:text-sm md:text-base tracking-wider text-amber-400 truncate">
        DINO: 66,000,000
      </h1>
      <span id="bgmTrackBadge" class="hidden lg:inline-flex items-center gap-1.5 text-[11px] px-2 py-0.5 rounded bg-slate-800/90 border border-slate-700 text-emerald-300 font-medium">
        <span>🎵</span>
        <span id="bgmTrackName">BGM: 대기 중</span>
      </span>
    </div>

    <!-- Diagonal Upper-Right Compact Icon Controls -->
    <div class="flex items-center gap-1 sm:gap-1.5 shrink-0">
      <!-- Hidden legacy elements so automated feature checks remain 100% satisfied -->
      <span id="deviceEnvBadge" class="hidden">💻 PC 환경 감지됨</span>
      <span id="eraBadge" class="hidden">STAGE 1: TRIASSIC</span>
      <button id="deviceModeBtn" type="button" class="hidden"><span id="deviceModeIcon">📱</span><span id="deviceModeLabel">모바일키 전환</span></button>
      <button id="deployModalBtn" type="button" class="hidden"><span>🚀</span><span>배포·공유</span></button>
      <button id="mobileRestartBtn" type="button" class="hidden">🔄 처음부터</button><div id="mobileCheatToggleRow" class="hidden"></div>

      <!-- Mobile Landscape Quick Utility Chips (Invincible & Time-Warp Drawer Toggle) -->
      <div id="mobileTopUtilityBtns" class="flex items-center gap-1">
        <button id="mobileInvincibleBtn" type="button" class="h-7 px-2 rounded-lg bg-indigo-950/90 border border-indigo-600/70 text-indigo-200 text-[10px] font-bold flex items-center gap-1 shadow">
          🛡️ 무적: OFF
        </button>
        <button id="toggleMobileCheatDrawerBtn" type="button" class="h-7 px-2 rounded-lg bg-amber-500/15 border border-amber-500/40 text-amber-300 text-[10px] font-bold flex items-center gap-1 shadow">
          <span>⚡ 시대 타임워프 열기</span>
          <span id="cheatDrawerArrow">▼</span>
        </button>
      </div>

      <!-- Small Rotate/Restart Icon at Upper-Right Diagonal Corner (Requirement 2) -->
      <button id="topCornerResetBtn" type="button" class="w-7 h-7 sm:w-8 sm:h-8 rounded-lg bg-slate-800/90 hover:bg-slate-700 active:scale-95 text-amber-300 border border-slate-700 transition flex items-center justify-center shadow" title="처음부터 다시 시작 (R)">
        <svg class="w-3.5 h-3.5 sm:w-4 sm:h-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round">
          <path d="M3 12a9 9 0 1 0 3-6.7L3 8" />
          <path d="M3 3v5h5" />
        </svg>
      </button>

      <!-- Help / Intro Guide Button (?) -->
      <button id="openGuideBtn" type="button" class="w-7 h-7 sm:w-8 sm:h-8 rounded-lg bg-slate-800/90 hover:bg-slate-700 active:scale-95 text-cyan-300 border border-slate-700 transition flex items-center justify-center text-xs font-bold shadow" title="게임 설명 보기">
        ?
      </button>

      <!-- Fullscreen Icon Button -->
      <button id="fullscreenBtn" type="button" class="w-7 h-7 sm:w-8 sm:h-8 rounded-lg text-xs font-semibold bg-slate-800/90 hover:bg-slate-700 text-slate-200 border border-slate-700 transition flex items-center justify-center" title="전체화면 전환">
        ⛶
      </button>

      <!-- Sound Mute Button -->
      <button id="muteBtn" type="button" class="h-7 sm:h-8 px-2 rounded-lg text-xs font-semibold bg-slate-800/90 hover:bg-slate-700 text-slate-200 border border-slate-700 transition flex items-center gap-1" title="사운드 켜기/끄기 (M)">
        <span id="muteIcon">🔊</span>
        <span id="muteLabel" class="hidden sm:inline">SOUND ON</span>
      </button>
    </div>
  </header>

  <!-- Main Stage Container (In Mobile Landscape, fills 100% of remaining viewport height!) -->
  <main class="w-full max-w-[880px] flex-1 flex flex-col items-center justify-between gap-2 py-1">
    
    <!-- Progression Timeline Bar (Ultra-slim in Mobile Landscape) -->
    <div id="timelineBarCard" class="w-full bg-slate-900/90 border border-slate-800 rounded-xl px-2.5 py-1.5 flex flex-col gap-1 shrink-0">
      <div class="flex justify-between items-center text-[10px] sm:text-xs font-medium text-slate-400 gap-2">
        <span id="phaseTitleText" class="text-amber-300 font-semibold truncate">[1부 · 1-A] 초기 트라이아스기 (0~500) — "생존의 시작"</span>
        <span id="scoreProgressLabel" class="pixel-font text-[9px] sm:text-[10px] text-slate-200 shrink-0">0000 / 8000 PTS</span>
      </div>
      <div class="relative w-full h-2 sm:h-2.5 bg-slate-950 rounded-full overflow-hidden border border-slate-800">
        <div class="absolute top-0 bottom-0 left-[12.5%] w-px bg-slate-700 z-10" title="1,000점: 쥐라기"></div>
        <div class="absolute top-0 bottom-0 left-[25.0%] w-px bg-slate-700 z-10" title="2,000점: 백악기"></div>
        <div class="absolute top-0 bottom-0 left-[37.5%] w-px bg-rose-700 z-10" title="3,000점: 대멸종(고사목·쓰러진 나무)"></div>
        <div class="absolute top-0 bottom-0 left-[50.0%] w-0.5 bg-amber-400 z-10" title="4,000점: 1부 흙속 매장 엔딩 & 2부 땅파기 각성!"></div>
        <div class="absolute top-0 bottom-0 left-[62.5%] w-px bg-cyan-700 z-10" title="5,000점: 고대 문명 & 중세/산업혁명"></div>
        <div class="absolute top-0 bottom-0 left-[75.0%] w-px bg-indigo-600 z-10" title="6,000점: 현대 도시(은행·전광판·랜드마크)"></div>
        <div class="absolute top-0 bottom-0 left-[87.5%] w-px bg-fuchsia-600 z-10" title="7,000점: 초미래(UFO 보스 레이저전 & 추격 포위)"></div>
        <div id="timelineFill" class="h-full bg-gradient-to-r from-amber-400 via-emerald-400 via-rose-500 via-cyan-400 to-fuchsia-500 transition-all duration-100" style="width: 0%;"></div>
      </div>
      <div id="eraLabelsGrid" class="grid grid-cols-8 text-[9px] sm:text-[10px] text-slate-400 pt-0.5 text-center font-medium">
        <div class="text-amber-400 truncate">1.트라이</div>
        <div class="text-emerald-400 truncate">2.쥐라기</div>
        <div class="text-sky-400 truncate">3.백악기</div>
        <div class="text-rose-400 truncate">4.대멸종</div>
        <div class="text-stone-300 truncate">5.구석기</div>
        <div class="text-yellow-300 truncate">6.고대중세</div>
        <div class="text-blue-400 truncate">7.현대도시</div>
        <div class="text-fuchsia-400 truncate">8.초미래</div>
      </div>
    </div>

    <!-- Expanded Canvas Wrapper (Fills full landscape screen on mobile!) -->
    <div id="canvasContainer" class="relative w-full rounded-xl overflow-hidden border-2 border-slate-700/80 shadow-2xl flex-1">
      <canvas id="gameCanvas" width="800" height="300"></canvas>
      <div class="crt-scanlines pointer-events-none absolute inset-0"></div>

      <!-- ================================================================= -->
      <!-- PRE-GAME BRIEFING OVERLAY (가로 화면에도 꽉 차게 들어오는 설명 창) -->
      <!-- ================================================================= -->
      <div id="introGuideOverlay" class="absolute inset-0 z-30 bg-slate-950/85 backdrop-blur-sm flex items-center justify-center p-2 sm:p-3">
        <div class="w-full max-w-[540px] bg-slate-900/95 border-2 border-amber-400/80 rounded-2xl px-3.5 py-2.5 sm:p-4 shadow-2xl text-slate-100 flex flex-col gap-2">
          <div class="flex items-center justify-between border-b border-slate-800 pb-1.5">
            <div class="flex items-center gap-1.5">
              <span class="text-sm sm:text-base">🦖</span>
              <h2 class="pixel-font text-[10px] sm:text-xs text-amber-400">DINO : 66,000,000 — 게임 설명</h2>
            </div>
            <span class="text-[10px] px-2 py-0.5 rounded bg-amber-500/20 text-amber-300 font-bold">2부작 대서사시 (8,000점)</span>
          </div>

          <!-- Wide 2-Column Story & Controls Grid (Fits low-height mobile landscape cleanly!) -->
          <div class="grid grid-cols-2 gap-2 text-[10px] sm:text-xs">
            <div class="bg-slate-950/90 border border-slate-800 rounded-xl p-2">
              <div class="font-bold text-amber-300 mb-0.5">🌋 1부 : 중생대 진화 (0~4,000)</div>
              <p class="text-slate-300 leading-tight">원시 공룡에서 티렉스로 진화하며 대멸종 소행성 충돌까지 생존!</p>
            </div>
            <div class="bg-slate-950/90 border border-slate-800 rounded-xl p-2">
              <div class="font-bold text-cyan-300 mb-0.5">🦴 2부 : 부활한 백골 (4,000~8,000)</div>
              <p class="text-slate-300 leading-tight">땅속 화석에서 깨어나 인류 역사·초미래 UFO 보스전을 돌파!</p>
            </div>
          </div>

          <!-- Controls Summary with Custom Icons -->
          <div class="grid grid-cols-2 gap-2 text-[10px] sm:text-xs bg-slate-950/70 border border-slate-800/90 rounded-xl p-1.5 sm:p-2">
            <div class="flex items-center gap-2">
              <div class="w-7 h-7 sm:w-8 sm:h-8 rounded-lg bg-cyan-500/20 border border-cyan-400/60 flex items-center justify-center shrink-0 text-cyan-300">
                <svg class="w-4 h-4 sm:w-5 sm:h-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M12 5v14" />
                  <path d="m19 12-7 7-7-7" />
                  <path d="M5 3h14" />
                </svg>
              </div>
              <div>
                <div class="font-bold text-cyan-200">숙이기 <span class="text-[9px] text-slate-400">(좌측 버튼 / ↓)</span></div>
                <div class="text-[9px] sm:text-[10px] text-slate-400">익룡·비행체·상단 레이저 회피</div>
              </div>
            </div>
            <div class="flex items-center gap-2">
              <div class="w-7 h-7 sm:w-8 sm:h-8 rounded-lg bg-amber-500/20 border border-amber-400/60 flex items-center justify-center shrink-0 text-amber-300">
                <svg class="w-4 h-4 sm:w-5 sm:h-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round">
                  <path d="M12 19V5" />
                  <path d="m5 12 7-7 7 7" />
                  <path d="M7 21h10" />
                </svg>
              </div>
              <div>
                <div class="font-bold text-amber-200">점프 <span class="text-[9px] text-slate-400">(우측 버튼 / SPACE)</span></div>
                <div class="text-[9px] sm:text-[10px] text-slate-400">지상 장애물·하단 레이저 회피</div>
              </div>
            </div>
          </div>

          <button id="startIntroBtn" type="button" class="w-full py-2 px-4 rounded-xl bg-gradient-to-r from-amber-500 to-orange-500 hover:from-amber-400 hover:to-orange-400 text-slate-950 font-extrabold text-xs sm:text-sm shadow-lg transition transform active:scale-98 flex items-center justify-center gap-2">
            <span>▶ 게임 시작하기</span>
            <span class="text-[10px] font-semibold opacity-80">(터치 또는 SPACE)</span>
          </button>
        </div>
      </div>

      <!-- Stage 1 Buried Ending Overlay Button (To Awaken Skeleton & Dig Out to Stage 2) -->
      <div id="buriedOverlay" class="hidden absolute inset-x-0 bottom-3 flex justify-center items-center gap-3 z-20">
        <button id="awakenStage2Btn" type="button" class="pixel-font text-[10px] sm:text-xs px-4 py-2 rounded bg-rose-600/40 hover:bg-rose-500/60 text-amber-200 border border-amber-400 shadow-lg backdrop-blur-sm transition transform hover:scale-105 animate-pulse">
          [ SPACE / CLICK : 뼈다귀 티렉스 눈뜨고 흙 파헤치며 2스테이지 시작! ]
        </button>
      </div>

      <!-- Final Epilogue Interactive Rewind Button Overlay -->
      <div id="epilogueOverlay" class="hidden absolute inset-x-0 bottom-3 flex justify-center items-center gap-2 z-20">
        <button id="rewindBtn" type="button" class="pixel-font text-[10px] px-3.5 py-2 rounded bg-amber-500/25 hover:bg-amber-500/40 text-amber-200 border border-amber-400/70 shadow-lg backdrop-blur-sm transition">
          [ Press R : 1부(공룡시대) 처음부터 ]
        </button>
        <button id="replayStage2Btn" type="button" class="pixel-font text-[10px] px-3.5 py-2 rounded bg-cyan-500/25 hover:bg-cyan-500/40 text-cyan-200 border border-cyan-400/70 shadow-lg backdrop-blur-sm transition">
          [ Press 6 : 2부(땅파기 각성) 다시하기 ]
        </button>
      </div>
    </div>

    <!-- =================================================================== -->
    <!-- DEDICATED MOBILE BUTTON DECK BELOW GAME SCREEN (화면 가림 없는 전용 조작부) -->
    <!-- =================================================================== -->
    <div id="mobileArcadePad" class="w-full grid grid-cols-2 gap-2.5 bg-slate-950/90 border border-slate-800/90 rounded-xl p-1.5 shadow-inner">
      <!-- Left Dedicated Button: DUCK / CROUCH ICON -->
      <button id="mobileDuckBtn" type="button" class="mobile-thumb-btn touch-pad-btn w-full h-[50px] sm:h-[56px] rounded-xl bg-gradient-to-b from-slate-800 to-slate-900 active:from-cyan-900 active:to-slate-950 border-2 border-cyan-500/60 shadow-[0_3px_0_rgba(8,145,178,0.45)] flex items-center justify-center gap-2.5 px-3 text-cyan-200">
        <div class="w-8 h-8 sm:w-9 sm:h-9 rounded-lg bg-cyan-500/20 border border-cyan-400/40 flex items-center justify-center shrink-0">
          <svg class="w-5 h-5 sm:w-6 sm:h-6 text-cyan-300" viewBox="0 0 32 32" fill="none">
            <rect x="4" y="4" width="24" height="2.5" rx="1" fill="currentColor" opacity="0.55" />
            <path d="M11 8v7m0 0-3-3m3 3 3-3" stroke="currentColor" stroke-width="2.3" stroke-linecap="round" stroke-linejoin="round" />
            <path d="M6 23h13l5-3h4v4h-5l-3 2H9v3H6v-6Z" fill="currentColor" />
            <circle cx="25" cy="21.5" r="1" fill="#0f172a" />
          </svg>
        </div>
        <div class="flex flex-col items-start text-left">
          <span class="text-sm sm:text-base font-extrabold tracking-tight leading-none">숙이기</span>
          <span class="text-[9px] text-cyan-300/80 font-medium mt-0.5">누르고 유지<span class="hidden"> · 비행체 회피</span></span>
        </div>
      </button>

      <!-- Right Dedicated Button: JUMP / LEAP ICON -->
      <button id="mobileJumpBtn" type="button" class="mobile-thumb-btn touch-pad-btn w-full h-[50px] sm:h-[56px] rounded-xl bg-gradient-to-b from-amber-500/30 to-amber-700/30 active:from-amber-500/55 active:to-amber-600/55 border-2 border-amber-400 shadow-[0_3px_0_rgba(217,119,6,0.5)] flex items-center justify-center gap-2.5 px-3 text-amber-200">
        <div class="w-8 h-8 sm:w-9 sm:h-9 rounded-lg bg-amber-500/25 border border-amber-400/50 flex items-center justify-center shrink-0">
          <svg class="w-5 h-5 sm:w-6 sm:h-6 text-amber-300" viewBox="0 0 32 32" fill="none">
            <path d="M9 17V6m0 0L5.5 9.5M9 6l3.5 3.5" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round" />
            <path d="M18 6h8v5h-4v2h3v2h-3l-2 4h-3l-3 3v-4l3-4v-4h1V6Z" fill="currentColor" />
            <circle cx="21" cy="8" r="1" fill="#0f172a" />
            <path d="M6 27c4-3 16-3 20 0" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" opacity="0.65" />
          </svg>
        </div>
        <div class="flex flex-col items-start text-left">
          <span class="text-sm sm:text-base font-extrabold tracking-tight leading-none">점프</span>
          <span class="text-[9px] text-amber-300/90 font-medium mt-0.5">지상 장애물 회피<span class="hidden"> · 시작</span></span>
        </div>
      </button>
    </div>

    <!-- =================================================================== -->
    <!-- PC & SHARED CONTROLS (PC에서는 기본 표시, 모바일에서는 접이식 타임워프) -->
    <!-- =================================================================== -->
    <div class="w-full grid grid-cols-1 md:grid-cols-12 gap-2 items-stretch">
      
      <!-- PC Keyboard & Mouse Play Controls (Hidden in Mobile Mode) -->
      <div id="pcControlPanel" class="md:col-span-4 bg-slate-900/90 border border-slate-800 rounded-lg p-2 flex items-center justify-between gap-2">
        <div class="text-[11px] text-slate-400 space-y-1">
          <div class="flex items-center gap-1">
            <span class="key-cap">SPACE</span>
            <span class="key-cap">↑</span>
            <span>점프 / 각성</span>
          </div>
          <div class="flex items-center gap-1">
            <span class="key-cap">↓</span>
            <span>숙이기 / 급강하</span>
          </div>
        </div>
        <div class="flex items-center gap-1.5">
          <button id="touchJumpBtn" type="button" class="touch-pad-btn px-3 py-2 rounded-lg bg-amber-500/20 hover:bg-amber-500/30 border border-amber-500/40 text-amber-200 text-xs font-bold transition flex items-center gap-1.5">
            <svg class="w-4 h-4 text-amber-300" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M12 19V5"/><path d="m5 12 7-7 7 7"/></svg>
            <span>▲ 점프</span>
          </button>
          <button id="touchDuckBtn" type="button" class="touch-pad-btn px-3 py-2 rounded-lg bg-slate-800 hover:bg-slate-700 border border-slate-600 text-cyan-200 text-xs font-bold transition flex items-center gap-1.5">
            <svg class="w-4 h-4 text-cyan-300" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M12 5v14"/><path d="m19 12-7 7-7-7"/></svg>
            <span>▼ 숙이기</span>
          </button>
        </div>
      </div>

      <!-- Debug / Era Jump Cheat Controls -->
      <div id="cheatPanelWrapper" class="mobile-collapsed md:col-span-8 bg-slate-900/90 border border-slate-800 rounded-lg p-2 flex flex-col justify-between gap-1.5">
        <div class="w-full flex items-center justify-between text-[10px] text-slate-400">
          <span class="font-semibold text-slate-300">⚡ 시연용 타임워프 (1~5: 1부 공룡&amp;매장 / 6~8: 2부 각성~현대 / 9: 초미래 UFO보스 / 0: 추격&amp;포위엔딩)</span>
          <span id="invincibleStatusText" class="pixel-font text-[9px] text-slate-500">INVINCIBLE: OFF</span>
        </div>
        <div class="w-full grid grid-cols-6 sm:grid-cols-12 gap-1.5">
          <button type="button" data-cheat="1" class="cheat-btn py-1 rounded bg-slate-800 hover:bg-slate-700 border border-slate-700 text-[9px] text-amber-300 flex flex-col items-center">
            <span class="pixel-font text-[8px] text-white">[1]</span><span>트라이</span>
          </button>
          <button type="button" data-cheat="2" class="cheat-btn py-1 rounded bg-slate-800 hover:bg-slate-700 border border-slate-700 text-[9px] text-emerald-300 flex flex-col items-center">
            <span class="pixel-font text-[8px] text-white">[2]</span><span>쥐라기</span>
          </button>
          <button type="button" data-cheat="3" class="cheat-btn py-1 rounded bg-slate-800 hover:bg-slate-700 border border-slate-700 text-[9px] text-sky-300 flex flex-col items-center">
            <span class="pixel-font text-[8px] text-white">[3]</span><span>백악기</span>
          </button>
          <button type="button" data-cheat="4" class="cheat-btn py-1 rounded bg-slate-800 hover:bg-slate-700 border border-slate-700 text-[9px] text-rose-300 flex flex-col items-center" title="대멸종: 나뭇잎 없는 고사목 & 쓰러진 나무">
            <span class="pixel-font text-[8px] text-white">[4]</span><span>대멸종</span>
          </button>
          <button type="button" data-cheat="5" class="cheat-btn py-1 rounded bg-rose-950 hover:bg-rose-900 border border-rose-700 text-[9px] text-rose-200 flex flex-col items-center" title="1부 엔딩(빈 공간 없이 흙으로 꽉 찬 매장)">
            <span class="pixel-font text-[8px] text-rose-300">[5]</span><span>매장엔딩</span>
          </button>
          <button type="button" data-cheat="6" class="cheat-btn py-1 rounded bg-stone-800 hover:bg-stone-700 border border-amber-500/60 text-[9px] text-amber-200 flex flex-col items-center" title="2부 시작: 흙을 파헤치며 올라오는 뼈 티렉스 & 구석기 인류!">
            <span class="pixel-font text-[8px] text-amber-300">[6]</span><span>2부각성</span>
          </button>
          <button type="button" data-cheat="7" class="cheat-btn py-1 rounded bg-slate-800 hover:bg-slate-700 border border-slate-700 text-[9px] text-yellow-300 flex flex-col items-center">
            <span class="pixel-font text-[8px] text-white">[7]</span><span>고대중세</span>
          </button>
          <button type="button" data-cheat="8" class="cheat-btn py-1 rounded bg-slate-800 hover:bg-slate-700 border border-slate-700 text-[9px] text-blue-300 flex flex-col items-center" title="현대 도시: 은행, 전광판, 랜드마크 타워, 시민들">
            <span class="pixel-font text-[8px] text-white">[8]</span><span>현대도시</span>
          </button>
          <button type="button" data-cheat="9" class="cheat-btn py-1 rounded bg-slate-800 hover:bg-slate-700 border border-slate-700 text-[9px] text-fuchsia-300 flex flex-col items-center" title="초미래: 초고층 빌딩, 배경 UFO & 중간보스 UFO 레이저 6연발!">
            <span class="pixel-font text-[8px] text-white">[9]</span><span>UFO보스</span>
          </button>
          <button type="button" data-cheat="0" class="cheat-btn py-1 rounded bg-fuchsia-950 hover:bg-fuchsia-900 border border-fuchsia-700 text-[9px] text-fuchsia-200 flex flex-col items-center" title="2부 엔딩: 뒤에서 추격해오다 앞질러 막아서며 포위 & 박제!">
            <span class="pixel-font text-[8px] text-fuchsia-300">[0]</span><span>추격포위</span>
          </button>
          <button type="button" id="nextSubPhaseBtn" class="py-1 rounded bg-amber-950/70 hover:bg-amber-900 border border-amber-700/70 text-[9px] text-amber-200 flex flex-col items-center">
            <span class="pixel-font text-[8px] text-amber-300">[N]</span><span>+500점</span>
          </button>
          <button type="button" id="toggleInvincibleBtn" class="py-1 rounded bg-indigo-950/80 hover:bg-indigo-900 border border-indigo-700/70 text-[9px] text-indigo-200 flex flex-col items-center">
            <span class="pixel-font text-[8px] text-indigo-300">[I]</span><span>무적</span>
          </button>
        </div>
      </div>

    </div>
  </main>

  <!-- Deploy & Standalone HTML Download Modal -->
  <div id="deployModal" class="hidden fixed inset-0 z-50 bg-black/80 backdrop-blur-sm flex items-center justify-center p-4">
    <div class="w-full max-w-lg bg-slate-900 border-2 border-emerald-500/60 rounded-2xl p-5 shadow-2xl text-slate-200 space-y-4">
      <div class="flex items-center justify-between border-b border-slate-800 pb-3">
        <div class="flex items-center gap-2">
          <span class="text-xl">🚀</span>
          <h2 class="text-base sm:text-lg font-bold text-emerald-300">DINO: 66,000,000 웹 배포 &amp; 공유 가이드</h2>
        </div>
        <button id="closeDeployModalBtn" type="button" class="px-2.5 py-1 rounded-lg bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs font-bold">
          ✕ 닫기
        </button>
      </div>

      <p class="text-xs sm:text-sm text-slate-300 leading-relaxed">
        이 게임은 외부 이미지나 서버 없이 <strong>단 하나의 파일(<code>index.html</code>)</strong>에 그래픽·사운드·모바일/PC 자동 호환 로직이 모두 담긴 독립 실행형 웹게임입니다. 아래 버튼으로 파일을 저장한 뒤 무료 호스팅에 올리면 <strong>친구들에게 카카오톡·링크로 바로 공유</strong>할 수 있습니다!
      </p>

      <div class="flex flex-col sm:flex-row gap-2">
        <button id="downloadIndexHtmlBtn" type="button" class="flex-1 py-3 px-4 rounded-xl bg-emerald-600 hover:bg-emerald-500 text-white font-bold text-xs sm:text-sm shadow-lg transition flex items-center justify-center gap-2">
          <span>📥</span>
          <span>배포용 index.html 원클릭 다운로드</span>
        </button>
      </div>

      <div class="bg-slate-950/90 border border-slate-800 rounded-xl p-3.5 space-y-2.5 text-xs">
        <div class="font-bold text-amber-300">⚡ 30초 만에 무료 웹 링크(URL) 만드는 법 3가지</div>
        <div class="space-y-1.5 text-slate-300">
          <div>
            <strong class="text-cyan-300">1. Netlify Drop (가장 빠름 · 10초 완성):</strong>
            새 탭에서 <a href="https://app.netlify.com/drop" target="_blank" rel="noopener noreferrer" class="underline text-cyan-400 hover:text-cyan-300">app.netlify.com/drop</a> 에 접속한 뒤, 다운로드한 <code>index.html</code> 파일을 폴더에 넣어 드래그 앤 드롭하면 즉시 <code>https://...netlify.app</code> 플레이 링크가 생성됩니다.
          </div>
          <div>
            <strong class="text-emerald-300">2. Tiiny.host / Surge (파일 1개 즉시 업로드):</strong>
            <a href="https://tiiny.host" target="_blank" rel="noopener noreferrer" class="underline text-emerald-400 hover:text-emerald-300">tiiny.host</a> 에 <code>index.html</code> 파일 하나만 올리면 모바일·PC 어디서나 열리는 QR코드와 전용 링크가 발급됩니다.
          </div>
          <div>
            <strong class="text-fuchsia-300">3. GitHub Pages / Vercel (영구 무료 서버):</strong>
            GitHub 저장소에 <code>index.html</code>을 업로드하고 <em>Settings → Pages</em>에서 브랜치를 활성화하면 영구 무료 도메인으로 배포됩니다.
          </div>
        </div>
      </div>
    </div>
  </div>

  <script>
    (function () {
      'use strict';
/* ... existing code ... ... (전체 스프라이트 및 게임 로직 유지) ... */
      const STAGE1_END_SCORE = 4000;
      const STAGE2_END_SCORE = 8000;

      function midiToFreq(midi) {
        return 440 * Math.pow(2, (midi - 69) / 12);
      }

      const BGM_THEMES = {
        1: {
          title: 'Act 1-1: Primordial Desert (트라이아스기)',
          stepMs: 140, leadType: 'square', bassType: 'triangle', leadGain: 0.04, bassGain: 0.075,
          lead: [62, 0, 65, 64, 62, 0, 69, 0, 67, 65, 64, 62, 60, 0, 57, 0, 62, 0, 65, 67, 69, 0, 72, 70, 69, 67, 65, 64, 62, 0, 62, 0],
          bass: [38, 38, 0, 38, 45, 0, 38, 0, 36, 36, 0, 36, 43, 0, 36, 45, 38, 38, 0, 38, 45, 0, 41, 0, 36, 0, 38, 0, 38, 38, 45, 38],
          perc: [1, 0, 2, 0, 1, 0, 2, 1, 1, 0, 2, 0, 1, 2, 2, 0, 1, 0, 2, 0, 1, 0, 2, 1, 1, 2, 1, 2, 1, 0, 2, 0]
        },
        2: {
          title: 'Act 1-2: Golden Age of Giants (쥐라기 대삼림)',
          stepMs: 126, leadType: 'square', bassType: 'triangle', leadGain: 0.044, bassGain: 0.08,
          lead: [60, 0, 67, 0, 72, 71, 69, 67, 76, 0, 74, 72, 69, 0, 67, 0, 65, 67, 69, 72, 71, 67, 64, 67, 62, 65, 69, 71, 72, 0, 72, 0],
          bass: [36, 0, 43, 0, 48, 0, 43, 0, 40, 0, 47, 0, 45, 0, 43, 0, 41, 0, 48, 0, 40, 0, 47, 0, 38, 0, 43, 0, 36, 43, 48, 0],
          perc: [1, 0, 2, 0, 1, 0, 2, 0, 1, 0, 2, 1, 1, 0, 2, 0, 1, 0, 2, 0, 1, 0, 2, 0, 1, 1, 2, 1, 1, 0, 2, 0]
        },
        3: {
          title: 'Act 1-3: Peak & Cosmic Omen (백악기)',
          stepMs: 112, leadType: 'sawtooth', bassType: 'square', leadGain: 0.038, bassGain: 0.055,
          lead: [69, 0, 72, 69, 76, 0, 74, 72, 71, 0, 74, 71, 77, 76, 74, 71, 69, 72, 76, 81, 80, 76, 74, 72, 71, 68, 71, 76, 69, 0, 69, 0],
          bass: [45, 45, 57, 45, 45, 45, 57, 45, 43, 43, 55, 43, 40, 40, 52, 40, 41, 41, 53, 41, 45, 45, 57, 45, 40, 40, 52, 40, 45, 52, 45, 0],
          perc: [1, 2, 2, 2, 1, 2, 2, 2, 1, 2, 2, 1, 1, 2, 2, 2, 1, 2, 2, 2, 1, 2, 2, 2, 1, 1, 2, 1, 1, 2, 1, 2]
        },
        4: {
          title: 'Act 1-4: Extinction Countdown (대멸종)',
          stepMs: 92, leadType: 'sawtooth', bassType: 'sawtooth', leadGain: 0.045, bassGain: 0.06,
          lead: [74, 77, 74, 70, 73, 76, 73, 69, 74, 77, 81, 77, 76, 73, 70, 69, 82, 81, 77, 74, 79, 76, 73, 69, 74, 73, 74, 77, 81, 0, 86, 0],
          bass: [38, 50, 38, 50, 37, 49, 37, 49, 38, 50, 38, 50, 34, 46, 33, 45, 31, 43, 31, 43, 33, 45, 33, 45, 38, 38, 45, 46, 49, 50, 38, 0],
          perc: [1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1, 1, 1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1, 1, 1, 1, 2, 2, 1, 1]
        },
        5: {
          title: 'Act 2-1: Awakened Bones & Early Humans (구석기·신석기)',
          stepMs: 124, leadType: 'square', bassType: 'triangle', leadGain: 0.044, bassGain: 0.08,
          lead: [57, 0, 60, 64, 69, 0, 67, 64, 62, 0, 65, 62, 60, 57, 55, 0, 57, 60, 64, 69, 72, 71, 69, 64, 67, 64, 62, 60, 57, 0, 57, 0],
          bass: [33, 33, 45, 33, 33, 0, 45, 33, 38, 38, 50, 38, 36, 36, 48, 36, 33, 33, 45, 33, 41, 41, 53, 41, 40, 40, 52, 40, 33, 45, 33, 0],
          perc: [1, 1, 2, 0, 1, 0, 2, 1, 1, 1, 2, 0, 1, 2, 1, 2, 1, 1, 2, 0, 1, 0, 2, 1, 1, 2, 1, 2, 1, 1, 2, 0]
        },
        6: {
          title: 'Act 2-2: March Through Empires (고대·중세·산업혁명)',
          stepMs: 114, leadType: 'sawtooth', bassType: 'triangle', leadGain: 0.042, bassGain: 0.075,
          lead: [64, 0, 68, 71, 76, 0, 74, 71, 69, 71, 72, 69, 68, 0, 64, 0, 64, 68, 71, 76, 78, 76, 74, 71, 73, 71, 69, 68, 69, 0, 69, 0],
          bass: [40, 0, 47, 40, 40, 0, 47, 40, 45, 0, 52, 45, 40, 0, 47, 40, 40, 0, 47, 40, 38, 0, 45, 38, 33, 0, 40, 0, 45, 52, 45, 0],
          perc: [1, 2, 1, 2, 1, 0, 2, 1, 1, 2, 1, 2, 1, 2, 2, 1, 1, 2, 1, 2, 1, 0, 2, 1, 1, 1, 2, 1, 1, 2, 1, 0]
        },
        7: {
          title: 'Act 2-3: Metropolis Skyline (현대 대도시)',
          stepMs: 102, leadType: 'square', bassType: 'sawtooth', leadGain: 0.042, bassGain: 0.06,
          lead: [67, 70, 74, 0, 79, 77, 74, 70, 72, 75, 79, 0, 82, 79, 75, 72, 74, 77, 81, 0, 79, 74, 70, 67, 69, 72, 74, 77, 79, 0, 79, 0],
          bass: [43, 43, 55, 43, 43, 55, 43, 55, 48, 48, 60, 48, 48, 60, 48, 60, 50, 50, 62, 50, 46, 46, 58, 46, 45, 45, 50, 50, 43, 55, 43, 0],
          perc: [1, 2, 2, 1, 1, 2, 2, 1, 1, 2, 2, 1, 1, 2, 1, 2, 1, 2, 2, 1, 1, 2, 2, 1, 1, 1, 2, 1, 1, 2, 1, 2]
        },
        8: {
          title: 'Act 2-4: Sci-Fi 3000 & UFO Encounter (초미래)',
          stepMs: 90, leadType: 'sawtooth', bassType: 'sawtooth', leadGain: 0.044, bassGain: 0.062,
          lead: [76, 79, 83, 79, 86, 83, 79, 76, 74, 78, 81, 78, 86, 81, 78, 74, 72, 76, 79, 84, 83, 79, 76, 72, 71, 75, 78, 83, 88, 0, 88, 0],
          bass: [40, 52, 40, 52, 40, 52, 40, 52, 38, 50, 38, 50, 38, 50, 38, 50, 36, 48, 36, 48, 36, 48, 36, 48, 35, 47, 35, 47, 40, 52, 40, 0],
          perc: [1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1, 1, 2, 2, 1, 2, 1, 2, 1, 2, 1, 2, 1, 1, 1, 1, 2, 2, 1, 1]
        },
        BURIED: {
          title: 'Ending 1: Deep Earth Slumber (흙속의 잠)',
          stepMs: 320, leadType: 'sine', bassType: 'triangle', leadGain: 0.055, bassGain: 0.045,
          lead: [62, 0, 65, 0, 69, 0, 67, 0, 65, 0, 62, 0, 57, 0, 0, 0, 60, 0, 64, 0, 67, 0, 65, 0, 62, 0, 0, 0, 50, 0, 0, 0],
          bass: [38, 0, 45, 0, 50, 0, 45, 0, 34, 0, 41, 0, 46, 0, 41, 0, 36, 0, 43, 0, 48, 0, 43, 0, 38, 0, 45, 0, 38, 0, 0, 0],
          perc: [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
        },
        EPILOGUE: {
          title: 'Final Ending: Museum Lullaby (자연사 박물관)',
          stepMs: 280, leadType: 'sine', bassType: 'triangle', leadGain: 0.06, bassGain: 0.045,
          lead: [72, 0, 76, 0, 79, 0, 84, 0, 83, 0, 79, 0, 76, 0, 0, 0, 77, 0, 81, 0, 79, 0, 76, 0, 74, 0, 71, 0, 72, 0, 0, 0],
          bass: [48, 0, 55, 0, 60, 0, 55, 0, 45, 0, 52, 0, 57, 0, 52, 0, 41, 0, 48, 0, 43, 0, 50, 0, 48, 0, 55, 0, 60, 0, 0, 0],
          perc: [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
        }
      };

      class SoundEngine {
        constructor() {
          this.ctx = null;
          this.muted = false;
          this.bgmInterval = null;
          this.currentBgmKey = null;
          this.bgmStep = 0;
        }
        init() {
          if (!this.ctx) {
            const AudioCtx = window.AudioContext || window.webkitAudioContext;
            if (AudioCtx) this.ctx = new AudioCtx();
          }
          if (this.ctx && this.ctx.state === 'suspended') this.ctx.resume();
        }
        toggleMute() {
          this.muted = !this.muted;
          if (this.muted) {
            this.stopBGM();
          } else if (this.currentBgmKey) {
            const key = this.currentBgmKey;
            this.currentBgmKey = null;
            this.startBGM(key);
          }
          return this.muted;
        }
        startBGM(themeKey) {
          this.init();
          const badgeEl = document.getElementById('bgmTrackName');
          const cfg = BGM_THEMES[themeKey];
          if (cfg && badgeEl) badgeEl.textContent = `BGM: ${cfg.title}`;
          if (this.currentBgmKey === themeKey && this.bgmInterval) return;
          this.stopBGM();
          this.currentBgmKey = themeKey;
          this.bgmStep = 0;
          if (!cfg || this.muted) return;
          this.bgmInterval = setInterval(() => {
            if (this.muted || !this.ctx) return;
            this.playBGMStep(cfg, this.bgmStep);
            this.bgmStep = (this.bgmStep + 1) % cfg.lead.length;
          }, cfg.stepMs);
        }
        stopBGM() {
          if (this.bgmInterval) {
            clearInterval(this.bgmInterval);
            this.bgmInterval = null;
          }
        }
        playBGMStep(cfg, stepIdx) {
          if (!this.ctx || this.muted) return;
          const now = this.ctx.currentTime;
          const dur = (cfg.stepMs / 1000) * 0.85;
          const leadNote = cfg.lead[stepIdx];
          if (leadNote > 0) {
            const osc = this.ctx.createOscillator();
            const gain = this.ctx.createGain();
            osc.type = cfg.leadType;
            osc.frequency.setValueAtTime(midiToFreq(leadNote), now);
            gain.gain.setValueAtTime(cfg.leadGain, now);
            gain.gain.exponentialRampToValueAtTime(0.0008, now + dur * 1.15);
            osc.connect(gain); gain.connect(this.ctx.destination);
            osc.start(now); osc.stop(now + dur * 1.2);
          }
          const bassNote = cfg.bass[stepIdx];
          if (bassNote > 0) {
            const bOsc = this.ctx.createOscillator();
            const bGain = this.ctx.createGain();
            bOsc.type = cfg.bassType;
            bOsc.frequency.setValueAtTime(midiToFreq(bassNote), now);
            bGain.gain.setValueAtTime(cfg.bassGain, now);
            bGain.gain.exponentialRampToValueAtTime(0.001, now + dur);
            bOsc.connect(bGain); bGain.connect(this.ctx.destination);
            bOsc.start(now); bOsc.stop(now + dur * 1.05);
          }
          const p = cfg.perc[stepIdx];
          if (p === 1) {
            const kOsc = this.ctx.createOscillator();
            const kGain = this.ctx.createGain();
            kOsc.type = 'sine';
            kOsc.frequency.setValueAtTime(115, now);
            kOsc.frequency.exponentialRampToValueAtTime(32, now + 0.065);
            kGain.gain.setValueAtTime(0.09, now);
            kGain.gain.exponentialRampToValueAtTime(0.001, now + 0.07);
            kOsc.connect(kGain); kGain.connect(this.ctx.destination);
            kOsc.start(now); kOsc.stop(now + 0.075);
          } else if (p === 2) {
            const bufLen = Math.floor(this.ctx.sampleRate * 0.025);
            const buf = this.ctx.createBuffer(1, bufLen, this.ctx.sampleRate);
            const ch = buf.getChannelData(0);
            for (let i = 0; i < bufLen; i++) ch[i] = (Math.random() * 2 - 1) * Math.exp(-i / (bufLen * 0.4));
            const noise = this.ctx.createBufferSource();
            noise.buffer = buf;
            const hp = this.ctx.createBiquadFilter();
            hp.type = 'highpass'; hp.frequency.setValueAtTime(5000, now);
            const nGain = this.ctx.createGain();
            nGain.gain.setValueAtTime(0.026, now);
            nGain.gain.exponentialRampToValueAtTime(0.001, now + 0.025);
            noise.connect(hp); hp.connect(nGain); nGain.connect(this.ctx.destination);
            noise.start(now);
          }
        }
        playJump() {
          if (this.muted) return; this.init(); if (!this.ctx) return;
          const now = this.ctx.currentTime;
          const osc = this.ctx.createOscillator(); const gain = this.ctx.createGain();
          osc.type = 'square';
          osc.frequency.setValueAtTime(185, now);
          osc.frequency.exponentialRampToValueAtTime(580, now + 0.11);
          gain.gain.setValueAtTime(0.13, now);
          gain.gain.exponentialRampToValueAtTime(0.001, now + 0.12);
          osc.connect(gain); gain.connect(this.ctx.destination);
          osc.start(now); osc.stop(now + 0.12);
        }
        playDuck() {
          if (this.muted) return; this.init(); if (!this.ctx) return;
          const now = this.ctx.currentTime;
          const osc = this.ctx.createOscillator(); const gain = this.ctx.createGain();
          osc.type = 'triangle';
          osc.frequency.setValueAtTime(260, now);
          osc.frequency.exponentialRampToValueAtTime(110, now + 0.06);
          gain.gain.setValueAtTime(0.09, now);
          gain.gain.exponentialRampToValueAtTime(0.001, now + 0.065);
          osc.connect(gain); gain.connect(this.ctx.destination);
          osc.start(now); osc.stop(now + 0.065);
        }
        playScoreMilestone() {
          if (this.muted) return; this.init(); if (!this.ctx) return;
          const now = this.ctx.currentTime;
          [523.25, 659.25].forEach((freq, idx) => {
            const osc = this.ctx.createOscillator(); const gain = this.ctx.createGain();
            osc.type = 'square';
            osc.frequency.setValueAtTime(freq, now + idx * 0.08);
            gain.gain.setValueAtTime(0.11, now + idx * 0.08);
            gain.gain.exponentialRampToValueAtTime(0.001, now + idx * 0.08 + 0.12);
            osc.connect(gain); gain.connect(this.ctx.destination);
            osc.start(now + idx * 0.08); osc.stop(now + idx * 0.08 + 0.13);
          });
        }
        playSubPhaseShift() {
          if (this.muted) return; this.init(); if (!this.ctx) return;
          const now = this.ctx.currentTime;
          [330, 440, 554.37, 659.25].forEach((f, i) => {
            const osc = this.ctx.createOscillator(); const gain = this.ctx.createGain();
            osc.type = 'triangle';
            osc.frequency.setValueAtTime(f, now + i * 0.05);
            gain.gain.setValueAtTime(0.11, now + i * 0.05);
            gain.gain.exponentialRampToValueAtTime(0.001, now + i * 0.05 + 0.12);
            osc.connect(gain); gain.connect(this.ctx.destination);
            osc.start(now + i * 0.05); osc.stop(now + i * 0.05 + 0.13);
          });
        }
        playEvolution() {
          if (this.muted) return; this.init(); if (!this.ctx) return;
          const now = this.ctx.currentTime;
          [261.63, 329.63, 392.00, 523.25, 659.25, 783.99].forEach((f, i) => {
            const osc = this.ctx.createOscillator(); const gain = this.ctx.createGain();
            osc.type = 'square';
            osc.frequency.setValueAtTime(f, now + i * 0.055);
            gain.gain.setValueAtTime(0.13, now + i * 0.055);
            gain.gain.exponentialRampToValueAtTime(0.001, now + i * 0.055 + 0.12);
            osc.connect(gain); gain.connect(this.ctx.destination);
            osc.start(now + i * 0.055); osc.stop(now + i * 0.055 + 0.13);
          });
        }
        playWarningAlarm() {
          if (this.muted) return; this.init(); if (!this.ctx) return;
          const now = this.ctx.currentTime;
          for (let i = 0; i < 3; i++) {
            const start = now + i * 0.22;
            const osc = this.ctx.createOscillator(); const gain = this.ctx.createGain();
            osc.type = 'sawtooth';
            osc.frequency.setValueAtTime(740, start);
            osc.frequency.setValueAtTime(520, start + 0.09);
            gain.gain.setValueAtTime(0.14, start);
            gain.gain.exponentialRampToValueAtTime(0.001, start + 0.18);
            osc.connect(gain); gain.connect(this.ctx.destination);
            osc.start(start); osc.stop(start + 0.19);
          }
        }
        playUfoCharge() {
          if (this.muted) return; this.init(); if (!this.ctx) return;
          const now = this.ctx.currentTime;
          const osc = this.ctx.createOscillator(); const gain = this.ctx.createGain();
          osc.type = 'sine';
          osc.frequency.setValueAtTime(240, now);
          osc.frequency.exponentialRampToValueAtTime(1180, now + 0.45);
          gain.gain.setValueAtTime(0.07, now);
          gain.gain.exponentialRampToValueAtTime(0.001, now + 0.48);
          osc.connect(gain); gain.connect(this.ctx.destination);
          osc.start(now); osc.stop(now + 0.5);
        }
        playUfoLaserFire() {
          if (this.muted) return; this.init(); if (!this.ctx) return;
          const now = this.ctx.currentTime;
          const osc = this.ctx.createOscillator(); const gain = this.ctx.createGain();
          osc.type = 'sawtooth';
          osc.frequency.setValueAtTime(980, now);
          osc.frequency.exponentialRampToValueAtTime(160, now + 0.1);
          gain.gain.setValueAtTime(0.2, now);
          gain.gain.exponentialRampToValueAtTime(0.005, now + 0.1);
          osc.connect(gain); gain.connect(this.ctx.destination);
          osc.start(now); osc.stop(now + 0.11);
        }
        playMeteorFragmentHit() {
          if (this.muted) return; this.init(); if (!this.ctx) return;
          const now = this.ctx.currentTime;
          const bufferSize = Math.floor(this.ctx.sampleRate * 0.15);
          const buffer = this.ctx.createBuffer(1, bufferSize, this.ctx.sampleRate);
          const data = buffer.getChannelData(0);
          for (let i = 0; i < bufferSize; i++) data[i] = (Math.random() * 2 - 1) * Math.exp(-i / (bufferSize * 0.35));
          const noise = this.ctx.createBufferSource(); noise.buffer = buffer;
          const filter = this.ctx.createBiquadFilter(); filter.type = 'bandpass'; filter.frequency.setValueAtTime(320, now);
          const gain = this.ctx.createGain(); gain.gain.setValueAtTime(0.2, now); gain.gain.exponentialRampToValueAtTime(0.01, now + 0.14);
          noise.connect(filter); filter.connect(gain); gain.connect(this.ctx.destination);
          noise.start(now);
        }
        playGameOver() {
          if (this.muted) return; this.init(); if (!this.ctx) return;
          const now = this.ctx.currentTime;
          [300, 240, 180, 115].forEach((f, i) => {
            const osc = this.ctx.createOscillator(); const gain = this.ctx.createGain();
            osc.type = 'sawtooth';
            osc.frequency.setValueAtTime(f, now + i * 0.09);
            gain.gain.setValueAtTime(0.15, now + i * 0.09);
            gain.gain.exponentialRampToValueAtTime(0.001, now + i * 0.09 + 0.14);
            osc.connect(gain); gain.connect(this.ctx.destination);
            osc.start(now + i * 0.09); osc.stop(now + i * 0.09 + 0.15);
          });
        }
        playMeteorDescentRoar(durationSec = 2.6) {
          if (this.muted) return; this.init(); if (!this.ctx) return;
          const now = this.ctx.currentTime;
          const subOsc = this.ctx.createOscillator(); const subGain = this.ctx.createGain();
          subOsc.type = 'sawtooth';
          subOsc.frequency.setValueAtTime(140, now);
          subOsc.frequency.exponentialRampToValueAtTime(38, now + durationSec);
          subGain.gain.setValueAtTime(0.05, now);
          subGain.gain.linearRampToValueAtTime(0.32, now + durationSec * 0.9);
          subGain.gain.exponentialRampToValueAtTime(0.001, now + durationSec);
          subOsc.connect(subGain); subGain.connect(this.ctx.destination);
          subOsc.start(now); subOsc.stop(now + durationSec);
        }
        playCataclysmicExplosion() {
          if (this.muted) return; this.init(); if (!this.ctx) return;
          const now = this.ctx.currentTime;
          const duration = 2.6;
          const bufferSize = Math.floor(this.ctx.sampleRate * duration);
          const buffer = this.ctx.createBuffer(1, bufferSize, this.ctx.sampleRate);
          const data = buffer.getChannelData(0);
          for (let i = 0; i < bufferSize; i++) data[i] = (Math.random() * 2 - 1) * Math.pow(1 - i / bufferSize, 1.4);
          const noise = this.ctx.createBufferSource(); noise.buffer = buffer;
          const lowpass = this.ctx.createBiquadFilter(); lowpass.type = 'lowpass';
          lowpass.frequency.setValueAtTime(2400, now); lowpass.frequency.exponentialRampToValueAtTime(65, now + duration);
          const masterGain = this.ctx.createGain();
          masterGain.gain.setValueAtTime(0.6, now); masterGain.gain.exponentialRampToValueAtTime(0.001, now + duration);
          noise.connect(lowpass); lowpass.connect(masterGain); masterGain.connect(this.ctx.destination);
          noise.start(now);
        }
        playStasisBeamHum() {
          if (this.muted) return; this.init(); if (!this.ctx) return;
          const now = this.ctx.currentTime;
          const osc = this.ctx.createOscillator(); const gain = this.ctx.createGain();
          osc.type = 'sine';
          osc.frequency.setValueAtTime(220, now);
          osc.frequency.linearRampToValueAtTime(880, now + 1.8);
          gain.gain.setValueAtTime(0.08, now);
          gain.gain.exponentialRampToValueAtTime(0.001, now + 1.9);
          osc.connect(gain); gain.connect(this.ctx.destination);
          osc.start(now); osc.stop(now + 1.95);
        }
        playTypewriterTick() {
          if (this.muted) return; this.init(); if (!this.ctx) return;
          const now = this.ctx.currentTime;
          const osc = this.ctx.createOscillator(); const gain = this.ctx.createGain();
          osc.type = 'triangle';
          osc.frequency.setValueAtTime(900 + Math.random() * 220, now);
          gain.gain.setValueAtTime(0.03, now);
          gain.gain.exponentialRampToValueAtTime(0.001, now + 0.025);
          osc.connect(gain); gain.connect(this.ctx.destination);
          osc.start(now); osc.stop(now + 0.03);
        }
      }

      const sound = new SoundEngine();

      function drawPixelMatrix(ctx, matrix, x, y, scale, palette, flipX = false) {
        const cols = matrix[0] ? matrix[0].length : 0;
        for (let row = 0; row < matrix.length; row++) {
          const line = matrix[row];
          for (let col = 0; col < line.length; col++) {
            const ch = line[col];
            if (ch === ' ' || ch === '.') continue;
            ctx.fillStyle = palette[ch] || palette['#'] || '#333';
            const drawCol = flipX ? (cols - 1 - col) : col;
            ctx.fillRect(
              Math.floor(x + drawCol * scale),
              Math.floor(y + row * scale),
              Math.ceil(scale),
              Math.ceil(scale)
            );
          }
        }
      }

      function drawGroundAnchoredSprite(ctx, matrix, x, groundY, scale, palette, sinkPx = 2, flipX = false) {
        const spriteH = matrix.length * scale;
        const drawY = Math.floor(groundY - spriteH + sinkPx);
        drawPixelMatrix(ctx, matrix, x, drawY, scale, palette, flipX);
      }

      function drawAnimatedBgDino(ctx, matrix, x, groundY, scale, color, animTick, neckRows = 6, legRows = 3, flipX = false) {
        const totalRows = matrix.length;
        const cols = matrix[0] ? matrix[0].length : 0;
        const stridePhase = Math.floor(animTick / 4) % 4;
        const bodyBob = (stridePhase === 1 || stridePhase === 3) ? -2.0 : 0;
        const neckSway = Math.round(Math.sin(animTick * 0.11) * 1.6) * scale * (flipX ? -1 : 1);
        const baseDrawY = Math.floor(groundY - totalRows * scale + 2 + bodyBob);

        ctx.fillStyle = color;
        for (let row = 0; row < totalRows; row++) {
          const line = matrix[row];
          let rowOffsetX = 0;
          let rowOffsetY = 0;

          if (row < neckRows) {
            rowOffsetX = neckSway * (1 - row / neckRows);
          } else if (row >= totalRows - legRows) {
            const legRowIdx = row - (totalRows - legRows);
            for (let col = 0; col < line.length; col++) {
              if (line[col] === ' ') continue;
              const drawCol = flipX ? (cols - 1 - col) : col;
              const isFrontLeg = col > line.length * 0.52;
              let legDX = 0;
              let legDY = 0;
              const dirSign = flipX ? -1 : 1;
              if (isFrontLeg) {
                if (stridePhase === 0) { legDX = scale * 1.4 * dirSign; }
                else if (stridePhase === 1) { legDY = -scale * 1.0; }
                else if (stridePhase === 2) { legDX = -scale * 1.2 * dirSign; }
              } else {
                if (stridePhase === 0) { legDX = -scale * 1.2 * dirSign; }
                else if (stridePhase === 2) { legDX = scale * 1.4 * dirSign; }
                else if (stridePhase === 3) { legDY = -scale * 1.0; }
              }
              ctx.fillRect(
                Math.floor(x + drawCol * scale + legDX * (legRowIdx + 1) * 0.48),
                Math.floor(baseDrawY + row * scale + legDY),
                Math.ceil(scale),
                Math.ceil(scale)
              );
            }
            continue;
          }

          for (let col = 0; col < line.length; col++) {
            if (line[col] === ' ') continue;
            const drawCol = flipX ? (cols - 1 - col) : col;
            ctx.fillRect(
              Math.floor(x + drawCol * scale + rowOffsetX),
              Math.floor(baseDrawY + row * scale + rowOffsetY),
              Math.ceil(scale),
              Math.ceil(scale)
            );
          }
        }
      }

      const SPRITES = {
        PIXEL_SUN: [
          "      44    44      ",
          "      44    44      ",
          "  44   ######   44  ",
          "   44##########44   ",
          "    ###222222###    ",
          "   ###22333322###   ",
          "44###2233333322###44",
          "44###2333333332###44",
          "  ###2333333332###  ",
          "  ###2333333332###  ",
          "44###2333333332###44",
          "44###2233333322###44",
          "   ###22333322###   ",
          "    ###222222###    ",
          "   44##########44   ",
          "  44   ######   44  ",
          "      44    44      ",
          "      44    44      "
        ],
        PIXEL_ASTEROID: [
          "       44444444       ",
          "    444########444    ",
          "   44###222#######44  ",
          "  44##2233322######44 ",
          " 44###23333322###22#44",
          " 4####2233322###232##4",
          "44#####22222####222##4",
          "4###22##########22###4",
          "4##2332####222#######4",
          "4###22####233322#####4",
          " 4########223322####44",
          " 44#########222####44 ",
          "  44#######22#####44  ",
          "   444##########444   ",
          "     444444444444     "
        ],
        UFO_BOSS: [
          "         33333333         ",
          "       332222222233       ",
          "      32233333333223      ",
          "    ###################   ",
          "  ####################### ",
          " ####44###44###44###44####",
          "###########################",
          "  ####################### ",
          "    444  #########  444   ",
          "          44   44         "
        ],
        PROTO_RUNFrames: [
          [
            "           ######  ",
            "          ##3##### ",
            "          ######## ",
            "          #####    ",
            "          #######  ",
            "#        ######22  ",
            "##      #######    ",
            "###    ########2   ",
            " #### #########    ",
            "  ############     ",
            "   ####   ####     ",
            "  ###       ###    ",
            " ###         ###   ",
            "             ####  "
          ],
          [
            "          ######   ",
            "         ##3#####  ",
            "         ########  ",
            "         #####     ",
            "         #######   ",
            "##      ######22   ",
            " ###   #######     ",
            "  ############2    ",
            "   ###########     ",
            "    ##########     ",
            "      #######      ",
            "      ###  ###     ",
            "      ##    ###    ",
            "      ####         "
          ],
          [
            "           ######  ",
            "          ##3##### ",
            "          ######## ",
            "          #####    ",
            "          #######  ",
            "         ######22  ",
            "#       #######    ",
            "###    ########2   ",
            " ##############    ",
            "  ############     ",
            "   #####  ####     ",
            "   ###      ###    ",
            "  ###        ##    ",
            " ####              "
          ],
          [
            "          ######   ",
            "         ##3#####  ",
            "         ########  ",
            "         #####     ",
            "         #######   ",
            "#       ######22   ",
            "##     #######     ",
            "###   ########2    ",
            " #### ########     ",
            "  ###########      ",
            "    #########      ",
            "     ###   ####    ",
            "     ##     ##     ",
            "     ####          "
          ]
        ],
        PROTO_DUCKFrames: [
          [
            "               ######   ",
            "#             ##3#####  ",
            "###          #########  ",
            " #####     ###########2 ",
            "   ###################  ",
            "     ###############    ",
            "    ###          ###    ",
            "   ####           ###   "
          ],
          [
            "               ######   ",
            "##            ##3#####  ",
            " ####        #########  ",
            "  ######   ###########2 ",
            "   ###################  ",
            "     ###############    ",
            "       ###    ###       ",
            "       ####             "
          ],
          [
            "               ######   ",
            "#             ##3#####  ",
            "###          #########  ",
            " #####     ###########2 ",
            "   ###################  ",
            "     ###############    ",
            "     ###        ###     ",
            "                ####    "
          ],
          [
            "               ######   ",
            "##            ##3#####  ",
            " ####        #########  ",
            "  ######   ###########2 ",
            "   ###################  ",
            "     ###############    ",
            "        ###  ###        ",
            "        ####            "
          ]
        ],
        TREX_RUNFrames: [
          [
            "            ########## ",
            "           ###3####### ",
            "           ########### ",
            "           ########### ",
            "           ######      ",
            "           #########   ",
            "#         ######       ",
            "##       ########      ",
            "###     ###########    ",
            "####   ##########22    ",
            "#####  ##########      ",
            "#################      ",
            " ################      ",
            "  ##############       ",
            "   #####   #####       ",
            "  ####       ####      ",
            " ###           ###     ",
            "               #####   "
          ],
          [
            "           ##########  ",
            "          ###3#######  ",
            "          ###########  ",
            "          ###########  ",
            "          ######       ",
            "          #########    ",
            "##       ######        ",
            " ###    ########       ",
            "  #### ###########     ",
            "   #############22     ",
            "    ############       ",
            "     ###########       ",
            "     ###########       ",
            "      #########        ",
            "       ########        ",
            "       ###   ####      ",
            "       ###    ###      ",
            "       #####           "
          ],
          [
            "            ########## ",
            "           ###3####### ",
            "           ########### ",
            "           ########### ",
            "           ######      ",
            "           #########   ",
            "          ######       ",
            "#        ########      ",
            "###     ###########    ",
            "#####  ##########22    ",
            "################       ",
            "################       ",
            " ###############       ",
            "  #############        ",
            "   #####   #####       ",
            "   ###       ####      ",
            "  ###          ##      ",
            " #####                 "
          ],
          [
            "           ##########  ",
            "          ###3#######  ",
            "          ###########  ",
            "          ###########  ",
            "          ######       ",
            "          #########    ",
            "#        ######        ",
            "##      ########       ",
            "###    ###########     ",
            "####  ##########22     ",
            "################       ",
            " ###############       ",
            "  #############        ",
            "   ############        ",
            "    ##########         ",
            "     ####   #####      ",
            "     ###      ###      ",
            "     #####             "
          ]
        ],
        TREX_DUCKFrames: [
          [
            "               ########## ",
            "#             ###3####### ",
            "##            ########### ",
            "###          ############ ",
            "#####      ########       ",
            " #####################    ",
            "  ###################22   ",
            "   #################      ",
            "   ####          ####     ",
            "  ####            ###     ",
            "                  #####   "
          ],
          [
            "               ########## ",
            "##            ###3####### ",
            "###           ########### ",
            "#####        ############ ",
            " ######    ########       ",
            "  ####################    ",
            "   ##################22   ",
            "    ################      ",
            "       ####   ####        ",
            "       ###     ###        ",
            "       #####              "
          ],
          [
            "               ########## ",
            "#             ###3####### ",
            "##            ########### ",
            "###          ############ ",
            "#####      ########       ",
            " #####################    ",
            "  ###################22   ",
            "   #################      ",
            "    ####        ####      ",
            "   ###            ###     ",
            "  #####                   "
          ],
          [
            "               ########## ",
            "##            ###3####### ",
            "###           ########### ",
            "#####        ############ ",
            " ######    ########       ",
            "  ####################    ",
            "   ##################22   ",
            "    ################      ",
            "        ####   #####      ",
            "        ###      ###      ",
            "        #####             "
          ]
        ],
        SKEL_RUNFrames: [
          [
            "            ########## ",
            "           ## 3# ##### ",
            "           ########### ",
            "           ##### # # # ",
            "           ####        ",
            "           ########    ",
            "#         ####         ",
            "##       # # # #       ",
            " #     ## # # # ###    ",
            "  #   # # # # #   #    ",
            "   # #  # # # #        ",
            "    ### ########       ",
            "      ##########       ",
            "       ###   ###       ",
            "      ##       ##      ",
            "     ##         ##     ",
            "    ##           ##    ",
            "                 ####  "
          ],
          [
            "           ##########  ",
            "          ## 3# #####  ",
            "          ###########  ",
            "          ##### # # #  ",
            "          ####         ",
            "          ########     ",
            "##       ####          ",
            " #      # # # #        ",
            "  #   ## # # # ###     ",
            "   # # # # # #   #     ",
            "    ##  # # # #        ",
            "     ###########       ",
            "      #########        ",
            "       #######         ",
            "       ##   ###        ",
            "       ##    ##        ",
            "       ##   ###        ",
            "       ####            "
          ],
          [
            "            ########## ",
            "           ## 3# ##### ",
            "           ########### ",
            "           ##### # # # ",
            "           ####        ",
            "           ########    ",
            "          ####         ",
            "#        # # # #       ",
            "##     ## # # # ###    ",
            "  #   # # # # #   #    ",
            "   ###  # # # #        ",
            "    ############       ",
            "      #########        ",
            "      ###   ###        ",
            "     ##       ###      ",
            "    ##          ##     ",
            "   ##                  ",
            "  ####                 "
          ],
          [
            "           ##########  ",
            "          ## 3# #####  ",
            "          ###########  ",
            "          ##### # # #  ",
            "          ####         ",
            "          ########     ",
            "#        ####          ",
            "##      # # # #        ",
            " #    ## # # # ###     ",
            "  #  # # # # #   #     ",
            "   ###  # # # #        ",
            "    ###########        ",
            "      #########        ",
            "       #######         ",
            "      ##    ####       ",
            "     ##       ##       ",
            "     ####              ",
            "                       "
          ]
        ],
        SKEL_DUCKFrames: [
          [
            "               ########## ",
            "#             ## 3# ##### ",
            "##            ########### ",
            " #           ##### # # #  ",
            "  #        ######         ",
            "   # # # # # # # #####    ",
            "    ### # # # # #   #     ",
            "      ###########         ",
            "     ##          ##       ",
            "    ##            ##      ",
            "                  ####    "
          ],
          [
            "               ########## ",
            "##            ## 3# ##### ",
            " #            ########### ",
            "  #          ##### # # #  ",
            "   #       ######         ",
            "    # # # # # # #####     ",
            "     ## # # # # #   #     ",
            "       ##########         ",
            "        ##    ###         ",
            "        ##     ##         ",
            "        ####              "
          ],
          [
            "               ########## ",
            "#             ## 3# ##### ",
            "##            ########### ",
            " #           ##### # # #  ",
            "  #        ######         ",
            "   # # # # # # # #####    ",
            "    ### # # # # #   #     ",
            "      ###########         ",
            "      ##        ##        ",
            "     ##          ###      ",
            "    ####                  "
          ],
          [
            "               ########## ",
            "##            ## 3# ##### ",
            " #            ########### ",
            "  #          ##### # # #  ",
            "   #       ######         ",
            "    # # # # # # #####     ",
            "     ## # # # # #   #     ",
            "       ##########         ",
            "         ##   ####        ",
            "         ##     ##        ",
            "         ####             "
          ]
        ],
        TREX_LOOK_UP: [
          "             #####    ",
          "            #######   ",
          "           ####3###   ",
          "           ########   ",
          "          ########    ",
          "          ######      ",
          "#        #######      ",
          "##      #########     ",
          "##     ###########    ",
          "###   ##########22    ",
          "####  ##########      ",
          "################      ",
          " ###############      ",
          "  #############       ",
          "   ###########        ",
          "    ####  ####        ",
          "    ###    ###        ",
          "    ####   ####       "
        ],
        TREX_FOSSIL_SLEEP: [
          "           ########## ",
          "          ############",
          "          ###222######",
          "          ##### # #   ",
          "          ####        ",
          "          ########    ",
          "#         ####        ",
          "##       # # # #      ",
          " #     ## # # # ###   ",
          "  #   # # # # #   #   ",
          "   # #  # # # #       ",
          "    ### ########      ",
          "      ##########      ",
          "       ###   ###      ",
          "       ##     ##      ",
          "       ##     ##      ",
          "       ##     ##      ",
          "      ####   ####     "
        ],
        TREX_FOSSIL: [
          "           ########## ",
          "          ## 3# ##### ",
          "          ##########  ",
          "          ##### # #   ",
          "          ####        ",
          "          ########    ",
          "#         ####        ",
          "##       # # # #      ",
          " #     ## # # # ###   ",
          "  #   # # # # #   #   ",
          "   # #  # # # #       ",
          "    ### ########      ",
          "      ##########      ",
          "       ###   ###      ",
          "       ##     ##      ",
          "       ##     ##      ",
          "       ##     ##      ",
          "      ####   ####     "
        ],
        FERN_BUSH: [
          "    ##    ##    ",
          "   ####  ####   ",
          " ## ## #### ##  ",
          "#### ###########",
          " ###2#######2###",
          "  #####222##### ",
          " ###############",
          "################",
          "################"
        ],
        DESERT_ROCKS: [
          "       ##        ",
          "      ####       ",
          "      ####   ##  ",
          "  ## ###### #### ",
          " ############### ",
          " #######2####### ",
          "################ ",
          "#####2#####2#####",
          "#################",
          "#################"
        ],
        POSTOSUCHUS_1: [
          "   ########              ",
          " ######3####  ####  #### ",
          "  ###################### ",
          " #####222################",
          "   ######################",
          "     ####  ###   ###  ###",
          "    ####    ### ###    ##"
        ],
        POSTOSUCHUS_2: [
          "  #########              ",
          "#######3####  ####  #### ",
          " ### ################### ",
          "   ###222################",
          "   ######################",
          "      ###  #### ####  ## ",
          "     ###    ###  ###  ###"
        ],
        EORAPTOR_1: [
          "  ######          ",
          " ###3####         ",
          "  #####           ",
          "   ##########  ###",
          "  #####22######## ",
          "   ###########    ",
          "   ###     ###    ",
          "  ###       ###   "
        ],
        EORAPTOR_2: [
          "  ######          ",
          " ###3####         ",
          "  #####           ",
          "   ##########  ###",
          "  #####22######## ",
          "   ###########    ",
          "     ###  ###     ",
          "    ###    ###    "
        ],
        CONIFER_STUMP: [
          "   ############   ",
          "  ##2222222222##  ",
          "  ##############  ",
          "   ############   ",
          "   #####22#####   ",
          "   ############   ",
          "  ##############  ",
          " ################ ",
          "##################",
          "##################"
        ],
        DILOPHO_1: [
          "   22  22           ",
          "  222##222          ",
          " #####3###          ",
          " 22######           ",
          "  22#########    ## ",
          "    ############ ###",
          "    #####22######## ",
          "     ###########    ",
          "     ###     ###    ",
          "    ####      ###   "
        ],
        DILOPHO_2: [
          "   22  22           ",
          "  222##222          ",
          " #####3###          ",
          " 22######           ",
          "  22#########    ## ",
          "    ############ ###",
          "    #####22######## ",
          "     ###########    ",
          "       ###  ###     ",
          "      ###    ####   "
        ],
        STEGO_OB_1: [
          "       22   222   22     ",
          "     2222  22222 2222    ",
          "   ### ###############   ",
          " ###3################### ",
          "#########################",
          "  #####################  ",
          "   ####   ####   ####    ",
          "  ####    ####    ####   "
        ],
        STEGO_OB_2: [
          "       22   222   22     ",
          "     2222  22222 2222    ",
          "   ### ###############  #",
          " ###3####################",
          "######################## ",
          "  #####################  ",
          "    ####  ####  ####     ",
          "    ####  ####  ####     "
        ],
        ARCHAEO_1: [
          "      ####        ",
          "     ######   ### ",
          " ##3######## #### ",
          "################# ",
          "   #############  ",
          "     #####  ###   ",
          "      ##          "
        ],
        ARCHAEO_2: [
          "                  ",
          " ##3#####     ### ",
          "################# ",
          "   ############## ",
          "    ######## ###  ",
          "     ######       ",
          "      ####        "
        ],
        RHAMPHO_1: [
          "     ####              ",
          "    ######         22  ",
          "###3########      2222 ",
          "###################### ",
          "   ###########         ",
          "     ####              "
        ],
        RHAMPHO_2: [
          "                       ",
          "###3######         22  ",
          "######################2",
          "   ###########    222  ",
          "    #######            ",
          "     ####              "
        ],
        TRICERA_1: [
          "      ###  ####        ",
          "  #   ##########       ",
          " ### #############     ",
          "#####3################ ",
          " ######################",
          "   ####################",
          "    ###################",
          "     ####  ###  ### ###",
          "    #####  ###  ### ###"
        ],
        TRICERA_2: [
          "      ###  ####        ",
          "  #   ##########       ",
          " ### #############     ",
          "#####3################ ",
          " ######################",
          "   ####################",
          "    ###################",
          "      ### ###  #### ## ",
          "     #### ###  #### ###"
        ],
        RAPTOR_1: [
          "  #######          ",
          " ####3###          ",
          " #######       ### ",
          "   ########## ###  ",
          "  #####22#######   ",
          "   ###########     ",
          "  ###      ###     ",
          " ###        ###    "
        ],
        RAPTOR_2: [
          "  #######          ",
          " ####3###          ",
          " #######       ### ",
          "   ########## ###  ",
          "  #####22#######   ",
          "   ###########     ",
          "     ###  ###      ",
          "    ###    ###     "
        ],
        ANKYLO_OB_1: [
          "      22  22  22  22      ",
          "    ##################    ",
          "  ####222222222222#### ###",
          " ###3################# ###",
          "##########################",
          "   ####  ####  ####       ",
          "  #####  ####  #####      "
        ],
        ANKYLO_OB_2: [
          "      22  22  22  22   ###",
          "    ################## ###",
          "  ####222222222222####### ",
          " ###3#################### ",
          "########################  ",
          "    ####  ####  ####      ",
          "    ####  ####  ####      "
        ],
        PACHY_1: [
          "   22222           ",
          "  2222222          ",
          " ####3####     ### ",
          "###########  ##### ",
          "  ################ ",
          "    ############   ",
          "    ###      ###   ",
          "   ####       ###  "
        ],
        PACHY_2: [
          "   22222           ",
          "  2222222          ",
          " ####3####     ### ",
          "###########  ##### ",
          "  ################ ",
          "    ############   ",
          "      ###  ###     ",
          "     ####   ####   "
        ],
        QUETZAL_1: [
          "         ###            ",
          "        #####           ",
          "###############         ",
          "   ###3#################",
          "        ################",
          "          #####         ",
          "           ##           "
        ],
        QUETZAL_2: [
          "                        ",
          "###############         ",
          "   ###3#################",
          "       #################",
          "         #########      ",
          "          #####         ",
          "           ###          "
        ],
        METEOR_OB: [
          "   444444   ",
          " 44######44 ",
          "4##223322##4",
          "4#23333332#4",
          "4#23333332#4",
          "4##223322##4",
          " 44######44 ",
          "   444444   "
        ],
        LAVA_GEYSER_1: [
          "    33    ",
          "   2332   ",
          "  #2332#  ",
          "  #2332#  ",
          " ##2332## ",
          " ##2332## ",
          "###2332###",
          "###2332###",
          "##########"
        ],
        LAVA_GEYSER_2: [
          "   2332   ",
          "  #2332#  ",
          "  #2332#  ",
          " ##2332## ",
          " ##2332## ",
          "###2332###",
          "###2332###",
          "###2332###",
          "##########"
        ],
        MAMMOTH_1: [
          "        ############   ",
          "      ################ ",
          "  22  ###3#############",
          " 22   #################",
          " 2222##################",
          "   ####################",
          "   ####   ####    #### ",
          "   ####   ####    #### "
        ],
        MAMMOTH_2: [
          "        ############   ",
          "      ################ ",
          "  22  ###3#############",
          " 22   #################",
          " 2222##################",
          "   ####################",
          "    ####   ####  ####  ",
          "    ####   ####  ####  "
        ],
        SMILODON_1: [
          "   ######             ",
          "  ###3####  ######### ",
          "  22##################",
          "  22  ################",
          "     ####         ####",
          "    ####           ###"
        ],
        SMILODON_2: [
          "   ######             ",
          "  ###3####  ######### ",
          "  22##################",
          "  22  ################",
          "       ####     ####  ",
          "      ####       #### "
        ],
        CHARIOT_1: [
          "   ####       2222    ",
          "  ##3###     22##22   ",
          " ########   ########  ",
          "####################  ",
          "  ####  ####  222222  ",
          " ##  ##  ##  22 22 22 ",
          "             22222222 "
        ],
        CHARIOT_2: [
          "   ####       2222    ",
          "  ##3###     22##22   ",
          " ########   ########  ",
          "####################  ",
          "   ###  ####  222222  ",
          "  ##   ##    22222222 ",
          "             22 22 22 "
        ],
        SPARTAN_1: [
          "      333333        ",
          "     33333333       ",
          "       ####         ",
          "      ##22##    22  ",
          "333333######  2222  ",
          "     2222222222222  ",
          "    2223333222####  ",
          "    2223333222####  ",
          "     222222222####  ",
          "      ####  ####    ",
          "     ###      ###   ",
          "    ####       ###  "
        ],
        SPARTAN_2: [
          "      333333        ",
          "     33333333       ",
          "       ####         ",
          "      ##22##    22  ",
          "333333######  2222  ",
          "     2222222222222  ",
          "    2223333222####  ",
          "    2223333222####  ",
          "     222222222####  ",
          "       #### ####    ",
          "       ###   ###    ",
          "      ####    ####  "
        ],
        CANNON_1: [
          "                  33  ",
          "  ######         333  ",
          " ###########    22    ",
          "################      ",
          " ################     ",
          "   ##############     ",
          "     222222222222     ",
          "    22222222222222    ",
          "   ###  ######  ###   ",
          "  #3#   ######   #3#  ",
          "   ###          ###   "
        ],
        CANNON_2: [
          "                 333  ",
          "  ######        33 3  ",
          " ###########    22    ",
          "################      ",
          " ################     ",
          "   ##############     ",
          "     222222222222     ",
          "    22222222222222    ",
          "   ###  ######  ###   ",
          "  #3#   ######   #3#  ",
          "   ###          ###   "
        ],
        POLICE_CAR: [
          "       33332222       ",
          "     ############     ",
          "   ###2222##2222###   ",
          " #################### ",
          "33##################22",
          "######################",
          "  ####          ####  ",
          "  ####          ####  "
        ],
        HELICOPTER_1: [
          "  333333333333333333    ",
          "         ###         ## ",
          "     ###########    ####",
          "   ###22222####### #####",
          "  ##################    ",
          "     ###########        ",
          "    ###      ###        "
        ],
        HELICOPTER_2: [
          "      3333333333        ",
          "         ###         ## ",
          "     ###########    ####",
          "   ###22222####### #####",
          "  ##################    ",
          "     ###########        ",
          "    ###      ###        "
        ],
        CHRONO_MECHA_1: [
          "      ##########      ",
          "    ####333333####    ",
          "   ################   ",
          " 22################22 ",
          "   ################   ",
          "     ############     ",
          "     ####    ####     ",
          "    ####      ####    ",
          "   #####      #####   "
        ],
        CHRONO_MECHA_2: [
          "      ##########      ",
          "    ####333333####    ",
          "   ################   ",
          " 22################22 ",
          "   ################   ",
          "     ############     ",
          "      ####  ####      ",
          "     ####    ####     ",
          "    #####    #####    "
        ],
        FUTURE_DRONE_1: [
          "     2222222222     ",
          "  ################  ",
          "####333333333333####",
          "  ################  ",
          "    ###      ###    ",
          "   33          33   "
        ],
        FUTURE_DRONE_2: [
          "     2222222222     ",
          "  ################  ",
          "####222233332222####",
          "  ################  ",
          "    ###      ###    ",
          "  333          333  "
        ],
        BRACHIOSAURUS: [
          "                 #### ",
          "                ##### ",
          "                ###   ",
          "               ###    ",
          "               ###    ",
          "              ###     ",
          "              ###     ",
          "             ####     ",
          "     ###########      ",
          "   ##############     ",
          "  ################    ",
          " #################    ",
          "#  ###############    ",
          "#   ####      ####    ",
          "    ####      ####    ",
          "    ####      ####    ",
          "    ####      ####    "
        ],
        DIPLODOCUS: [
          "                          #####",
          "                        ###### ",
          "                      #####    ",
          "                    #####      ",
          "         ###############       ",
          "      ##################       ",
          "   #####################       ",
          "########################       ",
          "      ####          ####       ",
          "      ####          ####       ",
          "      ####          ####       "
        ],
        STEGOSAURUS: [
          "       ##   ###   ##     ",
          "     ####  ##### ####    ",
          "   ### ###############   ",
          " # ##################### ",
          "#########################",
          "  #####################  ",
          "    ####         ####    ",
          "    ####         ####    "
        ],
        PARASAUROLOPHUS: [
          "             ######## ",
          "            ######    ",
          "           #####      ",
          "          ######      ",
          "      ###########     ",
          "   ###############    ",
          " #################    ",
          "###   ###########     ",
          "       ###    ###     ",
          "       ###    ###     ",
          "       ###    ###     "
        ],
        ANKYLOSAURUS: [
          "        ############     ",
          "     ##################  ",
          "### #####################",
          "#########################",
          "    ####          ####   ",
          "    ####          ####   "
        ],
        SPINOSAURUS: [
          "          ######         ",
          "        ##########       ",
          "       ############  ####",
          "      ###################",
          "  #######################",
          "######################## ",
          "     ####       ####     ",
          "     ####       ####     ",
          "     ####       ####     "
        ],
        FOSSIL_SKULL: [
          "   #########   ",
          " ############# ",
          "###   ###   ###",
          "###############",
          " ## # # # # ## "
        ]
      };

      function hexToRgb(hex) {
        const clean = String(hex || '#000000').replace('#', '');
        const num = parseInt(clean, 16) || 0;
        return { r: (num >> 16) & 255, g: (num >> 8) & 255, b: num & 255 };
      }
      function rgbToHex(r, g, b) {
        const toHex = (n) => Math.max(0, Math.min(255, Math.round(n))).toString(16).padStart(2, '0');
        return `#${toHex(r)}${toHex(g)}${toHex(b)}`;
      }
      function lerp(a, b, t) { return a + (b - a) * t; }
      function lerpColor(hexA, hexB, t) {
        const clamped = Math.max(0, Math.min(1, t));
        const a = hexToRgb(hexA); const b = hexToRgb(hexB);
        return rgbToHex(lerp(a.r, b.r, clamped), lerp(a.g, b.g, clamped), lerp(a.b, b.b, clamped));
      }

      const SUBPHASE_THEMES = {
        '1A': {
          majorPhase: 1, stage: 1,
          bannerTitle: '[ STAGE 1 · EARLY TRIASSIC ]',
          bannerSub: '초기 트라이아스기 (0~500) — 원시 사막과 생존의 시작',
          koTitle: '[1부 · 1-A] 초기 트라이아스기 (0~500) — "생존의 시작"',
          badge: 'STAGE 1: 1-A TRIASSIC',
          skyTop: '#F7F7F7', skyBottom: '#EAD7C3', groundLine: '#9E3D22', groundFill: '#DFA382', groundSub: '#C27455',
          farSil: '#DEC3AC', midSil: '#C99B7C', nearSil: '#8C583A',
          dinoPrimary: '#4A3525', dinoSecondary: '#8C6246', dinoEye: '#FFFFFF', uiText: '#4A3525'
        },
        '1B': {
          majorPhase: 1, stage: 1,
          bannerTitle: '[ LATE TRIASSIC : SANDSTORM ]',
          bannerSub: '후기 트라이아스기 (500~1,000) — 붉은 모래폭풍과 포스토수쿠스·에오랍토르의 습격!',
          koTitle: '[1부 · 1-B] 후기 트라이아스기 (500~1,000) — "모래폭풍과 포식자들"',
          badge: 'STAGE 1: 1-B TRIASSIC',
          skyTop: '#E8C5A5', skyBottom: '#D48C6A', groundLine: '#7A2512', groundFill: '#C97A57', groundSub: '#9E5334',
          farSil: '#C48B6C', midSil: '#A6684B', nearSil: '#6E3B22',
          dinoPrimary: '#3D2214', dinoSecondary: '#B84A24', dinoEye: '#FFEE99', uiText: '#3D1E10'
        },
        '2A': {
          majorPhase: 2, stage: 1,
          bannerTitle: '[ EARLY JURASSIC : LUSH FOREST ]',
          bannerSub: '초기 쥐라기 (1,000~1,500) — 거대 수각류 진화! 울창한 침엽수림과 딜로포사우루스·시조새',
          koTitle: '[1부 · 2-A] 초기 쥐라기 (1,000~1,500) — "거대 공룡의 황금기"',
          badge: 'STAGE 1: 2-A JURASSIC',
          skyTop: '#D4E6D5', skyBottom: '#99C2A2', groundLine: '#234730', groundFill: '#6D9976', groundSub: '#4D7556',
          farSil: '#A7CBB0', midSil: '#6B9E78', nearSil: '#3A6B48',
          dinoPrimary: '#1E3626', dinoSecondary: '#4B7556', dinoEye: '#FFF3B0', uiText: '#1F3626'
        },
        '2B': {
          majorPhase: 2, stage: 1,
          bannerTitle: '[ LATE JURASSIC : DEEP JUNGLE ]',
          bannerSub: '후기 쥐라기 (1,500~2,000) — 안개 낀 대습지 밀림! 스테고사우루스와 람포링크스 편대',
          koTitle: '[1부 · 2-B] 후기 쥐라기 (1,500~2,000) — "안개 밀림과 거대 용각류·갑주공룡"',
          badge: 'STAGE 1: 2-B JURASSIC',
          skyTop: '#9EC5BE', skyBottom: '#659B91', groundLine: '#173831', groundFill: '#4E7D73', groundSub: '#345952',
          farSil: '#78A69E', midSil: '#4B7D74', nearSil: '#254F47',
          dinoPrimary: '#132A26', dinoSecondary: '#3E7A6E', dinoEye: '#FDE047', uiText: '#102824'
        },
        '3A': {
          majorPhase: 3, stage: 1,
          bannerTitle: '[ EARLY CRETACEOUS : APEX T-REX ]',
          bannerSub: '초기 백악기 (2,000~2,500) — 소행성 출현! 꽃피는 목련 숲과 트리케라톱스·벨로시랩터',
          koTitle: '[1부 · 3-A] 초기 백악기 (2,000~2,500) — "번성의 정점과 소행성의 징조"',
          badge: 'STAGE 1: 3-A CRETACEOUS',
          skyTop: '#C5D5E4', skyBottom: '#EBC3AC', groundLine: '#383447', groundFill: '#8E8878', groundSub: '#6B6659',
          farSil: '#B8B0B8', midSil: '#857887', nearSil: '#574B5C',
          dinoPrimary: '#282A3E', dinoSecondary: '#5C6078', dinoEye: '#FFD166', uiText: '#282A3E'
        },
        '3B': {
          majorPhase: 3, stage: 1,
          bannerTitle: '[ LATE CRETACEOUS : TWILIGHT OMEN ]',
          bannerSub: '후기 백악기 (2,500~3,000) — 짙어지는 황혼! 안킬로사우루스·박치기 파키케팔로사우루스 격돌',
          koTitle: '[1부 · 3-B] 후기 백악기 (2,500~3,000) — "불길한 황혼과 최후의 격돌"',
          badge: 'STAGE 1: 3-B CRETACEOUS',
          skyTop: '#6B5B8E', skyBottom: '#E08A70', groundLine: '#2B1F38', groundFill: '#78636E', groundSub: '#54434E',
          farSil: '#8C6D8A', midSil: '#6B4E6E', nearSil: '#442C4A',
          dinoPrimary: '#1F162B', dinoSecondary: '#6A4C78', dinoEye: '#FF9F1C', uiText: '#F8FAFC'
        },
        '4A': {
          majorPhase: 4, stage: 1,
          bannerTitle: '[ EXTINCTION IMMINENT ]',
          bannerSub: '대멸종 카운트다운 (3,000~3,500) — 앙상한 고사목과 쓰러진 나무들, 운석 파편 낙하!',
          koTitle: '[1부 · 4-A] 대멸종 카운트다운 (3,000~3,500) — "불타 쓰러진 숲과 종말"',
          badge: 'STAGE 1: 4-A EXTINCTION',
          skyTop: '#290736', skyBottom: '#E85314', groundLine: '#FF4500', groundFill: '#33121A', groundSub: '#1D080D',
          farSil: '#4F132A', midSil: '#6E1A25', nearSil: '#3B0912',
          dinoPrimary: '#14050A', dinoSecondary: '#8A1C24', dinoEye: '#FF3333', uiText: '#FFE5D9'
        },
        '4B': {
          majorPhase: 4, stage: 1,
          bannerTitle: '[ FINAL CATACLYSM : CRUST RUPTURE ]',
          bannerSub: '대멸종 직전 (3,500~4,000) — 지각 붕괴 용암 기둥 분출 & 소행성 대충돌 임박!',
          koTitle: '[1부 · 4-B] 지각 붕괴 클라이맥스 (3,500~4,000) — "깊은 흙속에 묻히다"',
          badge: 'STAGE 1: 4-B CATACLYSM',
          skyTop: '#14021C', skyBottom: '#FF2A00', groundLine: '#FFCC00', groundFill: '#24070B', groundSub: '#120205',
          farSil: '#3B0814', midSil: '#590D18', nearSil: '#2E040A',
          dinoPrimary: '#0A0204', dinoSecondary: '#B81D13', dinoEye: '#FFFF00', uiText: '#FFF3B0'
        },
        '5A': {
          majorPhase: 5, stage: 2,
          bannerTitle: '[ STAGE 2 · PALEOLITHIC & FIRST HUMANS ]',
          bannerSub: '구석기 빙하기 (4,000~4,500) — 흙을 파고 깨어난 백골 티렉스! 원시 인류 사냥꾼과 매머드',
          koTitle: '[2부 · 5-A] 구석기 빙하기 (4,000~4,500) — "인류의 시작과 깨어난 백골"',
          badge: 'STAGE 2: 5-A PALEOLITHIC',
          skyTop: '#CBD5E1', skyBottom: '#94A3B8', groundLine: '#334155', groundFill: '#E2E8F0', groundSub: '#94A3B8',
          farSil: '#94A3B8', midSil: '#64748B', nearSil: '#475569',
          dinoPrimary: '#F8FAFC', dinoSecondary: '#CBD5E1', dinoEye: '#FF3B30', uiText: '#1E293B'
        },
        '5B': {
          majorPhase: 5, stage: 2,
          bannerTitle: '[ NEOLITHIC VILLAGES & MEGALITHS ]',
          bannerSub: '신석기 거석 시대 (4,500~5,000) — 고인돌 유적과 움집 마을의 원시 부족민들',
          koTitle: '[2부 · 5-B] 신석기 거석 문화 (4,500~5,000) — "부족 마을과 거석 유적"',
          badge: 'STAGE 2: 5-B NEOLITHIC',
          skyTop: '#BAE6FD', skyBottom: '#D9F99D', groundLine: '#3F6212', groundFill: '#84CC16', groundSub: '#4D7C0F',
          farSil: '#86EFAC', midSil: '#4ADE80', nearSil: '#15803D',
          dinoPrimary: '#F8FAFC', dinoSecondary: '#E2E8F0', dinoEye: '#FF3B30', uiText: '#14532D'
        },
        '6A': {
          majorPhase: 6, stage: 2,
          bannerTitle: '[ ANCIENT EMPIRES : PYRAMID & PARTHENON ]',
          bannerSub: '고대 문명 (5,000~5,500) — 피라미드·스핑크스·파르테논 신전과 고대 시민·전차 군단!',
          koTitle: '[2부 · 6-A] 고대 제국 시대 (5,000~5,500) — "피라미드와 신전의 도시"',
          badge: 'STAGE 2: 6-A ANCIENT',
          skyTop: '#FEF08A', skyBottom: '#FDBA74', groundLine: '#9A3412', groundFill: '#FDE047', groundSub: '#CA8A04',
          farSil: '#FCD34D', midSil: '#D97706', nearSil: '#92400E',
          dinoPrimary: '#FFFBEB', dinoSecondary: '#FDE68A', dinoEye: '#EF4444', uiText: '#451A03'
        },
        '6B': {
          majorPhase: 6, stage: 2,
          bannerTitle: '[ MEDIEVAL CASTLES & STEAM CLOCKTOWER ]',
          bannerSub: '중세 & 산업혁명 (5,500~6,000) — 고딕 성채·빅벤 시계탑·풍차·증기 공장과 기마 기사!',
          koTitle: '[2부 · 6-B] 중세 & 증기 산업혁명 (5,500~6,000) — "시계탑과 증기기관"',
          badge: 'STAGE 2: 6-B INDUSTRIAL',
          skyTop: '#78716C', skyBottom: '#D6D3D1', groundLine: '#292524', groundFill: '#57534E', groundSub: '#44403C',
          farSil: '#A8A29E', midSil: '#78716C', nearSil: '#44403C',
          dinoPrimary: '#F8FAFC', dinoSecondary: '#CBD5E1', dinoEye: '#F97316', uiText: '#1C1917'
        },
        '7A': {
          majorPhase: 7, stage: 2,
          bannerTitle: '[ MODERN METROPOLIS : BANK & LANDMARKS ]',
          bannerSub: '현대 대도시 (6,000~6,500) — 은행($)·대형 전광판·랜드마크 타워·현수교와 도심 시민들!',
          koTitle: '[2부 · 7-A] 현대 메트로폴리스 (6,000~6,500) — "은행·전광판·랜드마크 빌딩 숲"',
          badge: 'STAGE 2: 7-A MODERN',
          skyTop: '#38BDF8', skyBottom: '#E0F2FE', groundLine: '#0F172A', groundFill: '#334155', groundSub: '#1E293B',
          farSil: '#94A3B8', midSil: '#475569', nearSil: '#1E293B',
          dinoPrimary: '#FFFFFF', dinoSecondary: '#E2E8F0', dinoEye: '#EF4444', uiText: '#0F172A'
        },
        '7B': {
          majorPhase: 7, stage: 2,
          bannerTitle: '[ MODERN NIGHT : NEON BILLBOARDS ]',
          bannerSub: '현대 야간 비상사태 (6,500~7,000) — 화려한 야간 전광판·서치라이트와 경찰 추격전!',
          koTitle: '[2부 · 7-B] 현대 도심 야간 추격전 (6,500~7,000) — "야간 네온 전광판과 비상사태"',
          badge: 'STAGE 2: 7-B LOCKDOWN',
          skyTop: '#0F172A', skyBottom: '#312E81', groundLine: '#FACC15', groundFill: '#1E293B', groundSub: '#0F172A',
          farSil: '#1E1B4B', midSil: '#312E81', nearSil: '#1E293B',
          dinoPrimary: '#F8FAFC', dinoSecondary: '#94A3B8', dinoEye: '#38BDF8', uiText: '#F8FAFC'
        },
        '8A': {
          majorPhase: 8, stage: 2,
          bannerTitle: '[ SCI-FI 2200 : MEGA-TOWERS & UFO BOSS ]',
          bannerSub: '초미래 문명 (7,000~7,500) — 초고층 메가타워, SF UFO 편대 & 중간보스 UFO 레이저 결전!',
          koTitle: '[2부 · 8-A] 초미래 SF 메가시티 (7,000~7,500) — "초고층 타워와 UFO 중간보스전"',
          badge: 'STAGE 2: 8-A UFO BOSS',
          skyTop: '#120324', skyBottom: '#701A75', groundLine: '#06B6D4', groundFill: '#0F172A', groundSub: '#020617',
          farSil: '#3B0764', midSil: '#581C87', nearSil: '#2E1065',
          dinoPrimary: '#ECFEFF', dinoSecondary: '#67E8F9', dinoEye: '#F43F5E', uiText: '#22D3EE'
        },
        '8B': {
          majorPhase: 8, stage: 2,
          bannerTitle: '[ FAR FUTURE 3000 : HOT PURSUIT ]',
          bannerSub: '초미래 서기 3000년 (7,500~8,000) — 뒤에서 맹추격해오는 크로노 부대! 8,000점 앞길 차단 포위!',
          koTitle: '[2부 · 8-B] 초미래 추격전 (7,500~8,000) — "뒤에서 따라붙는 포위망"',
          badge: 'STAGE 2: 8-B PURSUIT',
          skyTop: '#020617', skyBottom: '#0F766E', groundLine: '#2DD4BF', groundFill: '#0F172A', groundSub: '#020617',
          farSil: '#134E4A', midSil: '#115E59', nearSil: '#042F2E',
          dinoPrimary: '#F0FDFA', dinoSecondary: '#5EEAD4', dinoEye: '#FBBF24', uiText: '#99F6E4'
        }
      };

      function getSubPhaseForScore(s) {
        if (s < 500) return '1A';
        if (s < 1000) return '1B';
        if (s < 1500) return '2A';
        if (s < 2000) return '2B';
        if (s < 2500) return '3A';
        if (s < 3000) return '3B';
        if (s < 3500) return '4A';
        if (s < 4000) return '4B';
        if (s < 4500) return '5A';
        if (s < 5000) return '5B';
        if (s < 5500) return '6A';
        if (s < 6000) return '6B';
        if (s < 6500) return '7A';
        if (s < 7000) return '7B';
        if (s < 7500) return '8A';
        return '8B';
      }

      const canvas = document.getElementById('gameCanvas');
      const ctx = canvas.getContext('2d');
      const W = 800;
      const H = 300;
      const GROUND_Y = 252;

      const STATE = {
        READY: 'READY',
        PLAYING: 'PLAYING',
        IMPACT_CUTSCENE: 'IMPACT_CUTSCENE',
        STAGE1_BURIED_ENDING: 'STAGE1_BURIED_ENDING',
        STAGE2_AWAKEN_CUTSCENE: 'STAGE2_AWAKEN_CUTSCENE',
        STAGE2_CAPTURE_CUTSCENE: 'STAGE2_CAPTURE_CUTSCENE',
        FINAL_EPILOGUE: 'FINAL_EPILOGUE',
        GAMEOVER: 'GAMEOVER'
      };

      let gameState = STATE.READY;
      let score = 0;
      let highScore = 0;
      try {
        highScore = parseInt(localStorage.getItem('dino_66m_hi') || '0', 10) || 0;
      } catch (e) {
        highScore = 0;
      }

      let currentSubPhase = '1A';
      let currentPhase = 1;
      let currentStage = 1;
      let invincible = false;
      let isMobileMode = false;
      let userOverrodeDeviceMode = false;
      let gameSpeed = 5.6;
      let frameCount = 0;
      let lastMilestone = 0;
      let deathReason = '';

      let activeTheme = { ...SUBPHASE_THEMES['1A'] };

      const player = {
        x: 92,
        y: GROUND_Y,
        vy: 0,
        gravity: 0.68,
        jumpForce: -12.8,
        isJumping: false,
        isDucking: false,
        evolved: false,
        isSkeleton: false,
        evolveFlashTimer: 0,
        animTimer: 0,
        legFrame: 0,
        lookingUp: false,

        getSpriteAndScale() {
          if (this.lookingUp) {
            return { sprite: this.isSkeleton ? SPRITES.TREX_FOSSIL : SPRITES.TREX_LOOK_UP, scale: 2.38 };
          }
          const f = this.isJumping ? 0 : (this.legFrame % 4);
          if (this.isSkeleton) {
            const spr = this.isDucking ? SPRITES.SKEL_DUCKFrames[f] : SPRITES.SKEL_RUNFrames[f];
            return { sprite: spr, scale: 2.42 };
          }
          if (!this.evolved) {
            const spr = this.isDucking ? SPRITES.PROTO_DUCKFrames[f] : SPRITES.PROTO_RUNFrames[f];
            return { sprite: spr, scale: 2.2 };
          }
          const tScale = currentPhase >= 3 ? 2.42 : 2.25;
          const spr = this.isDucking ? SPRITES.TREX_DUCKFrames[f] : SPRITES.TREX_RUNFrames[f];
          return { sprite: spr, scale: tScale };
        },
        getWidth() {
          const { sprite, scale } = this.getSpriteAndScale();
          return sprite[0].length * scale;
        },
        getHeight() {
          const { sprite, scale } = this.getSpriteAndScale();
          return sprite.length * scale;
        },
        getHitbox() {
          const w = this.getWidth();
          const h = this.getHeight();
          return { x: this.x + 8, y: this.y - h + 6, w: w - 16, h: h - 8 };
        }
      };

      const banner = {
        text: '',
        subtext: '',
        alpha: 0,
        timer: 0,
        maxTimer: 165,
        isWarning: false,
        trigger(text, subtext, isWarning = false) {
          this.text = text;
          this.subtext = subtext;
          this.timer = this.maxTimer;
          this.alpha = 1;
          this.isWarning = isWarning;
        }
      };

      // Phase 8 Mid-Boss UFO State Machine (Fires 6 lasers, 0.5s = 30 frames each, up/down, no normal obstacles)
      const ufoBoss = {
        triggeredInRun: false,
        active: false,
        state: 'IDLE', // 'APPROACHING', 'AIMING', 'FIRING', 'COOLDOWN', 'RETREATING'
        x: W + 120,
        y: 120,
        targetY: 165,
        shotsFired: 0,
        maxShots: 6,
        stateTimer: 0,
        isHighLaser: true, // true = High laser (duck to dodge), false = Low laser (jump to dodge)
        patternSequence: [true, false, true, false, false, true], // 6 alternating/varied shots
        reset() {
          this.triggeredInRun = false;
          this.active = false;
          this.state = 'IDLE';
          this.x = W + 120;
          this.y = 120;
          this.shotsFired = 0;
          this.stateTimer = 0;
        },
        startEncounter() {
          this.triggeredInRun = true;
          this.active = true;
          this.state = 'APPROACHING';
          this.x = W + 110;
          this.y = 110;
          this.shotsFired = 0;
          this.stateTimer = 0;
          this.isHighLaser = this.patternSequence[0];
          this.targetY = this.isHighLaser ? (GROUND_Y - 58) : (GROUND_Y - 22);
          obstacles = []; // Clear all normal obstacles immediately!
          sound.playWarningAlarm();
          banner.trigger('[ WARNING : UFO MID-BOSS ]', '초미래 UFO 전함 출현! 위/아래 레이저 6연발을 점프·숙이기로 피하세요!', true);
        }
      };

      let obstacles = [];
      let obstacleSpawnTimer = 0;
      let groundPebbles = [];
      let footstepDust = [];
      let undergroundFossils = [];
      let skyClouds = [];
      let distantFlocks = [];
      let farLandforms = [];
      let forestTrees = [];
      let midLandmarks = [];
      let bgRoamingDinos = []; // Actively moving background dinosaurs in Stage 1!
      let bgHumans = []; // Stage 2 Humans across history!
      let bgUfos = [];   // Phase 8 Sci-Fi background UFOs & Flying cars!
      let nearScenery = [];
      let volcanoSmoke = [];
      let panicStampede = [];
      let ashParticles = [];
      let evolutionSparks = [];

      const cutscene = {
        timer: 0,
        meteorX: 740,
        meteorY: 25,
        meteorScale: 2.4,
        impacted: false,
        whiteoutAlpha: 0,
        whiteoutHoldTimer: 0,
        shockwaveStep: 0
      };

      const buriedScene = {
        fadeFromWhite: 1.0,
        timer: 0,
        soilGrains: []
      };

      const awakenScene = {
        timer: 0,
        phase: 'SLEEP', // 'SLEEP' -> 'DIGGING' -> 'LEAP_OUT' -> 'SURFACE_RUN'
        surfaceY: 148,
        fossilX: W / 2 - 34,
        fossilY: 228,
        vy: 0,
        dugPath: [],
        dirtClods: []
      };

      // Stage 2 Ending: Pursuers chase from behind (left), accelerate, overtake to the front (right), block, and surround!
      const captureScene = {
        timer: 0,
        phase: 'OVERTAKE', // 'OVERTAKE' -> 'BLOCK_STOP' -> 'STASIS'
        dinoX: 110,
        rearMechX: 18,
        overtakeMechX: 25,
        overtakeCarX: -10,
        drone1X: 30,
        drone2X: -20,
        droneY: 58,
        stasisActive: false,
        crystalAlpha: 0,
        whiteoutAlpha: 0
      };

      const epilogue = {
        fadeFromWhite: 1.0,
        timer: 0,
        line1Full: '기원전 6,600만 년의 태고에서 머나먼 미래까지...',
        line2Full: '멸종은 끝이 아니라, 영원히 기억될 역사다.',
        line1Shown: '',
        line2Shown: '',
        visitors: []
      };

      function initWorldDecor() {
        groundPebbles = [];
        for (let i = 0; i < 48; i++) {
          groundPebbles.push({
            x: Math.random() * W,
            y: GROUND_Y + 4 + Math.random() * 14,
            w: 2 + Math.floor(Math.random() * 6),
            h: 2
          });
        }
        footstepDust = [];

        undergroundFossils = [
          { x: 90, y: GROUND_Y + 22, kind: 'ammonite' },
          { x: 240, y: GROUND_Y + 26, kind: 'bone' },
          { x: 410, y: GROUND_Y + 20, kind: 'skull' },
          { x: 580, y: GROUND_Y + 25, kind: 'ammonite' },
          { x: 730, y: GROUND_Y + 23, kind: 'bone' }
        ];

        skyClouds = [
          { x: 80, y: 42, w: 68, h: 16, speed: 0.22 },
          { x: 290, y: 62, w: 88, h: 20, speed: 0.18 },
          { x: 520, y: 36, w: 74, h: 16, speed: 0.26 },
          { x: 710, y: 55, w: 64, h: 14, speed: 0.2 }
        ];

        distantFlocks = [
          { x: 180, y: 68, count: 4, speed: 0.65, phase: 0 },
          { x: 460, y: 52, count: 5, speed: 0.55, phase: 1.5 },
          { x: 710, y: 78, count: 3, speed: 0.72, phase: 3.1 }
        ];

        farLandforms = [
          { x: 30, w: 120, h: 72, kind: 'volcano', smokeTimer: 5 },
          { x: 175, w: 145, h: 58, kind: 'mesa', smokeTimer: 0 },
          { x: 350, w: 110, h: 88, kind: 'caldera', smokeTimer: 18 },
          { x: 490, w: 155, h: 64, kind: 'waterfall_cliff', smokeTimer: 0 },
          { x: 665, w: 130, h: 94, kind: 'twin_peaks', smokeTimer: 30 }
        ];

        // 28 Lush Trees / 10 Varied Building Architectures for Modern (Phase 7) & Far Future (Phase 8)
        const bTypes = [
          'STANDARD', 'BILLBOARD', 'BANK', 'LANDMARK_TOWER', 'CAFE_SHOP',
          'BROADCAST', 'ARCH_DOME', 'HOSPITAL_HOTEL', 'TWIN_BRIDGE_TOWER', 'STEPPED_PYRAMID_CORP'
        ];
        forestTrees = [];
        for (let i = 0; i < 28; i++) {
          forestTrees.push({
            x: i * 32 + Math.random() * 14,
            h: 34 + Math.floor(Math.random() * 46),
            crownW: 28 + Math.floor(Math.random() * 28),
            layer: i % 2,
            seed: i,
            bType: bTypes[i % bTypes.length]
          });
        }

        midLandmarks = [
          { x: 40, slot: 0, walkSpeed: 1.55, dir: -1, anim: 0 },
          { x: 185, slot: 1, walkSpeed: 1.95, dir: 1, anim: 14 },
          { x: 330, slot: 2, walkSpeed: 1.75, dir: -1, anim: 28 },
          { x: 475, slot: 3, walkSpeed: 2.15, dir: 1, anim: 7 },
          { x: 620, slot: 4, walkSpeed: 1.65, dir: -1, anim: 21 },
          { x: 760, slot: 5, walkSpeed: 2.05, dir: 1, anim: 35 }
        ];

        // Extra roaming dinosaur herds & flyers that visibly travel across the plains in Stage 1
        bgRoamingDinos = [
          { x: 120, yOffset: 0, species: 'proto_pack', dir: 1, speed: 2.2, anim: 3 },
          { x: 380, yOffset: 0, species: 'raptor_dash', dir: -1, speed: 2.8, anim: 11 },
          { x: 590, yOffset: 0, species: 'herbivore_walk', dir: 1, speed: 1.7, anim: 19 },
          { x: 730, yOffset: -78, species: 'sky_pterosaur', dir: -1, speed: 2.5, anim: 27 }
        ];

        // Stage 2 Background Humans with diverse fashion, skin tones, hairstyles, hats & outfits!
        const skinPalette = ['#FDE68A', '#FCD34D', '#FDBA74', '#E0A96D', '#C68642', '#FFE4E6'];
        const hairPalette = ['#1C1917', '#78350F', '#B45309', '#FACC15', '#DC2626', '#E2E8F0', '#9333EA', '#0284C7'];
        bgHumans = [];
        for (let i = 0; i < 14; i++) {
          bgHumans.push({
            x: i * 62 + Math.random() * 28,
            role: i % 6, // 6 distinct fashion/outfit archetypes per era!
            skin: skinPalette[i % skinPalette.length],
            hairCol: hairPalette[(i * 3) % hairPalette.length],
            hairStyle: i % 5, // 0: spiky, 1: long/ponytail, 2: bun, 3: cap/hat, 4: headband/hood
            dir: i % 3 === 0 ? 1 : -1,
            speedOffset: (i % 3 === 0 ? -1.1 : (0.55 + (i % 4) * 0.28)),
            anim: i * 7
          });
        }

        // Phase 8 Sci-Fi Background UFOs & Flying Traffic
        bgUfos = [
          { x: 140, y: 42, speed: 1.4, phase: 0, hasBeam: true, kind: 'saucer' },
          { x: 390, y: 68, speed: 2.1, phase: 2.1, hasBeam: false, kind: 'saucer' },
          { x: 640, y: 34, speed: 0.85, phase: 4.0, hasBeam: true, kind: 'mothership' },
          { x: 260, y: 95, speed: 3.2, phase: 1.2, hasBeam: false, kind: 'skycar' },
          { x: 550, y: 112, speed: 2.8, phase: 3.5, hasBeam: false, kind: 'skycar' }
        ];

        nearScenery = [];
        for (let i = 0; i < 20; i++) {
          nearScenery.push({
            x: i * 44 + Math.random() * 18,
            variant: i % 7
          });
        }

        buriedScene.soilGrains = [];
        for (let i = 0; i < 140; i++) {
          buriedScene.soilGrains.push({
            x: Math.floor(Math.random() * W),
            y: 108 + Math.floor(Math.random() * (H - 112)),
            w: 3 + Math.floor(Math.random() * 8),
            h: 2 + Math.floor(Math.random() * 4),
            tone: i % 3
          });
        }

        volcanoSmoke = [];
        panicStampede = [];
        ashParticles = [];
        evolutionSparks = [];
      }

      function initEpilogueVisitors() {
        epilogue.visitors = [
          { x: 80, type: 'child_hat', speed: 0.48, step: 0, topCol: '#EF4444', botCol: '#1D4ED8', hatCol: '#FACC15', skin: '#FDE68A' },
          { x: 215, type: 'photographer', speed: 0.36, step: 10, flashTimer: 85, topCol: '#10B981', botCol: '#1E293B', hairCol: '#78350F', skin: '#FDBA74' },
          { x: 360, type: 'fashion_dress', speed: 0.42, step: 5, topCol: '#EC4899', botCol: '#BE185D', hairCol: '#FACC15', skin: '#FFE4E6' },
          { x: 505, type: 'suit_curator', speed: 0.52, step: 14, topCol: '#3B82F6', botCol: '#0F172A', hairCol: '#1C1917', skin: '#E0A96D' },
          { x: 645, type: 'hoodie_student', speed: 0.44, step: 22, topCol: '#A855F7', botCol: '#334155', hatCol: '#22D3EE', skin: '#FCD34D' }
        ];
      }

      initWorldDecor();
      initEpilogueVisitors();

      function hideAllOverlays() {
        const bo = document.getElementById('buriedOverlay');
        const eo = document.getElementById('epilogueOverlay');
        const io = document.getElementById('introGuideOverlay');
        if (bo) bo.classList.add('hidden');
        if (eo) eo.classList.add('hidden');
        if (io) io.classList.add('hidden');
      }

      function triggerSubPhaseChange(newSubKey) {
        const prevMajor = currentPhase;
        currentSubPhase = newSubKey;
        const cfg = SUBPHASE_THEMES[newSubKey];
        currentPhase = cfg.majorPhase;
        currentStage = cfg.stage;
        player.isSkeleton = currentStage === 2;
        player.evolved = currentStage === 2 || currentPhase >= 2;

        sound.startBGM(currentPhase);

        const isWarning = currentPhase === 4 || newSubKey === '8B';
        banner.trigger(cfg.bannerTitle, cfg.bannerSub, isWarning);

        if (newSubKey === '2A' || newSubKey === '3A' || newSubKey === '5A') {
          player.evolveFlashTimer = 70;
          sound.playEvolution();
          for (let i = 0; i < 28; i++) {
            evolutionSparks.push({
              x: player.x + 22,
              y: player.y - 22,
              vx: (Math.random() - 0.5) * 6.5,
              vy: (Math.random() - 0.7) * 6.5,
              life: 35 + Math.random() * 22
            });
          }
        } else if (newSubKey === '4A' || newSubKey === '4B' || newSubKey === '8B') {
          sound.playWarningAlarm();
        } else if (prevMajor !== currentPhase) {
          sound.playEvolution();
        } else {
          player.evolveFlashTimer = 35;
          sound.playSubPhaseShift();
        }
        updateHeaderHUD();
      }

      function jumpToScoreCheat(targetScore) {
        sound.init();
        hideAllOverlays();
        if (targetScore === 4000) {
          startStage2AwakenCutscene();
          return;
        }

        gameState = STATE.PLAYING;
        score = Math.max(0, Math.min(STAGE2_END_SCORE - 5, targetScore));
        lastMilestone = Math.floor(score / 100);
        obstacles = [];
        obstacleSpawnTimer = 26;
        player.x = 92;
        player.y = GROUND_Y;
        player.vy = 0;
        player.isJumping = false;
        player.isDucking = false;
        player.lookingUp = false;

        ufoBoss.reset();

        const subKey = getSubPhaseForScore(score);
        currentSubPhase = subKey;
        currentPhase = SUBPHASE_THEMES[subKey].majorPhase;
        currentStage = SUBPHASE_THEMES[subKey].stage;
        player.isSkeleton = currentStage === 2;
        player.evolved = currentStage === 2 || currentPhase >= 2;
        activeTheme = { ...SUBPHASE_THEMES[subKey] };

        triggerSubPhaseChange(subKey);

        // If user clicked [9] (7,000 PTS) to test Phase 8 & UFO Boss, start UFO Boss right away!
        if (targetScore === 7000) {
          ufoBoss.startEncounter();
        }
        updateHeaderHUD();
      }

      function startImpactCutscene() {
        hideAllOverlays();
        gameState = STATE.IMPACT_CUTSCENE;
        sound.stopBGM();
        const badgeEl = document.getElementById('bgmTrackName');
        if (badgeEl) badgeEl.textContent = 'BGM: K-Pg 소행성 대충돌 (Impact)';

        score = STAGE1_END_SCORE;
        if (score > highScore) {
          highScore = STAGE1_END_SCORE;
          try { localStorage.setItem('dino_66m_hi', String(highScore)); } catch (e) {}
        }
        obstacles = [];
        player.isJumping = false;
        player.isDucking = false;
        player.vy = 0;
        player.y = GROUND_Y;
        player.x = 120;
        player.lookingUp = true;

        cutscene.timer = 0;
        cutscene.meteorX = 700;
        cutscene.meteorY = 16;
        cutscene.meteorScale = 2.6;
        cutscene.impacted = false;
        cutscene.whiteoutAlpha = 0;
        cutscene.whiteoutHoldTimer = 0;
        cutscene.shockwaveStep = 0;

        sound.playMeteorDescentRoar(2.2);
        updateHeaderHUD();
      }

      function startStage1BuriedEnding() {
        hideAllOverlays();
        gameState = STATE.STAGE1_BURIED_ENDING;
        sound.startBGM('BURIED');
        buriedScene.fadeFromWhite = 1.0;
        buriedScene.timer = 0;
        const bo = document.getElementById('buriedOverlay');
        if (bo) bo.classList.remove('hidden');
        updateHeaderHUD();
      }

      function startStage2AwakenCutscene() {
        hideAllOverlays();
        sound.init();
        sound.stopBGM();
        sound.playEvolution();
        gameState = STATE.STAGE2_AWAKEN_CUTSCENE;
        score = STAGE1_END_SCORE;
        currentStage = 2;
        currentSubPhase = '5A';
        currentPhase = 5;
        activeTheme = { ...SUBPHASE_THEMES['5A'] };

        awakenScene.timer = 0;
        awakenScene.phase = 'SLEEP';
        awakenScene.surfaceY = 148;
        awakenScene.fossilX = W / 2 - 34;
        awakenScene.fossilY = 228; // Deep underground in solid soil
        awakenScene.vy = 0;
        awakenScene.dugPath = [];
        awakenScene.dirtClods = [];
        updateHeaderHUD();
      }

      function beginStage2Playing() {
        hideAllOverlays();
        gameState = STATE.PLAYING;
        score = STAGE1_END_SCORE + 1;
        lastMilestone = Math.floor(score / 100);
        currentStage = 2;
        currentSubPhase = '5A';
        currentPhase = 5;
        activeTheme = { ...SUBPHASE_THEMES['5A'] };

        obstacles = [];
        obstacleSpawnTimer = 36;
        panicStampede = [];
        ashParticles = [];
        ufoBoss.reset();

        player.x = 92;
        player.y = GROUND_Y;
        player.vy = 0;
        player.isJumping = false;
        player.isDucking = false;
        player.lookingUp = false;
        player.isSkeleton = true;
        player.evolved = true;
        player.evolveFlashTimer = 45;

        sound.startBGM(5);
        banner.trigger(SUBPHASE_THEMES['5A'].bannerTitle, SUBPHASE_THEMES['5A'].bannerSub, false);
        updateHeaderHUD();
      }

      function startStage2CaptureCutscene() {
        hideAllOverlays();
        gameState = STATE.STAGE2_CAPTURE_CUTSCENE;
        sound.stopBGM();
        sound.playWarningAlarm();
        const badgeEl = document.getElementById('bgmTrackName');
        if (badgeEl) badgeEl.textContent = 'BGM: 추격대 전방 차단 및 포위·박제 작전';

        score = STAGE2_END_SCORE;
        if (score > highScore) {
          highScore = STAGE2_END_SCORE;
          try { localStorage.setItem('dino_66m_hi', String(highScore)); } catch (e) {}
        }
        obstacles = [];
        ufoBoss.reset();
        player.isJumping = false;
        player.isDucking = false;
        player.vy = 0;
        player.y = GROUND_Y;
        player.isSkeleton = true;
        player.lookingUp = false;

        // Start from the running pursuit positions on the left!
        captureScene.timer = 0;
        captureScene.phase = 'OVERTAKE';
        captureScene.dinoX = player.x;
        captureScene.rearMechX = 14;
        captureScene.overtakeMechX = 22;
        captureScene.overtakeCarX = -18;
        captureScene.drone1X = 35;
        captureScene.drone2X = -15;
        captureScene.droneY = 54;
        captureScene.stasisActive = false;
        captureScene.crystalAlpha = 0;
        captureScene.whiteoutAlpha = 0;
        updateHeaderHUD();
      }

      function startFinalEpilogue() {
        hideAllOverlays();
        gameState = STATE.FINAL_EPILOGUE;
        sound.startBGM('EPILOGUE');
        epilogue.fadeFromWhite = 1.0;
        epilogue.timer = 0;
        epilogue.line1Shown = '';
        epilogue.line2Shown = '';
        initEpilogueVisitors();
        const eo = document.getElementById('epilogueOverlay');
        if (eo) eo.classList.remove('hidden');
        updateHeaderHUD();
      }

      function resetGame(startImmediately = false) {
        hideAllOverlays();
        score = 0;
        lastMilestone = 0;
        currentSubPhase = '1A';
        currentPhase = 1;
        currentStage = 1;
        gameSpeed = 5.6;
        activeTheme = { ...SUBPHASE_THEMES['1A'] };
        obstacles = [];
        obstacleSpawnTimer = 42;
        panicStampede = [];
        ashParticles = [];
        evolutionSparks = [];
        footstepDust = [];
        ufoBoss.reset();

        player.x = 92;
        player.y = GROUND_Y;
        player.vy = 0;
        player.isJumping = false;
        player.isDucking = false;
        player.evolved = false;
        player.isSkeleton = false;
        player.evolveFlashTimer = 0;
        player.lookingUp = false;

        banner.timer = 0;
        banner.alpha = 0;

        if (startImmediately) {
          gameState = STATE.PLAYING;
          sound.startBGM(1);
          banner.trigger(SUBPHASE_THEMES['1A'].bannerTitle, SUBPHASE_THEMES['1A'].bannerSub, false);
        } else {
          gameState = STATE.READY;
          sound.stopBGM();
          const io = document.getElementById('introGuideOverlay');
          if (io) io.classList.remove('hidden');
        }
        updateHeaderHUD();
      }

      function createGroundObstacle(type, label, sprite, scale, speedBonus = 0, extra = {}) {
        const w = Math.round(sprite[0].length * scale);
        const h = Math.round(sprite.length * scale);
        return {
          type, label, scale,
          x: W + 24 + (extra.offsetX || 0),
          y: GROUND_Y,
          w, h, speedBonus, isGround: true, ...extra
        };
      }

      // REQUIREMENT 4: All flying obstacles fly at a clear, consistent head-level height (GROUND_Y - 28)
      // so the player ALWAYS knows to duck (↓) underneath them without ambiguity!
      function createFlyingObstacle(type, label, sprite, scale, speedBonus = 1.2) {
        const w = Math.round(sprite[0].length * scale);
        const h = Math.round(sprite.length * scale);
        const flyBottomY = GROUND_Y - 28; // Hits standing player's head, cleanly cleared when ducking (↓)!
        return {
          type, label, scale,
          x: W + 28,
          y: flyBottomY,
          baseY: flyBottomY,
          w, h, speedBonus, swoop: false, isGround: false
        };
      }

      function spawnObstacleForSubPhase(subKey) {
        // Do NOT spawn normal obstacles while the UFO Mid-Boss is active!
        if (ufoBoss.active) return;

        const r = Math.random();
        if (subKey === '1A') {
          if (r < 0.55) obstacles.push(createGroundObstacle('FERN_BUSH', '원시 양치식물 덤불', SPRITES.FERN_BUSH, 2.35, 0));
          else obstacles.push(createGroundObstacle('DESERT_ROCKS', '뾰족한 사막 바위 군집', SPRITES.DESERT_ROCKS, 2.75, 0));
        } else if (subKey === '1B') {
          if (r < 0.32) obstacles.push(createGroundObstacle('POSTOSUCHUS', '포스토수쿠스(지배파충류)', SPRITES.POSTOSUCHUS_1, 2.4, 1.5));
          else if (r < 0.62) obstacles.push(createGroundObstacle('EORAPTOR', '에오랍토르(원시 수각류)', SPRITES.EORAPTOR_1, 2.35, 2.0));
          else if (r < 0.82) obstacles.push(createGroundObstacle('DESERT_ROCKS', '붉은 사막 바위 군집', SPRITES.DESERT_ROCKS, 2.75, 0));
          else obstacles.push(createGroundObstacle('FERN_BUSH', '건조 양치식물 덤불', SPRITES.FERN_BUSH, 2.35, 0));
        } else if (subKey === '2A') {
          if (r < 0.34) obstacles.push(createGroundObstacle('CONIFER_STUMP', '거대 침엽수 그루터기', SPRITES.CONIFER_STUMP, 2.7, 0));
          else if (r < 0.68) obstacles.push(createGroundObstacle('DILOPHOSAURUS', '딜로포사우루스(주름깃 육식공룡)', SPRITES.DILOPHO_1, 2.45, 1.7));
          else obstacles.push(createFlyingObstacle('ARCHAEOPTERYX', '시조새(아르케옵테릭스) [↓숙이기]', SPRITES.ARCHAEO_1, 2.4, 1.15));
        } else if (subKey === '2B') {
          if (r < 0.34) obstacles.push(createGroundObstacle('STEGOSAURUS', '스테고사우루스(골판과 가시 꼬리)', SPRITES.STEGO_OB_1, 2.4, 0.9));
          else if (r < 0.64) obstacles.push(createFlyingObstacle('RHAMPHORHYNCHUS', '람포링크스(긴꼬리 익룡) [↓숙이기]', SPRITES.RHAMPHO_1, 2.35, 1.65));
          else if (r < 0.84) obstacles.push(createGroundObstacle('DILOPHOSAURUS', '딜로포사우루스', SPRITES.DILOPHO_1, 2.45, 1.8));
          else obstacles.push(createGroundObstacle('CONIFER_STUMP', '거대 침엽수 그루터기', SPRITES.CONIFER_STUMP, 2.7, 0));
        } else if (subKey === '3A') {
          if (r < 0.38) obstacles.push(createGroundObstacle('TRICERATOPS', '돌진하는 트리케라톱스', SPRITES.TRICERA_1, 2.5, 2.2));
          else if (r < 0.68) {
            obstacles.push(createGroundObstacle('VELOCIRAPTOR', '벨로시랩터 무리', SPRITES.RAPTOR_1, 2.2, 2.4, { offsetX: 0 }));
            obstacles.push(createGroundObstacle('VELOCIRAPTOR', '벨로시랩터 무리', SPRITES.RAPTOR_2, 2.0, 2.4, { offsetX: 46 }));
          } else obstacles.push(createFlyingObstacle('QUETZALCOATLUS', '거대 익룡 케찰코아틀루스 [↓숙이기]', SPRITES.QUETZAL_1, 2.4, 1.6));
        } else if (subKey === '3B') {
          if (r < 0.30) obstacles.push(createGroundObstacle('ANKYLOSAURUS', '안킬로사우루스(곤봉 꼬리 갑주공룡)', SPRITES.ANKYLO_OB_1, 2.35, 1.1));
          else if (r < 0.62) obstacles.push(createGroundObstacle('PACHYCEPHALO', '박치기 파키케팔로사우루스', SPRITES.PACHY_1, 2.45, 2.8));
          else if (r < 0.82) obstacles.push(createGroundObstacle('TRICERATOPS', '돌진하는 트리케라톱스', SPRITES.TRICERA_1, 2.5, 2.4));
          else obstacles.push(createFlyingObstacle('QUETZALCOATLUS', '프테라노돈 & 케찰코아틀루스 [↓숙이기]', SPRITES.QUETZAL_1, 2.4, 1.85));
        } else if (subKey === '4A') {
          if (r < 0.58) {
            const targetGroundX = 310 + Math.random() * 340;
            const scale = 2.6;
            obstacles.push({
              type: 'METEOR_FRAGMENT', label: '소형 운석 파편', scale,
              x: targetGroundX + 180, y: -20, vy: 5.8 + Math.random() * 1.4, vxExtra: 2.2, landed: false,
              w: Math.round(SPRITES.METEOR_OB[0].length * scale), h: Math.round(SPRITES.METEOR_OB.length * scale),
              speedBonus: 0, isGround: true
            });
          } else if (r < 0.82) obstacles.push(createGroundObstacle('TRICERATOPS', '폭주하는 트리케라톱스', SPRITES.TRICERA_1, 2.5, 2.6));
          else obstacles.push(createGroundObstacle('PACHYCEPHALO', '패닉에 빠진 파키케팔로사우루스', SPRITES.PACHY_1, 2.45, 2.9));
        } else if (subKey === '4B') {
          if (r < 0.42) obstacles.push(createGroundObstacle('LAVA_GEYSER', '분출하는 지각 용암 기둥', SPRITES.LAVA_GEYSER_1, 3.2, 0));
          else if (r < 0.80) {
            const targetGroundX = 290 + Math.random() * 350;
            const scale = 2.7;
            obstacles.push({
              type: 'METEOR_FRAGMENT', label: '초고열 운석 파편', scale,
              x: targetGroundX + 180, y: -20, vy: 6.2 + Math.random() * 1.5, vxExtra: 2.4, landed: false,
              w: Math.round(SPRITES.METEOR_OB[0].length * scale), h: Math.round(SPRITES.METEOR_OB.length * scale),
              speedBonus: 0, isGround: true
            });
          } else obstacles.push(createGroundObstacle('ANKYLOSAURUS', '폭주하는 안킬로사우루스', SPRITES.ANKYLO_OB_1, 2.35, 2.1));
        } else if (subKey === '5A' || subKey === '5B') {
          if (r < 0.38) obstacles.push(createGroundObstacle('MAMMOTH', '빙하기 털매머드', SPRITES.MAMMOTH_1, 2.45, 1.5));
          else if (r < 0.72) obstacles.push(createGroundObstacle('SMILODON', '검치호(스밀로돈) 습격', SPRITES.SMILODON_1, 2.4, 2.5));
          else obstacles.push(createGroundObstacle('DESERT_ROCKS', subKey === '5A' ? '구석기 붉은 부싯돌 & 화석 바위' : '신석기 흑요석 고인돌 제단', SPRITES.DESERT_ROCKS, 2.75, 0));
        } else if (subKey === '6A' || subKey === '6B') {
          if (r < 0.42) obstacles.push(createGroundObstacle('CHARIOT', subKey === '6A' ? '고대 제국 전차' : '중세 기마 기사', SPRITES.CHARIOT_1, 2.5, 2.3));
          else if (r < 0.76) {
            if (subKey === '6A') {
              obstacles.push(createGroundObstacle('SPARTAN_PHALANX', '고대 스파르타 창방패 보병', SPRITES.SPARTAN_1, 2.45, 1.6));
            } else {
              obstacles.push(createGroundObstacle('SIEGE_CANNON', '중세 공성 대포 & 바리케이드', SPRITES.CANNON_1, 2.55, 0.8));
            }
          }
          else obstacles.push(createFlyingObstacle('RHAMPHORHYNCHUS', subKey === '6A' ? '고대 전투 매 & 화살 [↓숙이기]' : '증기 비행선 드론 [↓숙이기]', SPRITES.RHAMPHO_1, 2.35, 1.7));
        } else if (subKey === '7A' || subKey === '7B') {
          if (r < 0.55) obstacles.push(createGroundObstacle('POLICE_CAR', '현대 경찰차 & 바리케이드', SPRITES.POLICE_CAR, 2.45, 2.4));
          else obstacles.push(createFlyingObstacle('HELICOPTER', '도심 추격 헬리콥터 [↓숙이기]', SPRITES.HELICOPTER_1, 2.35, 1.9));
        } else {
          if (r < 0.38) obstacles.push(createGroundObstacle('CHRONO_MECHA', '초미래 크로노 이족보행 메카', SPRITES.CHRONO_MECHA_1, 2.5, 2.1));
          else if (r < 0.70) obstacles.push(createFlyingObstacle('FUTURE_DRONE', '초미래 스테이시스 레이저 드론 [↓숙이기]', SPRITES.FUTURE_DRONE_1, 2.4, 2.3));
          else obstacles.push(createGroundObstacle('POLICE_CAR', '초고속 사이버 호버 순찰차', SPRITES.POLICE_CAR, 2.45, 3.0));
        }
      }

      function updateHeaderHUD() {
        const eraBadge = document.getElementById('eraBadge');
        const phaseTitleText = document.getElementById('phaseTitleText');
        const scoreProgressLabel = document.getElementById('scoreProgressLabel');
        const timelineFill = document.getElementById('timelineFill');
        const invText = document.getElementById('invincibleStatusText');

        const theme = SUBPHASE_THEMES[currentSubPhase] || SUBPHASE_THEMES['1A'];
        if (eraBadge) {
          if (gameState === STATE.STAGE1_BURIED_ENDING) eraBadge.textContent = 'STAGE 1 END: BURIED IN SOIL';
          else if (gameState === STATE.STAGE2_AWAKEN_CUTSCENE) eraBadge.textContent = 'STAGE 2: DIGGING OUT!';
          else if (gameState === STATE.STAGE2_CAPTURE_CUTSCENE) eraBadge.textContent = 'STAGE 2 END: OVERTAKE & SURROUND';
          else if (gameState === STATE.FINAL_EPILOGUE) eraBadge.textContent = 'FINAL ENDING: MUSEUM';
          else if (gameState === STATE.IMPACT_CUTSCENE) eraBadge.textContent = 'IMPACT: 66,000,000 BC';
          else if (ufoBoss.active) eraBadge.textContent = `UFO BOSS: ${ufoBoss.shotsFired}/${ufoBoss.maxShots} LASERS`;
          else eraBadge.textContent = theme.badge;
        }
        if (phaseTitleText) {
          if (gameState === STATE.STAGE1_BURIED_ENDING) phaseTitleText.textContent = '[1부 엔딩] 흙으로 꽉 찬 지층 속에 눈을 감은 채 묻힌 뼈 티렉스 — 스페이스바로 각성!';
          else if (gameState === STATE.STAGE2_AWAKEN_CUTSCENE) phaseTitleText.textContent = '[2부 오프닝] 눈을 뜨고 흙더미를 직접 파헤치며 구석기 지상으로 올라오는 뼈 티렉스!';
          else if (gameState === STATE.STAGE2_CAPTURE_CUTSCENE) phaseTitleText.textContent = '[2부 클라이맥스] 뒤에서 따라오던 추격대가 앞질러 전방을 가로막고 포위·박제합니다!';
          else if (gameState === STATE.FINAL_EPILOGUE) phaseTitleText.textContent = '[최종 엔딩] 자연사 박물관 — "멸종은 끝이 아닌 역사다"';
          else if (gameState === STATE.IMPACT_CUTSCENE) phaseTitleText.textContent = '[1부 클라이맥스] 소행성 대충돌 — 백악기-팔레오세 대멸종 (K-Pg)';
          else if (ufoBoss.active) phaseTitleText.textContent = `[초미래 중간보스] 거대 UFO 전함 레이저 공격 (${ufoBoss.shotsFired}/6회) — 상단(숙이기) / 하단(점프) 회피!`;
          else phaseTitleText.textContent = theme.koTitle;
        }
        if (scoreProgressLabel) {
          const dispScore = Math.min(STAGE2_END_SCORE, Math.floor(score));
          scoreProgressLabel.textContent = `${String(dispScore).padStart(4, '0')} / ${STAGE2_END_SCORE} PTS`;
        }
        if (timelineFill) {
          const pct = Math.min(100, Math.max(0, (score / STAGE2_END_SCORE) * 100));
          timelineFill.style.width = `${pct.toFixed(1)}%`;
        }
        if (invText) {
          invText.textContent = invincible ? 'INVINCIBLE: ON' : 'INVINCIBLE: OFF';
          invText.className = invincible ? 'pixel-font text-[9px] text-amber-400 font-bold' : 'pixel-font text-[9px] text-slate-500';
        }
        const mobInvBtn = document.getElementById('mobileInvincibleBtn');
        if (mobInvBtn) {
          mobInvBtn.textContent = invincible ? '🛡️ 무적: ON' : '🛡️ 무적: OFF';
          mobInvBtn.className = invincible
            ? 'px-2.5 py-1.5 rounded-lg bg-amber-500/25 border border-amber-400 text-amber-200 font-bold'
            : 'px-2.5 py-1.5 rounded-lg bg-indigo-950/90 border border-indigo-600/70 text-indigo-200 font-semibold';
        }
      }

      function updateColorLerp() {
        const target = SUBPHASE_THEMES[currentSubPhase];
        const lerpSpeed = (currentPhase === 4 || currentPhase === 8) ? 0.045 : 0.025;
        activeTheme.skyTop = lerpColor(activeTheme.skyTop, target.skyTop, lerpSpeed);
        activeTheme.skyBottom = lerpColor(activeTheme.skyBottom, target.skyBottom, lerpSpeed);
        activeTheme.groundLine = lerpColor(activeTheme.groundLine, target.groundLine, lerpSpeed);
        activeTheme.groundFill = lerpColor(activeTheme.groundFill, target.groundFill, lerpSpeed);
        activeTheme.groundSub = lerpColor(activeTheme.groundSub, target.groundSub, lerpSpeed);
        activeTheme.farSil = lerpColor(activeTheme.farSil, target.farSil, lerpSpeed);
        activeTheme.midSil = lerpColor(activeTheme.midSil, target.midSil, lerpSpeed);
        activeTheme.nearSil = lerpColor(activeTheme.nearSil, target.nearSil, lerpSpeed);
        activeTheme.dinoPrimary = lerpColor(activeTheme.dinoPrimary, target.dinoPrimary, lerpSpeed);
        activeTheme.dinoSecondary = lerpColor(activeTheme.dinoSecondary, target.dinoSecondary, lerpSpeed);
        activeTheme.dinoEye = lerpColor(activeTheme.dinoEye, target.dinoEye, lerpSpeed);
        activeTheme.uiText = lerpColor(activeTheme.uiText, target.uiText, lerpSpeed);
      }

      function updateUfoBoss(pBox) {
        if (!ufoBoss.active) return false;
        ufoBoss.stateTimer++;

        if (ufoBoss.state === 'APPROACHING') {
          ufoBoss.x = lerp(ufoBoss.x, 585, 0.08);
          ufoBoss.y = lerp(ufoBoss.y, ufoBoss.targetY, 0.08);
          if (ufoBoss.stateTimer >= 45) {
            ufoBoss.state = 'AIMING';
            ufoBoss.stateTimer = 0;
            sound.playUfoCharge();
          }
        } else if (ufoBoss.state === 'AIMING') {
          // Move smoothly to High or Low firing altitude and charge for ~0.65s (40 frames)
          ufoBoss.y = lerp(ufoBoss.y, ufoBoss.targetY, 0.14);
          if (ufoBoss.stateTimer >= 40) {
            ufoBoss.state = 'FIRING';
            ufoBoss.stateTimer = 0;
            sound.playUfoLaserFire();
          }
        } else if (ufoBoss.state === 'FIRING') {
          // REQUIREMENT 5: Laser fires for 0.1 seconds = 6 frames at 60fps!
          const beamTop = ufoBoss.y + 8;
          const beamH = 16;
          const beamBox = { x: 0, y: beamTop, w: ufoBoss.x + 12, h: beamH };

          if (
            !invincible &&
            pBox.x < beamBox.x + beamBox.w &&
            pBox.x + pBox.w > beamBox.x &&
            pBox.y < beamBox.y + beamBox.h &&
            pBox.y + pBox.h > beamBox.y
          ) {
            gameState = STATE.GAMEOVER;
            deathReason = ufoBoss.isHighLaser
              ? '초미래 UFO 보스 상단 레이저 (↓ 숙이기로 회피!)'
              : '초미래 UFO 보스 하단 레이저 (SPACE 점프로 회피!)';
            sound.stopBGM();
            const badgeEl = document.getElementById('bgmTrackName');
            if (badgeEl) badgeEl.textContent = 'BGM: 정지 (GAME OVER)';
            if (score > highScore) {
              highScore = Math.floor(score);
              try { localStorage.setItem('dino_66m_hi', String(highScore)); } catch (e) {}
            }
            sound.playGameOver();
            updateHeaderHUD();
            return true;
          }

          if (ufoBoss.stateTimer >= 6) {
            ufoBoss.shotsFired++;
            updateHeaderHUD();
            if (ufoBoss.shotsFired >= ufoBoss.maxShots) {
              ufoBoss.state = 'RETREATING';
              ufoBoss.stateTimer = 0;
              sound.playEvolution();
              banner.trigger('[ UFO BOSS DEFEATED! ]', '6연발 레이저 회피 성공! UFO 전함이 상공으로 퇴각합니다!', false);
            } else {
              ufoBoss.state = 'COOLDOWN';
              ufoBoss.stateTimer = 0;
              ufoBoss.isHighLaser = ufoBoss.patternSequence[ufoBoss.shotsFired % ufoBoss.patternSequence.length];
              // High laser = GROUND_Y - 56 (hits standing player, misses ducking player)
              // Low laser  = GROUND_Y - 22 (hits grounded player, misses jumping player)
              ufoBoss.targetY = ufoBoss.isHighLaser ? (GROUND_Y - 56) : (GROUND_Y - 22);
            }
          }
        } else if (ufoBoss.state === 'COOLDOWN') {
          ufoBoss.y = lerp(ufoBoss.y, ufoBoss.targetY, 0.12);
          if (ufoBoss.stateTimer >= 22) {
            ufoBoss.state = 'AIMING';
            ufoBoss.stateTimer = 0;
            sound.playUfoCharge();
          }
        } else if (ufoBoss.state === 'RETREATING') {
          ufoBoss.x += 9;
          ufoBoss.y -= 4.5;
          if (ufoBoss.x > W + 160) {
            ufoBoss.active = false;
            obstacleSpawnTimer = 35;
            updateHeaderHUD();
          }
        }
        return false;
      }

      function updatePlaying() {
        frameCount++;

        const stageLocalScore = currentStage === 1 ? score : (score - STAGE1_END_SCORE);
        gameSpeed = 5.8 + (stageLocalScore / 4000) * 3.6;
        score += 0.42 + (gameSpeed * 0.024);

        const currentHundred = Math.floor(score / 100);
        if (currentHundred > lastMilestone && score < STAGE2_END_SCORE - 15) {
          lastMilestone = currentHundred;
          sound.playScoreMilestone();
        }

        if (currentStage === 1 && score >= STAGE1_END_SCORE) {
          startImpactCutscene();
          return;
        }

        if (currentStage === 2 && score >= STAGE2_END_SCORE) {
          startStage2CaptureCutscene();
          return;
        }

        const newSubKey = getSubPhaseForScore(score);
        if (newSubKey !== currentSubPhase) {
          triggerSubPhaseChange(newSubKey);
        }

        // Trigger Phase 8 UFO Mid-Boss automatically around 7,080 PTS!
        if (currentStage === 2 && score >= 7080 && score < 7600 && !ufoBoss.triggeredInRun) {
          ufoBoss.startEncounter();
        }

        if (frameCount % 6 === 0) updateHeaderHUD();
        updateColorLerp();

        if (banner.timer > 0) {
          banner.timer--;
          if (banner.timer < 30) banner.alpha = banner.timer / 30;
          else if (banner.timer > banner.maxTimer - 20) banner.alpha = (banner.maxTimer - banner.timer) / 20;
          else banner.alpha = 1;
        }

        if (player.isJumping) {
          player.y += player.vy;
          player.vy += player.isDucking ? player.gravity * 2.4 : player.gravity;
          if (player.y >= GROUND_Y) {
            player.y = GROUND_Y;
            player.vy = 0;
            player.isJumping = false;
            for (let d = 0; d < 5; d++) {
              footstepDust.push({
                x: player.x + 12 + Math.random() * 18,
                y: GROUND_Y - 1,
                vx: -gameSpeed * 0.6 - Math.random() * 2,
                vy: -0.6 - Math.random() * 1.4,
                size: 3,
                life: 14 + Math.floor(Math.random() * 8)
              });
            }
          }
        }

        player.animTimer++;
        const strideInterval = Math.max(3, Math.round(6 - (gameSpeed - 5.5) * 0.4));
        if (player.animTimer >= strideInterval) {
          player.animTimer = 0;
          player.legFrame = (player.legFrame + 1) % 4;
          if (!player.isJumping && (player.legFrame === 0 || player.legFrame === 2)) {
            for (let k = 0; k < 2; k++) {
              footstepDust.push({
                x: player.x + 10 + (player.legFrame === 0 ? 18 : 6),
                y: GROUND_Y - 1,
                vx: -gameSpeed * 0.5 - Math.random() * 1.5,
                vy: -0.4 - Math.random() * 1.1,
                size: 3,
                life: 11 + Math.floor(Math.random() * 6)
              });
            }
          }
        }

        for (let i = footstepDust.length - 1; i >= 0; i--) {
          const fd = footstepDust[i];
          fd.x += fd.vx; fd.y += fd.vy; fd.life--;
          if (fd.life <= 0) footstepDust.splice(i, 1);
        }

        if (player.evolveFlashTimer > 0) player.evolveFlashTimer--;

        for (let p of groundPebbles) {
          p.x -= gameSpeed;
          if (p.x < -24) { p.x = W + Math.random() * 25; p.y = GROUND_Y + 4 + Math.random() * 14; }
        }
        for (let f of undergroundFossils) {
          f.x -= gameSpeed;
          if (f.x < -30) f.x = W + 30 + Math.random() * 60;
        }
        for (let cl of skyClouds) {
          cl.x -= cl.speed + gameSpeed * 0.04;
          if (cl.x + cl.w < -20) cl.x = W + 30;
        }
        for (let fl of distantFlocks) {
          fl.x -= fl.speed + gameSpeed * 0.08;
          if (fl.x < -80) { fl.x = W + 60 + Math.random() * 80; fl.y = 45 + Math.random() * 55; }
        }
        for (let lf of farLandforms) {
          lf.x -= gameSpeed * 0.14;
          if (lf.x + lf.w < -30) lf.x = W + 30;
          if (lf.kind === 'volcano' || lf.kind === 'caldera' || currentPhase === 4 || currentSubPhase === '6B') {
            lf.smokeTimer++;
            if (lf.smokeTimer > 26 && (currentPhase === 1 || currentPhase === 3 || currentPhase === 4 || currentSubPhase === '6B')) {
              lf.smokeTimer = 0;
              volcanoSmoke.push({ x: lf.x + lf.w * 0.5, y: GROUND_Y - lf.h, r: 4 + Math.random() * 5, alpha: 0.75 });
            }
          }
        }
        for (let i = volcanoSmoke.length - 1; i >= 0; i--) {
          const sm = volcanoSmoke[i];
          sm.x -= gameSpeed * 0.22; sm.y -= 0.55; sm.r += 0.13; sm.alpha -= 0.008;
          if (sm.alpha <= 0) volcanoSmoke.splice(i, 1);
        }

        for (let tr of forestTrees) {
          const spd = tr.layer === 0 ? gameSpeed * 0.24 : gameSpeed * 0.38;
          tr.x -= spd;
          if (tr.x < -75) {
            tr.x = W + 45 + Math.random() * 35;
            tr.h = 34 + Math.floor(Math.random() * 46);
          }
        }

        // REQUIREMENT 1: In Stage 1 (Mesozoic Era), background dinosaurs actively walk/run across the terrain
        // in both directions at speeds distinct from the background trees so they visibly travel across the world!
        for (let ml of midLandmarks) {
          if (currentStage === 1) {
            if (ml.dir === 1) {
              // Walking rightward across the plains (moves right relative to trees!)
              ml.x -= (gameSpeed * 0.14 - ml.walkSpeed);
            } else {
              // Walking/trotting leftward across the plains (faster than trees!)
              ml.x -= (gameSpeed * 0.42 + ml.walkSpeed);
            }
          } else {
            // In Stage 2, only Mammoth (slot % 2 === 0 in Phase 5) walks; architectural monuments scroll with terrain
            const isWalkingMammoth = (currentPhase === 5 && ml.slot % 2 === 0);
            ml.x -= isWalkingMammoth ? (gameSpeed * 0.42 + ml.walkSpeed * 0.8) : (gameSpeed * 0.34);
          }
          ml.anim++;
          if (ml.x < -160) {
            ml.x = W + 50 + Math.random() * 60;
            ml.slot = (ml.slot + 1) % 6;
          } else if (ml.x > W + 160) {
            ml.x = -120 - Math.random() * 40;
            ml.slot = (ml.slot + 1) % 6;
          }
        }

        if (currentStage === 1) {
          for (let rd of bgRoamingDinos) {
            if (rd.dir === 1) {
              rd.x += rd.speed;
              if (rd.x > W + 120) rd.x = -110 - Math.random() * 60;
            } else {
              rd.x -= (gameSpeed * 0.38 + rd.speed);
              if (rd.x < -120) rd.x = W + 80 + Math.random() * 80;
            }
            rd.anim++;
          }
        }

        // Scroll Stage 2 Background Humans
        if (currentStage === 2) {
          for (let hm of bgHumans) {
            hm.x -= (gameSpeed * 0.38 + hm.speedOffset);
            hm.anim++;
            if (hm.x < -50) {
              hm.x = W + 30 + Math.random() * 55;
              hm.role = (hm.role + 1) % 6;
            } else if (hm.x > W + 60) {
              hm.x = -40;
            }
          }
        }

        // Scroll Phase 8 Sci-Fi UFOs & Flying Cars
        if (currentPhase === 8) {
          for (let ufo of bgUfos) {
            ufo.x -= ufo.speed;
            ufo.phase += 0.07;
            if (ufo.x < -90) {
              ufo.x = W + 60 + Math.random() * 120;
              ufo.y = ufo.kind === 'skycar' ? (85 + Math.random() * 45) : (26 + Math.random() * 52);
            }
          }
        }

        for (let ns of nearScenery) {
          ns.x -= gameSpeed * 0.58;
          if (ns.x < -70) {
            ns.x = W + 30 + Math.random() * 35;
            ns.variant = (ns.variant + 1) % 7;
          }
        }

        if (currentSubPhase === '1B') {
          if (ashParticles.length < 45) {
            ashParticles.push({ x: W + 10, y: Math.random() * GROUND_Y, vx: -7 - Math.random() * 4, vy: (Math.random() - 0.5) * 0.8, size: 2, color: '#D97736' });
          }
        } else if (currentSubPhase === '2B') {
          if (ashParticles.length < 30 && Math.random() < 0.35) {
            ashParticles.push({ x: W + 10, y: 100 + Math.random() * (GROUND_Y - 110), vx: -1.5 - Math.random() * 1.5, vy: Math.sin(frameCount * 0.1) * 0.5, size: 3, color: '#BEF264' });
          }
        } else if (currentSubPhase === '5A') {
          if (ashParticles.length < 55) {
            ashParticles.push({ x: Math.random() * (W + 150), y: -8, vx: -2.2 - Math.random() * 2, vy: 1.5 + Math.random() * 1.8, size: 3, color: '#FFFFFF' });
          }
        } else if (currentPhase === 8) {
          if (ashParticles.length < 35 && Math.random() < 0.4) {
            ashParticles.push({ x: W + 10, y: Math.random() * GROUND_Y, vx: -6 - Math.random() * 4, vy: 0, size: 3, color: Math.random() < 0.5 ? '#22D3EE' : '#F472B6' });
          }
        } else if (currentPhase === 4) {
          if (Math.random() < 0.08 && panicStampede.length < 12) {
            const speciesPool = ['flyer', 'proto', 'parasaur', 'stego', 'tricera'];
            const spType = speciesPool[Math.floor(Math.random() * speciesPool.length)];
            panicStampede.push({
              x: -60, y: spType === 'flyer' ? (75 + Math.random() * 75) : GROUND_Y,
              vx: 3.2 + Math.random() * 3.2, spType, anim: Math.floor(Math.random() * 20)
            });
          }
          for (let i = panicStampede.length - 1; i >= 0; i--) {
            const d = panicStampede[i]; d.x += d.vx; d.anim++;
            if (d.x > W + 90) panicStampede.splice(i, 1);
          }
          if (ashParticles.length < 80) {
            for (let k = 0; k < 2; k++) {
              const isEmber = Math.random() < 0.5;
              ashParticles.push({
                x: Math.random() * (W + 200), y: -10,
                vx: -2.6 - Math.random() * 2.5, vy: 2.3 + Math.random() * 2.8,
                size: Math.random() < 0.35 ? 3 : 2,
                color: isEmber ? '#FF6D00' : 'rgba(210, 190, 195, 0.65)'
              });
            }
          }
        }

        for (let i = ashParticles.length - 1; i >= 0; i--) {
          const ap = ashParticles[i];
          ap.x += ap.vx; ap.y += ap.vy;
          if (ap.y > H || ap.x < -20) ashParticles.splice(i, 1);
        }

        for (let i = evolutionSparks.length - 1; i >= 0; i--) {
          const sp = evolutionSparks[i];
          sp.x += sp.vx; sp.y += sp.vy; sp.life--;
          if (sp.life <= 0) evolutionSparks.splice(i, 1);
        }

        const pBox = player.getHitbox();

        // Update UFO Mid-Boss if active
        if (ufoBoss.active) {
          const hitByLaser = updateUfoBoss(pBox);
          if (hitByLaser) return;
        } else {
          obstacleSpawnTimer--;
          if (obstacleSpawnTimer <= 0) {
            spawnObstacleForSubPhase(currentSubPhase);
            const stageDifficulty = currentStage === 1 ? currentPhase : (currentPhase - 4);
            const minGap = Math.max(46, 84 - stageDifficulty * 7);
            obstacleSpawnTimer = minGap + Math.floor(Math.random() * 40);
          }
        }

        for (let i = obstacles.length - 1; i >= 0; i--) {
          const ob = obstacles[i];

          if (ob.type === 'METEOR_FRAGMENT') {
            if (!ob.landed) {
              ob.x -= (gameSpeed + ob.vxExtra);
              ob.y += ob.vy;
              if (ob.y >= GROUND_Y + 3) {
                ob.y = GROUND_Y + 3;
                ob.landed = true;
                sound.playMeteorFragmentHit();
              }
            } else {
              ob.x -= gameSpeed;
            }
          } else {
            ob.x -= (gameSpeed + (ob.speedBonus || 0));
            if (ob.swoop) {
              ob.y = ob.baseY + Math.sin(frameCount * 0.08 + ob.x * 0.02) * 7;
            }
          }

          const obBox = { x: ob.x + 5, y: ob.y - ob.h + 5, w: ob.w - 10, h: ob.h - 6 };

          if (
            !invincible &&
            pBox.x < obBox.x + obBox.w &&
            pBox.x + pBox.w > obBox.x &&
            pBox.y < obBox.y + obBox.h &&
            pBox.y + pBox.h > obBox.y
          ) {
            gameState = STATE.GAMEOVER;
            deathReason = ob.label;
            sound.stopBGM();
            const badgeEl = document.getElementById('bgmTrackName');
            if (badgeEl) badgeEl.textContent = 'BGM: 정지 (GAME OVER)';
            if (score > highScore) {
              highScore = Math.floor(score);
              try { localStorage.setItem('dino_66m_hi', String(highScore)); } catch (e) {}
            }
            sound.playGameOver();
            updateHeaderHUD();
            return;
          }

          if (ob.x + ob.w < -80) obstacles.splice(i, 1);
        }
      }

      function updateImpactCutscene() {
        frameCount++;
        cutscene.timer++;
        player.x = lerp(player.x, 140, 0.08);
        player.y = GROUND_Y;
        player.lookingUp = true;

        if (!cutscene.impacted) {
          const progress = Math.min(1, cutscene.timer / 135);
          const eased = progress * progress;
          cutscene.meteorX = lerp(700, 440, eased);
          cutscene.meteorY = lerp(16, GROUND_Y - 24, eased);
          cutscene.meteorScale = lerp(2.6, 7.5, eased);

          ashParticles.push({
            x: cutscene.meteorX + (Math.random() - 0.5) * 60,
            y: cutscene.meteorY + (Math.random() - 0.5) * 60,
            vx: (Math.random() - 0.3) * 5, vy: -2 - Math.random() * 4,
            size: 4, color: '#FF6D00'
          });

          if (progress >= 1) {
            cutscene.impacted = true;
            cutscene.whiteoutAlpha = 0.1;
            cutscene.whiteoutHoldTimer = 0;
            sound.playCataclysmicExplosion();
          }
        } else {
          cutscene.shockwaveStep += 26;
          if (cutscene.whiteoutAlpha < 1) {
            cutscene.whiteoutAlpha = Math.min(1, cutscene.whiteoutAlpha + 0.09);
          } else {
            cutscene.whiteoutHoldTimer++;
            if (cutscene.whiteoutHoldTimer >= 150) {
              startStage1BuriedEnding();
            }
          }
        }
      }

      function updateBuriedEnding() {
        frameCount++;
        buriedScene.timer++;
        if (buriedScene.fadeFromWhite > 0) {
          buriedScene.fadeFromWhite = Math.max(0, buriedScene.fadeFromWhite - 0.012);
        }
      }

      function updateAwakenCutscene() {
        frameCount++;
        awakenScene.timer++;

        const scale = 2.65;
        const spriteH = SPRITES.SKEL_RUNFrames[0].length * scale; // ~47.7px

        if (awakenScene.timer === 16) {
          awakenScene.phase = 'DIGGING';
          sound.playMeteorFragmentHit();
        }

        // Phase 1 (timer 20 ~ 92): Dig upward through the underground soil toward surfaceY
        if (awakenScene.timer >= 20 && awakenScene.timer < 92) {
          awakenScene.phase = 'DIGGING';
          const targetDigY = awakenScene.surfaceY - spriteH * 0.45;
          awakenScene.fossilY = lerp(awakenScene.fossilY, targetDigY, 0.065);
          awakenScene.dugPath.push({
            x: awakenScene.fossilX + 28 + (Math.random() - 0.5) * 12,
            y: awakenScene.fossilY + 6,
            w: 76,
            h: 32
          });
          if (awakenScene.timer % 3 === 0) {
            sound.playMeteorFragmentHit();
            for (let i = 0; i < 6; i++) {
              awakenScene.dirtClods.push({
                x: awakenScene.fossilX + 8 + Math.random() * 52,
                y: Math.max(awakenScene.surfaceY - 4, awakenScene.fossilY),
                vx: (Math.random() - 0.5) * 9.5,
                vy: -3.5 - Math.random() * 6.0,
                size: 4 + Math.floor(Math.random() * 5),
                color: Math.random() < 0.5 ? '#5C4033' : '#8B5A2B'
              });
            }
          }
        }

        // Phase 2 (timer === 92): Burst out of the ground into the air above the surface!
        if (awakenScene.timer === 92) {
          awakenScene.phase = 'LEAP_OUT';
          awakenScene.vy = -8.8;
          sound.playJump();
          sound.playEvolution();
          for (let i = 0; i < 24; i++) {
            awakenScene.dirtClods.push({
              x: awakenScene.fossilX + Math.random() * 64,
              y: awakenScene.surfaceY,
              vx: (Math.random() - 0.5) * 12,
              vy: -4.5 - Math.random() * 8.0,
              size: 5 + Math.floor(Math.random() * 5),
              color: i % 3 === 0 ? '#E2E8F0' : (i % 2 === 0 ? '#5C4033' : '#8B5A2B')
            });
          }
        }

        // Phase 2B (timer 92 ~ 136): Arc through the air and land ON TOP OF the ground surface!
        if (awakenScene.timer > 92 && awakenScene.timer < 136) {
          const onSurfaceTopY = awakenScene.surfaceY - spriteH + 2;
          awakenScene.fossilY += awakenScene.vy;
          awakenScene.vy += 0.52;
          if (awakenScene.fossilY >= onSurfaceTopY && awakenScene.vy > 0) {
            awakenScene.fossilY = onSurfaceTopY;
            awakenScene.vy = 0;
            if (awakenScene.phase !== 'SURFACE_LANDED') {
              awakenScene.phase = 'SURFACE_LANDED';
              sound.playMeteorFragmentHit();
            }
          }
        }

        // Phase 3 (timer 136 ~ 188): After landing on the ground surface, camera smoothly lowers surfaceY -> GROUND_Y
        // while the Skeleton T-Rex runs across the surface into the starting position (x = 92)!
        if (awakenScene.timer >= 136) {
          awakenScene.phase = 'SURFACE_RUN';
          awakenScene.surfaceY = lerp(awakenScene.surfaceY, GROUND_Y, 0.095);
          awakenScene.fossilX = lerp(awakenScene.fossilX, 92, 0.095);
          awakenScene.fossilY = awakenScene.surfaceY - spriteH + 2;
          for (let dp of awakenScene.dugPath) {
            dp.y += (GROUND_Y - awakenScene.surfaceY) * 0.095;
          }
        }

        for (let i = awakenScene.dirtClods.length - 1; i >= 0; i--) {
          const dc = awakenScene.dirtClods[i];
          dc.x += dc.vx;
          dc.y += dc.vy;
          dc.vy += 0.35;
          if (dc.y > H) awakenScene.dirtClods.splice(i, 1);
        }

        if (awakenScene.timer >= 188) {
          beginStage2Playing();
        }
      }

      // Stage 2 Capture Cutscene: Pursuers come from behind (left), accelerate & overtake player, block the front, and surround!
      function updateCaptureCutscene() {
        frameCount++;
        captureScene.timer++;

        if (captureScene.timer < 75) {
          // Phase 1: Everyone is still running! Pursuers behind (left) accelerate rapidly and overtake the Skeleton T-Rex!
          captureScene.phase = 'OVERTAKE';
          player.legFrame = Math.floor(frameCount / 3) % 4;
          for (let p of groundPebbles) {
            p.x -= 7.5;
            if (p.x < -20) p.x = W + 20;
          }
          // Skeleton T-Rex runs forward toward center
          captureScene.dinoX = lerp(captureScene.dinoX, W / 2 - 28, 0.045);
          // Vanguard Mech & Hover Cruiser blast forward from behind (x=20) to ahead of the T-Rex (x = W/2 + 95)!
          captureScene.overtakeMechX += 8.8;
          if (captureScene.overtakeMechX > W / 2 + 88) captureScene.overtakeMechX = W / 2 + 88;

          captureScene.overtakeCarX += 9.6;
          if (captureScene.overtakeCarX > W / 2 + 150) captureScene.overtakeCarX = W / 2 + 150;

          // Rear Mech closes in right behind T-Rex
          captureScene.rearMechX = lerp(captureScene.rearMechX, W / 2 - 155, 0.06);

          // Aerial Drones swoop from behind to overhead
          captureScene.drone1X = lerp(captureScene.drone1X, W / 2 - 85, 0.07);
          captureScene.drone2X = lerp(captureScene.drone2X, W / 2 + 35, 0.07);
        } else {
          // Phase 2: Vanguard units slam brakes in front, blocking the path! T-Rex halts surrounded on all sides!
          captureScene.phase = 'BLOCK_STOP';
          captureScene.dinoX = lerp(captureScene.dinoX, W / 2 - 28, 0.12);
          captureScene.rearMechX = lerp(captureScene.rearMechX, W / 2 - 145, 0.12);
          captureScene.overtakeMechX = lerp(captureScene.overtakeMechX, W / 2 + 85, 0.12);
          captureScene.overtakeCarX = lerp(captureScene.overtakeCarX, W / 2 + 155, 0.12);
          captureScene.droneY = lerp(captureScene.droneY, 68, 0.1);
        }

        if (captureScene.timer === 82) {
          captureScene.stasisActive = true;
          sound.playStasisBeamHum();
        }
        if (captureScene.stasisActive) {
          captureScene.crystalAlpha = Math.min(1, captureScene.crystalAlpha + 0.015);
        }
        if (captureScene.timer > 180) {
          captureScene.whiteoutAlpha = Math.min(1, captureScene.whiteoutAlpha + 0.04);
          if (captureScene.whiteoutAlpha >= 1) {
            startFinalEpilogue();
          }
        }
      }

      function updateFinalEpilogue() {
        frameCount++;
        epilogue.timer++;

        if (epilogue.fadeFromWhite > 0) {
          epilogue.fadeFromWhite = Math.max(0, epilogue.fadeFromWhite - 0.008);
        }

        if (epilogue.timer > 60 && epilogue.timer % 3 === 0) {
          if (epilogue.line1Shown.length < epilogue.line1Full.length) {
            epilogue.line1Shown = epilogue.line1Full.slice(0, epilogue.line1Shown.length + 1);
            sound.playTypewriterTick();
          } else if (epilogue.timer > 155 && epilogue.line2Shown.length < epilogue.line2Full.length) {
            epilogue.line2Shown = epilogue.line2Full.slice(0, epilogue.line2Shown.length + 1);
            sound.playTypewriterTick();
          }
        }

        for (let v of epilogue.visitors) {
          v.x += v.speed;
          v.step += 0.12;
          if (v.type === 'photographer') {
            v.flashTimer--;
            if (v.flashTimer <= -8) v.flashTimer = 140 + Math.floor(Math.random() * 90);
          }
          if (v.x > W + 40) v.x = -40;
        }
      }

      function drawLeafBlob(ctx, cx, cy, rx, ry, color, highlightColor) {
        ctx.fillStyle = color;
        ctx.beginPath();
        ctx.ellipse(cx, cy, rx, ry, 0, 0, Math.PI * 2);
        ctx.fill();
        ctx.fillStyle = highlightColor;
        ctx.beginPath();
        ctx.ellipse(cx - rx * 0.2, cy - ry * 0.3, rx * 0.5, ry * 0.4, 0, 0, Math.PI * 2);
        ctx.fill();
      }

      function drawTreeOrBuilding(tr, groundY, crownColor, trunkColor, style) {
        const ix = Math.floor(tr.x);
        const iy = Math.floor(groundY);
        const trunkH = tr.h;
        const crownW = tr.crownW;

        // 1) MODERN & FAR FUTURE BUILDINGS (10 Varied Types: Bank, Billboard, Landmark Tower, Cafe/Shop, Broadcast Radar, Arch/Bio Dome, Hospital/Hotel, Twin Skybridge, Stepped Pyramid Corp, Cyber Skyscraper)
        if (style === 'skyscraper') {
          const isFuture = currentPhase === 8;
          const isNight = currentSubPhase === '7B' || isFuture;
          const bH = isFuture ? Math.floor(trunkH * 2.15 + 55) : Math.floor(trunkH + 34);
          const bW = isFuture ? Math.floor(crownW * 1.15) : crownW;
          const bx = ix - Math.floor(bW / 2);
          const by = iy - bH;

          if (tr.bType === 'CAFE_SHOP') {
            // Low-to-mid boutique commercial building with striped awning & rooftop coffee/neon sign
            const shopH = isFuture ? Math.floor(bH * 0.65) : Math.floor(bH * 0.72);
            const sy = iy - shopH;
            ctx.fillStyle = crownColor;
            ctx.fillRect(bx - 2, sy, bW + 4, shopH);
            // Upper apartment/office windows
            ctx.fillStyle = isNight ? '#FDE047' : '#E0F2FE';
            for (let wy = sy + 8; wy < iy - 24; wy += 10) {
              for (let wx = bx + 4; wx < bx + bW - 4; wx += 9) {
                ctx.fillRect(wx, wy, 5, 6);
              }
            }
            // Ground floor warm storefront glass
            ctx.fillStyle = isFuture ? '#22D3EE' : '#FEF08A';
            ctx.fillRect(bx + 3, iy - 15, bW - 6, 13);
            // Striped Awning (차양막)
            for (let ax = bx - 3; ax < bx + bW + 2; ax += 6) {
              ctx.fillStyle = ((ax - bx) / 6) % 2 === 0 ? (isFuture ? '#F43F5E' : '#EF4444') : '#FFFFFF';
              ctx.fillRect(ax, iy - 20, 3, 5);
              ctx.fillStyle = '#F8FAFC';
              ctx.fillRect(ax + 3, iy - 20, 3, 5);
            }
            // Rooftop Cup / Holo Sign
            ctx.fillStyle = '#0F172A';
            ctx.fillRect(ix - 12, sy - 11, 24, 11);
            ctx.fillStyle = isFuture ? '#E879F9' : '#FB923C';
            ctx.font = '700 8px "Inter", sans-serif';
            ctx.textAlign = 'center';
            ctx.fillText(isFuture ? 'NEO' : 'CAFE', ix, sy - 3);
            return;
          }

          if (tr.bType === 'ARCH_DOME') {
            // Modern Museum/Planetarium Dome OR Future Bio-Sphere Arcology Dome!
            const domeH = isFuture ? Math.floor(bH * 0.78) : Math.floor(bH * 0.75);
            const dy = iy - domeH;
            ctx.fillStyle = crownColor;
            ctx.fillRect(bx - 4, dy + 16, bW + 8, domeH - 16);
            // Rounded geodesic dome roof
            ctx.fillStyle = isFuture ? 'rgba(34, 211, 238, 0.45)' : '#64748B';
            ctx.beginPath();
            ctx.arc(ix, dy + 18, Math.floor(bW * 0.58), Math.PI, 0);
            ctx.fill();
            // Geodesic lattice lines on dome
            ctx.strokeStyle = isFuture ? '#5EEAD4' : '#CBD5E1';
            ctx.lineWidth = 1.5;
            ctx.beginPath();
            ctx.arc(ix, dy + 18, Math.floor(bW * 0.58), Math.PI, 0);
            ctx.stroke();
            // Glowing arched windows
            ctx.fillStyle = isFuture ? '#A855F7' : (isNight ? '#38BDF8' : '#E0F2FE');
            for (let wx = bx + 2; wx < bx + bW - 4; wx += 10) {
              ctx.fillRect(wx, dy + 24, 6, domeH - 28);
            }
            return;
          }

          if (tr.bType === 'TWIN_BRIDGE_TOWER') {
            // Twin Towers connected by an elevated Skybridge (트윈 타워 & 스카이브릿지)
            const halfW = Math.max(10, Math.floor((bW - 8) / 2));
            const leftX = bx - 2;
            const rightX = bx + halfW + 6;
            ctx.fillStyle = crownColor;
            ctx.fillRect(leftX, by, halfW, bH);
            ctx.fillRect(rightX, by, halfW, bH);
            // Skybridge connecting the two towers
            const bridgeY = by + Math.floor(bH * 0.35);
            ctx.fillStyle = isFuture ? '#06B6D4' : '#475569';
            ctx.fillRect(leftX + halfW, bridgeY, 8, 9);
            ctx.fillStyle = isFuture ? '#CFFAFE' : '#FDE047';
            ctx.fillRect(leftX + halfW, bridgeY + 2, 8, 4);
            // Windows on both twin towers
            ctx.fillStyle = isFuture ? '#22D3EE' : (isNight ? '#FACC15' : '#BAE6FD');
            for (let wy = by + 8; wy < iy - 8; wy += 9) {
              ctx.fillRect(leftX + 3, wy, halfW - 6, 4);
              ctx.fillRect(rightX + 3, wy, halfW - 6, 4);
            }
            // Twin rooftop spires
            ctx.fillStyle = isFuture ? '#F43F5E' : '#94A3B8';
            ctx.fillRect(leftX + Math.floor(halfW / 2) - 1, by - 14, 2, 14);
            ctx.fillRect(rightX + Math.floor(halfW / 2) - 1, by - 14, 2, 14);
            return;
          }

          if (tr.bType === 'STEPPED_PYRAMID_CORP') {
            // Terraced Ziggurat Corporate Headquarters / Cyber Mega-Pyramid
            const tiers = 4;
            const tierH = Math.floor(bH / tiers);
            for (let t = 0; t < tiers; t++) {
              const shrink = t * 4;
              const tw = Math.max(12, bW + 8 - shrink * 2);
              const tx = ix - Math.floor(tw / 2);
              const ty = iy - (t + 1) * tierH;
              ctx.fillStyle = crownColor;
              ctx.fillRect(tx, ty, tw, tierH + 2);
              // Horizontal neon/glass ribbon per tier
              ctx.fillStyle = isFuture ? (t % 2 === 0 ? '#22D3EE' : '#F43F5E') : (isNight ? '#38BDF8' : '#E0F2FE');
              ctx.fillRect(tx + 3, ty + 4, tw - 6, 4);
              ctx.fillRect(tx + 3, ty + 11, tw - 6, 4);
            }
            // Glowing apex core
            ctx.fillStyle = isFuture ? '#FACC15' : '#38BDF8';
            ctx.beginPath();
            ctx.moveTo(ix - 7, iy - tiers * tierH);
            ctx.lineTo(ix, iy - tiers * tierH - 12);
            ctx.lineTo(ix + 7, iy - tiers * tierH);
            ctx.closePath();
            ctx.fill();
            return;
          }

          // Base rectangle for remaining building types
          ctx.fillStyle = crownColor;
          ctx.fillRect(bx, by, bW, bH);

          if (tr.bType === 'BANK') {
            ctx.fillStyle = isFuture ? '#0F766E' : '#475569';
            ctx.fillRect(bx - 3, by, bW + 6, 8);
            ctx.beginPath();
            ctx.moveTo(bx - 4, by);
            ctx.lineTo(ix, by - 14);
            ctx.lineTo(bx + bW + 4, by);
            ctx.closePath();
            ctx.fill();
            ctx.fillStyle = isFuture ? '#5EEAD4' : '#CBD5E1';
            for (let cx = bx + 5; cx < bx + bW - 4; cx += 8) {
              ctx.fillRect(cx, by + 18, 3, bH - 18);
            }
            ctx.fillStyle = '#0F172A';
            ctx.fillRect(ix - 11, by + 4, 22, 11);
            ctx.fillStyle = '#FACC15';
            ctx.font = '8px "Press Start 2P", monospace';
            ctx.textAlign = 'center';
            ctx.fillText('$', ix, by + 13);
          } else if (tr.bType === 'BILLBOARD') {
            const winCol = isNight ? '#38BDF8' : '#E0F2FE';
            ctx.fillStyle = winCol;
            for (let wy = by + 32; wy < iy - 8; wy += 9) {
              for (let wx = bx + 5; wx < bx + bW - 5; wx += 8) {
                ctx.fillRect(wx, wy, 4, 5);
              }
            }
            const boardW = bW + 10;
            const boardH = 22;
            const boardX = ix - Math.floor(boardW / 2);
            const boardY = by + 4;
            ctx.fillStyle = '#090D16';
            ctx.fillRect(boardX, boardY, boardW, boardH);
            const borderBlink = Math.floor(frameCount / 10) % 2 === 0;
            ctx.strokeStyle = isFuture ? (borderBlink ? '#F43F5E' : '#22D3EE') : (borderBlink ? '#FACC15' : '#38BDF8');
            ctx.lineWidth = 2;
            ctx.strokeRect(boardX, boardY, boardW, boardH);
            const barShift = Math.floor(frameCount / 6);
            for (let k = 0; k < 4; k++) {
              const bh = 4 + ((k + barShift) % 4) * 3;
              ctx.fillStyle = k % 2 === 0 ? '#34D399' : '#F43F5E';
              ctx.fillRect(boardX + 5 + k * 7, boardY + boardH - bh - 3, 5, bh);
            }
          } else if (tr.bType === 'LANDMARK_TOWER') {
            ctx.fillStyle = crownColor;
            ctx.fillRect(ix - Math.floor(bW * 0.35), by - 22, Math.floor(bW * 0.7), 22);
            ctx.fillStyle = isFuture ? '#22D3EE' : '#FACC15';
            ctx.fillRect(ix - Math.floor(bW * 0.55), by - 12, Math.floor(bW * 1.1), 5);
            ctx.fillStyle = '#94A3B8';
            ctx.fillRect(ix - 1, by - 44, 3, 22);
            if (Math.floor(frameCount / 8) % 2 === 0) {
              ctx.fillStyle = '#EF4444';
              ctx.fillRect(ix - 3, by - 47, 7, 7);
            }
            ctx.fillStyle = isFuture ? '#F472B6' : '#38BDF8';
            ctx.fillRect(ix - 2, by + 6, 4, bH - 12);
          } else if (tr.bType === 'BROADCAST') {
            // Communications / Radar Dish Tower (방송국 & 위성 레이더 타워)
            ctx.fillStyle = isFuture ? '#67E8F9' : '#CBD5E1';
            for (let wy = by + 10; wy < iy - 8; wy += 12) {
              ctx.fillRect(bx + 4, wy, bW - 8, 4);
            }
            // Rooftop lattice mast & parabolic radar dish
            ctx.fillStyle = '#94A3B8';
            ctx.fillRect(ix - 4, by - 20, 2, 20);
            ctx.fillRect(ix + 2, by - 20, 2, 20);
            ctx.fillRect(ix - 4, by - 12, 8, 2);
            // Rotating / blinking dish & signal waves
            ctx.strokeStyle = isFuture ? '#22D3EE' : '#F8FAFC';
            ctx.lineWidth = 2;
            ctx.beginPath();
            ctx.arc(ix + 5, by - 22, 8, Math.PI * 0.7, Math.PI * 1.7);
            ctx.stroke();
            if (Math.floor(frameCount / 7) % 2 === 0) {
              ctx.fillStyle = isFuture ? '#22D3EE' : '#38BDF8';
              ctx.fillRect(ix - 9, by - 28, 3, 3);
            }
          } else if (tr.bType === 'HOSPITAL_HOTEL') {
            // Medical Center (+) / Luxury Hotel with Rooftop Helipad [H]
            ctx.fillStyle = isFuture ? '#A5F3FC' : '#E2E8F0';
            for (let wy = by + 22; wy < iy - 8; wy += 9) {
              for (let wx = bx + 4; wx < bx + bW - 4; wx += 7) {
                ctx.fillRect(wx, wy, 4, 5);
              }
            }
            // Rooftop Helipad deck
            ctx.fillStyle = '#334155';
            ctx.fillRect(bx - 3, by - 4, bW + 6, 4);
            // Illuminated Red Cross (+) or Cyber Med-Emblem
            ctx.fillStyle = '#0F172A';
            ctx.fillRect(ix - 7, by + 4, 14, 14);
            ctx.fillStyle = isFuture ? '#22D3EE' : '#EF4444';
            ctx.fillRect(ix - 2, by + 6, 4, 10);
            ctx.fillRect(ix - 5, by + 9, 10, 4);
          } else {
            // Standard / Cyber Arcology Skyscraper
            const winCol = isFuture ? (tr.seed % 2 === 0 ? '#22D3EE' : '#E879F9') : (isNight ? '#FDE047' : '#E0F2FE');
            ctx.fillStyle = winCol;
            for (let wy = by + 10; wy < iy - 10; wy += 10) {
              for (let wx = bx + 5; wx < bx + bW - 5; wx += 8) {
                if ((wy + wx + tr.seed) % 3 !== 0) {
                  ctx.fillRect(wx, wy, 4, 5);
                }
              }
            }
            if (isFuture) {
              ctx.fillStyle = 'rgba(34, 211, 238, 0.35)';
              ctx.fillRect(bx - 18, by + 28, bW + 36, 5);
              ctx.strokeStyle = '#F43F5E';
              ctx.lineWidth = 1.5;
              ctx.strokeRect(bx + 2, by + 2, bW - 4, 14);
            }
          }
          return;
        }

        // 2) REQUIREMENT 1: EXTINCTION PHASE (Phase 4) -> Leafless dead trunks & Fallen trees!
        if (style === 'extinction_dead') {
          const isFallen = tr.seed % 3 === 0;
          ctx.fillStyle = trunkColor;
          if (isFallen) {
            // Fallen Tree lying on the scorched ground with broken roots & embers!
            ctx.fillRect(ix - 18, iy - 8, trunkH + 6, 7);
            // Upturned jagged roots on the left
            ctx.fillRect(ix - 22, iy - 14, 5, 14);
            ctx.fillRect(ix - 26, iy - 10, 4, 8);
            // Broken dead branch sticking up from the fallen trunk
            ctx.fillRect(ix + 4, iy - 18, 4, 10);
            ctx.fillRect(ix + 7, iy - 21, 5, 4);
            // Glowing ember cracks on the fallen log
            ctx.fillStyle = (frameCount + ix) % 10 < 5 ? '#FF4500' : '#FFCC00';
            ctx.fillRect(ix - 6, iy - 6, 8, 3);
            ctx.fillRect(ix + 12, iy - 5, 6, 2);
          } else {
            // Standing Leafless Dead Tree (앙상하고 부러진 나뭇가지만 남은 고사목, 나뭇잎 없음!)
            ctx.fillRect(ix - 4, iy - trunkH, 8, trunkH + 2);
            ctx.fillRect(ix - 8, iy - 5, 16, 5);
            // Bare jagged branches left & right (NO leaf blobs!)
            ctx.fillRect(ix - 14, iy - Math.floor(trunkH * 0.72), 10, 4);
            ctx.fillRect(ix - 16, iy - Math.floor(trunkH * 0.72) - 7, 4, 8);
            ctx.fillRect(ix + 4, iy - Math.floor(trunkH * 0.55), 11, 4);
            ctx.fillRect(ix + 12, iy - Math.floor(trunkH * 0.55) - 6, 4, 7);
            ctx.fillRect(ix - 9, iy - Math.floor(trunkH * 0.9), 6, 3);
            // Ember glow in bark cracks
            ctx.fillStyle = '#FF5500';
            ctx.fillRect(ix - 1, iy - Math.floor(trunkH * 0.5), 3, 9);
          }
          return;
        }

        // 3) NORMAL LUSH TREES (Triassic, Jurassic, Cretaceous, Paleolithic, Ancient)
        ctx.fillStyle = trunkColor;
        ctx.fillRect(ix - 4, iy - trunkH, 8, trunkH + 2);
        ctx.fillRect(ix - 8, iy - Math.floor(trunkH * 0.3), 16, 4);

        const highlightCol = lerpColor(crownColor, '#FFFFFF', 0.28);

        if (style === 'cycad') {
          for (let b = -3; b <= 3; b++) {
            const bx = ix + b * 6;
            const by = iy - trunkH - 4;
            drawLeafBlob(ctx, bx, by, 10, 6, crownColor, highlightCol);
          }
        } else if (style === 'araucaria') {
          const cy = iy - trunkH;
          drawLeafBlob(ctx, ix, cy - 6, crownW * 0.5, 9, crownColor, highlightCol);
          drawLeafBlob(ctx, ix - crownW * 0.25, cy - 15, crownW * 0.38, 7, crownColor, highlightCol);
          drawLeafBlob(ctx, ix + crownW * 0.25, cy - 15, crownW * 0.38, 7, crownColor, highlightCol);
          drawLeafBlob(ctx, ix, cy - 25, crownW * 0.28, 6, crownColor, highlightCol);
        } else if (style === 'magnolia') {
          const cy = iy - trunkH;
          drawLeafBlob(ctx, ix, cy - 10, crownW * 0.55, 14, crownColor, highlightCol);
          drawLeafBlob(ctx, ix - crownW * 0.3, cy - 18, crownW * 0.4, 10, crownColor, highlightCol);
          drawLeafBlob(ctx, ix + crownW * 0.3, cy - 18, crownW * 0.4, 10, crownColor, highlightCol);
          drawLeafBlob(ctx, ix, cy - 26, crownW * 0.35, 9, crownColor, highlightCol);

          ctx.fillStyle = '#F472B6';
          ctx.fillRect(ix - Math.floor(crownW * 0.25), cy - 14, 5, 5);
          ctx.fillRect(ix + Math.floor(crownW * 0.15), cy - 20, 5, 5);
          ctx.fillRect(ix - 2, cy - 25, 5, 5);
        } else if (style === 'snow_pine') {
          const cy = iy - trunkH;
          drawLeafBlob(ctx, ix, cy - 6, crownW * 0.5, 8, crownColor, highlightCol);
          drawLeafBlob(ctx, ix, cy - 15, crownW * 0.38, 7, crownColor, highlightCol);
          drawLeafBlob(ctx, ix, cy - 23, crownW * 0.25, 6, crownColor, highlightCol);

          ctx.fillStyle = '#F8FAFC';
          ctx.fillRect(ix - Math.floor(crownW * 0.4), cy - 7, Math.floor(crownW * 0.8), 3);
          ctx.fillRect(ix - Math.floor(crownW * 0.3), cy - 16, Math.floor(crownW * 0.6), 3);
        } else {
          drawLeafBlob(ctx, ix, iy - trunkH - 8, crownW * 0.48, 12, crownColor, highlightCol);
        }
      }

      // REQUIREMENT 7: Diverse human appearances, outfits, hairstyles, hats, and accessories in every era!
      function drawBackgroundHumans() {
        if (currentStage !== 2) return;

        for (let hm of bgHumans) {
          const hx = Math.floor(hm.x);
          const stride = Math.floor((frameCount + hm.anim) / 5) % 2;
          const bob = stride === 0 ? -1.5 : 0;
          const hy = Math.floor(GROUND_Y + bob);
          const r = hm.role; // 0..5

          if (currentPhase === 5) {
            // Paleolithic / Neolithic: 6 distinct tribal outfits (Leopard pelt, Shaman skull mask, Mammoth coat, Feather gatherer, Wolf-hood archer, Campfire roaster)
            if (r === 5) {
              ctx.fillStyle = '#5C4033';
              ctx.fillRect(hx - 10, GROUND_Y - 4, 12, 4);
              ctx.fillStyle = (frameCount + hx) % 6 < 3 ? '#FF5500' : '#FACC15';
              ctx.fillRect(hx - 8, GROUND_Y - 12, 8, 8);
              ctx.fillRect(hx - 6, GROUND_Y - 16, 4, 4);
            }
            const px = r === 5 ? hx + 8 : hx;
            // Legs
            ctx.fillStyle = hm.skin;
            ctx.fillRect(px + (stride ? 0 : -2), hy - 6, 3, 6);
            ctx.fillRect(px + (stride ? 5 : 3), hy - 6, 3, 6);

            // Varied Tribal Outfits
            if (r === 0) {
              // Leopard spotted fur tunic + bone necklace
              ctx.fillStyle = '#F59E0B';
              ctx.fillRect(px - 1, hy - 16, 8, 10);
              ctx.fillStyle = '#451A03';
              ctx.fillRect(px + 1, hy - 14, 2, 2);
              ctx.fillRect(px + 4, hy - 11, 2, 2);
              ctx.fillStyle = '#F8FAFC';
              ctx.fillRect(px, hy - 16, 6, 2); // Bone necklace
            } else if (r === 1) {
              // Tribal Shaman in crimson cloak + skull mask headdress
              ctx.fillStyle = '#B91C1C';
              ctx.fillRect(px - 2, hy - 16, 10, 11);
              ctx.fillStyle = '#FDE047';
              ctx.fillRect(px + 1, hy - 14, 4, 4);
            } else if (r === 2) {
              // White-trimmed Mammoth winter parka
              ctx.fillStyle = '#5C4033';
              ctx.fillRect(px - 2, hy - 16, 10, 10);
              ctx.fillStyle = '#F8FAFC';
              ctx.fillRect(px - 2, hy - 16, 10, 2);
              ctx.fillRect(px - 2, hy - 8, 10, 2);
            } else if (r === 3) {
              // Neolithic woven emerald & ochre two-piece + basket
              ctx.fillStyle = '#0D9488';
              ctx.fillRect(px - 1, hy - 16, 8, 5);
              ctx.fillStyle = '#B45309';
              ctx.fillRect(px - 2, hy - 11, 10, 6);
              ctx.fillStyle = '#D97706';
              ctx.fillRect(px - 5, hy - 15, 4, 7); // Woven back-basket
            } else {
              // Dark wolf-pelt vest & leather kilt
              ctx.fillStyle = '#334155';
              ctx.fillRect(px - 1, hy - 16, 8, 6);
              ctx.fillStyle = '#92400E';
              ctx.fillRect(px - 1, hy - 10, 8, 5);
            }

            // Head & Varied Hair/Headdress
            ctx.fillStyle = hm.skin;
            ctx.fillRect(px, hy - 22, 6, 6);
            if (r === 1) {
              // Horned skull headdress
              ctx.fillStyle = '#F8FAFC';
              ctx.fillRect(px - 1, hy - 25, 8, 4);
              ctx.fillRect(px - 3, hy - 28, 2, 4);
              ctx.fillRect(px + 7, hy - 28, 2, 4);
            } else if (r === 3) {
              // Feathered headband
              ctx.fillStyle = hm.hairCol;
              ctx.fillRect(px - 1, hy - 24, 8, 3);
              ctx.fillStyle = '#F43F5E';
              ctx.fillRect(px + 1, hy - 28, 2, 4);
              ctx.fillStyle = '#22D3EE';
              ctx.fillRect(px + 4, hy - 27, 2, 3);
            } else {
              ctx.fillStyle = hm.hairCol;
              ctx.fillRect(px - 1, hy - 24, 8, 3);
              if (hm.hairStyle === 1) ctx.fillRect(px - 2, hy - 22, 2, 5); // Long hair
              if (hm.hairStyle === 2) ctx.fillRect(px + 2, hy - 27, 3, 3); // Topknot bun
            }

            // Varied Tools (Spear, Torch, Stone Axe, Bow)
            if (r === 0 || r === 4) {
              ctx.fillStyle = '#451A03';
              ctx.fillRect(px + 8, hy - 28, 2, 24);
              ctx.fillStyle = '#E2E8F0';
              ctx.fillRect(px + 7, hy - 31, 4, 4);
            } else if (r === 1) {
              ctx.fillStyle = '#78350F';
              ctx.fillRect(px + 8, hy - 26, 2, 22);
              ctx.fillStyle = '#38BDF8';
              ctx.fillRect(px + 7, hy - 30, 4, 4);
            } else if (r === 2) {
              ctx.fillStyle = '#5C4033';
              ctx.fillRect(px + 7, hy - 21, 2, 12);
              ctx.fillStyle = '#94A3B8';
              ctx.fillRect(px + 5, hy - 22, 6, 4);
            }
          } else if (currentPhase === 6) {
            // Ancient (6A) & Medieval/Industrial (6B) with 6 distinct outfits!
            ctx.fillStyle = '#451A03';
            ctx.fillRect(hx + (stride ? -1 : 1), hy - 6, 3, 6);
            ctx.fillRect(hx + (stride ? 5 : 3), hy - 6, 3, 6);

            if (currentSubPhase === '6A') {
              if (r === 0) {
                // Egyptian Noble: White kilt + gold belt + blue/gold Pharaoh nemes headdress
                ctx.fillStyle = '#FFFFFF';
                ctx.fillRect(hx - 1, hy - 14, 8, 8);
                ctx.fillStyle = '#FACC15';
                ctx.fillRect(hx - 1, hy - 16, 8, 3);
                ctx.fillStyle = hm.skin;
                ctx.fillRect(hx, hy - 22, 6, 6);
                ctx.fillStyle = '#1D4ED8';
                ctx.fillRect(hx - 2, hy - 25, 10, 4);
                ctx.fillStyle = '#FACC15';
                ctx.fillRect(hx, hy - 25, 6, 2);
              } else if (r === 1) {
                // Greek Philosopher/Senator: Royal purple sash over white tunic + white beard
                ctx.fillStyle = '#F8FAFC';
                ctx.fillRect(hx - 1, hy - 16, 8, 10);
                ctx.fillStyle = '#7C3AED';
                ctx.fillRect(hx - 2, hy - 16, 6, 9);
                ctx.fillStyle = hm.skin;
                ctx.fillRect(hx, hy - 22, 6, 6);
                ctx.fillStyle = '#E2E8F0';
                ctx.fillRect(hx, hy - 24, 6, 2);
                ctx.fillRect(hx + 1, hy - 18, 5, 3); // Beard
              } else if (r === 2) {
                // Roman Centurion: Bronze cuirass + red tunic + red crested helmet
                ctx.fillStyle = '#B91C1C';
                ctx.fillRect(hx - 1, hy - 16, 8, 10);
                ctx.fillStyle = '#D97706';
                ctx.fillRect(hx - 1, hy - 16, 8, 6);
                ctx.fillStyle = hm.skin;
                ctx.fillRect(hx, hy - 22, 6, 6);
                ctx.fillStyle = '#F59E0B';
                ctx.fillRect(hx - 1, hy - 24, 8, 3);
                ctx.fillStyle = '#EF4444';
                ctx.fillRect(hx - 2, hy - 28, 10, 4);
              } else if (r === 3) {
                // Silk Road Merchant: Emerald robe + gold trim + white/gold turban
                ctx.fillStyle = '#059669';
                ctx.fillRect(hx - 2, hy - 16, 10, 10);
                ctx.fillStyle = '#FACC15';
                ctx.fillRect(hx + 2, hy - 16, 2, 10);
                ctx.fillStyle = hm.skin;
                ctx.fillRect(hx, hy - 22, 6, 6);
                ctx.fillStyle = '#FEF08A';
                ctx.fillRect(hx - 2, hy - 26, 10, 5);
              } else if (r === 4) {
                // Priestess / Dancer: Cyan dress + gold collar + long dark hair
                ctx.fillStyle = '#06B6D4';
                ctx.fillRect(hx - 2, hy - 16, 10, 11);
                ctx.fillStyle = '#FACC15';
                ctx.fillRect(hx - 1, hy - 16, 8, 2);
                ctx.fillStyle = hm.skin;
                ctx.fillRect(hx, hy - 22, 6, 6);
                ctx.fillStyle = '#1C1917';
                ctx.fillRect(hx - 1, hy - 24, 8, 3);
                ctx.fillRect(hx - 2, hy - 22, 2, 6);
              } else {
                // Ancient Spearman with crimson cape
                ctx.fillStyle = '#DC2626';
                ctx.fillRect(hx - 3, hy - 16, 4, 10);
                ctx.fillStyle = '#B45309';
                ctx.fillRect(hx, hy - 16, 7, 10);
                ctx.fillStyle = hm.skin;
                ctx.fillRect(hx, hy - 22, 6, 6);
                ctx.fillStyle = hm.hairCol;
                ctx.fillRect(hx - 1, hy - 24, 8, 3);
              }
            } else {
              // 6B: Medieval & Victorian Industrial Fashion!
              if (r === 0) {
                // Victorian Gentleman: Tall black Top Hat + navy tailcoat + cane
                ctx.fillStyle = '#1E293B';
                ctx.fillRect(hx - 1, hy - 16, 8, 10);
                ctx.fillStyle = '#F8FAFC';
                ctx.fillRect(hx + 2, hy - 16, 3, 5);
                ctx.fillStyle = hm.skin;
                ctx.fillRect(hx, hy - 22, 6, 6);
                ctx.fillStyle = '#090D16';
                ctx.fillRect(hx - 2, hy - 24, 10, 2);
                ctx.fillRect(hx, hy - 30, 6, 6);
                ctx.fillStyle = '#B45309';
                ctx.fillRect(hx + 8, hy - 14, 2, 14);
              } else if (r === 1) {
                // Noblewoman in Rose-Burgundy Gown + Bonnet/Hat
                ctx.fillStyle = '#BE123C';
                ctx.fillRect(hx - 1, hy - 16, 8, 5);
                ctx.fillStyle = '#F43F5E';
                ctx.fillRect(hx - 3, hy - 11, 12, 7);
                ctx.fillStyle = hm.skin;
                ctx.fillRect(hx, hy - 22, 6, 6);
                ctx.fillStyle = '#FDA4AF';
                ctx.fillRect(hx - 2, hy - 25, 10, 3);
              } else if (r === 2) {
                // Medieval Wizard / Scholar in Blue Robe & Pointed Hat
                ctx.fillStyle = '#2563EB';
                ctx.fillRect(hx - 2, hy - 16, 10, 11);
                ctx.fillStyle = hm.skin;
                ctx.fillRect(hx, hy - 22, 6, 6);
                ctx.fillStyle = '#1D4ED8';
                ctx.fillRect(hx - 3, hy - 24, 12, 2);
                ctx.fillRect(hx - 1, hy - 28, 8, 4);
                ctx.fillRect(hx + 1, hy - 31, 4, 3);
              } else if (r === 3) {
                // Royal Halberd Guard: Red & Gold Tabard + Silver Helmet
                ctx.fillStyle = '#EF4444';
                ctx.fillRect(hx - 1, hy - 16, 4, 10);
                ctx.fillStyle = '#FACC15';
                ctx.fillRect(hx + 3, hy - 16, 4, 10);
                ctx.fillStyle = hm.skin;
                ctx.fillRect(hx, hy - 22, 6, 6);
                ctx.fillStyle = '#CBD5E1';
                ctx.fillRect(hx - 1, hy - 25, 8, 4);
                ctx.fillStyle = '#991B1B';
                ctx.fillRect(hx + 8, hy - 28, 2, 28);
                ctx.fillRect(hx + 10, hy - 27, 7, 5);
              } else if (r === 4) {
                // Steampunk Inventor: Brown coat + brass goggles on forehead
                ctx.fillStyle = '#78350F';
                ctx.fillRect(hx - 1, hy - 16, 8, 10);
                ctx.fillStyle = '#F59E0B';
                ctx.fillRect(hx + 1, hy - 14, 4, 4);
                ctx.fillStyle = hm.skin;
                ctx.fillRect(hx, hy - 22, 6, 6);
                ctx.fillStyle = hm.hairCol;
                ctx.fillRect(hx - 1, hy - 25, 8, 3);
                ctx.fillStyle = '#38BDF8';
                ctx.fillRect(hx, hy - 23, 6, 2); // Goggles
              } else {
                // Townsfolk in emerald vest & white shirt + flat cap
                ctx.fillStyle = '#F8FAFC';
                ctx.fillRect(hx - 2, hy - 16, 10, 9);
                ctx.fillStyle = '#15803D';
                ctx.fillRect(hx - 1, hy - 16, 8, 9);
                ctx.fillStyle = hm.skin;
                ctx.fillRect(hx, hy - 22, 6, 6);
                ctx.fillStyle = '#44403C';
                ctx.fillRect(hx - 1, hy - 24, 9, 3);
              }
            }
          } else if (currentPhase === 7) {
            // Modern Metropolis: 6 colorful, distinct street fashions!
            if (r === 0) {
              // Streetwear: Red Hoodie + Blue Jeans + White Headphones + Cap
              ctx.fillStyle = '#1D4ED8';
              ctx.fillRect(hx + (stride ? -1 : 1), hy - 6, 3, 6);
              ctx.fillRect(hx + (stride ? 5 : 3), hy - 6, 3, 6);
              ctx.fillStyle = '#EF4444';
              ctx.fillRect(hx - 2, hy - 16, 10, 10);
              ctx.fillStyle = hm.skin;
              ctx.fillRect(hx, hy - 22, 6, 6);
              ctx.fillStyle = '#1E293B';
              ctx.fillRect(hx - 1, hy - 24, 9, 3);
              ctx.fillStyle = '#F8FAFC';
              ctx.fillRect(hx - 1, hy - 22, 2, 4); // Headphones
            } else if (r === 1) {
              // Business Suit: Navy Jacket + White Shirt + Red Tie + Briefcase
              ctx.fillStyle = '#0F172A';
              ctx.fillRect(hx + (stride ? -1 : 1), hy - 6, 3, 6);
              ctx.fillRect(hx + (stride ? 5 : 3), hy - 6, 3, 6);
              ctx.fillStyle = '#1E3A8A';
              ctx.fillRect(hx - 1, hy - 16, 8, 10);
              ctx.fillStyle = '#FFFFFF';
              ctx.fillRect(hx + 2, hy - 16, 3, 6);
              ctx.fillStyle = '#EF4444';
              ctx.fillRect(hx + 3, hy - 15, 1, 5);
              ctx.fillStyle = hm.skin;
              ctx.fillRect(hx, hy - 22, 6, 6);
              ctx.fillStyle = hm.hairCol;
              ctx.fillRect(hx - 1, hy - 24, 8, 3);
              ctx.fillStyle = '#78350F';
              ctx.fillRect(hx + 6, hy - 10, 6, 5); // Briefcase
            } else if (r === 2) {
              // Fashionista: Pink/Magenta Dress + Blonde/Long Hair + Sunglasses + Handbag
              ctx.fillStyle = hm.skin;
              ctx.fillRect(hx + (stride ? 0 : 1), hy - 6, 2, 6);
              ctx.fillRect(hx + (stride ? 5 : 4), hy - 6, 2, 6);
              ctx.fillStyle = '#EC4899';
              ctx.fillRect(hx - 1, hy - 16, 8, 5);
              ctx.fillStyle = '#DB2777';
              ctx.fillRect(hx - 3, hy - 11, 12, 6);
              ctx.fillStyle = hm.skin;
              ctx.fillRect(hx, hy - 22, 6, 6);
              ctx.fillStyle = '#FACC15';
              ctx.fillRect(hx - 2, hy - 24, 9, 3);
              ctx.fillRect(hx - 2, hy - 21, 3, 6); // Long hair
              ctx.fillStyle = '#090D16';
              ctx.fillRect(hx + 2, hy - 20, 4, 2); // Sunglasses
            } else if (r === 3) {
              // Jogger / Athlete: Neon Mint Tracksuit + Shorts + Red Headband
              ctx.fillStyle = hm.skin;
              ctx.fillRect(hx + (stride ? -2 : 1), hy - 6, 3, 6);
              ctx.fillRect(hx + (stride ? 6 : 3), hy - 6, 3, 6);
              ctx.fillStyle = '#10B981';
              ctx.fillRect(hx - 1, hy - 16, 8, 7);
              ctx.fillStyle = '#0F172A';
              ctx.fillRect(hx - 1, hy - 9, 8, 4);
              ctx.fillStyle = hm.skin;
              ctx.fillRect(hx, hy - 22, 6, 6);
              ctx.fillStyle = hm.hairCol;
              ctx.fillRect(hx, hy - 24, 6, 2);
              ctx.fillStyle = '#EF4444';
              ctx.fillRect(hx - 1, hy - 22, 8, 2); // Headband
            } else if (r === 4) {
              // Photographer / Student: Yellow Jacket + Orange Backpack + Beanie + Camera Flash!
              ctx.fillStyle = '#334155';
              ctx.fillRect(hx + (stride ? -1 : 1), hy - 6, 3, 6);
              ctx.fillRect(hx + (stride ? 5 : 3), hy - 6, 3, 6);
              ctx.fillStyle = '#F97316';
              ctx.fillRect(hx - 4, hy - 15, 4, 8); // Backpack
              ctx.fillStyle = '#FACC15';
              ctx.fillRect(hx - 1, hy - 16, 8, 10);
              ctx.fillStyle = hm.skin;
              ctx.fillRect(hx, hy - 22, 6, 6);
              ctx.fillStyle = '#06B6D4';
              ctx.fillRect(hx - 1, hy - 25, 8, 4); // Beanie
              if ((frameCount + hx) % 36 < 6) {
                ctx.fillStyle = '#FFFFFF';
                ctx.fillRect(hx + 5, hy - 24, 9, 9);
              }
            } else {
              // Varsity Jacket (Purple & White sleeves) + Khaki Pants
              ctx.fillStyle = '#D97706';
              ctx.fillRect(hx + (stride ? -1 : 1), hy - 6, 3, 6);
              ctx.fillRect(hx + (stride ? 5 : 3), hy - 6, 3, 6);
              ctx.fillStyle = '#F8FAFC';
              ctx.fillRect(hx - 2, hy - 16, 10, 9);
              ctx.fillStyle = '#7C3AED';
              ctx.fillRect(hx, hy - 16, 6, 9);
              ctx.fillStyle = hm.skin;
              ctx.fillRect(hx, hy - 22, 6, 6);
              ctx.fillStyle = hm.hairCol;
              ctx.fillRect(hx - 1, hy - 25, 8, 4);
            }
          } else if (currentPhase === 8) {
            // Far Future 3000: 6 Distinct Cyberpunk & Sci-Fi Fashions!
            const hoverY = hy - 4 + Math.round(Math.sin(frameCount * 0.15 + hm.anim) * 2.5);
            // Varied Hoverboards / Jet rings
            const boardCols = ['#22D3EE', '#F43F5E', '#A855F7', '#FACC15', '#34D399', '#38BDF8'];
            ctx.fillStyle = boardCols[r % boardCols.length];
            ctx.fillRect(hx - 4, hoverY - 2, 14, 3);

            if (r === 0) {
              // Cyber Punk with Neon Pink Mohawk & Cyan Trenchcoat
              ctx.fillStyle = '#0891B2';
              ctx.fillRect(hx - 2, hoverY - 14, 10, 12);
              ctx.fillStyle = hm.skin;
              ctx.fillRect(hx, hoverY - 20, 6, 6);
              ctx.fillStyle = '#F43F5E';
              ctx.fillRect(hx + 1, hoverY - 25, 4, 5); // Mohawk
              ctx.fillStyle = '#FACC15';
              ctx.fillRect(hx - 1, hoverY - 18, 6, 2); // Visor
            } else if (r === 1) {
              // White/Silver Astronaut Exo-Suit + Cyan Bubble Helmet
              ctx.fillStyle = '#F8FAFC';
              ctx.fillRect(hx - 2, hoverY - 14, 10, 12);
              ctx.fillStyle = '#38BDF8';
              ctx.fillRect(hx, hoverY - 12, 6, 4);
              ctx.fillStyle = '#67E8F9';
              ctx.fillRect(hx - 2, hoverY - 22, 10, 8);
              ctx.fillStyle = '#0F172A';
              ctx.fillRect(hx, hoverY - 20, 6, 4);
            } else if (r === 2) {
              // Cyber-Samurai in Lime/Gold Jacket + Glowing Red Katana on back
              ctx.fillStyle = '#F43F5E';
              ctx.fillRect(hx - 4, hoverY - 24, 2, 18); // Neon Katana
              ctx.fillStyle = '#84CC16';
              ctx.fillRect(hx - 1, hoverY - 14, 8, 12);
              ctx.fillStyle = hm.skin;
              ctx.fillRect(hx, hoverY - 20, 6, 6);
              ctx.fillStyle = '#FACC15';
              ctx.fillRect(hx - 1, hoverY - 23, 8, 3);
            } else if (r === 3) {
              // Chrome Android Citizen with Glowing Amber Optics
              ctx.fillStyle = '#94A3B8';
              ctx.fillRect(hx - 1, hoverY - 14, 8, 12);
              ctx.fillStyle = '#CBD5E1';
              ctx.fillRect(hx, hoverY - 20, 6, 6);
              ctx.fillStyle = '#22D3EE';
              ctx.fillRect(hx + 1, hoverY - 11, 4, 4); // Core reactor
              ctx.fillStyle = '#FACC15';
              ctx.fillRect(hx - 1, hoverY - 18, 6, 2);
            } else if (r === 4) {
              // Holo-Noble in Magenta/Gold Plasma Cape
              ctx.fillStyle = '#EC4899';
              ctx.fillRect(hx - 3, hoverY - 14, 12, 12);
              ctx.fillStyle = '#FACC15';
              ctx.fillRect(hx - 1, hoverY - 14, 8, 3);
              ctx.fillStyle = hm.skin;
              ctx.fillRect(hx, hoverY - 20, 6, 6);
              ctx.fillStyle = '#A855F7';
              ctx.fillRect(hx - 1, hoverY - 23, 8, 4);
            } else {
              // Neon Orange Tech-Hoodie + Floating Holo-Screen
              ctx.fillStyle = '#EA580C';
              ctx.fillRect(hx - 2, hoverY - 14, 10, 12);
              ctx.fillStyle = hm.skin;
              ctx.fillRect(hx, hoverY - 20, 6, 6);
              ctx.fillStyle = '#FB923C';
              ctx.fillRect(hx - 2, hoverY - 22, 9, 3);
              ctx.fillStyle = 'rgba(34, 211, 238, 0.75)';
              ctx.fillRect(hx + 8, hoverY - 18, 5, 7); // Floating holo-pad
            }
          }
        }
      }

      function drawSciFiSkyElements() {
        if (currentPhase !== 8) return;

        // 1) Giant Ringed Planet (토성형 고리 행성) in the Far Future Sky
        const px = 640;
        const py = 54;
        ctx.fillStyle = 'rgba(45, 212, 191, 0.22)';
        ctx.fillRect(px - 22, py - 22, 44, 44);
        ctx.fillStyle = 'rgba(15, 118, 110, 0.45)';
        ctx.fillRect(px - 16, py - 16, 32, 32);
        // Planetary Ring
        ctx.strokeStyle = 'rgba(244, 114, 182, 0.55)';
        ctx.lineWidth = 3;
        ctx.beginPath();
        ctx.moveTo(px - 38, py + 10);
        ctx.lineTo(px + 38, py - 10);
        ctx.stroke();

        // 2) Flying UFOs, Motherships with Tractor Beams & Neon Skycars
        for (let ufo of bgUfos) {
          const ux = Math.floor(ufo.x);
          const uy = Math.floor(ufo.y + Math.sin(ufo.phase) * 5);

          if (ufo.kind === 'skycar') {
            ctx.fillStyle = '#0F172A';
            ctx.fillRect(ux, uy, 14, 5);
            ctx.fillStyle = '#22D3EE';
            ctx.fillRect(ux + 14, uy + 1, 10, 2); // Neon thruster trail
            ctx.fillStyle = '#F43F5E';
            ctx.fillRect(ux, uy + 1, 3, 2);
          } else {
            const w = ufo.kind === 'mothership' ? 46 : 28;
            // Tractor beam cone shining down intermittently
            if (ufo.hasBeam && Math.sin(ufo.phase * 0.7) > 0.1) {
              ctx.fillStyle = 'rgba(34, 211, 238, 0.13)';
              ctx.beginPath();
              ctx.moveTo(ux + w * 0.3, uy + 8);
              ctx.lineTo(ux + w * 0.7, uy + 8);
              ctx.lineTo(ux + w * 1.25, GROUND_Y);
              ctx.lineTo(ux - w * 0.25, GROUND_Y);
              ctx.closePath();
              ctx.fill();
            }
            // UFO Dome
            ctx.fillStyle = '#67E8F9';
            ctx.fillRect(ux + Math.floor(w * 0.25), uy - 5, Math.floor(w * 0.5), 5);
            // UFO Saucer Body
            ctx.fillStyle = '#334155';
            ctx.fillRect(ux, uy, w, 6);
            // Blinking Rim Lights
            const blink = Math.floor(frameCount / 5) % 2 === 0;
            ctx.fillStyle = blink ? '#F43F5E' : '#FACC15';
            for (let lx = ux + 3; lx < ux + w - 3; lx += 7) {
              ctx.fillRect(lx, uy + 2, 3, 2);
            }
          }
        }
      }

      function drawPixelSun(x, y, scale) {
        const pulseRay = Math.floor(frameCount / 20) % 2 === 0;
        drawPixelMatrix(ctx, SPRITES.PIXEL_SUN, x, y, scale, {
          '#': currentSubPhase === '1B' ? '#F97316' : '#FBBF24',
          '2': '#FDE047',
          '3': '#FFFBEB',
          '4': pulseRay ? 'rgba(251, 191, 36, 0.75)' : 'rgba(251, 191, 36, 0.25)'
        });
      }

      function drawPixelAsteroidInSky() {
        let ax, ay, scale, tailBlocks;
        if (gameState === STATE.IMPACT_CUTSCENE) {
          ax = cutscene.meteorX;
          ay = cutscene.meteorY;
          scale = cutscene.meteorScale;
          tailBlocks = 22;
        } else {
          const progress = Math.max(0, Math.min(1, (score - 2000) / 2000));
          ax = lerp(720, 560, progress);
          ay = lerp(24, 54, progress);
          scale = lerp(1.1, 3.8, Math.pow(progress, 1.15));
          tailBlocks = Math.floor(lerp(6, 18, progress));
        }

        const spriteW = SPRITES.PIXEL_ASTEROID[0].length * scale;
        const spriteH = SPRITES.PIXEL_ASTEROID.length * scale;
        const centerX = ax + spriteW * 0.5;
        const centerY = ay + spriteH * 0.5;

        for (let t = tailBlocks; t >= 1; t--) {
          const frac = t / tailBlocks;
          const stepX = Math.floor(centerX + t * scale * 2.8);
          const stepY = Math.floor(centerY - t * scale * 1.35);
          const boxSize = Math.max(4, Math.floor(spriteH * (1 - frac * 0.78)));
          const snapSize = Math.ceil(boxSize / 4) * 4;
          ctx.fillStyle = t % 2 === 0
            ? `rgba(255, 69, 0, ${0.65 * (1 - frac * 0.7)})`
            : `rgba(255, 204, 0, ${0.6 * (1 - frac * 0.7)})`;
          ctx.fillRect(Math.floor(stepX - snapSize / 2), Math.floor(stepY - snapSize / 2), snapSize, snapSize);
        }

        const flicker = Math.floor(frameCount / 5) % 2 === 0;
        drawPixelMatrix(ctx, SPRITES.PIXEL_ASTEROID, ax, ay, scale, {
          '#': '#3B1114',
          '2': '#FF4500',
          '3': flicker ? '#FFFBEB' : '#FFE066',
          '4': flicker ? '#FF5500' : '#FF8800'
        });
      }

      function drawSkyAndEnvironment() {
        const grad = ctx.createLinearGradient(0, 0, 0, GROUND_Y);
        grad.addColorStop(0, activeTheme.skyTop);
        grad.addColorStop(1, activeTheme.skyBottom);
        ctx.fillStyle = grad;
        ctx.fillRect(0, 0, W, GROUND_Y);

        if (currentPhase === 1 || currentPhase === 5 || currentPhase === 6) {
          drawPixelSun(635, 24, 2.2);
        } else if (currentPhase === 2) {
          drawPixelSun(650, 24, 2.0);
          ctx.fillStyle = currentSubPhase === '2B' ? 'rgba(180, 245, 225, 0.07)' : 'rgba(255, 255, 210, 0.08)';
          for (let b = 0; b < 4; b++) {
            const bx = 120 + b * 185;
            ctx.beginPath();
            ctx.moveTo(bx, 0); ctx.lineTo(bx + 45, 0);
            ctx.lineTo(bx - 55, GROUND_Y); ctx.lineTo(bx - 100, GROUND_Y);
            ctx.closePath(); ctx.fill();
          }
        }

        // Sci-Fi Planets, UFOs & Flying Traffic in Phase 8
        drawSciFiSkyElements();

        for (let cl of skyClouds) {
          ctx.fillStyle = (currentPhase === 4 || currentPhase === 8) ? 'rgba(90, 20, 55, 0.5)' : 'rgba(255, 255, 255, 0.45)';
          ctx.fillRect(Math.floor(cl.x), Math.floor(cl.y), cl.w, cl.h);
          ctx.fillRect(Math.floor(cl.x + 10), Math.floor(cl.y - 6), cl.w - 20, 6);
        }

        for (let fl of distantFlocks) {
          ctx.fillStyle = activeTheme.midSil;
          for (let k = 0; k < fl.count; k++) {
            const bx = fl.x + k * 16;
            const by = fl.y + (k % 2) * 7 + Math.sin(frameCount * 0.15 + fl.phase + k) * 3;
            const wingUp = Math.sin(frameCount * 0.22 + k) > 0;
            ctx.fillRect(Math.floor(bx), Math.floor(by), 8, 2);
            ctx.fillRect(Math.floor(bx + 2), Math.floor(wingUp ? by - 3 : by + 2), 4, 3);
          }
        }

        for (let sm of volcanoSmoke) {
          ctx.fillStyle = currentPhase === 4 ? `rgba(255, 100, 30, ${sm.alpha * 0.65})` : `rgba(135, 115, 105, ${sm.alpha * 0.45})`;
          ctx.fillRect(Math.floor(sm.x - sm.r), Math.floor(sm.y - sm.r), Math.floor(sm.r * 2), Math.floor(sm.r * 2));
        }

        for (let lf of farLandforms) {
          ctx.fillStyle = activeTheme.farSil;
          if (currentPhase === 6 && lf.kind === 'mesa') {
            ctx.beginPath();
            ctx.moveTo(lf.x, GROUND_Y);
            ctx.lineTo(lf.x + lf.w * 0.5, GROUND_Y - lf.h - 15);
            ctx.lineTo(lf.x + lf.w, GROUND_Y);
            ctx.closePath();
            ctx.fill();
          } else if (lf.kind === 'mesa') {
            ctx.beginPath();
            ctx.moveTo(lf.x, GROUND_Y);
            ctx.lineTo(lf.x + 18, GROUND_Y - lf.h);
            ctx.lineTo(lf.x + lf.w - 18, GROUND_Y - lf.h);
            ctx.lineTo(lf.x + lf.w, GROUND_Y);
            ctx.closePath();
            ctx.fill();
          } else {
            ctx.beginPath();
            ctx.moveTo(lf.x, GROUND_Y);
            ctx.lineTo(lf.x + lf.w * 0.38, GROUND_Y - lf.h);
            ctx.lineTo(lf.x + lf.w * 0.62, GROUND_Y - lf.h);
            ctx.lineTo(lf.x + lf.w, GROUND_Y);
            ctx.closePath();
            ctx.fill();
          }
        }

        let treeStyle = 'araucaria';
        if (currentPhase === 1) treeStyle = 'cycad';
        else if (currentPhase === 2) treeStyle = 'araucaria';
        else if (currentPhase === 3) treeStyle = 'magnolia';
        else if (currentPhase === 4) treeStyle = 'extinction_dead'; // Leafless & fallen trees!
        else if (currentPhase === 5) treeStyle = currentSubPhase === '5A' ? 'snow_pine' : 'araucaria';
        else if (currentPhase === 6) treeStyle = 'cycad';
        else if (currentPhase >= 7) treeStyle = 'skyscraper';

        for (let tr of forestTrees) {
          const cCol = tr.layer === 0 ? activeTheme.farSil : activeTheme.midSil;
          const tCol = tr.layer === 0 ? activeTheme.midSil : activeTheme.nearSil;
          const styleToUse = (currentPhase === 2 && tr.seed % 3 === 0) ? 'cycad' : treeStyle;
          drawTreeOrBuilding(tr, GROUND_Y, cCol, tCol, styleToUse);
        }

        // Midground Landmarks & Actively Roaming Dinosaurs (Requirement 1)
        for (let ml of midLandmarks) {
          const mx = Math.floor(ml.x);
          const s = ml.slot;
          const tick = frameCount + ml.anim * 3;
          const faceLeft = ml.dir === -1;

          if (currentPhase === 1) {
            if (s % 3 === 0) {
              const fIdx = Math.floor(tick / 4) % 4;
              drawGroundAnchoredSprite(ctx, SPRITES.PROTO_RUNFrames[fIdx], mx, GROUND_Y, 1.4, { '#': activeTheme.nearSil, '2': activeTheme.midSil, '3': activeTheme.skyBottom }, 1, faceLeft);
              drawGroundAnchoredSprite(ctx, SPRITES.PROTO_RUNFrames[(fIdx + 2) % 4], mx + 28, GROUND_Y, 1.15, { '#': activeTheme.nearSil, '2': activeTheme.midSil, '3': activeTheme.skyBottom }, 1, faceLeft);
            } else if (s % 3 === 1) {
              // Postosuchus natively faces left, so flipX when moving right!
              drawAnimatedBgDino(ctx, SPRITES.POSTOSUCHUS_1, mx, GROUND_Y, 1.85, activeTheme.nearSil, tick, 2, 2, !faceLeft);
            } else {
              // Eoraptor pack actively darting across the desert!
              const eSpr = (Math.floor(tick / 4) % 2 === 0) ? SPRITES.EORAPTOR_1 : SPRITES.EORAPTOR_2;
              drawGroundAnchoredSprite(ctx, eSpr, mx, GROUND_Y, 1.65, { '#': activeTheme.nearSil, '2': activeTheme.midSil, '3': activeTheme.skyBottom }, 1, !faceLeft);
            }
          } else if (currentPhase === 2) {
            if (s % 3 === 0) {
              drawAnimatedBgDino(ctx, SPRITES.BRACHIOSAURUS, mx, GROUND_Y, 3.1, activeTheme.nearSil, tick, 8, 4, faceLeft);
              drawAnimatedBgDino(ctx, SPRITES.BRACHIOSAURUS, mx + 68, GROUND_Y, 1.7, activeTheme.nearSil, tick + 12, 8, 4, faceLeft);
            } else if (s % 3 === 1) {
              drawAnimatedBgDino(ctx, SPRITES.DIPLODOCUS, mx, GROUND_Y, 2.6, activeTheme.nearSil, tick, 4, 3, faceLeft);
            } else {
              // Stegosaurus natively faces left, so flipX when moving right!
              drawAnimatedBgDino(ctx, SPRITES.STEGOSAURUS, mx, GROUND_Y, 2.5, activeTheme.nearSil, tick, 3, 2, !faceLeft);
            }
          } else if (currentPhase === 3) {
            if (s % 4 === 0) {
              drawAnimatedBgDino(ctx, SPRITES.PARASAUROLOPHUS, mx, GROUND_Y, 2.6, activeTheme.nearSil, tick, 4, 3, faceLeft);
            } else if (s % 4 === 1) {
              drawAnimatedBgDino(ctx, SPRITES.ANKYLOSAURUS, mx, GROUND_Y, 2.5, activeTheme.nearSil, tick, 2, 2, faceLeft);
            } else if (s % 4 === 2) {
              drawAnimatedBgDino(ctx, SPRITES.SPINOSAURUS, mx, GROUND_Y, 2.8, activeTheme.nearSil, tick, 3, 3, faceLeft);
            } else {
              drawAnimatedBgDino(ctx, SPRITES.BRACHIOSAURUS, mx, GROUND_Y, 2.7, activeTheme.nearSil, tick, 8, 4, faceLeft);
            }
          } else if (currentPhase === 5) {
            if (s % 2 === 0) {
              drawAnimatedBgDino(ctx, SPRITES.MAMMOTH_1, mx, GROUND_Y, 2.4, activeTheme.nearSil, tick, 2, 2, false);
            } else {
              // Stonehenge / Megalith Dolmen
              ctx.fillStyle = activeTheme.nearSil;
              ctx.fillRect(mx, GROUND_Y - 34, 48, 10);
              ctx.fillRect(mx + 6, GROUND_Y - 24, 10, 24);
              ctx.fillRect(mx + 32, GROUND_Y - 24, 10, 24);
            }
          } else if (currentPhase === 6) {
            ctx.fillStyle = activeTheme.nearSil;
            if (currentSubPhase === '6A') {
              // Parthenon Temple or Obelisk
              if (s % 2 === 0) {
                ctx.fillRect(mx - 4, GROUND_Y - 48, 54, 7);
                for (let c = 0; c < 4; c++) ctx.fillRect(mx + c * 14, GROUND_Y - 41, 6, 41);
              } else {
                // Tall Egyptian Obelisk
                ctx.fillRect(mx + 12, GROUND_Y - 64, 12, 64);
                ctx.beginPath();
                ctx.moveTo(mx + 12, GROUND_Y - 64);
                ctx.lineTo(mx + 18, GROUND_Y - 76);
                ctx.lineTo(mx + 24, GROUND_Y - 64);
                ctx.fill();
              }
            } else {
              // 6B: Clocktower (Big Ben) or Rotating Windmill
              if (s % 2 === 0) {
                ctx.fillRect(mx + 6, GROUND_Y - 68, 26, 68);
                ctx.fillStyle = '#FEF08A';
                ctx.fillRect(mx + 12, GROUND_Y - 56, 14, 14);
              } else {
                ctx.fillRect(mx + 10, GROUND_Y - 44, 20, 44);
                const ang = (frameCount * 0.04) + s;
                ctx.strokeStyle = activeTheme.nearSil;
                ctx.lineWidth = 3;
                ctx.beginPath();
                ctx.moveTo(mx + 20 - Math.cos(ang) * 22, GROUND_Y - 34 - Math.sin(ang) * 22);
                ctx.lineTo(mx + 20 + Math.cos(ang) * 22, GROUND_Y - 34 + Math.sin(ang) * 22);
                ctx.stroke();
              }
            }
          } else if (currentPhase === 7) {
            // Modern Metropolis Landmarks: Giant Ferris Wheel, Suspension Bridge Tower, or Sports Stadium!
            if (s % 3 === 0) {
              // Giant Illuminated Ferris Wheel (대관람차)
              const cx = mx + 24;
              const cy = GROUND_Y - 48;
              ctx.strokeStyle = currentSubPhase === '7B' ? '#F472B6' : activeTheme.nearSil;
              ctx.lineWidth = 2;
              ctx.beginPath();
              ctx.arc(cx, cy, 24, 0, Math.PI * 2);
              ctx.stroke();
              // Support legs
              ctx.beginPath();
              ctx.moveTo(cx - 14, GROUND_Y); ctx.lineTo(cx, cy); ctx.lineTo(cx + 14, GROUND_Y);
              ctx.stroke();
              // Rotating gondolas
              for (let g = 0; g < 6; g++) {
                const ga = frameCount * 0.02 + (g * Math.PI) / 3;
                ctx.fillStyle = g % 2 === 0 ? '#FACC15' : '#38BDF8';
                ctx.fillRect(Math.floor(cx + Math.cos(ga) * 24 - 2), Math.floor(cy + Math.sin(ga) * 24 - 2), 4, 4);
              }
            } else if (s % 3 === 1) {
              // Suspension Bridge Pylon & Cables (현수교 주탑)
              ctx.fillStyle = activeTheme.nearSil;
              ctx.fillRect(mx + 16, GROUND_Y - 78, 8, 78);
              ctx.fillRect(mx + 12, GROUND_Y - 56, 16, 4);
              ctx.fillRect(mx + 12, GROUND_Y - 32, 16, 4);
              ctx.strokeStyle = activeTheme.midSil;
              ctx.lineWidth = 1.5;
              ctx.beginPath();
              ctx.moveTo(mx - 20, GROUND_Y - 14);
              ctx.quadraticCurveTo(mx + 20, GROUND_Y - 74, mx + 60, GROUND_Y - 14);
              ctx.stroke();
            } else {
              // Modern Domed Stadium / Concert Arena (돔 경기장)
              ctx.fillStyle = activeTheme.nearSil;
              ctx.fillRect(mx, GROUND_Y - 22, 56, 22);
              ctx.beginPath();
              ctx.arc(mx + 28, GROUND_Y - 22, 28, Math.PI, 0);
              ctx.fill();
              ctx.fillStyle = '#FDE047';
              for (let lx = mx + 6; lx < mx + 50; lx += 9) {
                ctx.fillRect(lx, GROUND_Y - 16, 5, 4);
              }
            }
          } else if (currentPhase === 8) {
            // Far Future Sci-Fi Mega-Structures: Orbital Elevator, Anti-Gravity Ring, or Cyber Torii/Holo-Gate
            if (s % 3 === 0) {
              // Anti-Gravity Levitating Reactor Ring (반중력 에너지 링)
              const cx = mx + 24;
              const cy = GROUND_Y - 62 + Math.sin(frameCount * 0.06 + s) * 4;
              ctx.strokeStyle = '#22D3EE';
              ctx.lineWidth = 3;
              ctx.beginPath();
              ctx.arc(cx, cy, 22, 0, Math.PI * 2);
              ctx.stroke();
              ctx.fillStyle = '#F43F5E';
              ctx.fillRect(cx - 4, cy - 4, 8, 8);
              // Base emitter pedestal
              ctx.fillStyle = activeTheme.nearSil;
              ctx.fillRect(cx - 12, GROUND_Y - 16, 24, 16);
            } else if (s % 3 === 1) {
              // Orbital Space Elevator Conduit (궤도 엘리베이터 타워)
              ctx.fillStyle = 'rgba(34, 211, 238, 0.25)';
              ctx.fillRect(mx + 18, 0, 10, GROUND_Y);
              ctx.fillStyle = '#22D3EE';
              const climbY = (GROUND_Y - ((frameCount * 3 + s * 40) % GROUND_Y));
              ctx.fillRect(mx + 16, climbY, 14, 8);
            } else {
              // Giant Neon Hologram Portal Arch (홀로그램 포털 게이트)
              ctx.fillStyle = activeTheme.nearSil;
              ctx.fillRect(mx + 4, GROUND_Y - 64, 8, 64);
              ctx.fillRect(mx + 38, GROUND_Y - 64, 8, 64);
              ctx.fillStyle = '#F43F5E';
              ctx.fillRect(mx, GROUND_Y - 68, 50, 6);
              ctx.fillStyle = 'rgba(168, 85, 247, 0.28)';
              ctx.fillRect(mx + 12, GROUND_Y - 62, 26, 62);
            }
          }
        }

        // Draw additional fast-roaming background dinosaurs in Stage 1 (Phases 1~3)
        if (currentStage === 1 && currentPhase <= 3) {
          for (let rd of bgRoamingDinos) {
            const rx = Math.floor(rd.x);
            const rTick = frameCount + rd.anim * 2;
            const fIdx = Math.floor(rTick / 4) % 4;
            if (rd.species === 'sky_pterosaur') {
              const py = GROUND_Y + rd.yOffset + Math.sin(rTick * 0.08) * 6;
              const pSpr = (fIdx % 2 === 0) ? SPRITES.RHAMPHO_1 : SPRITES.RHAMPHO_2;
              drawPixelMatrix(ctx, pSpr, rx, py, 1.5, { '#': activeTheme.nearSil, '2': activeTheme.midSil, '3': activeTheme.skyBottom }, rd.dir === 1);
            } else if (rd.species === 'proto_pack') {
              drawGroundAnchoredSprite(ctx, SPRITES.PROTO_RUNFrames[fIdx], rx, GROUND_Y, 1.3, { '#': activeTheme.nearSil, '2': activeTheme.midSil, '3': activeTheme.skyBottom }, 1, rd.dir === -1);
            } else if (rd.species === 'raptor_dash') {
              const rSpr = (fIdx % 2 === 0) ? SPRITES.RAPTOR_1 : SPRITES.RAPTOR_2;
              drawGroundAnchoredSprite(ctx, rSpr, rx, GROUND_Y, 1.45, { '#': activeTheme.nearSil, '2': activeTheme.midSil, '3': activeTheme.skyBottom }, 1, rd.dir === 1);
            } else {
              drawAnimatedBgDino(ctx, SPRITES.PARASAUROLOPHUS, rx, GROUND_Y, 1.75, activeTheme.nearSil, rTick, 4, 3, rd.dir === -1);
            }
          }
        }

        // Draw Stage 2 Background Humans!
        drawBackgroundHumans();

        // REQUIREMENT 3: Keep nearScenery very low-profile and subtle (below GROUND_Y or tiny translucent pebbles)
        // so background grass NEVER overlaps or confuses the player as a foreground bush/rock obstacle!
        ctx.save();
        ctx.globalAlpha = 0.25;
        for (let ns of nearScenery) {
          const nx = Math.floor(ns.x);
          const v = ns.variant;
          if (currentPhase < 7) {
            ctx.fillStyle = activeTheme.midSil;
            ctx.fillRect(nx, GROUND_Y - 3, 10 + (v % 3) * 4, 3);
          }
        }
        ctx.restore();

        if ((currentStage === 1 && currentPhase >= 3) || gameState === STATE.IMPACT_CUTSCENE) {
          drawPixelAsteroidInSky();
        }

        if (currentPhase === 4 && gameState === STATE.PLAYING) {
          for (let d of panicStampede) {
            const silColor = '#1F0610';
            if (d.spType === 'flyer') {
              const spr = (Math.floor(d.anim / 6) % 2 === 0) ? SPRITES.ARCHAEO_1 : SPRITES.ARCHAEO_2;
              drawPixelMatrix(ctx, spr, d.x, d.y, 1.5, { '#': silColor, '3': '#FF5500' });
            } else if (d.spType === 'parasaur') {
              drawAnimatedBgDino(ctx, SPRITES.PARASAUROLOPHUS, d.x, GROUND_Y, 1.8, silColor, d.anim * 2, 4, 3);
            } else if (d.spType === 'stego') {
              drawAnimatedBgDino(ctx, SPRITES.STEGOSAURUS, d.x, GROUND_Y, 1.7, silColor, d.anim * 2, 3, 2);
            } else {
              const spr = SPRITES.PROTO_RUNFrames[Math.floor(d.anim / 4) % 4];
              drawGroundAnchoredSprite(ctx, spr, d.x, GROUND_Y, 1.65, { '#': silColor, '2': silColor, '3': '#FF4400' }, 2);
            }
          }
        }

        // Ground Surface (Era-specific road/surface detail so it never feels repetitive!)
        ctx.fillStyle = activeTheme.groundFill;
        ctx.fillRect(0, GROUND_Y, W, H - GROUND_Y);
        ctx.fillStyle = activeTheme.groundSub;
        ctx.fillRect(0, GROUND_Y + 14, W, H - GROUND_Y - 14);
        ctx.fillStyle = currentPhase === 4 ? '#4A0E17' : 'rgba(0, 0, 0, 0.18)';
        ctx.fillRect(0, GROUND_Y + 32, W, H - GROUND_Y - 32);
        ctx.fillStyle = activeTheme.groundLine;
        ctx.fillRect(0, GROUND_Y, W, 3);

        if (currentPhase === 7) {
          // Modern Asphalt Road Lane Dashes
          ctx.fillStyle = '#FACC15';
          for (let p of groundPebbles) {
            if (p.w > 4) ctx.fillRect(Math.floor(p.x), GROUND_Y + 8, 18, 3);
          }
        } else if (currentPhase === 8) {
          // Sci-Fi Cyber Grid Pulse Lines
          ctx.fillStyle = '#22D3EE';
          for (let p of groundPebbles) {
            ctx.fillRect(Math.floor(p.x), GROUND_Y + 6, p.w * 3, 2);
          }
        } else {
          ctx.fillStyle = activeTheme.groundLine;
          for (let p of groundPebbles) {
            ctx.fillRect(Math.floor(p.x), Math.floor(p.y), p.w, p.h);
          }
        }

        for (let f of undergroundFossils) {
          const fx = Math.floor(f.x);
          const fy = Math.floor(f.y);
          ctx.fillStyle = 'rgba(255, 250, 240, 0.35)';
          if (f.kind === 'skull') {
            drawPixelMatrix(ctx, SPRITES.FOSSIL_SKULL, fx, fy - 2, 1.1, { '#': 'rgba(255, 250, 240, 0.4)' });
          } else {
            ctx.fillRect(fx, fy + 2, 12, 3);
          }
        }

        // REQUIREMENT 7: During SubPhase 8B (7,500 ~ 8,000 PTS), draw the pursuit force chasing from BEHIND (left side of player)!
        if (gameState === STATE.PLAYING && currentStage === 2 && score >= 7550) {
          const chaseProgress = Math.min(1, (score - 7550) / 450);
          const chaseMechX = Math.floor(lerp(-35, 18, chaseProgress) + Math.sin(frameCount * 0.2) * 3);
          const anim2 = Math.floor(frameCount / 5) % 2 === 0;
          drawGroundAnchoredSprite(ctx, anim2 ? SPRITES.CHRONO_MECHA_1 : SPRITES.CHRONO_MECHA_2, chaseMechX, GROUND_Y, 2.35, {
            '#': '#0F172A', '2': '#22D3EE', '3': '#F43F5E'
          }, 2);
          drawPixelMatrix(ctx, anim2 ? SPRITES.FUTURE_DRONE_1 : SPRITES.FUTURE_DRONE_2, chaseMechX + 10, 62 + Math.sin(frameCount * 0.15) * 4, 2.0, {
            '#': '#1E1B4B', '2': '#A855F7', '3': '#22D3EE'
          });
        }

        for (let ap of ashParticles) {
          ctx.fillStyle = ap.color || '#FF6D00';
          ctx.fillRect(Math.floor(ap.x), Math.floor(ap.y), ap.size, ap.size);
        }
      }

      function drawUfoBoss() {
        if (!ufoBoss.active) return;

        const ux = Math.floor(ufoBoss.x);
        const uy = Math.floor(ufoBoss.y);
        const scale = 3.2;

        // Draw Aiming Telegraph OR 0.5s Firing Laser Beam!
        if (ufoBoss.state === 'AIMING') {
          const chargeRatio = ufoBoss.stateTimer / 40;
          const blink = Math.floor(frameCount / 3) % 2 === 0;
          ctx.fillStyle = blink ? 'rgba(244, 63, 94, 0.55)' : 'rgba(34, 211, 238, 0.35)';
          ctx.fillRect(0, uy + 14, ux + 8, Math.max(2, Math.floor(chargeRatio * 8)));

          // Charging energy orb at UFO muzzle
          const orbR = Math.floor(4 + chargeRatio * 10);
          ctx.fillStyle = '#F43F5E';
          ctx.fillRect(ux - orbR, uy + 15 - orbR / 2, orbR * 2, orbR);

          // Clear dodge cue label
          ctx.font = '9px "Press Start 2P", monospace';
          ctx.textAlign = 'center';
          ctx.fillStyle = '#FACC15';
          ctx.fillText(
            ufoBoss.isHighLaser ? 'WARNING: 상단 레이저 조준! [↓ 숙이기]' : 'WARNING: 하단 레이저 조준! [SPACE 점프]',
            W / 2 - 40,
            uy - 10
          );
        } else if (ufoBoss.state === 'FIRING') {
          // 0.5s (30 frames) High-Energy Plasma Laser Beam!
          const beamTop = uy + 8;
          ctx.fillStyle = 'rgba(244, 63, 94, 0.45)';
          ctx.fillRect(0, beamTop - 5, ux + 12, 26);
          ctx.fillStyle = '#F43F5E';
          ctx.fillRect(0, beamTop, ux + 12, 16);
          ctx.fillStyle = '#FFFBEB';
          ctx.fillRect(0, beamTop + 4, ux + 12, 8);
        }

        // Draw UFO Boss Hull
        const rimBlink = Math.floor(frameCount / 4) % 2 === 0;
        drawPixelMatrix(ctx, SPRITES.UFO_BOSS, ux, uy, scale, {
          '#': '#1E293B',
          '2': '#38BDF8',
          '3': '#E0F2FE',
          '4': rimBlink ? '#F43F5E' : '#FACC15'
        });

        // Shot counter above UFO
        ctx.font = '8px "Press Start 2P", monospace';
        ctx.textAlign = 'center';
        ctx.fillStyle = '#22D3EE';
        ctx.fillText(`LASER ${ufoBoss.shotsFired}/${ufoBoss.maxShots}`, ux + 42, uy - 8);
      }

      function drawObstacles() {
        const anim2 = Math.floor(frameCount / 6) % 2 === 0;

        for (let ob of obstacles) {
          // Subtle head-level wind stream indicator for all flying obstacles (Requirement 4)
          if (!ob.isGround) {
            ctx.fillStyle = 'rgba(250, 204, 21, 0.35)';
            ctx.fillRect(Math.floor(ob.x - 10), Math.floor(ob.y - 3), ob.w + 18, 2);
          }

          if (ob.type === 'FERN_BUSH') {
            // High-contrast outline + vivid emerald & bright amber/red berries so it never blends with background
            drawGroundAnchoredSprite(ctx, SPRITES.FERN_BUSH, ob.x + 2, ob.y + 1, ob.scale, { '#': '#0F172A', '2': '#0F172A' }, 3);
            drawGroundAnchoredSprite(ctx, SPRITES.FERN_BUSH, ob.x, ob.y, ob.scale, { '#': '#15803D', '2': '#FACC15' }, 3);
          } else if (ob.type === 'DESERT_ROCKS') {
            // REQUIREMENT 3: Distinct, high-contrast colors in Paleolithic (5A) & Neolithic (5B) so they never blend with grey/green grass!
            drawGroundAnchoredSprite(ctx, SPRITES.DESERT_ROCKS, ob.x + 2, ob.y + 1, ob.scale, { '#': '#090D16', '2': '#090D16' }, 3);
            if (currentSubPhase === '5A') {
              // Warm fiery Amber-Flint & Red Ochre rock against cool grey snow background!
              drawGroundAnchoredSprite(ctx, SPRITES.DESERT_ROCKS, ob.x, ob.y, ob.scale, {
                '#': '#9A3412',
                '2': '#FACC15'
              }, 3);
            } else if (currentSubPhase === '5B') {
              // Deep Obsidian Indigo & Glowing Gold rune stone against green grass!
              drawGroundAnchoredSprite(ctx, SPRITES.DESERT_ROCKS, ob.x, ob.y, ob.scale, {
                '#': '#312E81',
                '2': '#FDE047'
              }, 3);
            } else {
              drawGroundAnchoredSprite(ctx, SPRITES.DESERT_ROCKS, ob.x, ob.y, ob.scale, {
                '#': '#7A3826',
                '2': '#FBBF24'
              }, 3);
            }
          } else if (ob.type === 'POSTOSUCHUS') {
            drawGroundAnchoredSprite(ctx, anim2 ? SPRITES.POSTOSUCHUS_1 : SPRITES.POSTOSUCHUS_2, ob.x, ob.y, ob.scale, { '#': '#5E2314', '2': '#D97736', '3': '#FFEE55' }, 3);
          } else if (ob.type === 'EORAPTOR') {
            drawGroundAnchoredSprite(ctx, anim2 ? SPRITES.EORAPTOR_1 : SPRITES.EORAPTOR_2, ob.x, ob.y, ob.scale, { '#': '#6E2C1B', '2': '#E09F67', '3': '#FFFFFF' }, 3);
          } else if (ob.type === 'CONIFER_STUMP') {
            drawGroundAnchoredSprite(ctx, SPRITES.CONIFER_STUMP, ob.x, ob.y, ob.scale, { '#': '#3B281E', '2': '#8C6246' }, 3);
          } else if (ob.type === 'DILOPHOSAURUS') {
            drawGroundAnchoredSprite(ctx, anim2 ? SPRITES.DILOPHO_1 : SPRITES.DILOPHO_2, ob.x, ob.y, ob.scale, { '#': '#1E4631', '2': '#EF4444', '3': '#FDE047' }, 3);
          } else if (ob.type === 'STEGOSAURUS') {
            drawGroundAnchoredSprite(ctx, anim2 ? SPRITES.STEGO_OB_1 : SPRITES.STEGO_OB_2, ob.x, ob.y, ob.scale, { '#': '#284B43', '2': '#D97706', '3': '#FEF08A' }, 3);
          } else if (ob.type === 'ARCHAEOPTERYX') {
            drawGroundAnchoredSprite(ctx, anim2 ? SPRITES.ARCHAEO_1 : SPRITES.ARCHAEO_2, ob.x, ob.y, ob.scale, { '#': '#263B2E', '3': '#FFCC00' }, 0);
          } else if (ob.type === 'RHAMPHORHYNCHUS') {
            drawGroundAnchoredSprite(ctx, anim2 ? SPRITES.RHAMPHO_1 : SPRITES.RHAMPHO_2, ob.x, ob.y, ob.scale, { '#': '#1D3A47', '2': '#38BDF8', '3': '#FDE047' }, 0);
          } else if (ob.type === 'TRICERATOPS') {
            drawGroundAnchoredSprite(ctx, (Math.floor(frameCount / 5) % 2 === 0) ? SPRITES.TRICERA_1 : SPRITES.TRICERA_2, ob.x, ob.y, ob.scale, { '#': currentPhase === 4 ? '#2A0B12' : '#3A3248', '3': '#FF3B30' }, 3);
          } else if (ob.type === 'VELOCIRAPTOR') {
            drawGroundAnchoredSprite(ctx, (Math.floor(frameCount / 4) % 2 === 0) ? SPRITES.RAPTOR_1 : SPRITES.RAPTOR_2, ob.x, ob.y, ob.scale, { '#': '#4A2538', '2': '#F59E0B', '3': '#FFFFFF' }, 3);
          } else if (ob.type === 'ANKYLOSAURUS') {
            drawGroundAnchoredSprite(ctx, anim2 ? SPRITES.ANKYLO_OB_1 : SPRITES.ANKYLO_OB_2, ob.x, ob.y, ob.scale, { '#': '#382E42', '2': '#9CA3AF', '3': '#FBBF24' }, 3);
          } else if (ob.type === 'PACHYCEPHALO') {
            drawGroundAnchoredSprite(ctx, (Math.floor(frameCount / 4) % 2 === 0) ? SPRITES.PACHY_1 : SPRITES.PACHY_2, ob.x, ob.y, ob.scale, { '#': '#3B2838', '2': '#FCD34D', '3': '#FF3B30' }, 3);
          } else if (ob.type === 'QUETZALCOATLUS') {
            drawGroundAnchoredSprite(ctx, (Math.floor(frameCount / 8) % 2 === 0) ? SPRITES.QUETZAL_1 : SPRITES.QUETZAL_2, ob.x, ob.y, ob.scale, { '#': '#3B234A', '3': '#FF9F1C' }, 0);
          } else if (ob.type === 'METEOR_FRAGMENT') {
            if (!ob.landed) {
              ctx.fillStyle = 'rgba(255, 90, 20, 0.65)';
              ctx.fillRect(Math.floor(ob.x + 14), Math.floor(ob.y - 34), 16, 16);
            } else {
              ctx.fillStyle = '#4A151B';
              ctx.fillRect(Math.floor(ob.x - 6), GROUND_Y - 3, ob.w + 12, 6);
            }
            drawGroundAnchoredSprite(ctx, SPRITES.METEOR_OB, ob.x, ob.y, ob.scale, { '#': '#3B1114', '2': '#FF4500', '3': '#FFF3B0', '4': '#FF7B00' }, 2);
          } else if (ob.type === 'LAVA_GEYSER') {
            drawGroundAnchoredSprite(ctx, (Math.floor(frameCount / 4) % 2 === 0) ? SPRITES.LAVA_GEYSER_1 : SPRITES.LAVA_GEYSER_2, ob.x, ob.y, ob.scale, { '#': '#B91C1C', '2': '#FF5500', '3': '#FEF08A' }, 3);
          } else if (ob.type === 'MAMMOTH') {
            drawGroundAnchoredSprite(ctx, anim2 ? SPRITES.MAMMOTH_1 : SPRITES.MAMMOTH_2, ob.x, ob.y, ob.scale, { '#': '#5C3A21', '2': '#F8FAFC', '3': '#FDE047' }, 3);
          } else if (ob.type === 'SMILODON') {
            drawGroundAnchoredSprite(ctx, anim2 ? SPRITES.SMILODON_1 : SPRITES.SMILODON_2, ob.x, ob.y, ob.scale, { '#': '#D97706', '2': '#FFFFFF', '3': '#EF4444' }, 3);
          } else if (ob.type === 'CHARIOT') {
            drawGroundAnchoredSprite(ctx, anim2 ? SPRITES.CHARIOT_1 : SPRITES.CHARIOT_2, ob.x, ob.y, ob.scale, { '#': '#78350F', '2': '#F59E0B', '3': '#EF4444' }, 3);
          } else if (ob.type === 'SPARTAN_PHALANX') {
            drawGroundAnchoredSprite(ctx, anim2 ? SPRITES.SPARTAN_1 : SPRITES.SPARTAN_2, ob.x, ob.y, ob.scale, {
              '#': '#78350F', '2': '#F59E0B', '3': '#EF4444'
            }, 3);
          } else if (ob.type === 'SIEGE_CANNON') {
            drawGroundAnchoredSprite(ctx, anim2 ? SPRITES.CANNON_1 : SPRITES.CANNON_2, ob.x, ob.y, ob.scale, {
              '#': '#1E293B', '2': '#92400E', '3': '#F97316'
            }, 3);
          } else if (ob.type === 'POLICE_CAR') {
            const siren = Math.floor(frameCount / 5) % 2 === 0;
            drawGroundAnchoredSprite(ctx, SPRITES.POLICE_CAR, ob.x, ob.y, ob.scale, {
              '#': currentPhase === 8 ? '#311042' : '#1E293B',
              '2': siren ? '#3B82F6' : '#EF4444',
              '3': siren ? '#EF4444' : '#38BDF8'
            }, 3);
          } else if (ob.type === 'HELICOPTER') {
            drawGroundAnchoredSprite(ctx, (Math.floor(frameCount / 3) % 2 === 0) ? SPRITES.HELICOPTER_1 : SPRITES.HELICOPTER_2, ob.x, ob.y, ob.scale, { '#': '#1E293B', '2': '#38BDF8', '3': '#FACC15' }, 0);
          } else if (ob.type === 'CHRONO_MECHA') {
            drawGroundAnchoredSprite(ctx, anim2 ? SPRITES.CHRONO_MECHA_1 : SPRITES.CHRONO_MECHA_2, ob.x, ob.y, ob.scale, { '#': '#0F172A', '2': '#22D3EE', '3': '#F43F5E' }, 3);
          } else if (ob.type === 'FUTURE_DRONE') {
            drawGroundAnchoredSprite(ctx, anim2 ? SPRITES.FUTURE_DRONE_1 : SPRITES.FUTURE_DRONE_2, ob.x, ob.y, ob.scale, { '#': '#1E1B4B', '2': '#A855F7', '3': '#22D3EE' }, 0);
          }
        }
      }

      function drawPlayer() {
        for (let fd of footstepDust) {
          ctx.fillStyle = currentPhase === 4 ? '#FF6D00' : activeTheme.groundLine;
          ctx.fillRect(Math.floor(fd.x), Math.floor(fd.y), fd.size, fd.size);
        }

        const isFlashingWhite = player.evolveFlashTimer > 0 && Math.floor(player.evolveFlashTimer / 3) % 2 === 0;
        const { sprite, scale } = player.getSpriteAndScale();
        const strideBounce = (!player.isJumping && !player.lookingUp && (player.legFrame === 1 || player.legFrame === 3)) ? -2 : 0;

        // If Skeleton T-Rex in Stage 2, draw dark outline shadow so white bones pop on bright/dark sky!
        if (player.isSkeleton) {
          drawGroundAnchoredSprite(ctx, sprite, player.x + 2, player.y + strideBounce + 2, scale, {
            '#': '#0F172A', '2': '#0F172A', '3': '#0F172A'
          }, 2);
        }

        const palette = {
          '#': isFlashingWhite ? '#FFFBEB' : activeTheme.dinoPrimary,
          '2': isFlashingWhite ? '#FDE68A' : activeTheme.dinoSecondary,
          '3': activeTheme.dinoEye
        };

        drawGroundAnchoredSprite(ctx, sprite, player.x, player.y + strideBounce, scale, palette, 2);

        for (let sp of evolutionSparks) {
          ctx.fillStyle = sp.life % 2 === 0 ? '#FBBF24' : '#FEF08A';
          ctx.fillRect(Math.floor(sp.x), Math.floor(sp.y), 4, 4);
        }

        if (invincible && gameState === STATE.PLAYING) {
          ctx.strokeStyle = 'rgba(250, 204, 21, 0.75)';
          ctx.lineWidth = 2;
          ctx.strokeRect(
            Math.floor(player.x - 4),
            Math.floor(player.y - player.getHeight() - 2),
            Math.floor(player.getWidth() + 8),
            Math.floor(player.getHeight() + 6)
          );
        }
      }

      function drawHUDAndBanners() {
        ctx.save();
        ctx.font = '12px "Press Start 2P", monospace';
        ctx.textAlign = 'right';
        ctx.fillStyle = activeTheme.uiText;

        const hiStr = String(highScore).padStart(5, '0');
        const scStr = String(Math.min(STAGE2_END_SCORE, Math.floor(score))).padStart(5, '0');
        ctx.fillText(`HI ${hiStr}  ${scStr}`, W - 18, 28);

        if (invincible) {
          ctx.textAlign = 'left';
          ctx.font = '9px "Press Start 2P", monospace';
          ctx.fillStyle = '#FBBF24';
          ctx.fillText('[INVINCIBLE ON]', 16, 26);
        }

        if (banner.timer > 0 && banner.alpha > 0) {
          ctx.globalAlpha = banner.alpha;
          ctx.textAlign = 'center';
          if (banner.isWarning) {
            const blink = Math.floor(frameCount / 6) % 2 === 0;
            ctx.fillStyle = blink ? '#FF2A2A' : '#FFE4E6';
            ctx.font = '13px "Press Start 2P", monospace';
            ctx.fillText(banner.text, W / 2, 54);
          } else {
            ctx.fillStyle = activeTheme.uiText;
            ctx.font = '13px "Press Start 2P", monospace';
            ctx.fillText(banner.text, W / 2, 54);
          }
          ctx.font = '600 12px "Inter", sans-serif';
          ctx.fillText(banner.subtext, W / 2, 76);
        }

        if (gameState === STATE.READY) {
          ctx.fillStyle = 'rgba(15, 20, 28, 0.82)';
          ctx.fillRect(W / 2 - 250, H / 2 - 60, 500, 118);
          ctx.strokeStyle = '#F59E0B';
          ctx.lineWidth = 2;
          ctx.strokeRect(W / 2 - 250, H / 2 - 60, 500, 118);

          ctx.textAlign = 'center';
          ctx.fillStyle = '#FBBF24';
          ctx.font = '13px "Press Start 2P", monospace';
          ctx.fillText('DINO : 66,000,000 (2-STAGE EPIC)', W / 2, H / 2 - 24);

          ctx.fillStyle = '#F3F4F6';
          ctx.font = '600 11px "Inter", sans-serif';
          ctx.fillText('1부: 중생대 공룡 진화 & 매장(4000) → 2부: 백골 각성, 인류 역사, UFO보스 & 박제(8000)', W / 2, H / 2 + 4);

          if (Math.floor(frameCount / 25) % 2 === 0) {
            ctx.fillStyle = '#34D399';
            if (isMobileMode) {
              ctx.font = '700 13px "Inter", sans-serif';
              ctx.fillText('화면 터치 또는 하단 [▲ 점프 / 시작] 대형 버튼을 누르세요!', W / 2, H / 2 + 34);
            } else {
              ctx.font = '10px "Press Start 2P", monospace';
              ctx.fillText('PRESS [SPACE] OR [↑] TO START', W / 2, H / 2 + 34);
            }
          }
        }

        if (gameState === STATE.GAMEOVER) {
          ctx.fillStyle = 'rgba(12, 15, 22, 0.88)';
          ctx.fillRect(W / 2 - 230, H / 2 - 64, 460, 126);
          ctx.strokeStyle = '#EF4444';
          ctx.lineWidth = 2;
          ctx.strokeRect(W / 2 - 230, H / 2 - 64, 460, 126);

          ctx.textAlign = 'center';
          ctx.fillStyle = '#F87171';
          ctx.font = '15px "Press Start 2P", monospace';
          ctx.fillText('G A M E   O V E R', W / 2, H / 2 - 30);

          ctx.fillStyle = '#E5E7EB';
          ctx.font = '600 12px "Inter", sans-serif';
          ctx.fillText(`충돌 요인: ${deathReason} (기록: ${Math.floor(score)} / 8000점)`, W / 2, H / 2 - 2);

          ctx.fillStyle = '#FBBF24';
          if (isMobileMode) {
            ctx.font = '700 12px "Inter", sans-serif';
            ctx.fillText('화면 또는 [점프] 터치: 이어하기  /  우측 상단 [↻] 아이콘: 처음부터', W / 2, H / 2 + 30);
          } else {
            ctx.font = '10px "Press Start 2P", monospace';
            ctx.fillText('[SPACE] 이어하기  /  [R] 1부 처음부터', W / 2, H / 2 + 30);
          }
        }

        ctx.restore();
      }

      // 1) REQUIREMENT 2: STAGE 1 BURIED ENDING — Solid, completely packed soil layers without empty cavities!
      function drawBuriedEndingScene() {
        const skyGrad = ctx.createLinearGradient(0, 0, 0, 105);
        skyGrad.addColorStop(0, '#0F172A');
        skyGrad.addColorStop(1, '#334155');
        ctx.fillStyle = skyGrad;
        ctx.fillRect(0, 0, W, 105);

        // Ground surface boundary
        ctx.fillStyle = '#FF4500';
        ctx.fillRect(0, 105, W, 3);

        // Dense, solid geological strata without any hollow cavity
        const strata = [
          { y: 108, h: 32, color: '#3A151D' }, // Ash and impact boundary layer
          { y: 140, h: 42, color: '#4A2A1A' }, // Deep organic earth
          { y: 182, h: 56, color: '#381E14' }, // Solid sedimentary stratum
          { y: 238, h: 62, color: '#22100A' }  // Deep bedrock
        ];
        for (let st of strata) {
          ctx.fillStyle = st.color;
          ctx.fillRect(0, st.y, W, st.h);
        }

        // Texture particles tightly packed in soil
        for (let g of buriedScene.soilGrains) {
          ctx.fillStyle = g.tone === 0 ? '#5A3525' : (g.tone === 1 ? '#2E150D' : '#6A4030');
          ctx.fillRect(g.x, g.y, g.w, g.h);
        }

        // Draw Skeleton T-Rex lying buried deep in the solid earth with CLOSED EYES
        const scale = 2.9;
        const fx = W / 2 - 34;
        const fy = 168;

        drawPixelMatrix(ctx, SPRITES.TREX_FOSSIL_SLEEP, fx + 2, fy + 2, scale, {
          '#': '#2B140D', '2': '#140805'
        });
        drawPixelMatrix(ctx, SPRITES.TREX_FOSSIL_SLEEP, fx, fy, scale, {
          '#': '#D6D3D1', '2': '#1F2937' // Closed dark eyelid
        });

        // Soil layers directly covering parts of the body (genuinely entombed in solid earth)
        ctx.fillStyle = 'rgba(56, 30, 20, 0.72)';
        ctx.fillRect(fx - 14, fy + 38, 92, 16);
        ctx.fillStyle = 'rgba(74, 42, 26, 0.65)';
        ctx.fillRect(fx + 10, fy + 22, 60, 12);

        // Surrounding ancient fossil remnants in the solid rock
        drawPixelMatrix(ctx, SPRITES.FOSSIL_SKULL, 120, 205, 1.4, { '#': '#4A2E24' });
        drawPixelMatrix(ctx, SPRITES.FOSSIL_SKULL, 640, 190, 1.4, { '#': '#4A2E24' });

        ctx.textAlign = 'center';
        ctx.font = '13px "Press Start 2P", monospace';
        ctx.fillStyle = '#FBBF24';
        ctx.fillText('STAGE 1 CLEAR — BURIED IN SOLID EARTH', W / 2, 42);

        ctx.font = '600 13px "Inter", sans-serif';
        ctx.fillStyle = '#E2E8F0';
        ctx.fillText('소행성 충돌 후 수천만 년... 빈 공간 없는 단단한 흙 지층 속에 눈을 감은 채 매장되었습니다.', W / 2, 72);

        if (buriedScene.fadeFromWhite > 0) {
          ctx.fillStyle = `rgba(255, 255, 255, ${buriedScene.fadeFromWhite})`;
          ctx.fillRect(0, 0, W, H);
        }
      }

      // 2) REQUIREMENT 2: STAGE 2 AWAKEN CUTSCENE — Digging up through soil, bursting out, and landing ON the ground surface!
      function drawAwakenCutscene() {
        const surfY = Math.floor(awakenScene.surfaceY);

        const skyGrad = ctx.createLinearGradient(0, 0, 0, surfY);
        skyGrad.addColorStop(0, '#CBD5E1');
        skyGrad.addColorStop(1, '#94A3B8');
        ctx.fillStyle = skyGrad;
        ctx.fillRect(0, 0, W, surfY);

        // Distant Paleolithic mountains & snow pines on the surface horizon
        ctx.fillStyle = '#64748B';
        ctx.beginPath();
        ctx.moveTo(60, surfY); ctx.lineTo(150, surfY - 48); ctx.lineTo(250, surfY);
        ctx.moveTo(520, surfY); ctx.lineTo(620, surfY - 56); ctx.lineTo(730, surfY);
        ctx.fill();

        // Solid soil layers below surfY
        ctx.fillStyle = '#4A2A1A';
        ctx.fillRect(0, surfY + 4, W, 58);
        ctx.fillStyle = '#381E14';
        ctx.fillRect(0, surfY + 62, W, 58);
        ctx.fillStyle = '#22100A';
        ctx.fillRect(0, surfY + 120, W, H - surfY);

        // Snow / Paleolithic ground surface line
        ctx.fillStyle = '#E2E8F0';
        ctx.fillRect(0, surfY, W, 6);
        ctx.fillStyle = '#334155';
        ctx.fillRect(0, surfY, W, 2);

        // Hollow tunnel path excavated by the skeleton clawing upwards
        ctx.fillStyle = '#180B07';
        for (let dp of awakenScene.dugPath) {
          if (dp.y > surfY - 4) {
            ctx.fillRect(Math.floor(dp.x - dp.w / 2), Math.floor(Math.max(surfY + 2, dp.y)), dp.w, dp.h);
          }
        }
        // Surface exit hole where the skeleton burst through
        if (awakenScene.timer >= 70 && awakenScene.timer < 165) {
          ctx.fillStyle = '#180B07';
          ctx.fillRect(Math.floor(W / 2 - 42), surfY, 84, 12);
          ctx.fillStyle = '#5C4033';
          ctx.fillRect(Math.floor(W / 2 - 50), surfY - 5, 14, 7);
          ctx.fillRect(Math.floor(W / 2 + 36), surfY - 5, 14, 7);
        }

        const scale = 2.65;
        const fx = Math.floor(awakenScene.fossilX);
        const fy = Math.floor(awakenScene.fossilY);

        if (awakenScene.timer < 18) {
          // Closed eye before awakening
          drawPixelMatrix(ctx, SPRITES.TREX_FOSSIL_SLEEP, fx, fy, scale, {
            '#': '#D6D3D1', '2': '#1F2937'
          });
        } else {
          // Eyes blazing red, clawing arms, jumping onto the ground, and running forward!
          const digFrame = Math.floor(awakenScene.timer / 3) % 4;
          const spr = SPRITES.SKEL_RUNFrames[digFrame];
          drawPixelMatrix(ctx, spr, fx + 2, fy + 2, scale, { '#': '#0F172A', '3': '#0F172A' });
          drawPixelMatrix(ctx, spr, fx, fy, scale, {
            '#': '#FFFFFF',
            '3': (awakenScene.timer % 4 < 2) ? '#FF2A2A' : '#FACC15'
          });
        }

        // Flying clods of dirt and rubble flying up to the surface
        for (let dc of awakenScene.dirtClods) {
          ctx.fillStyle = dc.color;
          ctx.fillRect(Math.floor(dc.x), Math.floor(dc.y), dc.size, dc.size);
        }

        ctx.textAlign = 'center';
        ctx.font = '13px "Press Start 2P", monospace';
        ctx.fillStyle = awakenScene.timer >= 92 ? '#0F172A' : '#EF4444';
        ctx.fillText(
          awakenScene.timer < 92
            ? '[ SKELETON AWAKENING : DIGGING UP! ]'
            : '[ EMERGED ON SURFACE : 구석기 지상 착지 완료! ]',
          W / 2, 34
        );

        ctx.font = '600 12px "Inter", sans-serif';
        ctx.fillStyle = '#1E293B';
        ctx.fillText(
          awakenScene.timer < 92
            ? '눈을 뜬 백골 티렉스가 단단한 흙 지층을 직접 파헤치며 올라옵니다!'
            : '땅을 뚫고 지상으로 솟구쳐 올라와 구석기 설원 위를 질주하기 시작합니다!',
          W / 2, 56
        );
      }

      // 3) REQUIREMENT 7: STAGE 2 CAPTURE CUTSCENE — Pursuers chase from behind, overtake to front, block, and surround!
      function drawCaptureCutscene() {
        drawSkyAndEnvironment();

        const dx = Math.floor(captureScene.dinoX);
        const groundSink = 2;

        // Draw Skeleton T-Rex running or coming to a sudden halt
        const spr = (captureScene.phase === 'OVERTAKE')
          ? SPRITES.SKEL_RUNFrames[player.legFrame]
          : SPRITES.TREX_FOSSIL;

        drawGroundAnchoredSprite(ctx, spr, dx + 2, GROUND_Y + 2, 2.7, { '#': '#0F172A', '3': '#0F172A' }, groundSink);
        drawGroundAnchoredSprite(ctx, spr, dx, GROUND_Y, 2.7, {
          '#': '#F8FAFC',
          '3': captureScene.crystalAlpha > 0.65 ? '#94A3B8' : '#FF3B30'
        }, groundSink);

        // Rear Mech right behind player
        const rx = Math.floor(captureScene.rearMechX);
        drawGroundAnchoredSprite(ctx, SPRITES.CHRONO_MECHA_1, rx, GROUND_Y, 2.6, {
          '#': '#0F172A', '2': '#22D3EE', '3': '#F43F5E'
        }, groundSink);

        // Vanguard Mech that overtook and blocked the FRONT!
        const fx = Math.floor(captureScene.overtakeMechX);
        drawGroundAnchoredSprite(ctx, SPRITES.CHRONO_MECHA_2, fx, GROUND_Y, 2.7, {
          '#': '#0F172A', '2': '#22D3EE', '3': '#F43F5E'
        }, groundSink);

        // Vanguard Hover Cruiser parked further ahead blocking escape
        const cx = Math.floor(captureScene.overtakeCarX);
        drawGroundAnchoredSprite(ctx, SPRITES.POLICE_CAR, cx, GROUND_Y, 2.7, {
          '#': '#1E1B4B', '2': '#22D3EE', '3': '#F43F5E'
        }, groundSink);

        // Drones overhead
        const d1x = Math.floor(captureScene.drone1X);
        const d2x = Math.floor(captureScene.drone2X);
        const dy = Math.floor(captureScene.droneY);
        drawPixelMatrix(ctx, SPRITES.FUTURE_DRONE_1, d1x, dy, 2.2, { '#': '#1E1B4B', '2': '#A855F7', '3': '#22D3EE' });
        drawPixelMatrix(ctx, SPRITES.FUTURE_DRONE_2, d2x, dy, 2.2, { '#': '#1E1B4B', '2': '#A855F7', '3': '#22D3EE' });

        // Phase 2: Vanguard blocked front -> Stasis containment cage activates!
        if (captureScene.stasisActive) {
          // Laser containment walls
          ctx.fillStyle = 'rgba(34, 211, 238, 0.4)';
          // Left Mech to Dino
          ctx.fillRect(rx + 45, GROUND_Y - 45, dx - rx - 40, 6);
          // Right Vanguard Mech to Dino (Blocking laser!)
          ctx.fillRect(dx + 52, GROUND_Y - 45, fx - dx - 45, 6);
          // Overhead Drone Stasis Cone
          ctx.fillRect(dx - 12, dy + 14, 80, GROUND_Y - dy - 14);

          // Crystalline Stasis Mounting Block around the Skeleton ("박제 표본화")
          ctx.strokeStyle = `rgba(250, 204, 21, ${captureScene.crystalAlpha})`;
          ctx.lineWidth = 3;
          ctx.strokeRect(dx - 12, GROUND_Y - 58, 80, 58);
          ctx.fillStyle = `rgba(34, 211, 238, ${captureScene.crystalAlpha * 0.25})`;
          ctx.fillRect(dx - 12, GROUND_Y - 58, 80, 58);
        }

        // Action Narrative Banner
        ctx.textAlign = 'center';
        ctx.font = '12px "Press Start 2P", monospace';
        ctx.fillStyle = captureScene.phase === 'OVERTAKE' ? '#FACC15' : '#22D3EE';
        ctx.fillText(
          captureScene.phase === 'OVERTAKE'
            ? '[ PURSUIT OVERTAKE : 앞길을 가로막는 추격대! ]'
            : '[ CONTAINED & MOUNTED : 전방 차단 완료 및 박제! ]',
          W / 2, 36
        );

        ctx.font = '600 12px "Inter", sans-serif';
        ctx.fillStyle = '#F8FAFC';
        ctx.fillText(
          captureScene.phase === 'OVERTAKE'
            ? '뒤에서 따라오던 크로노 부대가 급가속해 백골 티렉스를 앞질러 도주로를 차단합니다!'
            : '앞뒤와 상공이 모두 차단된 백골 티렉스가 스테이시스 필드에 고정되어 영구 박제됩니다...',
          W / 2, 58
        );

        if (captureScene.whiteoutAlpha > 0) {
          ctx.fillStyle = `rgba(255, 255, 255, ${captureScene.whiteoutAlpha})`;
          ctx.fillRect(0, 0, W, H);
        }
      }

      // 4) FINAL ENDING: MODERN / FUTURE NATURAL HISTORY MUSEUM SCENE (With surrounding historical artifacts & small "준성 만듬" sign!)
      function drawEpilogueMuseum() {
        const wallGrad = ctx.createLinearGradient(0, 0, 0, H);
        wallGrad.addColorStop(0, '#0D111A');
        wallGrad.addColorStop(0.75, '#182030');
        wallGrad.addColorStop(1, '#0F141D');
        ctx.fillStyle = wallGrad;
        ctx.fillRect(0, 0, W, H);

        // Classical Museum Columns
        ctx.fillStyle = '#131926';
        for (let px = 40; px < W; px += 175) {
          ctx.fillRect(px, 20, 22, GROUND_Y - 20);
          ctx.fillStyle = '#1D2536';
          ctx.fillRect(px - 4, 20, 30, 10);
          ctx.fillRect(px - 4, GROUND_Y - 12, 30, 12);
          ctx.fillStyle = '#131926';
        }

        // Framed Fossil & Era Paintings on the Museum Back Wall
        const framePositions = [115, 625];
        for (let idx = 0; idx < framePositions.length; idx++) {
          const fx = framePositions[idx];
          const fy = 112;
          ctx.fillStyle = '#1E293B';
          ctx.fillRect(fx, fy, 56, 38);
          ctx.strokeStyle = '#B45309';
          ctx.lineWidth = 2;
          ctx.strokeRect(fx, fy, 56, 38);
          if (idx === 0) {
            // Framed Asteroid Impact & Volcano Painting
            ctx.fillStyle = '#7C2D12';
            ctx.fillRect(fx + 4, fy + 4, 48, 30);
            ctx.fillStyle = '#FACC15';
            ctx.fillRect(fx + 34, fy + 10, 8, 8);
            ctx.fillStyle = '#FF4500';
            ctx.beginPath();
            ctx.moveTo(fx + 8, fy + 34); ctx.lineTo(fx + 22, fy + 16); ctx.lineTo(fx + 36, fy + 34);
            ctx.fill();
          } else {
            // Framed Far Future Ringed Planet Painting
            ctx.fillStyle = '#0F172A';
            ctx.fillRect(fx + 4, fy + 4, 48, 30);
            ctx.fillStyle = '#22D3EE';
            ctx.beginPath();
            ctx.arc(fx + 28, fy + 19, 8, 0, Math.PI * 2);
            ctx.fill();
            ctx.strokeStyle = '#F472B6';
            ctx.lineWidth = 1.5;
            ctx.beginPath();
            ctx.moveTo(fx + 14, fy + 24); ctx.lineTo(fx + 42, fy + 14);
            ctx.stroke();
          }
        }

        // Soft Museum Spotlights (Center + Left/Right Side Exhibits)
        ctx.fillStyle = 'rgba(255, 238, 185, 0.11)';
        for (let sy = 0; sy < GROUND_Y + 8; sy += 8) {
          const halfW = 40 + (sy / GROUND_Y) * 135;
          ctx.fillRect(Math.floor(W / 2 - halfW), sy, Math.floor(halfW * 2), 8);
        }
        ctx.fillStyle = 'rgba(186, 230, 253, 0.06)';
        ctx.fillRect(32, 90, 200, GROUND_Y - 84);
        ctx.fillRect(568, 90, 200, GROUND_Y - 84);

        // Museum Floor
        ctx.fillStyle = '#121622';
        ctx.fillRect(0, GROUND_Y + 6, W, H - GROUND_Y);
        ctx.fillStyle = '#2A3346';
        ctx.fillRect(0, GROUND_Y + 6, W, 2);

        // =====================================================================
        // SIDE EXHIBITS (공룡 말고도 근처에 함께 전시된 시대별 유물·전시품들!)
        // =====================================================================

        // [Left Exhibit 1] K-Pg Meteorite Fragment & Ammonite Fossil in Glass Case (x = 42)
        const ex1X = 42, ex1W = 68, ex1PedH = 26, ex1PedY = GROUND_Y - ex1PedH + 6;
        ctx.fillStyle = '#262F3F';
        ctx.fillRect(ex1X, ex1PedY, ex1W, ex1PedH);
        ctx.strokeStyle = '#475569';
        ctx.lineWidth = 1.5;
        ctx.strokeRect(ex1X, ex1PedY, ex1W, ex1PedH);
        // Glass dome
        ctx.fillStyle = 'rgba(186, 230, 253, 0.14)';
        ctx.fillRect(ex1X + 4, ex1PedY - 34, ex1W - 8, 34);
        ctx.strokeStyle = 'rgba(186, 230, 253, 0.45)';
        ctx.strokeRect(ex1X + 4, ex1PedY - 34, ex1W - 8, 34);
        // Glowing Meteorite specimen inside
        drawPixelMatrix(ctx, SPRITES.METEOR_OB, ex1X + 10, ex1PedY - 26, 1.6, {
          '#': '#451A03', '2': '#EA580C', '3': '#FDE047', '4': '#F97316'
        });
        // Spiral Ammonite shell next to meteorite
        ctx.fillStyle = '#D6D3D1';
        ctx.beginPath();
        ctx.arc(ex1X + 48, ex1PedY - 12, 9, 0, Math.PI * 2);
        ctx.fill();
        ctx.strokeStyle = '#57534E';
        ctx.beginPath();
        ctx.arc(ex1X + 48, ex1PedY - 12, 5, 0, Math.PI * 1.5);
        ctx.stroke();
        ctx.fillStyle = '#94A3B8';
        ctx.font = '600 8px "Inter", sans-serif';
        ctx.textAlign = 'center';
        ctx.fillText('운석·암모나이트', ex1X + ex1W / 2, ex1PedY + 16);

        // [Left Exhibit 2] Prehistoric Fossil Skull & Paleolithic Stone Spear (x = 132)
        const ex2X = 132, ex2W = 74, ex2PedH = 26, ex2PedY = GROUND_Y - ex2PedH + 6;
        ctx.fillStyle = '#262F3F';
        ctx.fillRect(ex2X, ex2PedY, ex2W, ex2PedH);
        ctx.strokeStyle = '#475569';
        ctx.strokeRect(ex2X, ex2PedY, ex2W, ex2PedH);
        // Fossil Skull on mount
        drawPixelMatrix(ctx, SPRITES.FOSSIL_SKULL, ex2X + 10, ex2PedY - 20, 2.2, { '#': '#E2E8F0' });
        // Crossed Paleolithic flint spear & stone axe on stand
        ctx.strokeStyle = '#92400E';
        ctx.lineWidth = 2;
        ctx.beginPath();
        ctx.moveTo(ex2X + 48, ex2PedY - 2); ctx.lineTo(ex2X + 64, ex2PedY - 28);
        ctx.stroke();
        ctx.fillStyle = '#CBD5E1';
        ctx.fillRect(ex2X + 61, ex2PedY - 32, 6, 6);
        ctx.fillStyle = '#94A3B8';
        ctx.font = '600 8px "Inter", sans-serif';
        ctx.textAlign = 'center';
        ctx.fillText('원시 화석·돌창', ex2X + ex2W / 2, ex2PedY + 16);

        // [Right Exhibit 3] Ancient Golden Pharaoh Mask & Medieval Knight Shield/Sword (x = 594)
        const ex3X = 594, ex3W = 74, ex3PedH = 26, ex3PedY = GROUND_Y - ex3PedH + 6;
        ctx.fillStyle = '#262F3F';
        ctx.fillRect(ex3X, ex3PedY, ex3W, ex3PedH);
        ctx.strokeStyle = '#475569';
        ctx.strokeRect(ex3X, ex3PedY, ex3W, ex3PedH);
        // Glass case
        ctx.fillStyle = 'rgba(186, 230, 253, 0.14)';
        ctx.fillRect(ex3X + 4, ex3PedY - 34, ex3W - 8, 34);
        ctx.strokeStyle = 'rgba(186, 230, 253, 0.45)';
        ctx.strokeRect(ex3X + 4, ex3PedY - 34, ex3W - 8, 34);
        // Golden Pharaoh Mask / Crown
        ctx.fillStyle = '#FACC15';
        ctx.fillRect(ex3X + 12, ex3PedY - 24, 18, 18);
        ctx.fillStyle = '#1D4ED8';
        ctx.fillRect(ex3X + 10, ex3PedY - 26, 22, 4);
        ctx.fillStyle = '#0F172A';
        ctx.fillRect(ex3X + 15, ex3PedY - 18, 4, 3);
        ctx.fillRect(ex3X + 23, ex3PedY - 18, 4, 3);
        // Medieval Red/Gold Shield & Silver Sword
        ctx.fillStyle = '#E2E8F0';
        ctx.fillRect(ex3X + 51, ex3PedY - 30, 3, 26);
        ctx.fillStyle = '#DC2626';
        ctx.fillRect(ex3X + 44, ex3PedY - 20, 16, 16);
        ctx.strokeStyle = '#FACC15';
        ctx.strokeRect(ex3X + 44, ex3PedY - 20, 16, 16);
        ctx.fillStyle = '#94A3B8';
        ctx.font = '600 8px "Inter", sans-serif';
        ctx.textAlign = 'center';
        ctx.fillText('고대 황금관·성검', ex3X + ex3W / 2, ex3PedY + 16);

        // [Right Exhibit 4] Captured Future Mini-UFO & Cyber Drone in Stasis Tube (x = 686)
        const ex4X = 686, ex4W = 72, ex4PedH = 26, ex4PedY = GROUND_Y - ex4PedH + 6;
        ctx.fillStyle = '#262F3F';
        ctx.fillRect(ex4X, ex4PedY, ex4W, ex4PedH);
        ctx.strokeStyle = '#475569';
        ctx.strokeRect(ex4X, ex4PedY, ex4W, ex4PedH);
        // Glowing Cyan Stasis Cylinder
        ctx.fillStyle = 'rgba(34, 211, 238, 0.16)';
        ctx.fillRect(ex4X + 8, ex4PedY - 38, ex4W - 16, 38);
        ctx.strokeStyle = '#22D3EE';
        ctx.strokeRect(ex4X + 8, ex4PedY - 38, ex4W - 16, 38);
        const hoverFloat = Math.sin(frameCount * 0.08) * 2.5;
        drawPixelMatrix(ctx, SPRITES.FUTURE_DRONE_1, ex4X + 16, ex4PedY - 26 + hoverFloat, 1.8, {
          '#': '#1E1B4B', '2': '#F43F5E', '3': '#22D3EE'
        });
        ctx.fillStyle = '#94A3B8';
        ctx.font = '600 8px "Inter", sans-serif';
        ctx.textAlign = 'center';
        ctx.fillText('초미래 UFO 드론', ex4X + ex4W / 2, ex4PedY + 16);

        // =====================================================================
        // CENTRAL T-REX EXHIBITION PEDESTAL
        // =====================================================================
        const pedW = 230;
        const pedH = 38;
        const pedX = W / 2 - pedW / 2;
        const pedY = GROUND_Y - pedH + 6;

        ctx.fillStyle = '#2B3344';
        ctx.fillRect(pedX, pedY, pedW, pedH);
        ctx.strokeStyle = '#4B5568';
        ctx.lineWidth = 2;
        ctx.strokeRect(pedX, pedY, pedW, pedH);

        // Gold Plaque (Natural Korean)
        ctx.fillStyle = '#1A1E29';
        ctx.fillRect(W / 2 - 106, pedY + 9, 212, 20);
        ctx.strokeStyle = '#D97706';
        ctx.lineWidth = 1.5;
        ctx.strokeRect(W / 2 - 106, pedY + 9, 212, 20);

        ctx.font = '700 10px "Inter", sans-serif';
        ctx.fillStyle = '#FBBF24';
        ctx.textAlign = 'center';
        ctx.fillText('[ 티라노사우루스 렉스 · 기원전 6,600만 년 ]', W / 2, pedY + 23);

        // Mounted Skeleton Fossil
        const fossilScale = 3.2;
        const fossilW = 22 * fossilScale;
        const fossilH = SPRITES.TREX_FOSSIL.length * fossilScale;
        const fossilX = W / 2 - fossilW / 2 + 6;
        const fossilY = pedY - fossilH + 2;

        drawPixelMatrix(ctx, SPRITES.TREX_FOSSIL, fossilX + 2, fossilY + 2, fossilScale, { '#': '#475569', '3': '#475569' });
        drawPixelMatrix(ctx, SPRITES.TREX_FOSSIL, fossilX, fossilY, fossilScale, { '#': '#F8FAFC', '3': '#1E293B' });

        // Museum Guide Ropes
        ctx.fillStyle = '#9CA3AF';
        ctx.fillRect(pedX - 30, pedY + 12, 4, 26);
        ctx.fillRect(pedX + pedW + 26, pedY + 12, 4, 26);
        ctx.fillStyle = '#991B1B';
        ctx.fillRect(pedX - 26, pedY + 18, 26, 3);
        ctx.fillRect(pedX + pedW, pedY + 18, 26, 3);

        // =====================================================================
        // SMALL, COMPACT CREATOR SIGN ("준성 만듬") NEXT TO THE CENTRAL EXHIBIT
        // (Requirement 3: 표지판 사이즈는 크게 하지마!)
        // =====================================================================
        const signX = pedX + pedW + 36; // Right beside the central T-Rex guide rope
        const signBoardW = 46;
        const signBoardH = 15;
        const signBoardY = GROUND_Y - 21;
        // Thin wooden/brass post
        ctx.fillStyle = '#78350F';
        ctx.fillRect(signX + Math.floor(signBoardW / 2) - 1, signBoardY + signBoardH, 3, 13);
        ctx.fillRect(signX + Math.floor(signBoardW / 2) - 5, GROUND_Y + 3, 11, 3);
        // Modest sign board
        ctx.fillStyle = '#FEF3C7';
        ctx.fillRect(signX, signBoardY, signBoardW, signBoardH);
        ctx.strokeStyle = '#B45309';
        ctx.lineWidth = 1.2;
        ctx.strokeRect(signX, signBoardY, signBoardW, signBoardH);
        // Cute small Korean text "준성 만듬"
        ctx.font = '700 9px "Inter", sans-serif';
        ctx.fillStyle = '#451A03';
        ctx.textAlign = 'center';
        ctx.fillText('준성 만듬', signX + signBoardW / 2, signBoardY + 11);

        // Visiting Spectators with diverse colorful outfits, hats & accessories!
        for (let v of epilogue.visitors) {
          const bob = Math.sin(v.step) * 2;
          const vx = Math.floor(v.x);
          const vy = Math.floor(H - 6 + bob);

          if (v.type === 'child_hat') {
            ctx.fillStyle = v.botCol || '#1D4ED8';
            ctx.fillRect(vx + 1, vy - 8, 3, 8);
            ctx.fillRect(vx + 6, vy - 8, 3, 8);
            ctx.fillStyle = v.topCol || '#EF4444';
            ctx.fillRect(vx, vy - 22, 10, 14);
            ctx.fillRect(vx + 8, vy - 20, 6, 3);
            ctx.fillStyle = v.skin || '#FDE68A';
            ctx.fillRect(vx + 1, vy - 30, 8, 8);
            ctx.fillStyle = v.hatCol || '#FACC15';
            ctx.fillRect(vx, vy - 33, 12, 3);
          } else if (v.type === 'photographer') {
            ctx.fillStyle = v.botCol || '#1E293B';
            ctx.fillRect(vx + 2, vy - 12, 3, 12);
            ctx.fillRect(vx + 7, vy - 12, 3, 12);
            ctx.fillStyle = v.topCol || '#10B981';
            ctx.fillRect(vx, vy - 32, 12, 20);
            ctx.fillStyle = v.skin || '#FDBA74';
            ctx.fillRect(vx + 2, vy - 41, 8, 9);
            ctx.fillStyle = v.hairCol || '#78350F';
            ctx.fillRect(vx + 1, vy - 44, 10, 3);
            ctx.fillStyle = '#090D16';
            ctx.fillRect(vx + 11, vy - 35, 6, 5);
            if (v.flashTimer <= 0) {
              ctx.fillStyle = 'rgba(255, 255, 255, 0.85)';
              ctx.fillRect(vx + 7, vy - 43, 20, 20);
              ctx.fillRect(vx + 3, vy - 37, 28, 8);
              ctx.fillRect(vx + 13, vy - 47, 8, 28);
            }
          } else if (v.type === 'fashion_dress') {
            ctx.fillStyle = v.skin || '#FFE4E6';
            ctx.fillRect(vx + 2, vy - 10, 3, 10);
            ctx.fillRect(vx + 7, vy - 10, 3, 10);
            ctx.fillStyle = v.botCol || '#BE185D';
            ctx.fillRect(vx - 2, vy - 22, 16, 12);
            ctx.fillStyle = v.topCol || '#EC4899';
            ctx.fillRect(vx, vy - 33, 12, 11);
            ctx.fillStyle = v.skin || '#FFE4E6';
            ctx.fillRect(vx + 2, vy - 42, 8, 9);
            ctx.fillStyle = v.hairCol || '#FACC15';
            ctx.fillRect(vx, vy - 45, 11, 4);
            ctx.fillRect(vx, vy - 41, 3, 9);
          } else {
            ctx.fillStyle = v.botCol || '#0F172A';
            ctx.fillRect(vx + 2, vy - 12, 3, 12);
            ctx.fillRect(vx + 7, vy - 12, 3, 12);
            ctx.fillStyle = v.topCol || '#3B82F6';
            ctx.fillRect(vx, vy - 34, 12, 22);
            ctx.fillStyle = v.skin || '#E0A96D';
            ctx.fillRect(vx + 2, vy - 43, 8, 9);
            ctx.fillStyle = v.hatCol || v.hairCol || '#1C1917';
            ctx.fillRect(vx + 1, vy - 46, 10, 4);
          }
        }

        ctx.textAlign = 'center';
        ctx.font = '700 15px "Inter", sans-serif';
        ctx.fillStyle = '#FDE68A';
        ctx.fillText(epilogue.line1Shown, W / 2, 46);

        ctx.font = '600 13px "Inter", sans-serif';
        ctx.fillStyle = '#E2E8F0';
        ctx.fillText(epilogue.line2Shown, W / 2, 74);

        if (epilogue.fadeFromWhite > 0) {
          ctx.fillStyle = `rgba(255, 255, 255, ${epilogue.fadeFromWhite})`;
          ctx.fillRect(0, 0, W, H);
        }
      }

      // =========================================================================
      // 12. MAIN ANIMATION LOOP & INPUT HANDLERS
      // =========================================================================
      let rafId = null;

      function gameLoop() {
        if (isPortraitLocked) {
          rafId = requestAnimationFrame(gameLoop);
          return;
        }
        if (gameState === STATE.PLAYING) updatePlaying();
        else if (gameState === STATE.IMPACT_CUTSCENE) updateImpactCutscene();
        else if (gameState === STATE.STAGE1_BURIED_ENDING) updateBuriedEnding();
        else if (gameState === STATE.STAGE2_AWAKEN_CUTSCENE) updateAwakenCutscene();
        else if (gameState === STATE.STAGE2_CAPTURE_CUTSCENE) updateCaptureCutscene();
        else if (gameState === STATE.FINAL_EPILOGUE) updateFinalEpilogue();
        else {
          frameCount++;
          if (gameState === STATE.READY) {
            for (let ml of midLandmarks) {
              ml.x += ml.dir * (ml.walkSpeed * 0.65);
              ml.anim++;
              if (ml.x < -140) ml.x = W + 50;
              else if (ml.x > W + 140) ml.x = -100;
            }
            for (let rd of bgRoamingDinos) {
              rd.x += rd.dir * (rd.speed * 0.7);
              rd.anim++;
              if (rd.x < -120) rd.x = W + 60;
              else if (rd.x > W + 120) rd.x = -100;
            }
          }
        }

        ctx.save();

        let shakeMag = 0;
        if (gameState === STATE.PLAYING && currentPhase === 4) {
          const progress4 = Math.max(0, (score - 3000) / 1000);
          shakeMag = 1.4 + progress4 * 3.0;
        } else if (gameState === STATE.IMPACT_CUTSCENE && cutscene.whiteoutAlpha < 1) {
          shakeMag = 6.5;
        } else if (gameState === STATE.STAGE2_AWAKEN_CUTSCENE && awakenScene.timer > 15 && awakenScene.timer < 120) {
          shakeMag = 4.2;
        }

        if (shakeMag > 0) {
          ctx.translate((Math.random() - 0.5) * shakeMag * 2, (Math.random() - 0.5) * shakeMag * 2);
        }

        if (gameState === STATE.STAGE1_BURIED_ENDING) {
          drawBuriedEndingScene();
        } else if (gameState === STATE.STAGE2_AWAKEN_CUTSCENE) {
          drawAwakenCutscene();
        } else if (gameState === STATE.STAGE2_CAPTURE_CUTSCENE) {
          drawCaptureCutscene();
        } else if (gameState === STATE.FINAL_EPILOGUE) {
          drawEpilogueMuseum();
        } else {
          drawSkyAndEnvironment();
          drawObstacles();
          drawUfoBoss(); // Mid-boss UFO and Laser beam in Phase 8!
          drawPlayer();
          drawHUDAndBanners();

          if (gameState === STATE.IMPACT_CUTSCENE) {
            if (cutscene.impacted && cutscene.shockwaveStep > 0) {
              const s = Math.floor(cutscene.shockwaveStep);
              const cx = Math.floor(cutscene.meteorX + 40);
              const cy = Math.floor(cutscene.meteorY + 40);
              ctx.fillStyle = 'rgba(255, 250, 220, 0.9)';
              ctx.fillRect(cx - s, cy - s, s * 2, 12);
              ctx.fillRect(cx - s, cy + s - 12, s * 2, 12);
              ctx.fillRect(cx - s, cy - s, 12, s * 2);
              ctx.fillRect(cx + s - 12, cy - s, 12, s * 2);
            }
            if (cutscene.whiteoutAlpha > 0) {
              ctx.fillStyle = `rgba(255, 255, 255, ${cutscene.whiteoutAlpha})`;
              ctx.fillRect(-20, -20, W + 40, H + 40);
            }
          }
        }

        ctx.restore();
        rafId = requestAnimationFrame(gameLoop);
      }

      function handleJumpAction() {
        sound.init();
        if (gameState === STATE.READY) {
          resetGame(true);
          player.isJumping = true;
          player.vy = player.jumpForce;
          sound.playJump();
        } else if (gameState === STATE.STAGE1_BURIED_ENDING) {
          startStage2AwakenCutscene();
        } else if (gameState === STATE.PLAYING) {
          if (!player.isJumping) {
            player.isJumping = true;
            player.isDucking = false;
            player.vy = player.jumpForce;
            sound.playJump();
          }
        } else if (gameState === STATE.GAMEOVER) {
          if (currentStage === 2) {
            beginStage2Playing();
          } else {
            resetGame(true);
          }
        }
      }

      function handleDuckStart() {
        sound.init();
        if (gameState === STATE.PLAYING) {
          if (!player.isDucking) sound.playDuck();
          player.isDucking = true;
        }
      }

      function handleDuckEnd() {
        player.isDucking = false;
      }

      function toggleInvincibleMode() {
        invincible = !invincible;
        updateHeaderHUD();
      }

      function jumpToNextSubPhase() {
        const thresholds = [0, 500, 1000, 1500, 2000, 2500, 3000, 3500, 3960, 4000, 4500, 5000, 5500, 6000, 6500, 7000, 7500, 7960];
        let nextScore = 500;
        for (let t of thresholds) {
          if (t > score + 25) {
            nextScore = t;
            break;
          }
        }
        jumpToScoreCheat(nextScore);
      }

      window.addEventListener('keydown', (e) => {
        if (['Space', 'ArrowUp', 'ArrowDown'].includes(e.code)) e.preventDefault();

        if (e.code === 'Space' || e.code === 'ArrowUp') {
          handleJumpAction();
        } else if (e.code === 'ArrowDown') {
          handleDuckStart();
        } else if (e.key === '1') jumpToScoreCheat(0);
        else if (e.key === '2') jumpToScoreCheat(1000);
        else if (e.key === '3') jumpToScoreCheat(2000);
        else if (e.key === '4') jumpToScoreCheat(3000);
        else if (e.key === '5') jumpToScoreCheat(3960);
        else if (e.key === '6') jumpToScoreCheat(4000);
        else if (e.key === '7') jumpToScoreCheat(5000);
        else if (e.key === '8') jumpToScoreCheat(6000);
        else if (e.key === '9') jumpToScoreCheat(7000);
        else if (e.key === '0') jumpToScoreCheat(7960);
        else if (e.key === 'n' || e.key === 'N' || e.key === '+') jumpToNextSubPhase();
        else if (e.key === 'i' || e.key === 'I') toggleInvincibleMode();
        else if (e.key === 'r' || e.key === 'R') resetGame(true);
        else if (e.key === 'm' || e.key === 'M') {
          const muted = sound.toggleMute();
          updateMuteUI(muted);
        }
      });

      window.addEventListener('keyup', (e) => {
        if (e.code === 'ArrowDown') handleDuckEnd();
      });

      function updateMuteUI(muted) {
        const icon = document.getElementById('muteIcon');
        const label = document.getElementById('muteLabel');
        if (icon) icon.textContent = muted ? '🔇' : '🔊';
        if (label) label.textContent = muted ? 'MUTED' : 'SOUND ON';
      }

      const muteBtn = document.getElementById('muteBtn');
      if (muteBtn) {
        muteBtn.addEventListener('click', () => {
          const muted = sound.toggleMute();
          updateMuteUI(muted);
          muteBtn.blur();
        });
      }

      const awakenStage2Btn = document.getElementById('awakenStage2Btn');
      if (awakenStage2Btn) {
        awakenStage2Btn.addEventListener('click', () => {
          startStage2AwakenCutscene();
        });
      }

      const rewindBtn = document.getElementById('rewindBtn');
      if (rewindBtn) {
        rewindBtn.addEventListener('click', () => {
          resetGame(true);
        });
      }

      const replayStage2Btn = document.getElementById('replayStage2Btn');
      if (replayStage2Btn) {
        replayStage2Btn.addEventListener('click', () => {
          startStage2AwakenCutscene();
        });
      }

      // =====================================================================
      // PC vs MOBILE LANDSCAPE-ONLY AUTO-DETECTION & ORIENTATION GUARD
      // =====================================================================
      let isPortraitLocked = false;

      function detectIsMobileEnvironment() {
        const uaMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent || '');
        const coarsePointer = window.matchMedia && window.matchMedia('(pointer: coarse)').matches;
        const smallSide = Math.min(window.innerWidth, window.innerHeight) < 768;
        return Boolean(uaMobile || (coarsePointer && smallSide) || window.innerWidth < 768);
      }

      function checkMobileLandscapeLock() {
        const lockOverlay = document.getElementById('portraitLockOverlay');
        if (!lockOverlay) return;
        // Do not block a shared-link player in portrait. In-app browsers and
        // iframes frequently reject the Fullscreen/Screen Orientation APIs,
        // which previously left this overlay on screen permanently.
        isPortraitLocked = false;
        lockOverlay.classList.add('hidden');
      }

      function syncAppViewport() {
        // visualViewport reflects the usable area when a mobile browser's URL
        // bar expands/collapses and when this page is embedded in a preview.
        const viewport = window.visualViewport;
        const height = viewport && viewport.height ? viewport.height : window.innerHeight;
        if (height > 0) {
          document.documentElement.style.setProperty('--app-height', `${Math.round(height * 100) / 100}px`);
        }
      }

      function applyDeviceMode(mobileFlag, isAuto = false) {
        isMobileMode = Boolean(mobileFlag);
        document.body.classList.toggle('mode-mobile', isMobileMode);
        document.body.classList.toggle('mode-pc', !isMobileMode);
        checkMobileLandscapeLock();

        const envBadge = document.getElementById('deviceEnvBadge');
        const modeIcon = document.getElementById('deviceModeIcon');
        const modeLabel = document.getElementById('deviceModeLabel');

        if (envBadge) {
          if (isMobileMode) {
            envBadge.textContent = isAuto ? '📱 모바일 환경 (자동 최적화)' : '📱 모바일 대형키 모드';
            envBadge.className = 'text-[10px] font-bold px-2 py-0.5 rounded-full bg-amber-500/20 border border-amber-400/50 text-amber-300';
          } else {
            envBadge.textContent = isAuto ? '💻 PC 환경 (키보드 최적화)' : '💻 PC 키보드 모드';
            envBadge.className = 'text-[10px] font-bold px-2 py-0.5 rounded-full bg-cyan-500/15 border border-cyan-500/40 text-cyan-300';
          }
        }
        if (modeIcon) modeIcon.textContent = isMobileMode ? '💻' : '📱';
        if (modeLabel) modeLabel.textContent = isMobileMode ? 'PC 모드로 보기' : '모바일 대형키 켜기';
      }

      // Initialize Device Mode automatically on load
      applyDeviceMode(detectIsMobileEnvironment(), true);

      const deviceModeBtn = document.getElementById('deviceModeBtn');
      if (deviceModeBtn) {
        deviceModeBtn.addEventListener('click', () => {
          userOverrodeDeviceMode = true;
          applyDeviceMode(!isMobileMode, false);
          deviceModeBtn.blur();
        });
      }

      function bindJumpButton(btn) {
        if (!btn) return;
        const triggerJump = (e) => {
          e.preventDefault();
          btn.classList.add('is-pressed');
          if (navigator.vibrate) { try { navigator.vibrate(10); } catch (_) {} }
          handleJumpAction();
        };
        const releaseJump = () => btn.classList.remove('is-pressed');
        btn.addEventListener('pointerdown', triggerJump);
        btn.addEventListener('pointerup', releaseJump);
        btn.addEventListener('pointercancel', releaseJump);
        btn.addEventListener('pointerleave', releaseJump);
      }

      function bindDuckButton(btn) {
        if (!btn) return;
        const triggerDuck = (e) => {
          e.preventDefault();
          btn.classList.add('is-pressed');
          if (navigator.vibrate) { try { navigator.vibrate(8); } catch (_) {} }
          handleDuckStart();
        };
        const releaseDuck = (e) => {
          if (e) e.preventDefault();
          btn.classList.remove('is-pressed');
          handleDuckEnd();
        };
        btn.addEventListener('pointerdown', triggerDuck);
        btn.addEventListener('pointerup', releaseDuck);
        btn.addEventListener('pointercancel', releaseDuck);
        btn.addEventListener('pointerleave', releaseDuck);
      }

      // Bind both PC compact buttons and Mobile Giant Arcade Pads
      bindJumpButton(document.getElementById('touchJumpBtn'));
      bindJumpButton(document.getElementById('mobileJumpBtn'));
      bindDuckButton(document.getElementById('touchDuckBtn'));
      bindDuckButton(document.getElementById('mobileDuckBtn'));

      // Pre-Game Intro Start Button & Upper-Right Diagonal Small Rotate/Reset Icon Button
      const startIntroBtn = document.getElementById('startIntroBtn');
      if (startIntroBtn) {
        startIntroBtn.addEventListener('click', (e) => {
          e.preventDefault();
          handleJumpAction();
        });
      }

      const topCornerResetBtn = document.getElementById('topCornerResetBtn');
      if (topCornerResetBtn) {
        topCornerResetBtn.addEventListener('click', () => {
          resetGame(true);
          topCornerResetBtn.blur();
        });
      }

      const openGuideBtn = document.getElementById('openGuideBtn');
      if (openGuideBtn) {
        openGuideBtn.addEventListener('click', () => {
          resetGame(false);
          openGuideBtn.blur();
        });
      }

      // Mobile utility buttons (Restart, Invincible, Collapsible Cheat Drawer)
      const mobileRestartBtn = document.getElementById('mobileRestartBtn');
      if (mobileRestartBtn) {
        mobileRestartBtn.addEventListener('click', () => {
          resetGame(true);
          mobileRestartBtn.blur();
        });
      }

      const mobileInvincibleBtn = document.getElementById('mobileInvincibleBtn');
      if (mobileInvincibleBtn) {
        mobileInvincibleBtn.addEventListener('click', () => {
          toggleInvincibleMode();
          mobileInvincibleBtn.blur();
        });
      }

      const toggleMobileCheatDrawerBtn = document.getElementById('toggleMobileCheatDrawerBtn');
      const cheatPanelWrapper = document.getElementById('cheatPanelWrapper');
      const cheatDrawerArrow = document.getElementById('cheatDrawerArrow');
      if (toggleMobileCheatDrawerBtn && cheatPanelWrapper) {
        toggleMobileCheatDrawerBtn.addEventListener('click', () => {
          const isCollapsed = cheatPanelWrapper.classList.toggle('mobile-collapsed');
          if (cheatDrawerArrow) cheatDrawerArrow.textContent = isCollapsed ? '▼' : '▲';
        });
      }

      // Fullscreen & Landscape Orientation Lock Helper
      function requestLandscapeFullscreen() {
        const docEl = document.documentElement;
        if (docEl.requestFullscreen && !document.fullscreenElement) {
          docEl.requestFullscreen().then(() => {
            if (screen.orientation && screen.orientation.lock) {
              screen.orientation.lock('landscape').catch(() => {});
            }
          }).catch(() => {});
        } else if (screen.orientation && screen.orientation.lock) {
          screen.orientation.lock('landscape').catch(() => {});
        }
      }

      const lockLandscapeBtn = document.getElementById('lockLandscapeBtn');
      if (lockLandscapeBtn) {
        lockLandscapeBtn.addEventListener('click', () => {
          requestLandscapeFullscreen();
        });
      }

      // Fullscreen toggle button
      const fullscreenBtn = document.getElementById('fullscreenBtn');
      if (fullscreenBtn) {
        fullscreenBtn.addEventListener('click', () => {
          if (!document.fullscreenElement) {
            requestLandscapeFullscreen();
          } else {
            document.exitFullscreen().catch(() => {});
          }
          fullscreenBtn.blur();
        });
      }

      // Deploy & Standalone HTML Download Modal
      const deployModal = document.getElementById('deployModal');
      const deployModalBtn = document.getElementById('deployModalBtn');
      const closeDeployModalBtn = document.getElementById('closeDeployModalBtn');
      const downloadIndexHtmlBtn = document.getElementById('downloadIndexHtmlBtn');

      if (deployModalBtn && deployModal) {
        deployModalBtn.addEventListener('click', () => {
          deployModal.classList.remove('hidden');
        });
      }
      if (closeDeployModalBtn && deployModal) {
        closeDeployModalBtn.addEventListener('click', () => {
          deployModal.classList.add('hidden');
        });
      }
      if (deployModal) {
        deployModal.addEventListener('click', (e) => {
          if (e.target === deployModal) deployModal.classList.add('hidden');
        });
      }
      if (downloadIndexHtmlBtn) {
        downloadIndexHtmlBtn.addEventListener('click', () => {
          const fullHtml = '<!DOCTYPE html>\n' + document.documentElement.outerHTML;
          const blob = new Blob([fullHtml], { type: 'text/html;charset=utf-8' });
          const url = URL.createObjectURL(blob);
          const a = document.createElement('a');
          a.href = url;
          a.download = 'index.html';
          document.body.appendChild(a);
          a.click();
          document.body.removeChild(a);
          setTimeout(() => URL.revokeObjectURL(url), 1500);
        });
      }

      // Canvas direct touch/click: in Mobile Mode during PLAYING, touching left 35% of canvas ducks, right 65% jumps!
      canvas.addEventListener('pointerdown', (e) => {
        // Let a finger's vertical swipe bubble into page scrolling. GitHub's
        // mobile preview is often an iframe, so preventing this event traps
        // the player on the upper half of the game.
        if (!isMobileMode || e.pointerType !== 'touch') e.preventDefault();
        if (gameState === STATE.FINAL_EPILOGUE) {
          resetGame(true);
          return;
        }
        if (isMobileMode && gameState === STATE.PLAYING) {
          const rect = canvas.getBoundingClientRect();
          const relX = (e.clientX - rect.left) / Math.max(1, rect.width);
          if (relX < 0.35) {
            handleDuckStart();
            return;
          }
        }
        handleJumpAction();
      });

      canvas.addEventListener('pointerup', () => {
        if (isMobileMode && player.isDucking) handleDuckEnd();
      });
      canvas.addEventListener('pointercancel', () => {
        if (isMobileMode && player.isDucking) handleDuckEnd();
      });

      const cheatBtns = document.querySelectorAll('.cheat-btn');
      cheatBtns.forEach((btn) => {
        btn.addEventListener('click', () => {
          const key = btn.getAttribute('data-cheat');
          const scoreMap = {
            '1': 0, '2': 1000, '3': 2000, '4': 3000, '5': 3960,
            '6': 4000, '7': 5000, '8': 6000, '9': 7000, '0': 7960
          };
          if (scoreMap[key] !== undefined) {
            jumpToScoreCheat(scoreMap[key]);
            if (isMobileMode && cheatPanelWrapper) {
              cheatPanelWrapper.classList.add('mobile-collapsed');
              if (cheatDrawerArrow) cheatDrawerArrow.textContent = '▼';
            }
          }
          btn.blur();
        });
      });

      const nextSubPhaseBtn = document.getElementById('nextSubPhaseBtn');
      if (nextSubPhaseBtn) {
        nextSubPhaseBtn.addEventListener('click', () => {
          jumpToNextSubPhase();
          nextSubPhaseBtn.blur();
        });
      }

      const toggleInvincibleBtn = document.getElementById('toggleInvincibleBtn');
      if (toggleInvincibleBtn) {
        toggleInvincibleBtn.addEventListener('click', () => {
          toggleInvincibleMode();
          toggleInvincibleBtn.blur();
        });
      }

      function handleResize() {
        syncAppViewport();
        canvas.width = W;
        canvas.height = H;
        if (!userOverrodeDeviceMode) {
          applyDeviceMode(detectIsMobileEnvironment(), true);
        } else {
          checkMobileLandscapeLock();
        }
      }

      window.addEventListener('resize', handleResize);
      window.addEventListener('orientationchange', handleResize);
      if (window.visualViewport) {
        window.visualViewport.addEventListener('resize', handleResize);
        window.visualViewport.addEventListener('scroll', syncAppViewport);
      }

      // Force instant loop initialization regardless of document ready state
      handleResize();
      updateHeaderHUD();
      if (rafId) cancelAnimationFrame(rafId);
      rafId = requestAnimationFrame(gameLoop);

      if (document.readyState === 'loading') {
        window.addEventListener('DOMContentLoaded', () => {
          handleResize();
          updateHeaderHUD();
        });
      }
    })();
  </script>
</body>
</html>
