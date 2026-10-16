# XOON — XML-Oriented Object Notation

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Zero Build](https://img.shields.io/badge/Build-Zero%20Dependency-brightgreen.svg)]()
[![Single File](https://img.shields.io/badge/Architecture-Single%20File-indigo.svg)](#architecture)
[![Agent Skill](https://img.shields.io/badge/Agent%20Skill-Compatible-purple.svg)](SKILL.md)
[![Token Reduction](https://img.shields.io/badge/Token%20Savings-58%25%20vs%20HTML-orange.svg)](#benchmarks)

> **Single-file Semantic HTML (XML-like structure) + TOON data notation, strictly externalizing JS and CSS for instant frontend preview and zero context loss for AI agents.**

---

## ⚡ The Breakthrough: "Clean Shell, Dense Core"

Until now, developers and AI agents had to choose between two painful compromises:
1. **The Dual-File Problem (`spec.md` + `spec.html`)**:  
   Requires running build/sync scripts (`toon sync`, VitePress, Hugo). Forgetting to sync creates stale visual previews and broken git diffs.
2. **The Inline Bloat Problem (Raw HTML)**:  
   Putting everything into a single HTML file injects hundreds of lines of `<style>` and `<script>`, wasting up to **70% of an LLM's context window** on CSS typography and UI logic instead of business data.

**XOON** eliminates this dilemma:
- **Single File**: A standards-compliant `.html` (or `.xoon.html`) file is the sole source of truth.
- **XML-Like Semantic Structure**: Uses `<header>`, `<metadata>`, `<dataset>`, and `<rules>` for unambiguous LLM scope isolation.
- **Zero Token Waste for AI**: All CSS (`style.css`) and JS (`xoon-renderer.js`) are strictly externalized. When an LLM reads the file, **100% of the tokens are pure domain data**.
- **Instant Browser Preview**: Double-clicking the file in any operating system opens Chrome/Safari/Firefox with interactive, sortable tables and responsive cards—with **zero build step**.

---

## 📊 Side-by-Side Comparison

Here is how the exact same 20-service microservices architecture dataset is represented across formats:

### 1. Pure XML (`1,858 tokens` — 2.34x larger)
```xml
<services>
  <service>
    <name>auth-gateway</name>
    <service_mesh>Istio</service_mesh>
    <runtime>Go 1.22</runtime>
    <database>Redis 7.2</database>
    <p99_latency_ms>14</p99_latency_ms>
    <uptime_pct>99.99</uptime_pct>
    <tier>Critical</tier>
  </service>
  <!-- Repetitive closing tags repeated for every single row! -->
</services>
```

### 2. Semantic HTML (`1,911 tokens` — 2.41x larger)
```html
<section>
  <h2>Services</h2>
  <table>
    <thead><tr><th scope="col">Name</th>...</tr></thead>
    <tbody>
      <tr><td>auth-gateway</td><td>Istio</td>...</tr>
    </tbody>
  </table>
</section>
```

### 3. Indented JSON (`1,473 tokens` — 1.86x larger)
```json
{
  "services": [
    {
      "name": "auth-gateway",
      "service_mesh": "Istio",
      "runtime": "Go 1.22",
      "database": "Redis 7.2",
      "p99_latency_ms": 14,
      "uptime_pct": 99.99,
      "tier": "Critical"
    }
  ]
}
```

### 4. Single-File XOON (`794 tokens` — Baseline 1.0x)
```html
<article class="xoon-container">
  <header>
    <h1>Enterprise Microservices Radar</h1>
    <metadata>
      as_of: 2026-10-16
      owner: Architecture Guild
    </metadata>
  </header>

  <main id="app"></main>

  <dataset id="data" type="text/xoon">
  services[20]{name,service_mesh,runtime,database,p99_latency_ms,uptime_pct,tier}:
    auth-gateway,Istio,Go 1.22,Redis 7.2,14,99.99,Critical
    billing-ledger,Linkerd,PHP 8.3 Laravel,PostgreSQL 16,38,99.95,Critical
    notification-hub,Consul,Node.js 20,MongoDB 7.0,85,99.90,Standard
    ...
  </dataset>

  <rules>
    - P99 latency above 100ms triggers automatic scaling alert.
  </rules>
</article>
```
*Advantage*: Retains HTML doctype and external CSS/JS, but cuts tokens by **58% vs HTML** and **57% vs XML**!

---

## 📈 Measured 5-Way Benchmarks

Measured using the standard OpenAI `cl100k_base` and `o200k_base` tokenizers on identical 20-service datasets:

| Format | File Size (Bytes) | Characters | Tokens (`cl100k`) | Tokens (`o200k`) | Multiplier | Reduction vs XOON |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Semantic HTML** | 6,684 | 6,684 | **1,911** | 1,916 | 2.41x | **58.45% less tokens** |
| **Pure XML** | 6,336 | 6,336 | **1,858** | 1,905 | 2.34x | **57.27% less tokens** |
| **JSON (Indented)** | 4,730 | 4,730 | **1,473** | 1,487 | 1.86x | **46.10% less tokens** |
| **Markdown Table** | 2,145 | 2,145 | **696** | 704 | 0.88x | — |
| **Single-File XOON** | **2,417** | **2,417** | **794** | **802** | **1.00x** | **Complete Single File** |

> 💡 *Note*: If the AI extracts only the `<dataset>` payload, the token count drops to **631 tokens**—beating raw Markdown while maintaining strict schema gates.

---

## 🏗️ Architecture

```
                      Single File: document.xoon.html
      ┌─────────────────────────────────────────────────────────────┐
      │  <head>                                                     │
      │    <link rel="stylesheet" href="./style.css">               │
      │    <script defer src="./xoon-renderer.js"></script>         │
      │  </head>                                                    │
      │                                                             │
      │  <article class="xoon-container">                           │
      │    <header><metadata>...</metadata></header>                │
      │    <main id="app"></main>   (hydrated by browser runtime)   │
      │    <dataset id="data">                                      │
      │      table[20]{name,mesh,runtime}:                          │
      │        auth,Istio,Go                                        │
      │    </dataset>                                               │
      │    <rules>...</rules>                                       │
      │  </article>                                                 │
      └──────────────────────────────┬──────────────────────────────┘
                                     │
            ┌────────────────────────┴────────────────────────┐
            ▼                                                 ▼
   [ Browser User View ]                             [ AI Agent Context ]
   - Double-click to preview                         - 1 single file to read
   - Zero-build client hydration                     - 0 tokens wasted on styles/scripts
   - Interactive column sorting                      - XML tags create strict scope
   - Dark/Light mode styles                          - In-place editing (no dual sync)
```

---

## 📦 Installation & Setup

### 1. Install CLI
```bash
git clone https://github.com/rairahulr1/xoon.git
cd xoon
mkdir -p ~/.local/bin
ln -s $(pwd)/bin/xoon ~/.local/bin/xoon
```

Verify:
```bash
xoon --help
```

### 2. Scaffold a New Document
```bash
xoon init "Microservices Radar"
# Creates Microservices-Radar.xoon.html
```

### 3. Open in Browser
Double-click `Microservices-Radar.xoon.html` or start the local preview server:
```bash
xoon serve --port 8000
```

---

## 🛠️ CLI Reference

```bash
# Validate schema consistency and row counts
xoon validate document.xoon.html

# Recount [N] array headers in place
xoon format document.xoon.html

# Start instant preview web server
xoon serve --port 8000

# Run the 5-way tokenizer benchmark
python3 benchmark/compare.py
```

---

## 📜 License

MIT License. Designed for the multi-agent AI coding era.
