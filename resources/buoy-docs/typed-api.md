---
title: "diffkit typed API"
summary: "Source-derived public signatures, properties, parameters, defaults, and return values."
category: "Reference"
order: 21
types: ["LineHunk", "TextDiff"]
dependencies: []
symbols: ["LineHunk", "TextDiff"]
related: ["api.md", "getting-started.md", "index.md"]
---

# diffkit typed API

Generated from the package source declarations. Platform guards are shown where present; inherited members are documented on the base type. See the [API contracts](api.md) for behavior and limitations.

## LineHunk

`Public Class LineHunk`

A contiguous change. Coordinates are zero-based line boundaries; arrays include terminators.

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `OldStart` | `Integer` | Yes | No | — | — |
| `NewStart` | `Integer` | Yes | No | — | — |
| `OldCount` | `Integer` | Yes | No | — | — |
| `NewCount` | `Integer` | Yes | No | — | — |
| `OldLines` | `Array Of String` | Yes | No | — | — |
| `NewLines` | `Array Of String` | Yes | No | — | — |

## TextDiff

`Public Class TextDiff`

Exact-source line diff. Build with DiffLines; instances and their hunks are immutable.

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `Hunks` | `Array Of LineHunk` | Yes | No | — | — |
| `HasChanges` | `Boolean` | Yes | No | — | — |

### ApplyTo

`Public Function ApplyTo(source As String) As String`

Reconstruct using the hunks, refusing any source other than the exact diff base.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `source` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### UnifiedDiff

`Public Function UnifiedDiff(oldLabel As String = "a/file", newLabel As String = "b/file", context As Integer = 3) As String`

GNU-compatible unified hunks. Headers reject newlines/tabs; zero context is supported.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `oldLabel` | `String` | `ByVal` | `"a/file"` |
| `newLabel` | `String` | `ByVal` | `"b/file"` |
| `context` | `Integer` | `ByVal` | `3` |

#### Returns

| Type |
| --- |
| `String` |

## DiffKit functions

### DiffLines

`Public Function DiffLines(oldText As String, newText As String, maxCells As Integer = 1000000) As TextDiff`

Minimal line edit script via bounded LCS after trimming common prefix/suffix.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `oldText` | `String` | `ByVal` | Required |
| `newText` | `String` | `ByVal` | Required |
| `maxCells` | `Integer` | `ByVal` | `1000000` |

#### Returns

| Type |
| --- |
| `TextDiff` |
