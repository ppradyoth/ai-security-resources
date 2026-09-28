# ⚡ 7 Days of AI Security

[![License: CC0-1.0](https://img.shields.io/badge/License-CC0--1.0-lightgrey.svg?style=for-the-badge)](LICENSE)
[![PRs: Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen.svg?style=for-the-badge)](CONTRIBUTING.md)
[![Progress](https://img.shields.io/badge/Days-0%2F7-red?style=for-the-badge)](#-the-7-days)

> One week. No prerequisites beyond basic Python and curiosity. By Day 7 you'll have broken a real LLM by hand, run an automated adversarial scan, exploited an indirect prompt injection, built a guardrail, and scanned an MCP config — and you'll know exactly which taxonomy (OWASP / MITRE ATLAS) every attack you learn from here on belongs in.

This is the **on-ramp**, not the whole field. It's a compressed version of [Getting Started in AI Security](GETTING_STARTED.md) turned into a week of daily reps. If you finish this and want more, go to **[30 Days of AI Security](LEARNING_PATH_30_DAYS.md)** or the full **[100 Days of AI Security](LEARNING_PATH_100_DAYS.md)**.

Every link below is a real, verifiable primary source — a tool's own repo, a named CTF, or the paper that coined the term. Nothing is invented to fill 7 days.

---

## The 7 Days

- [ ] **Day 1 — The one idea that explains the whole field.** Read [Simon Willison's prompt injection series](https://simonwillison.net/series/prompt-injection/): LLMs concatenate trusted instructions and untrusted data into one token stream and can't reliably tell them apart. Then skim [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/), focusing on **LLM01 Prompt Injection**, **LLM02 Sensitive Information Disclosure**, **LLM06 Excessive Agency**.

- [ ] **Day 2 — Break something by hand.** Play [Lakera's Gandalf](https://gandalf.lakera.ai/) to at least Level 5. After every win, write down which specific defense layer you defeated (naive prompt? output filter? guard model?). Then try [TensorTrust](https://tensortrust.ai/) in attacker mode.

- [ ] **Day 3 — Run your first automated attack.** Install and run [garak](https://github.com/NVIDIA/garak):
  ```bash
  python -m pip install -U garak
  python -m garak --target_type huggingface --target_name gpt2 --probes dan.Dan_11_0
  ```
  Open the report it generates. Try swapping `--probes` for `promptinject` or `encoding` and compare results.

- [ ] **Day 4 — Learn indirect injection, the attack that actually matters in production.** Read [Greshake et al., 2023](https://arxiv.org/abs/2302.12173) — the paper that defined the threat model behind almost every real 2025–26 AI incident (EchoLeak, agentic-browser hijacks). Then build [Lab 3](LABS.md#-lab-3-indirect-prompt-injection-via-rag--tool-hijacking): a local RAG agent that gets hijacked by a poisoned document into calling a tool it shouldn't.

- [ ] **Day 5 — Build a defense.** Install [LLM Guard](https://github.com/protectai/llm-guard) or build [Lab 5](LABS.md) from scratch: an input/output guardrail proxy. Then attack your own guardrail and find one payload that gets through.

- [ ] **Day 6 — Meet the newest attack surface: agents and MCP.** Read Simon Willison's [The Lethal Trifecta](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/) (private-data access + untrusted-content exposure + external communication = data theft). Install [mcp-scan](https://github.com/invariantlabs-ai/mcp-scan) and run it against any MCP config you have (Claude Desktop, Cursor, Claude Code).

- [ ] **Day 7 — Get your map.** Open [MITRE ATLAS](https://atlas.mitre.org/) and place every attack you did this week into its tactic matrix. Then pick your next step: **[Red Team](GETTING_STARTED.md#-red-team--i-find-what-breaks-before-attackers-do)**, **[Defender](GETTING_STARTED.md#-defender--i-keep-ai-systems-safe-in-production)**, or **[Researcher](GETTING_STARTED.md#-researcher--i-discover-new-attack-and-defense-classes)** — and go deep in **[30 Days of AI Security](LEARNING_PATH_30_DAYS.md)**.

---

## What you'll have after Day 7

- A first-hand answer to "why is prompt injection unfixable in general" — you'll have caused it yourself, twice.
- A garak scan report and a working indirect-injection exploit you built, not just read about.
- Enough OWASP/ATLAS vocabulary to not sound like a beginner in your next conversation about this.

## The family

This is the shortest of three linked curricula, all built from the same verified source: [`ai-security-resources`](README.md).

| Repo | For |
|:---|:---|
| **7 Days of AI Security** (this repo) | A taste test. One week, zero prerequisites. |
| [**30 Days of AI Security**](LEARNING_PATH_30_DAYS.md) | A serious, time-boxed month to real competence. |
| [**100 Days of AI Security**](LEARNING_PATH_100_DAYS.md) | The full curriculum, foundations to capstone. |
| [**AI Security Interview Questions**](INTERVIEW_QUESTIONS.md) | Prepping for an interview now, across 7 AI-security-adjacent roles. |
| [**Prompt Injection & Jailbreak Technique Library**](JAILBREAK_LIBRARY.md) | A categorized taxonomy of attack techniques, for feeding garak/PyRIT/promptfoo. |

## Contributing

Found a dead link or a better primary source for a day? See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[CC0 1.0](LICENSE) — public domain.
