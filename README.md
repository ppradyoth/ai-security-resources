# AI Security Resources

A premium, curated directory of state-of-the-art **Adversarial AI Security** tools, vulnerability scanners, academic benchmarks, safety datasets, enterprise guardrails, and governance frameworks.

Designed for AI Red Teamers, Security Engineers, and Machine Learning researchers working at the frontier of LLM safety and model robustness.

---

## 🗺️ Navigation Directory

*   [🛠️ Automated Vulnerability Scanners & Attack Tools](#%EF%B8%8F-automated-vulnerability-scanners--attack-tools)
*   [📊 Academic Benchmarks & Datasets](#-academic-benchmarks--datasets)
*   [🛡️ Input/Output Guardrails & Defense Tools](#%EF%B8%8F-inputoutput-guardrails--defense-tools)
*   [📚 Standards, Governance & Frameworks](#-standards-governance--frameworks)
*   [📝 Key SOTA Research & Reading List](#-key-sota-research--reading-list)
*   [🤝 How to Contribute](#-how-to-contribute)

---

## 🛠️ Automated Vulnerability Scanners & Attack Tools

These frameworks automate prompt injection, jailbreaking, data exfiltration, and adversarial evaluations across enterprise LLMs.

| Tool | Creator | Description | Key Features |
| :--- | :--- | :--- | :--- |
| **[NVIDIA/garak](https://github.com/NVIDIA/garak)** | NVIDIA | The industry-standard vulnerability scanner for LLMs (like Nmap for AI). | Probes for prompt injection, jailbreaks, data leakage, hallucinations, and toxicity. |
| **[confident-ai/deepteam](https://github.com/confident-ai/deepteam)** | Confident AI | An open-source automated red teaming framework. | Multi-turn/agentic attacks, 50+ vulnerabilities, 20+ attack vectors aligned to OWASP & NIST. |
| **[artkit-ai/artkit](https://github.com/artkit-ai/artkit)** | ARTKIT | Focuses on multi-turn, agentic red teaming simulations. | Simulates realistic attacker-target conversations; great for complex agentic workflows. |
| **[microsoft/PyRIT](https://github.com/microsoft/PyRIT)** | Microsoft | Python Risk Identification Tool for proactive red teaming. | Integrates with Azure AI; automates model probing, intent drift identification, and red team scaling. |
| **[promptfoo](https://github.com/promptfoo/promptfoo)** | Promptfoo | Evaluates LLM outputs against security, accuracy, and latency. | Built for CI/CD; automated prompt injection tests, custom assertions, and model comparisons. |
| **[deadbits/vigil](https://github.com/deadbits/vigil)** | Deadbits | Real-time prompt injection and jailbreak detection API. | Alpha library integrating vector databases, heuristics, and classification to block injections. |

---

## 📊 Academic Benchmarks & Datasets

Standardized datasets and benchmarks used by researchers to measure adversarial robustness and model safety quantitatively.

### 🌟 Featured Benchmark Aggregator
*   **[RedBench (RedEval)](https://github.com/knoveleng/redeval)**: A "universal" repository that aggregates **37 leading safety benchmarks** (such as HarmBench, AdvBench, and Do-Not-Answer) into a standardized taxonomy spanning 22 risk categories. Perfect for running holistic evaluations.

### 📚 Core Datasets & Benchmarks
*   **[HarmBench](https://github.com/causal-llm/HarmBench)**: A unified evaluation framework designed to standardize the measurement of LLM adversarial robustness, calculating precise Attack Success Rates (ASR).
*   **[Anthropic hh-rlhf](https://huggingface.co/datasets/Anthropic/hh-rlhf)**: Foundational dataset consisting of thousands of human-preference and red-team conversations designed to elicit unsafe behaviors to improve RLHF alignment.
*   **[DecodingTrust](https://github.com/AI-Secure/DecodingTrust)**: A comprehensive trust evaluation framework covering eight dimensions: toxicity, bias, fairness, robustness, out-of-distribution, adversarial robustness, privacy, and machine ethics.
*   **[WildJailbreak / WildGuardMix](https://allenai.org/) (Allen Institute for AI)**: 
    *   *WildJailbreak*: One of the largest public safety datasets containing **262,000 training examples** for adversarial robustness.
    *   *WildGuardMix*: A **92,000 example** multi-task dataset for evaluating input/output guardrails and moderation models.
*   **[RealToxicityPrompts](https://github.com/allenai/real-toxicity-prompts)**: Standard dataset of 100k prompts used to evaluate the propensity of language models to generate toxic content under varying conditions.

---

## 🛡️ Input/Output Guardrails & Defense Tools

Enterprise-grade software and middleware deployed in front of production LLMs to block attacks and monitor real-time activity.

*   **[NVIDIA/NeMo-Guardrails](https://github.com/NVIDIA/NeMo-Guardrails)**: An open-source toolkit to build safe and trustworthy LLM conversational systems. Allows developers to define semantic rails for topic constraints, safety, and security.
*   **[protectai/llm-guard](https://github.com/protectai/llm-guard)**: An advanced real-time scanner for LLM inputs and outputs. Detects prompt injections, jailbreaks, PII leaks, toxicity, and hallucinations.
*   **[Lakera Guard](https://github.com/lakeraai)**: Enterprise-grade developer API designed to intercept and defend against prompt injections and jailbreaks in milliseconds.
*   **[LangKit / whylogs](https://github.com/whylabs/langkit)**: Open-source telemetry library for monitoring LLM behavior, providing real-time metrics on toxicity, relevance, readability, and security drift.

---

## 📚 Standards, Governance & Frameworks

Industry-recognized compliance frameworks and threat-modeling matrixes mapping adversarial strategies.

*   **[OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)**: The definitive industry standard detailing the ten most critical vulnerabilities in LLM integrations (e.g., Insecure Output Handling, Prompt Injection, Model Theft).
*   **[MITRE ATLAS (Adversarial Threat Landscape for Artificial-Intelligence Systems)](https://atlas.mitre.org/)**: A globally accessible knowledge base of adversary tactics, techniques, and case studies against real-world AI systems, modeled after MITRE ATT&CK.
*   **[NIST Artificial Intelligence Risk Management Framework (AI RMF)](https://www.nist.gov/itl/ai-risk-management-framework)**: A highly structured framework providing guidelines to govern, map, measure, and manage risks associated with deploying generative and traditional AI.

---

## 📝 Key SOTA Research & Reading List

Essential reading to understand the mechanics of jailbreaks, prompt injection, and model alignment.

1.  **"Universal and Transferable Adversarial Attacks on Aligned Language Models"** (Zou et al., 2023)
    *   *Significance*: Discovered that suffix tokens (`!!! ...`) can bypass LLM safety alignment across open and closed-source models using automated optimization.
2.  **"HarmBench: A Standardized Evaluation Framework for Automated Adversarial Robustness and Red Teaming"** (Mazeika et al., 2024)
    *   *Significance*: Standardized how safety red-teaming metrics are calculated, introducing robust defense evaluations.
3.  **"DecodingTrust: A Holistic Assessment of Trustworthiness in GPT Models"** (Wang et al., 2023)
    *   *Significance*: An extensive audit of GPT-3.5 and GPT-4 safety properties, highlighting major vulnerabilities in privacy and adversarial robustness.
4.  **"Jailbroken: How Aligned Language Models Can Be Bypassed"** (Wei et al., 2023)
    *   *Significance*: Categorizes jailbreak mechanisms into two primary principles: competing objectives (utility vs. safety) and mismatch generalization.

---

## 👥 Contributors & Acknowledgements

*   **[@ppradyoth](https://github.com/ppradyoth)** (Lead Maintainer) — AI Red Teaming & Security Engineering.
*   **[Antigravity 🌌](https://github.com/google-deepmind)** (AI Co-Architect) — Agentic coding assistant developed by Google DeepMind.

---

## 🤝 How to Contribute

We welcome contributions from security engineers, adversarial researchers, and open-source advocates! 

1.  Fork this repository.
2.  Add a new tool, dataset, benchmark, or research paper (with a brief description and link).
3.  Ensure formatting remains visually clean and alphabetically ordered.
4.  Submit a Pull Request with a descriptive summary of your additions.

***

*Maintained by [@ppradyoth](https://github.com/ppradyoth). Built to secure the future of Generative AI.*
