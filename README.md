# AgentRein Documentation

The official documentation source for [AgentRein](https://agentrein.com) - the reliability, rollback, and governance layer for production AI agents.

[![Docs](https://img.shields.io/badge/Docs-agentrein.com%2Fdocs-2A6AD8?style=flat-square)](https://agentrein.com/docs)
[![Built with Mintlify](https://img.shields.io/badge/Built%20with-Mintlify-black?style=flat-square&logo=mintlify)](https://mintlify.com)
[![Core Monorepo](https://img.shields.io/badge/GitHub-agentrein%2Fagentrein-blueviolet?style=flat-square&logo=github)](https://github.com/agentrein/agentrein)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue?style=flat-square)](https://github.com/agentrein/docs/blob/main/LICENSE)


## Overview

This repository contains the complete documentation source code for [AgentRein](https://agentrein.com), deployed to [agentrein.com/docs](https://agentrein.com/docs). 

The platform documentation is authored in **MDX** (Markdown with interactive JSX components) and built using the [Mintlify](https://mintlify.com) documentation framework. It covers quickstart guides, core reliability concepts (transparent proxy, LIFO rollback, policy engine, and intent drift detection), SDK reference manuals for TypeScript and Python, full REST API specifications, and connector catalogs across 9 external platforms.

---

## Content Map & Information Architecture

The documentation is organized into four main tabs configured via [`docs.json`](docs.json):

### 1. Guides (`/docs`)

| Topic | File | Description |
| :--- | :--- | :--- |
| **Introduction** | [`index.mdx`](index.mdx) | High-level platform architecture, core capabilities, and value proposition |
| **Quick Start** | [`quickstart.mdx`](quickstart.mdx) | 5-minute onboarding guide with side-by-side TypeScript & Python code |
| **Core Concepts** | [`core-concepts.mdx`](core-concepts.mdx) | Deep dive into the Transparent Proxy, LIFO Rollback, and State Machines |
| **Session & Action Statuses** | [`statuses.mdx`](statuses.mdx) | Canonical state transitions (`PENDING_APPROVAL`, `COMPLETED`, `ROLLED_BACK`) |
| **Security & Privacy** | [`security-model.mdx`](security-model.mdx) | Data redaction, PCI-DSS / GDPR compliance, and credential tokenization |
| **FAQ** | [`faq.mdx`](faq.mdx) | Frequently asked questions about pricing, self-hosting, and safety |

### 2. SDK Reference (`/docs/sdk`)

| Guide | File | Coverage |
| :--- | :--- | :--- |
| **Installation & Sandbox** | [`sdk/installation.mdx`](sdk/installation.mdx) | Package manager commands (`npm`, `pip`, `poetry`) and `ar_test_` sandbox keys |
| **Session Initialization** | [`sdk/new-session.mdx`](sdk/new-session.mdx) | Creating sessions with declared `intent` and tenant isolation |
| **Transparent Proxy Wrapping** | [`sdk/call.mdx`](sdk/call.mdx) | Intercepting third-party SDK calls via `agentrein.wrap()` |
| **Rollback Configuration** | [`sdk/undo-config.mdx`](sdk/undo-config.mdx) | Snapshot capture, compensation strategies, and LIFO unwind order |
| **Connectors Overview** | [`sdk/connectors.mdx`](sdk/connectors.mdx) | Action prefixes, SDK pairing rules, and Python object-wrapping caveats |
| **Supported Integrations** | [`sdk/supported-integrations.mdx`](sdk/supported-integrations.mdx) | Detailed action breakdown across 9 supported external platforms |
| **Centralized Policies** | [`sdk/policies.mdx`](sdk/policies.mdx) | Centralized rules (`BLOCK > REQUIRE_APPROVAL > ALLOW`) and conditions |
| **Human-in-the-Loop (HITL)** | [`sdk/hitl.mdx`](sdk/hitl.mdx) | Pre-execution approval gates, timeouts, and reviewer workflows |
| **Error Handling** | [`sdk/errors.mdx`](sdk/errors.mdx) | Exception hierarchy (`ApprovalRejectedError`, `ApprovalTimeoutError`, etc.) |

### 3. API Reference (`/docs/api-reference`)

- [`api-reference.mdx`](api-reference.mdx): Complete REST API documentation for the AgentRein control plane, including endpoints for Agents, Sessions, Actions, Approvals, Connectors, Policies, Audit Logs, and Credentials.

### 4. Changelog (`/docs/changelog`)

- [`changelog.mdx`](changelog.mdx): Platform release notes, SDK version milestones, and connector expansion history.

---

## Local Development & Preview

You can run the documentation locally to preview MDX formatting, navigation structure, and interactive components before submitting changes.

### Prerequisites

- [Node.js](https://nodejs.org/) (v18.0.0 or higher)
- npm or pnpm

### Quick Start

```bash
# 1. Open the docs directory
cd docs

# 2. Launch the Mintlify development server
npx mintlify dev
```

The documentation preview will be available at:
👉 **[http://localhost:3000](http://localhost:3000)**

### Port Conflicts

If port `3000` is already in use by the AgentRein web dashboard (`website`), specify an alternative port:

```bash
npx mintlify dev --port 3333
```

### Validating Links

Check for broken internal links and unresolved anchors across all MDX pages:

```bash
npx mintlify broken-links
```

---

## Authoring Standards & Style Guide

To maintain visual and technical consistency across the documentation, follow these conventions:

### 1. Frontmatter
Every `.mdx` page must begin with YAML frontmatter:

```yaml
---
title: "Page Title"
description: "A concise 1-2 sentence description of the page content."
mode: "default" # "default" or "wide"
---
```

### 2. Dual-Language Code Examples
Whenever demonstrating SDK capabilities, provide simultaneous **TypeScript** and **Python** snippets using `<CodeGroup>`:

````markdown
<CodeGroup>
```typescript TypeScript
const safeOctokit = agentrein.wrap(octokit, session, { connector: 'github' });
await safeOctokit.issues.create({ owner: 'org', repo: 'app', title: 'Task' });
```

```python Python
safe_repo = agentrein.wrap(repo, session, connector="github")
safe_repo.create_issue(title="Task")
```
</CodeGroup>
````

### 3. Callout Banners
Use Mintlify alert components to highlight critical information:

- `<Note>`: Helpful tips, background context, or recommendations.
- `<Info>`: Implementation specifics and platform behavior notes.
- `<Warning>`: High-risk actions, irreversible operations, or breaking caveats.

### 4. Adding New Pages
When adding a new MDX document:
1. Create the `.mdx` file in the appropriate directory (`./` or `sdk/`).
2. Register the page slug in [`docs.json`](docs.json) under the relevant navigation group.
3. Test locally with `npx mintlify dev` to verify sidebar placement.

---

## Deployment Workflow

- **Continuous Deployment:** Merging pull requests into the `main` branch automatically triggers Mintlify's build pipeline, deploying changes live to [agentrein.com/docs](https://agentrein.com/docs) within minutes.
- **Preview Deployments:** Pull requests opened against `main` automatically receive temporary preview URLs generated by Mintlify for review.

---

## Related Links

- **Core Monorepo:** [`agentrein/agentrein`](https://github.com/agentrein/agentrein) — Open-source SDKs (TypeScript & Python), types, and connector catalog.
- **Documentation Portal:** [agentrein.com/docs](https://agentrein.com/docs)
- **AgentRein Platform:** [agentrein.com](https://agentrein.com)


---

## License

This documentation is licensed under the MIT License — see the [LICENSE](https://github.com/agentrein/agentrein/blob/main/LICENSE) file for details.
