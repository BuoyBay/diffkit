# DiffKit · 0.1.0

Pure Buoy line comparison for an IDE's compare view, change gutter and patch
preview. `Import diffkit` brings in `DiffLines`, `TextDiff` and `LineHunk`.
There are no native runtime dependencies.

## API

`DiffLines(oldText, newText, maxCells = 1000000)` returns an immutable `TextDiff`.
Its `HasChanges` property reports exact differences, including case and final
line terminators. `Hunks` returns an array of contiguous changes. Each `LineHunk`
has zero-based `OldStart`/`NewStart` line boundaries, `OldCount`/`NewCount`, and
`OldLines`/`NewLines` arrays whose strings retain their terminators. Arrays are
returned as copies so a consumer cannot corrupt the diff's content.

`TextDiff.ApplyTo(source)` reconstructs the new text from the hunks. It requires
the exact original source; a stale or differently cased source throws
`InvalidArgumentException`. It never writes files.

`TextDiff.UnifiedDiff(oldLabel = "a/file", newLabel = "b/file", context = 3)`
returns a unified patch with correctly merged context hunks, empty-range
coordinates, and `\ No newline at end of file` markers. An unchanged comparison
returns `""`. Context may be zero and must be in `0..1000000`. Labels containing
CR, LF or tab reject so they cannot inject patch headers. The result is text;
DiffKit does not invoke a shell or apply a patch to a workspace.

See [the complete example](../examples/diffkit_demo.bui), which prints a patch
for a changed Buoy source line and its hunk coordinates.

## Line and Unicode semantics

A line ends at LF. CRLF is preserved exactly: CR remains in the line content
and LF remains its terminator, including in unified output. A standalone CR is
content, not a separator. Empty text has zero lines; `"a\n"` has one terminated
line and `"a"` has one unterminated line. There is no phantom empty line at EOF.
Unicode is compared exactly, with no case folding or normalization. Hunks use
line coordinates, not UTF-8 byte or UTF-16 character coordinates.

## Bounds and complexity

The algorithm removes the common prefix and suffix, then computes a longest
common subsequence over the remaining lines. Ties choose deletions before
insertions, giving deterministic minimal insert/delete scripts. The LCS table
uses `(oldMiddleLines + 1) * (newMiddleLines + 1)` integer cells. The budget
rejects excessive comparisons before allocating this table; callers may select
`maxCells` in `1..10000000`. A budget failure throws `InvalidArgumentException`
rather than returning an approximate diff. Pure insertions, pure deletions and
unchanged text need no LCS table.

The default table ceiling is about 8 MB (64-bit integers), excluding the input,
line arrays and operation records. Other storage grows linearly with input size.
This is intended for source-file comparison; it is not a streaming or binary
patch implementation, and it does not perform move detection or three-way merge.

## Package archive

The package root contains the source and compiler manifest. A compatible sealer
can create an archive after any dependency prerequisites are provisioned:

```sh
mkdir -p dist
"${BPM_PACKAGER:-buoy}" pkg seal . -o dist/diffkit-0.1.0.buipkg
```

Install that archive into the consumer's `packages/` directory, or use BPM to
build directly from the source checkout. Sealing alone is not a behavioral check.
