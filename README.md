# instituto-mutante-rpg
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Instituto Mutante — RPG</title>
  <link href="https://fonts.googleapis.com/css2?family=Rajdhani:wght@400;500;600;700&family=Orbitron:wght@400;700;900&family=Exo+2:ital,wght@0,300;0,400;0,600;1,300&display=swap" rel="stylesheet"/>
  <style>
    :root {
      --bg: #08080f;
      --bg2: #0d0d1a;
      --bg3: #111122;
      --panel: rgba(15,15,35,0.92);
      --accent: #00e5ff;
      --accent2: #7c3aed;
      --accent3: #f0abfc;
      --gold: #fbbf24;
      --danger: #ef4444;
      --text: #e2e8f0;
      --muted: #94a3b8;
      --border: rgba(0,229,255,0.18);
      --glow: 0 0 18px rgba(0,229,255,0.35), 0 0 40px rgba(0,229,255,0.12);
      --glow2: 0 0 18px rgba(124,58,237,0.45), 0 0 40px rgba(124,58,237,0.15);
      --nav-h: 68px;
    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    html { scroll-behavior: smooth; }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'Exo 2', sans-serif;
      font-weight: 300;
      line-height: 1.7;
      min-height: 100vh;
      overflow-x: hidden;
    }

    /* ── BACKGROUND PARTICLES ── */
    #bg-canvas {
      position: fixed; top: 0; left: 0;
      width: 100%; height: 100%;
      pointer-events: none; z-index: 0;
      opacity: 0.35;
    }

    /* ── SCANLINE OVERLAY ── */
    body::after {
      content: '';
      position: fixed; inset: 0;
      background: repeating-linear-gradient(
        0deg,
        transparent,
        transparent 2px,
        rgba(0,0,0,0.07) 2px,
        rgba(0,0,0,0.07) 4px
      );
      pointer-events: none;
      z-index: 9999;
    }

    /* ── NAV ── */
    nav {
      position: fixed; top: 0; left: 0; right: 0;
      height: var(--nav-h);
      background: rgba(8,8,15,0.95);
      border-bottom: 1px solid var(--border);
      display: flex; align-items: center;
      padding: 0 2rem;
      z-index: 1000;
      backdrop-filter: blur(14px);
      box-shadow: 0 2px 40px rgba(0,229,255,0.08);
    }

    .nav-logo {
      font-family: 'Orbitron', sans-serif;
      font-weight: 900;
      font-size: 1.15rem;
      letter-spacing: 0.12em;
      color: var(--accent);
      text-shadow: var(--glow);
      white-space: nowrap;
      margin-right: 2.5rem;
      text-decoration: none;
      flex-shrink: 0;
    }
    .nav-logo span { color: var(--accent3); }

    .nav-links {
      display: flex; align-items: center;
      list-style: none;
      gap: 0;
      flex: 1;
    }

    .nav-links > li {
      position: relative;
    }

    .nav-links > li > a,
    .nav-links > li > span {
      display: flex; align-items: center; gap: 5px;
      padding: 0 1.05rem;
      height: var(--nav-h);
      font-family: 'Rajdhani', sans-serif;
      font-weight: 600;
      font-size: 0.82rem;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      color: var(--muted);
      text-decoration: none;
      cursor: pointer;
      transition: color 0.2s;
      white-space: nowrap;
      user-select: none;
    }

    .nav-links > li > a:hover,
    .nav-links > li > span:hover,
    .nav-links > li:hover > a,
    .nav-links > li:hover > span {
      color: var(--accent);
    }

    .nav-links > li > a.active { color: var(--accent); }

    .chevron {
      width: 10px; height: 10px;
      border-right: 2px solid currentColor;
      border-bottom: 2px solid currentColor;
      transform: rotate(45deg) translateY(-2px);
      display: inline-block;
      transition: transform 0.2s;
    }
    .nav-links > li:hover .chevron { transform: rotate(225deg) translateY(-2px); }

    /* Dropdown */
    .dropdown {
      position: absolute;
      top: calc(var(--nav-h) - 4px);
      left: 0;
      min-width: 190px;
      background: rgba(10,10,22,0.97);
      border: 1px solid var(--border);
      border-top: 2px solid var(--accent);
      box-shadow: var(--glow), 0 20px 60px rgba(0,0,0,0.7);
      opacity: 0; visibility: hidden;
      transform: translateY(-6px);
      transition: opacity 0.22s, transform 0.22s, visibility 0.22s;
      z-index: 100;
      backdrop-filter: blur(18px);
    }

    .nav-links > li:hover .dropdown {
      opacity: 1; visibility: visible; transform: translateY(0);
    }

    .dropdown a {
      display: block;
      padding: 0.6rem 1.2rem;
      font-family: 'Rajdhani', sans-serif;
      font-weight: 500;
      font-size: 0.8rem;
      letter-spacing: 0.09em;
      text-transform: uppercase;
      color: var(--muted);
      text-decoration: none;
      transition: color 0.15s, background 0.15s, padding-left 0.15s;
      border-bottom: 1px solid rgba(255,255,255,0.04);
    }
    .dropdown a:last-child { border-bottom: none; }
    .dropdown a:hover {
      color: var(--accent);
      background: rgba(0,229,255,0.06);
      padding-left: 1.6rem;
    }

    .nav-hamburger {
      display: none;
      flex-direction: column; gap: 5px;
      cursor: pointer; margin-left: auto;
    }
    .nav-hamburger span {
      display: block; width: 26px; height: 2px;
      background: var(--accent); border-radius: 2px;
      transition: 0.3s;
    }

    /* ── MAIN CONTENT ── */
    main {
      position: relative; z-index: 1;
      padding-top: var(--nav-h);
    }

    /* ── PAGES ── */
    .page { display: none; }
    .page.active { display: block; }

    /* ── HERO ── */
    #hero {
      position: relative;
      min-height: calc(100vh - var(--nav-h));
      display: flex; flex-direction: column;
      align-items: center; justify-content: center;
      text-align: center;
      overflow: hidden;
      padding: 4rem 2rem;
    }

    .hero-grid-bg {
      position: absolute; inset: 0;
      background-image:
        linear-gradient(rgba(0,229,255,0.04) 1px, transparent 1px),
        linear-gradient(90deg, rgba(0,229,255,0.04) 1px, transparent 1px);
      background-size: 60px 60px;
      mask-image: radial-gradient(ellipse 80% 70% at 50% 50%, black 40%, transparent 100%);
    }

    .hero-glow-orb {
      position: absolute;
      border-radius: 50%;
      filter: blur(80px);
      pointer-events: none;
    }
    .hero-glow-orb.a {
      width: 500px; height: 500px;
      background: radial-gradient(circle, rgba(124,58,237,0.3) 0%, transparent 70%);
      top: -100px; left: -100px;
      animation: floatOrb 8s ease-in-out infinite;
    }
    .hero-glow-orb.b {
      width: 600px; height: 400px;
      background: radial-gradient(circle, rgba(0,229,255,0.2) 0%, transparent 70%);
      bottom: -80px; right: -80px;
      animation: floatOrb 10s ease-in-out infinite reverse;
    }

    @keyframes floatOrb {
      0%,100% { transform: translate(0,0); }
      50% { transform: translate(30px, 20px); }
    }

    .hero-badge {
      display: inline-flex; align-items: center; gap: 8px;
      border: 1px solid var(--border);
      background: rgba(0,229,255,0.06);
      color: var(--accent);
      font-family: 'Rajdhani', sans-serif;
      font-weight: 600; font-size: 0.72rem;
      letter-spacing: 0.2em; text-transform: uppercase;
      padding: 0.4rem 1rem; border-radius: 2px;
      margin-bottom: 1.8rem;
      animation: fadeInUp 0.6s ease both;
    }
    .hero-badge::before {
      content: '';
      width: 6px; height: 6px;
      background: var(--accent);
      border-radius: 50%;
      box-shadow: 0 0 8px var(--accent);
      animation: pulse 1.5s ease infinite;
    }
    @keyframes pulse { 0%,100%{opacity:1;transform:scale(1)} 50%{opacity:.5;transform:scale(1.4)} }

    .hero-title {
      font-family: 'Orbitron', sans-serif;
      font-weight: 900;
      font-size: clamp(2.8rem, 7vw, 6rem);
      letter-spacing: -0.01em;
      line-height: 1.05;
      margin-bottom: 0.3rem;
      animation: fadeInUp 0.7s 0.1s ease both;
    }
    .hero-title .line1 { color: var(--text); }
    .hero-title .line2 {
      color: transparent;
      -webkit-text-stroke: 2px var(--accent);
      text-shadow: none;
    }

    .hero-subtitle {
      font-size: 1.05rem;
      color: var(--muted);
      max-width: 560px;
      margin: 1.5rem auto 2.8rem;
      font-weight: 300;
      animation: fadeInUp 0.7s 0.2s ease both;
    }

    .hero-cta-group {
      display: flex; gap: 1rem; flex-wrap: wrap;
      justify-content: center;
      animation: fadeInUp 0.7s 0.3s ease both;
    }

    .btn {
      display: inline-flex; align-items: center; gap: 8px;
      font-family: 'Rajdhani', sans-serif;
      font-weight: 700; font-size: 0.85rem;
      letter-spacing: 0.15em; text-transform: uppercase;
      text-decoration: none;
      padding: 0.75rem 1.8rem;
      border-radius: 2px;
      cursor: pointer;
      border: none;
      transition: all 0.2s;
    }
    .btn-primary {
      background: var(--accent);
      color: var(--bg);
      box-shadow: var(--glow);
    }
    .btn-primary:hover {
      background: #33edff;
      transform: translateY(-2px);
      box-shadow: 0 0 30px rgba(0,229,255,0.6);
    }
    .btn-outline {
      background: transparent;
      color: var(--accent);
      border: 1px solid var(--accent);
    }
    .btn-outline:hover {
      background: rgba(0,229,255,0.08);
      transform: translateY(-2px);
    }

    .hero-stats {
      display: flex; gap: 3rem; justify-content: center;
      margin-top: 4rem;
      animation: fadeInUp 0.7s 0.4s ease both;
    }
    .hero-stat { text-align: center; }
    .hero-stat .num {
      font-family: 'Orbitron', sans-serif;
      font-size: 2rem; font-weight: 900;
      color: var(--accent);
      text-shadow: var(--glow);
    }
    .hero-stat .lbl {
      font-size: 0.7rem;
      letter-spacing: 0.18em;
      text-transform: uppercase;
      color: var(--muted);
      font-family: 'Rajdhani', sans-serif;
    }

    @keyframes fadeInUp {
      from { opacity: 0; transform: translateY(24px); }
      to   { opacity: 1; transform: translateY(0); }
    }

    /* ── SECTION COMMON ── */
    .section {
      padding: 5rem 2rem;
      max-width: 1100px;
      margin: 0 auto;
    }

    .section-header {
      margin-bottom: 3rem;
    }

    .section-label {
      font-family: 'Rajdhani', sans-serif;
      font-weight: 600;
      font-size: 0.72rem;
      letter-spacing: 0.25em;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 0.6rem;
      display: flex; align-items: center; gap: 10px;
    }
    .section-label::before {
      content: '';
      display: inline-block;
      width: 30px; height: 1px;
      background: var(--accent);
    }

    .section-title {
      font-family: 'Orbitron', sans-serif;
      font-size: clamp(1.6rem, 3.5vw, 2.5rem);
      font-weight: 700;
      color: var(--text);
      margin-bottom: 1rem;
    }

    .section-desc {
      color: var(--muted);
      max-width: 620px;
      font-size: 0.95rem;
    }

    /* ── DIVIDER ── */
    .divider {
      height: 1px;
      background: linear-gradient(90deg, transparent, var(--border), var(--accent), var(--border), transparent);
      margin: 0;
    }

    /* ── CARDS ── */
    .card-grid {
      display: grid;
      gap: 1.2rem;
    }
    .card-grid-2 { grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); }
    .card-grid-3 { grid-template-columns: repeat(auto-fit, minmax(240px, 1fr)); }

    .card {
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 3px;
      padding: 1.8rem;
      position: relative;
      overflow: hidden;
      transition: border-color 0.2s, transform 0.2s, box-shadow 0.2s;
    }
    .card::before {
      content: '';
      position: absolute; top: 0; left: 0; right: 0;
      height: 2px;
      background: linear-gradient(90deg, transparent, var(--accent), transparent);
      opacity: 0;
      transition: opacity 0.2s;
    }
    .card:hover { border-color: rgba(0,229,255,0.4); transform: translateY(-3px); box-shadow: var(--glow); }
    .card:hover::before { opacity: 1; }

    .card-icon {
      font-size: 2.2rem; margin-bottom: 1rem;
    }
    .card-title {
      font-family: 'Rajdhani', sans-serif;
      font-weight: 700; font-size: 1.15rem;
      letter-spacing: 0.08em; text-transform: uppercase;
      color: var(--accent); margin-bottom: 0.5rem;
    }
    .card-text { font-size: 0.9rem; color: var(--muted); line-height: 1.65; }

    /* ── TAG ── */
    .tag {
      display: inline-block;
      font-family: 'Rajdhani', sans-serif;
      font-size: 0.68rem; font-weight: 600;
      letter-spacing: 0.14em; text-transform: uppercase;
      padding: 0.25rem 0.65rem;
      border-radius: 2px;
      margin: 2px;
    }
    .tag-cyan { background: rgba(0,229,255,0.1); color: var(--accent); border: 1px solid rgba(0,229,255,0.25); }
    .tag-purple { background: rgba(124,58,237,0.15); color: var(--accent3); border: 1px solid rgba(124,58,237,0.3); }
    .tag-gold { background: rgba(251,191,36,0.1); color: var(--gold); border: 1px solid rgba(251,191,36,0.25); }
    .tag-red { background: rgba(239,68,68,0.1); color: var(--danger); border: 1px solid rgba(239,68,68,0.25); }

    /* ── POWER TABLE ── */
    .power-table {
      width: 100%;
      border-collapse: collapse;
      font-size: 0.9rem;
    }
    .power-table th {
      font-family: 'Rajdhani', sans-serif;
      font-weight: 700;
      font-size: 0.75rem;
      letter-spacing: 0.15em;
      text-transform: uppercase;
      color: var(--accent);
      padding: 0.8rem 1rem;
      text-align: left;
      border-bottom: 1px solid var(--border);
      background: rgba(0,229,255,0.04);
    }
    .power-table td {
      padding: 0.75rem 1rem;
      border-bottom: 1px solid rgba(255,255,255,0.04);
      color: var(--muted);
      vertical-align: top;
    }
    .power-table td:first-child { color: var(--text); font-weight: 400; }
    .power-table tr:hover td { background: rgba(0,229,255,0.03); }

    /* ── TIMELINE ── */
    .timeline { position: relative; padding-left: 2.5rem; }
    .timeline::before {
      content: '';
      position: absolute; left: 0; top: 8px; bottom: 8px;
      width: 1px;
      background: linear-gradient(to bottom, var(--accent), var(--accent2), transparent);
    }
    .timeline-item {
      position: relative;
      margin-bottom: 2.5rem;
    }
    .timeline-item::before {
      content: '';
      position: absolute; left: -2.5rem; top: 6px;
      width: 10px; height: 10px;
      background: var(--accent);
      border-radius: 50%;
      box-shadow: 0 0 12px var(--accent);
      transform: translateX(-4px);
    }
    .timeline-year {
      font-family: 'Orbitron', sans-serif;
      font-size: 0.75rem;
      color: var(--accent);
      margin-bottom: 0.3rem;
      letter-spacing: 0.1em;
    }
    .timeline-title {
      font-family: 'Rajdhani', sans-serif;
      font-weight: 700; font-size: 1.05rem;
      letter-spacing: 0.06em; text-transform: uppercase;
      margin-bottom: 0.4rem;
    }
    .timeline-text { font-size: 0.9rem; color: var(--muted); }

    /* ── LOJA ── */
    .shop-item {
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 3px;
      padding: 1.5rem;
      display: flex; flex-direction: column;
      gap: 0.7rem;
      transition: all 0.2s;
      position: relative;
      overflow: hidden;
    }
    .shop-item:hover { border-color: var(--gold); transform: translateY(-3px); box-shadow: 0 0 20px rgba(251,191,36,0.15); }
    .shop-item-header { display: flex; justify-content: space-between; align-items: flex-start; }
    .shop-item-name {
      font-family: 'Rajdhani', sans-serif;
      font-weight: 700; font-size: 1.05rem;
      letter-spacing: 0.07em; text-transform: uppercase;
    }
    .shop-item-price {
      font-family: 'Orbitron', sans-serif;
      font-size: 0.9rem; font-weight: 700;
      color: var(--gold);
    }
    .shop-item-desc { font-size: 0.87rem; color: var(--muted); }

    /* ── GUIA STEPS ── */
    .steps { display: flex; flex-direction: column; gap: 1rem; }
    .step {
      display: flex; gap: 1.5rem; align-items: flex-start;
      padding: 1.5rem;
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 3px;
      transition: all 0.2s;
    }
    .step:hover { border-color: rgba(0,229,255,0.35); }
    .step-num {
      font-family: 'Orbitron', sans-serif;
      font-size: 1.5rem; font-weight: 900;
      color: transparent;
      -webkit-text-stroke: 1.5px var(--accent);
      min-width: 48px; line-height: 1;
    }
    .step-body {}
    .step-title {
      font-family: 'Rajdhani', sans-serif;
      font-weight: 700; font-size: 1rem;
      letter-spacing: 0.08em; text-transform: uppercase;
      color: var(--text); margin-bottom: 0.3rem;
    }
    .step-text { font-size: 0.88rem; color: var(--muted); }

    /* ── REGRAS ACCORDION ── */
    .accordion-item {
      border: 1px solid var(--border);
      border-radius: 3px;
      margin-bottom: 0.6rem;
      overflow: hidden;
    }
    .accordion-header {
      display: flex; justify-content: space-between; align-items: center;
      padding: 1rem 1.4rem;
      background: var(--panel);
      cursor: pointer;
      font-family: 'Rajdhani', sans-serif;
      font-weight: 700;
      font-size: 0.95rem;
      letter-spacing: 0.08em;
      text-transform: uppercase;
      transition: color 0.15s, background 0.15s;
    }
    .accordion-header:hover { color: var(--accent); background: rgba(0,229,255,0.04); }
    .accordion-header.open { color: var(--accent); border-bottom: 1px solid var(--border); }
    .accordion-arrow {
      width: 10px; height: 10px;
      border-right: 2px solid currentColor;
      border-bottom: 2px solid currentColor;
      transform: rotate(45deg);
      transition: transform 0.25s;
      flex-shrink: 0;
    }
    .accordion-header.open .accordion-arrow { transform: rotate(225deg); }
    .accordion-body {
      display: none;
      padding: 1.2rem 1.4rem;
      font-size: 0.9rem; color: var(--muted);
      background: rgba(8,8,15,0.6);
      line-height: 1.75;
    }
    .accordion-body.open { display: block; }

    /* ── ENTIDADES ── */
    .entity-card {
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 3px;
      padding: 1.5rem;
      transition: all 0.2s;
    }
    .entity-card:hover { border-color: rgba(240,171,252,0.4); box-shadow: var(--glow2); transform: translateY(-3px); }
    .entity-title {
      font-family: 'Rajdhani', sans-serif;
      font-weight: 700; font-size: 1.1rem;
      letter-spacing: 0.1em; text-transform: uppercase;
      color: var(--accent3); margin-bottom: 0.5rem;
    }
    .entity-origin {
      font-size: 0.75rem;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      color: var(--muted);
      font-family: 'Rajdhani', sans-serif;
      margin-bottom: 0.6rem;
    }
    .entity-desc { font-size: 0.87rem; color: var(--muted); }

    /* ── CRÉDITOS ── */
    .credits-grid { display: flex; flex-direction: column; gap: 1.5rem; }
    .credit-block {
      background: var(--panel);
      border: 1px solid var(--border);
      border-left: 3px solid var(--accent2);
      padding: 1.4rem 1.8rem;
      border-radius: 0 3px 3px 0;
    }
    .credit-role {
      font-family: 'Rajdhani', sans-serif;
      font-weight: 600; font-size: 0.72rem;
      letter-spacing: 0.2em; text-transform: uppercase;
      color: var(--accent3); margin-bottom: 0.3rem;
    }
    .credit-name {
      font-family: 'Orbitron', sans-serif;
      font-size: 1rem; font-weight: 700;
      color: var(--text); margin-bottom: 0.4rem;
    }
    .credit-note { font-size: 0.87rem; color: var(--muted); }

    /* ── PAGE HERO ── */
    .page-hero {
      background: linear-gradient(135deg, rgba(124,58,237,0.12) 0%, rgba(0,229,255,0.06) 100%);
      border-bottom: 1px solid var(--border);
      padding: 3.5rem 2rem 3rem;
      text-align: center;
      position: relative;
      overflow: hidden;
    }
    .page-hero::before {
      content: '';
      position: absolute; inset: 0;
      background-image:
        linear-gradient(rgba(0,229,255,0.03) 1px, transparent 1px),
        linear-gradient(90deg, rgba(0,229,255,0.03) 1px, transparent 1px);
      background-size: 40px 40px;
    }
    .page-hero-title {
      font-family: 'Orbitron', sans-serif;
      font-weight: 900;
      font-size: clamp(1.8rem, 4vw, 3rem);
      position: relative;
    }
    .page-hero-sub {
      color: var(--muted); font-size: 0.95rem; margin-top: 0.6rem;
      position: relative;
    }

    /* ── SCROLLBAR ── */
    ::-webkit-scrollbar { width: 6px; }
    ::-webkit-scrollbar-track { background: var(--bg); }
    ::-webkit-scrollbar-thumb { background: var(--accent2); border-radius: 3px; }

    /* ── HIGHLIGHT BOX ── */
    .highlight-box {
      border: 1px solid rgba(0,229,255,0.2);
      background: rgba(0,229,255,0.04);
      border-left: 3px solid var(--accent);
      padding: 1.2rem 1.5rem;
      border-radius: 0 3px 3px 0;
      margin: 1.5rem 0;
    }
    .highlight-box.warn {
      border-left-color: var(--gold);
      background: rgba(251,191,36,0.04);
      border-color: rgba(251,191,36,0.2);
    }
    .highlight-box p { font-size: 0.9rem; color: var(--muted); }
    .highlight-box strong { color: var(--text); }

    /* ── FOOTER ── */
    footer {
      position: relative; z-index: 1;
      border-top: 1px solid var(--border);
      background: rgba(8,8,15,0.97);
      padding: 2.5rem 2rem;
      text-align: center;
    }
    footer .footer-logo {
      font-family: 'Orbitron', sans-serif;
      font-weight: 900; font-size: 1rem;
      color: var(--accent); letter-spacing: 0.12em;
      margin-bottom: 0.6rem;
    }
    footer p {
      font-size: 0.8rem; color: var(--muted);
      font-family: 'Rajdhani', sans-serif;
      letter-spacing: 0.08em;
    }

    /* ── MOBILE ── */
    @media (max-width: 768px) {
      .nav-links { display: none; }
      .nav-links.open {
        display: flex; flex-direction: column;
        position: fixed; top: var(--nav-h); left: 0; right: 0;
        background: rgba(8,8,15,0.99);
        border-top: 1px solid var(--border);
        padding: 1rem 0;
        overflow-y: auto;
        max-height: calc(100vh - var(--nav-h));
      }
      .nav-links > li > a,
      .nav-links > li > span {
        height: auto; padding: 0.75rem 1.5rem;
      }
      .nav-hamburger { display: flex; }
      .dropdown {
        position: static;
        border-top: none;
        opacity: 1; visibility: visible;
        transform: none;
        box-shadow: none;
        border: none;
        background: rgba(0,229,255,0.03);
        display: none;
      }
      .dropdown.open { display: block; }
      .hero-stats { gap: 1.5rem; }
      .hero-stat .num { font-size: 1.5rem; }
    }
  </style>
</head>
<body>

<canvas id="bg-canvas"></canvas>

<!-- ── NAVIGATION ── -->
<nav>
  <a class="nav-logo" onclick="showPage('inicio')">INSTITUTO <span>MUTANTE</span></a>
  <ul class="nav-links" id="navLinks">
    <li><a onclick="showPage('inicio')" class="active" id="nav-inicio">Início</a></li>
    <li>
      <span id="nav-acampamento" onclick="showPage('acampamento')">Acampamento <i class="chevron"></i></span>
      <div class="dropdown">
        <a onclick="showPage('historia')">História</a>
        <a onclick="showPage('loja')">Loja</a>
      </div>
    </li>
    <li>
      <span id="nav-aprenda" onclick="showPage('aprenda')">Aprenda a Jogar <i class="chevron"></i></span>
      <div class="dropdown">
        <a onclick="showPage('guia')">Guia do Novato</a>
        <a onclick="showPage('regras')">Regras</a>
      </div>
    </li>
    <li>
      <span id="nav-mutacoes" onclick="showPage('mutacoes')">Mutações <i class="chevron"></i></span>
      <div class="dropdown">
        <a onclick="showPage('poderes')">Poderes</a>
        <a onclick="showPage('subpoderes')">Subpoderes</a>
      </div>
    </li>
    <li><a id="nav-entidades" onclick="showPage('entidades')">Entidades</a></li>
    <li><a id="nav-creditos" onclick="showPage('creditos')">Créditos</a></li>
  </ul>
  <div class="nav-hamburger" id="hamburger" onclick="toggleMenu()">
    <span></span><span></span><span></span>
  </div>
</nav>

<!-- ── MAIN ── -->
<main>

  <!-- INÍCIO -->
  <div id="page-inicio" class="page active">
    <section id="hero">
      <div class="hero-grid-bg"></div>
      <div class="hero-glow-orb a"></div>
      <div class="hero-glow-orb b"></div>
      <div class="hero-badge">Sistema de RPG por Texto — Comunidade Ativa</div>
      <h1 class="hero-title">
        <div class="line1">INSTITUTO</div>
        <div class="line2">MUTANTE</div>
      </h1>
      <p class="hero-subtitle">
        Um universo de poderes, dilemas e escolhas. Aqui, mutantes encontram seu lugar — ou constroem um.
        Bem-vindo ao nosso RPG colaborativo por texto.
      </p>
      <div class="hero-cta-group">
        <a class="btn btn-primary" onclick="showPage('guia')">▶ Começar a Jogar</a>
        <a class="btn btn-outline" onclick="showPage('acampamento')">Explorar</a>
      </div>
      <div class="hero-stats">
        <div class="hero-stat"><div class="num">40+</div><div class="lbl">Poderes</div></div>
        <div class="hero-stat"><div class="num">15+</div><div class="lbl">Entidades</div></div>
        <div class="hero-stat"><div class="num">∞</div><div class="lbl">Histórias</div></div>
      </div>
    </section>

    <div class="divider"></div>

    <div class="section">
      <div class="section-header">
        <div class="section-label">O que é</div>
        <h2 class="section-title">Sobre o Instituto</h2>
        <p class="section-desc">Um RPG colaborativo por texto ambientado em um mundo onde a mutação humana é real — e nem sempre bem-vinda.</p>
      </div>
      <div class="card-grid card-grid-3">
        <div class="card">
          <div class="card-icon">🧬</div>
          <div class="card-title">Mutações Únicas</div>
          <div class="card-text">Escolha entre dezenas de poderes cuidadosamente balanceados, divididos por categorias e níveis de raridade.</div>
        </div>
        <div class="card">
          <div class="card-icon">⚡</div>
          <div class="card-title">Narrativa Viva</div>
          <div class="card-text">O Instituto é mais do que um cenário — é uma comunidade que evolui com as escolhas de cada jogador.</div>
        </div>
        <div class="card">
          <div class="card-icon">🌐</div>
          <div class="card-title">Entidades & Aliados</div>
          <div class="card-text">Explore facções, entidades sobrenaturais e organizações que moldam o mundo além dos muros do Instituto.</div>
        </div>
        <div class="card">
          <div class="card-icon">📜</div>
          <div class="card-title">Regras Claras</div>
          <div class="card-text">Sistema de regras pensado para liberdade criativa com equilíbrio narrativo e justo para todos.</div>
        </div>
        <div class="card">
          <div class="card-icon">🏪</div>
          <div class="card-title">Sistema de Loja</div>
          <div class="card-text">Adquira itens, habilidades extras e privilégios especiais com moedas ganhas em aventuras.</div>
        </div>
        <div class="card">
          <div class="card-icon">🎭</div>
          <div class="card-title">Comunidade</div>
          <div class="card-text">Jogadores criativos, staff dedicado e histórias que ficam na memória. Venha fazer parte disso.</div>
        </div>
      </div>
    </div>
  </div>

  <!-- ACAMPAMENTO -->
  <div id="page-acampamento" class="page">
    <div class="page-hero">
      <p class="section-label" style="justify-content:center">Acampamento</p>
      <h1 class="page-hero-title">O Instituto</h1>
      <p class="page-hero-sub">Conheça o coração do nosso RPG</p>
    </div>
    <div class="section">
      <div class="card-grid card-grid-2">
        <div class="card">
          <div class="card-icon">📖</div>
          <div class="card-title">História</div>
          <div class="card-text">A origem do Instituto, os eventos que o moldaram e os segredos que ainda guarda. Clique abaixo para explorar a linha do tempo completa.</div>
          <br/><a class="btn btn-outline" style="font-size:0.78rem;padding:0.55rem 1.2rem" onclick="showPage('historia')">Ver História →</a>
        </div>
        <div class="card">
          <div class="card-icon">🏪</div>
          <div class="card-title">Loja</div>
          <div class="card-text">Itens, poderes extras e privilégios exclusivos disponíveis para os membros do Instituto. Gaste seus pontos com sabedoria.</div>
          <br/><a class="btn btn-outline" style="font-size:0.78rem;padding:0.55rem 1.2rem" onclick="showPage('loja')">Visitar Loja →</a>
        </div>
      </div>
    </div>
  </div>

  <!-- HISTÓRIA -->
  <div id="page-historia" class="page">
    <div class="page-hero">
      <p class="section-label" style="justify-content:center">Acampamento → História</p>
      <h1 class="page-hero-title">História do Instituto</h1>
      <p class="page-hero-sub">Do primeiro mutante registado até os dias atuais</p>
    </div>
    <div class="section">
      <div class="highlight-box">
        <p><strong>Nota de Lore:</strong> Toda a história abaixo é fictícia e parte integrante do universo do RPG. Nomes, datas e eventos foram criados pela staff para enriquecer a narrativa.</p>
      </div>
      <div class="timeline">
        <div class="timeline-item">
          <div class="timeline-year">Ano 0 — A Primeira Chama</div>
          <div class="timeline-title">Surgimento dos Primeiros Mutantes</div>
          <div class="timeline-text">Os primeiros registros de mutações humanas surgem em pequenas comunidades ao redor do globo. A maioria é silenciada, escondida ou perseguida. Uma minoria encontra refúgio nas sombras.</div>
        </div>
        <div class="timeline-item">
          <div class="timeline-year">Ano 12 — A Fundação</div>
          <div class="timeline-title">Criação do Instituto</div>
          <div class="timeline-text">O Dr. Elias Vorn, ele mesmo portador de uma rara mutação cognitiva, reúne seis mutantes sobreviventes e funda o Instituto em uma propriedade isolada. O objetivo: um lar. Uma fortaleza. Um recomeço.</div>
        </div>
        <div class="timeline-item">
          <div class="timeline-year">Ano 28 — A Guerra das Sombras</div>
          <div class="timeline-title">Conflito com a Ordem Vigil</div>
          <div class="timeline-text">A organização paramilitar Vigil declara os mutantes uma ameaça à ordem. O Instituto enfrenta sua primeira grande crise — e emerge mais forte, forjado no conflito. Nascem os primeiros Guardiões.</div>
        </div>
        <div class="timeline-item">
          <div class="timeline-year">Ano 35 — Renascimento</div>
          <div class="timeline-title">Expansão e Novos Programas</div>
          <div class="timeline-text">Com a Vigil enfraquecida, o Instituto expande suas instalações, cria programas de treinamento formais e abre suas portas para novas gerações de mutantes. A academia nasce oficialmente.</div>
        </div>
        <div class="timeline-item">
          <div class="timeline-year">Hoje — O Presente</div>
          <div class="timeline-title">Você Entra em Cena</div>
          <div class="timeline-text">O Instituto existe há décadas, mas o perigo nunca cessou. Novas ameaças emergem, alianças se formam e rompem, e cada mutante que adentra os portões carrega o peso de um destino por escrever.</div>
        </div>
      </div>
    </div>
  </div>

  <!-- LOJA -->
  <div id="page-loja" class="page">
    <div class="page-hero">
      <p class="section-label" style="justify-content:center">Acampamento → Loja</p>
      <h1 class="page-hero-title">Loja do Instituto</h1>
      <p class="page-hero-sub">Gaste seus pontos com inteligência, mutante</p>
    </div>
    <div class="section">
      <div class="highlight-box warn">
        <p><strong>Como ganhar pontos:</strong> Participe de narrativas, complete missões e interaja ativamente na comunidade para acumular pontos de Instituto.</p>
      </div>

      <div class="section-header" style="margin-top:2.5rem">
        <div class="section-label">Categoria</div>
        <h2 class="section-title" style="font-size:1.4rem">Equipamentos</h2>
      </div>
      <div class="card-grid card-grid-3">
        <div class="shop-item">
          <div class="shop-item-header">
            <div class="shop-item-name">⚔️ Armadura Tática</div>
            <div class="shop-item-price">150 pts</div>
          </div>
          <div style="display:flex;gap:4px;flex-wrap:wrap"><span class="tag tag-cyan">Equipamento</span><span class="tag tag-gold">Raro</span></div>
          <div class="shop-item-desc">Armadura de liga reforçada com filamentos adaptativos. Reduz dano físico recebido em combate narrativo.</div>
        </div>
        <div class="shop-item">
          <div class="shop-item-header">
            <div class="shop-item-name">🔮 Amuleto Nulo</div>
            <div class="shop-item-price">200 pts</div>
          </div>
          <div style="display:flex;gap:4px;flex-wrap:wrap"><span class="tag tag-purple">Mágico</span><span class="tag tag-gold">Épico</span></div>
          <div class="shop-item-desc">Suprime temporariamente mutações ativas, permitindo infiltração em zonas restritas sem detecção.</div>
        </div>
        <div class="shop-item">
          <div class="shop-item-header">
            <div class="shop-item-name">💊 Estabilizador Mutagênico</div>
            <div class="shop-item-price">80 pts</div>
          </div>
          <div style="display:flex;gap:4px;flex-wrap:wrap"><span class="tag tag-cyan">Consumível</span></div>
          <div class="shop-item-desc">Estabiliza mutações instáveis por até 24h no jogo. Ideal para mutantes em fase de despertar.</div>
        </div>
      </div>

      <div class="section-header" style="margin-top:2.5rem">
        <div class="section-label">Categoria</div>
        <h2 class="section-title" style="font-size:1.4rem">Habilidades Extras</h2>
      </div>
      <div class="card-grid card-grid-3">
        <div class="shop-item">
          <div class="shop-item-header">
            <div class="shop-item-name">🧠 Foco Aprimorado</div>
            <div class="shop-item-price">120 pts</div>
          </div>
          <div style="display:flex;gap:4px;flex-wrap:wrap"><span class="tag tag-purple">Passiva</span></div>
          <div class="shop-item-desc">Aumenta a precisão de poderes mentais em situações de alta pressão narrativa.</div>
        </div>
        <div class="shop-item">
          <div class="shop-item-header">
            <div class="shop-item-name">⚡ Sobrecarga</div>
            <div class="shop-item-price">180 pts</div>
          </div>
          <div style="display:flex;gap:4px;flex-wrap:wrap"><span class="tag tag-red">Ativo</span><span class="tag tag-gold">Raro</span></div>
          <div class="shop-item-desc">Permite amplificar um poder mutante além do seu limite normal por uma rodada. Uso único por arco.</div>
        </div>
        <div class="shop-item">
          <div class="shop-item-header">
            <div class="shop-item-name">🌀 Subpoder Livre</div>
            <div class="shop-item-price">250 pts</div>
          </div>
          <div style="display:flex;gap:4px;flex-wrap:wrap"><span class="tag tag-purple">Especial</span><span class="tag tag-gold">Épico</span></div>
          <div class="shop-item-desc">Adiciona um subpoder extra à ficha do personagem, sujeito à aprovação da staff.</div>
        </div>
      </div>

      <div class="section-header" style="margin-top:2.5rem">
        <div class="section-label">Categoria</div>
        <h2 class="section-title" style="font-size:1.4rem">Privilégios</h2>
      </div>
      <div class="card-grid card-grid-2">
        <div class="shop-item">
          <div class="shop-item-header">
            <div class="shop-item-name">🏅 Título de Guardião</div>
            <div class="shop-item-price">300 pts</div>
          </div>
          <div style="display:flex;gap:4px;flex-wrap:wrap"><span class="tag tag-gold">Privilégio</span></div>
          <div class="shop-item-desc">Cargo especial dentro do Instituto com acesso a áreas restritas e missões exclusivas de alto escalão.</div>
        </div>
        <div class="shop-item">
          <div class="shop-item-header">
            <div class="shop-item-name">📂 Segundo Personagem</div>
            <div class="shop-item-price">400 pts</div>
          </div>
          <div style="display:flex;gap:4px;flex-wrap:wrap"><span class="tag tag-cyan">Conta</span><span class="tag tag-gold">Especial</span></div>
          <div class="shop-item-desc">Libera a criação de um segundo personagem ativo na mesma conta, com ficha independente.</div>
        </div>
      </div>
    </div>
  </div>

  <!-- APRENDA A JOGAR -->
  <div id="page-aprenda" class="page">
    <div class="page-hero">
      <p class="section-label" style="justify-content:center">Aprenda a Jogar</p>
      <h1 class="page-hero-title">Como Funciona</h1>
      <p class="page-hero-sub">Tudo que você precisa para começar sua jornada</p>
    </div>
    <div class="section">
      <div class="card-grid card-grid-2">
        <div class="card">
          <div class="card-icon">🗺️</div>
          <div class="card-title">Guia do Novato</div>
          <div class="card-text">Passo a passo para criar seu personagem, entender o sistema e dar seus primeiros posts no RPG.</div>
          <br/><a class="btn btn-outline" style="font-size:0.78rem;padding:0.55rem 1.2rem" onclick="showPage('guia')">Ler Guia →</a>
        </div>
        <div class="card">
          <div class="card-icon">📋</div>
          <div class="card-title">Regras</div>
          <div class="card-text">Regras de conduta, regras de combate, uso de poderes e tudo que garante um jogo justo e divertido.</div>
          <br/><a class="btn btn-outline" style="font-size:0.78rem;padding:0.55rem 1.2rem" onclick="showPage('regras')">Ver Regras →</a>
        </div>
      </div>
    </div>
  </div>

  <!-- GUIA DO NOVATO -->
  <div id="page-guia" class="page">
    <div class="page-hero">
      <p class="section-label" style="justify-content:center">Aprenda a Jogar → Guia do Novato</p>
      <h1 class="page-hero-title">Guia do Novato</h1>
      <p class="page-hero-sub">Do zero ao primeiro post em 6 passos</p>
    </div>
    <div class="section">
      <div class="highlight-box">
        <p><strong>Bem-vindo, mutante!</strong> Não se preocupe se nunca jogou RPG por texto antes. Esse guia vai te conduzir por cada etapa. Se tiver dúvidas, procure a staff — estamos aqui para ajudar.</p>
      </div>
      <br/>
      <div class="steps">
        <div class="step">
          <div class="step-num">01</div>
          <div class="step-body">
            <div class="step-title">Leia as Regras Básicas</div>
            <div class="step-text">Antes de tudo, familiarize-se com as regras de conduta e as regras de combate. Isso garante uma experiência boa para todos.</div>
          </div>
        </div>
        <div class="step">
          <div class="step-num">02</div>
          <div class="step-body">
            <div class="step-title">Escolha uma Mutação</div>
            <div class="step-text">Navegue pela aba Mutações → Poderes e escolha um poder que combine com o tipo de personagem que você quer interpretar. Pode haver restrições de acesso para poderes raros.</div>
          </div>
        </div>
        <div class="step">
          <div class="step-num">03</div>
          <div class="step-body">
            <div class="step-title">Crie a Ficha do Personagem</div>
            <div class="step-text">Preencha o formulário de registro com nome, idade, aparência, histórico e poder escolhido. Seja criativo — a staff valoriza fichas bem escritas.</div>
          </div>
        </div>
        <div class="step">
          <div class="step-num">04</div>
          <div class="step-body">
            <div class="step-title">Aguarde a Aprovação</div>
            <div class="step-text">A staff irá revisar sua ficha em até 48h. Poderá pedir ajustes se algo precisar ser alterado para equilíbrio ou coerência com o lore.</div>
          </div>
        </div>
        <div class="step">
          <div class="step-num">05</div>
          <div class="step-body">
            <div class="step-title">Apresente-se</div>
            <div class="step-text">Com a ficha aprovada, entre no canal de apresentações e diga olá! A comunidade é receptiva e adoramos conhecer novos personagens.</div>
          </div>
        </div>
        <div class="step">
          <div class="step-num">06</div>
          <div class="step-body">
            <div class="step-title">Comece a Jogar!</div>
            <div class="step-text">Encontre um tópico aberto na plataforma de RPG, entre na narrativa e escreva seu primeiro post. Lembre-se: qualidade > quantidade. Não há limite mínimo de palavras, mas seja descritivo e respeite o turno.</div>
          </div>
        </div>
      </div>

      <div class="highlight-box warn" style="margin-top:2rem">
        <p><strong>Dica de ouro:</strong> Leia os últimos posts de um tópico antes de entrar para entender o contexto da cena. Isso evita inconsistências e mostra respeito pelos outros jogadores.</p>
      </div>
    </div>
  </div>

  <!-- REGRAS -->
  <div id="page-regras" class="page">
    <div class="page-hero">
      <p class="section-label" style="justify-content:center">Aprenda a Jogar → Regras</p>
      <h1 class="page-hero-title">Regras</h1>
      <p class="page-hero-sub">Para um jogo equilibrado e agradável para todos</p>
    </div>
    <div class="section">

      <div class="section-header">
        <div class="section-label">Seção 1</div>
        <h2 class="section-title" style="font-size:1.4rem">Conduta Geral</h2>
      </div>
      <div>
        <div class="accordion-item">
          <div class="accordion-header" onclick="toggleAccordion(this)">Respeito entre jogadores <span class="accordion-arrow"></span></div>
          <div class="accordion-body">Trate todos os jogadores com respeito, dentro e fora das narrativas. Discussões OOC (fora do personagem) devem ser sempre cordiais. Ofensas pessoais resultam em punições imediatas.</div>
        </div>
        <div class="accordion-item">
          <div class="accordion-header" onclick="toggleAccordion(this)">Separação entre IC e OOC <span class="accordion-arrow"></span></div>
          <div class="accordion-body">O que acontece na narrativa (IC – In Character) não reflete a opinião do jogador. Não leve conflitos de personagem para o lado pessoal. Se houver dúvidas, use os canais OOC para esclarecer.</div>
        </div>
        <div class="accordion-item">
          <div class="accordion-header" onclick="toggleAccordion(this)">Conteúdo permitido <span class="accordion-arrow"></span></div>
          <div class="accordion-body">Cenas de violência narrativa são permitidas com moderação. Conteúdo sexual, discurso de ódio e qualquer forma de assédio são estritamente proibidos e resultam em banimento imediato.</div>
        </div>
      </div>

      <div class="section-header" style="margin-top:2.5rem">
        <div class="section-label">Seção 2</div>
        <h2 class="section-title" style="font-size:1.4rem">Regras de Combate</h2>
      </div>
      <div>
        <div class="accordion-item">
          <div class="accordion-header" onclick="toggleAccordion(this)">Sistema de turnos <span class="accordion-arrow"></span></div>
          <div class="accordion-body">Em combates narrativos, cada jogador age em um post por rodada. Não é permitido agir duas vezes seguidas. A ordem dos turnos é definida pelo narrador ou pela ordem de entrada na cena.</div>
        </div>
        <div class="accordion-item">
          <div class="accordion-header" onclick="toggleAccordion(this)">God-modding e Metagaming <span class="accordion-arrow"></span></div>
          <div class="accordion-body"><strong>God-modding</strong> é controlar as ações de outro personagem sem consentimento. <strong>Metagaming</strong> é usar informações que seu personagem não teria acesso. Ambos são proibidos e passíveis de anulação do post.</div>
        </div>
        <div class="accordion-item">
          <div class="accordion-header" onclick="toggleAccordion(this)">Morte de personagem <span class="accordion-arrow"></span></div>
          <div class="accordion-body">A morte de personagem só ocorre com o consentimento explícito do jogador dono do personagem. Nenhum narrador ou jogador pode matar um personagem sem essa aprovação.</div>
        </div>
        <div class="accordion-item">
          <div class="accordion-header" onclick="toggleAccordion(this)">Uso de poderes em combate <span class="accordion-arrow"></span></div>
          <div class="accordion-body">Cada poder tem limites narrativos descritos na aba Mutações. Não invente efeitos que o poder não possui. Em caso de dúvida sobre o alcance de um poder, consulte a staff antes de postar.</div>
        </div>
      </div>

      <div class="section-header" style="margin-top:2.5rem">
        <div class="section-label">Seção 3</div>
        <h2 class="section-title" style="font-size:1.4rem">Atividade e Penalidades</h2>
      </div>
      <div>
        <div class="accordion-item">
          <div class="accordion-header" onclick="toggleAccordion(this)">Frequência mínima <span class="accordion-arrow"></span></div>
          <div class="accordion-body">Personagens em narrativas ativas devem postar ao menos uma vez por semana. Inatividade sem aviso prévio por mais de 14 dias pode resultar na suspensão temporária da ficha.</div>
        </div>
        <div class="accordion-item">
          <div class="accordion-header" onclick="toggleAccordion(this)">Comunicado de ausência <span class="accordion-arrow"></span></div>
          <div class="accordion-body">Se precisar se ausentar, comunique no canal de ausências com a data prevista de retorno. Ausências comunicadas não resultam em penalidades.</div>
        </div>
      </div>
    </div>
  </div>

  <!-- MUTAÇÕES -->
  <div id="page-mutacoes" class="page">
    <div class="page-hero">
      <p class="section-label" style="justify-content:center">Mutações</p>
      <h1 class="page-hero-title">Mutações</h1>
      <p class="page-hero-sub">O gene mutante se manifesta de formas infinitas</p>
    </div>
    <div class="section">
      <div class="card-grid card-grid-2">
        <div class="card">
          <div class="card-icon">⚡</div>
          <div class="card-title">Poderes</div>
          <div class="card-text">A lista completa de poderes mutantes disponíveis, organizados por categorias: Cinéticos, Mentais, Físicos, Místicos e mais.</div>
          <br/><a class="btn btn-outline" style="font-size:0.78rem;padding:0.55rem 1.2rem" onclick="showPage('poderes')">Ver Poderes →</a>
        </div>
        <div class="card">
          <div class="card-icon">🌀</div>
          <div class="card-title">Subpoderes</div>
          <div class="card-text">Variações e extensões dos poderes principais que podem ser adquiridas pelo jogo ou pela loja do Instituto.</div>
          <br/><a class="btn btn-outline" style="font-size:0.78rem;padding:0.55rem 1.2rem" onclick="showPage('subpoderes')">Ver Subpoderes →</a>
        </div>
      </div>
    </div>
  </div>

  <!-- PODERES -->
  <div id="page-poderes" class="page">
    <div class="page-hero">
      <p class="section-label" style="justify-content:center">Mutações → Poderes</p>
      <h1 class="page-hero-title">Poderes Mutantes</h1>
      <p class="page-hero-sub">Escolha com cuidado — seu poder define seu caminho</p>
    </div>
    <div class="section">
      <div class="highlight-box">
        <p>Poderes marcados como <span class="tag tag-red">Restrito</span> requerem aprovação especial da staff. Poderes <span class="tag tag-gold">Épico</span> são raros e podem ter limitações adicionais.</p>
      </div>

      <div class="section-header" style="margin-top:2rem">
        <div class="section-label">Categoria</div>
        <h2 class="section-title" style="font-size:1.3rem">⚡ Cinéticos</h2>
      </div>
      <table class="power-table">
        <thead><tr><th>Poder</th><th>Descrição</th><th>Raridade</th></tr></thead>
        <tbody>
          <tr><td>Telecinese</td><td>Mover e manipular objetos fisicamente com a mente, sem contato.</td><td><span class="tag tag-cyan">Comum</span></td></tr>
          <tr><td>Manipulação de Energia</td><td>Absorver, redirecionar ou projetar formas de energia como eletricidade e calor.</td><td><span class="tag tag-gold">Raro</span></td></tr>
          <tr><td>Aceleração Cinética</td><td>Aumentar a velocidade cinética de projéteis ou do próprio corpo.</td><td><span class="tag tag-cyan">Comum</span></td></tr>
        </tbody>
      </table>

      <div class="section-header" style="margin-top:2.5rem">
        <div class="section-label">Categoria</div>
        <h2 class="section-title" style="font-size:1.3rem">🧠 Mentais</h2>
      </div>
      <table class="power-table">
        <thead><tr><th>Poder</th><th>Descrição</th><th>Raridade</th></tr></thead>
        <tbody>
          <tr><td>Telepatia</td><td>Ler e transmitir pensamentos entre mentes. Uso ofensivo requer nível alto.</td><td><span class="tag tag-cyan">Comum</span></td></tr>
          <tr><td>Ilusão Mental</td><td>Projetar imagens, sons e sensações diretamente na mente de outros.</td><td><span class="tag tag-gold">Raro</span></td></tr>
          <tr><td>Controle Mental</td><td>Assumir controle das ações de outro ser consciente. Poder extremamente restrito.</td><td><span class="tag tag-red">Restrito</span></td></tr>
        </tbody>
      </table>

      <div class="section-header" style="margin-top:2.5rem">
        <div class="section-label">Categoria</div>
        <h2 class="section-title" style="font-size:1.3rem">💪 Físicos</h2>
      </div>
      <table class="power-table">
        <thead><tr><th>Poder</th><th>Descrição</th><th>Raridade</th></tr></thead>
        <tbody>
          <tr><td>Superforça</td><td>Força física muito acima do limite humano. Escalonável em níveis.</td><td><span class="tag tag-cyan">Comum</span></td></tr>
          <tr><td>Regeneração</td><td>Recuperação acelerada de ferimentos físicos. Não regenera membros amputados no nível básico.</td><td><span class="tag tag-cyan">Comum</span></td></tr>
          <tr><td>Transformação Física</td><td>Alterar a composição do próprio corpo (densidade, estado físico, aparência).</td><td><span class="tag tag-gold">Raro</span></td></tr>
          <tr><td>Imortalidade</td><td>Imunidade à morte por causas naturais e rápida recuperação de traumas letais.</td><td><span class="tag tag-red">Restrito</span></td></tr>
        </tbody>
      </table>

      <div class="section-header" style="margin-top:2.5rem">
        <div class="section-label">Categoria</div>
        <h2 class="section-title" style="font-size:1.3rem">🔮 Místicos</h2>
      </div>
      <table class="power-table">
        <thead><tr><th>Poder</th><th>Descrição</th><th>Raridade</th></tr></thead>
        <tbody>
          <tr><td>Magia Arcana</td><td>Acesso a feitiços básicos de escolas arcanas. Requer estudo e prática narrativa.</td><td><span class="tag tag-gold">Raro</span></td></tr>
          <tr><td>Necromancia</td><td>Comunicação e limitada manipulação de espíritos e mortos. Altamente restrito.</td><td><span class="tag tag-red">Restrito</span></td></tr>
          <tr><td>Visão Mística</td><td>Perceber auras, entidades e eventos sobrenaturais invisíveis ao olho comum.</td><td><span class="tag tag-cyan">Comum</span></td></tr>
        </tbody>
      </table>
    </div>
  </div>

  <!-- SUBPODERES -->
  <div id="page-subpoderes" class="page">
    <div class="page-hero">
      <p class="section-label" style="justify-content:center">Mutações → Subpoderes</p>
      <h1 class="page-hero-title">Subpoderes</h1>
      <p class="page-hero-sub">Extensões que tornam sua mutação verdadeiramente sua</p>
    </div>
    <div class="section">
      <div class="highlight-box">
        <p>Subpoderes são habilidades derivadas ou complementares ao poder principal. Todo personagem começa com <strong>1 subpoder</strong> gratuito ao criar a ficha. Subpoderes adicionais podem ser adquiridos na Loja ou ganhos por progressão narrativa.</p>
      </div>
      <br/>
      <div class="card-grid card-grid-3">
        <div class="card">
          <div class="card-title">Voo</div>
          <div style="margin-bottom:0.5rem"><span class="tag tag-cyan">Cinético</span><span class="tag tag-purple">Físico</span></div>
          <div class="card-text">Capacidade de levitar e voar. Velocidade e altitude variam com o nível de controle do personagem.</div>
        </div>
        <div class="card">
          <div class="card-title">Escudo Mental</div>
          <div style="margin-bottom:0.5rem"><span class="tag tag-purple">Mental</span></div>
          <div class="card-text">Proteção da própria mente contra intrusão telepática e ilusões. Reduz efeitos de poderes mentais ofensivos.</div>
        </div>
        <div class="card">
          <div class="card-title">Sentidos Aprimorados</div>
          <div style="margin-bottom:0.5rem"><span class="tag tag-cyan">Físico</span></div>
          <div class="card-text">Aguçamento de um ou mais sentidos (visão, audição, olfato) muito além do humano normal.</div>
        </div>
        <div class="card">
          <div class="card-title">Resistência Elementar</div>
          <div style="margin-bottom:0.5rem"><span class="tag tag-cyan">Físico</span><span class="tag tag-gold">Raro</span></div>
          <div class="card-text">Imunidade ou alta resistência a um tipo específico de dano elemental: fogo, frio, eletricidade etc.</div>
        </div>
        <div class="card">
          <div class="card-title">Empatia</div>
          <div style="margin-bottom:0.5rem"><span class="tag tag-purple">Mental</span></div>
          <div class="card-text">Perceber e sentir as emoções dos presentes. Em nível avançado, pode influenciar o estado emocional alheio.</div>
        </div>
        <div class="card">
          <div class="card-title">Cura Menor</div>
          <div style="margin-bottom:0.5rem"><span class="tag tag-purple">Místico</span><span class="tag tag-gold">Raro</span></div>
          <div class="card-text">Capacidade de curar ferimentos leves em outros. Não funciona em doenças graves ou lesões complexas.</div>
        </div>
      </div>
    </div>
  </div>

  <!-- ENTIDADES -->
  <div id="page-entidades" class="page">
    <div class="page-hero">
      <p class="section-label" style="justify-content:center">Entidades</p>
      <h1 class="page-hero-title">Entidades</h1>
      <p class="page-hero-sub">Seres além da mutação humana — aliados, ameaças ou algo mais complexo</p>
    </div>
    <div class="section">
      <div class="highlight-box warn">
        <p><strong>Atenção:</strong> Jogar como uma Entidade requer aprovação da staff e normalmente está disponível apenas para jogadores com histórico ativo no RPG. Cada entidade tem regras próprias de uso.</p>
      </div>
      <br/>
      <div class="card-grid card-grid-3">
        <div class="entity-card">
          <div class="entity-title">Asgardianos</div>
          <div class="entity-origin">Origem: Asgard — Divindade Nórdica</div>
          <div class="entity-desc">Seres divinos de Asgard, com força e resistência sobre-humanas. Conectados à magia rúnica e ao Yggdrasil.</div>
          <br/><div style="display:flex;gap:4px;flex-wrap:wrap"><span class="tag tag-gold">Divino</span><span class="tag tag-red">Restrito</span></div>
        </div>
        <div class="entity-card">
          <div class="entity-title">Atlanteans</div>
          <div class="entity-origin">Origem: Atlântida — Civilização Aquática</div>
          <div class="entity-desc">Descendentes da civilização submarina perdida. Adaptados à vida aquática, com biologia única e magia antiga.</div>
          <br/><div style="display:flex;gap:4px;flex-wrap:wrap"><span class="tag tag-cyan">Aquático</span><span class="tag tag-gold">Raro</span></div>
        </div>
        <div class="entity-card">
          <div class="entity-title">Fênix</div>
          <div class="entity-origin">Origem: Cósmica — Força da Criação</div>
          <div class="entity-desc">Portadores da Força Fênix, entidade cósmica de criação e destruição. Poder imenso e instável. Extremamente restrito.</div>
          <br/><div style="display:flex;gap:4px;flex-wrap:wrap"><span class="tag tag-red">Cósmico</span><span class="tag tag-red">Restrito</span></div>
        </div>
        <div class="entity-card">
          <div class="entity-title">Homo-Magi</div>
          <div class="entity-origin">Origem: Terrestre — Linha Mágica</div>
          <div class="entity-desc">Humanos com predisposição genética à magia. Não são mutantes, mas sua habilidade arcana os coloca na mesma esfera.</div>
          <br/><div style="display:flex;gap:4px;flex-wrap:wrap"><span class="tag tag-purple">Mágico</span><span class="tag tag-cyan">Comum</span></div>
        </div>
        <div class="entity-card">
          <div class="entity-title">Anoditas</div>
          <div class="entity-origin">Origem: Extraterrestre — Energia Pura</div>
          <div class="entity-desc">Seres compostos de energia vital pura. Na forma humana, possuem capacidade de manipulação energética incomparável.</div>
          <br/><div style="display:flex;gap:4px;flex-wrap:wrap"><span class="tag tag-purple">Energia</span><span class="tag tag-red">Restrito</span></div>
        </div>
        <div class="entity-card">
          <div class="entity-title">Valkyries</div>
          <div class="entity-origin">Origem: Asgard — Guerreiras Divinas</div>
          <div class="entity-desc">Escolhidas de Odin, guerreiras imortais com ligação especial com a morte e o destino dos guerreiros caídos.</div>
          <br/><div style="display:flex;gap:4px;flex-wrap:wrap"><span class="tag tag-gold">Divino</span><span class="tag tag-red">Restrito</span></div>
        </div>
        <div class="entity-card">
          <div class="entity-title">Tamarianos</div>
          <div class="entity-origin">Origem: Tamaran — Extraterrestre</div>
          <div class="entity-desc">Aliens de Tamaran capazes de absorver luz solar e convertê-la em voo e ataques energéticos.</div>
          <br/><div style="display:flex;gap:4px;flex-wrap:wrap"><span class="tag tag-cyan">Extraterrestre</span><span class="tag tag-gold">Raro</span></div>
        </div>
        <div class="entity-card">
          <div class="entity-title">Krees</div>
          <div class="entity-origin">Origem: Hala — Império Intergaláctico</div>
          <div class="entity-desc">Raça alienígena guerreira com biologia superior à humana. Tecnologia avançada e militarismo como filosofia de vida.</div>
          <br/><div style="display:flex;gap:4px;flex-wrap:wrap"><span class="tag tag-cyan">Extraterrestre</span><span class="tag tag-gold">Raro</span></div>
        </div>
        <div class="entity-card">
          <div class="entity-title">Azarathians</div>
          <div class="entity-origin">Origem: Azarath — Dimensão Mística</div>
          <div class="entity-desc">Habitantes da dimensão mística de Azarath, com poderes sombrios e capacidade de manipular energia das trevas.</div>
          <br/><div style="display:flex;gap:4px;flex-wrap:wrap"><span class="tag tag-purple">Dimensional</span><span class="tag tag-red">Restrito</span></div>
        </div>
      </div>
    </div>
  </div>

  <!-- CRÉDITOS -->
  <div id="page-creditos" class="page">
    <div class="page-hero">
      <p class="section-label" style="justify-content:center">Créditos</p>
      <h1 class="page-hero-title">Créditos</h1>
      <p class="page-hero-sub">O Instituto existe por causa dessas pessoas</p>
    </div>
    <div class="section">
      <div class="highlight-box">
        <p>Este RPG é um projeto colaborativo feito com amor pela comunidade. Todo o conteúdo foi criado pelos membros da staff listados abaixo. Para sugestões ou colaborações, entre em contato.</p>
      </div>
      <br/>
      <div class="credits-grid">
        <div class="credit-block">
          <div class="credit-role">Fundação & Direção Criativa</div>
          <div class="credit-name">[ Seu Nick Aqui ]</div>
          <div class="credit-note">Criador do Instituto Mutante, responsável pelo lore principal, regras e direção criativa geral do RPG.</div>
        </div>
        <div class="credit-block">
          <div class="credit-role">Co-Fundação & Curadoria de Poderes</div>
          <div class="credit-name">[ Nick do Co-fundador ]</div>
          <div class="credit-note">Responsável pela criação e balanceamento do sistema de mutações, poderes e subpoderes.</div>
        </div>
        <div class="credit-block">
          <div class="credit-role">Staff — Narrativa & Lore</div>
          <div class="credit-name">[ Nick do Staff ]</div>
          <div class="credit-note">Cuida das narrativas principais, eventos globais e da continuidade do lore do Instituto.</div>
        </div>
        <div class="credit-block">
          <div class="credit-role">Staff — Moderação & Fichas</div>
          <div class="credit-name">[ Nick do Moderador ]</div>
          <div class="credit-note">Aprovação de fichas, mediação de conflitos e suporte geral aos jogadores.</div>
        </div>
        <div class="credit-block">
          <div class="credit-role">Design do Site</div>
          <div class="credit-name">[ Nick do Designer ]</div>
          <div class="credit-note">Criação e manutenção do site oficial do Instituto Mutante.</div>
        </div>
      </div>

      <div class="section-header" style="margin-top:3.5rem">
        <div class="section-label">Referências</div>
        <h2 class="section-title" style="font-size:1.3rem">Inspirações & Agradecimentos</h2>
      </div>
      <div class="card-grid card-grid-2">
        <div class="card">
          <div class="card-title">Universo Marvel & DC</div>
          <div class="card-text">Grande parte dos conceitos de entidades e mutações é inspirada em personagens desses universos. Todo crédito aos criadores originais.</div>
        </div>
        <div class="card">
          <div class="card-title">Comunidade RPGista Brasileira</div>
          <div class="card-text">A todos os RPGs por texto que vieram antes e mostraram que é possível criar mundos incríveis com palavras e imaginação.</div>
        </div>
      </div>
    </div>
  </div>

</main>

<footer>
  <div class="footer-logo">INSTITUTO MUTANTE</div>
  <p>RPG por Texto · Comunidade Colaborativa · Todos os direitos reservados à Staff</p>
</footer>

<script>
  // ── PARTICLES ──
  const canvas = document.getElementById('bg-canvas');
  const ctx = canvas.getContext('2d');
  let W, H, particles = [];

  function resize() {
    W = canvas.width = window.innerWidth;
    H = canvas.height = window.innerHeight;
  }

  function initParticles() {
    particles = [];
    const n = Math.floor(W * H / 18000);
    for (let i = 0; i < n; i++) {
      particles.push({
        x: Math.random() * W,
        y: Math.random() * H,
        r: Math.random() * 1.2 + 0.3,
        vx: (Math.random() - 0.5) * 0.25,
        vy: (Math.random() - 0.5) * 0.25,
        o: Math.random() * 0.5 + 0.2
      });
    }
  }

  function drawParticles() {
    ctx.clearRect(0, 0, W, H);
    particles.forEach(p => {
      ctx.beginPath();
      ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
      ctx.fillStyle = `rgba(0,229,255,${p.o})`;
      ctx.fill();
      p.x += p.vx; p.y += p.vy;
      if (p.x < 0) p.x = W;
      if (p.x > W) p.x = 0;
      if (p.y < 0) p.y = H;
      if (p.y > H) p.y = 0;
    });
    requestAnimationFrame(drawParticles);
  }

  resize(); initParticles(); drawParticles();
  window.addEventListener('resize', () => { resize(); initParticles(); });

  // ── PAGE NAVIGATION ──
  const navMap = {
    'inicio': 'nav-inicio',
    'acampamento': 'nav-acampamento',
    'historia': 'nav-acampamento',
    'loja': 'nav-acampamento',
    'aprenda': 'nav-aprenda',
    'guia': 'nav-aprenda',
    'regras': 'nav-aprenda',
    'mutacoes': 'nav-mutacoes',
    'poderes': 'nav-mutacoes',
    'subpoderes': 'nav-mutacoes',
    'entidades': 'nav-entidades',
    'creditos': 'nav-creditos'
  };

  function showPage(id) {
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    const pg = document.getElementById('page-' + id);
    if (pg) { pg.classList.add('active'); window.scrollTo(0, 0); }

    // Update active nav
    document.querySelectorAll('.nav-links > li > a, .nav-links > li > span').forEach(el => el.classList.remove('active'));
    const navId = navMap[id];
    if (navId) {
      const navEl = document.getElementById(navId);
      if (navEl) navEl.classList.add('active');
    }

    // Close mobile menu
    document.getElementById('navLinks').classList.remove('open');
  }

  // ── ACCORDION ──
  function toggleAccordion(header) {
    const body = header.nextElementSibling;
    const isOpen = header.classList.contains('open');
    // Close all
    document.querySelectorAll('.accordion-header.open').forEach(h => {
      h.classList.remove('open');
      h.nextElementSibling.classList.remove('open');
    });
    if (!isOpen) {
      header.classList.add('open');
      body.classList.add('open');
    }
  }

  // ── MOBILE MENU ──
  function toggleMenu() {
    document.getElementById('navLinks').classList.toggle('open');
  }

  // Mobile dropdown toggle
  document.querySelectorAll('.nav-links > li > span').forEach(span => {
    span.addEventListener('click', function(e) {
      if (window.innerWidth <= 768) {
        const dropdown = this.parentElement.querySelector('.dropdown');
        if (dropdown) dropdown.classList.toggle('open');
      }
    });
  });
</script>
</body>
</html>
