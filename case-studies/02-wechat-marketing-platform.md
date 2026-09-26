# Case study 2 — WeChat marketing & payments platform

**Role**: independent full-stack developer (requirements → delivery) · **Period**: 2026

## The problem

A merchant wanted a turnkey campaign tool for offline stores: many campaigns in parallel, physical and virtual prizes, entries purchased through WeChat Pay, and winners managed end to end. Two things made it harder than a typical mini-program job:

- **Payments had to be exact.** Purchased entries, refunds and refund notifications all have to reconcile; a lost callback is a financial discrepancy, not a UI bug.
- **Marketing parameters change mid-campaign.** Probability, stock and rules get adjusted while people are playing — requiring a redeploy to change a probability is not acceptable in this business.

## What I built

**Payments (the part that had to be right)**

- Full WeChat Pay V3 flow: JSAPI payment → asynchronous callback → refund → refund notification, with idempotent callback handling so a retried notification cannot double-credit an entry.
- Alipay QR payment as a second channel, and WeCom (Enterprise WeChat) order management for the merchant side.

**Campaign engine**

- Multiple simultaneous campaigns, physical vs virtual prizes, draw entries purchased per campaign, winner list with fulfilment status.
- A hot-configuration mode: probability, stock and rules are stored so they can be changed at runtime with zero downtime and no redeploy.
- Read-only operational queries instead of "go look at the database": a read-only MySQL MCP server answers natural-language operational questions (stock, revenue, participation) through parameterised queries.

**Delivery**

- One-command Docker deployment, built on Windows and deployed to Linux.
- Automated code-review and SQL-safety checks packaged as reusable agent skills, plus standardised coding/deployment/security rules in the project instructions.
- Roughly **80% of the code was produced with AI assistance** using an explore → plan → code → commit workflow, with the reasoning and verification kept as files.

## Result

- The complete payment loop (pay → callback → refund → refund notification) shipped as one business flow rather than separate endpoints.
- Campaign parameters can be changed during a live campaign without touching the deployment.
- The merchant can answer "how much did we sell today, and is any prize about to run out" without opening a database client.

## What I would do differently

Two things. First, the read-only database role should have existed from day one — the tooling was written against a privileged account and tightened afterwards, which is the wrong order for anything that an LLM can call. Second, payment reconciliation deserves a scheduled self-check that compares local records against the payment platform, instead of relying on manual queries when something looks off.

## Notes on evidence

Client name, store data and campaign data are omitted. The ~80% AI-assisted figure is my own file-level accounting of authored code, not an estimate from a tool dashboard.
