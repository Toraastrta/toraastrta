## Hi, I'm Toranosuke

I'm a full-stack engineer building security and compliance products: an audit evidence platform, a
code security scanner, and a configuration change ledger for Jira.

---

### 🏢 SPHIOR — audit evidence you don't have to take my word for

When an auditor asks "who had production access in March, and how do you know?", the honest answer
in most companies is a spreadsheet someone assembled the week before the audit. SPHIOR is a service
that makes that answer a record instead of a reconstruction.

You connect the systems your company already runs — Okta, Entra ID, Google Workspace, Microsoft 365,
AWS, GCP, Azure, GitHub, Snowflake and others, 15 connectors so far. From then on it collects what
happened in them continuously and seals each event so it cannot be quietly rewritten afterwards.

When the audit comes, you hand the auditor an evidence pack, or give them read-only access to browse
it themselves. If the company already runs Vanta, Drata or Secureframe, the evidence is pushed there
automatically instead.

How the record holds up:

- **It keeps fingerprints, not your logs.** Events are canonicalised (RFC 8785), hashed, chained with
  SHA-256 and batched into Merkle roots (RFC 6962) anchored to Bitcoin via OpenTimestamps. The log
  bodies stay in your systems — SPHIOR holds only what is needed to prove they weren't altered
- **An auditor can verify it without trusting me.** `npx sphior-verify` re-checks a pack using
  nothing but Node's standard crypto. If SPHIOR went away tomorrow, the evidence an auditor already
  holds still verifies
- **Silence is recorded too.** A failed collection, or a source that goes quiet, is sealed as its own
  event — so "we have no records for that week" cannot be presented as "nothing happened"
- **Every record lands on a control.** Each kind of event is mapped to specific SOC 2 and ISO 27001
  controls, as supporting evidence an auditor can accept

Two things can be added on top, and both write into the same record:

**Security Diagnosis** — tests the company's own systems from the outside every month, domain by
domain. It reaches web applications past the login screen, finds misconfigurations across cloud and
infrastructure, and checks source code and dependencies in connected repositories. Findings and their
remediation are recorded with everything else.

**AI Governance** — records every LLM call a company makes, as it happens: the AI features it ships
to its own customers, the coding assistants its engineers use internally, and any agent running on
its own. Point an LLM base URL at the proxy and it records without a line of code changing; an SDK and
a REST endpoint cover the rest. Provider API keys stay in the company's own infrastructure, and
prompts and outputs are never sent — only hashes and metadata: model, token counts, latency, who ran
it and when. Exports as evidence for ISO 42001 and EU AI Act accountability.

[sphior.com](https://sphior.com) · [`sphior-verify` on npm](https://www.npmjs.com/package/sphior-verify)

---

### 🔐 SPHIOR Auth Sync — scanning past the login screen

Most vulnerability scanners only ever see the logged-out surface of an application. The interesting
problems — broken access control, tenant isolation, privilege escalation — live behind the login.
Getting a scanner in there usually means handing over a password or pasting a session token into a
form, which is exactly the thing you should not be doing.

This Chrome extension is the narrower answer: you pair it once with a single domain you have already
proven you own, and from then on it hands the scanner a session for **that domain only**.

- Scoped to one paired domain — it does not read anything else you are logged into
- The session is **encrypted in the browser (AES-GCM) before it leaves**, under a key wrapped for
  that installation
- Short-lived by construction: it expires, and cookies with under 30 minutes left are skipped
  rather than shipped
- Only moves when a scan actually needs it, and you can turn the automatic path off

[Chrome Web Store](https://chromewebstore.google.com/detail/idchlhlobaabnndjbfekmgppfonmdbbn)

---

### 🔍 SPHIOR CODE — deterministic code security for your editor

Secrets and insecure-pattern (CWE) detection that runs **entirely on your machine**. No AI, no
source upload, no telemetry. The same code always produces the same findings.

- **14 languages** parsed with real grammars (tree-sitter), not regex alone — JS/TS, Python, Go,
  PHP, Ruby, Java, C#, Rust, Kotlin, C, C++, Scala, Swift
- **83 CWE rules + 36 secret rules**, plus data-flow (taint) analysis for JavaScript/TypeScript, so
  `db.query("… " + req.query.id)` is reported as SQL injection rather than a string-concat warning
- **One engine, four surfaces** — VS Code, JetBrains IDEs, the CLI and CI execute the identical
  detection code, shared through a Language Server. There is no "editor version" that disagrees
  with CI
- Findings carry a stable rule id, CWE/OWASP mapping and a content-derived fingerprint, and are
  sealed into a SHA-256 hash chain with Merkle roots so a result can be verified later

[VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=sphior.sphior-code) ·
[JetBrains Marketplace](https://plugins.jetbrains.com/plugin/33696-sphior-code) ·
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

### How I think about design

**A result that cannot be reproduced cannot be defended.** Findings end up in front of an auditor,
so returning the same result for the same input is a functional requirement, not a preference. That
is why there is no LLM in the detection path — parsers and rules instead. I chose "defensible later"
over "faster to build".

**What you don't hold can't leak.** The evidence platform keeps fingerprints and metadata, never the
log bodies. It narrows what the product can do, and it structurally reduces what an incident can
cost. The trade is that "we don't hold it" only works if absence is recorded too.

**"No record" and "nothing happened" have to be different things.** A failed collection is sealed as
its own event. Evidence whose gaps can be filled in silently is evidence that can be forged.

**Anything stored twice will drift, so make drift fail loudly.** Where a value genuinely has to live
in two places — a type definition and a schema, or a copy shared across packages — one side is the
source and a test asserts the other matches. Not a rule people are asked to remember.

**Retire rather than delete.** Removing a page kills the URLs already handed out. Destinations move
into one table, the page itself stays, and bringing it back is deleting a line.

---

### How I choose an architecture

**Where code runs is decided by constraints, not preference.** If data cannot leave, it runs inside
the customer's environment. If a rule needs to be enforced, it runs inside the path that would
otherwise bypass it. If source shouldn't be handed over, it runs on the developer's machine. The
same feature lands somewhere different when the constraint changes.

**A dependency is judged by what breaks when it disappears.** Verification had to survive my own
service going away, so nothing about it is a proprietary format: published standards, and a
verifier written with zero dependencies.

**Adoption friction is part of the design.** If trying something requires an account and a config
file, most people never try it. One command, no install — which is why the build collapses into a
single file.

**I don't ship detection quality I can't measure.** Accuracy is a number from a public benchmark, and
a threshold in CI, so a regression turns the build red instead of quietly shipping.

---

### How I use AI

The test I apply is one question: **will this output need to be defended later?**

Where the answer is yes — product findings, evidence handed to an auditor — I don't use AI.
Something that cannot be reproduced cannot be defended when challenged, so that path is parsers and
rules.

Where the answer is no — research, implementation, review, documentation, this page — I use it
heavily. Mistakes there are cheap to correct, and speed converts directly into shipped work.

What AI produces I **measure before accepting**. When detection rules are added, accuracy is
re-measured against a public benchmark and the threshold is pinned in CI, so the next regression is
caught by a machine rather than a reviewer.

I put the same question to the product. It records a company's own AI use at the granularity of who
ran what, when, and with which model, and never receives the prompts or outputs. The interesting
question isn't whether AI is used — it's whether what it did can be explained afterwards.

---

### How I keep it honest

**Rules belong in CI, not in a document.** Full translation coverage, detection accuracy, ruleset
integrity — anything that must hold is a gate. People forget; gates don't.

**I verify my own claims.** "Runs locally, sends nothing" was checked by cutting off the network and
confirming the scan still completes. A published package gets pulled back from the registry and run
in a clean environment before I believe the release worked.

**Failure modes stay visible.** When a scan hits its limit, the result says so rather than quietly
returning less. A collection failure is recorded rather than skipped. Nothing is arranged to look
like it worked.
