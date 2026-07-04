# MCPExplorer

**The search engine and trust layer for MCP servers.**

### → [mcpexplorer.com](https://mcpexplorer.com)

[![Website](https://img.shields.io/badge/website-mcpexplorer.com-f6821f)](https://mcpexplorer.com)
[![MCP Server](https://img.shields.io/badge/MCP-server%20live-3f9142)](https://mcpexplorer.com/mcp)

MCPExplorer indexes [Model Context Protocol](https://mcpexplorer.com/what-is-an-mcp-server)
servers and **verifies each one with a live handshake** — so you can see the real
tools a server exposes, each labeled by risk, plus a trust score, *before* you
install it.

> Every "awesome MCP" list is a static, unverified README. MCPExplorer actually
> runs the servers.

This repo is the public home for MCPExplorer's docs and its MCP server manifest.
The product itself lives at **[mcpexplorer.com](https://mcpexplorer.com)**.

---

## The problem

As the MCP ecosystem grew past thousands of servers, the lists stopped being
useful. From a name and a one-line description you can't tell:

- what tools a server *actually* exposes,
- whether those tools are read-only or can delete your data,
- whether it needs credentials,
- or whether it's still maintained —

until you install it and hope.

## What MCPExplorer does

- **Verifies, doesn't just list.** It runs the real MCP `tools/list` handshake on
  every server — remote servers over the network, local/stdio servers in a
  hardened, ephemeral Docker sandbox — and extracts the true tool surface.
- **Labels tool risk.** Every tool is tagged **read / write / destructive**, so you
  know what a server can do before you connect it.
- **Scores trust.** A deterministic score from provenance, verification, adoption,
  and freshness — with every contributing factor shown on the page, not a black box.
- **Stays fresh.** Every fact carries provenance and a "last checked" timestamp.

**Currently ~965 servers indexed · 3,300+ tools extracted · 265 handshake-verified.**

## Start here

- 🔍 **Explore the index** — <https://mcpexplorer.com/explore>
- ⭐ **Best MCP servers** (ranked) — <https://mcpexplorer.com/best-mcp-servers>
- 📖 **What is an MCP server?** — <https://mcpexplorer.com/what-is-an-mcp-server>
- 📊 **State of MCP report** — <https://mcpexplorer.com/reports>

---

## The MCPExplorer MCP server

MCPExplorer exposes its own MCP server, so you can **search, vet, and assemble
servers from inside your agent** — without leaving your client. Hand `plan_toolset`
a plain-language goal and get back a recommended, governed toolset.

**Endpoint:** `https://mcpexplorer.com/mcp` (Streamable HTTP, no auth)

**Add it to Claude Code:**

```bash
claude mcp add --transport http mcpexplorer https://mcpexplorer.com/mcp
```

**Or add it to any MCP client:**

```json
{
  "mcpServers": {
    "mcpexplorer": {
      "type": "streamable-http",
      "url": "https://mcpexplorer.com/mcp"
    }
  }
}
```

**Tools (14):**

| Tool | What it does |
|---|---|
| `search_servers` | Search the index by capability, transport, and risk |
| `get_server` | A server's verified tools, install command, and trust breakdown |
| `get_capability` | Servers that provide a given capability |
| `compare_servers` | Side-by-side comparison of two servers |
| `get_alternatives` | Servers similar to a given one |
| `check_trust` | Explainable trust score and reasoning for a server |
| `generate_runtime_config` | Ready-to-paste client config for a server |
| `list_capabilities` | The capability slugs you can search and look up by |
| `list_runtimes` | Runtimes `generate_runtime_config` supports |
| `get_ecosystem_stats` | Live totals + provenance/transport breakdowns |
| `list_loadouts` | Curated server kits for a job (GTM, coding, research, …) |
| `get_loadout` | One kit with live trust + aggregated tool-risk |
| `plan_toolset` | Plain-language goal → a recommended governed toolset |
| `report_broken_server` | Flag a stale or broken listing |

---

## How verification works

Every server flows through a crawler pipeline:

```
discover → fetch → parse → normalize → resolve → enrich →
classify → verify (live handshake) → score trust → decide indexability
```

The **verify** stage is the heart of it. Remote servers are probed over the
network; local/stdio servers are executed in a locked-down container
(`--read-only`, `--cap-drop ALL`, `--security-opt no-new-privileges`, no host
mounts, no secrets, memory/CPU/PID limits, auto-removed) that runs only known
launchers (`npx`/`uvx`/`uv`/`pnpm`/`bunx`). The handshake extracts the real
`tools/list`, and each tool's risk is derived from its name and schema.

A page is only indexed once it has real MCP-server evidence **and** enough
verified facts — so the index stays servers, not framework repos or link lists.

## For agents & programmatic access

- **MCP server:** `https://mcpexplorer.com/mcp` (see above)
- **llms.txt:** <https://mcpexplorer.com/llms.txt>

## Submit a server

Missing something? Submit it at <https://mcpexplorer.com/submit> — it enters the
same verification pipeline as everything else.

## FAQ

**Is this affiliated with Anthropic?**
No. MCP is Anthropic's open protocol; MCPExplorer is an independent index built on
top of it.

**Is the trust score a black box?**
No. It's deterministic, and every `+`/`−` factor is shown on each server's page.

**How do you run untrusted servers safely?**
Local/stdio servers only ever run inside the hardened, ephemeral container
described above — never on the host, never with secrets.

---

<sub>MCPExplorer · [mcpexplorer.com](https://mcpexplorer.com) · a discovery, trust,
and capability graph for MCP servers.</sub>
