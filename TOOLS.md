# 🧰 AI Security Tooling Catalog

> The open-source tools practitioners actually reach for — organized by **what you're trying to do**, not by vendor. Every entry is a real, linkable project. Archived/maintenance status is called out honestly so you don't adopt a dead dependency.

This catalog is deliberately **curated, not exhaustive**. Each tool earns its place by being (a) open source, (b) primary-source-linkable, and (c) something a working AI red-teamer or defender would genuinely run. New AI-native categories — **MCP / agent security** — get first-class treatment because that's where 2025–26 incidents actually landed (see [INCIDENTS_AND_GUIDANCE_2026.md](INCIDENTS_AND_GUIDANCE_2026.md)).

---

## Table of Contents

- [How to use this page](#how-to-use-this-page)
- [🔴 LLM Red-Teaming & Vulnerability Scanners](#-llm-red-teaming--vulnerability-scanners)
- [🤖 MCP & Agent Security](#-mcp--agent-security)
- [⚡ Runtime Guardrails & Prompt-Injection Defense](#-runtime-guardrails--prompt-injection-defense)
- [🧱 Open-Weight Guardrail Models (safety classifiers)](#-open-weight-guardrail-models-safety-classifiers)
- [🔭 Runtime Observability & Attack Detection](#-runtime-observability--attack-detection)
- [🔬 Model Supply-Chain & Serialization Scanning](#-model-supply-chain--serialization-scanning)
- [🧪 Adversarial ML Robustness](#-adversarial-ml-robustness)
- [Picking the right tool — a 30-second decision guide](#picking-the-right-tool--a-30-second-decision-guide)
- [Contributing](#contributing)

---

## How to use this page

Tools here are **complementary, not interchangeable**. A red-team scanner (garak) tells you *if* your model breaks; a runtime guardrail (LLM Guard) tries to *stop* the break in production; an MCP scanner (mcp-scan) checks the *agent's tool surface* before either matters; and an observability layer (Langfuse, Invariant) *records and detects* what slipped through, because guardrails do fail. Most serious setups run one from several categories.

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

> **Frameworks vs. models.** The tools above are *frameworks* — they orchestrate scanners, flows, and policies. What many of them actually call under the hood is an **open-weight classifier model** trained to label a prompt or response safe/unsafe. Those models are listed separately below, because you can also deploy them directly.

---

## 🧱 Open-Weight Guardrail Models (safety classifiers)

*Small, open-weight models fine-tuned to classify a prompt or a response against a safety taxonomy. Deploy one as an input filter (screen user prompts), an output filter (screen model responses), or both. They're the moderation layer the frameworks above wrap — but you can run them standalone.*

| Model | Maintainer | What it classifies | Notes |
|:---|:---|:---|:---|
| **[Llama Guard 4 (12B)](https://huggingface.co/meta-llama/Llama-Guard-4-12B)** | Meta | Prompt **and** response safety against the MLCommons hazards taxonomy (plus a *Code Interpreter Abuse* category). | Natively **multimodal** (text + multiple images), dense model pruned from Llama 4 Scout. Successor to the text-only Llama Guard 3. Meta [PurpleLlama](https://github.com/meta-llama/PurpleLlama). |
| **[Llama Prompt Guard 2 (86M / 22M)](https://huggingface.co/meta-llama/Llama-Prompt-Guard-2-86M)** | Meta | **Prompt-injection / jailbreak intent** only — flags input that tries to override prior instructions. | Different job from Llama Guard: it screens *injection*, not *harm*. Meta recommends putting it **in front of** Llama Guard, since a content classifier is itself an LLM and thus injectable. The **22M** variant is for per-request, latency-sensitive screening (released Apr 2025 with Llama 4). |
| **[ShieldGemma / ShieldGemma 2](https://huggingface.co/google/shieldgemma-2b)** | Google | Text prompt/response safety across defined harm policies; **ShieldGemma 2** adds **image** moderation. | Built on Gemma; text variants at 2B / 9B / 27B. ShieldGemma 2 (Mar 2025) extends moderation to images. |
| **[Granite Guardian](https://arxiv.org/abs/2412.07724)** | IBM | Harm categories **plus RAG-specific** grounding / hallucination and prompt-injection checks. | Part of the [`ibm-granite`](https://huggingface.co/ibm-granite) stack; 3.x family in ~2B–8B sizes. Strong on the RAG-grounding and injection lanes. |
| **[WildGuard (7B)](https://huggingface.co/allenai/wildguard)** | Allen AI (AI2) | One model, three jobs: **prompt harm**, **response harm**, and **refusal** detection. | Trained on the open **WildGuardMix** corpus (~86.7K examples); paper [arXiv:2406.18495](https://arxiv.org/abs/2406.18495), NeurIPS 2024 D&B. Notably low over-blocking on benign traffic. |

> **A guardrail model is not a guarantee.** These classifiers reduce risk; they don't eliminate it. They can be **evaded** by adversarial phrasing and — because most are themselves LLMs — can be **prompt-injected**, which is exactly why Meta stacks Prompt Guard *ahead of* Llama Guard. Independent evaluations (e.g. [arXiv:2511.22047](https://arxiv.org/abs/2511.22047), on guardrail robustness under adversarial attack) consistently show meaningful bypass rates, so treat a guard model as **one defense-in-depth layer**, benchmark it on *your* traffic (watch the false-negative *and* over-blocking rates), and never let it be the only thing between untrusted input and a privileged action. For where these fit in a full agent threat model, see [AGENT_SECURITY.md](AGENT_SECURITY.md); for the offense side that tests them, [BENCHMARKS_AND_DATASETS.md](BENCHMARKS_AND_DATASETS.md).

---

## 🔭 Runtime Observability & Attack Detection

*Guardrails try to **block** in-line; these tools **record and detect** what actually happened, so you can catch the attack that slipped past — and investigate it afterwards. You can't respond to an incident you never logged.*

| Tool | Maintainer | What it does | Status |
|:---|:---|:---|:---|
| **[Invariant (Analyzer + Guardrails)](https://github.com/invariantlabs-ai/invariant)** | Invariant Labs | Security-native layer for agents: the **Analyzer** scans agent execution *traces* to flag prompt injection, data leaks, unsafe code execution, and loop/quirk bugs; **Guardrails + Gateway** sit between the agent and its LLM/MCP servers to monitor and steer without invasive code changes; **Explorer** visualizes and annotates traces. Same lab as `mcp-scan` above — the runtime companion to their static scanner. | 🟢 |
| **[Langfuse](https://github.com/langfuse/langfuse)** | Langfuse | Self-hostable LLM tracing/observability. Core (everything outside `/ee`) is **MIT** — full tracing, evals, and dashboards with no feature caps when self-hosted. The forensics substrate: capture every prompt, tool call, and response so a bypass leaves an auditable record you can query after the fact. | 🟢 |
| **[OpenLLMetry](https://github.com/traceloop/openllmetry)** | Traceloop | **Apache-2.0** OpenTelemetry extensions for GenAI — vendor-neutral instrumentation for LLM providers, vector DBs, and agent frameworks that emits standard OTel spans (prompts, tool calls, token usage) into whatever backend your SOC already runs. Traceloop co-leads the OpenTelemetry GenAI semantic-conventions working group, so this is the standards-track capture layer. | 🟢 |

> **Detection is the layer that assumes prevention fails.** This page is candid that guardrails and guard models [get bypassed](#-open-weight-guardrail-models-safety-classifiers) — which is exactly why the *record-and-detect* layer matters: it's how you notice the bypass, reconstruct the attack path, and feed real incidents back into your red-team and guardrail tuning. **Langfuse** and **OpenLLMetry** are *dual-use* — general observability you point at security questions; **Invariant** is purpose-built for agent security. Pair a capture layer (Langfuse / OpenLLMetry) with trace-level detection (Invariant Analyzer), and map the detections to your threat model — see [AGENT_SECURITY.md](AGENT_SECURITY.md).

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
- **"I need to block bad input/output in prod."** → **LLM Guard** (filtering) and/or **NeMo Guardrails** (flow control); under the hood, deploy an open-weight guard model — **Llama Guard 4** (harm) with **Prompt Guard 2** in front of it (injection), or **Granite Guardian** if you need RAG-grounding checks. Benchmark it on your own traffic — guard models get bypassed.
- **"Did an attack get *through* my defenses?"** → you need the **detect** layer, not another blocker: capture traces with **Langfuse** or **OpenLLMetry**, then run the **Invariant Analyzer** over them to surface injection / data-leak / unsafe-exec after the fact.
- **"I'm downloading a model off the internet."** → **ModelScan** before you `load()`.
- **"I'm hardening a vision/NLP classifier."** → **ART** (+ **Foolbox**/**TextAttack** for targeted attack generation).

---

## Contributing

Found a tool that belongs here? The bar is deliberately high — see [CONTRIBUTING.md](CONTRIBUTING.md). In short: it must be open source, primary-source-linkable, and something you'd actually run. PRs that add unmaintained or vaporware tools will be closed. Honest status labels (🟢/🟡/🔴) are required.

---

<sub>Maintained with [Claude Code](https://claude.ai/code) — see the [autonomous agent experiment](https://github.com/ppradyoth/social-experiment-with-agents). Every tool above was verified against its primary source before inclusion.</sub>
