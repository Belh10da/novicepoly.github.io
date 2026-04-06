<html lang="tr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>NOVICEPOLY</title>
  <style>
    :root {
      --bg: #dfe6de;
      --panel: rgba(255,255,255,0.92);
      --line: #111;
      --text: #111;
      --muted: #4b5563;
      --shadow: 0 10px 25px rgba(0,0,0,0.08);
      --soft: 0 4px 10px rgba(0,0,0,0.08);
      --board-base: #cdd9c7;
    }

    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: Arial, Helvetica, sans-serif;
      color: var(--text);
      background: #cfd8ce;
    }

    .app {
      max-width: 1800px;
      margin: 0 auto;
      padding: 12px;
    }

    .hero, .panel {
      background: var(--panel);
      border: 2px solid #111;
      box-shadow: var(--shadow);
      border-radius: 14px;
    }

    .hero {
      padding: 12px 18px;
      margin-bottom: 12px;
      display: flex;
      gap: 15px;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
    }

    .hero h1 { margin: 0 0 4px; font-size: 24px; line-height: 1; letter-spacing: 0.04em; }
    .hero p { margin: 0; font-size: 13px; color: var(--muted); }

    .controls-top { display: flex; gap: 10px; flex-wrap: wrap; align-items: center; }

    .layout {
      display: grid;
      grid-template-columns: 1fr 380px;
      gap: 12px;
      align-items: start;
    }

    .panel { padding: 14px; }
    
    .board-wrap { 
      padding: 12px; 
      display: flex; 
      justify-content: center; 
      align-items: center;
      background: transparent;
      border: none;
      box-shadow: none;
    }

    .board-shell {
      container-type: inline-size;
      aspect-ratio: 1 / 1;
      width: 100%;
      max-width: min(100%, calc(100vh - 100px)); 
      background: #111;
      border: 3px solid #111;
      padding: 0;
      overflow: hidden;
      margin: 0 auto;
      border-radius: 4px;
    }

    .board {
      display: grid;
      grid-template-columns: minmax(0, 1.6fr) repeat(9, minmax(0, 1fr)) minmax(0, 1.6fr);
      grid-template-rows: minmax(0, 1.6fr) repeat(9, minmax(0, 1fr)) minmax(0, 1.6fr);
      gap: 1.5px;
      width: 100%;
      height: 100%;
      background: #111;
    }

    button, select, .tile { touch-action: manipulation; }

    .tile {
      position: relative;
      background: #dce5d8;
      cursor: pointer;
      overflow: hidden;
    }

    .tile:hover { filter: brightness(0.95); }
    .tile.selected { outline: 3px solid #2563eb; outline-offset: -3px; z-index: 10; }

    .tile-inner {
      position: absolute; display: flex; flex-direction: column;
      box-sizing: border-box; background: #dce5d8;
    }

    .tile.normal.bottom .tile-inner { top: 0; left: 0; width: 100%; height: 100%; transform: none; }
    .tile.normal.top .tile-inner { top: 0; left: 0; width: 100%; height: 100%; transform: rotate(180deg); }
    .tile.normal.left .tile-inner { top: 50%; left: 50%; width: 62.5%; height: 160%; transform: translate(-50%, -50%) rotate(90deg); }
    .tile.normal.right .tile-inner { top: 50%; left: 50%; width: 62.5%; height: 160%; transform: translate(-50%, -50%) rotate(-90deg); }

    .tile.corner .tile-inner { top: 0; left: 0; width: 100%; height: 100%; transform: none; justify-content: center; padding: 0; }

    .color-bar { width: 100%; height: 22%; border-bottom: 1.5px solid #111; flex-shrink: 0; }

    .tile-body {
      flex-grow: 1; display: flex; flex-direction: column; justify-content: space-between;
      align-items: center; padding: 4px 2px 2px 2px; width: 100%; height: 100%;
      box-sizing: border-box; text-align: center; overflow: hidden;
    }

    .tile-title { font-size: clamp(4px, 1.3cqw, 10px); font-weight: 600; line-height: 1.1; text-transform: capitalize; letter-spacing: 0.02em; width: 100%; word-wrap: break-word; }
    .tile-price { font-size: clamp(6px, 1.2cqw, 12px); color: #111; font-weight: 800; letter-spacing: 0.05em; margin-top: 2px; }
    .tile-icon { font-size: clamp(14px, 3.2cqw, 36px); line-height: 1; margin: auto 0; }

    .mini-tags { display: flex; gap: 2px; flex-wrap: wrap; justify-content: center; margin-top: 2px; }
    .mini-house { width: 10px; height: 10px; border-radius: 2px; display: inline-block; border: 1.5px solid #111; }
    .mini-hotel { font-size: 9px; line-height: 1; padding: 1px 4px; border-radius: 2px; background: #ef4444; color: white; font-weight: 800; border: 1px solid #111; }
    .mortgage-badge { font-size: 8px; padding: 1px 4px; border-radius: 2px; background: #f59e0b; color: #111; font-weight: 800; border: 1px solid #111; }

    .owner-ribbon {
      position: absolute;
      z-index: 4;
      box-shadow: 0 1px 2px rgba(0,0,0,0.22);
      border: 1.5px solid rgba(255,255,255,0.95);
      pointer-events: none;
    }
    .tile.bottom .owner-ribbon, .tile.top .owner-ribbon { left: 8%; width: 84%; height: 6px; border-radius: 999px; }
    .tile.bottom .owner-ribbon { bottom: 3px; }
    .tile.top .owner-ribbon { bottom: 3px; }
    .tile.left .owner-ribbon, .tile.right .owner-ribbon { top: 8%; height: 84%; width: 6px; border-radius: 999px; }
    .tile.left .owner-ribbon { right: 3px; }
    .tile.right .owner-ribbon { right: 3px; }
    .tile.corner .owner-ribbon { left: 10%; right: 10%; bottom: 4px; height: 7px; border-radius: 999px; }

    .turn-summary { margin-top: 10px; background: #eff6ff; border: 2px solid #111; border-radius: 12px; padding: 10px 12px; }
    .turn-summary-label { font-size: 11px; font-weight: 800; letter-spacing: 0.06em; color: #1d4ed8; text-transform: uppercase; margin-bottom: 4px; }
    .turn-summary-text { font-size: 14px; font-weight: 700; line-height: 1.35; }

    .player-dots {
      position: absolute; inset: 0; display: flex; align-items: center; justify-content: center;
      flex-wrap: wrap; gap: 2px; pointer-events: none; z-index: 5;
    }
    
    /* GÜNCELLENEN PİYON STİLLERİ */
    .mini-player { 
      width: 18px; height: 18px; border-radius: 999px; border: 1.5px solid #fff; 
      box-shadow: 0 1px 3px rgba(0,0,0,0.4);
      display: flex; align-items: center; justify-content: center;
      font-size: 11px; 
    }

    .center-panel {
      grid-column: 2 / 11; grid-row: 2 / 11; background: var(--board-base); position: relative;
      overflow: hidden; display: flex; align-items: center; justify-content: center;
    }

    .center-title {
      transform: rotate(-45deg); font-size: clamp(24px, 8cqw, 90px); font-weight: 900; letter-spacing: 0.05em; line-height: 1;
      color: #fff; background: #e32b2b; border: 3px solid #fff; box-shadow: 0 0 0 2px #111, 0 8px 20px rgba(0,0,0,0.18);
      padding: 2cqw 6cqw; white-space: nowrap;
    }

    .center-card {
      position: absolute; width: 22%; aspect-ratio: 1.5 / 1; border: 2px solid #111; display: flex; flex-direction: column;
      align-items: center; justify-content: center; box-shadow: var(--soft);
    }
    .center-card.one { top: 16%; left: 16%; background: #a5d3f0; transform: rotate(-45deg); }
    .center-card.two { bottom: 16%; right: 16%; background: #f0a320; transform: rotate(-45deg); }
    .card-title { font-size: clamp(6px, 1.4cqw, 18px); font-weight: 900; text-transform: uppercase; }
    .card-icon { font-size: clamp(14px, 3.5cqw, 44px); margin-top: 2px; }

    /* Sağ Panel Standart Stiller */
    .side { display: grid; gap: 12px; align-content: start; }
    .panel h3 { margin: 0 0 10px; font-size: 17px; }
    .status-top { display: flex; justify-content: space-between; gap: 12px; align-items: center; }
    .dice-box { display: flex; gap: 8px; background: #f3f4f6; border: 2px solid #111; padding: 8px 10px; border-radius: 12px; font-size: 26px; min-width: 90px; justify-content: center; }
    .message { color: var(--muted); margin-top: 4px; font-size: 13px; font-weight: 600; }
    .btn-row, .action-grid { display: flex; gap: 8px; flex-wrap: wrap; margin-top: 10px; }
    
    button, select { font: inherit; font-size: 16px !important; }
    
    .btn, .select { border: 2px solid #111; background: white; border-radius: 10px; padding: 8px 12px; color: var(--text); outline: none; }
    .btn { cursor: pointer; font-weight: 700; transition: background 0.1s; }
    .btn:hover:not(:disabled) { filter: brightness(0.9); }
    .btn:active:not(:disabled) { transform: scale(0.98); } 
    .btn:disabled { opacity: .45; cursor: not-allowed; }
    .btn.primary { background: #111827; color: white; }
    .btn.soft { background: #eef2ff; color: #312e81; }

    .info-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
    .info-box, .rent-box, .log-item, .player-card { background: #f8fafc; border: 2px solid #111; border-radius: 10px; padding: 10px; }
    .rent-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 6px; font-size: 13px; }
    .players-list, .log-list { display: grid; gap: 8px; }
    .player-card.active { background: #e5e7eb; outline: 2px solid #2563eb; }
    .player-head { display: flex; align-items: flex-start; justify-content: space-between; gap: 8px; }
    .player-name-row { display: flex; align-items: center; gap: 6px; }
    
    /* GÜNCELLENEN TOKEN STİLLERİ */
    .token { 
      width: 22px; height: 22px; border-radius: 999px; display: inline-flex; 
      border: 2px solid #111; align-items: center; justify-content: center; font-size: 13px;
    }
    
    .pill { display: inline-flex; align-items: center; gap: 4px; padding: 4px 8px; border-radius: 999px; font-size: 11px; border: 1.5px solid #111; background: white; margin-right: 4px; margin-top: 4px; font-weight: 700;}
    .muted { color: var(--muted); }
    .small { font-size: 12px; }
    .log-list { max-height: 200px; overflow: auto; font-size: 13px; }
    .divider { height: 2px; background: #111; margin: 10px 0; }

    /* KART POP-UP SİSTEMİ */
    .modal-overlay {
      position: fixed; inset: 0; background: rgba(0,0,0,0.6);
      display: flex; align-items: center; justify-content: center;
      z-index: 9999; opacity: 0; pointer-events: none;
      transition: opacity 0.25s ease;
      backdrop-filter: blur(4px);
      -webkit-backdrop-filter: blur(4px);
    }
    .modal-overlay.active { opacity: 1; pointer-events: auto; }

    .card-modal {
      width: 90%; max-width: 400px; background: white;
      border: 4px solid #111; border-radius: 16px;
      display: flex; flex-direction: column; text-align: center;
      box-shadow: 0 24px 48px rgba(0,0,0,0.4);
      transform: translateY(30px) scale(0.95);
      transition: transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
      overflow: hidden;
    }
    .modal-overlay.active .card-modal { transform: translateY(0) scale(1); }

    .card-modal-header {
      padding: 18px; font-size: 26px; font-weight: 900; letter-spacing: 0.05em;
      border-bottom: 4px solid #111; text-transform: uppercase;
      display: flex; align-items: center; justify-content: center; gap: 10px;
    }
    .card-modal-header.chance { background: #f0a320; color: white; text-shadow: 0 2px 0 rgba(0,0,0,0.2); }
    .card-modal-header.community { background: #a5d3f0; color: #111; }

    .card-modal-body {
      padding: 36px 24px; font-size: 18px; font-weight: 700; line-height: 1.4;
      color: #111; min-height: 150px; display: flex; align-items: center; justify-content: center;
    }

    .card-modal-footer { padding: 16px; background: #f8fafc; border-top: 2px solid #e2e8f0; }
    .card-modal-footer .btn { width: 100%; font-size: 18px; padding: 14px; }

    /* SETUP MODAL */
    .setup-overlay {
      position: fixed; inset: 0; background: rgba(0,0,0,0.7);
      display: flex; align-items: center; justify-content: center;
      z-index: 9998; opacity: 0; pointer-events: none;
      transition: opacity 0.25s ease;
      backdrop-filter: blur(6px);
    }
    .setup-overlay.active { opacity: 1; pointer-events: auto; }

    .setup-modal {
      width: 92%; max-width: 480px; background: white;
      border: 4px solid #111; border-radius: 18px;
      box-shadow: 0 24px 60px rgba(0,0,0,0.45);
      transform: translateY(30px) scale(0.95);
      transition: transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
      overflow: hidden;
    }
    .setup-overlay.active .setup-modal { transform: translateY(0) scale(1); }

    .setup-header {
      background: #111827; color: white;
      padding: 18px 22px; font-size: 20px; font-weight: 900;
      letter-spacing: 0.05em; display: flex; align-items: center; gap: 10px;
    }

    .setup-body { padding: 20px 22px; display: grid; gap: 10px; }

    .setup-player-row {
      display: flex; align-items: center; gap: 10px;
    }
    .setup-color-dot {
      width: 16px; height: 16px; border-radius: 50%;
      border: 2px solid #111; flex-shrink: 0;
    }
    
    /* YENİ EKLENEN EMOJİ SEÇİCİ STİLİ */
    .setup-emoji-select {
      border: 2px solid #111; border-radius: 10px;
      padding: 8px 4px; font-size: 16px; outline: none; 
      background: white; cursor: pointer;
    }
    
    .setup-input {
      flex: 1; border: 2px solid #111; border-radius: 10px;
      padding: 9px 12px; font-size: 15px; font-family: inherit;
      font-weight: 600; outline: none; transition: border-color 0.15s;
    }
    .setup-input:focus { border-color: #2563eb; }

    .setup-footer {
      padding: 16px 22px; background: #f8fafc;
      border-top: 2px solid #e2e8f0;
      display: flex; gap: 10px;
    }
    .setup-footer .btn { flex: 1; padding: 12px; font-size: 16px; }

    /* STATİSTİK PANELİ */
    .stats-grid { display: grid; gap: 8px; }
    .stat-card {
      background: #f8fafc; border: 2px solid #111;
      border-radius: 10px; padding: 10px 12px;
    }
    .stat-card-header {
      display: flex; align-items: center; gap: 8px;
      margin-bottom: 8px;
    }
    .stat-rows { display: grid; grid-template-columns: 1fr 1fr; gap: 4px 10px; }
    .stat-row { font-size: 12px; }
    .stat-label { color: var(--muted); font-weight: 600; }
    .stat-value { font-weight: 800; font-size: 13px; }
    .net-worth-bar {
      height: 6px; border-radius: 999px; margin-top: 6px;
      background: #e5e7eb; overflow: hidden;
    }
    .net-worth-fill { height: 100%; border-radius: 999px; transition: width 0.4s ease; }

    /* KAYDET/YÜKLE BUTONLARI */
    .save-row { display: flex; gap: 6px; }
    .btn.save { background: #dcfce7; color: #166534; }
    .btn.load { background: #dbeafe; color: #1e40af; }
    .btn.danger { background: #fee2e2; color: #991b1b; }
    .toast {
      position: fixed; bottom: 24px; left: 50%; transform: translateX(-50%) translateY(80px);
      background: #111; color: white; padding: 10px 20px; border-radius: 12px;
      font-weight: 700; font-size: 14px; z-index: 99999;
      transition: transform 0.3s cubic-bezier(0.175,0.885,0.32,1.275);
      pointer-events: none;
    }
    .toast.show { transform: translateX(-50%) translateY(0); }
  </style>
</head>
<body>
  <div class="app">
    <div class="hero">
      <div>
        <h1>NOVICEPOLY</h1>
        <p>Buraya bakan gay</p>
      </div>
      <div class="controls-top">
          <div class="save-row">
            <button id="saveBtn" class="btn save">💾 Kaydet</button>
            <button id="loadBtn" class="btn load">📂 Yükle</button>
          </div>
          <select id="playerCount" class="select">
            <option value="2" selected>2 oyuncu</option>
            <option value="3">3 oyuncu</option>
            <option value="4">4 oyuncu</option>
            <option value="5">5 oyuncu</option>
            <option value="6">6 oyuncu</option>
            <option value="7">7 oyuncu</option>
          </select>
          <button id="newGameBtn" class="btn primary">Yeni Oyun</button>
        </div>
    </div>

    <div class="layout">
      <div class="board-wrap">
        <div class="board-shell">
          <div id="board" class="board"></div>
        </div>
      </div>

      <div class="side">
        <div class="panel">
          <div class="status-top">
            <div>
              <div class="small muted">Sıra</div>
              <div id="activePlayerName" style="font-size:20px;font-weight:800;">Oyuncu 1</div>
              <div id="message" class="message">Oyun hazır. İlk oyuncu zarı atabilir.</div>
              <div class="turn-summary">
                <div class="turn-summary-label">Bu tur</div>
                <div id="turnSummary" class="turn-summary-text">Henüz bir hamle yapılmadı.</div>
              </div>
            </div>
            <div id="diceBox" class="dice-box">⚀ ⚀</div>
          </div>
          <div class="btn-row">
            <button id="rollBtn" class="btn primary">Zar At</button>
            <button id="buyBtn" class="btn soft">Satın Al</button>
            <button id="endTurnBtn" class="btn">Turu Bitir</button>
          </div>
        </div>

        <div class="panel">
          <h3>Seçili Kare</h3>
          <div id="selectedTile"></div>
          <div class="divider"></div>
          <div class="action-grid">
            <button id="buildBtn" class="btn soft">Ev / Otel Ekle</button>
            <button id="mortgageBtn" class="btn">İpotek Aç/Kapat</button>
          </div>
        </div>

        <div class="panel">
          <h3>Oyuncular</h3>
          <div id="playersList" class="players-list"></div>
        </div>

        <div class="panel">
          <h3>Oyun Akışı</h3>
          <div id="logList" class="log-list"></div>
        </div>

        <div class="panel">
          <h3>📊 İstatistikler</h3>
          <div id="statsPanel" class="stats-grid"></div>
        </div>
      </div>
    </div>
  </div>

  <div id="cardModalOverlay" class="modal-overlay">
    <div class="card-modal">
      <div id="cardModalHeader" class="card-modal-header chance">❓ ŞANS</div>
      <div id="cardModalBody" class="card-modal-body">Kart metni buraya gelecek.</div>
      <div class="card-modal-footer">
        <button id="cardModalBtn" class="btn primary">Tamam</button>
      </div>
    </div>
  </div>

  <div id="setupOverlay" class="setup-overlay">
    <div class="setup-modal">
      <div class="setup-header">🎲 NOVICEPOLY — Oyuncular</div>
      <div id="setupBody" class="setup-body"></div>
      <div class="setup-footer">
        <button id="setupStartBtn" class="btn primary">Oyunu Başlat</button>
      </div>
    </div>
  </div>

  <div id="toast" class="toast"></div>

  <script>
    const COLORS = {
      kahverengi: '#92400e', acikMavi: '#7dd3fc', pembe: '#ec4899', turuncu: '#f97316',
      kirmizi: '#ef4444', sari: '#fde047', yesil: '#22c55e', lacivert: '#1d4ed8',
      istasyon: '#111827', servis: '#d1d5db', special: '#e5e7eb'
    };

    const HOUSE_COST = {
      kahverengi: 50, acikMavi: 50, pembe: 100, turuncu: 100,
      kirmizi: 150, sari: 150, yesil: 200, lacivert: 200,
    };

    const TOKENS = [
      { color: '#f43f5e', label: 'Gül' }, { color: '#3b82f6', label: 'Mavi' },
      { color: '#10b981', label: 'Yeşil' }, { color: '#8b5cf6', label: 'Mor' },
      { color: '#f59e0b', label: 'Turuncu' }, { color: '#14b8a6', label: 'Turkuaz' },
      { color: '#64748b', label: 'Gri' }
    ];

    // YENİ EMOJİ LİSTESİ
    const EMOJIS = ['🎓', '☕', '🚌', '🐱', '🎸', '⚽', '💻', '👽', '🍕', '🚗'];

    const board = [
      { id: 0, type: 'go', name: 'Başlangıç' },
      { id: 1, type: 'property', name: 'NB-212', group: 'kahverengi', price: 60, rent: [2, 10, 30, 90, 160, 250], mortgage: 30 },
      { id: 2, type: 'community', name: 'KAMU SANDIĞI' },
      { id: 3, type: 'property', name: 'N MOZART', group: 'kahverengi', price: 60, rent: [4, 20, 60, 180, 320, 450], mortgage: 30 },
      { id: 4, type: 'tax', name: 'DEVLET<br>BAZEN...', amount: 150 },
      { id: 5, type: 'station', name: 'SIHHİYE RİNGİ', group: 'istasyon', price: 200, rentByCount: [25, 50, 100, 200], mortgage: 100 },
      { id: 6, type: 'property', name: 'ARMADA', group: 'acikMavi', price: 100, rent: [6, 30, 90, 270, 400, 550], mortgage: 50 },
      { id: 7, type: 'chance', name: 'ŞANS' },
      { id: 8, type: 'property', name: 'KENTPARK', group: 'acikMavi', price: 100, rent: [6, 30, 90, 270, 400, 550], mortgage: 50 },
      { id: 9, type: 'property', name: 'KIZILAY AVM', group: 'acikMavi', price: 120, rent: [8, 40, 100, 300, 450, 600], mortgage: 60 },
      { id: 10, type: 'jail', name: 'SİLİVRİ / SADECE ZİYARET' },
      { id: 11, type: 'property', name: 'DOĞU YEMEKHANE', group: 'pembe', price: 140, rent: [10, 50, 150, 450, 625, 750], mortgage: 70 },
      { id: 12, type: 'utility', name: 'COFFEE BREAK', group: 'servis', price: 150, mortgage: 75 },
      { id: 13, type: 'property', name: 'TURUNCU CAFE', group: 'pembe', price: 140, rent: [10, 50, 150, 450, 625, 750], mortgage: 70 },
      { id: 14, type: 'property', name: 'CAFU', group: 'pembe', price: 160, rent: [12, 60, 180, 500, 700, 900], mortgage: 80 },
      { id: 15, type: 'station', name: 'TUNUS RİNGİ', group: 'istasyon', price: 200, rentByCount: [25, 50, 100, 200], mortgage: 100 },
      { id: 16, type: 'property', name: 'ABANT GÖLÜ', group: 'turuncu', price: 180, rent: [14, 70, 200, 550, 750, 950], mortgage: 90 },
      { id: 17, type: 'community', name: 'KAMU SANDIĞI' },
      { id: 18, type: 'property', name: 'EYMİR GÖLÜ', group: 'turuncu', price: 180, rent: [14, 70, 200, 550, 750, 950], mortgage: 90 },
      { id: 19, type: 'property', name: 'PTT MANGAL TESİSLERİ', group: 'turuncu', price: 200, rent: [16, 80, 220, 600, 800, 1000], mortgage: 100 },
      { id: 20, type: 'free-parking', name: 'FREE PARKING' },
      { id: 21, type: 'property', name: 'BİLMARKET YANI', group: 'kirmizi', price: 220, rent: [18, 90, 250, 700, 875, 1050], mortgage: 110 },
      { id: 22, type: 'chance', name: 'ŞANS' },
      { id: 23, type: 'property', name: 'TUNUS TEKEL ARASI', group: 'kirmizi', price: 220, rent: [18, 90, 250, 700, 875, 1050], mortgage: 110 },
      { id: 24, type: 'property', name: 'ANKUVA MERDİVENLER', group: 'kirmizi', price: 240, rent: [20, 100, 300, 750, 925, 1100], mortgage: 120 },
      { id: 25, type: 'station', name: 'YHT GAR', group: 'istasyon', price: 200, rentByCount: [25, 50, 100, 200], mortgage: 100 },
      { id: 26, type: 'property', name: 'ESKİŞEHİR', group: 'sari', price: 260, rent: [22, 110, 330, 800, 975, 1150], mortgage: 130 },
      { id: 27, type: 'property', name: 'KAHRAMANMARAŞ', group: 'sari', price: 260, rent: [22, 110, 330, 800, 975, 1150], mortgage: 130 },
      { id: 28, type: 'utility', name: 'SÖZERİ', group: 'servis', price: 150, mortgage: 75 },
      { id: 29, type: 'property', name: 'KONYA', group: 'sari', price: 280, rent: [24, 120, 360, 850, 1025, 1200], mortgage: 140 },
      { id: 30, type: 'go-to-jail', name: "SİLİVRİ'YE GİT" },
      { id: 31, type: 'property', name: 'IF SOKAK', group: 'yesil', price: 300, rent: [26, 130, 390, 900, 1100, 1275], mortgage: 150 },
      { id: 32, type: 'property', name: 'LAST PENNY', group: 'yesil', price: 300, rent: [26, 130, 390, 900, 1100, 1275], mortgage: 150 },
      { id: 33, type: 'community', name: 'KAMU SANDIĞI' },
      { id: 34, type: 'property', name: 'BOTANICA', group: 'yesil', price: 320, rent: [28, 150, 450, 1000, 1200, 1400], mortgage: 160 },
      { id: 35, type: 'station', name: 'ESENBOĞA HAVALİMANI', group: 'istasyon', price: 200, rentByCount: [25, 50, 100, 200], mortgage: 100 },
      { id: 36, type: 'chance', name: 'ŞANS' },
      { id: 37, type: 'property', name: "MAZUR'UN EVİ", group: 'lacivert', price: 350, rent: [35, 175, 500, 1100, 1300, 1500], mortgage: 175 },
      { id: 38, type: 'tax', name: 'LÜKS VERGİ', amount: 100 },
      { id: 39, type: 'property', name: "DİLA'NIN EVİ", group: 'lacivert', price: 400, rent: [50, 200, 600, 1400, 1700, 2000], mortgage: 200 },
    ];

    // ŞANS KARTLARI LİSTESİ
    const ORIGINAL_CHANCE = [
      { text: 'Dersler çok yordu. Ankuva Merdivenler\'e ilerle. Başlangıçtan geçersen M200 bursunu al.', kind: 'move', target: 24, rewardOnPass: true },
      { text: 'En yakın ulaşıma git. Sahipsizse satın alabilirsin.', kind: 'nearest-station' },
      { text: 'En yakın ulaşıma git. Sahipsizse satın alabilirsin.', kind: 'nearest-station' },
      { text: 'En yakın ulaşıma git. Sahipsizse satın alabilirsin.', kind: 'nearest-station' },
      { text: 'Başlangıca ilerle, bursun yatacak. (M200 AL)', kind: 'move', target: 0, rewardOnPass: false },
      { text: 'Cücü ve Çita\'nın kumu temizlenmeli. Dila\'nın Evi\'ne ilerle.', kind: 'move', target: 39, rewardOnPass: false },
      { text: 'Yeşim hoca DM\'den trip attı. Doğu Yemekhane\'ye gitmen lazım. Başlangıçtan geçersen, bursunu al.', kind: 'move', target: 11, rewardOnPass: true },
      { text: 'Üç adım geri git.', kind: 'move-relative', steps: -3 },
      { text: 'Buse bir gece ansızın gruptan çıktı. Artık otobüse biniyorsunuz. M15 öde.', kind: 'money', amount: -15 },
      { text: 'Nehirkoy kakasını yapabildi ve herkesle girdiğin iddiayı kaybettin. Herkese M50 öde.', kind: 'pay-each', amount: 50 },
      { text: 'Attığın siyasi tweet hit oldu. Silivri seni bekler, direkt ilerle. Başlangıçtan geçersen para da alma.', kind: 'go-to-jail' },
      { text: 'Otururken canın sıkıldı. Sıhhiye Ringi\'ne git. Başlangıçtan geçersen, bursunu al.', kind: 'move', target: 5, rewardOnPass: true },
      { text: 'Onur CB oldu ve konut vergisi getirdi. Her evin için, bankaya M25 öde. Her otelin için, bankaya M100 öde.', kind: 'property-tax', house: 25, hotel: 100 },
      { text: 'Zeynepaygun Misbaşak\'tan prim aldı ve sana verdi. M50 kazandın.', kind: 'money', amount: 50 },
      { text: 'Mazur kupon tutturdu. M150 kazandın.', kind: 'money', amount: 150 },
      { text: 'Torpil buldun, hapisten çıkabilirsin. Bu kart saklanabilir.', kind: 'get-out-of-jail' }
    ];

    // KAMU SANDIĞI KARTLARI LİSTESİ
    const ORIGINAL_COMMUNITY = [
      { text: 'Başlangıca ilerle, bursun yatacak. (M200 AL)', kind: 'move', target: 0, rewardOnPass: false },
      { text: 'Dilanın kirasının ödeme günü geldi. İyi bir dost ol. M200 öde.', kind: 'money', amount: -200 },
      { text: 'HIST200\'de diğer grubun üyelerini öldürdün. Silivri seni bekler, direkt ilerle.', kind: 'go-to-jail' },
      { text: 'Beste\'yi çamaşır gününde dışarı çağırdın. Kuru temizlemenin parasını öde. M50.', kind: 'money', amount: -50 },
      { text: 'Çağdaş Apartmanı\'na gittin ve çok beğendin. Kendi evlerine restorasyon yaptıracaksın. Sahip olduğun ev başına M40 öde.', kind: 'property-tax', house: 40, hotel: 115 },
      { text: 'Okul ücreti zamlandı. M100 öde.', kind: 'money', amount: -100 },
      { text: 'Dila, Sarıgül marka tarhana getirdi ama parasını ödememiş. Bankaya M50 öde.', kind: 'money', amount: -50 },
      { text: 'Springfest konserine kaçak girdin. M25 kazandın.', kind: 'money', amount: 25 },
      { text: 'Doruk\'la tanışmaya cesaret ettin. Sana kahve ısmarladı. M10 kazandın.', kind: 'money', amount: 10 },
      { text: 'BTO\'ya katıldın ve okul tüm bütçesini size verdi. M200 kazandın.', kind: 'money', amount: 200 },
      { text: '"SimonHarveyBrunkerJames kaç kere fuck der" iddiasını kazandın. M50 al.', kind: 'money', amount: 50 },
      { text: 'Ortak GPT üyeliğini sen ödedin. Her oyuncudan M10 topla.', kind: 'collect-each', amount: 10 },
      { text: 'Ece nesli tükenmek üzere olan bir böcek yakalayıp sattı. Parasını da sana verdi. M100 kazandın.', kind: 'money', amount: 100 },
      { text: 'Konya gezisinde dilencilik yaptın. M20 kazandın.', kind: 'money', amount: 20 },
      { text: 'Günlerdir yemekhanede yiyorsun. Para biriktirdin. M100 al.', kind: 'money', amount: 100 },
      { text: 'OR partisine bilet kazandın. M100 al.', kind: 'money', amount: 100 }
    ];

    const state = {
      players: [], currentPlayer: 0, owned: {}, tileStates: {}, dice: [1, 1],
      selectedTileId: 0, pendingTileId: null, message: '', log: [],
      chanceDeck: [], communityDeck: [], gameOver: false, turnStarted: false,
      turnSummary: 'Henüz bir hamle yapılmadı.',
      stats: {},
    };

    const el = {
      board: document.getElementById('board'), selectedTile: document.getElementById('selectedTile'),
      playersList: document.getElementById('playersList'), logList: document.getElementById('logList'),
      message: document.getElementById('message'), activePlayerName: document.getElementById('activePlayerName'),
      diceBox: document.getElementById('diceBox'), rollBtn: document.getElementById('rollBtn'),
      buyBtn: document.getElementById('buyBtn'), endTurnBtn: document.getElementById('endTurnBtn'),
      buildBtn: document.getElementById('buildBtn'), mortgageBtn: document.getElementById('mortgageBtn'),
      playerCount: document.getElementById('playerCount'), newGameBtn: document.getElementById('newGameBtn'),
      cardModalOverlay: document.getElementById('cardModalOverlay'), cardModalHeader: document.getElementById('cardModalHeader'),
      cardModalBody: document.getElementById('cardModalBody'), cardModalBtn: document.getElementById('cardModalBtn'),
      turnSummary: document.getElementById('turnSummary'),
      statsPanel: document.getElementById('statsPanel'),
      setupOverlay: document.getElementById('setupOverlay'), setupBody: document.getElementById('setupBody'),
      setupStartBtn: document.getElementById('setupStartBtn'),
      saveBtn: document.getElementById('saveBtn'), loadBtn: document.getElementById('loadBtn'),
      toast: document.getElementById('toast'),
    };

    // DESTE KARIŞTIRICI
    function shuffleArray(array) {
      const arr = [...array];
      for (let i = arr.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [arr[i], arr[j]] = [arr[j], arr[i]];
      }
      return arr;
    }

    function formatMoney(v) { return `M${v}`; }
    function selectedTile() { return board[state.selectedTileId]; }
    function activePlayer() { return state.players[state.currentPlayer]; }
    function tileState(tileId) { return state.tileStates[tileId] || { houses: 0, hotel: false, mortgaged: false }; }


    function cleanTileName(name) { return name.replace('<br>', ' '); }

    function setTurnSummary(text) {
      state.turnSummary = text;
    }

    function getOwnerRibbonHTML(tile) {
      const ownerId = state.owned[tile.id];
      if (!ownerId) return '';
      const owner = state.players.find(p => p.id === ownerId);
      if (!owner) return '';
      return `<div class="owner-ribbon" style="background:${owner.color}"></div>`;
    }

    // GÜNCELLENEN CREATE PLAYERS FONKSİYONU
    function createPlayers(count, playersData = []) {
      return Array.from({ length: count }, (_, i) => ({
        id: i + 1, 
        name: playersData[i]?.name || `Oyuncu ${i + 1}`, 
        emoji: playersData[i]?.emoji || EMOJIS[i],
        color: TOKENS[i].color,
        money: 1500, position: 0, properties: [], inJail: false, jailTurns: 0, getOutOfJailCards: 0,
      }));
    }

    function initStats(players) {
      const s = {};
      players.forEach(p => {
        s[p.id] = { diceRolls: 0, totalRentPaid: 0, totalRentReceived: 0 };
      });
      return s;
    }

    function addLog(text) {
      state.log.unshift(text);
      state.log = state.log.slice(0, 24);
    }

    function getRingPosition(index) {
      if (index <= 10) return { row: 10, col: 10 - index };
      if (index <= 20) return { row: 20 - index, col: 0 };
      if (index <= 30) return { row: 0, col: index - 20 };
      return { row: index - 30, col: 10 };
    }

    function getTileSide(tileId) {
      if (tileId === 0 || (tileId > 0 && tileId < 10)) return 'bottom';
      if (tileId === 10 || (tileId > 10 && tileId < 20)) return 'left';
      if (tileId === 20 || (tileId > 20 && tileId < 30)) return 'top';
      return 'right';
    }

    function getTileIcon(tile) {
      if (tile.type === 'station') return '🚋';
      if (tile.type === 'utility' && tile.name.toLowerCase().includes('coffee')) return '☕';
      if (tile.type === 'utility') return '🚰';
      if (tile.type === 'chance') return '❓';
      if (tile.type === 'community') return '🗳️';
      if (tile.type === 'tax') return '◈';
      return '';
    }

    function getTileTypeLabel(tile) {
      if (tile.type === 'property') return 'Tapu';
      if (tile.type === 'station') return 'Ulaşım';
      if (tile.type === 'utility') return 'Servis';
      return 'Özel Alan';
    }

    function playerOnTile(tileId) {
      return state.players.filter(p => p.position === tileId);
    }

    function nextStation(position) {
      const stations = [5, 15, 25, 35];
      return stations.find(s => s > position) ?? stations[0];
    }

    function checkBankruptcy() {
      const losers = state.players.filter(p => p.money < 0);
      if (losers.length) {
        state.gameOver = true;
        const winner = [...state.players].sort((a, b) => b.money - a.money)[0];
        state.message = `${winner.name} kazandı.`;
        addLog(`${winner.name} oyunu kazandı.`);
      }
    }

    function payBank(playerId, amount, reason) {
      const p = state.players.find(x => x.id === playerId);
      p.money -= amount;
      if (reason) addLog(reason);
      checkBankruptcy();
    }

    function receiveBank(playerId, amount, reason) {
      const p = state.players.find(x => x.id === playerId);
      p.money += amount;
      if (reason) addLog(reason);
    }

    function transferMoney(fromId, toId, amount) {
      const from = state.players.find(p => p.id === fromId);
      const to = state.players.find(p => p.id === toId);
      from.money -= amount;
      to.money += amount;
      if (state.stats[fromId]) state.stats[fromId].totalRentPaid += amount;
      if (state.stats[toId]) state.stats[toId].totalRentReceived += amount;
      checkBankruptcy();
    }

    function canBuildOnTile(tile, ownerId) {
      if (tile.type !== 'property') return false;
      const sameGroup = board.filter(t => t.type === 'property' && t.group === tile.group);
      return sameGroup.every(t => state.owned[t.id] === ownerId && !tileState(t.id).mortgaged);
    }

    function getRent(tile, ownerId) {
      const current = tileState(tile.id);
      if (current.mortgaged) return 0;

      if (tile.type === 'property') {
        if (current.hotel) return tile.rent[5];
        if (current.houses > 0) return tile.rent[current.houses];
        const sameGroup = board.filter(t => t.type === 'property' && t.group === tile.group);
        const monopoly = sameGroup.every(t => state.owned[t.id] === ownerId && !tileState(t.id).mortgaged);
        return monopoly ? tile.rent[0] * 2 : tile.rent[0];
      }

      if (tile.type === 'station') {
        const count = Object.entries(state.owned)
          .filter(([id, pid]) => Number(pid) === ownerId && board[Number(id)]?.type === 'station' && !tileState(Number(id)).mortgaged)
          .length;
        return tile.rentByCount[Math.max(0, count - 1)] || tile.rentByCount[0];
      }

      if (tile.type === 'utility') {
        const count = Object.entries(state.owned)
          .filter(([id, pid]) => Number(pid) === ownerId && board[Number(id)]?.type === 'utility' && !tileState(Number(id)).mortgaged)
          .length;
        const diceTotal = state.dice[0] + state.dice[1];
        return diceTotal * (count >= 2 ? 10 : 4);
      }

      return 0;
    }

    function movePlayerTo(playerId, target, rewardOnPass, reason) {
      const player = state.players.find(p => p.id === playerId);
      const startPos = player.position;
      const passedGo = rewardOnPass ? target < startPos : false;
      if (passedGo) player.money += 200;
      if (reason) addLog(reason);
      state.message = `${player.name} kart etkisiyle ilerliyor...`;
      setTurnSummary(`${player.name}, ${cleanTileName(board[target].name)} karesine taşınıyor.`);
      animateMove(playerId, startPos, target, () => {
        setTurnSummary(`${player.name}, ${cleanTileName(board[target].name)} karesine taşındı.`);
        resolveLanding(target);
      });
    }

    function executeCard(card) {
      const player = activePlayer();

      if (card.kind === 'money') {
        if (card.amount >= 0) receiveBank(player.id, card.amount, `${player.name}: Karttan M${card.amount} aldı.`);
        else payBank(player.id, Math.abs(card.amount), `${player.name}: Karta M${Math.abs(card.amount)} ödedi.`);
        state.message = "Kartın etkisi uygulandı.";
        setTurnSummary(`${player.name}, kart etkisiyle ${card.amount >= 0 ? `M${card.amount} aldı` : `M${Math.abs(card.amount)} ödedi`}.`);
        return;
      }

      if (card.kind === 'collect-each') {
        state.players.forEach(p => {
          if (p.id === player.id) p.money += card.amount * (state.players.length - 1);
          else p.money -= card.amount;
        });
        addLog(`${player.name} herkesten M${card.amount} topladı.`);
        state.message = "Paralar toplandı.";
        setTurnSummary(`${player.name}, tüm oyunculardan M${card.amount} topladı.`);
        checkBankruptcy();
        return;
      }

      if (card.kind === 'pay-each') {
        state.players.forEach(p => {
          if (p.id === player.id) p.money -= card.amount * (state.players.length - 1);
          else p.money += card.amount;
        });
        addLog(`${player.name} herkese M${card.amount} ödedi.`);
        state.message = "Paralar dağıtıldı.";
        setTurnSummary(`${player.name}, tüm oyunculara M${card.amount} ödedi.`);
        checkBankruptcy();
        return;
      }

      if (card.kind === 'get-out-of-jail') {
        player.getOutOfJailCards += 1;
        addLog(`${player.name} hapisten çıkış kartı aldı.`);
        state.message = "Kart saklandı.";
        setTurnSummary(`${player.name}, hapisten çıkış kartı aldı.`);
        return;
      }

      if (card.kind === 'nearest-station') {
        movePlayerTo(player.id, nextStation(player.position), false, `${player.name} en yakın ulaşıma gitti.`);
        return;
      }

      if (card.kind === 'move-relative') {
        movePlayerTo(player.id, (player.position + card.steps + board.length) % board.length, false, `${player.name} ${Math.abs(card.steps)} adım geri gitti.`);
        return;
      }

      if (card.kind === 'move') {
        movePlayerTo(player.id, card.target, card.rewardOnPass, `${player.name} kart ile ilerledi.`);
        return;
      }

      if (card.kind === 'go-to-jail') {
        player.position = 10;
        setTurnSummary(`${player.name}, kart yüzünden Silivri'ye gitti.`);
        player.inJail = true;
        player.jailTurns = 0;
        state.selectedTileId = 10;
        state.pendingTileId = 10;
        state.message = `${player.name} Silivri'ye gitti.`;
        addLog(`${player.name} hapse girdi.`);
        return;
      }

      if (card.kind === 'property-tax') {
        let totalTax = 0;
        player.properties.forEach(tileId => {
          const ts = tileState(tileId);
          if (ts.hotel) totalTax += card.hotel;
          else totalTax += ts.houses * card.house;
        });
        if (totalTax > 0) {
          payBank(player.id, totalTax, `${player.name}, mülk vergisi olarak M${totalTax} ödedi.`);
          state.message = `Mülk vergisi: M${totalTax} ödedin.`;
          setTurnSummary(`${player.name}, toplam ${formatMoney(totalTax)} mülk vergisi ödedi.`);
        } else {
          state.message = `Evin/otelin olmadığı için vergi ödemedin.`;
          setTurnSummary(`${player.name}, mülk vergisi kartı çekti ama evi olmadığı için ödeme yapmadı.`);
          addLog(`${player.name} hiç evi olmadığı için vergi ödemedi.`);
        }
        return;
      }
    }

    function showCardPopup(type, card) {
      el.cardModalHeader.className = `card-modal-header ${type}`;
      if (type === 'chance') {
        el.cardModalHeader.innerHTML = '❓ ŞANS KARTI';
      } else {
        el.cardModalHeader.innerHTML = '🗳️ KAMU SANDIĞI';
      }
      
      el.cardModalBody.innerHTML = card.text;
      setTurnSummary(`${activePlayer().name}, ${type === 'chance' ? 'Şans' : 'Kamu Sandığı'} kartı çekti: ${card.text}`);
      el.cardModalOverlay.classList.add('active');

      el.cardModalBtn.onclick = () => {
        el.cardModalOverlay.classList.remove('active');
        executeCard(card);
        render();
      };
    }

    function resolveLanding(tileIndex) {
      const tile = board[tileIndex];
      const player = activePlayer();
      state.selectedTileId = tile.id;
      state.pendingTileId = tile.id;

      // KART ÇEKME VE DESTE DÖNGÜSÜ MANTIĞI
      if (tile.type === 'chance') {
        const card = state.chanceDeck.shift(); // En üstten çek
        state.chanceDeck.push(card); // En alta koy (bitene kadar bir daha çıkmaz)
        showCardPopup('chance', card);
        return; 
      }

      if (tile.type === 'community') {
        const card = state.communityDeck.shift(); // En üstten çek
        state.communityDeck.push(card); // En alta koy
        showCardPopup('community', card);
        return; 
      }

      if (['property', 'station', 'utility'].includes(tile.type)) {
        const ownerId = state.owned[tile.id];
        if (!ownerId) {
          state.message = `${tile.name.replace('<br>',' ')} sahipsiz. Satın alabilirsin.`;
          setTurnSummary(`${player.name}, ${cleanTileName(tile.name)} karesine geldi. Kare sahipsiz.`);
          addLog(`${player.name}, ${tile.name.replace('<br>',' ')} üzerine geldi.`);
          render();
          return;
        }
        if (ownerId === player.id) {
          state.message = `${tile.name.replace('<br>',' ')} zaten sende.`;
          setTurnSummary(`${player.name}, kendi mülkü olan ${cleanTileName(tile.name)} karesine geldi.`);
          render();
          return;
        }
        if (tileState(tile.id).mortgaged) {
          state.message = `${tile.name.replace('<br>',' ')} ipotekli olduğu için kira yok.`;
          setTurnSummary(`${player.name}, ${cleanTileName(tile.name)} karesine geldi ama mülk ipotekliydi.`);
          addLog(`${tile.name.replace('<br>',' ')} ipotekliydi, kira alınmadı.`);
          render();
          return;
        }
        const rent = getRent(tile, ownerId);
        transferMoney(player.id, ownerId, rent);
        const owner = state.players.find(p => p.id === ownerId);
        state.message = `${player.name}, ${owner.name}'e ${formatMoney(rent)} kira ödedi.`;
        setTurnSummary(`${player.name}, ${cleanTileName(tile.name)} için ${owner.name}'e ${formatMoney(rent)} kira ödedi.`);
        addLog(`${player.name}, ${tile.name.replace('<br>',' ')} için ${owner.name}'e ${formatMoney(rent)} ödedi.`);
        render();
        return;
      }

      if (tile.type === 'tax') {
        payBank(player.id, tile.amount, `${player.name}, ${tile.name.replace('<br>',' ')} için ${formatMoney(tile.amount)} ödedi.`);
        state.message = `${tile.name.replace('<br>',' ')}: ${formatMoney(tile.amount)} ödedin.`;
        setTurnSummary(`${player.name}, ${cleanTileName(tile.name)} nedeniyle ${formatMoney(tile.amount)} ödedi.`);
        render();
        return;
      }

      if (tile.type === 'go-to-jail') {
        player.position = 10;
        player.inJail = true;
        player.jailTurns = 0;
        state.selectedTileId = 10;
        state.pendingTileId = 10;
        state.message = `${player.name} Silivri'ye gitti.`;
        setTurnSummary(`${player.name}, direkt Silivri'ye gönderildi.`);
        addLog(`${player.name} Silivri'ye gitti.`);
        render();
        return;
      }

      if (tile.type === 'free-parking') {
        state.message = 'Free Parking. Bu tur rahat.';
        setTurnSummary(`${player.name}, Free Parking karesine geldi.`);
        addLog(`${player.name} Free Parking'e geldi.`);
        render();
        return;
      }

      if (tile.type === 'jail') {
        state.message = 'Sadece ziyaret.';
        setTurnSummary(`${player.name}, Silivri'yi sadece ziyaret etti.`);
        render();
        return;
      }

      state.message = 'Başlangıç karesi.';
      setTurnSummary(`${player.name}, başlangıç karesine geldi.`);
      render();
    }

    function rollDice() {
      if (state.gameOver || state.turnStarted) return;
      const player = activePlayer();
      const d1 = Math.floor(Math.random() * 6) + 1;
      const d2 = Math.floor(Math.random() * 6) + 1;
      const total = d1 + d2;
      state.dice = [d1, d2];
      state.turnStarted = true;
      if (state.stats[player.id]) state.stats[player.id].diceRolls++;
      setTurnSummary(`${player.name} zar attı: ${d1} + ${d2} = ${total}.`);

      if (player.inJail) {
        if (d1 === d2) {
          player.inJail = false;
          player.jailTurns = 0;
          addLog(`${player.name} çift atıp Silivri'den çıktı.`);
        } else if (player.getOutOfJailCards > 0) {
          player.inJail = false;
          player.getOutOfJailCards -= 1;
          addLog(`${player.name} hapisten çıkış kartı kullandı.`);
        } else if (player.jailTurns >= 2) {
          player.inJail = false;
          player.jailTurns = 0;
          player.money -= 50;
          addLog(`${player.name} M50 ödeyip Silivri'den çıktı.`);
        } else {
          player.jailTurns += 1;
          state.message = `${player.name} Silivri'de kaldı. Turu bitir.`;
          setTurnSummary(`${player.name} bu tur Silivri'den çıkamadı.`);
          addLog(`${player.name} çıkamadı.`);
          render();
          return;
        }
      }

      const oldPos = player.position;
      const newPos = (oldPos + total) % board.length;
      const passedGo = oldPos + total >= board.length;
      if (passedGo) {
        player.money += 200;
        addLog(`${player.name} başlangıçtan geçti ve M200 aldı.`);
      }
      addLog(`${player.name} zarı attı: ${d1} + ${d2} = ${total}`);
      state.message = `${player.name} ${total} ilerliyor...`;
      setTurnSummary(`${player.name}, ${total} kare ilerliyor.`);
      animateMove(player.id, oldPos, newPos, () => {
        state.message = `${player.name} ${total} ilerledi.`;
        setTurnSummary(`${player.name}, ${cleanTileName(board[newPos].name)} karesine geldi.`);
        resolveLanding(newPos);
      });
    }

    function buyTile() {
      const tileId = state.pendingTileId;
      if (tileId == null) return;
      const tile = board[tileId];
      if (!['property', 'station', 'utility'].includes(tile.type)) return;
      if (state.owned[tile.id]) return;
      const player = activePlayer();
      if (player.money < tile.price) {
        state.message = 'Yeterli paran yok.';
        render();
        return;
      }
      state.owned[tile.id] = player.id;
      state.tileStates[tile.id] = { houses: 0, hotel: false, mortgaged: false };
      player.money -= tile.price;
      player.properties.push(tile.id);
      state.message = `${tile.name.replace('<br>',' ')} satın alındı.`;
      setTurnSummary(`${player.name}, ${cleanTileName(tile.name)} mülkünü satın aldı.`);
      addLog(`${player.name}, ${tile.name.replace('<br>',' ')} satın aldı (${formatMoney(tile.price)}).`);
      render();
    }

    function buildOnSelectedTile() {
      const tile = selectedTile();
      const player = activePlayer();
      if (!tile || tile.type !== 'property') return;
      if (state.owned[tile.id] !== player.id) {
        state.message = 'Bu mülk sende değil.';
        render();
        return;
      }
      if (!canBuildOnTile(tile, player.id)) {
        state.message = 'Bu renk grubunun tamamı sende ve ipoteksiz olmalı.';
        render();
        return;
      }
      const current = tileState(tile.id);
      if (current.hotel) {
        state.message = 'Bu karede zaten otel var.';
        render();
        return;
      }
      const cost = HOUSE_COST[tile.group] || 100;
      if (player.money < cost) {
        state.message = 'Yeterli paran yok.';
        render();
        return;
      }
      player.money -= cost;
      if (current.houses >= 4) {
        current.hotel = true;
        current.houses = 4;
        state.message = `${tile.name.replace('<br>',' ')} üzerine otel kuruldu.`;
        setTurnSummary(`${player.name}, ${cleanTileName(tile.name)} üzerine otel kurdu.`);
        addLog(`${player.name}, ${tile.name.replace('<br>',' ')} için otel aldı.`);
      } else {
        current.houses += 1;
        state.message = `${tile.name.replace('<br>',' ')} üzerine ev eklendi.`;
        setTurnSummary(`${player.name}, ${cleanTileName(tile.name)} üzerine bir ev ekledi.`);
        addLog(`${player.name}, ${tile.name.replace('<br>',' ')} için ev aldı.`);
      }
      state.tileStates[tile.id] = current;
      render();
    }

    function mortgageSelectedTile() {
      const tile = selectedTile();
      const player = activePlayer();
      if (!tile || !state.owned[tile.id] || state.owned[tile.id] !== player.id) {
        state.message = 'Sadece kendi mülkünü ipotek edebilirsin.';
        render();
        return;
      }
      const current = tileState(tile.id);
      if (current.houses > 0 || current.hotel) {
        state.message = 'İpotek için önce ev/oteli kaldırmalısın.';
        render();
        return;
      }
      if (current.mortgaged) {
        const repay = Math.ceil((tile.mortgage || 0) * 1.1);
        if (player.money < repay) {
          state.message = 'İpoteği kaldıracak paran yok.';
          render();
          return;
        }
        player.money -= repay;
        current.mortgaged = false;
        state.message = `${tile.name.replace('<br>',' ')} ipoteği kaldırıldı.`;
        setTurnSummary(`${player.name}, ${cleanTileName(tile.name)} ipoteğini kaldırdı.`);
        addLog(`${player.name}, ${tile.name.replace('<br>',' ')} ipoteğini kaldırdı.`);
      } else {
        player.money += tile.mortgage || 0;
        current.mortgaged = true;
        state.message = `${tile.name.replace('<br>',' ')} ipotek edildi.`;
        setTurnSummary(`${player.name}, ${cleanTileName(tile.name)} mülkünü ipotek etti.`);
        addLog(`${player.name}, ${tile.name.replace('<br>',' ')} ipotek etti.`);
      }
      state.tileStates[tile.id] = current;
      render();
    }

    function endTurn() {
      if (state.gameOver) return;
      state.pendingTileId = null;
      state.turnStarted = false;
      state.currentPlayer = (state.currentPlayer + 1) % state.players.length;
      state.message = 'Sıra sonraki oyuncuda. Zarı atabilirsiniz.';
      setTurnSummary(`${activePlayer().name} turunu bitirdi. Şimdi ${state.players[(state.currentPlayer + 1) % state.players.length].name} oynuyor.`);
      render();
    }

    function getNetWorth(player) {
      let worth = player.money;
      player.properties.forEach(id => {
        const tile = board[id];
        const ts = tileState(id);
        if (!ts.mortgaged) {
          worth += tile.price;
          worth += ts.houses * (HOUSE_COST[tile.group] || 0);
          if (ts.hotel) worth += (HOUSE_COST[tile.group] || 0);
        } else {
          worth += tile.mortgage || 0;
        }
      });
      return worth;
    }

    function showToast(msg) {
      el.toast.textContent = msg;
      el.toast.classList.add('show');
      setTimeout(() => el.toast.classList.remove('show'), 2200);
    }

    function saveGame() {
      try {
        localStorage.setItem('novicepoly_save', JSON.stringify(state));
        showToast('✅ Oyun kaydedildi!');
      } catch(e) {
        showToast('❌ Kayıt başarısız.');
      }
    }

    function loadGame() {
      try {
        const saved = localStorage.getItem('novicepoly_save');
        if (!saved) { showToast('📂 Kayıtlı oyun bulunamadı.'); return; }
        const loaded = JSON.parse(saved);
        Object.assign(state, loaded);
        showToast('📂 Oyun yüklendi!');
        render();
      } catch(e) {
        showToast('❌ Yükleme başarısız.');
      }
    }

    // GÜNCELLENEN SETUP EKRANI (EMOJİ SEÇİCİ İLE)
    function showSetupModal(count) {
      el.setupBody.innerHTML = Array.from({ length: count }, (_, i) => `
        <div class="setup-player-row">
          <div class="setup-color-dot" style="background:${TOKENS[i].color}"></div>
          <select class="setup-emoji-select" id="emojiInput${i}">
            ${EMOJIS.map((e, idx) => `<option value="${e}" ${idx === i ? 'selected' : ''}>${e}</option>`).join('')}
          </select>
          <input class="setup-input" id="nameInput${i}" type="text"
            placeholder="Oyuncu ${i + 1}" maxlength="18"
            value="" autocomplete="off" />
        </div>
      `).join('');
      el.setupOverlay.classList.add('active');
      document.getElementById('nameInput0')?.focus();
    }

    // GÜNCELLENEN BAŞLATMA FONKSİYONU
    function startGameWithNames(count) {
      const playersData = Array.from({ length: count }, (_, i) => {
        const name = document.getElementById(`nameInput${i}`)?.value.trim() || `Oyuncu ${i + 1}`;
        const emoji = document.getElementById(`emojiInput${i}`)?.value || EMOJIS[i];
        return { name, emoji };
      });
      el.setupOverlay.classList.remove('active');
      newGame(count, playersData); 
    }

    // GÜNCELLENEN YENİ OYUN FONKSİYONU
    function newGame(count, playersData = []) {
      state.players = createPlayers(count, playersData);
      state.currentPlayer = 0;
      state.owned = {};
      state.tileStates = {};
      state.dice = [1, 1];
      state.selectedTileId = 0;
      state.pendingTileId = null;
      state.message = 'Oyun sıfırlandı. İlk oyuncu zarı atabilir.';
      state.log = ['Oyun yeniden başladı.'];
      state.gameOver = false;
      state.turnStarted = false;
      state.turnSummary = 'Henüz bir hamle yapılmadı.';
      state.stats = initStats(state.players);

      state.chanceDeck = shuffleArray(ORIGINAL_CHANCE);
      state.communityDeck = shuffleArray(ORIGINAL_COMMUNITY);

      render();
    }

    function animateMove(playerId, fromPos, toPos, onComplete) {
      const player = state.players.find(p => p.id === playerId);
      if (!player) { onComplete(); return; }
      const totalSteps = ((toPos - fromPos) + board.length) % board.length;
      if (totalSteps === 0) { onComplete(); return; }

      let step = 0;
      function tick() {
        step += 1;
        player.position = (fromPos + step) % board.length;
        render();
        if (step < totalSteps) {
          setTimeout(tick, 120);
        } else {
          onComplete();
        }
      }
      tick();
    }

    /* RENDER ENGINE */
    function renderBoard() {
      el.board.innerHTML = '';

      board.forEach(tile => {
        const pos = getRingPosition(tile.id);
        const current = tileState(tile.id);
        const occupants = playerOnTile(tile.id);
        const side = getTileSide(tile.id);
        const icon = getTileIcon(tile);
        const isCorner = [0, 10, 20, 30].includes(tile.id);

        const div = document.createElement('div');
        div.className = `tile ${isCorner ? 'corner' : 'normal'} ${state.selectedTileId === tile.id ? 'selected' : ''} ${side}`;
        div.style.gridColumn = String(pos.col + 1);
        div.style.gridRow = String(pos.row + 1);

        if (isCorner) {
          let inner = '';
          if (tile.id === 0) {
            inner = `
              <div style="position:absolute; inset:8px; display:flex; flex-direction:column; justify-content:space-between; align-items:center;">
                <div style="transform: rotate(-45deg) translate(-10%, 15%); display:flex; flex-direction:column; align-items:center; width: 140%;">
                  <div style="font-size: clamp(4px, 1.3cqw, 10px); font-weight: 900; line-height: 1.1; text-align: center; white-space: nowrap;">AFERİN<br>M200 BURSUN<br>YATTI</div>
                  <div style="font-size: clamp(18px, 4.5cqw, 60px); font-weight: 900; line-height: 0.8; letter-spacing: -0.05em; margin-top:2px;">GO</div>
                </div>
                <div style="color:#d92323; font-size: clamp(14px, 3.5cqw, 44px); font-weight:900; line-height:0.5; width:100%; text-align:center; transform: scaleX(1.8) translateY(-4px);">⬅</div>
              </div>
            `;
          }
          else if (tile.id === 10) inner = `<div style="transform: rotate(45deg); text-align:center;"><div style="font-size: clamp(10px, 1.8cqw, 24px); font-weight: 900;">SİLİVRİ</div><div style="font-size: clamp(6px, 1.2cqw, 11px); font-weight: 700; margin-top:5px;">DON GETİRDİM</div></div>`;
          else if (tile.id === 20) inner = `<div style="transform: rotate(135deg); text-align:center;"><div style="font-size: clamp(6px, 1.2cqw, 11px); font-weight: 700;">MESCİT</div><div style="font-size: clamp(14px, 2.5cqw, 34px); color:#d92323;">🚘</div><div style="font-size: clamp(10px, 1.8cqw, 24px); font-weight: 900;">OTOPARK</div></div>`;
          else if (tile.id === 30) inner = `<div style="transform: rotate(-135deg); text-align:center;"><div style="font-size: clamp(10px, 1.8cqw, 24px); font-weight: 900;">SİLİVRİ</div><div style="font-size: clamp(14px, 2.5cqw, 34px);">👮</div><div style="font-size: clamp(10px, 1.8cqw, 24px); font-weight: 900;">ZAMANI</div></div>`;
          
          // GÜNCELLENEN PİYON RENDER KISMI (EMOJİLİ)
          div.innerHTML = `${getOwnerRibbonHTML(tile)}<div class="tile-inner">${inner}</div><div class="player-dots">${occupants.map(p => `<span class="mini-player" style="background:${p.color}">${p.emoji}</span>`).join('')}</div>`;
        } else {
          const hasColorBar = tile.type === 'property';
          let innerContent = '';

          if (hasColorBar) {
            innerContent = `
              <div class="color-bar" style="background:${COLORS[tile.group]}"></div>
              <div class="tile-body">
                <div class="tile-title">${tile.name}</div>
                <div class="mini-tags">
                  ${current.hotel ? '<span class="mini-hotel">O</span>' : ''}
                  ${!current.hotel && current.houses > 0 ? Array.from({ length: current.houses }).map(() => '<span class="mini-house" style="background:#22c55e"></span>').join('') : ''}
                  ${current.mortgaged ? '<span class="mortgage-badge">İP</span>' : ''}
                </div>
                <div class="tile-price">${formatMoney(tile.price)}</div>
              </div>
            `;
          } else {
            innerContent = `
              <div class="tile-body">
                <div class="tile-title">${tile.name}</div>
                ${icon ? `<div class="tile-icon">${icon}</div>` : ''}
                ${tile.price != null ? `<div class="tile-price">${formatMoney(tile.price)}</div>` : ''}
                ${tile.amount != null ? `<div class="tile-price">ÖDE M${tile.amount}</div>` : ''}
              </div>
            `;
          }

          // GÜNCELLENEN PİYON RENDER KISMI (EMOJİLİ)
          div.innerHTML = `
            ${getOwnerRibbonHTML(tile)}
            <div class="tile-inner">
              ${innerContent}
            </div>
            <div class="player-dots">
              ${occupants.map(p => `<span class="mini-player" style="background:${p.color}">${p.emoji}</span>`).join('')}
            </div>
          `;
        }

        div.addEventListener('click', () => { state.selectedTileId = tile.id; render(); });
        el.board.appendChild(div);
      });

      const center = document.createElement('div');
      center.className = 'center-panel';
      center.innerHTML = `
        <div class="center-card one">
          <div class="card-title">Kamu Sandığı</div>
          <div class="card-icon" style="color:white; text-shadow:0 2px 4px rgba(0,0,0,0.3)">🗳️</div>
        </div>
        <div class="center-title">NOVICEPOLY</div>
        <div class="center-card two">
          <div class="card-title" style="color:white">Şans</div>
          <div class="card-icon" style="color:white; text-shadow:0 2px 4px rgba(0,0,0,0.3)">❓</div>
        </div>
      `;
      el.board.appendChild(center);
    }

    function renderSelectedTile() {
      const tile = selectedTile();
      const current = tileState(tile.id);
      let html = `
        <div style="display:flex;gap:12px;align-items:center;">
          <div style="width:12px;height:56px;border-radius:999px;background:${COLORS[tile.group] || COLORS.special};"></div>
          <div>
            <div style="font-size:20px;font-weight:700;">${tile.name.replace('<br>', ' ')}</div>
            <div class="small muted">${getTileTypeLabel(tile)}</div>
          </div>
        </div>
      `;

      const ownerId = state.owned[tile.id];
      const owner = ownerId ? state.players.find(p => p.id === ownerId) : null;

      if (owner) {
        // SAHİPLİK KUTUSUNDA EMOJİ GÖSTERİMİ EKLENDİ
        html += `<div class="info-box" style="margin-top:12px; display:flex; align-items:center; gap:8px;"><span class="token" style="background:${owner.color}">${owner.emoji}</span> Sahip: <strong>${owner.name}</strong></div>`;
      }

      if (tile.price != null) {
        html += `
          <div class="info-grid" style="margin-top:12px;">
            <div class="info-box">Fiyat: <strong>${formatMoney(tile.price)}</strong></div>
            <div class="info-box">İpotek: <strong>${formatMoney(tile.mortgage || 0)}</strong></div>
          </div>
        `;
      }

      if (tile.type === 'property') {
        html += `
          <div class="rent-box" style="margin-top:12px;">
            <div style="font-weight:700;margin-bottom:8px;">Kira tablosu</div>
            <div class="rent-grid">
              <div>Boş arsa: ${formatMoney(tile.rent[0])}</div>
              <div>1 ev: ${formatMoney(tile.rent[1])}</div>
              <div>2 ev: ${formatMoney(tile.rent[2])}</div>
              <div>3 ev: ${formatMoney(tile.rent[3])}</div>
              <div>4 ev: ${formatMoney(tile.rent[4])}</div>
              <div>Otel: ${formatMoney(tile.rent[5])}</div>
            </div>
          </div>
        `;
      }

      if (tile.type === 'station') {
        html += `<div class="rent-box" style="margin-top:12px;">${tile.rentByCount.map((r, i) => `<div>${i + 1} ulaşım: ${formatMoney(r)}</div>`).join('')}</div>`;
      }

      if (tile.type === 'utility') {
        html += `<div class="rent-box" style="margin-top:12px;">1 servis varsa zarın 4 katı, 2 servis varsa zarın 10 katı kira.</div>`;
      }

      html += `<div class="small muted" style="margin-top:12px;">Ev: ${current.houses}${current.hotel ? ' • Otel var' : ''}${current.mortgaged ? ' • İpotekli' : ''}</div>`;
      el.selectedTile.innerHTML = html;
    }

    // GÜNCELLENEN OYUNCU LİSTESİ RENDER KISMI (EMOJİLİ)
    function renderPlayers() {
      el.playersList.innerHTML = state.players.map((p, idx) => `
        <div class="player-card ${idx === state.currentPlayer ? 'active' : ''}">
          <div class="player-head">
            <div>
              <div class="player-name-row">
                <span class="token" style="background:${p.color}">${p.emoji}</span>
                <strong>${p.name}</strong>
              </div>
              <div class="small muted" style="margin-top:4px;">Konum: ${board[p.position].name.replace('<br>', ' ')}</div>
            </div>
            <div class="pill"><strong>${formatMoney(p.money)}</strong></div>
          </div>
          <div style="margin-top:8px;">
            ${p.inJail ? '<span class="pill">Silivri</span>' : ''}
            ${p.getOutOfJailCards > 0 ? `<span class="pill">Hapis kartı: ${p.getOutOfJailCards}</span>` : ''}
          </div>
          <div style="margin-top:8px;">
            ${p.properties.length ? p.properties.map(id => `<span class="pill">${board[id].name.replace('<br>', ' ')}</span>`).join('') : '<span class="small muted">Henüz mülk yok</span>'}
          </div>
        </div>
      `).join('');
    }

    function renderLog() {
      el.logList.innerHTML = state.log.map(item => `<div class="log-item">${item}</div>`).join('');
    }

    function renderStatus() {
      const p = activePlayer();
      el.activePlayerName.textContent = p.name;
      el.message.textContent = state.message;
      const diceChars = ['⚀', '⚁', '⚂', '⚃', '⚄', '⚅'];
      el.diceBox.textContent = `${diceChars[state.dice[0] - 1]} ${diceChars[state.dice[1] - 1]}`;
      el.turnSummary.textContent = state.turnSummary;

      const pending = state.pendingTileId != null ? board[state.pendingTileId] : null;
      const canBuy = !!pending && ['property', 'station', 'utility'].includes(pending.type) && !state.owned[pending.id];
      
      el.buyBtn.disabled = !canBuy || state.gameOver;
      el.rollBtn.disabled = state.gameOver || state.turnStarted;
      el.endTurnBtn.disabled = state.gameOver || !state.turnStarted;
    }

    // GÜNCELLENEN İSTATİSTİKLER RENDER KISMI (EMOJİLİ)
    function renderStats() {
      const worths = state.players.map(p => getNetWorth(p));
      const maxWorth = Math.max(...worths, 1);
      el.statsPanel.innerHTML = state.players.map((p, i) => {
        const s = state.stats[p.id] || {};
        const worth = worths[i];
        const pct = Math.round((worth / maxWorth) * 100);
        return `
          <div class="stat-card">
            <div class="stat-card-header">
              <span class="token" style="background:${p.color}">${p.emoji}</span>
              <strong>${p.name}</strong>
              <span class="pill" style="margin-left:auto;">M${worth} net</span>
            </div>
            <div class="stat-rows">
              <div class="stat-row"><div class="stat-label">Nakit</div><div class="stat-value">M${p.money}</div></div>
              <div class="stat-row"><div class="stat-label">Zar sayısı</div><div class="stat-value">${s.diceRolls || 0}</div></div>
              <div class="stat-row"><div class="stat-label">Ödenen kira</div><div class="stat-value">M${s.totalRentPaid || 0}</div></div>
              <div class="stat-row"><div class="stat-label">Alınan kira</div><div class="stat-value">M${s.totalRentReceived || 0}</div></div>
            </div>
            <div class="net-worth-bar">
              <div class="net-worth-fill" style="width:${pct}%;background:${p.color}"></div>
            </div>
          </div>
        `;
      }).join('');
    }

    function render() {
      renderBoard(); renderSelectedTile(); renderPlayers(); renderLog(); renderStatus(); renderStats();
    }

    /* Event Listeners */
    el.rollBtn.addEventListener('click', rollDice);
    el.buyBtn.addEventListener('click', buyTile);
    el.endTurnBtn.addEventListener('click', endTurn);
    el.buildBtn.addEventListener('click', buildOnSelectedTile);
    el.mortgageBtn.addEventListener('click', mortgageSelectedTile);
    el.newGameBtn.addEventListener('click', () => showSetupModal(Number(el.playerCount.value)));
    el.saveBtn.addEventListener('click', saveGame);
    el.loadBtn.addEventListener('click', loadGame);
    el.setupStartBtn.addEventListener('click', () => startGameWithNames(Number(el.playerCount.value)));
    el.setupBody.addEventListener('keydown', e => {
      if (e.key === 'Enter') startGameWithNames(Number(el.playerCount.value));
    });

    // İlk açılışta setup modal göster
    showSetupModal(2);
  </script>
</body>
</html>
