# ZERO PAGE — Changelog

## 2026-03-03 (c)

### Bug Fixes

- **Immediate value notation in mission objectives**: Fixed objectives across multiple puzzles where hex values were written as `$XX` (which looks like a memory address) instead of `#$XX` (proper 6502 immediate notation). This could mislead players into using zero-page addressing instead of immediate mode. Affected puzzles: 2, 4, 6, 7, 8, plus equivalent fixes in hidden content.

### UI Improvements

- **Resizable left panel split**: The Mission Brief and Technical Reference panels in the left column now have a draggable resize handle between them. Players can adjust how much space each panel gets. Both panels scroll independently — no scroll traps.

- **Tech reference formatting**: Split cramped STX/STY and TAX/TAY/TXA/TYA entries onto separate lines to match the clean one-instruction-per-line format used everywhere else.

---

## 2026-03-03 (b)

### UI Improvements

- **Tech reference formatting cleanup**: Reformatted shift/rotate instructions (ASL, LSR, ROL, ROR) to prevent awkward line wrapping. Split PHA/PLA onto separate lines.

- **FLAGS notes text**: Increased size from 10px to 12px and brightened color from amber-dim to amber for better readability.

---

## 2026-03-03

### Bug Fixes

- **Debugger step highlighting**: The STEP button now highlights the line *about to execute*, not the line that already ran. On the first STEP click, the first instruction is highlighted without executing. Subsequent clicks execute the highlighted line and advance the highlight to the next one. (Reported by player feedback)

- **Puzzle 2 ambiguous objective text**: The UNSTABLE case objective previously read "Else store $00 into $03" which players interpreted as "store the contents of address $00" rather than "store the value zero." Changed to "Else store 0 into $03 (UNSTABLE)" to match the actual test logic. (Reported by AJ Erickson)

### UI Improvements

- **Left panel scroll trap fix**: Removed nested scroll areas in the left column that were trapping the scroll wheel. The entire left column now scrolls as one unified area.

- **Left panel font sizes**: Bumped up text sizes in the left column for readability — narrative and objectives from 15px to 16px, tech reference from 11px to 13px.

### Source Protection

- **Hidden content obfuscation**: Hidden puzzle definitions (HIDDEN_MISSIONS), system logs (SYSTEM_LOGS), and corruption trigger logic (checkCorruptionTriggers) are now base64-encoded in the source. Casual source viewers will see an opaque blob instead of readable puzzle names, narratives, unlock conditions, and reward text. The game functions identically — the content is decoded at runtime.
