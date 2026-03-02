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
  --board-light:#d4c5a9;
  --board-dark:#5c4033;
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
/* grain texture */
body::after{
  content:'';position:fixed;inset:0;pointer-events:none;z-index:999;
  background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='300' height='300'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.75' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='300' height='300' filter='url(%23n)' opacity='0.035'/%3E%3C/svg%3E");
  opacity:1;
}

.page{
  width:min(800px,100vw);
  padding:44px 40px 60px;
  display:grid;
  grid-template-columns:auto 1fr;
  gap:0;
  opacity:0;
  animation:appear 0.5s 0.15s ease forwards;
}
@keyframes appear{
  from{opacity:0;transform:translateY(6px);}
  to{opacity:1;transform:none;}
}

/* ── MASTHEAD ── */
.masthead{
  grid-column:1/-1;
  display:grid;
  grid-template-columns:1fr auto;
  align-items:end;
  padding-bottom:10px;
  border-bottom:1px solid var(--ink);
  margin-bottom:10px;
}
.masthead-name{
  font-family:'UnifrakturMaguntia',cursive;
  font-size:clamp(2.6rem,6vw,4.2rem);
  line-height:0.95;
}
.masthead-right{
  text-align:right;
  font-size:0.58rem;
  letter-spacing:0.2em;
  text-transform:uppercase;
  color:#888;
  line-height:2;
}
.masthead-rule{
  grid-column:1/-1;
  border:none;
  border-top:3px solid var(--ink);
  margin-bottom:28px;
}

/* ── LEFT: BOARD ── */
.board-col{
  padding-right:30px;
  border-right:1px solid #ccc;
}
.col-kicker{
  font-size:0.56rem;
  letter-spacing:0.22em;
  text-transform:uppercase;
  color:#999;
  margin-bottom:10px;
}
.board-frame{
  display:inline-block;
  position:relative;
}
.board-grid{
  --sq: 40px;
  display:grid;
  grid-template-columns:repeat(8, var(--sq));
  grid-template-rows:   repeat(8, var(--sq));
  width: calc(8 * var(--sq));
  height: calc(8 * var(--sq));
  border:2px solid var(--ink);
  box-shadow:5px 5px 0 var(--ink);
  flex-shrink: 0;
}
.sq{
  position:relative;
  width: var(--sq);
  height: var(--sq);
  display:flex;
  align-items:center;
  justify-content:center;
  overflow: hidden;
}
.sq.light{background:var(--board-light);}
.sq.dark {background:var(--board-dark);}

.sq.hl-from::before{
  content:'';position:absolute;inset:0;
  background:rgba(139,26,26,0.2);
  pointer-events:none;z-index:1;
}
.sq.hl-to::before{
  content:'';position:absolute;inset:0;
  background:rgba(139,26,26,0.42);
  pointer-events:none;z-index:1;
}
/* small dot on hl-from */
.sq.hl-from::after{
  content:'';
  position:absolute;
  width:28%;height:28%;
  border-radius:50%;
  background:rgba(139,26,26,0.5);
  pointer-events:none;z-index:2;
}

.piece{
  font-size: 1.55rem;
  line-height:1;
  z-index:3;
  user-select:none;
  position:relative;
  transition:transform 0.1s;
}
.piece.w{filter:drop-shadow(0 1px 0 rgba(0,0,0,0.45));}
.piece.b{filter:drop-shadow(0 1px 0 rgba(0,0,0,0.9));}
.piece.land{animation:land 0.3s cubic-bezier(.1,1.5,.4,1) forwards;}
@keyframes land{
  0%{transform:translateY(-8px) scale(1.25);}
  65%{transform:translateY(1px) scale(0.95);}
  100%{transform:none;}
}

.coord{
  position:absolute;
  font-size:0.5rem;
  font-family:'IBM Plex Mono',monospace;
  font-weight:300;
  color:rgba(0,0,0,0.28);
  pointer-events:none;z-index:4;line-height:1;
}
.sq.dark .coord{color:rgba(255,255,255,0.22);}
.coord.r{top:2px;left:3px;}
.coord.f{bottom:2px;right:3px;}

/* status under board */
.board-status{
  margin-top:10px;
  font-size:0.6rem;
  letter-spacing:0.14em;
  text-transform:uppercase;
  color:#bbb;
  min-height:15px;
  transition:color 0.3s;
}
.board-status.on{color:var(--red);}

/* ── RIGHT: SIDEBAR ── */
.sidebar{
  padding-left:28px;
  display:flex;
  flex-direction:column;
}
.rule{border:none;border-top:1px solid #ccc;margin:16px 0 12px;}
.rule.heavy{border-color:var(--ink);border-top-width:2px;margin:20px 0 14px;}

.s-kicker{
  font-size:0.56rem;
  letter-spacing:0.22em;
  text-transform:uppercase;
  color:#999;
  margin-bottom:6px;
}
.s-headline{
  font-family:'Cormorant Garamond',serif;
  font-size:clamp(1.4rem,2.8vw,2rem);
  font-weight:600;
  line-height:1.1;
  letter-spacing:-0.01em;
}
.s-headline em{font-style:italic;font-weight:300;color:var(--red);}
.s-body{
  font-family:'Cormorant Garamond',serif;
  font-size:0.88rem;
  font-weight:300;
  line-height:1.7;
  color:#3a3530;
  margin-top:7px;
}

/* score box */
.score-box{
  display:inline-block;
  border:1px solid var(--ink);
  padding:5px 12px;
  margin-top:10px;
  font-size:0.75rem;
  letter-spacing:0.1em;
  position:relative;
}
.score-box span{
  position:absolute;
  top:-8px;left:7px;
  background:var(--paper);
  padding:0 4px;
  font-size:0.48rem;
  letter-spacing:0.22em;
  text-transform:uppercase;
  color:#999;
}

/* tags */
.tags{display:flex;flex-wrap:wrap;gap:4px;margin-top:8px;}
.tag{
  border:1px solid var(--ink);
  font-size:0.54rem;
  letter-spacing:0.12em;
  text-transform:uppercase;
  padding:2px 6px;
}

/* move log — typeset notation style */
.log{
  display:flex;
  flex-wrap:wrap;
  gap:0 6px;
  font-size:0.66rem;
  line-height:2;
  font-variant-numeric:tabular-nums;
  color:#777;
}
.log-pair{white-space:nowrap;}
.log-n{color:#bbb;margin-right:1px;}
.log-w,.log-b{color:var(--ink);}
.log-w.cur,.log-b.cur{
  color:var(--red);
  text-decoration:underline;
  text-decoration-thickness:1px;
  text-underline-offset:2px;
}

/* ── FOOTER ── */
.footer{
  grid-column:1/-1;
  margin-top:28px;
  padding-top:10px;
  border-top:3px double var(--ink);
  display:flex;
  justify-content:space-between;
  align-items:baseline;
  font-size:0.56rem;
  letter-spacing:0.16em;
  text-transform:uppercase;
  color:#aaa;
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

  <!-- BOARD -->
  <div class="board-col">
    <div class="col-kicker">The Immortal Game · 1851</div>
    <div class="board-frame">
      <div class="board-grid" id="board"></div>
    </div>
    <div class="board-status" id="status">Setting up position…</div>
  </div>

  <!-- SIDEBAR -->
  <div class="sidebar">
    <div class="s-kicker">Featured Project</div>
    <div class="s-headline">A Chess Engine<br>That <em>Thinks</em></div>
    <p class="s-body">
      Minimax search with alpha-beta pruning, legal move generation,
      and a networked client–server architecture. A-Level NEA.
    </p>
    <div class="score-box"><span>NEA Score</span>73 / 75</div>
    <div class="tags">
      <span class="tag">Python</span>
      <span class="tag">Minimax</span>
      <span class="tag">α-β Pruning</span>
      <span class="tag">TCP/IP</span>
    </div>

    <hr class="rule heavy">
    <div class="s-kicker">Also Building</div>
    <p class="s-body" style="margin-top:0">
      <strong>Social Hub</strong> — one platform for society
      comms, events & payments. Launching at York.
    </p>
    <div class="tags">
      <span class="tag">Next.js</span>
      <span class="tag">Firebase</span>
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
const G={wK:'♔',wQ:'♕',wR:'♖',wB:'♗',wN:'♘',wP:'♙',bK:'♚',bQ:'♛',bR:'♜',bB:'♝',bN:'♞',bP:'♟'};

function freshBoard(){
  const b=Array.from({length:8},()=>new Array(8).fill(null));
  const back=['R','N','B','Q','K','B','N','R'];
  for(let c=0;c<8;c++){
    b[0][c]='b'+back[c]; b[1][c]='bP';
    b[6][c]='wP';        b[7][c]='w'+back[c];
  }
  return b;
}

// Immortal Game (condensed to ~20 clean moves)
// [fromCol, fromRow, toCol, toRow, notation]  row: 0=rank8, 7=rank1
const GAME=[
  [4,6,4,4,'e4'],
  [4,1,4,3,'e5'],
  [5,7,2,4,'Bc4'],
  [1,0,2,2,'Nc6'],
  [6,7,5,5,'Nf3'],
  [6,0,5,2,'Nf6'],
  [3,7,7,3,'Qh5'],
  [6,1,6,2,'g6'],
  [7,3,5,1,'Qxf7+'],
  [4,0,5,0,'Kf8'],
  [5,5,4,3,'Nd5'],
  [5,2,4,4,'Nxd5'],
  [2,4,4,2,'Bxd5'],
  [2,2,0,3,'Na5'],
  [3,7,5,5,'Qf3'],
  [0,3,1,1,'Nb3'],
  [4,7,3,7,'Kd1'],
  [1,1,3,2,'Nd3+'],
  [3,7,4,7,'Ke1'],
  [3,2,1,1,'Nb2'],
  [0,7,0,5,'Ra3'],
  [1,1,0,3,'Nxa3'],
  [5,5,4,4,'Nxe5'],
  [4,3,3,1,'Qd2+'],
  [4,7,5,7,'Kf1'],
  [3,1,4,3,'Qxe5'],
];

let board=freshBoard();
let cur=0,lastFrom=null,lastTo=null;
const log=[];

function render(){
  const el=document.getElementById('board');
  el.innerHTML='';
  for(let r=0;r<8;r++){
    for(let c=0;c<8;c++){
      const sq=document.createElement('div');
      const light=(r+c)%2===0;
      sq.className='sq '+(light?'light':'dark');
      const isF=lastFrom&&lastFrom[0]===c&&lastFrom[1]===r;
      const isT=lastTo&&lastTo[0]===c&&lastTo[1]===r;
      if(isF)sq.classList.add('hl-from');
      if(isT)sq.classList.add('hl-to');
      if(c===0){const d=document.createElement('div');d.className='coord r';d.textContent=8-r;sq.appendChild(d);}
      if(r===7){const d=document.createElement('div');d.className='coord f';d.textContent='abcdefgh'[c];sq.appendChild(d);}
      const p=board[r][c];
      if(p){
        const s=document.createElement('span');
        s.className='piece '+(p[0]==='w'?'w':'b');
        s.textContent=G[p];
        if(isT)s.classList.add('land');
        sq.appendChild(s);
      }
      el.appendChild(sq);
    }
  }
}

function renderLog(){
  const el=document.getElementById('log');
  el.innerHTML='';
  log.forEach(([w,b],i)=>{
    const pair=document.createElement('span');
    pair.className='log-pair';
    const n=document.createElement('span');n.className='log-n';n.textContent=(i+1)+'.';
    const ws=document.createElement('span');
    ws.className='log-w'+(cur===i*2+1?' cur':'');
    ws.textContent=w+' ';
    pair.appendChild(n);pair.appendChild(ws);
    if(b!==undefined){
      const bs=document.createElement('span');
      bs.className='log-b'+(cur===i*2+2?' cur':'');
      bs.textContent=b+' ';
      pair.appendChild(bs);
    }
    el.appendChild(pair);
  });
}

function step(){
  if(cur>=GAME.length){
    setTimeout(()=>{
      board=freshBoard();cur=0;lastFrom=null;lastTo=null;log.length=0;
      render();renderLog();
      setStatus('Restarting…',false);
      setTimeout(tick,1800);
    },4500);
    return;
  }
  const[fc,fr,tc,tr,note]=GAME[cur];
  lastFrom=[fc,fr];lastTo=[tc,tr];
  board[tr][tc]=board[fr][fc];
  board[fr][fc]=null;
  const pi=Math.floor(cur/2);
  if(cur%2===0) log[pi]=[note];
  else{if(!log[pi])log[pi]=['…',note];else log[pi][1]=note;}
  const wt=cur%2===0;
  setStatus((wt?'⬜ ':' ⬛ ')+note,true);
  cur++;
  render();renderLog();
}

function setStatus(t,on){
  const el=document.getElementById('status');
  el.textContent=t;
  el.className='board-status'+(on?' on':'');
}

function tick(){
  step();
  if(cur<=GAME.length) setTimeout(tick,1150+Math.random()*550);
}

render();renderLog();
setTimeout(()=>{setStatus('1851 · London · King\'s Gambit',false);setTimeout(tick,1200);},600);
</script>
</body>
</html>
