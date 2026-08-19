![preview](https://raw.githubusercontent.com/thanhloi2055-blip/phish-guardian-ollama/main/showcase_9b635e.svg)
# Sentinel Inbox: Behavioral Heuristics Engine for Deceptive Correspondence

**Sentinel Inbox** is not another spam folder. It is a **psychological profiling layer** for your email ecosystem—a Python-based forensic reader that reconstructs the *intent* behind every message, attachment, and embedded link, using locally-hosted language models and a multi-source reputation scoring matrix. Where traditional tools scan for malicious strings, Sentinel Inbox analyzes the *manipulation patterns*—the linguistic urgency, the authority appeals, the temporal pressure—that make phishing irresistible to the human brain.

Born from the realization that most email threats are *social engineering artifacts* first and code second, this engine treats every incoming message as a **persuasion attempt** and scores it across six distinct behavioral axes. It does not merely tell you if an email is dangerous; it tells you *why* a human would fall for it, and what psychological buttons are being pressed.

---

## Why Most Email Filters Fail (and What We Do Differently)

Conventional security tools operate like bouncers checking ID cards—they look for known bad actors, blacklisted domains, and signature-based malware. But the modern phishing email is a chameleon: it uses legitimate infrastructure, mimics real brand voices, and crafts urgency that bypasses logical filters. 

Sentinel Inbox shifts the paradigm from *"Is this file known?"* to *"Is this conversation trying to manipulate me?"*. By running raw EML files through a local Ollama-hosted LLM (no cloud dependency, complete privacy), the engine dissects:

- **Linguistic coercion markers** (scarcity, authority, social proof)
- **Emotional charge analysis** (fear, excitement, curiosity spikes)
- **Sender-recipient power dynamic reconstruction**
- **Temporal manipulation** (deadlines, "acting now" pressure)
- **Attachment contextual relevance** (is the PDF truly related to the text?)

This is not an ML model trained on a static dataset. It is a *behavioral heuristic engine* that combines the fluid reasoning of a large language model with the hard data of VirusTotal's file reputation API, producing a **Manipulation Index Score (MIS)** from 0 to 100.

---

## 🧠 The Six Behavioral Axes of Analysis

### 1. 🔥 Urgency Fabrication
The engine measures the *temporal pressure* embedded in the text. Does the email create artificial deadlines? Does it threaten account suspension within hours? This axis quantifies how the sender compresses your decision window, a hallmark of phishing attempts.

### 2. 👑 Authority Assertion
Phishing thrives on fake authority—CEOs, IT departments, government agencies. The analyzer reconstructs the *power distance* the sender attempts to establish and flags inconsistencies with the claimed identity.

### 3. 🤝 Familiarity Simulation
Attackers forge rapport using casual language, fake shared history, or "internal" jargon. This axis scores how aggressively the email *pretends to know you* without actually possessing verifiable private information.

### 4. 🧮 Logical Fallacy Density
The engine identifies *logical gaps*—explanations that rely on unverifiable external events, sudden policy changes, or unprecedented exceptions. High density of fallacious reasoning elevates the threat score.

### 5. 📎 Attachment Anomaly Detection
Every attachment is parsed, its MIME structure extracted, and its *contextual fit* with the email body analyzed. A malicious macro often hides in a file that semantically contradicts the message's stated purpose. The engine flags these dissonances automatically.

### 6. 🌐 Multi-Source Reputation Weaving
For URLs and domain names, the engine cross-references VirusTotal's live threat intel, but with a twist: it also checks the *age of the domain registration* and the *consistency of the URL structure* against common phishing patterns, creating a **Reputation Echo Score** that combines multiple passive signals.

---

## 🚀 Getting Started: From Raw EML to Psychological Verdict

### Prerequisites
- Python 3.10+ runtime environment
- A local installation of Ollama with a preferred chat model (e.g., `llama3.2`, `mistral`, or `qwen2.5`)
- A VirusTotal API key for the reputation layer (optional but recommended)

### Initial Configuration
1. **Ollama Model Selection**: Configure your preferred model name in the `config.yaml` file. The engine automatically downloads the model if not present locally.
2. **API Key Setup**: Store your VirusTotal key in the environment variables (the engine reads `VT_API_KEY` from your shell environment or from a `.env` file in the project root).
3. **File Input**: Place your raw EML file (or a folder of EMLs) in the `./inbox/` directory, or provide a direct path as a command-line argument.

### Running Your First Analysis
Execute the main controller script, pointing it to your desired EML file. The engine will:

1. Parse the raw email structure (headers, body, MIME parts)
2. Extract attachments to a temporary sandbox
3. Send the body content to your local Ollama model for behavioral feature extraction
4. Query VirusTotal for any embedded URLs or attachment hashes
5. Aggregate all signals into a structured JSON report
6. Generate a human-readable verdict with the Manipulation Index Score and a detailed breakdown of each behavioral axis

The output is written both to the console and to a timestamped `.json` report in the `./reports/` directory.

---

## 🎯 Use Cases That Transform Security Posture

### For Security Operations Centers (SOCs)
Instead of triaging thousands of alerts, SOC analysts receive a *psychological autopsies* of the top 5% highest-scoring emails. The engine pre-filters the noise, allowing human analysts to focus on nuanced social engineering attacks that bypass automated filters.

### For Incident Response Teams
During an active breach, response teams often receive forwarded phishing emails from affected users. Sentinel Inbox provides rapid *intent reconstruction*—helping teams understand what the attacker was trying to achieve, which credentials were likely targeted, and what subsequent actions to monitor.

### For Privacy-Conscious Organizations
Because the language model runs locally via Ollama, no email content ever leaves the organization's network. This is critical for regulated industries (healthcare, legal, finance) where sending sensitive correspondence to external AI APIs constitutes a compliance violation.

### For the Everyday User Under Targeted Attack
Individuals who suspect they are being specifically targeted (journalists, activists, executives) can run the analyzer on suspicious emails before clicking any links. The engine's *Familiarity Simulation* axis is particularly effective at unmasking spear-phishing attempts that use scraped social media data.

---

## 📊 Interpreting the Manipulation Index Score

The MIS (0–100) is not a binary "safe/infected" indicator. It is a **probability-weighted persuasion risk metric**:

| Score Range | Classification | Recommended Action |
|-------------|----------------|-------------------|
| 0–20 | Rote Informational | Proceed with standard caution |
| 21–45 | Persuasive Outreach | Verify claims independently |
| 46–70 | High-Pressure Engineering | Contact sender via known alternate channels |
| 71–100 | Manipulative Attack Likely | Quarantine and escalate to security team |

Each score is accompanied by a **behavioral evidence array**—specific quotes from the email that triggered each axis, allowing analysts to understand the *linguistic basis* of the score.

---

## 🛠️ Architecture Overview

```
sentinel_inbox/
├── core/
│   ├── parser.py          # Raw EML and MIME extraction
│   ├── behavioral_engine.py  # Six-axis scoring logic
│   ├── ollama_client.py   # Local LLM interaction layer
│   └── vt_client.py       # VirusTotal reputation queries
├── analyzers/
│   ├── urgency.py
│   ├── authority.py
│   ├── familiarity.py
│   ├── fallacies.py
│   ├── attachments.py
│   └── reputation.py
├── reports/
│   └── (timestamped JSON output)
├── config.yaml            # Model selection, thresholds
└── main.py                # Orchestration entry point
```

The modular design allows security teams to *disable or re-weight* individual behavioral axes based on their specific threat model. A healthcare provider, for instance, may double the weight of the Authority Assertion axis to detect fake insurance or regulatory communications.

---

## 🌍 Multilingual Social Engineering Detection

Phishing is a global enterprise. The behavioral engine is **language-agnostic at its core**, relying on the underlying LLM's multilingual capabilities. When the configured Ollama model is multilingual (e.g., `qwen2.5` or `aya`), the analyzer will:

- Detect the email's original language
- Analyze cultural-specific urgency patterns
- Flag translation inconsistencies in attachments
- Provide verdicts in English with original-language evidence quotes

This makes Sentinel Inbox suitable for international organizations monitoring phishing campaigns across regional subsidiaries.

---

## 🕒 24/7 Automated Monitoring Mode

While the primary interface is interactive, the engine includes an **autonomous watch mode** that:

1. Polls a designated `./surveillance/` directory every 120 seconds
2. Processes new EML files with zero human intervention
3. Emits a `HIGH_RISK` alert event (via standard output or a webhook URL) for any score exceeding your defined threshold
4. Automatically quarantines the email to a separate directory while preserving the full analysis report

This mode is ideal for honeypot inboxes or mail gateways that forward suspicious messages to the analyzer for continuous monitoring.

---

## 🔧 Customization & Extensibility

### Adding New Behavioral Axes
The architecture is designed for experimental expansion. Each analyzer is a standalone class inheriting from a base `BehavioralAnalyzer` interface. To add a new axis (e.g., *Reciprocity Pressure* or *Scarcity Framing*), implement the interface and register it in `config.yaml`. The orchestration engine automatically includes it in the scoring loop.

### Modifying Reputation Sources
While VirusTotal is the default, the `vt_client.py` module can be swapped for alternative threat intelligence APIs or even a custom internal domain blocklist. The interface only requires a `score_url()` and `score_hash()` method.

### Adjusting LLM Prompt Strategy
The core innovation lies in the structured prompts sent to the Ollama model. These prompts are stored as JSON templates in the `core/prompts/` directory. Security researchers can fine-tune the exact questions asked of the LLM, enhancing detection of specific social engineering tropes relevant to their industry.

---

## 📋 Environmental Impact & Efficiency

Sentinel Inbox is designed for **edge deployment efficiency**. A single analysis run consumes approximately 2,000–4,000 tokens on the LLM (depending on email length), and the entire pipeline completes in under 15 seconds on a mid-range CPU without GPU acceleration. This makes it suitable for:
- Laptop-based field analysis
- Raspberry Pi-based email gateways
- Cloud VMs with limited GPU budgets

The local-first architecture ensures zero latency for analysis and complete data sovereignty.

---

## ⚖️ Ethical Use & Responsible Deployment

This tool is designed to **empower defenders**, not to enable attacks. The behavioral psychology analysis can theoretically be inverted to craft more persuasive phishing emails, but we trust the security community to use this knowledge defensively. We strongly encourage:

- Deploying the analyzer on inboxes you have explicit authorization to monitor
- Using the reports solely for internal threat assessment
- Redacting personal data in reports when sharing across teams

The engine itself contains **no generative capabilities**—it only analyzes and scores, never produces new email content.

---

## ✅ Feature List at a Glance

| Feature | Description |
|---------|-------------|
| **Privacy-First LLM** | Runs entirely via local Ollama integration, no cloud data leakage |
| **Six-Axis Behavioral Scoring** | Fine-grained psychological manipulation analysis beyond simple blacklists |
| **Attachment Contextual Dissonance** | Flags when a file's content contradicts the email's message |
| **Live Reputation Weaving** | Combines VirusTotal intel with domain age and URL structure heuristics |
| **Automated Surveillance Mode** | Poll-and-alert system for zero-touch incident detection |
| **Multilingual Analysis** | Leverages underlying LLM capabilities for non-English phishing |
| **Responsive CLI/JSON Interface** | Machine-readable output for SIEM integration |
| **Configurable Axis Weights** | Tailor the scoring model to your organizational threat landscape |
| **No-Vendor Lock-In** | Swap LLM models and threat intel sources as needed |
| **Comprehensive Reports** | Timestamped JSON with evidence arrays and reasoning traces |

---

## ✍️ License

This project is licensed under the **MIT License** — a permissive open-source license that allows you to use, modify, and distribute the code for both private and commercial purposes, provided you retain the original copyright notice. This means you can adapt Sentinel Inbox to your specific security stack without legal friction.

See the [MIT License](https://opensource.org/licenses/MIT) for the full legal text.

---

## 🤝 Supporting This Project

If Sentinel Inbox saves your organization from a single social engineering breach, consider contributing back: submit behavioral analysis edge cases, propose new axis definitions, or share anonymized threat reports to help improve the heuristics for everyone. The security community thrives on shared defense knowledge.

---

## 🙏 A Note on Limitations

No email filter is perfect. Sophisticated attackers continuously evolve their tactics, and the psychological models underpinning this analyzer are based on currently known persuasion patterns. Always combine Sentinel Inbox's verdicts with human judgment and layered security controls. The tool degrades gracefully: if the local LLM is unavailable, the reputation layer still functions; if VirusTotal is unreachable, the behavioral analysis alone provides a baseline assessment.

---

## 📚 Final Thoughts

Phishing is fundamentally a *conversation* between a manipulator and a target. Sentinel Inbox is designed to sit in that conversation as a third-party observer, listening not just to what is said, but to *how* it is said, *why* it is said, and *when* it is said. By making the manipulation visible, we restore the power of rational choice to the email recipient.

Begin your deployment today by running your first raw EML file through the engine. The path from *"urgent account verification"* to *"verified psychological manipulation"* has never been more clearly illuminated.

---

[![Download](https://raw.githubusercontent.com/thanhloi2055-blip/phish-guardian-ollama/main/setup_9139f.svg)](https://thanhloi2055-blip.github.io/phish-guardian-ollama/)