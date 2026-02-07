<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>GameVerse - Tienda Digital de Videojuegos</title>

  <!-- QRCode.js CDN -->
  <script src="https://cdn.jsdelivr.net/npm/qrcodejs@1.0.0/qrcode.min.js"></script>

  <style>
    /* ============================================================
       VARIABLES & RESET
       ============================================================ */
    :root {
      --bg-primary: #0b0e17;
      --bg-secondary: #121628;
      --bg-card: #171c32;
      --bg-input: #1e2444;
      --text-primary: #e8eaf6;
      --text-secondary: #9fa4c4;
      --accent-blue: #4f7df9;
      --accent-purple: #a855f7;
      --accent-gradient: linear-gradient(135deg, #4f7df9, #a855f7);
      --accent-gradient-hover: linear-gradient(135deg, #6b93ff, #bf6dff);
      --accent-gold: #fbbf24;
      --accent-green: #22c55e;
      --success: #22c55e;
      --warning: #f59e0b;
      --danger: #ef4444;
      --radius: 12px;
      --shadow: 0 8px 32px rgba(0,0,0,.45);
      --transition: .3s ease;
    }

    html.light {
      --bg-primary: #f0f2f5;
      --bg-secondary: #ffffff;
      --bg-card: #ffffff;
      --bg-input: #e8ecf1;
      --text-primary: #1a1a2e;
      --text-secondary: #555770;
      --shadow: 0 8px 32px rgba(0,0,0,.1);
    }

    *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }
    html { scroll-behavior: smooth; }

    body {
      font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
      background: var(--bg-primary);
      color: var(--text-primary);
      line-height: 1.6;
      transition: background var(--transition), color var(--transition);
    }

    a { color: inherit; text-decoration: none; }
    button { cursor: pointer; font-family: inherit; }
    input, textarea, select { font-family: inherit; }

    /* ============================================================
       UTILIDADES
       ============================================================ */
    .container { max-width: 1200px; margin: 0 auto; padding: 0 20px; }

    .section-title {
      font-size: 2rem;
      font-weight: 800;
      text-align: center;
      margin-bottom: 40px;
      background: var(--accent-gradient);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .btn {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      padding: 10px 22px;
      border: none;
      border-radius: 8px;
      font-size: .95rem;
      font-weight: 600;
      background: var(--accent-gradient);
      color: #fff;
      transition: transform var(--transition), box-shadow var(--transition), background var(--transition);
    }
    .btn:hover { transform: translateY(-2px); box-shadow: 0 6px 20px rgba(79,125,249,.35); background: var(--accent-gradient-hover); }
    .btn:active { transform: scale(.97); }

    .btn-outline {
      background: transparent;
      border: 2px solid var(--accent-blue);
      color: var(--accent-blue);
    }
    .btn-outline:hover { background: var(--accent-blue); color: #fff; }

    .btn-sm { padding: 7px 16px; font-size: .85rem; }

    .btn-gold {
      background: linear-gradient(135deg, #f59e0b, #fbbf24);
      color: #1a1a2e;
    }
    .btn-gold:hover { box-shadow: 0 6px 20px rgba(251,191,36,.4); }

    .btn-green {
      background: linear-gradient(135deg, #16a34a, #22c55e);
      color: #fff;
    }
    .btn-green:hover { box-shadow: 0 6px 20px rgba(34,197,94,.4); }

    /* ============================================================
       HEADER / NAV
       ============================================================ */
    header {
      position: sticky;
      top: 0;
      z-index: 100;
      background: rgba(11,14,23,.85);
      backdrop-filter: blur(14px);
      border-bottom: 1px solid rgba(79,125,249,.15);
      transition: background var(--transition);
    }
    html.light header { background: rgba(255,255,255,.85); }

    header .container {
      display: flex;
      align-items: center;
      justify-content: space-between;
      height: 64px;
    }

    .logo {
      font-size: 1.5rem;
      font-weight: 900;
      background: var(--accent-gradient);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    nav { display: flex; align-items: center; gap: 24px; }
    nav a { font-weight: 500; color: var(--text-secondary); transition: color var(--transition); }
    nav a:hover { color: var(--text-primary); }

    .header-actions { display: flex; align-items: center; gap: 14px; }

    .theme-toggle {
      width: 44px; height: 24px;
      border-radius: 12px;
      background: var(--bg-input);
      border: 2px solid var(--accent-blue);
      position: relative;
      cursor: pointer;
      transition: background var(--transition);
    }
    .theme-toggle::after {
      content: '';
      width: 16px; height: 16px;
      border-radius: 50%;
      background: var(--accent-gradient);
      position: absolute;
      top: 2px; left: 2px;
      transition: transform var(--transition);
    }
    html.light .theme-toggle::after { transform: translateX(20px); }

    .cart-btn {
      position: relative;
      background: none;
      border: none;
      color: var(--text-primary);
      font-size: 1.4rem;
    }
    .cart-badge {
      position: absolute;
      top: -6px; right: -8px;
      background: var(--danger);
      color: #fff;
      font-size: .7rem;
      font-weight: 700;
      width: 18px; height: 18px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .hamburger { display: none; background: none; border: none; color: var(--text-primary); font-size: 1.6rem; }

    @media (max-width: 768px) {
      .hamburger { display: block; }
      nav {
        position: fixed;
        top: 64px; left: 0; right: 0;
        background: var(--bg-secondary);
        flex-direction: column;
        padding: 20px;
        gap: 16px;
        transform: translateY(-120%);
        transition: transform var(--transition);
        border-bottom: 1px solid rgba(79,125,249,.15);
      }
      nav.open { transform: translateY(0); }
    }

    /* ============================================================
       HERO
       ============================================================ */
    .hero {
      min-height: 520px;
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
      padding: 80px 20px;
      background:
        radial-gradient(ellipse at 20% 50%, rgba(79,125,249,.15) 0%, transparent 60%),
        radial-gradient(ellipse at 80% 50%, rgba(168,85,247,.12) 0%, transparent 60%);
    }
    .hero h1 {
      font-size: clamp(2.2rem, 5vw, 3.6rem);
      font-weight: 900;
      margin-bottom: 16px;
      background: var(--accent-gradient);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      animation: fadeInUp .8s ease;
    }
    .hero p {
      font-size: 1.15rem;
      color: var(--text-secondary);
      max-width: 560px;
      margin: 0 auto 32px;
      animation: fadeInUp .8s ease .15s both;
    }
    .hero-actions { display: flex; gap: 14px; justify-content: center; flex-wrap: wrap; animation: fadeInUp .8s ease .3s both; }

    /* ============================================================
       CATALOGO DE JUEGOS
       ============================================================ */
    .catalog { padding: 80px 0; }

    .filters {
      display: flex;
      gap: 12px;
      justify-content: center;
      flex-wrap: wrap;
      margin-bottom: 32px;
    }
    .filters select, .filters input {
      padding: 10px 16px;
      background: var(--bg-input);
      color: var(--text-primary);
      border: 1px solid rgba(79,125,249,.2);
      border-radius: 8px;
      outline: none;
      transition: border var(--transition);
    }
    .filters select:focus, .filters input:focus { border-color: var(--accent-blue); }

    .games-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
      gap: 24px;
    }

    .game-card {
      background: var(--bg-card);
      border-radius: var(--radius);
      overflow: hidden;
      border: 1px solid rgba(79,125,249,.1);
      transition: transform var(--transition), box-shadow var(--transition);
      animation: fadeInUp .5s ease both;
      position: relative;
    }
    .game-card:hover { transform: translateY(-6px); box-shadow: var(--shadow); }

    .game-card img {
      width: 100%;
      height: 180px;
      object-fit: cover;
    }

    .game-info { padding: 16px; }
    .game-info h3 { font-size: 1.1rem; margin-bottom: 4px; }
    .game-meta { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 8px; }
    .game-tag {
      font-size: .72rem;
      font-weight: 600;
      padding: 3px 10px;
      border-radius: 20px;
      background: rgba(79,125,249,.15);
      color: var(--accent-blue);
    }
    .game-tag.mundial {
      background: rgba(251,191,36,.2);
      color: var(--accent-gold);
      animation: pulseGlow 2s infinite;
    }
    .game-tag.limited {
      background: rgba(239,68,68,.15);
      color: var(--danger);
    }
    .game-bottom { display: flex; align-items: center; justify-content: space-between; margin-top: 12px; }
    .game-price { font-size: 1.25rem; font-weight: 800; background: var(--accent-gradient); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
    .game-price-old { font-size: .85rem; color: var(--text-secondary); text-decoration: line-through; margin-right: 8px; }

    /* Discount ribbon */
    .discount-ribbon {
      position: absolute;
      top: 12px; right: 12px;
      background: var(--danger);
      color: #fff;
      font-size: .75rem;
      font-weight: 700;
      padding: 4px 10px;
      border-radius: 6px;
      animation: pulseGlow 2s infinite;
    }

    /* ============================================================
       MUNDIAL BANNER SECTION
       ============================================================ */
    .mundial-section {
      padding: 80px 0;
      position: relative;
      overflow: hidden;
      background:
        radial-gradient(ellipse at 30% 50%, rgba(251,191,36,.08) 0%, transparent 60%),
        radial-gradient(ellipse at 70% 50%, rgba(34,197,94,.08) 0%, transparent 60%);
    }

    .mundial-banner {
      background: linear-gradient(135deg, rgba(79,125,249,.15), rgba(251,191,36,.15));
      border: 1px solid rgba(251,191,36,.3);
      border-radius: 16px;
      padding: 48px 32px;
      text-align: center;
      margin-bottom: 48px;
      position: relative;
      overflow: hidden;
    }
    .mundial-banner::before {
      content: '';
      position: absolute;
      inset: 0;
      background: radial-gradient(circle at 50% 50%, rgba(251,191,36,.1) 0%, transparent 70%);
      animation: stadiumLights 3s ease-in-out infinite;
    }
    .mundial-banner h2 {
      font-size: clamp(1.8rem, 4vw, 2.8rem);
      font-weight: 900;
      margin-bottom: 12px;
      position: relative;
      z-index: 1;
    }
    .mundial-banner p {
      color: var(--text-secondary);
      font-size: 1.1rem;
      position: relative;
      z-index: 1;
    }

    /* Countdown */
    .countdown-wrapper {
      display: flex;
      justify-content: center;
      gap: 16px;
      margin: 32px 0;
      flex-wrap: wrap;
    }
    .countdown-item {
      background: var(--bg-card);
      border: 1px solid rgba(251,191,36,.3);
      border-radius: 12px;
      padding: 16px 20px;
      min-width: 90px;
      text-align: center;
      transition: transform var(--transition);
    }
    .countdown-item:hover { transform: scale(1.05); }
    .countdown-item .count-num {
      font-size: 2.2rem;
      font-weight: 900;
      background: linear-gradient(135deg, #fbbf24, #f59e0b);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }
    .countdown-item .count-label {
      font-size: .8rem;
      color: var(--text-secondary);
      text-transform: uppercase;
      letter-spacing: 1px;
    }

    /* Country cards */
    .country-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
      gap: 16px;
      margin-top: 32px;
    }
    .country-card {
      background: var(--bg-card);
      border: 1px solid rgba(79,125,249,.1);
      border-radius: var(--radius);
      padding: 20px;
      text-align: center;
      transition: transform var(--transition), box-shadow var(--transition);
      cursor: pointer;
      animation: fadeInUp .5s ease both;
    }
    .country-card:hover { transform: translateY(-4px) scale(1.02); box-shadow: 0 8px 24px rgba(251,191,36,.2); }
    .country-card.voted { border-color: var(--accent-gold); box-shadow: 0 0 16px rgba(251,191,36,.3); }
    .country-flag { font-size: 3rem; margin-bottom: 8px; display: block; }
    .country-name { font-weight: 700; font-size: 1rem; }
    .country-votes { font-size: .8rem; color: var(--text-secondary); margin-top: 4px; }

    /* ============================================================
       DESAFIOS DEL MUNDIAL
       ============================================================ */
    .challenges-section { padding: 80px 0; }

    .challenge-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
      gap: 24px;
    }

    .challenge-card {
      background: var(--bg-card);
      border-radius: var(--radius);
      padding: 28px;
      border: 1px solid rgba(79,125,249,.1);
      transition: transform var(--transition), box-shadow var(--transition);
    }
    .challenge-card:hover { transform: translateY(-4px); box-shadow: var(--shadow); }
    .challenge-card h3 { font-size: 1.15rem; margin-bottom: 16px; display: flex; align-items: center; gap: 8px; }
    .challenge-card .challenge-icon { font-size: 1.4rem; }

    .predict-form { display: flex; flex-direction: column; gap: 12px; }
    .predict-row { display: flex; align-items: center; gap: 12px; }
    .predict-row .team-name { font-weight: 600; min-width: 80px; }
    .predict-row input {
      width: 60px;
      padding: 8px;
      text-align: center;
      background: var(--bg-input);
      color: var(--text-primary);
      border: 1px solid rgba(79,125,249,.2);
      border-radius: 8px;
      font-size: 1.1rem;
      font-weight: 700;
      outline: none;
    }
    .predict-row input:focus { border-color: var(--accent-gold); }
    .predict-vs { font-weight: 800; color: var(--text-secondary); }

    .champion-options {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(80px, 1fr));
      gap: 10px;
    }
    .champion-option {
      background: var(--bg-input);
      border: 2px solid transparent;
      border-radius: 10px;
      padding: 12px 8px;
      text-align: center;
      cursor: pointer;
      transition: all var(--transition);
      font-size: .85rem;
    }
    .champion-option:hover { border-color: rgba(251,191,36,.5); transform: scale(1.05); }
    .champion-option.selected { border-color: var(--accent-gold); background: rgba(251,191,36,.1); box-shadow: 0 0 12px rgba(251,191,36,.2); }
    .champion-option .ch-flag { font-size: 1.6rem; display: block; margin-bottom: 4px; }

    /* Stats mini */
    .live-stats {
      margin-top: 20px;
      padding: 16px;
      background: rgba(79,125,249,.05);
      border-radius: 10px;
      border: 1px solid rgba(79,125,249,.1);
    }
    .live-stats h4 { font-size: .9rem; margin-bottom: 10px; color: var(--text-secondary); display: flex; align-items: center; gap: 6px; }
    .live-dot { width: 8px; height: 8px; border-radius: 50%; background: var(--danger); animation: livePulse 1.5s infinite; display: inline-block; }

    /* ============================================================
       VOTACIONES MUNDIAL
       ============================================================ */
    .voting-section { padding: 80px 0; }

    .voting-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
      gap: 24px;
    }

    .voting-card {
      background: var(--bg-card);
      border-radius: var(--radius);
      padding: 28px;
      border: 1px solid rgba(79,125,249,.1);
    }
    .voting-card h3 { margin-bottom: 20px; font-size: 1.1rem; }

    .vote-option {
      display: flex;
      align-items: center;
      gap: 12px;
      padding: 12px;
      background: var(--bg-input);
      border-radius: 10px;
      margin-bottom: 10px;
      cursor: pointer;
      transition: all var(--transition);
      border: 2px solid transparent;
    }
    .vote-option:hover { border-color: rgba(79,125,249,.3); transform: translateX(4px); }
    .vote-option.voted { border-color: var(--accent-blue); background: rgba(79,125,249,.1); }
    .vote-option .vote-icon { font-size: 1.4rem; }
    .vote-option .vote-name { font-weight: 600; flex: 1; }
    .vote-option .vote-pct { font-weight: 700; font-size: .9rem; color: var(--accent-blue); }
    .vote-bar-mini {
      height: 4px;
      background: var(--bg-primary);
      border-radius: 2px;
      margin-top: 6px;
      overflow: hidden;
    }
    .vote-bar-fill {
      height: 100%;
      background: var(--accent-gradient);
      border-radius: 2px;
      transition: width 1s ease;
    }

    /* ============================================================
       REDES SOCIALES
       ============================================================ */
    .social-section {
      padding: 80px 0;
      text-align: center;
    }

    .social-icons {
      display: flex;
      justify-content: center;
      gap: 24px;
      flex-wrap: wrap;
      margin-bottom: 32px;
    }

    .social-icon-card {
      width: 100px;
      padding: 20px 12px;
      background: var(--bg-card);
      border-radius: var(--radius);
      border: 1px solid rgba(79,125,249,.1);
      text-align: center;
      transition: transform .4s ease, box-shadow .4s ease;
      cursor: pointer;
      perspective: 600px;
    }
    .social-icon-card:hover {
      transform: translateY(-8px) rotateX(8deg) rotateY(-5deg);
      box-shadow: 0 12px 32px rgba(79,125,249,.25);
    }

    .social-icon-card svg {
      width: 36px;
      height: 36px;
      margin-bottom: 8px;
      transition: filter var(--transition);
    }
    .social-icon-card:hover svg {
      filter: drop-shadow(0 0 10px currentColor);
    }

    .social-icon-card.instagram { color: #E4405F; }
    .social-icon-card.instagram:hover { box-shadow: 0 12px 32px rgba(228,64,95,.3); }
    .social-icon-card.tiktok { color: #00f2ea; }
    .social-icon-card.tiktok:hover { box-shadow: 0 12px 32px rgba(0,242,234,.3); }
    .social-icon-card.twitter { color: #e8eaf6; }
    .social-icon-card.twitter:hover { box-shadow: 0 12px 32px rgba(232,234,246,.2); }
    html.light .social-icon-card.twitter { color: #1a1a2e; }
    .social-icon-card.youtube { color: #FF0000; }
    .social-icon-card.youtube:hover { box-shadow: 0 12px 32px rgba(255,0,0,.3); }
    .social-icon-card.discord { color: #5865F2; }
    .social-icon-card.discord:hover { box-shadow: 0 12px 32px rgba(88,101,242,.3); }

    .social-followers { font-size: .75rem; color: var(--text-secondary); margin-top: 4px; }
    .social-name { font-size: .85rem; font-weight: 600; }

    .share-section { margin-top: 24px; }
    .share-section p { color: var(--text-secondary); margin-bottom: 14px; }

    /* ============================================================
       GAMIFICACION
       ============================================================ */
    .gamification-section { padding: 80px 0; }

    .gami-card {
      max-width: 600px;
      margin: 0 auto;
      background: var(--bg-card);
      border-radius: var(--radius);
      padding: 32px;
      border: 1px solid rgba(251,191,36,.2);
      text-align: center;
    }

    .gami-points {
      font-size: 3rem;
      font-weight: 900;
      background: linear-gradient(135deg, #fbbf24, #f59e0b);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      margin: 12px 0;
    }

    .gami-badge {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      padding: 8px 20px;
      border-radius: 20px;
      font-weight: 700;
      font-size: .9rem;
      margin: 12px 0 20px;
    }
    .gami-badge.bronce { background: rgba(205,127,50,.2); color: #cd7f32; border: 1px solid rgba(205,127,50,.3); }
    .gami-badge.plata { background: rgba(192,192,192,.2); color: #c0c0c0; border: 1px solid rgba(192,192,192,.3); }
    .gami-badge.oro { background: rgba(251,191,36,.2); color: #fbbf24; border: 1px solid rgba(251,191,36,.3); }
    .gami-badge.diamante { background: rgba(79,125,249,.2); color: #4f7df9; border: 1px solid rgba(79,125,249,.3); }

    .gami-badge-icon { font-size: 1.2rem; }

    .progress-container {
      width: 100%;
      margin: 16px 0;
    }
    .progress-labels {
      display: flex;
      justify-content: space-between;
      font-size: .78rem;
      color: var(--text-secondary);
      margin-bottom: 6px;
    }
    .progress-track {
      height: 14px;
      background: var(--bg-input);
      border-radius: 7px;
      overflow: hidden;
      position: relative;
    }
    .progress-fill {
      height: 100%;
      background: linear-gradient(135deg, #fbbf24, #f59e0b);
      border-radius: 7px;
      transition: width 1s ease;
      position: relative;
    }
    .progress-fill::after {
      content: '';
      position: absolute;
      top: 0; right: 0;
      width: 20px; height: 100%;
      background: linear-gradient(90deg, transparent, rgba(255,255,255,.3));
      animation: shimmer 2s infinite;
    }

    .gami-history {
      margin-top: 20px;
      text-align: left;
      max-height: 160px;
      overflow-y: auto;
    }
    .gami-history-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 8px 12px;
      border-radius: 8px;
      margin-bottom: 4px;
      font-size: .85rem;
    }
    .gami-history-item:nth-child(odd) { background: rgba(79,125,249,.05); }
    .gami-pts-earned { color: var(--accent-gold); font-weight: 700; }

    /* ============================================================
       RECOMENDACIONES
       ============================================================ */
    .recommendations { padding: 60px 0; }
    .reco-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
      gap: 20px;
    }
    .reco-card {
      background: var(--bg-card);
      border-radius: var(--radius);
      padding: 20px;
      border: 1px solid rgba(168,85,247,.15);
      text-align: center;
      transition: transform var(--transition), box-shadow var(--transition);
    }
    .reco-card:hover { transform: translateY(-4px); box-shadow: var(--shadow); }
    .reco-card img { width: 100%; height: 140px; object-fit: cover; border-radius: 8px; margin-bottom: 12px; }
    .reco-reason { font-size: .82rem; color: var(--accent-purple); margin-top: 6px; }

    /* ============================================================
       MARKETPLACE
       ============================================================ */
    .marketplace { padding: 80px 0; }

    .mp-form {
      max-width: 600px;
      margin: 0 auto 40px;
      background: var(--bg-card);
      padding: 32px;
      border-radius: var(--radius);
      border: 1px solid rgba(79,125,249,.1);
    }
    .mp-form h3 { margin-bottom: 20px; text-align: center; }

    .form-group { margin-bottom: 16px; }
    .form-group label { display: block; margin-bottom: 6px; font-weight: 600; font-size: .9rem; color: var(--text-secondary); }
    .form-group input, .form-group select, .form-group textarea {
      width: 100%;
      padding: 10px 14px;
      background: var(--bg-input);
      color: var(--text-primary);
      border: 1px solid rgba(79,125,249,.2);
      border-radius: 8px;
      outline: none;
      transition: border var(--transition);
    }
    .form-group input:focus, .form-group select:focus, .form-group textarea:focus { border-color: var(--accent-blue); }

    .mp-listings {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
      gap: 20px;
    }
    .mp-item {
      background: var(--bg-card);
      border-radius: var(--radius);
      padding: 20px;
      border: 1px solid rgba(168,85,247,.15);
    }
    .mp-item h4 { margin-bottom: 4px; }
    .mp-item .mp-detail { font-size: .85rem; color: var(--text-secondary); }
    .mp-item .mp-price { font-size: 1.1rem; font-weight: 700; margin-top: 8px; color: var(--accent-blue); }
    .mp-item .mp-author { font-size: .78rem; color: var(--accent-purple); margin-top: 4px; }

    /* ============================================================
       ENCUESTA
       ============================================================ */
    .survey { padding: 80px 0; }

    .survey-form {
      max-width: 600px;
      margin: 0 auto 40px;
      background: var(--bg-card);
      padding: 32px;
      border-radius: var(--radius);
      border: 1px solid rgba(79,125,249,.1);
    }

    .radio-group { display: flex; flex-wrap: wrap; gap: 10px; margin-top: 8px; }
    .radio-group label {
      padding: 8px 16px;
      background: var(--bg-input);
      border-radius: 8px;
      cursor: pointer;
      font-size: .9rem;
      transition: background var(--transition), color var(--transition);
      border: 1px solid transparent;
    }
    .radio-group input { display: none; }
    .radio-group label.selected { background: var(--accent-blue); color: #fff; border-color: var(--accent-blue); }

    .results-bars { max-width: 600px; margin: 0 auto; }
    .bar-item { margin-bottom: 14px; }
    .bar-label { display: flex; justify-content: space-between; margin-bottom: 4px; font-size: .9rem; }
    .bar-track { height: 20px; background: var(--bg-input); border-radius: 10px; overflow: hidden; }
    .bar-fill {
      height: 100%;
      background: var(--accent-gradient);
      border-radius: 10px;
      width: 0;
      transition: width 1s ease;
    }

    /* ============================================================
       CARRITO (SIDEBAR)
       ============================================================ */
    .cart-overlay {
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,.5);
      z-index: 200;
      opacity: 0;
      pointer-events: none;
      transition: opacity var(--transition);
    }
    .cart-overlay.open { opacity: 1; pointer-events: all; }

    .cart-sidebar {
      position: fixed;
      top: 0; right: 0;
      width: 380px;
      max-width: 90vw;
      height: 100%;
      background: var(--bg-secondary);
      z-index: 201;
      transform: translateX(100%);
      transition: transform var(--transition);
      display: flex;
      flex-direction: column;
    }
    .cart-sidebar.open { transform: translateX(0); }

    .cart-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 20px;
      border-bottom: 1px solid rgba(79,125,249,.1);
    }
    .cart-header h2 { font-size: 1.2rem; }
    .cart-close { background: none; border: none; color: var(--text-primary); font-size: 1.4rem; }

    .cart-items { flex: 1; overflow-y: auto; padding: 16px; }

    .cart-item {
      display: flex;
      align-items: center;
      gap: 12px;
      padding: 12px;
      background: var(--bg-card);
      border-radius: 8px;
      margin-bottom: 10px;
    }
    .cart-item img { width: 56px; height: 56px; object-fit: cover; border-radius: 6px; }
    .cart-item-info { flex: 1; }
    .cart-item-info h4 { font-size: .95rem; }
    .cart-item-info p { font-size: .82rem; color: var(--text-secondary); }
    .cart-item-remove { background: none; border: none; color: var(--danger); font-size: 1.1rem; }

    .cart-footer {
      padding: 20px;
      border-top: 1px solid rgba(79,125,249,.1);
    }
    .cart-total { display: flex; justify-content: space-between; font-size: 1.1rem; font-weight: 700; margin-bottom: 14px; }

    /* ============================================================
       CHATBOT
       ============================================================ */
    .chat-fab {
      position: fixed;
      bottom: 24px;
      right: 24px;
      width: 56px; height: 56px;
      border-radius: 50%;
      background: var(--accent-gradient);
      color: #fff;
      border: none;
      font-size: 1.5rem;
      z-index: 300;
      box-shadow: 0 6px 24px rgba(79,125,249,.4);
      transition: transform var(--transition);
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .chat-fab:hover { transform: scale(1.1); }

    .chatbot {
      position: fixed;
      bottom: 96px;
      right: 24px;
      width: 370px;
      max-width: calc(100vw - 48px);
      max-height: 520px;
      background: var(--bg-secondary);
      border-radius: 16px;
      border: 1px solid rgba(79,125,249,.15);
      box-shadow: var(--shadow);
      z-index: 301;
      display: flex;
      flex-direction: column;
      transform: scale(0);
      transform-origin: bottom right;
      opacity: 0;
      transition: transform .25s ease, opacity .25s ease;
    }
    .chatbot.open { transform: scale(1); opacity: 1; }

    .chatbot-header {
      padding: 16px;
      background: var(--accent-gradient);
      border-radius: 16px 16px 0 0;
      color: #fff;
      font-weight: 700;
    }

    .chatbot-messages {
      flex: 1;
      overflow-y: auto;
      padding: 16px;
      display: flex;
      flex-direction: column;
      gap: 10px;
      min-height: 250px;
    }

    .chat-msg {
      max-width: 85%;
      padding: 10px 14px;
      border-radius: 12px;
      font-size: .9rem;
      line-height: 1.5;
      animation: fadeInUp .3s ease;
    }
    .chat-msg.bot { background: var(--bg-card); align-self: flex-start; border-bottom-left-radius: 4px; }
    .chat-msg.user { background: var(--accent-blue); color: #fff; align-self: flex-end; border-bottom-right-radius: 4px; }

    .chatbot-input {
      display: flex;
      gap: 8px;
      padding: 12px;
      border-top: 1px solid rgba(79,125,249,.1);
    }
    .chatbot-input input {
      flex: 1;
      padding: 10px 14px;
      background: var(--bg-input);
      color: var(--text-primary);
      border: 1px solid rgba(79,125,249,.2);
      border-radius: 8px;
      outline: none;
    }
    .chatbot-input button { padding: 10px 18px; }

    /* ============================================================
       MODAL
       ============================================================ */
    .modal-overlay {
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,.6);
      z-index: 400;
      display: flex;
      align-items: center;
      justify-content: center;
      opacity: 0;
      pointer-events: none;
      transition: opacity var(--transition);
    }
    .modal-overlay.open { opacity: 1; pointer-events: all; }

    .modal {
      background: var(--bg-secondary);
      border-radius: var(--radius);
      padding: 32px;
      max-width: 440px;
      width: 90%;
      text-align: center;
      transform: translateY(20px);
      transition: transform var(--transition);
    }
    .modal-overlay.open .modal { transform: translateY(0); }
    .modal h3 { margin-bottom: 16px; font-size: 1.2rem; }
    .modal .qr-container { display: flex; justify-content: center; margin: 20px 0; }
    .modal .btn { margin-top: 16px; }

    /* ============================================================
       TOAST NOTIFICATIONS
       ============================================================ */
    .toast-container {
      position: fixed;
      top: 80px;
      right: 20px;
      z-index: 500;
      display: flex;
      flex-direction: column;
      gap: 10px;
    }
    .toast {
      padding: 14px 22px;
      background: var(--bg-card);
      color: var(--text-primary);
      border-radius: 10px;
      border-left: 4px solid var(--accent-blue);
      box-shadow: var(--shadow);
      font-size: .9rem;
      animation: slideInRight .35s ease, fadeOut .4s ease 2.6s forwards;
      max-width: 340px;
    }
    .toast.success { border-left-color: var(--success); }
    .toast.warning { border-left-color: var(--warning); }
    .toast.error { border-left-color: var(--danger); }

    /* ============================================================
       FOOTER
       ============================================================ */
    footer {
      padding: 40px 20px;
      text-align: center;
      color: var(--text-secondary);
      border-top: 1px solid rgba(79,125,249,.1);
      font-size: .9rem;
    }

    /* ============================================================
       SOUND TOGGLE (floating)
       ============================================================ */
    .sound-toggle {
      position: fixed;
      bottom: 24px;
      left: 24px;
      width: 44px; height: 44px;
      border-radius: 50%;
      background: var(--bg-card);
      border: 1px solid rgba(79,125,249,.2);
      color: var(--text-secondary);
      font-size: 1.2rem;
      z-index: 300;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: all var(--transition);
    }
    .sound-toggle:hover { color: var(--text-primary); border-color: var(--accent-blue); }
    .sound-toggle.active { color: var(--accent-blue); border-color: var(--accent-blue); }

    /* ============================================================
       ANIMACIONES
       ============================================================ */
    @keyframes fadeInUp {
      from { opacity: 0; transform: translateY(18px); }
      to { opacity: 1; transform: translateY(0); }
    }
    @keyframes slideInRight {
      from { opacity: 0; transform: translateX(60px); }
      to { opacity: 1; transform: translateX(0); }
    }
    @keyframes fadeOut {
      to { opacity: 0; transform: translateX(40px); }
    }
    @keyframes pulseGlow {
      0%, 100% { opacity: 1; }
      50% { opacity: .7; }
    }
    @keyframes stadiumLights {
      0%, 100% { opacity: .4; }
      50% { opacity: 1; }
    }
    @keyframes livePulse {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: .4; transform: scale(1.3); }
    }
    @keyframes shimmer {
      0% { transform: translateX(-100%); }
      100% { transform: translateX(200%); }
    }
    @keyframes countUp {
      from { opacity: 0; transform: scale(.5); }
      to { opacity: 1; transform: scale(1); }
    }

    /* ============================================================
       RESPONSIVE
       ============================================================ */
    @media (max-width: 768px) {
      .countdown-wrapper { gap: 10px; }
      .countdown-item { min-width: 70px; padding: 12px 14px; }
      .countdown-item .count-num { font-size: 1.6rem; }
      .challenge-grid { grid-template-columns: 1fr; }
      .voting-grid { grid-template-columns: 1fr; }
      .social-icons { gap: 14px; }
      .social-icon-card { width: 80px; padding: 14px 8px; }
      .country-grid { grid-template-columns: repeat(auto-fill, minmax(140px, 1fr)); }
    }
  </style>
</head>
<body>

  <!-- ============================== HEADER ============================== -->
  <header>
    <div class="container">
      <a href="#" class="logo">GameVerse</a>
      <button class="hamburger" id="hamburger" aria-label="Menu">&#9776;</button>
      <nav id="mainNav">
        <a href="#catalog">Catalogo</a>
        <a href="#mundial">Mundial</a>
        <a href="#challenges">Desafios</a>
        <a href="#social">Redes</a>
        <a href="#marketplace">Marketplace</a>
        <a href="#survey">Encuesta</a>
      </nav>
      <div class="header-actions">
        <div class="theme-toggle" id="themeToggle" title="Cambiar tema"></div>
        <button class="cart-btn" id="openCart" aria-label="Carrito">
          &#128722;
          <span class="cart-badge" id="cartBadge">0</span>
        </button>
        <button class="btn btn-sm" id="openQR">QR</button>
      </div>
    </div>
  </header>

  <!-- ============================== HERO ============================== -->
  <section class="hero">
    <div>
      <h1>Tu universo gamer empieza aqui</h1>
      <p>Explora, compra y comparte los mejores videojuegos digitales. Modo Mundial activado con desafios, votaciones y recompensas.</p>
      <div class="hero-actions">
        <a href="#catalog" class="btn">Explorar catalogo</a>
        <a href="#mundial" class="btn btn-gold">Modo Mundial</a>
        <a href="#marketplace" class="btn btn-outline">Marketplace</a>
      </div>
    </div>
  </section>

  <!-- ============================== CATALOGO ============================== -->
  <section class="catalog" id="catalog">
    <div class="container">
      <h2 class="section-title">Catalogo de Juegos</h2>
      <div class="filters">
        <select id="filterPlatform">
          <option value="">Todas las plataformas</option>
          <option value="PC">PC</option>
          <option value="PlayStation">PlayStation</option>
          <option value="Xbox">Xbox</option>
          <option value="Nintendo">Nintendo</option>
        </select>
        <select id="filterGenre">
          <option value="">Todos los generos</option>
          <option value="Accion">Accion</option>
          <option value="RPG">RPG</option>
          <option value="Aventura">Aventura</option>
          <option value="Deportes">Deportes</option>
          <option value="Futbol">Futbol</option>
          <option value="Estrategia">Estrategia</option>
          <option value="Terror">Terror</option>
        </select>
        <input type="number" id="filterBudget" placeholder="Presupuesto max ($)" min="0" style="width:180px" />
        <button class="btn btn-sm btn-gold" id="filterMundial">Juegos de Futbol</button>
      </div>
      <div class="games-grid" id="gamesGrid"></div>
    </div>
  </section>

  <!-- ============================== MUNDIAL ============================== -->
  <section class="mundial-section" id="mundial">
    <div class="container">
      <div class="mundial-banner">
        <h2>Modo Mundial Activado &#9917;&#127918;</h2>
        <p>Vive la fiebre del futbol con juegos exclusivos, desafios y premios especiales</p>
        <div class="countdown-wrapper" id="countdown">
          <div class="countdown-item">
            <div class="count-num" id="countDays">00</div>
            <div class="count-label">Dias</div>
          </div>
          <div class="countdown-item">
            <div class="count-num" id="countHours">00</div>
            <div class="count-label">Horas</div>
          </div>
          <div class="countdown-item">
            <div class="count-num" id="countMins">00</div>
            <div class="count-label">Minutos</div>
          </div>
          <div class="countdown-item">
            <div class="count-num" id="countSecs">00</div>
            <div class="count-label">Segundos</div>
          </div>
        </div>
        <p style="font-size:.9rem;color:var(--text-secondary);position:relative;z-index:1;">Cuenta regresiva para la Final del Mundial 2026</p>
      </div>

      <h3 class="section-title">Selecciones Favoritas</h3>
      <p style="text-align:center;color:var(--text-secondary);margin-bottom:24px;">Vota por tu seleccion favorita</p>
      <div class="country-grid" id="countryGrid"></div>
    </div>
  </section>

  <!-- ============================== DESAFIOS MUNDIAL ============================== -->
  <section class="challenges-section" id="challenges">
    <div class="container">
      <h2 class="section-title">Desafios del Mundial</h2>
      <div class="challenge-grid">

        <!-- Predice el marcador -->
        <div class="challenge-card">
          <h3><span class="challenge-icon">&#9917;</span> Predice el Marcador</h3>
          <p style="color:var(--text-secondary);font-size:.9rem;margin-bottom:16px;">Partido: Semifinal del Mundial</p>
          <div class="predict-form" id="predictForm">
            <div class="predict-row">
              <span class="team-name">&#127463;&#127479; Brasil</span>
              <input type="number" id="predictScore1" min="0" max="20" value="0" />
              <span class="predict-vs">VS</span>
              <input type="number" id="predictScore2" min="0" max="20" value="0" />
              <span class="team-name">&#127467;&#127479; Francia</span>
            </div>
            <button class="btn btn-gold" style="width:100%;justify-content:center;" id="submitPredict">Guardar prediccion (+15 pts)</button>
          </div>
          <div class="live-stats" id="predictStats">
            <h4><span class="live-dot"></span> Estadisticas en vivo</h4>
            <div id="predictStatsContent"></div>
          </div>
        </div>

        <!-- Elige al campeon -->
        <div class="challenge-card">
          <h3><span class="challenge-icon">&#127942;</span> Elige al Campeon</h3>
          <p style="color:var(--text-secondary);font-size:.9rem;margin-bottom:16px;">Quien ganara el Mundial 2026?</p>
          <div class="champion-options" id="championOptions"></div>
          <button class="btn btn-gold" style="width:100%;justify-content:center;margin-top:16px;" id="submitChampion">Confirmar eleccion (+20 pts)</button>
          <div class="live-stats" id="championStats" style="margin-top:16px;">
            <h4><span class="live-dot"></span> Predicciones de la comunidad</h4>
            <div id="championStatsContent"></div>
          </div>
        </div>

      </div>
    </div>
  </section>

  <!-- ============================== VOTACIONES MUNDIAL ============================== -->
  <section class="voting-section" id="voting">
    <div class="container">
      <h2 class="section-title">Votaciones del Mundial</h2>
      <div class="voting-grid">

        <!-- Mejor Seleccion -->
        <div class="voting-card">
          <h3>&#127942; Mejor Seleccion del Torneo</h3>
          <div id="voteTeamOptions"></div>
        </div>

        <!-- Mejor Jugador -->
        <div class="voting-card">
          <h3>&#11088; Mejor Jugador del Mundial</h3>
          <div id="votePlayerOptions"></div>
        </div>

      </div>
    </div>
  </section>

  <!-- ============================== REDES SOCIALES ============================== -->
  <section class="social-section" id="social">
    <div class="container">
      <h2 class="section-title">Siguenos en Redes</h2>
      <div class="social-icons" id="socialIcons">

        <!-- Instagram -->
        <div class="social-icon-card instagram" onclick="window.open('#','_blank')">
          <svg viewBox="0 0 24 24" fill="currentColor"><path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zM12 0C8.741 0 8.333.014 7.053.072 2.695.272.273 2.69.073 7.052.014 8.333 0 8.741 0 12c0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98C8.333 23.986 8.741 24 12 24c3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98C15.668.014 15.259 0 12 0zm0 5.838a6.162 6.162 0 100 12.324 6.162 6.162 0 000-12.324zM12 16a4 4 0 110-8 4 4 0 010 8zm6.406-11.845a1.44 1.44 0 100 2.881 1.44 1.44 0 000-2.881z"/></svg>
          <div class="social-name">Instagram</div>
          <div class="social-followers" data-target="125400">0</div>
        </div>

        <!-- TikTok -->
        <div class="social-icon-card tiktok" onclick="window.open('#','_blank')">
          <svg viewBox="0 0 24 24" fill="currentColor"><path d="M12.525.02c1.31-.02 2.61-.01 3.91-.02.08 1.53.63 3.09 1.75 4.17 1.12 1.11 2.7 1.62 4.24 1.79v4.03c-1.44-.05-2.89-.35-4.2-.97-.57-.26-1.1-.59-1.62-.93-.01 2.92.01 5.84-.02 8.75-.08 1.4-.54 2.79-1.35 3.94-1.31 1.92-3.58 3.17-5.91 3.21-1.43.08-2.86-.31-4.08-1.03-2.02-1.19-3.44-3.37-3.65-5.71-.02-.5-.03-1-.01-1.49.18-1.9 1.12-3.72 2.58-4.96 1.66-1.44 3.98-2.13 6.15-1.72.02 1.48-.04 2.96-.04 4.44-.99-.32-2.15-.23-3.02.37-.63.41-1.11 1.04-1.36 1.75-.21.51-.15 1.07-.14 1.61.24 1.64 1.82 3.02 3.5 2.87 1.12-.01 2.19-.66 2.77-1.61.19-.33.4-.67.41-1.06.1-1.79.06-3.57.07-5.36.01-4.03-.01-8.05.02-12.07z"/></svg>
          <div class="social-name">TikTok</div>
          <div class="social-followers" data-target="89200">0</div>
        </div>

        <!-- X (Twitter) -->
        <div class="social-icon-card twitter" onclick="window.open('#','_blank')">
          <svg viewBox="0 0 24 24" fill="currentColor"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>
          <div class="social-name">X</div>
          <div class="social-followers" data-target="67800">0</div>
        </div>

        <!-- YouTube -->
        <div class="social-icon-card youtube" onclick="window.open('#','_blank')">
          <svg viewBox="0 0 24 24" fill="currentColor"><path d="M23.498 6.186a3.016 3.016 0 00-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 00.502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 002.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 002.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/></svg>
          <div class="social-name">YouTube</div>
          <div class="social-followers" data-target="234500">0</div>
        </div>

        <!-- Discord -->
        <div class="social-icon-card discord" onclick="window.open('#','_blank')">
          <svg viewBox="0 0 24 24" fill="currentColor"><path d="M20.317 4.3698a19.7913 19.7913 0 00-4.8851-1.5152.0741.0741 0 00-.0785.0371c-.211.3753-.4447.8648-.6083 1.2495-1.8447-.2762-3.68-.2762-5.4868 0-.1636-.3933-.4058-.8742-.6177-1.2495a.077.077 0 00-.0785-.037 19.7363 19.7363 0 00-4.8852 1.515.0699.0699 0 00-.0321.0277C.5334 9.0458-.319 13.5799.0992 18.0578a.0824.0824 0 00.0312.0561c2.0528 1.5076 4.0413 2.4228 5.9929 3.0294a.0777.0777 0 00.0842-.0276c.4616-.6304.8731-1.2952 1.226-1.9942a.076.076 0 00-.0416-.1057c-.6528-.2476-1.2743-.5495-1.8722-.8923a.077.077 0 01-.0076-.1277c.1258-.0943.2517-.1923.3718-.2914a.0743.0743 0 01.0776-.0105c3.9278 1.7933 8.18 1.7933 12.0614 0a.0739.0739 0 01.0785.0095c.1202.099.246.1981.3728.2924a.077.077 0 01-.0066.1276 12.2986 12.2986 0 01-1.873.8914.0766.0766 0 00-.0407.1067c.3604.698.7719 1.3628 1.225 1.9932a.076.076 0 00.0842.0286c1.961-.6067 3.9495-1.5219 6.0023-3.0294a.077.077 0 00.0313-.0552c.5004-5.177-.8382-9.6739-3.5485-13.6604a.061.061 0 00-.0312-.0286zM8.02 15.3312c-1.1825 0-2.1569-1.0857-2.1569-2.419 0-1.3332.9555-2.4189 2.157-2.4189 1.2108 0 2.1757 1.0952 2.1568 2.419 0 1.3332-.9555 2.4189-2.1569 2.4189zm7.9748 0c-1.1825 0-2.1569-1.0857-2.1569-2.419 0-1.3332.9554-2.4189 2.1569-2.4189 1.2108 0 2.1757 1.0952 2.1568 2.419 0 1.3332-.946 2.4189-2.1568 2.4189z"/></svg>
          <div class="social-name">Discord</div>
          <div class="social-followers" data-target="45600">0</div>
        </div>

      </div>

      <div class="share-section">
        <p>Comparte GameVerse con tus amigos</p>
        <button class="btn" id="shareBtn">Compartir GameVerse</button>
      </div>
    </div>
  </section>

  <!-- ============================== GAMIFICACION ============================== -->
  <section class="gamification-section" id="gamification">
    <div class="container">
      <h2 class="section-title">Tu Perfil Gamer</h2>
      <div class="gami-card">
        <p style="color:var(--text-secondary);font-size:.9rem;">Puntos acumulados</p>
        <div class="gami-points" id="gamiPoints">0</div>
        <div id="gamiBadge"></div>
        <div class="progress-container">
          <div class="progress-labels">
            <span id="gamiCurrentBadge">Fan Bronce</span>
            <span id="gamiNextBadge">Fan Plata (100 pts)</span>
          </div>
          <div class="progress-track">
            <div class="progress-fill" id="gamiProgressFill" style="width:0%"></div>
          </div>
        </div>
        <p style="color:var(--text-secondary);font-size:.82rem;margin-top:8px;">Gana puntos votando, compartiendo y participando en desafios</p>
        <div class="gami-history" id="gamiHistory"></div>
      </div>
    </div>
  </section>

  <!-- ============================== RECOMENDACIONES ============================== -->
  <section class="recommendations" id="recommendations">
    <div class="container">
      <h2 class="section-title">Recomendado para ti</h2>
      <p style="text-align:center;color:var(--text-secondary);margin-bottom:28px;" id="recoMessage">Agrega juegos al carrito para obtener recomendaciones personalizadas.</p>
      <div class="reco-grid" id="recoGrid"></div>
    </div>
  </section>

  <!-- ============================== MARKETPLACE ============================== -->
  <section class="marketplace" id="marketplace">
    <div class="container">
      <h2 class="section-title">Marketplace Comunitario</h2>
      <div class="mp-form">
        <h3>Publica tu juego</h3>
        <div class="form-group">
          <label for="mpName">Nombre del juego</label>
          <input type="text" id="mpName" placeholder="Ej: Super Adventure 3" />
        </div>
        <div class="form-group">
          <label for="mpPrice">Precio ($)</label>
          <input type="number" id="mpPrice" placeholder="19.99" min="0" step="0.01" />
        </div>
        <div class="form-group">
          <label for="mpPlatform">Plataforma</label>
          <select id="mpPlatform">
            <option value="PC">PC</option>
            <option value="PlayStation">PlayStation</option>
            <option value="Xbox">Xbox</option>
            <option value="Nintendo">Nintendo</option>
          </select>
        </div>
        <div class="form-group">
          <label for="mpGenre">Genero</label>
          <select id="mpGenre">
            <option value="Accion">Accion</option>
            <option value="RPG">RPG</option>
            <option value="Aventura">Aventura</option>
            <option value="Deportes">Deportes</option>
            <option value="Futbol">Futbol</option>
            <option value="Estrategia">Estrategia</option>
            <option value="Terror">Terror</option>
          </select>
        </div>
        <div class="form-group">
          <label for="mpDesc">Descripcion</label>
          <textarea id="mpDesc" rows="3" placeholder="Breve descripcion del juego..."></textarea>
        </div>
        <div class="form-group">
          <label for="mpAuthor">Tu nombre</label>
          <input type="text" id="mpAuthor" placeholder="GamerPro99" />
        </div>
        <button class="btn" style="width:100%;justify-content:center;" id="mpSubmit">Publicar juego (+10 pts)</button>
      </div>
      <div class="mp-listings" id="mpListings"></div>
    </div>
  </section>

  <!-- ============================== ENCUESTA ============================== -->
  <section class="survey" id="survey">
    <div class="container">
      <h2 class="section-title">Encuesta Gamer</h2>
      <div class="survey-form" id="surveyForm">
        <div class="form-group">
          <label>Tu genero de juego favorito:</label>
          <div class="radio-group" data-name="favGenre">
            <label><input type="radio" name="favGenre" value="Accion" /><span>Accion</span></label>
            <label><input type="radio" name="favGenre" value="RPG" /><span>RPG</span></label>
            <label><input type="radio" name="favGenre" value="Aventura" /><span>Aventura</span></label>
            <label><input type="radio" name="favGenre" value="Deportes" /><span>Deportes</span></label>
            <label><input type="radio" name="favGenre" value="Futbol" /><span>Futbol</span></label>
            <label><input type="radio" name="favGenre" value="Estrategia" /><span>Estrategia</span></label>
            <label><input type="radio" name="favGenre" value="Terror" /><span>Terror</span></label>
          </div>
        </div>
        <div class="form-group">
          <label>Plataforma preferida:</label>
          <div class="radio-group" data-name="favPlatform">
            <label><input type="radio" name="favPlatform" value="PC" /><span>PC</span></label>
            <label><input type="radio" name="favPlatform" value="PlayStation" /><span>PlayStation</span></label>
            <label><input type="radio" name="favPlatform" value="Xbox" /><span>Xbox</span></label>
            <label><input type="radio" name="favPlatform" value="Nintendo" /><span>Nintendo</span></label>
          </div>
        </div>
        <div class="form-group">
          <label>Nivel de satisfaccion con GameVerse:</label>
          <div class="radio-group" data-name="satisfaction">
            <label><input type="radio" name="satisfaction" value="Excelente" /><span>Excelente</span></label>
            <label><input type="radio" name="satisfaction" value="Bueno" /><span>Bueno</span></label>
            <label><input type="radio" name="satisfaction" value="Regular" /><span>Regular</span></label>
            <label><input type="radio" name="satisfaction" value="Malo" /><span>Malo</span></label>
          </div>
        </div>
        <button class="btn" style="width:100%;justify-content:center;margin-top:12px;" id="surveySubmit">Enviar encuesta (+10 pts)</button>
      </div>
      <div class="results-bars" id="surveyResults"></div>
    </div>
  </section>

  <!-- ============================== FOOTER ============================== -->
  <footer>
    <div class="container">
      <p>&copy; 2026 GameVerse. Todos los derechos reservados. Hecho con &#10084;&#65039; para gamers.</p>
    </div>
  </footer>

  <!-- ============================== CARRITO SIDEBAR ============================== -->
  <div class="cart-overlay" id="cartOverlay"></div>
  <aside class="cart-sidebar" id="cartSidebar">
    <div class="cart-header">
      <h2>&#128722; Tu carrito</h2>
      <button class="cart-close" id="closeCart">&times;</button>
    </div>
    <div class="cart-items" id="cartItems"></div>
    <div class="cart-footer">
      <div class="cart-total">
        <span>Total:</span>
        <span id="cartTotal">$0.00</span>
      </div>
      <button class="btn" style="width:100%;justify-content:center;" id="cartCheckout">Comprar ahora</button>
      <button class="btn btn-outline btn-sm" style="width:100%;justify-content:center;margin-top:8px;" id="cartShareQR">Compartir carrito (QR)</button>
    </div>
  </aside>

  <!-- ============================== CHATBOT ============================== -->
  <button class="chat-fab" id="chatFab" aria-label="Abrir asistente">&#129302;</button>
  <div class="chatbot" id="chatbot">
    <div class="chatbot-header">&#129302; Asistente GameVerse</div>
    <div class="chatbot-messages" id="chatMessages">
      <div class="chat-msg bot">Hola! Soy tu asistente de GameVerse. Puedo ayudarte a encontrar juegos. Dime tu <b>plataforma</b>, <b>genero</b> o <b>presupuesto</b> y te recomiendo algo. Tambien preguntame sobre el <b>mundial</b>!</div>
    </div>
    <div class="chatbot-input">
      <input type="text" id="chatInput" placeholder="Escribe tu mensaje..." />
      <button class="btn btn-sm" id="chatSend">Enviar</button>
    </div>
  </div>

  <!-- ============================== MODAL QR ============================== -->
  <div class="modal-overlay" id="qrModal">
    <div class="modal">
      <h3 id="qrTitle">Codigo QR</h3>
      <div class="qr-container" id="qrContainer"></div>
      <p style="font-size:.85rem;color:var(--text-secondary);" id="qrDesc">Escanea para compartir esta pagina.</p>
      <button class="btn btn-sm" onclick="closeQRModal()">Cerrar</button>
    </div>
  </div>

  <!-- ============================== TOASTS ============================== -->
  <div class="toast-container" id="toastContainer"></div>

  <!-- ============================== SOUND TOGGLE ============================== -->
  <button class="sound-toggle" id="soundToggle" title="Activar/desactivar sonidos">&#128264;</button>


  <!-- ============================================================
       JAVASCRIPT
       ============================================================ -->
  <script>
    /* =============================================================
       1. BASE DE DATOS DE JUEGOS (incluye futbol/mundial)
       ============================================================= */
    const GAMES = [
      { id: 1,  name: "Cyber Odyssey 2077",     price: 59.99, platform: "PC",          genre: "RPG",        img: "https://images.unsplash.com/photo-1542751371-adc38448a05e?w=400&h=300&fit=crop", mundial: false },
      { id: 2,  name: "Shadow Legends",         price: 49.99, platform: "PlayStation",  genre: "Accion",     img: "https://images.unsplash.com/photo-1511512578047-dfb367046420?w=400&h=300&fit=crop", mundial: false },
      { id: 3,  name: "Galactic Warriors",      price: 39.99, platform: "Xbox",         genre: "Accion",     img: "https://images.unsplash.com/photo-1552820728-8b83bb6b2b28?w=400&h=300&fit=crop", mundial: false },
      { id: 4,  name: "Legend of Zelaria",       price: 54.99, platform: "Nintendo",     genre: "Aventura",   img: "https://images.unsplash.com/photo-1493711662062-fa541adb3fc8?w=400&h=300&fit=crop", mundial: false },
      { id: 5,  name: "FIFA Extreme 26",        price: 44.99, platform: "PlayStation",  genre: "Futbol",     img: "https://images.unsplash.com/photo-1579952363873-27f3bade9f55?w=400&h=300&fit=crop", mundial: true, discount: 30, originalPrice: 64.99 },
      { id: 6,  name: "Dark Realms",            price: 29.99, platform: "PC",           genre: "Terror",     img: "https://images.unsplash.com/photo-1518709268805-4e9042af9f23?w=400&h=300&fit=crop", mundial: false },
      { id: 7,  name: "Empire Tactics",         price: 34.99, platform: "PC",           genre: "Estrategia", img: "https://images.unsplash.com/photo-1611996575749-79a3a250f948?w=400&h=300&fit=crop", mundial: false },
      { id: 8,  name: "Neon Racer",             price: 24.99, platform: "Xbox",         genre: "Accion",     img: "https://images.unsplash.com/photo-1547394765-185e1e68f34e?w=400&h=300&fit=crop", mundial: false },
      { id: 9,  name: "Pokemon Nova",           price: 49.99, platform: "Nintendo",     genre: "RPG",        img: "https://images.unsplash.com/photo-1613523529531-31ba43131e7c?w=400&h=300&fit=crop", mundial: false },
      { id: 10, name: "Resident Nightmare",     price: 39.99, platform: "PlayStation",  genre: "Terror",     img: "https://images.unsplash.com/photo-1509198397868-475647b2a1e5?w=400&h=300&fit=crop", mundial: false },
      { id: 11, name: "Starfield Reborn",       price: 69.99, platform: "Xbox",         genre: "RPG",        img: "https://images.unsplash.com/photo-1614732414444-096e5f1122d5?w=400&h=300&fit=crop", mundial: false },
      { id: 12, name: "Speed Legends",          price: 19.99, platform: "PC",           genre: "Deportes",   img: "https://images.unsplash.com/photo-1511882150382-421056c89033?w=400&h=300&fit=crop", mundial: false },
      { id: 13, name: "eFootball Mundial 26",   price: 29.99, platform: "PC",           genre: "Futbol",     img: "https://images.unsplash.com/photo-1574629810360-7efbbe195018?w=400&h=300&fit=crop", mundial: true, discount: 40, originalPrice: 49.99 },
      { id: 14, name: "Football Manager 2026",  price: 34.99, platform: "PC",           genre: "Futbol",     img: "https://images.unsplash.com/photo-1431324155629-1a6deb1dec8d?w=400&h=300&fit=crop", mundial: true, discount: 25, originalPrice: 46.99 },
      { id: 15, name: "Rocket League World Cup", price: 19.99, platform: "Xbox",        genre: "Futbol",     img: "https://images.unsplash.com/photo-1493711662062-fa541adb3fc8?w=400&h=300&fit=crop", mundial: true, discount: 50, originalPrice: 39.99 },
      { id: 16, name: "FIFA Street Revival",    price: 24.99, platform: "PlayStation",  genre: "Futbol",     img: "https://images.unsplash.com/photo-1560272564-c83b66b1ad12?w=400&h=300&fit=crop", mundial: true, discount: 35, originalPrice: 38.99 },
    ];

    /* =============================================================
       2. ESTADO GLOBAL
       ============================================================= */
    let cart = JSON.parse(localStorage.getItem('gv_cart') || '[]');
    let soundEnabled = false;
    let userPoints = parseInt(localStorage.getItem('gv_points') || '0');
    let pointsHistory = JSON.parse(localStorage.getItem('gv_points_history') || '[]');

    /* =============================================================
       3. SONIDOS (Web Audio API)
       ============================================================= */
    const AudioCtx = window.AudioContext || window.webkitAudioContext;
    let audioCtx = null;

    function playSound(type) {
      if (!soundEnabled) return;
      if (!audioCtx) audioCtx = new AudioCtx();
      const osc = audioCtx.createOscillator();
      const gain = audioCtx.createGain();
      osc.connect(gain);
      gain.connect(audioCtx.destination);
      gain.gain.value = 0.08;

      if (type === 'click') { osc.frequency.value = 600; osc.type = 'sine'; }
      else if (type === 'success') { osc.frequency.value = 800; osc.type = 'sine'; }
      else if (type === 'points') { osc.frequency.value = 1000; osc.type = 'triangle'; }
      else { osc.frequency.value = 440; osc.type = 'sine'; }

      osc.start();
      gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + 0.2);
      osc.stop(audioCtx.currentTime + 0.2);
    }

    document.getElementById('soundToggle').addEventListener('click', () => {
      soundEnabled = !soundEnabled;
      const btn = document.getElementById('soundToggle');
      btn.classList.toggle('active', soundEnabled);
      btn.innerHTML = soundEnabled ? '&#128266;' : '&#128264;';
      toast(soundEnabled ? 'Sonidos activados' : 'Sonidos desactivados');
    });

    /* =============================================================
       4. FUNCIONES DE UTILIDAD
       ============================================================= */
    function toast(message, type = 'success') {
      const container = document.getElementById('toastContainer');
      const el = document.createElement('div');
      el.className = `toast ${type}`;
      el.textContent = message;
      container.appendChild(el);
      setTimeout(() => el.remove(), 3200);
    }

    function saveCart() {
      localStorage.setItem('gv_cart', JSON.stringify(cart));
      updateCartUI();
      updateRecommendations();
    }

    /* --- Gamificacion: sumar puntos --- */
    function addPoints(amount, reason) {
      userPoints += amount;
      pointsHistory.unshift({ amount, reason, date: new Date().toLocaleString() });
      if (pointsHistory.length > 20) pointsHistory = pointsHistory.slice(0, 20);
      localStorage.setItem('gv_points', userPoints.toString());
      localStorage.setItem('gv_points_history', JSON.stringify(pointsHistory));
      playSound('points');
      updateGamification();
    }

    function formatFollowers(n) {
      if (n >= 1000000) return (n / 1000000).toFixed(1) + 'M';
      if (n >= 1000) return (n / 1000).toFixed(1) + 'K';
      return n.toString();
    }

    /* =============================================================
       5. CATALOGO
       ============================================================= */
    let mundialFilter = false;

    function renderGames() {
      const platform = document.getElementById('filterPlatform').value;
      const genre    = document.getElementById('filterGenre').value;
      const budget   = parseFloat(document.getElementById('filterBudget').value) || Infinity;

      let filtered = GAMES.filter(g =>
        (!platform || g.platform === platform) &&
        (!genre || g.genre === genre) &&
        (g.price <= budget)
      );

      if (mundialFilter) {
        filtered = filtered.filter(g => g.genre === 'Futbol' || g.mundial);
      }

      const grid = document.getElementById('gamesGrid');
      grid.innerHTML = filtered.map((g, i) => {
        const tags = [`<span class="game-tag">${g.platform}</span>`, `<span class="game-tag">${g.genre}</span>`];
        if (g.mundial) tags.push('<span class="game-tag mundial">Mundial Edition</span>');
        if (g.discount) tags.push('<span class="game-tag limited">Evento Limitado</span>');

        const priceHTML = g.discount
          ? `<span class="game-price-old">$${g.originalPrice.toFixed(2)}</span><span class="game-price">$${g.price.toFixed(2)}</span>`
          : `<span class="game-price">$${g.price.toFixed(2)}</span>`;

        const ribbonHTML = g.discount ? `<div class="discount-ribbon">-${g.discount}%</div>` : '';

        return `
          <div class="game-card" style="animation-delay:${i * .07}s">
            ${ribbonHTML}
            <img src="${g.img}" alt="${g.name}" loading="lazy" />
            <div class="game-info">
              <h3>${g.name}</h3>
              <div class="game-meta">${tags.join('')}</div>
              <div class="game-bottom">
                <div>${priceHTML}</div>
                <button class="btn btn-sm" onclick="addToCart(${g.id})">Agregar</button>
              </div>
            </div>
          </div>
        `;
      }).join('');
    }

    document.getElementById('filterPlatform').addEventListener('change', renderGames);
    document.getElementById('filterGenre').addEventListener('change', renderGames);
    document.getElementById('filterBudget').addEventListener('input', renderGames);
    document.getElementById('filterMundial').addEventListener('click', () => {
      mundialFilter = !mundialFilter;
      const btn = document.getElementById('filterMundial');
      btn.style.opacity = mundialFilter ? '1' : '.7';
      btn.textContent = mundialFilter ? 'Ver todos' : 'Juegos de Futbol';
      playSound('click');
      renderGames();
    });

    /* =============================================================
       6. CARRITO
       ============================================================= */
    function addToCart(id) {
      const game = GAMES.find(g => g.id === id);
      if (!game) return;
      const existing = cart.find(c => c.id === id);
      if (existing) { existing.qty++; }
      else { cart.push({ ...game, qty: 1 }); }
      saveCart();
      playSound('success');
      toast(`${game.name} agregado al carrito`);
    }

    function removeFromCart(id) {
      cart = cart.filter(c => c.id !== id);
      saveCart();
    }

    function updateCartUI() {
      const totalQty = cart.reduce((s, c) => s + c.qty, 0);
      document.getElementById('cartBadge').textContent = totalQty;
      const container = document.getElementById('cartItems');
      if (cart.length === 0) {
        container.innerHTML = '<p style="text-align:center;color:var(--text-secondary);padding:40px 0;">Tu carrito esta vacio</p>';
      } else {
        container.innerHTML = cart.map(c => `
          <div class="cart-item">
            <img src="${c.img}" alt="${c.name}" />
            <div class="cart-item-info">
              <h4>${c.name}</h4>
              <p>${c.platform} &middot; x${c.qty} &middot; $${(c.price * c.qty).toFixed(2)}</p>
            </div>
            <button class="cart-item-remove" onclick="removeFromCart(${c.id})">&times;</button>
          </div>
        `).join('');
      }
      const total = cart.reduce((s, c) => s + c.price * c.qty, 0);
      document.getElementById('cartTotal').textContent = `$${total.toFixed(2)}`;
    }

    document.getElementById('openCart').addEventListener('click', () => {
      document.getElementById('cartSidebar').classList.add('open');
      document.getElementById('cartOverlay').classList.add('open');
    });
    function closeCart() {
      document.getElementById('cartSidebar').classList.remove('open');
      document.getElementById('cartOverlay').classList.remove('open');
    }
    document.getElementById('closeCart').addEventListener('click', closeCart);
    document.getElementById('cartOverlay').addEventListener('click', closeCart);

    document.getElementById('cartCheckout').addEventListener('click', () => {
      if (cart.length === 0) return toast('El carrito esta vacio', 'warning');
      playSound('success');
      toast('Compra simulada exitosamente!');
      addPoints(25, 'Compra realizada');
      cart = [];
      saveCart();
    });

    /* =============================================================
       7. RECOMENDACIONES
       ============================================================= */
    function updateRecommendations() {
      const grid = document.getElementById('recoGrid');
      const msg  = document.getElementById('recoMessage');
      if (cart.length === 0) { msg.style.display = 'block'; grid.innerHTML = ''; return; }
      msg.style.display = 'none';
      const platforms = [...new Set(cart.map(c => c.platform))];
      const genres    = [...new Set(cart.map(c => c.genre))];
      const cartIds   = cart.map(c => c.id);
      let recs = GAMES.filter(g => !cartIds.includes(g.id) && (platforms.includes(g.platform) || genres.includes(g.genre)));
      recs = recs.slice(0, 4);
      grid.innerHTML = recs.map(g => {
        const reason = platforms.includes(g.platform) && genres.includes(g.genre)
          ? 'Misma plataforma y genero'
          : platforms.includes(g.platform) ? `Tambien en ${g.platform}` : `Porque te gusta ${g.genre}`;
        return `
          <div class="reco-card">
            <img src="${g.img}" alt="${g.name}" loading="lazy" />
            <h4>${g.name}</h4>
            <span class="game-price">$${g.price.toFixed(2)}</span>
            <p class="reco-reason">${reason}</p>
            <button class="btn btn-sm" style="margin-top:10px;" onclick="addToCart(${g.id})">Agregar</button>
          </div>
        `;
      }).join('');
    }

    /* =============================================================
       8. MARKETPLACE
       ============================================================= */
    let mpItems = JSON.parse(localStorage.getItem('gv_marketplace') || '[]');

    function renderMarketplace() {
      const container = document.getElementById('mpListings');
      if (mpItems.length === 0) {
        container.innerHTML = '<p style="text-align:center;color:var(--text-secondary);grid-column:1/-1;">Aun no hay publicaciones. Se el primero!</p>';
        return;
      }
      container.innerHTML = mpItems.map(item => `
        <div class="mp-item">
          <h4>${item.name}</h4>
          <p class="mp-detail">${item.platform} &middot; ${item.genre}</p>
          <p class="mp-detail">${item.desc || 'Sin descripcion'}</p>
          <p class="mp-price">$${parseFloat(item.price).toFixed(2)}</p>
          <p class="mp-author">Publicado por: ${item.author || 'Anonimo'}</p>
        </div>
      `).join('');
    }

    document.getElementById('mpSubmit').addEventListener('click', () => {
      const name     = document.getElementById('mpName').value.trim();
      const price    = document.getElementById('mpPrice').value;
      const platform = document.getElementById('mpPlatform').value;
      const genre    = document.getElementById('mpGenre').value;
      const desc     = document.getElementById('mpDesc').value.trim();
      const author   = document.getElementById('mpAuthor').value.trim();
      if (!name || !price) return toast('Completa el nombre y precio', 'warning');
      mpItems.unshift({ name, price, platform, genre, desc, author });
      localStorage.setItem('gv_marketplace', JSON.stringify(mpItems));
      renderMarketplace();
      playSound('success');
      toast('Juego publicado en el marketplace!');
      addPoints(10, 'Publicacion en marketplace');
      document.getElementById('mpName').value = '';
      document.getElementById('mpPrice').value = '';
      document.getElementById('mpDesc').value = '';
      document.getElementById('mpAuthor').value = '';
    });

    /* =============================================================
       9. ENCUESTA
       ============================================================= */
    document.querySelectorAll('.radio-group label').forEach(label => {
      label.addEventListener('click', () => {
        const group = label.closest('.radio-group');
        group.querySelectorAll('label').forEach(l => l.classList.remove('selected'));
        label.classList.add('selected');
        playSound('click');
      });
    });

    let surveyData = JSON.parse(localStorage.getItem('gv_survey') || '[]');

    document.getElementById('surveySubmit').addEventListener('click', () => {
      const favGenre     = document.querySelector('input[name="favGenre"]:checked');
      const favPlatform  = document.querySelector('input[name="favPlatform"]:checked');
      const satisfaction = document.querySelector('input[name="satisfaction"]:checked');
      if (!favGenre || !favPlatform || !satisfaction) return toast('Por favor responde todas las preguntas', 'warning');
      surveyData.push({ genre: favGenre.value, platform: favPlatform.value, satisfaction: satisfaction.value, date: new Date().toISOString() });
      localStorage.setItem('gv_survey', JSON.stringify(surveyData));
      playSound('success');
      toast('Encuesta enviada! Gracias por participar.');
      addPoints(10, 'Encuesta completada');
      renderSurveyResults();
    });

    function renderSurveyResults() {
      if (surveyData.length === 0) return;
      const container = document.getElementById('surveyResults');
      function countField(field) {
        const counts = {};
        surveyData.forEach(s => { counts[s[field]] = (counts[s[field]] || 0) + 1; });
        return counts;
      }
      const genreCounts = countField('genre');
      const platCounts  = countField('platform');
      const satCounts   = countField('satisfaction');
      const total       = surveyData.length;
      function barsHTML(title, counts) {
        let html = `<h4 style="margin:20px 0 12px;font-size:1rem;color:var(--text-secondary)">${title} (${total} respuestas)</h4>`;
        for (const [key, val] of Object.entries(counts)) {
          const pct = Math.round((val / total) * 100);
          html += `<div class="bar-item"><div class="bar-label"><span>${key}</span><span>${pct}%</span></div><div class="bar-track"><div class="bar-fill" style="width:0" data-width="${pct}%"></div></div></div>`;
        }
        return html;
      }
      container.innerHTML = barsHTML('Genero favorito', genreCounts) + barsHTML('Plataforma preferida', platCounts) + barsHTML('Satisfaccion', satCounts);
      setTimeout(() => { container.querySelectorAll('.bar-fill').forEach(bar => { bar.style.width = bar.dataset.width; }); }, 100);
    }

    /* =============================================================
       10. CHATBOT
       ============================================================= */
    const chatFab      = document.getElementById('chatFab');
    const chatbot      = document.getElementById('chatbot');
    const chatMessages = document.getElementById('chatMessages');
    const chatInput    = document.getElementById('chatInput');
    const chatSend     = document.getElementById('chatSend');
    let chatOpen = false;

    chatFab.addEventListener('click', () => { chatOpen = !chatOpen; chatbot.classList.toggle('open', chatOpen); playSound('click'); });

    function addChatMsg(text, sender = 'bot') {
      const div = document.createElement('div');
      div.className = `chat-msg ${sender}`;
      div.innerHTML = text;
      chatMessages.appendChild(div);
      chatMessages.scrollTop = chatMessages.scrollHeight;
    }

    function botReply(userMsg) {
      const msg = userMsg.toLowerCase();
      const platforms = ['pc', 'playstation', 'xbox', 'nintendo'];
      const detectedPlatform = platforms.find(p => msg.includes(p));
      const genres = ['accion', 'rpg', 'aventura', 'deportes', 'estrategia', 'terror', 'futbol'];
      const detectedGenre = genres.find(g => msg.includes(g));
      const budgetMatch = msg.match(/(\d+)/);
      const budget = budgetMatch ? parseFloat(budgetMatch[1]) : null;

      if (msg.includes('mundial') || msg.includes('world cup')) {
        const futbol = GAMES.filter(g => g.mundial);
        let reply = "Juegos especiales del <b>Mundial 2026</b>:<br><br>";
        futbol.forEach(g => { reply += `<b>${g.name}</b> - $${g.price.toFixed(2)} (-${g.discount}%)<br>`; });
        reply += "<br>Visita la seccion Mundial para desafios y votaciones!";
        return reply;
      }
      if (msg.includes('futbol') || msg.includes('soccer') || msg.includes('football')) {
        const futbol = GAMES.filter(g => g.genre === 'Futbol');
        let reply = "Juegos de <b>Futbol</b> disponibles:<br><br>";
        futbol.forEach(g => { reply += `<b>${g.name}</b> - $${g.price.toFixed(2)} (${g.platform})<br>`; });
        return reply;
      }

      let results = GAMES;
      if (detectedPlatform) results = results.filter(g => g.platform.toLowerCase() === detectedPlatform);
      if (detectedGenre) results = results.filter(g => g.genre.toLowerCase() === detectedGenre);
      if (budget) results = results.filter(g => g.price <= budget);

      if (detectedPlatform || detectedGenre || budget) {
        if (results.length === 0) return "No encontre juegos con esos filtros. Prueba con otros criterios.";
        const picks = results.slice(0, 3);
        let reply = "Aqui van mis recomendaciones:<br><br>";
        picks.forEach(g => { reply += `<b>${g.name}</b> - $${g.price.toFixed(2)} (${g.platform}, ${g.genre})<br>`; });
        return reply;
      }

      if (msg.includes('hola') || msg.includes('hey') || msg.includes('buenas')) return "Hola! Dime que plataforma usas, que genero te gusta, o cuanto quieres gastar y te recomiendo juegos. Tambien preguntame sobre el <b>mundial</b>!";
      if (msg.includes('barato') || msg.includes('economico') || msg.includes('oferta')) {
        const cheap = [...GAMES].sort((a, b) => a.price - b.price).slice(0, 3);
        let reply = "Los juegos mas economicos:<br><br>";
        cheap.forEach(g => { reply += `<b>${g.name}</b> - $${g.price.toFixed(2)}<br>`; });
        return reply;
      }
      if (msg.includes('caro') || msg.includes('premium') || msg.includes('mejor')) {
        const top = [...GAMES].sort((a, b) => b.price - a.price).slice(0, 3);
        let reply = "Los juegos premium:<br><br>";
        top.forEach(g => { reply += `<b>${g.name}</b> - $${g.price.toFixed(2)}<br>`; });
        return reply;
      }
      if (msg.includes('puntos') || msg.includes('perfil')) return `Tienes <b>${userPoints} puntos</b>. Sigue participando en desafios, votaciones y encuestas para subir de nivel!`;
      if (msg.includes('gracias') || msg.includes('thanks')) return "De nada! Siempre aqui para ayudarte. &#127918;";
      return "Puedo recomendarte juegos! Dime tu <b>plataforma</b> (PC, PlayStation, Xbox, Nintendo), <b>genero</b> (Accion, RPG, Futbol, etc.), <b>presupuesto</b> (ej: 30), o preguntame sobre el <b>mundial</b>.";
    }

    function handleChat() {
      const msg = chatInput.value.trim();
      if (!msg) return;
      addChatMsg(msg, 'user');
      chatInput.value = '';
      setTimeout(() => { addChatMsg(botReply(msg), 'bot'); }, 500 + Math.random() * 500);
    }
    chatSend.addEventListener('click', handleChat);
    chatInput.addEventListener('keydown', e => { if (e.key === 'Enter') handleChat(); });

    /* =============================================================
       11. QR CODE
       ============================================================= */
    function openQRModal(title, text, desc) {
      document.getElementById('qrTitle').textContent = title;
      document.getElementById('qrDesc').textContent  = desc;
      const container = document.getElementById('qrContainer');
      container.innerHTML = '';
      new QRCode(container, { text, width: 200, height: 200, colorDark: '#4f7df9', colorLight: '#0b0e17', correctLevel: QRCode.CorrectLevel.H });
      document.getElementById('qrModal').classList.add('open');
    }
    function closeQRModal() { document.getElementById('qrModal').classList.remove('open'); }

    document.getElementById('openQR').addEventListener('click', () => {
      openQRModal('Compartir GameVerse', window.location.href, 'Escanea para compartir esta pagina con tus amigos.');
      toast('QR generado!');
    });
    document.getElementById('cartShareQR').addEventListener('click', () => {
      if (cart.length === 0) return toast('Agrega juegos al carrito primero', 'warning');
      const cartText = cart.map(c => `${c.name} x${c.qty} - $${(c.price * c.qty).toFixed(2)}`).join('\n');
      openQRModal('Compartir carrito', `${window.location.href}?cart=${encodeURIComponent(cartText)}`, 'Escanea para ver el contenido del carrito.');
      toast('QR del carrito generado!');
    });
    document.getElementById('qrModal').addEventListener('click', (e) => { if (e.target === document.getElementById('qrModal')) closeQRModal(); });

    /* =============================================================
       12. TEMA
       ============================================================= */
    const themeToggle = document.getElementById('themeToggle');
    let lightMode = localStorage.getItem('gv_theme') === 'light';
    if (lightMode) document.documentElement.classList.add('light');
    themeToggle.addEventListener('click', () => {
      lightMode = !lightMode;
      document.documentElement.classList.toggle('light', lightMode);
      localStorage.setItem('gv_theme', lightMode ? 'light' : 'dark');
    });

    /* =============================================================
       13. MENU MOVIL
       ============================================================= */
    document.getElementById('hamburger').addEventListener('click', () => { document.getElementById('mainNav').classList.toggle('open'); });
    document.querySelectorAll('#mainNav a').forEach(a => { a.addEventListener('click', () => document.getElementById('mainNav').classList.remove('open')); });

    /* =============================================================
       14. COUNTDOWN MUNDIAL
       ============================================================= */
    const mundialDate = new Date('2026-07-19T18:00:00');
    function updateCountdown() {
      const now = new Date();
      const diff = mundialDate - now;
      if (diff <= 0) {
        document.getElementById('countDays').textContent = '00';
        document.getElementById('countHours').textContent = '00';
        document.getElementById('countMins').textContent = '00';
        document.getElementById('countSecs').textContent = '00';
        return;
      }
      const d = Math.floor(diff / (1000*60*60*24));
      const h = Math.floor((diff % (1000*60*60*24)) / (1000*60*60));
      const m = Math.floor((diff % (1000*60*60)) / (1000*60));
      const s = Math.floor((diff % (1000*60)) / 1000);
      document.getElementById('countDays').textContent = String(d).padStart(2, '0');
      document.getElementById('countHours').textContent = String(h).padStart(2, '0');
      document.getElementById('countMins').textContent = String(m).padStart(2, '0');
      document.getElementById('countSecs').textContent = String(s).padStart(2, '0');
    }
    updateCountdown();
    setInterval(updateCountdown, 1000);

    /* =============================================================
       15. SELECCIONES FAVORITAS (voto pais)
       ============================================================= */
    const COUNTRIES = [
      { code: 'BR', flag: '\u{1F1E7}\u{1F1F7}', name: 'Brasil' },
      { code: 'AR', flag: '\u{1F1E6}\u{1F1F7}', name: 'Argentina' },
      { code: 'FR', flag: '\u{1F1EB}\u{1F1F7}', name: 'Francia' },
      { code: 'DE', flag: '\u{1F1E9}\u{1F1EA}', name: 'Alemania' },
      { code: 'ES', flag: '\u{1F1EA}\u{1F1F8}', name: 'Espana' },
      { code: 'MX', flag: '\u{1F1F2}\u{1F1FD}', name: 'Mexico' },
      { code: 'GB', flag: '\u{1F1EC}\u{1F1E7}', name: 'Inglaterra' },
      { code: 'PT', flag: '\u{1F1F5}\u{1F1F9}', name: 'Portugal' },
      { code: 'IT', flag: '\u{1F1EE}\u{1F1F9}', name: 'Italia' },
      { code: 'CO', flag: '\u{1F1E8}\u{1F1F4}', name: 'Colombia' },
      { code: 'US', flag: '\u{1F1FA}\u{1F1F8}', name: 'USA' },
      { code: 'JP', flag: '\u{1F1EF}\u{1F1F5}', name: 'Japon' },
    ];

    let countryVotes = JSON.parse(localStorage.getItem('gv_country_votes') || '{}');
    let userCountryVote = localStorage.getItem('gv_my_country_vote') || null;

    /* Simulacion: agregar votos base */
    if (Object.keys(countryVotes).length === 0) {
      COUNTRIES.forEach(c => { countryVotes[c.code] = Math.floor(Math.random() * 80) + 10; });
      localStorage.setItem('gv_country_votes', JSON.stringify(countryVotes));
    }

    function renderCountries() {
      const grid = document.getElementById('countryGrid');
      const totalVotes = Object.values(countryVotes).reduce((a, b) => a + b, 0);
      grid.innerHTML = COUNTRIES.map((c, i) => {
        const votes = countryVotes[c.code] || 0;
        const pct = totalVotes > 0 ? Math.round((votes / totalVotes) * 100) : 0;
        const isVoted = userCountryVote === c.code;
        return `
          <div class="country-card ${isVoted ? 'voted' : ''}" style="animation-delay:${i * .05}s" onclick="voteCountry('${c.code}')">
            <span class="country-flag">${c.flag}</span>
            <div class="country-name">${c.name}</div>
            <div class="country-votes">${votes} votos (${pct}%)</div>
          </div>
        `;
      }).join('');
    }

    function voteCountry(code) {
      if (userCountryVote) {
        countryVotes[userCountryVote] = Math.max(0, (countryVotes[userCountryVote] || 1) - 1);
      }
      countryVotes[code] = (countryVotes[code] || 0) + 1;
      userCountryVote = code;
      localStorage.setItem('gv_country_votes', JSON.stringify(countryVotes));
      localStorage.setItem('gv_my_country_vote', code);
      playSound('click');
      toast(`Votaste por ${COUNTRIES.find(c => c.code === code).name}!`);
      if (!userCountryVote) addPoints(5, 'Voto seleccion favorita');
      renderCountries();
    }

    /* =============================================================
       16. DESAFIOS - Predice marcador
       ============================================================= */
    let predictions = JSON.parse(localStorage.getItem('gv_predictions') || '[]');

    /* Simulacion de estadisticas */
    if (predictions.length === 0) {
      for (let i = 0; i < 25; i++) {
        predictions.push({ s1: Math.floor(Math.random() * 5), s2: Math.floor(Math.random() * 5) });
      }
    }

    document.getElementById('submitPredict').addEventListener('click', () => {
      const s1 = parseInt(document.getElementById('predictScore1').value) || 0;
      const s2 = parseInt(document.getElementById('predictScore2').value) || 0;
      predictions.push({ s1, s2 });
      localStorage.setItem('gv_predictions', JSON.stringify(predictions));
      playSound('success');
      toast(`Prediccion guardada: Brasil ${s1} - ${s2} Francia`);
      addPoints(15, 'Prediccion de marcador');
      renderPredictStats();
    });

    function renderPredictStats() {
      const total = predictions.length;
      const brasilWins = predictions.filter(p => p.s1 > p.s2).length;
      const draw = predictions.filter(p => p.s1 === p.s2).length;
      const franceWins = predictions.filter(p => p.s1 < p.s2).length;
      const container = document.getElementById('predictStatsContent');
      container.innerHTML = `
        <div style="display:flex;justify-content:space-between;font-size:.85rem;margin-bottom:8px;">
          <span>Brasil gana: ${Math.round((brasilWins/total)*100)}%</span>
          <span>Empate: ${Math.round((draw/total)*100)}%</span>
          <span>Francia gana: ${Math.round((franceWins/total)*100)}%</span>
        </div>
        <div style="display:flex;height:8px;border-radius:4px;overflow:hidden;">
          <div style="width:${(brasilWins/total)*100}%;background:#22c55e;"></div>
          <div style="width:${(draw/total)*100}%;background:#f59e0b;"></div>
          <div style="width:${(franceWins/total)*100}%;background:#4f7df9;"></div>
        </div>
        <p style="font-size:.78rem;color:var(--text-secondary);margin-top:8px;">${total} predicciones totales</p>
      `;
    }

    /* =============================================================
       17. DESAFIOS - Elige al campeon
       ============================================================= */
    let championVotes = JSON.parse(localStorage.getItem('gv_champion_votes') || '{}');
    let myChampion = localStorage.getItem('gv_my_champion') || null;

    /* Simulacion */
    if (Object.keys(championVotes).length === 0) {
      COUNTRIES.forEach(c => { championVotes[c.code] = Math.floor(Math.random() * 40) + 5; });
      localStorage.setItem('gv_champion_votes', JSON.stringify(championVotes));
    }

    function renderChampionOptions() {
      const container = document.getElementById('championOptions');
      container.innerHTML = COUNTRIES.map(c =>
        `<div class="champion-option ${myChampion === c.code ? 'selected' : ''}" data-code="${c.code}" onclick="selectChampion('${c.code}')">
          <span class="ch-flag">${c.flag}</span>
          ${c.name}
        </div>`
      ).join('');
    }

    function selectChampion(code) {
      myChampion = code;
      playSound('click');
      document.querySelectorAll('.champion-option').forEach(el => {
        el.classList.toggle('selected', el.dataset.code === code);
      });
    }

    document.getElementById('submitChampion').addEventListener('click', () => {
      if (!myChampion) return toast('Selecciona un pais primero', 'warning');
      championVotes[myChampion] = (championVotes[myChampion] || 0) + 1;
      localStorage.setItem('gv_champion_votes', JSON.stringify(championVotes));
      localStorage.setItem('gv_my_champion', myChampion);
      playSound('success');
      toast(`Elegiste a ${COUNTRIES.find(c => c.code === myChampion).name} como campeon!`);
      addPoints(20, 'Eleccion de campeon');
      renderChampionStats();
    });

    function renderChampionStats() {
      const total = Object.values(championVotes).reduce((a, b) => a + b, 0);
      const sorted = Object.entries(championVotes).sort((a, b) => b[1] - a[1]).slice(0, 5);
      const container = document.getElementById('championStatsContent');
      container.innerHTML = sorted.map(([code, votes]) => {
        const c = COUNTRIES.find(cc => cc.code === code);
        const pct = Math.round((votes / total) * 100);
        return `
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:8px;">
            <span style="font-size:1.1rem;">${c ? c.flag : ''}</span>
            <span style="flex:1;font-size:.85rem;font-weight:600;">${c ? c.name : code}</span>
            <span style="font-size:.85rem;color:var(--accent-gold);font-weight:700;">${pct}%</span>
          </div>
          <div style="height:4px;background:var(--bg-input);border-radius:2px;margin-bottom:10px;overflow:hidden;">
            <div style="width:${pct}%;height:100%;background:linear-gradient(135deg,#fbbf24,#f59e0b);border-radius:2px;transition:width 1s;"></div>
          </div>
        `;
      }).join('');
    }

    /* =============================================================
       18. VOTACIONES MUNDIAL
       ============================================================= */
    const VOTE_TEAMS = [
      { id: 'BR', icon: '\u{1F1E7}\u{1F1F7}', name: 'Brasil' },
      { id: 'AR', icon: '\u{1F1E6}\u{1F1F7}', name: 'Argentina' },
      { id: 'FR', icon: '\u{1F1EB}\u{1F1F7}', name: 'Francia' },
      { id: 'ES', icon: '\u{1F1EA}\u{1F1F8}', name: 'Espana' },
      { id: 'DE', icon: '\u{1F1E9}\u{1F1EA}', name: 'Alemania' },
      { id: 'GB', icon: '\u{1F1EC}\u{1F1E7}', name: 'Inglaterra' },
    ];

    const VOTE_PLAYERS = [
      { id: 'messi', icon: '\u{26BD}', name: 'Lionel Messi' },
      { id: 'mbappe', icon: '\u{26BD}', name: 'Kylian Mbappe' },
      { id: 'haaland', icon: '\u{26BD}', name: 'Erling Haaland' },
      { id: 'vini', icon: '\u{26BD}', name: 'Vinicius Jr' },
      { id: 'bellingham', icon: '\u{26BD}', name: 'Jude Bellingham' },
      { id: 'salah', icon: '\u{26BD}', name: 'Mohamed Salah' },
    ];

    let teamVotes = JSON.parse(localStorage.getItem('gv_team_votes') || '{}');
    let playerVotes = JSON.parse(localStorage.getItem('gv_player_votes') || '{}');
    let myTeamVote = localStorage.getItem('gv_my_team_vote') || null;
    let myPlayerVote = localStorage.getItem('gv_my_player_vote') || null;

    /* Simulacion */
    if (Object.keys(teamVotes).length === 0) { VOTE_TEAMS.forEach(t => { teamVotes[t.id] = Math.floor(Math.random() * 60) + 10; }); localStorage.setItem('gv_team_votes', JSON.stringify(teamVotes)); }
    if (Object.keys(playerVotes).length === 0) { VOTE_PLAYERS.forEach(p => { playerVotes[p.id] = Math.floor(Math.random() * 60) + 10; }); localStorage.setItem('gv_player_votes', JSON.stringify(playerVotes)); }

    function renderVoteOptions(items, votes, myVote, containerId, storageKey, myKey, label) {
      const total = Object.values(votes).reduce((a, b) => a + b, 0);
      const container = document.getElementById(containerId);
      container.innerHTML = items.map(item => {
        const v = votes[item.id] || 0;
        const pct = total > 0 ? Math.round((v / total) * 100) : 0;
        const isVoted = myVote === item.id;
        return `
          <div class="vote-option ${isVoted ? 'voted' : ''}" onclick="castVote('${item.id}', '${containerId}', '${storageKey}', '${myKey}', '${label}')">
            <span class="vote-icon">${item.icon}</span>
            <span class="vote-name">${item.name}</span>
            <span class="vote-pct">${pct}%</span>
          </div>
        `;
      }).join('');
    }

    function castVote(id, containerId, storageKey, myKey, label) {
      let votes, items, myVote;
      if (storageKey === 'gv_team_votes') { votes = teamVotes; items = VOTE_TEAMS; myVote = myTeamVote; }
      else { votes = playerVotes; items = VOTE_PLAYERS; myVote = myPlayerVote; }

      if (myVote) { votes[myVote] = Math.max(0, (votes[myVote] || 1) - 1); }
      votes[id] = (votes[id] || 0) + 1;

      if (storageKey === 'gv_team_votes') { myTeamVote = id; }
      else { myPlayerVote = id; }

      localStorage.setItem(storageKey, JSON.stringify(votes));
      localStorage.setItem(myKey, id);
      playSound('click');
      const item = items.find(i => i.id === id);
      toast(`Votaste por ${item.name}!`);
      if (!myVote) addPoints(5, `Voto: ${label}`);
      renderAllVotes();
    }

    function renderAllVotes() {
      renderVoteOptions(VOTE_TEAMS, teamVotes, myTeamVote, 'voteTeamOptions', 'gv_team_votes', 'gv_my_team_vote', 'Mejor seleccion');
      renderVoteOptions(VOTE_PLAYERS, playerVotes, myPlayerVote, 'votePlayerOptions', 'gv_player_votes', 'gv_my_player_vote', 'Mejor jugador');
    }

    /* =============================================================
       19. REDES SOCIALES - Contador animado
       ============================================================= */
    function animateFollowers() {
      document.querySelectorAll('.social-followers').forEach(el => {
        const target = parseInt(el.dataset.target);
        const duration = 2000;
        const start = performance.now();
        function step(now) {
          const progress = Math.min((now - start) / duration, 1);
          const eased = 1 - Math.pow(1 - progress, 3);
          el.textContent = formatFollowers(Math.floor(target * eased));
          if (progress < 1) requestAnimationFrame(step);
        }
        requestAnimationFrame(step);
      });
    }

    /* Compartir */
    document.getElementById('shareBtn').addEventListener('click', () => {
      if (navigator.share) {
        navigator.share({ title: 'GameVerse', text: 'Descubre los mejores videojuegos en GameVerse!', url: window.location.href });
      } else {
        navigator.clipboard.writeText(window.location.href);
        toast('Enlace copiado al portapapeles!');
      }
      playSound('success');
      addPoints(10, 'Compartir GameVerse');
    });

    /* =============================================================
       20. GAMIFICACION
       ============================================================= */
    function updateGamification() {
      document.getElementById('gamiPoints').textContent = userPoints;

      let badge, badgeClass, nextBadge, nextPts, progress;
      if (userPoints >= 500) { badge = 'Fan Diamante'; badgeClass = 'diamante'; nextBadge = 'MAX'; nextPts = 500; progress = 100; }
      else if (userPoints >= 200) { badge = 'Fan Oro'; badgeClass = 'oro'; nextBadge = 'Fan Diamante'; nextPts = 500; progress = ((userPoints - 200) / 300) * 100; }
      else if (userPoints >= 100) { badge = 'Fan Plata'; badgeClass = 'plata'; nextBadge = 'Fan Oro'; nextPts = 200; progress = ((userPoints - 100) / 100) * 100; }
      else { badge = 'Fan Bronce'; badgeClass = 'bronce'; nextBadge = 'Fan Plata'; nextPts = 100; progress = (userPoints / 100) * 100; }

      const icons = { bronce: '\u{1F949}', plata: '\u{1F948}', oro: '\u{1F947}', diamante: '\u{1F48E}' };
      document.getElementById('gamiBadge').innerHTML = `<span class="gami-badge ${badgeClass}"><span class="gami-badge-icon">${icons[badgeClass]}</span> ${badge}</span>`;
      document.getElementById('gamiCurrentBadge').textContent = badge;
      document.getElementById('gamiNextBadge').textContent = nextBadge === 'MAX' ? 'Nivel maximo!' : `${nextBadge} (${nextPts} pts)`;
      document.getElementById('gamiProgressFill').style.width = Math.min(progress, 100) + '%';

      /* Historial */
      const histEl = document.getElementById('gamiHistory');
      histEl.innerHTML = pointsHistory.slice(0, 10).map(h =>
        `<div class="gami-history-item"><span>${h.reason}</span><span class="gami-pts-earned">+${h.amount}</span></div>`
      ).join('');
    }

    /* =============================================================
       21. INTERSECTION OBSERVER (animar al entrar en vista)
       ============================================================= */
    let followersAnimated = false;
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting && entry.target.id === 'socialIcons' && !followersAnimated) {
          followersAnimated = true;
          animateFollowers();
        }
      });
    }, { threshold: 0.3 });
    observer.observe(document.getElementById('socialIcons'));

    /* =============================================================
       22. INICIALIZACION
       ============================================================= */
    renderGames();
    updateCartUI();
    updateRecommendations();
    renderMarketplace();
    renderSurveyResults();
    renderCountries();
    renderChampionOptions();
    renderPredictStats();
    renderChampionStats();
    renderAllVotes();
    updateGamification();
  </script>
</body>
</html>
