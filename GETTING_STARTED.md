# 🚀 Getting Started in AI Security

> **Zero to value in under an hour.** No theory dumps, no 12-month roadmap — just the shortest path from "I have a security or ML background" to "I broke an LLM with my own hands and understand why it worked."

If you want the exhaustive curriculum, that lives in **[ROADMAP.md](ROADMAP.md)**. This file is the on-ramp. Read it, run the labs, ship your first finding today.

---

## 👤 Who This Is For / What You'll Be Able To Do

You're a **security engineer, pentester, ML engineer, or researcher** who keeps hearing "prompt injection," "jailbreak," and "MCP" and wants to actually *do* the work instead of reading think-pieces about it. You already understand either how software gets attacked or how models get trained — you just need the AI-specific layer.

**After one focused hour you will be able to:**

- Explain why prompt injection is unfixable in the general case (it's not a bug, it's the architecture)
- Manually beat a prompt-injection challenge and articulate *which* defense layer you bypassed
- Run an automated LLM vulnerability scan against a real model and read the report
- Place any new attack you read about into a standard taxonomy (OWASP LLM Top 10 / MITRE ATLAS)
- Know exactly which deeper handbook in this repo to open next

---

## ⏱️ The 60-Minute Path

Do these in order. Each block is time-boxed. Don't optimize, don't tangent — just move.

### Minutes 0–10 — Get the one mental model that matters

Read **[Simon Willison's prompt injection series](https://simonwillison.net/series/prompt-injection/)** — start with the explainer posts. Willison coined the term, and his framing is the one every practitioner uses: **LLMs concatenate trusted instructions and untrusted data into the same token stream, and the model has no reliable way to tell them apart.** That single sentence is 80% of why this field exists.

Then skim **[OWASP Top 10 for LLM Applications (2025)](https://genai.owasp.org/llm-top-10/)**. You only need to absorb the top entries: **LLM01 Prompt Injection**, **LLM02 Sensitive Information Disclosure**, and **LLM06 Excessive Agency**. These three explain the majority of real-world LLM incidents.

### Minutes 10–30 — Break something by hand

Play **[Lakera's Gandalf](https://gandalf.lakera.ai/)**. It's a prompt-injection challenge where each level guards a password with progressively stronger defenses (Level 1 has none; later levels add input filters, output filters, and an LLM judge). Get to at least Level 4.

This is the *fastest* way to internalize that defenses **layer**, and that beating a system means identifying and bypassing each layer independently. When you extract a password, stop and name what you defeated: Was it a naive system prompt? An output filter scanning for the literal secret? A guard model? That muscle — *narrating which control you bypassed* — is the core red-team skill.

### Minutes 30–55 — Run a real scanner

Install and run **[garak](https://github.com/NVIDIA/garak)** (NVIDIA's LLM vulnerability scanner — think "Nmap for LLMs," 100+ probes for jailbreaks, injection, data leakage, toxicity):

```bash
python -m pip install -U garak

# Probe a small local Hugging Face model for a classic jailbreak family.
# Runs on CPU, no API key needed — this is your guaranteed first win.
python -m garak --target_type huggingface --target_name gpt2 --probes dan.Dan_11_0
```

Watch garak generate attack prompts, send them, and score the responses with its detectors. Open the HTML/JSONL report it writes at the end. **You just ran an automated adversarial campaign.** Tweak `--probes` to try `promptinject` or `encoding` and watch the attack surface change.

> Want to point garak at an API model (OpenAI, etc.)? See the target docs in the [garak README](https://github.com/NVIDIA/garak) — but the `gpt2` run above needs nothing but `pip`.

### Minutes 55–60 — Build your map

Open **[MITRE ATLAS](https://atlas.mitre.org/)** — the ATT&CK-style matrix of adversary tactics and techniques against AI systems. Find the technique that matches what Gandalf and garak just did to you (look under the prompt-injection and LLM-related techniques). From now on, every attack you learn gets filed into ATLAS or the OWASP list. **You now have a framework, not a pile of tricks.**

---

## 🧭 Pick Your Track

The 60-minute path is shared. Where you go next depends on the job you want. Pick one and commit to its first resources.

### 🔴 Red Team — "I find what breaks before attackers do"

1. **[garak](https://github.com/NVIDIA/garak)** — go deep. Learn the probe/detector architecture and write a custom probe.
2. **[Microsoft PyRIT](https://github.com/microsoft/PyRIT)** — the Python Risk Identification Toolkit for multi-turn, orchestrated attacks. This is where you graduate from single-shot prompts to automated conversation-level red teaming.
3. **[Universal and Transferable Adversarial Attacks on Aligned LLMs (Zou et al., 2023)](https://arxiv.org/abs/2307.15043)** — the GCG paper. Gradient-optimized adversarial suffixes that transfer across models. The canonical "jailbreaks can be *computed*, not just discovered" result.
4. Then open the repo's **[PLAYGROUNDS_AND_LABS.md](PLAYGROUNDS_AND_LABS.md)** and **[LABS.md](LABS.md)** for hands-on jailbreak and injection labs.

### 🔵 Defender — "I keep AI systems safe in production"

1. **[protectai/llm-guard](https://github.com/protectai/llm-guard)** — open-source input/output scanners (prompt injection, PII, secrets, toxicity). `pip install llm-guard` and wire it around a model.
2. **[OWASP Top 10 for LLM Applications (2025)](https://genai.owasp.org/llm-top-10/)** — read it *fully* this time. It's your control checklist and the language enterprise stakeholders speak.
3. **[MCP Security Best Practices](https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices)** — the Model Context Protocol connects agents to tools and is now a primary attack surface (token passthrough, tool poisoning, confused-deputy). Read before you let an agent touch anything real.
4. Then go deep with the repo's **[AGENT_SECURITY.md](AGENT_SECURITY.md)** and **[TOOLS.md](TOOLS.md)** (defensive tooling, honestly rated by maintenance status).

### 🟡 Researcher — "I discover new attack and defense classes"

1. **[Not What You've Signed Up For: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection (Greshake et al., 2023)](https://arxiv.org/abs/2302.12173)** — the paper that defined the *indirect* injection threat model and the modern agent risk landscape.
2. **[AgentDojo](https://github.com/ethz-spylab/agentdojo)** — a runnable benchmark (`pip install agentdojo`) for evaluating prompt-injection attacks and defenses against tool-using agents. Reproduce a result, then try to beat it.
3. **[MITRE ATLAS](https://atlas.mitre.org/)** — use it to find the *gaps*: techniques with thin case-study coverage are where novel research lives.
4. Then mine the repo's **[RESEARCH_PAPERS.md](RESEARCH_PAPERS.md)** and **[BENCHMARKS_AND_DATASETS.md](BENCHMARKS_AND_DATASETS.md)** for the citation graph and eval harnesses.

---

## 🏆 Your First Hands-On Win

If you only do one runnable thing, do this — it's verified to work with nothing but Python and a CPU:

```bash
python -m pip install -U garak
python -m garak --target_type huggingface --target_name gpt2 --probes dan.Dan_11_0
```

When it finishes, garak prints a pass/fail summary per probe and writes a report file. **That report is your first artifact.** Screenshot it, note which probes the model failed, and you have something concrete to talk about — exactly the kind of "I broke something" evidence that gets you hired (see the hiring guidance in [README.md](README.md)).

Want a no-install win first? Beat **[Gandalf](https://gandalf.lakera.ai/)** Level 3+ in your browser and write down the exact prompt that worked and why.

---

## ⚠️ Common Newcomer Mistakes

Opinionated, from watching people enter this field:

- **Treating prompt injection like a bug to be patched.** It's a structural property of mixing instructions and data in one channel. You *mitigate and contain blast radius*; you don't "fix" it. Anyone selling a 100% prompt-injection filter is selling snake oil.
- **Confusing jailbreaking with prompt injection.** Jailbreaking makes the model violate *its own* safety policy (you, the user, are the attacker). Prompt injection makes the model obey a *third party's* instructions hidden in data it processes (the attacker isn't the user). Different threat models, different defenses. Mixing them up marks you as a beginner instantly.
- **Skipping the fundamentals because the tools are easy to run.** Running garak is trivial. Understanding *why* a probe works requires knowing how attention and tokenization behave. Don't stay a script-runner — see the foundational sections in [README.md](README.md) and [ROADMAP.md](ROADMAP.md).
- **Chasing the newest flashy jailbreak instead of building a model.** Individual jailbreaks rot fast as models update. The taxonomy (OWASP, ATLAS) and the *why* persist. Learn the categories, not just the tricks.
- **Ignoring the agent/MCP layer.** The 2025–26 incident wave (e.g. the zero-click **EchoLeak / CVE-2025-32711** exfiltration in Microsoft 365 Copilot) happened at the *agent and tool* boundary, not the raw chat box. If you only test chatboxes you're testing last year's attack surface. See **[INCIDENTS_AND_GUIDANCE_2026.md](INCIDENTS_AND_GUIDANCE_2026.md)**.
- **Running attacks against systems you don't own.** Use the labs, CTFs, and your own deployments. Gandalf, garak-on-local-models, and AgentDojo exist precisely so you can practice legally.

---

## 🧭 Where to Go Next

You've done the hour. Here's the deeper repo, ordered by what most people need next:

| If you want to… | Open this |
|:---|:---|
| Follow a structured 12-month path | **[ROADMAP.md](ROADMAP.md)** |
| Run ready-made attack labs (FGSM, injection, pickle RCE, guardrails) | **[LABS.md](LABS.md)** |
| Find CTFs, playgrounds, and bug bounties | **[PLAYGROUNDS_AND_LABS.md](PLAYGROUNDS_AND_LABS.md)** |
| Secure autonomous agents & MCP | **[AGENT_SECURITY.md](AGENT_SECURITY.md)** |
| Pick tools by what you're trying to do | **[TOOLS.md](TOOLS.md)** |
| Study real disclosed incidents + authoritative guidance | **[INCIDENTS_AND_GUIDANCE_2026.md](INCIDENTS_AND_GUIDANCE_2026.md)** |
| Speak the compliance language (OWASP, NIST, ATLAS, EU AI Act) | **[STANDARDS_AND_COMPLIANCE.md](STANDARDS_AND_COMPLIANCE.md)** |
| Go deep on the literature | **[RESEARCH_PAPERS.md](RESEARCH_PAPERS.md)** |
| Understand the comp landscape before negotiating | **[SALARY_REALITY.md](SALARY_REALITY.md)** |

Then contribute back — found a lab that's broken, a tool that died, a paper we missed? See **[CONTRIBUTING.md](CONTRIBUTING.md)**. The fastest way to learn this field is to start curating it.

---

*Maintained with [Claude Code](https://claude.ai/code) — see the [autonomous agent experiment](https://github.com/ppradyoth/social-experiment-with-agents).*
