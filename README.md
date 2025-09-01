<!doctype html>
<html lang="pt-BR">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Sicredi • Accordion central + pesquisa</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
<style>
:root{--bg:#EFF3F8;--card:#fff;--muted:#6b7280;--border:#E4E6EB;--accent:#10b981;--ink:#0f172a}
*{box-sizing:border-box} body{margin:0;font-family:Inter,ui-sans-serif,-apple-system,"Segoe UI",Roboto;color:var(--ink);background:var(--bg)}
/* Top */
header{display:flex;justify-content:space-between;align-items:center;padding:12px 24px;background:#fff;border-bottom:1px solid var(--border)}
.logo{display:flex;align-items:center;gap:10px}.logo .mark{width:28px;height:28px;border-radius:999px;background:var(--accent);display:grid;place-items:center;color:#fff;font-weight:700}
.logo .title{color:#059669;font-weight:700}
/* Page */
.main{padding:24px;max-width:1200px;margin:0 auto}
.panel{border:1px solid var(--border);background:#fff;border-radius:12px;box-shadow:0 1px 2px rgba(2,6,23,.04);overflow:hidden}
.panel-hd{display:flex;gap:12px;align-items:center;justify-content:space-between;padding:16px 20px;border-bottom:1px solid var(--border)}
.panel-hd h2{margin:0;font-size:18px}
.search{position:relative}
.search input{width:340px;max-width:42vw;padding:9px 12px 9px 36px;border:1px solid var(--border);border-radius:10px;font-size:14px}
.search svg{position:absolute;left:10px;top:50%;transform:translateY(-50%);width:16px;height:16px;color:#94a3b8}
/* Two columns (accordion at center area, not lateral page) */
.body{display:grid;grid-template-columns:360px 1fr;gap:18px;padding:18px}
@media (max-width:980px){.body{grid-template-columns:1fr}}
/* Accordion */
.section{border:1px solid var(--border);border-radius:12px;overflow:hidden}
.section summary{list-style:none;cursor:pointer;display:flex;justify-content:space-between;align-items:center;padding:12px 14px;font-weight:600}
.section[open] summary{background:#f8fafc}
.chev{transition:transform .15s}.section[open] .chev{transform:rotate(180deg)}
.section-body{padding:10px 12px 14px;display:flex;flex-direction:column;gap:10px;max-height:52vh;overflow:auto}
.card{border:1px solid var(--border);border-radius:10px;padding:10px;background:#fff;display:flex;flex-direction:column;gap:8px}
.card h4{margin:0;font-size:14px}
.card p{margin:0;font-size:12px;color:var(--muted)}
.btn{align-self:flex-start;border:1px solid var(--border);border-radius:10px;background:#ecfdf5;color:#065f46;padding:6px 10px;cursor:pointer}
.counter{display:flex;gap:6px;align-items:center;font-size:12px;color:#6b7280}
.eye{width:16px;height:16px}
/* Tags */
.tags{display:flex;gap:6px;flex-wrap:wrap}
.tag{padding:3px 8px;border:1px solid var(--border);border-radius:999px;font-size:12px;color:#065f46;background:#ecfdf5;cursor:pointer}
.tags.collapsed .tag{display:none}
.tags.collapsed .tag.main{display:inline-flex}
.toggle{font-size:12px;color:#0ea5e9;cursor:pointer}
/* Viewer */
.viewer{border:1px solid var(--border);border-radius:12px;background:#fff;padding:12px}
.frame{background:#f8fafc;border:1px dashed #cbd5e1;border-radius:12px;height:520px;display:grid;place-items:center;color:#64748b}
.frame iframe{width:100%;height:100%;border:0;border-radius:12px}
.viewer-hd{display:flex;justify-content:space-between;align-items:center;margin-bottom:10px}
.viewer-tags{margin-top:10px}
</style>
</head>
<body>
<header>
  <div class="logo"><div class="mark">S</div><div class="title">Sicredi</div></div>
</header>

<div class="main">
  <section class="panel">
    <div class="panel-hd">
      <h2>Dashboards</h2>
      <div class="search">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="7"></circle><line x1="21" y1="21" x2="16.65" y2="16.65"></line></svg>
        <input id="q" placeholder="Pesquisar por título ou tag…" oninput="filterAll()"/>
      </div>
    </div>

    <div class="body">
      <!-- Accordion central -->
      <div id="accordion"></div>

      <!-- Viewer -->
      <div class="viewer">
        <div class="viewer-hd">
          <h3 id="dashTitle" style="margin:0">Selecione um dashboard</h3>
          <div id="dashCounter" class="counter">
            <svg class="eye" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M1 12s4-7 11-7 11 7 11 7-4 7-11 7-11-7-11-7Z"/><circle cx="12" cy="12" r="3"/></svg>
            <span>Acessos: 0</span>
          </div>
        </div>
        <div id="frameWrap" class="frame"><span>Nenhum dash aberto</span></div>
        <div id="viewerTags" class="tags" style="margin-top:10px"></div>
      </div>
    </div>
  </section>
</div>

<script>
/* DATA: tags principais em main; extras em hidden */
const DATA = {
  "Campanhas":[
    {id:"bola",title:"Bola na Rede",desc:"Campanha bola na rede",embedUrl:"https://app.powerbi.com/view?r=REPLACE_BOLA",tags:{main:["campanhas","bola"],hidden:["rede","marketing","captação"]}},
    {id:"rio",title:"Viva o Rio",desc:"Campanha Viva o Rio",embedUrl:"https://app.powerbi.com/view?r=REPLACE_RIO",tags:{main:["campanhas","rio"],hidden:["evento","engajamento"]}},
    {id:"feirao",title:"Feirão",desc:"Ofertas especiais",embedUrl:"https://app.powerbi.com/view?r=REPLACE_FEIRAO",tags:{main:["campanhas","ofertas"],hidden:["promo","vendas"]}},
    {id:"assoc",title:"Associados+",desc:"Relacionamento",embedUrl:"https://app.powerbi.com/view?r=REPLACE_ASSOC",tags:{main:["associados","nps"],hidden:["crm","csat"]}}
  ],
  "Placares":[
    {id:"ag",title:"Placar Agência",desc:"Por agências",embedUrl:"https://app.powerbi.com/view?r=REPLACE_AG",tags:{main:["placar","agência"],hidden:["meta","ranking"]}},
    {id:"ge",title:"Placar Gestor",desc:"Por gestor",embedUrl:"https://app.powerbi.com/view?r=REPLACE_GE",tags:{main:["placar","gestor"],hidden:["time","produtividade"]}},
    {id:"an",title:"Placar Analítico",desc:"Visão analítica",embedUrl:"https://app.powerbi.com/view?r=REPLACE_AN",tags:{main:["placar","analítico"],hidden:["drill","comparativos"]}}
  ],
  "Ciclo de Crédito":[
    {id:"rec",title:"Recuperação",desc:"Operações em atraso",embedUrl:"https://app.powerbi.com/view?r=REPLACE_REC",tags:{main:["ciclo","atraso"],hidden:["bucket","dias"]}},
    {id:"cob",title:"Cobrança",desc:"Oportunidades de cobrança",embedUrl:"https://app.powerbi.com/view?r=REPLACE_COB",tags:{main:["ciclo","cobrança"],hidden:["pipeline","negociação"]}},
    {id:"ren",title:"Renegociação",desc:"Acordos e propostas",embedUrl:"https://app.powerbi.com/view?r=REPLACE_REN",tags:{main:["ciclo","renegociação"],hidden:["inad","parcelas"]}},
    {id:"lib",title:"Liberações",desc:"Controle de liberações",embedUrl:"https://app.powerbi.com/view?r=REPLACE_LIB",tags:{main:["ciclo","liberações"],hidden:["fluxo","alçada"]}}
  ],
  "Crédito Comercial":[
    {id:"ofe",title:"Ofertas",desc:"Ofertas de crédito",embedUrl:"https://app.powerbi.com/view?r=REPLACE_OFE",tags:{main:["comercial","ofertas"],hidden:["taxa","campanha"]}},
    {id:"lic",title:"Liberações",desc:"Liberações comerciais",embedUrl:"https://app.powerbi.com/view?r=REPLACE_LC",tags:{main:["comercial","liberações"],hidden:["sla","squad"]}},
    {id:"apr",title:"Aprovações",desc:"Fluxo e SLA",embedUrl:"https://app.powerbi.com/view?r=REPLACE_APR",tags:{main:["comercial","aprovação"],hidden:["workflow","tempo"]}},
    {id:"car",title:"Carteira",desc:"Acompanhamento de carteira",embedUrl:"https://app.powerbi.com/view?r=REPLACE_CAR",tags:{main:["comercial","carteira"],hidden:["crm","pós-venda"]}}
  ]
};

/* Build accordion inside main card */
const acc = document.getElementById("accordion");
function buildAccordion(filter=""){
  acc.innerHTML="";
  const q = filter.toLowerCase().trim();
  Object.entries(DATA).forEach(([sec,items])=>{
    const list = items.filter(it=>{
      const all = [...it.tags.main, ...it.tags.hidden].join(" ").toLowerCase();
      const txt = (it.title+" "+it.desc).toLowerCase();
      return !q || all.includes(q) || txt.includes(q);
    });
    if(!list.length) return;

    const details = document.createElement("details");
    details.className="section"; details.open = true;
    details.innerHTML = `<summary>${sec}<span class="chev">▾</span></summary>`;
    const body = document.createElement("div"); body.className="section-body";

    list.forEach(it=>{
      const d = document.createElement("div"); d.className="card";
      d.innerHTML = `
        <h4>${it.title}</h4>
        <p>${it.desc||""}</p>
        <div class="tags collapsed" id="tags-${it.id}">
          ${it.tags.main.map(t=>`<span class="tag main" onclick="quickFilter('${t}')">#${t}</span>`).join("")}
          ${it.tags.hidden.map(t=>`<span class="tag" onclick="quickFilter('${t}')">#${t}</span>`).join("")}
        </div>
        <span class="toggle" onclick="toggleTags('${it.id}', this)">Mostrar tags</span>
        <button class="btn" onclick="openDash('${sec}','${it.id}')">Abrir</button>
        <div class="counter">
          <svg class="eye" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M1 12s4-7 11-7 11 7 11 7-4 7-11 7-11-7-11-7Z"/><circle cx="12" cy="12" r="3"/></svg>
          <span id="c-${it.id}">Acessos: ${getCount(it.id)}</span>
        </div>
      `;
      body.appendChild(d);
    });
    details.appendChild(body);
    acc.appendChild(details);
  });
}
function toggleTags(id, el){
  const box = document.getElementById(`tags-${id}`);
  const collapsed = box.classList.contains("collapsed");
  if(collapsed){ box.classList.remove("collapsed"); el.textContent="Ocultar tags"; }
  else{ box.classList.add("collapsed"); el.textContent="Mostrar tags"; }
}

/* Search */
function filterAll(){ buildAccordion(document.getElementById("q").value); }
function quickFilter(tag){ document.getElementById("q").value = tag; filterAll(); }

/* Open dash */
function getCount(id){ return Number(localStorage.getItem("access:"+id)||0); }
function openDash(section, id){
  const item = (DATA[section]||[]).find(i=>i.id===id); if(!item) return;
  document.getElementById("dashTitle").textContent = item.title;

  const fw = document.getElementById("frameWrap"); fw.innerHTML = "";
  const ifr = document.createElement("iframe"); ifr.src = item.embedUrl || "about:blank"; ifr.loading="lazy";
  fw.appendChild(ifr);

  const key = "access:"+id; const n = getCount(id)+1; localStorage.setItem(key, n);
  document.getElementById("dashCounter").lastElementChild.textContent = "Acessos: "+n;
  const cardC = document.getElementById("c-"+id); if(cardC) cardC.textContent = "Acessos: "+n;

  document.getElementById("viewerTags").innerHTML =
    [...item.tags.main, ...item.tags.hidden].map(t=>`<span class="tag" onclick="quickFilter('${t}')">#${t}</span>`).join("");
}

/* Init */
buildAccordion();
</script>
</body>
</html>