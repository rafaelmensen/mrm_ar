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