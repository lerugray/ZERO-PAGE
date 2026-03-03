# ZERO PAGE

## What This Is
A 6502 assembly language puzzle game played in the browser. The player takes on the role of a contractor writing low-level code for an unnamed government program. Puzzles teach real 6502 instructions progressively while an understated narrative builds moral unease through bureaucratic debrief text.

Think TIS-100 meets moral complicity. The mechanics are the hook; the narrative is the point.

## How to Run
Open `index.html` in a web browser. Everything is contained in a single HTML file — no build step, no dependencies, no server needed.

## Project Structure
- **Single-file architecture**: The entire game (HTML, CSS, JavaScript, puzzle definitions, assembler, simulator, UI) lives in one HTML file.
- **Puzzle definitions**: Located in a `MISSIONS` array in the JS. Each entry has: `id`, `name`, `narrative`, `objectives` array, `outputSlot`/`outputSlots`, `testLogic` function, and `debrief` object.
- **Puzzle roadmap**: See `zero-page-puzzle-roadmap.docx` for the full design document covering all puzzle clusters, design principles, narrative guidelines, and implementation notes.

## Current Status
- **Stable** — the game is fully playable
- **18 puzzles implemented** (15 in the main campaign across 4 clusters, plus 3 that are accessible through alternate means)
- **Sandbox mode** available for freeform 6502 programming
- **Snake demo** included as a sandbox showcase

## Puzzle Campaign Structure
The campaign is organized into 4 clusters that escalate in both technical complexity and narrative tone:

1. **Puzzles 1–3 (Foundation)**: Basic instructions — LDA, STA, SEC, SBC, AND, CMP, branching, zero-page addressing. Tone: routine.
2. **Puzzles 4–7 (Institutional)**: Index registers, looping, indexed addressing, bitwise operations. Tone: terse, procedural.
3. **Puzzles 8–11 (Complicit)**: Subroutines (JSR/RTS), stack (PHA/PLA), indirect indexed addressing. Tone: debriefs stop explaining.
4. **Puzzles 12–15 (Terminal)**: Bit rotation, memory INC/DEC, full pipeline puzzles. Tone: minimal. Final debrief is a single line.

Each puzzle introduces exactly one new concept, uses it meaningfully, and advances the narrative.

## Design Principles
- **Instruction gating**: Puzzles progressively reveal the 6502 instruction set. The tech reference panel should ideally hide/grey out locked instructions.
- **Narrative escalation**: Debrief tone moves from verbose bureaucratic to single-line silence. Never name the end-use. Let the player's imagination fill the gap.
- **Difficulty curve**: Early puzzles solvable in 5–8 lines, mid in 10–20, late may require subroutines and multi-pass logic. Cycle limits set at ~2–2.5x optimal solution.
- **Sandbox as contrast**: Sandbox mode is the player's own space — no framing, no unease, just freedom to code.

## Known Improvement Areas
1. **Debugger step highlighting**: When stepping through code, the line *about to be executed* should be highlighted, not the line that already ran.
2. **Updated roadmap**: The roadmap document needs updating to reflect the current full puzzle set including all 18 puzzles.
3. **Solutions document**: A separate text file with solutions for each puzzle and instructions for accessing all content.
4. **Evaluating discoverability**: Some in-game hints exist via corrupted debrief text, but it's worth evaluating whether these are too cryptic for players to find.

## Future Plans
- **Itch.io release** first (pay-what-you-want or free) to gather feedback
- **Steam release** ($5–7 range) via Electron wrapper if there's sufficient interest
- **Sound design**: Ambient drone, keyclick sounds, tones for RUN/PASS/FAIL/BRK — all synthesizable via Web Audio API
- **Save state**: Needed for desktop release (localStorage or JSON via Electron)
- **Target**: 500 copies is achievable with good trailer, community outreach (r/programming, r/gamedev, Hacker News, Zachtronics communities)

## For Claude Sessions
- The puzzle roadmap document has full implementation details including instruction lists, objectives, debrief text, and narrative guidelines for every puzzle.
- When adding new puzzles, follow the exact pattern of existing MISSIONS array entries.
- For puzzles testing multiple outputs, use `outputSlots` as an array.
- The simulator's memory array is accessible at test time — e.g., check `memory[0x10]` through `memory[0x17]`.
- Preserve the debrief voice — read existing debriefs carefully before writing new ones.
- The BIT instruction ($nn) is worth adding to the simulator for Puzzle 15 if not already present.
- Cycle limits should be calibrated by solving the puzzle first, then setting limit at 2–2.5x optimal.
