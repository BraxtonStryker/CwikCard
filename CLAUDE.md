# CwikCard

Flashcards with spaced repetition from a QuizQrusher question CSV. Part of the study apps at https://braxtonstryker.github.io/ (with QuizQrusher in `../Quiz_Qrusher/` and InfoBase in `../Class_Base/`). Hosted on GitHub Pages at `/CwikCard/` (repo `BraxtonStryker/CwikCard`). **v1.0.1** (2026-10-17: Beta tag next to the name, as in InfoBase). v1.0.0 built 2026-10-16.

## Layout
- `index.html`: the whole app. `sw.js`, `manifest.webmanifest`, `icons/` (`icon.svg` and `icon-maskable.svg` are the sources of the PNGs): the installable offline app.
- `tools/sw_version.py`: `python -I tools/sw_version.py --sync` after changing any precached file, or the service worker won't ship the change (same scheme as QuizQrusher's `check_shared.py`).

## Conventions (match QuizQrusher)
- Single-file HTML, inline CSS and JS, vanilla JS in an IIFE, `var` and `function` style, no frameworks, no build step, nothing from CDNs (the Atkinson Hyperlegible font is inlined from QuizQrusher).
- Every color is a CSS custom property on `:root`, with dark values for "Match system" and `data-theme="dark"`. Font sizes are `calc(Npx * var(--fs))`; card text also uses `--cs` (Settings > Card text size, and + / - while studying).
- American spelling. Every localStorage, IndexedDB and cache name starts with `cwikCard` (`cwikCard:settings`, `cwikCard:app`, `cwikCard-<version>`): all of the user's GitHub Pages apps share one origin.
- Copied from QuizQrusher and kept in step by hand: the appearance block in `<head>`, the CSV reader and shuffle engine (marked "Copied from QuizQrusher"), the storage backends, Settings rows, the app switcher, and the service worker. If QuizQrusher's CSV contract or shuffle changes, change it here too.
- The app switcher (`setupAppSwitch`) is the same menu as in QuizQrusher and InfoBase; keep the three in step. Hidden from `file://`.

## How it works
- **Data:** IndexedDB `cwikCard:app` with stores `decks` (name, `csvText`, `csvHash`, `edits`, `opts`) and `progress` (`cards`: card id -> state, `log`: one entry per answer `{t, c, r, k, i, d}`, `extra`: today's extra new cards). Falls back to localStorage, then memory, like QuizQrusher. Change the schema only with a new `DB_VERSION` and an upgrade step.
- **Notes and cards:** a note is a CSV row; its id is QuizQrusher's question hash in base 36 (duplicates get `-2`, `-3`). Cards are `<noteId>:f` (question first) and `<noteId>:r` (answer first). A card with no state is new. Edits are stored per note in `deck.edits` and laid over the CSV, so the id (and the progress) survives an edit. Update from CSV keeps progress for notes whose category, question text and correct option number are unchanged (the hash's inputs).
- **Scheduling:** `answerCard(state, rating, now, opts, cardId)` is pure, so the buttons preview every choice. FSRS-5 with its default parameters (`FSRS_W`), SM-2 (Anki's ease rules), or Leitner boxes; learning and relearning steps for FSRS and SM-2; Anki's fuzz, seeded by card and rep count so the preview matches; leeches. Days start at Settings > "Next day starts at" (`dayIndex`, `dayStartMs`).
- **Sessions:** like Anki, due learning cards first, then reviews and new cards with the new ones spread evenly, then learning cards within the 20-minute learn-ahead; daily limits per deck; a note's two sides aren't shown in one session. Practice rounds (`startPracticeIds`) never touch the schedule. Undo covers answers, bury and suspend.
- **Backups:** `cwikcard-deck` and `cwikcard-backup` JSON (newer copy wins on import); QuizQrusher's `quiz-qrusher-save` and `quiz-qrusher-backup` files import their question banks as decks.
