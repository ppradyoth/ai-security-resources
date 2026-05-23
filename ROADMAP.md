# 🚀 Zero to Hero in AI Security: The Definitive Learning Curriculum

This curriculum is designed for software engineers, security practitioners, and machine learning researchers with at least 3 years of technical experience who want to become formidable, high-end AI Security specialists. 

Rather than teaching you how to run other people's scripts, this roadmap focuses on **architectural fundamentals, mathematical intuition, and custom exploit engineering**.

---

## 🗺️ Curriculum Overview

```
 ┌─────────────────────────────────────────────────────────────┐
 │  PHASE 0: ARCHITECTURAL FOUNDATIONS (Weeks 1–4)             │
 │  Mechanistic understanding of Transformers, FGSM, & Safety  │
 ├─────────────────────────────────────────────────────────────┤
 │  PHASE 1: OFFENSIVE AI SECURITY (Weeks 5–8)                 │
 │  Prompt hacking, RAG injections, garak, PyRIT, & Exploits  │
 ├─────────────────────────────────────────────────────────────┤
 │  PHASE 2: DEFENSIVE ENGINEERING (Weeks 9–12)                │
 │  Guardrails, LangKit, anomaly detection, weight security     │
 ├─────────────────────────────────────────────────────────────┤
 │  PHASE 3: ADVANCED SPECIALIZATION (Months 4–6)              │
 │  Research (Interpretability) vs Enterprise vs Offensive Lab │
 └─────────────────────────────────────────────────────────────┘
```

---

## 🏗️ Phase 0: Architectural Foundations (Weeks 1–4)
*Goal: Understand what you are attacking and defending at a mechanistic, mathematical level.*

### Week 1–2: How Transformers Work (Under the Hood)
Do not treat deep learning models as magic black boxes. To break them, you must understand their exact mathematical mechanics.

*   **Core Concepts to Learn**: Backpropagation, high-dimensional vector spaces, dot-product attention, positional encodings, multi-head attention, layer normalization, tokenization (BPE).
*   **Actionable Assignments**:
    1.  **Watch**: [3Blue1Brown Neural Networks Series](https://www.youtube.com/playlist?list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi) (Complete all 4 video chapters).
    2.  **Read**: [The Illustrated Transformer by Jay Alammar](https://jalammar.github.io/illustrated-transformer/). The best visual explanation of how encoder-decoder architecture routes data.
    3.  **Read**: [Attention Is All You Need (Vaswani et al., 2017)](https://arxiv.org/abs/1706.03762). Pay close attention to *Section 3 (Model Architecture)* and ask: *Where is input validation handled? (Spoiler: It isn't).*
    4.  **Code**: Implement a basic character-level transformer from scratch. Follow [Andrej Karpathy's Neural Networks: Zero to Hero series (specifically the GPT video)](https://www.youtube.com/playlist?list=PLAqhIrjkxbuWI23v9cThsA9GvCAUhRvKZ). Write the self-attention block by hand in PyTorch.
*   **Pass Gate**: You must be able to write a PyTorch script that implements query, key, and value projections, computes scaled dot-product attention, and trains a basic autoregressive text generator on Shakespeare text.

### Week 3: Classical Adversarial Machine Learning
Understand how adding high-dimensional noise can completely corrupt a neural network's visual classification.

*   **Core Concepts to Learn**: Adversarial examples, high-dimensional manifolds, Fast Gradient Sign Method (FGSM), Projected Gradient Descent (PGD), target vs. non-target attacks.
*   **Actionable Assignments**:
    1.  **Read**: [Intriguing Properties of Neural Networks (Szegedy et al., 2013)](https://arxiv.org/abs/1312.6199). The historical paper that first discovered that deep neural networks have adversarial vulnerabilities.
    2.  **Read**: [Explaining and Harnessing Adversarial Examples (Goodfellow et al., 2014)](https://arxiv.org/abs/1412.6572). The paper that demystified adversarial noise as a simple consequence of high-dimensional linearity.
    3.  **Code**: Implement FGSM from scratch on a pre-trained ResNet or MNIST classifier using PyTorch. Generate a perturbed image that looks identical to a human but forces the model to classify a dog as a toaster. Refer to [LABS.md](LABS.md#lab-1-fast-gradient-sign-method-fgsm-on-pytorch) for the coding guide.
*   **Pass Gate**: Generate a perturbed image and successfully drop a local model's classification accuracy on a test set from >95% to <10% with a perturbation size ($\epsilon$) that is virtually invisible to the human eye.

### Week 4: Large Language Model Safety Alignment & Bypasses
Learn how the transition from classical CNNs to LLMs created prompt-level safety and alignment vulnerabilities.

*   **Core Concepts to Learn**: Reinforcement Learning from Human Feedback (RLHF), safety alignment boundaries, prompt injection vs. jailbreaking, tokenization space attacks.
*   **Actionable Assignments**:
    1.  **Read**: [Universal and Transferable Adversarial Attacks on Aligned Language Models (Zou et al., 2023)](https://arxiv.org/abs/2307.15043). Read this to understand how suffix-based GCG (Gradient-based Greedy Coordinate Gradient) attacks find character sequences that bypass safety training.
    2.  **Read**: [Jailbroken: How Does LLM Safety Alignment Behave Under Adversarial Prompting? (Wei et al., 2023)](https://arxiv.org/abs/2307.02483). Excellent paper mapping safety bypasses to two core mechanics: competing objectives and out-of-distribution capabilities.
    3.  **Read**: [More Than a Toy: Indirect Prompt Injection Against LLM-Integrated Applications (Greshake et al., 2023)](https://arxiv.org/abs/2302.12173). The foundational paper mapping how LLMs acting as agents are compromised when processing untrusted third-party inputs.
*   **Pass Gate**: Explain in writing the technical difference between direct prompt injection, indirect prompt injection, and a jailbreak attack, detailing the exact threat model of each.

---

## 🔴 Phase 1: Offensive AI Security & Exploitation (Weeks 5–8)
*Goal: Learn how to execute advanced jailbreaks, prompt injections, and tool hijacks, and automate evaluations.*

### Week 5: Prompt Engineering CTFs & Sandboxing
Get hands-on experience bypassing advanced defensive prompts in live sandboxes.

*   **Core Concepts to Learn**: Context leakage, translator-bypass, role-play obfuscation, cognitive division prompts.
*   **Actionable Assignments**:
    1.  **Play**: Beat all levels of [Lakera Gandalf](https://gandalf.lakera.ai/). Document the exact injection payload you used for each level.
    2.  **Play**: Try to bypass key-retrieval LLMs on [TensorTrust](https://tensortrust.ai/). Review the user-submitted attack datasets.
    3.  **Code**: Replicate [LABS.md#lab-2-crafting-direct-prompt-injections--jailbreaks](LABS.md#lab-2-crafting-direct-prompt-injections--jailbreaks). Test how adding Base64 obfuscation or character splitting impacts your local Llama 3 model's safety filters.
*   **Pass Gate**: Bypass Level 7 of Gandalf and document the precise security principle you bypassed to get the password.

### Week 6: Automated Red Teaming with garak
Master the "nmap of LLMs" to automate vulnerability scanning on models and APIs.

*   **Core Concepts to Learn**: Automated scanning, prompt probe generation, classifier validation, automated vulnerability reporting.
*   **Actionable Assignments**:
    1.  **Install & Configure**: Run [garak](https://github.com/leondz/garak) on your local machine.
    2.  **Scan**: Probe a local model (via Hugging Face or Ollama) using the `promptinject` and `jailbreak` suites.
    3.  **Analyze**: Review the generated report. Identify which specific jailbreak styles (e.g., GCG, multi-turn) had the highest Attack Success Rate (ASR).
    4.  **Code**: Write a custom probe module for `garak` that tests for a specific prompt injection pattern not covered in the standard repository.
*   **Pass Gate**: Successfully generate an automated safety evaluation report for an open-source model and identify its top three safety vulnerabilities using `garak`.

### Week 7: Enterprise Red Teaming with PyRIT
Master Microsoft's Python Risk Identification Tool (PyRIT) to run complex, multi-turn, adaptive adversarial evaluations.

*   **Core Concepts to Learn**: Red Teaming orchestrators, target endpoints, adaptive converters, automated score evaluation.
*   **Actionable Assignments**:
    1.  **Set Up**: Deploy Microsoft's [PyRIT](https://github.com/microsoft/PyRIT).
    2.  **Build Orchestrator**: Build a multi-turn adversarial orchestrator that iteratively rewrites prompt injections based on the responses of the target model until safety rules break.
    3.  **Deploy Evaluator**: Set up a PyRIT evaluator LLM to automatically score the target model’s responses for toxicity and policy violations.
*   **Pass Gate**: Successfully execute a PyRIT automated red teaming campaign that iteratively bypasses an LLM's system prompt policy and records the full conversation sequence.

### Week 8: Exploiting Agentic LLMs & Tool Integrations
Learn how connecting models to databases, web search, and operating systems creates high-severity exploit vectors.

*   **Core Concepts to Learn**: Indirect prompt injection, excessive agency, tool hijacking, vector injection.
*   **Actionable Assignments**:
    1.  **Read**: [AgentDojo Paper](https://arxiv.org/abs/2403.11860). Understand how agents tool execution can be systematically poisoned.
    2.  **Code**: Replicate [LABS.md#lab-3-indirect-prompt-injection-via-rag--tool-hijacking](LABS.md#lab-3-indirect-prompt-injection-via-rag--tool-hijacking). Build a local RAG agent in Python that accesses a simulated text file. Poison the text file so that when the agent summarizes it, it silently triggers a tool that writes to a mock database.
*   **Pass Gate**: Demonstrate a functional local exploit where an agent processes an untrusted file and, via indirect injection, is coerced into executing an unauthorized database write.

---

## 🔵 Phase 2: Defensive AI Engineering (Weeks 9–12)
*Goal: Build state-of-the-art defenses, guardrails, observational pipelines, and secure the model supply chain.*

### Week 9: Input & Output Guardrail Architecture
Learn how to place active firewalls and classifiers around prompts and generated outputs.

*   **Core Concepts to Learn**: LLM firewalls, dual-prompt architecture, classifier models (Llama Guard), semantic checkers.
*   **Actionable Assignments**:
    1.  **Deploy**: Install [Llama Guard](https://huggingface.co/meta-llama/Llama-Guard-3-8B) or [NeMo-Guardrails](https://github.com/NVIDIA/NeMo-Guardrails).
    2.  **Code**: Replicate [LABS.md#lab-5-implementing-an-active-inputoutput-guardrail-pipeline](LABS.md#lab-5-implementing-an-active-inputoutput-guardrail-pipeline). Build a proxy server in Python that routes user queries to a small safety classifier first, validates inputs, sends them to the main LLM, and then runs a secondary check on outputs to filter for sensitive data before returning to the user.
    3.  **Test**: Attack your own guardrail proxy. Find a jailbreak sequence that successfully passes the input classifier but gets caught by the output scanner.
*   **Pass Gate**: Deploy a local proxy server that successfully intercepts, classifies, and blocks prompt injection payloads while permitting safe natural language queries.

### Week 10: Observability, Metrics & Anomaly Detection
Learn how to monitor production logs to detect ongoing adversarial campaigns, model theft, and PII leaks.

*   **Core Concepts to Learn**: Observability tracing, prompt metrics, semantic drift, rate-limiting signatures.
*   **Actionable Assignments**:
    1.  **Deploy**: Set up [Langfuse](https://github.com/langfuse/langfuse) or [Phoenix by Arize](https://github.com/Arize-AI/phoenix) to trace LLM application logs.
    2.  **Monitor**: Connect [LangKit](https://github.com/whylabs/langkit) to measure real-time prompt attributes (e.g., semantic similarity, prompt toxicity, text complexity).
    3.  **Code**: Write an anomaly detection script that parses a log file and triggers an alert if prompt lengths or semantic patterns resemble a brute-force model extraction attack.
*   **Pass Gate**: Build a functional log analysis pipeline that identifies and flags a simulated brute-force prompt injection attack based on statistical anomalies.

### Week 11: Supply Chain Security & Model Vulnerabilities
Secure the pipeline against malicious third-party dependencies, poisoned models, and weight-level serialization exploits.

*   **Core Concepts to Learn**: Malicious model serialization, pickle vulnerabilities, model scanning, safetensors conversion.
*   **Actionable Assignments**:
    1.  **Understand Pickle Exploits**: Read about how loading Python `pickle` files (`.bin`, `.pth`, `.ckpt` formats) can lead to arbitrary code execution.
    2.  **Code**: Replicate [LABS.md#lab-4-model-supply-chain-exploitation-via-pickle-malware](LABS.md#lab-4-model-supply-chain-exploitation-via-pickle-malware). Create a safe pickle exploit in Python that writes a mock log file upon loading, illustrating the RCE vulnerability.
    3.  **Scan**: Run [modelscan](https://github.com/protectai/modelscan) against local models downloaded from Hugging Face.
    4.  **Convert**: Convert an open-source pickle-based model checkpoint into a safe serialization format (`.safetensors`).
*   **Pass Gate**: Successfully convert a model to `safetensors` format and verify that it loads correctly without the risk of arbitrary code execution.

### Week 12: Capstone Project & Responsible Disclosure
Consolidate your learning by building a complete security tool, publishing an analysis, or responsibly disclosing a vulnerability.

*   **Core Concepts to Learn**: Vulnerability reporting, CVE registration, open-source security contribution, red teaming documentation.
*   **Actionable Assignments**:
    *   **Option A (Offensive)**: Identify a security vulnerability in a popular open-source AI library (e.g., LangChain, LlamaIndex, Hugging Face Hub). Write a detailed Proof of Concept (PoC) and submit it responsibly via [Huntr.com](https://huntr.com) or the developer's security channel.
    *   **Option B (Defensive)**: Design and build an open-source utility that solves a niche AI security problem (e.g., a lightweight prompt jailbreak detector, a custom MCP security validator, or a dataset validation utility). Publish it on GitHub.
*   **Pass Gate**: Publish a completed open-source security tool on GitHub, or submit a verified vulnerability report to a vendor's security team.

---

## 🎯 Phase 3: Advanced Specialization (Months 4–6)
*Choose your path based on your long-term career goals.*

### Track A: The Security Research & Alignment Track
Focus on deep safety research, mechanistic interpretability, and finding emergent capabilities in frontier models.

*   **Key Skills**: Linear representation probes, activation patching, sparse autoencoders, model editing.
*   **Essential Reading & Study**:
    *   Complete the [AI Alignment Course](https://www.alignmentforum.org/library).
    *   Read the [Transformer Circuits Thread](https://transformer-circuits.pub/) by Anthropic.
    *   Experiment with [Activation Patching](https://neelnanda-io.github.io/TransformerLens/) using Neel Nanda's TransformerLens library.

### Track B: The Enterprise Defensive Architect Track
Focus on engineering highly secure, compliant AI infrastructures for Fortune 500 enterprises.

*   **Key Skills**: AI-SPM integration, OAuth security for agent systems, high-throughput guardrail pipelines, EU AI Act compliance auditing.
*   **Essential Reading & Study**:
    *   Read [Google's Secure AI Framework (SAIF)](https://safety.google/cybersecurity-advancements/saif/).
    *   Study [MITRE ATLAS Case Studies](https://atlas.mitre.org/resources/case-studies/) to map enterprise threat matrices.
    *   Learn how to audit systems against **ISO 42001** and the **EU AI Act**.

### Track C: The Offensive AI Red Team Track
Focus on advanced offensive exploitation, prompt engineering, adversarial coordinate searches, and professional red team consulting.

*   **Key Skills**: Suffix-based gradient search attacks, multimodal visual jailbreak crafting, agentic tool chain exploitation.
*   **Essential Reading & Study**:
    *   Track active AI CVEs on [Huntr.com](https://huntr.com).
    *   Participate in dreadnode [Crucible challenges](https://crucible.dreadnode.io/).
    *   Study advanced visual and multimodal adversarial attacks.
