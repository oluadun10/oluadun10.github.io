<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Prior Auth IDP Dashboard — Ochsner Health</title>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500&family=Sora:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<style>
:root {
  --bg: #0A0F14;
  --surface: #111820;
  --surface2: #162030;
  --border: rgba(255,255,255,0.07);
  --border2: rgba(255,255,255,0.12);
  --text: #E8EDF2;
  --text-soft: #7A9AB5;
  --text-muted: #3D5A72;
  --accent: #00C8A0;
  --accent2: #0094FF;
  --warn: #FF8A3D;
  --danger: #FF4D6A;
  --mono: 'IBM Plex Mono', monospace;
  --sans: 'Sora', sans-serif;
}

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

body {
  font-family: var(--sans);
  background: var(--bg);
  color: var(--text);
  min-height: 100vh;
  font-size: 14px;
  line-height: 1.5;
}

/* ── GRID SCAN LINE EFFECT ── */
body::before {
  content: '';
  position: fixed;
  inset: 0;
  background: repeating-linear-gradient(
    0deg,
    transparent,
    transparent 2px,
    rgba(255,255,255,0.008) 2px,
    rgba(255,255,255,0.008) 4px
  );
  pointer-events: none;
  z-index: 1000;
}

/* ── TOPBAR ── */
.topbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 1.5rem;
  height: 52px;
  border-bottom: 1px solid var(--border);
  background: var(--surface);
  position: sticky;
  top: 0;
  z-index: 100;
}

.topbar-left {
  display: flex;
  align-items: center;
  gap: 1.5rem;
}

.logo {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  font-family: var(--mono);
  font-size: 0.8rem;
  color: var(--text);
  letter-spacing: 0.06em;
}

.logo-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--accent);
  box-shadow: 0 0 8px var(--accent);
  animation: pulse-dot 2s ease-in-out infinite;
}

@keyframes pulse-dot {
  0%, 100% { opacity: 1; box-shadow: 0 0 8px var(--accent); }
  50% { opacity: 0.6; box-shadow: 0 0 16px var(--accent); }
}

.breadcrumb {
  font-family: var(--mono);
  font-size: 0.72rem;
  color: var(--text-muted);
  display: flex;
  align-items: center;
  gap: 0.4rem;
}

.breadcrumb span { color: var(--text-soft); }

.topbar-right {
  display: flex;
  align-items: center;
  gap: 1rem;
}

● Live-badge {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  font-family: var(--mono);
  font-size: 0.7rem;
  color: var(--accent);
  letter-spacing: 0.08em;
}

.LIVE-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--accent);
  animation: pulse-dot 1.5s ease-in-out infinite;
}

.user-chip {
  font-size: 0.75rem;
  color: var(--text-soft);
  background: var(--surface2);
  border: 1px solid var(--border2);
  border-radius: 20px;
  padding: 0.3rem 0.75rem;
}

/* ── LAYOUT ── */
.layout {
  display: grid;
  grid-template-columns: 200px 1fr;
  min-height: calc(100vh - 52px);
}

/* ── SIDEBAR ── */
.sidebar {
  background: var(--surface);
  border-right: 1px solid var(--border);
  padding: 1.5rem 0;
}

.nav-section {
  padding: 0 1rem;
  margin-bottom: 1.5rem;
}

.nav-label {
  font-family: var(--mono);
  font-size: 0.65rem;
  color: var(--text-muted);
  text-transform: uppercase;
  letter-spacing: 0.1em;
  padding: 0 0.5rem;
  margin-bottom: 0.5rem;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  padding: 0.5rem 0.75rem;
  border-radius: 6px;
  font-size: 0.82rem;
  color: var(--text-soft);
  cursor: pointer;
  transition: all 0.15s;
  margin-bottom: 2px;
}

.nav-item:hover { background: var(--surface2); color: var(--text); }
.nav-item.active { background: rgba(0,200,160,0.1); color: var(--accent); border: 1px solid rgba(0,200,160,0.2); }
.nav-item .icon { font-size: 0.9rem; width: 16px; text-align: center; }

.nav-badge {
  margin-left: auto;
  background: var(--danger);
  color: white;
  font-family: var(--mono);
  font-size: 0.6rem;
  padding: 0.1rem 0.35rem;
  border-radius: 10px;
  font-weight: 500;
}

/* ── MAIN ── */
.main {
  overflow-y: auto;
  background: var(--bg);
}

.page-header {
  padding: 1.5rem 2rem 1rem;
  border-bottom: 1px solid var(--border);
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
}

.page-title {
  font-size: 1.1rem;
  font-weight: 600;
  margin-bottom: 0.2rem;
}

.page-sub {
  font-size: 0.78rem;
  color: var(--text-muted);
  font-family: var(--mono);
}

.header-controls {
  display: flex;
  gap: 0.75rem;
  align-items: center;
}

.btn {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  padding: 0.45rem 0.9rem;
  font-size: 0.78rem;
  font-family: var(--sans);
  font-weight: 500;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.15s;
  border: none;
}

.btn-ghost {
  background: var(--surface2);
  color: var(--text-soft);
  border: 1px solid var(--border2);
}
.btn-ghost:hover { border-color: var(--accent); color: var(--accent); }

.btn-accent {
  background: var(--accent);
  color: #0A0F14;
  font-weight: 600;
}
.btn-accent:hover { opacity: 0.88; }

/* ── CONTENT ── */
.content { padding: 1.5rem 2rem; }

/* ── KPI ROW ── */
.kpi-row {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1rem;
  margin-bottom: 1.5rem;
}

.kpi {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 10px;
  padding: 1.1rem 1.25rem;
  position: relative;
  overflow: hidden;
  transition: border-color 0.2s;
}

.kpi:hover { border-color: var(--border2); }

.kpi::after {
  content: '';
  position: absolute;
  bottom: 0; left: 0; right: 0;
  height: 2px;
}

.kpi.green::after { background: var(--accent); }
.kpi.blue::after { background: var(--accent2); }
.kpi.warn::after { background: var(--warn); }
.kpi.danger::after { background: var(--danger); }

.kpi-label {
  font-family: var(--mono);
  font-size: 0.65rem;
  color: var(--text-muted);
  text-transform: uppercase;
  letter-spacing: 0.1em;
  margin-bottom: 0.5rem;
}

.kpi-value {
  font-family: var(--mono);
  font-size: 1.8rem;
  font-weight: 500;
  line-height: 1;
  margin-bottom: 0.4rem;
}

.kpi.green .kpi-value { color: var(--accent); }
.kpi.blue .kpi-value { color: var(--accent2); }
.kpi.warn .kpi-value { color: var(--warn); }
.kpi.danger .kpi-value { color: var(--danger); }

.kpi-delta {
  font-size: 0.72rem;
  display: flex;
  align-items: center;
  gap: 0.3rem;
}

.delta-up { color: var(--accent); }
.delta-down { color: var(--danger); }

/* ── GRID ── */
.grid-2 {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  margin-bottom: 1rem;
}

.grid-3-1 {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: 1rem;
  margin-bottom: 1rem;
}

/* ── CARD ── */
.card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 10px;
  overflow: hidden;
}

.card-header {
  padding: 1rem 1.25rem 0.75rem;
  border-bottom: 1px solid var(--border);
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.card-title {
  font-size: 0.82rem;
  font-weight: 600;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.card-meta {
  font-family: var(--mono);
  font-size: 0.65rem;
  color: var(--text-muted);
}

.card-body { padding: 1.25rem; }

/* ── CHART ── */
.chart-area {
  height: 160px;
  position: relative;
  display: flex;
  align-items: flex-end;
  gap: 6px;
  padding-top: 1rem;
}

.chart-bar-wrap {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  height: 100%;
  justify-content: flex-end;
}

.chart-bar {
  width: 100%;
  border-radius: 3px 3px 0 0;
  transition: height 1s cubic-bezier(0.34, 1.56, 0.64, 1);
  position: relative;
  cursor: pointer;
}

.chart-bar.before { background: rgba(255,138,61,0.4); border: 1px solid rgba(255,138,61,0.6); }
.chart-bar.after { background: rgba(0,200,160,0.4); border: 1px solid rgba(0,200,160,0.6); }
.chart-bar:hover { filter: brightness(1.3); }

.chart-bar .tooltip {
  position: absolute;
  bottom: calc(100% + 6px);
  left: 50%;
  transform: translateX(-50%);
  background: var(--surface2);
  border: 1px solid var(--border2);
  border-radius: 4px;
  padding: 0.3rem 0.5rem;
  font-family: var(--mono);
  font-size: 0.65rem;
  white-space: nowrap;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.15s;
  z-index: 10;
}

.chart-bar:hover .tooltip { opacity: 1; }

.chart-label {
  font-family: var(--mono);
  font-size: 0.6rem;
  color: var(--text-muted);
}

.chart-legend {
  display: flex;
  gap: 1rem;
  margin-top: 0.75rem;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 0.35rem;
  font-size: 0.72rem;
  color: var(--text-soft);
}

.legend-dot {
  width: 8px;
  height: 8px;
  border-radius: 2px;
}

/* ── QUEUE TABLE ── */
.queue-table {
  width: 100%;
  border-collapse: collapse;
}

.queue-table th {
  text-align: left;
  font-family: var(--mono);
  font-size: 0.62rem;
  color: var(--text-muted);
  text-transform: uppercase;
  letter-spacing: 0.08em;
  padding: 0.6rem 1rem;
  border-bottom: 1px solid var(--border);
  font-weight: 400;
}

.queue-table td {
  padding: 0.75rem 1rem;
  border-bottom: 1px solid var(--border);
  font-size: 0.8rem;
  vertical-align: middle;
}

.queue-table tr:last-child td { border-bottom: none; }
.queue-table tr:hover td { background: rgba(255,255,255,0.02); }

.status-chip {
  display: inline-flex;
  align-items: center;
  gap: 0.3rem;
  padding: 0.2rem 0.55rem;
  border-radius: 4px;
  font-family: var(--mono);
  font-size: 0.65rem;
  font-weight: 500;
}

.status-chip::before {
  content: '';
  width: 5px;
  height: 5px;
  border-radius: 50%;
}

.s-auto { background: rgba(0,200,160,0.1); color: var(--accent); border: 1px solid rgba(0,200,160,0.2); }
.s-auto::before { background: var(--accent); }
.s-review { background: rgba(255,138,61,0.1); color: var(--warn); border: 1px solid rgba(255,138,61,0.2); }
.s-review::before { background: var(--warn); }
.s-denied { background: rgba(255,77,106,0.1); color: var(--danger); border: 1px solid rgba(255,77,106,0.2); }
.s-denied::before { background: var(--danger); }

.confidence-bar {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.conf-track {
  width: 60px;
  height: 4px;
  background: var(--surface2);
  border-radius: 2px;
  overflow: hidden;
}

.conf-fill {
  height: 100%;
  border-radius: 2px;
}

.conf-high { background: var(--accent); }
.conf-med { background: var(--warn); }
.conf-low { background: var(--danger); }

.conf-pct {
  font-family: var(--mono);
  font-size: 0.68rem;
  color: var(--text-soft);
  width: 30px;
}

/* ── SAVINGS COUNTER ── */
.savings-display {
  text-align: center;
  padding: 1.5rem 1rem;
}

.savings-num {
  font-family: var(--mono);
  font-size: 2.8rem;
  font-weight: 500;
  color: var(--accent);
  line-height: 1;
  margin-bottom: 0.25rem;
  text-shadow: 0 0 30px rgba(0,200,160,0.3);
}

.savings-label {
  font-size: 0.72rem;
  color: var(--text-muted);
  text-transform: uppercase;
  letter-spacing: 0.1em;
  font-family: var(--mono);
  margin-bottom: 1.25rem;
}

.savings-breakdown {
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
}

.breakdown-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 0.75rem;
  padding: 0.5rem 0.75rem;
  background: var(--surface2);
  border-radius: 6px;
  border: 1px solid var(--border);
}

.breakdown-label { color: var(--text-soft); }
.breakdown-val {
  font-family: var(--mono);
  color: var(--accent);
  font-weight: 500;
}

/* ── PROCESSING GAUGE ── */
.gauge-wrap {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 1rem;
}

.gauge-svg { overflow: visible; }

.gauge-track { fill: none; stroke: var(--surface2); stroke-width: 10; stroke-linecap: round; }
.gauge-fill-before { fill: none; stroke: var(--warn); stroke-width: 10; stroke-linecap: round; transition: stroke-dasharray 1.5s ease; }
.gauge-fill-after { fill: none; stroke: var(--accent); stroke-width: 10; stroke-linecap: round; transition: stroke-dasharray 1.5s ease; }

.gauge-center {
  text-anchor: middle;
}

.gauge-pct {
  font-family: var(--mono);
  font-size: 1.6rem;
  fill: var(--accent);
  font-weight: 500;
}

.gauge-sub {
  font-family: var(--mono);
  font-size: 0.55rem;
  fill: var(--text-muted);
  letter-spacing: 0.08em;
}

.gauge-labels {
  display: flex;
  justify-content: space-between;
  width: 180px;
  margin-top: 0.5rem;
}

.gauge-lbl {
  font-family: var(--mono);
  font-size: 0.65rem;
  text-align: center;
}

/* ── TOGGLE ── */
.view-toggle {
  display: flex;
  background: var(--surface2);
  border: 1px solid var(--border);
  border-radius: 6px;
  overflow: hidden;
}

.toggle-btn {
  padding: 0.35rem 0.75rem;
  font-size: 0.72rem;
  font-family: var(--mono);
  cursor: pointer;
  border: none;
  background: transparent;
  color: var(--text-muted);
  transition: all 0.15s;
}

.toggle-btn.active {
  background: var(--accent);
  color: #0A0F14;
  font-weight: 600;
}

/* ── WATERMARK ── */
.prototype-bar {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(0,200,160,0.08);
  border-top: 1px solid rgba(0,200,160,0.2);
  padding: 0.4rem 1.5rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
  z-index: 200;
  backdrop-filter: blur(8px);
}

.prototype-label {
  font-family: var(--mono);
  font-size: 0.68rem;
  color: var(--accent);
  letter-spacing: 0.08em;
}

.prototype-link {
  font-family: var(--mono);
  font-size: 0.68rem;
  color: var(--text-muted);
  text-decoration: none;
}

.prototype-link:hover { color: var(--accent); }

/* ── ANIMATIONS ── */
@keyframes count-up {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

.kpi { animation: count-up 0.5s ease forwards; }
.kpi:nth-child(1) { animation-delay: 0.1s; opacity: 0; }
.kpi:nth-child(2) { animation-delay: 0.2s; opacity: 0; }
.kpi:nth-child(3) { animation-delay: 0.3s; opacity: 0; }
.kpi:nth-child(4) { animation-delay: 0.4s; opacity: 0; }
</style>
</head>
<body>

<!-- TOPBAR -->
<div class="topbar">
  <div class="topbar-left">
    <div class="logo">
      <div class="logo-dot"></div>
      OCHSNER · IDP PLATFORM
    </div>
    <div class="breadcrumb">
      Operations / <span>Prior Authorization</span> / <span>IDP Dashboard</span>
    </div>
  </div>
  <div class="topbar-right">
    <div class="live-badge"><div class="live-dot"></div> LIVE</div>
    <div class="user-chip">O. Adun · PM</div>
  </div>
</div>

<!-- LAYOUT -->
<div class="layout">

  <!-- SIDEBAR -->
  <nav class="sidebar">
    <div class="nav-section">
      <div class="nav-label">Overview</div>
      <div class="nav-item active"><span class="icon">◈</span> Dashboard</div>
      <div class="nav-item"><span class="icon">⊞</span> Analytics</div>
      <div class="nav-item"><span class="icon">◎</span> Reports</div>
    </div>
    <div class="nav-section">
      <div class="nav-label">Processing</div>
      <div class="nav-item"><span class="icon">⟳</span> Review Queue <span class="nav-badge">7</span></div>
      <div class="nav-item"><span class="icon">✓</span> Approved</div>
      <div class="nav-item"><span class="icon">✕</span> Denied</div>
      <div class="nav-item"><span class="icon">⚑</span> Escalated</div>
    </div>
    <div class="nav-section">
      <div class="nav-label">System</div>
      <div class="nav-item"><span class="icon">⚙</span> Model Config</div>
      <div class="nav-item"><span class="icon">⊟</span> Audit Log</div>
      <div class="nav-item"><span class="icon">⊕</span> Integrations</div>
    </div>
  </nav>

  <!-- MAIN -->
  <main class="main" style="padding-bottom: 3rem;">
    <div class="page-header">
      <div>
        <div class="page-title">Prior Authorization — IDP Overview</div>
        <div class="page-sub">FY2024 · Updated 4 minutes ago · Confidence threshold: 85%</div>
      </div>
      <div class="header-controls">
        <div class="view-toggle">
          <button class="toggle-btn active" onclick="setView(this,'ytd')">YTD</button>
          <button class="toggle-btn" onclick="setView(this,'q4')">Q4</button>
          <button class="toggle-btn" onclick="setView(this,'30d')">30D</button>
        </div>
        <button class="btn btn-ghost">↓ Export</button>
        <button class="btn btn-accent">+ New Review</button>
      </div>
    </div>

    <div class="content">

      <!-- KPIs -->
      <div class="kpi-row">
        <div class="kpi green">
          <div class="kpi-label">Cumulative Savings</div>
          <div class="kpi-value" id="kpi-savings">$3M</div>
          <div class="kpi-delta delta-up">↑ +$240K vs prior period</div>
        </div>
        <div class="kpi blue">
          <div class="kpi-label">Avg Processing Time</div>
          <div class="kpi-value" id="kpi-time">1.4h</div>
          <div class="kpi-delta delta-up">↓ 28% reduction from baseline</div>
        </div>
        <div class="kpi green">
          <div class="kpi-label">Auto-Approved Rate</div>
          <div class="kpi-value">78%</div>
          <div class="kpi-delta delta-up">↑ +12pp since launch</div>
        </div>
        <div class="kpi warn">
          <div class="kpi-label">Pending Human Review</div>
          <div class="kpi-value">7</div>
          <div class="kpi-delta">Avg wait: 22 min</div>
        </div>
      </div>

      <!-- ROW 1 -->
      <div class="grid-3-1">

        <!-- PROCESSING TIME CHART -->
        <div class="card">
          <div class="card-header">
            <div class="card-title">⟳ Processing Time — Before vs After IDP</div>
            <div class="card-meta">Hours per authorization · Monthly</div>
          </div>
          <div class="card-body">
            <div class="chart-area" id="chart"></div>
            <div class="chart-legend">
              <div class="legend-item"><div class="legend-dot" style="background:rgba(255,138,61,0.6)"></div>Before IDP</div>
              <div class="legend-item"><div class="legend-dot" style="background:rgba(0,200,160,0.6)"></div>After IDP</div>
            </div>
          </div>
        </div>

        <!-- SAVINGS -->
        <div class="card">
          <div class="card-header">
            <div class="card-title">◈ Cost Savings</div>
            <div class="card-meta">Cumulative FY2024</div>
          </div>
          <div class="card-body" style="padding:0">
            <div class="savings-display">
              <div class="savings-num" id="savings-counter">$0</div>
              <div class="savings-label">Total Savings Delivered</div>
              <div class="savings-breakdown">
                <div class="breakdown-row">
                  <span class="breakdown-label">FTE hours eliminated</span>
                  <span class="breakdown-val">14,200 hrs</span>
                </div>
                <div class="breakdown-row">
                  <span class="breakdown-label">Avg loaded labor rate</span>
                  <span class="breakdown-val">$58/hr</span>
                </div>
                <div class="breakdown-row">
                  <span class="breakdown-label">Error rework avoided</span>
                  <span class="breakdown-val">$340K</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- ROW 2 -->
      <div class="grid-2">

        <!-- QUEUE -->
        <div class="card">
          <div class="card-header">
            <div class="card-title">⟳ Live Authorization Queue</div>
            <div class="card-meta">Real-time · 7 pending review</div>
          </div>
          <table class="queue-table">
            <thead>
              <tr>
                <th>Auth ID</th>
                <th>Type</th>
                <th>Confidence</th>
                <th>Status</th>
                <th>Time</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td style="font-family:var(--mono);color:var(--text-soft)">PA-2024-8821</td>
                <td>MRI Lumbar</td>
                <td>
                  <div class="confidence-bar">
                    <div class="conf-track"><div class="conf-fill conf-high" style="width:94%"></div></div>
                    <span class="conf-pct">94%</span>
                  </div>
                </td>
                <td><span class="status-chip s-auto">Auto-approved</span></td>
                <td style="font-family:var(--mono);font-size:0.72rem;color:var(--text-muted)">0.8h</td>
              </tr>
              <tr>
                <td style="font-family:var(--mono);color:var(--text-soft)">PA-2024-8820</td>
                <td>Biologics Rx</td>
                <td>
                  <div class="confidence-bar">
                    <div class="conf-track"><div class="conf-fill conf-med" style="width:71%"></div></div>
                    <span class="conf-pct">71%</span>
                  </div>
                </td>
                <td><span class="status-chip s-review">Human review</span></td>
                <td style="font-family:var(--mono);font-size:0.72rem;color:var(--text-muted)">1.2h</td>
              </tr>
              <tr>
                <td style="font-family:var(--mono);color:var(--text-soft)">PA-2024-8819</td>
                <td>PT — 12 visits</td>
                <td>
                  <div class="confidence-bar">
                    <div class="conf-track"><div class="conf-fill conf-high" style="width:91%"></div></div>
                    <span class="conf-pct">91%</span>
                  </div>
                </td>
                <td><span class="status-chip s-auto">Auto-approved</span></td>
                <td style="font-family:var(--mono);font-size:0.72rem;color:var(--text-muted)">0.6h</td>
              </tr>
              <tr>
                <td style="font-family:var(--mono);color:var(--text-soft)">PA-2024-8818</td>
                <td>Cardiac Cath</td>
                <td>
                  <div class="confidence-bar">
                    <div class="conf-track"><div class="conf-fill conf-low" style="width:48%"></div></div>
                    <span class="conf-pct">48%</span>
                  </div>
                </td>
                <td><span class="status-chip s-review">Human review</span></td>
                <td style="font-family:var(--mono);font-size:0.72rem;color:var(--text-muted)">2.1h</td>
              </tr>
              <tr>
                <td style="font-family:var(--mono);color:var(--text-soft)">PA-2024-8817</td>
                <td>Spinal Fusion</td>
                <td>
                  <div class="confidence-bar">
                    <div class="conf-track"><div class="conf-fill conf-high" style="width:88%"></div></div>
                    <span class="conf-pct">88%</span>
                  </div>
                </td>
                <td><span class="status-chip s-denied">Denied</span></td>
                <td style="font-family:var(--mono);font-size:0.72rem;color:var(--text-muted)">1.9h</td>
              </tr>
            </tbody>
          </table>
        </div>

        <!-- GAUGE + BREAKDOWN -->
        <div class="card">
          <div class="card-header">
            <div class="card-title">◎ Processing Time Reduction</div>
            <div class="card-meta">Baseline vs current · Hours avg</div>
          </div>
          <div class="card-body">
            <div class="gauge-wrap">
              <svg class="gauge-svg" width="180" height="110" viewBox="0 0 180 110">
                <path class="gauge-track" d="M 20 100 A 70 70 0 0 1 160 100"/>
                <path class="gauge-fill-before" id="gauge-before" d="M 20 100 A 70 70 0 0 1 160 100" stroke-dasharray="0 220"/>
                <path class="gauge-fill-after" id="gauge-after" d="M 20 100 A 70 70 0 0 1 160 100" stroke-dasharray="0 220"/>
                <text class="gauge-center" x="90" y="85">
                  <tspan class="gauge-pct" x="90" dy="0">28%</tspan>
                  <tspan class="gauge-sub" x="90" dy="18">REDUCTION</tspan>
                </text>
              </svg>
              <div class="gauge-labels">
                <div class="gauge-lbl" style="color:var(--warn)">Before<br><span style="font-family:var(--mono)">~8.2h</span></div>
                <div class="gauge-lbl" style="color:var(--accent)">After<br><span style="font-family:var(--mono)">~1.4h</span></div>
              </div>
            </div>
            <div class="savings-breakdown" style="margin-top:1rem">
              <div class="breakdown-row">
                <span class="breakdown-label">Auto-approval rate</span>
                <span class="breakdown-val">78%</span>
              </div>
              <div class="breakdown-row">
                <span class="breakdown-label">Human review rate</span>
                <span class="breakdown-val" style="color:var(--warn)">22%</span>
              </div>
              <div class="breakdown-row">
                <span class="breakdown-label">SLA compliance</span>
                <span class="breakdown-val">96%</span>
              </div>
              <div class="breakdown-row">
                <span class="breakdown-label">Model confidence avg</span>
                <span class="breakdown-val">91.4%</span>
              </div>
            </div>
          </div>
        </div>

      </div>
    </div>
  </main>
</div>

<!-- PROTOTYPE BAR -->
<div class="prototype-bar">
  <span class="prototype-label">⬡ PROTOTYPE — IDP Prior Auth Dashboard · Ochsner Health · PM: Olu Adun</span>
  <a href="../index.html" class="prototype-link">← Back to portfolio</a>
</div>

<script>
// Chart data
const months = ['Jul','Aug','Sep','Oct','Nov','Dec','Jan','Feb','Mar','Apr','May','Jun'];
const before = [8.1,8.4,8.0,7.9,8.2,8.3,5.2,4.1,3.2,2.1,1.6,1.4];
const after =  [null,null,null,null,null,null,5.2,4.1,3.2,2.1,1.6,1.4];

const maxVal = 10;

function buildChart() {
  const chart = document.getElementById('chart');
  chart.innerHTML = '';
  months.forEach((m, i) => {
    const wrap = document.createElement('div');
    wrap.className = 'chart-bar-wrap';

    if (after[i] !== null) {
      const bar = document.createElement('div');
      bar.className = 'chart-bar after';
      bar.style.height = '0px';
      bar.innerHTML = `<div class="tooltip">${m}: ${after[i]}h after IDP</div>`;
      wrap.appendChild(bar);
      setTimeout(() => {
        bar.style.height = (after[i] / maxVal * 130) + 'px';
      }, 100 + i * 60);
    } else {
      const bar = document.createElement('div');
      bar.className = 'chart-bar before';
      bar.style.height = '0px';
      bar.innerHTML = `<div class="tooltip">${m}: ${before[i]}h baseline</div>`;
      wrap.appendChild(bar);
      setTimeout(() => {
        bar.style.height = (before[i] / maxVal * 130) + 'px';
      }, 100 + i * 60);
    }

    const lbl = document.createElement('div');
    lbl.className = 'chart-label';
    lbl.textContent = m;
    wrap.appendChild(lbl);
    chart.appendChild(wrap);
  });
}

// Savings counter animation
function animateSavings() {
  const target = 3040000;
  const el = document.getElementById('savings-counter');
  const duration = 2000;
  const start = performance.now();
  
  function update(now) {
    const elapsed = now - start;
    const progress = Math.min(elapsed / duration, 1);
    const eased = 1 - Math.pow(1 - progress, 3);
    const val = Math.floor(eased * target);
    el.textContent = '$' + (val / 1000000).toFixed(2) + 'M';
    if (progress < 1) requestAnimationFrame(update);
    else el.textContent = '$3.04M';
  }
  
  setTimeout(() => requestAnimationFrame(update), 600);
}

// View toggle
function setView(btn, view) {
  document.querySelectorAll('.toggle-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
}

buildChart();
animateSavings();
</script>
</body>
</html>
