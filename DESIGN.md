# ZERO PAGE — Design Document

## Concept

ZERO PAGE is a 6502 assembly language puzzle game played in the browser. The player takes on the role of an unnamed contractor writing low-level microprocessor code for a government program. Puzzles teach real 6502 instructions progressively while an understated narrative builds moral unease through bureaucratic debrief text that grows increasingly terse and disturbing.

Think TIS-100 meets moral complicity. The mechanics are the hook; the narrative is the point.

## Technical Architecture

- **Active working file**: `zero-page-v18.html` is the current working version of the game. Other HTML files in the repo (index.html, earlier versions) are outdated snapshots — do NOT edit them.
- **Single-file**: The entire game — HTML, CSS, JavaScript, assembler, simulator, puzzle definitions, narrative, UI — lives in one HTML file (~172 KB, ~4,000 lines).
- **Zero dependencies**: No build step, no frameworks, no server. Open in any modern browser.
- **Custom 6502 assembler**: Two-pass assembler with label resolution, supporting 45 instructions and 8 addressing modes.
- **Custom 6502 simulator**: Cycle-accurate execution with full CPU state (A, X, Y, SP, PC, flags), 256-byte zero-page memory, 256-byte stack ($100–$1FF), and memory-mapped I/O.
- **Syntax highlighting**: Real-time in-editor highlighting for mnemonics, immediates, zero-page addresses, labels, comments, and errors.

### Supported 6502 Instructions (45 total)

| Category | Instructions |
|---|---|
| Load/Store | LDA, STA, LDX, STX, LDY, STY |
| Arithmetic | ADC, SBC |
| Logic | AND, ORA, EOR |
| Shifts/Rotates | ASL, LSR, ROL, ROR |
| Compare | CMP, CPX, CPY |
| Branch | BCS, BCC, BEQ, BNE, BMI, BPL |
| Jump | JMP, JSR, RTS |
| Stack | PHA, PLA |
| Transfer | TAX, TAY, TXA, TYA, TXS, TSX |
| Inc/Dec | INX, INY, DEX, DEY, INC, DEC |
| Flags | SEC, CLC, SEI, CLI, SEV, CLV |
| Other | BIT, NOP, BRK |

### Addressing Modes

- Implied (e.g. `SEC`, `PHA`)
- Immediate (`#$nn`)
- Zero page (`$nn`)
- Zero page indexed (`$nn,X` / `$nn,Y`)
- Indexed indirect (`($nn,X)`)
- Indirect indexed (`($nn),Y`)
- Indirect (`($nn)`)
- Absolute (`$nnnn`)

---

## Game Modes

### Mission Mode (Default)

Linear progression through 15 main puzzles. Each puzzle:
- Presents a narrative briefing and technical objectives
- Seeds random inputs into memory
- Player writes assembly to produce correct outputs
- 10 randomized tests validate the solution
- Debrief modal shows narrative text after passing
- Best cycle count is tracked per puzzle

### Sandbox Mode

Freeform 6502 programming environment with no grading or narrative framing. Features:

- **Direct memory editing**: Click any cell in the memory grid to edit hex values
- **Live execution**: Continuous execution via `requestAnimationFrame` (50 instructions/frame)
- **Framebuffer display**: 16×8 pixel grid mapped to memory $80–$FF
  - Value `$00` = black (off)
  - Value `$7F` = amber (used for food in Snake demo)
  - All other values = green
  - Rendered on a `<canvas>` element at 20x scale (320×160 pixels)
- **Memory-mapped I/O**:
  - `$FF` — Keyboard input register. Returns last keypress (arrows: Up=$01, Down=$02, Left=$03, Right=$04; printable keys return ASCII). Auto-clears to $00 after read.
  - `$FE` — Random number generator. Each read returns a fresh pseudo-random byte (0–255). Value is never stored in memory.
- **Snake demo**: Pre-loaded program (`LOAD SNAKE DEMO` button) implementing a full Snake game with arrow-key controls, food spawning via RNG, score tracking, and framebuffer rendering. Demonstrates the full capability of the sandbox environment.

### Memory Map

| Range | Purpose |
|---|---|
| `$00–$01` | Primary inputs (r0, r1) |
| `$02–$03` | Common output slots (early puzzles) |
| `$10–$17` | Common output region |
| `$18` | Suppression counter (Puzzle 11) |
| `$20–$2F` | Thresholds, control values, late outputs |
| `$30–$3F` | Counters, flags, bitfields |
| `$40–$5F` | Data block A (Puzzle 10) |
| `$50–$5F` | Data block B (Puzzle 10) |
| `$60–$7F` | Data blocks (Puzzle 11) |
| `$70–$7D` | Data blocks (Hidden H2) |
| `$80–$FF` | Framebuffer (sandbox) / record blocks (Hidden H3) |
| `$C0–$CF` | Record block (Hidden H3) |
| `$E0–$E3` | Pointer tables (Puzzles 10–11) |
| `$E8–$E9` | Hidden vector table (Hidden H1) |
| `$F0–$F3` | Pointers (Hidden H2) |
| `$F8–$F9` | Pointers (Hidden H3) |
| `$FE` | RNG register (sandbox only) |
| `$FF` | Keyboard input (sandbox only) |
| `$100–$1FF` | Stack (implicit, managed by SP) |

---

## Puzzle Campaign

### Cluster 0 — Foundation (Puzzles 1–3)

**Tone**: Routine, instructional. Debriefs explain what happened and reassure.

| # | Name | Concept Introduced | Cycle Limit |
|---|---|---|---|
| 1 | FIELD BALLISTICS UNIT — PROTO A | LDA, STA, SEC, SBC — basic subtraction | 500 |
| 2 | IMPACT CLASSIFIER — PROTO B | CMP, BCS/BCC — conditional branching | 500 |
| 3 | ANOMALY SUPPRESSION FILTER — PROTO C | Multiple outputs, borrow detection, clamping | 500 |

### Cluster 1 — Institutional (Puzzles 4–7)

**Tone**: Terse, procedural. Debriefs note what was filed, hint at classification levels.

| # | Name | Concept Introduced | Cycle Limit |
|---|---|---|---|
| 4 | RANGE TABLE CORRECTION UNIT | X/Y registers, indexed loops, LDA $nn,X | 300 |
| 5 | THRESHOLD DENSITY SCANNER | Indexed addressing scan, counting pattern | 500 |
| 6 | SIGNAL INTEGRITY — CHECKSUM MODULE | Multi-byte addition, sentinel detection ($FF) | 600 |
| 7 | PRIORITY ENCODER — CHANNEL EXTRACTION | AND/LSR for nibble extraction, XOR parity | 400 |

### Cluster 2 — Complicit (Puzzles 8–11)

**Tone**: Neutral becoming sinister. Debriefs stop explaining. Notes are appended to files the player isn't sent.

| # | Name | Concept Introduced | Cycle Limit |
|---|---|---|---|
| 8 | CLAMP SUBROUTINE — REFACTOR | JSR/RTS — reusable subroutines | 700 |
| 9 | SEQUENCE REVERSAL — STACK METHOD | PHA/PLA — stack as data structure | 400 |
| 10 | DUAL POINTER ACCESS — INDIRECT MODES | ($nn,X) and ($nn),Y — indirect addressing | 500 |
| 11 | FILTERED COPY WITH MASK | Indirect indexed + AND masking + counters | 800 |

### Cluster 3 — Terminal (Puzzles 12–15)

**Tone**: Minimal, cryptic. Final debrief is one line + terminal closure.

| # | Name | Concept Introduced | Cycle Limit |
|---|---|---|---|
| 12 | SERIAL BIT PACKER | ROL/ROR — bit rotation through carry | 600 |
| 13 | THRESHOLD MATRIX SCAN | CPX/CPY — 2D iteration, bit flag assembly | 1000 |
| 14 | IN-PLACE NORMALISATION PASS | INC/DEC — in-place memory modification | 800 |
| 15 | DEAD RECKONING — POSITION INTEGRATOR | Full pipeline: bit decoding, multi-axis math, clamping | 1200 |

### Hidden Puzzles (3 — accessible through alternate means)

Three additional classified puzzles exist outside the main campaign. They are unlocked by specific in-game actions that attentive players can discover through narrative clues embedded in system logs, puzzle briefings, and reward text. When unlocked, an "ANOMALY DETECTED" banner flashes and the puzzle appears in the Mission Select screen under "CLASSIFIED MODULES."

| ID | Name | Cycle Limit |
|---|---|---|
| H1 | NULL RETURN PROTOCOL | 500 |
| H2 | VECTOR OVERWRITE — DEEP ACCESS | 700 |
| H3 | FINAL INTEGRATION — TERMINATION PASS | 900 |

Each hidden puzzle, upon completion, reveals a classified reward document that ties earlier puzzle mechanics to the narrative's true meaning — connecting "suppression filters," "density scanners," and "normalisation passes" to their actual purpose. Reward text cross-references the other hidden puzzles, forming a breadcrumb trail for players who find one secret to discover the others.

---

## Narrative Design

### Voice and Escalation

The narrative operates through bureaucratic debrief text that shifts in tone across the four clusters:

1. **Foundation (1–3)**: Verbose, procedural, reassuring. "No anomalies flagged at this time."
2. **Institutional (4–7)**: Terse. References to classification levels. "Its origin is not within the scope of this assignment."
3. **Complicit (8–11)**: Things stop being explained. Notes appended to reports the player isn't sent. "The counter has been appended to a report you have not been sent."
4. **Terminal (12–15)**: Minimal. Single-line assessments. Final debrief: "ZERO PAGE OPERATIONS CONCLUDED. TERMINAL CLOSING."

### System Logs

After certain puzzles, the debrief is replaced by a corrupted "system log" showing intercepted data — routing tables, hex dumps, geographic coordinates, and batch processing records. These logs form a breadcrumb trail that:

- References consistent batch IDs and geographic coordinates across entries
- Shows routing to "suppression queues" and "category-4 processing"
- Contains embedded narrative clues that reward careful reading
- Escalates from routine dispatch logs to records mentioning processed population clusters

### Hidden Puzzle Rewards

Each hidden puzzle completion reveals a classified document that retroactively recontextualizes earlier puzzles:

- **H1 reward**: Explains that a null density count caused zero resource allocation — and a population cluster ceased to exist
- **H2 reward**: Reveals the pointer table routes records to a "suppression queue" that processes and emits $00 confirmation receipts
- **H3 reward**: The full reveal — names every earlier puzzle and its actual function in the pipeline, ending with "Your code ran on every one of them"

### Design Rules

- Never explicitly name the end-use. Let the player's imagination fill the gap.
- Puzzle names use clinical/technical language that is retroactively sinister.
- Debrief text never asks the player to reflect — it simply states facts and moves on.
- The sandbox has no narrative framing. It is the player's own space.

---

## UI Structure

Three-column layout:

| Left (280px) | Center (flex) | Right (260px) |
|---|---|---|
| Mission panel (name, narrative, objectives) | Assembly editor with syntax highlighting | CPU state display (A, X, Y, SP, PC, flags) |
| Tech reference panel | Framebuffer canvas (sandbox only) | Memory cell grid |
| | Status bar | LCD output panel |

### Controls

- **RUN**: Execute until BRK or cycle limit
- **STEP**: Execute one instruction with UI update
- **TESTS**: Run 10 randomized validation tests (mission mode)
- **NEXT MISSION**: Advance to next puzzle (requires passing)
- **SANDBOX**: Toggle sandbox mode
- **LOAD SNAKE DEMO**: Load Snake game into editor (sandbox only)
- **MANUAL**: Open field manual documentation
- **MISSION SELECT**: View/replay cleared puzzles and access hidden puzzles

### Modals

- **Debrief**: Post-pass narrative assessment
- **System Log**: Corrupted intercepted data (replaces debrief on specific puzzles)
- **Reward**: Classified document revealed after completing hidden puzzles
- **Completion**: Shown once after clearing all 15 main puzzles
- **Mission Select**: Grid of all puzzles with clear status, best scores, and replay buttons
- **Manual**: Built-in 6502 field manual (opens in new window)

---

## Known Issues and Improvement Areas

### 1. Hidden Puzzle Discoverability (Partially Addressed)

Multiple layers of in-game hints now guide attentive players toward the hidden content:

- **Narrative hints**: Certain puzzle briefings contain subtle lines that hint at what the system does and doesn't monitor
- **System log clues**: Corrupted data logs shown after specific puzzles contain thematic references that reward careful reading
- **Glitch feedback**: Replaying a cleared puzzle and failing shows glitched status messages (e.g., "DEVIATION LOGGED") on relevant puzzles, hinting that the system notices non-standard behavior
- **Mission Select pulse**: Locked "[CLASSIFIED]" entries subtly pulse with an amber glow once the player has cleared the associated puzzle, signaling proximity to a secret
- **Reward cross-references**: Each hidden puzzle's reward text explicitly references the other hidden puzzles by module number, so discovering one leads to the others

**Remaining questions**:
- Is the current hint density sufficient for organic discovery, or do players still need community/external guidance?
- Should the hint system be further tuned based on player feedback after release?

### 3. Instruction Gating in Tech Reference

The tech reference panel currently shows all instructions. Ideally, it should hide or grey out instructions that haven't been introduced yet by the current puzzle, reinforcing the progressive learning curve.

### 4. Save State

No persistence currently exists. All progress (cleared missions, best scores, hidden puzzle unlocks) is lost on page refresh. This is acceptable for the current web prototype but must be resolved before any desktop release.

---

## Distribution Plans

### Phase 1: Itch.io Release

- **Format**: Single HTML file, pay-what-you-want or free
- **Goal**: Gather player feedback, validate interest, build an audience
- **Requirements**: Game is release-ready in current state

### Phase 2: Steam Release

- **Price**: $5–7 range
- **Wrapper**: Electron for desktop packaging
- **Additional requirements before Steam**:
  - Save state (localStorage or JSON via Electron file system)
  - Sound design (Web Audio API): ambient drone, keyclick sounds, tones for RUN/PASS/FAIL/BRK events
  - Possibly expanded puzzle count or post-game content
- **Target**: 500 copies is achievable with a good trailer and community outreach

### Marketing Channels

- r/programming, r/gamedev, r/indiegaming
- Hacker News
- Zachtronics community forums and Discord servers
- Assembly/retro computing communities
- YouTube/streamer outreach (puzzle game content creators)

---

## Future Work

- [x] ~~Fix debugger step highlighting to show line about to execute~~ — `doStep()` now highlights the upcoming line
- [x] ~~Evaluate hidden puzzle discoverability~~ — narrative hints, glitch messages, Mission Select pulse, and reward cross-references added
- [x] ~~Create puzzle cheat sheet for testing~~ — `CHEATSHEET.md` created (gitignored, local only)
- [x] ~~Fix nested scroll in left panel~~ — unified scroll in left column, no more scroll traps
- [ ] Implement instruction gating in tech reference panel
- [ ] Add save state persistence (localStorage)
- [ ] Sound design via Web Audio API
- [ ] Electron wrapper for Steam packaging
- [ ] Trailer production
- [ ] Update `zero-page-puzzle-roadmap.docx` to reflect all 18 puzzles
