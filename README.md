# scaleready
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SCALEREADY™ Execution Gap Diagnostic</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=Syne:wght@400;500;600;700;800&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --ink: #52211A;
    --ink-2: #6B3D34;
    --ink-3: #9A7A72;
    --ink-4: #C4ADA6;
    --paper: #F5F3EE;
    --paper-2: #EEECE6;
    --paper-3: #E4E1D8;
    --gold: #B8860B;
    --gold-light: #F0E4C0;
    --gold-mid: #D4A820;
    --teal: #c29580;
    --teal-light: #F3EDE8;
    --teal-mid: #e8e3de;
    --red-zone: #8B2020;
    --red-zone-bg: #F5E8E8;
    --amber-zone: #7A4B0A;
    --amber-zone-bg: #FBF0DC;
    --green-zone: #1A5C30;
    --green-zone-bg: #E4F2E8;
    --radius: 3px;
    --radius-lg: 8px;
    --radius-xl: 16px;
  }

  html { scroll-behavior: smooth; }

  body {
    font-family: 'DM Sans', sans-serif;
    background: var(--paper);
    color: var(--ink);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ─── COVER ─── */
  .cover {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    position: relative;
    background: var(--ink);
    overflow: hidden;
  }

  .cover-grid {
    position: absolute;
    inset: 0;
    background-image:
      linear-gradient(rgba(184,134,11,0.08) 1px, transparent 1px),
      linear-gradient(90deg, rgba(184,134,11,0.08) 1px, transparent 1px);
    background-size: 48px 48px;
  }

  .cover-accent {
    position: absolute;
    top: 0; right: 0;
    width: 50%;
    height: 100%;
    background: linear-gradient(135deg, transparent 30%, rgba(194,149,128,0.25) 100%);
    pointer-events: none;
  }

  .cover-bar {
    position: absolute;
    bottom: 0; left: 0; right: 0;
    height: 6px;
    background: linear-gradient(90deg, var(--gold) 0%, var(--teal) 100%);
  }

  .cover-inner {
    position: relative;
    z-index: 1;
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: 80px 64px;
    max-width: 900px;
  }

  .cover-eyebrow {
    font-family: 'Syne', sans-serif;
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--gold);
    margin-bottom: 28px;
    display: flex;
    align-items: center;
    gap: 12px;
  }
  .cover-eyebrow::before {
    content: '';
    display: block;
    width: 32px;
    height: 1px;
    background: var(--gold);
  }

  .cover-title {
    font-family: 'DM Serif Display', serif;
    font-size: clamp(44px, 6vw, 72px);
    line-height: 1.05;
    color: #F5F3EE;
    margin-bottom: 24px;
  }

  .cover-title em {
    font-style: italic;
    color: var(--gold-mid);
  }

  .cover-sub {
    font-size: 17px;
    line-height: 1.65;
    color: rgba(245,243,238,0.65);
    max-width: 560px;
    margin-bottom: 48px;
    font-weight: 300;
  }

  .cover-meta {
    display: flex;
    align-items: center;
    gap: 24px;
    flex-wrap: wrap;
  }

  .cover-time {
    font-family: 'Syne', sans-serif;
    font-size: 12px;
    font-weight: 600;
    color: rgba(245,243,238,0.45);
    letter-spacing: 0.1em;
  }

  .btn-start {
    display: inline-flex;
    align-items: center;
    gap: 10px;
    background: var(--gold);
    color: var(--ink);
    font-family: 'Syne', sans-serif;
    font-size: 13px;
    font-weight: 700;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    padding: 16px 32px;
    border: none;
    cursor: pointer;
    transition: background 0.2s, transform 0.15s;
    border-radius: var(--radius);
  }
  .btn-start:hover { background: var(--gold-mid); transform: translateY(-1px); }
  .btn-start svg { transition: transform 0.2s; }
  .btn-start:hover svg { transform: translateX(4px); }

  .cover-brand {
    position: absolute;
    bottom: 32px;
    right: 64px;
    font-family: 'Syne', sans-serif;
    font-size: 12px;
    font-weight: 700;
    color: rgba(245,243,238,0.25);
    letter-spacing: 0.15em;
    text-transform: uppercase;
  }

  /* ─── DIAGNOSTIC SHELL ─── */
  #diagnostic { display: none; }

  .diag-header {
    background: var(--ink);
    padding: 20px 48px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    position: sticky;
    top: 0;
    z-index: 100;
  }

  .diag-brand {
    font-family: 'Syne', sans-serif;
    font-size: 14px;
    font-weight: 800;
    color: var(--gold);
    letter-spacing: 0.12em;
    text-transform: uppercase;
  }

  .progress-track {
    flex: 1;
    max-width: 340px;
    margin: 0 40px;
  }

  .progress-labels {
    display: flex;
    justify-content: space-between;
    margin-bottom: 6px;
  }

  .progress-label {
    font-family: 'Syne', sans-serif;
    font-size: 10px;
    font-weight: 600;
    color: rgba(245,243,238,0.35);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    transition: color 0.3s;
  }
  .progress-label.active { color: var(--gold); }

  .progress-bar-wrap {
    height: 3px;
    background: rgba(255,255,255,0.1);
    border-radius: 2px;
    overflow: hidden;
  }
  .progress-bar-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--gold) 0%, var(--teal) 100%);
    border-radius: 2px;
    transition: width 0.5s cubic-bezier(0.4,0,0.2,1);
    width: 0%;
  }

  .diag-step-counter {
    font-family: 'Syne', sans-serif;
    font-size: 11px;
    font-weight: 600;
    color: rgba(245,243,238,0.4);
    letter-spacing: 0.08em;
    white-space: nowrap;
  }

  /* ─── SECTION PAGES ─── */
  .section-page {
    display: none;
    min-height: calc(100vh - 70px);
    padding: 60px 48px 80px;
    max-width: 840px;
    margin: 0 auto;
    animation: fadeUp 0.4s ease both;
  }
  .section-page.active { display: block; }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(18px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  .section-eyebrow {
    font-family: 'Syne', sans-serif;
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--teal);
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .section-eyebrow::before {
    content: '';
    width: 24px; height: 1px;
    background: var(--teal);
  }

  .section-title {
    font-family: 'DM Serif Display', serif;
    font-size: 36px;
    line-height: 1.15;
    color: var(--ink);
    margin-bottom: 10px;
  }

  .section-desc {
    font-size: 15px;
    line-height: 1.7;
    color: var(--ink-3);
    max-width: 560px;
    margin-bottom: 44px;
  }

  /* ─── QUESTIONS ─── */
  .question-block {
    margin-bottom: 36px;
    padding-bottom: 36px;
    border-bottom: 1px solid var(--paper-3);
  }
  .question-block:last-of-type { border-bottom: none; }

  .q-text {
    font-size: 16px;
    font-weight: 400;
    line-height: 1.6;
    color: var(--ink-2);
    margin-bottom: 18px;
  }
  .q-text strong { color: var(--ink); font-weight: 500; }

  .q-number {
    font-family: 'Syne', sans-serif;
    font-size: 10px;
    font-weight: 700;
    color: var(--ink-4);
    letter-spacing: 0.1em;
    margin-bottom: 6px;
  }

  .scale-row {
    display: flex;
    gap: 8px;
    align-items: center;
  }

  .scale-label {
    font-size: 11px;
    color: var(--ink-4);
    white-space: nowrap;
    width: 96px;
    flex-shrink: 0;
  }
  .scale-label:last-child { text-align: right; }

  .scale-options {
    display: flex;
    gap: 8px;
    flex: 1;
    justify-content: center;
  }

  .scale-btn {
    width: 44px; height: 44px;
    border-radius: 50%;
    border: 1.5px solid var(--paper-3);
    background: var(--paper-2);
    font-family: 'Syne', sans-serif;
    font-size: 13px;
    font-weight: 700;
    color: var(--ink-3);
    cursor: pointer;
    transition: all 0.18s;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
  }
  .scale-btn:hover {
    border-color: var(--teal);
    color: var(--teal);
    background: var(--teal-light);
    transform: scale(1.08);
  }
  .scale-btn.selected {
    background: var(--teal);
    border-color: var(--teal);
    color: var(--ink);
    transform: scale(1.12);
    box-shadow: 0 4px 12px rgba(82,33,26,0.3);
  }

  /* ─── NAV ─── */
  .section-nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 48px;
    padding-top: 32px;
    border-top: 1px solid var(--paper-3);
  }

  .btn-nav {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    font-family: 'Syne', sans-serif;
    font-size: 12px;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    padding: 13px 24px;
    border-radius: var(--radius);
    cursor: pointer;
    transition: all 0.18s;
    border: none;
  }

  .btn-back {
    background: transparent;
    color: var(--ink-3);
    border: 1.5px solid var(--paper-3);
  }
  .btn-back:hover { border-color: var(--ink-4); color: var(--ink-2); }

  .btn-next {
    background: var(--teal);
    color: var(--ink);
  }
  .btn-next:hover { background: #A87A66; color: #fff; transform: translateY(-1px); }
  .btn-next:disabled { background: var(--paper-3); color: var(--ink-4); cursor: not-allowed; transform: none; }

  .btn-submit {
    background: var(--gold);
    color: var(--ink);
  }
  .btn-submit:hover { background: var(--gold-mid); transform: translateY(-1px); }

  .nav-hint {
    font-size: 12px;
    color: var(--ink-4);
  }

  /* ─── RESULTS ─── */
  #results { display: none; }

  .results-hero {
    background: var(--ink);
    padding: 80px 64px 60px;
    position: relative;
    overflow: hidden;
  }

  .results-hero-grid {
    position: absolute;
    inset: 0;
    background-image:
      linear-gradient(rgba(184,134,11,0.06) 1px, transparent 1px),
      linear-gradient(90deg, rgba(184,134,11,0.06) 1px, transparent 1px);
    background-size: 48px 48px;
  }

  .results-inner { position: relative; z-index: 1; max-width: 840px; margin: 0 auto; }

  .results-eyebrow {
    font-family: 'Syne', sans-serif;
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--gold);
    margin-bottom: 16px;
  }

  .results-headline {
    font-family: 'DM Serif Display', serif;
    font-size: 48px;
    color: #F5F3EE;
    line-height: 1.1;
    margin-bottom: 12px;
  }

  .score-display {
    display: flex;
    align-items: flex-end;
    gap: 24px;
    margin: 36px 0;
  }

  .score-circle {
    width: 120px; height: 120px;
    border-radius: 50%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    border: 3px solid var(--gold);
    position: relative;
    flex-shrink: 0;
  }

  .score-number {
    font-family: 'Syne', sans-serif;
    font-size: 36px;
    font-weight: 800;
    color: #F5F3EE;
    line-height: 1;
  }

  .score-denom {
    font-family: 'Syne', sans-serif;
    font-size: 12px;
    color: rgba(245,243,238,0.4);
  }

  .score-zone-info { flex: 1; }

  .score-zone-label {
    font-family: 'Syne', sans-serif;
    font-size: 13px;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin-bottom: 8px;
  }

  .score-zone-desc {
    font-size: 16px;
    line-height: 1.65;
    color: rgba(245,243,238,0.6);
    font-weight: 300;
  }

  .results-body {
    padding: 60px 64px 80px;
    max-width: 840px;
    margin: 0 auto;
  }

  .pillar-results { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; margin-bottom: 48px; }

  .pillar-card {
    background: var(--paper-2);
    border: 1px solid var(--paper-3);
    border-radius: var(--radius-xl);
    padding: 24px;
  }

  .pillar-card-name {
    font-family: 'Syne', sans-serif;
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--ink-3);
    margin-bottom: 10px;
  }

  .pillar-score-row {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-bottom: 10px;
  }

  .pillar-score-val {
    font-family: 'Syne', sans-serif;
    font-size: 26px;
    font-weight: 800;
    color: var(--ink);
    line-height: 1;
  }

  .pillar-score-max {
    font-size: 13px;
    color: var(--ink-4);
  }

  .pillar-bar-wrap {
    height: 6px;
    background: var(--paper-3);
    border-radius: 3px;
    overflow: hidden;
    margin-bottom: 12px;
  }

  .pillar-bar-fill {
    height: 100%;
    border-radius: 3px;
    transition: width 1s cubic-bezier(0.4,0,0.2,1);
  }

  .pillar-status {
    font-size: 12px;
    font-weight: 500;
  }

  .status-critical { color: var(--red-zone); }
  .status-developing { color: var(--amber-zone); }
  .status-strong { color: var(--green-zone); }

  .bar-critical { background: #C0392B; }
  .bar-developing { background: #E67E22; }
  .bar-strong { background: var(--teal); }

  /* priority findings */
  .findings-title {
    font-family: 'DM Serif Display', serif;
    font-size: 28px;
    margin-bottom: 24px;
  }

  .finding-item {
    display: flex;
    gap: 16px;
    padding: 20px 24px;
    border-radius: var(--radius-lg);
    margin-bottom: 12px;
    align-items: flex-start;
  }

  .finding-critical { background: var(--red-zone-bg); border-left: 3px solid var(--red-zone); }
  .finding-developing { background: var(--amber-zone-bg); border-left: 3px solid #D97706; }
  .finding-strong { background: var(--green-zone-bg); border-left: 3px solid var(--teal); }

  .finding-icon {
    font-size: 18px;
    flex-shrink: 0;
    margin-top: 1px;
  }

  .finding-text { flex: 1; }

  .finding-label {
    font-family: 'Syne', sans-serif;
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    margin-bottom: 4px;
  }

  .finding-critical .finding-label { color: var(--red-zone); }
  .finding-developing .finding-label { color: var(--amber-zone); }
  .finding-strong .finding-label { color: var(--teal); }

  .finding-desc {
    font-size: 14px;
    line-height: 1.6;
    color: var(--ink-2);
  }

  /* CTA block */
  .cta-block {
    background: var(--ink);
    border-radius: var(--radius-xl);
    padding: 48px;
    margin-top: 48px;
    position: relative;
    overflow: hidden;
  }

  .cta-block::before {
    content: '';
    position: absolute;
    top: -40px; right: -40px;
    width: 200px; height: 200px;
    border-radius: 50%;
    background: radial-gradient(circle, rgba(184,134,11,0.15) 0%, transparent 70%);
    pointer-events: none;
  }

  .cta-eyebrow {
    font-family: 'Syne', sans-serif;
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--gold);
    margin-bottom: 14px;
  }

  .cta-title {
    font-family: 'DM Serif Display', serif;
    font-size: 30px;
    color: #F5F3EE;
    margin-bottom: 14px;
    line-height: 1.2;
  }

  .cta-desc {
    font-size: 15px;
    line-height: 1.65;
    color: rgba(245,243,238,0.55);
    margin-bottom: 32px;
    font-weight: 300;
    max-width: 480px;
  }

  .cta-actions {
    display: flex;
    gap: 12px;
    flex-wrap: wrap;
  }

  .btn-cta-primary {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    background: var(--gold);
    color: var(--ink);
    font-family: 'Syne', sans-serif;
    font-size: 12px;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    padding: 15px 28px;
    border: none;
    border-radius: var(--radius);
    cursor: pointer;
    transition: all 0.18s;
  }
  .btn-cta-primary:hover { background: var(--gold-mid); transform: translateY(-1px); }

  .btn-cta-secondary {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    background: transparent;
    color: rgba(245,243,238,0.6);
    font-family: 'Syne', sans-serif;
    font-size: 12px;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    padding: 15px 28px;
    border: 1.5px solid rgba(245,243,238,0.15);
    border-radius: var(--radius);
    cursor: pointer;
    transition: all 0.18s;
  }
  .btn-cta-secondary:hover { border-color: rgba(245,243,238,0.4); color: rgba(245,243,238,0.9); }

  .results-footer {
    text-align: center;
    padding: 32px;
    font-family: 'Syne', sans-serif;
    font-size: 11px;
    color: var(--ink-4);
    letter-spacing: 0.1em;
    text-transform: uppercase;
  }

  /* incomplete warning */
  .incomplete-warning {
    background: #FFF9E8;
    border: 1.5px solid #E6B400;
    border-radius: var(--radius-lg);
    padding: 14px 20px;
    font-size: 13px;
    color: #7A5400;
    display: none;
    margin-bottom: 16px;
  }
  .incomplete-warning.show { display: block; }

  @media (max-width: 680px) {
    .cover-inner { padding: 48px 28px; }
    .diag-header { padding: 16px 24px; }
    .progress-track { display: none; }
    .section-page { padding: 40px 28px 60px; }
    .scale-row { flex-direction: column; align-items: flex-start; gap: 12px; }
    .scale-label { width: auto; }
    .scale-options { justify-content: flex-start; }
    .scale-btn { width: 38px; height: 38px; font-size: 12px; }
    .pillar-results { grid-template-columns: 1fr; }
    .results-hero { padding: 48px 28px 40px; }
    .results-body { padding: 40px 28px 60px; }
    .cta-block { padding: 32px 24px; }
    .cta-actions { flex-direction: column; }
  }
</style>
</head>
<body>

<!-- ═══════════════════════════════ COVER ═══════════════════════════════ -->
<div class="cover" id="cover">
  <div class="cover-grid"></div>
  <div class="cover-accent"></div>
  <div class="cover-inner">
    <div class="cover-eyebrow">SCALEREADY™ · Execution Gap Diagnostic</div>
    <h1 class="cover-title">
      Is Your Organization<br>
      Built to <em>Scale</em>—<br>
      or Just Built to Survive?
    </h1>
    <p class="cover-sub">
      Most hypergrowth companies don't break because of bad strategy. They break because their execution infrastructure never caught up. This 25-question diagnostic pinpoints exactly where your gaps are before they become crises.
    </p>
    <div class="cover-meta">
      <button class="btn-start" onclick="startDiagnostic()">
        Begin Diagnostic
        <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
          <path d="M3 8h10M9 4l4 4-4 4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </button>
      <span class="cover-time">8–10 minutes · 5 pillars · Immediate results</span>
    </div>
  </div>
  <div class="cover-brand">Greater Ambitions LLC · SCALEREADY™</div>
  <div class="cover-bar"></div>
</div>

<!-- ═══════════════════════════════ DIAGNOSTIC ═══════════════════════════════ -->
<div id="diagnostic">
  <header class="diag-header">
    <div class="diag-brand">SCALEREADY™</div>
    <div class="progress-track">
      <div class="progress-labels">
        <span class="progress-label" id="pl-0">Strategy</span>
        <span class="progress-label" id="pl-1">Systems</span>
        <span class="progress-label" id="pl-2">Portfolio</span>
        <span class="progress-label" id="pl-3">Talent</span>
        <span class="progress-label" id="pl-4">Leadership</span>
      </div>
      <div class="progress-bar-wrap">
        <div class="progress-bar-fill" id="progressBar"></div>
      </div>
    </div>
    <div class="diag-step-counter" id="stepCounter">Section 1 of 5</div>
  </header>

  <!-- SECTION 1: Strategy Alignment -->
  <div class="section-page active" id="section-0">
    <div class="section-eyebrow">Pillar 1 · Strategy Alignment</div>
    <h2 class="section-title">Strategic Clarity<br>& Direction</h2>
    <p class="section-desc">Rate each statement based on your organization's current reality — not aspiration. 1 = Strongly disagree, 5 = Strongly agree.</p>

    <div class="question-block">
      <div class="q-number">Question 1 of 25</div>
      <div class="q-text">Our senior leadership team has a <strong>shared, written definition</strong> of what success looks like 12–24 months from now — and every department lead can articulate it without prompting.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q0"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 2 of 25</div>
      <div class="q-text">When new initiatives are proposed, we have a <strong>clear, consistent framework</strong> for deciding which ones align with our growth priorities and which ones are distractions.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q1"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 3 of 25</div>
      <div class="q-text">Our strategic priorities are <strong>actively tracked and reviewed</strong> at least quarterly — and we have a track record of completing what we commit to, not just launching new things.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q2"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 4 of 25</div>
      <div class="q-text">There is <strong>real alignment</strong> between what the CEO/founder says the company's priorities are and how teams actually spend their time and resources week to week.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q3"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 5 of 25</div>
      <div class="q-text">When our strategy needs to shift due to market conditions or new information, our organization can <strong>adapt execution quickly</strong> without losing momentum or falling into confusion.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q4"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="incomplete-warning" id="warn-0">Please answer all questions before continuing.</div>
    <div class="section-nav">
      <span class="nav-hint">5 questions · Pillar 1 of 5</span>
      <button class="btn-nav btn-next" onclick="nextSection(0)">
        Next: Execution Systems
        <svg width="14" height="14" viewBox="0 0 16 16" fill="none"><path d="M3 8h10M9 4l4 4-4 4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
      </button>
    </div>
  </div>

  <!-- SECTION 2: Execution Systems -->
  <div class="section-page" id="section-1">
    <div class="section-eyebrow">Pillar 2 · Execution Systems</div>
    <h2 class="section-title">Operational<br>Infrastructure</h2>
    <p class="section-desc">How well are the systems and processes that power your day-to-day execution actually working at your current growth stage?</p>

    <div class="question-block">
      <div class="q-number">Question 6 of 25</div>
      <div class="q-text">Our core operational processes are <strong>documented, standardized, and consistently followed</strong> — they don't live in one person's head or break down when someone leaves.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q5"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 7 of 25</div>
      <div class="q-text">We have <strong>clear visibility into bottlenecks</strong> in our operations — we know where work slows down, where handoffs break, and where rework is highest, and we're actively addressing these areas.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q6"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 8 of 25</div>
      <div class="q-text">Our <strong>technology and tools stack</strong> supports how we actually work at our current scale — it doesn't require workarounds, shadow spreadsheets, or manual reconciliation to keep things moving.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q7"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 9 of 25</div>
      <div class="q-text">When something goes wrong operationally, we have <strong>reliable mechanisms</strong> to identify root causes and prevent recurrence — not just fix the immediate problem and move on.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q8"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 10 of 25</div>
      <div class="q-text">We could <strong>double our revenue in 18 months</strong> without fundamentally breaking the operational systems we have in place today — they are built to scale, not just survive.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q9"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="incomplete-warning" id="warn-1">Please answer all questions before continuing.</div>
    <div class="section-nav">
      <button class="btn-nav btn-back" onclick="prevSection(1)">
        <svg width="14" height="14" viewBox="0 0 16 16" fill="none"><path d="M13 8H3M7 12l-4-4 4-4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
        Back
      </button>
      <button class="btn-nav btn-next" onclick="nextSection(1)">
        Next: Portfolio Management
        <svg width="14" height="14" viewBox="0 0 16 16" fill="none"><path d="M3 8h10M9 4l4 4-4 4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
      </button>
    </div>
  </div>

  <!-- SECTION 3: Portfolio & Program Management -->
  <div class="section-page" id="section-2">
    <div class="section-eyebrow">Pillar 3 · Portfolio & Program Management</div>
    <h2 class="section-title">Project Portfolio<br>Control</h2>
    <p class="section-desc">How effectively does your organization manage the full portfolio of work in flight — from strategic initiatives to day-to-day projects?</p>

    <div class="question-block">
      <div class="q-number">Question 11 of 25</div>
      <div class="q-text">We have a <strong>single, trusted view</strong> of all active projects and initiatives — leadership can see, at any moment, what's in flight, who owns it, and whether it's on track.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q10"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 12 of 25</div>
      <div class="q-text">Our teams are <strong>not over-committed</strong> — work is prioritized against actual capacity, not aspirational capacity, and people are not stretched thin across too many simultaneous priorities.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q11"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 13 of 25</div>
      <div class="q-text">Projects in our organization are <strong>delivered on time and within scope</strong> more often than not — chronic delays, scope creep, and missed milestones are the exception, not the norm.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q12"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 14 of 25</div>
      <div class="q-text">There is a <strong>clear governance structure</strong> for decision-making across projects — stakeholders know who approves what, escalation paths are defined, and decisions don't get stuck waiting for the right people.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q13"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 15 of 25</div>
      <div class="q-text">When we invest resources in a major initiative, we have a <strong>rigorous process</strong> for measuring whether we got the expected business value — and we apply those learnings to future decisions.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q14"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="incomplete-warning" id="warn-2">Please answer all questions before continuing.</div>
    <div class="section-nav">
      <button class="btn-nav btn-back" onclick="prevSection(2)">
        <svg width="14" height="14" viewBox="0 0 16 16" fill="none"><path d="M13 8H3M7 12l-4-4 4-4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
        Back
      </button>
      <button class="btn-nav btn-next" onclick="nextSection(2)">
        Next: Talent & Teams
        <svg width="14" height="14" viewBox="0 0 16 16" fill="none"><path d="M3 8h10M9 4l4 4-4 4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
      </button>
    </div>
  </div>

  <!-- SECTION 4: Talent & Teams -->
  <div class="section-page" id="section-3">
    <div class="section-eyebrow">Pillar 4 · Talent & Team Design</div>
    <h2 class="section-title">People &<br>Organizational Design</h2>
    <p class="section-desc">Growth exposes talent and structure gaps faster than anything else. How well is your organization designed for where you're going?</p>

    <div class="question-block">
      <div class="q-number">Question 16 of 25</div>
      <div class="q-text">Our <strong>organizational structure</strong> reflects how work actually flows — roles, teams, and reporting lines are designed for current scale, not inherited from when we were half the size.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q15"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 17 of 25</div>
      <div class="q-text">We can <strong>hire and onboard effectively</strong> at the pace our growth requires — new team members are contributing meaningfully within 60–90 days, not still finding their footing at 6 months.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q16"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 18 of 25</div>
      <div class="q-text">Our <strong>high performers are not carrying disproportionate operational burden</strong> — key results don't depend on one or two irreplaceable people who would create a crisis if they left.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q17"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 19 of 25</div>
      <div class="q-text">There is <strong>genuine accountability</strong> for results at every level — underperformance is addressed directly and promptly, and high performance is recognized in ways that reinforce the right behaviors.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q18"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 20 of 25</div>
      <div class="q-text">Our teams have the <strong>skills, training, and tools</strong> they need to operate at our current scale — we're not asking people to grow 3x without giving them the resources or development to do it.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q19"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="incomplete-warning" id="warn-3">Please answer all questions before continuing.</div>
    <div class="section-nav">
      <button class="btn-nav btn-back" onclick="prevSection(3)">
        <svg width="14" height="14" viewBox="0 0 16 16" fill="none"><path d="M13 8H3M7 12l-4-4 4-4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
        Back
      </button>
      <button class="btn-nav btn-next" onclick="nextSection(3)">
        Next: Leadership
        <svg width="14" height="14" viewBox="0 0 16 16" fill="none"><path d="M3 8h10M9 4l4 4-4 4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
      </button>
    </div>
  </div>

  <!-- SECTION 5: Leadership -->
  <div class="section-page" id="section-4">
    <div class="section-eyebrow">Pillar 5 · Leadership Capacity</div>
    <h2 class="section-title">Leadership at<br>Scale</h2>
    <p class="section-desc">The most overlooked execution gap: leadership that hasn't scaled with the organization. This pillar examines whether your leadership infrastructure can carry the company to the next level.</p>

    <div class="question-block">
      <div class="q-number">Question 21 of 25</div>
      <div class="q-text">Our executive and senior leadership team <strong>operates cohesively</strong> — there is genuine trust, constructive conflict, and shared ownership of outcomes across functions, not silos and turf protection.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q20"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 22 of 25</div>
      <div class="q-text">Our <strong>middle management layer</strong> is strong — managers between the senior team and individual contributors are developing their people, driving execution, and making good decisions without constant escalation.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q21"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 23 of 25</div>
      <div class="q-text">The <strong>founders and senior leaders</strong> in this organization have successfully transitioned from doing the work themselves to leading, developing, and multiplying others — and have genuinely let go of the parts they no longer need to own.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q22"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 24 of 25</div>
      <div class="q-text">We have <strong>deliberate leadership development</strong> in place — emerging leaders are being identified, developed, and given stretch opportunities rather than simply promoted based on technical performance.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q23"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="question-block">
      <div class="q-number">Question 25 of 25</div>
      <div class="q-text">Our organization has a <strong>clear view of who the next generation of leaders</strong> will be — and we have an active succession plan that doesn't rely on a single person in any critical seat.</div>
      <div class="scale-row">
        <span class="scale-label">Strongly disagree</span>
        <div class="scale-options" id="q24"></div>
        <span class="scale-label">Strongly agree</span>
      </div>
    </div>

    <div class="incomplete-warning" id="warn-4">Please answer all questions before continuing.</div>
    <div class="section-nav">
      <button class="btn-nav btn-back" onclick="prevSection(4)">
        <svg width="14" height="14" viewBox="0 0 16 16" fill="none"><path d="M13 8H3M7 12l-4-4 4-4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
        Back
      </button>
      <button class="btn-nav btn-next" onclick="goToInfoCapture()">
        See My Results
        <svg width="14" height="14" viewBox="0 0 16 16" fill="none"><path d="M3 8h10M9 4l4 4-4 4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
      </button>
    </div>
  </div>

  <!-- INFO CAPTURE SCREEN -->
  <div class="section-page" id="section-info">
    <div class="section-eyebrow">Almost There</div>
    <h2 class="section-title">Where Should We<br>Send Your Report?</h2>
    <p class="section-desc">Enter your information below to receive your personalized Execution Gap Report. Your results are ready — we just need to know where to send them.</p>

    <div style="display:grid; grid-template-columns:1fr 1fr; gap:16px; margin-bottom:8px;">
      <div>
        <label style="font-family:'Syne',sans-serif; font-size:11px; font-weight:700; letter-spacing:0.1em; text-transform:uppercase; color:var(--ink-3); display:block; margin-bottom:8px;">Full Name <span style="color:var(--red-zone);">*</span></label>
        <input type="text" id="resp-name" placeholder="Jane Smith" style="width:100%; font-family:'DM Sans',sans-serif; padding:14px 16px; border:1.5px solid var(--paper-3); border-radius:var(--radius-lg); background:var(--paper-2); font-size:14px; color:var(--ink); outline:none; transition: border-color 0.2s;" onfocus="this.style.borderColor='var(--teal)'" onblur="this.style.borderColor='var(--paper-3)'">
      </div>
      <div>
        <label style="font-family:'Syne',sans-serif; font-size:11px; font-weight:700; letter-spacing:0.1em; text-transform:uppercase; color:var(--ink-3); display:block; margin-bottom:8px;">Email Address <span style="color:var(--red-zone);">*</span></label>
        <input type="email" id="resp-email" placeholder="jane@company.com" style="width:100%; font-family:'DM Sans',sans-serif; padding:14px 16px; border:1.5px solid var(--paper-3); border-radius:var(--radius-lg); background:var(--paper-2); font-size:14px; color:var(--ink); outline:none; transition: border-color 0.2s;" onfocus="this.style.borderColor='var(--teal)'" onblur="this.style.borderColor='var(--paper-3)'">
      </div>
      <div>
        <label style="font-family:'Syne',sans-serif; font-size:11px; font-weight:700; letter-spacing:0.1em; text-transform:uppercase; color:var(--ink-3); display:block; margin-bottom:8px;">Title / Role</label>
        <input type="text" id="resp-title" placeholder="COO, VP of Operations, etc." style="width:100%; font-family:'DM Sans',sans-serif; padding:14px 16px; border:1.5px solid var(--paper-3); border-radius:var(--radius-lg); background:var(--paper-2); font-size:14px; color:var(--ink); outline:none; transition: border-color 0.2s;" onfocus="this.style.borderColor='var(--teal)'" onblur="this.style.borderColor='var(--paper-3)'">
      </div>
      <div>
        <label style="font-family:'Syne',sans-serif; font-size:11px; font-weight:700; letter-spacing:0.1em; text-transform:uppercase; color:var(--ink-3); display:block; margin-bottom:8px;">Company Name</label>
        <input type="text" id="resp-company" placeholder="Acme Corp" style="width:100%; font-family:'DM Sans',sans-serif; padding:14px 16px; border:1.5px solid var(--paper-3); border-radius:var(--radius-lg); background:var(--paper-2); font-size:14px; color:var(--ink); outline:none; transition: border-color 0.2s;" onfocus="this.style.borderColor='var(--teal)'" onblur="this.style.borderColor='var(--paper-3)'">
      </div>
    </div>

    <div class="incomplete-warning show" id="warn-info" style="display:none !important;">Please enter your name and email address to continue.</div>

    <div class="section-nav">
      <button class="btn-nav btn-back" onclick="backFromInfo()">
        <svg width="14" height="14" viewBox="0 0 16 16" fill="none"><path d="M13 8H3M7 12l-4-4 4-4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
        Back
      </button>
      <button class="btn-nav btn-submit" onclick="calculateResults()">
        View My Results
        <svg width="14" height="14" viewBox="0 0 16 16" fill="none"><path d="M3 8h10M9 4l4 4-4 4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
      </button>
    </div>
  </div>

</div>

<!-- ═══════════════════════════════ RESULTS ═══════════════════════════════ -->
<div id="results">
  <div class="results-hero">
    <div class="results-hero-grid"></div>
    <div class="results-inner">
      <div class="results-eyebrow">SCALEREADY™ · Your Execution Gap Report</div>
      <h2 class="results-headline" id="results-headline">Your Score</h2>
      <div class="score-display">
        <div class="score-circle">
          <div class="score-number" id="total-score">—</div>
          <div class="score-denom">out of 125</div>
        </div>
        <div class="score-zone-info">
          <div class="score-zone-label" id="zone-label" style="color: var(--gold)">—</div>
          <div class="score-zone-desc" id="zone-desc">—</div>
        </div>
      </div>
    </div>
  </div>

  <div class="results-body">
    <div class="pillar-results" id="pillar-results"></div>

    <div class="findings-title">Your Priority Findings</div>
    <div id="findings-list"></div>

    <!-- ═══ RESULTS EXPORT ═══ -->
    <div class="export-section" style="margin-top:48px; margin-bottom:48px;">
      <div class="findings-title">Save & Share Your Results</div>
      <div id="respondent-summary" style="background:var(--paper-2); border:1px solid var(--paper-3); border-radius:var(--radius-lg); padding:16px 20px; margin-bottom:20px; font-size:14px; color:var(--ink-2); line-height:1.8;"></div>

      <div style="display:flex; gap:12px; flex-wrap:wrap;">
        <button onclick="copyResultsToClipboard()" class="btn-nav btn-next" style="background:var(--teal);">
          📋 Copy Full Report to Clipboard
        </button>
        <button onclick="downloadResultsCSV()" class="btn-nav btn-next" style="background:var(--gold); color:var(--ink);">
          📊 Download as CSV
        </button>
        <button onclick="emailResults()" class="btn-nav btn-back" style="border-color:var(--teal); color:var(--teal);">
          ✉ Email Results
        </button>
      </div>
      <div id="copy-confirm" style="display:none; margin-top:12px; padding:12px 16px; background:var(--green-zone-bg); border:1.5px solid var(--green-zone); border-radius:var(--radius-lg); font-size:13px; color:var(--green-zone); font-weight:500;">
        ✓ Results copied to clipboard! Paste into a document, email, or spreadsheet.
      </div>
    </div>

    <div class="cta-block">
      <div class="cta-eyebrow">Your Next Step</div>
      <h3 class="cta-title" id="cta-title">Turn This Diagnostic Into a Roadmap</h3>
      <p class="cta-desc" id="cta-desc">The gaps identified here are exactly what the SCALEREADY Execution Architecture™ workshop is designed to close. Let's build your implementation plan together.</p>
      <div class="cta-actions">
        <button class="btn-cta-primary" onclick="alert('Schedule a SCALEREADY Strategy Session with Dr. Jaye — contact via LinkedIn or scaleready.com')">
          Book a Strategy Session
          <svg width="14" height="14" viewBox="0 0 16 16" fill="none"><path d="M3 8h10M9 4l4 4-4 4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
        </button>
        <button class="btn-cta-secondary" onclick="retakeDiagnostic()">
          Retake Diagnostic
        </button>
      </div>
    </div>
  </div>

  <div class="results-footer">
    SCALEREADY™ · Execution Architecture for Hypergrowth Companies · Dr. Jaye, DBA
  </div>
</div>

<script>
const PILLARS = [
  { name: "Strategy Alignment", questions: [0,1,2,3,4] },
  { name: "Execution Systems", questions: [5,6,7,8,9] },
  { name: "Portfolio Management", questions: [10,11,12,13,14] },
  { name: "Talent & Team Design", questions: [15,16,17,18,19] },
  { name: "Leadership Capacity", questions: [20,21,22,23,24] },
];

const answers = new Array(25).fill(null);
let currentSection = 0;

// Build scale buttons
for (let q = 0; q < 25; q++) {
  const container = document.getElementById('q' + q);
  if (!container) continue;
  for (let v = 1; v <= 5; v++) {
    const btn = document.createElement('button');
    btn.className = 'scale-btn';
    btn.textContent = v;
    btn.dataset.q = q;
    btn.dataset.v = v;
    btn.onclick = function() {
      answers[q] = v;
      container.querySelectorAll('.scale-btn').forEach(b => b.classList.remove('selected'));
      btn.classList.add('selected');
    };
    container.appendChild(btn);
  }
}

function startDiagnostic() {
  document.getElementById('cover').style.display = 'none';
  document.getElementById('diagnostic').style.display = 'block';
  updateProgress(0);
}

function updateProgress(sectionIdx) {
  const fill = ((sectionIdx) / 5) * 100;
  document.getElementById('progressBar').style.width = fill + '%';
  document.getElementById('stepCounter').textContent = 'Section ' + (sectionIdx + 1) + ' of 5';
  document.querySelectorAll('.progress-label').forEach((el, i) => {
    el.classList.toggle('active', i === sectionIdx);
  });
}

function sectionAnswered(sectionIdx) {
  return PILLARS[sectionIdx].questions.every(q => answers[q] !== null);
}

function nextSection(fromIdx) {
  const warn = document.getElementById('warn-' + fromIdx);
  if (!sectionAnswered(fromIdx)) {
    warn.classList.add('show');
    return;
  }
  warn.classList.remove('show');
  document.getElementById('section-' + fromIdx).classList.remove('active');
  currentSection = fromIdx + 1;
  document.getElementById('section-' + currentSection).classList.add('active');
  updateProgress(currentSection);
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

function prevSection(fromIdx) {
  document.getElementById('section-' + fromIdx).classList.remove('active');
  currentSection = fromIdx - 1;
  document.getElementById('section-' + currentSection).classList.add('active');
  updateProgress(currentSection);
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

function getPillarScore(pillarIdx) {
  return PILLARS[pillarIdx].questions.reduce((sum, q) => sum + (answers[q] || 0), 0);
}

function getStatus(score, max) {
  const pct = score / max;
  if (pct <= 0.5) return 'critical';
  if (pct <= 0.74) return 'developing';
  return 'strong';
}

function goToInfoCapture() {
  const warn = document.getElementById('warn-4');
  if (!sectionAnswered(4)) {
    warn.classList.add('show');
    return;
  }
  warn.classList.remove('show');
  document.getElementById('section-4').classList.remove('active');
  document.getElementById('section-info').classList.add('active');
  document.getElementById('progressBar').style.width = '100%';
  document.getElementById('stepCounter').textContent = 'Final Step';
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

function backFromInfo() {
  document.getElementById('section-info').classList.remove('active');
  document.getElementById('section-4').classList.add('active');
  updateProgress(4);
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

function calculateResults() {
  const name = document.getElementById('resp-name').value.trim();
  const email = document.getElementById('resp-email').value.trim();
  const warnInfo = document.getElementById('warn-info');

  if (!name || !email) {
    warnInfo.style.display = 'block';
    warnInfo.style.setProperty('display', 'block', 'important');
    return;
  }
  warnInfo.style.setProperty('display', 'none', 'important');

  document.getElementById('diagnostic').style.display = 'none';
  document.getElementById('results').style.display = 'block';
  window.scrollTo({ top: 0, behavior: 'smooth' });

  const total = answers.reduce((s, v) => s + (v || 0), 0);

  // Populate respondent summary in results
  const title = document.getElementById('resp-title').value.trim();
  const company = document.getElementById('resp-company').value.trim();
  const summaryEl = document.getElementById('respondent-summary');
  if (summaryEl) {
    summaryEl.innerHTML = `<strong>${name}</strong>${title ? ' &nbsp;·&nbsp; ' + title : ''}${company ? ' &nbsp;·&nbsp; ' + company : ''} &nbsp;·&nbsp; ${email}`;
  }

  // Auto-send full report to admin on completion
  sendAdminEmail();
  document.getElementById('total-score').textContent = total;

  // Zone
  let headline, zoneLabel, zoneDesc, zoneColor, ctaTitle, ctaDesc;
  if (total <= 62) {
    zoneLabel = "Critical Execution Risk";
    zoneColor = "#C0392B";
    headline = "Significant Execution\nGaps Detected";
    zoneDesc = "Your organization is operating with execution infrastructure that wasn't built for your current scale. The gaps identified here are likely already costing you in delayed projects, leadership friction, and strategic drift. Immediate intervention is recommended.";
    ctaTitle = "Your Execution Infrastructure Needs a Rebuild — Now";
    ctaDesc = "Companies at this risk level are typically 6–12 months away from a major operational crisis. The SCALEREADY Execution Architecture™ workshop and fractional engagement model is designed specifically for this stage. Let's build your intervention plan together.";
  } else if (total <= 93) {
    zoneLabel = "Developing — Scale Risk Present";
    zoneColor = "#D97706";
    headline = "Foundational Strengths,\nGrowth-Stage Gaps";
    zoneDesc = "You have real operational capability, but specific gaps are emerging as your organization grows. Left unaddressed, these developing weaknesses become critical problems at the next stage of scale. You're in the ideal window to act.";
    ctaTitle = "Close the Gaps Before They Close You";
    ctaDesc = "Companies in the developing zone have a 60–90 day window to build the execution infrastructure they'll need at the next growth stage. The SCALEREADY workshop series is built for this exact moment. Let's map your intervention priorities.";
  } else {
    zoneLabel = "Strong — Optimize for Scale";
    zoneColor = "#52211A";
    headline = "Strong Execution\nFoundation";
    zoneDesc = "Your organization has built meaningful execution infrastructure. The opportunity now is optimization and scaling — ensuring your systems, talent, and leadership can carry you through the next order-of-magnitude growth without breaking.";
    ctaTitle = "From Strong to Scale-Ready: The Optimization Opportunity";
    ctaDesc = "Organizations with strong foundations often hit invisible ceilings in the next growth phase. The SCALEREADY advanced workshop series helps high-performing teams build the execution architecture that sustains excellence at 2–10x current scale.";
  }

  document.getElementById('results-headline').textContent = headline;
  document.getElementById('zone-label').textContent = zoneLabel;
  document.getElementById('zone-label').style.color = zoneColor;
  document.getElementById('zone-desc').textContent = zoneDesc;
  document.getElementById('cta-title').textContent = ctaTitle;
  document.getElementById('cta-desc').textContent = ctaDesc;

  // Pillar breakdown
  const pillarContainer = document.getElementById('pillar-results');
  pillarContainer.innerHTML = '';
  PILLARS.forEach((p, i) => {
    const score = getPillarScore(i);
    const status = getStatus(score, 25);
    const pct = Math.round((score / 25) * 100);
    const barColor = status === 'critical' ? '#C0392B' : status === 'developing' ? '#E67E22' : 'var(--teal)';
    const statusLabel = status === 'critical' ? '⚠ Critical gap' : status === 'developing' ? '◎ Developing' : '✓ Strong';
    const statusClass = 'status-' + status;
    const card = document.createElement('div');
    card.className = 'pillar-card';
    card.innerHTML = `
      <div class="pillar-card-name">${p.name}</div>
      <div class="pillar-score-row">
        <span class="pillar-score-val">${score}</span>
        <span class="pillar-score-max">/ 25</span>
      </div>
      <div class="pillar-bar-wrap">
        <div class="pillar-bar-fill" style="width:0%; background:${barColor}" data-pct="${pct}"></div>
      </div>
      <div class="pillar-status ${statusClass}">${statusLabel} · ${pct}%</div>
    `;
    pillarContainer.appendChild(card);
  });

  // Animate bars after DOM inserts
  setTimeout(() => {
    document.querySelectorAll('.pillar-bar-fill').forEach(bar => {
      bar.style.width = bar.dataset.pct + '%';
    });
  }, 80);

  // Findings
  const findings = document.getElementById('findings-list');
  findings.innerHTML = '';

  const sorted = PILLARS.map((p, i) => ({ name: p.name, score: getPillarScore(i) }))
                        .sort((a, b) => a.score - b.score);

  const worstTwo = sorted.slice(0, 2);
  const bestOne = sorted[sorted.length - 1];

  worstTwo.forEach(p => {
    const s = getStatus(p.score, 25);
    const cls = s === 'critical' ? 'finding-critical' : 'finding-developing';
    const icon = s === 'critical' ? '⚠' : '◎';
    const label = s === 'critical' ? 'Critical Gap' : 'Development Priority';
    const insight = getInsight(p.name, p.score);
    const el = document.createElement('div');
    el.className = 'finding-item ' + cls;
    el.innerHTML = `<div class="finding-icon">${icon}</div><div class="finding-text"><div class="finding-label">${label} · ${p.name}</div><div class="finding-desc">${insight}</div></div>`;
    findings.appendChild(el);
  });

  const strengthEl = document.createElement('div');
  strengthEl.className = 'finding-item finding-strong';
  strengthEl.innerHTML = `<div class="finding-icon">✓</div><div class="finding-text"><div class="finding-label">Strength · ${bestOne.name}</div><div class="finding-desc">${getStrength(bestOne.name)}</div></div>`;
  findings.appendChild(strengthEl);
}

// ═══════════════════════════════════════
// AUTO-EMAIL TO ADMIN ON COMPLETION
// ═══════════════════════════════════════

async function sendAdminEmail() {
  const report = buildResultsText();
  const name = document.getElementById('resp-name').value || '[Not provided]';
  const title = document.getElementById('resp-title').value || '[Not provided]';
  const company = document.getElementById('resp-company').value || '[Not provided]';
  const email = document.getElementById('resp-email').value || '[Not provided]';
  const total = answers.reduce((s, v) => s + (v || 0), 0);

  let zone;
  if (total <= 62) zone = "Critical Execution Risk";
  else if (total <= 93) zone = "Developing — Scale Risk Present";
  else zone = "Strong — Optimize for Scale";

  const subject = `SCALEREADY Diagnostic — ${name} | ${company} | ${total}/125 (${zone})`;

  let pillarLines = '';
  PILLARS.forEach((p, i) => {
    const score = getPillarScore(i);
    const pct = Math.round((score / 25) * 100);
    const status = getStatus(score, 25);
    const statusLabel = status === 'critical' ? 'CRITICAL' : status === 'developing' ? 'DEVELOPING' : 'STRONG';
    pillarLines += `${p.name}: ${score}/25 (${pct}%) [${statusLabel}]\n`;
  });

  const plainText = `SCALEREADY™ DIAGNOSTIC — NEW SUBMISSION
==========================================
Name:    ${name}
Title:   ${title}
Company: ${company}
Email:   ${email}

OVERALL SCORE: ${total}/125 — ${zone}

PILLAR BREAKDOWN:
${pillarLines}
------------------------------------------
FULL QUESTION-LEVEL REPORT:
${report}`;

  try {
    await fetch('https://api.web3forms.com/submit', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json', 'Accept': 'application/json' },
      body: JSON.stringify({
        access_key: 'e8f9c565-7687-453b-acc7-2d3fdea5bc6d',
        subject: subject,
        from_name: 'SCALEREADY Diagnostic Tool',
        replyto: email !== '[Not provided]' ? email : 'noreply@greaterambitionsllc.com',
        message: plainText,
      })
    });
  } catch(e) {
    console.warn('Admin email notification could not be sent:', e);
  }
}

// ═══════════════════════════════════════
// EXPORT FUNCTIONS
// ═══════════════════════════════════════

const QUESTION_TEXTS = [
  "Shared, written definition of success 12-24 months out",
  "Clear framework for deciding which initiatives align with growth priorities",
  "Strategic priorities actively tracked and reviewed quarterly",
  "Real alignment between stated priorities and how teams spend time",
  "Organization can adapt execution quickly when strategy shifts",
  "Core operational processes documented, standardized, and consistently followed",
  "Clear visibility into bottlenecks in operations",
  "Technology and tools stack supports current scale without workarounds",
  "Reliable mechanisms to identify root causes and prevent recurrence",
  "Could double revenue in 18 months without breaking operational systems",
  "Single, trusted view of all active projects and initiatives",
  "Teams are not over-committed; work prioritized against actual capacity",
  "Projects delivered on time and within scope more often than not",
  "Clear governance structure for decision-making across projects",
  "Rigorous process for measuring whether initiatives delivered expected value",
  "Organizational structure reflects how work actually flows",
  "Can hire and onboard effectively at pace growth requires",
  "High performers not carrying disproportionate operational burden",
  "Genuine accountability for results at every level",
  "Teams have skills, training, and tools needed for current scale",
  "Executive/senior leadership team operates cohesively across functions",
  "Middle management layer is strong and driving execution",
  "Founders/senior leaders transitioned from doing to leading and multiplying",
  "Deliberate leadership development in place for emerging leaders",
  "Clear view of next-generation leaders with active succession plan",
];

function buildResultsText() {
  const name = document.getElementById('resp-name').value || '[Not provided]';
  const title = document.getElementById('resp-title').value || '[Not provided]';
  const company = document.getElementById('resp-company').value || '[Not provided]';
  const email = document.getElementById('resp-email').value || '[Not provided]';
  const total = answers.reduce((s, v) => s + (v || 0), 0);
  const date = new Date().toLocaleDateString('en-US', { year:'numeric', month:'long', day:'numeric' });

  let zoneLabel;
  if (total <= 62) zoneLabel = "Critical Execution Risk";
  else if (total <= 93) zoneLabel = "Developing — Scale Risk Present";
  else zoneLabel = "Strong — Optimize for Scale";

  let report = `SCALEREADY™ EXECUTION GAP DIAGNOSTIC — RESULTS REPORT\n`;
  report += `${'═'.repeat(60)}\n\n`;
  report += `Respondent: ${name}\n`;
  report += `Title: ${title}\n`;
  report += `Company: ${company}\n`;
  report += `Email: ${email}\n`;
  report += `Date: ${date}\n\n`;
  report += `${'─'.repeat(60)}\n`;
  report += `OVERALL SCORE: ${total} / 125 — ${zoneLabel}\n`;
  report += `${'─'.repeat(60)}\n\n`;

  report += `PILLAR SCORES\n`;
  report += `${'─'.repeat(40)}\n`;
  PILLARS.forEach((p, i) => {
    const score = getPillarScore(i);
    const pct = Math.round((score / 25) * 100);
    const status = getStatus(score, 25);
    const statusLabel = status === 'critical' ? 'CRITICAL' : status === 'developing' ? 'DEVELOPING' : 'STRONG';
    report += `${p.name.padEnd(24)} ${String(score).padStart(2)}/25  (${String(pct).padStart(3)}%)  [${statusLabel}]\n`;
  });

  report += `\n\nQUESTION-LEVEL DETAIL\n`;
  report += `${'─'.repeat(60)}\n`;
  PILLARS.forEach((p, pi) => {
    report += `\n${p.name.toUpperCase()}\n`;
    p.questions.forEach((q, qi) => {
      const score = answers[q] || 0;
      const bar = '█'.repeat(score) + '░'.repeat(5 - score);
      report += `  Q${q+1}. ${QUESTION_TEXTS[q]}\n`;
      report += `      Score: ${score}/5  ${bar}\n\n`;
    });
  });

  report += `\nPRIORITY FINDINGS\n`;
  report += `${'─'.repeat(60)}\n`;
  const sorted = PILLARS.map((p, i) => ({ name: p.name, score: getPillarScore(i) }))
                        .sort((a, b) => a.score - b.score);
  const worstTwo = sorted.slice(0, 2);
  const bestOne = sorted[sorted.length - 1];

  worstTwo.forEach(p => {
    const s = getStatus(p.score, 25);
    const label = s === 'critical' ? 'CRITICAL GAP' : 'DEVELOPMENT PRIORITY';
    report += `\n⚠ ${label}: ${p.name} (${p.score}/25)\n`;
    report += `  ${getInsight(p.name, p.score)}\n`;
  });

  report += `\n✓ STRENGTH: ${bestOne.name} (${bestOne.score}/25)\n`;
  report += `  ${getStrength(bestOne.name)}\n`;

  report += `\n${'═'.repeat(60)}\n`;
  report += `Generated by SCALEREADY™ Execution Gap Diagnostic\n`;
  report += `Dr. Minina Johnson, DBA, PMP · Greater Ambitions LLC\n`;
  report += `${'═'.repeat(60)}\n`;

  return report;
}

function copyResultsToClipboard() {
  const report = buildResultsText();
  navigator.clipboard.writeText(report).then(() => {
    const confirm = document.getElementById('copy-confirm');
    confirm.style.display = 'block';
    setTimeout(() => { confirm.style.display = 'none'; }, 4000);
  }).catch(() => {
    // Fallback for older browsers
    const ta = document.createElement('textarea');
    ta.value = report;
    document.body.appendChild(ta);
    ta.select();
    document.execCommand('copy');
    document.body.removeChild(ta);
    const confirm = document.getElementById('copy-confirm');
    confirm.style.display = 'block';
    setTimeout(() => { confirm.style.display = 'none'; }, 4000);
  });
}

function downloadResultsCSV() {
  const name = document.getElementById('resp-name').value || 'Unknown';
  const title = document.getElementById('resp-title').value || '';
  const company = document.getElementById('resp-company').value || '';
  const email = document.getElementById('resp-email').value || '';
  const total = answers.reduce((s, v) => s + (v || 0), 0);
  const date = new Date().toISOString().split('T')[0];

  let csv = 'SCALEREADY Execution Gap Diagnostic — Results Export\n\n';
  csv += 'Respondent Information\n';
  csv += `Name,Title,Company,Email,Date,Total Score,Zone\n`;

  let zone;
  if (total <= 62) zone = "Critical";
  else if (total <= 93) zone = "Developing";
  else zone = "Strong";

  csv += `"${name}","${title}","${company}","${email}","${date}",${total},"${zone}"\n\n`;

  csv += 'Pillar Scores\n';
  csv += 'Pillar,Score,Max,Percentage,Status\n';
  PILLARS.forEach((p, i) => {
    const score = getPillarScore(i);
    const pct = Math.round((score / 25) * 100);
    const status = getStatus(score, 25);
    csv += `"${p.name}",${score},25,${pct}%,"${status}"\n`;
  });

  csv += '\nQuestion-Level Detail\n';
  csv += 'Question #,Pillar,Question Text,Score\n';
  PILLARS.forEach((p) => {
    p.questions.forEach((q) => {
      csv += `${q+1},"${p.name}","${QUESTION_TEXTS[q]}",${answers[q] || 0}\n`;
    });
  });

  const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = `ScaleReady_Diagnostic_${company.replace(/[^a-zA-Z0-9]/g,'_') || 'Results'}_${date}.csv`;
  document.body.appendChild(a);
  a.click();
  document.body.removeChild(a);
  URL.revokeObjectURL(url);
}

function emailResults() {
  const name = document.getElementById('resp-name').value || '';
  const company = document.getElementById('resp-company').value || '';
  const total = answers.reduce((s, v) => s + (v || 0), 0);

  let zone;
  if (total <= 62) zone = "Critical Execution Risk";
  else if (total <= 93) zone = "Developing — Scale Risk Present";
  else zone = "Strong — Optimize for Scale";

  let pillarSummary = '';
  PILLARS.forEach((p, i) => {
    const score = getPillarScore(i);
    const status = getStatus(score, 25);
    const statusLabel = status === 'critical' ? 'CRITICAL' : status === 'developing' ? 'DEVELOPING' : 'STRONG';
    pillarSummary += `${p.name}: ${score}/25 [${statusLabel}]%0D%0A`;
  });

  const subject = encodeURIComponent(`SCALEREADY Diagnostic Results — ${name || 'New Respondent'} (${company || 'Company'})`);
  const body = `SCALEREADY Execution Gap Diagnostic Results%0D%0A%0D%0ARespondent: ${encodeURIComponent(name)}%0D%0ACompany: ${encodeURIComponent(company)}%0D%0AOverall Score: ${total}/125 — ${encodeURIComponent(zone)}%0D%0A%0D%0APILLAR SCORES:%0D%0A${pillarSummary}%0D%0A(Use "Copy Full Report to Clipboard" for the complete question-level detail, then paste below or attach as a document.)%0D%0A%0D%0A----%0D%0AGenerated by SCALEREADY™ Execution Gap Diagnostic`;

  window.location.href = `mailto:?subject=${subject}&body=${body}`;
}

function retakeDiagnostic() {
  answers.fill(null);
  document.querySelectorAll('.scale-btn').forEach(b => b.classList.remove('selected'));
  // Reset info fields
  ['resp-name','resp-title','resp-company','resp-email'].forEach(id => {
    const el = document.getElementById(id);
    if (el) el.value = '';
  });
  // Reset info screen back to hidden
  document.getElementById('section-info').classList.remove('active');
  // Reset all sections
  for (let i = 0; i < 5; i++) {
    document.getElementById('section-' + i).classList.remove('active');
  }
  document.getElementById('section-0').classList.add('active');
  document.getElementById('results').style.display = 'none';
  document.getElementById('cover').style.display = 'flex';
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

function getInsight(pillar, score) {
  const insights = {
    "Strategy Alignment": `Your strategy isn't landing at the execution layer. There's likely a gap between what leadership intends and what teams are actually prioritizing. Without alignment infrastructure, growth adds friction, not momentum.`,
    "Execution Systems": `Your operational infrastructure isn't built for your current scale. Processes that worked at half your size are now creating drag — workarounds, rework, and single points of failure are signs this pillar needs urgent attention.`,
    "Portfolio Management": `Your organization is likely over-committed and under-visible. Too many initiatives, unclear ownership, and chronic project delays are symptoms of a portfolio management gap that compounds at every growth stage.`,
    "Talent & Team Design": `Your organizational structure and talent systems aren't keeping pace with growth. Key-person dependency, slow onboarding, and unclear accountability are creating invisible drag on your execution velocity.`,
    "Leadership Capacity": `Your leadership infrastructure is the bottleneck. As the organization grows, leaders must multiply capacity — not just work harder. Gaps here create cascading execution failures across every other pillar.`,
  };
  return insights[pillar] || "This pillar requires focused development as you scale.";
}

function getStrength(pillar) {
  const strengths = {
    "Strategy Alignment": "You have meaningful strategic clarity at the leadership level. This is a genuine differentiator — build on it by connecting your strategy more tightly to execution at the team level.",
    "Execution Systems": "Your operational systems are a relative strength. Protect this advantage as you scale by investing in documentation and process infrastructure before the next growth phase.",
    "Portfolio Management": "Your portfolio management capability is ahead of many organizations at your stage. This gives you better visibility into where to invest and where to pull back.",
    "Talent & Team Design": "Your organizational design and talent systems show real strength. Continue investing in deliberate structure and accountability to sustain this as headcount grows.",
    "Leadership Capacity": "Your leadership team is a genuine asset. Continue developing the layer below senior leadership to ensure execution strength compounds as the organization scales.",
  };
  return strengths[pillar] || "This pillar represents a genuine organizational strength — protect and leverage it.";
}

</script>
</body>
</html>
