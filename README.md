# Human Experiment Suite: Risk & Decision-Making Tasks

This repository contains a suite of web-based behavioral experiments designed to measure human risk attitudes and contextual decision-making across three distinct domains (Spatial, Financial, and Medical). The suite is configured to provide a perfectly deterministic and standardized experience for remote participants, ensuring high ecological validity and reproducible data collection.

## 🚀 Experimental Workflow

The platform orchestrates four sequential components. To ensure data integrity, the three main tasks are locked until the participant completes a baseline risk preference measurement.

1. **Holt-Laury Baseline Task (`Experiment_HL`)**
   - A mandatory pre-test that measures baseline risk preference using the standard 10-row decision table (Payoffs: \$2.00/\$1.60 vs. \$3.85/\$0.10).
   - Serves as the gatekeeper; subsequent tasks remain inaccessible until completion.

2. **Drone Navigation Task (`Experiment_DSB`)**
   - **Domain:** Spatial / Navigational
   - Participants pilot a drone through procedurally generated obstacle fields under varying wind conditions. 
   - Measures spatial risk-taking (speed vs. collision probability).

3. **Financial Investment Task (`Experiment_FIP`)**
   - **Domain:** Financial
   - Participants observe simulated market indices and allocate a portfolio between Low, Medium, and High-Risk assets.
   - Measures financial risk sensitivity and volatility assessment.

4. **Emergency Triage Task (`Experiment_TPB`)**
   - **Domain:** Medical / Clinical
   - Participants review evolving patient profiles and assign an Emergency Severity Index (ESI) triage level.
   - Measures clinical risk assessment under time pressure and noisy vital signs.

---

## 🔒 Deterministic Scenarios & Standardization

To guarantee that every participant faces the exact same series of challenges, all procedural randomization (`Math.random()`) within the trial loops has been overridden with a deterministic `mulberry32` PRNG engine. 

- **Fixed Length:** Each of the three main experiments (DSB, FIP, TPB) is strictly locked to **exactly 3 trials**.
- **Fixed Difficulty Tiers:** The trials are hardcoded to represent three escalating difficulty conditions (e.g., Easy, Medium, Hard).
- **Data Yield:** A complete session yields exactly **9 task data points** per participant, plus the Holt-Laury baseline results.

---

## 📂 Project Structure

\`\`\`text
HumanExperimentConfig/
│
├── index.html                 # Main Menu Orchestrator & Webhook payload builder
├── README.md                  # Project documentation (this file)
│
├── assets/                    # Shared resources
│   ├── subject-id.js          # Handles URL parameter ID extraction
│   ├── tour.js / tour.css     # Interactive UI tutorial system
│   └── stage-hint.js          # Shared UI hints
│
├── Experiment_HL/             # Holt-Laury Baseline Task
│   └── index.html
│
├── Experiment_DSB/            # Drone Navigation Task
│   ├── index.html
│   ├── css/                   # DSB specific styling
│   ├── assets/                # Drone sprites and map icons
│   └── js/
│       ├── driver_layers_v6.js # Core game loop and PRNG overrides
│       └── config.json         # Trial block configurations
│
├── Experiment_FIP/            # Financial Investment Task
│   ├── index.html
│   └── js/
│       └── app_v111.js         # Market simulation and portfolio logic
│
└── Experiment_TPB/            # Emergency Triage Task
    ├── index.html
    └── js/
        └── driver_tpb_esi_v2_3.js # Patient profile and vitals generation
\`\`\`

---

## 💾 Data Collection & Webhooks

The platform utilizes standard HTML5 `localStorage` to persist data between task pages. 

When all four components are completed, the participant returns to the main `index.html` menu to submit their data. The orchestrator script extracts the arrays from local storage (`experiment_data_hl`, `experiment_data_dsb`, `experiment_data_fip`, `experiment_data_tpb`), concatenates them into a unified JSON payload, and POSTs them to a configured external Webhook (e.g., Google Apps Script) for safe remote storage.

## 🛠️ Usage / Local Testing

To run the experiments locally:
1. Clone the repository.
2. Serve the directory using a local web server to avoid CORS issues with module imports or local storage.
   \`\`\`bash
   python -m http.server 8000
   \`\`\`
3. Navigate to \`http://localhost:8000/index.html\` in your browser.
4. *Optional*: Append `?PROLIFIC_PID=test_user` to the URL to simulate a remote participant ID.
