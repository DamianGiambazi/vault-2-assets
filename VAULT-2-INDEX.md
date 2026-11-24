# Vault 2 — Control Document Templates (The SSOT Library): Asset Index

Every file in the Vault 2 pack, in the order you'll use it. Start with the **Reference** (the main text), then follow the sections.

**Requirements:**
- the Vault 1 stack: PowerShell 7, DuckDB, the `C:\Dev\smoke-lab` venv with LanceDB, and vector-snake 2.0 (the author's tool, **available on request**);
- git;
- a GitHub account.

Nothing new is installed. The same files, with their full history, are in the public repository [github.com/DamianGiambazi/novel-ssot](https://github.com/DamianGiambazi/novel-ssot).

**Main reference:** *Vault 2 — The SSOT Library: Reference* (Claude doc; export to PDF or Word): https://claude.ai/artifact/9AtiQw8oQMFrLPV5UUjkz2

## 1. Read first

| # | File | What it is | Video |
|---|---|---|---|
| 1 | `VAULT-2-DESCRIPTIONS.md` | the two video descriptions and the lesson blurb | both |
| 2 | `VAULT-2-ERRATA.md` | corrections to what was said on camera, and to earlier copies of the assets | both |
| 3 | `docs/01-The-SSOT-and-Control-Documents.md` | the Single Source of Truth, the four control documents, and why documents drift | full: opening |

## 2. Concepts

| # | File | What it is | Video |
|---|---|---|---|
| 4 | `docs/02-Structure-and-Meaning.md` | how vector-snake cuts the book, keywords vs meaning, and why similarity isn't significance | full: cuts, search |
| 5 | `docs/03-Git-and-GitHub.md` | git as notary, GitHub as courier: the commands, the seal, receipts in and vectors out, line endings, habits | full: seal, push, clone |

## 3. The book and the spine experiment

| # | File | What it is | Video |
|---|---|---|---|
| 6 | `docs/06-The-Spine-Experiment.md` | the method step by step, the commands, the result, and the questions people ask | full |
| 7 | `data/book/corpus/ch-01.md` … `ch-24.md` | the teaching novel: 24 chapters of draft notes, one file each | full |
| 8 | `data/book/questions/q1.txt` … `q4.txt` | the four spine questions: no content word in common with the book | full |
| 9 | `data/CORPUS.md` | how the corpus was made (copied, not written), and the recipe for a rebuild, including the model snapshot | full |
| 10 | `data/PREDICTION.md` | the sealed answer key: predicted chapters, pass rules, keyword baseline, rehearsal rows | full |
| 11 | `data/.gitignore`, `data/.gitattributes` | what git doesn't track (vectors, Parquet, previews), and LF line endings | full |
| 12 | `scripts/cuts.sql`, `scripts/cuts400.sql` | DuckDB: where the trial run cut the book, at 800 and at 400 characters | full: cuts |
| 13 | `scripts/gate.sql` | DuckDB gate: one run, one model, 24 chapters, 4 questions, nothing truncated → `corpus.parquet`, `questions.parquet` | full: gate |
| 14 | `scripts/spine_search.py` | LanceDB search: top 4 chapters per question, scored against the sealed rules → `SPINE_OK` / `SPINE_PARTIAL` | full: the spine |
| 15 | `scripts/seal-check.ps1` | was the prediction committed before the run started? → `SEALED_BEFORE_RUN` | reload |

**To run it:** clone the repository (it has this layout already), or put `data/` at the repository root and `scripts/` in `spine/`. Then follow `docs/06`, §2.

## 4. Declare it and make it travel

| # | File | What it is | Video |
|---|---|---|---|
| 16 | `templates/SSOT-TEMPLATE.md` | blank Single Source of Truth | full |
| 17 | `templates/CONSTITUTION-TEMPLATE.md` | blank rules for the AI | full |
| 18 | `templates/HANDOVER-TEMPLATE.md` | blank note for the next session | reload |
| 19 | `templates/READ-ONLY-AGREEMENT.md` | blank agreement for sources that must not be rewritten | — |
| 20 | `docs/05-Field-Guide-to-the-Templates.md` | how to fill each template, with examples from software, research and client work | — |
| 21 | `novel-ssot/SSOT.md` | the novel's SSOT as committed in `131ee69`, with the machine's evidence (1 of 4) | full |
| 22 | `novel-ssot/CONSTITUTION.md` | the novel's Constitution, with the Read-Only clause | full, reload |
| 23 | `novel-ssot/HANDOVER.md` | the novel's Handover, from session 1 to session 2 | reload |
| 24 | `docs/04-GitHub-First-Push.md` | the first push on Windows: sign-in routes, troubleshooting, privacy | full: push |

## 5. The reload check

| # | File | What it is | Video |
|---|---|---|---|
| 25 | `docs/07-The-Reload-Check.md` | the method, the six checks, the results across three models, and how to run it on your own project | reload |
| 26 | `novel-ssot/INHERITANCE.md` | the six-line checklist, committed before any chat saw the documents | reload |
| 27 | `novel-ssot/CHAT-PROMPT.txt` | the exact prompt, ending with the 100% Rule question | reload |
| 28 | `lab/reload-check/answer-chatgpt.md`, `answer-claude.md`, `answer-qwen.md` | the three models' answers, verbatim (third-party output) | reload |
| 29 | `lab/reload-check/checker-gemini.md` | the checker's prompt and verdict | reload |

## 6. Prove it, reproduce it, undo it

| # | File | What it is | Video |
|---|---|---|---|
| 30 | `scripts/compare.sql` | run in a fresh clone after a rebuild: identical cuts, identical vectors → `REBUILD_MATCHES_RECEIPT` | full: clone |

**Nothing to undo.** Vault 2 installs nothing. The check at the end compares tool copies, the user PATH and vector-snake's packages against snapshots.

## 7. Beyond the vault

| # | File | What it is |
|---|---|---|
| 31 | [github.com/DamianGiambazi/novel-ssot](https://github.com/DamianGiambazi/novel-ssot) | the public repository: the full history (seal `13913f5` → lock `131ee69` → phone edit `74fd9a4` → handover `dc9e692`) and the 29 run receipts |

## Not included

- **vector-snake** itself: available on request.
- **The vectors and Parquet files:** build output. Rebuild them with vector-snake from `data/` and `CORPUS.md`'s recipe.
- **The writer-only character manifest and continuity ledger:** the Core Wound is deliberately withheld; the reload check depends on it.
- **The run receipts (29 JSON files):** in the public repository, not repeated here.
- **The cards and blur list, beat sheets, runbook and rehearsal log:** recording-only files.
- **The pre-made "amnesia" chat shown in the cold open:** an illustration made before recording, not evidence.
