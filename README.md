<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>PUSPHARAJ // STARK.PROTOCOL</title>
<link rel="preconnect" href="https://fonts.googleapis.com"/>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Share+Tech+Mono&family=Audiowide&display=swap" rel="stylesheet"/>
<style>
  :root {
    --cyan: #00e5ff;
    --cyan-dim: #00bcd4;
    --cyan-ghost: rgba(0,229,255,0.08);
    --cyan-border: rgba(0,229,255,0.3);
    --bg: #000000;
    --bg2: #020d12;
    --glow: 0 0 20px rgba(0,229,255,0.4), 0 0 60px rgba(0,229,255,0.15);
    --glow-strong: 0 0 30px rgba(0,229,255,0.7), 0 0 80px rgba(0,229,255,0.3);
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: #a0d8ef;
    font-family: 'Share Tech Mono', monospace;
    overflow-x: hidden;
    cursor: crosshair;
  }

  /* ── ANIMATED GRID BACKGROUND ── */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(0,229,255,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,229,255,0.03) 1px, transparent 1px);
    background-size: 60px 60px;
    animation: gridDrift 20s linear infinite;
    pointer-events: none;
    z-index: 0;
  }

  @keyframes gridDrift {
    0% { background-position: 0 0; }
    100% { background-position: 60px 60px; }
  }

  /* ── CURSOR TRACKER ── */
  #cursor-glow {
    position: fixed;
    width: 300px;
    height: 300px;
    border-radius: 50%;
    background: radial-gradient(circle, rgba(0,229,255,0.06) 0%, transparent 70%);
    pointer-events: none;
    transform: translate(-50%, -50%);
    z-index: 1;
    transition: opacity 0.3s;
  }

  /* ── SCANLINE ── */
  .scanline {
    position: fixed;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, transparent, rgba(0,229,255,0.6), transparent);
    animation: scanDown 6s linear infinite;
    z-index: 2;
    pointer-events: none;
  }
  @keyframes scanDown {
    0% { top: -2px; opacity: 1; }
    90% { opacity: 1; }
    100% { top: 100vh; opacity: 0; }
  }

  /* ── NOISE OVERLAY ── */
  body::after {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.03'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 0;
    opacity: 0.4;
  }

  .wrapper { position: relative; z-index: 3; max-width: 1200px; margin: 0 auto; padding: 0 2rem; }

  /* ══════════════════════════════
     BOOT SEQUENCE
  ══════════════════════════════ */
  #boot-screen {
    position: fixed;
    inset: 0;
    background: #000;
    z-index: 1000;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    gap: 0.5rem;
    padding: 2rem;
    transition: opacity 0.8s ease;
  }
  #boot-screen.fade-out { opacity: 0; pointer-events: none; }

  .boot-logo {
    font-family: 'Orbitron', sans-serif;
    font-size: clamp(2rem, 6vw, 4rem);
    font-weight: 900;
    color: var(--cyan);
    text-shadow: var(--glow-strong);
    letter-spacing: 0.3em;
    margin-bottom: 2rem;
    animation: flicker 0.1s step-end 3;
  }

  @keyframes flicker { 0%,100%{opacity:1} 50%{opacity:0.3} }

  .boot-line {
    font-family: 'Share Tech Mono', monospace;
    font-size: 0.85rem;
    color: var(--cyan-dim);
    opacity: 0;
    animation: bootAppear 0.3s forwards;
    width: 100%;
    max-width: 500px;
  }
  .boot-line.ok { color: #00ff88; }
  .boot-line.warn { color: #ffcc00; }

  @keyframes bootAppear { to { opacity: 1; } }

  .boot-bar-wrap {
    width: 100%;
    max-width: 500px;
    height: 2px;
    background: rgba(0,229,255,0.15);
    margin-top: 1rem;
    border-radius: 2px;
    overflow: hidden;
  }
  .boot-bar {
    height: 100%;
    background: linear-gradient(90deg, var(--cyan-dim), var(--cyan));
    width: 0%;
    animation: bootLoad 2s ease forwards;
    box-shadow: 0 0 10px var(--cyan);
    animation-delay: 1.5s;
  }
  @keyframes bootLoad { to { width: 100%; } }

  /* ══════════════════════════════
     HERO / IDENTITY
  ══════════════════════════════ */
  #hero {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    overflow: hidden;
  }

  .hero-bg {
    position: absolute;
    inset: 0;
    background:
      radial-gradient(ellipse 80% 60% at 50% 50%, rgba(0,229,255,0.04) 0%, transparent 70%),
      radial-gradient(ellipse 40% 40% at 20% 80%, rgba(0,188,212,0.06) 0%, transparent 50%);
  }

  /* HUD rotating rings */
  .hud-ring {
    position: absolute;
    border: 1px solid var(--cyan-border);
    border-radius: 50%;
    animation: ringRotate linear infinite;
    top: 50%; left: 50%;
    transform-origin: center;
  }
  .hud-ring:nth-child(1) { width:600px;height:600px;margin:-300px 0 0 -300px;animation-duration:40s;opacity:0.3; border-style: dashed; }
  .hud-ring:nth-child(2) { width:450px;height:450px;margin:-225px 0 0 -225px;animation-duration:25s;animation-direction:reverse;opacity:0.2; }
  .hud-ring:nth-child(3) { width:300px;height:300px;margin:-150px 0 0 -150px;animation-duration:15s;opacity:0.15; border-style: dotted; }

  @keyframes ringRotate { from{transform:rotate(0deg)} to{transform:rotate(360deg)} }

  .hero-content {
    position: relative;
    z-index: 2;
    text-align: center;
    padding: 4rem 2rem;
  }

  .hero-tag {
    font-size: 0.75rem;
    letter-spacing: 0.4em;
    color: var(--cyan-dim);
    margin-bottom: 1rem;
    animation: fadeUp 1s ease 3.5s both;
  }

  .hero-name {
    font-family: 'Orbitron', sans-serif;
    font-size: clamp(3rem, 10vw, 7rem);
    font-weight: 900;
    color: var(--cyan);
    text-shadow: var(--glow-strong);
    letter-spacing: 0.15em;
    line-height: 1;
    margin-bottom: 0.5rem;
    animation: fadeUp 1s ease 3.6s both;
    position: relative;
  }

  .hero-name::after {
    content: '';
    position: absolute;
    bottom: -8px;
    left: 50%;
    transform: translateX(-50%);
    width: 60%;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--cyan), transparent);
  }

  .hero-title {
    font-family: 'Audiowide', sans-serif;
    font-size: clamp(0.8rem, 2vw, 1.1rem);
    color: #a0d8ef;
    letter-spacing: 0.35em;
    margin-top: 1.5rem;
    margin-bottom: 2rem;
    animation: fadeUp 1s ease 3.7s both;
  }

  .hero-status {
    display: inline-flex;
    align-items: center;
    gap: 0.6rem;
    font-size: 0.75rem;
    color: #00ff88;
    border: 1px solid rgba(0,255,136,0.3);
    padding: 0.4rem 1.2rem;
    border-radius: 2px;
    letter-spacing: 0.2em;
    animation: fadeUp 1s ease 3.8s both;
  }

  .status-dot {
    width: 6px; height: 6px;
    background: #00ff88;
    border-radius: 50%;
    animation: pulse 2s ease infinite;
    box-shadow: 0 0 8px #00ff88;
  }

  @keyframes pulse { 0%,100%{opacity:1;transform:scale(1)} 50%{opacity:0.4;transform:scale(0.7)} }
  @keyframes fadeUp { from{opacity:0;transform:translateY(30px)} to{opacity:1;transform:translateY(0)} }

  /* ══════════════════════════════
     SECTION STYLES
  ══════════════════════════════ */
  section { padding: 5rem 0; }

  .section-header {
    display: flex;
    align-items: center;
    gap: 1rem;
    margin-bottom: 3rem;
  }

  .section-number {
    font-family: 'Orbitron', sans-serif;
    font-size: 0.7rem;
    color: var(--cyan);
    opacity: 0.5;
    letter-spacing: 0.2em;
    min-width: 40px;
  }

  .section-title {
    font-family: 'Orbitron', sans-serif;
    font-size: clamp(0.9rem, 2vw, 1.2rem);
    color: var(--cyan);
    letter-spacing: 0.3em;
    text-shadow: 0 0 20px rgba(0,229,255,0.4);
    white-space: nowrap;
  }

  .section-line {
    flex: 1;
    height: 1px;
    background: linear-gradient(90deg, var(--cyan-border), transparent);
  }

  /* ── GLASS PANEL ── */
  .glass {
    background: rgba(0,229,255,0.02);
    border: 1px solid var(--cyan-border);
    border-radius: 2px;
    position: relative;
    overflow: hidden;
  }

  .glass::before {
    content: '';
    position: absolute;
    top: 0; left: -100%;
    width: 100%; height: 1px;
    background: linear-gradient(90deg, transparent, var(--cyan), transparent);
    animation: sweepLine 4s ease infinite;
  }

  @keyframes sweepLine { 0%,100%{left:-100%} 50%{left:100%} }

  /* Corner brackets */
  .glass::after {
    content: '';
    position: absolute;
    top: 4px; right: 4px;
    width: 12px; height: 12px;
    border-top: 1px solid var(--cyan);
    border-right: 1px solid var(--cyan);
    opacity: 0.6;
  }

  .corner-bl {
    position: absolute;
    bottom: 4px; left: 4px;
    width: 12px; height: 12px;
    border-bottom: 1px solid var(--cyan);
    border-left: 1px solid var(--cyan);
    opacity: 0.6;
  }

  /* ══════════════════════════════
     IDENTITY PANEL
  ══════════════════════════════ */
  .identity-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1.5rem;
  }

  @media(max-width:700px) { .identity-grid{grid-template-columns:1fr} }

  .identity-panel { padding: 2rem; }

  .panel-label {
    font-size: 0.65rem;
    letter-spacing: 0.4em;
    color: var(--cyan);
    opacity: 0.6;
    margin-bottom: 1.5rem;
    border-bottom: 1px solid var(--cyan-border);
    padding-bottom: 0.5rem;
  }

  .data-row {
    display: flex;
    gap: 1rem;
    margin-bottom: 0.8rem;
    font-size: 0.8rem;
    line-height: 1.5;
  }

  .data-key {
    color: var(--cyan);
    min-width: 120px;
    opacity: 0.7;
    font-size: 0.72rem;
  }

  .data-val {
    color: #c8e8f0;
  }

  .arc-reactor {
    width: 120px; height: 120px;
    border-radius: 50%;
    border: 2px solid var(--cyan-border);
    margin: 0 auto 1.5rem;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    animation: reactorPulse 3s ease infinite;
  }

  .arc-reactor::before {
    content: '';
    position: absolute;
    inset: 8px;
    border-radius: 50%;
    border: 1px solid var(--cyan-border);
    animation: ringRotate 8s linear infinite;
    border-top-color: var(--cyan);
  }

  .arc-core {
    width: 50px; height: 50px;
    background: radial-gradient(circle, rgba(0,229,255,0.9) 0%, rgba(0,188,212,0.4) 50%, transparent 70%);
    border-radius: 50%;
    box-shadow: 0 0 20px var(--cyan), 0 0 40px rgba(0,229,255,0.4), inset 0 0 20px rgba(0,229,255,0.3);
    animation: corePulse 2s ease infinite;
  }

  @keyframes reactorPulse { 0%,100%{box-shadow:0 0 20px rgba(0,229,255,0.3)} 50%{box-shadow:0 0 40px rgba(0,229,255,0.5)} }
  @keyframes corePulse { 0%,100%{opacity:1} 50%{opacity:0.7} }

  .auth-level {
    font-family: 'Orbitron', sans-serif;
    font-size: 0.7rem;
    color: var(--cyan);
    letter-spacing: 0.3em;
    text-align: center;
    margin-top: 0.5rem;
    animation: flicker2 5s ease infinite;
  }

  @keyframes flicker2 { 0%,90%,100%{opacity:1} 92%,98%{opacity:0.3} }

  /* ══════════════════════════════
     TECH REACTOR GRID
  ══════════════════════════════ */
  .tech-category {
    margin-bottom: 2.5rem;
  }

  .category-label {
    font-size: 0.65rem;
    letter-spacing: 0.5em;
    color: var(--cyan);
    opacity: 0.5;
    margin-bottom: 1rem;
    display: flex;
    align-items: center;
    gap: 0.8rem;
  }

  .category-label::after {
    content: '';
    flex: 1;
    height: 1px;
    background: var(--cyan-border);
  }

  .tech-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 0.8rem;
  }

  .tech-node {
    display: flex;
    align-items: center;
    gap: 0.6rem;
    padding: 0.5rem 1rem;
    border: 1px solid rgba(0,229,255,0.2);
    font-size: 0.75rem;
    color: #a0d8ef;
    letter-spacing: 0.15em;
    cursor: default;
    transition: all 0.3s ease;
    position: relative;
    overflow: hidden;
    background: rgba(0,229,255,0.02);
  }

  .tech-node::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(0,229,255,0.1), transparent);
    opacity: 0;
    transition: opacity 0.3s;
  }

  .tech-node:hover {
    border-color: var(--cyan);
    color: var(--cyan);
    box-shadow: var(--glow), inset 0 0 20px rgba(0,229,255,0.05);
    transform: translateY(-2px);
  }

  .tech-node:hover::before { opacity: 1; }

  .node-dot {
    width: 4px; height: 4px;
    background: var(--cyan);
    border-radius: 50%;
    box-shadow: 0 0 6px var(--cyan);
    animation: pulse 2s ease infinite;
  }

  .tech-node:hover .node-dot {
    animation: none;
    box-shadow: 0 0 12px var(--cyan);
  }

  /* ══════════════════════════════
     STATS TELEMETRY
  ══════════════════════════════ */
  .stats-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1.5rem;
  }

  @media(max-width:600px){ .stats-grid{grid-template-columns:1fr} }

  .stat-panel {
    padding: 1.5rem;
    position: relative;
  }

  .stat-panel img {
    width: 100%;
    display: block;
    border-radius: 2px;
    filter: brightness(0.9) contrast(1.05);
    transition: filter 0.3s;
  }

  .stat-panel:hover img { filter: brightness(1.05) contrast(1.1); }

  .stat-label {
    font-size: 0.6rem;
    letter-spacing: 0.4em;
    color: var(--cyan);
    opacity: 0.5;
    margin-bottom: 0.8rem;
  }

  /* scanning overlay on stat panels */
  .scan-overlay {
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, transparent, rgba(0,229,255,0.5), transparent);
    animation: scanH 3s linear infinite;
    pointer-events: none;
  }

  @keyframes scanH {
    0%{top:0} 100%{top:100%}
  }

  /* ══════════════════════════════
     COMM TERMINAL
  ══════════════════════════════ */
  .comm-grid {
    display: flex;
    gap: 1rem;
    flex-wrap: wrap;
    justify-content: center;
  }

  .comm-btn {
    display: flex;
    align-items: center;
    gap: 0.8rem;
    padding: 0.8rem 2rem;
    border: 1px solid var(--cyan-border);
    background: transparent;
    color: var(--cyan);
    font-family: 'Share Tech Mono', monospace;
    font-size: 0.75rem;
    letter-spacing: 0.3em;
    text-decoration: none;
    cursor: pointer;
    position: relative;
    overflow: hidden;
    transition: all 0.3s ease;
  }

  .comm-btn::before {
    content: '';
    position: absolute;
    inset: 0;
    background: var(--cyan);
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.4s ease;
    z-index: -1;
  }

  .comm-btn:hover {
    color: #000;
    border-color: var(--cyan);
    box-shadow: var(--glow-strong);
  }

  .comm-btn:hover::before { transform: scaleX(1); }

  /* ── RIPPLE ── */
  .comm-btn .ripple {
    position: absolute;
    border-radius: 50%;
    background: rgba(0,229,255,0.3);
    transform: scale(0);
    animation: rippleAnim 0.6s linear;
    pointer-events: none;
  }
  @keyframes rippleAnim { to{transform:scale(4);opacity:0} }

  /* ══════════════════════════════
     FOOTER
  ══════════════════════════════ */
  footer {
    border-top: 1px solid var(--cyan-border);
    padding: 2rem;
    text-align: center;
    font-size: 0.7rem;
    color: rgba(160,216,239,0.4);
    letter-spacing: 0.3em;
    position: relative;
  }

  footer::before {
    content: '';
    position: absolute;
    top: 0; left: 50%; transform: translateX(-50%);
    width: 60%;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--cyan), transparent);
    box-shadow: 0 0 10px var(--cyan);
  }

  .quote-text {
    font-family: 'Audiowide', sans-serif;
    font-size: 0.75rem;
    color: rgba(0,229,255,0.4);
    letter-spacing: 0.1em;
    font-style: italic;
    margin-bottom: 1rem;
    max-width: 600px;
    margin-left: auto;
    margin-right: auto;
    line-height: 1.8;
  }

  /* ══════════════════════════════
     SCROLL FADE IN
  ══════════════════════════════ */
  .reveal {
    opacity: 0;
    transform: translateY(40px);
    transition: opacity 0.8s ease, transform 0.8s ease;
  }
  .reveal.visible {
    opacity: 1;
    transform: translateY(0);
  }

  /* ══════════════════════════════
     TICKER
  ══════════════════════════════ */
  .ticker {
    border-top: 1px solid var(--cyan-border);
    border-bottom: 1px solid var(--cyan-border);
    padding: 0.5rem 0;
    overflow: hidden;
    background: rgba(0,229,255,0.02);
    margin-bottom: 0;
  }

  .ticker-inner {
    display: flex;
    gap: 4rem;
    animation: tickerScroll 30s linear infinite;
    white-space: nowrap;
  }

  .ticker-item {
    font-size: 0.65rem;
    letter-spacing: 0.3em;
    color: var(--cyan);
    opacity: 0.5;
  }

  @keyframes tickerScroll { from{transform:translateX(0)} to{transform:translateX(-50%)} }

  /* ── NAV ── */
  nav {
    position: fixed;
    top: 0; left: 0; right: 0;
    z-index: 100;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 1rem 2rem;
    background: rgba(0,0,0,0.7);
    backdrop-filter: blur(10px);
    border-bottom: 1px solid rgba(0,229,255,0.1);
    transform: translateY(-100%);
    animation: navSlide 0.5s ease 4.5s forwards;
  }

  @keyframes navSlide { to{transform:translateY(0)} }

  .nav-brand {
    font-family: 'Orbitron', sans-serif;
    font-size: 0.85rem;
    color: var(--cyan);
    letter-spacing: 0.3em;
    text-shadow: 0 0 15px rgba(0,229,255,0.5);
  }

  .nav-links {
    display: flex;
    gap: 2rem;
    list-style: none;
  }

  .nav-links a {
    font-size: 0.65rem;
    letter-spacing: 0.25em;
    color: rgba(160,216,239,0.6);
    text-decoration: none;
    transition: color 0.3s, text-shadow 0.3s;
  }

  .nav-links a:hover {
    color: var(--cyan);
    text-shadow: 0 0 10px rgba(0,229,255,0.5);
  }

  @media(max-width:600px){ .nav-links{display:none} }

  /* ── CONTRIBUTION GRAPH ── */
  .contrib-wrap {
    padding: 1.5rem;
    position: relative;
    overflow: hidden;
  }

  .contrib-wrap img { width:100%; border-radius:2px; }

  .radar-overlay {
    position: absolute;
    width: 200px; height: 200px;
    border-radius: 50%;
    border: 1px solid rgba(0,229,255,0.15);
    top: 50%; left: 50%;
    transform: translate(-50%,-50%);
    pointer-events: none;
  }

  .radar-overlay::before {
    content:'';
    position:absolute;
    inset:0;
    border-radius:50%;
    background: conic-gradient(rgba(0,229,255,0.2) 0deg, transparent 60deg, transparent 360deg);
    animation: radarSpin 4s linear infinite;
  }

  @keyframes radarSpin { from{transform:rotate(0deg)} to{transform:rotate(360deg)} }

  /* ── LEETCODE PANEL ── */
  .stats-row-2 {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1.5rem;
    margin-top: 1.5rem;
  }
  @media(max-width:600px){ .stats-row-2{grid-template-columns:1fr} }

</style>
</head>
<body>

<!-- CURSOR GLOW -->
<div id="cursor-glow"></div>

<!-- SCANLINE -->
<div class="scanline"></div>

<!-- BOOT SCREEN -->
<div id="boot-screen">
  <div class="boot-logo">STARK.PROTOCOL</div>
  <div id="boot-lines"></div>
  <div class="boot-bar-wrap"><div class="boot-bar"></div></div>
</div>

<!-- NAV -->
<nav>
  <div class="nav-brand">PUSPHARAJ // v9.0</div>
  <ul class="nav-links">
    <li><a href="#identity">IDENTITY</a></li>
    <li><a href="#tech">TECH STACK</a></li>
    <li><a href="#telemetry">TELEMETRY</a></li>
    <li><a href="#comm">COMM-LINK</a></li>
  </ul>
</nav>

<!-- TICKER -->
<div class="ticker" style="position:relative;z-index:4;margin-top:0;padding-top:3.5rem;">
  <div class="ticker-inner" id="ticker-inner"></div>
</div>

<!-- HERO -->
<section id="hero">
  <div class="hero-bg"></div>
  <div class="hud-ring"></div>
  <div class="hud-ring"></div>
  <div class="hud-ring"></div>
  <div class="hero-content">
    <div class="hero-tag">// STARK.PROTOCOL — IDENTITY MODULE INITIALIZED</div>
    <div class="hero-name">PUSPHARAJ</div>
    <div class="hero-title">SOFTWARE ENGINEER &nbsp;|&nbsp; BACKEND SPECIALIST &nbsp;|&nbsp; SYSTEMS ARCHITECT</div>
    <div class="hero-status">
      <div class="status-dot"></div>
      SYSTEM OPERATIONAL &nbsp;·&nbsp; AUTH LEVEL 9 &nbsp;·&nbsp; ROOT ACCESS
    </div>
  </div>
</section>

<!-- IDENTITY -->
<section id="identity">
  <div class="wrapper">
    <div class="section-header reveal">
      <div class="section-number">01</div>
      <div class="section-title">AI CORE IDENTITY PANEL</div>
      <div class="section-line"></div>
    </div>
    <div class="identity-grid reveal">
      <div class="glass identity-panel">
        <div class="corner-bl"></div>
        <div class="panel-label">USER_IDENTIFICATION</div>
        <div class="data-row"><span class="data-key">NAME</span><span class="data-val">Puspharaj</span></div>
        <div class="data-row"><span class="data-key">DESIGNATION</span><span class="data-val">Java Full Stack Engineer</span></div>
        <div class="data-row"><span class="data-key">AUTHORIZATION</span><span class="data-val" style="color:#00ff88">Level 9 — Root Access</span></div>
        <div class="data-row"><span class="data-key">SPECIALTY</span><span class="data-val">Backend Engineering &amp; Microservices</span></div>
        <br/>
        <div class="panel-label">CORE_DIRECTIVES</div>
        <div class="data-row"><span class="data-key">›</span><span class="data-val">Engineer high-performance APIs</span></div>
        <div class="data-row"><span class="data-key">›</span><span class="data-val">Design robust scalable systems</span></div>
        <div class="data-row"><span class="data-key">›</span><span class="data-val">Optimize database architectures</span></div>
        <div class="data-row"><span class="data-key">›</span><span class="data-val">Maintain maximum system uptime</span></div>
      </div>
      <div class="glass identity-panel" style="display:flex;flex-direction:column;align-items:center;justify-content:center;">
        <div class="corner-bl"></div>
        <div class="arc-reactor">
          <div class="arc-core"></div>
        </div>
        <div class="auth-level">ARC REACTOR // ONLINE</div>
        <br/>
        <div class="panel-label" style="text-align:center;">SYSTEM_STATUS</div>
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:0.8rem;width:100%;margin-top:0.5rem;">
          <div class="glass" style="padding:0.8rem;text-align:center;">
            <div style="font-size:0.6rem;opacity:0.5;letter-spacing:0.3em;margin-bottom:0.3rem;">BACKEND</div>
            <div style="color:#00ff88;font-size:0.75rem;">● OPTIMAL</div>
          </div>
          <div class="glass" style="padding:0.8rem;text-align:center;">
            <div style="font-size:0.6rem;opacity:0.5;letter-spacing:0.3em;margin-bottom:0.3rem;">FRONTEND</div>
            <div style="color:#00ff88;font-size:0.75rem;">● ONLINE</div>
          </div>
          <div class="glass" style="padding:0.8rem;text-align:center;">
            <div style="font-size:0.6rem;opacity:0.5;letter-spacing:0.3em;margin-bottom:0.3rem;">CLOUD</div>
            <div style="color:#00ff88;font-size:0.75rem;">● ACTIVE</div>
          </div>
          <div class="glass" style="padding:0.8rem;text-align:center;">
            <div style="font-size:0.6rem;opacity:0.5;letter-spacing:0.3em;margin-bottom:0.3rem;">API CORE</div>
            <div style="color:#00ff88;font-size:0.75rem;">● FIRING</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- TECH STACK -->
<section id="tech">
  <div class="wrapper">
    <div class="section-header reveal">
      <div class="section-number">02</div>
      <div class="section-title">TECHNOLOGY REACTOR GRID</div>
      <div class="section-line"></div>
    </div>

    <div class="tech-category reveal">
      <div class="category-label">CORE REACTOR — BACKEND SYSTEMS</div>
      <div class="tech-grid" id="backend-grid"></div>
    </div>

    <div class="tech-category reveal">
      <div class="category-label">HOLOGRAPHIC UI — FRONTEND MODULES</div>
      <div class="tech-grid" id="frontend-grid"></div>
    </div>

    <div class="tech-category reveal">
      <div class="category-label">STARK INFRA — CLOUD &amp; TOOLS</div>
      <div class="tech-grid" id="cloud-grid"></div>
    </div>
  </div>
</section>

<!-- TELEMETRY -->
<section id="telemetry">
  <div class="wrapper">
    <div class="section-header reveal">
      <div class="section-number">03</div>
      <div class="section-title">LIVE TELEMETRY COMMAND CENTER</div>
      <div class="section-line"></div>
    </div>

    <div class="stats-grid reveal">
      <div class="glass stat-panel">
        <div class="scan-overlay"></div>
        <div class="corner-bl"></div>
        <div class="stat-label">GITHUB CORE STATS</div>
        <img src="https://github-readme-stats.vercel.app/api?username=puspharaj-7&show_icons=true&bg_color=000000&title_color=00e5ff&text_color=a0d8ef&icon_color=00e5ff&hide_border=true&border_radius=0" alt="GitHub Stats"/>
      </div>
      <div class="glass stat-panel">
        <div class="scan-overlay" style="animation-delay:-1.5s"></div>
        <div class="corner-bl"></div>
        <div class="stat-label">STREAK MONITOR</div>
        <img src="https://github-readme-streak-stats.herokuapp.com/?user=puspharaj-7&background=000000&theme=dark&ring=00e5ff&fire=00e5ff&currStreakLabel=00e5ff&hide_border=true&stroke=00e5ff" alt="GitHub Streak"/>
      </div>
    </div>

    <div class="stats-row-2 reveal">
      <div class="glass stat-panel">
        <div class="scan-overlay" style="animation-delay:-0.5s"></div>
        <div class="corner-bl"></div>
        <div class="stat-label">LANGUAGE DISTRIBUTION</div>
        <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=puspharaj-7&layout=compact&bg_color=000000&title_color=00e5ff&text_color=a0d8ef&hide_border=true&border_radius=0" alt="Top Languages"/>
      </div>
      <div class="glass stat-panel">
        <div class="scan-overlay" style="animation-delay:-2s"></div>
        <div class="corner-bl"></div>
        <div class="stat-label">LEETCODE PERFORMANCE</div>
        <img src="https://leetcard.jacoblin.cool/puspharaj-7?theme=dark&font=Share+Tech+Mono&ext=contest&border=0&radius=0&bg=000000&color=00e5ff" alt="LeetCode Stats"/>
      </div>
    </div>

    <div class="glass contrib-wrap reveal" style="margin-top:1.5rem;">
      <div class="corner-bl"></div>
      <div class="stat-label" style="padding:1.5rem 1.5rem 0;">GLOBAL ACTIVITY HEATMAP // CONTRIBUTION NETWORK</div>
      <div style="padding:0 1.5rem 1.5rem;">
        <img src="https://github-readme-activity-graph.vercel.app/graph?username=puspharaj-7&bg_color=000000&color=00e5ff&line=00e5ff&point=FFFFFF&area=true&hide_border=true&custom_title=System%20Contributions" alt="Contribution Graph"/>
      </div>
      <div class="radar-overlay"></div>
    </div>
  </div>
</section>

<!-- COMM -->
<section id="comm">
  <div class="wrapper">
    <div class="section-header reveal">
      <div class="section-number">04</div>
      <div class="section-title">ESTABLISH COMM-LINK</div>
      <div class="section-line"></div>
    </div>
    <div class="glass" style="padding:3rem;text-align:center;" class="reveal">
      <div class="corner-bl"></div>
      <div style="font-size:0.65rem;letter-spacing:0.4em;opacity:0.4;margin-bottom:2rem;">COMMUNICATION TERMINAL // SECURE CHANNEL ACTIVE</div>
      <div class="comm-grid reveal">
        <a class="comm-btn" href="https://www.linkedin.com/in/puspharaj-s/" target="_blank">
          <svg width="14" height="14" viewBox="0 0 24 24" fill="currentColor"><path d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"/></svg>
          LINKEDIN
        </a>
        <a class="comm-btn" href="https://github.com/puspharaj-7" target="_blank">
          <svg width="14" height="14" viewBox="0 0 24 24" fill="currentColor"><path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/></svg>
          GITHUB
        </a>
        <a class="comm-btn" href="https://leetcode.com/puspharaj-7" target="_blank">
          <svg width="14" height="14" viewBox="0 0 24 24" fill="currentColor"><path d="M13.483 0a1.374 1.374 0 0 0-.961.438L7.116 6.226l-3.854 4.126a5.266 5.266 0 0 0-1.209 2.104 5.35 5.35 0 0 0-.125.513 5.527 5.527 0 0 0 .062 2.362 5.83 5.83 0 0 0 .349 1.017 5.938 5.938 0 0 0 1.271 1.818l4.277 4.193.039.038c2.248 2.165 5.852 2.133 8.063-.074l2.396-2.392c.54-.54.54-1.414.003-1.955a1.378 1.378 0 0 0-1.951-.003l-2.396 2.392a3.021 3.021 0 0 1-4.205.038l-.02-.019-4.276-4.193c-.652-.64-.972-1.469-.948-2.263a2.68 2.68 0 0 1 .066-.523 2.545 2.545 0 0 1 .619-1.164L9.13 8.114c1.058-1.134 3.204-1.27 4.43-.278l3.501 2.831c.593.48 1.461.387 1.94-.207a1.384 1.384 0 0 0-.207-1.943l-3.5-2.831c-.8-.647-1.766-1.045-2.774-1.202l2.015-2.158A1.384 1.384 0 0 0 13.483 0zm-2.866 12.815a1.38 1.38 0 0 0-1.38 1.382 1.38 1.38 0 0 0 1.38 1.382H20.79a1.38 1.38 0 0 0 1.38-1.382 1.38 1.38 0 0 0-1.38-1.382z"/></svg>
          LEETCODE
        </a>
      </div>
    </div>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <div class="quote-text">"You can take away my suits, you can take away my home, but there's one thing you can never take away from me. I am Iron Man."</div>
  <div>© PUSPHARAJ &nbsp;·&nbsp; STARK.PROTOCOL v9.0 &nbsp;·&nbsp; ALL SYSTEMS NOMINAL</div>
</footer>

<script>
// ── CURSOR GLOW ──
const glow = document.getElementById('cursor-glow');
document.addEventListener('mousemove', e => {
  glow.style.left = e.clientX + 'px';
  glow.style.top = e.clientY + 'px';
});

// ── BOOT SEQUENCE ──
const bootLines = [
  { text: '[INITIALIZING STARK.PROTOCOL...]', delay: 200 },
  { text: '[LOADING CORE SYSTEMS...]', delay: 500 },
  { text: '[AUTHENTICATING USER: PUSPHARAJ]', delay: 800 },
  { text: '[AUTHORIZATION LEVEL: ROOT ✓]', cls: 'ok', delay: 1100 },
  { text: '[BACKEND MODULE: ONLINE ✓]', cls: 'ok', delay: 1300 },
  { text: '[MICROSERVICES: ACTIVE ✓]', cls: 'ok', delay: 1500 },
  { text: '[DATABASE LAYER: CONNECTED ✓]', cls: 'ok', delay: 1700 },
  { text: '[ALL SYSTEMS OPTIMAL]', cls: 'ok', delay: 2000 },
  { text: '[WELCOME, SIR.]', delay: 2300 },
];

const container = document.getElementById('boot-lines');
bootLines.forEach(l => {
  setTimeout(() => {
    const el = document.createElement('div');
    el.className = 'boot-line' + (l.cls ? ' ' + l.cls : '');
    el.textContent = l.text;
    container.appendChild(el);
  }, l.delay);
});

setTimeout(() => {
  const bs = document.getElementById('boot-screen');
  bs.classList.add('fade-out');
  setTimeout(() => { bs.style.display = 'none'; }, 800);
}, 3800);

// ── TICKER ──
const tickerItems = [
  'BACKEND.SYSTEMS // ONLINE', 'JAVA.CORE // ACTIVE', 'SPRING.BOOT // RUNNING',
  'MICROSERVICES // DEPLOYED', 'API.GATEWAY // SECURED', 'DATABASE // OPTIMIZED',
  'CLOUD.INFRA // SCALING', 'DOCKER // CONTAINERIZED', 'REACT.UI // RENDERED',
  'SYSTEM.STATUS // NOMINAL', 'AUTH.LEVEL // ROOT', 'UPTIME // 99.99%',
];
const inner = document.getElementById('ticker-inner');
// duplicate for seamless loop
[...tickerItems, ...tickerItems].forEach(t => {
  const span = document.createElement('div');
  span.className = 'ticker-item';
  span.textContent = '◈ ' + t;
  inner.appendChild(span);
});

// ── TECH NODES ──
const techData = {
  backend: ['Java', 'Spring Boot', 'Python', 'MySQL', 'Node.js', 'Express', 'PostgreSQL', 'MongoDB', 'Redis'],
  frontend: ['JavaScript', 'TypeScript', 'React', 'HTML5', 'CSS3', 'Bootstrap', 'Tailwind', 'Vite', 'Next.js'],
  cloud: ['Git', 'GitHub', 'VS Code', 'IntelliJ', 'Docker', 'AWS', 'Postman', 'Linux', 'Nginx'],
};

function buildGrid(id, items) {
  const grid = document.getElementById(id);
  items.forEach(name => {
    const node = document.createElement('div');
    node.className = 'tech-node';
    node.innerHTML = `<div class="node-dot"></div>${name}`;
    grid.appendChild(node);
  });
}

buildGrid('backend-grid', techData.backend);
buildGrid('frontend-grid', techData.frontend);
buildGrid('cloud-grid', techData.cloud);

// ── RIPPLE on comm buttons ──
document.querySelectorAll('.comm-btn').forEach(btn => {
  btn.addEventListener('click', e => {
    const r = document.createElement('span');
    r.className = 'ripple';
    const rect = btn.getBoundingClientRect();
    const size = Math.max(rect.width, rect.height);
    r.style.cssText = `width:${size}px;height:${size}px;left:${e.clientX - rect.left - size/2}px;top:${e.clientY - rect.top - size/2}px`;
    btn.appendChild(r);
    setTimeout(() => r.remove(), 600);
  });
});

// ── SCROLL REVEAL ──
const reveals = document.querySelectorAll('.reveal');
const observer = new IntersectionObserver(entries => {
  entries.forEach(e => {
    if (e.isIntersecting) { e.target.classList.add('visible'); observer.unobserve(e.target); }
  });
}, { threshold: 0.1 });
reveals.forEach(el => observer.observe(el));
</script>
</body>
</html>



