<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ConnConn24</title>
<link href="https://fonts.googleapis.com/css2?family=UnifrakturMaguntia&family=IBM+Plex+Mono:wght@300;400;500&family=Cormorant+Garamond:ital,wght@0,300;0,600;1,300&display=swap" rel="stylesheet">
<style>
*{margin:0;padding:0;box-sizing:border-box;}
:root{
  --ink:#0e0c0a;
  --paper:#f2ede4;
  --red:#8b1a1a;
  --bl:#d4c5a9;
  --bd:#5c4033;
  --sq:44px;
}
html,body{
  min-height:100vh;
  background:var(--paper);
  color:var(--ink);
  font-family:'IBM Plex Mono',monospace;
  display:flex;
  align-items:flex-start;
  justify-content:center;
}
body::after{
  content:'';position:fixed;inset:0;pointer-events:none;z-index:999;
  background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='200' height='200'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.75' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='200' height='200' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
}
.page{
  width:min(820px,100vw);
  padding:44px 40px 60px;
  display:grid;
  grid-template-columns:auto 1fr;
  gap:0;
  opacity:0;
  animation:appear 0.5s 0.1s ease forwards;
}
@keyframes appear{from{opacity:0;transform:translateY(6px);}to{opacity:1;transform:none;}}

/* MASTHEAD */
.masthead{
  grid-column:1/-1;
  display:grid;
  grid-template-columns:1fr auto;
  align-items:end;
  padding-bottom:10px;
  border-bottom:1px solid var(--ink);
  margin-bottom:10px;
}
.masthead-name{font-family:'UnifrakturMaguntia',cursive;font-size:clamp(2.6rem,6vw,4.2rem);line-height:0.95;}
.masthead-right{text-align:right;font-size:0.58rem;letter-spacing:0.2em;text-transform:uppercase;color:#888;line-height:2;}
.masthead-rule{grid-column:1/-1;border:none;border-top:3px solid var(--ink);margin-bottom:28px;}

/* BOARD COL */
.board-col{padding-right:30px;border-right:1px solid #ccc;}
.col-kicker{font-size:0.56rem;letter-spacing:0.22em;text-transform:uppercase;color:#999;margin-bottom:10px;}

/* THE BOARD */
.board-wrap{display:inline-block;line-height:0;}
.board-grid{
  display:grid;
  grid-template-columns:repeat(8,var(--sq));
  grid-template-rows:repeat(8,var(--sq));
  width:calc(8*var(--sq));
  height:calc(8*var(--sq));
  border:2px solid var(--ink);
  box-shadow:5px 5px 0 var(--ink);
}
/* squares — built once, never rebuilt */
.sq{
  width:var(--sq);
  height:var(--sq);
  position:relative;
  display:flex;
  align-items:center;
  justify-content:center;
}
.sq.L{background:var(--bl);}
.sq.D{background:var(--bd);}
.sq.hF{background:rgba(139,26,26,0.35);}
.sq.hT{background:rgba(139,26,26,0.55);}
/* keep light/dark tint visible under highlight */
.sq.L.hF{background:color-mix(in srgb,var(--bl) 65%,rgba(139,26,26,1) 35%);}
.sq.L.hT{background:color-mix(in srgb,var(--bl) 45%,rgba(139,26,26,1) 55%);}
.sq.D.hF{background:color-mix(in srgb,var(--bd) 65%,rgba(139,26,26,1) 35%);}
.sq.D.hT{background:color-mix(in srgb,var(--bd) 45%,rgba(139,26,26,1) 55%);}

/* coord labels inside corner squares */
.coord{
  position:absolute;
  font-size:0.48rem;
  font-family:'IBM Plex Mono',monospace;
  font-weight:300;
  line-height:1;
  pointer-events:none;
  color:rgba(0,0,0,0.3);
}
.sq.D .coord{color:rgba(255,255,255,0.25);}
.coord.r{top:2px;left:3px;}
.coord.f{bottom:2px;right:3px;}

/* piece */
.piece{
  font-size:1.6rem;
  line-height:1;
  user-select:none;
  pointer-events:none;
  position:relative;
  z-index:2;
}
.piece.W{filter:drop-shadow(0 1px 0 rgba(0,0,0,0.5));}
.piece.B{filter:drop-shadow(0 1px 0 rgba(0,0,0,0.95));}
.piece.land{animation:land 0.28s cubic-bezier(.1,1.6,.4,1) forwards;}
@keyframes land{
  0%  {transform:translateY(-10px) scale(1.3);}
  65% {transform:translateY(2px) scale(0.93);}
  100%{transform:none;}
}

/* status */
.board-status{
  margin-top:10px;font-size:0.6rem;letter-spacing:0.14em;
  text-transform:uppercase;color:#bbb;min-height:16px;transition:color 0.3s;
}
.board-status.on{color:var(--red);}

/* SIDEBAR */
.sidebar{padding-left:28px;display:flex;flex-direction:column;}
.rule{border:none;border-top:1px solid #ccc;margin:16px 0 12px;}
.rule.heavy{border-color:var(--ink);border-top-width:2px;margin:20px 0 14px;}
.s-kicker{font-size:0.56rem;letter-spacing:0.22em;text-transform:uppercase;color:#999;margin-bottom:6px;}
.s-headline{font-family:'Cormorant Garamond',serif;font-size:clamp(1.4rem,2.8vw,2rem);font-weight:600;line-height:1.1;letter-spacing:-0.01em;}
.s-headline em{font-style:italic;font-weight:300;color:var(--red);}
.s-body{font-family:'Cormorant Garamond',serif;font-size:0.9rem;font-weight:300;line-height:1.7;color:#3a3530;margin-top:7px;}
.score-box{display:inline-block;border:1px solid var(--ink);padding:5px 12px;margin-top:10px;font-size:0.75rem;letter-spacing:0.1em;position:relative;}
.score-box span{position:absolute;top:-8px;left:7px;background:var(--paper);padding:0 4px;font-size:0.48rem;letter-spacing:0.22em;text-transform:uppercase;color:#999;}
.tags{display:flex;flex-wrap:wrap;gap:4px;margin-top:8px;}
.tag{border:1px solid var(--ink);font-size:0.54rem;letter-spacing:0.12em;text-transform:uppercase;padding:2px 6px;}

/* move log */
.log{display:flex;flex-wrap:wrap;gap:0 6px;font-size:0.66rem;line-height:2;color:#777;}
.log-pair{white-space:nowrap;}
.log-n{color:#bbb;margin-right:1px;}
.log-w,.log-b{color:var(--ink);}
.log-w.cur,.log-b.cur{color:var(--red);text-decoration:underline;text-decoration-thickness:1px;text-underline-offset:2px;}

/* FOOTER */
.footer{
  grid-column:1/-1;margin-top:28px;padding-top:10px;
  border-top:3px double var(--ink);
  display:flex;justify-content:space-between;align-items:baseline;
  font-size:0.56rem;letter-spacing:0.16em;text-transform:uppercase;color:#aaa;
}
.footer a{color:inherit;text-decoration:none;border-bottom:1px solid #ccc;padding-bottom:1px;}
</style>
</head>
<body>
<div class="page">

  <header class="masthead">
    <div class="masthead-name">ConnConn24</div>
    <div class="masthead-right">
      Computer Science · York<br>
      Co-Founder · Social Hub<br>
      Top 3% · Codewars
    </div>
  </header>
  <hr class="masthead-rule">

  <div class="board-col">
    <div class="col-kicker">The Immortal Game &nbsp;·&nbsp; Anderssen vs Kieseritzky, 1851</div>
    <div class="board-wrap">
      <div class="board-grid" id="board"></div>
    </div>
    <div class="board-status" id="status">Setting up position…</div>
  </div>

  <div class="sidebar">
    <div class="s-kicker">Featured Project</div>
    <div class="s-headline">A Chess Engine<br>That <em>Thinks</em></div>
    <p class="s-body">Minimax search with alpha-beta pruning, full legal move
    generation, and a networked client–server architecture. A-Level NEA.</p>
    <div class="score-box"><span>NEA Score</span>73 / 75</div>
    <div class="tags">
      <span class="tag">Python</span><span class="tag">Minimax</span>
      <span class="tag">α-β Pruning</span><span class="tag">TCP/IP</span>
    </div>
    <hr class="rule heavy">
    <div class="s-kicker">Also Building</div>
    <p class="s-body" style="margin-top:0"><strong>Social Hub</strong> — one platform
    for society comms, events &amp; payments. Launching at York.</p>
    <div class="tags">
      <span class="tag">Next.js</span><span class="tag">Firebase</span>
      <span class="tag">TypeScript</span>
    </div>
    <hr class="rule">
    <div class="s-kicker">Game Score</div>
    <div class="log" id="log"></div>
  </div>

  <footer class="footer">
    <span>Grade 8 Guitar &nbsp;·&nbsp; Grade 5 Piano</span>
    <span>github.com/<a href="https://github.com/ConnConn24">ConnConn24</a></span>
  </footer>
</div>

<script>
// ── pieces ──────────────────────────────────────────────────────────────────
const G = {
  wK:'♔',wQ:'♕',wR:'♖',wB:'♗',wN:'♘',wP:'♙',
  bK:'♚',bQ:'♛',bR:'♜',bB:'♝',bN:'♞',bP:'♟'
};

// ── board state ──────────────────────────────────────────────────────────────
// state[r][c]: piece string or null. r=0 → rank 8, r=7 → rank 1
function freshState() {
  const b = Array.from({length:8}, () => Array(8).fill(null));
  const back = ['R','N','B','Q','K','B','N','R'];
  for (let c = 0; c < 8; c++) {
    b[0][c] = 'b'+back[c]; b[1][c] = 'bP';
    b[6][c] = 'wP';        b[7][c] = 'w'+back[c];
  }
  return b;
}

// ── DOM grid — built ONCE, never torn down ───────────────────────────────────
const sqEls = []; // sqEls[r][c] = the div

function buildGrid() {
  const grid = document.getElementById('board');
  for (let r = 0; r < 8; r++) {
    sqEls[r] = [];
    for (let c = 0; c < 8; c++) {
      const div = document.createElement('div');
      div.className = 'sq ' + ((r+c)%2===0 ? 'L' : 'D');
      // rank coord on leftmost column
      if (c === 0) {
        const cr = document.createElement('span');
        cr.className = 'coord r';
        cr.textContent = 8 - r;
        div.appendChild(cr);
      }
      // file coord on bottom row
      if (r === 7) {
        const cf = document.createElement('span');
        cf.className = 'coord f';
        cf.textContent = 'abcdefgh'[c];
        div.appendChild(cf);
      }
      grid.appendChild(div);
      sqEls[r][c] = div;
    }
  }
}

// ── update board visuals (no innerHTML wipe) ─────────────────────────────────
let prevFrom = null, prevTo = null;

function updateBoard(state, from, to) {
  // remove old highlights
  if (prevFrom) setSqHighlight(prevFrom[0], prevFrom[1], false, false);
  if (prevTo)   setSqHighlight(prevTo[0],   prevTo[1],   false, false);

  // set new highlights
  if (from) setSqHighlight(from[0], from[1], true, false);
  if (to)   setSqHighlight(to[0],   to[1],   false, true);

  prevFrom = from; prevTo = to;

  // update all piece spans
  for (let r = 0; r < 8; r++) {
    for (let c = 0; c < 8; c++) {
      const div = sqEls[r][c];
      const piece = state[r][c];
      // find existing piece span (if any)
      let pspan = div.querySelector('.piece');
      if (piece) {
        const glyph = G[piece];
        const cls = 'piece ' + (piece[0]==='w' ? 'W' : 'B');
        const isLanding = to && to[0]===r && to[1]===c;
        if (!pspan) {
          pspan = document.createElement('span');
          div.appendChild(pspan);
        }
        pspan.className = cls + (isLanding ? ' land' : '');
        pspan.textContent = glyph;
      } else {
        if (pspan) pspan.remove();
      }
    }
  }
}

function setSqHighlight(r, c, isFrom, isTo) {
  const div = sqEls[r][c];
  div.classList.remove('hF','hT');
  if (isFrom) div.classList.add('hF');
  if (isTo)   div.classList.add('hT');
}

// ── game moves (Immortal Game, condensed) ────────────────────────────────────
// [fromCol, fromRow, toCol, toRow, notation]   row 0 = rank 8, row 7 = rank 1
const GAME = [
  [4,6, 4,4, 'e4'],
  [4,1, 4,3, 'e5'],
  [5,7, 2,4, 'Bc4'],
  [1,0, 2,2, 'Nc6'],
  [6,7, 5,5, 'Nf3'],
  [6,0, 5,2, 'Nf6'],
  [3,7, 7,3, 'Qh5'],
  [6,1, 6,2, 'g6'],
  [7,3, 5,1, 'Qxf7+'],
  [4,0, 5,0, 'Kf8'],
  [5,5, 4,3, 'Nd5'],
  [5,2, 4,4, 'Nxd5'],
  [2,4, 4,2, 'Bxd5'],
  [2,2, 0,3, 'Na5'],
  [3,7, 5,5, 'Qf3'],
  [0,3, 1,1, 'Nb3'],
  [4,7, 3,7, 'Kd1'],
  [1,1, 3,2, 'Nd3+'],
  [3,7, 4,7, 'Ke1'],
  [3,2, 1,1, 'Nb2'],
  [0,7, 0,5, 'Ra3'],
  [1,1, 0,3, 'Nxa3'],
  [5,5, 4,4, 'Nxe5'],
  [4,3, 3,1, 'Qd2+'],
  [4,7, 5,7, 'Kf1'],
  [3,1, 4,3, 'Qxe5'],
];

// ── move log ─────────────────────────────────────────────────────────────────
const logPairs = []; // [[white, black?], ...]

function renderLog(curIdx) {
  const el = document.getElementById('log');
  el.innerHTML = '';
  logPairs.forEach(([w, b], i) => {
    const span = document.createElement('span');
    span.className = 'log-pair';
    const n = document.createElement('span'); n.className='log-n'; n.textContent=(i+1)+'.';
    const ws = document.createElement('span');
    ws.className = 'log-w' + (curIdx===i*2+1 ? ' cur' : '');
    ws.textContent = w + ' ';
    span.appendChild(n); span.appendChild(ws);
    if (b !== undefined) {
      const bs = document.createElement('span');
      bs.className = 'log-b' + (curIdx===i*2+2 ? ' cur' : '');
      bs.textContent = b + ' ';
      span.appendChild(bs);
    }
    el.appendChild(span);
  });
}

// ── status ───────────────────────────────────────────────────────────────────
function setStatus(text, active) {
  const el = document.getElementById('status');
  el.textContent = text;
  el.className = 'board-status' + (active ? ' on' : '');
}

// ── game loop ─────────────────────────────────────────────────────────────────
let state = freshState();
let cur = 0;

function step() {
  if (cur >= GAME.length) {
    setTimeout(restart, 4500);
    return;
  }
  const [fc, fr, tc, tr, note] = GAME[cur];

  // update state
  state[tr][tc] = state[fr][fc];
  state[fr][fc] = null;

  // highlight: from=[r,c], to=[r,c]
  updateBoard(state, [fr, fc], [tr, tc]);

  // log
  const pi = Math.floor(cur / 2);
  if (cur % 2 === 0) logPairs[pi] = [note];
  else { if (!logPairs[pi]) logPairs[pi] = ['…', note]; else logPairs[pi][1] = note; }
  cur++;
  renderLog(cur);

  setStatus((cur%2===1 ? '⬜ ' : '⬛ ') + note, true);
  setTimeout(step, 1200 + Math.random() * 500);
}

function restart() {
  state = freshState();
  cur = 0;
  logPairs.length = 0;
  prevFrom = null; prevTo = null;
  updateBoard(state, null, null);
  renderLog(0);
  setStatus('Restarting…', false);
  setTimeout(step, 1800);
}

// ── boot ─────────────────────────────────────────────────────────────────────
buildGrid();
updateBoard(state, null, null);
setStatus('1851 · London · King\'s Gambit', false);
setTimeout(step, 1400);
</script>
</body>
</html>
