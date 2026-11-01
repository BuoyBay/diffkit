---
title: "DiffKit getting started"
summary: "Install DiffKit and try a complete usage example."
category: "Guides"
order: 10
keywords: ["diff", "patch", "compare", "hunks"]
related: ["index.md", "api.md", "typed-api.md"]
---

# Getting started

Copy `diffkit-0.1.0.buipkg` into `packages/`, then import `diffkit`.

## Preview and reconstruct a change

Keep the original text together with the diff. `ApplyTo` deliberately rejects a stale base instead of searching for a nearby place to apply the change.

```buoy
Import diffkit

Sub Main()
    Dim beforeText As String = "first\nsecond\n"
    Dim afterText As String = "first\nupdated\n"
    Dim delta As TextDiff = DiffLines(beforeText, afterText)
    If delta.HasChanges Then
        Print delta.UnifiedDiff("a/example.txt", "b/example.txt", 1)
        Print delta.ApplyTo(beforeText)
    End If
    For Each hunk As LineHunk In delta.Hunks
        Print "Old line: " + String.From(hunk.OldStart)
        Print "Removed: " + String.From(hunk.OldCount)
        Print "Added: " + String.From(hunk.NewCount)
    Next
End Sub
```

## Choose a budget

The default `maxCells` is 1,000,000 integer cells, roughly 8 MB for the LCS table alone. The valid range is 1 through 10,000,000. The engine first removes a common prefix and suffix; insert-only, delete-only, and unchanged comparisons need no LCS table.

Excessive work, invalid patch labels/context, or a stale base throws `InvalidArgumentException`. A budget failure yields no approximate result. Keep a fallback UI such as a plain side-by-side view for changes too large for the chosen budget.

[Package overview](index.md) · [API reference](api.md)
