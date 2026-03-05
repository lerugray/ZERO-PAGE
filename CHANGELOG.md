# ZERO PAGE — Changelog

## 2026-03-05

### Bug Fixes

- **Mission brief overflow**: Mission briefing text no longer falls under the footer on smaller screens. Changed flex layout to allow the brief panel to shrink with a max-height cap.

- **LCD display for all output slots**: Replaced 4 hardcoded LCD rows with a dynamic configuration supporting all 15 missions. LCD rows now update automatically based on the active mission's output slots.

- **Snake game memory flicker**: Fixed the full memory grid in sandbox mode flickering during Snake gameplay by switching from full DOM rebuilds to differential cell updates.

- **Save progress bug**: Fixed save system not persisting the correct mission index — completing a mission and refreshing would drop the player back one mission. Save now fires after the index advances.

### New Features

- **localStorage save system**: Game progress now persists across browser sessions. Saves cleared missions, best cycle scores, hidden mission unlocks, and current mission index. "CLEAR SAVE" button in header with confirmation prompt. Save/load abstracted behind `_writeSave()`/`_readSave()` for future Electron compatibility.

- **Editor draft persistence**: In-progress code survives browser refresh. Draft is cleared on mission advance or replay so new missions start fresh.

- **Mission select: classified section pinned**: Classified modules section is now always visible at the bottom of the mission select modal, independent of scroll position.

- **Mission select: subtle routing hints**: Certain missions now provide subliminal visual feedback in the classified section on hover. Encoded.

### UI Improvements

- **Mission brief scroll reset**: The mission briefing panel now scrolls to the top when advancing to a new mission, instead of retaining the previous mission's scroll position.

---

## 2026-03-04

### Bug Fixes

- **Right column overflow in mission mode**: The right column (Machine State, Memory, LCD Display) was clipping content on smaller screens with no way to scroll. Added `overflow-y: auto` so a scrollbar appears when needed.

---

## 2026-03-03 (e)

### Sandbox Mode — Sticky Framebuffer

- **Framebuffer pinned at top**: The framebuffer canvas now stays fixed at the top of the sandbox panel while the full memory grid scrolls independently beneath it. Players can watch live framebuffer output while inspecting any memory address.

---

## 2026-03-03 (d)

### Sandbox Mode — UI Overhaul

- **Full memory grid relocated**: Moved the full memory map ($00–$FF) from the right column into the center column beneath the framebuffer display. Both are sandbox-only panels that now scroll together naturally, eliminating the right column scroll trap.

- **Resizable editor/framebuffer split**: Added a draggable horizontal resize handle between the code editor and the framebuffer+memory section. Players can adjust the balance to their preference — wider memory grid or wider editor.

- **LCD watch slots**: In sandbox mode, the LCD panel now shows 4 rows instead of the redundant $00–$03 mission rows:
  - RANDOM $FE — RNG register with flash animation
  - KEY INPUT $FF — keyboard register with flash animation
  - Two user-configurable WATCH rows — click the label to type any hex address ($00–$FF) to monitor

- **Right column cleanup**: Right column no longer overflows in sandbox mode. Machine State, Memory $00–$0F, and LCD Display fit cleanly without scrolling.

---

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
