# ZERO PAGE — Ship Strategy

*March 25, 2026*

---

## Where We Are

The game is 90% done. 18 puzzles playable, narrative complete, save system
works, light/dark themes, sandbox mode with Snake demo. It could go on itch.io
today as-is, but several things would make it meaningfully better before
a proper launch.

---

## What Needs To Happen Before Launch

### 1. Respond to AJ's Emails & Consider Proposals

- **AJ Erickson** reported the Puzzle 2 wording bug (fixed).
- **Austin** provided detailed UX feedback (random-fill ZP, numbered objectives,
  I/O constraints — all implemented). Offered to help with the project.
- Austin's key insight: the difficulty curve jumps too fast. Players need more
  intermediate puzzles before conceptual leaps.
- **Action:** Respond to AJ/Austin. Decide whether Austin contributes to bridge
  puzzle design, playtesting, or both. His feedback has been consistently good.

### 2. Bridge Puzzles (6 proposed)

Smooth the difficulty curve. Currently the game goes from "store a value" to
"branch logic" to "indirect addressing" with too few stepping stones.

Proposed bridges (from `puzzle-roadmap.md`):
| Bridge | Name | Concept | Placed After |
|--------|------|---------|-------------|
| 1B | Signal Boost | CLC/ADC addition | P1 (subtraction) |
| 3B | Data Relay | LDX, STX, TAX (X register intro) | P3 (before loops) |
| 6B | Bitfield Status | AND masking, single-bit extraction | P6 (before parity) |
| 7B | Subroutine Drill | Simple JSR/RTS | P7 (before clamping) |
| 9B | Indirect Load | Basic ($nn),Y with Y=0 | P9 (before dual-mode) |
| 11B | Shift Register | ASL/LSR basics | P11 (before rotation) |

**Decision needed:** Do all 6, or pick the 3-4 that address the steepest jumps?
Austin's input would help prioritize.

### 3. Hidden Puzzle Discoverability Review

Current triggers require very specific memory corruption during normal play:
- H1: Write $00 to suppression flag when count is non-zero (Mission 5)
- H2: Modify forbidden pointer table (Mission 10)
- H3: Write non-zero to unchanged address (Mission 14)

**Problem:** These are subtle enough that most players will never find them
without external hints. The pulsing amber glow on [CLASSIFIED] entries helps,
but players need to both notice something is off AND intentionally write
"wrong" values — which goes against every instinct the game trains.

**Options to consider:**
- Add more narrative hints in debrief text ("Interesting that field $30
  accepts any value..." — nudge without spoiling)
- Add a post-game hint system after completing all 15 main puzzles
  ("Anomalies were detected during your contract. Review missions 5, 10, 14.")
- Make the first hidden puzzle (H1) easier to stumble into, then let the
  reward text breadcrumb to H2 and H3
- Leave as-is for hardcore players, but add a "developer commentary" unlock
  after finishing the main campaign that mentions secrets exist
- Consider whether the base64 obfuscation is too aggressive — players who
  inspect source to find secrets are exactly the audience for this game

### 4. Sound Design (Web Audio API)

No sound at all right now. For a game about typing code into a terminal,
sound matters more than it seems — it's what makes the environment feel alive.

**Minimum viable audio:**
- Ambient low drone (changes subtly per cluster — calm → tense → unsettling)
- Keyclick on each keystroke in the editor
- RUN button: mechanical engage sound
- PASS: clean confirmation tone
- FAIL: error buzz
- BRK: harsh interrupt sound
- Hidden puzzle unlock: anomaly alert (distinct, alarming)
- System log modal: static/corruption audio

**TIS-100 reference:** TIS-100 uses minimal, effective sound — quiet ambient
hum, satisfying mechanical clicks, clean pass/fail tones. That's the target.
Not chiptune music. Not orchestral. Just presence.

### 5. UI Audit — Move Away From "Claude Dev Look"

**The problem:** The current three-column layout with syntax highlighting
and memory grids reads as "developer tool" not "game." It's functional and
clean but doesn't feel special. TIS-100 looks like a game that happens to
involve code. ZERO PAGE currently looks like a code editor that happens to
have puzzles.

**What TIS-100 does right:**
- Monochrome palette with very limited accent colors
- Thick borders and node outlines that feel like hardware
- Fixed-width everything — the whole UI feels like it's printed on a circuit
- Execution visualization (data flowing between nodes) makes the code feel
  alive
- Minimal chrome — no buttons that look like web buttons
- CRT scanline effect (subtle but present)

**Specific UI changes to consider:**

*Layout:*
- Reduce the three-column feel — consider a more integrated layout where
  panels feel like parts of a single machine, not separate browser columns
- Add subtle CRT scanline overlay in dark mode (CSS only, no canvas needed)
- Consider a fixed-size window rather than responsive — the game should feel
  like a terminal, not a web app

*Buttons:*
- Replace standard web-style buttons with keyboard-shortcut-driven commands
  (like TIS-100's F5 to run). Show hotkeys prominently, make buttons look
  like labeled keys or terminal commands, not Bootstrap buttons
- Consider a command bar at the bottom (like vim/emacs status line) instead
  of a button row

*Typography:*
- Commit fully to one monospace font throughout — no mixing
- Consider whether the font size is right. TIS-100 uses relatively small
  text that makes you lean in. That's intentional.

*Color:*
- Dark mode should feel like a single phosphor color (green or amber) with
  minimal other colors. Right now syntax highlighting adds too many colors
  for the aesthetic
- Consider reducing syntax highlighting to 2-3 colors max in dark mode
  (code, comments, errors — that's it)
- Light mode can stay more readable/accessible but should still feel like
  a government form, not a modern IDE

*Memory Grid:*
- Should look like a hardware memory map, not a spreadsheet
- Consider hex dump format (address: XX XX XX XX XX XX XX XX) instead of
  a clickable grid

*Modals/Narrative:*
- Debrief text currently appears in standard modals. Consider making these
  feel like printed documents — typewriter font, stamps, redaction marks
- System logs should feel like corrupted terminal output, not alert boxes

*Overall feel:*
- The game should feel like you're sitting at a classified terminal in a
  government basement, not using a web app
- Every pixel should reinforce the fiction

### 6. Electron Wrapper (for Steam/desktop release)

- Package as standalone desktop app (Windows/Mac/Linux)
- Replace localStorage saves with file-based JSON (Electron fs)
- Add proper window title, icon, menu bar (or hide menu bar entirely
  for immersion)
- Boot sequence / title screen that doubles as main menu

---

## What NOT To Do

- Don't redesign the puzzle mechanics. They work. The 6502 simulation is
  solid and the puzzle design is thoughtful.
- Don't add multiplayer or leaderboards before shipping. Ship first.
- Don't over-scope the sound design. Simple ambient + click + pass/fail
  is enough. You can always add more post-launch.
- Don't build a puzzle creator before launch. That's a post-launch feature
  if the game gets traction.

---

## Launch Plan

**Phase 1 — itch.io (pay-what-you-want or $3-5)**
1. Finish bridge puzzles (or at least the top 3-4)
2. Add sound design (minimum viable)
3. Do the UI audit pass
4. Review hidden puzzle discoverability
5. Full QA playthrough of all puzzles
6. Write itch.io page copy
7. Launch

**Phase 2 — Steam ($5-7)**
1. Electron wrapper
2. File-based saves
3. Boot/title screen
4. Steam store page, screenshots, trailer
5. Community outreach: r/programming, Hacker News, Zachtronics communities

---

## Marketing Notes

**Target audience:** TIS-100 fans, assembly hobbyists, puzzle game enthusiasts,
retro computing fans, Zachtronics community.

**Pitch:** "A puzzle game that teaches real 6502 assembly through increasingly
unsettling government contracts."

**Unique angles:**
- Real instruction set (not fictional) — this is actual 6502 code
- Narrative that makes you question what you're building
- Sandbox mode with a playable Snake game written in 6502 assembly
- Single HTML file, zero dependencies (for itch.io — the purity appeals
  to the target audience)

**The narrative hook is the marketing.** "Your code ran on every one of them"
is the kind of line that gets shared on Twitter and Reddit. The mechanical
puzzle game gets people in; the slow-burn moral horror is what they talk about.
