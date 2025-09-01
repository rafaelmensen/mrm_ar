<img width="1912" height="921" alt="image" src="https://github.com/user-attachments/assets/f26ae8a7-a164-4df6-abd7-f70b8fa46274" />
   

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
        <div aria-hidden>🔍</div>
        <div class="top-avatar">
          <div class="avatar">RG</div>
          <div class="meta">
            <div class="name">Rafael G.</div>
            <div class="sub">Agência 01</div>
          </div>
        </div>
        <div aria-hidden>🔔</div>
      </div>
    </header>

    <div class="layout">
      <aside class="sidebar" role="navigation" aria-label="Barra lateral">
        <div class="icon" title="Home">-</div>
        <div class="icon" title="Grid">-</div>
        <div class="icon" title="Settings">-</div>
        <div class="icon" title="Messages">-</div>
        <div class="icon" title="Video">-</div>
        <div class="icon" title="List">-</div>
        <div class="icon" title="Search">-</div>
        <div class="icon" title="Store">-</div>
        <div class="icon" title="Users">-</div>
        <div class="icon" title="Headphones">-</div>

        <div class="bottom">
          <div class="icon" title="Analytics">📊</div>
        </div>
      </aside>

      <main>
        <div class="card">
          <div class="card-header">
            <h2>Power BI</h2>

            <div class="search-wrap">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden>
                <circle cx="11" cy="11" r="7"></circle>
                <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
              </svg>
              <input placeholder="Procurar..." />
            </div>
          </div>

          <div class="card-body">
            <div class="results">
              <div class="empty">Sem resultados encontrados</div>


   

              </div>
            </div>
          </div>
        </div>
      </main>
    </div>
  </div>
</body>
</html>

          </div>
        </main>
      </div>
    </div>
  );
}
