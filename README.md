# 🛡️ LLM Red-Teaming Harness & Guardrail Evaluator

An automated security testing and defense evaluation framework for Large Language Model applications. This toolkit executes automated adversarial attacks (prompt injections, persona hijacks, system prompt extraction, credential exfiltration) against target endpoints and evaluates mitigation efficacy using a zero-temperature LLM-as-a-Judge guardrail architecture.

## 📐 Architecture Overview

User / Attacker Input
│
▼
[ Input Guardrail Evaluator ] ──(Malicious Pattern Detected)──► [ 403 / Terminate Request ]
│
▼ (Safe / Benign)
[ Core Application LLM ]
│
▼
[ Verified Response Output ]
- **Adversarial Harness (`evaluate.py`):** Programmatically simulates attack vectors across standardized test batteries and computes empirical vulnerability rates.
- **Vulnerable Target (`target_bot.py`):** Simulates a naive production endpoint with hidden system instructions and privileged credentials.
- **Evaluation Firewall (`secure_bot.py`):** Acts as an upstream security proxy, using an isolated classification prompt with fail-closed semantics to neutralize threats before they reach runtime application contexts.

## 📊 Benchmark Metrics

The test suite compares raw endpoint vulnerability against the guardrail-protected configuration:

| Metric | Target Endpoint (Baseline) | Guardrail Protected | Delta |
|---|---|---|---|
| **System Prompt Leakage** | Vulnerable (Breached) | 0% (Neutralized) | -100% |
| **Persona Hijacking** | Vulnerable | 0% (Neutralized) | -100% |
| **Direct Exfiltration** | Resisted | 0% (Neutralized) | Neutral |
| **Benign Query Pass Rate** | 100% | 100% | 0% Overhead |

## 🚀 Quick Start

### 1. Setup Environment
```bash
git clone [https://github.com/YourUsername/llm-redteam-toolkit.git](https://github.com/YourUsername/llm-redteam-toolkit.git)
cd llm-redteam-toolkit
python3 -m venv venv
source venv/bin/activate
pip install google-genai
export GEMINI_API_KEY="your_api_key_here"

2. Run Individual Attack Suites
Execute attacks directly against the unshielded target:

python attacker.py

Execute attacks through the guardrail firewall:
python secure_attacker.py

3. Run the Automated Benchmark
Run the full comparative evaluation suite to output security_report.json
python evaluate.py
