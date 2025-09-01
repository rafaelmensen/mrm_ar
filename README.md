<!doctype html>
<html lang="pt-BR">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Sicredi • Dashboards</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
<style>
:root{--bg:#EFF3F8;--card:#fff;--muted:#6b7280;--border:#E4E6EB;--accent:#10b981;--ink:#0f172a;--ph:64px}
*{box-sizing:border-box} body{margin:0;font-family:Inter,ui-sans-serif,-apple-system,"Segoe UI",Roboto;color:var(--ink);background:var(--bg)}
/* Top */
header{display:flex;justify-content:space-between;align-items:center;padding:12px 24px;background:#fff;border-bottom:1px solid var(--border)}
.logo{display:flex;align-items:center;gap:10px}.logo .mark{width:28px;height:28px;border-radius:999px;background:var(--accent);display:grid;place-items:center;color:#fff;font-weight:700}
.logo .title{color:#059669;font-weight:700}

/* Layout */
.layout{display:flex;min-height:calc(100vh - 58px)}
aside.sidebar{width:60px;border-right:1px solid var(--border);background:#fff;padding:12px 8px;display:flex;flex-direction:column;align-items:center;gap:8px}
.icon{width:40px;height:40px;border-radius:12px;display:grid;place-items:center;color:#667085;cursor:pointer;transition:transform .12s,color .12s,box-shadow .18s}
.icon:hover{color:var(--accent);transform:translateY(-2px);box-shadow:0 8px 24px rgba(2,6,23,.08)}
.sidebar .bottom{margin-top:auto;margin-bottom:8px}

/* Main */
main.main{flex:1;padding:24px;position:relative}
.panel{border:1px solid var(--border);background:#fff;border-radius:12px;box-shadow:0 1px 2px rgba(2,6,23,.05)}
/* Header fixo */
.panel-hd{position:sticky;top:0;z-index:5;display:flex;gap:12px;align-items:center;justify-content:space-between;height:var(--ph);padding:12px 20px;background:#fff;border-bottom:1px solid var(--border)}
.panel-hd h2{margin:0;font-size:18px}
.hd-left{display:flex;align-items:center;gap:8px}
.search{position:relative}
.search input{width:320px;max-width:42vw;padding:9px 12px 9px 36px;border:1px solid var(--border);border-radius:10px;font-size:14px}
.search svg{position:absolute;left:10px;top:50%;transform:translateY(-50%);width:16px;height:16px;color:#94a3b8}
/* Back CTA no lugar da pesquisa quando um dash estiver aberto */
.back-cta{display:none;align-items:center;gap:8px;border:1px solid #34d399;background:#10b981;color:#fff;border-radius:10px;padding:9px 14px;cursor:pointer;font-weight:600}
@keyframes breathe{0%{transform:translateZ(0) scale(1)}50%{transform:translateZ(0) scale(1.015)}100%{transform:translateZ(0) scale(1)}}
.back-cta.anim{animation:breathe 2.4s ease-in-out infinite;box-shadow:0 8px 18px rgba(16,185,129,.25)}

/* Sections */
.navbox{padding:18px}
.section{border:1px solid var(--border);border-radius:14px;background:#fff;box-shadow:0 4px 14px rgba(2,6,23,.05);margin-bottom:18px;overflow:hidden}
.section-hd{display:flex;align-items:center;justify-content:space-between;padding:12px 16px;cursor:pointer;font-weight:600}
.section-hd .right{display:flex;align-items:center;gap:10px}
.badge-update{font-size:11px;color:#0369a1;background:#e0f2fe;border:1px solid #bae6fd;border-radius:999px;padding:4px 8px}
.chev{transition:transform .18s}
.section.open .chev{transform:rotate(180deg)}
.section.camp.open{background:linear-gradient(#fff,#fff) padding-box,linear-gradient(90deg,#22c55e,#14b8a6,#0ea5e9,#22c55e) border-box;border:2px solid transparent;animation:flow 6s linear infinite}
@keyframes flow{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
.section-body{height:0;overflow:hidden}
.section.open .section-body{height:auto}
.collapsing{transition:height .35s ease}
.inner{padding:14px 16px 18px}

/* Grids */
.grid{display:grid;gap:14px}
.grid.cols-3{grid-template-columns:repeat(3,minmax(0,1fr))}
@media (max-width:1100px){.grid.cols-3{grid-template-columns:repeat(2,minmax(0,1fr))}}
@media (max-width:760px){.grid.cols-3{grid-template-columns:1fr}}

/* Cards */
.card{border:1px solid var(--border);border-radius:12px;background:#fff;padding:10px;display:flex;flex-direction:column;gap:8px;box-shadow:0 6px 18px rgba(2,6,23,.06)}
.card h4{margin:0;font-size:14px}
.card p{margin:0;font-size:12px;color:#6b7280}
.counter{display:flex;gap:6px;align-items:center;font-size:12px;color:#6b7280}
.eye{width:16px;height:16px}
.badge-new{font-size:10px;color:#047857;background:#d1fae5;border:1px solid #a7f3d0;border-radius:999px;padding:2px 6px}

/* Thumb + CTA central */
.thumb{position:relative;height:110px;border:1px dashed #cbd5e1;border-radius:10px;background:#f8fafc;display:grid;place-items:center;color:#64748b;overflow:hidden;box-shadow:inset 0 1px 6px rgba(2,6,23,.04)}
.cta{position:absolute;left:50%;top:50%;transform:translate(-50%,-50%);display:inline-flex;align-items:center;gap:8px;padding:9px 14px;border-radius:12px;border:1px solid var(--border);background:#ecfdf5;color:#065f46;font-weight:600;cursor:pointer;box-shadow:0 6px 16px rgba(16,185,129,.18)}
.cta:hover{filter:brightness(0.98)}

/* Overlay */
.overlay{position:absolute;left:24px;right:24px;bottom:24px;top:calc(24px + var(--ph));background:#fff;border:1px solid var(--border);border-radius:12px;box-shadow:0 20px 40px rgba(2,6,23,.18);display:none;flex-direction:column;overflow:auto;z-index:4}
.overlay.show{display:flex}
.viewer{padding:16px;display:flex;flex-direction:column;gap:16px}
.frame{background:#f8fafc;border:1px dashed #cbd5e1;border-radius:12px;min-height:60vh;display:grid;place-items:center;color:#64748b}
.frame iframe{width:100%;height:70vh;border:0;border-radius:12px}

/* Related */
.related{border-top:1px solid var(--border);padding-top:12px}
.related h4{margin:0 0 10px 0;font-size:14px}
.related .relgrid{display:grid;gap:12px;grid-template-columns:repeat(3,minmax(0,1fr))}
@media (max-width:1100px){.related .relgrid{grid-template-columns:repeat(2,minmax(0,1fr))}}
@media (max-width:760px){.related .relgrid{grid-template-columns:1fr}}
</style>
</head>
<body>
<header>
  <div class="logo"><div class="mark">S</div><div class="title">Sicredi</div></div>
</header>

<div class="layout">
  <aside class="sidebar" aria-label="Barra lateral">
    <div class="icon" title="Home">🏠</div>
    <div class="icon" title="Campanhas">🗂️</div>
    <div class="icon" title="Placares">📊</div>
    <div class="bottom"><div class="icon">⚙️</div></div>
  </aside>

  <main class="main">
    <section class="panel">
      <!-- header fixo -->
      <div class="panel-hd">
        <div class="hd-left">
          <h2 id="pageTitle">Dashboards</h2>
        </div>
        <!-- Direita: pesquisa por padrão; vira botão Voltar quando um dash abre -->
        <div id="rightSearch" class="search">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="7"></circle><line x1="21" y1="21" x2="16.65" y2="16.65"></line></svg>
          <input id="q" placeholder="Pesquisar por título…" oninput="filterAll()"/>
        </div>
        <button id="backCta" class="back-cta">← Voltar ao menu</button>
      </div>

      <div id="navbox" class="navbox"></div>
    </section>

    <!-- Overlay -->
    <div id="overlay" class="overlay">
      <div class="viewer">
        <div id="frameWrap" class="frame"><span>Carregue um relatório</span></div>
        <div class="related">
          <h4>Outros dashboards relacionados</h4>
          <div class="relgrid" id="relGrid"></div>
        </div>
      </div>
    </div>
  </main>
</div>

<script>
const DATA = {
  "Campanhas":[
    {id:"bola",title:"Bola na Rede",end:"31/12/2025",embedUrl:"https://app.powerbi.com/view?r=REPLACE_BOLA"},
    {id:"rio", title:"Viva o Rio",  end:"30/11/2025",embedUrl:"https://app.powerbi.com/view?r=REPLACE_RIO"}
  ],
  "Placares":[
    {id:"ag",title:"Placar Agência",flagUpdate:true,embedUrl:"https://app.powerbi.com/view?r=REPLACE_AG"},
    {id:"ge",title:"Placar Gestor", flagUpdate:true,embedUrl:"https://app.powerbi.com/view?r=REPLACE_GE"}
  ]
};

const nav = document.getElementById("navbox");
const pageTitle = document.getElementById("pageTitle");
const rightSearch = document.getElementById("rightSearch");
const backCta = document.getElementById("backCta");
let currentDash = null;

function getCount(id){ return Number(localStorage.getItem("access:"+id)||0); }

/* render menu */
function buildNav(filter=""){
  nav.innerHTML = "";
  const q = filter.toLowerCase().trim();

  Object.entries(DATA).forEach(([sec,items])=>{
    const list = items.filter(it=>{
      const txt = (it.title+" "+(it.end||"")).toLowerCase();
      return !q || txt.includes(q);
    });
    if(!list.length) return;

    const s = document.createElement("div");
    s.className = "section " + (sec==="Campanhas" ? "camp" : "");
    s.dataset.section = sec;

    const hd = document.createElement("div");
    hd.className="section-hd";
    hd.innerHTML = `<span>${sec}</span>
      <div class="right">${sec==="Placares"?`<span class="badge-update">Nova atualização</span>`:""}<span class="chev">▾</span></div>`;
    hd.addEventListener("click",()=>toggleSection(s));
    s.appendChild(hd);

    const body = document.createElement("div"); body.className="section-body collapsing";
    const inner = document.createElement("div"); inner.className="inner";
    const grid = document.createElement("div"); grid.className = "grid cols-3";

    list.forEach(it=>{
      const card = document.createElement("div");
      card.className = "card" + (sec==="Campanhas" ? " glow" : "");
      card.innerHTML = `
        <div style="display:flex;justify-content:space-between;align-items:center">
          <h4>${it.title}</h4>
          ${sec==="Campanhas"?`<span class="badge-new">Novo</span>`:""}
          ${sec!=="Campanhas"&&it.flagUpdate?`<span class="badge-update">Nova atualização</span>`:""}
        </div>
        ${sec==="Campanhas"?`<p>Data fim: ${it.end}</p>`:""}
        <div class="thumb">
          <span>Prévia</span>
          <button class="cta" onclick="openDash('${sec}','${it.id}')">Abrir</button>
        </div>
        <div class="counter">
          <svg class="eye" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M1 12s4-7 11-7 11 7 11 7-4 7-11 7-11-7-11-7Z"/><circle cx="12" cy="12" r="3"/></svg>
          <span id="c-${it.id}">Acessos: ${getCount(it.id)}</span>
        </div>
      `;
      grid.appendChild(card);
    });

    inner.appendChild(grid); body.appendChild(inner); s.appendChild(body); nav.appendChild(s);
    if (sec === "Campanhas") requestAnimationFrame(()=>openSection(s,false));
  });
}

/* collapse animation */
function toggleSection(el){ el.classList.contains("open")?closeSection(el):openSection(el,true); }
function openSection(el, animate=true){
  const b = el.querySelector(".section-body");
  const prev = b.getBoundingClientRect().height;
  b.style.height = "auto";
  const target = b.getBoundingClientRect().height;
  b.style.height = prev+"px";
  if (animate) requestAnimationFrame(()=>b.style.height = target+"px");
  else b.style.height = target+"px";
  el.classList.add("open");
}
function closeSection(el){
  const b = el.querySelector(".section-body");
  const prev = b.getBoundingClientRect().height;
  b.style.height = prev+"px";
  requestAnimationFrame(()=>b.style.height = "0px");
  el.classList.remove("open");
}

/* relacionados: inclui o que estava aberto (swap) */
function buildRelated(newItem){
  const rel = document.getElementById("relGrid"); rel.innerHTML = "";
  const pool = [];
  if (currentDash && currentDash.id !== newItem.id) pool.push(currentDash);
  const sug1 = DATA["Campanhas"].find(x=>x.id==="rio");
  const sug2 = DATA["Placares"].find(x=>x.id==="ag");
  [sug1,sug2].forEach(x=>{ if(x && !pool.some(p=>p.id===x.id) && x.id!==newItem.id) pool.push(x); });
  pool.slice(0,3).forEach(r=>{
    const secRel = Object.keys(DATA).find(k=>DATA[k].some(i=>i.id===r.id));
    const c = document.createElement("div"); c.className="card";
    c.innerHTML = `<h4>${r.title}</h4>${r.end?`<p>Data fim: ${r.end}</p>`:""}
      <div class="thumb"><span>Prévia</span>
      <button class="cta" onclick="openDash('${secRel}','${r.id}')">Abrir</button></div>`;
    rel.appendChild(c);
  });
}

/* open dash */
function openDash(section, id){
  const item = (DATA[section]||[]).find(i=>i.id===id); if(!item) return;

  // título: só o nome do dash
  pageTitle.textContent = item.title;

  // trocar pesquisa pelo botão voltar verde animado
  rightSearch.style.display = "none";
  backCta.style.display = "inline-flex";
  backCta.classList.add("anim");
  backCta.onclick = backToMenu;

  // iframe
  const fw = document.getElementById("frameWrap"); fw.innerHTML = "";
  const ifr = document.createElement("iframe"); ifr.src = item.embedUrl || "about:blank"; ifr.loading="lazy";
  fw.appendChild(ifr);

  // relacionados e swap
  buildRelated(item);
  currentDash = item;

  // contador
  const key = "access:"+id; const n = getCount(id)+1; localStorage.setItem(key, n);
  const cardC = document.getElementById("c-"+id); if(cardC) cardC.textContent = "Acessos: "+n;

  document.getElementById("overlay").classList.add("show");
}

/* voltar */
function backToMenu(){
  document.getElementById("overlay").classList.remove("show");
  pageTitle.textContent = "Dashboards";
  backCta.style.display = "none";
  backCta.classList.remove("anim");
  rightSearch.style.display = "block";
}

/* search */
function filterAll(){ buildNav(document.getElementById("q").value); }

/* init */
buildNav();
</script>
</body>
</html>

