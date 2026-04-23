# Session Notes

## 2026-04-23 — public/private split; bridge puzzles moved to private repo

The 6 proposed bridge puzzles (1B, 3B, 6B, 7B, 9B, 11B) and the Phase 2
Electron/UI work have been split out to a new private repo:

- **Private repo:** https://github.com/lerugray/ZERO-PAGE-private
- **Local path:** `C:\Users\rweiss\Documents\Dev Work\ZERO-PAGE-private`

### Why the split

The private repo is the Steam-target Electron build. It carries:
- The **full 21-puzzle campaign** (15 main + 6 bridges + 3 hidden) — bridges live there only
- The **design brief** (`claude-design-brief.md`) and the generated React
  component scaffold (`claude-design-output/`), used for the Phase 2 UI
  rebuild per the TIS-100-inspired aesthetic
- A minimum-viable Electron wrapper that loads `game.html` in a sandboxed
  BrowserWindow
- `tools/apply-bridges.py` — the one-shot transform that produces the
  private `game.html` from a fresh copy of this repo's `zero-page-v20.html`

This public repo stays on the 15-puzzle campaign for itch.io / web
distribution. Do **not** cherry-pick bridge commits from private into
public.

### Untracked files in this working dir

`.claude/`, `ZeroPage.zip`, `claude-design-brief.md`, and
`claude-design-output/` are all now gitignored — they belong in the
private repo and were dropped here only as the handoff payload.

### Next up (in the private repo)

1. `npm install && npm start` — verify Electron build runs
2. Playtest each of the 6 bridge puzzles to confirm `testLogic`/`testFull`
   match a real 6502 solution; calibrate cycle limits
3. Phase 2 UI rebuild — scaffold Vite + React + Electron in `app/`, port
   simulator + missions, render with the design-ref components

See `ZERO-PAGE-private/session_notes.md` for the full handoff punch-list.

### Still open on the public side

- The public `zero-page-v20.html` has not been touched in this session.
- If Austin's bridge-puzzle feedback arrives and is materially different
  from what shipped to private, revisit.
- Pre-launch items from `STRATEGY.md` that remain in scope for the
  public/itch build: sound design (can be web-only), hidden puzzle
  discoverability review, QA playthrough of the 15-puzzle campaign.
