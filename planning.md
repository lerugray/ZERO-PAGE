# ZERO PAGE — Planning Notes

## 1. Light Mode Accessibility Overhaul

### Context
User feedback from someone with astigmatism: text is hard to read in light mode, needs higher contrast. ~8% of people have color vision issues.

### The Problem
Current light mode uses green-tinted text on warm beige. Several color combinations fail WCAG AA (4.5:1 minimum contrast ratio):
- `--text-dim` (#4a5a42) ~3.8:1 — FAILS
- `--hl-comment` (#4a5a42) ~3.8:1 — FAILS
- `--green-dim` (#6a8a62) ~2.7:1 — FAILS
- `--hl-zp` (#2a5a8a) ~4.3:1 — borderline
- `--text` (#2a4a2a) ~5.5:1 — borderline
- `--hl-mnem` (#1a6a1a) ~5.2:1 — borderline

### Plan — Two-tier approach

**Editor area — full black on white for maximum accessibility:**
- `--text` -> near-black (#111111 or #1a1a1a)
- `--text-bright` -> black (#000000)
- `--text-dim` -> dark grey (#3a3a3a)
- `--bg` for editor -> pure/near white (#ffffff or #fafafa)
- Syntax highlighting colors (`--hl-mnem`, `--hl-imm`, `--hl-zp`, `--hl-comment`, `--hl-label`) all deepened to pass 7:1 AAA against white
- Drop the green tint from editor text entirely

**Chrome/UI (memory map, CPU state, panels) — WCAG AA with warmth:**
- Can keep slight warm tint on backgrounds (#f5f2eb or similar)
- All text must still pass 4.5:1 AA minimum
- Borders, status colors (pass/fail/amber), and panel text reviewed for compliance

**Scope:**
- ~20-25 CSS variable values inside `body.light-theme` block (lines 51-88)
- May need targeted overrides for editor-specific elements if editor bg differs from panel bg
- Dark mode (:root defaults) unchanged
- Dark-themed modal overrides (reward/syslog/anomaly) unchanged

**Validation:**
- Use Firefox/Chrome built-in accessibility contrast checker
- Reference: https://www.washington.edu/accessibility/2025/11/06/color-contrast/
- Target: AAA (7:1) for editor text, AA (4.5:1) minimum for all other text

---

## 2. Difficulty Curve — Bridge Puzzles

### Context
Player feedback (Austin): the game needs more small problems so players build working knowledge before hitting harder puzzles. He also offered to help with the project.

### Plan
Add 1-2 bridge puzzles per cluster to smooth the difficulty curve. This would take the main campaign from 15 to roughly 19-23 puzzles. Each bridge puzzle must:
- Fit the cluster's narrative tone
- Reinforce recently introduced concepts before the next leap
- Follow the "one new concept per puzzle" rule (or drill a prior concept deeper)
- Provide additional opportunities for hidden content hints

### Where bridges are most needed (to be refined)
- **Cluster 0 (Foundation)**: Players jump from basic LDA/STA/SBC to multi-output + clamping by puzzle 3. A bridge between 1-2 or 2-3 could help.
- **Cluster 1 (Institutional)**: Index registers + loops are a big conceptual leap from branching. A gentle "use X as a counter" bridge before the full loop puzzles.
- **Cluster 2 (Complicit)**: Indirect addressing is notoriously confusing. A bridge that uses JSR/RTS in a simpler context before combining it with indirect modes.
- **Cluster 3 (Terminal)**: ROL/ROR and 2D iteration are dense. A simpler rotation drill before the full bit-packing puzzle.

### Status
Waiting for Austin's more detailed input. Will design specific puzzles once scope is agreed.

---

## 3. Quick UI/UX Improvements (from Austin's feedback)

### Random-fill zero page before test runs
- Currently unused memory is $00, making it hard to tell if code wrote a zero or didn't execute
- Fill all of zero page with random bytes before seeding puzzle inputs
- Small change, high value for debugging visibility

### Change objective bullet prefix
- Currently uses ">" which conflicts with comparison notation (A > $80)
- Switch to numbered steps: 1, 2, 3...
- Simple UI tweak in mission panel rendering

### Constrain early puzzle I/O to $00-$0F
- Austin's point: keep everything visible in the top 16 bytes so players can watch execution
- Early puzzles (Clusters 0-1) should stay within $00-$0F where possible
- Later puzzles (Clusters 2-3) legitimately need more address space — that's fine, document it
- Audit existing puzzles to see which can be tightened

---

## Priority Order
1. Light mode accessibility (user is actively blocked by this)
2. Quick UI/UX improvements (small changes, big impact)
3. Bridge puzzles (larger effort, design work needed first)
