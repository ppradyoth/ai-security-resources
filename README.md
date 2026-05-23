# 🛡️ AI Security Resources

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

## 🧠 Why This Matters — The Origin Story

> *Understanding the "why" separates a technician from a practitioner.*

### The Trajectory That Created This Problem

In 1986, Rumelhart and Hinton proved backpropagation worked at scale. Nobody cared. In 2012, AlexNet won ImageNet by a margin so absurd that the computer vision community had to sit down. In 2017, Google dropped the Transformer paper and the entire field pivoted in 18 months.

Here's what actually happened between the papers:

- **1943 — McCulloch-Pitts Neuron:** First mathematical model of a neuron. Irrelevant until hardware caught up 70 years later.
- **1958 — Perceptron:** Rosenblatt's learning machine. Hyped, then killed by Minsky's proof that it couldn't do XOR.
- **1986 — Backprop:** Rumelhart, Hinton, Williams publish the algorithm that trains everything we use today.
- **1997 — LSTMs (Hochreiter & Schmidhuber):** Memory for sequences. Dominated NLP until attention killed it.
- **2012 — AlexNet:** GPUs + ReLU + dropout + scale = CNN dominance. 10.8% gap over #2. Game over for hand-crafted features.
- **2017 — "Attention Is All You Need":** Transformers. Self-attention. Parallel training. The architecture that scales infinitely.
- **2020 — GPT-3:** 175B parameters. Few-shot learning emerges as a property. Capabilities no one designed for start appearing.
- **2022 — ChatGPT:** 100M users in 60 days. Security teams globally had no playbook.
- **2023 → Now:** Agents, tool use, multimodal, real-time action. The attack surface is no longer just "the model" — it's every system the model can touch.

### Why Risks Evolved

The risks evolved because the **deployment context** changed faster than security thinking could follow:

| Era | Model Type | Threat Surface | Primary Risk |
|:---|:---|:---|:---|
| Pre-2020 | Narrow ML classifiers | Training pipelines | Data poisoning, model evasion |
| 2020–2022 | Foundation models | API endpoints | Prompt injection, output manipulation |
| 2022–2023 | RLHF-aligned LLMs | Consumer products | Jailbreaks, alignment bypass |
| 2023–2024 | RAG + Tool-use | Enterprise deployments | Indirect injection, agent hijacking |
| 2024+ | Agentic systems | **Everything they can touch** | Autonomous exploitation, cascading failures |

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

### Benchmarks & Evaluation

| Benchmark | Description | Metric |
|:---|:---|:---|
| **[HarmBench](https://github.com/centerforaisafety/HarmBench)** | Standardized evaluation framework for adversarial robustness. 400+ behaviors across 18 attack methods. | Attack Success Rate (ASR) |
| **[DecodingTrust](https://github.com/AI-Secure/DecodingTrust)** | 8-dimensional trustworthiness evaluation: toxicity, privacy, OOD robustness, machine ethics, fairness. | Per-dimension scores |
| **[RedBench / RedEval](https://github.com/knoveleng/redeval)** | Aggregates 37 safety benchmarks into unified taxonomy. 22 risk categories. | Standardized taxonomy |
| **[HELM (Holistic Evaluation of LMs)](https://crfm.stanford.edu/helm/)** | Stanford's broad LLM evaluation. Includes robustness and safety scenarios. | Multi-scenario scoring |
| **[WMDP (Weapons of Mass Destruction Proxy)](https://www.wmdp.ai/)** | Biosecurity/cybersecurity uplift evaluation. Tests whether models provide dangerous uplift. | Uplift measurement |
| **[CyberSecEval (Meta)](https://github.com/meta-llama/PurpleLlama/tree/main/CybersecurityBenchmarks)** | Cybersecurity-specific LLM evaluation. Secure code generation, vulnerability identification. | Security-specific scoring |
| **[WildJailbreak](https://allenai.org/)** | 262k training examples for adversarial robustness. Largest public safety dataset. | ASR on held-out set |

### Datasets for Training Defenses

| Dataset | Size | Description |
|:---|:---|:---|
| **[Anthropic hh-rlhf](https://huggingface.co/datasets/Anthropic/hh-rlhf)** | 160k | Foundational RLHF dataset. Human preference + red team conversations. |
| **[WildGuardMix](https://allenai.org/)** | 92k | Multi-task I/O guardrail evaluation dataset. |
| **[RealToxicityPrompts](https://github.com/allenai/real-toxicity-prompts)** | 100k | Standard toxicity evaluation prompts. |
| **[Do-Not-Answer](https://github.com/Libr-AI/do-not-answer)** | 939 | Curated questions responsible LLMs should decline. Evaluates refusal calibration. |
| **[AdvBench](https://github.com/llm-attacks/llm-attacks)** | 520 | Harmful behaviors and strings benchmark. Paired with the GCG attack code. |

### Governance, Standards & Frameworks

| Framework | Organization | Description |
|:---|:---|:---|
| **[NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)** | NIST | GOVERN-MAP-MEASURE-MANAGE structure for enterprise AI risk. Required for US government compliance. |
| **[EU AI Act](https://artificialintelligenceact.eu/)** | European Union | Risk-tiered regulation. High-risk AI systems face mandatory conformity assessments. Know this for enterprise sales. |
| **[OWASP Top 10 for LLM Applications 2025](https://owasp.org/www-project-top-10-for-large-language-model-applications/)** | OWASP | Updated for agentic AI. Prompt Injection, Excessive Agency, Vector/Embedding weaknesses. |
| **[Google Secure AI Framework (SAIF)](https://safety.google/cybersecurity-advancements/saif/)** | Google | 6-element framework for securing AI systems. Operationally focused. |
| **[Frontier Model Forum Best Practices](https://www.frontiermodelforum.org/)** | Industry | OpenAI + Anthropic + Google + Microsoft joint safety commitments. |
| **[ISO/IEC 42001 AI Management System](https://www.iso.org/standard/81230.html)** | ISO | The AI equivalent of ISO 27001. Enterprise AI governance certification. |

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

> *Structured for someone with 3 years of experience who wants to be genuinely formidable in AI Security within 12 months.*

### Where You Stand

With 3 years of security/ML experience, you already have:
- A security mindset (attacker/defender thinking)
- Working knowledge of a codebase and APIs
- Some exposure to ML concepts

What you need to build:
- Deep architectural understanding of LLMs
- Hands-on adversarial ML intuition
- Ability to think at the level of the threat model, not just the tool

---

### Phase 0 — Architectural Foundation (Weeks 1–4)

*Goal: Understand what you're attacking at a mechanistic level.*

**Week 1–2: How Transformers Actually Work**
- [ ] Watch [3Blue1Brown Neural Networks](https://www.youtube.com/playlist?list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi) (4 hours)
- [ ] Read [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/) (2 hours)
- [ ] Implement a transformer from scratch: [Karpathy's makemore series](https://www.youtube.com/playlist?list=PLAqhIrjkxbuWI23v9cThsA9GvCAUhRvKZ) (8 hours)
- [ ] Read [Attention Is All You Need](https://arxiv.org/abs/1706.03762) — not to learn the architecture, but to see what was *not* said about safety

**Week 3: The First Adversarial ML Papers**
- [ ] Read [Intriguing Properties of Neural Networks (Szegedy, 2013)](https://arxiv.org/abs/1312.6199) — the moment adversarial examples were discovered
- [ ] Read [Explaining and Harnessing Adversarial Examples (Goodfellow, 2014)](https://arxiv.org/abs/1412.6572) — FGSM, the foundation
- [ ] Implement FGSM on an image classifier. *Feel* what it means to add imperceptible noise that breaks a model.

**Week 4: LLM Security Context**
- [ ] Read [Universal and Transferable Attacks (Zou et al., 2023)](https://arxiv.org/abs/2307.15043) — this is the FGSM moment for LLMs
- [ ] Read [Jailbroken (Wei et al., 2023)](https://arxiv.org/abs/2307.02483) — the two failure modes of alignment
- [ ] Read [Prompt Injection Against Integrated Applications (Greshake, 2023)](https://arxiv.org/abs/2302.12173) — the agentic threat model

---

### Phase 1 — Hands-On Attacks (Weeks 5–8)

*Goal: Build calluses. Every attack you understand by doing becomes intuition you keep forever.*

**Week 5: CTF Time**
- [ ] Complete all 8 levels of [Lakera Gandalf](https://gandalf.lakera.ai/) — document every technique you use
- [ ] Do 3 challenges on [Crucible by Dreadnode](https://crucible.dreadnode.io/)
- [ ] Participate in [HackAPrompt](https://www.hackaprompt.com/) or review past solutions

**Week 6: Tool Mastery — Garak**
- [ ] Install [garak](https://github.com/NVIDIA/garak) and run it against an LLM you control
- [ ] Run all probe categories: `promptinject`, `jailbreak`, `knowledgegrabs`, `leakage`
- [ ] Write a custom probe for a vulnerability you discovered in Week 5

**Week 7: Tool Mastery — PyRIT**
- [ ] Set up [PyRIT](https://github.com/microsoft/PyRIT) with an Azure or OpenAI endpoint
- [ ] Build a multi-turn attack chain against a system prompt you define
- [ ] Implement an orchestrator that iteratively refines attacks based on model responses

**Week 8: Agentic Red Teaming**
- [ ] Build a simple LLM agent with tool access (file read, web search)
- [ ] Inject adversarial content into the tool responses (indirect injection)
- [ ] Use [AgentDojo](https://github.com/ethz-spylab/agentdojo) to benchmark your findings

---

### Phase 2 — Defense Engineering (Weeks 9–12)

*Goal: You cannot build defenses you haven't broken. Now you know what to defend.*

**Week 9: Guardrail Engineering**
- [ ] Deploy [NeMo-Guardrails](https://github.com/NVIDIA/NeMo-Guardrails) + [llm-guard](https://github.com/protectai/llm-guard)
- [ ] Attack your own guardrails. Document what bypasses them.
- [ ] Implement [Llama Guard](https://github.com/meta-llama/PurpleLlama) as a secondary classifier

**Week 10: Observability & Detection**
- [ ] Set up [Langfuse](https://github.com/langfuse/langfuse) and instrument an LLM app
- [ ] Implement statistical monitoring with [LangKit](https://github.com/whylabs/langkit)
- [ ] Write an anomaly detection pipeline that flags unusual prompt patterns

**Week 11: Model Scanning**
- [ ] Use [modelscan](https://github.com/protectai/modelscan) on 10 models from Hugging Face
- [ ] Convert a model from pickle to safetensors — understand why format matters
- [ ] Read [Sleeper Agents (Anthropic, 2024)](https://arxiv.org/abs/2401.05566) and think through detection challenges

**Week 12: Write and Share**
- [ ] Write a blog post or GitHub report documenting an attack chain you discovered
- [ ] Submit a finding to an AI company's vulnerability disclosure program
- [ ] Contribute a tool, resource, or write-up to this repo

---

### Phase 3 — Specialization (Months 4–6)

Choose your path based on what energized you most in Phases 0–2:

#### Path A: Research Track
- Study [mechanistic interpretability](https://transformer-circuits.pub/) — understanding circuits enables novel attacks
- Reproduce a paper from HarmBench or MITRE ATLAS
- Submit to ML security workshops: [AdvML @ NeurIPS](https://advml-frontier.github.io/), [SaTML](https://satml.org/)

#### Path B: Engineering Track
- Build a CI/CD pipeline with automated red teaming (Promptfoo + garak)
- Implement an AI security posture management (AI-SPM) system
- Pursue [CAISP certification](https://www.aigov.institute/certifications)

#### Path C: Research + Engineering (Recommended)
- Alternate between paper reproduction and production implementation
- Nothing keeps you grounded like shipping something real between reading papers

---

### Phase 4 — Expert (Months 6–12)

- Contribute to MITRE ATLAS (submit case studies)
- Speak at AI Village (DEF CON) or SaTML
- Publish a tool or dataset that others use
- The people who get to principal/staff in this field have *something they built* that others rely on

---

### Certifications Worth Having

| Certification | Org | Signal | Time |
|:---|:---|:---|:---|
| **[Certified AI Security Professional (CAISP)](https://www.aigov.institute/certifications)** | AI Gov Institute | Practitioner-level AI security. Best available. | 60–80 hrs |
| **[GIAC GREM](https://www.giac.org/certifications/reverse-engineering-malware-grem/)** | SANS | Reverse engineering. Useful for model weight analysis. | 120 hrs |
| **[Google Professional ML Engineer](https://cloud.google.com/learn/certification/machine-learning-engineer)** | Google | ML fundamentals signal. Good for bridging to employers. | 40 hrs |
| **[AWS ML Specialty](https://aws.amazon.com/certification/certified-machine-learning-specialty/)** | AWS | Cloud ML deployment. Covers security of deployed models. | 60 hrs |
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

> Numbers are honest market estimates. This field commands a **30–56% premium** over generalist SWE/security roles globally — because the supply of people who genuinely understand both AI architecture and adversarial security is tiny.

---

#### 🇺🇸 United States

The global reference market. All comp figures include base + target bonus. "Total comp" at frontier labs includes RSUs.

| Level | Approx. Experience | Base Salary | Total Comp (Frontier Lab) | Notes |
|:---|:---|:---|:---|:---|
| **Junior / L3** | 1–3 yrs | $110k–$150k | $140k–$200k | Entry at OpenAI/Anthropic starts here with strong ML background |
| **Mid / L4** | 3–6 yrs | $150k–$200k | $200k–$320k | Most practitioners land here within 2–3 yrs of specialization |
| **Senior / L5** | 6–9 yrs | $200k–$270k | $300k–$480k | Requires owning a domain (e.g., agentic red teaming, model scanning) |
| **Staff / L6** | 9+ yrs | $250k–$320k | $400k–$700k+ | Rare. Requires published impact or a tool the field depends on |
| **Research Scientist** | PhD + 3 yrs | $220k–$300k | $400k–$800k+ | Labs pay for publication track record and capability research |

*Sources: [Levels.fyi](https://levels.fyi), [Blind](https://teamblind.com), [LinkedIn Salary](https://linkedin.com/salary)*

**City multipliers (vs. national average):**
- San Francisco / Bay Area: +30–45%
- New York City: +20–30%
- Seattle: +15–25%
- Austin / Remote: ±5% (many labs now pay location-adjusted)

---

#### 🇮🇳 India

The fastest-growing AI security talent market. Roles are concentrated in **Bengaluru, Hyderabad, and Pune** at MNCs and Global Capability Centers (GCCs). Product companies and funded startups pay 2–3× more than IT services firms for the same title.

| Level | Approx. Experience | IT Services (TCS/Infosys/Wipro) | MNC / GCC (Google, Microsoft, Cisco) | AI-Native Startup / Lab |
|:---|:---|:---|:---|:---|
| **Junior** | 0–3 yrs | ₹8–14 LPA | ₹15–22 LPA | ₹12–20 LPA |
| **Mid-Level** | 3–6 yrs | ₹14–22 LPA | ₹22–40 LPA | ₹25–45 LPA |
| **Senior** | 6–9 yrs | ₹22–35 LPA | ₹40–70 LPA | ₹45–80 LPA |
| **Staff / Principal** | 9+ yrs | ₹35–55 LPA | ₹70–120 LPA | ₹80–150+ LPA |
| **Research Scientist** | PhD / strong pubs | Uncommon | ₹60–120 LPA | ₹80–200+ LPA |

*LPA = Lakhs Per Annum (₹1 LPA = ₹100,000). Sources: [6figr.com](https://6figr.com), [Glassdoor India](https://glassdoor.co.in), [AmbitionBox](https://ambitionbox.com), [Naukri Salary Tool](https://naukri.com)*

**City context:**
- **Bengaluru:** Highest AI salaries. GCCs + product companies dominant. Mid-level AI security: ₹25–40 LPA typical.
- **Hyderabad:** GCCs growing fast (Microsoft, Google, Amazon R&D centers). Near-Bengaluru comp.
- **Mumbai:** BFSI (banking/finance) sector heavy. High demand for AI risk and model governance roles.
- **Pune:** MNC presence, slightly lower than Bengaluru/Hyderabad by 10–15%.
- **NCR (Delhi/Gurugram/Noida):** Strong consulting and enterprise demand (KPMG, Deloitte, EY AI practices).

**Important caveats for India:**
- "AI Security" as an explicit title is still emerging. Budget for 6–18 months of the role existing under a different name before your actual work matches.
- MNC comp often includes ESOPs/RSUs that vest over 4 years — evaluate total compensation, not just CTC.
- Switching companies every 2 years is the fastest path to market rate. Indian salary increments (8–12% p.a.) do not keep pace with role scarcity premium.
- GCCs from US labs (like Google DARS, Microsoft IDC, Cisco security research) offer closest to international parity.

**Who's actively hiring in India (2025–2026):**
Google, Microsoft, Cisco, KPMG, Deloitte, NatWest Group (AI risk), Zscaler, Protectt.ai, SISA, BlueFireRedteam, Parai Labs, and a growing number of AI safety annotation/red team annotation platforms (Mindrift, Mercor, Scale AI India).

---

#### 🇬🇧 United Kingdom

Strong AI security ecosystem anchored in **London** and **Cambridge**. The UK government's AI Safety Institute (AISI) is a major employer. Post-Brexit, some talent flow to EU has increased competition for experienced practitioners.

| Level | Approx. Experience | Base Salary (GBP) | Total Comp with Bonus/RSU |
|:---|:---|:---|:---|
| **Junior** | 0–3 yrs | £45k–£65k | £50k–£80k |
| **Mid-Level** | 3–6 yrs | £65k–£90k | £80k–£120k |
| **Senior** | 6–9 yrs | £90k–£130k | £115k–£180k |
| **Staff / Principal** | 9+ yrs | £130k–£175k | £175k–£280k+ |
| **Research Scientist** | PhD + publications | £90k–£150k | £120k–£250k+ |

*Sources: [LinkedIn UK Salary](https://linkedin.com), [CW Jobs](https://cwjobs.co.uk), [Glassdoor UK](https://glassdoor.co.uk), [ITJobsWatch](https://itjobswatch.co.uk)*

**Notable employers:** UK AI Safety Institute (AISI), DeepMind London, Anthropic (London office), Darktrace, BAE Systems AI, GCHQ / NCSC (government), Palantir UK, big 4 (AI risk practices).

---

#### 🇮🇪 Ireland

Dublin is the undisputed primary hub for European tech, acting as the EMEA headquarters for the world’s largest tech giants. This creates a highly bifurcated market: Tier-1 US multinationals (FAANG, Stripe, Salesforce) pay exceptional, stock-heavy packages that align with global standards, while local tech companies, fintechs, and consulting firms offer competitive European-level base salaries.

| Level | Approx. Experience | Base Salary (EUR) | Total Comp (US MNC / Tier 1) | Notes |
|:---|:---|:---|:---|:---|
| **Junior** | 0–3 yrs | €50k–€70k | €65k–€95k | Common for entry-level security analysts or junior ML engineers at large MNCs. |
| **Mid-Level** | 3–6 yrs | €70k–€100k | €100k–€150k | High demand for GRC (AI Act/GDPR) and system-level security engineers. |
| **Senior** | 6–9 yrs | €100k–€140k | €160k–€240k | High equity components in Big Tech HQs skew total comp significantly. |
| **Staff / Principal** | 9+ yrs | €140k–€190k | €250k–€380k+ | Very rare roles that often report directly to US engineering leadership. |

*Sources: [Levels.fyi Dublin](https://www.levels.fyi), [Morgan McKinley Ireland Tech Salary Guide](https://www.morganmckinley.com), [CPL Salary Survey](https://www.cpl.com), [Glassdoor Ireland](https://www.glassdoor.ie)*

**City context & Talent Hubs:**
- **Dublin ("Silicon Docks"):** The powerhouse of the Irish tech ecosystem. Over 90% of specialized AI security and high-end MLE/Security roles are concentrated in Dublin (D1, D2, D4, and Sandyford/D18).
- **Cork:** A growing secondary hub driven by Apple's massive EMEA campus, McAfee, and Dell, focusing on hardware-level security, systems engineering, and enterprise SaaS.
- **Galway & Limerick:** Emerging research and engineering satellites with centers for companies like Cisco (Galway) and Analog Devices (Limerick).

**Important caveats for Ireland:**
- **The "EMEA HQ" Premium:** Because Dublin hosts the European headquarters for major US tech corporations, Tier-1 employers pay an average 1.8x to 2.5x multiplier on Total Compensation (TC) compared to local Irish firms, largely via equity (RSUs) and sign-on/performance bonuses.
- **Aggressive Marginal Taxation:** High earners in Ireland hit the top tax bracket very quickly. Single individuals pay a 40% marginal PAYE rate on income over €44,000 (for 2026), plus the Universal Social Charge (USC) up to 8% and PRSI (4%). The effective tax rate on high senior/staff salaries easily approaches 50%.
- **The Housing Crisis Bottleneck:** Dublin is experiencing one of the most severe housing and rental shortages in Europe. Rent for a basic 1-bed apartment in Dublin ranges from €1,900 to €2,600/month, and finding accommodation is extremely difficult. Consequently, hybrid flexibility or fully remote work options within Ireland are highly valued and negotiated.
- **GDPR and AI Act Regulatory Nexus:** Because the Irish Data Protection Commission (DPC) is the lead supervisory authority for most US tech firms in the EU, there is a disproportionately high density of roles focusing on **AI Governance, AI Compliance, Privacy-Preserving ML, and Auditing** in Dublin relative to its size.

**Who's actively hiring in Ireland (2025–2026):**
- **US Tech Giants:** Google (EMEA HQ / Trust & Safety), Meta (EMEA HQ / AI Safety & Integrity), Microsoft, AWS, Salesforce, LinkedIn.
- **Fintech & Platforms:** Stripe (highly competitive infrastructure, fraud, and ML security teams), Mastercard Technology Hub, Workday.
- **Dedicated Security Firms:** Zscaler, CrowdStrike, Sysdig, Integrity360 (local security market leader), Tenable.
- **Enterprise Consulting:** PwC, Deloitte, EY, KPMG (actively building out AI risk assessment and AI Act advisory teams).

---

#### 🇩🇪 Germany

Strong automotive + enterprise AI sector. **Munich** and **Berlin** are the primary hubs. German market values depth over breadth — specialization pays disproportionately well.

| Level | Base Salary (EUR) | Notes |
|:---|:---|:---|
| **Junior (0–3 yrs)** | €50k–€70k | Entry is solid. Apprenticeship culture means companies invest in training. |
| **Mid-Level (3–6 yrs)** | €70k–€100k | AI security as explicit role growing in automotive (BMW, Bosch, SAP). |
| **Senior (6–9 yrs)** | €100k–€140k | Top 25% of market. Munich tends to pay 10–15% more than Berlin. |
| **Staff / Principal (9+)** | €140k–€200k+ | Rare at this level; research institutes (Fraunhofer, DFKI) also compete. |

*Note: German salaries are gross. After taxes and social contributions (approx. 40–45% for high earners), net take-home is significantly lower — offset by strong social benefits (healthcare, pension, unemployment insurance).*

---

#### 🇳🇱 Netherlands

Amsterdam tech hub with significant international talent due to the **30% tax ruling** — expats may pay tax on only 70% of their salary for up to 5 years, making nominal salaries competitive globally.

| Level | Base Salary (EUR) | Effective Net (with 30% ruling) |
|:---|:---|:---|
| **Mid-Level (3–6 yrs)** | €70k–€100k | Significantly higher than comparable UK/Germany net take-home |
| **Senior (6–9 yrs)** | €95k–€135k | Very competitive globally when ruling applies |
| **Staff / Principal (9+)** | €130k–€180k+ | Top tier. ASML, Booking.com, Elastic hiring at this level. |

---

#### 🇫🇷 France

**Paris** AI ecosystem anchored by Mistral AI, Meta AI (FAIR Paris), Hugging Face, and a growing government-backed AI safety initiative (ANSSI for AI). Salaries lower than UK/Germany/Netherlands but rising fast — and cost of living in Paris is lower than London.

| Level | Base Salary (EUR) |
|:---|:---|
| **Mid-Level (3–6 yrs)** | €60k–€85k |
| **Senior (6–9 yrs)** | €85k–€120k |
| **Staff / Principal (9+)** | €120k–€160k+ |

*Mistral AI is the standout French employer for AI security/safety roles. ANSSI (Agence Nationale de la Sécurité des Systèmes d'Information) is the government employer with AI security mandates.*

---

#### 🇨🇭 Switzerland

Consistently the highest **gross** salaries in Europe. Zurich and Basel anchored by finance and pharma. EPFL/ETH Zurich AI research ecosystem. High cost of living partially offsets high gross.

| Level | Base Salary (CHF) | USD Equivalent (approx.) |
|:---|:---|:---|
| **Mid-Level (3–6 yrs)** | CHF 110k–160k | ~$120k–$175k |
| **Senior (6–9 yrs)** | CHF 160k–220k | ~$175k–$240k |
| **Staff / Principal (9+)** | CHF 220k–350k+ | ~$240k–$385k+ |

---

#### 🇸🇪 Sweden

Stockholm tech scene. Ericsson, Spotify, Klarna, and a growing AI startup ecosystem. High taxes (~50% marginal rate for high earners) mean evaluating total package — but strong social benefits compensate.

| Level | Base Salary (SEK) | USD Equivalent (approx.) |
|:---|:---|:---|
| **Mid-Level (3–6 yrs)** | SEK 650k–850k | ~$62k–$82k |
| **Senior (6–9 yrs)** | SEK 850k–1.1M | ~$82k–$106k |
| **Staff / Principal (9+)** | SEK 1.1M–1.5M+ | ~$106k–$145k+ |

---

#### 🇵🇱 Poland + Eastern Europe

**Warsaw, Kraków, Wrocław** are major nearshoring hubs. Many global companies run significant security and ML engineering centres here. B2B contracting (rather than employment) is common and can increase effective income by 20–30%.

| Level | Poland (PLN/year) | Eastern Europe avg (EUR/year) |
|:---|:---|:---|
| **Mid-Level (3–6 yrs)** | PLN 200k–300k (~€46k–€70k) | €45k–€65k |
| **Senior (6–9 yrs)** | PLN 300k–420k (~€70k–€98k) | €60k–€80k |
| **Staff / Principal (9+)** | PLN 400k–600k+ (~€93k–€140k) | €75k–€100k+ |

*Many senior Polish engineers working for US/UK companies remotely can earn at near-Western rates. B2B contracts are standard — negotiate as a contractor, not an employee.*

---

#### 🌏 Asia-Pacific

| Country | Level | Salary Range | Notes |
|:---|:---|:---|:---|
| **🇸🇬 Singapore** | Mid | SGD $120k–$180k | Premier APAC hub. Government-backed AI National Strategy drives demand. Tax rate ~22% max — very competitive net. |
| **🇸🇬 Singapore** | Senior | SGD $180k–$280k | MAS (financial regulator) AI risk roles; GovTech; DBS, OCBC (banks with AI teams). |
| **🇸🇬 Singapore** | Principal | SGD $280k–$400k+ | Rare but exists at frontier lab APAC hubs. |
| **🇦🇺 Australia** | Mid | AUD $130k–$180k | Strong demand in finance, mining, government. |
| **🇦🇺 Australia** | Senior | AUD $180k–$260k | Severe talent shortage at senior/principal level. Contractors: AUD $1,200–$1,800/day. |
| **🇯🇵 Japan** | Mid | ¥8M–¥14M | Growing fast. Language barrier limits global pool — premium for bilingual AI security engineers. |
| **🇰🇷 South Korea** | Mid | KRW 70M–100M | Samsung, Kakao, Naver investing heavily in AI security R&D. |

---

#### 🇨🇦 Canada

Strong AI research cluster in **Toronto** (Vector Institute), **Montreal** (Mila), and **Vancouver**. Close proximity to US market creates competitive pressure — companies pay above typical Canadian SWE rates to retain talent that could easily work remotely for US companies.

| Level | Base Salary (CAD) | Notes |
|:---|:---|:---|
| **Mid-Level (3–6 yrs)** | CAD $110k–$160k | US labs with Canadian offices pay at US-adjacent rates |
| **Senior (6–9 yrs)** | CAD $160k–$220k | Mila-adjacent research roles + Vector Institute affiliates |
| **Staff / Principal (9+)** | CAD $200k–$290k+ | Cohere, Waabi, and other Canadian AI labs at this level |

---

#### 💡 Reading Salary Data Like a Professional

1. **Always convert to net take-home, not gross** — Switzerland's CHF 200k and Germany's €150k sound similar, but net income differs significantly after taxes and social contributions. Tools: [smartasset.com](https://smartasset.com) (US), [germanysalary.com](https://www.germanysalary.com) (DE), [income-tax.co.uk](https://income-tax.co.uk) (UK).

2. **Total compensation > base salary** — RSUs (Restricted Stock Units) at Anthropic, OpenAI, or Google can easily double your base salary. Always ask the vesting schedule, cliff, and refresh policy.

3. **"AI Security" as a title is new, the comp is not** — negotiate based on the *intersection* of what you bring (security depth + ML depth), not the title. You are solving a problem most companies don't even have a budget line for yet — price accordingly.

4. **Remote-first companies have equalized significantly** — OpenAI, Anthropic, Lakera, Protect AI all hire globally with compensation that varies but is increasingly global-range for exceptional candidates.

5. **The India-to-global pipeline is real** — strong Indian AI security practitioners are being hired directly by US/UK labs (remote or relocated). If you are in India with genuine adversarial ML depth, you are competing in a global market, not just a domestic one.

*Data sources: [Levels.fyi](https://levels.fyi), [Glassdoor](https://glassdoor.com), [LinkedIn Salary](https://linkedin.com/salary), [6figr.com](https://6figr.com), [AmbitionBox](https://ambitionbox.com), [ITJobsWatch](https://itjobswatch.co.uk), [swissdevjobs.ch](https://swissdevjobs.ch), [Blind](https://teamblind.com). All figures are 2025–2026 estimates — actual offers vary based on company, individual negotiation, and role scope.*

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
