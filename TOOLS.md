# 🧰 AI Security Tooling Catalog

> The open-source tools practitioners actually reach for — organized by **what you're trying to do**, not by vendor. Every entry is a real, linkable project. Archived/maintenance status is called out honestly so you don't adopt a dead dependency.

This catalog is deliberately **curated, not exhaustive**. Each tool earns its place by being (a) open source, (b) primary-source-linkable, and (c) something a working AI red-teamer or defender would genuinely run. New AI-native categories — **MCP / agent security** — get first-class treatment because that's where 2025–26 incidents actually landed (see [INCIDENTS_AND_GUIDANCE_2026.md](INCIDENTS_AND_GUIDANCE_2026.md)).

---

## Table of Contents

- [How to use this page](#how-to-use-this-page)
- [🔴 LLM Red-Teaming & Vulnerability Scanners](#-llm-red-teaming--vulnerability-scanners)
- [🤖 MCP & Agent Security](#-mcp--agent-security)
- [⚡ Runtime Guardrails & Prompt-Injection Defense](#-runtime-guardrails--prompt-injection-defense)
- [🔬 Model Supply-Chain & Serialization Scanning](#-model-supply-chain--serialization-scanning)
- [🧪 Adversarial ML Robustness](#-adversarial-ml-robustness)
- [Picking the right tool — a 30-second decision guide](#picking-the-right-tool--a-30-second-decision-guide)
- [Contributing](#contributing)

---

## How to use this page

Tools here are **complementary, not interchangeable**. A red-team scanner (garak) tells you *if* your model breaks; a runtime guardrail (LLM Guard) tries to *stop* the break in production; an MCP scanner (mcp-scan) checks the *agent's tool surface* before either matters. Most serious setups run one from several categories.

> **Maturity legend:** 🟢 actively maintained · 🟡 maintained but niche/research · 🔴 archived or read-only (use with eyes open).

---

## 🔴 LLM Red-Teaming & Vulnerability Scanners

*Probe a model/endpoint adversarially to find jailbreaks, leakage, and policy failures before an attacker does.*

| Tool | Maintainer | What it does | Status |
|:---|:---|:---|:---|
| **[garak](https://github.com/NVIDIA/garak)** | NVIDIA | The "nmap for LLMs." Dozens of plugins and thousands of prompts combining static, dynamic, and adaptive probes to make a model behave unexpectedly at inference time. The default starting point for LLM scanning. | 🟢 |
| **[PyRIT](https://github.com/Azure/PyRIT)** | Microsoft (Azure) | Python Risk Identification Tool for generative AI. Extensible framework for orchestrating multi-turn attacks against local, hosted, or remote endpoints. Used by the Microsoft AI Red Team across 100+ GenAI red-team operations. | 🟢 |
| **[promptfoo](https://github.com/promptfoo/promptfoo)** | promptfoo | Test-and-eval harness with a red-team mode that generates adversarial test cases and grades responses — pairs cleanly with CI so regressions in safety behavior fail the build. | 🟢 |

---

## 🤖 MCP & Agent Security

*The newest and fastest-moving category. MCP tool definitions are an injection surface; agent tool-sets compose into risks no single server shows. This is where 2025–26 disclosed incidents concentrated.*

| Tool | Maintainer | What it does | Status |
|:---|:---|:---|:---|
| **[mcp-scan](https://github.com/invariantlabs-ai/mcp-scan)** | Invariant Labs | The de-facto MCP scanner. Auto-discovers MCP configs from Claude Desktop, Cursor, Claude Code, Gemini CLI, and Windsurf, then statically flags tool poisoning, rug-pulls, cross-origin escalation, and prompt injection. Start here for any MCP setup. | 🟢 |
| **[MCP-Scanner](https://arxiv.org/html/2510.23673v1)** | Research (arXiv:2510.23673) | Multi-layer scanner combining keyword detection, semantic analysis, and an LLM judge to cover **all** MCP features — tools, prompts, *and* resources — not just tool metadata. | 🟡 |

> **Gap worth knowing:** the scanners above evaluate servers **individually**. The *lethal trifecta* — private-data access + untrusted-content exposure + external communication ([Simon Willison, 16 Jun 2025](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/)) — often emerges only from the **union** of multiple benign servers. Auditing that composition is still an open niche. (See the companion strategy repo's [Trifecta Composer](https://github.com/ppradyoth/ai-security-strategy) tool concept.) For the threat-modeling background, read [AGENT_SECURITY.md](AGENT_SECURITY.md).

---

## ⚡ Runtime Guardrails & Prompt-Injection Defense

*Sit in the request/response path in production to detect, redact, or block malicious input and unsafe output.*

| Tool | Maintainer | What it does | Status |
|:---|:---|:---|:---|
| **[LLM Guard](https://github.com/protectai/llm-guard)** | Protect AI | Input/output firewall: a battery of input scanners on prompts and output scanners on responses covering prompt injection, PII, toxicity, secrets-in-code, and more. | 🟢 |
| **[NeMo Guardrails](https://github.com/NVIDIA-NeMo/Guardrails)** | NVIDIA | Programmable guardrails (a DSL, *Colang*) for constraining conversational flows, topics, and tool use; integrates LLM-vulnerability scanning into its eval docs. | 🟢 |
| **[Rebuff](https://github.com/protectai/rebuff)** | Protect AI | Prompt-injection detector layering heuristics, an LLM check, a vector DB of known attacks, and canary tokens. Conceptually still a great reference design. | 🔴 archived |

---

## 🔬 Model Supply-Chain & Serialization Scanning

*Models are code. A pickle/`.bin` file can carry arbitrary execution. Scan artifacts before you load them.*

| Tool | Maintainer | What it does | Status |
|:---|:---|:---|:---|
| **[ModelScan](https://github.com/protectai/modelscan)** | Protect AI | Scans model files (Pickle, HDF5, SavedModel, etc.) for unsafe serialization and embedded code before they're deserialized into your process. | 🟢 |
| **[PickleScan](https://github.com/mmaitre314/picklescan)** | Community (used in HF Hub scanning) | Scans pickle files for malicious opcodes/imports; the scanner most of the ecosystem leans on for pickle malware. Keep it **`≥ 0.0.31`** — earlier versions carry the bypass trio below. | 🟢 |
| **[safetensors](https://github.com/huggingface/safetensors)** | Hugging Face | Not a scanner — the *fix*. A serialization format that stores tensors only, so **no code executes on load**. Prefer it over pickle wherever the framework allows. | 🟢 |

> ⚠️ **A scanner is not a green light.** In Dec 2025, three PickleScan bypasses ([CVE-2025-10155 / -10156 / -10157](INCIDENTS_AND_GUIDANCE_2026.md#8-picklescan--the-scanner-you-trust-to-catch-malicious-models-is-itself-bypassable-cve-2025-10155---10156---10157), all CVSS 9.3) let a malicious PyTorch model pass as clean and still execute on load — because a scanner is just another parser, and any gap between how *it* parses a file and how the *loader* does is a bypass. Treat scanners as defense-in-depth with known gaps, keep them patched, and prefer **not executing code on load at all** (safetensors / `weights_only=True`).
>
> For hands-on pickle-RCE exploitation labs, cross-reference [LABS.md](LABS.md); for the supply-chain threat model, [STANDARDS_AND_COMPLIANCE.md](STANDARDS_AND_COMPLIANCE.md).

---

## 🧪 Adversarial ML Robustness

*The "classic" pre-LLM adversarial-ML toolchain — still essential for vision/NLP model robustness work.*

| Tool | Maintainer | What it does | Status |
|:---|:---|:---|:---|
| **[Adversarial Robustness Toolbox (ART)](https://github.com/Trusted-AI/adversarial-robustness-toolbox)** | LF AI (Trusted-AI) | Python library spanning evasion, poisoning, extraction, and inference attacks **and** defenses, with robustness metrics. The reference adversarial-ML toolkit. | 🟢 |
| **[TextAttack](https://github.com/QData/TextAttack)** | QData (UVA) | Framework for adversarial attacks, data augmentation, and adversarial training for NLP models — robustness testing beyond chat-only scanners. | 🟡 |
| **[Foolbox](https://github.com/bethgelab/foolbox)** | Bethge Lab | Fast adversarial-example generation against PyTorch/TensorFlow/JAX models, for benchmarking model robustness. | 🟡 |

---

## Picking the right tool — a 30-second decision guide

- **"Is my LLM jailbreakable?"** → start with **garak**, escalate to **PyRIT** for multi-turn campaigns.
- **"I'm shipping an agent with MCP tools."** → run **mcp-scan** on the config first; think about trifecta composition before you connect a web-fetch server to a private-data server.
- **"I need to block bad input/output in prod."** → **LLM Guard** (filtering) and/or **NeMo Guardrails** (flow control).
- **"I'm downloading a model off the internet."** → **ModelScan** before you `load()`.
- **"I'm hardening a vision/NLP classifier."** → **ART** (+ **Foolbox**/**TextAttack** for targeted attack generation).

---

## Contributing

Found a tool that belongs here? The bar is deliberately high — see [CONTRIBUTING.md](CONTRIBUTING.md). In short: it must be open source, primary-source-linkable, and something you'd actually run. PRs that add unmaintained or vaporware tools will be closed. Honest status labels (🟢/🟡/🔴) are required.

---

<sub>Maintained with [Claude Code](https://claude.ai/code) — see the [autonomous agent experiment](https://github.com/ppradyoth/social-experiment-with-agents). Every tool above was verified against its primary source before inclusion.</sub>
