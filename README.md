5
<img width="1912" height="921" alt="image" src="https://github.com/user-attachments/assets/f26ae8a7-a164-4df6-abd7-f70b8fa46274" />
   

<!doctype html>
<html lang="pt-BR">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Sicredi • Navegação Vertical</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
<style>
:root{--bg:#EFF3F8;--card:#fff;--muted:#6b7280;--border:#E4E6EB;--accent:#10b981;--ink:#0f172a}
*{box-sizing:border-box} body{margin:0;font-family:Inter,ui-sans-serif,-apple-system,"Segoe UI",Roboto;color:var(--ink);background:var(--bg)}

header{display:flex;justify-content:space-between;align-items:center;padding:12px 24px;background:#fff;border-bottom:1px solid var(--border)}
.logo{display:flex;align-items:center;gap:10px}.logo .mark{width:28px;height:28px;border-radius:999px;background:var(--accent);display:grid;place-items:center;color:#fff;font-weight:700}
.logo .title{color:#059669;font-weight:700}

.layout{display:flex;min-height:calc(100vh - 58px)}
nav{width:300px;background:#fff;border-right:1px solid var(--border);display:flex;flex-direction:column;gap:8px;padding:16px}
.nav-search{position:relative;margin-bottom:6px}
.nav-search input{width:100%;padding:9px 12px 9px 36px;border:1px solid var(--border);border-radius:10px;font-size:14px}
.nav-search svg{position:absolute;left:10px;top:50%;transform:translateY(-50%);width:16px;height:16px;color:#94a3b8}

.section{border:1px solid var(--border);border-radius:12px;overflow:hidden}
.section summary{list-style:none;cursor:pointer;display:flex;justify-content:space-between;align-items:center;padding:12px 14px;font-weight:600}
.section[open] summary{background:#f8fafc}
.chev{transition:transform .15s}.section[open] .chev{transform:rotate(180deg)}

.section-body{padding:10px 12px 14px;display:flex;flex-direction:column;gap:10px}
.dash{border:1px solid var(--border);border-radius:10px;padding:10px;background:#fff;display:flex;flex-direction:column;gap:8px}
.dash-title{font-size:14px;font-weight:600;margin:0}
.dash-desc{font-size:12px;color:var(--muted);margin:0}
.tags{display:flex;gap:6px;flex-wrap:wrap}
.tag{padding:3px 8px;border:1px solid var(--border);border-radius:999px;font-size:12px;color:#065f46;background:#ecfdf5;cursor:pointer}
.tag.hidden-tag{display:none}
.toggle-tags{font-size:12px;color:#0ea5e9;cursor:pointer}
.open-btn{align-self:flex-start;border:1px solid var(--border);border-radius:10px;background:#ecfdf5;color:#065f46;padding:6px 10px;cursor:pointer}
.counter{font-size:12px;color:#6b7280}

main{flex:1;padding:24px}
.card{border:1px solid var(--border);background:#fff;border-radius:12px;box-shadow:0 1px 2px rgba(2,6,23,.04);overflow:hidden}
.card-header{display:flex;align-items:center;justify-content:space-between;padding:16px 20px;border-bottom:1px solid var(--border)}
.card-header h2{margin:0;font-size:18px}
.viewer{padding:18px}
.frame{background:#f8fafc;border:1px dashed #cbd5e1;border-radius:12px;height:520px;display:grid;place-items:center;color:#64748b}
.frame iframe{width:100%;height:100%;border:0;border-radius:12px}
</style>
</head>
<body>
<header>
  <div class="logo"><div class="mark">S</div><div class="title">Sicredi</div></div>
</header>

<div class="layout">
  <nav>
    <div class="nav-search">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="7"></circle><line x1="21" y1="21" x2="16.65" y2="16.65"></line></svg>
      <input id="navQuery" placeholder="Pesquisar tags/título…" oninput="applyNavFilter()">
    </div>
    <div id="navSections"></div>
  </nav>

  <main>
    <section class="card">
      <div class="card-header"><h2 id="dashTitle">Selecione um dashboard</h2><div id="dashCounter" class="counter"></div></div>
      <div class="viewer">
        <div id="frameWrap" class="frame"><span>Nenhum dash aberto</span></div>
        <div id="dashTags" class="tags" style="margin-top:12px"></div>
      </div>
    </section>
  </main>
</div>

<script>
const DATA = {
  "Campanhas":[
    {id:"bola",title:"Bola na Rede",desc:"Campanha bola na rede",embedUrl:"https://app.powerbi.com/view?r=REPLACE_BOLA",tags:{main:["campanhas","bola"],hidden:["rede","marketing"]}},
    {id:"rio",title:"Viva o Rio",desc:"Campanha Viva o Rio",embedUrl:"https://app.powerbi.com/view?r=REPLACE_RIO",tags:{main:["campanhas","rio"],hidden:["evento","captação"]}}
  ],
  "Placares":[
    {id:"agencia",title:"Placar Agência",desc:"Por agências",embedUrl:"https://app.powerbi.com/view?r=REPLACE_AG",tags:{main:["placar","agência"],hidden:["meta"]}},
    {id:"gestor",title:"Placar Gestor",desc:"Por gestor",embedUrl:"https://app.powerbi.com/view?r=REPLACE_GE",tags:{main:["placar","gestor"],hidden:["produtividade"]}},
    {id:"analitico",title:"Placar Analítico",desc:"Visão analítica",embedUrl:"https://app.powerbi.com/view?r=REPLACE_AN",tags:{main:["placar","analítico"],hidden:["drill"]}}
  ]
};

function renderNav(filter=""){
  const nav=document.getElementById("navSections");nav.innerHTML="";
  const q=filter.toLowerCase();
  Object.entries(DATA).forEach(([sec,items])=>{
    const group=items.filter(it=>{
      const all=[...it.tags.main,...it.tags.hidden].join(" ").toLowerCase();
      return !q||all.includes(q)||it.title.toLowerCase().includes(q)||it.desc.toLowerCase().includes(q);
    });
    if(!group.length)return;
    const det=document.createElement("details");det.className="section";det.open=true;
    det.innerHTML=`<summary>${sec}<span class="chev">▾</span></summary>`;
    const body=document.createElement("div");body.className="section-body";
    group.forEach(it=>{
      const div=document.createElement("div");div.className="dash";
      div.innerHTML=`<h4 class="dash-title">${it.title}</h4>
        <p class="dash-desc">${it.desc}</p>
        <div class="tags">
          ${it.tags.main.map(t=>`<span class="tag" onclick="openByTag('${t}')">#${t}</span>`).join("")}
          ${it.tags.hidden.map(t=>`<span class="tag hidden-tag" onclick="openByTag('${t}')">#${t}</span>`).join("")}
          <span class="toggle-tags" onclick="toggleHidden(this)">+ ver mais</span>
        </div>
        <button class="open-btn" onclick="openDash('${sec}','${it.id}')">Abrir</button>
        <div class="counter" id="c-${it.id}">Acessos: ${getCount(it.id)}</div>`;
      body.appendChild(div);
    });
    det.appendChild(body);nav.appendChild(det);
  });
}
function toggleHidden(el){
  const hidden=el.parentElement.querySelectorAll(".hidden-tag");
  const show=hidden[0]?.style.display==="inline-flex";
  hidden.forEach(t=>t.style.display=show?"none":"inline-flex");
  el.textContent=show?"+ ver mais":"− ver menos";
}
function openByTag(tag){document.getElementById("navQuery").value=tag;applyNavFilter();}
function applyNavFilter(){renderNav(document.getElementById("navQuery").value);}
function getCount(id){return Number(localStorage.getItem("access:"+id)||0);}
function openDash(sec,id){
  const item=DATA[sec].find(i=>i.id===id);if(!item)return;
  document.getElementById("dashTitle").textContent=item.title;
  const wrap=document.getElementById("frameWrap");wrap.innerHTML="";
  const ifr=document.createElement("iframe");ifr.src=item.embedUrl;wrap.appendChild(ifr);
  const k="access:"+id;const n=getCount(id)+1;localStorage.setItem(k,n);document.getElementById("dashCounter").textContent="Acessos: "+n;document.getElementById("c-"+id).textContent="Acessos: "+n;
  document.getElementById("dashTags").innerHTML=[...item.tags.main,...item.tags.hidden].map(t=>`<span class="tag" onclick="openByTag('${t}')">#${t}</span>`).join("");
}
renderNav();
</script>
</body>
</html>













kkkkkkkllllll

<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>App - Categorias</title>
  <style>
    :root{
      --bg:#EFF3F8; --card:#FFFFFF; --muted:#5a5a5a; --border:#E1E0E4; --accent:#10b981;
    }
    *{box-sizing:border-box}
    body{margin:0;font-family:Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;color:#0f172a;background:var(--bg);}
    .container{min-height:100vh;display:flex;flex-direction:column;}
/* borda esquerda arredondada + sombra no header */
header {
  position: relative;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 24px;
  border-bottom: 1px solid var(--border);
  background: var(--card);
  border-top-left-radius: 12px;
  border-bottom-left-radius: 12px;
  box-shadow: 10px 0 30px rgba(2,6,23,0.08); /* sombra à esquerda mais suave */
  overflow: visible;
}
    .logo{display:flex;align-items:center;gap:12px}
    .logo .mark{width:36px;height:36px;border-radius:8px;background:#e6eef6;display:flex;align-items:center;justify-content:center;font-weight:700;color:#0f172a}
    .top-right{display:flex;align-items:center;gap:16px}
    .top-avatar{display:flex;align-items:center;gap:10px}
    .avatar{width:40px;height:40px;border-radius:999px;background:var(--accent);color:#fff;display:flex;align-items:center;justify-content:center;font-weight:700}
    .avatar .meta{font-size:13px;line-height:1}
    .meta .name{font-weight:600}
    .meta .sub{color:var(--muted);font-size:12px}

    .layout{display:flex;flex:1}
    aside.sidebar{width:60px;border-right:1px solid var(--border);background:var(--card);padding:12px 8px;display:flex;flex-direction:column;align-items:center;gap:8px}
    .icon{width:36px;height:36px;border-radius:10px;display:flex;align-items:center;justify-content:center;color:#475569;cursor:pointer;transition:box-shadow .18s, transform .12s}
    .icon:hover{color:var(--accent); transform:translateY(-2px); box-shadow: 0 6px 18px rgba(2,6,23,0.08)}
    .sidebar .bottom{margin-top:auto;margin-bottom:12px}

    main{flex:1;padding:24px}
    .card{border-radius:12px;border:1px solid var(--border);background:var(--card);box-shadow:0 1px 2px rgba(2,6,23,0.04)}
    .card .card-header{display:flex;align-items:center;justify-content:space-between;padding:16px 20px;border-bottom:1px solid var(--border)}
    .card h2{margin:0;font-size:16px}
    .search-wrap{position:relative}
    .search-wrap input{padding:8px 12px 8px 36px;width:240px;border-radius:10px;border:1px solid var(--border);outline:none;font-size:14px}
    .search-wrap svg{position:absolute;left:10px;top:50%;transform:translateY(-50%);width:16px;height:16px;color:var(--muted)}

    .card .card-body{padding:16px 20px}
    .results{border-radius:12px;border:1px solid var(--border);background:var(--card);min-height:120px;box-shadow: 0 8px 24px rgba(2,6,23,0.06); padding:0; overflow:hidden}
    .results .empty{padding:40px;text-align:center;color:var(--muted);font-size:14px}

    .pagination{padding:12px;border-top:1px solid var(--border);display:flex;justify-content:center}
    .pagination .dots{display:inline-flex;gap:8px;align-items:center}
    .dot{width:8px;height:8px;border-radius:999px;background:#cbd5e1}
    .pg-btn{background:none;border:none;color:var(--muted);cursor:pointer;padding:6px 8px;border-radius:6px}
    .pg-btn:hover{color:var(--accent)}
    @media (max-width:720px){
      .search-wrap input{width:160px}
    }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <div class="logo">
        <div class="mark">S</div>
        <div class="title">Sicredi</div>
      </div>

      <div class="top-right">
        




mmmm



<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>App - Categorias</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg:#EFF3F8; --card:#FFFFFF; --muted:#6b7280; --border:#E4E6EB; --accent:#10b981; --ink:#0f172a;
    }
    *{box-sizing:border-box}
    body{margin:0;font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,"Helvetica Neue",Arial;color:var(--ink);background:var(--bg)}
    .container{min-height:100vh;display:flex;flex-direction:column}

    /* TOP BAR */
    header{
      position:sticky;top:0;z-index:10;
      display:flex;justify-content:space-between;align-items:center;
      padding:12px 24px;background:var(--card);border-bottom:1px solid var(--border)
    }
    .logo{display:flex;align-items:center;gap:10px}
    .logo .mark{width:28px;height:28px;border-radius:999px;background:var(--accent);display:grid;place-items:center;color:#fff;font-weight:700}
    .logo .title{color:#0ea5e9; color:#059669; font-weight:700; letter-spacing:.2px}
    .top-right{display:flex;align-items:center;gap:16px}
    .bell, .searchTop{width:36px;height:36px;border-radius:999px;display:grid;place-items:center;color:#6b7280;background:#f8fafc;border:1px solid var(--border)}
    .top-avatar{display:flex;align-items:center;gap:10px}
    .avatar{width:36px;height:36px;border-radius:999px;background:#e2e8f0;color:#334155;display:grid;place-items:center;font-weight:700}
    .meta{font-size:12px;line-height:1}
    .meta .name{font-weight:600;font-size:13px}
    .meta .sub{color:var(--muted)}

    /* LAYOUT */
    .layout{display:flex;flex:1;min-height:0}
    aside.sidebar{
      width:60px;border-right:1px solid var(--border);background:var(--card);
      padding:12px 8px;display:flex;flex-direction:column;align-items:center;gap:8px
    }
    .icon{
      width:40px;height:40px;border-radius:12px;display:grid;place-items:center;
      color:#667085;cursor:pointer;transition:transform .12s,color .12s,box-shadow .18s
    }
    .icon:hover{color:var(--accent);transform:translateY(-2px);box-shadow:0 8px 24px rgba(2,6,23,.08)}
    .sidebar .bottom{margin-top:auto;margin-bottom:8px}

    main{flex:1;padding:24px;min-width:0}

    /* CARD */
    .card{border:1px solid var(--border);background:var(--card);border-radius:12px;box-shadow:0 1px 2px rgba(2,6,23,.04)}
    .card-header{display:flex;align-items:center;justify-content:space-between;padding:16px 20px;border-bottom:1px solid var(--border)}
    .card-header h2{margin:0;font-size:18px}

    .search-wrap{position:relative}
    .search-wrap input{
      width:260px;padding:9px 12px 9px 36px;border:1px solid var(--border);border-radius:10px;font-size:14px;outline:none;
      background:#fff
    }
    .search-wrap input:focus{border-color:#a7f3d0;box-shadow:0 0 0 3px rgba(16,185,129,.15)}
    .search-wrap svg{position:absolute;left:10px;top:50%;transform:translateY(-50%);width:16px;height:16px;color:#94a3b8}

    /* TABLE AREA (vazia) */
    .inner{
      margin:16px 20px;border:1px solid var(--border);border-radius:12px;overflow:hidden;background:#fff;
      box-shadow:0 8px 24px rgba(2,6,23,.06)
    }
    .empty{
      padding:28px;color:var(--muted);font-size:14px
    }
    .pagination{
      padding:12px;border-top:1px solid var(--border);display:flex;justify-content:center;align-items:center;gap:18px;color:#94a3b8;font-size:12px
    }
    .pg-btn{background:none;border:none;color:#9aa3b2;cursor:pointer;padding:6px 8px;border-radius:6px}
    .pg-btn:hover{color:var(--accent)}
    .dots{display:inline-flex;gap:10px}
    .dot{width:6px;height:6px;border-radius:999px;background:#cbd5e1}

    @media (max-width:720px){ .search-wrap input{width:170px} }
  </style>
</head>
<body>
  <div class="container">
    <!-- TOP -->
    <header>
      <div class="logo">
        <div class="mark">S</div>
        <div class="title">Sicredi</div>
      </div>

      <div class="top-right">
        <div class="searchTop" aria-hidden>🔍</div>
        <div class="top-avatar">
          <div class="avatar">RG</div>
          <div class="meta">
            <div class="name">Rafael G.</div>
            <div class="sub">Agência 01</div>
          </div>
        </div>
        <div class="bell" aria-hidden>🔔</div>
      </div>
    </header>

    <!-- BODY -->
    <div class="layout">
      <!-- SIDEBAR -->
      <aside class="sidebar" role="navigation" aria-label="Barra lateral">
        <div class="icon" title="Home">🏠</div>
        <div class="icon" title="Grid">🗂️</div>
        <div class="icon" title="Settings">⚙️</div>
        <div class="icon" title="Messages">💬</div>
        <div class="icon" title="Video">🎥</div>
        <div class="icon" title="List">📋</div>
        <div class="icon" title="Search">🔎</div>
        <div class="icon" title="Store">🛒</div>
        <div class="icon" title="Users">👥</div>
        <div class="icon" title="Headphones">🎧</div>
        <div class="bottom"><div class="icon" title="Analytics">📊</div></div>
      </aside>

      <!-- MAIN -->
      <main>
        <div class="card">
          <div class="card-header">
            <h2>Categorias</h2>
            <div class="search-wrap">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden>
                <circle cx="11" cy="11" r="7"></circle>
                <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
              </svg>
              <input placeholder="Procurar..." />
            </div>
          </div>

          <div class="inner">
            <div class="empty">Sem resultados encontrados</div>
            <div class="pagination">
              <button class="pg-btn" aria-label="Primeira">«</button>
              <button class="pg-btn" aria-label="Anterior">‹</button>
              <span class="dots"><span class="dot"></span><span class="dot"></span><span class="dot"></span></span>
              <button class="pg-btn" aria-label="Próxima">›</button>
              <button class="pg-btn" aria-label="Última">»</button>
            </div>
          </div>
        </div>
      </main>
    </div>
  </div>
</body>
</html>



kkkkk



<!doctype html>
<html lang="pt-BR">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Sicredi • Mock</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#EFF3F8; --card:#FFFFFF; --muted:#6b7280; --border:#E4E6EB; --accent:#10b981; --ink:#0f172a;
}
*{box-sizing:border-box} body{margin:0;font-family:Inter,ui-sans-serif,-apple-system,"Segoe UI",Roboto;color:var(--ink);background:var(--bg)}
.container{min-height:100vh;display:flex;flex-direction:column}

/* TOP */
header{display:flex;justify-content:space-between;align-items:center;padding:12px 24px;background:#fff;border-bottom:1px solid var(--border)}
.logo{display:flex;align-items:center;gap:10px}
.logo .mark{width:28px;height:28px;border-radius:999px;background:var(--accent);display:grid;place-items:center;color:#fff;font-weight:700}
.logo .title{color:#059669;font-weight:700;letter-spacing:.2px}
.top-right{display:flex;align-items:center;gap:16px}
.pill{width:36px;height:36px;border-radius:999px;display:grid;place-items:center;color:#6b7280;background:#f8fafc;border:1px solid var(--border)}
.top-avatar{display:flex;align-items:center;gap:10px}
.avatar{width:36px;height:36px;border-radius:999px;background:#e2e8f0;color:#334155;display:grid;place-items:center;font-weight:700}
.meta{font-size:12px;line-height:1}.meta .name{font-weight:600;font-size:13px}.meta .sub{color:var(--muted)}

/* LAYOUT */
.layout{display:flex;flex:1;min-height:0}
aside.sidebar{width:60px;border-right:1px solid var(--border);background:#fff;padding:12px 8px;display:flex;flex-direction:column;align-items:center;gap:8px}
.icon{width:40px;height:40px;border-radius:12px;display:grid;place-items:center;color:#667085;cursor:pointer;transition:transform .12s,color .12s,box-shadow .18s}
.icon:hover{color:var(--accent);transform:translateY(-2px);box-shadow:0 8px 24px rgba(2,6,23,.08)}
.sidebar .bottom{margin-top:auto;margin-bottom:8px}
main{flex:1;padding:24px;min-width:0}

/* CARD */
.card{border:1px solid var(--border);background:#fff;border-radius:12px;box-shadow:0 1px 2px rgba(2,6,23,.04)}
.card-header{display:flex;align-items:center;justify-content:space-between;padding:16px 20px;border-bottom:1px solid var(--border)}
.card-header h2{margin:0;font-size:18px}
.search-wrap{position:relative}
.search-wrap input{width:260px;padding:9px 12px 9px 36px;border:1px solid var(--border);border-radius:10px;font-size:14px;outline:none}
.search-wrap input:focus{border-color:#a7f3d0;box-shadow:0 0 0 3px rgba(16,185,129,.15)}
.search-wrap svg{position:absolute;left:10px;top:50%;transform:translateY(-50%);width:16px;height:16px;color:#94a3b8}

.grid{display:grid;gap:18px;padding:18px}
.grid.cols-2{grid-template-columns:repeat(2,minmax(0,1fr))}
.grid.cols-3{grid-template-columns:repeat(3,minmax(0,1fr))}
.tile{border:1px solid var(--border);border-radius:12px;background:#fff;padding:16px}
.tile h3{margin:0 0 8px;font-size:15px}
.tile p{margin:0 0 16px;font-size:12px;color:var(--muted)}
.btn{display:inline-flex;align-items:center;gap:8px;padding:8px 12px;border-radius:10px;border:1px solid var(--border);background:#fff;cursor:pointer}
.btn.primary{border-color:#34d399;background:#ecfdf5;color:#065f46}
.btn:hover{filter:brightness(0.98)}
.back{display:inline-flex;align-items:center;gap:6px;color:#64748b;font-size:14px;cursor:pointer}
.hidden{display:none}

.pagination{padding:12px;border-top:1px solid var(--border);display:flex;justify-content:center;align-items:center;gap:18px;color:#94a3b8;font-size:12px}
.pg-btn{background:none;border:none;color:#9aa3b2;cursor:pointer;padding:6px 8px;border-radius:6px}
.pg-btn:hover{color:var(--accent)}
.dots{display:inline-flex;gap:10px}.dot{width:6px;height:6px;border-radius:999px;background:#cbd5e1}

@media (max-width:900px){ .grid.cols-3{grid-template-columns:1fr} .grid.cols-2{grid-template-columns:1fr} .search-wrap input{width:170px} }
</style>
</head>
<body>
<div class="container">
  <!-- TOP -->
  <header>
    <div class="logo"><div class="mark">S</div><div class="title">Sicredi</div></div>
    <div class="top-right">
      <div class="pill">🔍</div>
      <div class="top-avatar">
        <div class="avatar">RG</div>
        <div class="meta"><div class="name">Rafael G.</div><div class="sub">Agência 01</div></div>
      </div>
      <div class="pill">🔔</div>
    </div>
  </header>

  <div class="layout">
    <!-- SIDEBAR -->
    <aside class="sidebar" role="navigation" aria-label="Barra lateral">
      <div class="icon" onclick="go('home')" title="Home">🏠</div>
      <div class="icon" onclick="go('campanhas')" title="Campanhas">🗂️</div>
      <div class="icon" onclick="go('dashboards')" title="Dashboards">📊</div>
      <div class="bottom"><div class="icon">⚙️</div></div>
    </aside>

    <!-- MAIN -->
    <main>
      <!-- HOME -->
      <section id="view-home" class="card">
        <div class="card-header">
          <h2>Categorias</h2>
          <div class="search-wrap">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden>
              <circle cx="11" cy="11" r="7"></circle>
              <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
            </svg>
            <input placeholder="Procurar..." />
          </div>
        </div>

        <div class="grid cols-2">
          <!-- Card Campanha -->
          <div class="tile">
            <small class="sub">Minítitulo</small>
            <h3>Campanha</h3>
            <p>Listas e dashboards de campanhas.</p>
            <button class="btn primary" onclick="go('campanhas')">Acessar</button>
          </div>

          <!-- Card Dashboards -->
          <div class="tile">
            <small class="sub">Minítitulo</small>
            <h3>Dashboards</h3>
            <p>Acesso rápido aos dashboards 1, 2 e 3.</p>
            <button class="btn primary" onclick="go('dashboards')">Acessar</button>
          </div>
        </div>

        <div class="pagination">
          <button class="pg-btn">«</button><button class="pg-btn">‹</button>
          <span class="dots"><span class="dot"></span><span class="dot"></span><span class="dot"></span></span>
          <button class="pg-btn">›</button><button class="pg-btn">»</button>
        </div>
      </section>

      <!-- CAMPANHAS -->
      <section id="view-campanhas" class="card hidden">
        <div class="card-header">
          <div class="back" onclick="go('home')">← Voltar</div>
          <h2 style="margin-left:8px">Campanhas</h2>
          <div class="search-wrap">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden>
              <circle cx="11" cy="11" r="7"></circle>
              <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
            </svg>
            <input placeholder="Procurar..." />
          </div>
        </div>

        <div class="grid cols-2">
          <div class="tile"><h3>Bola na Rede</h3><p>Campanha Bola na Rede</p><button class="btn" onclick="go('dashboards')">Abrir</button></div>
          <div class="tile"><h3>Viva o Rio</h3><p>Campanha Viva o Rio</p><button class="btn" onclick="go('dashboards')">Abrir</button></div>
          <div class="tile"><h3>Feirão</h3><p>Ofertas especiais</p><button class="btn" onclick="go('dashboards')">Abrir</button></div>
          <div class="tile"><h3>Associados+</h3><p>Relacionamento</p><button class="btn" onclick="go('dashboards')">Abrir</button></div>
        </div>

        <div class="pagination">
          <button class="pg-btn">«</button><button class="pg-btn">‹</button>
          <span class="dots"><span class="dot"></span><span class="dot"></span><span class="dot"></span></span>
          <button class="pg-btn">›</button><button class="pg-btn">»</button>
        </div>
      </section>

      <!-- DASHBOARDS -->
      <section id="view-dashboards" class="card hidden">
        <div class="card-header">
          <div class="back" onclick="go('campanhas')">← Voltar</div>
          <h2 style="margin-left:8px">Dashboards</h2>
          <div class="search-wrap">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden>
              <circle cx="11" cy="11" r="7"></circle>
              <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
            </svg>
            <input placeholder="Procurar..." />
          </div>
        </div>

        <div class="grid cols-3">
          <div class="tile">
            <h3>Dash 1</h3><p>Resumo operacional</p>
            <button class="btn primary" onclick="openDash(1)">Abrir</button>
          </div>
          <div class="tile">
            <h3>Dash 2</h3><p>Agências e gerentes</p>
            <button class="btn primary" onclick="openDash(2)">Abrir</button>
          </div>
          <div class="tile">
            <h3>Dash 3</h3><p>Analítico</p>
            <button class="btn primary" onclick="openDash(3)">Abrir</button>
          </div>
        </div>

        <div class="pagination">
          <button class="pg-btn">«</button><button class="pg-btn">‹</button>
          <span class="dots"><span class="dot"></span><span class="dot"></span><span class="dot"></span></span>
          <button class="pg-btn">›</button><button class="pg-btn">»</button>
        </div>
      </section>

      <!-- DASH DETAIL -->
      <section id="view-dash-detail" class="card hidden">
        <div class="card-header">
          <div class="back" onclick="go('dashboards')">← Voltar</div>
          <h2 id="dash-title" style="margin-left:8px">Dash</h2>
          <div style="width:260px"></div>
        </div>
        <div class="grid cols-2">
          <div class="tile"><h3>Visão</h3><p>Iframe/BI aqui.</p><button class="btn">Placeholder</button></div>
          <div class="tile"><h3>KPIs</h3><p>Métricas chave.</p><button class="btn">Placeholder</button></div>
        </div>
      </section>

    </main>
  </div>
</div>

<script>
const views = ["home","campanhas","dashboards","dash-detail"];
function go(name){
  views.forEach(v=>document.getElementById(`view-${v}`).classList.add("hidden"));
  document.getElementById(`view-${name}`).classList.remove("hidden");
  history.replaceState({}, "", `#${name}`);
}
function openDash(n){
  document.getElementById("dash-title").textContent = `Dash ${n}`;
  go("dash-detail");
}
window.addEventListener("load",()=>{
  const route = location.hash.replace("#","");
  go(views.includes(route)?route:"home");
});
</script>
</body>
</html>



llllllll



<!doctype html>
<html lang="pt-BR">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Sicredi • Mock</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
<style>
:root{--bg:#EFF3F8;--card:#fff;--muted:#6b7280;--border:#E4E6EB;--accent:#10b981;--ink:#0f172a}
*{box-sizing:border-box} body{margin:0;font-family:Inter,ui-sans-serif,-apple-system,"Segoe UI",Roboto;color:var(--ink);background:var(--bg)}
.container{min-height:100vh;display:flex;flex-direction:column}

/* TOP */
header{display:flex;justify-content:space-between;align-items:center;padding:12px 24px;background:#fff;border-bottom:1px solid var(--border)}
.logo{display:flex;align-items:center;gap:10px}.logo .mark{width:28px;height:28px;border-radius:999px;background:var(--accent);display:grid;place-items:center;color:#fff;font-weight:700}
.logo .title{color:#059669;font-weight:700}
.top-right{display:flex;align-items:center;gap:16px}.pill{width:36px;height:36px;border-radius:999px;display:grid;place-items:center;color:#6b7280;background:#f8fafc;border:1px solid var(--border)}
.top-avatar{display:flex;align-items:center;gap:10px}.avatar{width:36px;height:36px;border-radius:999px;background:#e2e8f0;color:#334155;display:grid;place-items:center;font-weight:700}
.meta{font-size:12px;line-height:1}.meta .name{font-weight:600;font-size:13px}.meta .sub{color:var(--muted)}

/* LAYOUT */
.layout{display:flex;flex:1;min-height:0}
aside.sidebar{width:60px;border-right:1px solid var(--border);background:#fff;padding:12px 8px;display:flex;flex-direction:column;align-items:center;gap:8px}
.icon{width:40px;height:40px;border-radius:12px;display:grid;place-items:center;color:#667085;cursor:pointer;transition:transform .12s,color .12s,box-shadow .18s}
.icon:hover{color:var(--accent);transform:translateY(-2px);box-shadow:0 8px 24px rgba(2,6,23,.08)}
.sidebar .bottom{margin-top:auto;margin-bottom:8px}
main{flex:1;padding:24px;min-width:0}

/* CARD + GRID */
.card{border:1px solid var(--border);background:#fff;border-radius:12px;box-shadow:0 1px 2px rgba(2,6,23,.04)}
.card-header{display:flex;align-items:center;justify-content:space-between;padding:16px 20px;border-bottom:1px solid var(--border)}
.card-header h2{margin:0;font-size:18px}
.search-wrap{position:relative}
.search-wrap input{width:260px;padding:9px 12px 9px 36px;border:1px solid var(--border);border-radius:10px;font-size:14px;outline:none}
.search-wrap input:focus{border-color:#a7f3d0;box-shadow:0 0 0 3px rgba(16,185,129,.15)}
.search-wrap svg{position:absolute;left:10px;top:50%;transform:translateY(-50%);width:16px;height:16px;color:#94a3b8}
.tag-suggest{position:absolute;top:42px;right:0;left:0;background:#fff;border:1px solid var(--border);border-radius:10px;padding:8px;display:none;gap:6px;flex-wrap:wrap;z-index:2}
.tag{display:inline-flex;gap:6px;align-items:center;padding:4px 8px;border:1px solid var(--border);border-radius:999px;font-size:12px;color:#065f46;background:#ecfdf5;cursor:pointer}
.grid{display:grid;gap:18px;padding:18px}
.grid.cols-2{grid-template-columns:repeat(2,minmax(0,1fr))}
.grid.cols-3{grid-template-columns:repeat(3,minmax(0,1fr))}
.tile{border:1px solid var(--border);border-radius:12px;background:#fff;padding:16px}
.tile h3{margin:0 0 6px;font-size:15px}
.tile p{margin:0 0 12px;font-size:12px;color:var(--muted)}
.tags{display:flex;gap:6px;flex-wrap:wrap;margin-top:6px}
.btn{display:inline-flex;align-items:center;gap:8px;padding:8px 12px;border-radius:10px;border:1px solid var(--border);background:#fff;cursor:pointer}
.btn.primary{border-color:#34d399;background:#ecfdf5;color:#065f46}
.counter{margin-top:8px;font-size:12px;color:#6b7280}
.back{display:inline-flex;align-items:center;gap:6px;color:#64748b;font-size:14px;cursor:pointer}
.hidden{display:none}

/* Detail */
.viewer{padding:18px}
.frame{background:#f8fafc;border:1px dashed #cbd5e1;border-radius:12px;height:480px;display:grid;place-items:center;color:#64748b}
.frame iframe{width:100%;height:100%;border:0;border-radius:12px}

/* Pagination */
.pagination{padding:12px;border-top:1px solid var(--border);display:flex;justify-content:center;align-items:center;gap:18px;color:#94a3b8;font-size:12px}
.pg-btn{background:none;border:none;color:#9aa3b2;cursor:pointer;padding:6px 8px;border-radius:6px}
.pg-btn:hover{color:var(--accent)} .dots{display:inline-flex;gap:10px}.dot{width:6px;height:6px;border-radius:999px;background:#cbd5e1}

@media (max-width:900px){.grid.cols-3,.grid.cols-2{grid-template-columns:1fr}.search-wrap input{width:170px}}
</style>
</head>
<body>
<div class="container">
  <!-- TOP -->
  <header>
    <div class="logo"><div class="mark">S</div><div class="title">Sicredi</div></div>
    <div class="top-right">
      <div class="pill">🔍</div>
      <div class="top-avatar">
        <div class="avatar">RG</div>
        <div class="meta"><div class="name">Rafael G.</div><div class="sub">Agência 01</div></div>
      </div>
      <div class="pill">🔔</div>
    </div>
  </header>

  <div class="layout">
    <!-- SIDEBAR -->
    <aside class="sidebar" role="navigation" aria-label="Barra lateral">
      <div class="icon" onclick="go('home')" title="Home">🏠</div>
      <div class="icon" onclick="go('campanhas')" title="Campanhas">🗂️</div>
      <div class="icon" onclick="go('placares')" title="Placares">📊</div>
      <div class="icon" onclick="go('ciclo')" title="Ciclo de Crédito">💳</div>
      <div class="icon" onclick="go('comercial')" title="Crédito Comercial">🏦</div>
      <div class="bottom"><div class="icon">⚙️</div></div>
    </aside>

    <!-- MAIN -->
    <main>
      <!-- HOME -->
      <section id="view-home" class="card">
        <div class="card-header">
          <h2>Categorias</h2>
          <div class="search-wrap">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden>
              <circle cx="11" cy="11" r="7"></circle><line x1="21" y1="21" x2="16.65" y2="16.65"></line>
            </svg>
            <input id="q-home" placeholder="Procurar..." onfocus="showTags('home')" oninput="filterTiles('home')"/>
            <div id="tags-home" class="tag-suggest"></div>
          </div>
        </div>

        <div id="grid-home" class="grid cols-2"></div>

        <div class="pagination">
          <button class="pg-btn">«</button><button class="pg-btn">‹</button>
          <span class="dots"><span class="dot"></span><span class="dot"></span><span class="dot"></span></span>
          <button class="pg-btn">›</button><button class="pg-btn">»</button>
        </div>
      </section>

      <!-- LISTA DE CAMPANHAS -->
      <section id="view-campanhas" class="card hidden">
        <div class="card-header">
          <div class="back" onclick="go('home')">← Voltar</div>
          <h2 style="margin-left:8px">Campanhas</h2>
          <div class="search-wrap">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden>
              <circle cx="11" cy="11" r="7"></circle><line x1="21" y1="21" x2="16.65" y2="16.65"></line>
            </svg>
            <input id="q-campanhas" placeholder="Procurar..." onfocus="showTags('campanhas')" oninput="filterTiles('campanhas')"/>
            <div id="tags-campanhas" class="tag-suggest"></div>
          </div>
        </div>
        <div id="grid-campanhas" class="grid cols-2"></div>
        <div class="pagination"><button class="pg-btn">«</button><button class="pg-btn">‹</button><span class="dots"><span class="dot"></span><span class="dot"></span><span class="dot"></span></span><button class="pg-btn">›</button><button class="pg-btn">»</button></div>
      </section>

      <!-- LISTA PLACARES -->
      <section id="view-placares" class="card hidden">
        <div class="card-header">
          <div class="back" onclick="go('home')">← Voltar</div>
          <h2 style="margin-left:8px">Placares</h2>
          <div class="search-wrap">
            <svg viewBox="0 0 24 24"><circle cx="11" cy="11" r="7" fill="none" stroke="currentColor" stroke-width="2"/><line x1="21" y1="21" x2="16.65" y2="16.65" stroke="currentColor" stroke-width="2"/></svg>
            <input id="q-placares" placeholder="Procurar..." onfocus="showTags('placares')" oninput="filterTiles('placares')"/>
            <div id="tags-placares" class="tag-suggest"></div>
          </div>
        </div>
        <div id="grid-placares" class="grid cols-3"></div>
      </section>

      <!-- LISTA CICLO DE CRÉDITO -->
      <section id="view-ciclo" class="card hidden">
        <div class="card-header">
          <div class="back" onclick="go('home')">← Voltar</div>
          <h2 style="margin-left:8px">Ciclo de Crédito</h2>
          <div class="search-wrap">
            <svg viewBox="0 0 24 24"><circle cx="11" cy="11" r="7" fill="none" stroke="currentColor" stroke-width="2"/><line x1="21" y1="21" x2="16.65" y2="16.65" stroke="currentColor" stroke-width="2"/></svg>
            <input id="q-ciclo" placeholder="Procurar..." onfocus="showTags('ciclo')" oninput="filterTiles('ciclo')"/>
            <div id="tags-ciclo" class="tag-suggest"></div>
          </div>
        </div>
        <div id="grid-ciclo" class="grid cols-2"></div>
      </section>

      <!-- LISTA CRÉDITO COMERCIAL -->
      <section id="view-comercial" class="card hidden">
        <div class="card-header">
          <div class="back" onclick="go('home')">← Voltar</div>
          <h2 style="margin-left:8px">Crédito Comercial</h2>
          <div class="search-wrap">
            <svg viewBox="0 0 24 24"><circle cx="11" cy="11" r="7" fill="none" stroke="currentColor" stroke-width="2"/><line x1="21" y1="21" x2="16.65" y2="16.65" stroke="currentColor" stroke-width="2"/></svg>
            <input id="q-comercial" placeholder="Procurar..." onfocus="showTags('comercial')" oninput="filterTiles('comercial')"/>
            <div id="tags-comercial" class="tag-suggest"></div>
          </div>
        </div>
        <div id="grid-comercial" class="grid cols-2"></div>
      </section>

      <!-- DASH DETAIL -->
      <section id="view-dash-detail" class="card hidden">
        <div class="card-header">
          <div class="back" onclick="go(lastList)">← Voltar</div>
          <h2 id="dash-title" style="margin-left:8px">Dash</h2>
          <div style="width:260px"></div>
        </div>
        <div class="viewer">
          <div id="frameWrap" class="frame"><span>Power BI Iframe</span></div>
          <div id="dash-counter" class="counter"></div>
          <div id="dash-tags" class="tags"></div>
        </div>
      </section>

    </main>
  </div>
</div>

<script>
/* ===== DATA (edite os embedUrl pelos seus) ===== */
const DATA = {
  home: [
    {id:'campanhas', title:'Campanhas', desc:'Dashboards de campanhas', action:'campanhas'},
    {id:'placares',  title:'Placares',  desc:'Agência, Gestor e Analítico', action:'placares'},
    {id:'ciclo',     title:'Ciclo de Crédito', desc:'Recuperação e cobrança', action:'ciclo'},
    {id:'comercial', title:'Crédito Comercial', desc:'Ofertas e liberações', action:'comercial'},
  ],
  campanhas: [
    { id:'bola-rede', title:'Bola na Rede', desc:'Campanha bola na rede',
      embedUrl:'https://app.powerbi.com/view?r=REPLACE_BOLA',
      tags:['campanhas','bola','rede','marketing'] },
    { id:'viva-rio', title:'Viva o Rio', desc:'Campanha Viva o Rio',
      embedUrl:'https://app.powerbi.com/view?r=REPLACE_RIO',
      tags:['campanhas','rio','captação'] },
    { id:'feirao', title:'Feirão', desc:'Ofertas especiais',
      embedUrl:'https://app.powerbi.com/view?r=REPLACE_FEIRAO',
      tags:['campanhas','ofertas','vendas'] },
    { id:'associados-plus', title:'Associados+', desc:'Relacionamento',
      embedUrl:'https://app.powerbi.com/view?r=REPLACE_ASSOC',
      tags:['associados','nps','relacionamento'] },
  ],
  placares: [
    { id:'placar-agencia',  title:'Placar Agência', desc:'Placar por agências',
      embedUrl:'https://app.powerbi.com/view?r=REPLACE_AG',
      tags:['placar','agência','meta'] },
    { id:'placar-gestor',   title:'Placar Gestor', desc:'Placar por gestor',
      embedUrl:'https://app.powerbi.com/view?r=REPLACE_GE',
      tags:['placar','gestor','desempenho'] },
    { id:'placar-analitico',title:'Placar Analítico', desc:'Visão analítica',
      embedUrl:'https://app.powerbi.com/view?r=REPLACE_AN',
      tags:['placar','analítico','drill'] },
  ],
  ciclo: [
    { id:'recuperacao', title:'Recuperação', desc:'Operações em atraso',
      embedUrl:'https://app.powerbi.com/view?r=REPLACE_REC',
      tags:['ciclo','atraso','recuperação'] },
    { id:'cobranca', title:'Cobrança', desc:'Oportunidades de cobrança',
      embedUrl:'https://app.powerbi.com/view?r=REPLACE_COB',
      tags:['ciclo','cobrança','pipeline'] },
    { id:'renegociacao', title:'Renegociação', desc:'Acordos e propostas',
      embedUrl:'https://app.powerbi.com/view?r=REPLACE_REN',
      tags:['ciclo','renegociação'] },
    { id:'liberacoes', title:'Liberações', desc:'Controle de liberações',
      embedUrl:'https://app.powerbi.com/view?r=REPLACE_LIB',
      tags:['ciclo','liberações'] },
  ],
  comercial: [
    { id:'ofertas', title:'Ofertas', desc:'Ofertas de crédito',
      embedUrl:'https://app.powerbi.com/view?r=REPLACE_OFE',
      tags:['comercial','ofertas'] },
    { id:'liberacoes-com', title:'Liberações', desc:'Liberações comerciais',
      embedUrl:'https://app.powerbi.com/view?r=REPLACE_LC',
      tags:['comercial','liberações'] },
    { id:'aprovacoes', title:'Aprovações', desc:'Fluxo e SLA',
      embedUrl:'https://app.powerbi.com/view?r=REPLACE_APR',
      tags:['comercial','aprovação','sla'] },
    { id:'carteira', title:'Carteira', desc:'Acompanhamento de carteira',
      embedUrl:'https://app.powerbi.com/view?r=REPLACE_CAR',
      tags:['comercial','carteira','crm'] },
  ],
};

/* ===== RENDER HELPERS ===== */
const qs = (s,p=document)=>p.querySelector(s); const qsa=(s,p=document)=>[...p.querySelectorAll(s)];
let lastList = 'home';

function go(name){
  ['home','campanhas','placares','ciclo','comercial','dash-detail']
    .forEach(v=>qs(`#view-${v}`).classList.add('hidden'));
  qs(`#view-${name}`).classList.remove('hidden');
  if (name in DATA) { renderList(name); lastList = name; }
  history.replaceState({}, "", `#${name}`);
}

function renderList(name){
  const grid = qs(`#grid-${name}`) || qs(`#grid-home`);
  grid.innerHTML = '';
  const items = name==='home' ? DATA.home : DATA[name];
  items.forEach(item=>{
    const wrap = document.createElement('div'); wrap.className='tile';
    wrap.innerHTML = `
      <h3>${item.title}</h3>
      <p>${item.desc||''}</p>
      <button class="btn primary" ${name==='home'
        ? `onclick="go('${item.action}')"`
        : `onclick="openDash('${name}','${item.id}')"`}>${name==='home'?'Acessar':'Abrir'}</button>
      ${item.tags? `<div class="tags">${item.tags.map(t=>`<span class="tag" onclick="applyTag('${name}','${t}')">#${t}</span>`).join('')}</div>`:''}
      `;
    grid.appendChild(wrap);
  });
  buildTagSuggest(name);
}

function openDash(listName, id){
  const item = (DATA[listName]||[]).find(i=>i.id===id);
  if (!item) return;
  qs('#dash-title').textContent = item.title; // muda o título para o nome do dash
  const wrap = qs('#frameWrap'); wrap.innerHTML = '';
  const iframe = document.createElement('iframe');
  iframe.loading = 'lazy';
  iframe.src = item.embedUrl || 'about:blank'; // troque o URL pelos seus embeds
  wrap.appendChild(iframe);

  // contador de acessos
  const key = `access:${id}`;
  const count = Number(localStorage.getItem(key) || 0) + 1;
  localStorage.setItem(key, String(count));
  qs('#dash-counter').textContent = `Acessos: ${count}`;

  // tags visíveis no detalhe
  qs('#dash-tags').innerHTML = (item.tags||[]).map(t=>`<span class="tag" onclick="applyTag('${listName}','${t}')">#${t}</span>`).join('');

  go('dash-detail');
}

/* ===== SEARCH + TAGS ===== */
function buildTagSuggest(name){
  const box = qs(`#tags-${name}`); if (!box) return;
  const items = name==='home' ? [] : DATA[name];
  const tags = [...new Set(items.flatMap(i=>i.tags||[]))];
  box.innerHTML = tags.map(t=>`<span class="tag" onclick="applyTag('${name}','${t}')">#${t}</span>`).join('');
}
function showTags(name){
  const box = qs(`#tags-${name}`); if (box) box.style.display='flex';
}
function hideTags(name){
  const box = qs(`#tags-${name}`); if (box) box.style.display='none';
}
['home','campanhas','placares','ciclo','comercial'].forEach(n=>{
  const input = qs(`#q-${n}`);
  if (input){
    input.addEventListener('blur',()=>setTimeout(()=>hideTags(n),150));
  }
});
function applyTag(name, tag){
  const input = qs(`#q-${name}`); if (!input) return;
  input.value = tag; showTags(name); filterTiles(name);
}
function filterTiles(name){
  const q = (qs(`#q-${name}`)?.value||'').toLowerCase();
  const grid = qs(`#grid-${name}`) || qs('#grid-home');
  qsa('.tile', grid).forEach(card=>{
    const text = card.textContent.toLowerCase();
    card.style.display = text.includes(q) ? '' : 'none';
  });
}

/* ===== INIT ===== */
function initHome(){
  const grid = qs('#grid-home');
  grid.innerHTML = '';
  DATA.home.forEach(c=>{
    const el = document.createElement('div'); el.className='tile';
    el.innerHTML = `<h3>${c.title}</h3><p>${c.desc||''}</p>
      <button class="btn primary" onclick="go('${c.action}')">Acessar</button>`;
    grid.appendChild(el);
  });
}
window.addEventListener('load',()=>{
  initHome();
  const route = location.hash.replace('#','');
  go(['home','campanhas','placares','ciclo','comercial','dash-detail'].includes(route)?route:'home');
});
</script>
</body>
</html>

