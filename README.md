# Vault 2 — The SSOT Library: Reference

Oct 30, 2026 · @Damian

## What Vault 2 is

Vault 2 turns a novel's draft notes into a book with written rules that any device, session or AI can pick up, and proves each step on the same 8 GB laptop as Vault 1. The spine is searched by meaning with the Vault 1 stack, declared by the author in a Single Source of Truth, sealed in git, carried by GitHub and rebuilt from a fresh copy.

> **Draft status (30 Oct 2026):** the results below come from the rehearsal on SPOOKY. They are checked against the recorded take before this Reference is published.

The method carries beyond novels: write down what must not change, seal it before you test it, let any machine or person check against it, and rebuild it anywhere from the recipe.

- **Laptop** — Lenovo V15-IGL ("SPOOKY"): Intel Celeron N4020, 2 cores, 8 GB RAM, no AVX, Windows 11 Pro 25H2
- **Stack (from Vault 1)** — DuckDB 1.5.6 · LanceDB 0.40.0 · vector-snake 2.0.0 with all-MiniLM-L6-v2 (384 numbers per passage; Hugging Face snapshot `1110a243...`) · PowerShell 7.6.9 · git 2.50.1 (at recording)
- **New this vault** — nothing installed; GitHub repository `novel-ssot` under the account `DamianGiambazi`
- **Corpus** — the teaching novel: 24 chapters of draft notes (one file each) and 4 sealed questions
- **Videos** — *Reproducible and Portable Truth: Finding the Spine, Then Making It Travel* (full lesson) · *Vault 2 summary* (planned)

## The Single Source of Truth

An AI keeps nothing between chats, so a project's decisions must live in documents, not in a conversation. "The AI doesn't have amnesia. We do." The fix is a **Single Source of Truth (SSOT)**: one written, agreed version of the facts that everyone, person or machine, works from.

Vault 2 uses four control documents. Each does one job:

| Document | Governs | Holds | In the novel |
| --- | --- | --- | --- |
| SSOT | the project | scope, Locked Decisions, Frozen Constants, the spine, pacing, the Leviathan Lock, a changelog, and the machine's evidence labelled as evidence | `SSOT.md`: Locked Decisions D1–D5; the Lie "I chose this life"; the Figure only in chapters 1, 7, 19 and 24 |
| Constitution | the AI | persona, supersession, load order, "do not relitigate", "do not invent", confidence tags, "when uncertain, ask" | `CONSTITUTION.md`: "Never rewrite my prose unless I explicitly ask" |
| Handover | the next session | where we are, load order, decisions not to reopen, the next action | `HANDOVER.md`: next action, Chapter 2's structural beats |
| Read-Only Agreement | sacred source material | analyse, cite, question; never rewrite; flag ambiguity as `[AMBIGUOUS]` | a clause in the Constitution protecting Chapter 1 |

Three terms do the work:

- **Locked Decision:** settled, with its reason. Not reopened without a new decision.
- **Frozen Constant:** a fact every chapter must agree with.
- **Leviathan Lock:** what the AI must never do (here: rewrite Chapter 1, alter a Frozen Constant, move a pillar, give the Figure dialogue).

The machine proposes; the author declares. The SSOT records what the author decides the book means. Machine evidence may sit inside it, in its own section, under the rule "evidence informs; it does not decide".

For a technical reader, a novel is a codebase whose files must agree:

| In the novel | In software |
| --- | --- |
| chapters | source modules |
| character manifest | interface definitions |
| Locked Decisions | architecture decision records |
| Frozen Constants | constants |
| continuity ledger | referential integrity |
| Constitution | linter and CI rules |
| Handover | the README for the next developer |
| vectors | build output, rebuildable from source |

## Structure and meaning

We cut by structure and search by meaning. vector-snake decides where passages end; the embedding model decides what each passage is about.

**How the cuts are chosen.** vector-snake does not cut at a fixed length. It splits at paragraphs first, then lines, sentences and words, and only then characters, and packs the pieces up to a budget. The name for this is **structure-aware (recursive) chunking**.

| Budget | Files | Passages | Files cut | Where the cuts fall |
| --- | --- | --- | --- | --- |
| 800 characters | 28 | 33 | 5 (chapters 3–7) | every cut on a paragraph break |
| 400 characters | 28 | 49 | 10 | some cuts now fall on a sentence inside a paragraph |

**Why 800, not Vault 1's 1,000:** the model reads at most 256 tokens. These notes are denser than the newsletter, so each passage gets less room. The run's receipt confirms it: `over_budget_count = 0` in all 28 files.

**The run-up:** each passage after the first repeats up to 120 characters from the one before, starting on a word boundary, or at a sentence start when one fits. Chapter 5's run-up is only `I chose this."` (16 characters, mid-quote). It is a chosen default, not a law.

**A trial run first:** `--dry-run` cuts and audits with no model and no numbers, in seconds. Never stack two unknowns: check the cuts before you pay for the numbers.

**Keywords versus meaning.** A keyword search for "window" finds 9 chapters: the Figure's four, plus five false alarms. Two of those say the opposite: "does not look out of the window once" and "does not go to the window". A keyword can't read "not". The question's own words ("motionless", "glass", "observed", "building", "opposite") find nothing, because the book never uses them.

The meaning search compares 384-number vectors: arrows in the same space, made by the same model in the same run. LanceDB measures the angle between the question's arrow and each passage's arrow (cosine distance; smaller is closer).

**Similarity is not significance.** The model finds what passages *look like*: scenes, objects, actions. It cannot see what they *mean* in the story, because meaning is rarely on the page. It finds the images; the author keeps the meaning.

## Git and GitHub

Git records versions on your own computer; GitHub carries them to anywhere else. They are two different jobs.

| Tool | Job | In Vault 2 |
| --- | --- | --- |
| git | the notary: records what changed and when, on this laptop | seals the prediction before the run; `seal-check.ps1` proves the order |
| GitHub | the courier: holds a copy others can reach | carries the book, the rules and the receipts to a browser, a phone, a fresh clone |
| Git Credential Manager | signs git in to GitHub (ships with Git for Windows) | opens a browser sign-in on the first push; Windows stores the result |

**The first push.** GitHub no longer accepts an account password for git. On Windows, `git push -u origin main` prints `info: please complete authentication in your browser...`, and you sign in there (with two-factor authentication if it's on). One command, no error. Other routes (personal access tokens, the GitHub CLI, SSH keys) are in `docs/04-GitHub-First-Push.md`.

**Receipts in, vectors out.** The repository holds the source (the chapters, the questions, the rules) and the recipe (vector-snake's receipts: model, budget, every cut's fingerprint). It does not hold the vectors: `.gitignore` excludes them, because they are build output that anyone with the recipe can rebuild. Software people will recognise this: source and lockfile in the repository, build output ignored.

**Line endings.** Git for Windows converts LF to CRLF by default (`core.autocrlf true`). A copy downloaded from GitHub would then differ, byte for byte, from the original, and its cuts could too. One line in `.gitattributes` fixes it: `* text=auto eol=lf`.

Three habits for working anywhere:

1. Before deleting a local copy, push, then check `git status` reports a clean tree.
2. On a borrowed computer, remove the stored sign-in afterwards (`cmdkey /delete:git:https://github.com`).
3. For your own manuscript, keep the repository **Private**. The teaching novel is **Public** so that you can clone it.

**Privacy for a public repository:** commit with GitHub's no-reply address (Settings → Emails → Keep my email addresses private), and search the receipts for personal paths before the first push (`Select-String receipts\*.json -Pattern 'Users\'` should find nothing).

## Finding the spine, step by step

The author names the spine; the machine is given a fair, sealed chance to find it by meaning. The test only counts because it could fail.

The book and the questions travel the same road; the answer key, written and sealed first, never enters the machine and judges only at the end.

1. **The author names four threads** from the whole book, using the SSOT's Emotional Spine and the character's arc ("from watching to being seen... from the window to the door"): the watcher, the lie, the cost, the door.

2. **The corpus is copied, not written.** One file per chapter, so each search result names one chapter. The writer's labels ("SETTING:", "FIGURE APPEARS") are removed, because they would hand the search its answers. 227 of 238 sentences are verbatim from the notes (`CORPUS.md`).

3. **One question per thread, with no content word from the book.** The author knows the answers; the questions describe them without quoting the book. Small words such as "a", "and" and "never" are shared; no word that carries meaning is. They were written on 18 Oct and never edited after a result.

*the spine pipeline · 5 stages, 1 sealed key*

| Thread | Question |
| --- | --- |
| The watcher | Someone stands motionless behind glass, observed from a building opposite, and never moves. |
| The lie | A comforting story somebody repeats about how they ended up here, so they never have to confess they fled. |
| The cost | Hurting a friend rather than facing the truth, and suffering consequences for it. |
| The door | Finally stepping outside and departing, to start over in an unfamiliar place. |

4. **The answer key is written and sealed before the run** (`PREDICTION.md`): which chapters each question should bring into its top 4, and how many count as a pass. It stays plain text. It is never turned into vectors, so the machine never sees it.

5. **Seal in git** (`git init`, `git add .`, `git commit`), then run. `seal-check.ps1` later compares the commit time with the run's own start time.

6. **Translate and audit.**

   ```powershell
   & $VS_PY $VS book --output-dir receipts --max-chars 800 --offline
   duckdb -bail -f spine\gate.sql
   ```

   vector-snake translates the 24 chapters and the 4 questions in one run with one model: 29 passages plus 4 questions, 384 numbers each. The DuckDB gate checks one run, no warnings or failures, one model, one vector size, all 24 chapters and exactly 4 questions. Only then does it write `corpus.parquet` and `questions.parquet`: `GATE_PASS runs=1 chapters=24 chunks=29 questions=4`. The questions are translated with the book but kept in their own file, so a question never finds itself.

7. **Search and score.**

   ```powershell
   & $LAB_PY spine\spine_search.py
   ```

   For each question LanceDB ranks every passage by cosine distance. Each chapter scores by its closest passage, and the top 4 chapters are compared with the sealed key: **HELD** or **NOT HELD**.

## The proof

One prediction of four held, a fresh AI inherited the rules from four files, and a fresh copy rebuilt every passage identically. Rehearsal results, 30 Oct 2026; the search gave identical ranks and distances three times.

**The spine: `SPINE_PARTIAL 1/4`.** The prediction was committed at 09:42:02 UTC and the run started at 09:45:51 UTC: `SEALED_BEFORE_RUN`.

| Thread | Predicted (pass rule) | Observed top 4 | Verdict |
| --- | --- | --- | --- |
| The watcher | 1 · 7 · 19 · 24 (3 of 4) | 1 · 7 · 24 · 19 (distances 0.57–0.59) | HELD, 4 of 4 |
| The lie | 2 · 5 · 13 · 21 (2 of 4) | 7 · 13 · 8 · 3 (0.68–0.74) | NOT HELD, 1 |
| The cost | 4 · 5 · 7 (2 of 3) | 14 · 2 · 21 · 6 (0.77–0.84) | NOT HELD, 0 |
| The door | 22 · 23 · 24 (2 of 3) | 10 · 11 · 5 · 24 (0.64–0.76) | NOT HELD, 1 |

**What the result means.** The concrete image was found perfectly: the four Figure chapters, from a question sharing no content word with them. The three abstract threads missed, and in a telling way: "hurting a friend" found the Sarah chapters (the friend, not the hurt); "departing" found people literally walking and leaving; "the lie" found the evasions, not the lie. The machine matched what passages look like.

Factors that probably shaped it (an interpretation, not measured):

- **Image versus idea:** the watcher is a physical scene; the lie, the cost and the door are inferred by the reader and never stated.
- The Figure chapters mirror each other by design (Locked Decision D3), so they cluster tightly.
- The cost question matched nothing closely (all distances 0.77 or more).
- **Negation and quotation:** the Lie appears as denial ("I'm fine. I chose this."), which small embedding models handle poorly.
- **One-line chapters:** chapters 21–23 are 74–91 characters, too little text to carry an idea.
- **The model and the corpus:** a small 384-number model, and terse notes rather than prose.

A larger embedding model might find more. That is a different experiment, with its own sealed prediction. This one tested what this laptop can do, and the questions are not changed after the result.

**Fresh inheritance: 3 of 3.** A new chat with memory off and no project received `SSOT.md`, `CONSTITUTION.md`, `book/corpus/ch-01.md` and `HANDOVER.md`, then one task and the 100% Rule question. Each answer was checked against the six lines of `spine/INHERITANCE.md`:

| Check | Result (3 runs) |
| --- | --- |
| states the Lie, "I chose this life", and that they fled into it | 3 of 3 |
| Chapter 2's routine as armour, not calm (the line added in the browser) | 3 of 3 |
| structural beats, no prose | 3 of 3 |
| says the Core Wound is not in the documents, or asks | 3 of 3 |
| invents no Core Wound details | 3 of 3 |
| no Figure dialogue, no moved pillar, no rewritten Chapter 1 | 3 of 3 |

Every run opened "Not 100% ready" and named the missing Core Wound. Unasked, every run also noticed that the Handover calls Chapter 2 a new deliverable while the evidence shows a Chapter 2 already in the corpus. The AI didn't remember; the documents did.

**Clone and rebuild: identical.** A fresh `git clone` brought 29 receipts and no vectors. vector-snake rebuilt the book in 32 s, and `compare.sql` returned `CUTS_IDENTICAL 33 33 33`, `VECTORS_MATCH 33` with a minimum cosine of 1.0, and `REBUILD_MATCHES_RECEIPT`.

Rebuilding needs vector-snake (available on request) and the same model snapshot (recorded in `CORPUS.md`). On a different machine the last decimals may differ, while the cuts stay identical.

**The machine, unchanged.** Nothing was installed. DuckDB, the HF CLI, Ollama and git still have one copy each; the user PATH and vector-snake's packages match the snapshots taken before the rehearsal.

## What the work taught us

Each snag below was real, found in rehearsal on SPOOKY, diagnosed and fixed at its source.

| Snag | What it teaches |
| --- | --- |
| Three of the four sealed predictions missed | a test that can't fail proves nothing; similarity is not significance |
| A fresh chat noticed the Handover contradicting the SSOT's evidence about Chapter 2 | documents drift too; a reader with no memory is a good checker |
| The notes put the tea box on the counter in chapter 6 and in the locked drawer in chapters 9 and 20 | an SSOT and a continuity ledger exist to catch exactly this |
| LanceDB's notice printed a temporary folder under the user's name; the first fix set TMP, but Python on Windows reads TEMP first | check the fix, not just the idea; set both |
| A table pasted through Notepad gained blank lines and broke on GitHub | look at the result where it is published |
| Git's default line-ending conversion would have made the clone differ from the original | fix it before the first commit, with `.gitattributes` |
| The commit email was a personal address, about to go into a public history | use GitHub's no-reply address before the first push |
| A new terminal tab opened Windows PowerShell 5.1 again | check that the prompt reads PS and the version is 7 before every block |
| GitHub's "quick setup" page suggested a first commit that would have replaced the seal | follow your own runbook, not the page's defaults |

## Errata for the videos

None yet. This section is filled after the recording, using the IDs V2-E01 onwards, and mirrors Section A of `VAULT-2-ERRATA.md`.

| ID | Video | Said | Correct |
| --- | --- | --- | --- |
|  |  |  |  |

## Assets in this vault

Every file is listed, in order of use, in `VAULT-2-INDEX.md`: the four templates and their field guide, the novel's filled-in control documents, the corpus and questions, the spine scripts (`cuts.sql`, `gate.sql`, `spine_search.py`, `seal-check.ps1`, `compare.sql`), the first-push guide, and the public repository [github.com/DamianGiambazi/novel-ssot](https://github.com/DamianGiambazi/novel-ssot). vector-snake itself is available on request. The writer-only character manifest is not included.

## Glossary

| Term | Meaning here |
| --- | --- |
| SSOT | Single Source of Truth: one written, agreed version of a project's facts |
| Constitution | the rules an AI must follow when working on the project |
| Handover | a note that lets the next session start where the last one stopped |
| Read-Only Agreement | a rule that a source may be analysed and cited but never rewritten |
| Locked Decision | a settled choice, with its reason, not reopened without a new decision |
| Frozen Constant | a fact every part of the project must agree with |
| Leviathan Lock | the list of things the AI must never do |
| spine | the few threads a whole book hangs on, named by the author |
| corpus | the set of documents searched; here, 24 chapter files |
| structure-aware chunking | cutting at paragraphs, then lines, sentences, words, up to a budget |
| run-up | the up-to-120 characters a passage repeats from the one before |
| dry run | cutting and auditing without the model: no numbers, seconds to run |
| receipt | vector-snake's manifest: model, budget and a fingerprint for every cut |
| cosine distance | how far apart two vectors point; smaller means closer in meaning |
| similarity vs significance | two passages can look alike without meaning the same thing in the story |
| seal | a git commit made before a run, so the prediction's date can be proved |
| repository (repo) | a folder whose history git records |
| commit, push, pull, clone | record a version; send it to GitHub; bring changes down; copy a whole repository |
| `.gitignore`, `.gitattributes` | files telling git what not to track, and how to store line endings |
| Git Credential Manager | the part of Git for Windows that signs git in to GitHub |
| no-reply address | a GitHub email address that keeps your real one out of public commits |

## What comes next

Vault 3 turns this searchable index into a tool any AI client can call: a custom MCP server, built in phases, each gated before the next. The SSOT, Constitution and Handover from this vault govern that build.