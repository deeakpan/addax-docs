# DCA Orders

DCA splits a strategy into multiple time slices, each with execution constraints.

## Core fields

- `startTime`
- `intervalSeconds`
- `sliceIndex` (0-based)
- `totalSlices`
- `fillWindowSeconds`
- per-slice `minOut`

## Execution model

- Each slice should be represented by its own signed order context.
- Reactor validates slice timing and rejects early/late fills.
- Per-slice minimum output protects users from bad fills.

## Missed slices

Typical policy choices:

- Skip and continue next slice
- Retry within allowed window
- Cancel strategy after threshold misses

Choose and document filler policy clearly.

