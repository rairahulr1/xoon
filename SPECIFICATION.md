# XOON Specification (Version 1.0.0)

> **XML-Oriented Object Notation & Semantic HTML Standard**  
> *The Single-File Universal Document Format for AI Agents and Browsers.*

---

## 1. Abstract

Modern software engineering and multi-agent systems require documents that serve two conflicting audiences:
1. **AI Agents**: Need minimal token consumption, zero stylistic noise, and strict boundary scopes to avoid hallucinations.
2. **Humans**: Need instant, zero-build visual previews with responsive tables, accessible typography, and interactive sorting.

**XOON** resolves this contradiction through a **Single-File Architecture**:
- An **outer Semantic XML/HTML5 shell** (`<article>`, `<header>`, `<metadata>`, `<dataset>`, `<rules>`) providing unambiguous structural bounding boxes.
- An **inner schema-first TOON data stream** (`table[count]{col1,col2}:`) eliminating repetitive tag bloat.
- **Strictly externalized styling and rendering runtime** (`style.css`, `xoon-renderer.js`) to guarantee that **zero tokens** are wasted on CSS rules or JavaScript logic when read by an LLM.

---

## 2. Document Anatomy

A valid XOON document is a standards-compliant HTML5 file structured as follows:

```html
<!DOCTYPE html>
<html lang="en" data-engine="xoon">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Document Title</title>
  <link rel="stylesheet" href="./style.css">
  <script defer src="./xoon-renderer.js"></script>
</head>
<body>

<article class="xoon-container">
  <header class="document-header">
    <h1>Document Title</h1>
    <metadata>
      key: value
      as_of: YYYY-MM-DD
      owner: Name
    </metadata>
  </header>

  <!-- Browser mounting point: empty in source, hydrated at runtime -->
  <main id="app" aria-live="polite"></main>

  <!-- Authoritative Data Core: XML semantic tag + schema stream -->
  <dataset id="data" type="text/xoon">
  table_name[count]{col1,col2,col3}:
    row1_c1,row1_c2,row1_c3
    row2_c1,row2_c2,row2_c3

  list_name[count]:
    - Item 1
    - Item 2
  </dataset>

  <!-- Semantic Directives -->
  <rules>
    - Rule description one.
    - Rule description two.
  </rules>
</article>

</body>
</html>
```

---

## 3. Core Rules & Constraints

### 3.1 Externalization Invariant
Under no circumstances may a XOON document contain inline `<style>` tags or inline `<script>` function implementations. All stylesheets must be linked via `<link rel="stylesheet">`, and all client rendering engines must be loaded via `<script defer src="...">`.
*Rationale*: Inline CSS and JS waste up to 70% of an LLM's context window on layout code.

### 3.2 Schema Integrity Gate
Every tabular dataset declared inside `<dataset>` must specify an explicit count `[count]` and column definitions `{col1,col2,...}`:
```xoon
services[3]{name,runtime,status}:
  auth,Go,active
  billing,Laravel,active
  search,Node,idle
```
The CLI (`xoon validate`) treats any discrepancy between `[count]` and the physical row count as an error, preventing silent truncation or generative hallucination.

### 3.3 Semantic Tags Hierarchy
XOON mandates semantic HTML5 / XML landmark elements:
- `<header>`: High-level document identity.
- `<metadata>`: Key-value attributes.
- `<main id="app">`: Designated mounting canvas for client hydration.
- `<dataset id="data" type="text/xoon">`: The single source of truth for all structured data.
- `<rules>` or `<notes>`: Unambiguous constraints for AI reasoning.
