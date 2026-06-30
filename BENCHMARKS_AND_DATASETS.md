# AI Security Evaluation Benchmarks & Adversarial Datasets

To build robust defenses, security teams must be able to scientifically measure model vulnerability. This requires utilizing standardized datasets and robustness benchmarks that stress-test models against prompt injections, jailbreaks, data poisoning, and model extraction.

This document collects and maps the most high-signal datasets, evaluation benchmarks, and testing frameworks in AI security.

---

## 📊 Adversarial Robustness & Safety Benchmarks

Benchmarks are automated frameworks that systematically query models with thousands of adversarial prompts to score their safety, alignment, and robustness against exploits.

### 🛡️ Core Safety & Robustness Benchmarks

#### [HarmBench](https://github.com/centerforaisafety/HarmBench)
*   **Purpose**: A standardized, unified benchmark designed to evaluate LLM safety and the effectiveness of adversarial attacks (jailbreaks) and defenses.
*   **Why it matters**: It provides a highly rigorous framework to test how well a model can resist state-of-the-art jailbreak algorithms (such as GCG, AutoDAN, PAIR) across hundreds of harmful behaviors, yielding consistent, comparable safety scores.

#### [AdvGLUE](https://adversarialglue.github.io/)
*   **Purpose**: An adversarial version of the famous GLUE (General Language Understanding Evaluation) benchmark. It evaluates the robustness of NLP models under diverse adversarial attacks.
*   **Why it matters**: It injects word-level, sentence-level, and character-level perturbations into text inputs. A robust model should still maintain high accuracy; AdvGLUE exposes how easily standard LLMs can be tricked into incorrect classification or reasoning by tiny, human-imperceptible input modifications.

#### [CyberSecEval (by Meta AI)](https://github.com/meta-llama/PurpleLlama/tree/main/CyberSecEval)
*   **Purpose**: A massive cybersecurity evaluation benchmark suite developed under Meta’s PurpleLlama project.
*   **Why it matters**: It evaluates models on four critical cyber-safety vectors:
    1.  **Exploit Generation**: Does the model generate functional code exploits?
    2.  **Vulnerability Insecurity**: Does the model suggest insecure code with known vulnerabilities?
    3.  **Cyberattack Aid**: Does the model assist in planning or executing active cyberattacks?
    4.  **Jailbreak Vulnerability**: How easily can the model be coerced into assisting with cyberattacks under adversarial prompts?

#### [AgentDojo](https://github.com/ethz-spylab/agentdojo)
*   **Purpose**: A benchmark suite (ETH Zurich SPY Lab; NeurIPS 2024 Datasets & Benchmarks, [arXiv:2406.13352](https://arxiv.org/abs/2406.13352)) designed specifically for evaluating security vulnerabilities in **Agentic LLMs** (agents that can invoke tools, search databases, and execute code). Ships **97 realistic user tasks and 629 security test cases** across four domains (banking, Slack, travel, workspace).
*   **Why it matters**: It focuses heavily on **Indirect Prompt Injection** scenarios, measuring how easily a rogue email or web page can hijack an agent, force it to abuse its available tools, leak user data, or perform unauthorized transactions. Crucially, it scores results via **formal utility checks over environment state** (not an LLM judge), so runs are reproducible, and it is explicitly extensible with new tasks/attacks/defenses. Public leaderboard at [agentdojo.spylab.ai](https://agentdojo.spylab.ai).

#### [InjecAgent](https://github.com/uiuc-kang-lab/InjecAgent)
*   **Purpose**: The earlier tool-call-centric benchmark for **indirect prompt injection** in tool-integrated LLM agents (UIUC; ACL 2024 Findings, [arXiv:2403.02691](https://arxiv.org/abs/2403.02691)). Comprises **1,054 test cases** across **17 user tools and 62 attacker tools**.
*   **Why it matters**: It splits attacker goals into **direct-harm** vs. **data-stealing** attacks, giving a clean taxonomy of what an injected instruction is *trying to do* through an agent's tools. Pairs with AgentDojo to triangulate the agentic-injection threat model.

#### [HarmfulQA](https://github.com/declare-lab/red-instruct)
*   **Purpose**: A ChatGPT-distilled safety benchmark and alignment dataset (declare-lab; [arXiv:2308.09662](https://arxiv.org/abs/2308.09662)) of **1,960 harmful questions** across 10 topics, built with the Chain-of-Utterances prompt. Dataset on [Hugging Face](https://huggingface.co/datasets/declare-lab/HarmfulQA).
*   **Why it matters**: It evaluates model alignment and refusal behavior across diverse categories of sensitive and dangerous questions, scoring how safely a model refuses without being overly sensitive to benign inputs.

#### [FigStep](https://github.com/ThuCCSLab/FigStep)
*   **Purpose**: A benchmark evaluating **Multimodal LLM (VLM)** security against visual jailbreaks.
*   **Why it matters**: Many vision-language models can block textual jailbreaks but fail when the adversarial instruction is printed as text inside an image file and uploaded to the model. FigStep measures multimodal safety alignment ([arXiv:2311.05608](https://arxiv.org/abs/2311.05608), AAAI 2025 Oral).

---

## 🗄️ Adversarial Datasets & Attack Repositories

Datasets are collections of historical jailbreaks, prompt injections, poisoned data samples, and extraction payloads used to train safety filters or evaluate model vulnerability.

### 🔓 Prompt Injection & Jailbreak Datasets

#### [In-The-Wild Jailbreak Prompts (`jailbreak_llms`)](https://github.com/verazuo/jailbreak_llms)
*   **Purpose**: The dataset behind *"Do Anything Now": Characterizing and Evaluating In-The-Wild Jailbreak Prompts on LLMs* (ACM CCS 2024). **15,140 prompts** collected Dec 2022–Dec 2023 from Reddit, Discord, websites, and open-source datasets — including **1,405 labeled jailbreak prompts**.
*   **Why it matters**: It contains diverse jailbreak styles (role-play, character splits, translator modes, hypothetical scenarios, virtual environments). Excellent for training security classifiers or populating input-filter databases.

#### [HackAPrompt Dataset](https://huggingface.co/datasets/hackaprompt/hackaprompt-dataset)
*   **Purpose**: The official dataset generated from the global *HackAPrompt* security competition.
*   **Why it matters**: It contains over 600,000 diverse prompt injection payloads submitted by security researchers and hackers attempting to bypass safety rules. It represents one of the largest empirical datasets of real-world prompt injection attempts in existence.

#### [TensorTrust Dataset](https://tensortrust.ai/posts/announcing-the-dataset/)
*   **Purpose**: Crowdsourced dataset of prompt injection attacks and defenses generated from the *TensorTrust* interactive security game.
*   **Why it matters**: It contains both attack prompts (designed to bypass constraints and extract secret keys) and defense system prompts (written by players to protect keys). Highly useful for analyzing the real-world dynamics of prompt-level security engineering.

#### [Do-Not-Answer Dataset](https://github.com/Libr-AI/do-not-answer)
*   **Purpose**: A dataset of 939 carefully curated questions that models should refuse to answer based on standard AI safety guidelines (LibrAI).
*   **Why it matters**: Organized across distinct risk categories (e.g., identity theft, cyberattacks, self-harm, harassment), it serves as a high-quality test set to evaluate if a model's safety alignment matches modern compliance baseline standards.

### 🧪 Data Poisoning & Backdoor Datasets

#### [Trojan Detection Challenge Dataset (TDC)](https://trojandetection.ai/)
*   **Purpose**: Standardized datasets generated for the annual Trojan Detection Challenge, aimed at identifying hidden backdoors (trojans) in deep learning models.
*   **Why it matters**: It contains neural networks with pre-injected backdoors alongside standard clean networks. Useful for researchers developing tools to scan model weights and detect hidden triggers.

---

## 🛠️ Automated Evaluation & Safety Testing Toolkits

These are the operational toolkits that security engineers can run locally or in CI/CD pipelines to systematically evaluate models against these benchmarks.

*   **[garak](https://github.com/NVIDIA/garak)**: The "nmap for LLMs" (now maintained under NVIDIA). Automatically runs hundreds of attack modules (prompt injection, jailbreak, data leakage, toxicity) and scores the target model's failure rates.
*   **[PyRIT (Python Risk Identification Tool)](https://github.com/Azure/PyRIT)**: Microsoft's open-source framework for red teaming generative AI systems. Allows researchers to orchestrate multi-turn, adaptive adversarial conversations.
*   **[CyberSecEval Toolkit](https://github.com/meta-llama/PurpleLlama/tree/main/CyberSecEval)**: Meta's testing framework to locally run the CyberSecEval benchmark on any custom-trained LLM.
*   **[promptfoo](https://github.com/promptfoo/promptfoo)**: Open-source (MIT) CLI/library for evaluating and **red-teaming** LLM apps, agents, and RAG pipelines. Auto-generates adversarial probes via 50+ attack plugins (prompt injection, jailbreaks, PII leakage, excessive agency) with first-class CI/CD integration and OWASP LLM Top 10 / NIST AI RMF / MITRE ATLAS report mappings.
*   **[AgentDojo](https://github.com/ethz-spylab/agentdojo)**: Run the agentic indirect-prompt-injection benchmark above locally — pip-installable, with formal utility scoring and a pluggable attack/defense interface (see the Benchmarks section for details).
*   **[Inspect AI](https://github.com/UKGovernmentBEIS/inspect_ai)**: Developed by the UK government's AI Security Institute (AISI), a highly scalable, robust framework for evaluating model capabilities, cybersecurity risks, and autonomous capabilities. Companion eval collection: [`inspect_evals`](https://github.com/UKGovernmentBEIS/inspect_evals).

---

> **Link integrity:** Every repository link on this page is verified against its canonical source. Last full verification pass: **2026-06-30** — corrected the AgentDojo, garak, Inspect AI, Do-Not-Answer, HarmfulQA, FigStep, and `jailbreak_llms` links to their canonical repositories, and added InjecAgent. Spot a stale or moved link? [Open an issue or PR](CONTRIBUTING.md).
>
> Maintained with [Claude Code](https://claude.ai/code) — see the [autonomous agent experiment](https://github.com/ppradyoth/social-experiment-with-agents).
