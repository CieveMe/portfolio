# Case studies — Zhen He

Written-up delivery work, with the constraints and the numbers. Client names, contracts, customer data and internal endpoints are intentionally omitted; everything here is either my own architecture description or a sanitised metric.

**Contact** · [cieve94107@gmail.com](mailto:cieve94107@gmail.com) · [LinkedIn](https://www.linkedin.com/in/zhen-he-a2336a43a) · [GitHub](https://github.com/CieveMe)

---

| # | Case study | What it shows |
|---|---|---|
| 1 | [Video surveillance & vehicle-device platform](case-studies/01-video-surveillance-platform.md) | GB28181/GA-T 1400 integration, live + playback streaming, multi-tenant data scope, production delivery discipline (tests, rollback, handover) |
| 2 | [WeChat marketing & payments platform](case-studies/02-wechat-marketing-platform.md) | Full WeChat Pay V3 loop, hot-configurable campaign engine, AI-assisted delivery (~80% of code), read-only MCP tooling for operations |
| 3 | [mysql-ops-mcp](https://github.com/CieveMe/mysql-ops-mcp) *(live repo)* | Read-only-first MCP server: SSH tunnel management, SQL whitelist guard, mutating tools off by default, 24 unit tests |

### How I work

- **Deliver end to end**, not a slice: requirements → architecture → implementation → deployment → documentation → source handover.
- **Measure before claiming**: test counts, stream counts, rollback drills. If something could not be verified on real hardware, it is written down as unverified instead of quietly assumed.
- **AI in the loop, evidence in the repo**: Claude Code / MCP servers / agent skills for speed, with the reasoning, decisions and verification steps kept as files so the next person (or agent) can pick it up.
