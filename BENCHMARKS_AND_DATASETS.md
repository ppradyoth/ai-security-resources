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

#### [AgentDojo](https://github.com/nareshr/agentdojo)
*   **Purpose**: A benchmark suite designed specifically for evaluating security vulnerabilities in **Agentic LLMs** (agents that can invoke tools, search databases, and execute code).
*   **Why it matters**: It focuses heavily on **Indirect Prompt Injection** scenarios, measuring how easily a rogue email or web page can hijack an agent, force it to abuse its available tools, leak user data, or perform unauthorized transactions.

#### [Harmful-QA](https://github.com/poloclub/harmful-qa)
*   **Purpose**: A benchmark designed to evaluate model alignment and response filters across diverse categories of sensitive and dangerous questions.
*   **Why it matters**: It contains questions spanning topics like cyber-sabotage, biological threats, financial fraud, and physical harm, scoring models on how safely they refuse requests without being overly sensitive to benign inputs.

#### [FigStep](https://github.com/Visual-Adversarial-ML/FigStep)
*   **Purpose**: A benchmark evaluating **Multimodal LLM (VLM)** security against visual jailbreaks.
*   **Why it matters**: Many vision-language models can block textual jailbreaks but fail when the adversarial instruction is printed as text inside an image file and uploaded to the model. FigStep measures multimodal safety alignment.

---

## 🗄️ Adversarial Datasets & Attack Repositories

Datasets are collections of historical jailbreaks, prompt injections, poisoned data samples, and extraction payloads used to train safety filters or evaluate model vulnerability.

### 🔓 Prompt Injection & Jailbreak Datasets

#### [Jailbreak-LLM-Dataset](https://github.com/verazuo/jailbreak-LLM)
*   **Purpose**: A collection of thousands of historical jailbreak prompts scraped from online forums, Reddit (`r/ChatGPT`), and academic papers.
*   **Why it matters**: It contains diverse jailbreak styles (role-play, character splits, translator modes, hypothetical scenarios, virtual environments). Excellent for training security classifiers or populating input-filter databases.

#### [HackAPrompt Dataset](https://huggingface.co/datasets/hackaprompt/hackaprompt-dataset)
*   **Purpose**: The official dataset generated from the global *HackAPrompt* security competition.
*   **Why it matters**: It contains over 600,000 diverse prompt injection payloads submitted by security researchers and hackers attempting to bypass safety rules. It represents one of the largest empirical datasets of real-world prompt injection attempts in existence.

#### [TensorTrust Dataset](https://tensortrust.ai/posts/announcing-the-dataset/)
*   **Purpose**: Crowdsourced dataset of prompt injection attacks and defenses generated from the *TensorTrust* interactive security game.
*   **Why it matters**: It contains both attack prompts (designed to bypass constraints and extract secret keys) and defense system prompts (written by players to protect keys). Highly useful for analyzing the real-world dynamics of prompt-level security engineering.

#### [Do-Not-Answer Dataset](https://github.com/LianminZheng/do-not-answer)
*   **Purpose**: A dataset of 936 carefully curated questions that models should refuse to answer based on standard AI safety guidelines.
*   **Why it matters**: Organized across distinct risk categories (e.g., identity theft, cyberattacks, self-harm, harassment), it serves as a high-quality test set to evaluate if a model's safety alignment matches modern compliance baseline standards.

### 🧪 Data Poisoning & Backdoor Datasets

#### [Trojan Detection Challenge Dataset (TDC)](https://trojandetection.ai/)
*   **Purpose**: Standardized datasets generated for the annual Trojan Detection Challenge, aimed at identifying hidden backdoors (trojans) in deep learning models.
*   **Why it matters**: It contains neural networks with pre-injected backdoors alongside standard clean networks. Useful for researchers developing tools to scan model weights and detect hidden triggers.

---

## 🛠️ Automated Evaluation & Safety Testing Toolkits

These are the operational toolkits that security engineers can run locally or in CI/CD pipelines to systematically evaluate models against these benchmarks.

*   **[garak](https://github.com/leondz/garak)**: The "nmap for LLMs". Automatically runs hundreds of attack modules (prompt injection, jailbreak, data leakage, toxicity) and scores the target model's failure rates.
*   **[PyRIT (Python Risk Identification Tool)](https://github.com/Azure/PyRIT)**: Microsoft's open-source framework for red teaming generative AI systems. Allows researchers to orchestrate multi-turn, adaptive adversarial conversations.
*   **[CyberSecEval Toolkit](https://github.com/meta-llama/PurpleLlama/tree/main/CyberSecEval)**: Meta's testing framework to locally run the CyberSecEval benchmark on any custom-trained LLM.
*   **[PromptArmor Evaluator](https://github.com/PromptArmor/evaluator)**: Focused on testing indirect prompt injection vulnerabilities inside RAG (Retrieval-Augmented Generation) pipelines and search integrations.
*   **[Inspect AI](https://github.com/UKGovernmentBEIS/inspect)**: Developed by the UK government's AI Safety Institute, a highly scalable, robust framework for evaluating model capabilities, cybersecurity risks, and autonomous capabilities.
