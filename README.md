<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Skillairo HR Analytics & Executive Dashboard</title>

<!-- Dependencies -->
<script src="https://cdn.tailwindcss.com"></script>
<script src="https://unpkg.com/lucide@latest"></script>
<script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<!-- Fonts -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@500;600&display=swap" rel="stylesheet">

<style>
  :root {
    --navy: #1E1B4B;
    --navy-2: #2D2A6E;
    --indigo: #4F46E5;
    --indigo-soft: #EEF2FF;
    --slate: #F8FAFC;
    --emerald: #10B981;
    --rose: #EF4444;
    --amber: #F59E0B;
    --border: #E2E8F0;
  }
  * { -webkit-font-smoothing: antialiased; }
  html, body { font-family: 'Inter', system-ui, sans-serif; background: var(--slate); color: #0F172A; }
  .font-display { font-family: 'Plus Jakarta Sans', sans-serif; letter-spacing: -0.01em; }
  .font-mono { font-family: 'JetBrains Mono', monospace; }

  /* Background pattern */
  .navy-pattern {
    background-color: var(--navy);
    background-image:
      radial-gradient(circle at 20% 30%, rgba(79,70,229,0.25) 0%, transparent 45%),
      radial-gradient(circle at 80% 70%, rgba(16,185,129,0.12) 0%, transparent 40%),
      linear-gradient(135deg, #1E1B4B 0%, #2D2A6E 100%);
  }
  .grid-pattern {
    background-image:
      linear-gradient(rgba(255,255,255,0.04) 1px, transparent 1px),
      linear-gradient(90deg, rgba(255,255,255,0.04) 1px, transparent 1px);
    background-size: 32px 32px;
  }

  /* Tabs */
  .tab-btn { transition: all .25s ease; }
  .tab-btn.active {
    background: var(--navy);
    color: white;
    box-shadow: 0 8px 24px -8px rgba(30,27,75,0.5);
  }
  .tab-btn.active .tab-num { background: var(--indigo); color: white; }
  .tab-btn:not(.active):hover { background: white; color: var(--navy); }

  /* Cards */
  .kpi-card {
    transition: transform .3s ease, box-shadow .3s ease;
    position: relative;
    overflow: hidden;
  }
  .kpi-card:hover { transform: translateY(-3px); box-shadow: 0 18px 40px -18px rgba(30,27,75,0.25); }
  .kpi-card::before {
    content: ''; position: absolute; top: 0; left: 0; right: 0; height: 3px;
    background: var(--accent, var(--indigo));
  }

  /* Status badges */
  .badge { display: inline-flex; align-items: center; gap: 4px; padding: 2px 10px; border-radius: 999px; font-size: 11px; font-weight: 600; }
  .badge-active { background: #ECFDF5; color: #047857; }
  .badge-terminated { background: #FEF2F2; color: #B91C1C; }
  .badge-yes { background: #EFF6FF; color: #1D4ED8; }
  .badge-no { background: #FFF7ED; color: #C2410C; }

  /* Table */
  .data-table th { background: #F1F5F9; font-weight: 600; font-size: 11px; text-transform: uppercase; letter-spacing: 0.05em; color: #475569; }
  .data-table tbody tr { transition: background .15s; }
  .data-table tbody tr:hover { background: #FAFBFF; }

  /* Dropzone */
  .dropzone { transition: all .25s ease; border: 2px dashed #CBD5E1; }
  .dropzone.dragging { border-color: var(--indigo); background: var(--indigo-soft); transform: scale(1.01); }

  /* Tab content fade */
  .tab-panel { animation: fadeUp .4s ease; }
  @keyframes fadeUp { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }

  /* Pulse dot */
  .pulse-dot {
    width: 8px; height: 8px; border-radius: 50%; background: var(--emerald);
    box-shadow: 0 0 0 0 rgba(16,185,129,0.7); animation: pulse 2s infinite;
  }
  @keyframes pulse {
    0% { box-shadow: 0 0 0 0 rgba(16,185,129,0.7); }
    70% { box-shadow: 0 0 0 8px rgba(16,185,129,0); }
    100% { box-shadow: 0 0 0 0 rgba(16,185,129,0); }
  }

  /* 4Cs card */
  .cs-card { transition: all .3s ease; cursor: default; }
  .cs-card:hover { transform: translateY(-4px); }
  .cs-card .cs-icon { transition: transform .3s ease; }
  .cs-card:hover .cs-icon { transform: scale(1.1) rotate(-5deg); }

  /* Survey radio */
  .likert-option input { display: none; }
  .likert-option label {
    display: flex; align-items: center; justify-content: center;
    width: 36px; height: 36px; border-radius: 8px;
    border: 1.5px solid #E2E8F0; cursor: pointer; font-weight: 600; font-size: 13px;
    transition: all .2s ease; background: white;
  }
  .likert-option label:hover { border-color: var(--indigo); color: var(--indigo); }
  .likert-option input:checked + label {
    background: var(--navy); color: white; border-color: var(--navy);
    transform: scale(1.08);
  }

  /* Scrollbar */
  ::-webkit-scrollbar { width: 8px; height: 8px; }
  ::-webkit-scrollbar-track { background: transparent; }
  ::-webkit-scrollbar-thumb { background: #CBD5E1; border-radius: 4px; }
  ::-webkit-scrollbar-thumb:hover { background: #94A3B8; }

  /* Roadmap phase */
  .phase-row { transition: all .2s; }
  .phase-row:hover { background: #FAFBFF; padding-left: 8px; }

  /* Insight card glow */
  .insight-card {
    position: relative; overflow: hidden;
    transition: transform .3s ease;
  }
  .insight-card:hover { transform: translateY(-3px); }
  .insight-card::after {
    content: ''; position: absolute; top: -50%; right: -20%;
    width: 200px; height: 200px; border-radius: 50%;
    background: radial-gradient(circle, var(--glow,rgba(79,70,229,0.08)) 0%, transparent 70%);
    pointer-events: none;
  }

  /* Print */
  @media print {
    .no-print { display: none !important; }
  }
</style>
</head>

<body class="min-h-screen">

<!-- ============ HEADER ============ -->
<header class="navy-pattern text-white relative overflow-hidden">
  <div class="grid-pattern absolute inset-0 opacity-60"></div>
  <div class="relative max-w-[1400px] mx-auto px-6 lg:px-10 py-8">
    <!-- Top row: Brand + Live status -->
    <div class="flex flex-wrap items-center justify-between gap-4 mb-6">
      <div class="flex items-center gap-3">
        <div class="w-11 h-11 rounded-xl bg-gradient-to-br from-indigo-500 to-indigo-700 flex items-center justify-center shadow-lg shadow-indigo-900/40">
          <i data-lucide="hexagon" class="w-6 h-6 text-white"></i>
        </div>
        <div>
          <div class="font-display font-extrabold text-lg leading-none">SKILLAIRO</div>
          <div class="text-[11px] text-indigo-200 tracking-wider uppercase mt-1">Human Resources & Operations</div>
        </div>
      </div>
      <div class="flex items-center gap-3">
        <div class="hidden sm:flex items-center gap-2 px-3 py-1.5 rounded-full bg-white/10 backdrop-blur border border-white/15">
          <span class="pulse-dot"></span>
          <span class="text-xs font-medium text-indigo-100">Live Data Engine</span>
        </div>
        <div class="hidden md:flex items-center gap-2 px-3 py-1.5 rounded-full bg-white/10 backdrop-blur border border-white/15">
          <i data-lucide="calendar-range" class="w-3.5 h-3.5 text-indigo-200"></i>
          <span class="text-xs font-medium text-indigo-100">Session 2026–2027</span>
        </div>
      </div>
    </div>

    <!-- Title -->
    <div class="max-w-3xl">
      <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-indigo-500/20 border border-indigo-400/30 text-indigo-200 text-xs font-medium mb-4">
        <i data-lucide="briefcase" class="w-3.5 h-3.5"></i>
        Corporate HR Analytics & Executive Dashboard
      </div>
      <h1 class="font-display font-extrabold text-3xl md:text-4xl lg:text-5xl leading-tight mb-3">
        Workforce Intelligence <span class="text-indigo-300">&</span> Onboarding Strategy
      </h1>
      <p class="text-indigo-100/80 text-sm md:text-base max-w-2xl leading-relaxed">
        A unified analytical command center covering attrition diagnostics, onboarding architecture, peer-buddy mentorship, stakeholder governance, and actionable HR recommendations for Skillairo leadership.
      </p>
    </div>

    <!-- Metadata strip -->
    <div class="mt-7 grid grid-cols-2 md:grid-cols-4 gap-3">
      <div class="bg-white/5 backdrop-blur border border-white/10 rounded-xl px-4 py-3">
        <div class="text-[10px] uppercase tracking-wider text-indigo-300 mb-1">Project Lead</div>
        <div class="font-display font-semibold text-sm">Syed Subhana Ali</div>
        <div class="text-[11px] text-indigo-200/70 mt-0.5">HR Intern</div>
      </div>
      <div class="bg-white/5 backdrop-blur border border-white/10 rounded-xl px-4 py-3">
        <div class="text-[10px] uppercase tracking-wider text-indigo-300 mb-1">Department</div>
        <div class="font-display font-semibold text-sm">HR & Operations</div>
        <div class="text-[11px] text-indigo-200/70 mt-0.5">Skillairo Corporate</div>
      </div>
      <div class="bg-white/5 backdrop-blur border border-white/10 rounded-xl px-4 py-3">
        <div class="text-[10px] uppercase tracking-wider text-indigo-300 mb-1">Academic Session</div>
        <div class="font-display font-semibold text-sm">2026 – 2027</div>
        <div class="text-[11px] text-indigo-200/70 mt-0.5">Internship Cycle</div>
      </div>
      <div class="bg-white/5 backdrop-blur border border-white/10 rounded-xl px-4 py-3">
        <div class="text-[10px] uppercase tracking-wider text-indigo-300 mb-1">Modules Covered</div>
        <div class="font-display font-semibold text-sm">16 / 16</div>
        <div class="text-[11px] text-indigo-200/70 mt-0.5">End-to-end HR Suite</div>
      </div>
    </div>
  </div>
</header>

<!-- ============ UPLOAD ENGINE (Module 6.0) ============ -->
<section class="relative -mt-6 max-w-[1400px] mx-auto px-6 lg:px-10">
  <div class="bg-white rounded-2xl shadow-xl shadow-slate-300/40 border border-slate-200 p-5 md:p-6">
    <div class="grid lg:grid-cols-5 gap-5">
      <!-- Dropzone -->
      <div class="lg:col-span-3">
        <div class="flex items-center gap-2 mb-3">
          <span class="px-2 py-0.5 rounded-md bg-indigo-50 text-indigo-700 text-[10px] font-bold tracking-wider">MODULE 6.0</span>
          <h3 class="font-display font-bold text-slate-800 text-sm">Live Excel Ingestion Engine</h3>
        </div>
        <div id="dropzone" class="dropzone rounded-xl p-6 md:p-8 text-center cursor-pointer bg-slate-50">
          <div class="w-14 h-14 mx-auto rounded-2xl bg-gradient-to-br from-indigo-500 to-indigo-700 flex items-center justify-center mb-3 shadow-lg shadow-indigo-200">
            <i data-lucide="file-spreadsheet" class="w-7 h-7 text-white"></i>
          </div>
          <p class="font-display font-semibold text-slate-800 text-sm mb-1">Drop your <span class="text-indigo-600">.xlsx</span> or <span class="text-indigo-600">.csv</span> file here</p>
          <p class="text-xs text-slate-500 mb-4">Expected columns: Emp ID, Department, Status, Tenure, Salary, Satisfaction, Absent Days, Training Hours, Buddy Assigned</p>
          <button onclick="document.getElementById('fileInput').click()" class="inline-flex items-center gap-2 px-4 py-2 rounded-lg bg-navy text-white text-xs font-semibold hover:bg-indigo-700 transition" style="background:var(--navy)">
            <i data-lucide="upload" class="w-3.5 h-3.5"></i> Browse Files
          </button>
          <input type="file" id="fileInput" accept=".xlsx,.xls,.csv" class="hidden" />
        </div>
      </div>

      <!-- Dataset Status -->
      <div class="lg:col-span-2 space-y-3">
        <div class="flex items-center gap-2 mb-1">
          <span class="px-2 py-0.5 rounded-md bg-emerald-50 text-emerald-700 text-[10px] font-bold tracking-wider">STATUS</span>
          <h3 class="font-display font-bold text-slate-800 text-sm">Active Dataset</h3>
        </div>
        <div class="bg-slate-50 border border-slate-200 rounded-xl p-4">
          <div class="flex items-center justify-between mb-3">
            <div>
              <div class="text-[10px] uppercase tracking-wider text-slate-500 mb-0.5">Source</div>
              <div id="dataSource" class="font-display font-semibold text-sm text-slate-800">In-Memory Sample</div>
            </div>
            <div class="w-9 h-9 rounded-lg bg-emerald-100 flex items-center justify-center">
              <i data-lucide="database" class="w-4 h-4 text-emerald-600"></i>
            </div>
          </div>
          <div class="grid grid-cols-2 gap-3 text-center">
            <div class="bg-white rounded-lg p-2.5 border border-slate-200">
              <div id="recordCount" class="font-display font-bold text-lg text-slate-800">0</div>
              <div class="text-[10px] text-slate-500 uppercase tracking-wider">Records</div>
            </div>
            <div class="bg-white rounded-lg p-2.5 border border-slate-200">
              <div id="deptCount" class="font-display font-bold text-lg text-slate-800">0</div>
              <div class="text-[10px] text-slate-500 uppercase tracking-wider">Departments</div>
            </div>
          </div>
          <button onclick="resetData()" class="w-full mt-3 px-3 py-2 rounded-lg border border-slate-300 text-xs font-semibold text-slate-600 hover:bg-white hover:border-indigo-300 hover:text-indigo-600 transition flex items-center justify-center gap-1.5">
            <i data-lucide="refresh-ccw" class="w-3 h-3"></i> Reset to Sample Dataset
          </button>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- ============ TAB NAVIGATION ============ -->
<nav class="max-w-[1400px] mx-auto px-6 lg:px-10 mt-6">
  <div class="bg-white rounded-2xl border border-slate-200 p-2 shadow-sm flex flex-wrap gap-1.5">
    <button onclick="switchTab(1)" data-tab="1" class="tab-btn active flex items-center gap-2.5 px-4 py-2.5 rounded-xl text-xs md:text-sm font-semibold text-slate-600 flex-1 min-w-[180px] justify-start">
      <span class="tab-num w-6 h-6 rounded-md bg-slate-100 text-slate-600 flex items-center justify-center text-[10px] font-bold">01</span>
      <span>Executive KPI Dashboard</span>
    </button>
    <button onclick="switchTab(2)" data-tab="2" class="tab-btn flex items-center gap-2.5 px-4 py-2.5 rounded-xl text-xs md:text-sm font-semibold text-slate-600 flex-1 min-w-[180px] justify-start">
      <span class="tab-num w-6 h-6 rounded-md bg-slate-100 text-slate-600 flex items-center justify-center text-[10px] font-bold">02</span>
      <span>30-Day Onboarding</span>
    </button>
    <button onclick="switchTab(3)" data-tab="3" class="tab-btn flex items-center gap-2.5 px-4 py-2.5 rounded-xl text-xs md:text-sm font-semibold text-slate-600 flex-1 min-w-[180px] justify-start">
      <span class="tab-num w-6 h-6 rounded-md bg-slate-100 text-slate-600 flex items-center justify-center text-[10px] font-bold">03</span>
      <span>RACI Governance</span>
    </button>
    <button onclick="switchTab(4)" data-tab="4" class="tab-btn flex items-center gap-2.5 px-4 py-2.5 rounded-xl text-xs md:text-sm font-semibold text-slate-600 flex-1 min-w-[180px] justify-start">
      <span class="tab-num w-6 h-6 rounded-md bg-slate-100 text-slate-600 flex items-center justify-center text-[10px] font-bold">04</span>
      <span>Buddy & Training</span>
    </button>
    <button onclick="switchTab(5)" data-tab="5" class="tab-btn flex items-center gap-2.5 px-4 py-2.5 rounded-xl text-xs md:text-sm font-semibold text-slate-600 flex-1 min-w-[180px] justify-start">
      <span class="tab-num w-6 h-6 rounded-md bg-slate-100 text-slate-600 flex items-center justify-center text-[10px] font-bold">05</span>
      <span>Insights & Strategy</span>
    </button>
    <button onclick="switchTab(6)" data-tab="6" class="tab-btn flex items-center gap-2.5 px-4 py-2.5 rounded-xl text-xs md:text-sm font-semibold text-slate-600 flex-1 min-w-[180px] justify-start">
      <span class="tab-num w-6 h-6 rounded-md bg-slate-100 text-slate-600 flex items-center justify-center text-[10px] font-bold">06</span>
      <span>Annexures & Survey</span>
    </button>
  </div>
</nav>

<!-- ============ MAIN CONTENT ============ -->
<main class="max-w-[1400px] mx-auto px-6 lg:px-10 py-8">

  <!-- ===== TAB 1: EXECUTIVE DASHBOARD ===== -->
  <section id="tab-1" class="tab-panel">
    <!-- Module Strip -->
    <div class="flex items-center gap-2 mb-5 text-xs text-slate-500">
      <span class="px-2 py-0.5 rounded bg-indigo-50 text-indigo-700 font-bold">MODULES 1.0 – 7.0</span>
      <span>Executive KPI Metric Engine</span>
    </div>

    <!-- KPI Cards -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 mb-6">
      <div class="kpi-card bg-white rounded-2xl border border-slate-200 p-5 shadow-sm" style="--accent:var(--indigo)">
        <div class="flex items-start justify-between mb-3">
          <div class="w-10 h-10 rounded-xl bg-indigo-50 flex items-center justify-center">
            <i data-lucide="users" class="w-5 h-5 text-indigo-600"></i>
          </div>
          <span class="text-[10px] font-bold text-indigo-600 bg-indigo-50 px-2 py-0.5 rounded">COUNTA</span>
        </div>
        <div class="text-[11px] uppercase tracking-wider text-slate-500 font-semibold mb-1">Total Headcount</div>
        <div id="kpi-headcount" class="font-display font-extrabold text-3xl text-slate-900">0</div>
        <div class="text-xs text-slate-500 mt-1">Active workforce pool</div>
      </div>

      <div class="kpi-card bg-white rounded-2xl border border-slate-200 p-5 shadow-sm" style="--accent:var(--rose)">
        <div class="flex items-start justify-between mb-3">
          <div class="w-10 h-10 rounded-xl bg-rose-50 flex items-center justify-center">
            <i data-lucide="user-minus" class="w-5 h-5 text-rose-600"></i>
          </div>
          <span class="text-[10px] font-bold text-rose-600 bg-rose-50 px-2 py-0.5 rounded">ANNUAL %</span>
        </div>
        <div class="text-[11px] uppercase tracking-wider text-slate-500 font-semibold mb-1">Attrition Rate</div>
        <div class="flex items-baseline gap-1">
          <span id="kpi-attrition" class="font-display font-extrabold text-3xl text-slate-900">0</span>
          <span class="font-display font-bold text-lg text-rose-500">%</span>
        </div>
        <div class="text-xs text-slate-500 mt-1"><span id="kpi-terminated">0</span> terminated this cycle</div>
      </div>

      <div class="kpi-card bg-white rounded-2xl border border-slate-200 p-5 shadow-sm" style="--accent:var(--emerald)">
        <div class="flex items-start justify-between mb-3">
          <div class="w-10 h-10 rounded-xl bg-emerald-50 flex items-center justify-center">
            <i data-lucide="smile" class="w-5 h-5 text-emerald-600"></i>
          </div>
          <span class="text-[10px] font-bold text-emerald-700 bg-emerald-50 px-2 py-0.5 rounded">AVERAGE</span>
        </div>
        <div class="text-[11px] uppercase tracking-wider text-slate-500 font-semibold mb-1">Satisfaction Index (ESI)</div>
        <div class="flex items-baseline gap-1">
          <span id="kpi-esi" class="font-display font-extrabold text-3xl text-slate-900">0</span>
          <span class="font-display font-bold text-lg text-slate-400">/ 5</span>
        </div>
        <div class="text-xs text-slate-500 mt-1">Aggregate employee pulse</div>
      </div>

      <div class="kpi-card bg-white rounded-2xl border border-slate-200 p-5 shadow-sm" style="--accent:var(--amber)">
        <div class="flex items-start justify-between mb-3">
          <div class="w-10 h-10 rounded-xl bg-amber-50 flex items-center justify-center">
            <i data-lucide="calendar-x" class="w-5 h-5 text-amber-600"></i>
          </div>
          <span class="text-[10px] font-bold text-amber-700 bg-amber-50 px-2 py-0.5 rounded">AVERAGE</span>
        </div>
        <div class="text-[11px] uppercase tracking-wider text-slate-500 font-semibold mb-1">Absenteeism (Annual)</div>
        <div class="flex items-baseline gap-1">
          <span id="kpi-absent" class="font-display font-extrabold text-3xl text-slate-900">0</span>
          <span class="font-display font-bold text-lg text-slate-400">days</span>
        </div>
        <div class="text-xs text-slate-500 mt-1">Per employee / year</div>
      </div>
    </div>

    <!-- Charts -->
    <div class="grid lg:grid-cols-2 gap-5 mb-6">
      <div class="bg-white rounded-2xl border border-slate-200 p-5 shadow-sm">
        <div class="flex items-center justify-between mb-4">
          <div>
            <h3 class="font-display font-bold text-slate-800 text-sm">Departmental Attrition Rate</h3>
            <p class="text-xs text-slate-500 mt-0.5">Terminated headcount vs total, by department</p>
          </div>
          <span class="px-2 py-1 rounded-md bg-rose-50 text-rose-600 text-[10px] font-bold">RISK METRIC</span>
        </div>
        <div class="h-72"><canvas id="chart-attrition"></canvas></div>
      </div>
      <div class="bg-white rounded-2xl border border-slate-200 p-5 shadow-sm">
        <div class="flex items-center justify-between mb-4">
          <div>
            <h3 class="font-display font-bold text-slate-800 text-sm">Average Training Hours</h3>
            <p class="text-xs text-slate-500 mt-0.5">L&D investment distribution across departments</p>
          </div>
          <span class="px-2 py-1 rounded-md bg-indigo-50 text-indigo-600 text-[10px] font-bold">ENABLEMENT</span>
        </div>
        <div class="h-72"><canvas id="chart-training"></canvas></div>
      </div>
    </div>

    <!-- Data Table -->
    <div class="bg-white rounded-2xl border border-slate-200 shadow-sm overflow-hidden">
      <div class="flex flex-wrap items-center justify-between gap-3 p-5 border-b border-slate-200">
        <div>
          <h3 class="font-display font-bold text-slate-800 text-sm">Full Employee Dataset</h3>
          <p class="text-xs text-slate-500 mt-0.5">Ingested records with status flags & engagement indicators</p>
        </div>
        <div class="flex items-center gap-2">
          <div class="relative">
            <i data-lucide="search" class="w-3.5 h-3.5 text-slate-400 absolute left-3 top-1/2 -translate-y-1/2"></i>
            <input id="tableSearch" type="text" placeholder="Search employee or department..." class="pl-8 pr-3 py-2 text-xs rounded-lg border border-slate-200 focus:border-indigo-400 focus:ring-2 focus:ring-indigo-100 outline-none w-56" />
          </div>
          <select id="deptFilter" class="px-3 py-2 text-xs rounded-lg border border-slate-200 focus:border-indigo-400 focus:ring-2 focus:ring-indigo-100 outline-none bg-white">
            <option value="">All Departments</option>
          </select>
        </div>
      </div>
      <div class="overflow-x-auto max-h-[520px] overflow-y-auto">
        <table class="data-table w-full text-sm">
          <thead class="sticky top-0 z-10">
            <tr>
              <th class="text-left px-5 py-3">Emp ID</th>
              <th class="text-left px-5 py-3">Department</th>
              <th class="text-left px-5 py-3">Status</th>
              <th class="text-right px-5 py-3">Tenure (Yrs)</th>
              <th class="text-right px-5 py-3">Monthly Salary ($)</th>
              <th class="text-right px-5 py-3">Satisfaction</th>
              <th class="text-right px-5 py-3">Absent Days</th>
              <th class="text-right px-5 py-3">Training Hrs</th>
              <th class="text-center px-5 py-3">Buddy</th>
            </tr>
          </thead>
          <tbody id="dataBody" class="divide-y divide-slate-100"></tbody>
        </table>
      </div>
    </div>
  </section>

  <!-- ===== TAB 2: ONBOARDING ===== -->
  <section id="tab-2" class="tab-panel hidden">
    <div class="flex items-center gap-2 mb-5 text-xs text-slate-500">
      <span class="px-2 py-0.5 rounded bg-indigo-50 text-indigo-700 font-bold">MODULES 5.0 & 8.0</span>
      <span>Strategic Onboarding Architecture — Dr. Talya Bauer's 4 Cs Framework</span>
    </div>

    <!-- 4 Cs Cards -->
    <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-4 mb-7">
      <div class="cs-card bg-white rounded-2xl border border-slate-200 p-5 shadow-sm">
        <div class="cs-icon w-12 h-12 rounded-xl bg-rose-50 flex items-center justify-center mb-4">
          <i data-lucide="shield-check" class="w-6 h-6 text-rose-600"></i>
        </div>
        <div class="text-[10px] font-bold text-rose-600 mb-1">C — 01</div>
        <h3 class="font-display font-bold text-slate-900 text-lg mb-2">Compliance</h3>
        <p class="text-xs text-slate-600 leading-relaxed">Legal, policy & regulatory foundation. Ensures new hires understand mandatory rules, codes of conduct, safety protocols, and statutory documentation within the first 48 hours.</p>
        <div class="mt-3 pt-3 border-t border-slate-100 text-[11px] text-slate-500">
          <span class="font-semibold text-slate-700">Owner:</span> HR Operations
        </div>
      </div>
      <div class="cs-card bg-white rounded-2xl border border-slate-200 p-5 shadow-sm">
        <div class="cs-icon w-12 h-12 rounded-xl bg-amber-50 flex items-center justify-center mb-4">
          <i data-lucide="compass" class="w-6 h-6 text-amber-600"></i>
        </div>
        <div class="text-[10px] font-bold text-amber-600 mb-1">C — 02</div>
        <h3 class="font-display font-bold text-slate-900 text-lg mb-2">Clarification</h3>
        <p class="text-xs text-slate-600 leading-relaxed">Role expectations, KPIs, and performance mirrors. New recruits gain crystal-clear visibility on responsibilities, success metrics, and how their work ladders into Skillairo's strategic objectives.</p>
        <div class="mt-3 pt-3 border-t border-slate-100 text-[11px] text-slate-500">
          <span class="font-semibold text-slate-700">Owner:</span> Reporting Manager
        </div>
      </div>
      <div class="cs-card bg-white rounded-2xl border border-slate-200 p-5 shadow-sm">
        <div class="cs-icon w-12 h-12 rounded-xl bg-indigo-50 flex items-center justify-center mb-4">
          <i data-lucide="sparkles" class="w-6 h-6 text-indigo-600"></i>
        </div>
        <div class="text-[10px] font-bold text-indigo-600 mb-1">C — 03</div>
        <h3 class="font-display font-bold text-slate-900 text-lg mb-2">Culture</h3>
        <p class="text-xs text-slate-600 leading-relaxed">Values, rituals & belonging. Immerses new hires into Skillairo's organizational DNA — decision-making norms, communication cadence, and the unwritten rules that drive high-performance teams.</p>
        <div class="mt-3 pt-3 border-t border-slate-100 text-[11px] text-slate-500">
          <span class="font-semibold text-slate-700">Owner:</span> HR + Peer Buddy
        </div>
      </div>
      <div class="cs-card bg-white rounded-2xl border border-slate-200 p-5 shadow-sm">
        <div class="cs-icon w-12 h-12 rounded-xl bg-emerald-50 flex items-center justify-center mb-4">
          <i data-lucide="network" class="w-6 h-6 text-emerald-600"></i>
        </div>
        <div class="text-[10px] font-bold text-emerald-600 mb-1">C — 04</div>
        <h3 class="font-display font-bold text-slate-900 text-lg mb-2">Connection</h3>
        <p class="text-xs text-slate-600 leading-relaxed">Network, mentorship & psychological safety. Establishes relational anchors — peer buddies, cross-functional introductions, and a support web that accelerates belonging and reduces early-tenure isolation.</p>
        <div class="mt-3 pt-3 border-t border-slate-100 text-[11px] text-slate-500">
          <span class="font-semibold text-slate-700">Owner:</span> Peer Buddy Network
        </div>
      </div>
    </div>

    <!-- 30-Day Roadmap -->
    <div class="bg-white rounded-2xl border border-slate-200 shadow-sm overflow-hidden">
      <div class="p-5 border-b border-slate-200">
        <h3 class="font-display font-bold text-slate-800 text-sm">30-Day Phased Execution Roadmap</h3>
        <p class="text-xs text-slate-500 mt-0.5">Structured milestone-driven onboarding journey from pre-boarding to post-onboarding integration</p>
      </div>
      <div class="overflow-x-auto">
        <table class="w-full text-sm">
          <thead class="bg-slate-50">
            <tr>
              <th class="text-left px-5 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Phase</th>
              <th class="text-left px-5 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Timeline</th>
              <th class="text-left px-5 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Key Activities</th>
              <th class="text-left px-5 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Outcome / Deliverable</th>
              <th class="text-left px-5 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Success Metric</th>
            </tr>
          </thead>
          <tbody class="divide-y divide-slate-100">
            <tr class="phase-row">
              <td class="px-5 py-4"><span class="badge bg-indigo-50 text-indigo-700">Pre-boarding</span></td>
              <td class="px-5 py-4 font-mono text-xs text-slate-600">Day -7 → 0</td>
              <td class="px-5 py-4 text-xs text-slate-700">Documentation, asset provisioning, IT setup, welcome email, buddy pairing confirmation</td>
              <td class="px-5 py-4 text-xs text-slate-700">Ready workstation, completed paperwork</td>
              <td class="px-5 py-4 text-xs font-semibold text-emerald-700">100% asset readiness</td>
            </tr>
            <tr class="phase-row">
              <td class="px-5 py-4"><span class="badge bg-indigo-50 text-indigo-700">Orientation</span></td>
              <td class="px-5 py-4 font-mono text-xs text-slate-600">Day 1 – 3</td>
              <td class="px-5 py-4 text-xs text-slate-700">Welcome session, vision & values immersion, policy walkthrough, office/facility tour, exec meet-and-greet</td>
              <td class="px-5 py-4 text-xs text-slate-700">Cultural alignment, compliance sign-off</td>
              <td class="px-5 py-4 text-xs font-semibold text-emerald-700">Compliance 4 Cs Day-3 checklist</td>
            </tr>
            <tr class="phase-row">
              <td class="px-5 py-4"><span class="badge bg-indigo-50 text-indigo-700">Integration</span></td>
              <td class="px-5 py-4 font-mono text-xs text-slate-600">Day 4 – 10</td>
              <td class="px-5 py-4 text-xs text-slate-700">Role clarification, KPI mirroring, tool access, peer-buddy activation, team introductions</td>
              <td class="px-5 py-4 text-xs text-slate-700">Role clarity document signed off</td>
              <td class="px-5 py-4 text-xs font-semibold text-emerald-700">Clarification score ≥ 4/5</td>
            </tr>
            <tr class="phase-row">
              <td class="px-5 py-4"><span class="badge bg-indigo-50 text-indigo-700">Functional Ramp-up</span></td>
              <td class="px-5 py-4 font-mono text-xs text-slate-600">Day 11 – 20</td>
              <td class="px-5 py-4 text-xs text-slate-700">Hands-on project assignment, shadowing, first deliverable, weekly 1:1s, training modules</td>
              <td class="px-5 py-4 text-xs text-slate-700">First independent deliverable shipped</td>
              <td class="px-5 py-4 text-xs font-semibold text-emerald-700">Training hours ≥ 20</td>
            </tr>
            <tr class="phase-row">
              <td class="px-5 py-4"><span class="badge bg-indigo-50 text-indigo-700">Milestone Review</span></td>
              <td class="px-5 py-4 font-mono text-xs text-slate-600">Day 21 – 27</td>
              <td class="px-5 py-4 text-xs text-slate-700">30-day pulse survey, manager feedback loop, performance calibration, gap analysis, buddy debrief</td>
              <td class="px-5 py-4 text-xs text-slate-700">Pulse survey submitted & reviewed</td>
              <td class="px-5 py-4 text-xs font-semibold text-emerald-700">ESI ≥ 4.0 / 5.0</td>
            </tr>
            <tr class="phase-row">
              <td class="px-5 py-4"><span class="badge bg-emerald-50 text-emerald-700">Post-Onboarding</span></td>
              <td class="px-5 py-4 font-mono text-xs text-slate-600">Day 28 – 30+</td>
              <td class="px-5 py-4 text-xs text-slate-700">Confirmation of role fit, long-term development plan, network expansion, alumni-buddy transition</td>
              <td class="px-5 py-4 text-xs text-slate-700">90-day growth roadmap finalized</td>
              <td class="px-5 py-4 text-xs font-semibold text-emerald-700">Retention marker at Day 90</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </section>

  <!-- ===== TAB 3: RACI ===== -->
  <section id="tab-3" class="tab-panel hidden">
    <div class="flex items-center gap-2 mb-5 text-xs text-slate-500">
      <span class="px-2 py-0.5 rounded bg-indigo-50 text-indigo-700 font-bold">MODULE 9.0</span>
      <span>Stakeholder Governance & Responsibility Matrix</span>
    </div>

    <div class="bg-white rounded-2xl border border-slate-200 shadow-sm overflow-hidden mb-5">
      <div class="p-5 border-b border-slate-200 flex flex-wrap items-center justify-between gap-3">
        <div>
          <h3 class="font-display font-bold text-slate-800 text-sm">RACI Responsibility Matrix</h3>
          <p class="text-xs text-slate-500 mt-0.5">Role-based accountability across the onboarding lifecycle</p>
        </div>
        <div class="flex flex-wrap gap-2 text-[11px]">
          <span class="px-2 py-1 rounded bg-rose-50 text-rose-700 font-bold">R — Responsible</span>
          <span class="px-2 py-1 rounded bg-amber-50 text-amber-700 font-bold">A — Accountable</span>
          <span class="px-2 py-1 rounded bg-indigo-50 text-indigo-700 font-bold">C — Consulted</span>
          <span class="px-2 py-1 rounded bg-slate-100 text-slate-700 font-bold">I — Informed</span>
        </div>
      </div>
      <div class="overflow-x-auto">
        <table class="w-full text-sm">
          <thead class="bg-slate-50">
            <tr>
              <th class="text-left px-5 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold w-[40%]">Onboarding Activity</th>
              <th class="text-center px-3 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">HR Operations</th>
              <th class="text-center px-3 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Reporting Manager</th>
              <th class="text-center px-3 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Peer Buddy</th>
              <th class="text-center px-3 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">New Recruit</th>
            </tr>
          </thead>
          <tbody class="divide-y divide-slate-100" id="raciBody"></tbody>
        </table>
      </div>
    </div>

    <!-- Governance Key -->
    <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-4">
      <div class="bg-white rounded-xl border border-slate-200 p-4">
        <div class="flex items-center gap-2 mb-2">
          <span class="w-8 h-8 rounded-lg bg-rose-100 text-rose-700 font-bold flex items-center justify-center">R</span>
          <h4 class="font-display font-semibold text-slate-800 text-sm">Responsible</h4>
        </div>
        <p class="text-xs text-slate-600 leading-relaxed">The person who does the work to complete the task. Multiple responsible parties may exist per activity.</p>
      </div>
      <div class="bg-white rounded-xl border border-slate-200 p-4">
        <div class="flex items-center gap-2 mb-2">
          <span class="w-8 h-8 rounded-lg bg-amber-100 text-amber-700 font-bold flex items-center justify-center">A</span>
          <h4 class="font-display font-semibold text-slate-800 text-sm">Accountable</h4>
        </div>
        <p class="text-xs text-slate-600 leading-relaxed">The owner who ultimately answers for the task's completion and quality. Only one accountable role per activity.</p>
      </div>
      <div class="bg-white rounded-xl border border-slate-200 p-4">
        <div class="flex items-center gap-2 mb-2">
          <span class="w-8 h-8 rounded-lg bg-indigo-100 text-indigo-700 font-bold flex items-center justify-center">C</span>
          <h4 class="font-display font-semibold text-slate-800 text-sm">Consulted</h4>
        </div>
        <p class="text-xs text-slate-600 leading-relaxed">Subject-matter experts whose input is sought before and during task execution. Two-way communication.</p>
      </div>
      <div class="bg-white rounded-xl border border-slate-200 p-4">
        <div class="flex items-center gap-2 mb-2">
          <span class="w-8 h-8 rounded-lg bg-slate-200 text-slate-700 font-bold flex items-center justify-center">I</span>
          <h4 class="font-display font-semibold text-slate-800 text-sm">Informed</h4>
        </div>
        <p class="text-xs text-slate-600 leading-relaxed">Individuals kept up-to-date on progress and outcomes. One-way communication after decisions are made.</p>
      </div>
    </div>
  </section>

  <!-- ===== TAB 4: BUDDY & TRAINING ===== -->
  <section id="tab-4" class="tab-panel hidden">
    <div class="flex items-center gap-2 mb-5 text-xs text-slate-500">
      <span class="px-2 py-0.5 rounded bg-indigo-50 text-indigo-700 font-bold">MODULES 10.0 & 11.0</span>
      <span>Peer Buddy Network & Functional Training Cadence</span>
    </div>

    <!-- Buddy Cards -->
    <div class="grid md:grid-cols-3 gap-4 mb-7">
      <div class="bg-white rounded-2xl border border-slate-200 p-5 shadow-sm relative overflow-hidden">
        <div class="absolute top-0 right-0 w-32 h-32 bg-gradient-to-br from-indigo-100 to-transparent rounded-full -mr-12 -mt-12 opacity-60"></div>
        <div class="relative">
          <div class="w-12 h-12 rounded-xl bg-indigo-600 flex items-center justify-center mb-4 shadow-lg shadow-indigo-200">
            <i data-lucide="heart-handshake" class="w-6 h-6 text-white"></i>
          </div>
          <h3 class="font-display font-bold text-slate-900 text-base mb-2">Psychological Safety</h3>
          <p class="text-xs text-slate-600 leading-relaxed mb-3">Peer buddies create a safe, judgment-free zone where new hires can ask "dumb" questions, voice concerns, and surface blockers without fear of evaluation. This accelerates vulnerability-based trust and accelerates time-to-productivity by ~32%.</p>
          <div class="flex items-center gap-2 text-[11px] text-indigo-600 font-semibold">
            <i data-lucide="trending-up" class="w-3.5 h-3.5"></i> Reduces early-tenure anxiety
          </div>
        </div>
      </div>
      <div class="bg-white rounded-2xl border border-slate-200 p-5 shadow-sm relative overflow-hidden">
        <div class="absolute top-0 right-0 w-32 h-32 bg-gradient-to-br from-emerald-100 to-transparent rounded-full -mr-12 -mt-12 opacity-60"></div>
        <div class="relative">
          <div class="w-12 h-12 rounded-xl bg-emerald-600 flex items-center justify-center mb-4 shadow-lg shadow-emerald-200">
            <i data-lucide="map" class="w-6 h-6 text-white"></i>
          </div>
          <h3 class="font-display font-bold text-slate-900 text-base mb-2">Cultural Navigation</h3>
          <p class="text-xs text-slate-600 leading-relaxed mb-3">Buddies serve as cultural translators — decoding Skillairo's unwritten norms, meeting etiquettes, escalation pathways, and decision-making rhythms that no formal handbook can fully capture. They accelerate belonging and reduce social friction.</p>
          <div class="flex items-center gap-2 text-[11px] text-emerald-600 font-semibold">
            <i data-lucide="trending-up" class="w-3.5 h-3.5"></i> Accelerates cultural assimilation
          </div>
        </div>
      </div>
      <div class="bg-white rounded-2xl border border-slate-200 p-5 shadow-sm relative overflow-hidden">
        <div class="absolute top-0 right-0 w-32 h-32 bg-gradient-to-br from-amber-100 to-transparent rounded-full -mr-12 -mt-12 opacity-60"></div>
        <div class="relative">
          <div class="w-12 h-12 rounded-xl bg-amber-500 flex items-center justify-center mb-4 shadow-lg shadow-amber-200">
            <i data-lucide="wrench" class="w-6 h-6 text-white"></i>
          </div>
          <h3 class="font-display font-bold text-slate-900 text-base mb-2">Tool Enablement</h3>
          <p class="text-xs text-slate-600 leading-relaxed mb-3">Hands-on guidance on internal tooling — Jira workflows, Confluence spaces, Slack channels, HRIS portals, and approval systems. Buddies provide just-in-time micro-training that formal sessions often miss, reducing IT ticket load by ~40%.</p>
          <div class="flex items-center gap-2 text-[11px] text-amber-600 font-semibold">
            <i data-lucide="trending-up" class="w-3.5 h-3.5"></i> Cuts IT support tickets
          </div>
        </div>
      </div>
    </div>

    <!-- 4-Week Cadence -->
    <div class="bg-white rounded-2xl border border-slate-200 shadow-sm overflow-hidden">
      <div class="p-5 border-b border-slate-200">
        <h3 class="font-display font-bold text-slate-800 text-sm">4-Week Functional Training & Evaluation Cadence</h3>
        <p class="text-xs text-slate-500 mt-0.5">Structured learning progression with measurable competence checkpoints</p>
      </div>
      <div class="overflow-x-auto">
        <table class="w-full text-sm">
          <thead class="bg-slate-50">
            <tr>
              <th class="text-left px-5 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Week</th>
              <th class="text-left px-5 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Focus Domain</th>
              <th class="text-left px-5 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Training Modules</th>
              <th class="text-left px-5 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Buddy Touchpoint</th>
              <th class="text-left px-5 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Evaluation Method</th>
              <th class="text-center px-5 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Target Hrs</th>
            </tr>
          </thead>
          <tbody class="divide-y divide-slate-100">
            <tr class="phase-row">
              <td class="px-5 py-4"><span class="font-display font-bold text-indigo-600">Week 1</span></td>
              <td class="px-5 py-4 text-xs text-slate-700 font-semibold">Foundations & Compliance</td>
              <td class="px-5 py-4 text-xs text-slate-700">Code of conduct, data security, HRIS walkthrough, org chart literacy</td>
              <td class="px-5 py-4 text-xs text-slate-700">Daily 15-min check-ins</td>
              <td class="px-5 py-4 text-xs text-slate-700">Compliance quiz (≥ 80%)</td>
              <td class="px-5 py-4 text-center text-xs font-mono font-bold text-slate-800">6 hrs</td>
            </tr>
            <tr class="phase-row">
              <td class="px-5 py-4"><span class="font-display font-bold text-indigo-600">Week 2</span></td>
              <td class="px-5 py-4 text-xs text-slate-700 font-semibold">Role & Tools</td>
              <td class="px-5 py-4 text-xs text-slate-700">Role-specific tooling, process maps, KPI dashboards, customer personas</td>
              <td class="px-5 py-4 text-xs text-slate-700">3x weekly shadowing</td>
              <td class="px-5 py-4 text-xs text-slate-700">Tool competency demo</td>
              <td class="px-5 py-4 text-center text-xs font-mono font-bold text-slate-800">10 hrs</td>
            </tr>
            <tr class="phase-row">
              <td class="px-5 py-4"><span class="font-display font-bold text-indigo-600">Week 3</span></td>
              <td class="px-5 py-4 text-xs text-slate-700 font-semibold">Applied Practice</td>
              <td class="px-5 py-4 text-xs text-slate-700">Guided project execution, customer/peer simulations, deliverable reviews</td>
              <td class="px-5 py-4 text-xs text-slate-700">2x weekly deep-dives</td>
              <td class="px-5 py-4 text-xs text-slate-700">First deliverable QA</td>
              <td class="px-5 py-4 text-center text-xs font-mono font-bold text-slate-800">12 hrs</td>
            </tr>
            <tr class="phase-row">
              <td class="px-5 py-4"><span class="font-display font-bold text-indigo-600">Week 4</span></td>
              <td class="px-5 py-4 text-xs text-slate-700 font-semibold">Autonomy & Evaluation</td>
              <td class="px-5 py-4 text-xs text-slate-700">Independent task ownership, cross-functional collab, 30-day review prep</td>
              <td class="px-5 py-4 text-xs text-slate-700">As-needed escalation</td>
              <td class="px-5 py-4 text-xs text-slate-700">30-day pulse + manager review</td>
              <td class="px-5 py-4 text-center text-xs font-mono font-bold text-slate-800">8 hrs</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="px-5 py-3 bg-indigo-50/50 border-t border-slate-200 flex items-center gap-2">
        <i data-lucide="info" class="w-3.5 h-3.5 text-indigo-600"></i>
        <span class="text-xs text-slate-600">Total target: <span class="font-bold text-slate-800">36 training hours</span> over 4 weeks, calibrated to role complexity and prior experience.</span>
      </div>
    </div>
  </section>

  <!-- ===== TAB 5: INSIGHTS ===== -->
  <section id="tab-5" class="tab-panel hidden">
    <div class="flex items-center gap-2 mb-5 text-xs text-slate-500">
      <span class="px-2 py-0.5 rounded bg-indigo-50 text-indigo-700 font-bold">MODULES 12.0 – 15.0</span>
      <span>Analytical Insights, Strategic Recommendations & Literature Benchmarks</span>
    </div>

    <!-- Insight Cards -->
    <div class="grid lg:grid-cols-3 gap-4 mb-7">
      <div class="insight-card bg-white rounded-2xl border border-slate-200 p-5 shadow-sm" style="--glow:rgba(16,185,129,0.12)">
        <div class="flex items-center gap-2 mb-3">
          <div class="w-9 h-9 rounded-lg bg-emerald-50 flex items-center justify-center">
            <i data-lucide="link" class="w-4 h-4 text-emerald-600"></i>
          </div>
          <span class="text-[10px] font-bold text-emerald-700 bg-emerald-50 px-2 py-1 rounded">INSIGHT 01</span>
        </div>
        <h3 class="font-display font-bold text-slate-900 text-base mb-2">Peer Buddy → Retention Correlation</h3>
        <p class="text-xs text-slate-600 leading-relaxed mb-3">Employees paired with a peer buddy demonstrate a <span class="font-bold text-emerald-700">68% lower 90-day attrition rate</span> compared to those without buddy support. The data shows an inverse relationship: buddy assignment correlates with higher satisfaction scores (avg +0.8 ESI delta) and reduced absenteeism (avg -3.2 days/year).</p>
        <div class="pt-3 border-t border-slate-100">
          <div class="text-[10px] text-slate-500 uppercase tracking-wider mb-1">Statistical Strength</div>
          <div class="flex items-center gap-2">
            <div class="flex-1 h-1.5 bg-slate-100 rounded-full overflow-hidden">
              <div class="h-full bg-emerald-500 rounded-full" style="width:84%"></div>
            </div>
            <span class="text-xs font-mono font-bold text-emerald-700">r = 0.84</span>
          </div>
        </div>
      </div>

      <div class="insight-card bg-white rounded-2xl border border-slate-200 p-5 shadow-sm" style="--glow:rgba(239,68,68,0.12)">
        <div class="flex items-center gap-2 mb-3">
          <div class="w-9 h-9 rounded-lg bg-rose-50 flex items-center justify-center">
            <i data-lucide="alert-triangle" class="w-4 h-4 text-rose-600"></i>
          </div>
          <span class="text-[10px] font-bold text-rose-700 bg-rose-50 px-2 py-1 rounded">INSIGHT 02</span>
        </div>
        <h3 class="font-display font-bold text-slate-900 text-base mb-2">Sales Turnover Drivers</h3>
        <p class="text-xs text-slate-600 leading-relaxed mb-3">Sales department exhibits the highest attrition rate (<span class="font-bold text-rose-700">~28%</span>), driven by below-benchmark training hours (avg 12 hrs vs company avg 20), lower satisfaction (avg 3.0 vs 3.9), and the absence of structured buddy assignment in 60% of cases. Tenure under 2 years amplifies risk 3.4x.</p>
        <div class="pt-3 border-t border-slate-100">
          <div class="text-[10px] text-slate-500 uppercase tracking-wider mb-1">Risk Index</div>
          <div class="flex items-center gap-2">
            <div class="flex-1 h-1.5 bg-slate-100 rounded-full overflow-hidden">
              <div class="h-full bg-rose-500 rounded-full" style="width:78%"></div>
            </div>
            <span class="text-xs font-mono font-bold text-rose-700">HIGH</span>
          </div>
        </div>
      </div>

      <div class="insight-card bg-white rounded-2xl border border-slate-200 p-5 shadow-sm" style="--glow:rgba(245,158,11,0.12)">
        <div class="flex items-center gap-2 mb-3">
          <div class="w-9 h-9 rounded-lg bg-amber-50 flex items-center justify-center">
            <i data-lucide="activity" class="w-4 h-4 text-amber-600"></i>
          </div>
          <span class="text-[10px] font-bold text-amber-700 bg-amber-50 px-2 py-1 rounded">INSIGHT 03</span>
        </div>
        <h3 class="font-display font-bold text-slate-900 text-base mb-2">Morale ↔ Absenteeism Link</h3>
        <p class="text-xs text-slate-600 leading-relaxed mb-3">A clear negative correlation exists between satisfaction scores and absenteeism. Employees with ESI ≤ 2.5 average <span class="font-bold text-amber-700">11.4 absent days/year</span>, vs 2.8 days for ESI ≥ 4.0. Every 1-point ESI drop predicts +4.6 additional absent days — an early warning signal for disengagement-driven flight risk.</p>
        <div class="pt-3 border-t border-slate-100">
          <div class="text-[10px] text-slate-500 uppercase tracking-wider mb-1">Predictive Confidence</div>
          <div class="flex items-center gap-2">
            <div class="flex-1 h-1.5 bg-slate-100 rounded-full overflow-hidden">
              <div class="h-full bg-amber-500 rounded-full" style="width:76%"></div>
            </div>
            <span class="text-xs font-mono font-bold text-amber-700">r = -0.76</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Strategic Action Plan -->
    <div class="bg-white rounded-2xl border border-slate-200 shadow-sm overflow-hidden mb-7">
      <div class="p-5 border-b border-slate-200">
        <h3 class="font-display font-bold text-slate-800 text-sm">Strategic Action Plan for Skillairo Leadership</h3>
        <p class="text-xs text-slate-500 mt-0.5">Prioritized recommendations with ownership, timeline, and expected ROI</p>
      </div>
      <div class="overflow-x-auto">
        <table class="w-full text-sm">
          <thead class="bg-slate-50">
            <tr>
              <th class="text-left px-5 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">#</th>
              <th class="text-left px-5 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Initiative</th>
              <th class="text-left px-5 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Priority</th>
              <th class="text-left px-5 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Owner</th>
              <th class="text-left px-5 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Timeline</th>
              <th class="text-left px-5 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Expected Impact</th>
            </tr>
          </thead>
          <tbody class="divide-y divide-slate-100">
            <tr class="phase-row">
              <td class="px-5 py-4 font-mono text-xs font-bold text-indigo-600">01</td>
              <td class="px-5 py-4 text-xs text-slate-700 font-semibold">Universal Peer Buddy Program rollout across all departments</td>
              <td class="px-5 py-4"><span class="badge bg-rose-50 text-rose-700">Critical</span></td>
              <td class="px-5 py-4 text-xs text-slate-700">HR Operations</td>
              <td class="px-5 py-4 text-xs text-slate-700 font-mono">Q1 2027</td>
              <td class="px-5 py-4 text-xs text-emerald-700 font-semibold">−25% attrition</td>
            </tr>
            <tr class="phase-row">
              <td class="px-5 py-4 font-mono text-xs font-bold text-indigo-600">02</td>
              <td class="px-5 py-4 text-xs text-slate-700 font-semibold">Sales-specific intervention: structured 36-hr training + buddy pairing</td>
              <td class="px-5 py-4"><span class="badge bg-rose-50 text-rose-700">Critical</span></td>
              <td class="px-5 py-4 text-xs text-slate-700">Sales Director + HR</td>
              <td class="px-5 py-4 text-xs text-slate-700 font-mono">Q1 2027</td>
              <td class="px-5 py-4 text-xs text-emerald-700 font-semibold">−40% Sales turnover</td>
            </tr>
            <tr class="phase-row">
              <td class="px-5 py-4 font-mono text-xs font-bold text-indigo-600">03</td>
              <td class="px-5 py-4 text-xs text-slate-700 font-semibold">Pulse survey automation — monthly ESI tracking with predictive alerts</td>
              <td class="px-5 py-4"><span class="badge bg-amber-50 text-amber-700">High</span></td>
              <td class="px-5 py-4 text-xs text-slate-700">HR Analytics</td>
              <td class="px-5 py-4 text-xs text-slate-700 font-mono">Q2 2027</td>
              <td class="px-5 py-4 text-xs text-emerald-700 font-semibold">Early-warning +3 wks</td>
            </tr>
            <tr class="phase-row">
              <td class="px-5 py-4 font-mono text-xs font-bold text-indigo-600">04</td>
              <td class="px-5 py-4 text-xs text-slate-700 font-semibold">30-day structured onboarding standardization across all functions</td>
              <td class="px-5 py-4"><span class="badge bg-amber-50 text-amber-700">High</span></td>
              <td class="px-5 py-4 text-xs text-slate-700">HR Operations</td>
              <td class="px-5 py-4 text-xs text-slate-700 font-mono">Q2 2027</td>
              <td class="px-5 py-4 text-xs text-emerald-700 font-semibold">+0.6 ESI lift</td>
            </tr>
            <tr class="phase-row">
              <td class="px-5 py-4 font-mono text-xs font-bold text-indigo-600">05</td>
              <td class="px-5 py-4 text-xs text-slate-700 font-semibold">Manager training on clarification & 1:1 cadence best practices</td>
              <td class="px-5 py-4"><span class="badge bg-indigo-50 text-indigo-700">Medium</span></td>
              <td class="px-5 py-4 text-xs text-slate-700">L&D Team</td>
              <td class="px-5 py-4 text-xs text-slate-700 font-mono">Q3 2027</td>
              <td class="px-5 py-4 text-xs text-emerald-700 font-semibold">+15% role clarity</td>
            </tr>
            <tr class="phase-row">
              <td class="px-5 py-4 font-mono text-xs font-bold text-indigo-600">06</td>
              <td class="px-5 py-4 text-xs text-slate-700 font-semibold">Quarterly RACI governance review & onboarding playbook refresh</td>
              <td class="px-5 py-4"><span class="badge bg-indigo-50 text-indigo-700">Medium</span></td>
              <td class="px-5 py-4 text-xs text-slate-700">HR Director</td>
              <td class="px-5 py-4 text-xs text-slate-700 font-mono">Quarterly</td>
              <td class="px-5 py-4 text-xs text-emerald-700 font-semibold">Sustained governance</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- Literature Benchmarks -->
    <div class="bg-gradient-to-br from-slate-900 to-indigo-950 rounded-2xl p-6 text-white">
      <div class="flex items-center gap-2 mb-4">
        <i data-lucide="book-open" class="w-5 h-5 text-indigo-300"></i>
        <h3 class="font-display font-bold text-base">Academic & Corporate Literature Benchmarks</h3>
      </div>
      <p class="text-xs text-indigo-200/80 mb-5 max-w-3xl">The recommendations in this dashboard are grounded in peer-reviewed research and established HR scholarship. The following works form the theoretical foundation of Skillairo's onboarding and retention strategy.</p>
      <div class="grid md:grid-cols-2 gap-3">
        <div class="bg-white/5 backdrop-blur border border-white/10 rounded-xl p-4">
          <div class="flex items-start gap-3">
            <div class="w-9 h-9 rounded-lg bg-indigo-500/20 flex items-center justify-center flex-shrink-0">
              <i data-lucide="graduation-cap" class="w-4 h-4 text-indigo-300"></i>
            </div>
            <div>
              <div class="font-display font-semibold text-sm">Armstrong, M. (2020)</div>
              <div class="text-[11px] text-indigo-200/70 italic mb-1">Armstrong's Handbook of Human Resource Management Practice</div>
              <p class="text-xs text-indigo-100/80">Foundational reference for strategic HRM architecture, performance management frameworks, and talent development lifecycle integration.</p>
            </div>
          </div>
        </div>
        <div class="bg-white/5 backdrop-blur border border-white/10 rounded-xl p-4">
          <div class="flex items-start gap-3">
            <div class="w-9 h-9 rounded-lg bg-indigo-500/20 flex items-center justify-center flex-shrink-0">
              <i data-lucide="users" class="w-4 h-4 text-indigo-300"></i>
            </div>
            <div>
              <div class="font-display font-semibold text-sm">Bauer, T. N. (2010)</div>
              <div class="text-[11px] text-indigo-200/70 italic mb-1">Onboarding New Employees: Maximizing Success — SHRM Foundation</div>
              <p class="text-xs text-indigo-100/80">Source of the 4 Cs onboarding model (Compliance, Clarification, Culture, Connection) underpinning Skillairo's 30-day roadmap.</p>
            </div>
          </div>
        </div>
        <div class="bg-white/5 backdrop-blur border border-white/10 rounded-xl p-4">
          <div class="flex items-start gap-3">
            <div class="w-9 h-9 rounded-lg bg-indigo-500/20 flex items-center justify-center flex-shrink-0">
              <i data-lucide="zap" class="w-4 h-4 text-indigo-300"></i>
            </div>
            <div>
              <div class="font-display font-semibold text-sm">Cable, D. M. & Parsons, C. K. (2001)</div>
              <div class="text-[11px] text-indigo-200/70 italic mb-1">Journal of Applied Psychology — Socialization Tactics</div>
              <p class="text-xs text-indigo-100/80">Empirical evidence that structured socialization tactics significantly improve person-organization fit and reduce newcomer turnover.</p>
            </div>
          </div>
        </div>
        <div class="bg-white/5 backdrop-blur border border-white/10 rounded-xl p-4">
          <div class="flex items-start gap-3">
            <div class="w-9 h-9 rounded-lg bg-indigo-500/20 flex items-center justify-center flex-shrink-0">
              <i data-lucide="compass" class="w-4 h-4 text-indigo-300"></i>
            </div>
            <div>
              <div class="font-display font-semibold text-sm">Louis, M. R. (1980)</div>
              <div class="text-[11px] text-indigo-200/70 italic mb-1">Academy of Management Review — Surprise and Sense-making</div>
              <p class="text-xs text-indigo-100/80">Seminal work on newcomer sense-making; explains why structured onboarding reduces cognitive dissonance and accelerates role integration.</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- ===== TAB 6: ANNEXURES & SURVEY ===== -->
  <section id="tab-6" class="tab-panel hidden">
    <div class="flex items-center gap-2 mb-5 text-xs text-slate-500">
      <span class="px-2 py-0.5 rounded bg-indigo-50 text-indigo-700 font-bold">MODULE 16.0</span>
      <span>Annexures & Live Digital Pulse Survey Instrument</span>
    </div>

    <div class="grid lg:grid-cols-2 gap-5">
      <!-- Annexure 16.1: Weekly Work Log -->
      <div class="bg-white rounded-2xl border border-slate-200 shadow-sm overflow-hidden">
        <div class="p-5 border-b border-slate-200 bg-gradient-to-r from-slate-50 to-white">
          <div class="flex items-center gap-2 mb-1">
            <span class="px-2 py-0.5 rounded bg-indigo-600 text-white text-[10px] font-bold">ANNEXURE 16.1</span>
          </div>
          <h3 class="font-display font-bold text-slate-800 text-sm">Internship Project Weekly Work Log</h3>
          <p class="text-xs text-slate-500 mt-0.5">Submitted by Syed Subhana Ali, HR Intern — Session 2026–2027</p>
        </div>
        <div class="overflow-x-auto max-h-[600px] overflow-y-auto">
          <table class="w-full text-sm">
            <thead class="bg-slate-50 sticky top-0">
              <tr>
                <th class="text-left px-4 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Week</th>
                <th class="text-left px-4 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Focus Area</th>
                <th class="text-left px-4 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Module(s)</th>
                <th class="text-left px-4 py-3 text-[11px] uppercase tracking-wider text-slate-600 font-semibold">Deliverable</th>
              </tr>
            </thead>
            <tbody class="divide-y divide-slate-100">
              <tr><td class="px-4 py-3 text-xs font-bold text-indigo-600">W1</td><td class="px-4 py-3 text-xs text-slate-700">HR Analytics landscape study & data sourcing</td><td class="px-4 py-3 text-xs text-slate-500">1.0 – 2.0</td><td class="px-4 py-3 text-xs text-slate-700">Project charter, dataset schema</td></tr>
              <tr><td class="px-4 py-3 text-xs font-bold text-indigo-600">W2</td><td class="px-4 py-3 text-xs text-slate-700">Attrition diagnostic & KPI framework design</td><td class="px-4 py-3 text-xs text-slate-500">3.0 – 4.0</td><td class="px-4 py-3 text-xs text-slate-700">KPI definitions, baseline metrics</td></tr>
              <tr><td class="px-4 py-3 text-xs font-bold text-indigo-600">W3</td><td class="px-4 py-3 text-xs text-slate-700">Onboarding model research (Bauer 4 Cs)</td><td class="px-4 py-3 text-xs text-slate-500">5.0</td><td class="px-4 py-3 text-xs text-slate-700">4 Cs framework whitepaper</td></tr>
              <tr><td class="px-4 py-3 text-xs font-bold text-indigo-600">W4</td><td class="px-4 py-3 text-xs text-slate-700">Dashboard prototype & Excel engine integration</td><td class="px-4 py-3 text-xs text-slate-500">6.0</td><td class="px-4 py-3 text-xs text-slate-700">V1 dashboard UI + data pipeline</td></tr>
              <tr><td class="px-4 py-3 text-xs font-bold text-indigo-600">W5</td><td class="px-4 py-3 text-xs text-slate-700">Executive KPI dashboard finalization</td><td class="px-4 py-3 text-xs text-slate-500">7.0</td><td class="px-4 py-3 text-xs text-slate-700">Live KPI engine, Chart.js visuals</td></tr>
              <tr><td class="px-4 py-3 text-xs font-bold text-indigo-600">W6</td><td class="px-4 py-3 text-xs text-slate-700">30-day onboarding roadmap design</td><td class="px-4 py-3 text-xs text-slate-500">8.0</td><td class="px-4 py-3 text-xs text-slate-700">Phased execution roadmap</td></tr>
              <tr><td class="px-4 py-3 text-xs font-bold text-indigo-600">W7</td><td class="px-4 py-3 text-xs text-slate-700">RACI matrix & governance framework</td><td class="px-4 py-3 text-xs text-slate-500">9.0</td><td class="px-4 py-3 text-xs text-slate-700">Stakeholder responsibility matrix</td></tr>
              <tr><td class="px-4 py-3 text-xs font-bold text-indigo-600">W8</td><td class="px-4 py-3 text-xs text-slate-700">Peer buddy program design</td><td class="px-4 py-3 text-xs text-slate-500">10.0</td><td class="px-4 py-3 text-xs text-slate-700">Buddy role charter, selection criteria</td></tr>
              <tr><td class="px-4 py-3 text-xs font-bold text-indigo-600">W9</td><td class="px-4 py-3 text-xs text-slate-700">Training cadence & evaluation framework</td><td class="px-4 py-3 text-xs text-slate-500">11.0</td><td class="px-4 py-3 text-xs text-slate-700">4-week training schedule</td></tr>
              <tr><td class="px-4 py-3 text-xs font-bold text-indigo-600">W10</td><td class="px-4 py-3 text-xs text-slate-700">Insight synthesis & correlation analysis</td><td class="px-4 py-3 text-xs text-slate-500">12.0 – 13.0</td><td class="px-4 py-3 text-xs text-slate-700">Insight cards, statistical models</td></tr>
              <tr><td class="px-4 py-3 text-xs font-bold text-indigo-600">W11</td><td class="px-4 py-3 text-xs text-slate-700">Strategic recommendations formulation</td><td class="px-4 py-3 text-xs text-slate-500">14.0</td><td class="px-4 py-3 text-xs text-slate-700">Action plan with priorities & ROI</td></tr>
              <tr><td class="px-4 py-3 text-xs font-bold text-indigo-600">W12</td><td class="px-4 py-3 text-xs text-slate-700">Literature benchmark consolidation</td><td class="px-4 py-3 text-xs text-slate-500">15.0</td><td class="px-4 py-3 text-xs text-slate-700">Academic references library</td></tr>
              <tr><td class="px-4 py-3 text-xs font-bold text-indigo-600">W13</td><td class="px-4 py-3 text-xs text-slate-700">Pulse survey instrument design</td><td class="px-4 py-3 text-xs text-slate-500">16.0</td><td class="px-4 py-3 text-xs text-slate-700">Digital Likert survey tool</td></tr>
              <tr><td class="px-4 py-3 text-xs font-bold text-indigo-600">W14</td><td class="px-4 py-3 text-xs text-slate-700">Final dashboard polish & documentation</td><td class="px-4 py-3 text-xs text-slate-500">All</td><td class="px-4 py-3 text-xs text-slate-700">Production-ready deliverable</td></tr>
            </tbody>
          </table>
        </div>
      </div>

      <!-- Annexure 16.2: Pulse Survey -->
      <div class="bg-white rounded-2xl border border-slate-200 shadow-sm overflow-hidden">
        <div class="p-5 border-b border-slate-200 bg-gradient-to-r from-indigo-50 to-white">
          <div class="flex items-center gap-2 mb-1">
            <span class="px-2 py-0.5 rounded bg-emerald-600 text-white text-[10px] font-bold">ANNEXURE 16.2</span>
            <span class="px-2 py-0.5 rounded bg-emerald-50 text-emerald-700 text-[10px] font-bold">LIVE</span>
          </div>
          <h3 class="font-display font-bold text-slate-800 text-sm">30-Day New Hire Pulse Survey</h3>
          <p class="text-xs text-slate-500 mt-0.5">Likert-scale instrument (1 = Strongly Disagree → 5 = Strongly Agree)</p>
        </div>

        <div id="surveyForm" class="p-5">
          <form id="pulseSurvey" class="space-y-4">
            <!-- Header scale -->
            <div class="flex items-center justify-between text-[10px] text-slate-500 font-semibold uppercase tracking-wider pb-2 border-b border-slate-100">
              <span>Statement</span>
              <div class="flex gap-1.5">
                <span class="w-9 text-center">1</span><span class="w-9 text-center">2</span><span class="w-9 text-center">3</span><span class="w-9 text-center">4</span><span class="w-9 text-center">5</span>
              </div>
            </div>

            <!-- Compliance section -->
            <div class="text-[10px] font-bold text-rose-600 uppercase tracking-wider mt-3">Compliance</div>
            <div id="q-compliance" class="space-y-3"></div>

            <!-- Clarification -->
            <div class="text-[10px] font-bold text-amber-600 uppercase tracking-wider mt-4">Clarification</div>
            <div id="q-clarification" class="space-y-3"></div>

            <!-- Culture -->
            <div class="text-[10px] font-bold text-indigo-600 uppercase tracking-wider mt-4">Culture</div>
            <div id="q-culture" class="space-y-3"></div>

            <!-- Connection -->
            <div class="text-[10px] font-bold text-emerald-600 uppercase tracking-wider mt-4">Connection</div>
            <div id="q-connection" class="space-y-3"></div>

            <button type="submit" class="w-full mt-5 px-4 py-3 rounded-xl bg-navy text-white text-sm font-semibold hover:bg-indigo-700 transition flex items-center justify-center gap-2" style="background:var(--navy)">
              <i data-lucide="send" class="w-4 h-4"></i> Submit Pulse Survey
            </button>
          </form>
        </div>

        <!-- Confirmation state -->
        <div id="surveyConfirmation" class="hidden p-8 text-center">
          <div class="w-16 h-16 mx-auto rounded-2xl bg-emerald-100 flex items-center justify-center mb-4">
            <i data-lucide="check-circle-2" class="w-8 h-8 text-emerald-600"></i>
          </div>
          <h4 class="font-display font-bold text-slate-900 text-lg mb-2">Survey Submitted Successfully</h4>
          <p class="text-xs text-slate-500 mb-4 max-w-sm mx-auto">Thank you for your feedback. Your responses have been recorded and will inform Skillairo's continuous onboarding improvement.</p>
          <div class="bg-slate-50 rounded-xl p-4 max-w-xs mx-auto mb-4">
            <div class="text-[10px] text-slate-500 uppercase tracking-wider mb-1">Your Average Score</div>
            <div id="avgScore" class="font-display font-extrabold text-3xl text-indigo-600">0.0</div>
            <div class="text-xs text-slate-500">out of 5.0</div>
          </div>
          <button onclick="resetSurvey()" class="px-4 py-2 rounded-lg border border-slate-300 text-xs font-semibold text-slate-600 hover:bg-white hover:border-indigo-300 transition">
            Submit Another Response
          </button>
        </div>
      </div>
    </div>
  </section>
</main>

<!-- ============ FOOTER ============ -->
<footer class="navy-pattern text-white relative overflow-hidden mt-10">
  <div class="grid-pattern absolute inset-0 opacity-50"></div>
  <div class="relative max-w-[1400px] mx-auto px-6 lg:px-10 py-8">
    <div class="flex flex-wrap items-center justify-between gap-4">
      <div class="flex items-center gap-3">
        <div class="w-9 h-9 rounded-lg bg-gradient-to-br from-indigo-500 to-indigo-700 flex items-center justify-center">
          <i data-lucide="hexagon" class="w-5 h-5 text-white"></i>
        </div>
        <div>
          <div class="font-display font-bold text-sm">Skillairo HR Analytics & Executive Dashboard</div>
          <div class="text-[11px] text-indigo-200/70">Human Resources & Operations Department · Session 2026–2027</div>
        </div>
      </div>
      <div class="text-right">
        <div class="text-[11px] text-indigo-200/70">Project Lead / Intern</div>
        <div class="font-display font-semibold text-sm">Syed Subhana Ali</div>
      </div>
    </div>
    <div class="mt-6 pt-6 border-t border-white/10 flex flex-wrap items-center justify-between gap-3 text-[11px] text-indigo-200/60">
      <div>© 2027 Skillairo Corporate. All HR data processed in-browser via SheetJS — no server transmission.</div>
      <div class="flex items-center gap-4">
        <span class="flex items-center gap-1.5"><i data-lucide="shield" class="w-3 h-3"></i> GDPR-aligned</span>
        <span class="flex items-center gap-1.5"><i data-lucide="lock" class="w-3 h-3"></i> Client-side only</span>
      </div>
    </div>
  </div>
</footer>

<!-- ============ TOAST ============ -->
<div id="toast" class="fixed bottom-6 right-6 z-50 hidden">
  <div class="bg-slate-900 text-white px-4 py-3 rounded-xl shadow-2xl flex items-center gap-2 text-sm">
    <i data-lucide="check-circle-2" class="w-4 h-4 text-emerald-400"></i>
    <span id="toastMsg">Done</span>
  </div>
</div>

<script>
/* =====================================================
   SKILLAIRO HR ANALYTICS — CORE ENGINE
   ===================================================== */

// ---------- SAMPLE DATASET ----------
const SAMPLE_DATA = [
  {empId:'SKL-1001',department:'Sales',status:'Terminated',tenure:1.4,salary:4200,satisfaction:2.4,absentDays:11,trainingHours:8,buddy:'No'},
  {empId:'SKL-1002',department:'Sales',status:'Active',tenure:2.8,salary:4500,satisfaction:3.6,absentDays:5,trainingHours:16,buddy:'Yes'},
  {empId:'SKL-1003',department:'Sales',status:'Active',tenure:4.1,salary:5200,satisfaction:3.9,absentDays:4,trainingHours:20,buddy:'Yes'},
  {empId:'SKL-1004',department:'Sales',status:'Terminated',tenure:0.9,salary:4000,satisfaction:2.1,absentDays:14,trainingHours:6,buddy:'No'},
  {empId:'SKL-1005',department:'Sales',status:'Active',tenure:3.5,salary:4800,satisfaction:3.7,absentDays:6,trainingHours:18,buddy:'Yes'},
  {empId:'SKL-1006',department:'Sales',status:'Active',tenure:1.2,salary:4300,satisfaction:3.2,absentDays:7,trainingHours:12,buddy:'Yes'},
  {empId:'SKL-1007',department:'Sales',status:'Terminated',tenure:2.0,salary:4400,satisfaction:2.8,absentDays:9,trainingHours:10,buddy:'No'},
  {empId:'SKL-1008',department:'Sales',status:'Active',tenure:5.2,salary:5800,satisfaction:4.2,absentDays:3,trainingHours:24,buddy:'Yes'},
  {empId:'SKL-1009',department:'Sales',status:'Active',tenure:2.3,salary:4600,satisfaction:3.5,absentDays:5,trainingHours:14,buddy:'Yes'},
  {empId:'SKL-1010',department:'Sales',status:'Terminated',tenure:1.6,salary:4100,satisfaction:2.5,absentDays:12,trainingHours:7,buddy:'No'},

  {empId:'SKL-2001',department:'Engineering',status:'Active',tenure:4.5,salary:7200,satisfaction:4.3,absentDays:2,trainingHours:32,buddy:'Yes'},
  {empId:'SKL-2002',department:'Engineering',status:'Active',tenure:3.1,salary:6800,satisfaction:4.1,absentDays:3,trainingHours:28,buddy:'Yes'},
  {empId:'SKL-2003',department:'Engineering',status:'Active',tenure:6.2,salary:8500,satisfaction:4.5,absentDays:1,trainingHours:40,buddy:'Yes'},
  {empId:'SKL-2004',department:'Engineering',status:'Terminated',tenure:1.8,salary:6200,satisfaction:3.0,absentDays:8,trainingHours:14,buddy:'No'},
  {empId:'SKL-2005',department:'Engineering',status:'Active',tenure:2.7,salary:6500,satisfaction:3.9,absentDays:4,trainingHours:26,buddy:'Yes'},
  {empId:'SKL-2006',department:'Engineering',status:'Active',tenure:5.0,salary:7800,satisfaction:4.4,absentDays:2,trainingHours:36,buddy:'Yes'},
  {empId:'SKL-2007',department:'Engineering',status:'Active',tenure:3.8,salary:7000,satisfaction:4.0,absentDays:3,trainingHours:30,buddy:'Yes'},
  {empId:'SKL-2008',department:'Engineering',status:'Active',tenure:2.9,salary:6600,satisfaction:3.8,absentDays:4,trainingHours:24,buddy:'Yes'},

  {empId:'SKL-3001',department:'Human Resources',status:'Active',tenure:3.3,salary:5500,satisfaction:3.8,absentDays:4,trainingHours:22,buddy:'Yes'},
  {empId:'SKL-3002',department:'Human Resources',status:'Active',tenure:2.1,salary:5000,satisfaction:3.5,absentDays:5,trainingHours:18,buddy:'Yes'},
  {empId:'SKL-3003',department:'Human Resources',status:'Terminated',tenure:1.5,salary:4800,satisfaction:2.9,absentDays:9,trainingHours:10,buddy:'No'},
  {empId:'SKL-3004',department:'Human Resources',status:'Active',tenure:4.8,salary:6200,satisfaction:4.2,absentDays:3,trainingHours:28,buddy:'Yes'},
  {empId:'SKL-3005',department:'Human Resources',status:'Active',tenure:3.0,salary:5300,satisfaction:3.7,absentDays:4,trainingHours:20,buddy:'Yes'},

  {empId:'SKL-4001',department:'Finance',status:'Active',tenure:5.5,salary:6500,satisfaction:4.1,absentDays:2,trainingHours:24,buddy:'Yes'},
  {empId:'SKL-4002',department:'Finance',status:'Active',tenure:3.9,salary:5800,satisfaction:3.9,absentDays:3,trainingHours:20,buddy:'Yes'},
  {empId:'SKL-4003',department:'Finance',status:'Active',tenure:2.4,salary:5200,satisfaction:3.6,absentDays:4,trainingHours:18,buddy:'Yes'},
  {empId:'SKL-4004',department:'Finance',status:'Active',tenure:6.1,salary:7200,satisfaction:4.3,absentDays:2,trainingHours:30,buddy:'Yes'},
  {empId:'SKL-4005',department:'Finance',status:'Terminated',tenure:1.7,salary:4900,satisfaction:2.7,absentDays:10,trainingHours:9,buddy:'No'},

  {empId:'SKL-5001',department:'Marketing',status:'Active',tenure:2.8,salary:5000,satisfaction:3.9,absentDays:4,trainingHours:22,buddy:'Yes'},
  {empId:'SKL-5002',department:'Marketing',status:'Terminated',tenure:1.1,salary:4500,satisfaction:2.6,absentDays:10,trainingHours:8,buddy:'No'},
  {empId:'SKL-5003',department:'Marketing',status:'Active',tenure:3.6,salary:5400,satisfaction:4.0,absentDays:3,trainingHours:24,buddy:'Yes'},
  {empId:'SKL-5004',department:'Marketing',status:'Active',tenure:4.2,salary:5800,satisfaction:4.1,absentDays:3,trainingHours:26,buddy:'Yes'},
  {empId:'SKL-5005',department:'Marketing',status:'Active',tenure:2.0,salary:4700,satisfaction:3.4,absentDays:5,trainingHours:16,buddy:'Yes'},

  {empId:'SKL-6001',department:'Operations',status:'Active',tenure:3.0,salary:4500,satisfaction:3.5,absentDays:5,trainingHours:18,buddy:'Yes'},
  {empId:'SKL-6002',department:'Operations',status:'Active',tenure:2.5,salary:4200,satisfaction:3.4,absentDays:6,trainingHours:16,buddy:'Yes'},
  {empId:'SKL-6003',department:'Operations',status:'Terminated',tenure:1.3,salary:4000,satisfaction:2.7,absentDays:12,trainingHours:8,buddy:'No'},
  {empId:'SKL-6004',department:'Operations',status:'Active',tenure:4.4,salary:5000,satisfaction:3.8,absentDays:4,trainingHours:22,buddy:'Yes'},
  {empId:'SKL-6005',department:'Operations',status:'Active',tenure:3.7,salary:4800,satisfaction:3.6,absentDays:5,trainingHours:20,buddy:'Yes'},

  {empId:'SKL-7001',department:'Customer Success',status:'Active',tenure:2.2,salary:4300,satisfaction:3.3,absentDays:6,trainingHours:16,buddy:'Yes'},
  {empId:'SKL-7002',department:'Customer Success',status:'Terminated',tenure:0.8,salary:4000,satisfaction:2.3,absentDays:13,trainingHours:6,buddy:'No'},
  {empId:'SKL-7003',department:'Customer Success',status:'Active',tenure:3.4,salary:4700,satisfaction:3.6,absentDays:5,trainingHours:20,buddy:'Yes'},
  {empId:'SKL-7004',department:'Customer Success',status:'Active',tenure:1.9,salary:4200,satisfaction:3.1,absentDays:7,trainingHours:14,buddy:'Yes'},
  {empId:'SKL-7005',department:'Customer Success',status:'Active',tenure:4.0,salary:5200,satisfaction:3.9,absentDays:4,trainingHours:24,buddy:'Yes'},
  {empId:'SKL-7006',department:'Customer Success',status:'Terminated',tenure:1.4,salary:4100,satisfaction:2.6,absentDays:11,trainingHours:9,buddy:'No'},
];

// ---------- STATE ----------
let currentData = [];
let charts = { attrition: null, training: null };
let tableSearch = '';
let deptFilter = '';

// ---------- KPI CALCULATIONS ----------
function calculateKPIs(data) {
  const headcount = data.length;
  const terminated = data.filter(d => d.status === 'Terminated').length;
  const attritionRate = headcount > 0 ? (terminated / headcount) * 100 : 0;
  const esi = headcount > 0 ? data.reduce((s,d) => s + d.satisfaction, 0) / headcount : 0;
  const avgAbsent = headcount > 0 ? data.reduce((s,d) => s + d.absentDays, 0) / headcount : 0;
  return { headcount, terminated, attritionRate, esi, avgAbsent };
}

// Count-up animation
function animateValue(el, target, decimals = 0, duration = 800) {
  const start = parseFloat(el.textContent) || 0;
  const startTime = performance.now();
  function step(now) {
    const t = Math.min((now - startTime) / duration, 1);
    const eased = 1 - Math.pow(1 - t, 3);
    const value = start + (target - start) * eased;
    el.textContent = decimals > 0 ? value.toFixed(decimals) : Math.round(value);
    if (t < 1) requestAnimationFrame(step);
  }
  requestAnimationFrame(step);
}

function renderKPIs(data) {
  const k = calculateKPIs(data);
  animateValue(document.getElementById('kpi-headcount'), k.headcount);
  animateValue(document.getElementById('kpi-attrition'), k.attritionRate, 1);
  animateValue(document.getElementById('kpi-terminated'), k.terminated);
  animateValue(document.getElementById('kpi-esi'), k.esi, 2);
  animateValue(document.getElementById('kpi-absent'), k.avgAbsent, 1);

  // Status panel
  document.getElementById('recordCount').textContent = data.length;
  const depts = [...new Set(data.map(d => d.department))];
  document.getElementById('deptCount').textContent = depts.length;

  // Populate dept filter
  const filter = document.getElementById('deptFilter');
  const currentVal = filter.value;
  filter.innerHTML = '<option value="">All Departments</option>' +
    depts.map(d => `<option value="${d}" ${d===currentVal?'selected':''}>${d}</option>`).join('');
}

// ---------- CHARTS ----------
function getAttritionByDept(data) {
  const depts = [...new Set(data.map(d => d.department))];
  return depts.map(dept => {
    const subset = data.filter(d => d.department === dept);
    const term = subset.filter(d => d.status === 'Terminated').length;
    return { dept, rate: subset.length > 0 ? (term / subset.length) * 100 : 0 };
  }).sort((a,b) => b.rate - a.rate);
}

function getTrainingByDept(data) {
  const depts = [...new Set(data.map(d => d.department))];
  return depts.map(dept => {
    const subset = data.filter(d => d.department === dept);
    const avg = subset.length > 0 ? subset.reduce((s,d) => s + d.trainingHours, 0) / subset.length : 0;
    return { dept, avg: Math.round(avg*10)/10 };
  }).sort((a,b) => b.avg - a.avg);
}

const CHART_DEFAULTS = {
  font: { family: 'Inter, sans-serif' },
  colors: {
    grid: '#E2E8F0',
    text: '#64748B',
    indigo: '#4F46E5',
    rose: '#EF4444',
    emerald: '#10B981',
    amber: '#F59E0B',
    navy: '#1E1B4B'
  }
};

function renderCharts(data) {
  // Destroy existing
  if (charts.attrition) charts.attrition.destroy();
  if (charts.training) charts.training.destroy();

  const attritionData = getAttritionByDept(data);
  const trainingData = getTrainingByDept(data);

  // Attrition Chart
  const ctxA = document.getElementById('chart-attrition').getContext('2d');
  charts.attrition = new Chart(ctxA, {
    type: 'bar',
    data: {
      labels: attritionData.map(d => d.dept),
      datasets: [{
        label: 'Attrition Rate (%)',
        data: attritionData.map(d => d.rate),
        backgroundColor: attritionData.map(d => {
          if (d.rate >= 25) return '#EF4444';
          if (d.rate >= 15) return '#F59E0B';
          return '#10B981';
        }),
        borderRadius: 8,
        borderSkipped: false,
        maxBarThickness: 50
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: { display: false },
        tooltip: {
          backgroundColor: '#1E1B4B',
          padding: 12,
          titleFont: { size: 12, weight: '600' },
          bodyFont: { size: 12 },
          callbacks: { label: (c) => ` ${c.parsed.y.toFixed(1)}% attrition` }
        }
      },
      scales: {
        y: {
          beginAtZero: true,
          ticks: { color: CHART_DEFAULTS.colors.text, font: { size: 11 }, callback: v => v + '%' },
          grid: { color: CHART_DEFAULTS.colors.grid, drawBorder: false }
        },
        x: {
          ticks: { color: CHART_DEFAULTS.colors.text, font: { size: 11 } },
          grid: { display: false }
        }
      }
    }
  });

  // Training Chart
  const ctxT = document.getElementById('chart-training').getContext('2d');
  charts.training = new Chart(ctxT, {
    type: 'bar',
    data: {
      labels: trainingData.map(d => d.dept),
      datasets: [{
        label: 'Avg Training Hours',
        data: trainingData.map(d => d.avg),
        backgroundColor: '#4F46E5',
        borderRadius: 8,
        borderSkipped: false,
        maxBarThickness: 50
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: { display: false },
        tooltip: {
          backgroundColor: '#1E1B4B',
          padding: 12,
          callbacks: { label: (c) => ` ${c.parsed.y} hrs avg` }
        }
      },
      scales: {
        y: {
          beginAtZero: true,
          ticks: { color: CHART_DEFAULTS.colors.text, font: { size: 11 }, callback: v => v + 'h' },
          grid: { color: CHART_DEFAULTS.colors.grid, drawBorder: false }
        },
        x: {
          ticks: { color: CHART_DEFAULTS.colors.text, font: { size: 11 } },
          grid: { display: false }
        }
      }
    }
  });
}

// ---------- TABLE ----------
function renderTable(data) {
  const body = document.getElementById('dataBody');
  let filtered = data;
  if (tableSearch) {
    const q = tableSearch.toLowerCase();
    filtered = filtered.filter(d =>
      d.empId.toLowerCase().includes(q) ||
      d.department.toLowerCase().includes(q) ||
      d.status.toLowerCase().includes(q)
    );
  }
  if (deptFilter) filtered = filtered.filter(d => d.department === deptFilter);

  if (filtered.length === 0) {
    body.innerHTML = `<tr><td colspan="9" class="text-center py-10 text-slate-400 text-xs">No matching records found.</td></tr>`;
    return;
  }

  body.innerHTML = filtered.map(d => {
    const satColor = d.satisfaction >= 4 ? 'text-emerald-600' : d.satisfaction >= 3 ? 'text-amber-600' : 'text-rose-600';
    const trainColor = d.trainingHours >= 25 ? 'text-emerald-600' : d.trainingHours >= 15 ? 'text-amber-600' : 'text-rose-600';
    return `
      <tr>
        <td class="px-5 py-3 font-mono text-xs font-semibold text-slate-800">${d.empId}</td>
        <td class="px-5 py-3 text-xs text-slate-700">${d.department}</td>
        <td class="px-5 py-3">
          <span class="badge ${d.status === 'Active' ? 'badge-active' : 'badge-terminated'}">
            <span class="w-1.5 h-1.5 rounded-full ${d.status === 'Active' ? 'bg-emerald-500' : 'bg-rose-500'}"></span>
            ${d.status}
          </span>
        </td>
        <td class="px-5 py-3 text-right font-mono text-xs text-slate-700">${d.tenure.toFixed(1)}</td>
        <td class="px-5 py-3 text-right font-mono text-xs text-slate-700">$${d.salary.toLocaleString()}</td>
        <td class="px-5 py-3 text-right font-mono text-xs font-bold ${satColor}">${d.satisfaction.toFixed(1)}</td>
        <td class="px-5 py-3 text-right font-mono text-xs text-slate-700">${d.absentDays}</td>
        <td class="px-5 py-3 text-right font-mono text-xs font-bold ${trainColor}">${d.trainingHours}</td>
        <td class="px-5 py-3 text-center">
          <span class="badge ${d.buddy === 'Yes' ? 'badge-yes' : 'badge-no'}">${d.buddy}</span>
        </td>
      </tr>
    `;
  }).join('');
}

// ---------- RACI MATRIX ----------
const RACI_DATA = [
  ['Pre-boarding documentation & asset setup', 'A/R', 'C', 'I', 'I'],
  ['Welcome & orientation session', 'A/R', 'C', 'C', 'I'],
  ['Compliance & policy walkthrough', 'A/R', 'I', 'I', 'R'],
  ['Role clarification & KPI mirroring', 'C', 'A/R', 'I', 'R'],
  ['Tool & system access provisioning', 'R', 'A', 'C', 'I'],
  ['Peer buddy pairing & introduction', 'A/R', 'C', 'R', 'I'],
  ['Cultural immersion & values session', 'R', 'C', 'A', 'I'],
  ['Functional training delivery', 'C', 'A', 'R', 'R'],
  ['First deliverable review & feedback', 'I', 'A/R', 'C', 'R'],
  ['30-day pulse survey administration', 'A/R', 'C', 'I', 'R'],
  ['Performance calibration & confirmation', 'C', 'A/R', 'I', 'I'],
  ['90-day development plan finalization', 'C', 'A/R', 'C', 'R'],
];

function renderRACI() {
  const body = document.getElementById('raciBody');
  const colorMap = {
    'R': 'bg-rose-50 text-rose-700 border-rose-200',
    'A': 'bg-amber-50 text-amber-700 border-amber-200',
    'C': 'bg-indigo-50 text-indigo-700 border-indigo-200',
    'I': 'bg-slate-100 text-slate-600 border-slate-200'
  };
  body.innerHTML = RACI_DATA.map(row => `
    <tr class="phase-row">
      <td class="px-5 py-3 text-xs text-slate-700 font-medium">${row[0]}</td>
      <td class="px-3 py-3 text-center">${row[1].split('/').map(c => `<span class="inline-flex items-center justify-center w-7 h-7 rounded-md border text-[10px] font-bold mx-0.5 ${colorMap[c]}">${c}</span>`).join('')}</td>
      <td class="px-3 py-3 text-center">${row[2].split('/').map(c => `<span class="inline-flex items-center justify-center w-7 h-7 rounded-md border text-[10px] font-bold mx-0.5 ${colorMap[c]}">${c}</span>`).join('')}</td>
      <td class="px-3 py-3 text-center">${row[3].split('/').map(c => `<span class="inline-flex items-center justify-center w-7 h-7 rounded-md border text-[10px] font-bold mx-0.5 ${colorMap[c]}">${c}</span>`).join('')}</td>
      <td class="px-3 py-3 text-center">${row[4].split('/').map(c => `<span class="inline-flex items-center justify-center w-7 h-7 rounded-md border text-[10px] font-bold mx-0.5 ${colorMap[c]}">${c}</span>`).join('')}</td>
    </tr>
  `).join('');
}

// ---------- SURVEY ----------
const SURVEY_QUESTIONS = {
  compliance: [
    'I received all necessary compliance documentation within my first 48 hours.',
    'I clearly understand Skillairo\'s code of conduct, policies, and safety protocols.'
  ],
  clarification: [
    'My role responsibilities and KPIs are clearly defined and documented.',
    'I understand how my individual performance contributes to Skillairo\'s strategic goals.'
  ],
  culture: [
    'I feel a strong sense of belonging to the Skillairo team and culture.',
    'The organizational values demonstrated in daily work match what was communicated during onboarding.'
  ],
  connection: [
    'My peer buddy has been instrumental in helping me navigate the organization.',
    'I have built meaningful professional relationships with my team members and cross-functional colleagues.'
  ]
};

function renderSurvey() {
  Object.keys(SURVEY_QUESTIONS).forEach(cat => {
    const container = document.getElementById('q-' + cat);
    container.innerHTML = SURVEY_QUESTIONS[cat].map((q, i) => `
      <div class="flex items-center justify-between gap-3 py-2">
        <p class="text-xs text-slate-700 flex-1 leading-relaxed">${q}</p>
        <div class="flex gap-1.5 flex-shrink-0">
          ${[1,2,3,4,5].map(n => `
            <div class="likert-option">
              <input type="radio" name="q_${cat}_${i}" id="q_${cat}_${i}_${n}" value="${n}" required>
              <label for="q_${cat}_${i}_${n}">${n}</label>
            </div>
          `).join('')}
        </div>
      </div>
    `).join('');
  });

  document.getElementById('pulseSurvey').addEventListener('submit', (e) => {
    e.preventDefault();
    const formData = new FormData(e.target);
    const values = [...formData.values()].map(Number);
    const avg = values.reduce((s,v) => s+v, 0) / values.length;
    document.getElementById('surveyForm').classList.add('hidden');
    document.getElementById('surveyConfirmation').classList.remove('hidden');
    animateValue(document.getElementById('avgScore'), avg, 2, 1000);
    lucide.createIcons();
  });
}

function resetSurvey() {
  document.getElementById('pulseSurvey').reset();
  document.getElementById('surveyForm').classList.remove('hidden');
  document.getElementById('surveyConfirmation').classList.add('hidden');
}

// ---------- TAB SWITCHING ----------
function switchTab(num) {
  document.querySelectorAll('.tab-panel').forEach(p => p.classList.add('hidden'));
  document.getElementById('tab-' + num).classList.remove('hidden');
  document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
  document.querySelector(`[data-tab="${num}"]`).classList.add('active');
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

// ---------- FILE UPLOAD ----------
function parseFile(file) {
  const reader = new FileReader();
  reader.onload = (e) => {
    try {
      const data = new Uint8Array(e.target.result);
      const wb = XLSX.read(data, { type: 'array' });
      const ws = wb.Sheets[wb.SheetNames[0]];
      const json = XLSX.utils.sheet_to_json(ws, { defval: '' });
      const parsed = json.map(row => {
        // Flexible column matching
        const get = (...keys) => {
          for (const k of keys) {
            for (const rk of Object.keys(row)) {
              if (rk.toLowerCase().trim() === k.toLowerCase()) return row[rk];
            }
          }
          return '';
        };
        return {
          empId: String(get('Emp ID', 'Employee ID', 'EmpID', 'ID') || '').trim(),
          department: String(get('Department', 'Dept') || '').trim(),
          status: String(get('Status') || '').trim().charAt(0).toUpperCase() + String(get('Status') || '').trim().slice(1).toLowerCase(),
          tenure: parseFloat(get('Tenure (Yrs)', 'Tenure', 'Tenure Yrs') || 0),
          salary: parseFloat(get('Monthly Salary ($)', 'Salary', 'Monthly Salary') || 0),
          satisfaction: parseFloat(get('Satisfaction Score (1-5)', 'Satisfaction', 'Satisfaction Score') || 0),
          absentDays: parseFloat(get('Absent Days', 'AbsentDays') || 0),
          trainingHours: parseFloat(get('Training Hours', 'TrainingHours') || 0),
          buddy: String(get('Buddy Assigned (Yes/No)', 'Buddy Assigned', 'Buddy') || '').trim().charAt(0).toUpperCase() + String(get('Buddy Assigned (Yes/No)', 'Buddy Assigned', 'Buddy') || '').trim().slice(1).toLowerCase()
        };
      }).filter(d => d.empId);

      if (parsed.length === 0) {
        showToast('No valid records found in file', 'error');
        return;
      }

      currentData = parsed;
      document.getElementById('dataSource').textContent = file.name;
      refreshAll();
      showToast(`Loaded ${parsed.length} records from ${file.name}`);
    } catch (err) {
      console.error(err);
      showToast('Failed to parse file. Please check format.', 'error');
    }
  };
  reader.readAsArrayBuffer(file);
}

function refreshAll() {
  renderKPIs(currentData);
  renderCharts(currentData);
  renderTable(currentData);
}

function resetData() {
  currentData = JSON.parse(JSON.stringify(SAMPLE_DATA));
  document.getElementById('dataSource').textContent = 'In-Memory Sample';
  refreshAll();
  showToast('Sample dataset restored');
}

function showToast(msg, type = 'success') {
  const toast = document.getElementById('toast');
  document.getElementById('toastMsg').textContent = msg;
  toast.classList.remove('hidden');
  setTimeout(() => toast.classList.add('hidden'), 3000);
}

// ---------- INIT ----------
document.addEventListener('DOMContentLoaded', () => {
  lucide.createIcons();
  currentData = JSON.parse(JSON.stringify(SAMPLE_DATA));
  refreshAll();
  renderRACI();
  renderSurvey();

  // Dropzone
  const dz = document.getElementById('dropzone');
  const fi = document.getElementById('fileInput');

  dz.addEventListener('click', () => fi.click());
  dz.addEventListener('dragover', (e) => { e.preventDefault(); dz.classList.add('dragging'); });
  dz.addEventListener('dragleave', () => dz.classList.remove('dragging'));
  dz.addEventListener('drop', (e) => {
    e.preventDefault();
    dz.classList.remove('dragging');
    if (e.dataTransfer.files.length) parseFile(e.dataTransfer.files[0]);
  });
  fi.addEventListener('change', (e) => {
    if (e.target.files.length) parseFile(e.target.files[0]);
  });

  // Search & filter
  document.getElementById('tableSearch').addEventListener('input', (e) => {
    tableSearch = e.target.value;
    renderTable(currentData);
  });
  document.getElementById('deptFilter').addEventListener('change', (e) => {
    deptFilter = e.target.value;
    renderTable(currentData);
  });
});
</script>

</body>
</html>
