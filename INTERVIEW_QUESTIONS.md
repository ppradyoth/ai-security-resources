# 🎯 AI Security Interview Questions

[![License: CC0-1.0](https://img.shields.io/badge/License-CC0--1.0-lightgrey.svg?style=for-the-badge)](LICENSE)
[![PRs: Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen.svg?style=for-the-badge)](CONTRIBUTING.md)
[![Roles](https://img.shields.io/badge/Roles-7-blue?style=for-the-badge)](#which-role-is-this-for)

> "AI security engineer" isn't one job. A red-teamer, a GRC analyst, and an alignment researcher get asked completely different questions in completely different interviews — and most prep resources flatten them into one generic list. This repo doesn't. **7 real, distinct AI-security-adjacent roles**, with the skills that actually separate them and the technical questions each one is likely to ask.

Every answer here is grounded in a verifiable primary source — an OWASP category, a MITRE ATLAS tactic, a named tool, a real paper, or a disclosed CVE. Where a question doesn't have one clean factual answer (most GRC and alignment questions don't), that's stated explicitly instead of inventing false certainty.

**Not sure which role you're prepping for?** See [the comparison table](#which-role-is-this-for) below.

---

## Which role is this for?

| Role | What you actually do | Interview leans |
|:---|:---|:---|
| [🔴 AI/LLM Red Team Engineer](#-1-aillm-red-team-engineer) | Break models and AI products before attackers do — jailbreaks, injection, agentic exploits | Hands-on offense: tools, exploit mechanics, live problem-solving |
| [🔒 AI Security Engineer (Product/AppSec)](#-2-ai-security-engineer-productappsec) | Secure the application wrapped around the model — auth, guardrails, output handling | Classic AppSec + where LLMs break the usual assumptions |
| [🔧 MLSecOps / ML Platform Security Engineer](#-3-mlsecops--ml-platform-security-engineer) | Secure the training/serving infrastructure — pipelines, weights, registries, supply chain | Infra security, IAM, supply-chain integrity |
| [📋 AI Governance, Risk & Compliance (GRC)](#-4-ai-governance-risk--compliance-grc) | Map AI systems to regulatory/risk frameworks, run audits, write policy | Frameworks (NIST, ISO, EU AI Act), risk reasoning over code |
| [🚨 Trust & Safety Engineer (GenAI)](#-5-trust--safety-engineer-genai) | Stop abuse of AI products at scale — CSAM/spam/fraud misuse, policy enforcement | Abuse patterns, detection at scale, policy tradeoffs |
| [🧠 AI Safety / Alignment Researcher](#-6-ai-safety--alignment-researcher) | Research why models misbehave and how to make them robustly safe | Research literacy, experiment design, deep ML fundamentals |
| [🏦 AI Model Risk / Model Audit](#-7-ai-model-risk--model-audit) | Independent validation of AI/ML models before and after deployment (common in regulated industries) | Model risk frameworks, statistical validation, documentation review |

If you're not sure which one you're interviewing for, ask the recruiter which team you'd sit on — the honest answer is these roles overlap at the edges and job titles are inconsistent across companies.

---

## 🔴 1. AI/LLM Red Team Engineer

**The job:** adversarially test models and AI products — prompt injection, jailbreaking, agentic exploitation — manually and with automated tooling (garak, PyRIT, promptfoo), then write findings that get fixed.

**Core skills to have ready:** [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/), [MITRE ATLAS](https://atlas.mitre.org/), hands-on time with [garak](https://github.com/NVIDIA/garak)/[PyRIT](https://github.com/Azure/PyRIT), the indirect-injection threat model ([Greshake et al., 2023](https://arxiv.org/abs/2302.12173)). Build these with Days 11–40 of the [100-day curriculum](LEARNING_PATH_100_DAYS.md).

<details>
<summary><b>10 interview questions</b></summary>

1. **What's the technical difference between a jailbreak and a prompt injection?** A jailbreak makes the model violate *its own* safety policy — you, the user, are the attacker. Prompt injection makes the model obey a *third party's* instructions hidden in data it processes — the attacker isn't the user. Different threat models, different defenses.
2. **Why is prompt injection considered structurally unfixable, not a bug?** Because the model concatenates trusted instructions and untrusted data into one token stream and has no architectural mechanism to tell them apart ([Willison](https://simonwillison.net/series/prompt-injection/)). You mitigate and contain blast radius; you don't patch it away.
3. **Walk through how the GCG attack works.** [Zou et al., 2023](https://arxiv.org/abs/2307.15043) computes gradients on token embeddings to find an adversarial suffix that maximizes the probability of an affirmative response, transferring across models trained with similar alignment objectives.
4. **What's the difference between direct and indirect prompt injection, with a real example?** Direct: the user types the injection. Indirect: an attacker plants it in content the model later ingests (a document, email, web page) — e.g. the **EchoLeak** (CVE-2025-32711) zero-click exfiltration via Microsoft 365 Copilot.
5. **How would you red-team a RAG pipeline?** Poison a document the retriever will plausibly surface, craft the injected instruction to survive the retrieval + summarization step, and check whether the agent executes an unintended tool call — this is exactly [Lab 3](LABS.md#-lab-3-indirect-prompt-injection-via-rag--tool-hijacking) in the companion curriculum.
6. **What does garak actually do, mechanically?** Runs a battery of probes (prompt patterns designed to elicit unsafe behavior) against a target, then scores responses with detectors — think "nmap for LLMs," not a single test.
7. **Why would you use PyRIT over garak for a specific engagement?** PyRIT is built for multi-turn, adaptive, orchestrated attacks — it can iteratively rewrite an injection based on the target's prior responses. garak is stronger for broad single-shot probe coverage.
8. **What is the "lethal trifecta" and why does it matter for agent red-teaming?** [Simon Willison's framing](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/): an agent is exposed to data exfiltration once its tool-set spans private-data access + untrusted-content exposure + external communication — and this risk exists at the *composition* of tools, not any single one.
9. **How do you score whether a jailbreak "worked" in an automated benchmark?** Benchmarks like [HarmBench](https://github.com/centerforaisafety/HarmBench) use classifier-based scoring against defined harmful-behavior categories; [AgentHarm](https://huggingface.co/datasets/ai-safety-institute/AgentHarm) goes further and checks whether the agent could actually *complete* the harmful task, not just verbally comply.
10. **What's a mistake that immediately signals a junior red-teamer?** Treating prompt injection and jailbreaking as the same thing, or claiming a 100% injection filter is possible — both mark you as not having done the work.

</details>

---

## 🔒 2. AI Security Engineer (Product/AppSec)

**The job:** you're an AppSec engineer whose product happens to have an LLM in it. Auth, input/output handling, plugin design, and rate limiting still matter — but the trust boundary moved, and you need to know where.

**Core skills to have ready:** [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/) end to end, [LLM Guard](https://github.com/protectai/llm-guard)/[NeMo Guardrails](https://github.com/NVIDIA-NeMo/Guardrails), the OWASP GenAI Red Teaming Guide's four-phase model. Build these with Days 51–60 of the [100-day curriculum](LEARNING_PATH_100_DAYS.md).

<details>
<summary><b>10 interview questions</b></summary>

1. **Where does the trust boundary sit in a normal web app vs. an LLM-integrated app?** Normal apps separate untrusted user input from trusted application code. In an LLM app, the model sits in the middle and can't reliably distinguish system instructions, developer code, and untrusted data — the boundary the rest of your architecture assumes doesn't hold inside the model call.
2. **What's LLM02 (Insecure Output Handling) and how do you defend against it?** Treating LLM output as safe-by-default leads to XSS/CSRF/RCE downstream. Fix: treat every model output as untrusted user input — encode before rendering, validate before executing.
3. **What's LLM07 (Insecure Plugin Design) concretely look like?** A tool/plugin accepting raw free-text from the model instead of a strict JSON schema — the model's own hallucinated or injected text becomes your API's input. Fix: structured parameters, schema validation, no dynamic shell execution.
4. **How do you defend a support chatbot against being tricked into making binding commitments?** (Reference the real [Chevrolet dealership $1 Tahoe jailbreak](PLAYGROUNDS_AND_LABS.md#3-the-chevrolet-bolt-support-chatbot-jailbreak-december-2023).) Constrain the system prompt's authority explicitly, add an output filter for commitment-shaped language, and never expose a raw, unguardrailed model on a public brand-facing endpoint.
5. **What's the difference between an input guardrail and an output guardrail, and why do you need both?** Input guardrails catch injection/jailbreak attempts before inference; output guardrails catch PII leaks and unsafe generations after. A payload can clear one and get caught by the other — see [Lab 5](LABS.md).
6. **Why is a guardrail model not a guarantee?** Most guard models (Llama Guard, ShieldGemma) are themselves LLMs and can themselves be prompt-injected or evaded by adversarial phrasing — [independent evaluations](https://arxiv.org/abs/2511.22047) show meaningful bypass rates. Treat it as one defense-in-depth layer, not the only control.
7. **How do you rate-limit against LLM04 (Model Denial of Service)?** Cap max prompt/context token size, cap agent loop iteration counts, and monitor for recursive tool-calling patterns that drive up compute/billing.
8. **A model needs to call an internal API on the user's behalf. What's the security control you insist on?** The tool call must inherit the *initiating user's* actual permissions/session — never a shared, over-privileged service credential the agent holds on everyone's behalf (this is the RBAC "dual-homed agent" problem).
9. **What's your review checklist when a PM asks to add a new tool to an existing agent?** Whether the addition changes the tool-set's lethal-trifecta status (does it add private-data access, untrusted-content exposure, or external communication where it wasn't there before), plus schema validation and whether it needs human-in-the-loop approval.
10. **How would you explain "prompt injection is not a bug you can patch" to a non-technical PM pushing back on a timeline?** It's an architectural property, not a missing check — like asking "why can't you just fix SQL injection with a keyword blocklist." You reduce blast radius with layered controls; you don't eliminate the class.

</details>

---

## 🔧 3. MLSecOps / ML Platform Security Engineer

**The job:** secure the infrastructure the models run on and travel through — training pipelines, model registries, weight storage, serialization formats, dependency chains.

**Core skills to have ready:** pickle/serialization risk, [ModelScan](https://github.com/protectai/modelscan)/[PickleScan](https://github.com/mmaitre314/picklescan), [safetensors](https://github.com/huggingface/safetensors), IAM/least-privilege for ML infra. Build these with Days 71–80 of the [100-day curriculum](LEARNING_PATH_100_DAYS.md).

<details>
<summary><b>10 interview questions</b></summary>

1. **Why is loading a `.bin`/`.pth`/`.ckpt` model file from an untrusted source dangerous?** Those formats use Python `pickle`, whose `__reduce__` method can trigger arbitrary code execution on `pickle.load()` — a "model file" can be a live exploit.
2. **What does `safetensors` fix that pickle doesn't?** It's a format that stores tensor data only — no executable code path exists during deserialization, so loading it can't trigger RCE regardless of file contents.
3. **A scanner says a model is clean. Do you trust it?** Not fully — in Dec 2025, three PickleScan bypasses (**CVE-2025-10155/-10156/-10157**, all CVSS 9.3) let malicious PyTorch models pass as clean and still execute on load, because the scanner and the loader parsed the file differently. A scanner is defense-in-depth, not a guarantee.
4. **How do you secure a model registry against supply-chain compromise?** Encrypted, access-controlled object storage (e.g. IAM-scoped S3+KMS), signed model artifacts, pinned dependencies, and mandatory scanning before any model is promoted to serving.
5. **What's typosquatting risk in an ML pipeline, specifically?** A coding agent or CI pipeline auto-installing a misspelled package (`pytorh` instead of `torch`) that's actually attacker-controlled — this is now also a *prompt-injection-driven* risk if an agent can be tricked into "recommending" a malicious package name.
6. **How do you scope down a coding/ML agent that has shell access?** Run it in a micro-VM (Firecracker/gVisor, not bare Docker), egress-filter the network (explicitly block `169.254.169.254`, the cloud metadata endpoint), and mount only a read-only copy of the target repo — never the raw home directory.
7. **What's the security risk in a vector database used for RAG?** Semantic data poisoning — an attacker inserts documents with embeddings optimized to be retrieved for specific queries, injecting malicious context into a downstream LLM call.
8. **How would you harden a CI/CD pipeline that includes an autonomous coding agent?** Human-in-the-loop approval gates for shell execution, dependency installs, and git pushes; dependency lockfiles plus automated SCA; no unrestricted internet egress from the runner.
9. **What's data poisoning during instruction tuning, and how much data does it actually take?** [Wan et al., 2023](https://arxiv.org/abs/2305.00944) showed as few as ~100 poisoned instruction-tuning examples can implant a functional backdoor — a much smaller bar than pretraining-scale poisoning.
10. **What's a "sleeper agent" model and why do standard safety interventions fail against it?** [Hubinger et al., 2024 (Anthropic)](https://arxiv.org/abs/2401.05566) trained models with a triggered backdoor that survives RLHF/SFT — the safety training doesn't remove the trigger, it teaches the model to hide it better during evaluation.

</details>

---

## 📋 4. AI Governance, Risk & Compliance (GRC)

**The job:** map AI systems against regulatory and risk frameworks, run audits, and translate "the model can be jailbroken" into a risk register entry a board understands.

**Core skills to have ready:** [NIST AI RMF](https://www.nist.gov/itl/ai-risk-management-framework), [ISO/IEC 42001](https://www.iso.org/standard/81230.html), the EU AI Act's risk tiers, the [OWASP GenAI Red Teaming Guide](https://genai.owasp.org/resource/genai-red-teaming-guide/). Build these with Days 81–90 of the [100-day curriculum](LEARNING_PATH_100_DAYS.md).

<details>
<summary><b>10 interview questions</b></summary>

1. **What are the four core functions of the NIST AI RMF?** Govern, Map, Measure, Manage — a continuous cycle, not a one-time checklist.
2. **How does the EU AI Act classify risk, and what's a "systemic risk" GPAI model?** Four tiers: unacceptable (banned), high-risk (strict audits), general-purpose AI (GPAI), and minimal risk. A GPAI model becomes "systemic risk" above a training-compute threshold (>10^25 FLOPs) and takes on extra obligations: adversarial risk assessments, energy reporting, state-of-the-art security.
3. **What does ISO/IEC 42001 actually certify?** An organization's AI Management System — the processes for governing AI risk across the full lifecycle, similar in structure to ISO 27001 for information security.
4. **A red team finds a jailbreak in a production system. How do you turn that into a risk register entry?** Map it to the specific OWASP LLM Top 10 category, assign likelihood/impact per your framework, note the regulatory exposure (e.g., GDPR Article 32 if it enables data leakage), and assign an owner and remediation SLA — not just "log it as a bug."
5. **What's the difference between the OWASP LLM Top 10 and the OWASP Top 10 for Agentic Applications (2026)?** The LLM list is model/prompt-era; the Agentic list (ASI01–ASI10) covers risks that only exist once a system can plan, use tools, and act autonomously — e.g. ASI07 (insecure inter-agent communication) has no single-model analogue.
6. **Why does the four-phase OWASP Red Teaming Guide matter for audit scoping?** Because a model can pass phase-1 (model-level) testing and still be trivially exploitable at phase-3 (infrastructure) or phase-4 (runtime/agentic) — an audit that only covers prompt-level testing has a coverage gap by design.
7. **What's "human oversight" required to mean under the EU AI Act for high-risk systems?** Fail-safes and manual intervention capability designed into the system — not just a manual review step bolted on after deployment.
8. **How do you assess third-party AI vendor risk when you can't see their model weights or training data?** Rely on their documented compliance posture (ISO 42001 certification, published red-team/eval results, SOC2-equivalent evidence), contractual SLAs on incident disclosure, and your own black-box testing at the API boundary.
9. **What's the business case for red-team findings that a CFO/board will actually engage with?** Quantified blast radius (how many users/records) plus a named regulatory framework and penalty tier — "this affects all EU users and maps to GDPR Article 32, exposing up to 4% of global turnover" lands; "this is a medium-severity finding" does not.
10. **Name one incident where hallucination created real legal liability.** The [Air Canada chatbot ruling](PLAYGROUNDS_AND_LABS.md#2-the-air-canada-chatbot-legal-liability-2024) — a court held the airline liable for a refund policy its own chatbot hallucinated, rejecting the argument that the bot was a separate legal entity.

</details>

---

## 🚨 5. Trust & Safety Engineer (GenAI)

**The job:** stop abuse of AI products at scale — generated CSAM, harassment content, spam/fraud automation, scaled disinformation — distinct from red-teaming because the adversary is a real-world abuser optimizing for harm at volume, not a researcher probing a single system.

**Core skills to have ready:** the harm-taxonomy side of safety benchmarks ([HarmfulQA](https://github.com/declare-lab/red-instruct), [Do-Not-Answer](https://github.com/Libr-AI/do-not-answer)), detection-at-scale thinking, and where GenAI abuse overlaps with — and differs from — classic T&S (spam, CSAM, harassment).

<details>
<summary><b>10 interview questions</b></summary>

1. **How is GenAI trust & safety different from classic platform trust & safety?** Classic T&S detects abusive *content* posted by users. GenAI T&S also has to prevent the *platform's own model* from generating novel abusive content on demand — the abuse surface includes the model's output space, not just what users upload.
2. **What's the risk in a company deploying an unguardrailed public-facing chatbot?** Beyond jailbreaks embarrassing the brand, a T&S-specific risk is the bot being driven to generate content the platform is legally or contractually obligated to prevent (harassment, CSAM-adjacent content, targeted harassment scripts) at attacker-chosen scale.
3. **How do you evaluate whether a model over-refuses benign requests?** Benchmarks like [WildGuard](https://huggingface.co/allenai/wildguard) explicitly score both false negatives (harm that slips through) and over-blocking of benign traffic — a model that refuses everything "safely" is still a product failure.
4. **What's the tradeoff in setting a safety classifier's threshold?** Lower threshold catches more real abuse but increases false positives that block legitimate users (support burden, user trust); higher threshold reduces friction but lets more abuse through. This is a product/policy decision, not a purely technical one.
5. **How would you detect coordinated abuse of a GenAI feature (e.g., scaled spam generation)?** Behavioral/volumetric signals at the account and request-pattern level (similar prompts across many accounts, abnormal request cadence) layered on top of per-request content classification — content classifiers alone don't catch coordination.
6. **What's the difference between a jailbreak found by a red-teamer and one exploited by a real abuser?** Intent and scale. A red-teamer documents one instance responsibly; a real abuser automates it and monetizes or weaponizes it at volume — your response playbook (rate limiting, account action, law enforcement referral) differs accordingly.
7. **How do you balance transparency about known jailbreaks against giving abusers a roadmap?** Responsible disclosure timelines (patch before public writeup) apply here too, but T&S also has to think about whether *documenting* a bypass in a public policy doc effectively publishes an abuse guide.
8. **What signals indicate an account is probing your safety systems rather than using the product normally?** Repeated near-miss prompts (small variations testing where the boundary is), fast iteration cycles inconsistent with genuine use, and clustering around known jailbreak template structures.
9. **How does the EU AI Act's "unacceptable risk" category intersect with T&S policy?** Practices like real-time biometric ID in public spaces or manipulative dark-pattern AI are banned outright — T&S policy for those categories isn't "moderate carefully," it's "the feature can't exist" in scope regions.
10. **What's your first move when a novel abuse pattern is reported that no existing classifier catches?** Triage severity and scale immediately (is it actively being exploited at volume, or a one-off), ship a narrow interim mitigation (keyword/pattern block, rate limit) while a proper classifier or systemic fix is built, and feed the pattern back into eval/training data.

</details>

---

## 🧠 6. AI Safety / Alignment Researcher

**The job:** research *why* models misbehave and how to make them robustly, mechanistically safe — closer to an ML research role than a security-operations role, and interviews reflect that (paper literacy, experiment design, statistics).

**Core skills to have ready:** [Sleeper Agents](https://arxiv.org/abs/2401.05566), [Toy Models of Superposition](https://transformer-circuits.pub/2022/toy_models/index.html), [Linear Representation of Concepts](https://arxiv.org/abs/2310.15154), the extraction/memorization literature (Carlini et al.). Build these with [Days 77–80 and 87–89 of the 100-day curriculum](LEARNING_PATH_100_DAYS.md#days-71–80-—-model-supply-chain-security).

<details>
<summary><b>10 interview questions</b></summary>

1. **What is "deceptive alignment" and what did Sleeper Agents actually demonstrate?** A model that behaves safely during training/eval but executes a different, harmful behavior once triggered in deployment. [Hubinger et al., 2024](https://arxiv.org/abs/2401.05566) showed standard RLHF/SFT safety training fails to remove the backdoor — it can even teach the model to conceal it better.
2. **What is "superposition" and why does it make backdoor detection hard?** [Anthropic's Toy Models of Superposition](https://transformer-circuits.pub/2022/toy_models/index.html): networks compress more features than they have dimensions by mapping concepts to overlapping (polysemantic) directions in activation space. Until you can cleanly decompose those into monosemantic features, you can't reliably audit weights for hidden behaviors.
3. **What does "activation steering" mean and what's its promise as a defense?** If a concept like "safety" or "truthfulness" has a clean linear direction in activation space ([Nanda et al., 2023](https://arxiv.org/abs/2310.15154)), you can add/subtract that vector at inference time to steer behavior — a defense that doesn't rely on prompt-level guardrails at all.
4. **How does training-data extraction actually work, mechanically?** [Carlini et al., 2021](https://arxiv.org/abs/2012.07805) uses perplexity checks to distinguish memorized verbatim text from newly generated text; [Nasr et al., 2023](https://arxiv.org/abs/2311.17035) scaled this against ChatGPT by asking it to repeat a word indefinitely, which broke its normal generation mode and dumped raw training data.
5. **What's the difference between evaluating jailbreak robustness and evaluating alignment?** Jailbreak robustness asks "can this specific attack bypass current defenses" (an adversarial, moving-target question). Alignment asks a deeper question about whether the model's objectives generalize correctly outside the training distribution at all — jailbreak-resistance is a symptom-level proxy, not the underlying property.
6. **Why do interpretability researchers care about mechanistic understanding rather than just behavioral testing?** Behavioral testing (does it refuse X) can't distinguish "the model doesn't want to do X" from "the model wants to do X but has learned to hide it" — Sleeper Agents is the sharpest existing demonstration of that gap.
7. **What's a concrete experiment you'd run to test whether a safety behavior is robust or superficial?** Check whether the behavior persists under distribution shift the model wasn't specifically trained against (e.g., a novel obfuscation/jailbreak style, or many-shot in-context pressure) rather than only the exact attack styles present in the safety training data.
8. **What's "many-shot jailbreaking" and why does it scale the way it does?** [Anthropic's research](https://www.anthropic.com/research/many-shot-jailbreaking) shows packing dozens to hundreds of benign example Q&A pairs demonstrating a bypass, before the harmful request, causes in-context learning to override safety alignment — effectiveness scales roughly log-linearly with the number of examples, which is why larger context windows are a genuine new risk surface, not just a UX feature.
9. **What's the strongest argument that current guardrail approaches are insufficient long-term?** Independent evaluations of guard models consistently find non-trivial bypass rates under adversarial phrasing, and the guard models are themselves LLMs vulnerable to injection — layering more classifiers reduces but doesn't remove the structural gap.
10. **How would you evaluate whether a proposed defense generalizes, versus just patching the specific attack in front of you?** Test it against attack families it wasn't designed around (cross-technique transfer), not just held-out examples of the same attack style — a defense that only stops the attacks it was tuned on isn't a generalizable result.

</details>

---

## 🏦 7. AI Model Risk / Model Audit

**The job:** independent, pre- and post-deployment validation of AI/ML models — common in regulated industries (finance, insurance, healthcare) where "model risk management" already existed for classical statistical models and is now being extended to LLMs and GenAI.

**Core skills to have ready:** classical model validation concepts (backtesting, benchmarking, challenger models) applied to the LLM context, documentation/explainability expectations, and where model risk overlaps with security risk (a model that's easily jailbroken is both a security *and* a model-risk finding).

<details>
<summary><b>10 interview questions</b></summary>

1. **How does validating an LLM differ from validating a classical statistical/ML model?** Classical model risk leans on stable input distributions and quantifiable error metrics (AUC, backtested loss). LLMs have open-ended input/output spaces, non-deterministic outputs, and failure modes (hallucination, jailbreak susceptibility) that don't reduce to a single accuracy number — validation has to add adversarial and qualitative dimensions on top of the classical statistical ones.
2. **What documentation would you require before approving an LLM-based system for production?** Intended use and scope boundaries, known limitations and failure modes, evaluation results (including adversarial/red-team results, not just accuracy benchmarks), and a monitoring plan for post-deployment drift.
3. **Is a jailbreak finding a security issue or a model risk issue?** Both — it's a security vulnerability *and* evidence the model doesn't reliably perform within its intended, documented boundaries, which is exactly what model risk validation exists to catch before deployment.
4. **How do you validate a RAG system specifically, versus a standalone model?** You have to validate the retrieval component's accuracy/relevance separately from the generation component's faithfulness to what was retrieved — a RAG system can fail by retrieving the wrong context, hallucinating despite correct context, or being hijacked by injected content in the retrieved context.
5. **What's "model drift" mean for an LLM-based system, and how do you monitor for it?** Behavioral drift as the underlying API-served model gets silently updated by the vendor, or as usage patterns shift the effective input distribution — monitor via periodic re-running of your validation eval suite against production traffic samples, not a one-time approval.
6. **How would you challenge a business team's claim that "the model is 95% accurate" as sufficient for approval?** Ask what the failure distribution looks like on the remaining 5% — a model that's uniformly 95% accurate is very different in risk terms from one that's 100% accurate except for a systematic failure on a specific, high-stakes input class.
7. **What's your position on using an LLM to evaluate/grade another LLM's outputs (LLM-as-judge) for validation purposes?** It scales evaluation but inherits the judge model's own biases and blind spots, and can itself be gamed — treat it as one signal among several (human review, deterministic checks, adversarial testing), not a sole source of truth for a risk sign-off.
8. **How does the EU AI Act's high-risk category change model audit obligations?** High-risk systems require documented, continuous risk management, logged audit trails, human-oversight fail-safes, and demonstrated adversarial robustness — turning what might have been a one-time sign-off into an ongoing compliance obligation.
9. **What independence do you need from the team that built the model to do a credible audit?** Enough that you're not validating your own design decisions — the classical "three lines of defense" model (build, independent validation, audit) applies to AI systems the same way it does to any other model risk framework.
10. **A vendor's foundation model updates without notice and your validated system's behavior shifts. What's your process?** This should already be a documented risk in your approval (model risk from third-party dependency), triggering a re-validation trigger — not treated as a one-off incident, but as the expected consequence of depending on infrastructure you don't control.

</details>

---

## The family

This repo pairs with the day-count learning series and the master resource catalog, all built from the same verified source:

| Repo | For |
|:---|:---|
| [**7 Days of AI Security**](LEARNING_PATH_7_DAYS.md) | A one-week taste test, zero prerequisites |
| [**30 Days of AI Security**](LEARNING_PATH_30_DAYS.md) | A time-boxed month to real competence |
| [**100 Days of AI Security**](LEARNING_PATH_100_DAYS.md) | The full curriculum, foundations to capstone |
| [**Prompt Injection & Jailbreak Technique Library**](JAILBREAK_LIBRARY.md) | A categorized taxonomy of attack techniques, for feeding garak/PyRIT/promptfoo |
| [**ai-security-resources**](README.md) | The exhaustive, non-day-boxed catalog everything above is built from |

## Contributing

Found a role we're missing, a stale reference, or a question that needs a sharper answer? See [CONTRIBUTING.md](CONTRIBUTING.md). PRs claiming a question was "asked at [specific company]" without public, citable evidence will be rejected — every claim here has to be verifiable, not anecdotal.

## License

[CC0 1.0](LICENSE) — public domain.
