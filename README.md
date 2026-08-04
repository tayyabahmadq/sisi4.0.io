<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sisi Systems | Sovereign Air-Gapped AI Operating System</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;600&display=swap');

        :root {
            --bg-black: #080808;
            --card-black: #121212;
            --card-black-hover: #1a1a1a;
            --border-dark: #242424;
            --border-red: #450a0a;
            --text-white: #f8fafc;
            --text-muted: #94a3b8;
            --primary-red: #e11d48;
            --primary-red-bright: #ff2b56;
            --red-glow: rgba(225, 29, 72, 0.25);
            --card-red-bg: rgba(225, 29, 72, 0.05);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            scroll-behavior: smooth;
        }

        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background-color: var(--bg-black);
            color: var(--text-white);
            line-height: 1.6;
            overflow-x: hidden;
        }

        /* Ambient Glow Effect */
        .ambient-glow {
            position: fixed;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 100vw;
            height: 100vh;
            background: radial-gradient(circle at 50% 10%, var(--red-glow) 0%, transparent 50%);
            pointer-events: none;
            z-index: 0;
        }

        .container {
            max-width: 1240px;
            margin: 0 auto;
            padding: 0 24px;
            position: relative;
            z-index: 1;
        }

        /* Navbar */
        header {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            z-index: 1000;
            background: rgba(8, 8, 8, 0.85);
            backdrop-filter: blur(16px);
            border-bottom: 1px solid var(--border-dark);
            padding: 18px 0;
        }

        .nav-container {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .logo {
            font-size: 20px;
            font-weight: 800;
            letter-spacing: -0.5px;
            display: flex;
            align-items: center;
            gap: 10px;
            color: var(--text-white);
            text-decoration: none;
        }

        .logo-badge {
            background: linear-gradient(135deg, var(--primary-red), #9f1239);
            color: white;
            font-size: 10px;
            padding: 3px 8px;
            border-radius: 4px;
            text-transform: uppercase;
            font-weight: 800;
            letter-spacing: 1px;
        }

        .nav-links {
            display: flex;
            gap: 32px;
            list-style: none;
            align-items: center;
        }

        .nav-links a {
            color: var(--text-muted);
            text-decoration: none;
            font-size: 14px;
            font-weight: 600;
            transition: color 0.2s;
        }

        .nav-links a:hover {
            color: var(--text-white);
        }

        .btn {
            background: linear-gradient(135deg, var(--primary-red), var(--primary-red-bright));
            color: white;
            padding: 12px 24px;
            border-radius: 6px;
            font-size: 14px;
            font-weight: 700;
            text-decoration: none;
            transition: all 0.3s ease;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            border: none;
            cursor: pointer;
            box-shadow: 0 4px 20px rgba(225, 29, 72, 0.3);
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 30px rgba(225, 29, 72, 0.5);
        }

        .btn-secondary {
            background: var(--card-black);
            border: 1px solid var(--border-dark);
            color: var(--text-white);
            box-shadow: none;
        }

        .btn-secondary:hover {
            background: var(--card-black-hover);
            border-color: #404040;
            box-shadow: none;
        }

        /* Hero Section */
        section {
            padding: 100px 0;
            border-bottom: 1px solid var(--border-dark);
        }

        .hero {
            padding-top: 180px;
            padding-bottom: 100px;
            text-align: center;
        }

        .pill-badge {
            display: inline-flex;
            align-items: center;
            gap: 8px;
            background: var(--card-red-bg);
            border: 1px solid var(--border-red);
            color: #fca5a5;
            padding: 8px 18px;
            border-radius: 100px;
            font-size: 13px;
            font-weight: 700;
            letter-spacing: 0.5px;
            margin-bottom: 28px;
        }

        .hero-title {
            font-size: 58px;
            line-height: 1.1;
            margin-bottom: 24px;
            max-width: 950px;
            margin-left: auto;
            margin-right: auto;
        }

        .hero-title span {
            background: linear-gradient(135deg, #ffffff, #94a3b8);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .hero-title .highlight {
            background: linear-gradient(135deg, var(--primary-red-bright), #fb7185);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .hero-subtitle {
            font-size: 19px;
            color: var(--text-muted);
            max-width: 820px;
            margin: 0 auto 40px auto;
            font-weight: 400;
            line-height: 1.6;
        }

        .hero-cta {
            display: flex;
            gap: 16px;
            justify-content: center;
            align-items: center;
        }

        /* Metrics Grid */
        .metrics-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 20px;
            margin-top: 70px;
        }

        .metric-card {
            background: var(--card-black);
            border: 1px solid var(--border-dark);
            padding: 28px 24px;
            border-radius: 12px;
            text-align: left;
            transition: all 0.3s ease;
        }

        .metric-card:hover {
            border-color: var(--primary-red);
            transform: translateY(-3px);
        }

        .metric-card h4 {
            font-size: 36px;
            font-weight: 800;
            color: var(--primary-red-bright);
            margin-bottom: 6px;
        }

        .metric-card p {
            font-size: 13px;
            color: var(--text-muted);
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        /* Section Headers */
        .section-header {
            margin-bottom: 60px;
            text-align: left;
        }

        .section-tag {
            color: var(--primary-red-bright);
            font-size: 12px;
            font-weight: 800;
            text-transform: uppercase;
            letter-spacing: 2px;
            margin-bottom: 10px;
            display: block;
        }

        .section-title {
            font-size: 38px;
            color: var(--text-white);
            margin-bottom: 16px;
        }

        .section-desc {
            font-size: 16px;
            color: var(--text-muted);
            max-width: 780px;
            line-height: 1.6;
        }

        /* Cards and Grids */
        .grid-3 {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 24px;
        }

        .grid-2 {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 24px;
        }

        .grid-4 {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 20px;
        }

        .feature-card {
            background: var(--card-black);
            border: 1px solid var(--border-dark);
            padding: 32px;
            border-radius: 12px;
            transition: all 0.3s ease;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }

        .feature-card:hover {
            background: var(--card-black-hover);
            border-color: var(--border-red);
            transform: translateY(-4px);
        }

        .card-tag {
            background: var(--card-red-bg);
            border: 1px solid var(--border-red);
            color: var(--primary-red-bright);
            font-size: 11px;
            font-weight: 800;
            padding: 4px 10px;
            border-radius: 4px;
            text-transform: uppercase;
            letter-spacing: 1px;
            display: inline-block;
            margin-bottom: 16px;
            width: fit-content;
        }

        .feature-card h3 {
            font-size: 20px;
            margin-bottom: 12px;
            color: var(--text-white);
        }

        .feature-card p {
            font-size: 14.5px;
            color: var(--text-muted);
            line-height: 1.65;
        }

        /* Interactive Architecture Cards (Replacing Diagrams) */
        .arch-card {
            background: var(--card-black);
            border: 1px solid var(--border-dark);
            border-radius: 12px;
            padding: 32px;
            position: relative;
            overflow: hidden;
        }

        .arch-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 4px;
            height: 100%;
            background: var(--primary-red);
        }

        .arch-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 16px;
        }

        .arch-title {
            font-size: 22px;
            color: var(--text-white);
            font-weight: 700;
        }

        .arch-badge {
            font-family: 'JetBrains Mono', monospace;
            font-size: 11px;
            background: #1e1e1e;
            color: #f1f5f9;
            padding: 4px 10px;
            border-radius: 4px;
            border: 1px solid #333;
        }

        .arch-text {
            font-size: 15px;
            color: var(--text-muted);
            line-height: 1.7;
            margin-bottom: 20px;
        }

        .arch-list {
            list-style: none;
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 12px;
        }

        .arch-list li {
            font-size: 13.5px;
            color: var(--text-white);
            background: rgba(255, 255, 255, 0.03);
            border: 1px solid var(--border-dark);
            padding: 10px 14px;
            border-radius: 6px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .arch-list li::before {
            content: '✓';
            color: var(--primary-red-bright);
            font-weight: 800;
        }

        /* Differentiators Table */
        .table-wrapper {
            background: var(--card-black);
            border: 1px solid var(--border-dark);
            border-radius: 12px;
            overflow: hidden;
        }

        table {
            width: 100%;
            border-collapse: collapse;
        }

        th {
            background: #181818;
            padding: 20px 24px;
            text-align: left;
            font-size: 13px;
            font-weight: 800;
            text-transform: uppercase;
            letter-spacing: 1px;
            color: var(--text-white);
            border-bottom: 1px solid var(--border-dark);
        }

        td {
            padding: 20px 24px;
            font-size: 14.5px;
            border-bottom: 1px solid var(--border-dark);
            color: var(--text-muted);
        }

        tr:last-child td {
            border-bottom: none;
        }

        td.feature-title-cell {
            color: var(--text-white);
            font-weight: 700;
        }

        td.sisi-highlight {
            color: var(--text-white);
            font-weight: 700;
            background: var(--card-red-bg);
            border-left: 2px solid var(--primary-red);
        }

        /* Roadmap Timeline Cards */
        .roadmap-card {
            background: var(--card-black);
            border: 1px solid var(--border-dark);
            padding: 28px;
            border-radius: 12px;
            position: relative;
            transition: all 0.3s ease;
        }

        .roadmap-card.milestone {
            border-color: var(--primary-red);
            background: linear-gradient(180deg, var(--card-black) 0%, var(--card-red-bg) 100%);
            box-shadow: 0 0 25px rgba(225, 29, 72, 0.15);
        }

        .roadmap-phase {
            font-size: 12px;
            font-weight: 800;
            color: var(--primary-red-bright);
            text-transform: uppercase;
            letter-spacing: 1.5px;
            margin-bottom: 10px;
            display: block;
        }

        .roadmap-card h3 {
            font-size: 18px;
            margin-bottom: 12px;
            color: var(--text-white);
        }

        .roadmap-card p {
            font-size: 13.5px;
            color: var(--text-muted);
            line-height: 1.6;
        }

        /* CTA Banner Section */
        .cta-banner {
            background: linear-gradient(135deg, #181818 0%, #0d0d0d 100%);
            border: 1px solid var(--border-red);
            border-radius: 16px;
            padding: 60px 40px;
            text-align: center;
            position: relative;
            overflow: hidden;
            margin-top: 40px;
        }

        .cta-banner h2 {
            font-size: 36px;
            margin-bottom: 16px;
        }

        .cta-banner p {
            font-size: 16px;
            color: var(--text-muted);
            max-width: 650px;
            margin: 0 auto 32px auto;
        }

        /* Footer */
        footer {
            padding: 80px 0 40px 0;
            background: #050508;
            border-top: 1px solid var(--border-dark);
        }

        .footer-grid {
            display: grid;
            grid-template-columns: 2fr 1fr 1fr 1fr;
            gap: 40px;
            margin-bottom: 60px;
        }

        .footer-logo {
            font-size: 20px;
            font-weight: 900;
            color: var(--text-white);
            margin-bottom: 16px;
            display: inline-block;
        }

        .footer-text {
            color: var(--text-muted);
            font-size: 14px;
            line-height: 1.6;
            max-width: 360px;
        }

        .footer-col h4 {
            font-size: 14px;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 20px;
            color: var(--text-white);
        }

        .footer-col ul {
            list-style: none;
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .footer-col a {
            color: var(--text-muted);
            text-decoration: none;
            font-size: 14px;
            transition: color 0.2s;
        }

        .footer-col a:hover {
            color: var(--text-white);
        }

        .footer-bottom {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding-top: 30px;
            border-top: 1px solid var(--border-dark);
            font-size: 13px;
            color: var(--text-muted);
        }

        /* Responsive Breakpoints */
        @media (max-width: 1024px) {
            .hero-title { font-size: 42px; }
            .grid-3, .metrics-grid, .roadmap-grid, .grid-4 { grid-template-columns: repeat(2, 1fr); }
            .arch-list { grid-template-columns: 1fr; }
        }

        @media (max-width: 640px) {
            .hero-title { font-size: 32px; }
            .grid-2, .grid-3, .metrics-grid, .roadmap-grid, .grid-4, .footer-grid { grid-template-columns: 1fr; }
            .nav-links { display: none; }
        }
    </style>
</head>
<body>

    <div class="ambient-glow"></div>

    <!-- NAVBAR -->
    <header>
        <div class="container nav-container">
            <a href="#" class="logo">
                SISI SYSTEMS <span class="logo-badge">OS 4.0</span>
            </a>
            <ul class="nav-links">
                <li><a href="#problem">Market Bottleneck</a></li>
                <li><a href="#solution">Sisi 4.0 Solution</a></li>
                <li><a href="#architecture">Decoupled Systems</a></li>
                <li><a href="#differentiators">Competitive Moat</a></li>
                <li><a href="#roadmap">Go-To-Market</a></li>
            </ul>
            <a href="#contact" class="btn">Inquire for Pilot</a>
        </div>
    </header>

    <!-- HERO SECTION -->
    <section class="hero">
        <div class="container">
            <div class="pill-badge">ADGM Target Entity • Hub71+ Access Program Applicant</div>
            <h1 class="hero-title">Uncompromising Data Sovereignty Meets <span class="highlight">100% Deterministic AI</span></h1>
            <p class="hero-subtitle">Sisi 4.0 is the world's premier Air-Gapped, Zero-Hallucination AI Operating System engineered from bare metal up for national defense, central banking, healthcare networks, and sovereign enterprise infrastructure.</p>
            
            <div class="hero-cta">
                <a href="#contact" class="btn">Request Private Pilot Demo →</a>
                <a href="#architecture" class="btn btn-secondary">Explore Architecture</a>
            </div>

            <div class="metrics-grid">
                <div class="metric-card">
                    <h4>100%</h4>
                    <p>Air-Gapped Local Isolation</p>
                </div>
                <div class="metric-card">
                    <h4>0%</h4>
                    <p>Hallucination Risk (Formal Proofs)</p>
                </div>
                <div class="metric-card">
                    <h4>&lt;1µs</h4>
                    <p>SIMD Threat & Injection Scrubbing</p>
                </div>
                <div class="metric-card">
                    <h4>Month 5</h4>
                    <p>Commercial Go-Live Launch</p>
                </div>
            </div>
        </div>
    </section>

    <!-- PROBLEM SECTION -->
    <section id="problem">
        <div class="container">
            <div class="section-header">
                <span class="section-tag">01 / Market Vulnerability</span>
                <h2 class="section-title">The Enterprise AI Bottleneck</h2>
                <p class="section-desc">Global governments, central banks, and regulated entities are currently locked out of generative AI adoption due to critical structural vulnerabilities in cloud LLM providers.</p>
            </div>

            <div class="grid-3">
                <div class="feature-card">
                    <div>
                        <span class="card-tag">Security Risk</span>
                        <h3>Cloud Data Leaks & Telemetry Exposure</h3>
                        <p>Off-the-shelf generative models require streaming confidential corporate records, IP, and state secrets across third-party web APIs. This directly violates UAE Data Protection Laws, ADGM frameworks, and national sovereignty mandates.</p>
                    </div>
                </div>

                <div class="feature-card">
                    <div>
                        <span class="card-tag">Reliability Risk</span>
                        <h3>Probabilistic Model Hallucinations</h3>
                        <p>Current LLMs guess outputs based on token probability rather than logic. In high-stakes environments like central banking, defense, compliance, and medical diagnostics, a 95% accuracy rate represents a 100% liability risk.</p>
                    </div>
                </div>

                <div class="feature-card">
                    <div>
                        <span class="card-tag">Operational Threat</span>
                        <h3>Unshielded Kernel Threat Vectors</h3>
                        <p>Standard AI wrappers lack kernel-level process isolation, leaving internal hardware vulnerable to prompt injection exploits, malicious script execution, and memory buffer corruption.</p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- SOLUTION SECTION -->
    <section id="solution">
        <div class="container">
            <div class="section-header">
                <span class="section-tag">02 / The Sovereign Operating Brain</span>
                <h2 class="section-title">Sisi 4.0 Operating System</h2>
                <p class="section-desc">A bare-metal, air-gapped operating system designed to bridge high-throughput AI reasoning with absolute data privacy and mathematical precision.</p>
            </div>

            <div class="grid-2">
                <div class="feature-card">
                    <span class="card-tag">Air-Gapped Core</span>
                    <h3>100% On-Premise Hardware Isolation</h3>
                    <p>Web crawling and external data retrieval are strictly trapped inside a sandboxed buffer. Internal reasoning models operate 100% offline on local hardware with zero outbound telemetry calls.</p>
                </div>

                <div class="feature-card">
                    <span class="card-tag">Formal Logic Engine</span>
                    <h3>Zero-Hallucination Verification</h3>
                    <p>Integrates formal mathematical logic provers to mathematically verify every step of reasoning before an answer is rendered, replacing probabilistic guessing with deterministic certainty.</p>
                </div>

                <div class="feature-card">
                    <span class="card-tag">Self-Healing Bus</span>
                    <h3>Autonomous Background System Maintenance</h3>
                    <p>An automated maintenance process continuously scans local system binaries, discovers code bottlenecks, tests fixes in an isolated minor replica sandbox, and deploys verified patches.</p>
                </div>

                <div class="feature-card">
                    <span class="card-tag">Dual Accessibility</span>
                    <h3>Human & AI Knowledge Architecture</h3>
                    <p>Knowledge is stored in plain-English structured files, allowing AI models sub-2ms deterministic B-Tree access while rendering visual web dashboards for non-technical human auditors.</p>
                </div>
            </div>
        </div>
    </section>

    <!-- DECOUPLED ARCHITECTURE SECTION (REPLACING DIAGRAM WITH DESCRIPTIVE MODULE CARDS) -->
    <section id="architecture">
        <div class="container">
            <div class="section-header">
                <span class="section-tag">03 / Core Engineering</span>
                <h2 class="section-title">Decoupled Subsystem Architecture</h2>
                <p class="section-desc">Every domain in Sisi 4.0 operates in complete process isolation under the KING supervisor. A failure or restart in one domain never causes a cascading failure across the OS.</p>
            </div>

            <div class="grid-2" style="margin-bottom: 24px;">
                <div class="arch-card">
                    <div class="arch-header">
                        <h3 class="arch-title">AIFM Management Dashboard</h3>
                        <span class="arch-badge">Master Control Layer</span>
                    </div>
                    <p class="arch-text">The centralized administrative switchboard providing master process power switches, dynamic worker swarm allocation (1 to 300 active agents), and immutable multi-vault audit logging.</p>
                    <ul class="arch-list">
                        <li>Domain Power Control</li>
                        <li>Dynamic Swarm Allocator</li>
                        <li>Top-N Speed Filter</li>
                        <li>Isolated Audit Vaults</li>
                    </ul>
                </div>

                <div class="arch-card">
                    <div class="arch-header">
                        <h3 class="arch-title">KING Kernel Supervisor</h3>
                        <span class="arch-badge">Sovereign Anchor</span>
                    </div>
                    <p class="arch-text">The central system supervisor enforcing absolute dependency decoupling. KING continuously monitors thread health via eBPF kernel watchdogs and prevents cross-domain memory breaches.</p>
                    <ul class="arch-list">
                        <li>Bare-Metal Kernel Anchor</li>
                        <li>eBPF Process Watchdogs</li>
                        <li>POSIX Thread Isolation</li>
                        <li>IPC Telemetry Router</li>
                    </ul>
                </div>
            </div>

            <div class="grid-3">
                <div class="feature-card">
                    <span class="card-tag">Sandboxed Buffer</span>
                    <h3>Green Zone DMZ</h3>
                    <p>The sole internet-facing fetcher. Connects to web endpoints and streams raw text into a isolated network buffer with zero internal execution rights.</p>
                </div>

                <div class="feature-card">
                    <span class="card-tag">Security Gate</span>
                    <h3>Commander Funnel Team</h3>
                    <p>Sub-microsecond sanitization pipeline that scrubs prompt injections, malicious scripts, conversational clutter, and factual noise before data touches storage.</p>
                </div>

                <div class="feature-card">
                    <span class="card-tag">Execution Enclosure</span>
                    <h3>Isolated Sarina Atmosphere</h3>
                    <p>Houses the core operating brain (ARIS) and the background self-healing bus (JULES). Operates completely air-gapped from internet-facing fetchers.</p>
                </div>
            </div>
        </div>
    </section>

    <!-- DIFFERENTIATORS SECTION -->
    <section id="differentiators">
        <div class="container">
            <div class="section-header">
                <span class="section-tag">04 / Competitive Advantage</span>
                <h2 class="section-title">Proprietary Core Differentiators</h2>
                <p class="section-desc">Why Sisi 4.0 outperforms commercial cloud LLMs across regulated enterprise sectors.</p>
            </div>

            <div class="table-wrapper">
                <table>
                    <thead>
                        <tr>
                            <th>Capability Dimension</th>
                            <th>Standard Commercial LLM Wrappers</th>
                            <th>Sisi 4.0 Operating System</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td class="feature-title-cell">Execution Boundary</td>
                            <td>Third-Party Cloud APIs (Shared Hardware)</td>
                            <td class="sisi-highlight">100% Air-Gapped On-Premise Local Hardware Isolation</td>
                        </tr>
                        <tr>
                            <td class="feature-title-cell">Accuracy Guarantee</td>
                            <td>Probabilistic (Prone to Unpredictable Hallucinations)</td>
                            <td class="sisi-highlight">100% Formal Mathematical Logic Verification</td>
                        </tr>
                        <tr>
                            <td class="feature-title-cell">Threat Scrubbing</td>
                            <td>Basic Post-Processing Prompt Filters</td>
                            <td class="sisi-highlight">Ultra-Fast Threat & Injection Scrubbing Engine</td>
                        </tr>
                        <tr>
                            <td class="feature-title-cell">System Maintenance</td>
                            <td>Manual Developer Engineering & Patching</td>
                            <td class="sisi-highlight">Autonomous Background System Self-Maintenance</td>
                        </tr>
                        <tr>
                            <td class="feature-title-cell">Audit Governance</td>
                            <td>Black-Box Cloud Logs (Telemetry Leaks)</td>
                            <td class="sisi-highlight">Multi-Vault Isolated Audit History (/decision_logs/)</td>
                        </tr>
                        <tr>
                            <td class="feature-title-cell">Human Usability</td>
                            <td>Developer Tooling & Complex APIs Required</td>
                            <td class="sisi-highlight">Plain-English Structured Docs + Local HTML Dashboard</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </section>

    <!-- ROADMAP SECTION -->
    <section id="roadmap">
        <div class="container">
            <div class="section-header">
                <span class="section-tag">05 / Commercial Plan</span>
                <h2 class="section-title">Execution & Commercial Launch</h2>
                <p class="section-desc">A structured 12-month commercial deployment schedule backed by a Month 5 production completion milestone.</p>
            </div>

            <div class="roadmap-grid">
                <div class="roadmap-card">
                    <span class="roadmap-phase">Phase 1 • Months 1–2</span>
                    <h3>ADGM Entity & Alpha</h3>
                    <p>Finalize corporate ADGM establishment in Abu Dhabi; deploy live alpha test of KING Kernel, Green Zone DMZ, and AIFM management interface.</p>
                </div>

                <div class="roadmap-card">
                    <span class="roadmap-phase">Phase 2 • Months 3–4</span>
                    <h3>Formal Logic Prover</h3>
                    <p>Integrate formal logic verification engines into local GPU/VRAM hardware clusters; conduct full security stress testing.</p>
                </div>

                <div class="roadmap-card milestone">
                    <span class="roadmap-phase">Phase 3 • Month 5 ★</span>
                    <h3>Commercial Go-Live</h3>
                    <p><strong>System Completion Milestone:</strong> Launch production-ready Sisi 4.0 OS; initiate direct enterprise subscription and contract sales.</p>
                </div>

                <div class="roadmap-card">
                    <span class="roadmap-phase">Phase 4 • Months 6–12</span>
                    <h3>GCC Commercial Scale</h3>
                    <p>Scale commercial licensing, annual subscription contracts, and sovereign deployment grants across GCC government and banking sectors.</p>
                </div>
            </div>

            <div class="cta-banner">
                <h2>Ready for Sovereign AI Deployment?</h2>
                <p>Join leading regional defense contractors, central banks, and government ministries evaluating Sisi 4.0 for Month 5 commercial deployment.</p>
                <a href="#contact" class="btn">Inquire for Pilot Admission →</a>
            </div>
        </div>
    </section>

    <!-- FOOTER / CONTACT SECTION -->
    <footer id="contact">
        <div class="container">
            <div class="footer-grid">
                <div>
                    <a href="#" class="footer-logo">SISI SYSTEMS</a>
                    <p class="footer-text">The world’s premier Sovereign, Zero-Hallucination Air-Gapped AI Operating System engineered for national security, central banks, and regulated infrastructure.</p>
                </div>

                <div class="footer-col">
                    <h4>Navigation</h4>
                    <ul>
                        <li><a href="#problem">Market Vulnerability</a></li>
                        <li><a href="#solution">Sisi 4.0 Solution</a></li>
                        <li><a href="#architecture">Core Subsystems</a></li>
                        <li><a href="#differentiators">Competitive Moat</a></li>
                        <li><a href="#roadmap">Commercial Plan</a></li>
                    </ul>
                </div>

                <div class="footer-col">
                    <h4>Jurisdiction</h4>
                    <p style="font-size: 14px; color: var(--text-muted); line-height: 1.6;">
                        Abu Dhabi Global Market (ADGM)<br>
                        Abu Dhabi, United Arab Emirates<br>
                        Hub71+ Access Program Target
                    </p>
                </div>

                <div class="footer-col">
                    <h4>Inquiries</h4>
                    <p style="font-size: 14px; color: var(--text-muted); line-height: 1.6;">
                        <strong>Email:</strong> c.tayyabahmad1072@gmail.com<br>
                        <strong>Go-Live:</strong>April 2027<br>
                        <strong>Sales Engine:</strong> Enterprise Contracts
                    </p>
                </div>
            </div>

            <div class="footer-bottom">
                <span>&copy; 2026 Sisi Systems. All rights reserved. Confidential Documentation.</span>
                <span>Hub71+ Access Program Applicant</span>
            </div>
        </div>
    </footer>

</body>
</html>
