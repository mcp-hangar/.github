<div align="center">

# MCP Hangar

**The MCP policy enforcement plane.**

One deterministic allow/deny path on every Model Context Protocol call — where Cilium sits on the packet path and OPA Gatekeeper sits on the admission path.

[![mcp-hangar on PyPI](https://img.shields.io/pypi/v/mcp-hangar?label=pypi)](https://pypi.org/project/mcp-hangar/)
[![Latest operator release](https://img.shields.io/github/v/release/mcp-hangar/mcp-hangar-operator?label=operator)](https://github.com/mcp-hangar/mcp-hangar-operator/releases)
[![MIT licensed](https://img.shields.io/github/license/mcp-hangar/mcp-hangar)](https://github.com/mcp-hangar/mcp-hangar/blob/main/LICENSE)

</div>

Hangar enforces explicit policy **on** the MCP call path, not beside it. Every tool call crosses one decision path: caller identity, tool-access authorization, schema digest verification, egress rules. Nothing is inferred — there are no anomaly scores and no learned baselines, so a decision is reproducible from the policy that produced it. Self-hosted and MIT across the stack.

### Enforced today

- Caller identity from JWT/OIDC, with RFC 8707 audience binding and multi-issuer trust.
- **Tool-schema** digest pinning: a server that changes a pinned tool's schema fails closed.
- L7 egress policy written in MCP semantics — which upstream, which tool, which arguments — through the `MCPEgressPolicy` CRD (alpha API).
- Governed task relay with a consent gate: Hangar interposes on the task lifecycle and never executes the task. Preview, on the 2.0 line.
- Attributable audit chain, exported to SIEM as CEF, LEEF 2.0, RFC 5424 syslog or JSON-lines, and to OTLP.
- Kubernetes-native: CRDs, admission validation, and an operator that reconciles them.

### What Hangar deliberately is not

- **Not ML threat detection.** Policy is deterministic and auditable.
- **Not SaaS.** No hosted control plane, and none planned.
- **Not MDM or endpoint discovery.** It governs MCP servers you register, not devices.
- **Not a connector marketplace.** It governs servers; it does not distribute them.
- **Not full OWASP MCP Top 10 coverage.** Command injection, intent-flow subversion and context injection are out of scope by design: prompt and argument semantics are not parsed. [Coverage per category](https://mcp-hangar.io/docs/security/OWASP_MCP_TOP_10_COVERAGE).

### Repositories

| Repository | What it is |
| --- | --- |
| [mcp-hangar](https://github.com/mcp-hangar/mcp-hangar) | The enforcement plane itself, and the Python package. |
| [mcp-hangar-operator](https://github.com/mcp-hangar/mcp-hangar-operator) | Kubernetes operator: CRDs, admission validation, default-deny egress. |
| [helm-charts](https://github.com/mcp-hangar/helm-charts) | OCI-published charts for the core and the operator. |
| [docs](https://github.com/mcp-hangar/docs) | Policy model, guides, ADRs, OWASP coverage. |
| [benchmarks](https://github.com/mcp-hangar/benchmarks) | Sequential vs parallel MCP tool-call measurements. |

### Start

```bash
pip install mcp-hangar
helm install mcp-hangar oci://ghcr.io/mcp-hangar/charts/mcp-hangar --namespace mcp-hangar
```

---

[Website](https://mcp-hangar.io) · [Docs](https://mcp-hangar.io/docs) · [Learn](https://mcp-hangar.io/learn) · [Why Is This Down?](https://whyisthisdown.com) — our writing on observability and operations · [MIT License](https://github.com/mcp-hangar/mcp-hangar/blob/main/LICENSE)
