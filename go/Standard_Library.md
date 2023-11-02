# Standard library

## Overview
> based on go1.21.3

| Name | Description |
|----|----|
| container | Implements basic data structure like heap, doubly linked list and circular list. |
| context | Defines the Context type, which carries deadlines, cancellation signals, and other request-scoped values across API boundaries and between processes. |
| encoding | Functions for converting data to and from byte-level and textual representations. |
| maps | Functions maps. |
| slices | Functions for slices. |
| test | Functions for automated testing. |
| time | Functions for measuring and displaying time. |

## Slice
| Function | Description |
|----|----|
| `func Clone(s S) S` | Clones a slice `s` (shallow clone). |
| `func Contains(s S, v E) bool` | Checks the slice `s` has the value `v` or not. |
| `func Index(s S, v E) int` | Returns the index of the first occurrence of the value `v` from the slice `s` |
| `func Sort(x S)` | Sorts the slice `x` in ascending order. |
| `func SortFunc(x S, cmp func(a, b E) int)` | Sorts the slice `x` in ascending order as determined by the `cmp` function. |
