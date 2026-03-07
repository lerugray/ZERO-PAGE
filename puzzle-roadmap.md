# ZERO PAGE — Puzzle Roadmap

This document reflects the current puzzle campaign (18 puzzles: 15 main + 3 hidden) and proposes bridge puzzles to smooth the difficulty curve. Proposed puzzles are clearly marked.

---

## Cluster 0 — Foundation

**Tone**: Routine, instructional. Debriefs explain and reassure.

| # | Name | Concept | I/O Range | Cycle Limit | Status |
|---|---|---|---|---|---|
| 1 | FIELD BALLISTICS UNIT — PROTO A | LDA, STA, SEC, SBC | $00–$02 | 500 | Live |
| 1B | **SIGNAL BOOST — ADDITIVE CORRECTION** | CLC, ADC — addition as complement to P1's subtraction | $00–$02 | 500 | **PROPOSED** |
| 2 | IMPACT CLASSIFIER — PROTO B | CMP, BCS/BCC — conditional branching | $00–$03 | 500 | Live |
| 3 | ANOMALY SUPPRESSION FILTER — PROTO C | Multiple outputs, borrow detection, clamping | $00–$03 | 500 | Live |

### Proposed: Puzzle 1B — SIGNAL BOOST — ADDITIVE CORRECTION

> Field sensors report attenuated signal levels. A calibration offset must be added to restore nominal amplitude. Program the boost module.

- Read SIGNAL LEVEL from $00
- Read CALIBRATION OFFSET from $01
- Store ($00 + $01) & $FF into $02
- Do not modify $00 or $01
- Terminate with BRK

**Rationale**: P1 teaches SEC/SBC. This drills the mirror operation (CLC/ADC) before P2 introduces branching. Players solidify load/store/arithmetic fundamentals with a second pass before the conceptual leap to conditionals.

---

## Cluster 1 — Institutional

**Tone**: Terse, procedural. References to classification levels.

| # | Name | Concept | I/O Range | Cycle Limit | Status |
|---|---|---|---|---|---|
| 3B | **DATA RELAY — REGISTER STAGING** | LDX, STX, TAX — introduce X register before loops | $00–$05 | 500 | **PROPOSED** |
| 4 | RANGE TABLE CORRECTION UNIT | X/Y registers, indexed loops, LDA $nn,X | $00–$07 | 300 | Live |
| 5 | THRESHOLD DENSITY SCANNER | Indexed addressing scan, counting pattern | $00–$09 | 500 | Live |
| 6 | SIGNAL INTEGRITY — CHECKSUM MODULE | Multi-byte addition, sentinel detection ($FF) | $00–$07 | 600 | Live |
| 6B | **BITFIELD STATUS — MASK AND TEST** | AND masking to extract/test individual bits | $00–$05 | 400 | **PROPOSED** |
| 7 | PRIORITY ENCODER — CHANNEL EXTRACTION | AND/LSR for nibble extraction, XOR parity | $00–$0A | 400 | Live |

### Proposed: Puzzle 3B — DATA RELAY — REGISTER STAGING

> Incoming field data requires redistribution across staging registers. Transfer each input value to its corresponding output slot. Use the index register — direct copy is not authorized for this pipeline.

- Read 3 values from $00–$02
- Copy each to $03–$05 using X as an index
- Terminate with BRK

**Rationale**: Cluster 1 opens with indexed loops (P4), but players have never used X/Y registers. This bridge introduces LDX, INX, and indexed addressing (`LDA $00,X` / `STA $03,X`) in a trivial context — just copy data — so the loop mechanics in P4 aren't fighting the register concept at the same time.

### Proposed: Puzzle 6B — BITFIELD STATUS — MASK AND TEST

> A system status byte at $00 encodes four independent flags. Extract each flag as a 0/1 value into separate output bytes for the monitoring subsystem.

- Read status byte from $00
- Store bit 0 (masked) in $01
- Store bit 1 (masked and shifted) in $02
- Store bit 2 in $03
- Store bit 3 in $04
- Terminate with BRK

**Rationale**: P7 combines nibble extraction, shifting, AND masking, and XOR parity all at once. This bridge isolates AND masking and LSR shifting in a simpler "extract individual bits" task, so P7's parity computation isn't the first time players see bitwise operations.

---

## Cluster 2 — Complicit

**Tone**: Debriefs stop explaining. Notes appended to files the player isn't sent.

| # | Name | Concept | I/O Range | Cycle Limit | Status |
|---|---|---|---|---|---|
| 7B | **DUAL CORRECTION — SUBROUTINE DRILL** | JSR/RTS — simplest possible subroutine use | $00–$05 | 600 | **PROPOSED** |
| 8 | CLAMP SUBROUTINE — REFACTOR | JSR/RTS — reusable subroutines | $00–$0F | 700 | Live |
| 9 | SEQUENCE REVERSAL — STACK METHOD | PHA/PLA — stack as data structure | $00–$0F | 400 | Live |
| 9B | **INDIRECT LOAD — POINTER BASICS** | ($nn),Y with Y=0 — simplest indirect access | $00–$0F | 400 | **PROPOSED** |
| 10 | DUAL POINTER ACCESS — INDIRECT MODES | ($nn,X) and ($nn),Y — indirect addressing | $40–$5F, $E0–$E3 | 500 | Live |
| 11 | FILTERED COPY WITH MASK | Indirect indexed + AND masking + counters | $60–$7F, $E0–$E3 | 800 | Live |

### Proposed: Puzzle 7B — DUAL CORRECTION — SUBROUTINE DRILL

> Two independent data channels require the same correction applied. Implement the correction as a subroutine and call it twice. Inline duplication is not authorized.

- Read value A from $00, correction from $02
- Read value B from $01
- Write corrected A ($00 + $02) to $03
- Write corrected B ($01 + $02) to $04
- Must use JSR/RTS — the correction logic must be a subroutine called twice
- Terminate with BRK

**Rationale**: P8 asks players to write a clamping subroutine that takes parameters and makes decisions. This bridge introduces JSR/RTS in the simplest possible form — a subroutine that just adds a constant — so players learn the call/return mechanic before combining it with conditional logic.

### Proposed: Puzzle 9B — INDIRECT LOAD — POINTER BASICS

> A data address has been stored at $0E (low byte). Load the value at that address and store a copy in $0F. The pointer system must work for any target address in the zero page.

- Read pointer low byte from $0E (high byte is always $00)
- Use indirect indexed addressing to load the value at the pointed-to address
- Store the loaded value in $0F
- Terminate with BRK

**Rationale**: P10 uses both ($nn,X) and ($nn),Y modes simultaneously with pointer tables. This bridge introduces just ($nn),Y with Y=0 — the simplest indirect access — so players understand "memory contains an address" before dealing with dual pointer modes.

---

## Cluster 3 — Terminal

**Tone**: Minimal, cryptic. Final debrief is one line.

| # | Name | Concept | I/O Range | Cycle Limit | Status |
|---|---|---|---|---|---|
| 11B | **SHIFT REGISTER — FIELD EXTRACTION** | ASL/LSR — simple bit shifts before rotation | $00–$03 | 400 | **PROPOSED** |
| 12 | SERIAL BIT PACKER | ROL/ROR — bit rotation through carry | $00–$0F | 600 | Live |
| 13 | THRESHOLD MATRIX SCAN | CPX/CPY — 2D iteration, bit flag assembly | $00–$3F | 1000 | Live |
| 14 | IN-PLACE NORMALISATION PASS | INC/DEC — in-place memory modification | $00–$0F | 800 | Live |
| 15 | DEAD RECKONING — POSITION INTEGRATOR | Full pipeline: bit decoding, multi-axis math, clamping | $00–$3F | 1200 | Live |

### Proposed: Puzzle 11B — SHIFT REGISTER — FIELD EXTRACTION

> A packed data word at $00 contains two 4-bit fields. Extract the upper field by shifting right 4 times. Extract the lower field by masking. Store both.

- Read packed byte from $00
- Shift right 4 times (LSR x4) to extract upper nibble → store in $01
- Mask lower nibble (AND #$0F) → store in $02
- Terminate with BRK

**Rationale**: P12 introduces ROL/ROR which rotate through carry — a subtle distinction from ASL/LSR. This bridge gives players practice with simple shifts so they understand the base concept before carry rotation adds complexity. (Note: P7 used LSR for nibble extraction, but that was several puzzles ago and combined with parity. This refreshes the mechanic.)

---

## Hidden Puzzles

Accessible through alternate in-game actions. Not part of the main progression.

| ID | Name | Cycle Limit | Status |
|---|---|---|---|
| H1 | NULL RETURN PROTOCOL | 500 | Live |
| H2 | VECTOR OVERWRITE — DEEP ACCESS | 700 | Live |
| H3 | FINAL INTEGRATION — TERMINATION PASS | 900 | Live |

---

## Summary

| Category | Count |
|---|---|
| Current main campaign | 15 |
| Current hidden | 3 |
| **Proposed bridges** | **6** |
| **Total if all bridges added** | **24** |

### Bridge puzzle priority

1. **3B (Data Relay)** and **1B (Signal Boost)** — highest impact, address the steepest early jumps
2. **7B (Subroutine Drill)** and **6B (Bitfield Status)** — smooth the mid-game conceptual leaps
3. **9B (Indirect Load)** and **11B (Shift Register)** — ease into the hardest mechanics
