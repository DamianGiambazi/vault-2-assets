# Vault 2 — Errata (2 Nov 2026)

Format: Vault Series Asset Specification v1.0, §7.
**Types:**
- **Fact:** a wrong claim;
- **Precision:** right idea, imprecise words;
- **Omission:** something left out that matters;
- **Transcript:** a captions or speech-to-text error.

These are corrections to the two videos and to earlier copies of the assets. The Reference and the files in this pack already use the corrected versions. This sheet tells you what to change anywhere else: captions, the pinned comment, older copies.

**Videos:**
- **Full:** *Reproducible and Portable Truth: Finding the Spine, Then Making It Travel* (41:00).
- **Reload:** *The Reload Check*, the short follow-up on the Handover.

## A. Video corrections

| ID | Video | Where | Type | Said | Correct |
|---|---|---|---|---|---|
| V2-E01 | Full | the spine search | Fact | "executing the Python file to run the query through Ollama"; "see if our LLM can extract…" | The spine search uses no language model and no Ollama. LanceDB compares the MiniLM vectors (384 numbers each) by cosine distance, and `spine_search.py` scores them against the sealed rules. |
| V2-E02 | Full | the clone | Fact | "upon setup, it automatically fires vector-snake to rebuild" | Cloning runs nothing. vector-snake was run by command in the clone, and `compare.sql` then checked the rebuild: `REBUILD_MATCHES_RECEIPT`. |
| V2-E03 | Full | the recap | Fact | "analysing 28 chapters instead of just four small documents" | 24 chapters plus 4 questions = 28 files. (Vault 1: four documents plus one question.) |
| V2-E04 | Full | the seal | Precision | "commit our predictions to GitHub first" | Committed locally with git first; pushed to GitHub later. Said correctly moments later. |
| V2-E05 | Full | the cuts | Precision | "vector-snake is context-aware" | Structure-aware: it splits at paragraphs, then lines, sentences and words, up to the budget. |
| V2-E06 | Full | the gate | Precision | "we included the DuckDB audit in the same command string" | A separate command, `duckdb -bail -f spine\gate.sql`, run straight after vector-snake. |
| V2-E07 | Full | `git init` | Precision | "the .git folder records every action and change" | Git records what you commit, when you commit it. |
| V2-E08 | Full | `.gitignore` | Precision | the vectors aren't pushed because they are "massive databases" | They're left out because they're build output, rebuildable from the book and the receipts. Size isn't the reason here. |
| V2-E09 | Full | the close | Precision | "we can query creative intuition and thematic meaning"; "we've found the upper limit of this small model" | One sealed test: the concrete image was found (4 of 4) and three abstract threads were missed. It shows this model's limit on this test, not its upper limit. |
| V2-E10 | Full | the experiment | Omission | the seal check isn't shown | `seal-check.ps1`: prediction sealed 16:02:34 UTC, run started 16:14:05 UTC, `SEALED_BEFORE_RUN` (verified 2 Nov; see the Reference). |
| V2-E11 | Full | opening | Precision | "Vault 2: Control Documents" | Vault 2 — Control Document Templates (The SSOT Library). |
| V2-E12 | Reload | the checker | Precision | an AI checker is "more accurate" than checking by hand | It is faster and consistent; its accuracy isn't established by this test. A spot check agreed on all 18 results, with two borderline cases noted in `docs/07`. |
| V2-E13 | Reload | opening | Transcript | "novel_ssot" | `novel-ssot`. Fix the captions. |

## B. Asset corrections (applied in this pack)

| File | Change |
|---|---|
| *GitHub First Push* guide → `docs/04-GitHub-First-Push.md` | an old account name → `DamianGiambazi`; Git Credential Manager's browser sign-in (what the video shows) added as the default route; the GitHub CLI kept as an option; a privacy section added |
| `PREDICTION.md` (draft of 18 Oct) | the "Sealed" line reworded on 30 Oct, **before** the take's seal, to say what really happens (a rehearsal seal, then the on-camera seal); rehearsal rows R1–R2 filled |
| `HANDOVER.md` (draft of 12 Oct) | load order now lists `book/corpus/ch-01.md` and the Handover itself; "Spine evidence: SSOT § Machine Evidence" added |
| `CONSTITUTION.md` (draft of 12 Oct) | the Read-Only Agreement added as a clause |
| `SSOT.md` (draft of 12 Oct) | Machine Evidence section added; changelog dated |
| `novel-ssot/SSOT.md` | ships as committed in `131ee69`, with the run ID filled. The one-line pacing rule added from the phone is in the repository version (`74fd9a4`) |
| `lab/reload-check/answer-qwen.md` | the answer had been pasted twice; the duplicate was removed, wording unchanged |
| outline and beat sheet (12–18 Oct) | "single continuous session" → Honest Cut doctrine; an old account name → `DamianGiambazi`; "Desk Reference" → the Reference (recording-only files, not shipped) |

**Not changed on purpose:**
- **`data/`** (24 chapters, 4 questions, `CORPUS.md`, `PREDICTION.md`, `.gitignore`, `.gitattributes`) is byte-identical to the seal commit `13913f5`. The receipts in the repository fingerprint every passage, so any edit would break the rebuild check.
- **The tea-box continuity slip:** on the counter in ch-06, in the locked drawer in ch-09 and ch-20. It's left in the notes as a worked example of drift (`docs/01`, §6).
- **The phone edit's commit message,** "Update Chapter 2 description in SSOT.md" (suggested by the GitHub app; the plan said `docs: add chapter 2 pacing note`). History is never rewritten.

## C. Versions to record

| Item | At recording (1–2 Nov 2026) | Note |
|---|---|---|
| PowerShell | 7.6.9 | 7.6.7 at the Vault 1 recording |
| git | 2.50.1.windows.1 | `core.autocrlf true`, so every repository gets `.gitattributes` |
| DuckDB / LanceDB | 1.5.6 / 0.40.0 | unchanged since Vault 1 |
| vector-snake | 2.0.0 | v2.1 deferred |
| Embedding model | all-MiniLM-L6-v2, snapshot `1110a243fdf4706b3f48f1d95db1a4f5529b4d41` | recorded in `CORPUS.md` |
| Ollama / HF CLI | 0.40.11 / 2.2.0 | installed, not used in Vault 2 |
| Reload-check models | ChatGPT 6 (medium), Claude Opus 4.6 (medium), Qwen 3.8 Max (thinking); checker Gemini 3.1 Pro Preview | as labelled in the chat apps |
| User PATH | +1 entry since the 30 Oct snapshot: Gitleaks (an unrelated project) | not a Vault 2 change |

## D. Open before publishing

(none)
