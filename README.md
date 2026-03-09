[wavelength-game.html](https://github.com/user-attachments/files/25830563/wavelength-game.html)
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>WAVELENGTH</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&family=Noto+Sans+JP:wght@400;700;900&display=swap');

* { margin:0; padding:0; box-sizing:border-box; }

:root {
  --bg: #F5F5F3;
  --black: #111111;
  --mid: #888;
  --light: #DDDDD9;
  --white: #FFFFFF;
}

body {
  font-family: 'Noto Sans JP', sans-serif;
  background: var(--bg);
  color: var(--black);
  min-height: 100vh;
}

.screen { display:none; }
.screen.active { display:flex; }

.mono { font-family:'DM Mono', monospace; }

.tag {
  font-family:'DM Mono', monospace;
  font-size:11px;
  letter-spacing:.25em;
  text-transform:uppercase;
  color: var(--mid);
}

.btn {
  font-family:'DM Mono', monospace;
  font-size:13px;
  letter-spacing:.08em;
  padding:14px 32px;
  border:1.5px solid var(--black);
  background:transparent;
  color:var(--black);
  cursor:pointer;
  transition:all .15s;
  text-transform:uppercase;
}
.btn:hover { background:var(--black); color:var(--white); }
.btn.fill { background:var(--black); color:var(--white); }
.btn.fill:hover { background:#333; }

input[type=text], input[type=number], textarea {
  font-family:'Noto Sans JP', sans-serif;
  font-size:15px;
  background:var(--white);
  border:1.5px solid var(--light);
  color:var(--black);
  padding:12px 16px;
  outline:none;
  width:100%;
  transition:border-color .15s;
}
input:focus, textarea:focus { border-color:var(--black); }

/* HOME */
#screen-home {
  flex-direction:column;
  align-items:center;
  justify-content:center;
  min-height:100vh;
  gap:56px;
}
.home-logo { text-align:center; }
.home-logo-word {
  font-family:'DM Mono', monospace;
  font-size:clamp(48px,10vw,108px);
  font-weight:500;
  letter-spacing:.06em;
  line-height:1;
}
.home-logo-sub {
  margin-top:16px;
  font-family:'DM Mono', monospace;
  font-size:12px;
  letter-spacing:.35em;
  color:var(--mid);
}
.home-btns { display:flex; gap:12px; }

/* SETUP */
#screen-setup {
  flex-direction:column;
  align-items:center;
  justify-content:center;
  min-height:100vh;
  padding:60px 24px;
  gap:40px;
}
.setup-box {
  width:100%;
  max-width:560px;
  display:flex;
  flex-direction:column;
  gap:32px;
}
.field { display:flex; flex-direction:column; gap:8px; }
.word-row {
  display:grid;
  grid-template-columns:1fr 32px 1fr;
  gap:8px;
  align-items:center;
}
.word-sep {
  font-family:'DM Mono', monospace;
  font-size:13px;
  color:var(--mid);
  text-align:center;
}
.slider-wrap { display:flex; align-items:center; gap:20px; }
input[type=range] {
  -webkit-appearance:none;
  flex:1;
  height:2px;
  background:var(--black);
  outline:none;
  cursor:pointer;
  padding:0;
  border:none;
}
input[type=range]::-webkit-slider-thumb {
  -webkit-appearance:none;
  width:18px; height:18px;
  background:var(--white);
  border:2px solid var(--black);
  border-radius:50%;
  cursor:pointer;
}
.slider-val {
  font-family:'DM Mono', monospace;
  font-size:28px;
  min-width:40px;
  text-align:center;
}
.presets { display:flex; flex-wrap:wrap; gap:6px; }
.preset {
  font-family:'DM Mono', monospace;
  font-size:11px;
  letter-spacing:.04em;
  padding:7px 12px;
  border:1px solid var(--light);
  background:transparent;
  color:var(--mid);
  cursor:pointer;
  transition:all .12s;
}
.preset:hover { border-color:var(--black); color:var(--black); }
.setup-btns { display:flex; gap:10px; }

/* GAME */
#screen-game {
  flex-direction:column;
  align-items:center;
  justify-content:center;
  min-height:100vh;
  padding:60px 40px;
  gap:52px;
}
.topic-block { text-align:center; }
.topic-words {
  display:flex;
  align-items:baseline;
  justify-content:center;
  gap:32px;
  margin-top:14px;
}
.topic-word {
  font-size:clamp(36px,7vw,88px);
  font-weight:900;
  line-height:1;
}
.topic-word.dim { color:var(--mid); }
.topic-divider {
  font-family:'DM Mono', monospace;
  font-size:clamp(20px,4vw,40px);
  color:var(--light);
  align-self:center;
}

.scale-wrap { width:100%; max-width:860px; }
.scale-labels {
  display:flex;
  justify-content:space-between;
  font-family:'DM Mono', monospace;
  font-size:11px;
  letter-spacing:.2em;
  color:var(--mid);
  margin-bottom:10px;
}
.scale-track {
  position:relative;
  height:3px;
  background:var(--black);
}
.scale-nums {
  display:flex;
  justify-content:space-between;
  margin-top:10px;
  font-family:'DM Mono', monospace;
  font-size:11px;
  color:var(--light);
}
.markers-layer {
  position:absolute;
  inset:0;
  pointer-events:none;
}
.marker {
  position:absolute;
  display:flex;
  flex-direction:column;
  align-items:center;
  transform:translateX(-50%);
  top:-28px;
  animation:mdrop .3s cubic-bezier(.34,1.56,.64,1) both;
}
@keyframes mdrop {
  from { opacity:0; transform:translateX(-50%) translateY(-12px); }
  to   { opacity:1; transform:translateX(-50%) translateY(0); }
}
.marker-label {
  font-family:'DM Mono', monospace;
  font-size:9px;
  color:var(--black);
  white-space:nowrap;
  margin-bottom:3px;
}
.marker-dot {
  width:8px; height:8px;
  background:var(--black);
  border-radius:50%;
}

.input-row {
  display:flex;
  gap:10px;
  align-items:stretch;
  width:100%;
  max-width:500px;
}
.input-row input[type=text] { flex:1; }
.input-row input[type=number] {
  width:80px;
  text-align:center;
  font-family:'DM Mono', monospace;
  font-size:18px;
  padding:12px 8px;
}
.game-footer { display:flex; gap:10px; flex-wrap:wrap; justify-content:center; }

/* REVEAL */
.reveal-overlay {
  display:none;
  position:fixed;
  inset:0;
  background:var(--black);
  z-index:200;
  flex-direction:column;
  align-items:center;
  justify-content:center;
  gap:48px;
  padding:60px 40px;
}
.reveal-overlay.on { display:flex; }
.reveal-topic { display:flex; align-items:center; gap:32px; }
.rev-word {
  font-size:clamp(32px,6vw,72px);
  font-weight:900;
  color:var(--white);
  line-height:1;
}
.rev-word.dim { color:#444; }
.rev-div {
  font-family:'DM Mono', monospace;
  font-size:28px;
  color:#333;
}
.rev-scale { width:100%; max-width:860px; }
.rev-track { height:3px; background:#2A2A2A; position:relative; }
.rev-needle {
  position:absolute;
  top:-48px;
  display:flex;
  flex-direction:column;
  align-items:center;
  transition:left 1.4s cubic-bezier(.34,1.1,.64,1);
}
.rev-needle-tag {
  font-family:'DM Mono', monospace;
  font-size:12px;
  color:var(--white);
  border:1px solid #333;
  padding:4px 14px;
  margin-bottom:4px;
  background:var(--black);
  white-space:nowrap;
}
.rev-needle-line { width:2px; height:60px; background:#666; }

.results-grid {
  display:flex;
  flex-wrap:wrap;
  gap:10px;
  justify-content:center;
  max-width:860px;
}
.res-card {
  background:#161616;
  border:1px solid #222;
  padding:16px 22px;
  text-align:center;
  min-width:90px;
  animation:cardIn .35s both;
}
@keyframes cardIn {
  from { opacity:0; transform:scale(.85); }
  to   { opacity:1; transform:scale(1); }
}
.res-card.top { border-color:#555; }
.res-name { font-size:12px; color:#555; margin-bottom:6px; }
.res-num { font-family:'DM Mono', monospace; font-size:28px; color:var(--white); }
.res-diff { font-family:'DM Mono', monospace; font-size:10px; color:#444; margin-top:4px; }
.res-card.top .res-diff { color:#888; }

/* SCORES */
#screen-scores-view {
  flex-direction:column;
  align-items:center;
  justify-content:center;
  min-height:100vh;
  gap:36px;
  padding:60px 24px;
}
.score-list {
  width:100%;
  max-width:480px;
  display:flex;
  flex-direction:column;
  gap:2px;
}
.score-item {
  display:flex;
  align-items:center;
  gap:20px;
  padding:16px 20px;
  background:var(--white);
  border:1px solid var(--light);
  animation:slideIn .3s both;
}
.score-item:first-child { background:var(--black); color:var(--white); border-color:var(--black); }
@keyframes slideIn {
  from { opacity:0; transform:translateX(-20px); }
  to   { opacity:1; transform:translateX(0); }
}
.sc-rank { font-family:'DM Mono', monospace; font-size:13px; min-width:28px; opacity:.4; }
.sc-name { flex:1; font-size:15px; font-weight:700; }
.sc-pts { font-family:'DM Mono', monospace; font-size:20px; }

@media (max-width:600px) {
  #screen-game { padding:40px 20px; gap:36px; }
  .topic-word { font-size:44px; }
  .input-row { flex-wrap:wrap; }
}
</style>
</head>
<body>

<!-- HOME -->
<div id="screen-home" class="screen active">
  <div class="home-logo">
    <div class="home-logo-word">WAVELENGTH</div>
    <div class="home-logo-sub">team icebreaker &nbsp;/&nbsp; 16 players</div>
  </div>
  <div class="home-btns">
    <button class="btn fill" onclick="go('setup')">ゲームを設定する</button>
    <button class="btn" onclick="go('scores-view')">スコアを見る</button>
  </div>
</div>

<!-- SETUP -->
<div id="screen-setup" class="screen">
  <div class="tag">Round Setup</div>
  <div class="setup-box">

    <div class="field">
      <div class="tag">お題ワード</div>
      <div class="word-row">
        <input type="text" id="w-left" placeholder="左端（例：犬）" />
        <div class="word-sep">↔</div>
        <input type="text" id="w-right" placeholder="右端（例：猫）" />
      </div>
    </div>

    <div class="field">
      <div class="tag" style="margin-bottom:4px">プリセット</div>
      <div class="presets">
        <button class="preset" onclick="sp('朝型','夜型')">朝型 ↔ 夜型</button>
        <button class="preset" onclick="sp('静か','うるさい')">静か ↔ うるさい</button>
        <button class="preset" onclick="sp('デジタル','アナログ')">デジタル ↔ アナログ</button>
        <button class="preset" onclick="sp('クール','アツい')">クール ↔ アツい</button>
        <button class="preset" onclick="sp('インドア','アウトドア')">インドア ↔ アウトドア</button>
        <button class="preset" onclick="sp('計画的','行き当たりばったり')">計画的 ↔ 行き当たりばったり</button>
        <button class="preset" onclick="sp('甘い','辛い')">甘い ↔ 辛い</button>
        <button class="preset" onclick="sp('シャイ','オープン')">シャイ ↔ オープン</button>
        <button class="preset" onclick="sp('理系','文系')">理系 ↔ 文系</button>
        <button class="preset" onclick="sp('直感','論理')">直感 ↔ 論理</button>
      </div>
    </div>

    <div class="field">
      <div class="tag">正解の位置（参加者には見せない）</div>
      <div class="slider-wrap">
        <input type="range" id="ans-sl" min="1" max="10" value="5" oninput="updSl()">
        <div class="slider-val" id="sl-val">5</div>
      </div>
    </div>

    <div class="field">
      <div class="tag">参加者名（改行区切り）</div>
      <textarea id="p-in" rows="4" placeholder="田中&#10;鈴木&#10;山田"></textarea>
    </div>

    <div class="setup-btns">
      <button class="btn" onclick="go('home')" style="flex:1">← 戻る</button>
      <button class="btn fill" onclick="startGame()" style="flex:2">ゲーム開始</button>
    </div>
  </div>
</div>

<!-- GAME -->
<div id="screen-game" class="screen">
  <div class="tag" id="round-tag">Round 1</div>

  <div class="topic-block">
    <div class="tag">この2つの間で、正解はどのあたり？</div>
    <div class="topic-words">
      <div class="topic-word" id="g-l">犬</div>
      <div class="topic-divider">—</div>
      <div class="topic-word dim" id="g-r">猫</div>
    </div>
  </div>

  <div class="scale-wrap">
    <div class="scale-labels">
      <span id="sl-l">← 犬</span>
      <span id="sl-r">猫 →</span>
    </div>
    <div class="scale-track">
      <div class="markers-layer" id="markers"></div>
    </div>
    <div class="scale-nums">
      <span>1</span><span>2</span><span>3</span><span>4</span><span>5</span>
      <span>6</span><span>7</span><span>8</span><span>9</span><span>10</span>
    </div>
  </div>

  <div class="input-row">
    <input type="text" id="in-n" placeholder="名前" />
    <input type="number" id="in-v" min="1" max="10" placeholder="1–10" />
    <button class="btn fill" onclick="addAns()">入力</button>
  </div>

  <div class="game-footer">
    <button class="btn fill" onclick="showReveal()">正解を発表</button>
    <button class="btn" onclick="nextRound()">次のラウンド</button>
    <button class="btn" onclick="go('scores-view')">スコア確認</button>
  </div>
</div>

<!-- REVEAL -->
<div class="reveal-overlay" id="reveal">
  <div class="tag" style="color:#333">Answer Reveal</div>

  <div class="reveal-topic">
    <div class="rev-word" id="rv-l">犬</div>
    <div class="rev-div">—</div>
    <div class="rev-word dim" id="rv-r">猫</div>
  </div>

  <div class="rev-scale">
    <div class="rev-track">
      <div class="rev-needle" id="needle" style="left:50%">
        <div class="rev-needle-tag" id="needle-tag">正解: 5</div>
        <div class="rev-needle-line"></div>
      </div>
    </div>
  </div>

  <div class="results-grid" id="res-grid"></div>

  <button class="btn" style="color:#fff;border-color:#333" onclick="closeReveal()">次へ →</button>
</div>

<!-- SCORES -->
<div id="screen-scores-view" class="screen">
  <div class="tag">Scoreboard</div>
  <div class="score-list" id="scoreboard"></div>
  <div style="display:flex;gap:10px;flex-wrap:wrap;justify-content:center">
    <button class="btn" onclick="go('setup')">新しいラウンド</button>
    <button class="btn" onclick="resetAll()">全リセット</button>
  </div>
</div>

<script>
let G = { round:1, word:{left:'',right:'',answer:5}, answers:[], players:[] };

function go(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById('screen-' + id).classList.add('active');
  if (id === 'scores-view') renderScores();
}

function updSl() {
  document.getElementById('sl-val').textContent = document.getElementById('ans-sl').value;
}

function sp(l,r) {
  document.getElementById('w-left').value = l;
  document.getElementById('w-right').value = r;
}

function startGame() {
  const left  = document.getElementById('w-left').value.trim()  || '左';
  const right = document.getElementById('w-right').value.trim() || '右';
  const ans   = +document.getElementById('ans-sl').value;
  const txt   = document.getElementById('p-in').value.trim();

  G.word = { left, right, answer:ans };
  G.answers = [];

  if (txt && G.players.length === 0) {
    G.players = txt.split('\n').map(n=>n.trim()).filter(Boolean).map(n=>({name:n,score:0}));
  }

  document.getElementById('g-l').textContent = left;
  document.getElementById('g-r').textContent = right;
  document.getElementById('sl-l').textContent = '← ' + left;
  document.getElementById('sl-r').textContent = right + ' →';
  document.getElementById('round-tag').textContent = 'Round ' + G.round;
  document.getElementById('markers').innerHTML = '';

  go('game');
}

function addAns() {
  const name = document.getElementById('in-n').value.trim();
  const num  = +document.getElementById('in-v').value;
  if (!name || !num || num < 1 || num > 10) return;

  G.answers.push({ name, num });

  const pct = ((num-1)/9)*100;
  const m = document.createElement('div');
  m.className = 'marker';
  m.style.left = pct + '%';
  m.style.animationDelay = (G.answers.length * 0.04) + 's';
  m.innerHTML = `<div class="marker-label">${name}</div><div class="marker-dot"></div>`;
  document.getElementById('markers').appendChild(m);

  document.getElementById('in-n').value = '';
  document.getElementById('in-v').value = '';
  document.getElementById('in-n').focus();
}

function showReveal() {
  const { left, right, answer } = G.word;
  document.getElementById('rv-l').textContent = left;
  document.getElementById('rv-r').textContent = right;
  document.getElementById('reveal').classList.add('on');

  setTimeout(() => {
    const pct = ((answer-1)/9)*100;
    document.getElementById('needle').style.left = pct + '%';
    document.getElementById('needle-tag').textContent = '正解: ' + answer;
  }, 300);

  const grid = document.getElementById('res-grid');
  grid.innerHTML = '';
  const sorted = [...G.answers].sort((a,b)=>Math.abs(a.num-answer)-Math.abs(b.num-answer));

  sorted.forEach((p,i) => {
    const diff = Math.abs(p.num - answer);
    const pts  = Math.max(0, 5 - diff);
    const pl   = G.players.find(x=>x.name===p.name);
    if (pl) pl.score += pts;
    else G.players.push({ name:p.name, score:pts });

    const d = document.createElement('div');
    d.className = 'res-card' + (i===0?' top':'');
    d.style.animationDelay = (i*0.07)+'s';
    const label = diff===0 ? 'EXACT' : diff===1 ? '+1 CLOSE' : 'diff '+diff;
    d.innerHTML = `
      <div class="res-name">${p.name}</div>
      <div class="res-num">${p.num}</div>
      <div class="res-diff">${label} · +${pts}pt</div>
    `;
    grid.appendChild(d);
  });
}

function closeReveal() {
  document.getElementById('reveal').classList.remove('on');
}

function nextRound() {
  G.round++;
  document.getElementById('w-left').value  = '';
  document.getElementById('w-right').value = '';
  document.getElementById('ans-sl').value  = 5;
  updSl();
  go('setup');
}

function renderScores() {
  const el = document.getElementById('scoreboard');
  el.innerHTML = '';
  const s = [...G.players].sort((a,b)=>b.score-a.score);
  if (!s.length) {
    el.innerHTML = '<div style="font-family:\'DM Mono\',monospace;font-size:13px;color:#999;padding:40px;text-align:center">No scores yet</div>';
    return;
  }
  s.forEach((p,i) => {
    const d = document.createElement('div');
    d.className = 'score-item';
    d.style.animationDelay = (i*0.05)+'s';
    d.innerHTML = `
      <div class="sc-rank">${String(i+1).padStart(2,'0')}</div>
      <div class="sc-name">${p.name}</div>
      <div class="sc-pts">${p.score}<span style="font-size:11px;opacity:.4">pt</span></div>
    `;
    el.appendChild(d);
  });
}

function resetAll() {
  if (!confirm('スコアを全リセットしますか？')) return;
  G = { round:1, word:{left:'',right:'',answer:5}, answers:[], players:[] };
  go('home');
}

document.addEventListener('keydown', e => {
  if (e.key==='Enter' && document.getElementById('screen-game').classList.contains('active')) addAns();
});
</script>
</body>
</html>
