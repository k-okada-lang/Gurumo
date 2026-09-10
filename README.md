
Gurumo
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>GURUMO | 東京グルメ・レストラン検索</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Shippori+Mincho:wght@500;700;800&family=Zen+Kaku+Gothic+New:wght@400;500;700;900&family=Cormorant+Garamond:ital,wght@0,500;1,500&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#fbf4e9;
    --bg-elevated:#ffffff;
    --card:#ffffff;
    --card-line:rgba(90,58,30,0.12);
    --terracotta:#c1592f;
    --terracotta-soft:#e0895a;
    --olive:#8a9450;
    --olive-soft:#aab672;
    --text:#3a2a1e;
    --text-muted:#8a7460;
    --text-faint:#b7a691;
    --shadow-warm: 0 0 18px rgba(193,89,47,0.25);
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:
      radial-gradient(1200px 600px at 12% -10%, rgba(193,89,47,0.09), transparent 60%),
      radial-gradient(1000px 500px at 92% 0%, rgba(138,148,80,0.10), transparent 55%),
      var(--bg);
    color:var(--text);
    font-family:'Zen Kaku Gothic New', sans-serif;
    min-height:100vh;
    -webkit-font-smoothing:antialiased;
  }
  .serif{ font-family:'Shippori Mincho', serif; }
  .eyebrow{
    font-family:'Cormorant Garamond', serif;
    font-style:italic;
    letter-spacing:0.22em;
    color:var(--terracotta);
    font-size:13px;
    text-transform:uppercase;
  }
  a{ color:inherit; text-decoration:none; }
  button{ font-family:inherit; cursor:pointer; }
  input,select,textarea{ font-family:inherit; }

  /* ---------- topbar ---------- */
  .topbar{
    display:flex; align-items:center; justify-content:space-between;
    padding:22px clamp(18px,4vw,56px);
    border-bottom:1px solid var(--card-line);
    position:sticky; top:0; z-index:40;
    background:rgba(251,244,233,0.88);
    backdrop-filter:blur(10px);
  }
  .brand{ display:flex; flex-direction:column; gap:2px; cursor:pointer; }
  .brand .mark{
    font-family:'Shippori Mincho', serif;
    font-weight:800;
    font-size:22px;
    letter-spacing:0.06em;
    color:var(--terracotta);
  }
  .brand .sub{ font-size:10px; letter-spacing:0.32em; color:var(--text-faint); }
  .nav-actions{ display:flex; align-items:center; gap:10px; }
  .btn-ghost{
    background:transparent; border:1px solid var(--card-line); color:var(--text-muted);
    padding:9px 16px; border-radius:999px; font-size:13px; letter-spacing:0.03em;
    transition:.2s;
  }
  .btn-ghost:hover{ border-color:var(--terracotta); color:var(--terracotta); }

  main{ display:none; }
  main.active{ display:block; }

  /* ---------- hero / search ---------- */
  .hero{
    padding:56px clamp(18px,4vw,56px) 20px;
    text-align:center;
  }
  .hero h1{
    font-family:'Shippori Mincho', serif;
    font-weight:800;
    font-size:clamp(28px,4.4vw,46px);
    line-height:1.35;
    margin:14px 0 8px;
  }
  .hero h1 .accent{ color:var(--terracotta); }
  .hero p.lead{ color:var(--text-muted); font-size:14.5px; max-width:520px; margin:0 auto; line-height:1.9;}

  .search-panel{
    max-width:920px; margin:34px auto 0;
    background:linear-gradient(180deg, var(--bg-elevated), #fff8ee);
    border:1px solid var(--card-line);
    border-radius:18px;
    padding:20px clamp(14px,3vw,28px);
    box-shadow: 0 20px 50px rgba(90,58,30,0.10), inset 0 1px 0 rgba(255,255,255,0.6);
    position:relative;
  }
  .search-panel::before{
    content:"";
    position:absolute; inset:-1px;
    border-radius:18px;
    padding:1px;
    background:linear-gradient(120deg, rgba(193,89,47,0.4), transparent 30%, transparent 70%, rgba(138,148,80,0.35));
    -webkit-mask:linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0);
    -webkit-mask-composite:xor; mask-composite:exclude;
    pointer-events:none;
  }
  .search-grid{
    display:grid;
    grid-template-columns: 1.6fr 1fr 1fr auto;
    gap:12px;
  }
  @media (max-width:760px){ .search-grid{ grid-template-columns: 1fr 1fr; } }

  .field label{
    display:block; font-size:11px; letter-spacing:0.14em; color:var(--text-faint);
    margin-bottom:6px; text-transform:uppercase;
  }
  .field input, .field select{
    width:100%; background:rgba(255,255,255,0.9); border:1px solid var(--card-line);
    color:var(--text); padding:11px 12px; border-radius:9px; font-size:14px; outline:none;
    transition:.15s;
  }
  .field input:focus, .field select:focus{ border-color:var(--terracotta); box-shadow:0 0 0 3px rgba(193,89,47,0.12); }
  .field select{ appearance:none; background-image:linear-gradient(45deg, transparent 50%, var(--terracotta) 50%), linear-gradient(135deg, var(--terracotta) 50%, transparent 50%); background-position: calc(100% - 18px) center, calc(100% - 13px) center; background-size:5px 5px, 5px 5px; background-repeat:no-repeat; }

  .btn-search{
    background:linear-gradient(120deg, var(--terracotta), #a5461f);
    border:none; color:#fff; padding:0 26px; border-radius:9px; font-weight:700; font-size:14px;
    letter-spacing:0.04em; box-shadow:0 8px 22px rgba(193,89,47,0.30);
    transition:.2s; white-space:nowrap;
  }
  .btn-search:hover{ transform:translateY(-1px); box-shadow:0 10px 26px rgba(193,89,47,0.40); }

  /* ---------- quick icon filters ---------- */
  .quick-filters{
    max-width:1180px; margin:36px auto 0; padding:0 clamp(18px,4vw,56px);
    display:flex; flex-direction:column; gap:24px;
  }
  .quick-block .quick-label{
    font-size:11.5px; letter-spacing:0.14em; color:var(--text-faint); text-transform:uppercase;
    margin-bottom:12px; display:flex; align-items:center; gap:8px;
  }
  .quick-block .quick-label::after{ content:""; flex:1; height:1px; background:var(--card-line); }
  .quick-icons{ display:flex; gap:16px; flex-wrap:wrap; }
  .quick-chip{
    display:flex; flex-direction:column; align-items:center; gap:8px; width:76px;
    cursor:pointer; background:none; border:none; padding:0; color:inherit;
  }
  .quick-chip .circle{
    width:58px; height:58px; border-radius:50%; background:var(--card); border:1px solid var(--card-line);
    display:flex; align-items:center; justify-content:center; transition:.2s;
    box-shadow:0 2px 8px rgba(90,58,30,0.06);
  }
  .quick-chip .circle svg{ width:23px; height:23px; stroke:var(--text-muted); fill:none; }
  .quick-chip .label{ font-size:11.5px; color:var(--text-muted); text-align:center; line-height:1.3; transition:.2s; }
  .quick-chip:hover .circle{ border-color:var(--terracotta); transform:translateY(-2px); }
  .quick-chip.active .circle{
    background:linear-gradient(135deg, rgba(193,89,47,0.16), rgba(138,148,80,0.12));
    border-color:var(--terracotta); box-shadow:var(--shadow-warm);
  }
  .quick-chip.active .circle svg{ stroke:var(--terracotta); }
  .quick-chip.active .label{ color:var(--terracotta); }

  .result-meta{
    max-width:1180px; margin:38px auto 14px; padding:0 clamp(18px,4vw,56px);
    display:flex; align-items:baseline; justify-content:space-between; gap:12px; flex-wrap:wrap;
  }
  .result-meta h2{ font-family:'Shippori Mincho',serif; font-size:19px; font-weight:700; margin:0; }
  .result-meta .count{ color:var(--terracotta); }
  .sort-inline{ display:flex; align-items:center; gap:10px; font-size:12.5px; color:var(--text-muted); flex-wrap:wrap; }
  .sort-inline select{ background:var(--card); border:1px solid var(--card-line); color:var(--text); border-radius:7px; padding:6px 10px; font-size:12.5px; }
  .loc-btn{
    background:var(--card); border:1px solid var(--card-line); color:var(--text-muted);
    padding:7px 14px; border-radius:999px; font-size:12.5px; display:inline-flex; align-items:center; gap:6px;
    transition:.2s;
  }
  .loc-btn:hover{ border-color:var(--terracotta); color:var(--terracotta); }
  .loc-btn.active{ border-color:var(--olive); color:var(--olive); background:rgba(138,148,80,0.10); }
  .dist-badge{ font-size:11px; color:var(--text-faint); margin-top:2px; }

  /* ---------- grid / cards ---------- */
  .grid{
    max-width:1180px; margin:0 auto; padding:0 clamp(18px,4vw,56px) 70px;
    display:grid; grid-template-columns:repeat(auto-fill, minmax(268px,1fr)); gap:20px;
  }
  .card{
    background:var(--card); border:1px solid var(--card-line); border-radius:16px; overflow:hidden;
    cursor:pointer; transition:.22s; display:flex; flex-direction:column;
    box-shadow:0 2px 10px rgba(90,58,30,0.05);
  }
  .card:hover{ transform:translateY(-4px); border-color:rgba(193,89,47,0.4); box-shadow:0 16px 34px rgba(90,58,30,0.14); }
  .card .photo{
    height:150px; position:relative; display:flex; align-items:center; justify-content:center;
    font-family:'Shippori Mincho',serif; font-size:40px; font-weight:800; color:rgba(193,89,47,0.20);
    overflow:hidden; background:linear-gradient(160deg, #f6e9d8, #f1e0c9);
  }
  .card .photo img{ width:100%; height:100%; object-fit:cover; }
  .card .photo .badge{
    position:absolute; top:10px; left:10px; background:rgba(255,255,255,0.86); border:1px solid rgba(193,89,47,0.35);
    color:var(--terracotta); font-size:10.5px; padding:4px 9px; border-radius:999px; letter-spacing:0.04em;
  }
  .card .body{ padding:16px 16px 18px; display:flex; flex-direction:column; gap:8px; flex:1; }
  .card .name{ font-family:'Shippori Mincho',serif; font-weight:700; font-size:17px; }
  .card .loc{ font-size:12px; color:var(--text-muted); letter-spacing:0.02em; }
  .card .catch{ font-size:12.5px; color:var(--text-muted); line-height:1.7; flex:1; }
  .card .foot{ display:flex; align-items:center; justify-content:space-between; margin-top:4px; }
  .budget{ color:var(--terracotta); font-weight:700; font-size:13px; }
  .tags{ display:flex; flex-wrap:wrap; gap:6px; }
  .tag{ font-size:10.5px; color:var(--olive); border:1px solid rgba(138,148,80,0.4); padding:3px 8px; border-radius:999px; }
  .empty{ grid-column:1/-1; text-align:center; padding:70px 20px; color:var(--text-faint); }
  .empty .serif{ font-size:18px; color:var(--text-muted); display:block; margin-bottom:8px; }

  /* ---------- detail ---------- */
  .detail-wrap{ max-width:820px; margin:0 auto; padding:26px clamp(18px,4vw,56px) 80px; }
  .back{ display:inline-flex; align-items:center; gap:6px; color:var(--text-muted); font-size:13px; margin-bottom:18px; }
  .back:hover{ color:var(--terracotta); }
  .detail-photo{
    width:100%; aspect-ratio:16/9; border-radius:18px; overflow:hidden; position:relative;
    background:linear-gradient(160deg, #f6e9d8, #f1e0c9); border:1px solid var(--card-line);
    display:flex; align-items:center; justify-content:center;
    font-family:'Shippori Mincho',serif; font-size:56px; color:rgba(193,89,47,0.20); font-weight:800;
  }
  .detail-photo img{ width:100%; height:100%; object-fit:cover; }
  .thumb-row{ display:flex; gap:10px; margin-top:12px; }
  .thumb{
    width:76px; height:76px; border-radius:10px; overflow:hidden; padding:0;
    border:2px solid transparent; background:none; cursor:pointer; opacity:.65; transition:.15s; flex-shrink:0;
  }
  .thumb img{ width:100%; height:100%; object-fit:cover; display:block; }
  .thumb:hover{ opacity:1; }
  .thumb.active{ border-color:var(--terracotta); opacity:1; }
  .detail-head{ margin-top:22px; display:flex; align-items:flex-start; justify-content:space-between; gap:16px; flex-wrap:wrap; }
  .detail-head .eyebrow{ display:block; margin-bottom:6px; }
  .detail-head h1{ font-family:'Shippori Mincho',serif; font-size:clamp(24px,3.4vw,32px); margin:0 0 6px; font-weight:800; }
  .detail-head .loc{ color:var(--text-muted); font-size:13.5px; }
  .sns-row{ display:flex; gap:10px; }
  .sns-btn{
    width:42px; height:42px; border-radius:50%; border:1px solid var(--card-line);
    display:flex; align-items:center; justify-content:center; background:var(--card);
    transition:.2s;
  }
  .sns-btn svg{ width:19px; height:19px; stroke:var(--text-muted); fill:none; }
  .sns-btn:hover{ border-color:var(--terracotta); box-shadow:var(--shadow-warm); }
  .sns-btn:hover svg{ stroke:var(--terracotta); }
  .sns-btn.disabled{ opacity:0.3; pointer-events:none; }

  .detail-stats{ display:flex; gap:10px; flex-wrap:wrap; margin:22px 0 6px; }
  .stat-chip{ background:var(--card); border:1px solid var(--card-line); border-radius:11px; padding:10px 16px; }
  .stat-chip .k{ font-size:10.5px; color:var(--text-faint); letter-spacing:0.08em; }
  .stat-chip .v{ font-size:15px; color:var(--terracotta); font-weight:700; margin-top:2px; }

  .detail-tags{ display:flex; flex-wrap:wrap; gap:8px; margin:18px 0; }
  .section-title{ font-family:'Shippori Mincho',serif; font-size:16px; font-weight:700; margin:30px 0 10px; padding-bottom:8px; border-bottom:1px solid var(--card-line); }
  .desc{ color:var(--text-muted); line-height:2; font-size:14.5px; white-space:pre-wrap; }

  .info-grid{ display:flex; flex-direction:column; gap:10px; }
  .info-row{ display:flex; gap:16px; font-size:13.5px; padding:10px 0; border-bottom:1px dashed var(--card-line); }
  .info-row:last-child{ border-bottom:none; }
  .info-row .ik{ width:88px; flex-shrink:0; color:var(--text-faint); font-size:11.5px; letter-spacing:0.06em; padding-top:2px; }
  .info-row .iv{ color:var(--text); line-height:1.7; }
  .map-link{ color:var(--terracotta); font-size:12.5px; margin-left:8px; white-space:nowrap; }
  .map-link:hover{ text-decoration:underline; }

  .apply-bar{
    margin-top:34px; display:flex; gap:16px; flex-wrap:wrap; align-items:center;
    padding:20px; border-radius:14px; background:linear-gradient(120deg, rgba(193,89,47,0.10), rgba(138,148,80,0.08));
    border:1px solid var(--card-line);
  }
  .apply-buttons{ display:flex; flex-direction:column; gap:10px; }
  .btn-apply{
    background:linear-gradient(120deg, var(--terracotta), #a5461f); color:#fff; border:none; padding:13px 26px;
    border-radius:999px; font-weight:700; font-size:14px; box-shadow:0 8px 22px rgba(193,89,47,0.30);
    text-align:center;
  }
  .btn-line{
    display:inline-flex; align-items:center; justify-content:center;
    background:#06C755; color:#fff; border:none; padding:12px 26px;
    border-radius:999px; font-weight:700; font-size:13.5px; box-shadow:0 6px 18px rgba(6,199,85,0.30);
    width:fit-content; transition:.2s;
  }
  .btn-line:hover{ filter:brightness(1.06); transform:translateY(-1px); }
  .apply-bar .info{ font-size:12.5px; color:var(--text-muted); }

  /* ---------- admin ---------- */
  .admin-wrap{ max-width:960px; margin:0 auto; padding:34px clamp(18px,4vw,56px) 90px; }
  .admin-gate{ max-width:360px; margin:80px auto; text-align:center; }
  .admin-gate .serif{ font-size:20px; margin-bottom:14px; display:block; }
  .admin-gate input{ width:100%; background:var(--card); border:1px solid var(--card-line); color:var(--text); padding:12px 14px; border-radius:9px; margin:14px 0; text-align:center; letter-spacing:0.1em; }
  .admin-gate .hint{ font-size:11.5px; color:var(--text-faint); margin-top:8px; }
  .admin-header{ display:flex; align-items:center; justify-content:space-between; margin-bottom:22px; flex-wrap:wrap; gap:10px; }
  .admin-header h1{ font-family:'Shippori Mincho',serif; font-size:24px; margin:0; }

  /* ---------- manager cards (popular areas / tag presets) ---------- */
  .manager-card{ background:var(--card); border:1px solid var(--card-line); border-radius:14px; padding:20px 22px; margin-bottom:18px; }
  .manager-card h3{ font-family:'Shippori Mincho',serif; font-size:15px; margin:0 0 4px; }
  .manager-card .field-hint{ margin-top:0; }
  .manager-chips{ display:flex; flex-wrap:wrap; gap:8px; margin:14px 0; }
  .manager-chip{
    display:inline-flex; align-items:center; gap:6px; background:#fffaf3; border:1px solid var(--card-line);
    color:var(--text); font-size:12.5px; padding:6px 6px 6px 13px; border-radius:999px;
  }
  .chip-x{ background:none; border:none; color:var(--text-faint); font-size:15px; line-height:1; cursor:pointer; padding:3px 5px; border-radius:50%; }
  .chip-x:hover{ color:#c0392b; background:rgba(192,57,43,0.10); }
  .manager-add-row{ display:flex; gap:8px; }
  .manager-add-row input{
    flex:1; background:#fffaf3; border:1px solid var(--card-line); border-radius:8px;
    padding:9px 12px; font-size:13px; color:var(--text); outline:none;
  }
  .manager-add-row input:focus{ border-color:var(--terracotta); }
  .btn-manager-add{ background:var(--terracotta); color:#fff; border:none; padding:0 20px; border-radius:8px; font-size:13px; font-weight:700; }
  .btn-manager-add:hover{ background:#a5461f; }
  .manager-empty{ color:var(--text-faint); font-size:12.5px; padding:2px 0 12px; }

  /* ---------- tag presets inside store form ---------- */
  .tag-preset-chips{ display:flex; flex-wrap:wrap; gap:6px; margin-top:9px; }
  .tag-chip{
    background:#fffaf3; border:1px solid var(--card-line); color:var(--text-muted);
    font-size:11.5px; padding:5px 11px; border-radius:999px; cursor:pointer; transition:.15s;
  }
  .tag-chip:hover{ border-color:var(--terracotta); color:var(--terracotta); }
  .tag-chip.active{ background:rgba(193,89,47,0.14); border-color:var(--terracotta); color:var(--terracotta); font-weight:700; }

  /* ---------- scrollable select (area picker) ---------- */
  .form-grid select.scroll-select{
    background-image:none; padding:6px; height:150px; overflow-y:auto;
  }
  .form-grid select.scroll-select option{ padding:7px 8px; border-radius:5px; }

  /* ---------- photo upload slots (admin) ---------- */
  .photo-slots{ display:flex; gap:14px; flex-wrap:wrap; margin-top:8px; }
  .photo-slot{ display:flex; flex-direction:column; align-items:center; gap:6px; }
  .photo-drop{
    width:128px; height:128px; border:1.5px dashed var(--card-line); border-radius:12px;
    background:#fffaf3; display:flex; align-items:center; justify-content:center;
    cursor:pointer; overflow:hidden; position:relative; transition:.15s;
  }
  .photo-drop:hover{ border-color:var(--terracotta); background:rgba(193,89,47,0.06); }
  .photo-drop.drag-over{ border-color:var(--terracotta); background:rgba(193,89,47,0.12); }
  .photo-drop.has-image{ border-style:solid; }
  .photo-drop img{ width:100%; height:100%; object-fit:cover; }
  .photo-drop-hint{ display:flex; flex-direction:column; align-items:center; gap:4px; color:var(--text-faint); font-size:11px; text-align:center; line-height:1.5; padding:8px; }
  .photo-drop-hint span{ font-size:24px; line-height:1; color:var(--terracotta); }
  .photo-remove{ background:none; border:none; color:var(--text-faint); font-size:11.5px; cursor:pointer; padding:2px 6px; }
  .photo-remove:hover{ color:#c0392b; }
  .photo-slot-label{ font-size:11px; color:var(--text-faint); }
  .admin-panel{ display:grid; grid-template-columns: 1fr 1.3fr; gap:22px; align-items:start; }
  @media (max-width:860px){ .admin-panel{ grid-template-columns:1fr; } }
  .admin-list{ background:var(--card); border:1px solid var(--card-line); border-radius:14px; padding:10px; max-height:640px; overflow:auto; }
  .admin-row{ display:flex; align-items:center; justify-content:space-between; padding:12px 10px; border-radius:9px; cursor:pointer; gap:8px; }
  .admin-row:hover{ background:rgba(193,89,47,0.05); }
  .admin-row.active{ background:rgba(193,89,47,0.10); border:1px solid rgba(193,89,47,0.3); }
  .admin-row .rn{ font-size:13.5px; font-weight:700; }
  .admin-row .ra{ font-size:11px; color:var(--text-faint); }
  .admin-row .del{ color:var(--text-faint); font-size:12px; padding:5px 8px; border-radius:6px; }
  .admin-row .del:hover{ color:#c0392b; background:rgba(192,57,43,0.08); }
  .admin-add{ width:100%; margin-top:10px; background:transparent; border:1px dashed var(--card-line); color:var(--terracotta); padding:11px; border-radius:9px; font-size:13px; }
  .admin-add:hover{ border-color:var(--terracotta); }

  .admin-form{ background:var(--card); border:1px solid var(--card-line); border-radius:14px; padding:22px; }
  .admin-form h3{ font-family:'Shippori Mincho',serif; margin:0 0 16px; font-size:16px; }
  .form-grid{ display:grid; grid-template-columns:1fr 1fr; gap:12px; }
  .form-grid .full{ grid-column:1/-1; }
  .form-grid label{ display:block; font-size:11px; color:var(--text-faint); letter-spacing:0.06em; margin-bottom:5px; }
  .form-grid input, .form-grid select, .form-grid textarea{
    width:100%; background:#fffaf3; border:1px solid var(--card-line); color:var(--text);
    padding:10px 11px; border-radius:8px; font-size:13.5px; outline:none;
  }
  .form-grid textarea{ resize:vertical; min-height:100px; line-height:1.7; }
  .form-grid input:focus, .form-grid select:focus, .form-grid textarea:focus{ border-color:var(--terracotta); }
  .form-actions{ display:flex; gap:10px; margin-top:18px; align-items:center; }
  .btn-save{ background:linear-gradient(120deg, var(--terracotta), #a5461f); border:none; color:#fff; font-weight:800; padding:11px 22px; border-radius:9px; font-size:13.5px; }
  .save-flag{ font-size:12px; color:var(--terracotta); opacity:0; transition:.3s; }
  .save-flag.show{ opacity:1; }
  .field-hint{ font-size:10.5px; color:var(--text-faint); margin-top:4px; }

  .toast{
    position:fixed; bottom:24px; left:50%; transform:translateX(-50%) translateY(20px);
    background:var(--text); border:1px solid rgba(255,255,255,0.15); color:#fff8ee;
    padding:12px 22px; border-radius:999px; font-size:13px; box-shadow:0 10px 30px rgba(0,0,0,0.25);
    opacity:0; pointer-events:none; transition:.25s; z-index:100;
  }
  .toast.show{ opacity:1; transform:translateX(-50%) translateY(0); }

  footer{ text-align:center; padding:30px; color:var(--text-faint); font-size:11.5px; border-top:1px solid var(--card-line); }

</style>
</head>
<body>


<header class="topbar">
  <div class="brand" onclick="go('home')">
    <span class="mark">GURUMO</span>
    <span class="sub">TOKYO RESTAURANT GUIDE</span>
  </div>
  <div class="nav-actions">
    <a class="btn-ghost" onclick="go('admin')">店舗管理（アドミン）</a>
  </div>
</header>

<main id="view-home" class="active">
  <section class="hero">
    <span class="eyebrow">Restaurant Guide</span>
    <h1>あなたにぴったりの一軒が、<br class="brk"><span class="accent">ここで見つかる。</span></h1>
    <p class="lead">エリア・ジャンル・ご予算から、行きたいお店を検索。写真とSNSで雰囲気を見てから予約できます。</p>

    <div class="search-panel">
      <div class="search-grid">
        <div class="field">
          <label>キーワード</label>
          <input id="f-keyword" type="text" placeholder="店名・エリア・こだわりで検索">
        </div>
        <div class="field">
          <label>エリア</label>
          <select id="f-area"><option value="">すべて</option></select>
        </div>
        <div class="field">
          <label>ジャンル</label>
          <select id="f-genre"><option value="">すべて</option></select>
        </div>
        <div class="field" style="align-self:end;">
          <button class="btn-search" onclick="applyFilters()">検索する</button>
        </div>
      </div>
    </div>

    <div class="quick-filters">
      <div class="quick-block">
        <div class="quick-label">ジャンルから探す</div>
        <div class="quick-icons" id="genre-icons"></div>
      </div>
      <div class="quick-block">
        <div class="quick-label">人気エリアから探す</div>
        <div class="quick-icons" id="area-icons"></div>
      </div>
    </div>
  </section>

  <div class="result-meta">
    <h2>掲載店舗 <span class="count" id="result-count">0</span> 件</h2>
    <div class="sort-inline">
      <button class="loc-btn" id="loc-btn" onclick="useMyLocation()">📍 現在地から探す</button>
      並び替え：
      <select id="f-sort" onchange="applyFilters()">
        <option value="new">新着順</option>
        <option value="budget-asc">ご予算が安い順</option>
        <option value="name">店名順</option>
        <option value="distance" id="distance-option" disabled>現在地から近い順</option>
      </select>
    </div>
  </div>

  <div class="grid" id="store-grid"></div>
</main>

<main id="view-detail">
  <div class="detail-wrap" id="detail-content"></div>
</main>

<main id="view-admin">
  <div class="admin-wrap" id="admin-content"></div>
</main>

<footer>GURUMO — 飲食店紹介サイト（デモ） / 掲載内容はアドミン画面から編集できます</footer>
<div class="toast" id="toast"></div>

<script>
/* ---------------- storage layer ----------------
   Uses Claude Artifacts' window.storage (shared) when running inside
   Claude, and falls back to the browser's localStorage when hosted
   standalone (e.g. GitHub Pages). See README.md for details/limits. */
const siteStorage = {
  async get(key){
    if(typeof window.storage !== 'undefined'){
      try{ return await window.storage.get(key, true); }catch(e){ /* fall through */ }
    }
    const v = localStorage.getItem(key);
    return v !== null ? {key, value:v, shared:false} : null;
  },
  async set(key, value){
    if(typeof window.storage !== 'undefined'){
      try{ return await window.storage.set(key, value, true); }catch(e){ /* fall through */ }
    }
    localStorage.setItem(key, value);
    return {key, value, shared:false};
  }
};

/* ---------------- data layer ---------------- */
const STORAGE_KEY = 'goodtable:stores';
const ADMIN_PASS = 'goodtable2026';

const DEFAULT_STORES = [
  {
    id:'s1', name:'旬彩 銀座無双', area:'銀座', genre:'和食',
    catch:'旬の食材を活かした会席と、落ち着いた個室。接待にも。',
    wage:'¥8,000〜¥12,000', tags:['個室あり','接待に人気','要予約'],
    photos:[], instagram:'https://www.instagram.com/', tiktok:'',
    phone:'03-1234-5678', email:'reserve@example.com',
    address:'東京都中央区銀座5-3-1', hours:'17:00〜23:00（L.O.22:00）', closedDay:'日曜日',
    description:'銀座の路地裏にひっそりと佇む和食店です。市場から仕入れる旬の食材を、板前が目の前で丁寧に仕上げます。掘りごたつの個室を3室ご用意しており、接待やご会食にもご利用いただけます。ご予算に応じたコース相談も承っております。'
  },
  {
    id:'s2', name:'Trattoria Verde', area:'代官山', genre:'イタリアン',
    catch:'自家製パスタとナチュラルワインが自慢のテラス席あり店。',
    wage:'¥5,000〜¥7,000', tags:['テラス席あり','デートに人気','ワイン豊富'],
    photos:[], instagram:'https://www.instagram.com/', tiktok:'https://www.tiktok.com/',
    phone:'03-2345-6789', email:'info@example.com',
    address:'東京都渋谷区代官山町14-6', hours:'11:30〜15:00 / 18:00〜22:30', closedDay:'月曜日',
    description:'代官山の緑に囲まれたテラス席が人気のトラットリアです。北イタリアで修業したシェフによる自家製パスタと、自然派ワインの品揃えが自慢。天気の良い日はペットとご一緒にテラスでのお食事もお楽しみいただけます。'
  },
  {
    id:'s3', name:'鮨 一心', area:'銀座', genre:'寿司',
    catch:'カウンター10席のみ。大将におまかせする至福のひととき。',
    wage:'¥18,000〜', tags:['カウンター席','おまかせのみ','記念日におすすめ'],
    photos:[], instagram:'https://www.instagram.com/', tiktok:'',
    phone:'03-3456-7890', email:'ichishin@example.com',
    address:'東京都中央区銀座7-8-4', hours:'18:00〜22:00（二部制）', closedDay:'日曜・祝日',
    description:'カウンター10席のみの小さな鮨屋です。毎朝豊洲市場で仕入れる旬のネタを、大将が目の前で握ります。コースはおまかせのみ。静かで特別な時間を過ごしたい方、記念日のお食事におすすめです。'
  },
  {
    id:'s4', name:'焼肉 炭家 恵比寿', area:'恵比寿', genre:'焼肉',
    catch:'黒毛和牛を備長炭で。飲み放題付きコースが人気。',
    wage:'¥6,000〜¥9,000', tags:['食べ放題あり','飲み放題付き','個室あり'],
    photos:[], instagram:'https://www.instagram.com/', tiktok:'https://www.tiktok.com/',
    phone:'03-4567-8901', email:'sumiya@example.com',
    address:'東京都渋谷区恵比寿1-11-2', hours:'17:00〜24:00（L.O.23:00）', closedDay:'年中無休',
    description:'厳選した黒毛和牛を備長炭でじっくり焼き上げる焼肉店。恵比寿駅から徒歩3分と好立地で、女子会から宴会まで幅広くご利用いただいています。飲み放題付きコースはコスパの良さで常連さんに人気です。'
  },
  {
    id:'s5', name:'Bistro Lumière', area:'中目黒', genre:'フレンチ',
    catch:'目黒川沿いの隠れ家ビストロ。記念日ディナーに。',
    wage:'¥7,000〜¥10,000', tags:['記念日におすすめ','ワイン豊富','夜景がきれい'],
    photos:[], instagram:'https://www.instagram.com/', tiktok:'',
    phone:'03-5678-9012', email:'lumiere@example.com',
    address:'東京都目黒区上目黒2-1-3', hours:'18:00〜23:00（L.O.22:00）', closedDay:'火曜日',
    description:'目黒川沿いに佇む隠れ家的なビストロです。フランス各地から届くワインと、季節の食材を使った気取らないフレンチをお楽しみいただけます。誕生日や記念日にはメッセージプレート（無料）もご用意可能です。'
  },
  {
    id:'s6', name:'喫茶ことり', area:'浅草', genre:'カフェ',
    catch:'昭和レトロな喫茶店。ひとり時間にもぴったり。',
    wage:'¥800〜¥1,500', tags:['一人利用歓迎','Wi-Fiあり','モーニングあり'],
    photos:[], instagram:'https://www.instagram.com/', tiktok:'https://www.tiktok.com/',
    phone:'03-6789-0123', email:'kotori@example.com',
    address:'東京都台東区浅草2-3-5', hours:'8:00〜19:00', closedDay:'水曜日',
    description:'浅草の路地にたたずむ昭和レトロな喫茶店です。自家焙煎のコーヒーと、ふわふわの厚焼き玉子サンドが看板メニュー。朝はモーニングセットも人気で、ひとりでゆっくり過ごすお客様も多くいらっしゃいます。'
  },
  {
    id:'s7', name:'酒場 三代目', area:'新宿', genre:'居酒屋',
    catch:'新宿の路地裏で三代続く大衆酒場。深夜まで営業。',
    wage:'¥3,000〜¥4,500', tags:['飲み放題あり','深夜営業','宴会OK'],
    photos:[], instagram:'https://www.instagram.com/', tiktok:'',
    phone:'03-7890-1234', email:'sandaime@example.com',
    address:'東京都新宿区歌舞伎町2-14-9', hours:'17:00〜翌3:00', closedDay:'不定休',
    description:'新宿の路地裏で祖父の代から三代続く大衆酒場です。日替わりのお通しと、豊富な日本酒の品揃えが自慢。深夜まで営業しているので、仕事帰りの一杯にもぴったりです。20名までの貸切宴会も承ります。'
  },
  {
    id:'s8', name:'中華飯店 龍鳳', area:'池袋', genre:'中華',
    catch:'本格四川の辛さが評判。ランチの担々麺も大人気。',
    wage:'¥2,500〜¥4,000', tags:['本格四川','辛口メニュー豊富','ランチ営業'],
    photos:[], instagram:'', tiktok:'',
    phone:'03-8901-2345', email:'ryuho@example.com',
    address:'東京都豊島区西池袋1-15-7', hours:'11:00〜15:00 / 17:00〜22:30', closedDay:'月曜日',
    description:'本場四川の花椒をふんだんに使った本格中華のお店です。看板メニューの担々麺はランチタイムに行列ができるほどの人気。辛さは5段階から選べるので、辛いものが苦手な方にもおすすめです。'
  }
];

let STORES = [];
let currentDetailId = null;
let adminUnlocked = false;
let adminSelectedId = null;

function normalizeStore(s){
  const photos = Array.isArray(s.photos) ? s.photos.filter(Boolean).slice(0,3) : (s.photo ? [s.photo] : []);
  return Object.assign({}, s, { photos });
}

async function loadStores(){
  try{
    const res = await siteStorage.get(STORAGE_KEY);
    if(res && res.value){
      STORES = JSON.parse(res.value).map(normalizeStore);
      return;
    }
  }catch(e){ /* not found yet */ }
  STORES = DEFAULT_STORES.map(normalizeStore);
  try{ await siteStorage.set(STORAGE_KEY, JSON.stringify(STORES)); }catch(e){}
}

async function saveStores(){
  try{
    const r = await siteStorage.set(STORAGE_KEY, JSON.stringify(STORES));
    return !!r;
  }catch(e){ return false; }
}

/* ---------------- utils ---------------- */
function esc(s){
  return (s||'').replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
}
function showToast(msg){
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  clearTimeout(showToast._h);
  showToast._h = setTimeout(()=> t.classList.remove('show'), 2200);
}
function mainPhoto(store){
  return (store.photos && store.photos[0]) || '';
}
function cardPhotoHtml(store){
  const src = mainPhoto(store);
  const initial = (store.name||'?').trim().charAt(0);
  const inner = src ? `<img src="${esc(src)}" alt="${esc(store.name)}">` : esc(initial);
  return `<div class="photo">${inner}<span class="badge">${esc(store.genre)}</span></div>`;
}

let currentGalleryPhotos = [];
function galleryHtml(store){
  const photos = Array.isArray(store.photos) ? store.photos.filter(Boolean) : [];
  currentGalleryPhotos = photos;
  const initial = (store.name||'?').trim().charAt(0);
  const mainInner = photos[0]
    ? `<img src="${esc(photos[0])}" alt="${esc(store.name)}" id="gallery-main-img">`
    : `<span>${esc(initial)}</span>`;
  const thumbs = photos.length > 1
    ? `<div class="thumb-row">${photos.map((p,i)=>`
        <button type="button" class="thumb ${i===0?'active':''}" onclick="setGalleryPhoto(${i}, this)">
          <img src="${esc(p)}" alt="${esc(store.name)} 写真${i+1}">
        </button>
      `).join('')}</div>`
    : '';
  return `<div class="detail-photo">${mainInner}</div>${thumbs}`;
}
function setGalleryPhoto(idx, btn){
  const src = currentGalleryPhotos[idx];
  const mainImg = document.getElementById('gallery-main-img');
  if(mainImg && src) mainImg.src = src;
  document.querySelectorAll('.thumb-row .thumb').forEach(el=>el.classList.remove('active'));
  if(btn) btn.classList.add('active');
}
function igIcon(){
  return `<svg viewBox="0 0 24 24" stroke-width="1.7"><rect x="3" y="3" width="18" height="18" rx="6"/><circle cx="12" cy="12" r="4.2"/><circle cx="17.2" cy="6.8" r="1.1" fill="currentColor" stroke="none"/></svg>`;
}
function ttIcon(){
  return `<svg viewBox="0 0 24 24" stroke-width="1.7"><path d="M14 3v10.8a3.6 3.6 0 1 1-3-3.55"/><path d="M14 3c.5 2.6 2.3 4.3 5 4.6"/></svg>`;
}

/* ---------------- quick filter icons ---------------- */
const GENRE_ICONS = {
  '和食': `<svg viewBox="0 0 24 24" stroke-width="1.6"><path d="M3 11h18a9 9 0 0 1-18 0Z"/><path d="M6 11a6 6 0 0 1 12 0"/><path d="M15 3.5 17 9M18 3.5 16.5 9"/></svg>`,
  'イタリアン': `<svg viewBox="0 0 24 24" stroke-width="1.6"><path d="M12 3.5 20.5 18.5Q12 22 3.5 18.5Z"/><circle cx="12" cy="10.5" r="1.15" fill="currentColor" stroke="none"/><circle cx="9" cy="14.5" r="1.15" fill="currentColor" stroke="none"/><circle cx="14.5" cy="15" r="1.15" fill="currentColor" stroke="none"/></svg>`,
  'フレンチ': `<svg viewBox="0 0 24 24" stroke-width="1.6"><path d="M7 3h10l-1 7a4 4 0 0 1-8 0L7 3Z"/><path d="M12 14v6M8.5 20h7"/></svg>`,
  '焼肉': `<svg viewBox="0 0 24 24" stroke-width="1.6"><path d="M4 14.2c0 4 3.6 6.8 8 6.8s8-2.8 8-6.8c0-2.8-1.8-4.6-2.8-6.4-.9 1.8-1.8 2.3-2.7 1.4.5-2.8-.9-5-2.5-6.2 0 2.8-.9 4.6-2.7 6.4-1.4 1.4-3.7 2.8-3.7 5.1 4.7-.9 3.7-2.8 3.7-2.8Z"/></svg>`,
  '寿司': `<svg viewBox="0 0 24 24" stroke-width="1.6"><path d="M4 15.5c0-2.2 2.2-3.3 8-3.3s8 1.1 8 3.3-2.2 3.3-8 3.3-8-1.1-8-3.3Z"/><path d="M6.5 12.2c0-3.2 2.4-6.7 5.5-6.7s5.5 3.5 5.5 6.7"/></svg>`,
  '中華': `<svg viewBox="0 0 24 24" stroke-width="1.6"><path d="M4 9h16l-1.5 10a2 2 0 0 1-2 1.8H7.5A2 2 0 0 1 5.5 19L4 9Z"/><path d="M9 9 8 4h8l-1 5M9 13v4M15 13v4"/></svg>`,
  'カフェ': `<svg viewBox="0 0 24 24" stroke-width="1.6"><path d="M5 8h11v6a5 5 0 0 1-5 5h-1a5 5 0 0 1-5-5V8Z"/><path d="M16 9.5h1.5a2.5 2.5 0 0 1 0 5H16"/><path d="M8 5c0-1 .9-1 .9-2M11.5 5c0-1 .9-1 .9-2"/></svg>`,
  '居酒屋': `<svg viewBox="0 0 24 24" stroke-width="1.6"><path d="M12 3v2M12 19v2"/><path d="M7 6c0-.7 2.2-1.1 5-1.1s5 .4 5 1.1c0 6.2-2 8.4-2 8.4H9S7 12.2 7 6Z"/><path d="M9 14.4h6"/></svg>`,
  'その他': `<svg viewBox="0 0 24 24" stroke-width="1.6"><path d="M6 3v6a1.8 1.8 0 0 0 1.8 1.8V21M6 3v4.5M8 3v4.5"/><path d="M17 3c-1.4 0-2.4 1.9-2.4 4.6S15.6 12 17 12v9"/></svg>`
};
function pinIcon(){
  return `<svg viewBox="0 0 24 24" stroke-width="1.6"><path d="M12 21s7-6.2 7-11.3A7 7 0 0 0 5 9.7C5 14.8 12 21 12 21Z"/><circle cx="12" cy="9.6" r="2.4"/></svg>`;
}
function lineIcon(){
  return `<svg viewBox="0 0 24 24" stroke-width="1.7" style="width:16px;height:16px;vertical-align:-3px;margin-right:5px;"><path d="M4 12c0-4.4 3.8-8 8.5-8S21 7.6 21 12s-3.8 8-8.5 8c-.9 0-1.8-.1-2.6-.4L6 21l1-3.8C5.2 15.8 4 14 4 12Z"/></svg>`;
}

/* ---------------- popular areas / tag presets (admin-managed) ---------------- */
const AREAS_STORAGE_KEY = 'goodtable:areas';
const TAGS_STORAGE_KEY = 'goodtable:tagPresets';

const DEFAULT_AREAS = ['銀座','渋谷','恵比寿','代官山','新宿','浅草','六本木','中目黒','池袋'];
let POPULAR_AREAS = DEFAULT_AREAS.slice();

const GENRE_LIST = ['和食','イタリアン','フレンチ','焼肉','寿司','中華','カフェ','居酒屋','その他'];

const DEFAULT_TAG_PRESETS = ['個室あり','接待に人気','要予約','テラス席あり','デートに人気','ワイン豊富','カウンター席','おまかせのみ','記念日におすすめ','食べ放題あり','飲み放題付き','夜景がきれい','一人利用歓迎','Wi-Fiあり','モーニングあり','飲み放題あり','深夜営業','宴会OK','本格四川','辛口メニュー豊富','ランチ営業'];
let TAG_PRESETS = DEFAULT_TAG_PRESETS.slice();

async function loadAreas(){
  try{
    const res = await siteStorage.get(AREAS_STORAGE_KEY);
    if(res && res.value){ POPULAR_AREAS = JSON.parse(res.value); return; }
  }catch(e){ /* not found yet */ }
  POPULAR_AREAS = DEFAULT_AREAS.slice();
  try{ await siteStorage.set(AREAS_STORAGE_KEY, JSON.stringify(POPULAR_AREAS)); }catch(e){}
}
async function saveAreas(){
  try{ const r = await siteStorage.set(AREAS_STORAGE_KEY, JSON.stringify(POPULAR_AREAS)); return !!r; }catch(e){ return false; }
}

async function loadTagPresets(){
  try{
    const res = await siteStorage.get(TAGS_STORAGE_KEY);
    if(res && res.value){ TAG_PRESETS = JSON.parse(res.value); return; }
  }catch(e){ /* not found yet */ }
  TAG_PRESETS = DEFAULT_TAG_PRESETS.slice();
  try{ await siteStorage.set(TAGS_STORAGE_KEY, JSON.stringify(TAG_PRESETS)); }catch(e){}
}
async function saveTagPresets(){
  try{ const r = await siteStorage.set(TAGS_STORAGE_KEY, JSON.stringify(TAG_PRESETS)); return !!r; }catch(e){ return false; }
}

function renderQuickFilters(){
  const gWrap = document.getElementById('genre-icons');
  gWrap.innerHTML = GENRE_LIST.map(g => `
    <button type="button" class="quick-chip" data-genre="${esc(g)}" onclick="toggleGenreFilter('${esc(g)}')">
      <span class="circle">${GENRE_ICONS[g]}</span>
      <span class="label">${esc(g)}</span>
    </button>
  `).join('');

  const aWrap = document.getElementById('area-icons');
  aWrap.innerHTML = POPULAR_AREAS.map(a => `
    <button type="button" class="quick-chip" data-area="${esc(a)}" onclick="toggleAreaFilter('${esc(a)}')">
      <span class="circle">${pinIcon()}</span>
      <span class="label">${esc(a)}</span>
    </button>
  `).join('');
}

function toggleGenreFilter(g){
  const sel = document.getElementById('f-genre');
  sel.value = (sel.value === g) ? '' : g;
  applyFilters();
}
function toggleAreaFilter(a){
  const sel = document.getElementById('f-area');
  sel.value = (sel.value === a) ? '' : a;
  applyFilters();
}
function syncQuickChips(){
  const genreVal = document.getElementById('f-genre').value;
  const areaVal = document.getElementById('f-area').value;
  document.querySelectorAll('#genre-icons .quick-chip').forEach(el=>{
    el.classList.toggle('active', el.dataset.genre === genreVal && genreVal !== '');
  });
  document.querySelectorAll('#area-icons .quick-chip').forEach(el=>{
    el.classList.toggle('active', el.dataset.area === areaVal && areaVal !== '');
  });
}

/* ---------------- nearby / geolocation ---------------- */
const AREA_COORDS = {
  '銀座':{lat:35.6717,lng:139.7650},
  '六本木':{lat:35.6627,lng:139.7307},
  '渋谷':{lat:35.6595,lng:139.7005},
  '新宿':{lat:35.6905,lng:139.7005},
  '新宿・歌舞伎町':{lat:35.6938,lng:139.7034},
  '恵比寿':{lat:35.6467,lng:139.7100},
  '代官山':{lat:35.6488,lng:139.6994},
  '中目黒':{lat:35.6446,lng:139.6989},
  '浅草':{lat:35.7118,lng:139.7966},
  '池袋':{lat:35.7295,lng:139.7109},
  '西麻布':{lat:35.6564,lng:139.7239},
  '新橋':{lat:35.6665,lng:139.7583},
  '赤坂':{lat:35.6737,lng:139.7368},
  '上野':{lat:35.7141,lng:139.7774},
  '五反田':{lat:35.6262,lng:139.7238}
};
const DEFAULT_COORDS = {lat:35.6812,lng:139.7671}; // 東京駅を仮の中心地とする
let userLocation = null;

function coordsForStore(s){
  if(typeof s.lat === 'number' && typeof s.lng === 'number' && !isNaN(s.lat) && !isNaN(s.lng)){
    return {lat:s.lat, lng:s.lng};
  }
  if(s.area && AREA_COORDS[s.area]) return AREA_COORDS[s.area];
  const key = s.area ? Object.keys(AREA_COORDS).find(k => s.area.includes(k) || k.includes(s.area)) : null;
  if(key) return AREA_COORDS[key];
  return DEFAULT_COORDS;
}
function haversineKm(lat1,lng1,lat2,lng2){
  const R = 6371;
  const dLat = (lat2-lat1) * Math.PI/180;
  const dLng = (lng2-lng1) * Math.PI/180;
  const a = Math.sin(dLat/2)**2 + Math.cos(lat1*Math.PI/180)*Math.cos(lat2*Math.PI/180)*Math.sin(dLng/2)**2;
  return R * 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));
}
function distanceForStore(s){
  if(!userLocation) return null;
  const c = coordsForStore(s);
  return haversineKm(userLocation.lat, userLocation.lng, c.lat, c.lng);
}
function useMyLocation(){
  const btn = document.getElementById('loc-btn');
  if(!navigator.geolocation){ showToast('お使いのブラウザは位置情報に対応していません'); return; }
  btn.textContent = '📍 取得中...';
  navigator.geolocation.getCurrentPosition(
    pos => {
      userLocation = {lat:pos.coords.latitude, lng:pos.coords.longitude};
      btn.textContent = '📍 現在地から検索中';
      btn.classList.add('active');
      document.getElementById('distance-option').disabled = false;
      document.getElementById('f-sort').value = 'distance';
      applyFilters();
      showToast('現在地から近い順に表示しています');
    },
    () => {
      btn.textContent = '📍 現在地から探す';
      showToast('現在地を取得できませんでした。位置情報の利用を許可してください。');
    },
    {timeout:10000}
  );
}

/* ---------------- navigation ---------------- */
function go(view){
  document.querySelectorAll('main').forEach(m => m.classList.remove('active'));
  document.getElementById('view-'+view).classList.add('active');
  window.scrollTo({top:0, behavior:'smooth'});
  if(view === 'admin') renderAdmin();
}

/* ---------------- home / search ---------------- */
function populateFilterOptions(){
  const areas = [...new Set(STORES.map(s=>s.area))].sort();
  const genres = [...new Set(STORES.map(s=>s.genre))].sort();
  const areaSel = document.getElementById('f-area');
  const genreSel = document.getElementById('f-genre');
  areaSel.innerHTML = '<option value="">すべて</option>' + areas.map(a=>`<option value="${esc(a)}">${esc(a)}</option>`).join('');
  genreSel.innerHTML = '<option value="">すべて</option>' + genres.map(g=>`<option value="${esc(g)}">${esc(g)}</option>`).join('');
}

function wageNumber(store){
  const m = (store.wage||'').match(/[\d,]+/);
  return m ? parseInt(m[0].replace(/,/g,''),10) : 0;
}

function applyFilters(){
  const kw = document.getElementById('f-keyword').value.trim().toLowerCase();
  const area = document.getElementById('f-area').value;
  const genre = document.getElementById('f-genre').value;
  const sort = document.getElementById('f-sort').value;

  let list = STORES.filter(s=>{
    if(area && s.area !== area) return false;
    if(genre && s.genre !== genre) return false;
    if(kw){
      const hay = [s.name, s.area, s.genre, s.catch, (s.tags||[]).join(' ')].join(' ').toLowerCase();
      if(!hay.includes(kw)) return false;
    }
    return true;
  });

  if(sort === 'budget-asc') list = list.slice().sort((a,b)=> wageNumber(a)-wageNumber(b));
  else if(sort === 'name') list = list.slice().sort((a,b)=> a.name.localeCompare(b.name,'ja'));
  else if(sort === 'distance' && userLocation) list = list.slice().sort((a,b)=> distanceForStore(a) - distanceForStore(b));

  syncQuickChips();
  renderGrid(list, sort === 'distance' && !!userLocation);
}

function renderGrid(list, showDistance){
  const grid = document.getElementById('store-grid');
  document.getElementById('result-count').textContent = list.length;
  if(list.length === 0){
    grid.innerHTML = `<div class="empty"><span class="serif">該当するお店が見つかりませんでした</span>キーワードやエリアを変えて、もう一度検索してみてください。</div>`;
    return;
  }
  grid.innerHTML = list.map(s => `
    <div class="card" onclick="openDetail('${s.id}')">
      ${cardPhotoHtml(s)}
      <div class="body">
        <div class="name">${esc(s.name)}</div>
        <div class="loc">${esc(s.area)} ・ ${esc(s.genre)}</div>
        <div class="catch">${esc(s.catch)}</div>
        <div class="tags">${(s.tags||[]).slice(0,3).map(t=>`<span class="tag">${esc(t)}</span>`).join('')}</div>
        <div class="foot"><span class="budget">ご予算 ${esc(s.wage)}</span></div>
        ${showDistance ? `<div class="dist-badge">📍現在地から約${distanceForStore(s).toFixed(1)}km</div>` : ''}
      </div>
    </div>
  `).join('');
}

/* ---------------- detail ---------------- */
function mapUrl(s){
  const q = s.address ? s.address : `${s.name} ${s.area}`;
  return `https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(q)}`;
}

function openDetail(id){
  currentDetailId = id;
  const s = STORES.find(x=>x.id===id);
  if(!s) return;
  const el = document.getElementById('detail-content');
  const hasIg = !!s.instagram, hasTt = !!s.tiktok;
  const hasPhone = !!s.phone, hasMail = !!s.email, hasLine = !!s.lineUrl;
  el.innerHTML = `
    <a class="back" onclick="go('home')">← 検索結果に戻る</a>
    ${galleryHtml(s)}
    <div class="detail-head">
      <div>
        <span class="eyebrow">${esc(s.genre)} ・ ${esc(s.area)}</span>
        <h1>${esc(s.name)}</h1>
        <div class="loc">${esc(s.catch)}</div>
      </div>
      <div class="sns-row">
        <a class="sns-btn ${hasIg?'':'disabled'}" href="${hasIg? esc(s.instagram) : '#'}" target="_blank" rel="noopener" title="Instagram">${igIcon()}</a>
        <a class="sns-btn ${hasTt?'':'disabled'}" href="${hasTt? esc(s.tiktok) : '#'}" target="_blank" rel="noopener" title="TikTok">${ttIcon()}</a>
      </div>
    </div>

    <div class="detail-stats">
      <div class="stat-chip"><div class="k">ご予算目安</div><div class="v">${esc(s.wage)}</div></div>
      <div class="stat-chip"><div class="k">エリア</div><div class="v">${esc(s.area)}</div></div>
      <div class="stat-chip"><div class="k">ジャンル</div><div class="v">${esc(s.genre)}</div></div>
    </div>
    <div class="detail-tags">${(s.tags||[]).map(t=>`<span class="tag">${esc(t)}</span>`).join('')}</div>

    <div class="section-title">店舗情報</div>
    <div class="info-grid">
      <div class="info-row"><div class="ik">住所</div><div class="iv">${s.address? esc(s.address) : '準備中'} ${s.address? `<a class="map-link" href="${mapUrl(s)}" target="_blank" rel="noopener">地図で見る →</a>`:''}</div></div>
      <div class="info-row"><div class="ik">営業時間</div><div class="iv">${s.hours? esc(s.hours) : '準備中'}</div></div>
      <div class="info-row"><div class="ik">定休日</div><div class="iv">${s.closedDay? esc(s.closedDay) : '準備中'}</div></div>
    </div>

    <div class="section-title">お店について</div>
    <div class="desc">${esc(s.description || 'この店舗の詳細情報は準備中です。')}</div>

    <div class="apply-bar">
      <div class="apply-buttons">
        <a class="btn-apply" href="${hasPhone? 'tel:'+esc(s.phone) : (hasMail? 'mailto:'+esc(s.email) : '#')}">${hasPhone? '電話で予約する' : 'この店舗に問い合わせる'}</a>
        ${hasLine ? `<a class="btn-line" href="${esc(s.lineUrl)}" target="_blank" rel="noopener">${lineIcon()}LINEで問い合わせる</a>` : ''}
      </div>
      <span class="info">${hasMail? 'メールでのお問い合わせ：'+esc(s.email) : (hasPhone? '':'お問い合わせ先は準備中です')}</span>
    </div>
  `;
  go('detail');
}

/* ---------------- admin ---------------- */
function emptyStoreDraft(){
  return { id:'', name:'', area:'', genre:'和食', catch:'', wage:'', tags:[], photos:[], instagram:'', tiktok:'', lineUrl:'', phone:'', email:'', address:'', hours:'', closedDay:'', description:'' };
}

/* ---- photo upload (drag & drop / click to browse, up to 3 per store) ---- */
let adminPhotoDraft = ['', '', ''];

function renderPhotoSlots(){
  const wrap = document.getElementById('photo-slots');
  if(!wrap) return;
  wrap.innerHTML = [0,1,2].map(i => {
    const src = adminPhotoDraft[i];
    return `
      <div class="photo-slot">
        <div class="photo-drop ${src?'has-image':''}"
             ondragover="event.preventDefault(); this.classList.add('drag-over')"
             ondragleave="this.classList.remove('drag-over')"
             ondrop="handlePhotoDrop(event, ${i})"
             onclick="document.getElementById('photo-file-${i}').click()">
          ${src
            ? `<img src="${esc(src)}" alt="店舗写真${i+1}">`
            : `<div class="photo-drop-hint"><span>＋</span>ドロップ<br>または選択</div>`}
          <input type="file" id="photo-file-${i}" accept="image/*" hidden onchange="handlePhotoInput(event, ${i})">
        </div>
        ${src ? `<button type="button" class="photo-remove" onclick="event.stopPropagation(); removePhoto(${i})">削除</button>` : `<span class="photo-slot-label">写真${i+1}${i===0?'（メイン）':''}</span>`}
      </div>
    `;
  }).join('');
}

function handlePhotoDrop(e, idx){
  e.preventDefault();
  e.currentTarget.classList.remove('drag-over');
  const file = e.dataTransfer && e.dataTransfer.files && e.dataTransfer.files[0];
  if(file) processPhotoFile(file, idx);
}
function handlePhotoInput(e, idx){
  const file = e.target.files && e.target.files[0];
  if(file) processPhotoFile(file, idx);
  e.target.value = '';
}
function processPhotoFile(file, idx){
  if(!file.type || !file.type.startsWith('image/')){ showToast('画像ファイルを選択してください'); return; }
  const reader = new FileReader();
  reader.onload = () => {
    const img = new Image();
    img.onload = () => {
      const maxDim = 1280;
      let w = img.width, h = img.height;
      if(w > maxDim || h > maxDim){
        if(w > h){ h = Math.round(h * maxDim / w); w = maxDim; }
        else{ w = Math.round(w * maxDim / h); h = maxDim; }
      }
      const canvas = document.createElement('canvas');
      canvas.width = w; canvas.height = h;
      canvas.getContext('2d').drawImage(img, 0, 0, w, h);
      adminPhotoDraft[idx] = canvas.toDataURL('image/jpeg', 0.82);
      renderPhotoSlots();
    };
    img.onerror = () => showToast('画像を読み込めませんでした');
    img.src = reader.result;
  };
  reader.onerror = () => showToast('画像を読み込めませんでした');
  reader.readAsDataURL(file);
}
function removePhoto(idx){
  adminPhotoDraft[idx] = '';
  renderPhotoSlots();
}

function areaOptionsHtml(currentArea){
  let list = POPULAR_AREAS.slice();
  if(currentArea && !list.includes(currentArea)) list = [currentArea, ...list];
  if(list.length===0) return `<option value="">（エリアが未登録です。下の「人気エリアの管理」から追加してください）</option>`;
  return list.map(a=>`<option value="${esc(a)}" ${a===currentArea?'selected':''}>${esc(a)}</option>`).join('');
}

function renderTagChipsHtml(currentTagsStr){
  const current = (currentTagsStr||'').split(',').map(t=>t.trim()).filter(Boolean);
  if(TAG_PRESETS.length===0) return `<div class="manager-empty">タグがまだ登録されていません</div>`;
  return TAG_PRESETS.map(t => `
    <button type="button" class="tag-chip ${current.includes(t)?'active':''}" onclick="toggleTagPreset('${esc(t)}')">${esc(t)}</button>
  `).join('');
}

function toggleTagPreset(tag){
  const input = document.getElementById('af-tags');
  if(!input) return;
  let current = input.value.split(',').map(t=>t.trim()).filter(Boolean);
  if(current.includes(tag)) current = current.filter(t=>t!==tag);
  else current.push(tag);
  input.value = current.join(', ');
  const chipsWrap = document.getElementById('tag-preset-chips');
  if(chipsWrap) chipsWrap.innerHTML = renderTagChipsHtml(input.value);
}

/* ---- popular area manager ---- */
function renderAreaManager(){
  const wrap = document.getElementById('area-manager-list');
  if(!wrap) return;
  if(POPULAR_AREAS.length===0){ wrap.innerHTML = `<div class="manager-empty">エリアがまだ登録されていません</div>`; return; }
  wrap.innerHTML = POPULAR_AREAS.map(a => `
    <span class="manager-chip">${esc(a)}<button type="button" class="chip-x" onclick="handleDeleteArea('${esc(a)}')" title="削除">×</button></span>
  `).join('');
}

async function handleAddArea(){
  const input = document.getElementById('new-area-input');
  const v = input.value.trim();
  if(!v){ showToast('エリア名を入力してください'); return; }
  if(POPULAR_AREAS.includes(v)){ showToast('すでに登録されています'); return; }
  POPULAR_AREAS.push(v);
  await saveAreas();
  input.value = '';
  renderAreaManager();
  renderQuickFilters();
  syncQuickChips();
  showToast(`「${v}」を追加しました`);
}

async function handleDeleteArea(a){
  POPULAR_AREAS = POPULAR_AREAS.filter(x => x !== a);
  await saveAreas();
  renderAreaManager();
  renderQuickFilters();
  syncQuickChips();
  const areaSel = document.getElementById('af-area');
  if(areaSel) areaSel.innerHTML = areaOptionsHtml(areaSel.value);
  showToast(`「${a}」を削除しました`);
}

/* ---- tag preset manager ---- */
function renderTagManager(){
  const wrap = document.getElementById('tag-manager-list');
  if(!wrap) return;
  if(TAG_PRESETS.length===0){ wrap.innerHTML = `<div class="manager-empty">タグがまだ登録されていません</div>`; return; }
  wrap.innerHTML = TAG_PRESETS.map(t => `
    <span class="manager-chip">${esc(t)}<button type="button" class="chip-x" onclick="handleDeleteTagPreset('${esc(t)}')" title="削除">×</button></span>
  `).join('');
}

async function handleAddTagPreset(){
  const input = document.getElementById('new-tag-input');
  const v = input.value.trim();
  if(!v){ showToast('タグを入力してください'); return; }
  if(TAG_PRESETS.includes(v)){ showToast('すでに登録されています'); return; }
  TAG_PRESETS.push(v);
  await saveTagPresets();
  input.value = '';
  renderTagManager();
  const chipsWrap = document.getElementById('tag-preset-chips');
  const tagsInput = document.getElementById('af-tags');
  if(chipsWrap && tagsInput) chipsWrap.innerHTML = renderTagChipsHtml(tagsInput.value);
  showToast(`「${v}」を追加しました`);
}

async function handleDeleteTagPreset(t){
  TAG_PRESETS = TAG_PRESETS.filter(x => x !== t);
  await saveTagPresets();
  renderTagManager();
  const chipsWrap = document.getElementById('tag-preset-chips');
  const tagsInput = document.getElementById('af-tags');
  if(chipsWrap && tagsInput) chipsWrap.innerHTML = renderTagChipsHtml(tagsInput.value);
  showToast(`「${t}」を削除しました`);
}

function renderAdmin(){
  const el = document.getElementById('admin-content');
  if(!adminUnlocked){
    el.innerHTML = `
      <div class="admin-gate">
        <span class="serif">アドミンログイン</span>
        <div style="color:var(--text-muted); font-size:12.5px; line-height:1.8;">店舗情報の編集にはパスコードが必要です</div>
        <input type="password" id="admin-pass-input" placeholder="パスコードを入力">
        <button class="btn-search" style="width:100%;" onclick="tryAdminLogin()">ログイン</button>
        <div class="hint">デモ用パスコード：goodtable2026</div>
      </div>`;
    return;
  }
  const selected = STORES.find(s=>s.id===adminSelectedId) || null;
  el.innerHTML = `
    <div class="admin-header">
      <h1>掲載店舗の管理</h1>
      <a class="btn-ghost" onclick="go('home')">サイトを見る</a>
    </div>

    <div class="manager-card">
      <h3>人気エリアの管理</h3>
      <div class="field-hint">ホーム画面の「人気エリアから探す」に表示され、店舗登録時のエリア選択肢にもなります。</div>
      <div class="manager-chips" id="area-manager-list"></div>
      <div class="manager-add-row">
        <input type="text" id="new-area-input" placeholder="新しいエリア名（例：目黒）">
        <button type="button" class="btn-manager-add" onclick="handleAddArea()">追加</button>
      </div>
    </div>

    <div class="manager-card">
      <h3>よく使うタグの管理</h3>
      <div class="field-hint">店舗編集フォームでワンクリックで追加・削除できるタグの候補です。</div>
      <div class="manager-chips" id="tag-manager-list"></div>
      <div class="manager-add-row">
        <input type="text" id="new-tag-input" placeholder="新しいタグ（例：貸切可）">
        <button type="button" class="btn-manager-add" onclick="handleAddTagPreset()">追加</button>
      </div>
    </div>

    <div class="admin-panel">
      <div>
        <div class="admin-list" id="admin-list"></div>
        <button class="admin-add" onclick="selectAdminStore(null,true)">＋ 新しい店舗を追加</button>
      </div>
      <div id="admin-form-wrap"></div>
    </div>
  `;
  renderAdminList();
  renderAdminForm(selected);
  renderAreaManager();
  renderTagManager();
}

function tryAdminLogin(){
  const v = document.getElementById('admin-pass-input').value;
  if(v === ADMIN_PASS){
    adminUnlocked = true;
    renderAdmin();
  }else{
    showToast('パスコードが違います');
  }
}

function renderAdminList(){
  const list = document.getElementById('admin-list');
  if(STORES.length===0){ list.innerHTML = `<div style="padding:20px; color:var(--text-faint); font-size:13px;">店舗がまだ登録されていません</div>`; return; }
  list.innerHTML = STORES.map(s=>`
    <div class="admin-row ${s.id===adminSelectedId?'active':''}" onclick="selectAdminStore('${s.id}')">
      <div>
        <div class="rn">${esc(s.name)}</div>
        <div class="ra">${esc(s.area)} ・ ${esc(s.genre)}</div>
      </div>
      <span class="del" onclick="event.stopPropagation(); deleteStore('${s.id}')">削除</span>
    </div>
  `).join('');
}

function selectAdminStore(id, isNew){
  adminSelectedId = isNew ? null : id;
  renderAdminList();
  renderAdminForm(isNew ? emptyStoreDraft() : STORES.find(s=>s.id===id));
}

function renderAdminForm(store){
  const wrap = document.getElementById('admin-form-wrap');
  if(!store){
    wrap.innerHTML = `<div class="admin-form" style="text-align:center; color:var(--text-faint); padding:60px 20px;">左のリストから店舗を選ぶか、<br>「＋ 新しい店舗を追加」を押してください</div>`;
    return;
  }
  const isNew = !store.id;
  adminPhotoDraft = [0,1,2].map(i => (store.photos && store.photos[i]) || '');
  wrap.innerHTML = `
    <div class="admin-form">
      <h3>${isNew ? '新規店舗を追加' : '店舗情報を編集：'+esc(store.name)}</h3>
      <div class="form-grid">
        <div><label>店舗名</label><input id="af-name" value="${esc(store.name)}"></div>
        <div><label>ジャンル</label>
          <select id="af-genre">
            ${GENRE_LIST.map(g=>`<option ${store.genre===g?'selected':''}>${g}</option>`).join('')}
          </select>
        </div>
        <div><label>エリア</label>
          <select id="af-area" size="6" class="scroll-select">
            ${areaOptionsHtml(store.area)}
          </select>
          <div class="field-hint">登録済みの人気エリアからスクロールして選択します（下の「人気エリアの管理」から追加できます）</div>
        </div>
        <div><label>ご予算表記</label><input id="af-wage" value="${esc(store.wage)}" placeholder="例：¥5,000〜¥7,000"></div>
        <div class="full"><label>キャッチコピー</label><input id="af-catch" value="${esc(store.catch)}" placeholder="一覧に表示される一言"></div>
        <div class="full">
          <label>タグ（カンマ区切り）</label>
          <input id="af-tags" value="${esc((store.tags||[]).join(', '))}" placeholder="個室あり, 記念日におすすめ, テラス席あり">
          <div class="field-hint">よく使うタグをクリックすると追加・削除できます</div>
          <div class="tag-preset-chips" id="tag-preset-chips">${renderTagChipsHtml((store.tags||[]).join(', '))}</div>
        </div>
        <div class="full">
          <label>店舗写真（最大3枚）</label>
          <div class="photo-slots" id="photo-slots"></div>
          <div class="field-hint">クリック、またはドラッグ＆ドロップで画像をアップロードできます。1枚目が一覧・詳細ページのメイン写真になります。</div>
        </div>
        <div class="full"><label>住所</label><input id="af-address" value="${esc(store.address)}" placeholder="例：東京都中央区銀座5-3-1"></div>
        <div><label>営業時間</label><input id="af-hours" value="${esc(store.hours)}" placeholder="例：11:30〜15:00 / 18:00〜22:30"></div>
        <div><label>定休日</label><input id="af-closed" value="${esc(store.closedDay)}" placeholder="例：月曜日"></div>
        <div><label>Instagram URL</label><input id="af-ig" value="${esc(store.instagram)}" placeholder="https://www.instagram.com/..."></div>
        <div><label>TikTok URL</label><input id="af-tt" value="${esc(store.tiktok)}" placeholder="https://www.tiktok.com/..."></div>
        <div><label>電話番号</label><input id="af-phone" value="${esc(store.phone)}"></div>
        <div><label>予約受付メール</label><input id="af-email" value="${esc(store.email)}"></div>
        <div class="full"><label>LINE 相談先URL</label><input id="af-line" value="${esc(store.lineUrl)}" placeholder="https://line.me/... または https://lin.ee/...">
          <div class="field-hint">入力すると詳細ページに「LINEで問い合わせる」ボタンが表示されます</div>
        </div>
        <div><label>緯度（任意）</label><input id="af-lat" value="${store.lat!=null?esc(String(store.lat)):''}" placeholder="例：35.6717">
          <div class="field-hint">未入力ならエリア名から自動推定</div>
        </div>
        <div><label>経度（任意）</label><input id="af-lng" value="${store.lng!=null?esc(String(store.lng)):''}" placeholder="例：139.7650"></div>
        <div class="full"><label>店舗詳細文</label><textarea id="af-desc" placeholder="お店の雰囲気、こだわり、こんなシーンにおすすめ...等">${esc(store.description)}</textarea></div>
      </div>
      <div class="form-actions">
        <button class="btn-save" onclick="saveAdminForm('${isNew?'':store.id}')">保存する</button>
        <span class="save-flag" id="save-flag">保存しました</span>
      </div>
    </div>
  `;
  renderPhotoSlots();
}

async function saveAdminForm(existingId){
  const draft = {
    id: existingId || ('s' + Date.now()),
    name: document.getElementById('af-name').value.trim() || '無題の店舗',
    genre: document.getElementById('af-genre').value,
    area: document.getElementById('af-area').value.trim(),
    wage: document.getElementById('af-wage').value.trim(),
    catch: document.getElementById('af-catch').value.trim(),
    tags: document.getElementById('af-tags').value.split(',').map(t=>t.trim()).filter(Boolean),
    photos: adminPhotoDraft.filter(Boolean),
    address: document.getElementById('af-address').value.trim(),
    hours: document.getElementById('af-hours').value.trim(),
    closedDay: document.getElementById('af-closed').value.trim(),
    instagram: document.getElementById('af-ig').value.trim(),
    tiktok: document.getElementById('af-tt').value.trim(),
    lineUrl: document.getElementById('af-line').value.trim(),
    phone: document.getElementById('af-phone').value.trim(),
    email: document.getElementById('af-email').value.trim(),
    description: document.getElementById('af-desc').value.trim(),
    lat: (()=>{ const v=document.getElementById('af-lat').value.trim(); return v===''? null : parseFloat(v); })(),
    lng: (()=>{ const v=document.getElementById('af-lng').value.trim(); return v===''? null : parseFloat(v); })(),
  };
  const idx = STORES.findIndex(s=>s.id===draft.id);
  if(idx >= 0) STORES[idx] = draft; else STORES.push(draft);

  const ok = await saveStores();
  adminSelectedId = draft.id;
  populateFilterOptions();
  applyFilters();
  renderAdminList();
  const flag = document.getElementById('save-flag');
  flag.textContent = ok ? '保存しました' : '保存に失敗しました（再試行してください）';
  flag.classList.add('show');
  setTimeout(()=>flag.classList.remove('show'), 2200);
  showToast(ok ? `「${draft.name}」を保存しました` : '保存に失敗しました');
}

async function deleteStore(id){
  const s = STORES.find(x=>x.id===id);
  if(!s) return;
  if(!confirm(`「${s.name}」を削除しますか？`)) return;
  STORES = STORES.filter(x=>x.id!==id);
  if(adminSelectedId === id) adminSelectedId = null;
  await saveStores();
  populateFilterOptions();
  applyFilters();
  renderAdmin();
  showToast('店舗を削除しました');
}

/* ---------------- boot ---------------- */
(async function init(){
  document.getElementById('store-grid').innerHTML = `<div class="empty"><span class="serif">読み込み中...</span></div>`;
  await Promise.all([loadStores(), loadAreas(), loadTagPresets()]);
  populateFilterOptions();
  renderQuickFilters();
  applyFilters();
  document.getElementById('f-keyword').addEventListener('keydown', e=>{ if(e.key==='Enter') applyFilters(); });
})();

</script>
</body>
</html>
