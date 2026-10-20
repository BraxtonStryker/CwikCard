# CwikCard

Flashcards with spaced repetition, from the same question CSV that [QuizQrusher](https://braxtonstryker.github.io/Quiz_Qrusher/) uses, or from a term list. Each question becomes a card (the question on the front, its correct answer on the back), and Anki-style scheduling brings each card back just before you'd forget it.

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

## Term list format

```
Term,Definition,Category,Source
Mitochondrion,The organelle that makes most of the cell's ATP,Biology,Lecture 2
Osmosis,Water moving toward the side with more solute,Physiology,4
```

The term goes on the front, the definition on the back, the category becomes the card's tag, and the source (a slide number or deck name) is shown under the answer; a bare number reads as "Slide 4". The header row is optional. Three columns (`Term,Definition,Source`) also work, with the source as the tag.

## Repository

- `index.html`: the whole app, one file with inline CSS and JS, no build step and nothing loaded from other sites.
- `sw.js`, `manifest.webmanifest`, `icons/`: the installable offline app. After changing any of them, run `python -I tools/sw_version.py --sync` so browsers pick up the new version.
- See [CLAUDE.md](CLAUDE.md) for development notes.
