---
title: "DiffKit"
summary: "Compare source text by lines and produce deterministic patch previews."
category: "Overview"
order: 0
keywords: ["diff", "patch", "compare", "hunks"]
related: ["getting-started.md", "api.md", "typed-api.md"]
---

# DiffKit

DiffKit is a pure Buoy line comparison engine for compare views, change gutters, and patch previews. `DiffLines` creates an immutable result with contiguous change hunks. It can reconstruct the target text or render a unified patch, without invoking a shell or writing a file.

Use it for source-file-sized comparisons with an explicit work budget. It does not perform move detection, binary patching, three-way merging, or streaming comparison. There are no native libraries or package dependencies.

## Exact text semantics

LF ends a line. CRLF bytes remain intact; standalone CR is content. Empty text contains zero lines, and an LF-terminated last line does not create a phantom empty line. Case, Unicode spelling, and final line terminators participate in comparisons without normalization.

Hunk coordinates are zero-based line boundaries. They are neither EditorKit byte offsets nor LSP UTF-16 positions. Each hunk's line arrays retain terminators and are returned as defensive copies.

## Read next

- [Getting started](getting-started.md)
- [API reference](api.md)
