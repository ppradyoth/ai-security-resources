# 🔴 100 Days of AI Security

[![License: CC0-1.0](https://img.shields.io/badge/License-CC0--1.0-lightgrey.svg?style=for-the-badge)](LICENSE)
[![PRs: Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen.svg?style=for-the-badge)](CONTRIBUTING.md)
[![Progress](https://img.shields.io/badge/Days-0%2F100-red?style=for-the-badge)](#-the-100-days)

> A free, self-paced, 100-day curriculum that takes you from "I've read about prompt injection" to "I've run garak, PyRIT, and promptfoo against real models, built a working RAG-injection exploit, deployed guardrails, scanned a model for pickle RCE, and can map any attack to OWASP LLM Top 10 / OWASP Agentic Top 10 / MITRE ATLAS."

No theory dumps. Every single day links to one real, verifiable resource — a paper, a tool, a CTF, or a disclosed CVE — and ends in something you can point to and say "I did that." Fork it, check off days as you go, and post your progress with `#100DaysOfAISecurity`.

**New to the field?** Start at Day 1. **Already know the basics?** Jump straight to whichever week matches what you're missing — every day stands on its own.

**Want something shorter first?** This is the deep track. See **[7 Days of AI Security](LEARNING_PATH_7_DAYS.md)** (a one-week taste test) or **[30 Days of AI Security](LEARNING_PATH_30_DAYS.md)** (a time-boxed month) — same verified source, less time.

---

## Why this exists

The "100 Days of X" format has a real track record — [`100-Days-Of-ML-Code`](https://github.com/Avik-Jain/100-Days-Of-ML-Code) is one of the most-starred learning repos on GitHub. AI security has never had one. This is it: built from a working curriculum ([`ai-security-resources`](README.md)) that's already cited and cross-referenced against primary sources — not 100 days of filler.

**The rule for every entry in this repo:** if it's a paper, it's linked to arXiv or the publisher. If it's a tool, it's linked to its GitHub repo with an honest maintenance status. If it's a CVE, it's a real CVE number. Nothing here is invented to pad the count to 100.

---

## How to use this

1. Fork the repo.
2. Each day: read/do the linked thing, then check the box in your fork (or just track it in a notebook — the checkboxes are for you, not for us).
3. Skip days that are already easy for you. This is a curriculum, not a prison sentence.
4. Stuck, or found a dead link / outdated CVE reference? Open an issue — see [CONTRIBUTING.md](CONTRIBUTING.md).
5. Day 100 ends with a real capstone (a disclosed finding or a published tool) — that's the artifact you actually put on your résumé.

---

## The 100 Days

<details>
<summary><b>Days 1–10 — Foundations: how transformers and adversarial ML actually work</b></summary>

- [ ] **Day 1** — Get the mental model of an LLM's forward pass. Watch [3Blue1Brown's Neural Networks series](https://www.youtube.com/playlist?list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi) (all 4 chapters).
- [ ] **Day 2** — Read [The Illustrated Transformer (Jay Alammar)](https://jalammar.github.io/illustrated-transformer/) — the best visual walkthrough of attention and encoder/decoder routing.
- [ ] **Day 3** — Read [Attention Is All You Need (Vaswani et al., 2017)](https://arxiv.org/abs/1706.03762), Section 3. Ask yourself: where is input validation handled? (It isn't — that's the whole field.)
- [ ] **Day 4** — Follow [Karpathy's "Neural Networks: Zero to Hero" (the GPT video)](https://www.youtube.com/playlist?list=PLAqhIrjkxbuWI23v9cThsA9GvCAUhRvKZ) and hand-write a self-attention block in PyTorch.
- [ ] **Day 5** — Finish the char-level transformer from Day 4. Train it on Shakespeare text until it generates coherent-ish output.
- [ ] **Day 6** — Read [Intriguing Properties of Neural Networks (Szegedy et al., 2013)](https://arxiv.org/abs/1312.6199) — the paper that discovered adversarial examples exist.
- [ ] **Day 7** — Read [Explaining and Harnessing Adversarial Examples (Goodfellow et al., 2014)](https://arxiv.org/abs/1412.6572) — the FGSM paper.
- [ ] **Day 8** — Implement FGSM from scratch on a pretrained ResNet (full code: [`ai-security-resources/LABS.md#lab-1`](LABS.md#-lab-1-fast-gradient-sign-method-fgsm-on-pytorch)). Drop a classifier's accuracy from >95% to <10% with a human-invisible perturbation.
- [ ] **Day 9** — Read [Towards Evaluating the Robustness of Neural Networks (Carlini & Wagner, 2017)](https://arxiv.org/abs/1608.04644) — why most adversarial defenses don't actually work.
- [ ] **Day 10** — Read the [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/) in full. You'll use this taxonomy every day from here on.

</details>

<details>
<summary><b>Days 11–20 — Prompt injection & jailbreaking fundamentals</b></summary>

- [ ] **Day 11** — Read [Simon Willison's prompt injection series](https://simonwillison.net/series/prompt-injection/) and learn the one sentence that explains the whole field: LLMs concatenate instructions and data into one token stream and can't reliably tell them apart.
- [ ] **Day 12** — Write down, in your own words, the technical difference between direct injection, indirect injection, and jailbreaking. (If you can't, re-read Day 11.)
- [ ] **Day 13** — Play [Lakera's Gandalf](https://gandalf.lakera.ai/) up to Level 4. After each win, name exactly which defense layer you bypassed.
- [ ] **Day 14** — Beat Gandalf Level 7. Document the precise payload and the security principle it defeats.
- [ ] **Day 15** — Play [TensorTrust](https://tensortrust.ai/) in attacker mode — try to extract a secret key from another player's system prompt.
- [ ] **Day 16** — Play TensorTrust in defender mode — write a system prompt to protect a key, then watch it get broken. Read the [TensorTrust dataset writeup](https://tensortrust.ai/posts/announcing-the-dataset/).
- [ ] **Day 17** — Try [PromptHack](https://prompthack.org/) and the [HackAPrompt](https://www.hackaprompt.com/) archive challenges.
- [ ] **Day 18** — Read [Universal and Transferable Adversarial Attacks on Aligned Language Models (Zou et al., 2023)](https://arxiv.org/abs/2307.15043) — the GCG suffix-attack paper.
- [ ] **Day 19** — Read [Jailbroken: How Does LLM Safety Alignment Behave Under Adversarial Prompting? (Wei et al., 2023)](https://arxiv.org/abs/2307.02483) — the competing-objectives / OOD-capability taxonomy.
- [ ] **Day 20** — Read Anthropic's [Many-Shot Jailbreaking](https://www.anthropic.com/research/many-shot-jailbreaking). Then run [Lab 2](LABS.md#-lab-2-crafting-direct-prompt-injections--jailbreaks): build a local Ollama sandbox and test Base64/roleplay/cognitive-split obfuscation against Llama 3.

</details>

<details>
<summary><b>Days 21–30 — Automated red teaming (garak, promptfoo, PyRIT)</b></summary>

- [ ] **Day 21** — Install and run [garak](https://github.com/NVIDIA/garak): `python -m garak --target_type huggingface --target_name gpt2 --probes dan.Dan_11_0`. This is your first automated adversarial campaign.
- [ ] **Day 22** — Re-run garak with the `promptinject` and `encoding` probe suites. Compare attack surfaces.
- [ ] **Day 23** — Read garak's probe/detector architecture, then write one custom probe for a pattern not in the default set.
- [ ] **Day 24** — Install [promptfoo](https://github.com/promptfoo/promptfoo) and run its red-team mode against a model of your choice.
- [ ] **Day 25** — Wire promptfoo into a CI pipeline so a safety regression fails the build. Map its output to OWASP LLM Top 10.
- [ ] **Day 26** — Set up Microsoft's [PyRIT](https://github.com/Azure/PyRIT).
- [ ] **Day 27** — Build a PyRIT orchestrator that iteratively rewrites an injection based on the target's responses until it breaks the system prompt.
- [ ] **Day 28** — Add a PyRIT evaluator LLM that auto-scores responses for policy violations. Run a full multi-turn campaign and save the transcript.
- [ ] **Day 29** — Read [HarmBench](https://github.com/centerforaisafety/HarmBench) — the standardized benchmark for scoring jailbreak attacks (GCG, AutoDAN, PAIR) and defenses.
- [ ] **Day 30** — Read [CyberSecEval (Meta)](https://github.com/meta-llama/PurpleLlama/tree/main/CyberSecEval) and run it locally against a model — it scores exploit generation, insecure-code suggestion, and cyberattack assistance.

</details>

<details>
<summary><b>Days 31–40 — Indirect injection & agentic exploitation</b></summary>

- [ ] **Day 31** — Read [More Than a Toy: Indirect Prompt Injection Against LLM-Integrated Applications (Greshake et al., 2023)](https://arxiv.org/abs/2302.12173) in full — the foundational paper for this whole attack class.
- [ ] **Day 32** — Run [Lab 3](LABS.md#-lab-3-indirect-prompt-injection-via-rag--tool-hijacking): build a local RAG agent, poison a text file it ingests, and get it to silently trigger an unauthorized "tool" call.
- [ ] **Day 33** — Install [AgentDojo](https://github.com/ethz-spylab/agentdojo) (`pip install agentdojo`) — 97 tasks, 629 security test cases across banking, Slack, travel, and workspace domains.
- [ ] **Day 34** — Reproduce one AgentDojo result and check it against the public leaderboard at [agentdojo.spylab.ai](https://agentdojo.spylab.ai).
- [ ] **Day 35** — Read [InjecAgent](https://github.com/uiuc-kang-lab/InjecAgent) and learn its direct-harm vs. data-stealing taxonomy for injected tool calls.
- [ ] **Day 36** — Read [AgentHarm](https://huggingface.co/datasets/ai-safety-institute/AgentHarm) — measures whether a jailbroken agent can actually *complete* a harmful multi-step task, not just agree to it.
- [ ] **Day 37** — Read [Agent Security Bench (ASB)](https://github.com/agiresearch/ASB) — 10 scenarios, 400+ tools, 27 attack/defense methods. Note its headline finding: an 84.3% attack success rate against current defenses.
- [ ] **Day 38** — Read ["Your AI, My Shell" (Liu et al., 2025)](https://arxiv.org/abs/2509.22040) — 314 attack payloads mapped to 70 MITRE ATT&CK techniques, up to 84% success rate against GitHub Copilot and Cursor.
- [ ] **Day 39** — Read [Maloyan & Namiot, 2026](https://arxiv.org/abs/2601.17548) — across 18 known defenses, most stop less than half of adaptive attacks on agentic coding assistants.
- [ ] **Day 40** — Case-study day: read the [Air Canada chatbot liability ruling](PLAYGROUNDS_AND_LABS.md#2-the-air-canada-chatbot-legal-liability-2024) and the [Chevrolet dealership $1 Tahoe jailbreak](PLAYGROUNDS_AND_LABS.md#3-the-chevrolet-bolt-support-chatbot-jailbreak-december-2023). Write one paragraph on the business/legal blast radius of each.

</details>

<details>
<summary><b>Days 41–50 — MCP & agent security</b></summary>

- [ ] **Day 41** — Read the [Model Context Protocol security best practices](https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices).
- [ ] **Day 42** — Install [mcp-scan](https://github.com/invariantlabs-ai/mcp-scan) and run it against a real MCP config (Claude Desktop, Cursor, or Claude Code).
- [ ] **Day 43** — Read Simon Willison's [The Lethal Trifecta](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/) — private-data access + untrusted-content exposure + external communication.
- [ ] **Day 44** — Audit exercise: enumerate every tool your own agent setup can reach in one session, tag each by capability axis, and check whether the union closes the trifecta.
- [ ] **Day 45** — Read the [MCP-Scanner paper (arXiv:2510.23673)](https://arxiv.org/html/2510.23673v1) — why per-server scanning misses composed risk.
- [ ] **Day 46** — Study the coding-agent attack vectors in [`AGENT_SECURITY.md`](AGENT_SECURITY.md): indirect injection via codebase ingestion, dependency typosquatting, denial-of-wallet loops, cloud metadata theft.
- [ ] **Day 47** — Study sandboxing architecture for agents: Firecracker/gVisor micro-VMs, egress filtering (block `169.254.169.254`), least-privilege filesystem mounts.
- [ ] **Day 48** — Study the "dual-homed agent" RBAC threat and vector-database semantic poisoning, both in `AGENT_SECURITY.md`.
- [ ] **Day 49** — Read the [OWASP Top 10 for Agentic Applications 2026](https://genai.owasp.org/2025/12/09/owasp-top-10-for-agentic-applications-the-benchmark-for-agentic-security-in-the-age-of-autonomous-ai/) (ASI01–ASI10).
- [ ] **Day 50** — Map three ASI categories to real, named incidents: ASI01 → EchoLeak (CVE-2025-32711), ASI04 → the `postmark-mcp` malicious MCP server, ASI05 → the Semantic Kernel `eval()` RCE pair (CVE-2026-26030 / CVE-2026-25592).

</details>

<details>
<summary><b>Days 51–60 — Defensive engineering: guardrails & guard models</b></summary>

- [ ] **Day 51** — Deploy [LLM Guard](https://github.com/protectai/llm-guard) and test its input/output scanners against your Day-20 jailbreak payloads.
- [ ] **Day 52** — Deploy [NeMo Guardrails](https://github.com/NVIDIA-NeMo/Guardrails) and write a Colang flow that constrains topic/tool use.
- [ ] **Day 53** — Run [Lab 5, "Implementing an Active Input/Output Guardrail Pipeline"](LABS.md): build your own input/output guardrail proxy from scratch in Python.
- [ ] **Day 54** — Attack your own Day-53 proxy. Find one sequence that clears the input classifier but gets caught by the output scanner (or vice versa).
- [ ] **Day 55** — Deploy [Llama Guard 4 (12B)](https://huggingface.co/meta-llama/Llama-Guard-4-12B) and test it against the MLCommons hazards taxonomy.
- [ ] **Day 56** — Deploy [Llama Prompt Guard 2](https://huggingface.co/meta-llama/Llama-Prompt-Guard-2-86M). Understand why Meta stacks it *in front of* Llama Guard (a content classifier is itself an injectable LLM).
- [ ] **Day 57** — Test [ShieldGemma 2](https://huggingface.co/google/shieldgemma-2b) on image + text moderation.
- [ ] **Day 58** — Test [Granite Guardian](https://arxiv.org/abs/2412.07724) for RAG-grounding and hallucination checks.
- [ ] **Day 59** — Test [WildGuard](https://huggingface.co/allenai/wildguard) — one model scoring prompt harm, response harm, and refusal.
- [ ] **Day 60** — Read the guardrail-bypass research at [arXiv:2511.22047](https://arxiv.org/abs/2511.22047), then benchmark any guard model from this week against your own adversarial set. Note the false-negative rate — it won't be zero.

</details>

<details>
<summary><b>Days 61–70 — Observability, detection & incident analysis</b></summary>

- [ ] **Day 61** — Deploy [Langfuse](https://github.com/langfuse/langfuse) and trace a full LLM application's prompts, tool calls, and outputs.
- [ ] **Day 62** — Deploy [OpenLLMetry](https://github.com/traceloop/openllmetry) for vendor-neutral OTel instrumentation of the same app.
- [ ] **Day 63** — Deploy [Invariant's Analyzer](https://github.com/invariantlabs-ai/invariant) over your captured traces to flag injection and unsafe tool execution after the fact.
- [ ] **Day 64** — Connect [LangKit](https://github.com/whylabs/langkit) and measure real-time prompt toxicity and semantic drift.
- [ ] **Day 65** — Write an anomaly-detection script that flags a simulated brute-force prompt-injection campaign from a log file based on statistical patterns.
- [ ] **Day 66** — Assemble Days 61–65 into one pipeline: capture → detect → alert. Run a Day-20 style attack through it end to end.
- [ ] **Day 67** — Browse the [AVID (AI Vulnerability Database)](https://avidml.org/) — read three real submitted incident reports.
- [ ] **Day 68** — Pick one incident from [MITRE ATLAS case studies](https://atlas.mitre.org/resources/case-studies/) and map it to ATLAS tactics start to finish.
- [ ] **Day 69** — Browse the [OECD AI Incident Monitor](https://oecd.ai/en/wonk/aim) for cross-industry AI failures outside security (bias, safety, reliability).
- [ ] **Day 70** — Write a full recon-to-impact ATLAS walkthrough for the EchoLeak (CVE-2025-32711) zero-click Copilot exfiltration.

</details>

<details>
<summary><b>Days 71–80 — Model supply chain security</b></summary>

- [ ] **Day 71** — Understand why `pickle` deserialization is inherently unsafe for shipping model weights.
- [ ] **Day 72** — Run [Lab 4](LABS.md#-lab-4-model-supply-chain-exploitation-via-pickle-malware): build a safe, educational pickle-RCE proof of concept.
- [ ] **Day 73** — Run [ModelScan](https://github.com/protectai/modelscan) against a batch of models downloaded from Hugging Face.
- [ ] **Day 74** — Run [PickleScan](https://github.com/mmaitre314/picklescan) (`≥ 0.0.31`) and read up on why the pre-patch versions were bypassable ([CVE-2025-10155 / -10156 / -10157](INCIDENTS_AND_GUIDANCE_2026.md), all CVSS 9.3).
- [ ] **Day 75** — Convert a pickle-based checkpoint to [`safetensors`](https://github.com/huggingface/safetensors) and confirm it loads with zero code execution.
- [ ] **Day 76** — Read the safetensors spec directly (it's short) and study the Rust header-parsing logic — this is the "Concept A" research angle for anyone chasing a novel supply-chain finding.
- [ ] **Day 77** — Read [Sleeper Agents (Hubinger et al., 2024, Anthropic)](https://arxiv.org/abs/2401.05566) — deceptive models that pass safety training and defect on trigger.
- [ ] **Day 78** — Read [Poisoning Language Models During Instruction Tuning (Wan et al., 2023)](https://arxiv.org/abs/2305.00944) — as few as ~100 poisoned examples can implant a functional backdoor.
- [ ] **Day 79** — Explore the [Trojan Detection Challenge dataset](https://trojandetection.ai/) — practice scanning for pre-injected backdoors.
- [ ] **Day 80** — Read [Toy Models of Superposition (Anthropic, 2022)](https://transformer-circuits.pub/2022/toy_models/index.html) — why polysemantic weights make backdoor detection mathematically hard.

</details>

<details>
<summary><b>Days 81–90 — Standards, compliance & extraction attacks</b></summary>

- [ ] **Day 81** — Read the full [MITRE ATLAS](https://atlas.mitre.org/) tactic matrix (13 tactics) and map your own lab environment against every one.
- [ ] **Day 82** — Read the [OWASP GenAI Red Teaming Guide](https://genai.owasp.org/resource/genai-red-teaming-guide/) — the four-phase method (model, implementation, infrastructure, runtime).
- [ ] **Day 83** — Read the [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) — Govern / Map / Measure / Manage.
- [ ] **Day 84** — Read [ISO/IEC 42001](https://www.iso.org/standard/81230.html) — the AI Management System standard's core compliance areas.
- [ ] **Day 85** — Read the EU AI Act's risk tiers (unacceptable / high / GPAI / minimal) and the systemic-risk obligations for frontier models.
- [ ] **Day 86** — Apply the [Enterprise AI Security Hardening Checklist](STANDARDS_AND_COMPLIANCE.md#-enterprise-ai-security-hardening-checklist) to a real project you have access to.
- [ ] **Day 87** — Read [Extracting Training Data from LLMs (Carlini et al., 2021)](https://arxiv.org/abs/2012.07805) — perplexity-based memorization extraction.
- [ ] **Day 88** — Read [Scalable Extraction of Training Data from (Private) LMs (Nasr et al., 2023)](https://arxiv.org/abs/2311.17035) — the "repeat this word forever" ChatGPT extraction attack.
- [ ] **Day 89** — Read [Linear Representation of Concepts in LLMs (Nanda et al., 2023)](https://arxiv.org/abs/2310.15154) — the activation-steering angle on future defenses.
- [ ] **Day 90** — Read [FigStep](https://github.com/ThuCCSLab/FigStep) and test a vision-language model against a text-in-image jailbreak.

</details>

<details>
<summary><b>Days 91–100 — Bug bounty, specialization & capstone</b></summary>

- [ ] **Day 91** — Read [Huntr.com](https://huntr.com/)'s scope for open-source AI/ML libraries (LangChain, LlamaIndex, Ray, MLflow) and shortlist a real target.
- [ ] **Day 92** — Build a mini eval harness using the [Do-Not-Answer](https://github.com/Libr-AI/do-not-answer) and [HarmfulQA](https://github.com/declare-lab/red-instruct) datasets.
- [ ] **Day 93** — Install the UK AISI's [Inspect AI](https://github.com/UKGovernmentBEIS/inspect_ai) framework and run one suite from [`inspect_evals`](https://github.com/UKGovernmentBEIS/inspect_evals).
- [ ] **Day 94** — Pick your specialization track from [`ROADMAP.md`](ROADMAP.md#-phase-3-advanced-specialization-months-46) — Research/Interpretability, Enterprise Defense, or Offensive Red Team — and read its core reading list.
- [ ] **Day 95** — Draft a full vulnerability report using the CVSS-scored template in [`ai-security-resources`](README.md) for a finding you've made (real or from your own lab).
- [ ] **Day 96** — Capstone setup: choose **Option A** (find and responsibly disclose a real vulnerability in an open-source AI library via Huntr) or **Option B** (build and publish an open-source AI security utility — a jailbreak detector, an MCP validator, a dataset checker).
- [ ] **Day 97** — Capstone build, day 1.
- [ ] **Day 98** — Capstone build, day 2. Write the technical root-cause explanation, not just the reproduction steps.
- [ ] **Day 99** — Publish the capstone on GitHub with a full writeup (background → mechanism → PoC → mitigation), the standard this repo's own [`ai-security-resources`](README.md) writeups hold to.
- [ ] **Day 100** — Map everything you built across the 100 days back onto OWASP LLM Top 10 / OWASP Agentic Top 10 / MITRE ATLAS in one page. Publish it as your retrospective and post it with `#100DaysOfAISecurity`.

</details>

---

## What you'll have after Day 100

- Hands-on reps with **garak, PyRIT, promptfoo, mcp-scan, ModelScan, LLM Guard, NeMo Guardrails, Langfuse, and Invariant** — not just reading about them.
- A working local exploit for indirect RAG injection, a pickle-RCE PoC, and a from-scratch guardrail proxy.
- Fluency mapping any attack to **OWASP LLM Top 10**, **OWASP Agentic Top 10 (ASI01–10)**, and **MITRE ATLAS**.
- A real, published capstone — a disclosed finding or a shipped tool — that's an actual portfolio artifact, not a certificate of completion.

If you want the exhaustive, non-day-boxed version of all of this — including the full tooling catalog, incident tracker, and salary/career data — it lives in the main guide: **[`ai-security-resources`](README.md)**.

Interviewing soon instead of studying long-term? **[AI Security Interview Questions](INTERVIEW_QUESTIONS.md)** covers 7 distinct AI-security-adjacent roles (red team, AppSec, MLSecOps, GRC, trust & safety, alignment research, model risk) with grounded technical Q&A for each.

Need real payloads to feed garak/PyRIT/promptfoo instead of reading about them? **[Prompt Injection & Jailbreak Technique Library](JAILBREAK_LIBRARY.md)** is a categorized taxonomy of attack technique classes, sourced entirely from public tools, papers, and datasets.

## Contributing

Found a dead link, a stale CVE reference, or a day that needs a better resource? See [CONTRIBUTING.md](CONTRIBUTING.md). PRs that swap in unverified or unmaintained resources will be closed — every link here has to trace back to a primary source.

## License

[CC0 1.0](LICENSE) — public domain. Take it, fork it, teach a class with it.
