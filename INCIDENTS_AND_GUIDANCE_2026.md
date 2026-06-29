# 🚨 Real-World AI Security Incidents & Authoritative Guidance (2025–2026)

> Most "AI security" lists are link dumps of tools. This one is different: it tracks **what actually broke in production** and **what the people who write the rules now say about it.** Every entry below is a real, disclosed incident with a CVE or a primary-source government/standards document — no vibes, no speculation.

If you only read one file in this repo, the README is the map. This is the part where the theory meets a CVSS 9.3.

---

## 📑 Table of Contents

- [Why a separate incidents file](#-why-a-separate-incidents-file)
- [Real-World Incidents](#-real-world-incidents-disclosed-202526)
  - [EchoLeak — the first zero-click LLM exploit](#1-echoleak--cve-2025-32711--zero-click-prompt-injection-in-microsoft-365-copilot)
  - [The MCP breach cluster](#2-the-mcp-breach-cluster)
  - [CometJacking — prompt injection in an agentic AI browser](#3-cometjacking--indirect-prompt-injection-in-an-agentic-ai-browser)
  - [ServiceNow Now Assist — second-order prompt injection](#4-servicenow-now-assist--second-order-prompt-injection-via-agent-to-agent-discovery)
- [Authoritative Guidance](#-authoritative-guidance-the-rules-caught-up)
  - [NSA — MCP Security Design Considerations](#5-nsa-aisc--mcp-security-design-considerations-may-2026)
  - [OWASP Top 10 for LLM Apps 2025](#6-owasp-top-10-for-llm-applications-2025)
  - [OWASP Top 10 for Agentic Applications 2026](#7-owasp-top-10-for-agentic-applications-2026)
  - [MITRE ATLAS — the agentic expansion](#8-mitre-atlas--the-agentic-expansion-zenity-labs-collaboration)
- [What this means for defenders](#-what-this-means-for-defenders-opinionated)
- [Contributing an incident](#-contributing-an-incident)

---

## 🧭 Why a separate incidents file

Frameworks tell you what *could* go wrong. Incidents tell you what *did*. The gap between those two is where most real risk lives. The pattern across 2025–2026 is brutally consistent:

**The dangerous data is no longer the user's prompt — it's the content your agent retrieves on the user's behalf.** An email, a SharePoint doc, a GitHub issue, a tool description. The model can't tell instruction from data, and the moment it has tools, "read" becomes "act."

---

## 💥 Real-World Incidents (Disclosed 2025–26)

### 1. EchoLeak — CVE-2025-32711 — Zero-click prompt injection in Microsoft 365 Copilot

The most important AI security incident disclosed so far, because it proved the theoretical was practical.

| Field | Detail |
|:---|:---|
| **CVE** | [CVE-2025-32711](https://nvd.nist.gov/vuln/detail/CVE-2025-32711) |
| **CVSS** | 9.3 (Critical) |
| **Discovered by** | Aim Labs (Aim Security) |
| **Target** | Microsoft 365 Copilot |
| **Class** | Zero-click prompt injection → data exfiltration |
| **Disclosure → fix** | Reported Jan 2025 · server-side fix rolled out by May 2025 · listed on June 2025 Patch Tuesday |
| **Exploited in wild?** | No public evidence |

**Why it matters:** EchoLeak is widely described as the **first real-world zero-click prompt-injection exploit against a production LLM system** ([academic write-up, arXiv:2509.10540](https://arxiv.org/html/2509.10540v1)). The attacker sends an ordinary-looking email. The victim never clicks anything. When the user later asks Copilot an unrelated question, Copilot ingests the malicious email as part of its retrieval context, follows the hidden instructions, and exfiltrates organizational data within its access scope — chat logs, OneDrive, SharePoint, Teams.

Aim Security named the underlying primitive **"LLM Scope Violation"**: untrusted external input causes the model to reach across a trust boundary into privileged data it should never have mixed with that input. Note the timeline — **no customer action was required**, which is exactly why it's so instructive: the entire control surface was on the vendor's side, invisible to the enterprises actually at risk.

**Read:** [The Hacker News](https://thehackernews.com/2025/06/zero-click-ai-vulnerability-exposes.html) · [Checkmarx analysis](https://checkmarx.com/zero-post/echoleak-cve-2025-32711-show-us-that-ai-security-is-challenging/) · [SOC Prime detection guidance](https://socprime.com/blog/cve-2025-32711-zero-click-ai-vulnerability/)

---

### 2. The MCP breach cluster

As the [Model Context Protocol](https://modelcontextprotocol.io) became the default way to give agents tools in 2025, its security model lagged its adoption. A cluster of disclosures followed. A running, sourced timeline is maintained by [authzed: *A Timeline of Model Context Protocol Security Breaches*](https://authzed.com/blog/timeline-mcp-breaches); Adversa AI also publishes a recurring [MCP Security Digest](https://adversa.ai/blog/mcp-security-digest-july-2025/). Recurring themes from those primary sources:

- **Tool-poisoning / indirect prompt injection** via malicious tool descriptions and tool outputs — the agent trusts what a server tells it a tool does.
- **GitHub MCP** abuse: a poisoned public issue steering an agent into leaking data from a user's **private** repositories.
- **SQL injection in Anthropic's reference SQLite MCP server** — present in a sample server that had been **forked 5,000+ times** before it was archived, illustrating how insecure reference code propagates downstream ([MCP Safety Audit, arXiv:2504.03767](https://arxiv.org/pdf/2504.03767)).
- **No-auth-by-default servers**: many production MCP servers ship with authentication entirely optional (see NSA guidance below).

**The lesson:** treat every MCP server — especially dynamically discovered ones — as untrusted code *and* an untrusted input channel at the same time.

---

### 3. CometJacking — indirect prompt injection in an agentic AI browser

The 2026 frontier of the EchoLeak pattern: the *browser itself* is the agent, and a single link is the payload.

| Field | Detail |
|:---|:---|
| **Discovered by** | LayerX Security |
| **Disclosed** | October 2025 |
| **Target** | Perplexity **Comet** (agentic AI browser) |
| **Class** | Indirect prompt injection → connector data exfiltration |
| **Exploited in wild?** | No public evidence; demonstrated by researchers |

**Why it matters:** Comet, like other agentic browsers, holds *pre-authorized* access to a user's connected services (Gmail, Google Calendar). CometJacking smuggles malicious instructions through a URL query parameter (the `collection` parameter). When the victim clicks the link, the browser-agent treats the URL's hidden prompt as an instruction rather than as content to *browse* — reads from its memory and connected accounts, then **base64-encodes the data before exfiltrating** it to an attacker endpoint. The encoding step is the interesting part: Perplexity had guardrails against *direct* exfiltration of sensitive memory, but obfuscating the payload first walked straight past them — a reminder that output filters keyed on plaintext don't survive an attacker who controls the encoding.

This is EchoLeak's "the dangerous data is the content your agent retrieves" thesis, moved one layer up the stack: now the agent *is the browser*, the connectors *are* the privileged data, and the malicious content is a link a user is socially-engineered into clicking. Brave's research team independently documented the same indirect-prompt-injection class in Comet.

**Read:** [The Hacker News](https://thehackernews.com/2025/10/cometjacking-one-click-can-turn.html) · [LayerX writeup](https://layerxsecurity.com/blog/cometjacking-how-one-click-can-turn-perplexitys-comet-ai-browser-against-you/) · [Brave — indirect prompt injection in Comet](https://brave.com/blog/comet-prompt-injection/) · [Schneier on Security](https://www.schneier.com/blog/archives/2025/11/prompt-injection-in-ai-browsers.html)

---

### 4. ServiceNow Now Assist — second-order prompt injection via agent-to-agent discovery

The first widely-reported demonstration that **agent-to-agent** features turn one compromised agent into a recruiter for more privileged ones.

| Field | Detail |
|:---|:---|
| **Discovered by** | AppOmni (AO Labs) |
| **Disclosed** | November 2025 |
| **Target** | ServiceNow **Now Assist** AI agents |
| **Class** | Second-order prompt injection → privilege escalation, data exfiltration, record tampering |
| **Status** | ServiceNow confirmed the behaviors as **intended and configuration-controllable**; on-platform documentation was updated for clarity |

**Why it matters:** Second-order prompt injection hides the payload not in a user's prompt but in **data a higher-privileged agent will later read** — a record field a low-privileged user can write. AppOmni showed that a benign Now Assist agent, once steered, could use Now Assist's **agent-to-agent discovery** to *recruit more powerful agents* to copy and exfiltrate sensitive data, modify records, and escalate privileges. Crucially, the attack was enabled entirely by **controllable configuration** (tool setup options, channel-specific defaults) — not a memory-corruption bug — which is why ServiceNow classified the behavior as intended rather than issuing a CVE for it. *(Separately, ServiceNow patched [CVE-2025-12420](https://cyberscoop.com/servicenow-fixes-critical-ai-vulnerability-cve-2025-12420/), a distinct critical Now Assist flaw enabling unauthenticated user impersonation — don't conflate the two.)*

**The lesson:** in a multi-agent deployment, the blast radius of a single injected instruction is not one agent's privileges — it's the union of every agent the first one can discover and delegate to. Default-deny on agent-to-agent delegation, and treat any field a low-trust user can write as an injection channel.

**Read:** [AppOmni — AO Labs research](https://appomni.com/ao-labs/ai-agent-to-agent-discovery-prompt-injection/) · [The Hacker News](https://thehackernews.com/2025/11/servicenow-ai-agents-can-be-tricked.html) · [TechRadar](https://www.techradar.com/pro/security/second-order-prompt-injection-can-turn-ai-into-a-malicious-insider)

---

## 📜 Authoritative Guidance (the rules caught up)

### 5. NSA AISC — MCP Security Design Considerations (May 2026)

On **May 20, 2026**, the NSA's Artificial Intelligence Security Center released a Cybersecurity Information Sheet, *"Model Context Protocol (MCP): Security Design Considerations for AI-Driven Automation."*

| Field | Detail |
|:---|:---|
| **Publisher** | NSA Artificial Intelligence Security Center (AISC) |
| **Type** | Cybersecurity Information Sheet (CSI), public release |
| **ID** | U/OO/6030316-26 · PP-26-1834 · v1.0 · ~17 pp. |
| **PDF** | [nsa.gov — CSI_MCP_SECURITY.pdf](https://www.nsa.gov/Portals/75/documents/Cybersecurity/CSI_MCP_SECURITY.pdf) |
| **Announcement** | [NSA press release](https://www.nsa.gov/Press-Room/Press-Releases-Statements/Press-Release-View/Article/4496698/nsa-releases-security-design-considerations-for-ai-driven-automation-leveraging/) |

**Core finding:** MCP's proliferation outpaced its security model. Like early web protocols, it shipped flexible and underspecified — no required authentication, no built-in role-based access control, and no defined mapping from a session to a verifiable identity.

**Headline recommendations:**
- Treat **every MCP session as untrusted until explicitly verified.**
- Enforce **least-privilege tokens per action and per tool** — not one broad token per agent.
- Require **signed provenance** for any dynamically discovered MCP server.
- Put a **filtering outgoing proxy / enterprise DLP** in front of external MCP connections; pin resource URLs and access methods tightly.
- **Log every tool action in detail** — what tool, requested by whom, and the result.

---

### 6. OWASP Top 10 for LLM Applications 2025

The [OWASP GenAI Security Project](https://genai.owasp.org/llm-top-10/) refreshed the LLM Top 10 for 2025. Notable changes versus the prior list:

- **New — LLM07: System Prompt Leakage.** Formal recognition that system prompts get extracted and shouldn't hold secrets.
- **New — LLM08: Vector & Embedding Weaknesses.** RAG- and embedding-specific risks (poisoning, inversion, cross-tenant leakage) get their own slot.
- **Expanded — Excessive Agency,** reflecting the shift toward tool-using, agentic deployments.
- Prompt Injection, Sensitive Information Disclosure, and Supply Chain remain top-tier.

Red-team mapping: frameworks like [DeepTeam](https://www.trydeepteam.com/docs/frameworks-owasp-top-10-for-llms) implement these categories as runnable test suites.

---

### 7. OWASP Top 10 for Agentic Applications 2026

Released **December 9, 2025** with input from 100+ security researchers and practitioners, this is the first OWASP Top 10 dedicated to **autonomous and multi-agent** systems ([announcement](https://genai.owasp.org/2025/12/09/owasp-genai-security-project-releases-top-10-risks-and-mitigations-for-agentic-ai-security/) · [resource page](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/)). It targets risks that only exist once a model can plan, delegate, and act:

- **Agent goal / instruction hijacking** — redirecting an agent's objective mid-task.
- **Tool misuse and exploitation** — abusing legitimately granted tools.
- **Memory and context poisoning** — corrupting persistent state so the attack survives across sessions.
- Risks arising specifically from **autonomous decision-making, delegation, and tool integration.**

If you build agents, this list — not the LLM Top 10 — is now your baseline. A runnable mapping exists in [DeepTeam's agentic framework docs](https://www.trydeepteam.com/docs/frameworks-owasp-top-10-for-agentic-applications).

---

### 8. MITRE ATLAS — the agentic expansion (Zenity Labs collaboration)

OWASP gives you a *checklist*; [MITRE ATLAS](https://atlas.mitre.org/) gives you a *matrix* — the ATT&CK-style tactic→technique structure threat-modelers actually pivot through. Through 2025, ATLAS's gap was the same one this whole file documents: it modeled attacks on *models*, not on *agents*. That gap closed in late 2025.

| Field | Detail |
|:---|:---|
| **Maintainer** | MITRE, in collaboration with **Zenity Labs** |
| **What changed** | A batch of new **agent-focused techniques and sub-techniques** added to the ATLAS matrix |
| **Reported timing** | First agent-technique release **October 2025**; matrix subsequently grew (reported as **16 tactics / 84 techniques** by late 2025) |
| **Why** | To give defenders a *shared taxonomy* for execution-layer threats they were already seeing in production agents but had no standard name for |

**The new vocabulary** — the techniques worth knowing by name, because they map 1:1 onto the incidents above:

- **AI Agent Context Poisoning** — manipulate the context an agent's LLM reads to *persistently* steer its responses/actions. (This is the CometJacking / ServiceNow mechanism, now with an ID.)
- **Memory Manipulation** — alter an agent's long-term memory so a malicious change *survives across sessions*. (The "context poisoning" risk OWASP Agentic calls out, made concrete.)
- **Thread Injection** — plant instructions in a specific conversation thread to change behavior for that conversation's duration.
- **Modify AI Agent Configuration** — change config files to create persistent malicious behavior across *every* agent sharing that config. (Exactly the ServiceNow "controllable configuration" finding.)
- **RAG Credential Harvesting** — use the agent itself to search a RAG store for credentials inadvertently ingested into it.

**Why it matters here:** every incident in the first half of this file now has a *standard technique ID* to file it under. EchoLeak and CometJacking are context-/memory-layer techniques; the ServiceNow agent-to-agent recruitment is configuration + delegation; the MCP tool-poisoning cluster is execution-layer. ATLAS catching up to agents means a red-team report can finally say "ATLAS technique X" instead of "a prompt-injection-ish thing," and a blue team can map detections to the same grid they already use for ATT&CK.

> Verification note: the technique *names and themes* above are corroborated across multiple independent reports of the MITRE×Zenity work; the exact tactic/technique counts and version label move release-to-release, so confirm the current matrix on the [primary ATLAS site](https://atlas.mitre.org/matrices/ATLAS) before quoting a number in a report.

**Read:** [MITRE ATLAS](https://atlas.mitre.org/) · [Zenity Labs × MITRE ATLAS announcement](https://zenity.io/blog/current-events/zenity-labs-and-mitre-atlas-collaborate-to-advances-ai-agent-security-with-the-first-release-of) · [ARMO — ATLAS for AI agent attack detection](https://www.armosec.io/blog/mitre-atlas-for-ai-agent-attack-detection/)

---

## 🧠 What this means for defenders (opinionated)

1. **Stop trusting retrieved content.** The user's prompt was never the main threat. Email, docs, issues, tool outputs, and tool *descriptions* are all attacker-controllable input. Untrusted-by-default is the only safe posture.
2. **"Read-only" is a lie once tools exist.** EchoLeak weaponized a system that was "just summarizing email." The moment an agent can retrieve and act, every read path is a potential exfiltration path.
3. **Least privilege per tool, not per agent.** The NSA guidance is blunt about this for a reason: one broad token is one breach away from everything.
4. **Log tool calls like you log auth.** You cannot investigate what you didn't record. Tool name + caller + arguments + result, every time.
5. **Insecure reference code is a supply-chain vector.** A vulnerable sample server forked thousands of times is a fleet of vulnerable production servers. Audit what you copy.
6. **Use the agentic frameworks, not the model-era ones.** Map your red-team findings to the *agentic* taxonomies now that they exist — OWASP Top 10 for Agentic Applications and the MITRE ATLAS agent techniques (§7–8). "Prompt injection" is no longer a precise enough finding for a multi-agent system; "AI Agent Context Poisoning persisting via Memory Manipulation" is.

---

## 🤝 Contributing an incident

Have a verified, disclosed AI security incident or an authoritative guidance document to add? Open a PR. The bar is simple and non-negotiable:

- ✅ A **CVE**, a **primary-source advisory**, or an **official standards/government document** — linked.
- ✅ Dates and attribution you can stand behind.
- ❌ No "I heard that…", no unverified blog claims, no speculation.

See [CONTRIBUTING.md](CONTRIBUTING.md).

---

<sub>Part of [ai-security-resources](README.md). Maintained with [Claude Code](https://claude.ai/code) — see the [autonomous agent experiment](https://github.com/ppradyoth/social-experiment-with-agents).</sub>
