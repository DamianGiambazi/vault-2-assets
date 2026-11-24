# The spine experiment: method, result and the questions people ask

The author names the spine of the book. The machine is given a fair, sealed chance to find it by meaning. The test counts because it could fail, and three of its four predictions did.

## 1. The method, step by step

1. **Name the threads.** The author reads the whole book and names four threads, from the SSOT's Emotional Spine and the character's arc ("from watching to being seen… from the window to the door"): the watcher, the lie, the cost, the door.
2. **Prepare the corpus.** One file per chapter, copied from the notes, not written. The writer's labels ("SETTING:", "FIGURE APPEARS") are removed, because they'd hand the search its answers. 227 of 238 sentences are verbatim (`data/CORPUS.md`).
3. **Write the questions.** One per thread, describing it without any content word from the book: *describe the answer without quoting it.* They were frozen on 18 Oct 2026 and never edited after a result (`data/book/questions/`).
4. **Write the answer key.** Which chapters each question should bring into its top 4, and how many count as a pass (`data/PREDICTION.md`). It stays plain text; the machine never sees it.
5. **Seal.** Commit everything to git before any run. `seal-check.ps1` later proves the order.
6. **Cut and translate.** A trial run (`--dry-run`), then the real run: vector-snake turns the 24 chapters and the 4 questions into vectors in one run, with one model. The questions are translated with the book but kept in their own file, so a question never finds itself.
7. **Audit.** The DuckDB gate (`gate.sql`) checks: one run, no warnings or failures, one model, one vector size, all 24 chapters, exactly 4 questions. Only then does it write `corpus.parquet` and `questions.parquet`.
8. **Search and score.** `spine_search.py`: LanceDB ranks every passage by cosine distance; each chapter scores by its closest passage; the top 4 are compared with the sealed key.
9. **Declare.** The author's spine goes into the SSOT. The machine's result goes underneath, labelled as evidence.
10. **Make it travel.** Push, edit from a browser or phone, pull, clone, rebuild, compare.

## 2. The commands (from the repository root, PowerShell 7)

```powershell
$VS     = 'C:\Dev\vector-snake\vector_snake_v2.py'
$VS_PY  = "$env:USERPROFILE\.pyenv\pyenv-win\versions\3.11.9\python.exe"   # the Python vector-snake runs in
$LAB_PY = 'C:\Dev\smoke-lab\.venv\Scripts\python.exe'                    # the Vault 1 venv with LanceDB
$env:TEMP = 'C:\Dev\tmp'; $env:TMP = 'C:\Dev\tmp'                       # keeps LanceDB's temp folder off your user path

& $VS_PY $VS book --output-dir preview --dry-run --max-chars 800
duckdb -bail -f spine\cuts.sql
& $VS_PY $VS book --output-dir receipts --max-chars 800 --offline
duckdb -bail -f spine\gate.sql
& $LAB_PY spine\spine_search.py
.\spine\seal-check.ps1
```

Never pipe vector-snake's output: it crashes on the `→` character and marks every file FAILED.

## 3. The result

`SPINE_PARTIAL 1/4`: the watcher was found in all four of its chapters, and the lie, the cost and the door were not. The ranks and distances were identical in rehearsal (30 Oct) and on the take (1 Nov). The details are in `docs/02-Structure-and-Meaning.md`.

## 4. The questions people ask

| Question | Answer |
|---|---|
| Didn't you write the questions knowing the answers? | Yes, deliberately. They are independent of the book's *words*, not its meaning, like an examiner rephrasing a question. |
| Are the questions part of the book now? | They're translated with it and searched against it, but never part of it. They live in their own file. |
| Why seal it in git? | To prove the prediction came before the result. Git records the commit time; vector-snake records the run's start; the seal check compares them. |
| Is the method broken if three of four miss? | No. A test that can't fail proves nothing. The one hit counts because the misses were possible. |
| Would a bigger model find the other three? | It might. That would be a different experiment, with its own sealed prediction. |
| Did the AI "understand" the book? | No. A pass means the prediction held. The search measures similarity; it doesn't understand. |
| So what is the local search good for? | Finding *where* things happen: images, objects, scenes, continuity. Not deciding what they mean. |
| Why keep a failed result in the SSOT? | Because it's true, and it tells the author exactly what the tool can and can't do. |
| Can I work on this on my phone? | The words and rules, yes, through GitHub. The meaning search and rebuild need a computer with the tools. |
| Is this only for novels? | No. The control documents and git generalise to code, research and client work. The meaning search is an assistant, not a judge. |
