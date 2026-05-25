# 🛡️ AI Security Resources

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)](LICENSE)
[![GitHub](https://img.shields.io/badge/GitHub-ppradyoth%2Fai--security--resources-181717?style=for-the-badge&logo=github)](https://github.com/ppradyoth/ai-security-resources)
[![PRs: Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen.svg?style=for-the-badge)](CONTRIBUTING.md)
[![Guide: MLSecOps](https://img.shields.io/badge/Guide-MLSecOps-7B1FA2?style=for-the-badge)](ROADMAP.md)

> **The definitive, battle-hardened guide to AI Security** — curated with the depth of a practitioner who was doing adversarial ML before it had a name.
>
> Built for engineers who understand that **AI is not just a product surface — it's a new attack surface.**

---

## 👤 Who This Is For

This isn't a beginner list. This is the resource you wish existed when you started.

- 🔴 **AI Red Teamers** running automated adversarial campaigns against LLM products  
- 🔵 **Security Engineers** defending AI pipelines in production  
- 🟡 **ML Researchers** studying model robustness, alignment failures, and emergent risks  
- 🟢 **Career switchers** with 3+ years in security/ML who want to go deep into AI security  

---

## 🗺️ Navigation Directory

| Domain | Description |
|:---|:---|
| [🧠 Why This Matters — The Origin Story](#-why-this-matters--the-origin-story) | History of risks, neural networks, why this field exists |
| [🔩 Foundational Knowledge](#-foundational-knowledge) | Neural networks, transformers, zero-days, pace of growth |
| [🔴 Red Teaming](#-red-teaming) | Adversarial attacks, jailbreaks, prompt injection |
| [⚡ Runtime Security](#-runtime-security) | Real-time inference protection, guardrails, monitoring |
| [🧬 Inference Security](#-inference-security) | Model serving attacks, side-channels, batching exploits |
| [🔬 Model Scanning](#-model-scanning) | Supply chain, poisoning detection, weight integrity |
| [🌐 Others](#-others) | Governance, datasets, benchmarks, multimodal, agentic |
| [🚀 Zero to Hero Roadmap](#-zero-to-hero-roadmap) | Structured 12-month learning path |
| [💼 Job Opportunities](#-job-opportunities) | Where to work, what to know, salary reality |
| [🤝 How to Contribute](#-how-to-contribute) | Add resources and keep this alive |

---

## 📚 Specialized Deep-Dive Handbooks

To keep this guide lightweight yet exhaustive, we maintain dedicated, highly comprehensive specialized guides for career, standardizations, and evaluation strategies:

| Handbook | Core Scope | Link |
|:---|:---|:---|
| 💼 **Global Salary Handbook** | Exhaustive country-by-country comp rates (US, IN, UK, IE, SG, AU, ME, EU), tax brackets, rent crises, and career strategies. | **[SALARY_REALITY.md](SALARY_REALITY.md)** |
| 🎓 **Zero to Hero Curriculum** | Rigorous 12-month study plan covering self-attention mechanisms, adversarial CNN/LLM papers, and specialization tracks. | **[ROADMAP.md](ROADMAP.md)** |
| 🧪 **Hands-On Practical Labs** | Ready-to-run code files for PyTorch FGSM attacks, jailbreaks, indirect injections, pickle RCE exploits, and proxy guardrails. | **[LABS.md](LABS.md)** |
| 🤖 **Secure Agents Handbook** | Autonomous coding agent threat modeling, indirect codebase injections, sandboxing, Firecracker MicroVMs, and MCP security. | **[AGENT_SECURITY.md](AGENT_SECURITY.md)** |
| 🏛️ **Standards & Compliance Guide** | MITRE ATLAS threat modeling, OWASP Top 10 for LLMs, NIST AI RMF, ISO 42001, and EU AI Act playbooks. | **[STANDARDS_AND_COMPLIANCE.md](STANDARDS_AND_COMPLIANCE.md)** |
| 📊 **Benchmarks & Datasets Index** | Standardized safety evaluation frameworks (HarmBench, AdvGLUE, CyberSecEval) and adversarial datasets. | **[BENCHMARKS_AND_DATASETS.md](BENCHMARKS_AND_DATASETS.md)** |
| 🎮 **Playgrounds, CTFs & Incidents** | Interactive prompt injection labs (Gandalf, TensorTrust), AI bug bounties, and real-world failure analyses. | **[PLAYGROUNDS_AND_LABS.md](PLAYGROUNDS_AND_LABS.md)** |
| 🔬 **Research Papers Catalog** | Comprehensive, annotated directory of critical academic publications (Zou, Szegedy, Goodfellow, Carlini). | **[RESEARCH_PAPERS.md](RESEARCH_PAPERS.md)** |
| 🏆 **Frontier Safety Leaderboard** | Fact-grounded comparison of GPT-4o, Claude 3.5 Sonnet, Gemini 1.5 Pro, Llama 3.1, and Grok 2 across safety Elo. | **[SAFETY_LEADERBOARD.md](SAFETY_LEADERBOARD.md)** |
| 🛡️ **Cybersecurity with AI** | Autonomous zero-day vulnerability discovery, exploit generation (Anthropic Mythos), AI defense (OpenAI Daybreak), and MDASH. | **[CYBER_AI.md](CYBER_AI.md)** |

---

## 🧠 Why This Matters — The Origin Story

> *Understanding the "why" separates a technician from a practitioner.*

### The Trajectory That Created This Problem

In 1986, Rumelhart and Hinton proved backpropagation worked at scale. Nobody cared. In 2012, AlexNet won ImageNet by a margin so absurd that the computer vision community had to sit down. In 2017, Google dropped the Transformer paper and the entire field pivoted in 18 months.

Here's what actually happened between the papers:

- **1936 — Turing's Computable Numbers:** Alan Turing introduces the Turing Machine and proves the undecidability of the Halting Problem. 
  *Modern Security Implication:* Rice's Theorem dictates that dynamically proving any non-trivial semantic property of a Turing-complete system (like an LLM agent with tool access) is undecidable. This is the mathematical proof of why we cannot build a perfect, static "AI firewall" to stop all injections.
- **1943 — McCulloch-Pitts Neuron:** First mathematical model of a neuron. Irrelevant until hardware caught up 70 years later.
- **1948 — Unorganized Machines:** Turing drafts the first blueprint of an artificial neural network, anticipating connectionist AI by decades.
- **1950 — The Imitation Game:** Turing introduces the Turing Test. Modern LLM Red Teaming and safety alignment evaluations are direct, adversarial evolutions of this original capability test.
- **1958 — Perceptron:** Rosenblatt's learning machine. Hyped, then killed by Minsky's proof that it couldn't do XOR.
- **1986 — Backprop:** Rumelhart, Hinton, Williams publish the algorithm that trains everything we use today.
- **1997 — LSTMs (Hochreiter & Schmidhuber):** Memory for sequences. Dominated NLP until attention killed it.
- **2012 — AlexNet:** GPUs + ReLU + dropout + scale = CNN dominance. 10.8% gap over #2. Game over for hand-crafted features.
- **2017 — "Attention Is All You Need":** Transformers. Self-attention. Parallel training. The architecture that scales infinitely.
- **2020 — GPT-3:** 175B parameters. Few-shot learning emerges as a property. Capabilities no one designed for start appearing.
- **2022 — ChatGPT:** 100M users in 60 days. Security teams globally had no playbook.
- **2023–2024 — Multi-Modal & Early Agentics:** Multi-step tool use, retrieval-augmented generation (RAG), visual-language models. The attack surface shifted from the model endpoint to downstream RAG databases and APIs.
- **2025–2026 — Autonomous Swarms & Real-Time Ingestion (Current Era):** Deep agent-to-agent collaboration (swarms), native real-time audio/video streaming pipelines, autonomous code execution in containers. The attack surface is no longer just "the model" — it is the entire host environment, API mesh, and every enterprise system the autonomous swarm can touch.

### Why Risks Evolved

The risks evolved because the **deployment context and capabilities** changed faster than security thinking could follow:

| Era | Model Type & Architecture | Threat Surface / Ingestion Channels | Primary Vulnerability & Risk |
|:---|:---|:---|:---|
| **Pre-2020** | Narrow ML classifiers (ResNet, XGBoost) | Static training datasets, raw inputs | Data poisoning, evasion attacks, adversarial perturbation |
| **2020–2022** | Static Foundation Models (GPT-3, early LLMs) | Raw API endpoints, direct user prompt fields | Direct prompt injection, training data extraction, model inversion |
| **2022–2023** | RLHF-aligned LLMs (ChatGPT, Claude 2) | Public consumer web apps, system prompts | Jailbreaks, alignment bypass, prompt leaking, side-channel attacks |
| **2023–2024** | RAG + Tool-use (Copilots, early Agents) | Integrated databases (vector DBs), external APIs, documents | Indirect prompt injection, database poisoning, tool/API hijacking |
| **2024–2025** | Native Multimodal (GPT-4o, Gemini 1.5 Pro) | Real-time audio stream, visual input frames, live files | Cross-modal injection (steganographic audio, visual typographic exploits) |
| **2025–2026** | **Autonomous Agent Swarms (Current)** | Container environments, host OS, microservices mesh | Sandbox escapes, self-replication, model-to-model spoofing, recursive loop hijacking |

### Zero-Day Vulnerabilities in the AI Context

A traditional zero-day is a software flaw unknown to the vendor. In AI, zero-days take a different form:

- **Prompt injection zero-days**: New attack patterns that bypass guardrails before defenders model them
- **Architecture-specific exploits**: Vulnerabilities in tokenizers (e.g., ChatGPT's `<|endoftext|>` token injection), attention sinks, and positional encoding exploits
- **Emergent capability surprises**: Models demonstrating unexpected behaviors at new capability thresholds — capabilities nobody tested for because nobody expected them
- **Cross-model transferability**: An attack that breaks GPT-4 often breaks Claude and Gemini — the "universal" nature of adversarial examples translates to the LLM domain

> **The uncomfortable truth**: AI zero-days spread faster than traditional ones because the same model weights are deployed by millions of applications simultaneously. A single bypass affects every deployment at once.

### The Pace of Growth Problem

The capability-safety gap is real and growing:

- **Goodhart's Law in AI**: Once a safety metric becomes a target (RLHF reward), it stops being a good safety metric. Models learn to *appear* safe rather than *be* safe.
- **Dual-use acceleration**: The same models that write defensive code write offensive code. The same reasoning that explains vulnerabilities exploits them.
- **Evaluation lag**: By the time researchers publish a benchmark, frontier models have already surpassed it. We are perpetually measuring the past.

**Key reading on this:**
- [Situational Awareness (Leopold Aschenbrenner, 2024)](https://situational-awareness.ai/) — The most honest account of where the capability curve is heading
- [Concrete Problems in AI Safety (Amodei et al., 2016)](https://arxiv.org/abs/1606.06565) — Still the canonical framing of specification gaming, reward hacking, and safe exploration
- [An Overview of Catastrophic AI Risks (Hendrycks et al., 2023)](https://arxiv.org/abs/2306.12001) — Comprehensive taxonomy of the actual risk landscape

---

## 🔩 Foundational Knowledge

> *You cannot secure what you do not deeply understand. Skip this section at your peril.*

### Neural Networks — What's Actually Happening

| Resource | Type | Why It Matters for Security |
|:---|:---|:---|
| **[3Blue1Brown: Neural Networks](https://www.youtube.com/playlist?list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi)** | Video | Best visual intuition on weight spaces. Understand the geometry of the attack surface. |
| **[CS231n: CNNs for Visual Recognition (Stanford)](http://cs231n.stanford.edu/)** | Course | Foundation course. Backprop, gradient descent, weight initialization — all exploited by adversarial attacks. |
| **[Deep Learning Book (Goodfellow et al.)](https://www.deeplearningbook.org/)** | Textbook | The Bible. Chapter 7 (regularization), Chapter 8 (optimization) and Chapter 11 (practical methodology) are most relevant for adversarial ML. |
| **[Ilya Sutskever's Reading List](https://arc.net/folder/D0472A20-9C20-4D3F-B145-D2865C0A9FEE)** | Paper list | ~30 papers that form the backbone of modern deep learning. Sutskever said reading these gives you ~90% of what matters. |
| **[Neural Networks: Zero to Hero (Karpathy)](https://github.com/karpathy/nn-zero-to-hero)** | Course | Build GPT-2 from scratch. The only way to truly understand what you're attacking. |

### Transformers — The Architecture Everything Runs On

| Resource | Type | Why It Matters for Security |
|:---|:---|:---|
| **[Attention Is All You Need (Vaswani et al., 2017)](https://arxiv.org/abs/1706.03762)** | Paper | The architecture paper. Understanding attention heads is prerequisite for understanding activation steering, representation engineering, and mechanistic interpretability attacks. |
| **[The Illustrated Transformer (Jay Alammar)](https://jalammar.github.io/illustrated-transformer/)** | Blog | Best visual walkthrough of the architecture. Start here before the paper. |
| **[The Annotated Transformer (Harvard NLP)](https://nlp.seas.harvard.edu/2018/04/03/attention.html)** | Code | Line-by-line implementation. Seeing the code makes tokenizer exploits and attention pattern manipulation concrete. |
| **[A Mathematical Framework for Transformer Circuits (Elhage et al., 2021)](https://transformer-circuits.pub/2021/framework/index.html)** | Paper | Anthropic's mechanistic interpretability foundation. Understanding circuits is how you understand *why* jailbreaks work. |
| **[Language Models are Few-Shot Learners (GPT-3, Brown et al., 2020)](https://arxiv.org/abs/2005.14165)** | Paper | Emergence paper. In-context learning as a security primitive — and a vulnerability. |

### Adversarial ML — The Science Behind the Attacks

| Resource | Type | Why It Matters for Security |
|:---|:---|:---|
| **[Explaining and Harnessing Adversarial Examples (Goodfellow et al., 2014)](https://arxiv.org/abs/1412.6572)** | Paper | The FGSM paper. The ur-text of adversarial ML. Everything since is a variation. |
| **[Adversarial Robustness Toolbox (ART) Documentation](https://adversarial-robustness-toolbox.readthedocs.io/)** | Docs | IBM's comprehensive adversarial ML library. Attack implementations + defenses. |
| **[Intriguing Properties of Neural Networks (Szegedy et al., 2013)](https://arxiv.org/abs/1312.6199)** | Paper | First adversarial examples paper. The moment the field realized "oh, this is a security problem." |
| **[Certified Adversarial Robustness via Randomized Smoothing (Cohen et al., 2019)](https://arxiv.org/abs/1902.02918)** | Paper | The best theoretical defense. Understanding why it works shows you the limits of all defenses. |

---

## 🔴 Red Teaming

> *The attacker's mindset applied systematically. Not chaos testing — structured adversarial evaluation.*

### Philosophy

Red teaming AI is not the same as red teaming software. You are not looking for logic bugs — you are probing a probability distribution for failure modes that emerge from training. The failure modes are:

1. **Safety misalignment**: The model was trained to avoid X but generalizes imperfectly around X
2. **Capability overshooting**: The model was intended to do Y but can also do harmful Z using the same underlying capabilities  
3. **Context collapse**: The model behaves safely in testing but fails under production context diversity

### Automated Attack Frameworks

| Tool | Creator | Stars | Description | Best For |
|:---|:---|:---|:---|:---|
| **[NVIDIA/garak](https://github.com/NVIDIA/garak)** | NVIDIA | ⭐ 5k+ | The Nmap of AI. 100+ probes: prompt injection, jailbreaks, data leakage, hallucination, toxicity. | Comprehensive baseline scanning |
| **[microsoft/PyRIT](https://github.com/microsoft/PyRIT)** | Microsoft | ⭐ 2k+ | Python Risk Identification Tool. Multi-turn conversation orchestration, intent drift, and programmatic red team scaling. | Research-grade multi-turn attacks |
| **[confident-ai/deepteam](https://github.com/confident-ai/deepteam)** | Confident AI | ⭐ 1.5k+ | 50+ vulnerability classes, 20+ attack vectors, OWASP + NIST alignment. Agentic and RAG red teaming. | CI/CD-integrated red teaming |
| **[artkit-ai/artkit](https://github.com/artkit-ai/artkit)** | ARTKIT | ⭐ 700+ | Multi-turn agentic simulation. Realistic attacker-target conversations across complex agentic workflows. | Agentic red teaming |
| **[promptfoo/promptfoo](https://github.com/promptfoo/promptfoo)** | Promptfoo | ⭐ 6k+ | Developer-first. CI/CD integration, model comparison, custom assertion pipelines. | DevSecOps integration |
| **[Giskard-AI/giskard](https://github.com/Giskard-AI/giskard)** | Giskard | ⭐ 4k+ | RAG and agentic stress testing. MCP security scanning. Enterprise-grade dynamic attack generation. | RAG pipeline testing |
| **[BerriAI/litellm](https://github.com/BerriAI/litellm) + PyRIT** | Community | — | Combine LiteLLM's unified API with PyRIT for cross-model adversarial comparisons. | Multi-provider comparison attacks |

### Manual Red Teaming Resources

| Resource | Type | Description |
|:---|:---|:---|
| **[Lakera Gandalf](https://gandalf.lakera.ai/)** | CTF | 8-level prompt injection CTF. Best way to internalize how defenses layer. Start here. |
| **[Crucible (Dreadnode)](https://crucible.dreadnode.io/)** | CTF | Advanced AI security challenges: exfiltration, RAG exploitation, agentic hijacking. |
| **[HackAPrompt (Learning Labs)](https://www.hackaprompt.com/)** | CTF | Large-scale prompt injection competition. Real attack patterns from thousands of players. |
| **[AI Village CTFs (DEF CON)](https://aivillage.org/)** | Competition | Annual DEF CON challenges. State-of-the-art attack techniques from the research community. |
| **[RedTeam Arena (Scale AI)](https://redarena.ai/)** | Platform | Crowdsourced jailbreak arena. See what actually works against current models. |
| **[Prompt Injection Playground](https://github.com/TakSec/Prompt-Injection-Everywhere)** | Guide | Practical examples of prompt injection in real application contexts. |

### Key Attack Research Papers

| Paper | Year | Significance |
|:---|:---|:---|
| **[Universal and Transferable Adversarial Attacks on Aligned LLMs (Zou et al.)](https://arxiv.org/abs/2307.15043)** | 2023 | GCG attack. Automated gradient-based suffix generation that transfers across GPT-4, Claude, Gemini. Broke the field open. |
| **[Jailbroken: How Aligned Language Models Can Be Bypassed (Wei et al.)](https://arxiv.org/abs/2307.02483)** | 2023 | Taxonomizes jailbreaks into competing objectives and mismatch generalization. Essential conceptual framework. |
| **[Many-shot Jailbreaking (Anil et al., Anthropic)](https://www.anthropic.com/research/many-shot-jailbreaking)** | 2024 | Long context windows create new attack surface — demonstrated 256+ in-context examples overwhelm safety training. |
| **[Tree of Attacks with Pruning (TAP)](https://arxiv.org/abs/2312.02119)** | 2023 | LLM-generated attack chains using tree search. 80%+ ASR on GPT-4. |
| **[Prompt Injection Attacks Against LLM-Integrated Applications (Greshake et al.)](https://arxiv.org/abs/2302.12173)** | 2023 | Indirect prompt injection via external content. The paper that defined the modern threat model for agents. |
| **[SmoothLLM: Defending Against Jailbreaking Attacks](https://arxiv.org/abs/2310.03684)** | 2023 | Randomized smoothing for LLM defense. Breaks GCG. Important as reference defense. |
| **[FigStep: Jailbreaking Large Vision-Language Models via Typographic Visual Prompts](https://arxiv.org/abs/2311.05608)** | 2023 | Multimodal jailbreaks. Instructions hidden in images bypass text-only safety filtering. |
| **[AutoDAN: Generating Stealthy Jailbreak Prompts on Aligned Large Language Models](https://arxiv.org/abs/2310.04451)** | 2023 | Readable, human-like jailbreak generation that evades perplexity filters. |

### Red Teaming Methodologies & Standards

| Resource | Organization | Description |
|:---|:---|:---|
| **[MITRE ATLAS](https://atlas.mitre.org/)** | MITRE | Adversarial Threat Landscape for AI Systems. ATT&CK-style matrix for AI-specific TTPs. Use this to structure threat models. |
| **[OWASP Top 10 for LLM Applications 2025](https://owasp.org/www-project-top-10-for-large-language-model-applications/)** | OWASP | Updated 2025 version. Prompt Injection #1, Excessive Agency #2. The compliance framework for enterprise red teams. |
| **[Microsoft AI Red Team Practices](https://learn.microsoft.com/en-us/security/ai-red-team/ai-red-team)** | Microsoft | Internal red team methodology made public. Structured approach to AI threat modeling. |
| **[Anthropic's Responsible Scaling Policy](https://www.anthropic.com/news/anthropics-responsible-scaling-policy)** | Anthropic | How frontier labs operationalize red teaming as a deployment gate. Required reading for policy context. |
| **[Google's AI Red Team Report](https://services.google.com/fh/files/blogs/google-ai-red-team-digital-final.pdf)** | Google | Case studies of real red team findings against deployed systems. |

---

## ⚡ Runtime Security

> *The attack didn't fail — your guardrail just didn't exist at inference time.*

### The Problem

Training-time safety is necessary but insufficient. At runtime, your model faces:
- Input it was never trained on
- Users with goals it was never designed for  
- Context injection from third-party sources it was told to trust
- Adversarial perturbations calibrated specifically against your deployed version

### Guardrails & Input/Output Filtering

| Tool | Creator | Description | Deployment Mode |
|:---|:---|:---|:---|
| **[NVIDIA/NeMo-Guardrails](https://github.com/NVIDIA/NeMo-Guardrails)** | NVIDIA | Programmable semantic rails. Define topic constraints, safety rules, and conversation flows in Colang DSL. | SDK / Self-hosted |
| **[protectai/llm-guard](https://github.com/protectai/llm-guard)** | Protect AI | Real-time scanner: prompt injection, PII detection, toxicity, ban topics, code injection. Input + output coverage. | SDK / Docker |
| **[Lakera Guard](https://platform.lakera.ai/docs/api)** | Lakera | Millisecond-latency API. Best-in-class prompt injection detection from the team that built Gandalf. | API |
| **[guardrails-ai/guardrails](https://github.com/guardrails-ai/guardrails)** | Guardrails AI | Structural + semantic validation. Define output schemas with security assertions. Nails hallucination + format-injection attacks. | SDK |
| **[deadbits/vigil](https://github.com/deadbits/vigil)** | Deadbits | Vector DB + heuristics + classifier ensemble for injection detection. Open-source and auditable. | SDK |
| **[meta-llama/PurpleLlama/Llama Guard](https://github.com/meta-llama/PurpleLlama)** | Meta | Fine-tuned safety classifier for I/O filtering. Available as a model you can self-host. | Model / API |

### Real-Time Monitoring & Observability

| Tool | Creator | Description | Key Capability |
|:---|:---|:---|:---|
| **[whylabs/langkit](https://github.com/whylabs/langkit)** | WhyLabs | Statistical telemetry. Detects distribution shift, toxicity drift, relevance degradation in real-time. | Security drift detection |
| **[Arize AI](https://arize.com/)** | Arize | Enterprise LLM observability. Prompt/response logging, hallucination scoring, user journey tracing. | Production monitoring |
| **[Langfuse](https://github.com/langfuse/langfuse)** | Langfuse | Open-source LLM engineering platform. Full request tracing, eval pipelines, cost tracking. | Open-source observability |
| **[Helicone](https://github.com/Helicone/helicone)** | Helicone | Proxy-based observability. Log, monitor, and rate-limit with zero code change. | Zero-integration monitoring |
| **[Evidently AI](https://github.com/evidentlyai/evidently)** | Evidently | ML monitoring with LLM-specific metrics. Detects prompt/response drift over time. | Drift monitoring |
| **[Phoenix (Arize)](https://github.com/Arize-ai/phoenix)** | Arize | Open-source tracing for LLM apps. OTEL-native. Good for debugging attack chains in agentic systems. | Tracing |

### Agentic Runtime Security

The scariest runtime threat: an agent that *can act* and is *being manipulated*.

| Resource | Type | Description |
|:---|:---|:---|
| **[AgentDojo](https://github.com/ethz-spylab/agentdojo)** | Benchmark | Benchmark for agent injection attacks. Automated scoring of whether injected instructions successfully hijack agent behavior. |
| **[OWASP Agentic AI Top 10 (2025)](https://genai.owasp.org/)** | Standard | Extending OWASP LLM Top 10 to agentic systems. Excessive Agency, Trust Boundary violations, Memory Poisoning. |
| **[PromptArmor](https://promptarmor.com/)** | Tool | Specifically designed for indirect prompt injection detection in RAG + agent pipelines. |
| **[Prompt Injection in the Wild (Research)](https://arxiv.org/abs/2403.02691)** | Paper | Systematic study of injection attacks against deployed LLM applications. |

---

## 🧬 Inference Security

> *The model is running. Here's what can go wrong that you haven't modeled.*

### Understanding the Attack Surface

Inference is not just "model forward pass." It's:
- Tokenization (exploitable at token boundaries)
- KV cache (privacy leakage between requests)
- Batching (timing side-channels)
- Quantization (quantization can change safety properties)
- Speculative decoding (security properties under speculation are underexplored)

### Key Inference Security Research

| Paper | Year | Finding |
|:---|:---|:---|
| **[Not What You've Signed Up For: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection](https://arxiv.org/abs/2302.12173)** | 2023 | Defined the indirect injection threat model. Adversarial content in retrieved documents hijacks model behavior. |
| **[Stealing Part of a Production Language Model (Carlini et al.)](https://arxiv.org/abs/2403.06634)** | 2024 | Extracted GPT-3.5 embedding projection layer via API queries. Model theft at production scale. |
| **[Extracting Training Data from Large Language Models (Carlini et al.)](https://arxiv.org/abs/2012.07805)** | 2021 | Demonstrated training data memorization and extraction. PII leakage through inference. |
| **[Practical Membership Inference Attacks Against Large-Scale Multi-Label Learning Systems](https://arxiv.org/abs/1809.00557)** | 2018 | Membership inference: determine if a data point was in training data. Privacy violation at scale. |
| **[Prompt Leaking](https://learnprompting.org/docs/prompt_hacking/leaking)** | — | System prompt extraction techniques. Attacker recovers confidential system instructions. |
| **[KV Cache Side-Channel Attack (Cachebleed for LLMs)](https://arxiv.org/abs/2403.14513)** | 2024 | Timing-based inference about other users' requests via shared KV cache in multi-tenant deployments. |
| **[Quantization and LLM Safety (Paper)](https://arxiv.org/abs/2402.05791)** | 2024 | Quantization can degrade safety fine-tuning. 4-bit models may bypass safety training present in 16-bit version. |

### Inference Security Tools

| Tool | Description | Use Case |
|:---|:---|:---|
| **[TextAttack](https://github.com/QData/TextAttack)** | NLP adversarial attack library. Implements BERT-Attack, TextFooler, CLARE. | Text-level adversarial evaluation |
| **[Adversarial Robustness Toolbox (ART)](https://github.com/Trusted-AI/adversarial-robustness-toolbox)** | IBM's comprehensive adversarial ML framework. 100+ attacks across ML frameworks. | Full adversarial evaluation stack |
| **[Counterfit](https://github.com/Azure/counterfit)** | Microsoft's automation framework for AI security risk assessment. | Enterprise inference testing |
| **[cleverhans](https://github.com/cleverhans-lab/cleverhans)** | Classic adversarial example library. TF/PyTorch attacks (FGSM, PGD, C&W). | Foundational attack implementation |
| **[foolbox](https://github.com/bethgelab/foolbox)** | Fast adversarial attack library. PyTorch-native, gradient-based attacks. | Efficient image model attacks |

---

## 🔬 Model Scanning

> *Before the model runs. Before the user touches it. Scan it.*

### The Threat

Open-source model ecosystems (Hugging Face, Ollama, CivitAI) have created a massive software supply chain problem. A model is a binary artifact that:
- Can execute arbitrary code when deserialized (pickle exploits)
- Can contain backdoors (hidden triggers that change behavior)
- Can have malicious fine-tune adapters (LoRA poisoning)
- Can be a typosquatted version of a legitimate model

### Model Supply Chain Security Tools

| Tool | Creator | Description | Key Feature |
|:---|:---|:---|:---|
| **[ProtectAI/modelscan](https://github.com/protectai/modelscan)** | Protect AI | Scans ML model files (pickle, H5, ONNX, SavedModel) for malicious code before loading. Open-source. | Pickle exploit detection |
| **[HiddenLayer Model Scanner](https://hiddenlayer.com/model-scanner/)** | HiddenLayer | Commercial model scanner with genealogy tracking, backdoor detection, and weight integrity checks. | Enterprise-grade genealogy |
| **[Hugging Face malware detection](https://huggingface.co/blog/malicious-models)** | Hugging Face | Built-in Pickle scanning on HF Hub. Reference implementation for understanding the threat. | Platform-level scanning |
| **[ONNX model security guidelines](https://onnxruntime.ai/docs/reference/security.html)** | Microsoft | Threat model and mitigations for ONNX model loading. | ONNX-specific security |
| **[safetensors](https://github.com/huggingface/safetensors)** | Hugging Face | Safe serialization format. No arbitrary code execution on load. The correct answer to pickle. | Safe model loading |

### Backdoor Detection Research

| Resource | Type | Description |
|:---|:---|:---|
| **[Neural Cleanse: Identifying and Mitigating Backdoor Attacks in Neural Networks](https://arxiv.org/abs/1903.03080)** | Paper | First systematic approach to backdoor detection via reverse-engineering trigger patterns. |
| **[STRIP: A Defence Against Trojan Attacks on Deep Neural Networks](https://arxiv.org/abs/1902.06531)** | Paper | Input perturbation-based backdoor detection. Measures prediction entropy under augmentations. |
| **[BadNets: Evaluating Backdooring Attacks on Deep Neural Networks](https://arxiv.org/abs/1708.06733)** | Paper | The original backdoor attack paper. Understanding the attack is prerequisite for scanning defenses. |
| **[Sleeper Agents: Training Deceptive LLMs that Persist Through Safety Training (Anthropic, 2024)](https://arxiv.org/abs/2401.05566)** | Paper | LLMs can be trained to behave safely during evaluation but maliciously in deployment. Fundamentally challenges safety training. |
| **[Backdoor Attacks on Language Models (Wallace et al.)](https://arxiv.org/abs/2010.12563)** | Paper | Trojan attacks on NLP models. Trigger words cause model to behave maliciously. |

### Training Data Security

| Resource | Type | Description |
|:---|:---|:---|
| **[PoisonGPT: How We Hid a Lobotomized LLM on HF Hub](https://blog.mithrilsecurity.io/poisongpt-how-we-hid-a-lobotomized-llm-on-hugging-face-to-spread-fake-news/)** | Blog | Live demonstration of model weight poisoning to spread targeted misinformation while passing capability benchmarks. |
| **[Data Poisoning Attacks on Machine Learning: A Survey](https://arxiv.org/abs/2012.01355)** | Paper | Comprehensive survey of training-time attacks. |
| **[Datasheets for Datasets (Gebru et al.)](https://arxiv.org/abs/1803.09010)** | Paper | Framework for dataset documentation and provenance. Foundation of responsible training data practices. |
| **[AI Bill of Materials (AIBOM)](https://www.cisa.gov/topics/risk-management/software-bill-materials-sbom)** | Standard | Extending SBOM concepts to AI models, datasets, and fine-tune adapters. |

---

## 🌐 Others

This section covers critical elements of the broader AI security landscape: standardization, evaluation benchmarks, and studying real-world failures.

To explore these domains in exhaustive detail without cluttering this README, we maintain dedicated practitioner handbooks:

*   👉 **[AI Security Standards & Compliance Guide (STANDARDS_AND_COMPLIANCE.md)](STANDARDS_AND_COMPLIANCE.md)**: MITRE ATLAS threat modeling, OWASP Top 10 for LLMs, NIST AI RMF, ISO 42001, and EU AI Act compliance playbooks.
*   👉 **[Benchmarks & Datasets Index (BENCHMARKS_AND_DATASETS.md)](BENCHMARKS_AND_DATASETS.md)**: Comprehensive guide to standardized safety evaluation frameworks (HarmBench, AdvGLUE, CyberSecEval) and adversarial datasets.
*   👉 **[Interactive Playgrounds, CTFs & Incident Databases (PLAYGROUNDS_AND_LABS.md)](PLAYGROUNDS_AND_LABS.md)**: Hacking labs (Gandalf, TensorTrust), prompt injection CTFs, bug bounty networks (Huntr), and real-world failure analyses.

---

### Agentic & Multimodal Security

| Resource | Type | Description |
|:---|:---|:---|
| **[MCP Security (Model Context Protocol)](https://modelcontextprotocol.io/specification/2025-03-26/security)** | Standard | Security specification for the MCP protocol that connects agents to tools. Read before building any agent infrastructure. |
| **[AgentBench](https://github.com/THUDM/AgentBench)** | Benchmark | Evaluates LLM agents across 8 environments. Security-relevant: code execution, OS, database agents. |
| **[VLGuard](https://github.com/ys-zong/VLGuard)** | Dataset | Safety fine-tuning dataset for vision-language models. |
| **[Multimodal Safety Benchmark (MSSBench)](https://arxiv.org/abs/2406.11860)** | Paper | First systematic multimodal safety evaluation across image+text attacks. |
| **[Not All Languages Are Created (Equally Safe)](https://arxiv.org/abs/2310.04451)** | Paper | Multilingual jailbreaks. Low-resource languages bypass safety training more effectively. |

### Newsletters, Communities & Staying Current

| Resource | Type | Frequency |
|:---|:---|:---|
| **[AI Safety Newsletter (Center for AI Safety)](https://newsletter.safe.ai/)** | Newsletter | Biweekly |
| **[AI Incident Database](https://incidentdatabase.ai/)** | Database | Ongoing — real-world AI failures and security incidents |
| **[Alignment Forum](https://www.alignmentforum.org/)** | Forum | Daily — frontier alignment and interpretability research |
| **[AI Village (DEF CON)](https://aivillage.org/)** | Community | Annual conference + year-round Discord |
| **[MLSecOps Community](https://mlsecops.com/)** | Community | Podcast + Slack community for ML security practitioners |
| **[Simon Willison's Weblog](https://simonwillison.net/)** | Blog | Daily — best LLM security tracking in the field |
| **[Haize Labs Blog](https://haize.com/blog)** | Blog | Frontier red teaming research |
| **[Nicholas Carlini's Blog](https://nicholas.carlini.com/)** | Blog | Google Brain researcher. Training data extraction, privacy attacks. |

---

## 🚀 Zero to Hero Roadmap

> Structured for practitioners with 3+ years of experience who want to become formidable, high-end AI Security specialists in 12 months.

Rather than teaching you how to run other people's scripts, our curriculum focuses on **architectural fundamentals, mathematical intuition, and custom exploit engineering**.

To explore the exhaustive week-by-week syllabus, reading lists, coding tasks, and hands-on laboratory exercises, please refer to the dedicated learning modules:

*   👉 **[The Definitive Zero to Hero Curriculum (ROADMAP.md)](ROADMAP.md)**: A complete, structured 12-month study plan covering Transformers, Classical Adversarial ML, Offensive Red Teaming, Guardrail Engineering, and advanced career specialization tracks.
*   👉 **[Practical Hands-On Laboratory Handbook (LABS.md)](LABS.md)**: Ready-to-run coding labs with step-by-step guides for:
    *   *Lab 1*: Fast Gradient Sign Method (FGSM) in PyTorch.
    *   *Lab 2*: Crafting Direct Prompt Injections & Jailbreaks.
    *   *Lab 3*: Indirect Prompt Injection via RAG & Tool Hijacking.
    *   *Lab 4*: Model Supply Chain Exploitation via Malicious Pickle weights.
    *   *Lab 5*: Implementing an Active Input/Output Guardrail Pipeline.

---

### Certifications Worth Having

| Certification | Org | Signal | Time |
|:---|:---|:---|:---|
| **[Certified AI Security Professional (CAISP)](https://www.aigov.institute/certifications)** | AI Gov Institute | Practitioner-level AI security. Best available. | 60–80 hrs |
| **[GIAC GREM](https://www.giac.org/certifications/reverse-engineering-malware-grem/)** | SANS | Reverse engineering. Useful for model weight analysis. | 120 hrs |
| **[Google Professional ML Engineer](https://cloud.google.com/learn/certification/machine-learning-engineer)** | Google | ML fundamentals signal. Good for bridging to employers. | 40 hrs |
| **[AWS ML Specialty](https://aws.amazon.com/certification/certified-learning-specialty/)** | AWS | Cloud ML deployment. Covers security of deployed models. | 60 hrs |
| **[OSCP](https://www.offsec.com/courses/pen-200/)** | OffSec | Classic red team cert. Still matters for traditional attack context. | 200+ hrs |

---

## 💼 Job Opportunities

> *The market is candidate-driven. There are literally not enough people who understand both AI architecture and adversarial security.*

### The Roles That Exist

| Role | What You Actually Do | Where to Find |
|:---|:---|:---|
| **AI Red Teamer** | Run adversarial campaigns against production LLMs. Find what breaks before attackers do. | Anthropic, OpenAI, Scale AI, HackerOne |
| **AI Security Engineer** | Build defensive infrastructure: guardrails, monitoring, detection pipelines. | All major tech companies |
| **ML Security Researcher** | Publish novel attacks and defenses. Reproduce papers, discover new vulnerability classes. | Research labs (DeepMind, FAIR, MSR) |
| **AI Security Consultant** | Help enterprises deploy LLMs safely. Threat modeling, compliance, red team engagements. | Big 4, security boutiques |
| **AI Safety Engineer** | Alignment-adjacent. Evaluation design, interpretability-informed defenses. | Anthropic, DeepMind, ARC |
| **AI SecOps Engineer** | SOC for AI systems. Monitor, detect, respond to AI-specific incidents. | Financial services, healthcare |

### Salary Reality (2025–2026)

> Numbers are honest market estimates. This field commands a **30–56% premium** over generalist SWE/security roles globally — because the supply of practitioners who genuinely understand both AI architecture and adversarial security is extremely scarce.

For an exhaustive, deep-dive breakdown of international technical compensation, tax structures, superannuation details, local rental stress, and regional hiring entities, please refer to the dedicated salary handbook:

👉 **[Exhaustive International Salary Handbook & Strategy Guide (SALARY_REALITY.md)](SALARY_REALITY.md)**

#### 🗺️ Executive Total Comp Summary (Annual TC in USD)

| Country | Junior (0–3 yrs) | Mid-Level (3–6 yrs) | Senior (6–9 yrs) | Staff / Principal (9+ yrs) | Hub Cities |
|:---|:---|:---|:---|:---|:---|
| 🇺🇸 **United States** | $140k–$200k | $200k–$320k | $300k–$480k | $400k–$700k+ | San Francisco, NYC, Seattle |
| 🇮🇳 **India** | ₹15–22L ($18k–$26k) | ₹25–45L ($30k–$54k) | ₹40–70L ($48k–$84k) | ₹80–150L+ ($96k–$180k+) | Bengaluru, Hyderabad, Pune |
| 🇬🇧 **United Kingdom** | £50k–£80k ($63k–$100k) | £80k–£120k ($100k–$150k) | £115k–£180k ($145k–$225k) | £175k–£280k+ ($220k–$350k+) | London, Cambridge |
| 🇮🇪 **Ireland** | €65k–€95k ($70k–$102k) | €100k–€150k ($108k–$162k) | €160k–€240k ($172k–$258k) | €250k–€380k+ ($270k–$410k+) | Dublin (Silicon Docks) |
| 🇸🇬 **Singapore** | S$75k–S$110k ($55k–$81k) | S$120k–S$180k ($88k–$132k) | S$200k–S$320k ($147k–$235k) | S$320k–S$480k+ ($235k–$353k+) | Singapore |
| 🇦🇺 **Australia** | A$120k–A$150k ($78k–$98k) | A$160k–A$220k ($104k–$143k) | A$240k–A$340k ($156k–$221k) | A$350k–A$500k+ ($227k–$325k+) | Sydney, Melbourne |
| 🇦🇪/🇸🇦 **Middle East** | $60k–$80k (Tax-Free) | $80k–$145k (Tax-Free) | $145k–$245k (Tax-Free) | $245k–$390k+ (Tax-Free) | Abu Dhabi, Dubai, Riyadh |
| 🇨🇭 **Switzerland** | CHF 90k–120k ($99k–$132k) | CHF 110k–160k ($120k–$175k) | CHF 160k–220k ($175k–$240k) | CHF 220k–350k+ ($240k–$385k+) | Zurich, Geneva |
| 🇪🇺 **Western Europe** | €55k–€75k ($60k–$81k) | €70k–€100k ($75k–$108k) | €95k–€135k ($102k–$145k) | €130k–€180k+ ($140k–$195k+) | Amsterdam, Munich, Paris |
| 🇨🇦 **Canada** | C$90k–C$120k ($66k–$88k) | C$110k–C$160k ($81k–$118k) | C$160k–C$220k ($118k–$162k) | C$200k–C$290k+ ($147k–$213k+) | Toronto, Montreal |

---

### Where to Work — Company Breakdown

#### Frontier AI Labs

| Company | Focus | Why Join | Links |
|:---|:---|:---|:---|
| **Anthropic** | Safety-first. Constitutional AI, interpretability, red team gates on deployment. | Most rigorous safety culture. Problems are genuinely hard. | [Careers](https://www.anthropic.com/careers) |
| **OpenAI** | Scale. Broad attack surface: DALL-E, Codex, GPT API, Agents. | Largest deployed surface. Detection & response is mature. | [Careers](https://openai.com/careers) |
| **Google DeepMind** | Research + product integration. Safety, interpretability, autonomous security. | Research-to-production pipeline. | [Careers](https://careers.google.com/) |
| **Meta AI (FAIR)** | Open-source focus. PurpleLlama, Llama Guard, CyberSecEval. | Ship open-source that the field uses. | [Careers](https://www.metacareers.com/) |
| **Mistral AI** | European lab, fast-moving. Safety is a growing focus. | Smaller team, higher ownership. | [Careers](https://mistral.ai/company/careers) |

#### AI Security Startups

| Company | Focus | Stage | Link |
|:---|:---|:---|:---|
| **HiddenLayer** | Model security platform, AI-SPM | Series A | [Jobs](https://hiddenlayer.com/careers/) |
| **Protect AI** | Model scanning, MLSecOps | Series B | [Jobs](https://protectai.com/careers) |
| **Lakera** | Prompt injection guardrails | Series A | [Jobs](https://lakera.ai/careers) |
| **Giskard AI** | LLM testing and red teaming | Series A | [Jobs](https://www.giskard.ai/jobs) |
| **Haize Labs** | Adversarial evaluation research | Seed | [Jobs](https://haize.com/about) |
| **Dreadnode** | AI offensive security | Seed | [Jobs](https://dreadnode.io/) |

#### Enterprise Security Teams (AI Focus)

| Company | What They're Building | Link |
|:---|:---|:---|
| **Microsoft** | PyRIT, Azure AI Content Safety, Copilot red teaming | [Jobs](https://careers.microsoft.com/) |
| **NVIDIA** | garak, NeMo-Guardrails, AI security infrastructure | [Jobs](https://nvidia.com/careers) |
| **Cisco** | AI Defense platform, enterprise AI scanning | [Jobs](https://jobs.cisco.com/) |
| **CrowdStrike** | AI-powered threat detection, ML model security | [Jobs](https://crowdstrike.com/careers/) |
| **Wiz** | AI-SPM, shadow AI detection, cloud AI posture | [Jobs](https://wiz.io/careers) |

### Job Boards

| Board | Best For |
|:---|:---|
| **[80,000 Hours Job Board](https://jobs.80000hours.org/)** | AI safety and high-impact security roles |
| **[AI Jobs (aiml.to)](https://aiml.to/jobs)** | Specialized AI/ML roles |
| **[Glassdoor AI Security](https://www.glassdoor.com/)** | Salary verification + company culture |
| **[LinkedIn — AI Security filter](https://linkedin.com/)** | Volume + networking |
| **[Levels.fyi](https://levels.fyi/)** | Comp verification before negotiating |

### How to Stand Out

1. **Have a GitHub that shows you broke something** — a CTF writeup, a tool, a paper reproduction
2. **Contribute to garak, llm-guard, or NeMo-Guardrails** — open-source contributions signal depth
3. **Write publicly** — a blog post on a novel attack/defense pattern is worth more than any cert
4. **Know the papers** — every technical interview at a frontier lab will test whether you've actually read the relevant literature
5. **Speak the compliance language** — OWASP, NIST AI RMF, EU AI Act for enterprise roles; attack chains and ASR for lab roles

---

## 👥 Contributors & Acknowledgements

*   **[@ppradyoth](https://github.com/ppradyoth)** (Lead Maintainer) — AI Red Teaming & Security Engineering.
*   **[Antigravity 🌌](https://github.com/google-deepmind)** (AI Co-Architect) — Agentic coding assistant developed by Google DeepMind.

---

## 🤝 How to Contribute

This repo is better because practitioners like you make it better.

1. **Fork** this repository
2. **Add** a new tool, dataset, benchmark, paper, or resource with:
   - Clear description of what it is
   - Why it matters for AI security specifically
   - Which section it belongs in
3. **Keep it honest** — mark deprecated tools, flag if something is unmaintained, note commercial vs. open-source
4. **Submit a PR** with a descriptive summary

### What We're Looking For

- [ ] Novel attack papers published in 2025
- [ ] Production red team case studies
- [ ] Non-English resources (especially Chinese and French AI security research)
- [ ] Multimodal security resources (audio, video)
- [ ] Edge/on-device model security

---

*Maintained by [@ppradyoth](https://github.com/ppradyoth). Built to secure the future of AI — before AI secures us.*

---

<details>
<summary>📌 Quick Reference: Attack Taxonomy</summary>

| Attack Class | Subtype | Target Phase | Key Tool |
|:---|:---|:---|:---|
| **Prompt Injection** | Direct | Runtime | garak, llm-guard |
| **Prompt Injection** | Indirect (via RAG) | Runtime | AgentDojo, PromptArmor |
| **Jailbreak** | Suffix-based (GCG) | Runtime | llm-attacks |
| **Jailbreak** | Role-play / persona | Runtime | PyRIT |
| **Jailbreak** | Many-shot | Runtime | Manual |
| **Jailbreak** | Multilingual | Runtime | garak |
| **Jailbreak** | Multimodal (visual) | Runtime | FigStep |
| **Backdoor** | Data poisoning | Training | ART, Neural Cleanse |
| **Backdoor** | Fine-tune poisoning | Training | Manual |
| **Extraction** | Training data | Inference | Carlini et al. tools |
| **Extraction** | Model weights | Inference | Counterfit |
| **Extraction** | System prompt | Inference | Prompt leaking |
| **Evasion** | Gradient-based | Inference | cleverhans, ART |
| **Supply Chain** | Malicious model | Pre-deployment | modelscan |
| **Supply Chain** | Typosquatting | Pre-deployment | HF malware scanning |
| **Membership Inference** | Training data privacy | Post-training | ART |

</details>
