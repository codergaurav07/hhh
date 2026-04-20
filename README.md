
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <!-- FIX #4: Viewport meta tag for mobile responsiveness -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>EcoTrack – Smart Plastic Waste Management</title>
  <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;600;700&family=Syne:wght@400;600;700;800&display=swap" rel="stylesheet" />
  <style>
    /* =============================================
       CSS VARIABLES & RESET
    ============================================= */
    :root {
      --green-dark:   #0d3320;
      --green-mid:    #1a5c38;
      --green-main:   #2e7d52;
      --green-light:  #4caf7d;
      --green-glow:   #6fcf97;
      --green-pale:   #d4f0e0;
      --accent:       #f0c040;
      --accent2:      #ff7043;
      --bg-dark:      #0a1f14;
      --bg-card:      #112b1c;
      --bg-card2:     #163524;
      --text-main:    #e8f5ee;
      --text-muted:   #7db898;
      --text-dim:     #3d6e52;
      --border:       #1e4d30;
      --shadow:       0 4px 24px rgba(0,0,0,0.45);
      --radius:       14px;
      --radius-sm:    8px;
      --font-head:    'Syne', sans-serif;
      --font-body:    'Space Grotesk', sans-serif;
      --transition:   0.28s cubic-bezier(.4,0,.2,1);
    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    html { scroll-behavior: smooth; }

    body {
      font-family: var(--font-body);
      background: var(--bg-dark);
      color: var(--text-main);
      min-height: 100vh;
      overflow-x: hidden;
    }

    /* =============================================
       AUTH SCREEN
    ============================================= */
    #auth-screen {
      position: fixed;
      inset: 0;
      background: linear-gradient(135deg, var(--bg-dark) 0%, #0d2e1a 60%, #0a1f14 100%);
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 9999;
      padding: 20px;
    }

    #auth-screen::before {
      content: '';
      position: absolute;
      inset: 0;
      background: radial-gradient(ellipse 70% 60% at 50% 30%, rgba(46,125,82,0.18) 0%, transparent 70%);
      pointer-events: none;
    }

    .auth-box {
      background: var(--bg-card);
      border: 1px solid var(--border);
      border-radius: 20px;
      padding: 44px 40px;
      width: 100%;
      max-width: 420px;
      position: relative;
      box-shadow: var(--shadow), 0 0 60px rgba(46,125,82,0.12);
    }

    .auth-logo {
      text-align: center;
      margin-bottom: 28px;
    }

    .auth-logo .leaf-icon {
      font-size: 48px;
      display: block;
      margin-bottom: 8px;
      animation: leafFloat 3s ease-in-out infinite;
    }

    @keyframes leafFloat {
      0%,100% { transform: translateY(0) rotate(-4deg); }
      50%      { transform: translateY(-8px) rotate(4deg); }
    }

    .auth-logo h1 {
      font-family: var(--font-head);
      font-size: 2rem;
      font-weight: 800;
      background: linear-gradient(120deg, var(--green-glow), var(--accent));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .auth-logo p {
      color: var(--text-muted);
      font-size: 0.82rem;
      margin-top: 4px;
    }

    .auth-tabs {
      display: flex;
      background: var(--bg-dark);
      border-radius: var(--radius-sm);
      padding: 4px;
      margin-bottom: 28px;
    }

    .auth-tab {
      flex: 1;
      padding: 9px;
      text-align: center;
      font-size: 0.9rem;
      font-weight: 600;
      cursor: pointer;
      border-radius: 6px;
      color: var(--text-muted);
      transition: var(--transition);
      border: none;
      background: none;
    }

    .auth-tab.active {
      background: var(--green-main);
      color: #fff;
    }

    .auth-form { display: none; flex-direction: column; gap: 14px; }
    .auth-form.active { display: flex; }

    .auth-form input {
      background: var(--bg-dark);
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      padding: 13px 16px;
      color: var(--text-main);
      font-family: var(--font-body);
      font-size: 0.9rem;
      outline: none;
      transition: var(--transition);
    }

    .auth-form input:focus {
      border-color: var(--green-light);
      box-shadow: 0 0 0 3px rgba(76,175,125,0.15);
    }

    .auth-form input::placeholder { color: var(--text-dim); }

    .btn-primary {
      background: linear-gradient(135deg, var(--green-main), var(--green-light));
      color: #fff;
      border: none;
      border-radius: var(--radius-sm);
      padding: 13px 24px;
      font-family: var(--font-body);
      font-size: 0.95rem;
      font-weight: 600;
      cursor: pointer;
      transition: var(--transition);
      letter-spacing: 0.02em;
    }

    .btn-primary:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 24px rgba(46,125,82,0.4);
    }

    .btn-primary:active { transform: translateY(0); }

    .auth-error {
      color: #ff7043;
      font-size: 0.82rem;
      text-align: center;
      min-height: 18px;
    }

    /* =============================================
       MAIN APP LAYOUT
    ============================================= */
    #app {
      display: none;
      flex-direction: column;
      min-height: 100vh;
    }

    /* =============================================
       TOP NAVBAR
    ============================================= */
    .topbar {
      position: fixed;
      top: 0; left: 0; right: 0;
      height: 60px;
      background: rgba(10,31,20,0.95);
      backdrop-filter: blur(12px);
      border-bottom: 1px solid var(--border);
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 20px;
      z-index: 500;
    }

    .topbar-brand {
      font-family: var(--font-head);
      font-weight: 800;
      font-size: 1.2rem;
      background: linear-gradient(90deg, var(--green-glow), var(--accent));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .topbar-right {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .topbar-credits {
      background: var(--bg-card2);
      border: 1px solid var(--border);
      border-radius: 20px;
      padding: 5px 14px;
      font-size: 0.82rem;
      color: var(--accent);
      font-weight: 600;
    }

    .hamburger {
      display: none;
      flex-direction: column;
      gap: 5px;
      cursor: pointer;
      padding: 6px;
      background: none;
      border: none;
    }

    .hamburger span {
      display: block;
      width: 22px;
      height: 2px;
      background: var(--text-main);
      border-radius: 2px;
      transition: var(--transition);
    }

    /* =============================================
       SIDEBAR NAV
    ============================================= */
    .sidebar {
      position: fixed;
      top: 60px;
      left: 0;
      width: 220px;
      height: calc(100vh - 60px);
      background: var(--bg-card);
      border-right: 1px solid var(--border);
      overflow-y: auto;
      z-index: 400;
      padding: 16px 0;
      transition: transform var(--transition);
    }

    .nav-item {
      display: flex;
      align-items: center;
      gap: 12px;
      padding: 12px 20px;
      cursor: pointer;
      color: var(--text-muted);
      font-size: 0.9rem;
      font-weight: 500;
      transition: var(--transition);
      border-left: 3px solid transparent;
      user-select: none;
    }

    .nav-item:hover {
      background: rgba(46,125,82,0.12);
      color: var(--text-main);
    }

    .nav-item.active {
      background: rgba(76,175,125,0.15);
      color: var(--green-glow);
      border-left-color: var(--green-glow);
    }

    .nav-item .nav-icon { font-size: 1.1rem; width: 22px; text-align: center; }
    .nav-item .nav-label { flex: 1; }

    /* =============================================
       MAIN CONTENT AREA
    ============================================= */
    .main-content {
      margin-left: 220px;
      margin-top: 60px;
      min-height: calc(100vh - 60px);
      position: relative;
    }

    /* FIX #1, #2: Section switching — all hidden by default */
    .section {
      display: none;
      padding: 32px 28px;
      animation: fadeIn 0.3s ease;
      min-height: calc(100vh - 60px);
    }

    .section.active { display: block; }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to   { opacity: 1; transform: translateY(0); }
    }

    /* =============================================
       REUSABLE COMPONENTS
    ============================================= */
    .section-title {
      font-family: var(--font-head);
      font-size: 1.6rem;
      font-weight: 700;
      margin-bottom: 6px;
      color: var(--text-main);
    }

    .section-subtitle {
      color: var(--text-muted);
      font-size: 0.88rem;
      margin-bottom: 28px;
    }

    .card {
      background: var(--bg-card);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 24px;
      transition: var(--transition);
    }

    .card:hover { border-color: var(--green-main); }

    .card-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
      gap: 16px;
    }

    .stat-card {
      background: var(--bg-card);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 20px 22px;
      display: flex;
      align-items: center;
      gap: 16px;
    }

    .stat-icon {
      font-size: 2rem;
      width: 52px;
      height: 52px;
      display: flex;
      align-items: center;
      justify-content: center;
      background: rgba(46,125,82,0.18);
      border-radius: 12px;
    }

    .stat-info .value {
      font-family: var(--font-head);
      font-size: 1.6rem;
      font-weight: 700;
      color: var(--green-glow);
    }

    .stat-info .label {
      font-size: 0.78rem;
      color: var(--text-muted);
      margin-top: 2px;
    }

    .badge {
      display: inline-block;
      padding: 3px 10px;
      border-radius: 20px;
      font-size: 0.75rem;
      font-weight: 600;
    }

    .badge-green { background: rgba(76,175,125,0.2); color: var(--green-glow); }
    .badge-yellow { background: rgba(240,192,64,0.2); color: var(--accent); }
    .badge-red { background: rgba(255,112,67,0.2); color: var(--accent2); }

    .eco-tip {
      background: linear-gradient(135deg, rgba(46,125,82,0.2), rgba(76,175,125,0.1));
      border: 1px solid rgba(76,175,125,0.3);
      border-radius: var(--radius);
      padding: 14px 18px;
      font-size: 0.85rem;
      color: var(--green-pale);
      display: flex;
      align-items: flex-start;
      gap: 10px;
      margin-bottom: 20px;
    }

    .btn-secondary {
      background: transparent;
      border: 1px solid var(--green-main);
      color: var(--green-light);
      border-radius: var(--radius-sm);
      padding: 10px 20px;
      font-family: var(--font-body);
      font-size: 0.88rem;
      font-weight: 600;
      cursor: pointer;
      transition: var(--transition);
    }

    .btn-secondary:hover {
      background: rgba(46,125,82,0.15);
    }

    .btn-accent {
      background: linear-gradient(135deg, #f0c040, #f5a623);
      color: #0d1f10;
      border: none;
      border-radius: var(--radius-sm);
      padding: 11px 22px;
      font-family: var(--font-body);
      font-size: 0.9rem;
      font-weight: 700;
      cursor: pointer;
      transition: var(--transition);
    }

    .btn-accent:hover { transform: translateY(-2px); box-shadow: 0 6px 20px rgba(240,192,64,0.35); }

    progress {
      width: 100%;
      height: 8px;
      border-radius: 4px;
      border: none;
      appearance: none;
      background: var(--bg-dark);
      overflow: hidden;
    }

    progress::-webkit-progress-bar { background: var(--bg-dark); border-radius: 4px; }
    progress::-webkit-progress-value { background: linear-gradient(90deg, var(--green-main), var(--green-glow)); border-radius: 4px; }

    /* =============================================
       HOME SECTION
    ============================================= */
    .home-hero {
      background: linear-gradient(135deg, var(--bg-card2) 0%, var(--bg-card) 100%);
      border: 1px solid var(--border);
      border-radius: 20px;
      padding: 40px 36px;
      margin-bottom: 24px;
      position: relative;
      overflow: hidden;
    }

    .home-hero::after {
      content: '♻️';
      position: absolute;
      right: 30px;
      top: 50%;
      transform: translateY(-50%);
      font-size: 5rem;
      opacity: 0.12;
    }

    .home-hero h2 {
      font-family: var(--font-head);
      font-size: clamp(1.4rem, 3vw, 2.2rem);
      font-weight: 800;
      line-height: 1.2;
      margin-bottom: 10px;
    }

    .home-hero h2 span {
      background: linear-gradient(90deg, var(--green-glow), var(--accent));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .home-hero p { color: var(--text-muted); font-size: 0.9rem; margin-bottom: 22px; max-width: 480px; }

    .home-actions { display: flex; gap: 12px; flex-wrap: wrap; }

    .pce-banner {
      background: linear-gradient(135deg, #1a3a28, #0f2a1e);
      border: 1px solid rgba(240,192,64,0.3);
      border-radius: var(--radius);
      padding: 20px 24px;
      margin-bottom: 24px;
      display: flex;
      align-items: center;
      gap: 18px;
    }

    .pce-icon { font-size: 2.2rem; }

    .pce-info h4 {
      font-family: var(--font-head);
      font-size: 1rem;
      font-weight: 700;
      color: var(--accent);
      margin-bottom: 4px;
    }

    .pce-info p { font-size: 0.82rem; color: var(--text-muted); }

    /* =============================================
       DASHBOARD SECTION
    ============================================= */
    .chart-bar-row {
      display: flex;
      align-items: flex-end;
      gap: 6px;
      height: 100px;
      margin-top: 16px;
    }

    .chart-bar {
      flex: 1;
      background: linear-gradient(to top, var(--green-main), var(--green-glow));
      border-radius: 4px 4px 0 0;
      position: relative;
      transition: height 0.6s ease;
      cursor: pointer;
    }

    .chart-bar:hover { opacity: 0.85; }

    .chart-labels {
      display: flex;
      gap: 6px;
      margin-top: 6px;
    }

    .chart-labels span {
      flex: 1;
      text-align: center;
      font-size: 0.7rem;
      color: var(--text-dim);
    }

    .activity-list { display: flex; flex-direction: column; gap: 12px; margin-top: 16px; }

    .activity-item {
      display: flex;
      align-items: center;
      gap: 14px;
      padding: 12px 16px;
      background: var(--bg-dark);
      border-radius: var(--radius-sm);
      border: 1px solid var(--border);
    }

    .activity-item .act-icon { font-size: 1.3rem; }
    .activity-item .act-text { flex: 1; font-size: 0.85rem; }
    .activity-item .act-text strong { display: block; color: var(--text-main); }
    .activity-item .act-text span { color: var(--text-muted); font-size: 0.78rem; }
    .activity-item .act-pts { font-size: 0.82rem; font-weight: 700; color: var(--accent); }

    .dashboard-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 18px;
      margin-top: 20px;
    }

    /* =============================================
       SCAN SECTION
    ============================================= */
    .scan-box {
      max-width: 480px;
      margin: 0 auto;
    }

    .scan-upload-zone {
      border: 2px dashed var(--green-main);
      border-radius: var(--radius);
      padding: 50px 30px;
      text-align: center;
      cursor: pointer;
      transition: var(--transition);
      background: rgba(46,125,82,0.05);
      margin-bottom: 20px;
    }

    .scan-upload-zone:hover {
      border-color: var(--green-glow);
      background: rgba(46,125,82,0.1);
    }

    .scan-upload-zone .upload-icon { font-size: 3rem; margin-bottom: 12px; }
    .scan-upload-zone p { color: var(--text-muted); font-size: 0.88rem; }
    .scan-upload-zone p strong { color: var(--green-glow); }

    #file-input { display: none; }

    .scan-or {
      text-align: center;
      color: var(--text-dim);
      font-size: 0.82rem;
      margin: 12px 0;
      position: relative;
    }

    .scan-or::before, .scan-or::after {
      content: '';
      position: absolute;
      top: 50%;
      width: 40%;
      height: 1px;
      background: var(--border);
    }

    .scan-or::before { left: 0; }
    .scan-or::after  { right: 0; }

    .camera-btn-wrap { text-align: center; margin-bottom: 24px; }

    .scan-result {
      background: var(--bg-card2);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 22px;
      display: none;
    }

    .scan-result.active { display: block; }

    .plastic-type-badge {
      display: inline-block;
      padding: 5px 14px;
      border-radius: 20px;
      font-size: 0.78rem;
      font-weight: 700;
      margin-bottom: 12px;
    }

    .plastic-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 8px 0;
      border-bottom: 1px solid var(--border);
      font-size: 0.85rem;
    }

    .plastic-row:last-child { border-bottom: none; }
    .plastic-row .k { color: var(--text-muted); }
    .plastic-row .v { font-weight: 600; }

    /* =============================================
       CAMERA MODAL — FIX #5: Full screen
    ============================================= */
    #camera-modal {
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,0.96);
      z-index: 9998;
      display: none;
      flex-direction: column;
      align-items: center;
      justify-content: center;
    }

    #camera-modal.open { display: flex; }

    #camera-video {
      width: 100%;
      height: 100%;
      object-fit: cover;
      position: absolute;
      inset: 0;
    }

    .camera-overlay {
      position: relative;
      z-index: 2;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: space-between;
      height: 100%;
      width: 100%;
      padding: 40px 20px 50px;
    }

    .camera-header {
      text-align: center;
      color: #fff;
    }

    .camera-header h3 { font-family: var(--font-head); font-size: 1.2rem; font-weight: 700; }
    .camera-header p  { font-size: 0.8rem; opacity: 0.7; margin-top: 4px; }

    .camera-frame {
      width: min(320px, 80vw);
      height: min(320px, 80vw);
      border: 2px solid rgba(76,175,125,0.7);
      border-radius: 16px;
      box-shadow: 0 0 0 2000px rgba(0,0,0,0.4);
      position: relative;
    }

    .camera-frame::before, .camera-frame::after,
    .camera-frame .corner-br, .camera-frame .corner-bl {
      content: '';
      position: absolute;
      width: 24px;
      height: 24px;
      border-color: var(--green-glow);
      border-style: solid;
    }

    .camera-frame::before  { top: -2px; left: -2px; border-width: 3px 0 0 3px; border-radius: 4px 0 0 0; }
    .camera-frame::after   { top: -2px; right: -2px; border-width: 3px 3px 0 0; border-radius: 0 4px 0 0; }
    .camera-frame .corner-br { bottom: -2px; right: -2px; border-width: 0 3px 3px 0; border-radius: 0 0 4px 0; }
    .camera-frame .corner-bl { bottom: -2px; left: -2px; border-width: 0 0 3px 3px; border-radius: 0 0 0 4px; }

    .camera-scan-line {
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 2px;
      background: linear-gradient(90deg, transparent, var(--green-glow), transparent);
      animation: scanLine 2s ease-in-out infinite;
    }

    @keyframes scanLine {
      0%   { top: 0; opacity: 1; }
      100% { top: 100%; opacity: 0.3; }
    }

    .camera-footer {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 16px;
    }

    .btn-capture {
      width: 68px;
      height: 68px;
      border-radius: 50%;
      background: #fff;
      border: 4px solid var(--green-glow);
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 1.6rem;
      transition: var(--transition);
    }

    .btn-capture:hover { transform: scale(1.06); }

    .btn-close-camera {
      background: rgba(255,255,255,0.12);
      border: 1px solid rgba(255,255,255,0.25);
      color: #fff;
      border-radius: 20px;
      padding: 9px 24px;
      font-family: var(--font-body);
      font-size: 0.88rem;
      font-weight: 600;
      cursor: pointer;
      transition: var(--transition);
      backdrop-filter: blur(6px);
    }

    .btn-close-camera:hover { background: rgba(255,255,255,0.2); }

    /* =============================================
       REWARDS SECTION
    ============================================= */
    .rewards-balance {
      background: linear-gradient(135deg, #1c3a28, #0f2a1e);
      border: 1px solid rgba(240,192,64,0.35);
      border-radius: 18px;
      padding: 28px;
      text-align: center;
      margin-bottom: 24px;
    }

    .rewards-balance .bal-label { font-size: 0.82rem; color: var(--text-muted); margin-bottom: 6px; }
    .rewards-balance .bal-value {
      font-family: var(--font-head);
      font-size: 3rem;
      font-weight: 800;
      color: var(--accent);
    }
    .rewards-balance .bal-sub { font-size: 0.78rem; color: var(--text-muted); margin-top: 4px; }

    .rewards-list { display: flex; flex-direction: column; gap: 14px; }

    .reward-item {
      background: var(--bg-card);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 18px 20px;
      display: flex;
      align-items: center;
      gap: 16px;
      transition: var(--transition);
    }

    .reward-item:hover { border-color: var(--green-main); transform: translateX(3px); }
    .reward-item .r-icon { font-size: 2rem; }
    .reward-item .r-info { flex: 1; }
    .reward-item .r-info h4 { font-size: 0.92rem; font-weight: 600; margin-bottom: 3px; }
    .reward-item .r-info p { font-size: 0.78rem; color: var(--text-muted); }
    .reward-item .r-pts { font-family: var(--font-head); font-size: 1.1rem; font-weight: 700; color: var(--accent); }

    /* =============================================
       MAP SECTION
    ============================================= */
    .map-container {
      background: var(--bg-card2);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      overflow: hidden;
      margin-bottom: 20px;
    }

    .map-placeholder {
      height: 320px;
      background: linear-gradient(145deg, #0d2418, #0a1f14);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      position: relative;
    }

    .map-dots {
      position: absolute;
      inset: 0;
    }

    .map-dot {
      position: absolute;
      width: 12px;
      height: 12px;
      border-radius: 50%;
      border: 2px solid var(--green-glow);
      background: rgba(76,175,125,0.3);
    }

    .map-dot::after {
      content: '';
      position: absolute;
      inset: -4px;
      border-radius: 50%;
      border: 1px solid rgba(76,175,125,0.3);
      animation: ping 2s ease-out infinite;
    }

    @keyframes ping {
      0%   { transform: scale(1); opacity: 0.7; }
      100% { transform: scale(2.5); opacity: 0; }
    }

    .map-center-text { color: var(--text-muted); font-size: 0.85rem; text-align: center; z-index: 1; }

    .bin-list { display: flex; flex-direction: column; gap: 12px; }

    .bin-row {
      background: var(--bg-card);
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      padding: 14px 18px;
      display: flex;
      align-items: center;
      gap: 14px;
    }

    .bin-row .bin-ico { font-size: 1.4rem; }
    .bin-row .bin-info { flex: 1; }
    .bin-row .bin-info h5 { font-size: 0.88rem; font-weight: 600; margin-bottom: 3px; }
    .bin-row .bin-info small { font-size: 0.76rem; color: var(--text-muted); }
    .bin-row .bin-dist { font-size: 0.8rem; color: var(--text-muted); }

    /* =============================================
       SMART DUSTBIN SECTION
    ============================================= */
    .dustbin-visual {
      display: flex;
      justify-content: center;
      margin: 20px 0 30px;
    }

    .dustbin-svg-wrap {
      width: 140px;
      text-align: center;
    }

    .dustbin-body {
      width: 100px;
      height: 130px;
      margin: 0 auto;
      border-radius: 0 0 16px 16px;
      background: var(--bg-card2);
      border: 2px solid var(--border);
      position: relative;
      overflow: hidden;
    }

    .dustbin-fill {
      position: absolute;
      bottom: 0; left: 0; right: 0;
      background: linear-gradient(to top, var(--green-main), var(--green-glow));
      border-radius: 0 0 14px 14px;
      transition: height 1s ease;
    }

    .dustbin-lid {
      width: 110px;
      height: 18px;
      background: var(--bg-card);
      border: 2px solid var(--border);
      border-radius: 8px;
      margin: 0 auto 2px;
    }

    .dustbin-pct {
      font-family: var(--font-head);
      font-size: 1.4rem;
      font-weight: 700;
      color: var(--green-glow);
      margin-top: 10px;
    }

    .bin-metrics { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; margin-top: 20px; }

    .bin-metric {
      background: var(--bg-card);
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      padding: 16px;
      text-align: center;
    }

    .bin-metric .bm-val { font-family: var(--font-head); font-size: 1.4rem; font-weight: 700; color: var(--green-glow); }
    .bin-metric .bm-lbl { font-size: 0.76rem; color: var(--text-muted); margin-top: 3px; }

    .dustbin-alerts { margin-top: 20px; display: flex; flex-direction: column; gap: 10px; }

    .alert-row {
      padding: 12px 16px;
      border-radius: var(--radius-sm);
      font-size: 0.83rem;
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .alert-warn { background: rgba(240,192,64,0.12); border: 1px solid rgba(240,192,64,0.3); color: var(--accent); }
    .alert-ok   { background: rgba(76,175,125,0.12); border: 1px solid rgba(76,175,125,0.3); color: var(--green-glow); }

    /* =============================================
       LEADERBOARD SECTION
    ============================================= */
    .podium {
      display: flex;
      justify-content: center;
      align-items: flex-end;
      gap: 12px;
      margin-bottom: 28px;
    }

    .podium-slot {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 8px;
    }

    .podium-avatar {
      width: 48px;
      height: 48px;
      border-radius: 50%;
      background: var(--bg-card2);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 1.4rem;
      border: 2px solid var(--border);
    }

    .podium-slot:nth-child(1) .podium-avatar { border-color: #c0c0c0; width: 44px; height: 44px; }
    .podium-slot:nth-child(2) .podium-avatar { border-color: var(--accent); width: 56px; height: 56px; }
    .podium-slot:nth-child(3) .podium-avatar { border-color: #cd7f32; width: 44px; height: 44px; }

    .podium-name { font-size: 0.78rem; color: var(--text-muted); }

    .podium-block {
      border-radius: 6px 6px 0 0;
      display: flex;
      align-items: center;
      justify-content: center;
      font-family: var(--font-head);
      font-weight: 700;
      font-size: 1rem;
    }

    .podium-slot:nth-child(1) .podium-block { width: 70px; height: 70px; background: #2d2d3a; color: #c0c0c0; }
    .podium-slot:nth-child(2) .podium-block { width: 80px; height: 90px; background: linear-gradient(to top, #3a2e0d, #5a4500); color: var(--accent); }
    .podium-slot:nth-child(3) .podium-block { width: 70px; height: 50px; background: #2d1e0d; color: #cd7f32; }

    .lb-table { width: 100%; border-collapse: collapse; }
    .lb-table th { font-size: 0.75rem; color: var(--text-muted); text-align: left; padding: 8px 12px; border-bottom: 1px solid var(--border); }
    .lb-table td { padding: 12px; font-size: 0.85rem; border-bottom: 1px solid rgba(30,77,48,0.4); }
    .lb-table tr:hover td { background: rgba(46,125,82,0.06); }
    .lb-table .rank { font-family: var(--font-head); font-weight: 700; color: var(--text-muted); }
    .lb-table .pts  { font-weight: 700; color: var(--accent); }
    .lb-table .me td { background: rgba(76,175,125,0.08); color: var(--green-glow); }

    /* =============================================
       PROFILE SECTION
    ============================================= */
    .profile-header {
      display: flex;
      align-items: center;
      gap: 20px;
      padding: 28px;
      background: var(--bg-card);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      margin-bottom: 20px;
    }

    .profile-avatar {
      width: 72px;
      height: 72px;
      border-radius: 50%;
      background: linear-gradient(135deg, var(--green-main), var(--green-glow));
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 2.2rem;
      border: 3px solid var(--green-glow);
      flex-shrink: 0;
    }

    .profile-meta h3 { font-family: var(--font-head); font-size: 1.2rem; font-weight: 700; margin-bottom: 4px; }
    .profile-meta p { font-size: 0.82rem; color: var(--text-muted); }

    .profile-badges {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-top: 8px;
    }

    .profile-badge {
      background: rgba(46,125,82,0.15);
      border: 1px solid var(--green-main);
      border-radius: 6px;
      padding: 5px 12px;
      font-size: 0.75rem;
      color: var(--green-glow);
    }

    .profile-stats { display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; margin-bottom: 20px; }

    .pstat {
      background: var(--bg-card);
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      padding: 16px;
      text-align: center;
    }

    .pstat .val { font-family: var(--font-head); font-size: 1.4rem; font-weight: 700; color: var(--green-glow); }
    .pstat .lbl { font-size: 0.72rem; color: var(--text-muted); margin-top: 3px; }

    .profile-settings { display: flex; flex-direction: column; gap: 12px; }

    .pref-row {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 14px 18px;
      background: var(--bg-card);
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      font-size: 0.88rem;
    }

    .toggle {
      width: 42px;
      height: 24px;
      background: var(--green-main);
      border-radius: 12px;
      position: relative;
      cursor: pointer;
    }

    .toggle::after {
      content: '';
      position: absolute;
      top: 3px; right: 3px;
      width: 18px; height: 18px;
      background: #fff;
      border-radius: 50%;
      transition: var(--transition);
    }

    .toggle.off { background: var(--bg-dark); }
    .toggle.off::after { right: auto; left: 3px; }

    /* =============================================
       CONTACT SECTION
    ============================================= */
    .contact-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; }

    .contact-form { display: flex; flex-direction: column; gap: 14px; }

    .contact-form input,
    .contact-form textarea {
      background: var(--bg-dark);
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      padding: 12px 16px;
      color: var(--text-main);
      font-family: var(--font-body);
      font-size: 0.88rem;
      outline: none;
      transition: var(--transition);
      resize: vertical;
    }

    .contact-form input:focus,
    .contact-form textarea:focus {
      border-color: var(--green-light);
    }

    .contact-info { display: flex; flex-direction: column; gap: 14px; }

    .contact-info-item {
      background: var(--bg-card);
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      padding: 16px 18px;
      display: flex;
      align-items: center;
      gap: 14px;
    }

    .contact-info-item .ci-icon { font-size: 1.4rem; }
    .contact-info-item .ci-text h5 { font-size: 0.88rem; font-weight: 600; margin-bottom: 3px; }
    .contact-info-item .ci-text p { font-size: 0.78rem; color: var(--text-muted); }

    .msg-sent {
      display: none;
      text-align: center;
      padding: 20px;
      color: var(--green-glow);
      font-size: 0.9rem;
    }

    /* =============================================
       MOBILE RESPONSIVENESS — FIX #4
    ============================================= */
    @media (max-width: 768px) {
      .hamburger { display: flex; }

      .sidebar {
        transform: translateX(-100%);
        width: 240px;
        z-index: 600;
      }

      .sidebar.open { transform: translateX(0); }

      .sidebar-overlay {
        display: none;
        position: fixed;
        inset: 0;
        background: rgba(0,0,0,0.5);
        z-index: 550;
      }

      .sidebar-overlay.open { display: block; }

      .main-content { margin-left: 0; }

      .section { padding: 20px 16px; }

      .card-grid { grid-template-columns: 1fr; }

      .dashboard-grid { grid-template-columns: 1fr; }

      .contact-grid { grid-template-columns: 1fr; }

      .profile-stats { grid-template-columns: 1fr 1fr; }

      .home-hero { padding: 28px 20px; }
      .home-hero::after { display: none; }

      .pce-banner { flex-direction: column; text-align: center; }

      .podium { gap: 8px; }

      .auth-box { padding: 32px 24px; }
    }

    @media (max-width: 480px) {
      .profile-stats { grid-template-columns: 1fr; }
      .bin-metrics   { grid-template-columns: 1fr; }
      .profile-header { flex-direction: column; text-align: center; }
      .profile-badges { justify-content: center; }
      .rewards-balance .bal-value { font-size: 2.4rem; }
    }

    /* =============================================
       SCROLLBAR STYLING
    ============================================= */
    ::-webkit-scrollbar { width: 6px; }
    ::-webkit-scrollbar-track { background: var(--bg-dark); }
    ::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }
    ::-webkit-scrollbar-thumb:hover { background: var(--green-main); }
  </style>
</head>
<body>

<!-- ============================================================
     AUTH SCREEN — LOGIN / SIGNUP
============================================================ -->
<div id="auth-screen">
  <div class="auth-box">
    <div class="auth-logo">
      <span class="leaf-icon">🌿</span>
      <h1>EcoTrack</h1>
      <p>Smart Plastic Waste Management System</p>
    </div>
    <div class="auth-tabs">
      <button class="auth-tab active" onclick="switchAuthTab('login')">Login</button>
      <button class="auth-tab" onclick="switchAuthTab('signup')">Sign Up</button>
    </div>

    <!-- LOGIN FORM -->
    <div id="login-form" class="auth-form active">
      <input type="email" id="login-email" placeholder="Email address" />
      <input type="password" id="login-pass" placeholder="Password" />
      <button class="btn-primary" onclick="handleLogin()">Login to EcoTrack</button>
      <div id="login-error" class="auth-error"></div>
    </div>

    <!-- SIGNUP FORM -->
    <div id="signup-form" class="auth-form">
      <input type="text"     id="su-name"  placeholder="Full name" />
      <input type="email"    id="su-email" placeholder="Email address" />
      <input type="password" id="su-pass"  placeholder="Password (min 6 chars)" />
      <button class="btn-primary" onclick="handleSignup()">Create Account</button>
      <div id="signup-error" class="auth-error"></div>
    </div>
  </div>
</div>

<!-- ============================================================
     MAIN APPLICATION
============================================================ -->
<div id="app">

  <!-- TOPBAR -->
  <div class="topbar">
    <div style="display:flex;align-items:center;gap:12px;">
      <button class="hamburger" id="hamburger-btn" onclick="toggleSidebar()" aria-label="Toggle menu">
        <span></span><span></span><span></span>
      </button>
      <span class="topbar-brand">🌿 EcoTrack</span>
    </div>
    <div class="topbar-right">
      <div class="topbar-credits">🪙 <span id="top-credits">1,240</span> Credits</div>
    </div>
  </div>

  <!-- SIDEBAR OVERLAY (mobile) -->
  <div class="sidebar-overlay" id="sidebar-overlay" onclick="toggleSidebar()"></div>

  <!-- SIDEBAR NAVIGATION -->
  <nav class="sidebar" id="sidebar">
    <div class="nav-item active"  onclick="showSection('home')">       <span class="nav-icon">🏠</span><span class="nav-label">Home</span></div>
    <div class="nav-item"         onclick="showSection('dashboard')">  <span class="nav-icon">📊</span><span class="nav-label">Dashboard</span></div>
    <div class="nav-item"         onclick="showSection('scan')">       <span class="nav-icon">🔍</span><span class="nav-label">Scan</span></div>
    <div class="nav-item"         onclick="showSection('rewards')">    <span class="nav-icon">🎁</span><span class="nav-label">Rewards</span></div>
    <div class="nav-item"         onclick="showSection('map')">        <span class="nav-icon">🗺️</span><span class="nav-label">Map</span></div>
    <div class="nav-item"         onclick="showSection('dustbin')">    <span class="nav-icon">🗑️</span><span class="nav-label">Smart Dustbin</span></div>
    <div class="nav-item"         onclick="showSection('leaderboard')"><span class="nav-icon">🏆</span><span class="nav-label">Leaderboard</span></div>
    <div class="nav-item"         onclick="showSection('profile')">    <span class="nav-icon">👤</span><span class="nav-label">Profile</span></div>
    <div class="nav-item"         onclick="showSection('contact')">    <span class="nav-icon">✉️</span><span class="nav-label">Contact</span></div>
    <div style="margin-top:auto;padding:16px 20px;border-top:1px solid var(--border);">
      <button class="btn-secondary" style="width:100%;font-size:0.82rem;" onclick="handleLogout()">🚪 Logout</button>
    </div>
  </nav>

  <!-- MAIN CONTENT AREA -->
  <main class="main-content">

    <!-- ==================== HOME ==================== -->
    <section id="section-home" class="section active">
      <div class="home-hero">
        <h2>Track. Reduce. <span>Earn Credits.</span></h2>
        <p>Join the Plastic Credit Economy — scan plastic waste, earn green credits, and help build a sustainable future for our planet.</p>
        <div class="home-actions">
          <button class="btn-primary" onclick="showSection('scan')">🔍 Scan Plastic</button>
          <button class="btn-secondary" onclick="showSection('dashboard')">📊 View Dashboard</button>
        </div>
      </div>

      <div class="pce-banner">
        <div class="pce-icon">💰</div>
        <div class="pce-info">
          <h4>Plastic Credit Economy (PCE)</h4>
          <p>Every piece of plastic you properly dispose of earns you credits that can be redeemed for real rewards. Help the planet and get rewarded.</p>
        </div>
      </div>

      <div class="eco-tip">💡 <span><strong>Eco Tip:</strong> Rinsing plastic containers before disposal increases their recycling value by up to 30%. Small actions, big impact!</span></div>

      <div class="card-grid">
        <div class="stat-card">
          <div class="stat-icon">♻️</div>
          <div class="stat-info">
            <div class="value">2.4 kg</div>
            <div class="label">Plastic Recycled This Week</div>
          </div>
        </div>
        <div class="stat-card">
          <div class="stat-icon">🪙</div>
          <div class="stat-info">
            <div class="value">1,240</div>
            <div class="label">Total Credits Earned</div>
          </div>
        </div>
        <div class="stat-card">
          <div class="stat-icon">🌳</div>
          <div class="stat-info">
            <div class="value">12</div>
            <div class="label">Trees Equivalent Saved</div>
          </div>
        </div>
        <div class="stat-card">
          <div class="stat-icon">🏅</div>
          <div class="stat-info">
            <div class="value">#8</div>
            <div class="label">Community Rank</div>
          </div>
        </div>
      </div>
    </section>

    <!-- ==================== DASHBOARD ==================== -->
    <section id="section-dashboard" class="section">
      <h2 class="section-title">📊 Dashboard</h2>
      <p class="section-subtitle">Your recycling activity at a glance</p>

      <div class="card-grid" style="margin-bottom:20px;">
        <div class="stat-card">
          <div class="stat-icon">📦</div>
          <div class="stat-info">
            <div class="value">47</div>
            <div class="label">Items Scanned Total</div>
          </div>
        </div>
        <div class="stat-card">
          <div class="stat-icon">💧</div>
          <div class="stat-info">
            <div class="value">8.1 kg</div>
            <div class="label">Total Plastic Recycled</div>
          </div>
        </div>
        <div class="stat-card">
          <div class="stat-icon">🌍</div>
          <div class="stat-info">
            <div class="value">4.2 kg</div>
            <div class="label">CO₂ Emissions Prevented</div>
          </div>
        </div>
        <div class="stat-card">
          <div class="stat-icon">🔥</div>
          <div class="stat-info">
            <div class="value">14</div>
            <div class="label">Day Streak</div>
          </div>
        </div>
      </div>

      <div class="dashboard-grid">
        <div class="card">
          <h4 style="font-size:0.9rem;font-weight:600;margin-bottom:4px;">Weekly Activity</h4>
          <p style="font-size:0.76rem;color:var(--text-muted);">Items scanned per day</p>
          <div class="chart-bar-row">
            <div class="chart-bar" style="height:35%;" title="Mon: 2"></div>
            <div class="chart-bar" style="height:55%;" title="Tue: 4"></div>
            <div class="chart-bar" style="height:40%;" title="Wed: 3"></div>
            <div class="chart-bar" style="height:80%;" title="Thu: 7"></div>
            <div class="chart-bar" style="height:60%;" title="Fri: 5"></div>
            <div class="chart-bar" style="height:90%;" title="Sat: 8"></div>
            <div class="chart-bar" style="height:50%;" title="Sun: 4"></div>
          </div>
          <div class="chart-labels">
            <span>M</span><span>T</span><span>W</span><span>T</span><span>F</span><span>S</span><span>S</span>
          </div>
        </div>

        <div class="card">
          <h4 style="font-size:0.9rem;font-weight:600;margin-bottom:14px;">Plastic Types Breakdown</h4>
          <div style="display:flex;flex-direction:column;gap:10px;">
            <div>
              <div style="display:flex;justify-content:space-between;font-size:0.78rem;margin-bottom:4px;"><span>PET (#1)</span><span style="color:var(--green-glow);">38%</span></div>
              <progress value="38" max="100"></progress>
            </div>
            <div>
              <div style="display:flex;justify-content:space-between;font-size:0.78rem;margin-bottom:4px;"><span>HDPE (#2)</span><span style="color:var(--green-glow);">24%</span></div>
              <progress value="24" max="100"></progress>
            </div>
            <div>
              <div style="display:flex;justify-content:space-between;font-size:0.78rem;margin-bottom:4px;"><span>PVC (#3)</span><span style="color:var(--green-glow);">18%</span></div>
              <progress value="18" max="100"></progress>
            </div>
            <div>
              <div style="display:flex;justify-content:space-between;font-size:0.78rem;margin-bottom:4px;"><span>LDPE (#4)</span><span style="color:var(--accent);">12%</span></div>
              <progress value="12" max="100"></progress>
            </div>
            <div>
              <div style="display:flex;justify-content:space-between;font-size:0.78rem;margin-bottom:4px;"><span>Other</span><span style="color:var(--text-muted);">8%</span></div>
              <progress value="8" max="100"></progress>
            </div>
          </div>
        </div>
      </div>

      <div class="card" style="margin-top:18px;">
        <h4 style="font-size:0.9rem;font-weight:600;margin-bottom:14px;">Recent Activity</h4>
        <div class="activity-list">
          <div class="activity-item">
            <div class="act-icon">🧴</div>
            <div class="act-text">
              <strong>Shampoo Bottle (HDPE #2)</strong>
              <span>Today, 11:32 AM · 0.18 kg</span>
            </div>
            <span class="act-pts">+25 pts</span>
          </div>
          <div class="activity-item">
            <div class="act-icon">🥤</div>
            <div class="act-text">
              <strong>Water Bottle (PET #1)</strong>
              <span>Today, 9:14 AM · 0.05 kg</span>
            </div>
            <span class="act-pts">+10 pts</span>
          </div>
          <div class="activity-item">
            <div class="act-icon">🛍️</div>
            <div class="act-text">
              <strong>Plastic Bag (LDPE #4)</strong>
              <span>Yesterday, 6:45 PM · 0.03 kg</span>
            </div>
            <span class="act-pts">+5 pts</span>
          </div>
          <div class="activity-item">
            <div class="act-icon">📦</div>
            <div class="act-text">
              <strong>Food Container (PP #5)</strong>
              <span>Yesterday, 2:10 PM · 0.12 kg</span>
            </div>
            <span class="act-pts">+18 pts</span>
          </div>
        </div>
      </div>
    </section>

    <!-- ==================== SCAN ==================== -->
    <section id="section-scan" class="section">
      <h2 class="section-title">🔍 Scan Plastic</h2>
      <p class="section-subtitle">Identify plastic type and earn credits</p>

      <div class="eco-tip">♻️ <span>Scanning helps identify plastic resin code — this determines correct disposal and your credit value.</span></div>

      <div class="scan-box">
        <!-- Upload Zone -->
        <div class="scan-upload-zone" onclick="document.getElementById('file-input').click()">
          <div class="upload-icon">📸</div>
          <p><strong>Tap to upload image</strong></p>
          <p>Supports JPG, PNG — max 10MB</p>
        </div>
        <input type="file" id="file-input" accept="image/*" onchange="handleFileUpload(event)" />

        <div class="scan-or">OR</div>

        <!-- Camera Button -->
        <div class="camera-btn-wrap">
          <button class="btn-primary" onclick="openCamera()" style="font-size:1rem;padding:14px 32px;">
            📷 Open Camera
          </button>
        </div>

        <!-- Scan Result -->
        <div class="scan-result" id="scan-result">
          <span class="plastic-type-badge badge-green" id="result-badge">PET #1 – Recyclable ✅</span>
          <div class="plastic-row"><span class="k">Plastic Type</span><span class="v" id="res-type">PET (Polyethylene Terephthalate)</span></div>
          <div class="plastic-row"><span class="k">Resin Code</span><span class="v" id="res-code">#1</span></div>
          <div class="plastic-row"><span class="k">Recyclability</span><span class="v" id="res-rec">High ✅</span></div>
          <div class="plastic-row"><span class="k">Estimated Weight</span><span class="v" id="res-wt">~0.08 kg</span></div>
          <div class="plastic-row"><span class="k">Credits Earned</span><span class="v" id="res-cred" style="color:var(--accent);">+15 🪙</span></div>
          <button class="btn-primary" style="margin-top:18px;width:100%;" onclick="confirmDeposit()">✅ Confirm & Deposit</button>
        </div>
      </div>
    </section>

    <!-- ==================== REWARDS ==================== -->
    <section id="section-rewards" class="section">
      <h2 class="section-title">🎁 Rewards</h2>
      <p class="section-subtitle">Redeem your Plastic Credits for real rewards</p>

      <div class="rewards-balance">
        <div class="bal-label">Available Plastic Credits</div>
        <div class="bal-value" id="credits-display">1,240</div>
        <div class="bal-sub">≈ ₹248 worth of rewards available</div>
      </div>

      <h4 style="font-size:0.88rem;color:var(--text-muted);margin-bottom:14px;font-weight:500;">AVAILABLE REWARDS</h4>
      <div class="rewards-list">
        <div class="reward-item">
          <div class="r-icon">🛒</div>
          <div class="r-info">
            <h4>₹50 Shopping Voucher</h4>
            <p>Redeemable at partnered eco-stores</p>
          </div>
          <div style="text-align:right;">
            <div class="r-pts">250 pts</div>
            <button class="btn-secondary" style="margin-top:6px;font-size:0.75rem;padding:6px 14px;" onclick="redeemReward(250,'₹50 Shopping Voucher')">Redeem</button>
          </div>
        </div>
        <div class="reward-item">
          <div class="r-icon">🌱</div>
          <div class="r-info">
            <h4>Plant a Tree</h4>
            <p>We'll plant a tree on your behalf</p>
          </div>
          <div style="text-align:right;">
            <div class="r-pts">400 pts</div>
            <button class="btn-secondary" style="margin-top:6px;font-size:0.75rem;padding:6px 14px;" onclick="redeemReward(400,'Plant a Tree')">Redeem</button>
          </div>
        </div>
        <div class="reward-item">
          <div class="r-icon">⚡</div>
          <div class="r-info">
            <h4>Mobile Recharge ₹100</h4>
            <p>Valid on all major operators</p>
          </div>
          <div style="text-align:right;">
            <div class="r-pts">500 pts</div>
            <button class="btn-secondary" style="margin-top:6px;font-size:0.75rem;padding:6px 14px;" onclick="redeemReward(500,'Mobile Recharge ₹100')">Redeem</button>
          </div>
        </div>
        <div class="reward-item">
          <div class="r-icon">🎫</div>
          <div class="r-info">
            <h4>Movie Ticket</h4>
            <p>1 ticket at BookMyShow partner cinemas</p>
          </div>
          <div style="text-align:right;">
            <div class="r-pts">800 pts</div>
            <button class="btn-secondary" style="margin-top:6px;font-size:0.75rem;padding:6px 14px;" onclick="redeemReward(800,'Movie Ticket')">Redeem</button>
          </div>
        </div>
        <div class="reward-item">
          <div class="r-icon">☕</div>
          <div class="r-info">
            <h4>Free Coffee</h4>
            <p>At eco-certified café partners</p>
          </div>
          <div style="text-align:right;">
            <div class="r-pts">150 pts</div>
            <button class="btn-secondary" style="margin-top:6px;font-size:0.75rem;padding:6px 14px;" onclick="redeemReward(150,'Free Coffee')">Redeem</button>
          </div>
        </div>
      </div>
    </section>

    <!-- ==================== MAP ==================== -->
    <section id="section-map" class="section">
      <h2 class="section-title">🗺️ Nearby Collection Points</h2>
      <p class="section-subtitle">Find smart dustbins and recycling centers near you</p>

      <div class="map-container">
        <div class="map-placeholder">
          <div class="map-dots">
            <!-- Simulated location dots -->
            <div class="map-dot" style="top:28%;left:40%;"></div>
            <div class="map-dot" style="top:55%;left:62%;animation-delay:0.5s;"></div>
            <div class="map-dot" style="top:42%;left:25%;animation-delay:1s;"></div>
            <div class="map-dot" style="top:68%;left:48%;animation-delay:0.3s;"></div>
            <div class="map-dot" style="top:20%;left:70%;animation-delay:0.8s;"></div>
          </div>
          <div class="map-center-text">
            <div style="font-size:2rem;margin-bottom:8px;">📍</div>
            <p>Live map integration ready</p>
            <p style="font-size:0.75rem;margin-top:4px;opacity:0.6;">Enable location to see nearby bins</p>
          </div>
        </div>
      </div>

      <div style="display:flex;gap:10px;margin-bottom:18px;flex-wrap:wrap;">
        <button class="btn-secondary" style="font-size:0.82rem;" onclick="alert('📍 Locating nearby bins...')">📍 Use My Location</button>
        <button class="btn-secondary" style="font-size:0.82rem;" onclick="alert('Filtering by type...')">♻️ Recycling Centers</button>
        <button class="btn-secondary" style="font-size:0.82rem;" onclick="alert('Filtering smart bins...')">🗑️ Smart Dustbins</button>
      </div>

      <h4 style="font-size:0.88rem;color:var(--text-muted);margin-bottom:12px;font-weight:500;">NEAREST LOCATIONS</h4>
      <div class="bin-list">
        <div class="bin-row">
          <div class="bin-ico">🗑️</div>
          <div class="bin-info">
            <h5>Smart Bin – Civil Lines</h5>
            <small>Active · 32% Full · Accepts: PET, HDPE</small>
          </div>
          <div class="bin-dist">0.4 km →</div>
        </div>
        <div class="bin-row">
          <div class="bin-ico">♻️</div>
          <div class="bin-info">
            <h5>EcoCollect Center – Malviya Nagar</h5>
            <small>Open · 8:00 AM – 8:00 PM · All types</small>
          </div>
          <div class="bin-dist">1.2 km →</div>
        </div>
        <div class="bin-row">
          <div class="bin-ico">🗑️</div>
          <div class="bin-info">
            <h5>Smart Bin – C-Scheme</h5>
            <small>Active · 78% Full · Accepts: PET only</small>
          </div>
          <div class="bin-dist">1.8 km →</div>
        </div>
        <div class="bin-row">
          <div class="bin-ico">🏭</div>
          <div class="bin-info">
            <h5>Recycling Facility – Sitapura</h5>
            <small>Mon–Sat · Industrial plastic accepted</small>
          </div>
          <div class="bin-dist">5.3 km →</div>
        </div>
      </div>
    </section>

    <!-- ==================== SMART DUSTBIN ==================== -->
    <section id="section-dustbin" class="section">
      <h2 class="section-title">🗑️ Smart Dustbin</h2>
      <p class="section-subtitle">Real-time monitoring of your registered smart bin</p>

      <div class="card" style="text-align:center;">
        <h4 style="font-size:0.88rem;color:var(--text-muted);margin-bottom:4px;">BIN ID: ECO-JA-0042</h4>
        <div style="display:inline-block;padding:4px 12px;background:rgba(76,175,125,0.15);border-radius:20px;font-size:0.75rem;color:var(--green-glow);margin-bottom:16px;">● Online &amp; Active</div>

        <div class="dustbin-visual">
          <div class="dustbin-svg-wrap">
            <div class="dustbin-lid"></div>
            <div class="dustbin-body">
              <div class="dustbin-fill" id="bin-fill-bar" style="height:68%;"></div>
            </div>
            <div class="dustbin-pct" id="bin-fill-pct">68%</div>
            <div style="font-size:0.72rem;color:var(--text-muted);margin-top:3px;">Capacity Used</div>
          </div>
        </div>

        <div class="bin-metrics">
          <div class="bin-metric">
            <div class="bm-val">2.1 kg</div>
            <div class="bm-lbl">Current Load</div>
          </div>
          <div class="bin-metric">
            <div class="bm-val">34°C</div>
            <div class="bm-lbl">Temperature</div>
          </div>
          <div class="bin-metric">
            <div class="bm-val">92%</div>
            <div class="bm-lbl">Battery</div>
          </div>
          <div class="bin-metric">
            <div class="bm-val">4G</div>
            <div class="bm-lbl">Signal</div>
          </div>
        </div>

        <div class="dustbin-alerts">
          <div class="alert-row alert-warn">⚠️ Bin is 68% full. Schedule a pickup soon.</div>
          <div class="alert-row alert-ok">✅ Sorting sensor working normally</div>
          <div class="alert-row alert-ok">✅ Last emptied: 2 days ago</div>
        </div>

        <div style="margin-top:20px;display:flex;gap:12px;justify-content:center;flex-wrap:wrap;">
          <button class="btn-primary" onclick="alert('📬 Pickup scheduled! Estimated arrival: 2 hours.')">📬 Schedule Pickup</button>
          <button class="btn-secondary" onclick="alert('📊 Viewing full bin history...')">📊 View History</button>
        </div>
      </div>
    </section>

    <!-- ==================== LEADERBOARD ==================== -->
    <section id="section-leaderboard" class="section">
      <h2 class="section-title">🏆 Leaderboard</h2>
      <p class="section-subtitle">Top eco-warriors in your community</p>

      <div class="card">
        <div class="podium">
          <!-- 2nd Place -->
          <div class="podium-slot">
            <div class="podium-avatar">🧑</div>
            <div class="podium-name">Priya S.</div>
            <div class="podium-block">2</div>
          </div>
          <!-- 1st Place -->
          <div class="podium-slot">
            <div class="podium-avatar">👩</div>
            <div class="podium-name">Meera K.</div>
            <div class="podium-block">1</div>
          </div>
          <!-- 3rd Place -->
          <div class="podium-slot">
            <div class="podium-avatar">👦</div>
            <div class="podium-name">Arjun R.</div>
            <div class="podium-block">3</div>
          </div>
        </div>

        <table class="lb-table">
          <thead>
            <tr>
              <th>#</th>
              <th>User</th>
              <th>Items</th>
              <th>Points</th>
            </tr>
          </thead>
          <tbody>
            <tr><td class="rank">1</td><td>🥇 Meera K.</td><td>142</td><td class="pts">3,480</td></tr>
            <tr><td class="rank">2</td><td>🥈 Priya S.</td><td>128</td><td class="pts">3,120</td></tr>
            <tr><td class="rank">3</td><td>🥉 Arjun R.</td><td>115</td><td class="pts">2,890</td></tr>
            <tr><td class="rank">4</td><td>👤 Rahul M.</td><td>98</td><td class="pts">2,450</td></tr>
            <tr><td class="rank">5</td><td>👤 Sunita D.</td><td>87</td><td class="pts">2,175</td></tr>
            <tr><td class="rank">6</td><td>👤 Vikram P.</td><td>76</td><td class="pts">1,900</td></tr>
            <tr><td class="rank">7</td><td>👤 Kavya N.</td><td>68</td><td class="pts">1,700</td></tr>
            <tr class="me"><td class="rank">8</td><td>⭐ You</td><td>47</td><td class="pts">1,240</td></tr>
            <tr><td class="rank">9</td><td>👤 Aman S.</td><td>41</td><td class="pts">1,025</td></tr>
            <tr><td class="rank">10</td><td>👤 Rohan T.</td><td>38</td><td class="pts">950</td></tr>
          </tbody>
        </table>
      </div>

      <div class="eco-tip" style="margin-top:20px;">🚀 <span>You're 260 points away from rank #7! Scan 10 more items to climb up.</span></div>
    </section>

    <!-- ==================== PROFILE ==================== -->
    <section id="section-profile" class="section">
      <h2 class="section-title">👤 My Profile</h2>
      <p class="section-subtitle">Your eco journey at a glance</p>

      <div class="profile-header">
        <div class="profile-avatar">🌿</div>
        <div class="profile-meta">
          <h3 id="profile-name">Eco Warrior</h3>
          <p id="profile-email">eco@example.com</p>
          <div style="margin-top:6px;"><span class="badge badge-green">Level 5 Recycler</span></div>
          <div class="profile-badges" style="margin-top:8px;">
            <span class="profile-badge">🌊 Ocean Guard</span>
            <span class="profile-badge">🔥 14-Day Streak</span>
            <span class="profile-badge">♻️ 8kg Recycled</span>
          </div>
        </div>
      </div>

      <div class="profile-stats">
        <div class="pstat"><div class="val">47</div><div class="lbl">Items Scanned</div></div>
        <div class="pstat"><div class="val">8.1kg</div><div class="lbl">Total Recycled</div></div>
        <div class="pstat"><div class="val">1,240</div><div class="lbl">Credits</div></div>
      </div>

      <h4 style="font-size:0.88rem;color:var(--text-muted);margin-bottom:12px;font-weight:500;">LEVEL PROGRESS</h4>
      <div class="card" style="margin-bottom:20px;">
        <div style="display:flex;justify-content:space-between;font-size:0.82rem;margin-bottom:8px;">
          <span>Level 5 — Eco Warrior</span>
          <span style="color:var(--green-glow);">1,240 / 1,500 pts</span>
        </div>
        <progress value="1240" max="1500"></progress>
        <p style="font-size:0.76rem;color:var(--text-muted);margin-top:8px;">260 pts to reach Level 6: Eco Champion</p>
      </div>

      <h4 style="font-size:0.88rem;color:var(--text-muted);margin-bottom:12px;font-weight:500;">SETTINGS</h4>
      <div class="profile-settings">
        <div class="pref-row">
          <span>📲 Push Notifications</span>
          <div class="toggle" onclick="this.classList.toggle('off')"></div>
        </div>
        <div class="pref-row">
          <span>📍 Location Services</span>
          <div class="toggle" onclick="this.classList.toggle('off')"></div>
        </div>
        <div class="pref-row">
          <span>🌙 Dark Mode</span>
          <div class="toggle" onclick="this.classList.toggle('off')"></div>
        </div>
        <div class="pref-row">
          <span>📊 Weekly Report Email</span>
          <div class="toggle off" onclick="this.classList.toggle('off')"></div>
        </div>
      </div>

      <button class="btn-secondary" style="margin-top:16px;width:100%;" onclick="alert('Profile update feature coming soon!')">✏️ Edit Profile</button>
    </section>

    <!-- ==================== CONTACT ==================== -->
    <section id="section-contact" class="section">
      <h2 class="section-title">✉️ Contact Us</h2>
      <p class="section-subtitle">Get in touch with the EcoTrack team</p>

      <div class="eco-tip">🌿 <span>Have a suggestion or found a bug? We'd love to hear from you. Every improvement helps the planet.</span></div>

      <div class="contact-grid">
        <div>
          <div class="contact-form">
            <input type="text" id="c-name" placeholder="Your name" />
            <input type="email" id="c-email" placeholder="Email address" />
            <input type="text" id="c-subject" placeholder="Subject" />
            <textarea id="c-msg" rows="5" placeholder="Your message..."></textarea>
            <button class="btn-primary" onclick="sendContactForm()">📨 Send Message</button>
          </div>
          <div class="msg-sent" id="msg-sent">✅ Message sent! We'll get back to you within 24 hours.</div>
        </div>

        <div class="contact-info">
          <div class="contact-info-item">
            <div class="ci-icon">📧</div>
            <div class="ci-text">
              <h5>Email</h5>
              <p>hello@ecotrack.in</p>
            </div>
          </div>
          <div class="contact-info-item">
            <div class="ci-icon">📞</div>
            <div class="ci-text">
              <h5>Helpline</h5>
              <p>+91 98765 43210</p>
            </div>
          </div>
          <div class="contact-info-item">
            <div class="ci-icon">📍</div>
            <div class="ci-text">
              <h5>Office</h5>
              <p>Jaipur, Rajasthan, India</p>
            </div>
          </div>
          <div class="contact-info-item">
            <div class="ci-icon">⏰</div>
            <div class="ci-text">
              <h5>Support Hours</h5>
              <p>Mon–Sat, 9 AM – 6 PM IST</p>
            </div>
          </div>
          <div class="card" style="padding:16px;">
            <h5 style="font-size:0.85rem;margin-bottom:10px;">Follow EcoTrack</h5>
            <div style="display:flex;gap:10px;flex-wrap:wrap;">
              <button class="btn-secondary" style="font-size:0.78rem;padding:7px 14px;">🐦 Twitter</button>
              <button class="btn-secondary" style="font-size:0.78rem;padding:7px 14px;">📸 Instagram</button>
              <button class="btn-secondary" style="font-size:0.78rem;padding:7px 14px;">💼 LinkedIn</button>
            </div>
          </div>
        </div>
      </div>
    </section>

  </main><!-- end .main-content -->
</div><!-- end #app -->

<!-- ============================================================
     CAMERA MODAL — FIX #5: Full Screen Camera
============================================================ -->
<div id="camera-modal">
  <video id="camera-video" autoplay playsinline muted></video>
  <div class="camera-overlay">
    <div class="camera-header">
      <h3>📷 Scan Plastic Item</h3>
      <p>Point camera at plastic label or barcode</p>
    </div>
    <div class="camera-frame">
      <div class="corner-br"></div>
      <div class="corner-bl"></div>
      <div class="camera-scan-line"></div>
    </div>
    <div class="camera-footer">
      <button class="btn-capture" onclick="captureFromCamera()" title="Capture">📸</button>
      <button class="btn-close-camera" onclick="closeCamera()">✕ Close Camera</button>
    </div>
  </div>
</div>

<!-- ============================================================
     JAVASCRIPT
============================================================ -->
<script>
  /* ==============================================
     USER DATA STORE (simple in-memory session)
  ============================================== */
  let currentUser = null;
  let userCredits = 1240;

  /* ==============================================
     AUTH SYSTEM
  ============================================== */
  function switchAuthTab(tab) {
    document.querySelectorAll('.auth-tab').forEach((t, i) => {
      t.classList.toggle('active', (i === 0 && tab === 'login') || (i === 1 && tab === 'signup'));
    });
    document.getElementById('login-form').classList.toggle('active', tab === 'login');
    document.getElementById('signup-form').classList.toggle('active', tab === 'signup');
    document.getElementById('login-error').textContent  = '';
    document.getElementById('signup-error').textContent = '';
  }

  function handleLogin() {
    const email = document.getElementById('login-email').value.trim();
    const pass  = document.getElementById('login-pass').value.trim();
    const err   = document.getElementById('login-error');

    if (!email || !pass) { err.textContent = '⚠️ Please fill in all fields.'; return; }
    if (!email.includes('@')) { err.textContent = '⚠️ Please enter a valid email.'; return; }
    if (pass.length < 4)   { err.textContent = '⚠️ Password too short.'; return; }

    // Simple validation — accept any valid-format credentials
    currentUser = { name: email.split('@')[0], email };
    launchApp();
  }

  function handleSignup() {
    const name  = document.getElementById('su-name').value.trim();
    const email = document.getElementById('su-email').value.trim();
    const pass  = document.getElementById('su-pass').value.trim();
    const err   = document.getElementById('signup-error');

    if (!name || !email || !pass) { err.textContent = '⚠️ Please fill all fields.'; return; }
    if (!email.includes('@'))     { err.textContent = '⚠️ Invalid email address.'; return; }
    if (pass.length < 6)          { err.textContent = '⚠️ Password must be at least 6 characters.'; return; }

    currentUser = { name, email };
    launchApp();
  }

  function launchApp() {
    document.getElementById('auth-screen').style.display = 'none';
    const app = document.getElementById('app');
    app.style.display = 'flex';

    // Set profile name/email from logged-in user
    document.getElementById('profile-name').textContent  = currentUser.name.charAt(0).toUpperCase() + currentUser.name.slice(1);
    document.getElementById('profile-email').textContent = currentUser.email;

    // Show default section = Home
    showSection('home');
  }

  function handleLogout() {
    if (!confirm('Log out of EcoTrack?')) return;
    currentUser = null;
    document.getElementById('app').style.display       = 'none';
    document.getElementById('auth-screen').style.display = 'flex';
    document.getElementById('login-email').value = '';
    document.getElementById('login-pass').value  = '';
    document.getElementById('login-error').textContent = '';
  }

  /* ==============================================
     FIX #1, #2, #3: SECTION SWITCHING
     — Hide ALL, show ONLY selected, update nav
  ============================================== */
  function showSection(id) {
    // 1. Hide all sections
    document.querySelectorAll('.section').forEach(s => {
      s.classList.remove('active');
    });

    // 2. Remove active class from all nav items
    document.querySelectorAll('.nav-item').forEach(n => {
      n.classList.remove('active');
    });

    // 3. Show selected section
    const target = document.getElementById('section-' + id);
    if (target) {
      target.classList.add('active');
      // Scroll content area to top
      target.scrollIntoView({ block: 'start', behavior: 'instant' });
    }

    // 4. Highlight correct nav item
    document.querySelectorAll('.nav-item').forEach(n => {
      if (n.getAttribute('onclick') && n.getAttribute('onclick').includes("'" + id + "'")) {
        n.classList.add('active');
      }
    });

    // 5. Close sidebar on mobile after navigation
    closeSidebar();
  }

  /* ==============================================
     FIX #3: MOBILE SIDEBAR TOGGLE
  ============================================== */
  function toggleSidebar() {
    document.getElementById('sidebar').classList.toggle('open');
    document.getElementById('sidebar-overlay').classList.toggle('open');
  }

  function closeSidebar() {
    document.getElementById('sidebar').classList.remove('open');
    document.getElementById('sidebar-overlay').classList.remove('open');
  }

  /* ==============================================
     FIX #5: CAMERA — Full Screen Modal
  ============================================== */
  let cameraStream = null;

  async function openCamera() {
    const modal = document.getElementById('camera-modal');
    const video = document.getElementById('camera-video');

    try {
      // Request camera permission and get stream
      cameraStream = await navigator.mediaDevices.getUserMedia({
        video: { facingMode: { ideal: 'environment' }, width: { ideal: 1280 }, height: { ideal: 720 } },
        audio: false
      });
      video.srcObject = cameraStream;
      modal.classList.add('open');
    } catch (err) {
      if (err.name === 'NotAllowedError') {
        alert('❌ Camera permission denied. Please allow camera access in your browser settings.');
      } else if (err.name === 'NotFoundError') {
        alert('❌ No camera found on this device.');
      } else {
        alert('❌ Could not open camera: ' + err.message);
      }
    }
  }

  function closeCamera() {
    // Stop all camera tracks
    if (cameraStream) {
      cameraStream.getTracks().forEach(track => track.stop());
      cameraStream = null;
    }
    const video = document.getElementById('camera-video');
    video.srcObject = null;

    // Hide modal
    document.getElementById('camera-modal').classList.remove('open');
  }

  function captureFromCamera() {
    closeCamera();
    // Simulate a scan result after capture
    simulateScanResult();
  }

  /* ==============================================
     SCAN FUNCTIONALITY
  ============================================== */
  const plasticTypes = [
    { code: '#1', name: 'PET (Polyethylene Terephthalate)', rec: 'High ✅', wt: '~0.05 kg', cred: 10, label: 'PET #1 – Recyclable ✅', cls: 'badge-green' },
    { code: '#2', name: 'HDPE (High-Density Polyethylene)', rec: 'High ✅', wt: '~0.18 kg', cred: 25, label: 'HDPE #2 – Recyclable ✅', cls: 'badge-green' },
    { code: '#3', name: 'PVC (Polyvinyl Chloride)', rec: 'Low ⚠️', wt: '~0.22 kg', cred: 5, label: 'PVC #3 – Hazardous ⚠️', cls: 'badge-yellow' },
    { code: '#4', name: 'LDPE (Low-Density Polyethylene)', rec: 'Medium ⚠️', wt: '~0.03 kg', cred: 5, label: 'LDPE #4 – Check Local ⚠️', cls: 'badge-yellow' },
    { code: '#5', name: 'PP (Polypropylene)', rec: 'Medium ✅', wt: '~0.12 kg', cred: 18, label: 'PP #5 – Recyclable ✅', cls: 'badge-green' },
    { code: '#6', name: 'PS (Polystyrene)', rec: 'Low ❌', wt: '~0.08 kg', cred: 3, label: 'PS #6 – Non-Recyclable ❌', cls: 'badge-red' },
  ];

  function handleFileUpload(event) {
    const file = event.target.files[0];
    if (!file) return;
    // Show a brief loading message then simulate result
    setTimeout(simulateScanResult, 800);
  }

  function simulateScanResult() {
    const type = plasticTypes[Math.floor(Math.random() * plasticTypes.length)];
    const result = document.getElementById('scan-result');
    const badge  = document.getElementById('result-badge');

    badge.textContent       = type.label;
    badge.className         = 'plastic-type-badge ' + type.cls;
    document.getElementById('res-type').textContent = type.name;
    document.getElementById('res-code').textContent = type.code;
    document.getElementById('res-rec').textContent  = type.rec;
    document.getElementById('res-wt').textContent   = type.wt;
    document.getElementById('res-cred').textContent = '+' + type.cred + ' 🪙';

    result.classList.add('active');
    result.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
  }

  function confirmDeposit() {
    const credText = document.getElementById('res-cred').textContent;
    const pts = parseInt(credText.replace(/\D/g, '')) || 10;
    userCredits += pts;
    document.getElementById('top-credits').textContent  = userCredits.toLocaleString();
    document.getElementById('credits-display').textContent = userCredits.toLocaleString();
    document.getElementById('scan-result').classList.remove('active');
    document.getElementById('file-input').value = '';
    alert('✅ Deposit confirmed! +' + pts + ' credits added to your account. Thank you for recycling!');
  }

  /* ==============================================
     REWARDS
  ============================================== */
  function redeemReward(cost, name) {
    if (userCredits < cost) {
      alert('❌ Not enough credits. You need ' + cost + ' pts but have ' + userCredits + ' pts.');
      return;
    }
    if (!confirm('Redeem "' + name + '" for ' + cost + ' credits?')) return;
    userCredits -= cost;
    document.getElementById('top-credits').textContent   = userCredits.toLocaleString();
    document.getElementById('credits-display').textContent = userCredits.toLocaleString();
    alert('🎉 "' + name + '" redeemed successfully! Check your email for the voucher.');
  }

  /* ==============================================
     CONTACT FORM
  ============================================== */
  function sendContactForm() {
    const name    = document.getElementById('c-name').value.trim();
    const email   = document.getElementById('c-email').value.trim();
    const subject = document.getElementById('c-subject').value.trim();
    const msg     = document.getElementById('c-msg').value.trim();

    if (!name || !email || !msg) { alert('⚠️ Please fill in Name, Email, and Message.'); return; }
    if (!email.includes('@'))    { alert('⚠️ Please enter a valid email.'); return; }

    document.querySelector('.contact-form').style.display = 'none';
    document.getElementById('msg-sent').style.display = 'block';

    // Reset after a few seconds
    setTimeout(() => {
      document.querySelector('.contact-form').style.display = 'flex';
      document.getElementById('msg-sent').style.display = 'none';
      ['c-name','c-email','c-subject','c-msg'].forEach(id => document.getElementById(id).value = '');
    }, 4000);
  }

  /* ==============================================
     INIT: Close camera on ESC key
  ============================================== */
  document.addEventListener('keydown', e => {
    if (e.key === 'Escape' && cameraStream) closeCamera();
  });
</script>

</body>
</html>
