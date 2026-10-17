# CwikCard

Flashcards with spaced repetition, from the same question CSV that [QuizQrusher](https://braxtonstryker.github.io/Quiz_Qrusher/) uses. Each question becomes a card (the question on the front, its correct answer on the back), and Anki-style scheduling brings each card back just before you'd forget it.

**Use it:** https://braxtonstryker.github.io/CwikCard/

- **Scheduling:** FSRS (the scheduler Anki uses today), SM-2 (classic Anki) or Leitner boxes, chosen per deck, with learning and relearning steps, daily limits for new cards and reviews, fuzz, and leeches.
- **Studying:** front to back, back to front, or both; Again / Hard / Good / Easy with the next review time on each button; undo, flag, bury, suspend and edit; keyboard shortcuts and swipes.
- **Practice rounds** that leave the schedule alone, shuffled by QuizQrusher's shuffle engine, with filters for flagged cards, hard cards, categories and more.
- **Browse** and **Stats**, backups (per deck or everything), and import of QuizQrusher backups.
- Installable, works offline, and keeps everything in your browser. Nothing is uploaded.

## Question CSV format

```
Category,Question_Text,Option_1,Option_2,Option_3,Option_4,Option_5,Option_6,Correct_Option_Number
```

`Correct_Option_Number` is 1 to 6. Header matching is lenient (case, spaces and annotations like "(1-6)" are ignored). QuizQrusher's Help has AI prompts that write these files from your lecture slides.

## Repository

- `index.html`: the whole app, one file with inline CSS and JS, no build step and nothing loaded from other sites.
- `sw.js`, `manifest.webmanifest`, `icons/`: the installable offline app. After changing any of them, run `python -I tools/sw_version.py --sync` so browsers pick up the new version.
- See [CLAUDE.md](CLAUDE.md) for development notes.
