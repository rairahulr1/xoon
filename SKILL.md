---
name: xoon
description: "XML-Oriented Object Notation (XOON) & Single-File Semantic HTML Standard. Slashes token consumption by 55-65% vs XML/HTML, eliminating dual-file drift while providing zero-build browser previews."
---

# `xoon` — XML-Oriented Object Notation & Single-File Semantic HTML Skill

XOON is a single-file universal document format engineered for AI coding agents and human developers. It marries **XML-like semantic structure** (`<metadata>`, `<dataset>`, `<rules>`) with **schema-first TOON data streams**, strictly externalizing all CSS and JS to preserve 100% of the LLM context window for domain logic.

---

## 1. Operating Protocol for AI Agents

### Reading XOON Documents
- **Single Read**: Read `document.xoon.html` directly.
- **Context Isolation**: The file contains **0 bytes of inline CSS or JS**. Every token ingested is pure metadata, semantic tags, and dense data.
- **Direct Data Extraction**: To inspect just the dataset, extract `<dataset id="data">...</dataset>`.

### Creating XOON Documents
Use the template or run:
```bash
xoon init "Document Title"
```
Or draft a `.html` file following the [XOON Specification](SPECIFICATION.md).

### Modifying XOON Documents
1. Edit data lines inside `<dataset id="data" type="text/xoon">` directly.
2. Run `xoon format <file>` to automatically recompute `[count]` values.
3. Run `xoon validate <file>` to verify schema and column counts.
4. Open or refresh in any browser — changes appear immediately without running static site builds.

---

## 2. CLI Tooling Reference

```bash
# Validate schema and row counts
xoon validate <file.html>

# Automatically recount rows and format in place
xoon format <file.html>

# Initialize a new single-file document
xoon init "Project Roadmap"

# Start instant preview server
xoon serve --port 8080
```
