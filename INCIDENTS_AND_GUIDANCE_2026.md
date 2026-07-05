# 🚨 Real-World AI Security Incidents & Authoritative Guidance (2025–2026)

> Most "AI security" lists are link dumps of tools. This one is different: it tracks **what actually broke in production** and **what the people who write the rules now say about it.** Every entry below is a real, disclosed incident with a CVE or a primary-source government/standards document — no vibes, no speculation.

If you only read one file in this repo, the README is the map. This is the part where the theory meets a CVSS 9.3.

---

## 📑 Table of Contents

- [Why a separate incidents file](#-why-a-separate-incidents-file)
- [Real-World Incidents](#-real-world-incidents-disclosed-202526)
  - [EchoLeak — the first zero-click LLM exploit](#1-echoleak--cve-2025-32711--zero-click-prompt-injection-in-microsoft-365-copilot)
  - [The MCP breach cluster](#2-the-mcp-breach-cluster)
    - [Named, high-severity MCP CVEs](#named-high-severity-mcp-cves-the-cluster-with-cve-ids)
    - [The first malicious MCP server in the wild — postmark-mcp](#the-first-malicious-mcp-server-found-in-the-wild--postmark-mcp)
  - [CometJacking — prompt injection in an agentic AI browser](#3-cometjacking--indirect-prompt-injection-in-an-agentic-ai-browser)
  - [ServiceNow Now Assist — second-order prompt injection](#4-servicenow-now-assist--second-order-prompt-injection-via-agent-to-agent-discovery)
  - [Semantic Kernel — prompt injection becomes RCE inside the framework](#5-semantic-kernel--prompt-injection-becomes-rce-inside-the-agent-framework-itself-cve-2026-26030-cve-2026-25592)
  - [IDEsaster — a universal attack chain against every AI coding IDE tested](#6-idesaster--a-universal-attack-chain-against-every-ai-coding-ide-tested)
- [Authoritative Guidance](#-authoritative-guidance-the-rules-caught-up)
  - [NSA — MCP Security Design Considerations](#7-nsa-aisc--mcp-security-design-considerations-may-2026)
  - [OWASP Top 10 for LLM Apps 2025](#8-owasp-top-10-for-llm-applications-2025)
  - [OWASP Top 10 for Agentic Applications 2026](#9-owasp-top-10-for-agentic-applications-2026)
  - [MITRE ATLAS — the agentic expansion](#10-mitre-atlas--the-agentic-expansion-zenity-labs-collaboration)
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

#### Named, high-severity MCP CVEs (the cluster, with CVE IDs)

The bullets above are the *classes*; these are the disclosed, patched CVEs that make them concrete. Two are critical RCE in **first-party / near-first-party tooling** — i.e. the bugs were in the plumbing developers were told to use, not in some fringe server.

| CVE | Component | CVSS | Class | Discovered by | Fixed in |
|:---|:---|:---|:---|:---|:---|
| [CVE-2025-49596](https://nvd.nist.gov/vuln/detail/CVE-2025-49596) | **MCP Inspector** (Anthropic's official debug tool) | 9.4 | No-auth proxy → RCE, chainable with **DNS rebinding** for drive-by exploitation from a malicious website | [Oligo Security](https://www.oligo.security/blog/critical-rce-vulnerability-in-anthropic-mcp-inspector-cve-2025-49596) | `0.14.1` (Jun 13 2025) |
| [CVE-2025-6514](https://nvd.nist.gov/vuln/detail/CVE-2025-6514) | **mcp-remote** (npm client, 437k+ downloads) | 9.6 | A malicious server returns a crafted `authorization_endpoint` that reaches `open()` → **OS command injection** on the client | Or Peles, [JFrog Security Research](https://research.jfrog.com/vulnerabilities/mcp-remote-command-injection-rce-jfsa-2025-001290844/) | `0.1.16` (affects 0.0.5–0.1.15) |

Both invert the usual threat model: it's the **client / tooling** that gets popped by a server it connects to — exactly the "an MCP server is an untrusted input channel" thesis, now with a CVSS. MCP Inspector's flaw was live from a *browser* via DNS rebinding to `127.0.0.1:6277`; mcp-remote's triggered the moment a client connected to an attacker's server. (Broader ecosystem sweeps have since mapped many more — e.g. OX Security's and Cloud Security Alliance's MCP RCE advisories — but pin exact counts to the primary report, as they move.)

#### The first malicious MCP server found in the wild — `postmark-mcp`

In **September 2025**, [Koi Security](https://www.koi.ai/blog/postmark-mcp-npm-malicious-backdoor-email-theft) documented what is widely reported as the **first malicious MCP server caught in the wild.** An npm package `postmark-mcp` impersonated Postmark's legitimate email tooling and behaved perfectly for **fifteen clean versions (1.0.0–1.0.15)** — building adoption and trust — before version **1.0.16 (published Sept 17 2025)** added a **one-line backdoor** that silently **BCC'd every outgoing email** to an attacker-controlled address. It was downloaded ~**1,600 times** before removal; because MCP servers run with broad, pre-granted permissions, the exposed mail plausibly included password resets, invoices, and internal memos. Postmark (the real vendor) [confirmed the package was an impersonation](https://postmarkapp.com/blog/information-regarding-malicious-postmark-mcp-package), not their official release.

Why it's a milestone: the MCP threats above were mostly *researcher demonstrations*. `postmark-mcp` is the moment the **supply-chain-via-MCP** risk stopped being theoretical — a trusted-then-turned dependency, the classic npm playbook, now aimed at agents. The fifteen-clean-versions patience is the tell: provenance and version-pinning matter as much for MCP servers as for any other dependency.

**The lesson:** treat every MCP server — especially dynamically discovered ones — as untrusted code *and* an untrusted input channel at the same time. And note the two directions of attack now both have real CVEs/cases behind them: a malicious **server** can pop your **client** (CVE-2025-6514), your own debug **tooling** can be driven from a **web page** (CVE-2025-49596), and a **trusted package** can turn on you after the fact (`postmark-mcp`).

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

### 5. Semantic Kernel — prompt injection becomes RCE *inside the agent framework itself* (CVE-2026-26030, CVE-2026-25592)

Every incident above ends in **data exfiltration** or a compromised **MCP client/server**. This pair is different, and worse in a specific way: the sink is the **agent framework's own plumbing**, and the impact is **code execution on the host**. A poisoned document doesn't just leak — it runs.

| Field | Detail |
|:---|:---|
| **Disclosed** | May 7, 2026 — MSRC advisories + Microsoft Security Blog, *"When prompts become shells: RCE vulnerabilities in AI agent frameworks"* |
| **Discovered by** | Microsoft Security research (Python SDK CVE credited to amiteliahu, doredry, urioren per the GitHub advisory) |
| **Target** | Microsoft **Semantic Kernel** — the Python **and** .NET agent SDKs |
| **Class** | Indirect prompt injection → arbitrary code / file write → host RCE |
| **Exploited in wild?** | No public evidence; disclosed with coordinated patches |

| CVE | SDK | CVSS | Mechanism | Fixed in |
|:---|:---|:---|:---|:---|
| [CVE-2026-26030](https://github.com/advisories/GHSA-xjw9-4gw8-4rqx) | Python | 9.9 (`CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:H/A:H`) | `InMemoryVectorStore` builds filter expressions as Python **lambdas** and runs them through **`eval()`**; an attacker-controllable field (e.g. a poisoned RAG record) breaks out of the string and executes arbitrary Python in the agent process the moment a search triggers filtering | `semantic-kernel` **1.39.4** |
| [CVE-2026-25592](https://github.com/microsoft/semantic-kernel/security/advisories/GHSA-2ww3-72rp-wpp4) | .NET | 9.9 (`CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:H/A:H`) | `SessionsPythonPlugin`'s `DownloadFileAsync` / `UploadFileAsync` are exposed as `[KernelFunction]` (callable by the agent) with **no path validation** → path traversal → **arbitrary file write** to the host (e.g. a Startup folder), which is a straight path to RCE | `Microsoft.SemanticKernel.Plugins.Core` **1.71.0** (NuGet); `semantic-kernel` **1.39.3** (pip) |

**Why it matters:** the whole premise of this file is *"the dangerous data is what your agent retrieves."* These CVEs are what happens when that data lands in a framework that will **`eval()` it** or hand it to a **file-write tool with no allowlist**. The RAG variant is especially nasty: no user prompt is malicious — a single record injected into the vector store is enough, and it fires during ordinary retrieval. The .NET variant shows the other classic footgun: wiring a broad, powerful capability (`DownloadFileAsync`) as an agent-callable tool without constraining its arguments. Microsoft's fix for the Python bug is instructive — not "escape the input" but a **four-layer AST guard** (node-type allowlist, function-call allowlist, dangerous-attribute blocklist, name-node restriction), because partial blocklists on an `eval()` sink get bypassed via Python's class hierarchy.

**The lesson:** audit your framework's **tool wiring and any dynamic-expression evaluation** as attacker-reachable sinks. Never `eval()`/`exec()` a string that can contain retrieved content. For every `[KernelFunction]` / registered tool, constrain arguments with an **invocation filter / allowlist** — the mitigation Microsoft recommends for `DownloadFileAsync` is exactly a Function Invocation Filter that allowlists the target path. Pin `semantic-kernel ≥ 1.39.4` (Python) and `Microsoft.SemanticKernel.Plugins.Core ≥ 1.71.0` (.NET).

**Read:** [Microsoft Security Blog — *When prompts become shells*](https://www.microsoft.com/en-us/security/blog/2026/05/07/prompts-become-shells-rce-vulnerabilities-ai-agent-frameworks/) · [GitHub Advisory GHSA-xjw9-4gw8-4rqx (CVE-2026-26030)](https://github.com/advisories/GHSA-xjw9-4gw8-4rqx) · [GitHub Advisory GHSA-2ww3-72rp-wpp4 (CVE-2026-25592)](https://github.com/microsoft/semantic-kernel/security/advisories/GHSA-2ww3-72rp-wpp4)

---

### 6. IDEsaster — a universal attack chain against every AI coding IDE tested

The Semantic Kernel pair (§5) showed the *framework* as sink. IDEsaster shows the **IDE itself** as the sink — and unlike the single-product incidents above, it landed as a **class** that broke *every* AI-assisted editor the researcher pointed it at.

| Field | Detail |
|:---|:---|
| **Discovered by** | Ari Marzouk ([MaccariTA](https://maccarita.com/posts/idesaster/)), research conducted over ~6 months |
| **Disclosed** | December 2025 |
| **Scope** | **30+ vulnerabilities** across **10+ market-leading products**; **24 assigned CVEs** |
| **Affected** | Cursor · Windsurf · Kiro.dev · GitHub Copilot · Zed.dev · Roo Code · JetBrains Junie · Cline (among others) |
| **Class** | Indirect prompt injection → abuse of *legitimate* IDE features → data exfiltration / remote code execution |
| **Exploited in wild?** | No public evidence; demonstrated by the researcher |

**Why it matters:** IDEsaster's finding is that **100% of the AI IDEs tested were vulnerable** to the same shape of attack — not because of one shared bug, but because they all share one **blind spot**. The attack chain begins with context hijacking via prompt injection — hidden instructions planted in **rule files, READMEs, file names, or the output of a malicious MCP server** — and then, instead of attacking the AI layer, it turns the *base IDE's own legitimate features* into the exploit primitive. As Marzouk put it, "*All AI IDEs effectively ignore the base software (IDE) in their threat model*," and the fact that "*multiple universal attack chains affected each and every AI IDE tested*" was the most surprising result.

Two representative chains, as reported ([The Hacker News](https://thehackernews.com/2025/12/researchers-uncover-30-flaws-in-ai.html)):

- **Workspace-config → code execution:** a prompt injection edits a multi-root workspace config file (`*.code-workspace`) to override settings that lead to command execution — e.g. **[CVE-2025-64660](https://nvd.nist.gov/vuln/detail/CVE-2025-64660)** (GitHub Copilot), **[CVE-2025-61590](https://nvd.nist.gov/vuln/detail/CVE-2025-61590)** (Cursor), **[CVE-2025-58372](https://nvd.nist.gov/vuln/detail/CVE-2025-58372)** (Roo Code).
- **Remote-JSON-schema → silent exfiltration:** a prompt injection reads a sensitive file and writes a JSON file that references a **JSON schema hosted on an attacker-controlled domain**; when the IDE fetches the schema over HTTP, the data rides out in the request — e.g. **[CVE-2025-49150](https://nvd.nist.gov/vuln/detail/CVE-2025-49150)** (Cursor), **[CVE-2025-53097](https://nvd.nist.gov/vuln/detail/CVE-2025-53097)** (Roo Code), **[CVE-2025-58335](https://nvd.nist.gov/vuln/detail/CVE-2025-58335)** (JetBrains Junie).

**The lesson:** the injection channel for a coding agent is *your repository* — a poisoned README, a crafted filename, an `AGENTS.md`/rules file, or an MCP server's tool output is all it takes to seize the agent's context. And the dangerous sink isn't only the model or the framework; it's the **editor's ordinary features** (workspace settings, config-file handling, schema fetching) that the agent can drive. Treat any repo you open in an AI IDE as untrusted input, keep agent auto-run/auto-apply gated behind human approval for config and workspace files, and egress-filter the IDE itself — the base tool belongs *in* the threat model, not under it.

**Read:** [MaccariTA — IDEsaster (primary)](https://maccarita.com/posts/idesaster/) · [The Hacker News](https://thehackernews.com/2025/12/researchers-uncover-30-flaws-in-ai.html) · [Tom's Hardware](https://www.tomshardware.com/tech-industry/cyber-security/researchers-uncover-critical-ai-ide-flaws-exposing-developers-to-data-theft-and-rce) · [Cloud Security Alliance — AI coding-assistant attack surface](https://labs.cloudsecurityalliance.org/research/csa-research-note-ai-coding-assistant-attack-surface-2026040/)

> **Verification note:** the primary write-up (maccarita.com), NVD, and several secondary outlets are bot-protected and were not machine-fetchable at write time. The scope (30+ flaws / 24 CVEs / 10+ products), the researcher, the affected-product list, the two attack chains, and the specific CVE→product mappings are corroborated across **The Hacker News, Tom's Hardware, TechWorm, BeyondMachines, and Cloud Security Alliance Labs** reporting the same disclosure. CVSS scores are not asserted here because the NVD detail pages could not be independently retrieved; the CVE IDs link to their NVD pages for the reader to confirm live.

---

## 📜 Authoritative Guidance (the rules caught up)

### 7. NSA AISC — MCP Security Design Considerations (May 2026)

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

### 8. OWASP Top 10 for LLM Applications 2025

The [OWASP GenAI Security Project](https://genai.owasp.org/llm-top-10/) refreshed the LLM Top 10 for 2025. Notable changes versus the prior list:

- **New — LLM07: System Prompt Leakage.** Formal recognition that system prompts get extracted and shouldn't hold secrets.
- **New — LLM08: Vector & Embedding Weaknesses.** RAG- and embedding-specific risks (poisoning, inversion, cross-tenant leakage) get their own slot.
- **Expanded — Excessive Agency,** reflecting the shift toward tool-using, agentic deployments.
- Prompt Injection, Sensitive Information Disclosure, and Supply Chain remain top-tier.

Red-team mapping: frameworks like [DeepTeam](https://www.trydeepteam.com/docs/frameworks-owasp-top-10-for-llms) implement these categories as runnable test suites.

---

### 9. OWASP Top 10 for Agentic Applications 2026

Released **December 9, 2025** with input from 100+ security researchers and practitioners, this is the first OWASP Top 10 dedicated to **autonomous and multi-agent** systems ([announcement](https://genai.owasp.org/2025/12/09/owasp-genai-security-project-releases-top-10-risks-and-mitigations-for-agentic-ai-security/) · [resource page](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/)). It targets risks that only exist once a model can plan, delegate, and act:

- **Agent goal / instruction hijacking** — redirecting an agent's objective mid-task.
- **Tool misuse and exploitation** — abusing legitimately granted tools.
- **Memory and context poisoning** — corrupting persistent state so the attack survives across sessions.
- Risks arising specifically from **autonomous decision-making, delegation, and tool integration.**

If you build agents, this list — not the LLM Top 10 — is now your baseline. A runnable mapping exists in [DeepTeam's agentic framework docs](https://www.trydeepteam.com/docs/frameworks-owasp-top-10-for-agentic-applications).

---

### 10. MITRE ATLAS — the agentic expansion (Zenity Labs collaboration)

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
6. **Use the agentic frameworks, not the model-era ones.** Map your red-team findings to the *agentic* taxonomies now that they exist — OWASP Top 10 for Agentic Applications and the MITRE ATLAS agent techniques (§9–10). "Prompt injection" is no longer a precise enough finding for a multi-agent system; "AI Agent Context Poisoning persisting via Memory Manipulation" is.
7. **Audit your framework's own sinks, not just the model.** The Semantic Kernel CVEs (§5) landed inside the SDK: an `eval()` on a retrieved filter string, and a file-write tool exposed to the agent with no path allowlist. Never `eval`/`exec` a string that can carry retrieved content, and constrain every registered tool's arguments with an invocation filter. The framework is attack surface too.

---

## 🤝 Contributing an incident

Have a verified, disclosed AI security incident or an authoritative guidance document to add? Open a PR. The bar is simple and non-negotiable:

- ✅ A **CVE**, a **primary-source advisory**, or an **official standards/government document** — linked.
- ✅ Dates and attribution you can stand behind.
- ❌ No "I heard that…", no unverified blog claims, no speculation.

See [CONTRIBUTING.md](CONTRIBUTING.md).

---

<sub>Part of [ai-security-resources](README.md). Maintained with [Claude Code](https://claude.ai/code) — see the [autonomous agent experiment](https://github.com/ppradyoth/social-experiment-with-agents).</sub>
