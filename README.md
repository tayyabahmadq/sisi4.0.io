<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sisi 4.0 — Hub71+ Pitch Deck | Tayyab Ahmed</title>
    <style>
        :root {
            --bg-color: #0a0a0c;
            --panel-bg: #141418;
            --card-border: #26262e;
            --accent-red: #ff2e4c;
            --text-primary: #ffffff;
            --text-secondary: #d0d0d5;
            --text-muted: #8e8e99;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-primary);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            padding: 20px;
        }

        .deck-container {
            width: 100%;
            max-width: 1000px;
            aspect-ratio: 16 / 9;
            background-color: var(--panel-bg);
            border: 1px solid var(--card-border);
            border-radius: 12px;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.8);
            position: relative;
            overflow: hidden;
            display: flex;
            flex-direction: column;
        }

        .slide {
            display: none;
            flex-direction: column;
            justify-content: space-between;
            height: 100%;
            padding: 40px;
            opacity: 0;
            transition: opacity 0.3s ease-in-out;
        }

        .slide.active {
            display: flex;
            opacity: 1;
        }

        .slide-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 2px solid var(--accent-red);
            padding-bottom: 12px;
            margin-bottom: 20px;
        }

        .slide-title {
            font-size: 24px;
            font-weight: 700;
            letter-spacing: -0.5px;
            color: var(--text-primary);
        }

        .badge {
            background-color: var(--accent-red);
            color: #fff;
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .slide-content {
            flex-grow: 1;
            display: flex;
            flex-direction: column;
            justify-content: center;
            gap: 16px;
        }

        .grid-2 {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }

        .grid-3 {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 16px;
        }

        .grid-4 {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr 1fr;
            gap: 12px;
        }

        .card {
            background: #1c1c22;
            border: 1px solid var(--card-border);
            border-radius: 8px;
            padding: 16px;
            transition: border-color 0.2s ease;
        }

        .card:hover {
            border-color: var(--accent-red);
        }

        .card-title {
            color: var(--accent-red);
            font-size: 14px;
            font-weight: 700;
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .card-text {
            color: var(--text-secondary);
            font-size: 12px;
            line-height: 1.5;
        }

        .big-number {
            font-size: 32px;
            font-weight: 800;
            color: var(--text-primary);
            margin-bottom: 4px;
        }

        .slide-footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-top: 1px solid var(--card-border);
            padding-top: 12px;
            margin-top: 16px;
            font-size: 11px;
            color: var(--text-muted);
        }

        .controls {
            display: flex;
            gap: 12px;
            margin-top: 20px;
            align-items: center;
        }

        .btn {
            background-color: var(--panel-bg);
            color: var(--text-primary);
            border: 1px solid var(--card-border);
            padding: 10px 20px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 13px;
            font-weight: 600;
            transition: all 0.2s ease;
        }

        .btn:hover {
            border-color: var(--accent-red);
            color: var(--accent-red);
        }

        .btn-primary {
            background-color: var(--accent-red);
            border-color: var(--accent-red);
            color: #ffffff;
        }

        .btn-primary:hover {
            background-color: #d61f38;
            color: #ffffff;
        }

        .slide-indicator {
            font-size: 13px;
            color: var(--text-muted);
            min-width: 80px;
            text-align: center;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 12px;
        }

        th, td {
            padding: 10px 12px;
            text-align: left;
            border-bottom: 1px solid var(--card-border);
        }

        th {
            color: var(--accent-red);
            font-weight: 700;
            background-color: #1a1a20;
        }

        td {
            color: var(--text-secondary);
        }
    </style>
</head>
<body>

    <div class="deck-container">
        <!-- Slide 1 -->
        <div class="slide active">
            <div class="slide-header">
                <div class="slide-title">SISI 4.0</div>
                <div class="badge">Hub71+ Pitch Deck</div>
            </div>
            <div class="slide-content" style="text-align: center; justify-content: center; align-items: center;">
                <h1 style="font-size: 38px; color: var(--text-primary); margin-bottom: 8px;">Sovereign Air-Gapped AI OS</h1>
                <p style="font-size: 16px; color: var(--accent-red); max-width: 700px; font-weight: 600; margin-bottom: 12px;">
                    Deterministic, Zero-Hallucination Intelligence for Enterprise & Government Infrastructure
                </p>
                <p style="font-size: 14px; color: var(--text-secondary); margin-bottom: 24px; font-weight: 500;">
                    Founded & Engineered by <strong style="color: #ffffff;">Tayyab Ahmed</strong>
                </p>
                <div class="grid-3" style="width: 100%; max-width: 800px;">
                    <div class="card">
                        <div class="card-title">100% Air-Gapped</div>
                        <div class="card-text">Zero cloud dependencies or telemetry leaks.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">Deterministic Proofs</div>
                        <div class="card-text">Lean 4 & Z3 formal mathematical verification.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">Edge Ready</div>
                        <div class="card-text">Runs fully on &lt;30GB NVMe & 16GB local RAM.</div>
                    </div>
                </div>
            </div>
            <div class="slide-footer">
                <span>Founder: Tayyab Ahmed — Sisi 4.0 Systems</span>
                <span>Slide 1 of 10</span>
            </div>
        </div>

        <!-- Slide 2 -->
        <div class="slide">
            <div class="slide-header">
                <div class="slide-title">The Problem</div>
                <div class="badge">Enterprise Cloud Risks</div>
            </div>
            <div class="slide-content">
                <div class="grid-3">
                    <div class="card">
                        <div class="card-title">1. Data Leakage</div>
                        <div class="card-text">Sending sensitive government or banking data to third-party cloud APIs violates strict sovereignty regulations.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">2. AI Hallucinations</div>
                        <div class="card-text">Probabilistic LLM token generation carries severe legal and financial risks in mission-critical operations.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">3. Massive CapEx</div>
                        <div class="card-text">Local enterprise RAG requires millions in multi-GPU server clusters ($30,000+ NVIDIA H100s).</div>
                    </div>
                </div>
            </div>
            <div class="slide-footer">
                <span>Founder: Tayyab Ahmed — Sisi 4.0 Systems</span>
                <span>Slide 2 of 10</span>
            </div>
        </div>

        <!-- Slide 3 -->
        <div class="slide">
            <div class="slide-header">
                <div class="slide-title">The Solution</div>
                <div class="badge">Sovereign Architecture</div>
            </div>
            <div class="slide-content">
                <div class="grid-4">
                    <div class="card">
                        <div class="card-title">1. Bare-Metal Shield</div>
                        <div class="card-text">Microsecond C watchdog monitoring syscalls and memory threads.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">2. Skill Cover Router</div>
                        <div class="card-text">Sub-2ms $O(1)$ zero-vector search over local .okf.md nodes.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">3. Swarm + AREE Logic</div>
                        <div class="card-text">300 micro-workers backed by Lean 4 formal logic proof verification.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">4. Output Engine</div>
                        <div class="card-text">Delivers guaranteed factual, verified, and secure results.</div>
                    </div>
                </div>
            </div>
            <div class="slide-footer">
                <span>Founder: Tayyab Ahmed — Sisi 4.0 Systems</span>
                <span>Slide 3 of 10</span>
            </div>
        </div>

        <!-- Slide 4 -->
        <div class="slide">
            <div class="slide-header">
                <div class="slide-title">Proprietary Core Layers</div>
                <div class="badge">Deep-Tech Stack</div>
            </div>
            <div class="slide-content">
                <div class="grid-2">
                    <div class="card">
                        <div class="card-title">The Hands (C Watchdog)</div>
                        <div class="card-text">Bare-metal security kernel terminating compromised threads in &lt;1µs before execution memory leaks occur.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">Skill Cover Router</div>
                        <div class="card-text">Inverted B-Tree indexes over Wikitext nodes replacing high-RAM vector DBs with sub-2ms query speeds.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">AREE Symbolic Engine</div>
                        <div class="card-text">Lean 4 & Z3 SMT solvers converting textual claims into mathematical logic proofs for 0% hallucination.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">M.ARIS 2 Delta Pipeline</div>
                        <div class="card-text">Real-time local content diffing engine keeping internal knowledge bases synchronized automatically.</div>
                    </div>
                </div>
            </div>
            <div class="slide-footer">
                <span>Founder: Tayyab Ahmed — Sisi 4.0 Systems</span>
                <span>Slide 4 of 10</span>
            </div>
        </div>

        <!-- Slide 5 -->
        <div class="slide">
            <div class="slide-header">
                <div class="slide-title">Target Market Verticals</div>
                <div class="badge">High-Value Monetization</div>
            </div>
            <div class="slide-content">
                <div class="grid-3">
                    <div class="card">
                        <div class="card-title">Government & Defense</div>
                        <div class="card-text">Air-gapped intelligence for classified document processing, policy automation, and OT network security.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">ADGM Financial & Banking</div>
                        <div class="card-text">Deterministic compliance audit trails, credit risk verification, and automated transaction checks.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">Critical Infrastructure</div>
                        <div class="card-text">Sovereign OT monitoring across energy, telecom, and healthcare data networks without external connectivity.</div>
                    </div>
                </div>
            </div>
            <div class="slide-footer">
                <span>Founder: Tayyab Ahmed — Sisi 4.0 Systems</span>
                <span>Slide 5 of 10</span>
            </div>
        </div>

        <!-- Slide 6 -->
        <div class="slide">
            <div class="slide-header">
                <div class="slide-title">Business & Monetization</div>
                <div class="badge">Revenue Model</div>
            </div>
            <div class="slide-content">
                <div class="grid-3">
                    <div class="card">
                        <div class="card-title">Annual Licensing</div>
                        <div class="big-number">$150k - $500k</div>
                        <div class="card-text">Tiered annual sovereign license per enterprise/government node deployment.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">Air-Gapped Setup</div>
                        <div class="big-number">$50k - $150k</div>
                        <div class="card-text">Custom .okf.md knowledge base compilation and local hardware integration.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">Hardware ROI</div>
                        <div class="big-number">80%+ Savings</div>
                        <div class="card-text">Drastically slashes client server CapEx compared to standard GPU clusters.</div>
                    </div>
                </div>
            </div>
            <div class="slide-footer">
                <span>Founder: Tayyab Ahmed — Sisi 4.0 Systems</span>
                <span>Slide 6 of 10</span>
            </div>
        </div>

        <!-- Slide 7 -->
        <div class="slide">
            <div class="slide-header">
                <div class="slide-title">Competitive Matrix</div>
                <div class="badge">Market Positioning</div>
            </div>
            <div class="slide-content">
                <table>
                    <thead>
                        <tr>
                            <th>Feature</th>
                            <th>Cloud LLMs (GPT-4o)</th>
                            <th>Falcon AI Family</th>
                            <th>Sisi 4.0 Stack</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td><strong>Air-Gap Security</strong></td>
                            <td>❌ Cloud API Dependent</td>
                            <td>⚠️ High Setup Overhead</td>
                            <td>🏆 <strong>Native Air-Gapped</strong></td>
                        </tr>
                        <tr>
                            <td><strong>Fact Verification</strong></td>
                            <td>❌ Probabilistic</td>
                            <td>❌ Probabilistic</td>
                            <td>🏆 <strong>Deterministic Proofs</strong></td>
                        </tr>
                        <tr>
                            <td><strong>Hardware Cost</strong></td>
                            <td>❌ High Token Fees</td>
                            <td>❌ Multi-GPU Clusters</td>
                            <td>🏆 <strong>Edge GPU (&lt;30GB NVMe)</strong></td>
                        </tr>
                        <tr>
                            <td><strong>Threat Response</strong></td>
                            <td>❌ Software Prompts</td>
                            <td>❌ Standard OS Layer</td>
                            <td>🏆 <strong>&lt;1µs Bare-Metal C Shield</strong></td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <div class="slide-footer">
                <span>Founder: Tayyab Ahmed — Sisi 4.0 Systems</span>
                <span>Slide 7 of 10</span>
            </div>
        </div>

        <!-- Slide 8 -->
        <div class="slide">
            <div class="slide-header">
                <div class="slide-title">Hardware & Compute Footprint</div>
                <div class="badge">Ultra-Low Overhead</div>
            </div>
            <div class="slide-content">
                <div class="grid-4">
                    <div class="card">
                        <div class="card-title">NVMe Storage</div>
                        <div class="big-number">&lt;30 GB</div>
                        <div class="card-text">Total on-premise installation footprint.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">System RAM</div>
                        <div class="big-number">16 GB</div>
                        <div class="card-text">Standard workstation RAM allocation.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">GPU Acceleration</div>
                        <div class="big-number">4GB - 8GB</div>
                        <div class="card-text">Runs on basic consumer edge GPUs.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">Search Latency</div>
                        <div class="big-number">&lt;2 ms</div>
                        <div class="card-text">Instant document lookup via B-Trees.</div>
                    </div>
                </div>
            </div>
            <div class="slide-footer">
                <span>Founder: Tayyab Ahmed — Sisi 4.0 Systems</span>
                <span>Slide 8 of 10</span>
            </div>
        </div>

        <!-- Slide 9 -->
        <div class="slide">
            <div class="slide-header">
                <div class="slide-title">Strategic Fit with Hub71+</div>
                <div class="badge">Abu Dhabi Ecosystem</div>
            </div>
            <div class="slide-content">
                <div class="grid-3">
                    <div class="card">
                        <div class="card-title">UAE AI Strategy Alignment</div>
                        <div class="card-text">Directly reinforces Abu Dhabi's ambition to command sovereign deep-tech and IP ownership.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">ADGM Sandbox & Pilot Fit</div>
                        <div class="card-text">Ideal for financial compliance, banking audit sandboxes, and public sector pilot trials.</div>
                    </div>
                    <div class="card">
                        <div class="card-title">UAE-India CEPA Corridor</div>
                        <div class="card-text">Leveraging structured startup corridors to expand engineering capabilities across GCC markets.</div>
                    </div>
                </div>
            </div>
            <div class="slide-footer">
                <span>Founder: Tayyab Ahmed — Sisi 4.0 Systems</span>
                <span>Slide 9 of 10</span>
            </div>
        </div>

        <!-- Slide 10 -->
        <div class="slide">
            <div class="slide-header">
                <div class="slide-title">12-Month Expansion Roadmap</div>
                <div class="badge">Execution Strategy</div>
            </div>
            <div class="slide-content">
                <div class="grid-2">
                    <div class="card">
                        <div class="card-title">Phase 1: Months 1 – 4 (Technical & Pilots)</div>
                        <div class="card-text">
                            • <strong>M1:</strong> Abu Dhabi ADGM entity setup & audit readiness.<br>
                            • <strong>M2:</strong> Deploy air-gapped POC testbeds (Banks & Gov).<br>
                            • <strong>M3:</strong> ADGM financial compliance & air-gap trials.<br>
                            • <strong>M4:</strong> Lock multi-year commercial node contracts.
                        </div>
                    </div>
                    <div class="card">
                        <div class="card-title">Phase 2: Months 5 – 12 (Networking & Scaling)</div>
                        <div class="card-text">
                            • <strong>M5-6:</strong> Sovereign wealth channel integration (G42/MGX).<br>
                            • <strong>M7-8:</strong> GCC expansion (Saudi Arabia, Qatar, Kuwait).<br>
                            • <strong>M9-10:</strong> IT integrator alliances & defense partnerships.<br>
                            • <strong>M11-12:</strong> MENA regional scaling via CEPA corridor.
                        </div>
                    </div>
                </div>
            </div>
            <div class="slide-footer">
                <span>Founder: Tayyab Ahmed — Sisi 4.0 Systems</span>
                <span>Slide 10 of 10</span>
            </div>
        </div>
    </div>

    <!-- Controls -->
    <div class="controls">
        <button class="btn" id="prevBtn" onclick="changeSlide(-1)">Previous</button>
        <div class="slide-indicator" id="slideIndicator">Slide 1 / 10</div>
        <button class="btn" id="nextBtn" onclick="changeSlide(1)">Next</button>
        <button class="btn btn-primary" onclick="window.print()">Save as PDF</button>
    </div>

    <script>
        let currentSlide = 0;
        const slides = document.querySelectorAll('.slide');
        const indicator = document.getElementById('slideIndicator');

        function showSlide(index) {
            slides.forEach((slide, i) => {
                slide.classList.remove('active');
                if (i === index) slide.classList.add('active');
            });
            indicator.textContent = `Slide ${index + 1} / ${slides.length}`;
        }

        function changeSlide(direction) {
            currentSlide += direction;
            if (currentSlide < 0) currentSlide = 0;
            if (currentSlide >= slides.length) currentSlide = slides.length - 1;
            showSlide(currentSlide);
        }

        document.addEventListener('keydown', (e) => {
            if (e.key === 'ArrowRight' || e.key === 'PageDown') changeSlide(1);
            if (e.key === 'ArrowLeft' || e.key === 'PageUp') changeSlide(-1);
        });
    </script>
</body>
</html>
