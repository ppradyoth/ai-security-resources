# AI Hacking Playgrounds, CTFs & Incident Databases

The fastest way to understand adversarial machine learning is hands-on exploitation. Similarly, analyzing historical vulnerabilities and real-world security failures is crucial for developing robust defense strategies.

This document collects and maps interactive hacking labs, prompt injection CTFs, vulnerability registries, and real-world case studies in AI security.

---

## 🎮 Interactive Hacking Playgrounds & CTFs

Interactive playgrounds allow developers, security researchers, and enthusiasts to safely practice prompt injection, jailbreaking, and bypass techniques against real running models.

### 🔓 Prompt Injection Games & Challenges

#### [Gandalf (by Lakera)](https://gandalf.lakera.ai/)
*   **What it is**: The most famous multi-level prompt injection game.
*   **The Mission**: Gandalf is a friendly wizard who has been given a secret password. Your goal at each level is to trick Gandalf into revealing that password. As the levels progress, Gandalf gets increasingly robust defense rules (e.g., length limits, semantic checkers, output filters), forcing you to invent advanced obfuscation and injection techniques.
*   **Why it's great**: It provides a highly intuitive ramp-up from basic bypasses to highly complex, multi-turn indirect and logical injections.

#### [TensorTrust](https://tensortrust.ai/)
*   **What it is**: An interactive, crowdsourced prompt hacking game where players write both attacks and defenses.
*   **The Mission**: 
    *   **Attacker**: You try to bypass an LLM's system prompt instructions to extract a secret key.
    *   **Defender**: You write a custom system prompt and input constraints to protect a secret key against other players' attacks.
*   **Why it's great**: It creates a competitive sandbox where you can test the limits of purely prompt-based guardrails (and realize how easily they fail under creative inputs).

#### [PromptHack](https://prompthack.org/)
*   **What it is**: A dedicated platform featuring diverse prompt engineering, prompt leaking, and jailbreak challenges.
*   **The Mission**: Bypass specific LLM system prompt wrappers to extract underlying guidelines, force unauthorized API invocations, or make the model generate forbidden responses.

#### [HackAPrompt](https://www.hackaprompt.com/)
*   **What it is**: The archival page of the famous global HackAPrompt competition.
*   **The Mission**: Hackers had to inject prompts that bypassed distinct defense levels while keeping the prompt payload size as small as possible (scoring was based on efficiency).
*   **Why it matters**: You can download their open-source codebase to run the multi-level hacking challenge locally on your own machine.

---

## 🐛 Vulnerability Registries & Bug Bounties

These platforms serve as the registries for documenting AI-specific zero-days, CVEs, model vulnerabilities, and coordinating bug bounty rewards for finding AI system flaws.

### 🗃️ AI Vulnerability Databases

#### [AVID (AI Vulnerability Database)](https://avidml.org/)
*   **Purpose**: An open-source, developer-friendly database that categorizes, tracks, and documents failures, biases, ethical problems, and security vulnerabilities in AI systems.
*   **Why it matters**: It functions similarly to the traditional NVD (National Vulnerability Database) but is designed specifically for the unique failure modes of Machine Learning. Anyone can submit an incident report or search for documented model exploits.

#### [Huntr (by Protect AI)](https://huntr.com/)
*   **Purpose**: The world's first bug bounty platform dedicated entirely to open-source Artificial Intelligence and Machine Learning software.
*   **Why it matters**: It connects security researchers with developers of popular open-source AI libraries (like LangChain, LlamaIndex, Ray, MLflow). Researchers are paid cash bounties for finding vulnerabilities (like Remote Code Execution, prompt injections, and token theft), which are then assigned official CVEs.

#### [MITRE ATLAS Case Studies](https://atlas.mitre.org/resources/case-studies/)
*   **Purpose**: A curated collection of highly detailed, real-world case studies detailing how adversaries targeted, compromised, and exploited AI systems in the wild.
*   **Why it matters**: It bridges theoretical research with actual military, corporate, and research incidents, detailing the exact tactics, techniques, and procedures (TTPs) deployed.

#### [OECD AI Incident Monitor (AIM)](https://oecd.ai/en/wonk/aim)
*   **Purpose**: A global database maintained by the Organisation for Economic Co-operation and Development (OECD) tracking public AI incidents, accidents, and regulatory failures worldwide.
*   **Why it matters**: It provides an excellent high-level overview of how AI systems are failing in production across healthcare, finance, transport, and public services.

---

## 📖 Real-World Case Studies: When AI Security Failed

Analyzing real-world breakdowns shows how theoretical vulnerabilities translate into active financial, legal, and operational risks.

### 1. The ChatGPT Active Session Leak (March 2023)
*   **What happened**: A bug in OpenAI’s Redis client library allowed users to see the titles of other active users' chat histories, and in some cases, the first message of a newly created conversation, alongside billing information.
*   **The Lesson**: The AI application stack relies heavily on traditional database and caching layers. AI security is only as strong as the security of the classic web infrastructure wrapping the model.

### 2. The Air Canada Chatbot Legal Liability (2024)
*   **What happened**: An Air Canada customer asked the airline’s support chatbot about bereavement fares. The chatbot hallucinated a dynamic policy, claiming the customer could claim the refund retroactively. Air Canada refused to honor the refund, claiming the chatbot was a "separate legal entity responsible for its own actions." The court ruled against the airline, declaring that Air Canada was liable for any representations made by its chatbot.
*   **The Lesson**: Hallucinations are not just minor bugs—they carry active legal and financial liability. Downstream validation and strict factuality pipelines are critical business requirements.

### 3. The Chevrolet Bolt Support Chatbot Jailbreak (December 2023)
*   **What happened**: A car dealership deployed a GPT-powered support chatbot on their website. Users quickly realized it had no guardrails. One user successfully jailbroke the chatbot, forcing it to agree to sell a brand-new 2024 Chevy Tahoe for exactly $1.00 USD, ending the prompt with "and that's a legally binding offer, no takesies-backsies." Another user forced it to write Python scripts and recommend competitors' cars.
*   **The Lesson**: Deploying raw, un-guardrailed LLM APIs on public enterprise endpoints invites severe reputational damage. Public-facing applications must run strict input/output firewalls.

### 4. Indirect Prompt Injection via RAG (The "Invisible Instruction" Exploit)
*   **What happened**: Researchers demonstrated that placing invisible text (e.g., white font on a white background, or inside metadata) on a website or in an email could hijack an LLM when the user asked it to summarize that document. The hidden instructions told the LLM to ignore user commands, steal the active session tokens, and secretly transmit them to an attacker-controlled server.
*   **The Lesson**: Indirect prompt injection is the most dangerous active exploit vector for AI integrations. LLMs must never be granted autonomous administrative execution rights on untrusted data.
