# 🟠 30 Days of AI Security

[![License: CC0-1.0](https://img.shields.io/badge/License-CC0--1.0-lightgrey.svg?style=for-the-badge)](LICENSE)
[![PRs: Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen.svg?style=for-the-badge)](CONTRIBUTING.md)
[![Progress](https://img.shields.io/badge/Days-0%2F30-orange?style=for-the-badge)](#-the-30-days)

> A month, not a week and not a quarter. If you can commit ~1–2 hours/day for 30 days, this takes you from "I understand what prompt injection is" to "I've run garak/PyRIT/promptfoo campaigns, built and defended against an indirect-injection exploit, audited an MCP tool-set for the lethal trifecta, scanned a model for supply-chain RCE, and shipped a real capstone."

This sits between the two other repos in the family: faster and more compressed than the [100-day curriculum](LEARNING_PATH_100_DAYS.md) (each day here does the work of roughly three 100-day entries), and considerably deeper than the [7-day taste test](LEARNING_PATH_7_DAYS.md). Every link is a real, verifiable primary source — a paper, a tool's own repo, or a named CVE. Nothing is padded.

---

## The 30 Days

<details>
<summary><b>Days 1–6 — Foundations & prompt injection core</b></summary>

- [ ] **Day 1** — Get the mental model: [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/) + [Attention Is All You Need, Section 3](https://arxiv.org/abs/1706.03762). Note where input validation happens (nowhere — that's the point).
- [ ] **Day 2** — Classical adversarial ML: read [Goodfellow et al., 2014](https://arxiv.org/abs/1412.6572) (FGSM) and build [Lab 1](LABS.md#-lab-1-fast-gradient-sign-method-fgsm-on-pytorch) — drop a classifier from >95% to <10% accuracy with an invisible perturbation.
- [ ] **Day 3** — Read the [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/) in full and write, in your own words, the difference between direct injection, indirect injection, and jailbreaking.
- [ ] **Day 4** — Hands-on: beat [Gandalf](https://gandalf.lakera.ai/) to Level 7, documenting which defense layer you defeated each time. Then try [TensorTrust](https://tensortrust.ai/) in both attacker and defender mode.
- [ ] **Day 5** — Read [Zou et al., 2023](https://arxiv.org/abs/2307.15043) (the GCG suffix attack) and [Wei et al., 2023](https://arxiv.org/abs/2307.02483) (the competing-objectives jailbreak taxonomy).
- [ ] **Day 6** — Read Anthropic's [Many-Shot Jailbreaking](https://www.anthropic.com/research/many-shot-jailbreaking), then run [Lab 2](LABS.md#-lab-2-crafting-direct-prompt-injections--jailbreaks): test Base64/roleplay/cognitive-split obfuscation against a local Llama 3.

</details>

<details>
<summary><b>Days 7–12 — Automated red teaming</b></summary>

- [ ] **Day 7** — Install [garak](https://github.com/NVIDIA/garak), run it against a local model, then write one custom probe.
- [ ] **Day 8** — Install [promptfoo](https://github.com/promptfoo/promptfoo), run its red-team mode, and wire it into a CI pipeline mapped to OWASP LLM Top 10.
- [ ] **Day 9** — Set up Microsoft's [PyRIT](https://github.com/Azure/PyRIT).
- [ ] **Day 10** — Build a PyRIT orchestrator that iteratively rewrites an injection until it breaks a target's system prompt, with an auto-scoring evaluator LLM.
- [ ] **Day 11** — Read [HarmBench](https://github.com/centerforaisafety/HarmBench) and [CyberSecEval](https://github.com/meta-llama/PurpleLlama/tree/main/CyberSecEval); run CyberSecEval locally.
- [ ] **Day 12** — Consolidation day: run garak, promptfoo, and PyRIT against the *same* target model and compare which one surfaces which failure classes.

</details>

<details>
<summary><b>Days 13–18 — Agentic exploitation & MCP security</b></summary>

- [ ] **Day 13** — Read [Greshake et al., 2023](https://arxiv.org/abs/2302.12173) (indirect injection, foundational) and build [Lab 3](LABS.md#-lab-3-indirect-prompt-injection-via-rag--tool-hijacking): a poisoned RAG document that hijacks a tool call.
- [ ] **Day 14** — Install [AgentDojo](https://github.com/ethz-spylab/agentdojo) and read [InjecAgent](https://github.com/uiuc-kang-lab/InjecAgent); compare their injection taxonomies.
- [ ] **Day 15** — Read [AgentHarm](https://huggingface.co/datasets/ai-safety-institute/AgentHarm) and [Agent Security Bench (ASB)](https://github.com/agiresearch/ASB) — note ASB's 84.3% attack success rate against current defenses.
- [ ] **Day 16** — Read ["Your AI, My Shell" (Liu et al., 2025)](https://arxiv.org/abs/2509.22040) and [Maloyan & Namiot, 2026](https://arxiv.org/abs/2601.17548) — agentic coding-assistant injection, empirically measured.
- [ ] **Day 17** — Read the [MCP security best practices](https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices) and run [mcp-scan](https://github.com/invariantlabs-ai/mcp-scan) against a real config.
- [ ] **Day 18** — Read [The Lethal Trifecta](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/), audit your own agent's tool-set against it, and study the sandboxing/RBAC material in [`AGENT_SECURITY.md`](AGENT_SECURITY.md).

</details>

<details>
<summary><b>Days 19–24 — Defense: guardrails, observability, supply chain</b></summary>

- [ ] **Day 19** — Deploy [LLM Guard](https://github.com/protectai/llm-guard) and [NeMo Guardrails](https://github.com/NVIDIA-NeMo/Guardrails).
- [ ] **Day 20** — Build [Lab 5](LABS.md) (input/output guardrail proxy) from scratch, then attack your own proxy and find a bypass.
- [ ] **Day 21** — Survey and deploy one open-weight guard model: [Llama Guard 4](https://huggingface.co/meta-llama/Llama-Guard-4-12B), [Prompt Guard 2](https://huggingface.co/meta-llama/Llama-Prompt-Guard-2-86M), [ShieldGemma 2](https://huggingface.co/google/shieldgemma-2b), [Granite Guardian](https://arxiv.org/abs/2412.07724), or [WildGuard](https://huggingface.co/allenai/wildguard).
- [ ] **Day 22** — Build an observability pipeline: [Langfuse](https://github.com/langfuse/langfuse) or [OpenLLMetry](https://github.com/traceloop/openllmetry) for capture, [Invariant's Analyzer](https://github.com/invariantlabs-ai/invariant) for detection.
- [ ] **Day 23** — Run [ModelScan](https://github.com/protectai/modelscan) and [PickleScan](https://github.com/mmaitre314/picklescan) (`≥0.0.31`) against downloaded models, then build [Lab 4](LABS.md#-lab-4-model-supply-chain-exploitation-via-pickle-malware) (safe pickle-RCE PoC) and convert a checkpoint to [`safetensors`](https://github.com/huggingface/safetensors).
- [ ] **Day 24** — Read [Sleeper Agents (Hubinger et al., 2024)](https://arxiv.org/abs/2401.05566) and [Poisoning Language Models During Instruction Tuning (Wan et al., 2023)](https://arxiv.org/abs/2305.00944).

</details>

<details>
<summary><b>Days 25–30 — Standards, bug bounty & capstone</b></summary>

- [ ] **Day 25** — Read the full [MITRE ATLAS](https://atlas.mitre.org/) tactic matrix and the [OWASP GenAI Red Teaming Guide](https://genai.owasp.org/resource/genai-red-teaming-guide/)'s four-phase method.
- [ ] **Day 26** — Read the [OWASP Top 10 for Agentic Applications 2026](https://genai.owasp.org/2025/12/09/owasp-top-10-for-agentic-applications-the-benchmark-for-agentic-security-in-the-age-of-autonomous-ai/) (ASI01–10) and map three categories to real incidents (EchoLeak, `postmark-mcp`, the Semantic Kernel `eval()` CVE pair).
- [ ] **Day 27** — Read the [NIST AI RMF](https://www.nist.gov/itl/ai-risk-management-framework), [ISO/IEC 42001](https://www.iso.org/standard/81230.html), and the EU AI Act's risk tiers.
- [ ] **Day 28** — Review [Huntr.com](https://huntr.com/)'s open-source AI/ML scope, shortlist a target, and draft a full CVSS-scored vulnerability report template.
- [ ] **Day 29** — Capstone build day: **Option A** — find and responsibly disclose a real vulnerability in an open-source AI library. **Option B** — build and ship an open-source AI security utility.
- [ ] **Day 30** — Publish the capstone with a full writeup (background → mechanism → PoC → mitigation) and a one-page retrospective mapping everything you did across the 30 days to OWASP/MITRE ATLAS. Post it with `#30DaysOfAISecurity`.

</details>

---

## What you'll have after Day 30

- Real reps with garak, PyRIT, promptfoo, mcp-scan, ModelScan, LLM Guard, and at least one guard model and one observability stack.
- Working exploits (indirect RAG injection, pickle RCE) and working defenses (a guardrail proxy) you built yourself.
- A published capstone that's an actual portfolio artifact.

## The family

| Repo | For |
|:---|:---|
| [**7 Days of AI Security**](LEARNING_PATH_7_DAYS.md) | A taste test. One week, zero prerequisites. |
| **30 Days of AI Security** (this repo) | A serious, time-boxed month to real competence. |
| [**100 Days of AI Security**](LEARNING_PATH_100_DAYS.md) | The full curriculum, foundations to capstone. |
| [**AI Security Interview Questions**](INTERVIEW_QUESTIONS.md) | Prepping for an interview now, across 7 AI-security-adjacent roles. |
| [**Prompt Injection & Jailbreak Technique Library**](JAILBREAK_LIBRARY.md) | A categorized taxonomy of attack techniques, for feeding garak/PyRIT/promptfoo. |

All five are built from the same verified source: [`ai-security-resources`](README.md).

## Contributing

Found a dead link or a better primary source? See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[CC0 1.0](LICENSE) — public domain.
