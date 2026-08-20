## Hi, I'm Tora

I build **systems that produce records you can verify** — instead of dashboards you have to trust.

Most security and compliance tooling asks you to believe its output. I keep ending up on the other
side of that problem: making the result reproducible, making the evidence checkable by someone who
does not trust the party that produced it, and running the analysis **where the data already lives**
so nothing has to be shipped somewhere else first.

Five products, shipped to seven marketplaces. Everything below is live and installable today.

---

### 🔍 SPHIOR CODE — deterministic code security for your editor

Secrets and insecure-pattern (CWE) detection that runs **entirely on your machine**. No AI, no
source upload, no telemetry. The same code always produces the same findings.

- **14 languages** parsed with real grammars (tree-sitter), not regex alone — JS/TS, Python, Go,
  PHP, Ruby, Java, C#, Rust, Kotlin, C, C++, Scala, Swift
- **83 CWE rules + 36 secret rules**, plus data-flow (taint) analysis for JavaScript/TypeScript, so
  `db.query("… " + req.query.id)` is reported as SQL injection rather than a string-concat warning
- **One engine, four surfaces** — the editor, the CLI and CI execute the identical detection code,
  shared through a Language Server. There is no "editor version" that disagrees with CI
- Findings carry a stable rule id, CWE/OWASP mapping and a content-derived fingerprint, and are
  sealed into a SHA-256 hash chain with Merkle roots so a result can be verified later

[VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=sphior.sphior-code) ·
[Open VSX](https://open-vsx.org/extension/sphior/sphior-code) ·
**[Source code](https://github.com/Toraastrta/sphior-code)**

> The engine is open source. The "runs locally, sends nothing" claim is the kind of thing you should
> be able to check rather than take my word for — so you can.

---

### 🔌 sphior-code-mcp — findings inside your AI assistant

A Model Context Protocol server that surfaces code findings into Claude Code, Cursor, Windsurf, Zed
and any MCP-compatible client. Eight tools: list and inspect findings, get deterministic remediation
context, triage as resolved or false-positive, verify evidence.

Detection stays deterministic; the AI only reads the results and helps you fix them.

[npm](https://www.npmjs.com/package/sphior-code-mcp) ·
[MCP Registry](https://registry.modelcontextprotocol.io) (`io.github.Toraastrta/sphior-code-mcp`)

---

### 📋 Sphior Ledger for Jira — tamper-evident configuration history

Snapshot, diff and verify every change to Jira workflows, schemes, permissions, custom fields,
projects and screens. Changes are chained with SHA-256 so the history cannot be quietly rewritten,
and one click produces an evidence pack for an auditor.

Runs **on Atlassian** — customer data never leaves their tenant.

[Atlassian Marketplace](https://marketplace.atlassian.com/apps/4197217154/sphior-ledger-for-jira)

---

### 💰 Sphior Margin — stop discounts from eating the margin

Caps over-discounting, blocks loss-making orders and controls payment and delivery methods using
**Shopify Functions**, so the rules run inside Shopify's checkout rather than as an afterthought.
Then it shows the merchant the margin it actually protected, on an ROI dashboard.

[Shopify App Store](https://apps.shopify.com/marginguard-7)

---

### 🔐 SPHIOR Auth Sync — scanning past the login screen

A Chrome extension that syncs a logged-in session to SPHIOR so security scans can test the parts of
an application that only exist **after** authentication — which is where the interesting
vulnerabilities usually are.

[Chrome Web Store](https://chromewebstore.google.com/detail/idchlhlobaabnndjbfekmgppfonmdbbn)

---

### 🏢 SPHIOR

The platform the above feeds into: automated security diagnosis plus a tamper-evident record of what
your systems and code actually did, usable as supporting evidence in SOC 2 / ISO 27001 work.

[sphior.com](https://sphior.com)

---

### How I tend to build

- **Deterministic over clever.** If a result cannot be reproduced, it cannot be defended. I removed
  an LLM from the detection path on purpose and replaced it with parsers and rules
- **Run where the data is.** Local process, inside the customer's Atlassian tenant, inside Shopify's
  own function runtime — the architecture that doesn't require trust is usually also the simpler one
- **Ship it.** Seven marketplace reviews across Microsoft, Eclipse, JetBrains, Atlassian, Shopify,
  Google and npm. Getting something approved and installable is a different skill from getting it
  working, and I wanted both
  

