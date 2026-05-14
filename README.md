<!DOCTYPE html>

<html lang="uz">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
<meta name="color-scheme" content="light dark">
<title>Lumen — Salomatlik</title>
<script src="https://telegram.org/js/telegram-web-app.js"></script>
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=Nunito:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #F5F1EC;          /* warm sand */
    --surface: #FDFCFA;     /* card surface */
    --surface-2: #EFE9E1;   /* secondary surface */
    --ink: #2A2823;         /* primary text */
    --ink-2: #6B6760;       /* secondary text */
    --ink-3: #9A958C;       /* tertiary text */
    --accent: #7C6B8E;      /* muted lavender — primary brand */
    --accent-soft: #D9CFE3; /* lavender wash */
    --sage: #8FA68E;        /* sage — secondary */
    --sage-soft: #D6DFD3;
    --terracotta: #C58A6F;  /* warm accent for period */
    --terracotta-soft: #ECD6CB;
    --ocean: #6B8A9E;       /* sleep blue */
    --ocean-soft: #CDDAE2;
    --line: #E5DFD5;
    --shadow: 0 1px 2px rgba(42, 40, 35, 0.04), 0 4px 12px rgba(42, 40, 35, 0.04);
    --shadow-lg: 0 4px 24px rgba(42, 40, 35, 0.08);
    --radius: 16px;
    --radius-sm: 10px;
  }

- { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }

html, body {
background: linear-gradient(140deg, #EDE6DA 0%, #F5F1EC 40%, #E8DFE6 100%);
min-height: 100vh;
font-family: ‘Nunito’, -apple-system, sans-serif;
font-weight: 500;
color: var(–ink);
-webkit-font-smoothing: antialiased;
overflow-x: hidden;
letter-spacing: -0.005em;
}

/* Telegram WebView — use full container, no phone frame */
.demo-wrap {
min-height: 100vh;
min-height: 100dvh;
display: flex;
flex-direction: column;
}

.demo-info { display: none; }

/* Phone frame becomes the full container in Telegram */
.phone-frame {
width: 100%;
height: 100vh;
height: 100dvh;
background: transparent;
border-radius: 0;
padding: 0;
box-shadow: none;
position: relative;
}
.phone-screen {
width: 100%;
height: 100%;
background: var(–bg);
border-radius: 0;
overflow: hidden;
position: relative;
}

/* Status bar — hidden in Telegram (Telegram provides its own) */
.status-bar {
display: none;
}

/* Screen container */
.screen {
position: absolute;
top: 0; left: 0; right: 0; bottom: 0;
display: flex;
flex-direction: column;
opacity: 0;
pointer-events: none;
transition: opacity 0.35s ease;
background: var(–bg);
}
.screen.active {
opacity: 1;
pointer-events: auto;
z-index: 10;
}

/* Onboarding common */
.ob-content {
flex: 1;
padding: 32px 28px 24px;
display: flex;
flex-direction: column;
overflow-y: auto;
}
.ob-skip {
position: absolute;
top: 56px;
right: 24px;
background: none;
border: none;
color: var(–ink-3);
font-size: 13px;
font-family: ‘Nunito’;
cursor: pointer;
padding: 8px;
}
.ob-back {
position: absolute;
top: 56px;
left: 24px;
background: none;
border: none;
color: var(–ink-2);
font-size: 20px;
cursor: pointer;
padding: 4px 8px;
}
.ob-progress {
height: 3px;
background: var(–line);
margin: 12px 28px 0;
border-radius: 100px;
overflow: hidden;
}
.ob-progress-fill {
height: 100%;
background: var(–accent);
border-radius: 100px;
transition: width 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}
.ob-title {
font-family: ‘DM Serif Display’, serif;
font-weight: 400;
font-size: 28px;
line-height: 1.2;
color: var(–ink);
margin-top: 40px;
margin-bottom: 12px;
letter-spacing: -0.5px;
}
.ob-title em {
font-style: italic;
color: var(–accent);
}
.ob-sub {
font-size: 14px;
font-weight: 500;
line-height: 1.65;
color: var(–ink-2);
margin-bottom: 28px;
}
.ob-spacer { flex: 1; }
.ob-cta-area {
padding: 16px 28px 32px;
}
.btn-primary {
width: 100%;
height: 52px;
background: var(–ink);
color: var(–bg);
border: none;
border-radius: 100px;
font-family: ‘Nunito’;
font-size: 15px;
font-weight: 500;
letter-spacing: 0.2px;
cursor: pointer;
transition: transform 0.15s ease, background 0.2s ease;
}
.btn-primary:hover { background: var(–accent); }
.btn-primary:active { transform: scale(0.98); }
.btn-secondary {
width: 100%;
height: 52px;
background: transparent;
color: var(–ink-2);
border: none;
border-radius: 100px;
font-family: ‘Nunito’;
font-size: 14px;
font-weight: 500;
cursor: pointer;
margin-top: 4px;
}

/* Splash */
#screen-splash { background: linear-gradient(160deg, #EDE6DA 0%, #E8DFE6 60%, #D9CFE3 100%); }
.splash-inner {
flex: 1;
display: flex;
flex-direction: column;
justify-content: center;
align-items: center;
padding: 40px;
}
.splash-mark {
font-family: ‘DM Serif Display’, serif;
font-size: 64px;
font-weight: 300;
font-style: italic;
color: var(–accent);
letter-spacing: -2px;
margin-bottom: 8px;
animation: fadeUp 1s ease;
}
.splash-tag {
font-family: ‘Nunito’;
font-size: 12px;
color: var(–ink-2);
letter-spacing: 4px;
text-transform: uppercase;
animation: fadeUp 1s ease 0.2s both;
}
@keyframes fadeUp {
from { opacity: 0; transform: translateY(20px); }
to { opacity: 1; transform: translateY(0); }
}

/* Privacy screen */
.privacy-icon {
width: 64px;
height: 64px;
margin-bottom: 24px;
background: var(–accent-soft);
border-radius: 100px;
display: flex;
align-items: center;
justify-content: center;
}
.privacy-list {
list-style: none;
margin-top: 8px;
}
.privacy-list li {
padding: 14px 0;
border-bottom: 1px solid var(–line);
font-size: 14px;
color: var(–ink);
display: flex;
gap: 14px;
align-items: flex-start;
}
.privacy-list li:last-child { border-bottom: none; }
.privacy-list .check {
color: var(–sage);
flex-shrink: 0;
margin-top: 2px;
}

/* Name input */
.name-input {
width: 100%;
height: 56px;
border: 1px solid var(–line);
border-radius: 14px;
background: var(–surface);
padding: 0 20px;
font-family: ‘DM Serif Display’, serif;
font-size: 22px;
color: var(–ink);
outline: none;
transition: border-color 0.2s ease;
}
.name-input:focus { border-color: var(–accent); }
.name-input::placeholder { color: var(–ink-3); font-style: italic; }

/* Mini calendar for date picker */
.mini-cal {
background: var(–surface);
border-radius: var(–radius);
padding: 20px;
border: 1px solid var(–line);
}
.mini-cal-header {
display: flex;
justify-content: space-between;
align-items: center;
margin-bottom: 16px;
}
.mini-cal-month {
font-family: ‘DM Serif Display’, serif;
font-size: 17px;
color: var(–ink);
}
.mini-cal-nav {
background: none;
border: none;
color: var(–ink-2);
font-size: 18px;
cursor: pointer;
padding: 4px 10px;
}
.mini-cal-grid {
display: grid;
grid-template-columns: repeat(7, 1fr);
gap: 4px;
}
.mini-cal-dow {
font-size: 10px;
color: var(–ink-3);
text-align: center;
padding: 6px 0;
text-transform: uppercase;
letter-spacing: 1px;
}
.mini-cal-day {
aspect-ratio: 1;
display: flex;
align-items: center;
justify-content: center;
font-size: 13px;
color: var(–ink);
border-radius: 100px;
cursor: pointer;
transition: background 0.15s ease, color 0.15s ease;
}
.mini-cal-day:hover { background: var(–surface-2); }
.mini-cal-day.muted { color: var(–ink-3); opacity: 0.4; }
.mini-cal-day.selected {
background: var(–terracotta);
color: white;
font-weight: 600;
}
.mini-cal-day.today {
box-shadow: inset 0 0 0 1px var(–accent);
color: var(–accent);
font-weight: 600;
}

/* Cycle length picker */
.number-picker {
display: flex;
align-items: center;
justify-content: center;
gap: 24px;
background: var(–surface);
border-radius: var(–radius);
padding: 28px 20px;
border: 1px solid var(–line);
}
.np-btn {
width: 48px;
height: 48px;
border-radius: 100px;
background: var(–surface-2);
border: none;
font-size: 24px;
color: var(–ink);
cursor: pointer;
display: flex;
align-items: center;
justify-content: center;
transition: background 0.15s ease;
}
.np-btn:hover { background: var(–accent-soft); }
.np-value {
font-family: ‘DM Serif Display’, serif;
font-size: 56px;
font-weight: 300;
color: var(–ink);
min-width: 80px;
text-align: center;
line-height: 1;
}
.np-unit {
font-family: ‘Nunito’;
font-size: 12px;
color: var(–ink-3);
text-transform: uppercase;
letter-spacing: 2px;
margin-top: 8px;
text-align: center;
}
.np-link {
display: block;
text-align: center;
margin-top: 18px;
background: none;
border: none;
color: var(–accent);
font-size: 13px;
cursor: pointer;
text-decoration: underline;
text-underline-offset: 3px;
}

/* ========================
MAIN APP — Calendar screen
======================== */
.app-header {
padding: 16px 24px 12px;
display: flex;
justify-content: space-between;
align-items: flex-end;
}
.greeting-block .greeting-pre {
font-size: 11px;
color: var(–ink-3);
text-transform: uppercase;
letter-spacing: 2px;
margin-bottom: 4px;
}
.greeting-block .greeting-name {
font-family: ‘DM Serif Display’, serif;
font-size: 26px;
font-weight: 400;
color: var(–ink);
line-height: 1.1;
}
.greeting-block .greeting-name em {
font-style: italic;
color: var(–accent);
}
.avatar {
width: 40px;
height: 40px;
border-radius: 100px;
background: linear-gradient(135deg, var(–accent), var(–terracotta));
color: white;
display: flex;
align-items: center;
justify-content: center;
font-family: ‘DM Serif Display’, serif;
font-size: 18px;
flex-shrink: 0;
}

/* Phase card — today’s status */
.status-card {
margin: 8px 20px 16px;
background: var(–surface);
border-radius: 20px;
padding: 20px;
border: 1px solid var(–line);
box-shadow: var(–shadow);
}
.status-top {
display: flex;
justify-content: space-between;
align-items: flex-start;
margin-bottom: 14px;
}
.status-phase {
font-size: 11px;
text-transform: uppercase;
letter-spacing: 2px;
color: var(–terracotta);
font-weight: 600;
}
.status-day {
font-family: ‘DM Serif Display’, serif;
font-size: 13px;
font-style: italic;
color: var(–ink-2);
}
.status-title {
font-family: ‘DM Serif Display’, serif;
font-size: 26px;
font-weight: 400;
line-height: 1.2;
color: var(–ink);
margin-bottom: 8px;
}
.status-meta {
font-size: 13px;
color: var(–ink-2);
line-height: 1.5;
}

/* Calendar */
.cal-section {
padding: 0 20px;
margin-bottom: 16px;
}
.cal-header {
display: flex;
justify-content: space-between;
align-items: center;
margin-bottom: 14px;
padding: 0 4px;
}
.cal-month {
font-family: ‘DM Serif Display’, serif;
font-size: 17px;
color: var(–ink);
}
.cal-nav {
display: flex;
gap: 4px;
}
.cal-nav button {
width: 30px;
height: 30px;
border-radius: 100px;
border: 1px solid var(–line);
background: transparent;
color: var(–ink-2);
cursor: pointer;
display: flex;
align-items: center;
justify-content: center;
}
.cal-grid {
display: grid;
grid-template-columns: repeat(7, 1fr);
gap: 6px;
}
.cal-dow {
font-size: 10px;
color: var(–ink-3);
text-align: center;
padding: 4px 0;
text-transform: uppercase;
letter-spacing: 1px;
}
.cal-day {
aspect-ratio: 1;
display: flex;
align-items: center;
justify-content: center;
font-size: 13px;
color: var(–ink);
border-radius: 12px;
cursor: pointer;
position: relative;
background: transparent;
border: none;
font-family: ‘Nunito’;
transition: background 0.15s ease;
}
.cal-day:hover:not(.muted) { background: var(–surface); }
.cal-day.muted { color: var(–ink-3); opacity: 0.3; cursor: default; }
.cal-day.period {
background: var(–terracotta);
color: white;
font-weight: 600;
}
.cal-day.predicted {
background: var(–terracotta-soft);
color: var(–terracotta);
font-weight: 500;
}
.cal-day.today {
box-shadow: inset 0 0 0 1.5px var(–accent);
font-weight: 600;
color: var(–accent);
}
.cal-day.today.period { color: white; box-shadow: inset 0 0 0 1.5px white; }
.cal-day .dot {
position: absolute;
bottom: 4px;
left: 50%;
transform: translateX(-50%);
width: 4px;
height: 4px;
border-radius: 100px;
background: var(–sage);
}

/* Legend */
.cal-legend {
display: flex;
gap: 16px;
padding: 14px 4px 4px;
font-size: 11px;
color: var(–ink-2);
}
.cal-legend span { display: flex; align-items: center; gap: 6px; }
.cal-legend .swatch {
width: 10px; height: 10px; border-radius: 4px;
}
.cal-legend .sw-period { background: var(–terracotta); }
.cal-legend .sw-predicted { background: var(–terracotta-soft); border: 1px solid var(–terracotta); }

/* Quick stats / cards */
.quick-row {
display: grid;
grid-template-columns: 1fr 1fr;
gap: 10px;
padding: 0 20px 16px;
}
.quick-card {
background: var(–surface);
border-radius: var(–radius);
padding: 16px;
border: 1px solid var(–line);
cursor: pointer;
transition: transform 0.15s ease, box-shadow 0.2s ease;
}
.quick-card:active { transform: scale(0.98); }
.quick-card:hover { box-shadow: var(–shadow); }
.qc-label {
font-size: 10px;
color: var(–ink-3);
text-transform: uppercase;
letter-spacing: 1.5px;
margin-bottom: 8px;
}
.qc-value {
font-family: ‘DM Serif Display’, serif;
font-size: 22px;
color: var(–ink);
line-height: 1.1;
margin-bottom: 4px;
}
.qc-value .unit {
font-size: 12px;
color: var(–ink-3);
font-family: ‘Nunito’;
margin-left: 2px;
}
.qc-meta {
font-size: 11px;
color: var(–ink-2);
}
.qc-water .qc-value { color: var(–ocean); }
.qc-sleep .qc-value { color: var(–accent); }
.qc-card-empty {
background: var(–surface-2);
border: 1px dashed var(–ink-3);
color: var(–ink-2);
text-align: center;
display: flex;
flex-direction: column;
justify-content: center;
align-items: center;
gap: 4px;
}
.qc-card-empty .qc-plus {
width: 28px; height: 28px;
border-radius: 100px;
background: var(–surface);
display: flex;
align-items: center;
justify-content: center;
margin-bottom: 4px;
color: var(–accent);
font-size: 18px;
}
.qc-card-empty .qc-empty-label {
font-size: 12px;
color: var(–ink-2);
}
.qc-card-empty .qc-empty-sub {
font-size: 10px;
color: var(–ink-3);
}

/* Bottom nav */
.bottom-nav {
display: flex;
justify-content: space-around;
align-items: center;
padding: 14px 16px 28px;
background: var(–surface);
border-top: 1px solid var(–line);
flex-shrink: 0;
}
.nav-btn {
background: none;
border: none;
display: flex;
flex-direction: column;
align-items: center;
gap: 4px;
color: var(–ink-3);
font-size: 10px;
font-family: ‘Nunito’;
cursor: pointer;
padding: 4px 8px;
text-transform: uppercase;
letter-spacing: 0.5px;
}
.nav-btn.active { color: var(–accent); }
.nav-btn svg { display: block; }

.scroll-area {
flex: 1;
overflow-y: auto;
}

/* ============ LOG DAY MODAL ============ */
.modal-overlay {
position: absolute;
inset: 0;
background: rgba(42, 40, 35, 0.4);
backdrop-filter: blur(4px);
z-index: 100;
display: none;
align-items: flex-end;
}
.modal-overlay.active {
display: flex;
animation: fadeIn 0.2s ease;
}
@keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
@keyframes slideUp {
from { transform: translateY(100%); }
to { transform: translateY(0); }
}
.modal-sheet {
background: var(–bg);
width: 100%;
border-radius: 28px 28px 0 0;
padding: 12px 24px 32px;
max-height: 90%;
overflow-y: auto;
animation: slideUp 0.3s cubic-bezier(0.34, 1.3, 0.64, 1);
}
.modal-handle {
width: 40px;
height: 4px;
background: var(–line);
border-radius: 100px;
margin: 0 auto 16px;
}
.modal-title {
font-family: ‘DM Serif Display’, serif;
font-size: 22px;
color: var(–ink);
margin-bottom: 4px;
}
.modal-date {
font-size: 12px;
color: var(–ink-3);
text-transform: uppercase;
letter-spacing: 1.5px;
margin-bottom: 24px;
}
.log-section { margin-bottom: 22px; }
.log-section h4 {
font-size: 11px;
text-transform: uppercase;
letter-spacing: 1.5px;
color: var(–ink-2);
margin-bottom: 10px;
font-weight: 500;
}
.flow-buttons {
display: grid;
grid-template-columns: repeat(4, 1fr);
gap: 8px;
}
.flow-btn {
padding: 14px 6px;
background: var(–surface);
border: 1px solid var(–line);
border-radius: var(–radius-sm);
font-family: ‘Nunito’;
font-size: 12px;
color: var(–ink);
cursor: pointer;
transition: all 0.15s ease;
text-align: center;
}
.flow-btn.selected {
background: var(–terracotta);
border-color: var(–terracotta);
color: white;
}
.flow-btn .drop {
display: block;
margin: 0 auto 4px;
height: 14px;
}

.mood-grid {
display: grid;
grid-template-columns: repeat(5, 1fr);
gap: 6px;
}
.mood-btn {
aspect-ratio: 1;
background: var(–surface);
border: 1px solid var(–line);
border-radius: var(–radius-sm);
font-size: 22px;
cursor: pointer;
display: flex;
align-items: center;
justify-content: center;
transition: all 0.15s ease;
}
.mood-btn.selected {
background: var(–accent-soft);
border-color: var(–accent);
transform: scale(1.05);
}

.energy-track {
display: flex;
gap: 6px;
justify-content: space-between;
}
.energy-dot {
flex: 1;
height: 36px;
background: var(–surface);
border: 1px solid var(–line);
border-radius: var(–radius-sm);
cursor: pointer;
transition: all 0.15s ease;
}
.energy-dot.active {
background: var(–sage);
border-color: var(–sage);
}
.note-input {
width: 100%;
min-height: 60px;
padding: 12px 14px;
background: var(–surface);
border: 1px solid var(–line);
border-radius: var(–radius-sm);
font-family: ‘Nunito’;
font-size: 13px;
color: var(–ink);
resize: none;
outline: none;
}
.note-input:focus { border-color: var(–accent); }
.modal-actions {
display: flex;
gap: 10px;
margin-top: 8px;
}
.modal-actions .btn-primary { flex: 1; }
.btn-ghost {
width: 56px;
height: 52px;
background: var(–surface);
border: 1px solid var(–line);
border-radius: 100px;
color: var(–ink-2);
cursor: pointer;
font-size: 14px;
}

/* ============ WATER SCREEN ============ */
.water-hero {
padding: 20px 24px 20px;
text-align: center;
}
.water-ring-wrap {
width: 200px;
height: 200px;
margin: 8px auto 16px;
position: relative;
}
.water-ring-bg, .water-ring-fg {
width: 100%; height: 100%;
transform: rotate(-90deg);
}
.water-ring-bg circle {
fill: none;
stroke: var(–ocean-soft);
stroke-width: 14;
}
.water-ring-fg {
position: absolute;
inset: 0;
}
.water-ring-fg circle {
fill: none;
stroke: var(–ocean);
stroke-width: 14;
stroke-linecap: round;
transition: stroke-dashoffset 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}
.water-ring-center {
position: absolute;
inset: 0;
display: flex;
flex-direction: column;
align-items: center;
justify-content: center;
}
.water-amount {
font-family: ‘DM Serif Display’, serif;
font-size: 40px;
font-weight: 300;
color: var(–ink);
line-height: 1;
}
.water-amount .unit {
font-size: 18px;
color: var(–ink-3);
font-family: ‘Nunito’;
}
.water-target {
font-size: 11px;
color: var(–ink-3);
text-transform: uppercase;
letter-spacing: 1.5px;
margin-top: 6px;
}
.water-actions {
display: flex;
gap: 12px;
justify-content: center;
padding: 0 24px 16px;
}
.glass-btn {
padding: 12px 22px;
background: var(–ocean);
color: white;
border: none;
border-radius: 100px;
font-family: ‘Nunito’;
font-size: 13px;
font-weight: 500;
cursor: pointer;
display: flex;
align-items: center;
gap: 8px;
transition: transform 0.15s ease;
}
.glass-btn:active { transform: scale(0.96); }
.glass-btn.minus {
background: var(–surface);
color: var(–ink-2);
border: 1px solid var(–line);
}

.water-formula {
background: var(–surface);
margin: 8px 20px 20px;
padding: 18px 20px;
border-radius: var(–radius);
border: 1px solid var(–line);
}
.water-formula h4 {
font-size: 11px;
text-transform: uppercase;
letter-spacing: 1.5px;
color: var(–ink-2);
margin-bottom: 8px;
font-weight: 500;
}
.water-formula p {
font-size: 13px;
color: var(–ink);
line-height: 1.5;
}
.water-formula em { font-style: italic; color: var(–accent); }

.week-tracker {
margin: 0 20px 20px;
}
.week-tracker h4 {
font-size: 11px;
text-transform: uppercase;
letter-spacing: 1.5px;
color: var(–ink-2);
margin-bottom: 14px;
font-weight: 500;
padding-left: 4px;
}
.week-bars {
display: grid;
grid-template-columns: repeat(7, 1fr);
gap: 8px;
background: var(–surface);
padding: 16px;
border-radius: var(–radius);
border: 1px solid var(–line);
}
.week-bar {
display: flex;
flex-direction: column;
align-items: center;
gap: 6px;
}
.bar-track {
width: 100%;
height: 60px;
background: var(–surface-2);
border-radius: 6px;
position: relative;
overflow: hidden;
}
.bar-fill {
position: absolute;
bottom: 0;
left: 0;
right: 0;
background: var(–ocean);
border-radius: 6px 6px 0 0;
transition: height 0.4s ease;
}
.bar-day {
font-size: 9px;
color: var(–ink-3);
text-transform: uppercase;
letter-spacing: 0.5px;
}
.bar-day.today { color: var(–accent); font-weight: 600; }

/* ============ SLEEP SCREEN ============ */
.sleep-hero {
padding: 16px 24px 8px;
}
.sleep-current {
background: linear-gradient(135deg, var(–accent), #5D5070);
border-radius: 24px;
padding: 24px;
color: white;
position: relative;
overflow: hidden;
}
.sleep-current::after {
content: ‘’;
position: absolute;
top: -40px;
right: -40px;
width: 120px;
height: 120px;
background: radial-gradient(circle, rgba(255,255,255,0.15) 0%, transparent 70%);
border-radius: 100px;
}
.sleep-label {
font-size: 11px;
text-transform: uppercase;
letter-spacing: 2px;
opacity: 0.75;
margin-bottom: 8px;
}
.sleep-duration {
font-family: ‘DM Serif Display’, serif;
font-size: 44px;
font-weight: 300;
line-height: 1;
margin-bottom: 6px;
}
.sleep-duration .unit {
font-size: 18px;
opacity: 0.7;
font-family: ‘Nunito’;
}
.sleep-range {
font-size: 13px;
opacity: 0.85;
font-family: ‘DM Serif Display’;
font-style: italic;
}

.sleep-log-card {
margin: 16px 20px;
background: var(–surface);
border-radius: var(–radius);
padding: 20px;
border: 1px solid var(–line);
}
.sleep-log-card h4 {
font-size: 13px;
color: var(–ink);
margin-bottom: 14px;
font-family: ‘DM Serif Display’;
font-weight: 500;
}
.time-pickers {
display: grid;
grid-template-columns: 1fr 1fr;
gap: 12px;
margin-bottom: 14px;
}
.time-picker {
background: var(–surface-2);
border-radius: var(–radius-sm);
padding: 12px;
text-align: center;
}
.tp-label {
font-size: 10px;
text-transform: uppercase;
letter-spacing: 1.5px;
color: var(–ink-3);
margin-bottom: 6px;
}
.tp-value {
font-family: ‘DM Serif Display’, serif;
font-size: 24px;
color: var(–ink);
}
.sleep-save {
width: 100%;
height: 44px;
background: var(–ink);
color: var(–bg);
border: none;
border-radius: 100px;
font-size: 13px;
cursor: pointer;
font-family: ‘Nunito’;
font-weight: 500;
}

.sleep-week {
margin: 0 20px 20px;
}
.sleep-week h4 {
font-size: 11px;
text-transform: uppercase;
letter-spacing: 1.5px;
color: var(–ink-2);
margin-bottom: 14px;
font-weight: 500;
padding-left: 4px;
}
.sleep-week-card {
background: var(–surface);
padding: 18px;
border-radius: var(–radius);
border: 1px solid var(–line);
}
.sleep-stats-row {
display: flex;
justify-content: space-between;
margin-bottom: 16px;
padding-bottom: 14px;
border-bottom: 1px solid var(–line);
}
.sleep-stat-label {
font-size: 10px;
color: var(–ink-3);
text-transform: uppercase;
letter-spacing: 1px;
margin-bottom: 4px;
}
.sleep-stat-value {
font-family: ‘DM Serif Display’, serif;
font-size: 20px;
color: var(–ink);
}
.sleep-trend {
color: var(–sage);
font-size: 12px;
font-style: italic;
font-family: ‘DM Serif Display’;
}
.sleep-bars {
display: grid;
grid-template-columns: repeat(7, 1fr);
gap: 6px;
align-items: end;
height: 80px;
}
.sleep-bar {
display: flex;
flex-direction: column;
align-items: center;
gap: 4px;
height: 100%;
justify-content: flex-end;
}
.sleep-bar-fill {
width: 100%;
background: var(–accent);
border-radius: 4px 4px 0 0;
opacity: 0.85;
}
.sleep-bar-label {
font-size: 9px;
color: var(–ink-3);
text-transform: uppercase;
}

/* Setup screens (water onboarding) */
.weight-display {
background: var(–surface);
border-radius: var(–radius);
padding: 32px 20px;
text-align: center;
border: 1px solid var(–line);
}
.weight-value {
font-family: ‘DM Serif Display’, serif;
font-size: 64px;
font-weight: 300;
color: var(–ink);
line-height: 1;
}
.weight-value .unit {
font-size: 18px;
color: var(–ink-3);
font-family: ‘Nunito’;
}
.weight-slider {
width: 100%;
margin-top: 24px;
-webkit-appearance: none;
height: 4px;
background: var(–surface-2);
border-radius: 100px;
outline: none;
}
.weight-slider::-webkit-slider-thumb {
-webkit-appearance: none;
width: 24px;
height: 24px;
background: var(–accent);
border-radius: 100px;
cursor: pointer;
box-shadow: 0 2px 8px rgba(124, 107, 142, 0.3);
}

/* Toast */
.toast {
position: absolute;
bottom: 100px;
left: 50%;
transform: translateX(-50%) translateY(20px);
background: var(–ink);
color: var(–bg);
padding: 12px 20px;
border-radius: 100px;
font-size: 13px;
opacity: 0;
pointer-events: none;
transition: all 0.3s ease;
z-index: 200;
white-space: nowrap;
}
.toast.show {
opacity: 1;
transform: translateX(-50%) translateY(0);
}

/* Reset / demo controls */
.demo-controls {
position: fixed;
top: env(safe-area-inset-top, 8px);
right: 8px;
z-index: 1000;
display: flex;
gap: 8px;
}
.demo-controls button {
background: rgba(42, 40, 35, 0.7);
color: white;
border: none;
padding: 6px 12px;
border-radius: 100px;
font-size: 10px;
font-family: ‘Nunito’;
cursor: pointer;
backdrop-filter: blur(8px);
}

/* Onboarding pre-text */
.ob-step-label {
font-size: 10px;
text-transform: uppercase;
letter-spacing: 2.5px;
color: var(–ink-3);
margin-top: 32px;
}

/* “View full calendar” detail */
.day-detail-strip {
background: var(–surface);
border-radius: var(–radius);
margin: 0 20px 16px;
padding: 14px 18px;
display: flex;
justify-content: space-between;
align-items: center;
border: 1px solid var(–line);
cursor: pointer;
}
.day-detail-strip:hover { box-shadow: var(–shadow); }
.dds-info { display: flex; align-items: center; gap: 12px; }
.dds-emoji { font-size: 24px; }
.dds-text {
font-size: 13px;
color: var(–ink);
}
.dds-text strong { font-weight: 600; }
.dds-text small { display: block; color: var(–ink-3); font-size: 11px; margin-top: 2px; }
</style>

</head>
<body>

<div class="demo-wrap">

  <div class="demo-info">
    <div class="tag">V1 Prototip</div>
    <div class="brand">Lumen.</div>
    <h2>Hayz, uyqu va suv tartibi uchun xotirjam, shaxsiy salomatlik hamrohi.</h2>
    <p>Bu — foydalanuvchi sinovi uchun mo'ljallangan bosqichli prototip. Ma'lumotlar faqat xotirada saqlanadi — sahifa yangilansa, hammasi qaytadan boshlanadi.</p>
    <p><strong>Quyidagilarni sinab ko'ring:</strong></p>
    <ul class="pill-list">
      <li>60 soniyalik ro'yxatdan o'tishni yakunlang</li>
      <li>Kayfiyat va energiyani belgilash uchun istalgan kunni bosing</li>
      <li>Suv kuzatuvini sozlang — haqiqiy formulaga asoslangan</li>
      <li>Uyquni qayd qiling va haftalik tendentsiyani kuzating</li>
    </ul>
  </div>

  <div class="phone-frame">
    <div class="phone-screen" id="screen-root">

```
  <!-- Toast -->
  <div class="toast" id="toast"></div>

  <!-- ========== SPLASH ========== -->
  <div class="screen active" id="screen-splash">
    <div class="status-bar">
      <span>9:41</span>
      <span class="icons">
        <svg width="16" height="10" viewBox="0 0 16 10" fill="currentColor"><path d="M1 6h2v3H1V6zm4-2h2v5H5V4zm4-2h2v7H9V2zm4-2h2v9h-2V0z"/></svg>
        <svg width="14" height="10" viewBox="0 0 14 10" fill="none" stroke="currentColor" stroke-width="1.2"><rect x="1" y="3" width="9" height="4" rx="1"/><path d="M12 4.5v1"/></svg>
        <svg width="22" height="10" viewBox="0 0 22 10" fill="none" stroke="currentColor" stroke-width="1.2"><rect x="1" y="1" width="18" height="8" rx="1.5"/><rect x="3" y="3" width="14" height="4" fill="currentColor"/></svg>
      </span>
    </div>
    <div class="splash-inner">
      <div class="splash-mark">Lumen</div>
      <div class="splash-tag">Xotirjam yashashning yo'li</div>
    </div>
  </div>

  <!-- ========== PRIVACY ========== -->
  <div class="screen" id="screen-privacy">
    <div class="status-bar">
      <span>9:41</span>
      <span class="icons">
        <svg width="22" height="10" viewBox="0 0 22 10" fill="none" stroke="currentColor" stroke-width="1.2"><rect x="1" y="1" width="18" height="8" rx="1.5"/><rect x="3" y="3" width="14" height="4" fill="currentColor"/></svg>
      </span>
    </div>
    <div class="ob-progress"><div class="ob-progress-fill" style="width: 16%"></div></div>
    <div class="ob-content">
      <div class="privacy-icon">
        <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="#7C6B8E" stroke-width="1.5">
          <path d="M12 2L3 6v6c0 5.5 3.84 10.74 9 12 5.16-1.26 9-6.5 9-12V6l-9-4z"/>
          <path d="m9 12 2 2 4-4"/>
        </svg>
      </div>
      <h1 class="ob-title">Ma'lumotlaringiz<br><em>faqat sizniki.</em></h1>
      <p class="ob-sub">Boshlashdan oldin, sizga va'da qilamiz:</p>
      <ul class="privacy-list">
        <li>
          <svg class="check" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M20 6L9 17l-5-5"/></svg>
          <span>Hayz ma'lumotlari faqat telefoningizda saqlanadi.</span>
        </li>
        <li>
          <svg class="check" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M20 6L9 17l-5-5"/></svg>
          <span>Hech narsani sotmaymiz yoki ulashmaymiz.</span>
        </li>
        <li>
          <svg class="check" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M20 6L9 17l-5-5"/></svg>
          <span>Hech qachon reklama yo'q. Kuzatuvchi xizmatlar ham yo'q.</span>
        </li>
        <li>
          <svg class="check" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M20 6L9 17l-5-5"/></svg>
          <span>Istalgan vaqtda ma'lumotni yuklab olish yoki o'chirish mumkin.</span>
        </li>
      </ul>
    </div>
    <div class="ob-cta-area">
      <button class="btn-primary" onclick="goTo('screen-name')">Tushundim</button>
    </div>
  </div>

  <!-- ========== NAME ========== -->
  <div class="screen" id="screen-name">
    <div class="status-bar"><span>9:41</span><span class="icons"><svg width="22" height="10" viewBox="0 0 22 10" fill="none" stroke="currentColor" stroke-width="1.2"><rect x="1" y="1" width="18" height="8" rx="1.5"/><rect x="3" y="3" width="14" height="4" fill="currentColor"/></svg></span></div>
    <div class="ob-progress"><div class="ob-progress-fill" style="width: 33%"></div></div>
    <button class="ob-back" onclick="goTo('screen-privacy')">←</button>
    <button class="ob-skip" onclick="skipName()">O'tkazib yuborish</button>
    <div class="ob-content">
      <div class="ob-step-label">1-bosqich / 3</div>
      <h1 class="ob-title">Sizni qanday<br><em>chaqiraylik?</em></h1>
      <p class="ob-sub">Faqat ismingiz — yoki anonim qolishni istasangiz, o'tkazib yuboring.</p>
      <input type="text" class="name-input" id="name-input" placeholder="Ismingiz" maxlength="20" autocomplete="off">
    </div>
    <div class="ob-cta-area">
      <button class="btn-primary" onclick="submitName()">Davom etish</button>
    </div>
  </div>

  <!-- ========== LAST PERIOD ========== -->
  <div class="screen" id="screen-last-period">
    <div class="status-bar"><span>9:41</span><span class="icons"><svg width="22" height="10" viewBox="0 0 22 10" fill="none" stroke="currentColor" stroke-width="1.2"><rect x="1" y="1" width="18" height="8" rx="1.5"/><rect x="3" y="3" width="14" height="4" fill="currentColor"/></svg></span></div>
    <div class="ob-progress"><div class="ob-progress-fill" style="width: 50%"></div></div>
    <button class="ob-back" onclick="goTo('screen-name')">←</button>
    <div class="ob-content">
      <div class="ob-step-label">2-bosqich / 3</div>
      <h1 class="ob-title">Oxirgi hayzingiz<br><em>qachon boshlangan?</em></h1>
      <p class="ob-sub">Oxirgi hayzingiz boshlangan kunni belgilang.</p>
      <div class="mini-cal" id="mini-cal-container"></div>
    </div>
    <div class="ob-cta-area">
      <button class="btn-primary" onclick="submitLastPeriod()" id="last-period-btn" disabled style="opacity:0.4">Davom etish</button>
    </div>
  </div>

  <!-- ========== CYCLE LENGTH ========== -->
  <div class="screen" id="screen-cycle-length">
    <div class="status-bar"><span>9:41</span><span class="icons"><svg width="22" height="10" viewBox="0 0 22 10" fill="none" stroke="currentColor" stroke-width="1.2"><rect x="1" y="1" width="18" height="8" rx="1.5"/><rect x="3" y="3" width="14" height="4" fill="currentColor"/></svg></span></div>
    <div class="ob-progress"><div class="ob-progress-fill" style="width: 75%"></div></div>
    <button class="ob-back" onclick="goTo('screen-last-period')">←</button>
    <div class="ob-content">
      <div class="ob-step-label">3-bosqich / 3</div>
      <h1 class="ob-title">Hayz davringizning<br><em>o'rtacha uzunligi?</em></h1>
      <p class="ob-sub">Ko'pchilik uchun bu 21 va 35 kun orasida. O'rtacha — 28 kun.</p>
      <div class="number-picker">
        <button class="np-btn" onclick="adjustCycle(-1)">−</button>
        <div>
          <div class="np-value" id="cycle-value">28</div>
          <div class="np-unit">Kun</div>
        </div>
        <button class="np-btn" onclick="adjustCycle(1)">+</button>
      </div>
      <button class="np-link" onclick="state.cycleLength=28; submitCycleLength()">Bilmayman — o'rtacha qiymatdan foydalaning</button>
    </div>
    <div class="ob-cta-area">
      <button class="btn-primary" onclick="submitCycleLength()">Davom etish</button>
    </div>
  </div>

  <!-- ========== REVEAL ========== -->
  <div class="screen" id="screen-reveal">
    <div class="status-bar"><span>9:41</span><span class="icons"><svg width="22" height="10" viewBox="0 0 22 10" fill="none" stroke="currentColor" stroke-width="1.2"><rect x="1" y="1" width="18" height="8" rx="1.5"/><rect x="3" y="3" width="14" height="4" fill="currentColor"/></svg></span></div>
    <div class="ob-progress"><div class="ob-progress-fill" style="width: 100%"></div></div>
    <div class="ob-content" style="justify-content:center; align-items:flex-start;">
      <div style="margin-top: 40px;">
        <div class="ob-step-label">Hammasi tayyor</div>
        <h1 class="ob-title" id="reveal-title">Xush kelibsiz,<br><em>do'stim.</em></h1>
        <p class="ob-sub">Kalendaringiz tayyor. Keyingi hayzingiz taxminan <strong id="reveal-prediction">tez orada</strong>.</p>
        <p class="ob-sub" style="margin-top:16px;">Endi har kuni o'zingizni qanday his qilayotganingizni qayd qilishingiz, suv kuzatuvini qo'shishingiz yoki uyqu vaqtini belgilashingiz mumkin — barchasi ixtiyoriy.</p>
      </div>
    </div>
    <div class="ob-cta-area">
      <button class="btn-primary" onclick="enterApp()">Kalendarimni ko'rish</button>
    </div>
  </div>

  <!-- ========== HOME (CALENDAR) ========== -->
  <div class="screen" id="screen-home">
    <div class="status-bar"><span>9:41</span><span class="icons"><svg width="22" height="10" viewBox="0 0 22 10" fill="none" stroke="currentColor" stroke-width="1.2"><rect x="1" y="1" width="18" height="8" rx="1.5"/><rect x="3" y="3" width="14" height="4" fill="currentColor"/></svg></span></div>

    <div class="scroll-area">
      <div class="app-header">
        <div class="greeting-block">
          <div class="greeting-pre" id="home-date">—</div>
          <div class="greeting-name">Salom, <em id="home-name">do'stim</em></div>
        </div>
        <div class="avatar" id="home-avatar">·</div>
      </div>

      <div class="status-card">
        <div class="status-top">
          <div class="status-phase" id="status-phase">FOLLIKULYAR BOSQICH</div>
          <div class="status-day" id="status-day">Kun —</div>
        </div>
        <div class="status-title" id="status-title">Yumshoq kun</div>
        <div class="status-meta" id="status-meta">Keyingi hayz — kundan keyin</div>
      </div>

      <div class="cal-section">
        <div class="cal-header">
          <div class="cal-month" id="cal-month">—</div>
          <div class="cal-nav">
            <button onclick="changeMonth(-1)">‹</button>
            <button onclick="changeMonth(1)">›</button>
          </div>
        </div>
        <div class="cal-grid" id="main-cal-grid"></div>
        <div class="cal-legend">
          <span><span class="swatch sw-period"></span> Qayd qilingan</span>
          <span><span class="swatch sw-predicted"></span> Bashorat</span>
        </div>
      </div>

      <div class="day-detail-strip" onclick="openLogModal(state.today)" id="today-strip">
        <div class="dds-info">
          <div class="dds-emoji">✿</div>
          <div class="dds-text">
            <strong>Bugungi kunni qayd qilish</strong>
            <small>Kayfiyat · oqim · energiya · eslatmalar</small>
          </div>
        </div>
        <div style="color: var(--ink-3); font-size: 18px;">›</div>
      </div>

      <div class="quick-row">
        <div class="quick-card qc-water" id="water-card" onclick="openWater()">
          <div class="qc-label">Suv</div>
          <div class="qc-value"><span id="water-display">+</span></div>
          <div class="qc-meta" id="water-meta">Kuzatuvni qo'shish</div>
        </div>
        <div class="quick-card qc-sleep" id="sleep-card" onclick="openSleep()">
          <div class="qc-label">Uyqu</div>
          <div class="qc-value"><span id="sleep-display">+</span></div>
          <div class="qc-meta" id="sleep-meta">Kuzatuvni qo'shish</div>
        </div>
      </div>

      <div style="height: 20px;"></div>
    </div>

    <div class="bottom-nav">
      <button class="nav-btn active" onclick="navTo('screen-home')">
        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><rect x="3" y="4" width="18" height="18" rx="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/></svg>
        <span>Hayz</span>
      </button>
      <button class="nav-btn" onclick="navTo('screen-water')">
        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M12 2.69l5.66 5.66a8 8 0 1 1-11.31 0z"/></svg>
        <span>Suv</span>
      </button>
      <button class="nav-btn" onclick="navTo('screen-sleep')">
        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"/></svg>
        <span>Uyqu</span>
      </button>
      <button class="nav-btn" onclick="showToast('Sozlamalar — V2 da keladi')">
        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-4 0v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1 0-4h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 4 0v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 0 4h-.09a1.65 1.65 0 0 0-1.51 1z"/></svg>
        <span>Profil</span>
      </button>
    </div>
  </div>

  <!-- ========== LOG DAY MODAL ========== -->
  <div class="modal-overlay" id="log-modal" onclick="if(event.target===this) closeLogModal()">
    <div class="modal-sheet">
      <div class="modal-handle"></div>
      <div class="modal-title">Kunni qayd qilish</div>
      <div class="modal-date" id="log-modal-date">—</div>

      <div class="log-section">
        <h4>Oqim</h4>
        <div class="flow-buttons" id="flow-buttons">
          <button class="flow-btn" data-flow="none">
            <span style="font-size:18px;">○</span><br>Yo'q
          </button>
          <button class="flow-btn" data-flow="light">
            <span style="font-size:18px;">·</span><br>Yengil
          </button>
          <button class="flow-btn" data-flow="medium">
            <span style="font-size:18px;">●</span><br>O'rta
          </button>
          <button class="flow-btn" data-flow="heavy">
            <span style="font-size:18px;">⬤</span><br>Kuchli
          </button>
        </div>
      </div>

      <div class="log-section">
        <h4>Kayfiyat</h4>
        <div class="mood-grid" id="mood-grid">
          <button class="mood-btn" data-mood="happy">😊</button>
          <button class="mood-btn" data-mood="calm">😌</button>
          <button class="mood-btn" data-mood="neutral">😐</button>
          <button class="mood-btn" data-mood="low">😔</button>
          <button class="mood-btn" data-mood="irritated">😠</button>
        </div>
      </div>

      <div class="log-section">
        <h4>Energiya</h4>
        <div class="energy-track" id="energy-track">
          <div class="energy-dot" data-energy="1"></div>
          <div class="energy-dot" data-energy="2"></div>
          <div class="energy-dot" data-energy="3"></div>
          <div class="energy-dot" data-energy="4"></div>
          <div class="energy-dot" data-energy="5"></div>
        </div>
      </div>

      <div class="log-section">
        <h4>Eslatmalar</h4>
        <textarea class="note-input" id="note-input" placeholder="Bugun haqida eslab qolmoqchi bo'lgan har narsa..."></textarea>
      </div>

      <div class="modal-actions">
        <button class="btn-ghost" onclick="closeLogModal()">×</button>
        <button class="btn-primary" onclick="saveLog()">Saqlash</button>
      </div>
    </div>
  </div>

  <!-- ========== WATER SETUP ========== -->
  <div class="screen" id="screen-water-setup">
    <div class="status-bar"><span>9:41</span><span class="icons"><svg width="22" height="10" viewBox="0 0 22 10" fill="none" stroke="currentColor" stroke-width="1.2"><rect x="1" y="1" width="18" height="8" rx="1.5"/><rect x="3" y="3" width="14" height="4" fill="currentColor"/></svg></span></div>
    <button class="ob-back" onclick="goTo('screen-home')">←</button>
    <div class="ob-content">
      <h1 class="ob-title" style="margin-top:48px;">Vazningiz<br><em>qancha?</em></h1>
      <p class="ob-sub">Bu ma'lumot kunlik suv miqdorini hisoblash uchungina kerak. Boshqa hech narsaga emas.</p>
      <div class="weight-display">
        <div class="weight-value"><span id="weight-value">60</span><span class="unit">kg</span></div>
        <input type="range" min="40" max="120" value="60" class="weight-slider" id="weight-slider" oninput="updateWeight(this.value)">
      </div>
      <p class="ob-sub" style="margin-top:20px; font-size:12px; text-align:center;">
        Kunlik me'yor: <em style="color:var(--accent); font-style:italic;">30 ml × vazn</em> formulasi.
      </p>
    </div>
    <div class="ob-cta-area">
      <button class="btn-primary" onclick="completeWaterSetup()">Suv rejasini sozlash</button>
      <button class="btn-secondary" onclick="goTo('screen-home')">Keyinroq</button>
    </div>
  </div>

  <!-- ========== WATER SCREEN ========== -->
  <div class="screen" id="screen-water">
    <div class="status-bar"><span>9:41</span><span class="icons"><svg width="22" height="10" viewBox="0 0 22 10" fill="none" stroke="currentColor" stroke-width="1.2"><rect x="1" y="1" width="18" height="8" rx="1.5"/><rect x="3" y="3" width="14" height="4" fill="currentColor"/></svg></span></div>
    <div class="scroll-area">
      <div class="app-header">
        <div class="greeting-block">
          <div class="greeting-pre">Bugun</div>
          <div class="greeting-name"><em>Suv tartibi</em></div>
        </div>
      </div>

      <div class="water-hero">
        <div class="water-ring-wrap">
          <svg class="water-ring-bg" viewBox="0 0 100 100">
            <circle cx="50" cy="50" r="42"/>
          </svg>
          <svg class="water-ring-fg" viewBox="0 0 100 100">
            <circle cx="50" cy="50" r="42" id="water-ring-circle"
              stroke-dasharray="263.9"
              stroke-dashoffset="263.9"/>
          </svg>
          <div class="water-ring-center">
            <div class="water-amount"><span id="water-now">0</span><span class="unit">L</span></div>
            <div class="water-target">bugun <span id="water-target">1.8</span>L dan</div>
          </div>
        </div>
      </div>

      <div class="water-actions">
        <button class="glass-btn minus" onclick="addGlass(-1)">−</button>
        <button class="glass-btn" onclick="addGlass(1)">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 2.69l5.66 5.66a8 8 0 1 1-11.31 0z"/></svg>
          Stakan qo'shish (250ml)
        </button>
      </div>

      <div class="water-formula">
        <h4>Sizning rejangiz</h4>
        <p>Vazningiz <em id="formula-weight">60</em> kg bo'lgani uchun, kuniga <em id="formula-target">1.8L</em> suv tavsiya etiladi — taxminan <em id="formula-glasses">7</em> stakan.</p>
      </div>

      <div class="week-tracker">
        <h4>Shu hafta</h4>
        <div class="week-bars" id="water-week-bars"></div>
      </div>

      <div style="height: 20px;"></div>
    </div>

    <div class="bottom-nav">
      <button class="nav-btn" onclick="navTo('screen-home')">
        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><rect x="3" y="4" width="18" height="18" rx="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/></svg>
        <span>Hayz</span>
      </button>
      <button class="nav-btn active">
        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M12 2.69l5.66 5.66a8 8 0 1 1-11.31 0z"/></svg>
        <span>Suv</span>
      </button>
      <button class="nav-btn" onclick="navTo('screen-sleep')">
        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"/></svg>
        <span>Uyqu</span>
      </button>
      <button class="nav-btn" onclick="showToast('Sozlamalar — V2 da keladi')">
        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><circle cx="12" cy="12" r="3"/></svg>
        <span>Profil</span>
      </button>
    </div>
  </div>

  <!-- ========== SLEEP SETUP ========== -->
  <div class="screen" id="screen-sleep-setup">
    <div class="status-bar"><span>9:41</span><span class="icons"><svg width="22" height="10" viewBox="0 0 22 10" fill="none" stroke="currentColor" stroke-width="1.2"><rect x="1" y="1" width="18" height="8" rx="1.5"/><rect x="3" y="3" width="14" height="4" fill="currentColor"/></svg></span></div>
    <button class="ob-back" onclick="goTo('screen-home')">←</button>
    <div class="ob-content">
      <h1 class="ob-title" style="margin-top:48px;">Uyqu maqsadini<br><em>belgilang.</em></h1>
      <p class="ob-sub">Ko'pchilik kattalar uchun ideal vaqt — 7-9 soat. Istalgan paytda o'zgartirishingiz mumkin.</p>
      <div class="weight-display">
        <div class="weight-value"><span id="sleep-goal-value">8</span><span class="unit">soat</span></div>
        <input type="range" min="5" max="11" value="8" step="0.5" class="weight-slider" id="sleep-goal-slider" oninput="updateSleepGoal(this.value)">
      </div>
    </div>
    <div class="ob-cta-area">
      <button class="btn-primary" onclick="completeSleepSetup()">Uyqu maqsadini saqlash</button>
      <button class="btn-secondary" onclick="goTo('screen-home')">Keyinroq</button>
    </div>
  </div>

  <!-- ========== SLEEP SCREEN ========== -->
  <div class="screen" id="screen-sleep">
    <div class="status-bar"><span>9:41</span><span class="icons"><svg width="22" height="10" viewBox="0 0 22 10" fill="none" stroke="currentColor" stroke-width="1.2"><rect x="1" y="1" width="18" height="8" rx="1.5"/><rect x="3" y="3" width="14" height="4" fill="currentColor"/></svg></span></div>
    <div class="scroll-area">
      <div class="app-header">
        <div class="greeting-block">
          <div class="greeting-pre">Kechagi tun</div>
          <div class="greeting-name"><em>Uyqu</em></div>
        </div>
      </div>

      <div class="sleep-hero">
        <div class="sleep-current">
          <div class="sleep-label">Siz uxladingiz</div>
          <div class="sleep-duration"><span id="last-sleep-hrs">7</span><span class="unit">s </span><span id="last-sleep-mins">30</span><span class="unit">d</span></div>
          <div class="sleep-range" id="last-sleep-range">23:30 — 07:00</div>
        </div>
      </div>

      <div class="sleep-log-card">
        <h4>Kechagi tunni qayd qilish</h4>
        <div class="time-pickers">
          <div class="time-picker" onclick="cycleTime('bed')">
            <div class="tp-label">Yotgan vaqt</div>
            <div class="tp-value" id="bed-time">23:30</div>
          </div>
          <div class="time-picker" onclick="cycleTime('wake')">
            <div class="tp-label">Uyg'ongan vaqt</div>
            <div class="tp-value" id="wake-time">07:00</div>
          </div>
        </div>
        <button class="sleep-save" onclick="saveSleep()">Saqlash</button>
      </div>

      <div class="sleep-week">
        <h4>Shu hafta</h4>
        <div class="sleep-week-card">
          <div class="sleep-stats-row">
            <div>
              <div class="sleep-stat-label">O'rtacha</div>
              <div class="sleep-stat-value" id="sleep-avg">7s 24d</div>
            </div>
            <div>
              <div class="sleep-stat-label">Maqsad</div>
              <div class="sleep-stat-value" id="sleep-goal-display">8s</div>
            </div>
            <div>
              <div class="sleep-stat-label">Tendentsiya</div>
              <div class="sleep-trend">yaxshilanmoqda</div>
            </div>
          </div>
          <div class="sleep-bars" id="sleep-week-bars"></div>
        </div>
      </div>
      <div style="height: 20px;"></div>
    </div>

    <div class="bottom-nav">
      <button class="nav-btn" onclick="navTo('screen-home')">
        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><rect x="3" y="4" width="18" height="18" rx="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/></svg>
        <span>Hayz</span>
      </button>
      <button class="nav-btn" onclick="navTo('screen-water')">
        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M12 2.69l5.66 5.66a8 8 0 1 1-11.31 0z"/></svg>
        <span>Suv</span>
      </button>
      <button class="nav-btn active">
        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"/></svg>
        <span>Uyqu</span>
      </button>
      <button class="nav-btn" onclick="showToast('Sozlamalar — V2 da keladi')">
        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><circle cx="12" cy="12" r="3"/></svg>
        <span>Profil</span>
      </button>
    </div>
  </div>

</div>
```

  </div>
</div>

<div class="demo-controls">
  <button onclick="resetDemo()">Boshidan</button>
</div>

<script>
// ========== TELEGRAM WEBAPP INTEGRATION ==========
const tg = window.Telegram?.WebApp;
if (tg) {
  tg.ready();
  tg.expand();                                  // open to full height
  try { tg.requestFullscreen?.(); } catch(e) {}
  tg.setHeaderColor?.('#F5F1EC');               // match our bg
  tg.setBackgroundColor?.('#F5F1EC');
  tg.disableVerticalSwipes?.();                 // prevent accidental close on swipe
  tg.enableClosingConfirmation?.();             // ask before close if user has unsaved logs
}

// Pre-fill user's name from their Telegram profile if available
const tgUser = tg?.initDataUnsafe?.user;
const tgFirstName = tgUser?.first_name || '';

// ========== STATE ==========
const state = {
  name: '',
  lastPeriod: null,        // Date
  cycleLength: 28,
  weight: 60,
  hasWater: false,
  hasSleep: false,
  waterToday: 0,           // glasses (250ml each)
  waterTarget: 1.8,        // L
  sleepGoal: 8,
  bedTime: '23:30',
  wakeTime: '07:00',
  logs: {},                // { 'YYYY-MM-DD': {flow, mood, energy, note} }
  currentLog: { flow: null, mood: null, energy: null, note: '' },
  currentLogDate: null,
  miniCalCurMonth: null,
  mainCalCurMonth: null,
  today: new Date(),
  waterWeek: [0, 0, 0, 0, 0, 0, 0],   // glasses per day
  sleepWeek: [0, 0, 0, 0, 0, 0, 0]    // hours per day
};

// ========== HELPERS ==========
const UZ_MONTHS_LONG = ['Yanvar','Fevral','Mart','Aprel','May','Iyun','Iyul','Avgust','Sentabr','Oktabr','Noyabr','Dekabr'];
const UZ_WEEKDAYS_LONG = ['Yakshanba','Dushanba','Seshanba','Chorshanba','Payshanba','Juma','Shanba'];
const UZ_DOW_LETTERS = ['Y','D','S','C','P','J','Sh'];

function fmtDate(d) {
  return `${d.getDate()}-${UZ_MONTHS_LONG[d.getMonth()]}`;
}
function fmtMonthYear(d) {
  return `${UZ_MONTHS_LONG[d.getMonth()]} ${d.getFullYear()}`;
}
function fmtFullDate(d) {
  return `${UZ_WEEKDAYS_LONG[d.getDay()]}, ${d.getDate()}-${UZ_MONTHS_LONG[d.getMonth()]}`;
}
function dateKey(d) {
  return d.toISOString().slice(0, 10);
}
function daysBetween(a, b) {
  const diff = (b - a) / (1000 * 60 * 60 * 24);
  return Math.floor(diff);
}
function sameDay(a, b) {
  return a.getFullYear() === b.getFullYear() &&
         a.getMonth() === b.getMonth() &&
         a.getDate() === b.getDate();
}

// ========== NAVIGATION ==========
function goTo(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  window.scrollTo(0, 0);
}
function navTo(id) {
  // bottom-nav switching
  goTo(id);
}

// ========== PERSISTENCE (Telegram CloudStorage + localStorage fallback) ==========
const CLOUD = tg?.CloudStorage;

function saveState() {
  const persistable = {
    name: state.name,
    lastPeriod: state.lastPeriod ? state.lastPeriod.toISOString() : null,
    cycleLength: state.cycleLength,
    weight: state.weight,
    hasWater: state.hasWater,
    hasSleep: state.hasSleep,
    waterToday: state.waterToday,
    waterTarget: state.waterTarget,
    sleepGoal: state.sleepGoal,
    bedTime: state.bedTime,
    wakeTime: state.wakeTime,
    logs: state.logs,
    waterWeek: state.waterWeek,
    sleepWeek: state.sleepWeek,
    onboarded: true
  };
  const json = JSON.stringify(persistable);
  if (CLOUD) {
    try { CLOUD.setItem('lumen_state', json); } catch(e) {}
  }
  try { localStorage.setItem('lumen_state', json); } catch(e) {}
}

function loadState(cb) {
  const apply = (json) => {
    if (!json) return cb(false);
    try {
      const d = JSON.parse(json);
      if (!d.onboarded) return cb(false);
      state.name = d.name || '';
      state.lastPeriod = d.lastPeriod ? new Date(d.lastPeriod) : null;
      state.cycleLength = d.cycleLength || 28;
      state.weight = d.weight || 60;
      state.hasWater = !!d.hasWater;
      state.hasSleep = !!d.hasSleep;
      state.waterToday = d.waterToday || 0;
      state.waterTarget = d.waterTarget || 1.8;
      state.sleepGoal = d.sleepGoal || 8;
      state.bedTime = d.bedTime || '23:30';
      state.wakeTime = d.wakeTime || '07:00';
      state.logs = d.logs || {};
      state.waterWeek = d.waterWeek || [0,0,0,0,0,0,0];
      state.sleepWeek = d.sleepWeek || [0,0,0,0,0,0,0];
      cb(true);
    } catch(e) { cb(false); }
  };
  if (CLOUD) {
    CLOUD.getItem('lumen_state', (err, val) => {
      if (!err && val) return apply(val);
      try { apply(localStorage.getItem('lumen_state')); } catch(e) { cb(false); }
    });
  } else {
    try { apply(localStorage.getItem('lumen_state')); } catch(e) { cb(false); }
  }
}

// On boot: try to load saved state and skip onboarding if already set up
window.addEventListener('load', () => {
  loadState((loaded) => {
    if (loaded && state.lastPeriod) {
      // Skip onboarding entirely — go straight to home
      setTimeout(() => {
        goTo('screen-home');
        renderHome();
      }, 1000);
    }
  });
});


// ========== TOAST ==========
let toastTimer = null;
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  clearTimeout(toastTimer);
  toastTimer = setTimeout(() => t.classList.remove('show'), 2200);
}

// ========== ONBOARDING ==========
setTimeout(() => {
  if (document.getElementById('screen-splash').classList.contains('active')) {
    goTo('screen-privacy');
  }
  // Pre-fill name from Telegram if available
  if (tgFirstName) {
    const nameInput = document.getElementById('name-input');
    if (nameInput) nameInput.value = tgFirstName;
  }
}, 1800);

function skipName() {
  state.name = '';
  goTo('screen-last-period');
  renderMiniCal();
}
function submitName() {
  state.name = document.getElementById('name-input').value.trim();
  goTo('screen-last-period');
  renderMiniCal();
}

// Mini calendar for last period
function renderMiniCal() {
  const container = document.getElementById('mini-cal-container');
  const ref = state.miniCalCurMonth || new Date(state.today.getFullYear(), state.today.getMonth(), 1);
  state.miniCalCurMonth = ref;

  const year = ref.getFullYear();
  const month = ref.getMonth();
  const firstDow = new Date(year, month, 1).getDay();
  const daysInMonth = new Date(year, month + 1, 0).getDate();

  let html = `
    <div class="mini-cal-header">
      <button class="mini-cal-nav" onclick="changeMiniCal(-1)">‹</button>
      <div class="mini-cal-month">${fmtMonthYear(ref)}</div>
      <button class="mini-cal-nav" onclick="changeMiniCal(1)">›</button>
    </div>
    <div class="mini-cal-grid">
  `;
  ['Y','D','S','C','P','J','Sh'].forEach(d => {
    html += `<div class="mini-cal-dow">${d}</div>`;
  });
  for (let i = 0; i < firstDow; i++) html += `<div></div>`;
  for (let d = 1; d <= daysInMonth; d++) {
    const date = new Date(year, month, d);
    const isToday = sameDay(date, state.today);
    const isFuture = date > state.today;
    const isSel = state.lastPeriod && sameDay(date, state.lastPeriod);
    const cls = ['mini-cal-day'];
    if (isFuture) cls.push('muted');
    if (isToday) cls.push('today');
    if (isSel) cls.push('selected');
    html += `<button class="${cls.join(' ')}" ${isFuture ? 'disabled' : `onclick="selectLastPeriod(${year},${month},${d})"`}>${d}</button>`;
  }
  html += `</div>`;
  container.innerHTML = html;
}
function changeMiniCal(delta) {
  const m = state.miniCalCurMonth;
  state.miniCalCurMonth = new Date(m.getFullYear(), m.getMonth() + delta, 1);
  renderMiniCal();
}
function selectLastPeriod(y, m, d) {
  state.lastPeriod = new Date(y, m, d);
  const btn = document.getElementById('last-period-btn');
  btn.disabled = false;
  btn.style.opacity = 1;
  renderMiniCal();
}
function submitLastPeriod() {
  if (!state.lastPeriod) return;
  goTo('screen-cycle-length');
}

function adjustCycle(delta) {
  state.cycleLength = Math.max(20, Math.min(40, state.cycleLength + delta));
  document.getElementById('cycle-value').textContent = state.cycleLength;
}
function submitCycleLength() {
  goTo('screen-reveal');
  populateReveal();
}

function populateReveal() {
  const greeting = state.name ? state.name : 'do\'stim';
  document.getElementById('reveal-title').innerHTML = `Xush kelibsiz,<br><em>${greeting}.</em>`;
  // Next period: last period + cycle length
  const next = new Date(state.lastPeriod);
  next.setDate(next.getDate() + state.cycleLength);
  const today = state.today;
  const daysAway = daysBetween(today, next);
  let pred;
  if (daysAway < 0) {
    // overdue — use following cycle
    next.setDate(next.getDate() + state.cycleLength);
    pred = fmtDate(next);
  } else if (daysAway === 0) {
    pred = 'bugun';
  } else {
    pred = `${fmtDate(next)} kuni (${daysAway} kun ichida)`;
  }
  document.getElementById('reveal-prediction').textContent = pred;
}

function enterApp() {
  goTo('screen-home');
  // pre-populate water-week and sleep-week with some realistic past data
  if (state.waterWeek.every(v => v === 0)) {
    state.waterWeek = [6, 7, 5, 7, 6, 8, 0];
  }
  if (state.sleepWeek.every(v => v === 0)) {
    state.sleepWeek = [7.2, 6.8, 7.5, 8.1, 7.0, 7.8, 0];
  }
  renderHome();
  saveState();
}

// ========== HOME / CALENDAR ==========
function renderHome() {
  const today = state.today;
  document.getElementById('home-date').textContent = fmtFullDate(today).toUpperCase();
  const nm = state.name || 'do\'stim';
  document.getElementById('home-name').textContent = nm;
  document.getElementById('home-avatar').textContent = nm.charAt(0).toUpperCase() || '·';

  // cycle phase calculation
  const cycleDay = ((daysBetween(state.lastPeriod, today) % state.cycleLength) + state.cycleLength) % state.cycleLength + 1;
  let phase, phaseTitle, phaseMeta;
  if (cycleDay <= 5) {
    phase = 'HAYZ BOSQICHI';
    phaseTitle = 'Sokin kun';
    phaseMeta = 'O\'zingizga g\'amxo\'r bo\'ling. Dam olish foydaliroq.';
  } else if (cycleDay <= 13) {
    phase = 'FOLLIKULYAR BOSQICH';
    phaseTitle = 'Yuksalish kuni';
    phaseMeta = 'Energiya ortib bormoqda. Yangi ishlar uchun yaxshi vaqt.';
  } else if (cycleDay <= 16) {
    phase = 'OVULYATSIYA BOSQICHI';
    phaseTitle = 'Yorqin kun';
    phaseMeta = 'Eng yuqori energiya va ishonch — uni qadrlang.';
  } else {
    phase = 'LUTEAL BOSQICH';
    phaseTitle = 'O\'ylanish kuni';
    phaseMeta = 'Asta-sekin sustlashtiring. PMS belgilari paydo bo\'lishi mumkin.';
  }
  // Next period prediction
  const next = new Date(state.lastPeriod);
  while (next <= today) next.setDate(next.getDate() + state.cycleLength);
  const daysAway = daysBetween(today, next);

  document.getElementById('status-phase').textContent = phase;
  document.getElementById('status-day').textContent = `Kun ${cycleDay}`;
  document.getElementById('status-title').textContent = phaseTitle;
  document.getElementById('status-meta').textContent = `${phaseMeta} · Keyingi hayz ${daysAway} kun ichida.`;

  renderMainCal();
  renderQuickCards();
}

function renderMainCal() {
  const ref = state.mainCalCurMonth || new Date(state.today.getFullYear(), state.today.getMonth(), 1);
  state.mainCalCurMonth = ref;
  const year = ref.getFullYear();
  const month = ref.getMonth();
  const firstDow = new Date(year, month, 1).getDay();
  const daysInMonth = new Date(year, month + 1, 0).getDate();

  document.getElementById('cal-month').textContent = fmtMonthYear(ref);

  // build period and predicted period sets
  const periodDays = new Set();
  const predictedDays = new Set();

  // Past logged period: lastPeriod + ~5 days
  for (let i = 0; i < 5; i++) {
    const d = new Date(state.lastPeriod);
    d.setDate(d.getDate() + i);
    periodDays.add(dateKey(d));
  }
  // Add any logged-flow days
  Object.entries(state.logs).forEach(([k, v]) => {
    if (v.flow && v.flow !== 'none') periodDays.add(k);
  });
  // Predicted future periods (next 2)
  for (let cyc = 1; cyc <= 3; cyc++) {
    const start = new Date(state.lastPeriod);
    start.setDate(start.getDate() + cyc * state.cycleLength);
    if (start <= state.today) continue;
    for (let i = 0; i < 5; i++) {
      const d = new Date(start);
      d.setDate(d.getDate() + i);
      predictedDays.add(dateKey(d));
    }
  }

  const grid = document.getElementById('main-cal-grid');
  let html = '';
  ['Y','D','S','C','P','J','Sh'].forEach(d => {
    html += `<div class="cal-dow">${d}</div>`;
  });
  for (let i = 0; i < firstDow; i++) html += `<div></div>`;
  for (let d = 1; d <= daysInMonth; d++) {
    const date = new Date(year, month, d);
    const key = dateKey(date);
    const isToday = sameDay(date, state.today);
    const cls = ['cal-day'];
    if (periodDays.has(key)) cls.push('period');
    else if (predictedDays.has(key)) cls.push('predicted');
    if (isToday) cls.push('today');
    const hasLog = state.logs[key] && (state.logs[key].mood || state.logs[key].energy || state.logs[key].note);
    const dot = hasLog && !periodDays.has(key) ? `<span class="dot"></span>` : '';
    html += `<button class="${cls.join(' ')}" onclick="openLogModalForDate(${year},${month},${d})">${d}${dot}</button>`;
  }
  grid.innerHTML = html;
}

function changeMonth(delta) {
  const m = state.mainCalCurMonth;
  state.mainCalCurMonth = new Date(m.getFullYear(), m.getMonth() + delta, 1);
  renderMainCal();
}

function renderQuickCards() {
  if (state.hasWater) {
    const ml = state.waterToday * 250;
    const liters = (ml / 1000).toFixed(1);
    document.getElementById('water-display').textContent = liters + ' L';
    document.getElementById('water-meta').textContent = `${state.waterTarget}L dan`;
  } else {
    document.getElementById('water-display').textContent = '+';
    document.getElementById('water-meta').textContent = 'Kuzatuvni qo\'shish';
  }
  if (state.hasSleep) {
    const last = state.sleepWeek[5]; // yesterday-ish
    const h = Math.floor(last);
    const m = Math.round((last - h) * 60);
    document.getElementById('sleep-display').textContent = `${h}s ${m}d`;
    document.getElementById('sleep-meta').textContent = 'Kechagi tun';
  } else {
    document.getElementById('sleep-display').textContent = '+';
    document.getElementById('sleep-meta').textContent = 'Kuzatuvni qo\'shish';
  }
}

// ========== LOG MODAL ==========
function openLogModal(date) {
  state.currentLogDate = new Date(date);
  const key = dateKey(state.currentLogDate);
  const existing = state.logs[key] || { flow: null, mood: null, energy: null, note: '' };
  state.currentLog = { ...existing };

  document.getElementById('log-modal-date').textContent = fmtFullDate(state.currentLogDate).toUpperCase();

  // Render selection state
  document.querySelectorAll('#flow-buttons .flow-btn').forEach(b => {
    b.classList.toggle('selected', b.dataset.flow === state.currentLog.flow);
  });
  document.querySelectorAll('#mood-grid .mood-btn').forEach(b => {
    b.classList.toggle('selected', b.dataset.mood === state.currentLog.mood);
  });
  document.querySelectorAll('#energy-track .energy-dot').forEach(d => {
    d.classList.toggle('active', state.currentLog.energy !== null && parseInt(d.dataset.energy) <= state.currentLog.energy);
  });
  document.getElementById('note-input').value = state.currentLog.note || '';

  document.getElementById('log-modal').classList.add('active');
}
function openLogModalForDate(y, m, d) {
  const date = new Date(y, m, d);
  if (date > state.today) {
    showToast('Kelajak kunlarni hali qayd qilib bo\'lmaydi');
    return;
  }
  openLogModal(date);
}
function closeLogModal() {
  document.getElementById('log-modal').classList.remove('active');
}

// flow button listeners
document.querySelectorAll('#flow-buttons .flow-btn').forEach(b => {
  b.addEventListener('click', () => {
    state.currentLog.flow = b.dataset.flow;
    document.querySelectorAll('#flow-buttons .flow-btn').forEach(o => o.classList.remove('selected'));
    b.classList.add('selected');
  });
});
document.querySelectorAll('#mood-grid .mood-btn').forEach(b => {
  b.addEventListener('click', () => {
    state.currentLog.mood = b.dataset.mood;
    document.querySelectorAll('#mood-grid .mood-btn').forEach(o => o.classList.remove('selected'));
    b.classList.add('selected');
  });
});
document.querySelectorAll('#energy-track .energy-dot').forEach(d => {
  d.addEventListener('click', () => {
    const v = parseInt(d.dataset.energy);
    state.currentLog.energy = v;
    document.querySelectorAll('#energy-track .energy-dot').forEach(o => {
      o.classList.toggle('active', parseInt(o.dataset.energy) <= v);
    });
  });
});
document.getElementById('note-input').addEventListener('input', e => {
  state.currentLog.note = e.target.value;
});

function saveLog() {
  const key = dateKey(state.currentLogDate);
  state.logs[key] = { ...state.currentLog };
  tg?.HapticFeedback?.notificationOccurred?.('success');
  closeLogModal();
  renderMainCal();
  saveState();
  showToast('Saqlandi');
}

// ========== WATER ==========
function openWater() {
  if (!state.hasWater) {
    goTo('screen-water-setup');
  } else {
    navTo('screen-water');
    renderWater();
  }
}
function updateWeight(v) {
  state.weight = parseInt(v);
  document.getElementById('weight-value').textContent = v;
}
function completeWaterSetup() {
  state.hasWater = true;
  state.waterTarget = Math.round((state.weight * 30) / 100) / 10;  // L, rounded to 0.1
  navTo('screen-water');
  renderWater();
  saveState();
  showToast('Suv rejasi tayyor');
}
function renderWater() {
  const ml = state.waterToday * 250;
  const liters = (ml / 1000).toFixed(1);
  document.getElementById('water-now').textContent = liters;
  document.getElementById('water-target').textContent = state.waterTarget;
  document.getElementById('formula-weight').textContent = state.weight;
  document.getElementById('formula-target').textContent = state.waterTarget + 'L';
  document.getElementById('formula-glasses').textContent = Math.round((state.waterTarget * 1000) / 250);

  const pct = Math.min(1, (parseFloat(liters)) / state.waterTarget);
  const circumference = 2 * Math.PI * 42;
  const offset = circumference * (1 - pct);
  document.getElementById('water-ring-circle').setAttribute('stroke-dasharray', circumference);
  document.getElementById('water-ring-circle').setAttribute('stroke-dashoffset', offset);

  // Week bars - today is last position
  state.waterWeek[6] = state.waterToday;
  const targetGlasses = Math.round((state.waterTarget * 1000) / 250);
  const bars = document.getElementById('water-week-bars');
  const dows = ['Du','Se','Cho','Pa','Ju','Sh','Bugun'];
  bars.innerHTML = state.waterWeek.map((g, i) => {
    const pct = Math.min(100, (g / targetGlasses) * 100);
    return `<div class="week-bar">
      <div class="bar-track"><div class="bar-fill" style="height:${pct}%"></div></div>
      <div class="bar-day ${i === 6 ? 'today' : ''}">${dows[i]}</div>
    </div>`;
  }).join('');
}
function addGlass(delta) {
  state.waterToday = Math.max(0, state.waterToday + delta);
  tg?.HapticFeedback?.impactOccurred?.('light');
  renderWater();
  renderQuickCards();
  saveState();
  if (delta > 0 && state.waterToday * 250 / 1000 >= state.waterTarget) {
    tg?.HapticFeedback?.notificationOccurred?.('success');
    showToast('Kunlik me\'yorga yetdingiz ✿');
  }
}

// ========== SLEEP ==========
function openSleep() {
  if (!state.hasSleep) {
    goTo('screen-sleep-setup');
  } else {
    navTo('screen-sleep');
    renderSleep();
  }
}
function updateSleepGoal(v) {
  state.sleepGoal = parseFloat(v);
  document.getElementById('sleep-goal-value').textContent = v;
}
function completeSleepSetup() {
  state.hasSleep = true;
  // pre-fill with realistic data
  if (state.sleepWeek.every(v => v === 0)) {
    state.sleepWeek = [7.2, 6.8, 7.5, 8.1, 7.0, 7.5, 7.5];
  }
  navTo('screen-sleep');
  renderSleep();
  saveState();
  showToast('Uyqu kuzatuvi tayyor');
}

const bedTimes = ['22:00','22:30','23:00','23:30','00:00','00:30','01:00'];
const wakeTimes = ['05:30','06:00','06:30','07:00','07:30','08:00','08:30','09:00'];
let bedIdx = 3, wakeIdx = 3;
function cycleTime(which) {
  if (which === 'bed') {
    bedIdx = (bedIdx + 1) % bedTimes.length;
    document.getElementById('bed-time').textContent = bedTimes[bedIdx];
    state.bedTime = bedTimes[bedIdx];
  } else {
    wakeIdx = (wakeIdx + 1) % wakeTimes.length;
    document.getElementById('wake-time').textContent = wakeTimes[wakeIdx];
    state.wakeTime = wakeTimes[wakeIdx];
  }
}

function saveSleep() {
  // rough duration based on indexes
  const dur = 7.5 + (wakeIdx - 3) * 0.5 - (bedIdx - 3) * 0.5;
  state.sleepWeek[6] = Math.max(4, Math.min(11, dur));
  tg?.HapticFeedback?.notificationOccurred?.('success');
  renderSleep();
  saveState();
  showToast('Uyqu qayd qilindi');
}

function renderSleep() {
  const last = state.sleepWeek[6] || 7.5;
  const h = Math.floor(last);
  const m = Math.round((last - h) * 60);
  document.getElementById('last-sleep-hrs').textContent = h;
  document.getElementById('last-sleep-mins').textContent = m;
  document.getElementById('last-sleep-range').textContent = `${state.bedTime} — ${state.wakeTime}`;
  document.getElementById('bed-time').textContent = state.bedTime;
  document.getElementById('wake-time').textContent = state.wakeTime;

  const avg = state.sleepWeek.reduce((a, b) => a + b, 0) / state.sleepWeek.length;
  const ah = Math.floor(avg);
  const am = Math.round((avg - ah) * 60);
  document.getElementById('sleep-avg').textContent = `${ah}s ${am}d`;
  document.getElementById('sleep-goal-display').textContent = `${state.sleepGoal}s`;

  const bars = document.getElementById('sleep-week-bars');
  const dows = ['Du','Se','Cho','Pa','Ju','Sh','Bugun'];
  const max = 11;
  bars.innerHTML = state.sleepWeek.map((s, i) => {
    const pct = (s / max) * 100;
    return `<div class="sleep-bar">
      <div class="sleep-bar-fill" style="height:${pct}%"></div>
      <div class="sleep-bar-label">${dows[i]}</div>
    </div>`;
  }).join('');
}

// ========== RESET ==========
function resetDemo() {
  if (confirm('Prototipni qayta boshlaysizmi? Barcha sinov ma\'lumotlari o\'chiriladi.')) {
    if (CLOUD) {
      try { CLOUD.removeItem('lumen_state'); } catch(e) {}
    }
    try { localStorage.removeItem('lumen_state'); } catch(e) {}
    location.reload();
  }
}
</script>

</body>
</html>
