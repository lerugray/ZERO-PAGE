# ZERO PAGE — Changelog

## 2026-03-03

### Bug Fixes

- **Debugger step highlighting**: The STEP button now highlights the line *about to execute*, not the line that already ran. On the first STEP click, the first instruction is highlighted without executing. Subsequent clicks execute the highlighted line and advance the highlight to the next one. (Reported by player feedback)

- **Puzzle 2 ambiguous objective text**: The UNSTABLE case objective previously read "Else store $00 into $03" which players interpreted as "store the contents of address $00" rather than "store the value zero." Changed to "Else store 0 into $03 (UNSTABLE)" to match the actual test logic. (Reported by AJ Erickson)

### UI Improvements

- **Left panel scroll trap fix**: Removed nested scroll areas in the left column that were trapping the scroll wheel. The entire left column now scrolls as one unified area.

- **Left panel font sizes**: Bumped up text sizes in the left column for readability — narrative and objectives from 15px to 16px, tech reference from 11px to 13px.

### Source Protection

- **Hidden content obfuscation**: Hidden puzzle definitions (HIDDEN_MISSIONS), system logs (SYSTEM_LOGS), and corruption trigger logic (checkCorruptionTriggers) are now base64-encoded in the source. Casual source viewers will see an opaque blob instead of readable puzzle names, narratives, unlock conditions, and reward text. The game functions identically — the content is decoded at runtime.
