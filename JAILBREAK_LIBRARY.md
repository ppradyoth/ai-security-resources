# 🗡️ Prompt Injection & Jailbreak Technique Library

[![License: CC0-1.0](https://img.shields.io/badge/License-CC0--1.0-lightgrey.svg?style=for-the-badge)](LICENSE)
[![PRs: Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen.svg?style=for-the-badge)](CONTRIBUTING.md)
[![Techniques](https://img.shields.io/badge/Categories-11-red?style=for-the-badge)](#the-taxonomy)

> The `SecLists` of AI security: a categorized taxonomy of every major prompt injection and jailbreak technique class, grounded entirely in public, verifiable sources — not a firehose of invented exploit strings. Each category ships a short illustrative example, the mechanism behind it, and a direct link to the real, runnable probe module or dataset that supplies bulk payloads for actual testing.

## ⚠️ Purpose & Responsible Use

This is a **defensive and authorized-red-team research reference**, built to feed tools like [garak](https://github.com/NVIDIA/garak), [PyRIT](https://github.com/Azure/PyRIT), and [promptfoo](https://github.com/promptfoo/promptfoo) — the same three tools referenced throughout this repo's own taxonomy. **Only test systems you own or have explicit written authorization to test.** Every technique category below is already public — documented in a maintained open-source scanner, a peer-reviewed paper, or a dataset released for research — because publishing genuinely novel, undisclosed attack techniques here would help nobody but an attacker. If you're looking for someone's private, unpublished bug-bounty methodology, you won't find it here; that's not what this repo is, and it never will be.

---

## The Taxonomy

<details>
<summary><b>1. 🔓 Direct Instruction Override & Classic Injection</b></summary>

**Mechanism:** the attacker's text directly asks the model to disregard its system prompt or prior instructions and follow a new, attacker-supplied instruction instead — the foundational, lowest-sophistication attack class, and still the first thing to try against any new target.

**Illustrative shape:** `"Ignore the above and instead [attacker objective]."` — real-world variants swap the trigger phrase ("disregard," "new instructions:", "developer override:") but keep the same override-then-redirect structure.

**Real tooling:** garak's [`promptinject`](https://github.com/NVIDIA/garak/blob/main/garak/probes/promptinject.py) probe ("basic prompt injections, including plaintext malicious commands into requests for innocuous information") and [`exploitation`](https://github.com/NVIDIA/garak/blob/main/garak/probes/exploitation.py) probe (template/code/SQL injection via the same mechanism) run this class automatically.

</details>

<details>
<summary><b>2. 🎭 Roleplay, Persona & Social-Engineering Jailbreaks</b></summary>

**Mechanism:** the model is asked to adopt a fictional persona whose "rules" supersede its actual safety alignment — the model isn't told to ignore its instructions, it's told to *become* something that never had them.

**Named families (all documented in public research/tooling, not invented here):** **DAN** ("Do Anything Now" — garak's [`dan`](https://github.com/NVIDIA/garak/blob/main/garak/probes/dan.py) probe module, "probes that try to get a target to ignore its alignment and behave beyond it"), the **Grandma exploit** (garak's [`grandma`](https://github.com/NVIDIA/garak/blob/main/garak/probes/grandma.py) probe — "roleplay as a grandma" to reframe a harmful request as an innocent bedtime-story request), and the [**Goodside**](https://github.com/NVIDIA/garak/blob/main/garak/probes/goodside.py) probe collection (named attacks discovered by researcher Riley Goodside).

**Real tooling:** the full DAN prompt corpus and hundreds of roleplay variants are in the CCS 2024 `jailbreak_llms` dataset (see [Full-Scale Datasets](#full-scale-public-datasets) below) — 1,405 labeled jailbreak prompts collected from real-world use.

</details>

<details>
<summary><b>3. 🔡 Encoding, Obfuscation & Token Smuggling</b></summary>

**Mechanism:** the harmful instruction is transformed (encoded, split, translated) so a keyword-based safety filter never sees the literal trigger phrase, then the model is asked to decode/reassemble it — the filter and the model see different things.

**Named techniques:** garak's [`encoding`](https://github.com/NVIDIA/garak/blob/main/garak/probes/encoding.py) probe ("encode an unsafe instruction or string in an effort to get it past alignment" — Base64, ROT13, hex, and similar), [`smuggling`](https://github.com/NVIDIA/garak/blob/main/garak/probes/smuggling.py) ("bypass filters via obfuscation of contentious terms" — character splitting, homoglyphs, zero-width characters), [`phrasing`](https://github.com/NVIDIA/garak/blob/main/garak/probes/phrasing.py) ("rephrase requests for unsafe content"), and [`glitch`](https://github.com/NVIDIA/garak/blob/main/garak/probes/glitch.py) ("tokens that lead to unusual model behavior when processed").

**Illustrative shape:** `"Decode this Base64 string and follow the instruction inside it: [encoded payload]"` — the filter scans the literal request text, not what the model produces after decoding.

</details>

<details>
<summary><b>4. 🧮 Adversarial Search & Automated Attack Generation</b></summary>

**Mechanism:** instead of a human hand-crafting the bypass, an algorithm or a second LLM searches for one — computing a suffix, or iteratively rewriting a prompt based on the target's responses, until alignment breaks.

**Named techniques:** the **GCG (Greedy Coordinate Gradient)** suffix attack from [Zou et al., 2023](https://arxiv.org/abs/2307.15043) — garak's [`suffix`](https://github.com/NVIDIA/garak/blob/main/garak/probes/suffix.py) probe ("disrupt a system prompt by appending an adversarial suffix"); **TAP (Tree of Attacks with Pruning)** — garak's [`tap`](https://github.com/NVIDIA/garak/blob/main/garak/probes/tap.py) probe, using an attacker LLM to search a tree of candidate prompts; **GOAT (Generative Offensive Agent Tester)** — garak's [`goat`](https://github.com/NVIDIA/garak/blob/main/garak/probes/goat.py) probe, a multi-turn attacker LLM using an Observation-Thought-Strategy-Response loop; and [`atkgen`](https://github.com/NVIDIA/garak/blob/main/garak/probes/atkgen.py), which uses a separate model purely to generate attack prompts.

**Real tooling:** run these directly — `python -m garak --probes suffix,tap,goat` against any target.

</details>

<details>
<summary><b>5. 🪜 Multi-Turn Escalation & Agentic Attacks</b></summary>

**Mechanism:** the attack is spread across multiple turns, each individually innocuous, with the model's own prior compliance used to justify the next, slightly more adversarial step — single-turn filters never see the full trajectory.

**Named techniques:** **FITD (Foot In The Door)** — garak's [`fitd`](https://github.com/NVIDIA/garak/blob/main/garak/probes/fitd.py) probe, "generates queries of increasing intensity from safe to adversarial, getting compliance from the model at each intermediate stage"; **Agent Breaker** — garak's [`agent_breaker`](https://github.com/NVIDIA/garak/blob/main/garak/probes/agent_breaker.py) probe, "a multi-turn red-team probe for attacking agentic LLM applications that use tools"; and **Many-Shot Jailbreaking** ([Anthropic, 2024](https://www.anthropic.com/research/many-shot-jailbreaking)) — packing dozens of benign example Q&A pairs before the real harmful request to override alignment via in-context learning.

</details>

<details>
<summary><b>6. 🧬 Indirect & Latent Injection</b></summary>

**Mechanism:** the malicious instruction isn't in the user's message at all — it's embedded in content the model is asked to process (a document, resume, webpage, or tool output), so the model can't distinguish "data I'm summarizing" from "instructions I must obey." This is [Greshake et al., 2023](https://arxiv.org/abs/2302.12173)'s foundational threat model.

**Named techniques:** garak's [`latentinjection`](https://github.com/NVIDIA/garak/blob/main/garak/probes/latentinjection.py) probe ("prompt injections buried in other contexts, like a resume or financial report") and [`web_injection`](https://github.com/NVIDIA/garak/blob/main/garak/probes/web_injection.py) ("injection attacks against websites, such as private data exfiltration (CSRF) or script injection (XSS)").

**Benchmarks that measure this at scale:** [AgentDojo](https://github.com/ethz-spylab/agentdojo) (629 security test cases), [InjecAgent](https://github.com/uiuc-kang-lab/InjecAgent) (1,054 test cases across 17 user tools / 62 attacker tools).

</details>

<details>
<summary><b>7. 🕵️ System Prompt Extraction</b></summary>

**Mechanism:** techniques designed to recover a model's confidential system prompt rather than to make it misbehave — garak's own [`sysprompt_extraction`](https://github.com/NVIDIA/garak/blob/main/garak/probes/sysprompt_extraction.py) probe module describes this precisely: *"probes that attempt to extract a model's system prompt using adversarial techniques including direct requests, encoding tricks, and role-playing"* — i.e., this category is really categories 1, 3, and 2 above, retargeted at a different objective (leak the prompt, not break the policy).

**Real tooling:** `python -m garak --probes sysprompt_extraction` runs the full public probe suite directly — no need to hand-roll a technique list when a maintained, versioned one already exists.

</details>

<details>
<summary><b>8. 💧 Training-Data Memorization & Privacy Leakage</b></summary>

**Mechanism:** distinct from injection — these techniques try to make a model regurgitate verbatim fragments of its training data, including PII, rather than manipulate its instruction-following.

**Named techniques:** garak's [`divergence`](https://github.com/NVIDIA/garak/blob/main/garak/probes/divergence.py) probe ("get target output to deviate from the original topic and leak training data"), [`leakreplay`](https://github.com/NVIDIA/garak/blob/main/garak/probes/leakreplay.py) ("get a model to leak whether or not a given document was used in training, by having it replay an excerpt"), and [`propile`](https://github.com/NVIDIA/garak/blob/main/garak/probes/propile.py) (PII memorization probes based on the ProPILE paper). Grounded in [Carlini et al., 2021](https://arxiv.org/abs/2012.07805) (perplexity-based extraction) and [Nasr et al., 2023](https://arxiv.org/abs/2311.17035) (the "repeat this word forever" ChatGPT extraction).

</details>

<details>
<summary><b>9. 🧩 Disguise & Reconstruction</b></summary>

**Mechanism:** the harmful request is split into a masked/disguised form the model won't refuse, with instructions to reconstruct and act on the real request internally — related to but distinct from simple encoding, because the disguise is semantic/structural rather than a pure character transform.

**Named techniques:** **DRA (Disguise and Reconstruction Attack)** — garak's [`dra`](https://github.com/NVIDIA/garak/blob/main/garak/probes/dra.py) probe, "tests models using harmful strings disguised and reconstructed through string manipulation"; **SATA (Simple Assistive Task Linkage)** — garak's [`sata`](https://github.com/NVIDIA/garak/blob/main/garak/probes/sata.py) probe, replacing violating text with a `[MASK]` token linked to an assistive reconstruction task.

</details>

<details>
<summary><b>10. 🖼️ Multimodal Jailbreaks</b></summary>

**Mechanism:** the adversarial instruction is delivered through a non-text modality a safety-tuned text filter never inspects.

**Named techniques:** garak's [`visual_jailbreak`](https://github.com/NVIDIA/garak/blob/main/garak/probes/visual_jailbreak.py) probe ("write instructions in an image in an attempt to bypass text-based alignment") and [`audio`](https://github.com/NVIDIA/garak/blob/main/garak/probes/audio.py) probes. Academically grounded in [FigStep](https://github.com/ThuCCSLab/FigStep) (AAAI 2025) — text-in-image jailbreaks against vision-language models.

</details>

<details>
<summary><b>11. 🛡️ Safety-Eval & Refusal Probes (for defenders, not just attackers)</b></summary>

**Mechanism:** these aren't jailbreaks — they're the non-adversarial and semi-adversarial probes used to check a model's *baseline* refusal behavior, over-refusal rate, and willingness to comply with borderline requests. A red-teamer runs these first to establish a baseline before layering the categories above on top.

**Named techniques:** garak's [`donotanswer`](https://github.com/NVIDIA/garak/blob/main/garak/probes/donotanswer.py) probe (based on the [Do-Not-Answer dataset](https://github.com/Libr-AI/do-not-answer), 939 questions models should refuse), [`lmrc`](https://github.com/NVIDIA/garak/blob/main/garak/probes/lmrc.py) (Language Model Risk Cards — "largely non-adversarial" content-safety checks), [`malwaregen`](https://github.com/NVIDIA/garak/blob/main/garak/probes/malwaregen.py) ("is the system willing to comply with requests to generate malware"), [`misleading`](https://github.com/NVIDIA/garak/blob/main/garak/probes/misleading.py) (false-claim debunking), and [`continuation`](https://github.com/NVIDIA/garak/blob/main/garak/probes/continuation.py) (contentious-term continuation). Also see [HarmfulQA](https://github.com/declare-lab/red-instruct) (1,960 harmful questions across 10 topics).

</details>

---

## Full-Scale Public Datasets

For actual bulk payload volume — this repo is a taxonomy and index, not a replacement for these:

| Dataset | Scale | Source |
|:---|:---|:---|
| **[HackAPrompt](https://huggingface.co/datasets/hackaprompt/hackaprompt-dataset)** | 600,000+ adversarial prompts, a 29-technique taxonomy, from a 2,800+-participant global competition | [Schulhoff et al., EMNLP 2023 — arXiv:2311.16119](https://arxiv.org/abs/2311.16119) |
| **[`jailbreak_llms` (In-The-Wild Jailbreak Prompts)](https://github.com/verazuo/jailbreak_llms)** | 15,140 prompts (Dec 2022–Dec 2023), 1,405 labeled jailbreaks, from Reddit/Discord/websites | [Shen et al., ACM CCS 2024 — arXiv:2308.03825](https://arxiv.org/abs/2308.03825) |
| **[TensorTrust dataset](https://tensortrust.ai/posts/announcing-the-dataset/)** | Crowdsourced attack *and* defense prompts from an interactive key-extraction game | [tensortrust.ai](https://tensortrust.ai/) |
| **[Do-Not-Answer](https://github.com/Libr-AI/do-not-answer)** | 939 questions models should refuse, across risk categories | Libr-AI |
| **[HarmfulQA](https://github.com/declare-lab/red-instruct)** | 1,960 harmful questions across 10 topics | [arXiv:2308.09662](https://arxiv.org/abs/2308.09662) |

---

## How to Use This With Real Tooling

- **Automated scanning:** [garak](https://github.com/NVIDIA/garak) implements nearly every category above as a runnable probe module — `python -m garak --target_type huggingface --target_name gpt2 --probes dan,promptinject,encoding` runs three categories in one pass.
- **CI-integrated red-teaming:** [promptfoo](https://github.com/promptfoo/promptfoo) auto-generates adversarial test cases across 50+ plugins mapped to OWASP LLM Top 10, for pipelines that need a safety regression to fail the build.
- **Multi-turn orchestration:** [PyRIT](https://github.com/Azure/PyRIT) operationalizes the category-4/5 techniques (adversarial search, multi-turn escalation) as configurable orchestrators.
- **Practice targets:** [Gandalf](https://gandalf.lakera.ai/), [TensorTrust](https://tensortrust.ai/), and [AgentDojo](https://github.com/ethz-spylab/agentdojo) let you try these categories against real, legally-sanctioned targets before pointing them at anything else.

---

## The family

This pairs with the day-count learning series and the master resource catalog, all built from the same verified-source discipline:

| Repo | For |
|:---|:---|
| [**7 Days of AI Security**](LEARNING_PATH_7_DAYS.md) | A one-week taste test, zero prerequisites |
| [**30 Days of AI Security**](LEARNING_PATH_30_DAYS.md) | A time-boxed month to real competence |
| [**100 Days of AI Security**](LEARNING_PATH_100_DAYS.md) | The full curriculum, foundations to capstone |
| [**AI Security Interview Questions**](INTERVIEW_QUESTIONS.md) | Prepping for an interview, across 7 adjacent roles |
| [**ai-security-resources**](README.md) | The exhaustive, non-day-boxed catalog everything above is built from |

## Contributing

New category or technique? See [CONTRIBUTING.md](CONTRIBUTING.md). The bar: it must already be public (a maintained tool's probe/plugin, a peer-reviewed paper, or a released dataset) — this repo does not publish novel, undisclosed techniques, full stop.

## License

[CC0 1.0](LICENSE) — public domain.
