# Create line diffs and unified patches

Pure Buoy line comparison for an IDE's compare view, change gutter and patch
preview. `Import diffkit` brings in `DiffLines`, `TextDiff` and `LineHunk`.
There are no native runtime dependencies.

## Install

From a Buoy consumer project with BPM installed:

```sh
bpm add diffkit --git https://github.com/BuoyBay/diffkit.git --rev refs/heads/main
```

BPM records the resolved commit in its lockfile. Keep that lockfile with the
consumer project for reproducible installs.

## Example

```basic
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

## Build from source

With the native `build-packages` driver from [devtools](https://github.com/BuoyBay/devtools)
on your `PATH`, run from this checkout:

```sh
build-packages --root "$PWD" --compile-consumer diffkit
```

The driver checks source targets and compiles an import consumer. See the
[API guide](docs/API.md) for behavior, limits and any host prerequisites.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```


## Contributing

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
