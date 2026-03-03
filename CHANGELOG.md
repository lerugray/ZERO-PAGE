# ZERO PAGE — Changelog

## 2026-03-03

### Bug Fixes

- **Debugger step highlighting**: The STEP button now highlights the line *about to execute*, not the line that already ran. On the first STEP click, the first instruction is highlighted without executing. Subsequent clicks execute the highlighted line and advance the highlight to the next one. (Reported by player feedback)

- **Puzzle 2 ambiguous objective text**: The UNSTABLE case objective previously read "Else store $00 into $03" which players interpreted as "store the contents of address $00" rather than "store the value zero." Changed to "Else store 0 into $03 (UNSTABLE)" to match the actual test logic. (Reported by AJ Erickson)
