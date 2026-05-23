# AI Security Standards, Compliance & Regulatory Frameworks

As AI systems move from experimental prototypes to mission-critical enterprise production, security auditing and compliance have become primary requirements. Security architects and compliance officers must navigate a rapidly evolving landscape of AI-specific threat matrices and international regulations.

This document serves as the definitive reference for AI security standards, risk frameworks, and regulatory compliance.

---

## 🗺️ MITRE ATLAS (Adversarial Threat Landscape for Artificial-Intelligence Systems)

[MITRE ATLAS](https://atlas.mitre.org/) is the absolute industry-standard framework for threat modeling AI systems. Modeled after the classic MITRE ATT&CK framework, ATLAS maps the specific tactics, techniques, and procedures (TTPs) used by real-world adversaries to compromise Machine Learning systems.

### The ATLAS Tactical Matrix
ATLAS organizes attack techniques into 12 core tactics representing the logical progression of an AI compromise:

1.  **Reconnaissance**: Gathering information on target model architectures, data sources, API endpoints, and training pipelines.
2.  **Resource Development**: Acquiring infrastructure, poisoning datasets, or building specialized adversarial attack tools.
3.  **Initial Access**: Gaining a foothold through compromised Hugging Face tokens, supply chain poisoning, or prompt injection endpoints.
4.  **Execution**: Launching adversarial prompts, executing code via malicious model serialization (e.g., pickle exploits), or invoking unauthorized agent tools.
5.  **Persistence**: Injecting persistent backdoors in training data, fine-tuning checkpoints, or system prompts.
6.  **Privilege Escalation**: Escalating from an unauthenticated LLM user to invoking administrator-level OS commands via tool calling.
7.  **Defense Evasion**: Evading detection by adding subtle perturbations to inputs (evasion attacks) or formatting malicious payloads in Base64/multilingual scripts.
8.  **Credential Access**: Extracting API keys, Hugging Face tokens, or developer credentials stored in model registries or environment variables.
9.  **Discovery**: Mapping the internal network, discovering model endpoints, scanning connected vector databases, and listing active agent tools.
10. **Lateral Movement**: Moving from a compromised LLM orchestration layer to the underlying cloud Kubernetes clusters or corporate databases.
11. **Collection**: Gathering sensitive training data, system prompts, model weights, or user conversation logs.
12. **Exfiltration**: Exfiltrating stolen model weights or proprietary data via model output leaks (steganography) or traditional network channels.
13. **Impact**: Denying service by flooding model endpoints (AI-specific DoS), corrupting model weights, or manipulating outputs to cause reputational damage.

### 💡 Tactical Application: How to Use MITRE ATLAS for Threat Modeling
*   **Step 1: Map the AI Pipeline Asset Surface**: Define where data is ingested, where model weights are stored (e.g., MLflow, Hugging Face), where inference occurs, and what external tools/APIs the model can invoke.
*   **Step 2: Walk Through the Matrix**: For each asset, ask: *"How could an attacker exploit this technique at this stage?"* (e.g., *T1566: Poison Training Data* during the data ingestion phase).
*   **Step 3: Document Mitigations**: Map each identified threat to specific defensive controls (e.g., cryptographic hashing of datasets, runtime prompt firewalls).

---

## 🛡️ OWASP Top 10 for LLM Applications

The [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/) represents the foundational vulnerabilities that application security teams must mitigate when building LLM-powered applications.

| Vulnerability Class | Core Threat | Primary Defense |
|:---|:---|:---|
| **LLM01: Prompt Injection** | Adversarial inputs manipulate the LLM into executing unauthorized actions or leaking system rules (Direct/Indirect). | Enforce strict input sanitization, separate data from instruction context, and use runtime LLM firewalls. |
| **LLM02: Insecure Output Handling** | Accepting LLM-generated output without validation, leading to XSS, CSRF, or Remote Code Execution (RCE) in downstream systems. | Treat all LLM outputs as untrusted user inputs. Encode outputs before rendering and validate prior to execution. |
| **LLM03: Training Data Poisoning** | Malicious corruption of pre-training or fine-tuning datasets to introduce backdoors, bias, or security bypasses. | Verify data sources, cryptographically sign datasets, audit labels, and run adversarial evaluations post-training. |
| **LLM04: Model Denial of Service** | Resource-heavy queries (e.g., massive context windows, recursive tool calling) that exhaust GPU memory and drive up API billing. | Enforce strict rate limits, restrict maximum prompt token size, cap agent loop execution counts, and monitor GPU utilization. |
| **LLM05: Supply Chain Vulnerabilities** | Compromised base models, malicious third-party datasets, compromised PyPI packages, or hijacked model registries. | Scan model weights for serialization exploits (e.g., using `safetensors`), pin library dependencies, and sign model artifacts. |
| **LLM06: Sensitive Information Disclosure** | The LLM revealing proprietary data, PII, system prompts, or trade secrets in its generated responses. | Sanitize pre-training data, use system prompt defenses, and deploy active PII detection/scrubbing layers on outputs. |
| **LLM07: Insecure Plugin Design** | LLM plugins/tools accepting raw text inputs from the model without validation, allowing attackers to hijack underlying APIs. | Design plugins to require structured parameters (e.g., strict JSON schema), avoid dynamic shell execution, and enforce OAuth. |
| **LLM08: Excessive Agency** | Granting LLMs broad permissions, high privileges, or autonomous execution rights without human-in-the-loop checks. | Follow the principle of least privilege. Do not allow LLMs to write code and execute it directly without user confirmation. |
| **LLM09: Overreliance** | Blindly trusting LLM outputs (code, security advice, facts) without validation, leading to software supply chain vulnerabilities. | Implement automated static/dynamic code analysis (SAST/DAST) on all LLM-generated code before deployment. |
| **LLM10: Model Theft** | Exfiltrating proprietary model weights or copying model functionality through recursive API querying (distillation). | Enforce API rate-limiting, monitor output query patterns for extraction signatures, and secure model storage registries. |

---

## 🏛️ NIST Artificial Intelligence Risk Management Framework (NIST AI RMF 1.0)

Released by the U.S. National Institute of Standards and Technology, the [NIST AI RMF](https://www.nist.gov/itl/ai-risk-management-framework) is a voluntary organizational framework designed to help enterprises design, deploy, and govern trustworthy and secure AI systems.

### The Four Pillars of AI Governance
The framework is structured around four continuous core functions:

```
                  ┌───────────────┐
                  │    GOVERN     │
                  └───────┬───────┘
            ┌─────────────┼─────────────┐
            ▼             ▼             ▼
      ┌───────────┐ ┌───────────┐ ┌───────────┐
      │    MAP    │ │  MEASURE  │ │  MANAGE   │
      └───────────┘ └───────────┘ └───────────┘
```

1.  **GOVERN**: Build a culture of AI risk management. Establish clear internal policies, assign accountability, define ethical guidelines, and train teams on security and compliance mandates.
2.  **MAP**: Categorize the AI systems, identify the target users, document the system's operational boundaries, and map out the potential security and compliance risks.
3.  **MEASURE**: Conduct rigorous evaluations. Run quantitative testing (e.g., automated red teaming, bias assessment, latency tests) to verify the system's security, privacy, and safety posture.
4.  **MANAGE**: Allocate resources to continuously mitigate identified risks. Deploy runtime guardrails, implement fallback architectures, and set up continuous monitoring to handle failures.

---

## 🌐 ISO/IEC 42001:2023 (Artificial Intelligence Management System - AIMS)

Released in late 2023, [ISO/IEC 42001](https://www.iso.org/standard/81230.html) is the world's first international standard for certifying AI Management Systems. Similar to ISO 27001 (Information Security), it provides a structured process for organizations to manage AI risks throughout the product lifecycle.

### Core Areas of ISO 42001 Compliance
*   **System Integrity**: Organizations must demonstrate controls to protect the confidentiality, integrity, and availability of AI assets (data, weights, models, code).
*   **Data Governance**: Strict requirements for verifying training data provenance, ensuring fair collection practices, and protecting data privacy.
*   **Transparency and Explainability**: Documented processes to ensure that AI model outputs can be traced, explained, and audited when errors or security incidents occur.
*   **Lifecycle Management**: Defining risk assessments at every stage—from feasibility research and data acquisition to model development, testing, deployment, and eventual retirement.

---

## 🇪🇺 The EU AI Act Compliance Playbook (2025–2026)

The **European Union Artificial Intelligence Act (EU AI Act)** is the world's first comprehensive legal framework regulating AI. It operates on a strict, risk-based classification system, imposing severe penalties (up to €35M or 7% of global annual turnover) for non-compliance.

### Risk Categorization & Obligations

```
 ┌─────────────────────────────────────────────────────────────┐
 │  UNACCEPTABLE RISK (BANNED)                                 │
 │  e.g., Social scoring, biometric categorization, dark-patterns│
 ├─────────────────────────────────────────────────────────────┤
 │  HIGH RISK (STRICT AUDITS)                                  │
 │  e.g., Critical infrastructure, hiring, biometrics, healthcare│
 ├─────────────────────────────────────────────────────────────┤
 │  GENERAL PURPOSE AI (GPAI / FRONTIER LABS)                 │
 │  e.g., Base LLMs (Llama 3, GPT-4, Claude 3)                 │
 ├─────────────────────────────────────────────────────────────┤
 │  MINIMAL RISK (LOW REGULATION)                              │
 │  e.g., Spam filters, video games, basic classification       │
 └─────────────────────────────────────────────────────────────┘
```

#### 🚫 Unacceptable Risk (Banned)
*   **Systems**: Real-time remote biometric identification in public spaces, cognitive behavioral manipulation (e.g., voice-activated toys encouraging dangerous behavior), social scoring, and untargeted scraping of facial images from the internet.
*   **Compliance Timeline**: Banned entirely within 6 months of entry into force.

#### ⚠️ High Risk (Strict Compliance Required)
*   **Systems**: AI used in critical infrastructure (water, electricity), CV sorting software for hiring, biometric identification, grading exams, credit scoring, and law enforcement.
*   **Compliance Requirements**:
    1.  **Rigorous Risk Management**: Establish a continuous, documented risk management system.
    2.  **High-Quality Datasets**: Training, validation, and testing datasets must be checked for bias, poisoning, and data-governance violations.
    3.  **Technical Documentation**: Maintain detailed system logs and architecture documents to prove compliance.
    4.  **Logging Capability**: Automatically log events (audit trails) throughout the system's operational lifetime.
    5.  **Human Oversight**: Design systems with fail-safes and manual intervention overrides ("Human-in-the-Loop").
    6.  **Accuracy, Robustness, and Cybersecurity**: Systems must achieve high levels of adversarial robustness against inputs designed to alter performance (jailbreaks, poisoning, data exploitation).

#### 🧠 General Purpose AI (GPAI / Foundation Models)
*   **Systems**: Base foundation models trained on massive data (like GPT-4, Llama 3, Claude 3).
*   **Compliance Requirements**:
    *   Draw up technical documentation (e.g., training processes, evaluation results).
    *   Publish a detailed summary of the content used to train the model.
    *   Respect EU copyright law.
*   **Models with Systemic Risk** (Models trained on > $10^{25}$ FLOPs, e.g., frontier models):
    *   Must conduct rigorous model evaluations (red teaming).
    *   Must perform adversarial risk assessments.
    *   Must report energy efficiency metrics.
    *   Must implement state-of-the-art cybersecurity protections throughout model training and deployment.

---

## 📝 Enterprise AI Security Hardening Checklist

To protect and prepare your AI systems for compliance, implement this technical hardening baseline:

- [ ] **Weight Protection**: Store model weights in a secure, encrypted object registry (e.g., AWS S3 with KMS). Restrict access using strict IAM roles. Never store model registry API tokens in plaintext in code.
- [ ] **Malicious Serialization Defense**: Disable the loading of unverified models via `pickle`. Enforce the use of safe file formats (like `safetensors`) which prevent arbitrary code execution during loading.
- [ ] **Data Provenance Checks**: Cryptographically sign raw training data (`sha256` or GPG). Run outlier-detection algorithms on all ingested datasets to screen for data poisoning and backdoors.
- [ ] **Downstream Output Isolation**: Treat all model responses as untrusted. Enforce HTML-encoding, output sanitization, and strictly typed JSON schemas before passing outputs to downstream applications, databases, or APIs (prevents indirect SQLi/XSS).
- [ ] **Excessive Agency Restriction**: Set maximum loop boundaries on all autonomous agent runs. Run tool execution in isolated, read-only Docker containers or serverless runtimes. Implement explicit human approval prompts for high-risk actions (e.g., database writes, emails).
- [ ] **Pipeline Dependency Pinning**: Lock all package dependencies using a package-lock system (`requirements.txt.lock`, `pnpm-lock.yaml`). Run automated software composition analysis (SCA) to detect compromised Python packages before builds.
- [ ] **Runtime Guardrails**: Deploy a runtime guardrail framework (e.g., Llama Guard, NeMo Guardrails) to intercept prompts and output generations, filtering for PII, toxic content, and prompt injection signatures.
- [ ] **Strict Prompt Logging & Auditing**: Maintain a secure, tamper-proof audit trail of all prompts, outputs, agent decisions, and tool executions to comply with high-risk system obligations under international law.
