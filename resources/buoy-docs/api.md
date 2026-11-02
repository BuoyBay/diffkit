---
title: "DiffKit API"
summary: "Public types and operations supplied by DiffKit."
category: "Reference"
order: 20
keywords: ["diff", "patch", "compare", "hunks"]
symbols: ["DiffLines", "TextDiff", "TextDiff.Hunks", "TextDiff.HasChanges", "TextDiff.ApplyTo", "TextDiff.UnifiedDiff", "LineHunk"]
related: ["index.md", "getting-started.md", "typed-api.md"]
---

# API reference

## Compare

| Symbol | Contract |
| --- | --- |
| `DiffLines(oldText, newText, maxCells = 1000000)` | Return an immutable `TextDiff`; reject an excessive remaining LCS table before allocation. |
| `TextDiff.HasChanges` | True for any exact difference, including case or final terminators. |
| `TextDiff.Hunks` | Copy of contiguous `LineHunk` changes. |

The algorithm chooses a minimal insertion/deletion script. Equal LCS choices prefer deletions, making output deterministic. Storage outside the table grows with the input and operation records.

## Hunk data

| Property | Meaning |
| --- | --- |
| `OldStart`, `NewStart` | Zero-based line boundaries in old/new content. |
| `OldCount`, `NewCount` | Numbers of removed/inserted lines. |
| `OldLines`, `NewLines` | Independent arrays retaining original terminators. |

## Apply and export

`TextDiff.ApplyTo(source)` requires the exact original source and returns the new text; it does not mutate a workspace or filesystem. A differently cased source is stale too.

`TextDiff.UnifiedDiff(oldLabel = "a/file", newLabel = "b/file", context = 3)` returns patch text, or an empty string for no change. Context is 0 through 1,000,000. Labels cannot contain CR, LF, or tab. Context hunks merge correctly and unterminated files receive the conventional no-newline marker. External patch application belongs to the caller.

[Getting started](getting-started.md) · [Package overview](index.md)

[Full typed API: properties, parameters, defaults, and returns](typed-api.md)
