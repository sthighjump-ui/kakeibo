[gemini-code-1795012335706.html](https://github.com/user-attachments/files/35033650/gemini-code-1795012335706.html)
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <meta name="apple-mobile-web-app-capable" content="yes" />
  <meta name="apple-mobile-web-app-status-bar-style" content="default" />
  <meta name="apple-mobile-web-app-title" content="ふたりの家計簿" />
  <title>ふたりの家計簿｜しゅん ＆ ゆう</title>
  <!-- Supabase JS SDK -->
  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
  <style>
    :root {
      --bg: #f1f5f9;
      --card: #ffffff;
      --text: #0f172a;
      --sub: #64748b;
      --border: #e2e8f0;
      --shun: #2563eb;
      --shun-bg: #eff6ff;
      --yu: #e11d48;
      --yu-bg: #fff1f2;
      --ok: #10b981;
      --ok-bg: #ecfdf5;
      --warn: #f59e0b;
      --warn-bg: #fffbeb;
    }
    * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
    body {
      font-family: -apple-system, BlinkMacSystemFont, "Hiragino Sans", "Hiragino Kaku Gothic ProN", "Yu Gothic", sans-serif;
      background: var(--bg);
      color: var(--text);
      line-height: 1.5;
      padding-bottom: 88px;
    }
    .app-wrap {
      max-width: 480px;
      margin: 0 auto;
      min-height: 100vh;
      background: #f8fafc;
      box-shadow: 0 0 24px rgba(15, 23, 42, 0.06);
      position: relative;
    }
    header {
      position: sticky;
      top: 0;
      z-index: 30;
      background: rgba(255, 255, 255, 0.95);
      backdrop-filter: blur(8px);
      border-bottom: 1px solid var(--border);
      padding: 10px 14px;
    }
    .header-top {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 6px;
    }
    .brand { font-size: 16px; font-weight: 800; display: flex; align-items: center; gap: 6px; }
    .header-actions { display: flex; align-items: center; gap: 5px; }
    .icon-btn {
      border: 1px solid var(--border);
      background: #fff;
      color: var(--text);
      font-size: 11px;
      font-weight: 700;
      padding: 5px 8px;
      border-radius: 999px;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 3px;
    }
    .icon-btn.active-sync { border-color: #a7f3d0; background: var(--ok-bg); color: #065f46; }
    .icon-btn.locked { border-color: #fecdd3; background: var(--yu-bg); color: var(--yu); }
    .user-toggle {
      display: flex;
      background: #e2e8f0;
      padding: 2px;
      border-radius: 999px;
    }
    .user-toggle button {
      border: none;
      background: transparent;
      padding: 4px 10px;
      border-radius: 999px;
      font-size: 11px;
      font-weight: 800;
      color: var(--sub);
      cursor: pointer;
    }
    .user-toggle button.active-shun { background: var(--shun); color: #fff; }
    .user-toggle button.active-yu { background: var(--yu); color: #fff; }

    .month-bar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: #fff;
      padding: 8px 14px;
      border-bottom: 1px solid var(--border);
    }
    .month-nav-btn {
      border: 1px solid var(--border);
      background: #f8fafc;
      width: 32px;
      height: 32px;
      border-radius: 8px;
      font-weight: 800;
      cursor: pointer;
      color: var(--text);
    }
    .month-label {
      font-size: 15px;
      font-weight: 800;
      display: flex;
      align-items: center;
      gap: 8px;
    }
    .settled-pill {
      font-size: 10px;
      font-weight: 800;
      background: var(--ok-bg);
      color: #065f46;
      border: 1px solid #a7f3d0;
      padding: 2px 8px;
      border-radius: 999px;
    }

    main { padding: 14px; }
    .view { display: none; }
    .view.active { display: block; }

    .card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 15px;
      margin-bottom: 12px;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.03);
    }
    .card-title {
      font-size: 12px;
      font-weight: 800;
      color: var(--sub);
      margin-bottom: 10px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .total-hero { text-align: center; padding: 6px 0 12px; }
    .total-hero .lbl { font-size: 12px; font-weight: 700; color: var(--sub); }
    .total-hero .val { font-size: 32px; font-weight: 900; letter-spacing: -0.02em; margin-top: 2px; }
    .ratio-bar {
      height: 12px;
      background: #e2e8f0;
      border-radius: 999px;
      overflow: hidden;
      display: flex;
      margin: 10px 0;
    }
    .ratio-shun { background: var(--shun); height: 100%; transition: width 0.3s; }
    .ratio-yu { background: var(--yu); height: 100%; transition: width 0.3s; }
    .split-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
    .split-box { padding: 10px; border-radius: 12px; }
    .split-box.shun { background: var(--shun-bg); border: 1px solid #bfdbfe; }
    .split-box.yu { background: var(--yu-bg); border: 1px solid #fecdd3; }
    .split-box .name { font-size: 11px; font-weight: 800; }
    .split-box.shun .name { color: var(--shun); }
    .split-box.yu .name { color: var(--yu); }
    .split-box .amt { font-size: 18px; font-weight: 900; margin-top: 2px; }

    .utility-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; }
    .util-chip {
      border-radius: 12px;
      padding: 10px 6px;
      text-align: center;
      cursor: pointer;
      border: 1.5px solid var(--border);
      transition: transform 0.1s;
    }
    .util-chip:active { transform: scale(0.97); }
    .util-chip.missing { background: var(--warn-bg); border-color: #fcd34d; color: #92400e; }
    .util-chip.done { background: var(--ok-bg); border-color: #a7f3d0; color: #065f46; }
    .util-chip .u-name { font-size: 12px; font-weight: 800; }
    .util-chip .u-stat { font-size: 11px; font-weight: 700; margin-top: 2px; }

    .rent-row { display: flex; justify-content: space-between; align-items: center; }
    .rent-amt { font-size: 20px; font-weight: 900; color: var(--yu); }
    .mini-btn {
      border: 1px solid var(--border);
      background: #f8fafc;
      padding: 6px 10px;
      border-radius: 8px;
      font-size: 11px;
      font-weight: 800;
      cursor: pointer;
    }

    .tx-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 11px 0;
      border-bottom: 1px solid var(--border);
      gap: 8px;
    }
    .tx-item:last-child { border-bottom: none; padding-bottom: 0; }
    .tx-left { display: flex; align-items: center; gap: 10px; min-width: 0; }
    .tx-ico {
      width: 38px;
      height: 38px;
      border-radius: 12px;
      background: #f1f5f9;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 18px;
      flex-shrink: 0;
    }
    .tx-info { min-width: 0; }
    .tx-title { font-size: 14px; font-weight: 800; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
    .tx-meta { font-size: 11px; color: var(--sub); display: flex; flex-wrap: wrap; align-items: center; gap: 5px; margin-top: 2px; }
    .badge { font-size: 10px; font-weight: 800; padding: 1px 6px; border-radius: 6px; }
    .badge-shun { background: var(--shun-bg); color: var(--shun); }
    .badge-yu { background: var(--yu-bg); color: var(--yu); }
    .badge-mf { background: #e0e7ff; color: #3730a3; }
    .tx-right { text-align: right; flex-shrink: 0; }
    .tx-amt { font-size: 15px; font-weight: 900; }
    .tx-actions { display: flex; justify-content: flex-end; gap: 4px; margin-top: 4px; }
    .act-btn {
      border: 1px solid var(--border);
      background: #fff;
      font-size: 11px;
      padding: 2px 7px;
      border-radius: 999px;
      cursor: pointer;
      font-weight: 700;
      color: var(--sub);
    }
    .act-btn.thanked { background: #ffe4e6; border-color: #fecdd3; color: var(--yu); }

    .amount-display {
      background: #0f172a;
      color: #fff;
      border-radius: 16px;
      padding: 16px;
      text-align: right;
      margin-bottom: 12px;
    }
    .amount-display .sub { font-size: 11px; color: #94a3b8; }
    .amount-display .num { font-size: 34px; font-weight: 900; letter-spacing: 0.02em; }
    .cat-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 6px; margin-bottom: 12px; }
    .cat-btn {
      border: 1.5px solid var(--border);
      background: #fff;
      border-radius: 12px;
      padding: 8px 4px;
      text-align: center;
      cursor: pointer;
      font-size: 11px;
      font-weight: 800;
    }
    .cat-btn.selected { border-color: var(--shun); background: var(--shun-bg); color: var(--shun); }
    .cat-btn .c-ico { font-size: 18px; display: block; margin-bottom: 2px; }
    .memo-input {
      width: 100%;
      padding: 10px 12px;
      border: 1.5px solid var(--border);
      border-radius: 12px;
      font-size: 14px;
      margin-bottom: 12px;
    }
    .numpad { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; margin-bottom: 12px; }
    .num-btn {
      background: #fff;
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 14px 0;
      font-size: 20px;
      font-weight: 800;
      cursor: pointer;
    }
    .num-btn:active { background: #e2e8f0; }
    .primary-btn {
      width: 100%;
      border: none;
      border-radius: 14px;
      padding: 15px;
      font-size: 15px;
      font-weight: 900;
      color: #fff;
      background: var(--shun);
      cursor: pointer;
    }
    .primary-btn.yu-mode { background: var(--yu); }
    .primary-btn.ok-mode { background: var(--ok); }

    .drop-zone {
      border: 2px dashed #93c5fd;
      background: var(--shun-bg);
      border-radius: 14px;
      padding: 20px;
      text-align: center;
      margin-bottom: 10px;
    }
    .secondary-btn {
      width: 100%;
      border: 1.5px solid var(--shun);
      background: #fff;
      color: var(--shun);
      border-radius: 12px;
      padding: 12px;
      font-size: 13px;
      font-weight: 800;
      cursor: pointer;
      margin-top: 8px;
    }

    .settle-box {
      background: linear-gradient(135deg, #0f172a, #1e293b);
      color: #fff;
      border-radius: 16px;
      padding: 18px;
      text-align: center;
      margin-bottom: 12px;
    }
    .settle-arrow { font-size: 14px; font-weight: 800; color: #93c5fd; margin-bottom: 6px; }
    .settle-val { font-size: 32px; font-weight: 900; }
    .slider-wrap { margin: 12px 0; }
    input[type="range"] { width: 100%; }

    nav.bottom-nav {
      position: fixed;
      bottom: 0;
      left: 50%;
      transform: translateX(-50%);
      width: 100%;
      max-width: 480px;
      background: #fff;
      border-top: 1px solid var(--border);
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      padding: 6px 4px 10px;
      z-index: 40;
    }
    .nav-btn {
      border: none;
      background: transparent;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 2px;
      font-size: 10px;
      font-weight: 800;
      color: var(--sub);
      cursor: pointer;
    }
    .nav-btn .n-ico { font-size: 20px; }
    .nav-btn.active { color: var(--shun); }

    .modal-backdrop {
      position: fixed;
      inset: 0;
      background: rgba(15, 23, 42, 0.6);
      display: none;
      align-items: center;
      justify-content: center;
      padding: 16px;
      z-index: 100;
    }
    .modal-backdrop.open { display: flex; }
    .modal-card {
      background: #fff;
      width: 100%;
      max-width: 420px;
      border-radius: 18px;
      padding: 18px;
      max-height: 90vh;
      overflow-y: auto;
    }
    .modal-title { font-size: 16px; font-weight: 900; margin-bottom: 12px; }
    .field-lbl { font-size: 11px; font-weight: 800; color: var(--sub); display: block; margin: 8px 0 4px; }
    .field-inp {
      width: 100%;
      padding: 10px;
      border: 1.5px solid var(--border);
      border-radius: 10px;
      font-size: 14px;
    }
    .modal-btns { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-top: 14px; }
    .danger-btn {
      background: #fee2e2;
      color: #b91c1c;
      border: none;
      border-radius: 10px;
      padding: 12px;
      font-weight: 800;
      cursor: pointer;
    }
    .cancel-btn {
      background: #f1f5f9;
      color: var(--text);
      border: none;
      border-radius: 10px;
      padding: 12px;
      font-weight: 800;
      cursor: pointer;
    }
    .lock-screen {
      position: fixed;
      inset: 0;
      background: #0f172a;
      color: #fff;
      z-index: 200;
      display: none;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 24px;
      text-align: center;
    }
    .lock-screen.active { display: flex; }
    .toast {
      position: fixed;
      top: 64px;
      left: 50%;
      transform: translateX(-50%);
      background: #0f172a;
      color: #fff;
      padding: 8px 16px;
      border-radius: 999px;
      font-size: 12px;
      font-weight: 800;
      z-index: 150;
      opacity: 0;
      pointer-events: none;
      transition: opacity 0.2s;
    }
    .toast.show { opacity: 1; }
  </style>
</head>
<body>

  <div id="lockScreen" class="lock-screen">
    <div style="font-size: 42px; margin-bottom: 10px;">🔒</div>
    <h2 style="font-size: 20px; font-weight: 900; margin-bottom: 6px;">ふたりの家計簿</h2>
    <p style="font-size: 12px; color: #94a3b8; margin-bottom: 18px;">2人だけの合言葉を入力してロック解除</p>
    <input type="password" id="unlockInput" placeholder="合言葉を入力" style="width: 240px; padding: 12px; border-radius: 12px; border: none; font-size: 16px; text-align: center; margin-bottom: 10px;" />
    <button id="unlockBtn" style="width: 240px; padding: 12px; border-radius: 12px; border: none; background: #2563eb; color: #fff; font-weight: 900; font-size: 14px; cursor: pointer;">ロック解除</button>
    <p id="unlockErr" style="color: #fda4af; font-size: 12px; margin-top: 8px;"></p>
  </div>

  <div id="toast" class="toast"></div>

  <div class="app-wrap">
    <header>
      <div class="header-top">
        <div class="brand">💑 ふたりの家計簿</div>
        <div class="header-actions">
          <button id="syncModalBtn" class="icon-btn">☁️ 同期</button>
          <button id="passModalBtn" class="icon-btn">🔓 合言葉</button>
          <div class="user-toggle">
            <button id="roleShunBtn" class="active-shun">しゅん</button>
            <button id="roleYuBtn">ゆう</button>
          </div>
        </div>
      </div>
    </header>

    <div class="month-bar">
      <button id="prevMonthBtn" class="month-nav-btn">◀</button>
      <div class="month-label">
        <span id="currentMonthLabel">2026年11月</span>
        <span id="settledBadge" class="settled-pill" style="display:none;">✅ 精算済み</span>
      </div>
      <button id="nextMonthBtn" class="month-nav-btn">▶</button>
    </div>

    <main>
      <section id="view-home" class="view active">
        <div class="card">
          <div class="total-hero">
            <div class="lbl">今月2人で使った共通支出合計</div>
            <div class="val" id="totalAmountText">¥0</div>
          </div>
          <div class="ratio-bar">
            <div id="barShun" class="ratio-shun" style="width:50%;"></div>
            <div id="barYu" class="ratio-yu" style="width:50%;"></div>
          </div>
          <div class="split-grid">
            <div class="split-box shun">
              <div class="name">👨 しゅん支払い (<span id="pctShun">50</span>%)</div>
              <div class="amt" id="amtShunText">¥0</div>
            </div>
            <div class="split-box yu">
              <div class="name">👩 ゆう支払い (<span id="pctYu">50</span>%)</div>
              <div class="amt" id="amtYuText">¥0</div>
            </div>
          </div>
        </div>

        <div class="card">
          <div class="card-title">
            <span>⚡ 今月の光熱費チェック（タップで即入力）</span>
          </div>
          <div class="utility-grid" id="utilityChecklist"></div>
        </div>

        <div class="card">
          <div class="card-title">
            <span>🏠 毎月の固定費（ゆうちゃん支払い）</span>
            <button id="editRentBtn" class="mini-btn">金額変更</button>
          </div>
          <div class="rent-row">
            <div>
              <div style="font-size:13px; font-weight:800;">家賃（自動計上）</div>
              <div style="font-size:11px; color:var(--sub);">毎月ゆうの支払いに自動加算</div>
            </div>
            <div style="display:flex; align-items:center; gap:8px;">
              <span class="rent-amt" id="rentAmountText">¥75,000</span>
              <button id="thankRentBtn" class="act-btn">💖 ありがとう</button>
            </div>
          </div>
        </div>

        <div class="card">
          <div class="card-title">
            <span>📝 共通支出の履歴（タップで編集）</span>
            <span id="txCountText">0件</span>
          </div>
          <div id="txListContainer"></div>
        </div>
      </section>

      <section id="view-input" class="view">
        <div class="card">
          <div class="card-title">
            <span id="inputWhoLabel">👨 しゅん の支払いとして登録</span>
          </div>
          <div class="amount-display">
            <div class="sub" id="selectedCatLabel">カテゴリ：🛒 スーパー</div>
            <div class="num" id="keypadDisplay">¥0</div>
          </div>
          <div class="cat-grid" id="catSelector"></div>
          <input type="text" id="memoInput" class="memo-input" placeholder="メモ（例：ツルヤ、電気代11月分など）" />
          <div class="numpad" id="numpadGrid"></div>
          <button id="submitExpenseBtn" class="primary-btn">この金額を登録する</button>
        </div>
      </section>

      <section id="view-mf" class="view">
        <div class="card">
          <div class="card-title"><span>📥 マネーフォワード明細CSV取込</span></div>
          <p style="font-size:12px; color:var(--sub); margin-bottom:12px;">
            マネーフォワード MEからダウンロードしたCSVを読み込み、食費・日用品・光熱費など<strong>2人の共通費だけ</strong>を自動抽出します（重複データ・個人支出は自動スキップ）。
          </p>
          <div class="drop-zone">
            <div style="font-size:28px; margin-bottom:6px;">📄</div>
            <div style="font-size:13px; font-weight:800; margin-bottom:8px;">MFのCSVファイルを選択</div>
            <input type="file" id="mfFileInput" accept=".csv" style="font-size:12px; max-width:100%;" />
          </div>
          <button id="mfSampleBtn" class="secondary-btn">✨ サンプルMFデータで取り込みテスト</button>
        </div>
      </section>

      <section id="view-settle" class="view">
        <div class="settle-box">
          <div class="settle-arrow" id="settleDirectionText">差額なし（精算不要）</div>
          <div class="settle-val" id="settleAmountText">¥0</div>
          <div style="font-size:11px; color:#94a3b8; margin-top:6px;" id="settleBreakdownText">家賃を含めた合計から自動計算</div>
        </div>

        <div class="card">
          <div class="card-title">
            <span>⚖️ 2人の負担割合</span>
            <span id="ratioLabelText">しゅん 50% ： ゆう 50%</span>
          </div>
          <div class="slider-wrap">
            <input type="range" id="ratioSlider" min="0" max="100" step="5" value="50" />
          </div>
          <div style="display:flex; gap:6px; justify-content:center; margin-bottom:14px;">
            <button class="mini-btn ratio-preset" data-val="50">50 : 50</button>
            <button class="mini-btn ratio-preset" data-val="60">60 : 40</button>
            <button class="mini-btn ratio-preset" data-val="70">70 : 30</button>
          </div>
          <button id="toggleSettleBtn" class="primary-btn ok-mode">✅ この月の精算を完了にする</button>
        </div>
      </section>
    </main>

    <nav class="bottom-nav">
      <button class="nav-btn active" data-tab="home"><span class="n-ico">📊</span>ホーム</button>
      <button class="nav-btn" data-tab="input"><span class="n-ico">⚡</span>3秒入力</button>
      <button class="nav-btn" data-tab="mf"><span class="n-ico">📥</span>MF取込</button>
      <button class="nav-btn" data-tab="settle"><span class="n-ico">🤝</span>精算</button>
    </nav>
  </div>

  <div id="editModal" class="modal-backdrop">
    <div class="modal-card">
      <div class="modal-title">✏️ 履歴の編集・削除</div>
      <input type="hidden" id="editTxId" />
      <label class="field-lbl">支払った人</label>
      <select id="editTxPayer" class="field-inp">
        <option value="shun">👨 しゅん</option>
        <option value="yu">👩 ゆう</option>
      </select>
      <label class="field-lbl">カテゴリ</label>
      <select id="editTxCategory" class="field-inp"></select>
      <label class="field-lbl">金額（円）</label>
      <input type="number" id="editTxAmount" class="field-inp" />
      <label class="field-lbl">メモ・店名</label>
      <input type="text" id="editTxTitle" class="field-inp" />
      <div class="modal-btns">
        <button id="deleteTxBtn" class="danger-btn">🗑️ 削除する</button>
        <button id="saveEditTxBtn" class="primary-btn" style="padding:12px;">保存する</button>
      </div>
      <button id="closeEditModalBtn" class="cancel-btn" style="width:100%; margin-top:8px;">キャンセル</button>
    </div>
  </div>

  <div id="rentModal" class="modal-backdrop">
    <div class="modal-card">
      <div class="modal-title">🏠 今月の家賃（ゆう支払い）を変更</div>
      <label class="field-lbl">家賃金額（円）</label>
      <input type="number" id="rentAmountInput" class="field-inp" />
      <div class="modal-btns">
        <button id="closeRentModalBtn" class="cancel-btn">キャンセル</button>
        <button id="saveRentBtn" class="primary-btn yu-mode" style="padding:12px;">更新する</button>
      </div>
    </div>
  </div>

  <div id="passModal" class="modal-backdrop">
    <div class="modal-card">
      <div class="modal-title">🔒 2人だけの合言葉ロック設定</div>
      <p style="font-size:12px; color:var(--sub); margin-bottom:8px;">
        合言葉を設定すると、次回アプリを開いた時にロック画面が表示されます（空欄で保存するとロック解除）。
      </p>
      <label class="field-lbl">合言葉（4桁の数字や記念日など）</label>
      <input type="text" id="passcodeSettingInput" class="field-inp" placeholder="例：0615" />
      <div class="modal-btns">
        <button id="lockNowBtn" class="cancel-btn">今すぐロック</button>
        <button id="savePasscodeBtn" class="primary-btn" style="padding:12px;">設定を保存</button>
      </div>
      <button id="closePassModalBtn" class="cancel-btn" style="width:100%; margin-top:8px;">閉じる</button>
    </div>
  </div>

  <div id="syncModal" class="modal-backdrop">
    <div class="modal-card">
      <div class="modal-title">☁️ Supabase クラウドリアルタイム同期</div>
      <p style="font-size:12px; color:var(--sub); margin-bottom:8px;">
        SupabaseのProject URLとAnon Keyを設定すると、2人のスマホでデータがリアルタイム同期されます。
      </p>
      <label class="field-lbl">Supabase Project URL</label>
      <input type="text" id="sbUrlInput" class="field-inp" />
      <label class="field-lbl">Supabase Anon Public Key</label>
      <input type="text" id="sbKeyInput" class="field-inp" />
      <div class="modal-btns">
        <button id="closeSyncModalBtn" class="cancel-btn">閉じる</button>
        <button id="saveSyncBtn" class="primary-btn ok-mode" style="padding:12px;">接続して同期</button>
      </div>
    </div>
  </div>

  <script>
    const DEFAULT_SUPABASE_URL = "https://plzmegdaifmlnhiscnle.supabase.co";
    const DEFAULT_SUPABASE_KEY = "sb_publishable_vU6wDJxHbk3jE0uD24GxUA_qgaX5IfL";

    const CATEGORIES = [
      { id: "スーパー", icon: "🛒" },
      { id: "外食", icon: "🍽️" },
      { id: "日用品", icon: "🧻" },
      { id: "電気", icon: "⚡" },
      { id: "ガス", icon: "🔥" },
      { id: "水道", icon: "💧" },
      { id: "通信・ネット", icon: "📶" },
      { id: "その他", icon: "📦" }
    ];

    let state = {
      role: "shun",
      currentMonth: "2026-11",
      shunRatio: 50,
      keypadStr: "",
      selectedCat: "スーパー",
      expenses: [],
      monthMeta: {}
    };

    let sbClient = null;

    function showToast(msg) {
      const el = document.getElementById("toast");
      el.textContent = msg;
      el.classList.add("show");
      setTimeout(() => el.classList.remove("show"), 2200);
    }

    function getCatIcon(catName) {
      const found = CATEGORIES.find(c => c.id === catName);
      return found ? found.icon : "📦";
    }

    function ensureMonthMeta(m) {
      if (!state.monthMeta[m]) {
        state.monthMeta[m] = { month: m, rent_amount: 75000, rent_thanked: false, settled: false };
      }
      return state.monthMeta[m];
    }

    function saveLocal() {
      localStorage.setItem("futari_kakeibo_v3", JSON.stringify({
        expenses: state.expenses,
        monthMeta: state.monthMeta,
        shunRatio: state.shunRatio
      }));
    }

    function loadLocal() {
      const raw = localStorage.getItem("futari_kakeibo_v3");
      if (raw) {
        try {
          const parsed = JSON.parse(raw);
          state.expenses = parsed.expenses || [];
          state.monthMeta = parsed.monthMeta || {};
          state.shunRatio = parsed.shunRatio ?? 50;
        } catch (e) {}
      }
      if (state.expenses.length === 0) {
        state.expenses = [
          { id: "init-1", month: "2026-11", date: "2026-11-05", category: "スーパー", title: "ツルヤ 週末まとめ買い", amount: 6480, payer: "shun", source: "mf", thanked: true },
          { id: "init-2", month: "2026-11", date: "2026-11-10", category: "外食", title: "イタリアンディナー", amount: 5200, payer: "yu", source: "manual", thanked: false },
          { id: "init-3", month: "2026-11", date: "2026-11-14", category: "日用品", title: "ドラッグストア 洗剤ほか", amount: 2350, payer: "shun", source: "manual", thanked: false }
        ];
        ensureMonthMeta("2026-11");
        saveLocal();
      }
    }

    async function initSupabase() {
      const url = localStorage.getItem("sb_url") || DEFAULT_SUPABASE_URL;
      const key = localStorage.getItem("sb_key") || DEFAULT_SUPABASE_KEY;
      document.getElementById("sbUrlInput").value = url;
      document.getElementById("sbKeyInput").value = key;

      if (!url || !key || !window.supabase) return;

      try {
        sbClient = window.supabase.createClient(url, key);
        document.getElementById("syncModalBtn").classList.add("active-sync");
        document.getElementById("syncModalBtn").textContent = "☁️ 同期中";

        const [expRes, metaRes] = await Promise.all([
          sbClient.from("kakeibo_expenses").select("*"),
          sbClient.from("kakeibo_months").select("*")
        ]);

        if (!expRes.error && expRes.data && expRes.data.length > 0) {
          state.expenses = expRes.data;
        } else if (!expRes.error && expRes.data && expRes.data.length === 0 && state.expenses.length > 0) {
          await sbClient.from("kakeibo_expenses").upsert(state.expenses);
        }
        if (!metaRes.error && metaRes.data) {
          metaRes.data.forEach(row => { state.monthMeta[row.month] = row; });
        }
        saveLocal();
        renderAll();

        sbClient.channel("kakeibo-live")
          .on("postgres_changes", { event: "*", schema: "public", table: "kakeibo_expenses" }, async () => {
            const { data } = await sbClient.from("kakeibo_expenses").select("*");
            if (data) { state.expenses = data; saveLocal(); renderAll(); }
          })
          .on("postgres_changes", { event: "*", schema: "public", table: "kakeibo_months" }, async () => {
            const { data } = await sbClient.from("kakeibo_months").select("*");
            if (data) {
              data.forEach(r => { state.monthMeta[r.month] = r; });
              saveLocal();
              renderAll();
            }
          })
          .subscribe();
      } catch (err) {
        console.error(err);
      }
    }

    async function syncExpenseUpsert(item) {
      saveLocal();
      if (sbClient) {
        await sbClient.from("kakeibo_expenses").upsert(item);
      }
    }

    async function syncExpenseDelete(id) {
      saveLocal();
      if (sbClient) {
        await sbClient.from("kakeibo_expenses").delete().eq("id", id);
      }
    }

    async function syncMonthMetaUpsert(meta) {
      saveLocal();
      if (sbClient) {
        await sbClient.from("kakeibo_months").upsert(meta);
      }
    }

    function renderAll() {
      const m = state.currentMonth;
      const meta = ensureMonthMeta(m);
      const [yy, mm] = m.split("-");
      document.getElementById("currentMonthLabel").textContent = `${yy}年${parseInt(mm, 10)}月`;
      document.getElementById("settledBadge").style.display = meta.settled ? "inline-block" : "none";

      document.getElementById("roleShunBtn").className = state.role === "shun" ? "active-shun" : "";
      document.getElementById("roleYuBtn").className = state.role === "yu" ? "active-yu" : "";
      document.getElementById("inputWhoLabel").textContent = state.role === "shun" ? "👨 しゅん の支払いとして登録" : "👩 ゆう の支払いとして登録";
      const subBtn = document.getElementById("submitExpenseBtn");
      subBtn.className = state.role === "yu" ? "primary-btn yu-mode" : "primary-btn";

      const monthTx = state.expenses.filter(e => e.month === m);
      let shunTotal = 0;
      let yuTotal = Number(meta.rent_amount) || 0;

      monthTx.forEach(tx => {
        if (tx.payer === "shun") shunTotal += Number(tx.amount);
        else yuTotal += Number(tx.amount);
      });

      const grandTotal = shunTotal + yuTotal;
      const shunPct = grandTotal > 0 ? Math.round((shunTotal / grandTotal) * 100) : 50;
      const yuPct = 100 - shunPct;

      document.getElementById("totalAmountText").textContent = `¥${grandTotal.toLocaleString()}`;
      document.getElementById("amtShunText").textContent = `¥${shunTotal.toLocaleString()}`;
      document.getElementById("amtYuText").textContent = `¥${yuTotal.toLocaleString()}`;
      document.getElementById("pctShun").textContent = shunPct;
      document.getElementById("pctYu").textContent = yuPct;
      document.getElementById("barShun").style.width = `${shunPct}%`;
      document.getElementById("barYu").style.width = `${yuPct}%`;

      document.getElementById("rentAmountText").textContent = `¥${Number(meta.rent_amount).toLocaleString()}`;
      const rentThankBtn = document.getElementById("thankRentBtn");
      rentThankBtn.className = meta.rent_thanked ? "act-btn thanked" : "act-btn";
      rentThankBtn.textContent = meta.rent_thanked ? "💖 ありがとう済" : "💖 ありがとう";

      const utils = ["電気", "ガス", "水道"];
      const utilBox = document.getElementById("utilityChecklist");
      utilBox.innerHTML = "";
      utils.forEach(u => {
        const found = monthTx.find(t => t.category === u);
        const div = document.createElement("div");
        div.className = found ? "util-chip done" : "util-chip missing";
        div.innerHTML = `
          <div class="u-name">${getCatIcon(u)} ${u}代</div>
          <div class="u-stat">${found ? `¥${Number(found.amount).toLocaleString()}` : "⚠️ 未入力"}</div>
        `;
        div.onclick = () => {
          state.selectedCat = u;
          switchTab("input");
          renderInputSelection();
          showToast(`${u}代の入力画面を開きました`);
        };
        utilBox.appendChild(div);
      });

      const listEl = document.getElementById("txListContainer");
      document.getElementById("txCountText").textContent = `${monthTx.length}件`;
      listEl.innerHTML = "";
      if (monthTx.length === 0) {
        listEl.innerHTML = `<p style="font-size:12px; color:var(--sub); text-align:center; padding:16px 0;">この月の支出履歴はまだありません</p>`;
      } else {
        [...monthTx].reverse().forEach(tx => {
          const row = document.createElement("div");
          row.className = "tx-item";
          const payerBadge = tx.payer === "shun"
            ? `<span class="badge badge-shun">しゅん</span>`
            : `<span class="badge badge-yu">ゆう</span>`;
          const mfBadge = tx.source === "mf" ? `<span class="badge badge-mf">MF取込</span>` : "";
          row.innerHTML = `
            <div class="tx-left">
              <div class="tx-ico">${getCatIcon(tx.category)}</div>
              <div class="tx-info">
                <div class="tx-title">${tx.title || tx.category}</div>
                <div class="tx-meta">${tx.date.slice(5)} ${payerBadge} ${mfBadge}</div>
              </div>
            </div>
            <div class="tx-right">
              <div class="tx-amt">¥${Number(tx.amount).toLocaleString()}</div>
              <div class="tx-actions">
                <button class="act-btn ${tx.thanked ? "thanked" : ""}" data-thank="${tx.id}">💖</button>
                <button class="act-btn" data-edit="${tx.id}">✏️ 編集</button>
              </div>
            </div>
          `;
          listEl.appendChild(row);
        });
      }

      const targetShun = Math.round(grandTotal * (state.shunRatio / 100));
      const diff = shunTotal - targetShun;
      document.getElementById("ratioLabelText").textContent = `しゅん ${state.shunRatio}% ： ゆう ${100 - state.shunRatio}%`;
      document.getElementById("ratioSlider").value = state.shunRatio;

      if (diff > 0) {
        document.getElementById("settleDirectionText").textContent = "👩 ゆう → 👨 しゅん へ支払い";
        document.getElementById("settleAmountText").textContent = `¥${diff.toLocaleString()}`;
      } else if (diff < 0) {
        document.getElementById("settleDirectionText").textContent = "👨 しゅん → 👩 ゆう へ支払い";
        document.getElementById("settleAmountText").textContent = `¥${Math.abs(diff).toLocaleString()}`;
      } else {
        document.getElementById("settleDirectionText").textContent = "差額なし（ぴったり精算済み）";
        document.getElementById("settleAmountText").textContent = "¥0";
      }

      const settleBtn = document.getElementById("toggleSettleBtn");
      settleBtn.textContent = meta.settled ? "↩️ 精算済み状態を解除する" : "✅ この月の精算を完了にする";
      settleBtn.className = meta.settled ? "primary-btn" : "primary-btn ok-mode";
    }

    function switchTab(tabName) {
      document.querySelectorAll(".view").forEach(v => v.classList.remove("active"));
      document.getElementById(`view-${tabName}`).classList.add("active");
      document.querySelectorAll(".nav-btn").forEach(b => {
        b.classList.toggle("active", b.dataset.tab === tabName);
      });
    }

    function renderInputSelection() {
      document.getElementById("selectedCatLabel").textContent = `カテゴリ：${getCatIcon(state.selectedCat)} ${state.selectedCat}`;
      document.querySelectorAll(".cat-btn").forEach(btn => {
        btn.classList.toggle("selected", btn.dataset.cat === state.selectedCat);
      });
      const val = parseInt(state.keypadStr || "0", 10);
      document.getElementById("keypadDisplay").textContent = `¥${val.toLocaleString()}`;
    }

    function setupUI() {
      const catGrid = document.getElementById("catSelector");
      const editCatSelect = document.getElementById("editTxCategory");
      CATEGORIES.forEach(c => {
        const b = document.createElement("button");
        b.className = "cat-btn" + (c.id === state.selectedCat ? " selected" : "");
        b.dataset.cat = c.id;
        b.innerHTML = `<span class="c-ico">${c.icon}</span>${c.id}`;
        b.onclick = () => { state.selectedCat = c.id; renderInputSelection(); };
        catGrid.appendChild(b);

        const opt = document.createElement("option");
        opt.value = c.id;
        opt.textContent = `${c.icon} ${c.id}`;
        editCatSelect.appendChild(opt);
      });

      const keys = ["7", "8", "9", "4", "5", "6", "1", "2", "3", "00", "0", "⌫"];
      const pad = document.getElementById("numpadGrid");
      keys.forEach(k => {
        const b = document.createElement("button");
        b.className = "num-btn";
        b.textContent = k;
        b.onclick = () => {
          if (k === "⌫") state.keypadStr = state.keypadStr.slice(0, -1);
          else if (state.keypadStr.length < 7) state.keypadStr += k;
          renderInputSelection();
        };
        pad.appendChild(b);
      });

      document.querySelectorAll(".nav-btn").forEach(btn => {
        btn.onclick = () => switchTab(btn.dataset.tab);
      });

      document.getElementById("roleShunBtn").onclick = () => { state.role = "shun"; renderAll(); };
      document.getElementById("roleYuBtn").onclick = () => { state.role = "yu"; renderAll(); };

      document.getElementById("prevMonthBtn").onclick = () => {
        const [y, m] = state.currentMonth.split("-").map(Number);
        const d = new Date(y, m - 2, 1);
        state.currentMonth = `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, "0")}`;
        renderAll();
      };
      document.getElementById("nextMonthBtn").onclick = () => {
        const [y, m] = state.currentMonth.split("-").map(Number);
        const d = new Date(y, m, 1);
        state.currentMonth = `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, "0")}`;
        renderAll();
      };

      document.getElementById("submitExpenseBtn").onclick = async () => {
        const amt = parseInt(state.keypadStr || "0", 10);
        if (amt <= 0) { showToast("金額を入力してね！"); return; }
        const memo = document.getElementById("memoInput").value.trim();
        const today = new Date();
        const dateStr = `${state.currentMonth}-${String(today.getDate()).padStart(2, "0")}`;
        const item = {
          id: "tx-" + Date.now(),
          month: state.currentMonth,
          date: dateStr,
          category: state.selectedCat,
          title: memo || state.selectedCat,
          amount: amt,
          payer: state.role,
          source: "manual",
          thanked: false
        };
        state.expenses.push(item);
        state.keypadStr = "";
        document.getElementById("memoInput").value = "";
        renderInputSelection();
        await syncExpenseUpsert(item);
        renderAll();
        switchTab("home");
        showToast("登録しました！");
      };

      document.getElementById("txListContainer").onclick = async (e) => {
        const thankId = e.target.dataset.thank;
        const editId = e.target.dataset.edit;
        if (thankId) {
          const tx = state.expenses.find(x => x.id === thankId);
          if (tx) {
            tx.thanked = !tx.thanked;
            await syncExpenseUpsert(tx);
            renderAll();
            if (tx.thanked) showToast("💖 ありがとうを送りました！");
          }
        }
        if (editId) {
          const tx = state.expenses.find(x => x.id === editId);
          if (tx) {
            document.getElementById("editTxId").value = tx.id;
            document.getElementById("editTxPayer").value = tx.payer;
            document.getElementById("editTxCategory").value = tx.category;
            document.getElementById("editTxAmount").value = tx.amount;
            document.getElementById("editTxTitle").value = tx.title;
            document.getElementById("editModal").classList.add("open");
          }
        }
      };

      document.getElementById("saveEditTxBtn").onclick = async () => {
        const id = document.getElementById("editTxId").value;
        const tx = state.expenses.find(x => x.id === id);
        if (tx) {
          tx.payer = document.getElementById("editTxPayer").value;
          tx.category = document.getElementById("editTxCategory").value;
          tx.amount = parseInt(document.getElementById("editTxAmount").value || "0", 10);
          tx.title = document.getElementById("editTxTitle").value.trim() || tx.category;
          await syncExpenseUpsert(tx);
          renderAll();
          showToast("履歴を更新しました");
        }
        document.getElementById("editModal").classList.remove("open");
      };
      document.getElementById("deleteTxBtn").onclick = async () => {
        const id = document.getElementById("editTxId").value;
        state.expenses = state.expenses.filter(x => x.id !== id);
        await syncExpenseDelete(id);
        renderAll();
        document.getElementById("editModal").classList.remove("open");
        showToast("削除しました");
      };
      document.getElementById("closeEditModalBtn").onclick = () => document.getElementById("editModal").classList.remove("open");

      document.getElementById("editRentBtn").onclick = () => {
        const meta = ensureMonthMeta(state.currentMonth);
        document.getElementById("rentAmountInput").value = meta.rent_amount;
        document.getElementById("rentModal").classList.add("open");
      };
      document.getElementById("saveRentBtn").onclick = async () => {
        const meta = ensureMonthMeta(state.currentMonth);
        meta.rent_amount = parseInt(document.getElementById("rentAmountInput").value || "0", 10);
        await syncMonthMetaUpsert(meta);
        renderAll();
        document.getElementById("rentModal").classList.remove("open");
        showToast("家賃を更新しました");
      };
      document.getElementById("closeRentModalBtn").onclick = () => document.getElementById("rentModal").classList.remove("open");

      document.getElementById("thankRentBtn").onclick = async () => {
        const meta = ensureMonthMeta(state.currentMonth);
        meta.rent_thanked = !meta.rent_thanked;
        await syncMonthMetaUpsert(meta);
        renderAll();
        if (meta.rent_thanked) showToast("💖 家賃のありがとうを送りました！");
      };

      document.getElementById("mfSampleBtn").onclick = async () => {
        const samples = [
          { title: "ツルヤ 軽井沢店", category: "スーパー", amount: 4820, date: `${state.currentMonth}-18` },
          { title: "中部電力ミライズ", category: "電気", amount: 7450, date: `${state.currentMonth}-19` },
          { title: "ニトリ 収納グッズ", category: "日用品", amount: 3190, date: `${state.currentMonth}-20` }
        ];
        let added = 0;
        for (const s of samples) {
          const dup = state.expenses.some(e => e.month === state.currentMonth && e.title === s.title && e.amount === s.amount);
          if (!dup) {
            const item = {
              id: "mf-" + Date.now() + "-" + Math.random().toString(36).slice(2, 6),
              month: state.currentMonth,
              date: s.date,
              category: s.category,
              title: s.title,
              amount: s.amount,
              payer: "shun",
              source: "mf",
              thanked: false
            };
            state.expenses.push(item);
            await syncExpenseUpsert(item);
            added++;
          }
        }
        renderAll();
        switchTab("home");
        showToast(added > 0 ? `MF明細を${added}件取り込みました！` : "重複データはすべてスキップしました");
      };

      document.getElementById("mfFileInput").onchange = (e) => {
        const file = e.target.files[0];
        if (!file) return;
        const reader = new FileReader();
        reader.onload = async (ev) => {
          const lines = ev.target.result.split(/\r?\n/);
          let added = 0;
          const allowedCats = ["食費", "日用品", "水道・光熱費", "通信費"];
          for (let i = 1; i < lines.length; i++) {
            const cols = lines[i].split(",").map(c => c.replace(/^"|"$/g, ""));
            if (cols.length < 6) continue;
            const dateStr = cols[1]?.replace(/\//g, "-") || `${state.currentMonth}-01`;
            const title = cols[2] || "MF明細";
            const rawAmt = Math.abs(parseInt(cols[3] || "0", 10));
            const bigCat = cols[5] || "";
            if (rawAmt > 0 && allowedCats.some(ac => bigCat.includes(ac))) {
              let mappedCat = "その他";
              if (bigCat.includes("食費")) mappedCat = "スーパー";
              else if (bigCat.includes("日用品")) mappedCat = "日用品";
              else if (bigCat.includes("光熱")) mappedCat = "電気";
              else if (bigCat.includes("通信")) mappedCat = "通信・ネット";

              const item = {
                id: "mf-" + Date.now() + "-" + i,
                month: dateStr.slice(0, 7) || state.currentMonth,
                date: dateStr,
                category: mappedCat,
                title: title,
                amount: rawAmt,
                payer: "shun",
                source: "mf",
                thanked: false
              };
              state.expenses.push(item);
              await syncExpenseUpsert(item);
              added++;
            }
          }
          renderAll();
          switchTab("home");
          showToast(`MFの共通費を${added}件取り込みました！`);
        };
        reader.readAsText(file, "Shift_JIS");
      };

      document.getElementById("ratioSlider").oninput = (e) => {
        state.shunRatio = parseInt(e.target.value, 10);
        saveLocal();
        renderAll();
      };
      document.querySelectorAll(".ratio-preset").forEach(b => {
        b.onclick = () => {
          state.shunRatio = parseInt(b.dataset.val, 10);
          saveLocal();
          renderAll();
        };
      });
      document.getElementById("toggleSettleBtn").onclick = async () => {
        const meta = ensureMonthMeta(state.currentMonth);
        meta.settled = !meta.settled;
        await syncMonthMetaUpsert(meta);
        renderAll();
        showToast(meta.settled ? "✅ 今月の精算を完了にしました！" : "精算済みを解除しました");
      };

      const savedPass = localStorage.getItem("futari_passcode") || "";
      if (savedPass) {
        document.getElementById("lockScreen").classList.add("active");
        document.getElementById("passModalBtn").classList.add("locked");
      }
      document.getElementById("unlockBtn").onclick = () => {
        const inp = document.getElementById("unlockInput").value;
        if (inp === localStorage.getItem("futari_passcode")) {
          document.getElementById("lockScreen").classList.remove("active");
          document.getElementById("unlockInput").value = "";
          document.getElementById("unlockErr").textContent = "";
        } else {
          document.getElementById("unlockErr").textContent = "合言葉が違います";
        }
      };
      document.getElementById("passModalBtn").onclick = () => {
        document.getElementById("passcodeSettingInput").value = localStorage.getItem("futari_passcode") || "";
        document.getElementById("passModal").classList.add("open");
      };
      document.getElementById("savePasscodeBtn").onclick = () => {
        const val = document.getElementById("passcodeSettingInput").value.trim();
        localStorage.setItem("futari_passcode", val);
        document.getElementById("passModalBtn").classList.toggle("locked", !!val);
        document.getElementById("passModal").classList.remove("open");
        showToast(val ? "🔒 合言葉を設定しました" : "🔓 合言葉ロックを解除しました");
      };
      document.getElementById("lockNowBtn").onclick = () => {
        const val = localStorage.getItem("futari_passcode");
        if (!val) { showToast("先に合言葉を保存してね！"); return; }
        document.getElementById("passModal").classList.remove("open");
        document.getElementById("lockScreen").classList.add("active");
      };
      document.getElementById("closePassModalBtn").onclick = () => document.getElementById("passModal").classList.remove("open");

      document.getElementById("syncModalBtn").onclick = () => document.getElementById("syncModal").classList.add("open");
      document.getElementById("closeSyncModalBtn").onclick = () => document.getElementById("syncModal").classList.remove("open");
      document.getElementById("saveSyncBtn").onclick = async () => {
        const u = document.getElementById("sbUrlInput").value.trim();
        const k = document.getElementById("sbKeyInput").value.trim();
        localStorage.setItem("sb_url", u);
        localStorage.setItem("sb_key", k);
        document.getElementById("syncModal").classList.remove("open");
        await initSupabase();
        showToast("Supabase設定を保存しました！");
      };
    }

    document.addEventListener("DOMContentLoaded", async () => {
      loadLocal();
      setupUI();
      renderAll();
      await initSupabase();
    });
  </script>
</body>
</html>
